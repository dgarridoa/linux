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