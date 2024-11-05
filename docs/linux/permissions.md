# Permissions

Every file and directory in your UNIX/Linux system has the following 3 permissions defined for all the 3 owners discussed above.

* **`r`** = read permission
* **`w`** = write permission
* **`x`** = execute permission
* **`–`** = no permission

`Read` - This permission gives you the authority to open and read a file. Read permission on a directory gives you the ability to list its content.

`Write` - The write permission gives you the authority to modify the contents of a file. The write permission on a directory gives you the authority to add, remove and rename files stored in the directory. Consider a scenario where you have to write permission on file but do not have write permission on the directory where the file is stored. You will be able to modify the file contents. But you will not be able to rename, move or remove the file from the directory.

`Execute` - In Windows, an executable program usually has an extension “.exe” and which you can easily run. In Unix/Linux, you cannot run a program unless the execute permission is set. If the execute permission is not set, you might still be able to see/modify the program code(provided read & write permissions are set), but not run it.

R = 4

W = 2

X = 1

```shell
rwx--x--x  1 root root   181 Aug  5  2023  cats.jpg
---------  - ---- ----   --- ------------  --------
|    |     |   |   |      |        |           |
|    |     |   |   |      |        |           +----- file name
|    |     |   |   |      |        +----- modification time
|    |     |   |   |      +----- file size in bytes
|    |     |   |   +----- group owner
|    |     |   +----- user owner
|    |     +----- hard link count
|    +----- permissions
+---- file type
```

## Absolute(Numeric) Mode in Linux

In this mode, file permissions are not represented as characters but a three-digit octal number.

| Number 	| Permission Type       	| Symbol 	|
|--------	|-----------------------	|--------	|
| 0      	| No Permission         	|    —   	|
| 1      	| Execute               	|   –x   	|
| 2      	| Write                 	|   -w-  	|
| 3      	| Execute + Write       	|   -wx  	|
| 4      	| Read                  	|   r–   	|
| 5      	| Read + Execute        	|   r-x  	|
| 6      	| Read +Write           	|   rw-  	|
| 7      	| Read + Write +Execute 	|   rwx  	|

## Recommended file and directory permissions

!!! warning "Important Notice"

    PHP files should be set to 600.

    Typically files within a /html/ directory should be set to 644 and directories to 755; the typical exception to this is configuration files (such as wp-config.php) where consideration should be made to set those to a lower value such as 600 to prevent unauthorised reading and writing of the file.

### Common Permission Types

`644 :: r-wr–r–` 

* 6 – Owner has read and write permissions;
* 4 – Group has only read permissions;
* 4 – Others have only read permissions.

**Info**:  Allows only the owner to make changes to the file or directory.

`755 :: rwxr-xr-x` 

* 7 – Owner has read, write, and execute;
* 5 – Group has read and execute permissions;
* 5 – Others have read and execute permissions.

**Info**:  Required to install some scripts or to browse to a required directory.

For example, finding files/directories and changing mode to default values:

`find . -type f -exec chmod 644 {}  \;`

`find . -type d -exec chmod 755 {} \;`
