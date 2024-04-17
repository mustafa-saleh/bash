# Bash Scripting Tutorial for Beginners

- [Free code camp course on Youtube](https://www.youtube.com/watch?v=tK9Oc6AEnR4)
- you can execute script by passing file to bash `bash script.sh` or make file executable `chmod 700 script.sh` or `chmod u+x script.sh` then run it `./script.sh`

## Variables

```bash
echo what is s your name? 
read FIRST_NAME
echo what is your last name?
read LAST_NAME

echo Hello $FIRST_NAME $LAST_NAME
```

## Positional Argument

```bash
echo $1 $2
```

## Piping

- pipe operator `|` used to pass output of one command to another

## redirection

- output redirection operators `>` & append `>>`
- for input 
    - `<` read from file
    - `<<` multi-line text
    - `<<<` string stream

```bash
# 
echo have a good day enjoy bye > file.txt

# pass file as positional argument to wc
wc -w file.txt # print "6 file.txt"

# pass file input to wc
wc -w < file.txt # print only "6"

# to pass multiple line of text with << operator, 'EOF' is random text used to define start & end of multi-line text
cat << EOF
> first line of text
> second line of text
> third line of text
> EOF
# above command print following
first line of text
second line of text
third line of text

# string stream, string must be surrounded with double quotes
wc -w <<< "hello bye"
# pring "2"
```

## Test Operators

- test command cab be used to check if expression is true or not, square brackets can be used to write test

```bash
[ hello = hello ] # space are mandatory
# above command print nothing, to check result
echo $? # $? print result of last executed command
# 0 indicates success exit code
```
