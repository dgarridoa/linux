# Commands viewed along the course

https://www.omgubuntu.co.uk/2019/09/useful-ubuntu-keyboard-shortcuts

https://help.ubuntu.com/stable/ubuntu-help/shell-keyboard-shortcuts.html.en

## SSH
### 1. Simple connextion

  1. ennable connextion to ssh client:  
  	- ssh IP/Host Name (ex:localhost)
  	- ssh username@hostname
  2. close connextion:
  	- exit

### 2. Connect to VM through ssh

Setting > Network > Advanced > Port Forwarding, then set the SSH parameter:
  - Name: SSH
  - Protocol: TCP
  - Host IP: 127.0.0.1
  - Host Port: 2222
  - IP Guest: Empty
  - Port Guest: 22

Run: ssh adminuser@127.0.0.1 -p 2222

https://medium.com/@pierangelo1982/setting-ssh-connection-to-ubuntu-on-virtualbox-af243f737b8b

## Time
1. ask the time during which the machine is running: uptime

## User
1. create new user: useradd nameuser
2. give password: passwd username
3. ask actual user: whoiam
4. switch of user:su - username


## Basic Commands
- ls : Lists directory contents.
- cd : Changes the current directory.
- pwd - Displays the present working directory.
- cat - Concatenates and displays files. tac - return the same but in reverse order.
- echo - Displays arguments or variables to the screen.
  - echo $PATH
- man - Displays the online manual.
 - man -k command : search manual pages associated to the command
- command --help, -h : get help abput a command.
 - man ls
 - g: move to the top, G: move to the bottom, q: exit.
- exit : Exits the shell or your current session.
- clear : Clears the screen.

## Directories

- ls empty/path: Lists directory contents.
  - -l : long listing format. information: permissions, number of links, owner name, group name, number of bytes in the file, las modification time, file name.
  - -a, --all : do not ignore entries startinh with . (hidden files)
  - -F: reveal the type, / Directory, @ Link, * Executable.
  - t: list files by time in descending order.
  - r: reverse order.
  - R: list files recursively
- tree empty/path: similar to ls -R, but creates visual output.
  - -d: list directories only.
  - -C: colorize output.
- cd: Changes the current directory.
 - cd . : this directory
 - cd .. : the parent directory
 - cd -, $OLDPWD : change to the previous directory
 - cd empty: back to the home directory
- ./command: execute command in the current dir and not in $PATH.
- path/command: execute command in this full path and not in $PATH.
  - /bin/cat
  - /home/dgarrido/my-cat/cat
- pwd: Displays the present working directory.
- mkdir [-p] directory: create a directory.
  - mkdir newfolder
  - mkdir -p 1/2/3
- rmdir [-p] directory: remove an empty directory.
- rm -rf directory: recursively removes directory.
- mv old_name new_name: rename folder/file.
- touch file: make a file.
- path with space: "space folder" or space\ folder.

## Permissions
With `ls -l` is possible view the permissions structure of regular files (`-`), directories (`d`) or symbolic links (`|`).

| Permission  | File                            | Directory                                           |
|-------------|---------------------------------|-----------------------------------------------------|
| Read (r)     | Allows a file to be read.       | Allows file names in the directory to be read.      |
| Write (w)   | Allows a file to modified.      | Allows entries to be modified within the directory. |
| Execute (x) | Allows the execution of a file. | Allows access to contents and metadata for entries. |

 There are four permission categories, user (`u`), group (`g`), other (`o`) and all (`a`). For example,`-rw-rw-r--` means that the file has permissions of reading and writing for user, reading for group and nothing for other.

- groups, id -Gn: displays a user's groups.
- chgrp: changes the group.
- chmod: change mode command.
  - chmod u+rwx, g+rw, o+r file
  - chmod 764 file
- ugoa: user category.
- +-= add, subtract, or set permissions
- umask: determines default permission, if no mask were used permission would be 777 for directories and 666 for files and default umask is 022.
|                      | Directory | File |
|----------------------|-----------|------|
| Base Permission      | 777       | 666  |
| Subtract Umask       | -022      | -022 |
| Creations Permission | 755       | 644  |
- rwx: read, write, execute.
 - symblic: rwx, r-x, r--.
 - binary: 111, 101, 100.
 - decimal: 7 (4+2+1), 5 (4+0+1), 4 (4+0+0).
 - octal: -rwxr-xr-x = 755, -rw-r--r--=644

## Search
- find [path][expression]: recursively finds files in path that match expression. If no arguments are supplied it find all files in the current directory.
  - -name pattern: find files and directories that match pattern.
      - find /path -name Django
      - find /path -name *v: return all files that contains "v" in its name.
  - -iname pattern: like -name, but ignore case (is not case sensitive).
    - find /path -iname django
  - -ls: performs an ls on each of the found items.
  - -mtime days: finds files that are days old.
    - find . -mtime +1 -mtime -5
  - -size num: finds files that are of size num.
    - find . -size +500M
  - -newer file: finds files that area newer than file.
    - find . -type d -newer file.txt:search all directories in the current path that are newer than file.txt.
  - -exec commando {} \; : run command against all the files that are found.
- locate pattern: faster than the find command, this queries an index (lookup table). Must be updated for find new files.
  - locate commands.md

## Environment variables
- echo : Displays arguments or variables to the screen.
- $PATH: is a list of directories where a command is searched, if the command is found it will be executed else return the message command not found.
  - which  command: display the path where is located a command
- $OLDPWD: holds the directory that you were previosly in.
