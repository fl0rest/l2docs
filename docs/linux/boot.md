# Linux Boot Process Overview

The Linux boot process begins with the BIOS or UEFI performing a POST to check hardware functionality. After initializing the hardware, the firmware executes a bootloader (like GRUB) that loads the Linux kernel into memory. The kernel then initializes system resources and starts the init process, which manages the launch of user-space processes.

## BIOS and UEFI

### BIOS (Basic Input/Output System):

This is an older type of firmware that helps initialize your computer's hardware when you turn it on. It prepares everything before passing control over to the bootloader, which then starts your operating system.
It does have some limitations. For example, it can’t boot from hard drives larger than 2.2 TB, which can be a hindrance in today’s world of large-capacity storage.

### UEFI (Unified Extensible Firmware Interface):

UEFI is the modern replacement for BIOS. It's designed to be more powerful and flexible.
One of the main advantages of UEFI is its support for much larger drives—over 2.2 TB—along with quicker boot times. Additionally, it includes advanced features like Secure Boot, which enhances system security by preventing unauthorized software from loading during startup.

## Bootloader

A bootloader is a piece of software that loads the operating system (OS) into memory when the computer starts. It serves as an intermediary between the firmware (BIOS/UEFI) and the OS.

Common Bootloaders:

* **GRUB** (GNU Grand Unified Bootloader): The most widely used bootloader in Linux systems.

* **LILO** (Linux Loader): An older bootloader that's largely obsolete.

* **Syslinux**: A simpler bootloader often used for booting from USB drives.

## Linux Kernel

The Linux Kernel is the core of the Linux operating system, managing hardware, system resources, and communication between software and hardware components.

Four Main Kernel Roles:

1. **Process Management**: Handles process scheduling and CPU time allocation.

2. **Memory Management**: Manages system memory and the resources allocated to processes.

3. **Device Management**: Provides a communication layer for hardware drivers and userspace applications.

4. **File System Management**: Manages file operations and access to various file systems.

## Daemons and Init Systems

A daemon is a background process that runs continuously and handles system or service tasks without user interaction. 

For example:

* `sshd`: The SSH daemon that allows secure remote login and command execution over a network.

* `httpd`: The Apache HTTP Server daemon, which serves web pages to clients over the internet.

* `mysqld`: The MySQL database server daemon, managing database operations.

* `ftpd`: The FTP server daemon, which allows file transfers over the File Transfer Protocol.

* `rsyslogd`: The logging daemon that collects and manages log messages from various system components.

Init is the parent of all processes in a Linux system and is responsible for starting and managing user processes after the kernel has booted.
The process ID (PID) of the init process is 1.

Init Systems:

* **sysVinit**: The traditional init system using run levels to manage services.

* **Upstart**: An event-based replacement for sysVinit, designed for faster booting.

* **systemd**: A modern init system that uses units and targets, providing parallelization and improved service management.

## SWAP Memory

SWAP memory is a space on a hard drive used as virtual memory when physical RAM is full. 

* **Swap Partition**: A dedicated area of the hard drive designated for swap space.

* **Swap File**: A regular file on a filesystem used for swap space, providing more flexibility than a partition.

To check swap memory on a Linux system, you can use various commands.
The `free -h` command provides a human-readable summary of memory usage, including swap. The swapon `--show` command lists swap devices and their usage, while `cat /proc/swaps` displays a detailed view of currently used swap spaces.

## Pseudo Filesystem

A pseudo filesystem is a filesystem that doesn't exist on physical storage but represents system information. An example is `/proc`, which provides insights into system and process information dynamically.

Another nice example is `/tmp` which is often used for temporary files.

## Linux Permissions

Linux utilizes a permission system to control access to files and directories. Each file has three types of permissions for three categories of users: `owner`, `group`, and `others` (`read`, `write`, `execute`).

Permissions are often shown using a string of characters (e.g. `rwxr-xr--`).

For more information, please check [here](https://vaxeman.github.io/l2docs/linux/permissions.html).

## Find vs. Locate Commands

* `find`: Searches for files in a directory hierarchy based on various criteria. It's slower but more powerful and accurate because it searches the filesystem in real time.

* `locate`: Uses a pre-built database to find files quickly. It’s faster but may not be up to date. Use `updatedb` to refresh the database.

!!! question "When to use them, you may ask?"

    Essentially, select `find` for detailed or immediate searches and `locate` for simple, rapid filename lookups.

## Mount Command

The mount command is used to attach a filesystem (like a hard drive or USB) to a specified directory in the Linux directory tree, making it accessible.

The basic syntax of the mount command is:

```console
mount [options] <source> <mount_point>
```

`<source>`: The device or filesystem to be mounted, such as /dev/sdb1 or a remote filesystem with an appropriate protocol.

`<mount_point>`: The directory where the filesystem will be attached.

Example:
To mount a USB drive located at `/dev/sdb1` to a directory called `/mnt/usb`, you would use:

```console
sudo mount /dev/sdb1 /mnt/usb
```

Unmounting

To detach or unmount a filesystem, you use the umount command followed by the mount point or the device name. This is crucial for safely removing the device, ensuring all data is written back and the filesystem is no longer in use.

## IPtables and CSF/APF

**IPtables**: A user-space utility that allows an administrator to configure firewall rules within the Linux kernel, controlling incoming and outgoing traffic.

**CSF** (ConfigServer Security & Firewall) and **APF** (Advanced Policy Firewall): Tools that use iptables to provide a frontend for managing firewall rules and enhancing security.

Linux Filesystem Structure

* `/etc/passwd`: Stores user account information. 600 permission.

* `/etc/shadow`: Contains hashed passwords and security information for user accounts. 

    It contains the basic information for each user account, such as the user name, user ID, group ID, home directory, login shell, etc. 644 permission.

* `/proc`: A pseudo-filesystem that provides process and kernel information.

* `/etc/fstab`: Contains information related to disk drives and filesystems to be mounted.

* `/etc/hosts`: Maps hostnames to IP addresses.

    For example, if the hosts option is set to:
    hosts: files dns
    then the /etc/hosts file will be searched first, and if no match is found, the DNS server will be queried next.

* `/etc/nsswitch.conf`: Configures the sources used for name service switch.

## Sudo and Wheel Group

`sudo`: A command that allows users to run programs with the privileges of another user (usually root), enhancing security.

The behavior and permissions of `sudo` are governed by the `/etc/sudoers` file, where administrators can specify which users or groups have permission to run specific commands with `sudo`.

!!! warning "You need to be VERY CAREFUL when running commands as `sudo`!"


Wheel Group: A special user group that grants users the ability to execute commands using `sudo`.
Like `sudo`, the groups and permissions associated with the wheel group can also be configured in the `/etc/sudoers` file.

To add a user to the wheel group, you could use the following command:

```console
sudo usermod -aG wheel username
```

- To check which groups a user belongs to, you can use the command groups `[username]` or id `[username]`.

## Linux Niceness

Linux niceness refers to the priority level of processes in the scheduler. The niceness value ranges from -20 (highest priority) to 19 (lowest priority).
A lower niceness value means the process is more "greedy" for CPU time, whereas a higher niceness value indicates that the process is less demanding, allowing other processes to receive more CPU time.

The default nice level when a process is started using the nice command is 10. This means that the process will have a lower priority than the normal processes, which have a nice level of 0.
The renice command changes the nice level of a running process.
For example, renice -n 10 1234 will change the nice level of the process with PID 1234 to 10.

## Umask

Umask is a user file-creation mask controlling the default file permissions given when a new file or directory is created. It specifies which permission bits should be disabled by default.
The umask value is subtracted from the maximum permissions for files and directories, which are `666` and `777`.

??? question "Which of the following settings for umask ensures that new files have the default permissions `-rw-r----`?"

    The correct setting for umask that ensures that new files have the default permissions `-rw- r-----` is `0027`.
    Since the first digit of the umask value is for the special permissions, such as setuid, setgid, and sticky bit, we need to add a zero at the beginning to indicate that no special permissions are set.
