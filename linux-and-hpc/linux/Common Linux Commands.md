#linux #unix #gpt

## File and Directory Management **(REQUIRED TO KNOW)**

| **Command** | **Description**                                           | **Example**                     |
| ----------- | --------------------------------------------------------- | ------------------------------- |
| `ls`        | Lists files and directories in the current directory.     | `ls -l` (detailed list)         |
| `cd`        | Changes the current directory.                            | `cd /path/to/directory`         |
| `pwd`       | Prints the current working directory.                     | `pwd`                           |
| `mkdir`     | Creates a new directory.                                  | `mkdir new_folder`              |
| `rmdir`     | Removes an empty directory.                               | `rmdir empty_folder`            |
| `rm`        | Removes files or directories.                             | `rm file.txt`, `rm -r folder`   |
| `cp`        | Copies files or directories.                              | `cp source.txt destination.txt` |
| `mv`        | Moves or renames files or directories.                    | `mv old_name.txt new_name.txt`  |
| `touch`     | Creates an empty file or updates the timestamp of a file. | `touch new_file.txt`            |
| `find`      | Searches for files and directories.                       | `find /path -name "*.txt"`      |

## File Viewing **(REQUIRED TO KNOW)**

|**Command**|**Description**|**Example**|
|---|---|---|
|`cat`|Displays the contents of a file.|`cat file.txt`|
|`more`|Paginates through a file's content.|`more file.txt`|
|`less`|Similar to `more`, but allows backward navigation.|`less file.txt`|
|`head`|Displays the first lines of a file.|`head -n 10 file.txt`|
|`tail`|Displays the last lines of a file.|`tail -n 10 file.txt`|
|`wc`|Counts lines, words, and characters in a file.||
|`wc -l` number of lines|`wc file.txt`, `wc -l file.txt`||

## File and Text Processing

|**Command**|**Description**|**Example**|
|---|---|---|
|`grep`|Searches for patterns in text.||
|`grep -v` reverse search|`grep "pattern" file.txt`||
|`sed`|Stream editor for search and replace.|`sed 's/old/new/' file.txt`|
|`awk`|Text processing and data extraction.|`awk '{print $1}' file.txt`|
|`sort`|Sorts lines in a file.|`sort file.txt`|
|`uniq`|Removes duplicate lines.|`uniq sorted_file.txt`|
|`cut`|Extracts specific columns or fields.|`cut -d',' -f2 file.csv`|

## Archiving and Compression **(REQUIRED TO KNOW)**

|**Command**|**Description**|**Example**|
|---|---|---|
|`tar`|Archives files into a tarball.|`tar -cvf archive.tar file1 file2`|
|`gzip`|Compresses files using gzip.|`gzip file.txt`|
|`gunzip`|Decompresses gzip files.|`gunzip file.txt.gz`|
|`zip`|Compresses files into a ZIP archive.|`zip archive.zip file.txt`|
|`unzip`|Extracts files from a ZIP archive.|`unzip archive.zip`|

## Networking

|**Command**|**Description**|**Example**|
|---|---|---|
|`ping`|Checks connectivity to a host.|`ping google.com`|
|`wget`|Downloads files from the internet.|`wget <http://example.com/file.txt`>|
|`curl`|Transfers data from or to a server.|`curl <http://example.com`>|
|`netstat`|Displays network connections (use `ss` for newer systems).|`netstat -tuln`|
|`scp`|Securely copies files between systems.|`scp file.txt user@host:/path/to/destination`|
|`ssh`|Logs into a remote system.|`ssh user@host`|

## Permissions and Ownership, see [[Linux Permissions]]

|**Command**|**Description**|**Example**|
|---|---|---|
|`chmod`|Changes file permissions.|`chmod 755 file.sh`|
|`chown`|Changes file ownership.|`chown user:group file.txt`|
|`umask`|Sets default file permissions.|`umask 022`|

## Process Management

|**Command**|**Description**|**Example**|
|---|---|---|
|`ps`|Displays running processes.|`ps aux`|
|`top`|Interactive view of system processes.|`top`|
|`htop`|Advanced version of `top` (if installed).|`htop`|
|`kill`|Sends a signal to a process (e.g., terminate).|`kill 12345`|
|`killall`|Kills processes by name.|`killall firefox`|
|`jobs`|Lists background jobs.|`jobs`|
|`fg`|Brings a background job to the foreground.|`fg %1`|
|`bg`|Resumes a background job.|`bg %1`|

## System Monitoring and Management

|**Command**|**Description**|**Example**|
|---|---|---|
|`df`|Shows disk usage.|`df -h`|
|`du`|Displays the size of directories and files.|`du -sh folder`|
|`free`|Shows memory usage.|`free -h`|
|`uptime`|Displays system uptime and load.|`uptime`|
|`uname`|Displays system information.|`uname -a`|
|`dmesg`|Prints system boot and kernel messages.|`dmesg|

## User Management

|**Command**|**Description**|**Example**|
|---|---|---|
|`whoami`|Prints the current user.|`whoami`|
|`who`|Displays logged-in users.|`who`|
|`adduser`|Adds a new user.|`adduser new_user`|
|`passwd`|Changes a user’s password.|`passwd`|

## Miscellaneous

|**Command**|**Description**|**Example**|
|---|---|---|
|`alias`|Creates a shortcut for a command.|`alias ll='ls -l'`|
|`history`|Displays the command history.|`history`|
|`echo`|Prints text to the terminal.|`echo "Hello, World!"`|
|`date`|Displays or sets the system date and time.|`date`|
|`clear`|Clears the terminal screen.|`clear`|

