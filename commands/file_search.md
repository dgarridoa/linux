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
- `2>`: redirect standard error to a file and not display error in the screen.
  - `ls here nohere > out.txt 2>err.txt`
- `>/dev/null`: redirect output to nowhere.
  -` ls here nohere >out.txt 2>/dev/null`
- `&`: used with redirection to signal that a file descriptor is being used.
- `2>&1`: combine stderr and standard output.
  - `ls here nohere > out.txt 2>&1`


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