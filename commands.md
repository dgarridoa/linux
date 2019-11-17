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
1. uptime: ask the time during which the machine is running.

## User
1. create new user: useradd nameuser
2. give password: passwd username
3. ask actual user: whoiam
4. switch of user:su - username

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
    - ls -d \*/.
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

## Search
- find [path][expression]: recursively finds files in path that match expression. If no arguments are supplied it find all files in the current directory.
  - -name pattern: find files and directories that match pattern.
      - find /path -name Django
      - find /path -name \*v: return all files that contains "v" in its name.
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

## Deleting, Copying, Moving, and Renaming Files.
  - Removing Files.
    - rm file: remove file.
    - rm -r dir: remove the directory and its contents recursively.
    - rm -f file: force removal and never prompt for confirmation.
  - Copying Files.
    - cp source_file destination_file: copy source_file to destination_file. If destination exist is overwritten.
    - cp src_file1 ... soruce_fileN dest_dir: copy source_files to destination_directory. If file exist in destination_directory the file is overwritten.
    - cp -i: run in interactive mode, ask before overwrite.
    - cp -r source_directory destination: copy src_directory recursively to destination.
  - Moving and Renaming files.
    - mv: move or rename files and directories.
    - mv oldname newname.
    - mv source destination.
    - mv -i source destination: ask before overwrite.
  - Sorting Data.
    - sort file: sort text in file.
    - -r: sort in reverse order.
    - -u: sort unique.
    - -k F: sort by key. F is the field number or column by default is the first column in alphabetical order.
    - sort -ruk2 file
  - Creating a Collection of Files.
    - tar [-] c|x|t f tarfile [pattern]: create, extract or list contents of a tar archive using pattern, if supplied.
    - -c: create a tar archive.
      - tar -cf tardir.tar dir
    - -x: extract files from the archive.
      - tar xf path/tadir.dar
    - -t: display the table of contents (list).
      - tar tf tardir.tar
      - unzip -l file.zip.
    - -v: be verbose.
    - -z: use compression.
      - tar -zcf tardir.tgz dir
    - -f file: use this file.
  - Compressing Files To Save Space
    - gzip: compress files.
    - gunzip: UNcompress files.
    - gzcat or zcat: concatenates compressed files.
  - Disk Usage.
    - du: estimates file usage.
    - du -k: display sizes in kilobytes.
    - du -h: display size in human readable format.

## Displaying the Contents of files
- cat file: display the contents of file.
  - -n: display line number.
- more file: browse through a text file.
  - space: move to the next page.
  - enter: move to the next line.
- less file: more features than more.
- head -n_lines file: output the beginning (or top) portion of file.
- tail -n_lines file: output the ending (or bottom) portion of file.
  - tail -f file: display data as it is being written to the file.
- nano file: nano editor.
- Vi - Editor
  - vi file: vi editor.
  - vim file: same as vi, but more features.
  - view file: starts vim in read-only mode.
  - vimtutor: display manual.
    - Vi - Command mode and navigation
      - k: up one line.
      - j: down one line.
      - h: left one character.
      - l: right one character.
      - w: right one word.
      - b: left one word.
      - ^: go to the beginning of the line.
      - $: go to the end of the line.
      - esc: exit from command mode.
      - G: move to the end of the file.
    - Vi - Insert mode
      - i: insert at the cursor position.
      - I: insert at the beginning of the line.
      - a: append after the cursor position.
      - A: append at the end of the line.
    - Vi - Line mode
      - :w: writes (save) the file.
      - :w!: force the file to be saved.
      - :q: quit.
      - q!: quit without saving changes.
      - wq!: write and quit.
      - :x : sames as :wq.
      - :n: positions the cursor at line n.
      - :$: positions the cursor on the last line.
      - :set nu: turn on line numbering.
      - :set nonu: turn off line numbering.
      - :help [subcommand]: get help.
    - Vi Modes: Command - Esc, Insert - i I a A, Line - :.
    - Vi - Repeating Commands by preceding it with a number:
      - 5k= Move up a line 5 times.
      - 80i<Text><ESC>=Insert<Text> 80 times
      - 80i_<Esc> = Insert 80 "\_" characters
    - Vi - Deleting Text.
      - x: delete a character.
      - dw: delete a word.
      - dd: delete a line.
      - D: delete from the current position.
    - Vi - Changing Text
      - r: replace the current character.
      - cw: change the current word.
      - cc: change the current line.
      - c$: change the text from the current position.
      - C: sames as c$.
      - ~: reverse the case of a character.
    - Vi - Copying and Pasting
      - v: highlight text.
      - V: highlight multiples line.
      - yy: yank (copy) the current line.
      - y <position>: yank the <position>.
      - p: paste the most recent deleted or yanked text.
    - Vi- Undo/Redo
      - u: Undo.
      - Ctrl-R: Redo.
    - Vi - Searching.
      - /<pattern>: start a forward search.
        - n: move to the next item.
        - N: move to the next previous item.
      - ?<pattern>: start reverse search.
        - n: move to the next item.
        - N: move to the next previous item.
  - Emacs editor.
    - emacs [file]: edit file in emacs GUI.
    - emacs -nw [file]: edit file using emcas on terminal.
    - C-<char>:Ctrl while pressing <char>.
    - M-<char>: "Meta" key (alt key) while pressing <char>.
    - M-<char>: esc, then type <char>.
    - Emacs - Commands.
      - C-h:help.
      - C-x C-c: exit.
      - C-x C-s: save the file.
      - C-h t: built-in tutorial.
      - C-h k <key>: describe key.
    - Emacs - Navigation.
      - C-p: previous line.
      - C-n: next line.
      - C-b: backward one character.
      - C-f: forward one character.
      - M-f: forward one word.
      - M-b: backward one word.
      - C-a: go to the beginning of the line.
      - C-e: go to the end of the line.
      - M-<: go to the beginning of the file.
      - M->: go to the end of the file.
    - Emacs - Deleting Text.
      - C-d: delete a character.
      - M-d: delete a word.
    - Emacs - Copying, Pasting, and Undo.
      - C-k: kill (cut).
      - C-y: yank (paste).
      - C-x u: undo.
    - Emacs - Searching.
      - C-s: start a forward search.
      - C-r: start a reverse search.
    - Emacs - Repeating Commands.
      - C-u N <command>: repeat <command> N times.

## Wildcards
- \*: matches zero or more characters.
  - \*.txt: matches all the files that end with ".txt".
  - a*: to match all files that start with "a".
  - a*.txt: to match all files that start with "a" and end with ".txt".
- ?: matches exactly one character.
  - ?.txt: to match all files that end with ".txt" and are preceding by exactly one character.
  - a?: to match all files that start with "a" and are followed by exactly one character.
  - a?.txt: to match all files that start with "a" an then are followed by exactly one more character and end with ".txt".
  - ?: to match all files of length one.
- []: a character class. Matches any of the characters included between the brackets. Matches exacly one character.
  - ca[nt]\*: to match all files that start with "ca" and are followed by "n" or "t".
  - [aeiou]: to match all files that start with a vowel.
- [!]: matches any of the characters not included between the brackets. Matches exactly one character.
  - [!aeiou]\*: to match all files that do not start with the vowel.
- [-]: use two characters seṕarated by a hyphen to create a range in a character class.
  - [a-g]\*: matches all files that start with a, b, c, d, e, f or g.
  - [3-6]\*: mathes all files that start with 3,4,5 or 6.
  - [[:alpha:]]: matches alphabetic letter (upper and lower case).
  - [[:alnum:]]: matches alphanumeric.
  - [[:digit:]]: matches digit from 0 to 9.
  - [[:lower:]]: matches any lower case letter.
  - [[:space:]]: matches spaces, tabs and newline characters.
  - [[:upper:]]: matches any upper case letter.
- \ : escape character. Use if you want to match a wildcard character you must use the escape character before it.
  - \*\? : match all files that end with a question mark.

## Input, Output and Redireciton

|     I/O Name    | Abbreviation  | File Descriptor |
|:---------------:|:-------------:|:---------------:|
| Standard Input  |     stdin     |        0        |
| Standard Output |     stdout    |        1        |
|  Standard Error |     stderr    |        2        |

- \> : redirects standard output to a file. Overwrites (truncating) existing contents.
  - ls here nohere > out.txt. Display the error in the screen.
- \>> : redirects standard output to a file. Appends to any existing contents.
  - echo message >> file.txt.
- <: redirects input from a file to a command.
  - sort < file.txt > sortfile.txt
- &: used with redirection to signal that a file descriptor is being used.
- 2>&1: combine stderr and standard output.
- 2>file: redirect standard error to a file.
  - ls here nohere > out.txt 2>err.txt
- \>/dev/null: redirect output to nowhere.
  - ls here nohere >out.txt 2>/dev/null.

## Comparing Files
- diff file1 file2: compare two files.
  - LineNumFIle1-Action-LineNumFile2. Action = (A)dd (C)hange (D)elete.
  - 1,3c1: display the lines 1-3 from the first file and line 1 from the second file and show (C)hanges.
- sdiff file1 file2: side by side comparison.
- vimdiff file1 file2: highlight differences in vim.
  - Ctrl-w w: go to next window.
  - :q: quit (close current window).
  - :qa: quit all (close both files).
  - :qa!: force quit all.

## Searching in files using Pipes
- grep pattern file: display lines matching a pattern.
  - -i: perform a search, ignoring case.
  - -c: count the number of ocurrences in a file.
  - -n: precede output with line numbers.
  - -v: invert match. Print lines that don't match.
- file file_name: display the file type.
  - file sales.data >> sales.data:ASCII text
  - file * >> display all files at the current directory an their types.
- strings file: display printable strings. Useful for display content of a binary file in a human readable way.
- cut [file]: cut out selected portions of file. If file is omitted, use standard input.
  - d delimiter: use delimiter as the field separator.
  - f N: display the Nth field.
  - cut file.txt -d'|' -f1,5: cut file by separator and display the first and fifth element of each line.
- tr [option] set1 "set2 : translating or deleting characters. Work with standart input.
  - -c : complements the set of characters in string.i.e., operations apply to characters not in the given set
  - -d : delete characters in the first set from the output.
  - -s : replaces repeated characters listed in the set1 with single occurrence
  - -t : truncates set1.
  - tr -s "oldChar" "newChar" < file.txt
- Pipes: | Pipe symbol, command-output | command-input.
  - strings file.mp3 | grep -i artist_name| man
  - strings file.mp3 | grep -i artist_name | head -1| cut -d' ' -f2.
  - cat file.txt | tr -s "oldChar" "newChar".

## Environment variables
- echo : Displays arguments or variables to the screen.
- $PATH: is a list of directories where a command is searched, if the command is found it will be executed else return the message command not found.
  - which  command: display the path where is located a command
- $OLDPWD: holds the directory that you were previosly in.
