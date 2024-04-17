# Bash for Beginners

- [Course by Microsoft developer](https://youtube.com/playlist?list=PLlrxD0HtieHh9ZhrnEbZKhzk0cetzuX7l&si=3CpA5zFFFR3TXtb_)

## Get Help

- To get help abot any command 
    - help <cmd>
    - <cmd> --help
    - man <cmd>

- In man page, check below most important sections 
    - Name
    - Synopsis
    - Examples (to search, type forward slash followed by seach text)
    - Description & options for more details if required

- In man pages square brackets denotes optional parameters, pipes denotes one option or the other

## Directories

- To get help in man page, use letter h. checkout navigation shortcuts
- `pushd` adds directory to top of stach
- `popd` remove entries from directory stack
- `dirs -v` display directoy stack
- `tree` to list as tree

## List

- In `ls` you can search by in cluding an `*` in command
    - `ls S*` list all files that start with capital S
    - `ls [SC]*` list all files the start with capital S or capital C
    - `ls *.md` `*` for any (zero or more) character
    - `ls *.??` `?` for any (one) character 
    - `ls [[:upper:]]*` begins with uppercase letter only

## Find Files

- `whereis bash` locate the binay, source & man-pages files of a command
- `which bash` locate the executable only for a command
- `find . -iname file.txt` search directory for givern file name or expressio `find /home -name "*.md"`

- `mkdir -p a/b/c/d` `-p` denotes parent, it allows to create nested directories at once without throwing errors & create all folders in the way

## Content View

- `head -n <n> file` && `tail -n <n> file` display <n> lines from file
- `more file` && `less file` gives experience like text editor. more & less are similar with less providing more options like (search, navigate, ...etc) whatever you get in man page
- `grep <pattern> file` display only lines that match pattern

## Variables

- `echo $var` print environemnt variable
- `export var=value` set environment variable (only for current session)
- to persist variable between sessions (new terminals), add export statement to one of startup files, ex:
    - `/etc/profile` for system wide settings (affects all users)
    - `~/profile` or `~/.bashrc` for user specific settings

## Redirection & Pipelines

- to redirect the standard output use `>` operator. `ls -l > output` override output file, to append `ls -l >> output`
- to redirect standard error, use `2` file descriptor to denote errors `ls -l ./none 2> error`
- redirect both `ls -l ./none >> error 2>&1` or use newer syntax `ls -l &> output` which doesn't support append & will override `output` file
- pipes `ls -l | less` or `ls -l | sort`

## Permissions

- `-rw-rw-rw-` file type (file or directory), owner, group & other permissions
- `chown newOwner file` change file owner
- `chown :newGroup file` change file group, or `chgrp newGroup file` but only owner can use this command to change the group ($USER=owner)
- `chmod +x script.sh` to make file executable for all, or use octal `chmod 664 file` to remove execute permission for all

## Scripts

- shebang `#!` is placed in first line to teel environemnt which interpreter we're gonna be using, in this case bash `#!/bin/bash`
- you can excute script by providing explict path `./bath/to/script.sh` or include scripts in common directory, ex `~/.local/bin`.
- for user scoped scripts, it's a good idea to keep them in `.local/bin` directory under user home `~/.local/bin`
- convention is to seperate multi-names variable with underscores
- `readonly` keyword to declare constants in script file

```bash
#!/bin/bash

# Welcome to your first Bash Script

hello_message='Hello World!'

current_dir=$(pwd)

readonly constant="readonly text!"

constant='raise error, const cannot be changed'

echo "$hello_message from $current_dir, const $constant"
```

## Conditional Statements

```bash
number=123213;

if [ $((number % 2)) -eq 0 ];
then
    echo "number $number is even!";
else
    echo "number $number is odd!"
fi
```

## Case Statement

- `;;` used to exit switch statement (break)

```bash
read -p "enter a value: " n

case $n in
    1|2)
        echo "one or two";;
    [3-9])
        echo "three to nine";;
    ??)
        echo "two chars";;
    *.txt)
        echo "txt file";;
    *)
        echo "other";;
esac
```

## Functions

- define function before you call it

```bash
check_even () {
    local mod=2
    echo "The value of mod is $mod"
    if [ $(("$1" % $mod)) -eq 0 ]
    then 
       echo "The number $1 is even!";
    else 
       echo "The number $1 is odd!"
    fi
}

number=2344
check_even $number
```

## Loops

```bash
# while
counter=1
while [[ "$counter" -le 10 ]]; do
    echo "The counter is at: $counter"
    counter=$((counter + 1))
done
echo "The count has finished."

# until
counter=1
until [[ "$counter" -gt 10 ]]; do
    echo "The counter is at: $counter"
    counter=$((counter + 1))
done
echo "The count has finished."

# for
services=("loadbalancer" "virtualmachine" "storage")

for i in "${services[@]}"
do
   echo $i
done

# for 2
for (( i=0; i<5; i=i+1 )); do
    echo "The counter is at: $i"
done

# break
while true; do
  read -p "Enter a number between 1 and 25: " n
  if [[ $n -ge 1 && $n -le 25 ]]; then
    echo "You entered $n"
  else 
    echo "You didn't enter a number in range, goodbye."
    break
  fi
done

echo "Break happened"
```