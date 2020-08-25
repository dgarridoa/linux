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

# Swithching Users and Running Commands as otherwise

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


# Managing Users and Groups
Acounts have a:
  - Username (or login ID), root (0)
  - UID (user ID)
  - GID (group ID)
  - Coments
  - Shell
  - Home directory location
  - `/etc/passwd`: file that store all the information above. username_password:UID:GIP:comments:home_dir:shell. 
  - `/etc/shadow`: encrypted passwords are stored in this file.
  - `/etc/group`: file with the group details. group_name:password:GID:account1,accountN.
  - `/etc/gshadow`: encrypted group passwords are stored in this file.
  - `etc/shells`: file with the list of available shells.
    - to prevent interative use of an account, use **/usr/bin/nologin** or **/bin/false**.
- `useradd username`: create new user.
  - `-c"COMMENT"`: comments for the account.
  - `-m`: create the home directory if it does not exist.
  - `-d DIRETORY`: specify the home directory.
  - `-r`: create a system account, typically for run application.
  - `-s '/shell/path`: the path to the user's shell.
  - `-u UID`: to specify the UID.
  - `-g GROUP`: to specify the default group
  - `-G GROUP1, GROUPN`: additional groups.
- `passwd username`: give/change password.
- `userdel username`: delete user.
  - `-r`: delete user home directory.
- `usermod [options] username`: to modify an existing user.
- `groups username`: display user groups.
- `groupadd groupname`: create new group.
  - `-g GID`: to specify the GID.
- `groupdel groupname`: to delete group.
- `groupmod [options] groupname`: to modify an existing group.
  - `-g GID`: change the group ID to GID
  - `-n GROUP`: rename the group to GROUP.
- `newgrp`: command for swith groups.


# Special Permisions Modes
- 1) setuid = Set User ID upon execution
- 2) setgid = Set Group ID upon execution
- 3) sticky bit: use on a directory to only allow the owner of the file/directory to delete it.
- When a process is started, it runs using the starting user's UID and GID.
- Are ofthen used to allow users on a computer system to run programs with temporarily elevated privilegies.
- Are flags only have effect on binary executable files, then any users able to execute the file will automatically execute the file with the privileges of the file's owners and/or the file's group.
- `find / -perm /4000 -ls`: find setuid files.
- `find / -perm /2000 -ls`: find setgid files.
- `find / -perm /1000 -ls`: find sticky bit directories.
- `chmod u+s/4755 file`: adding the setuid attributte.
  - rw**s**r-xr-x root root 4090 Feb 1 09:47 test
- `chmod u-s/0755 file`: remove the setuid attributte.
- `chmod g+s/2775 file`: adding the setgid attributte.
  - rwxrw**s**r-x root root 4090 Feb 1 09:47 test
- `chmod g-s/0775 file`: remove the setgid attributte.
- `chmod o+s/1777 file`: adding the sticky bit attributte.
  - dwxrwxrw**t** root root 4090 Feb 1 09:47 /tmp
- `chmod o-s/0777 file`: remove the sticky bit attributte