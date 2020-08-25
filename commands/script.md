# Shell Scripting
Contain a series of commands, and an interpreter executes commands in the script. Anything you can type at the command line, you can put in a script. Great for automating tasks.
- `#!`: this symbol is known as **Shebang** (shark bang) and is used at the first line preceded by the path of the interpreter to use for run the script (can not be used more than one interpreter). If a script does not contain a shebang the commands are executed using the shell.
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
  - `-s file`: true if file exists and is not empty.
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
      tar -zcf /archives/${USER}.tar.gz /home/${USER}
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
    tar -zcf /archives/${USER}.tar.gz /home/${USER}
    ```