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
  - `sudo !!`: repeat the last command with root previleges.
- `!string`: repeat the most recent command starting with "string".
- `!:N <Event><Separator><Word>`
  - `!`: represent a command line (or event)
  - `:N`: represents a word on the command line. 0=command, 1=first argument, etc.
  - `!^`: represents the first argument, equal to !:1.
  - `!$`: represents the last argument.
  - `!*`: represents all the arguments.
    - `command !*`: run command with the previous arguments.

## Searching Shell History
- `Ctrl-r`: reverse shell history search. Press again to move to the next.
- `Enter`: execute the command.
- `Arrows`: edit the command.
- `Ctrl-g`: cancel the search.