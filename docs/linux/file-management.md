# File management

Linux file management involves organizing, accessing, and manipulating files and directories within the Linux operating system. 
Users interact with files using commands like `ls` (list), `cp` (copy), `mv` (move), and `rm` (remove). 
Directories, or folders, provide a hierarchical structure for organizing files. Permissions control access to files, determining who can `read`, `write`, or `execute` them.
Key concepts include file `ownership`, represented by `users` and `groups`, and `symbolic links`, which are pointers to other files or directories.
Understanding these fundamentals enables efficient navigation and management of files in the Linux environment.

## Creating, copying, deleting and renaming

- `touch` creates a new, empty file
- `rm` permanently remove file/directory
- `cp` copying files/directories from one location to another; **example:**

    ``` shell
    $ cp [...file/directory-sources] [destination]

    # [file/directory-sources] specifies the sources of the files or directories you want to copy. And the [destination] argument specifies the location you want to copy the file to.
    ```

- `mv` versatile tool used for moving or renaming files/directories; **example:**

    ### Renaming Files with ‘mv’
    ``` shell
    $ mv helloworld.txt hellolinux.txt

    # Output:
    # The file previously named 'helloworld.txt' is now named 'hellolinux.txt'

    ```

    ### Moving Files with ‘mv’
    ``` shell
    mv /home/user/Documents/old_folder/file.txt /home/user/Documents/new_folder/

    # Output:
    # 'file.txt' is moved from 'old_folder' to 'new_folder'
    ```

    ### Moving and Renaming Directories
    ```shell
    mv old_directory/ new_directory/

    # Output:
    # The directory 'old_directory' is now renamed to 'new_directory'
    ```

    ### Using Wildcards with ‘mv’
    ```shell
    mv *.txt text_files/

    # Output:
    # All .txt files are moved to the 'text_files' directory
    ```

    !!! danger "Overwriting Files"
        A potential pitfall with the ‘mv’ command is that it overwrites files without any warning. If a file with the destination name already exists, the ‘mv’ command will overwrite it.
    
    ### Overwriting Files
    ```shell
    mv file1.txt file2.txt

    # Output:
    # 'file1.txt' is renamed to 'file2.txt', overwriting the existing 'file2.txt'
    ```

- `cp -r` i `rm -r` označava rekurzivno kopiranje i brisanje, briše direktorij i sve datoteke i poddirektorije u njemu

    - kod naredbe `mv`, za razliku od `cp` i `rm`, nema potrebe za rekurzijom; radi se o preimenovanju

## Glob patterns

File globbing is pattern-matching mechanism used for filename expansion in the shell. The term "glob" represents the concept of matching patterns globally or expansively across multiple filenames or paths. It allows users to specify wildcard patterns to match files or directories based on certain criteria.

- [glob patterns](https://en.wikipedia.org/wiki/Glob_(programming)), globs ili wildcards

    - koriste se za pretraživanje uzoraka koji odgovaraju zadanome
    - način da više datoteka koje imaju slična imena povežemo jednom naredbom
    - glob != regex, samo ima donekle sličnu sintaksu i namjenu

- `?` -- is used to match exactly one character, any will do
- `*` -- is used to match any number of characters (zero or more)
- `[chars]` -- can be used to match exact characters or you can also specify a range within [] `[A-Z]`, `[a-z]` ili `[0-9]`
- `!` -- is used to exclude characters from the list that is specified within the square brackets

- `\` -- tzv. *prekidni znak*

## Pretraživanje datotečnog sustava

- `find` u specificiranim direktorijima traži datoteke ili skupine datoteka

    - sintaksa: `find <direktoriji> <uvjeti>` (direktorij koji se pretražuje *mora* biti naveden prije uvjeta)
    - direktorij može biti dan korištenjem apsolutnog ili relativnog referenciranja
    - može koristiti regularne izraze parametar `-regex`
    - pregled često korištenih parametara:

        | Parametar | Ograničenje pretraživanja |
        | --------- | ------------------------- |
        | `-user <ime korisnika>` | Samo datoteke određenog korisnika |
        | `-size <veličina>` | Samo datoteke specifične veličine |
        | `-type f` | Samo datoteke (ne direktoriji) |
        | `-type d` | Samo direktoriji |
        | `-name <ime datoteke>` | Samo datoteke određenog imena |
        | `-atime <broj dana>` | Samo datoteke čije je vrijeme posljednjeg pristupa manje od navedenog u danima |
        | `-amin <broj minuta>` | Isto kao iznad samo se navode minute umjesto dana |
        | `-ctime <broj dana>` | Samo datoteke čije je vrijeme izrade manje od navedenog u danima |
        | `-cmin <broj minuta>` | Isto kao iznad samo se navode minute umjesto dana |
        | `-mtime <broj dana>` | Samo datoteke čije je vrijeme posljednje promjene manje od navedenog u danima |
        | `-mmin <broj minuta>` | Isto kao iznad samo se navode minute umjesto dana |
        | `-newer <datoteka>` | Samo datoteke stvorene prije određene datoteke |

    - naredba je spora jer mora provjeriti svaki file koji se nalazi na zadanoj putanji
    - česte primjene:

        - brisanje nađenih datoteka naredbom find: `find ... -exec rm {} \;` ili `find ... | xargs rm`
        - pretraživanje nađenih datoteka: `find ... -type f | xargs grep <izraz>`


- `locate` pretražuje bazu datoteka za datoteku koja u imenu sadrži dani niz znakova

    - sintaksa: `locate <ime datoteke>`
    - rezultat pretraživanja je puna putanja do tražene datoteke
    - pretražuje brže jer stvara bazu imena datoteka koje postoje u datotećnom sustavu te nema potrebe pretraživati svaku datoteku koja postoji na datotečnom sustavu; baza se osvježava naredbom `updatedb`, na većini modernih distribucija se pokreće automatski na dnevnoj bazi

- `which` se koristi kada želimo saznati punu putanju do određene izvršne datoteke datoteke koja se nalazi u nekom od direktorija navedenom u varijabli okoline `PATH`

- `whereis` pretražuje lokaciju izvornog koda, binarnih datoteka i stranica priručnika

    - pregled često korištenih parametara:

        | Parametar | Uloga |
        | --------- | ----- |
        | `-b` | traženje binarnih datoteka |
        | `-m` | traženje stranica priručnika |
        | `-s` | traženje izvršnog koda |