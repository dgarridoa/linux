# Commands viewed along the course

# Keyboard shorcuts
- https://www.omgubuntu.co.uk/2019/09/useful-ubuntu-keyboard-shortcuts
- https://help.ubuntu.com/stable/ubuntu-help/shell-keyboard-shortcuts.html.en

# Help
- `man [command]`: displays the online manual.
 - `man -k [command]` : search manual pages associated to the command.
- `command --help/-h` : get help about a command.

# SSH
## 1. Simple connextion

  1. ennable connextion to ssh client:  
  	- `ssh [IP/Host Name]`
  	- `ssh username@hostname`
  2. close connection:
  	- `exit`

## 2. Connect to VM through ssh
On VM do:
Setting > Network > Advanced > Port Forwarding, then set the SSH parameter:
  - Name: SSH
  - Protocol: TCP
  - Host IP: 127.0.0.1
  - Host Port: 2222
  - IP Guest: Empty
  - Port Guest: 22

Run:
  - `ssh adminuser@127.0.0.1 -p 2222`
  - `ssh adminuser@localhost -p 2222`

  https://medium.com/@pierangelo1982/setting-ssh-connection-to-ubuntu-on-virtualbox-af243f737b8b

## 3. Transferring and Copying Files
- `scp source destination`: copy source to destination.
  - local to remote: `scp file.txt adminuser@localhost:/path`.
  - remote to local: `ssh adminuser@127.0.0.1 -p 2222 > ssh scp file.txt dgarrido@192.168.100.13:~/`
- `sftp host`: start a secure file transfer session with host.
  - `sftp -P 2222 adminuser@localhost`
  - `l[command]`: run command in local device.
    - `lls`: `ls` in local device.
  - `put source destination`: upload local source in remote destination.
- `ftp host`: start a file transfer session with host.

# Swithching Users and Running Commands as otherwise
- `useradd nameuser`: create new user.
- `passwd username`: give/change password.
- `su [username]`: switch user or become superuser if username is not provided. Requires the user's password.
  - `-`: a hyphen is used to provide an environment.
  - `-c command`: specify a command to be executed.
- `whoami`: displays the effective username.
- `sudo [username]`: execute a command as another user, typically the superuser if username is not provided. Requieres the current user's password.
  - `-l`: list available commands.
  - `command`: run command as root.
  - `-u root command`: same as above.
  - `-u user command`: run as user.
  - `su`: swith to the superuser account.
  - `su -`: swith to the superuser account with root's environment.
  - `su - username`: switch to the username account.
  - `-s`: start a shell.
  - `-u root -s`: same as above.
  - `-u user -s`: start a shell as user.
- `visudo`: edit the /etc/sudoers file for configurations of user's privileges.

# Permissions
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

# Directories
- `ls [path]`: Lists directory contents.
  - `-l`: long listing format. information: permissions, number of links, owner name, group name, number of bytes in the file, las modification time, file name.
  - `-a, --all`: do not ignore entries startinh with . (hidden files)
  - `-F`: reveal the type, / Directory, @ Link, * Executable.
  - `-t`: list files by time in descending order.
  - `-r`: reverse order.
  - `-R`: list files recursively
- `tree [path]`: similar to ls -R, but creates visual output.
  - `-d`: list directories only.
    - `ls -d */`.
  - `-C`: colorize output.
- `cd [path]`: Changes the current directory.
 - `cd .`: this directory
 - `cd ..` : the parent directory
 - `cd -, $OLDPWD` : change to the previous directory
 - `cd`: back to the home directory
- `./command`: execute command in the current dir and not in $PATH.
- `path/command`: execute command in this full path and not in $PATH.
  - `/bin/cat`
  - `/home/dgarrido/my-cat/cat`
- `pwd`: Displays the present working directory.
- `mkdir directory`: create a directory.
  - `-p:` make parent directories as needed.
- `rmdir directory`: remove an empty directory.
  - `-p`: remove directory an its ancestors.
- `rm file/dir`: remove files or directories.
  - `-f`: force, ignore nonexistent files and arguments, never prompt.
  - `-i`: prompt before every removal.
  - `-r`: remove directories an their contents recursively.
  - `-d`: remove empty directories.
- `mv old_name new_name`: rename folder/file.
- `touch file`: make a file.
- **path with space**: "space folder" or space\ folder.

# Search files
- `find [path][expression]`: recursively finds files in path that match expression. If no arguments are supplied it find all files in the current directory.
  - `-name pattern`: find files and directories that match pattern.
      - `find /path -name Django`
      - `find /path -name *v`: return all files that contains "v" in its name.
  - `-iname pattern`: like -name, but ignore case (is not case sensitive).
    - `find /path -iname django`
  - `-type [arg]`: match only object of that type.
    - `c`: character (unbuffered) special, default.
    - `d`: directory.
    - `f`: regular file.
  - `-ls`: performs an ls on each of the found items.
  - `-mtime days`: finds files that are days old.
    - `find . -mtime +1 -mtime -5`
  - `-size num`: finds files that are of size num.
    - `find . -size +500M`
  - `-newer file`: finds files that are newer than file.
    - `find . -type d -newer file.txt`: search all directories in the current path that are newer than file.txt.
  - `-exec command {} \;` : run command against all the files that are found.
- `locate pattern`: faster than the find command, this queries an index (lookup table). Must be updated for find new files.
  - `locate commands.md` > /home/dgarrido/Desktop/Development/Linux/commands.md

# Deleting, Copying, Moving, and Renaming Files.
  - Removing Files.
    - `rm file`: remove file.
    - `rm -f file`: force removal and never prompt for confirmation.
  - Copying Files.
    - `cp src_file1 ... src_fileN dest_dir1 ... dest_dirM`: copy source files to destinations dirs. If in destination the file exist it is overwritten.
      - `-i`: run in interactive mode, ask before overwrite.
      - `-r src_dir dest_dir`: copy source directory recursively to destination.
  - Moving and Renaming files.
    - `mv`: move or rename files and directories.
      - `mv oldname newname`.
      - `mv source destination`.
      - `-i`: run in interactive mode, ask before overwrite.
  - Sorting Data.
    - `sort file`: sort text in file.
    - `-r`: sort in reverse order.
    - `-u`: sort unique.
    - `-k F`: sort by key. F is the field number or column by default is the first column in alphabetical order.
  - Creating a Collection of Files.
    - `tar [-] c|x|t f tarfile [pattern]`: create, extract or list contents of a tar archive using pattern, if supplied.
    - `-f file`: use this file.
    - `-c`: create a tar archive.
      - `tar -cf tardir.tar dir`
    - `-x`: extract files from the archive.
      - `tar -xf tadir.dar`
    - `-t`: display the table of contents (list).
      - `tar -tf tardir.tar`
      - `unzip -l file.zip`
    - `-v`: be verbose.
    - `-z`: use compression.
      - `tar -zcf tardir.tgz dir`
  - Compressing Files To Save Space
    - `gzip file`: compress files.
    - `gunzip file.gz`: Uncompress files.
    - `gzcat/zcat`: concatenates compressed files.
  - Disk Usage.
    - `du file/dir`: estimates file usage.
      - `-k`: display sizes in kilobytes.
      - `-h`: display size in human readable format.

# Displaying the Contents of files
- `cat file1 .. fileN`: concatenate files and print on the standard output.
  - `-n`: number all output lines.
- `tac file`: display the same but in reverse order.
- `more file`: browse through a text file.
- `less file`: more features than more.
- `head -n lines
qes file`: output the beginning (or top) portion of file.
- `tail -n lines file`: output the ending (or bottom) portion of file.
  - `-f`: output appended data as the file grows, display data as it is being written to the file.
- `nano file`: nano editor.
- **Vi - Editor**
  - `vi file`: vi editor.
  - `vim file`: same as vi, but more features.
  - `view file`: starts vim in read-only mode.
  - `vimtutor`: display manual.
  - **Vi Modes**: Command - Esc, Insert - i I a A, Line - :.
    - **Vi - Command mode and navigation**
      - `k`: up one line.
      - `j`: down one line.
      - `h`: left one character.
      - `l`: right :one character.
      - `w`: right one word.
      - `b`: left one word.
      - `^`: go to the beginning of the line, alt + ^ keys twice.
      - `$`: go to the end of the line.
      - `esc`: exit from command mode.
      - `gg`: move to the first line.
      - `G`: move to the end of the file.
    - **Vi - Insert mode**
      - `i`: insert at the cursor position.
      - `I`: insert at the beginning of the line.
      - `a`: append after the cursor position.
      - `A`: append at the end of the line.
    - **Vi - Line mode**
      - `:w`: writes (save) the file.
      - `:w!`: force the file to be saved.
      - `:q`: quit.
      - `q!`: quit without saving changes.
      - `wq!`: write and quit.
      - `:x`: sames as :wq.
      - `:n`: positions the cursor at line n.
      - `:$`: positions the cursor on the last line.
      - `:set nu`: turn on line numbering.
      - `:set nonu`: turn off line numbering.
      - `:help [subcommand]`: get help.
    - **Vi - Repeating Commands by preceding it with a number**
      - `5k`= Move up a line 5 times.
      - `<N>i<Text><ESC>`=Insert<Text> N times
      - `80i_<Esc>` = Insert 80 "\_" characters
    - **Vi - Deleting Text**.
      - `x`: delete a character.
      - `dw`: delete a word.
      - `dd`: delete a line.
      - `D`: delete from the current position.
    - **Vi - Changing Text**
      - `r`: replace the current character.
      - `cw`: change the current word.
      - `cc`: change the current line.
      - `c$`: change the text from the current position.
      - `C`: sames as c$.
      - `~`: reverse the case of a character.
    - **Vi - Copying and Pasting**
      - `v`: highlight text.
      - `V`: highlight multiples line.
      - `yy`: yank (copy) the current line.
      - `y <position>`: yank the <position>.
      - `p`: paste the most recent deleted or yanked text.
    - **Vi - Undo/Redo**
      - `u`: Undo.
      - `Ctrl-R`: Redo.
    - **Vi - Searching**
      - `/<pattern>`: start a forward search.
        - `n`: move to the next item.
        - `N`: move to the next previous item.
      - `?<pattern>`: start reverse search.
        - `n`: move to the next item.
        - `N`: move to the next previous item.
  - **Emacs editor**
    - `emacs [file]`: edit file in emacs GUI.
    - `emacs -nw [file]`: edit file using emcas on terminal.
    - `C-<char>`:Ctrl while pressing <char>.
    - `M-<char>`: "Meta" key (alt key) while pressing <char>.
    - `M-<char>`: esc, then type <char>.
    - **Emacs - Commands**
      - `C-h`:help.
      - `C-x C-c`: exit.
      - `C-x C-s`: save the file.
      - `C-h t`: built-in tutorial.
      - `C-h k <key>`: describe key.
    - **Emacs - Navigation**
      - `C-p`: previous line.
      - `C-n`: next line.
      - `C-b`: backward one character.
      - `C-f`: forward one character.
      - `M-f`: forward one word.
      - `M-b`: backward one word.
      - `C-a`: go to the beginning of the line.
      - `C-e`: go to the end of the line.
      - `M-<`: go to the beginning of the file.
      - `M->`: go to the end of the file.
    - **Emacs - Deleting Text**
      - `C-d`: delete a character.
      - `M-d`: delete a word.
    - **Emacs - Copying, Pasting, and Undo**
      - `C-k`: kill (cut).
      - `C-y`: yank (paste).
      - `C-x u`: undo.
    - **Emacs - Searching**
      - `C-s`: start a forward search.
      - `C-r`: start a reverse search.
    - **Emacs - Repeating Commands**
      - `C-u N <command>`: repeat <command> N times.

# Wildcards
- `*`: matches zero or more characters.
  - `*.txt`: matches all the files that end with ".txt".
  - `a*`: to match all files that start with "a".
  - `a*.txt`: to match all files that start with "a" and end with ".txt".
- `?`: matches exactly one character.
  - `?.txt`: to match all files that end with ".txt" and are preceding by exactly one character.
  - `a?`: to match all files that start with "a" and are followed by exactly one character.
  - `a?.txt`: to match all files that start with "a" an then are followed by exactly one more character and end with ".txt".
  - `?`: to match all files of length one.
- `[]`: a character class. Matches any of the characters included between the brackets. Matches exacly one character.
  - `ca[nt]*`: to match all files that start with "ca" and are followed by "n" or "t".
  - `[aeiou]`: to match all files that start with a vowel.
- `[!]`: matches any of the characters not included between the brackets. Matches exactly one character.
  - `[!aeiou]*`: to match all files that do not start with the vowel.
- `[-]`: use two characters seṕarated by a hyphen to create a range in a character class.
  - `[a-g]*`: matches all files that start with a, b, c, d, e, f or g.
  - `[3-6]*`: mathes all files that start with 3,4,5 or 6.
  - `[[:alpha:]]`: matches alphabetic letter (upper and lower case).
  - `[[:alnum:]]`: matches alphanumeric.
  - `[[:digit:]]`: matches digit from 0 to 9.
  - `[[:lower:]]`: matches any lower case letter.
  - `[[:space:]]`: matches spaces, tabs and newline characters.
  - `[[:upper:]]`: matches any upper case letter.
- `\` : escape character, use it if you want to match a wildcard character you must use the escape character before it.
  - `*\?`: match all files that end with a question mark.

# Input, Output and Redireciton

|     I/O Name    | Abbreviation  | File Descriptor |
|:---------------:|:-------------:|:---------------:|
| Standard Input  |     stdin     |        0        |
| Standard Output |     stdout    |        1        |
|  Standard Error |     stderr    |        2        |

- `>` : redirects standard output to a file. Overwrites (truncating) existing contents.
  - `ls here nohere > out.txt` Display the error in the screen.
- `>>` : redirects standard output to a file. Appends to any existing contents.
  - `echo message >> file.txt`
- `<`: redirects input from a file to a command.
  - `sort < file.txt > sortfile.txt`
- `2>`: redirect standard error to a file.
  - `ls here nohere > out.txt 2>err.txt`
- `>/dev/null`: redirect output to nowhere.
  -` ls here nohere >out.txt 2>/dev/null`
- `&`: used with redirection to signal that a file descriptor is being used.
- `2>&1`: combine stderr and standard output.
  - `ls here nohere > out.txt 2>&1`

# Comparing Files
- `diff file1 file2`: compare two files.
  - LineNumFIle1-Action-LineNumFile2. Action = (A)dd (C)hange (D)elete.
  - 1,3c1: display the lines 1-3 from the first file and line 1 from the second file and show (C)hanges.
- `sdiff file1 file2`: side by side comparison.
- `vimdiff file1 file2`: highlight differences in vim.
  - `Ctrl-w w`: go to next window.
  - `:q`: quit (close current window).
  - `:qa`: quit all (close both files).
  - `:qa!`: force quit all.

# Searching in files using Pipes
- `grep pattern file`: display lines matching a pattern.
  - `-i`: perform a search, ignoring case.
  - `-c`: count the number of ocurrences in a file.
  - `-n`: precede output with line numbers.
  - `-v`: invert match. Print lines that don't match.
  - `grep -in ssh commands.md`
- `file file_name`: display the file type.
  - `file sales.data` >> sales.data:ASCII text
  - `file *` >> display all files at the current directory an their types.
- `strings file`: display printable strings. Useful for display content of a binary file in a human readable way.
- `cut [file]`: cut out selected portions of file. If file is omitted, use standard input.
  - `-d delimiter`: use delimiter as the field separator.
  - `-f N`: display the Nth field.
  - `cut file.txt -d'|' -f1,5`: cut file by separator and display the first and fifth element of each line.
- `tr [option] set1 set2 `: translating or deleting characters. Work with standart input.
  - `-c` : complements the set of characters in string.i.e., operations apply to characters not in the given set.
  - `-d` : delete characters in the first set from the output.
  - `-s` : replaces repeated characters listed in the set1 with single occurrence.
  - `-t` : truncates set1 to length of set2.
  - `tr -s "oldChar" "newChar" < file.txt`
- **Pipes**: | Pipe symbol, command-output | command-input.
  - `strings file.mp3 | grep -i artist_name| man`
  - `strings file.mp3 | grep -i artist_name | head -1| cut -d' ' -f2`
  - `cat file.txt | tr -s "oldChar" "newChar"`

# Customizing the Shell Prompt.
- `$PS1`: environment variable in charge of prompt style.
- `\d`: date in weekday month date format, for example, Tue May 26.
- `\h`: hostname up to the first periord.
- `\H`: hostname.
- `\n`: newline, for have multiples lines in the prompt.
- `\t`: current time in 24-hour HH:MM:SS format.
- `\T`: current time in 12-hour HH:MM:SS format.
- `\@`: current time in 12-hour am/pm format.
- `\A`: current time in 24-hour HH:MM format.
- `\u`: username of the current user.
- `\w`: current working directory.
- `\W`: basename of the current working directory.
- `\$`: if the effective UID is 0, a #, otherwise a $.
- Update $PS1 variable in .bashrc.
  - `echo 'export PS1="[\t \u@\h]: \w\$"' >> ~/.bashrc`

# Aliases
Are shortcus use for long commands or that you type often. Add to .bashrc to make them persist between sessions.
- `alias`: display all aliases.
- `alias [name[=value]]`: create an alias, name=value to create a new alias.
- `unalias name`: remove the "name" alias.
  - `-a`: remove all aliases.

# Environment variables
Name/Value pairs. Can change how an applications -behaves.  
- `env`: run a program in a modified environment.
- `printenv ENV_NAME`: display variable to the screen, if ENV_NAME is empty display all variables.
- `echo $ENV_NAME`: Displays arguments or variables to the screen.
  - `echo $PATH`.
- `export ENV_NAME="VALUE"`: allows vars to be used by subshells. Create or update an enviroment variable.
  - `export TZ="US/Central"`
- `unset ENV_NAME`: remove or set to default an enviroment variable.
- `$PATH`: is a list of directories where a command is searched, if the command is found it will be executed else return the message command not found.
  - `which  command`: locate a command.
  - `whereis command`: locate the binary, source, and manual page files for a command.
- `$OLDPWD`: holds the directory that you were previosly in.
- `man command`: at the end of the manual are shown the enviroment variables that are used.

# Processes and Job Control
- **Foreground Processes**: processes that are running in the shell and is not possible type in the shell until the processes end (are running in a first plane).
  - `command ...`
  - `Ctrl-c`: kill the foreground process.
  - `Ctrl-z`: suspend the foreground process.
  - `fg [%num]`: foreground a background process.
- **Background Processes**: processes that are running in a second plane and allow type in the shell while the process is running.
  - `command & ...`
  - `bg [%num]`: background a suspended process.
- `ps`: display process status.
  - `-e`: everything, all processes.
  - `-f`: full format listing.
  - `-u username`: display username's processes.
  - `-p pid`: display information for PID (processes id).
  - `-H or --forest`: display a process tree.
- `pstree`: display processes in a tree format.
- `top`: interasctive process viewer.
- `htop`: interative process viewer, similar  to top, but with additional features.
- `kill [-sig] pid or [%num]`: kill a process by job number or PID.
  - `-l`: display a list of signals
- `jobs [%num]`: list jobs.

# Scheduling Repeated Jobs with Cron

Use cron to schedule and automate tasks.
 - `uptime`: ask the time during which the machine is running.
- `cron`: a time based job scheduling service.
- `crontab`: a program to create, read, update, and delete your job schedules.
- `crontab file`: install a new crontab from file.
  - Create cron file: vi mycron >>
  - Add cron file to crontab: crontab mycron
- `crontab -l`: list your cron jobs.
- `crontab -e`:edit your cron jobs.
- `crontab -r`: remove all of your cron jobs.

Crontab Format:

```
* * * * * command
| | | | |
| | | | +-- Day of the Week (0-6)
| | | +---- Month of the Year (1-12)
| | +------ Day of the Month (1-31)
| +-------- Hour (0-23)
+---------- Minute (0-59)
```
- \# Run very Monday at 07:00 and send output.
  - `0 7 * * 1 /path > out.txt 2> err.txt`
- \# Run at 02:00 every day a python file.
  - `0 2 * * * /home/dgarrido/pyhello.py >> out.txt`
  ```
  #!/usr/bin/python3.6
  print("Hello World")
  ```
- \# Run every 30 minutes a bash script.
  - `0,30 * * * * /home/dgarrido/echohello.sh >> out.txt`
  ```
  #!/bin/sh
  echo "Hello World"
  ```
- \# Another way to do the same thing.
  - `*/2 * * * * /home/dgarrido/pyhello.sh >> outsh.txt`
  ```
  #!/bin/sh
  cd /home/dgarrido
  python3.6 pyhello.py
  ```
- \# Run for the first 5 minutes of the hour
  - `0-4 * * * * /home/dgarrido/Desktop/Development/Linux/dir/run.sh`
  ```
  #!/bin/sh
  cd ~/Desktop/MATERIAL_DOCENTE/7_septimo/Primavera/MIR/tareas/T1
  /home/dgarrido/anaconda3/envs/opencvenv/bin/python main.py 'television/mega-2014_04_17.mp4' 'comerciales/'
  ```

Shortcuts:
- `@yearly` = `0 0 1 1 *`
- `@annually` = `0 0 1 1 *`
- `@monthly` = `0 0 1 * *`
- `@weekly` = `0 0 * * 0`
- `@daily` = `0 0 * * *`
- `@midnight` = `0 0 * * *`
- `@hourly` = `0 * * * *`

# Shell History and Autocompletion
Executed commands are added to the history. Shell history can be displayed and recalled. Shell history is stored in memory and on disk.
```
~/.bash_history
~/.history
~/.histfile
```
## History Command
- `history`: displays the shell history.
- `HISTSIZEFILE`:controls the number of commands to retain in history file (.bash_history). By default is 2000.
- `HISTSIZE`: controls the number of commands to retain in history.
  - `export HISTSIZE=1000`. By default is 1000.

## ! Syntax
- `!N`: repeat command line number N.
- `!-N`: repeat command line number N in reverse order.
- `!!`: repeat the previous command line.
- `!string`: repeat the most recent command starting with "string".
- `!:N <Event><Separator><Word>`
  - `!`: represent a command line (or event)
  - `:N`: represents a word on the command line. 0=command, 1=first argument, etc.
  - `!^`: represents the first argument, equal to !:1.
  - `!$`: represents the last argument.

## Searching Shell History
- `Ctrl-r`: reverse shell history search. Press again to move to the next.
- `Enter`: execute the command.
- `Arrows`: edit the command.
- `Ctrl-g`: cancel the search.

# Installing and Managing Software
Managing software for DEB based distros. APT - Advanced Packaging Tool.

- `apt-cache search string`: search for string.
- `apt-get install [-y] package`: install package.
- `apt-get remove package`: remove package, leaving configuration.
- `apt-get purge package`: remove package, deleting configuration.
- `apt-cache show package`: display information about package.
- `dpkg -l`: list installed packages.
- `dpkg -S /path/to/file`: list file's package.
- `dpkg -L package`: list all files in package.
- `dpkg -i package.deb`: install package.


# Shell Scripting
Contain a series of commands, an interpreter executes commands in the script. Anything you can type at the command line, you can put in a script. Great for automating tasks.
- `#!`: this symbol is know as **Shebang** (shark bang) and is used at the first line preceded by the path of the interpreter to use for run the script (can not be used more than one interpreter). If a script does not contain a shebang the commands are executed using the shell.
  ```
  #!/bin/bash
  echo "Hola Mundo"
  ```

  ```
  $./script.sh
  Hola Mundo
  ```

- `sh path/script.sh`: execute a .sh file without the shebang line with the path to the bash interpreter, this file must have execution permission. The same result using `/bin/bash path/file.sh`
- `.path/script.sh`: execute a .sh file with the shebang line.
- **Variables**: name-value pairs that storage locations that have a name. Are case sensitive, can't start with a `number` or contain `-` or `@`.
  - **syntax**: `VARIABLE_NAME="value"`.Without space.
  - **usage**: `$VARIABLE_NAME` or `${VARIABLE_NAME}` (robust to concatenation)
  - **assign**: `VARIABLE_NAME=$(commands)` or close commands with \` symbol (old notation).
- **Test**: `[ condition-to-test-for ]` (syntax). With space at the start and at the end. More information in `man test`.
  - `-d file`: true if file is a directory.
  - `-e file`: true if file exists.
  - `-f file`: true if file exists and is a regular file.
  - `-r file`: true if file is readable by you.
  -` -s file`: true if file exists and is not empty.
  - `-w file`: true if the file is writable by you.
  - `-x file`: true if the file is executable by you.
  - `-z string`: true if string is empty.
  - `-n string`: true if string is not empty.
  - `string1 = string2`: true if the strings are equal.
  - `string1 != string2`: true if the strings are not equal.
  - `arg1 -eq arg2`: true if arg1 is equal to arg2.
  - `arg1 -ne arg2`: true if arg1 is not equal to arg2.
  - `arg1 -lt arg2`: true if arg1 is greater than arg2.
  - `arg1 -gt arg2`: true if arg1 is greater than arg2.
  - `arg1 -ge arg2`: true if arg1 is greater than or equal to arg2.
- **if/elif/else**
  ```
  if [ condition-is-true ]
  then
    command N
  elif [ condition-is-true ]
  then
    command N
  else
    command N
  fi
  ```
- **For loop**
  ```
  for VARIABLE_NAME in ITEM_1 ITEM_N
  do
    command 1
    command 2
    command N
  done
  ```
- **Positional Parameters**: are variables, the zero position is the command or script and the rest are arguments or parameters.
  - `$0`: script.
  - `$n`: parameter n-th, n>1.
  - `$@`: all parameters.
  - **example**:

    ```
    $ ./archive_user.sh chet joe

    #!/bin/bash
    echo "Executing script: $0"
    for USER in $@
    do
      echo "Archiving user: $USER"
      # Lock the account
      passwd –l $USER
      # Create an archive of the home directory.
      tar cf /archives/${USER}.tar.gz /home/${USER}
    done
    ```

- **Accepting User Input (STDIN)**: Use `read` command to accepts standard input.
  - **syntax**: `read -p "PROMPT" VARIABLE`
  - **example**:

    ```
    #!/bin/bash
    read –p "Enter a user name: " USER
    echo "Archiving user: $USER"

    # Lock the account
    passwd –l $USER

    # Create an archive of the home directory.
    tar cf /archives/${USER}.tar.gz /home/${USER}
    ```

# LVM: Logical Volume Manager

LVM introduces layers of abstraction including:
- Physical Volumes (PVs).
- Volume Groups (VGs).
- Logial Volumes (LVs)

Create one or more PVs from unused devices, create a VG from those one or more PVs, create one or more LVs from the VG. A LV is like a disk partition.

Uses:
1. **Flexible Capacity**: create file systems that extend across multiple storage devices.
2. **Easily Resize Storage While Online**: expand or shrink file systems in real-time while the data remains online and fully accessible.
3. **Online Data Relocation**: easily migrate data from one to anothe while online.
4. **Convenient Device Naming**: human-readable device names of your choosing.
5. **Disk Striping**: increase throughput by allowing your system to read data in parallel.
6. **Data Redundancy/Data Mirroring**: increase fault tolerance and reliability by having more than one copy of your data.
7. **Snapshots**: create point-in-time snapshots of your file systems.

- `lvmdiskscan`: list devices that may be used as physical volumes.
- `lsblk`: list block devices.
  - `-p`: print full device paths.
- `df`: report file system disk space usage.
  - `-h`: print sizes in a human readable format, in porwers of 1024.
- `fdisk`: manipulate disk partition table.
  - `-l`: list the partition tables for the specified devices and then exit.

## Physical Volume (PV)
- `pvcreate device`: create a physical volume. LVM will only create a pv label on a device if it is not currently in use.
  - `pvcreate /dev/sdb1`
- `pvremove path`: remove physical volume.
- `pvmove pv1_path pv2_path`: migrate data from PV1 to PV2. This task can be done online.
- `pvs`: list of physical volumes.
- `pvdisplay`: display physical volumes information.
  - `-m`: display the mapping of physical extents to LVs and logical extents.

## Volume Group (VG)
- `vgcreate vg_name pv1_name ... pvn_name`: create a volume group.
- `vgremove vg_name`: remove a volume group.
- `vgextend vg_name pv1_name ... pvb_name`: extend a volume group.
- `vgreduce vg_name pv1_name ... pvb_name`: reduce a volume group.
- `vgs`: list of volume groups.
- `vgdisplay`: display volume groups information.

## Logical Volume (LV)
- `lvcreate -L Size -n lv_name vg_name`: create a logical volume from a volume group.
  - `lvcreate -L 20G -n lv_data vg_app`
  - `lvcreate -l 10%FREE -n lv_logs vg_app`
- `lvremove lv_path`: remove a logical volume.
- `-m NumberCopies`: specifies the number of mirror images in addition to the original LV image. The mirror are allocated in different partitions/devices.
- `lvextend -L +Size -r lv_path`: extend size of a logical volume.
  - `resize2fs lv_path`: if `-r` argument is not given this solve the mistake.
- `lvs`: list of logical volumes.
  - `-a`: display all logical volumes, including mirrors.
- `lvdisplay`: display information about a logical volume. Shows the attribute of LVs, like size, path, read/write status, snapshot, information, etc.
  - `-m`: display the mapping of logical extents to PVs and physical extents (PEs). For each 5 GB are attaching 1280 MB  to metadata.
- `mkfs -t type device`: build a linux filesystem.
  - `mkfs -t ext4 /dev/vg_app/lv_data`
- `mount device /dir`: mounts a storage device or filesystem, making it accessible and attaching it to an existing directory structure.
  - `mount /dev/vg_app/lv_daa /data`
  - ```
    # /etc/fstab:
    /dev/vg_app/lv_app /app ext4 defaults 0 0

    hostname:/# mount /app
    ```
- `umount /dir`: unmount file systems. Must be executed before to remove the LV.
