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

# Aliases
Are shortcus use for long commands or that you type often. Add to .bashrc to make them persist between sessions.
- `alias`: display all aliases.
- ` alias shortName="your custom command here"`: create an alias.
- `unalias name`: remove the "name" alias.
  - `-a`: remove all aliases.