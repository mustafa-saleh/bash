# Advanced Bash Scripting Tutorial

- [Advanced Bash Scripting Tutorial](https://www.youtube.com/watch?v=emhouufDnB4)
- [Bash hackers Wikik Reference](https://github.com/rawiriblundell/wiki.bash-hackers.org)
- [Advanced Bash-Scripting Guide](https://tldp.org/LDP/abs/html/)
- [Bash Guide for Beginners](https://tldp.org/LDP/Bash-Beginners-Guide/html/)

## Expansions

- brace expansion `touch {apple,bananna,cherry}` to create 3 files
- to create 1000 files `touch file_{1..1000}`
- to define step `echo {1..10..2}` or letters `echo {w..d..2}`
- or combined `touch {apple,banana,cherry}_{01..100}{w..d}`

## Redirection

- 1 & 2 represent standard output & error `rm -r * 1> success.txt 2> error.txt`
- to ignore output, send to `/dev/null` `rm -r * 1> /dev/null 2> error.txt`

## Attributes

- to give variable attributes, use declare keyword

```bash
declare -i d=12 # d is integer
declare -r e=34 # e is read-only
declare -l f="LOLCats" # f is lolcats (lowercase)
declare -u g="LOLCats" # g is LOLCATS (uppercase)
```

- built in vavriables are environment variables such as `$HOME $SHELL $SECONDS ...`
- `$SECONDS` define how long session been running (since started), if you use it in script it'll count from script start

## Command Substitution

- use `$(command)` to substitute command

```bash
#!/bin/bash

working_dir=$(pwd)
echo $working_dir

ping_time=$(ping -c 1 example.com | grep 'bytes from' | cut -d = -f 4)
echo $ping_time
```

## Arithematic Operators

- to tell the interpreter that you gonna do math, wrap expression with double parentheses `(( expression ))`
- to assign value to a variable, use `$` for example `val=$(( expression ))`
- expression can use either literal numbers or variables
- you can use below operators `+ - * / % ** += -= *= /= ++ --`
- if you remove doubke parentheses, you will end up with string concatination

```bash
a=2
b=$((a+2)) # note inside expression
echo $a $b

((b+=4))
echo $b # 8

b+=5
echo $b # 85
```

- bash math works only with integers & not floating point numbers, you can use `bc program` to get floating point number

```bash
a=$((1/3))
echo $a # 0

g=$(echo 1/3 | bc -l)
echo $g # 0.3333
```

## Comparison Operators

- use square brackets to denote conditional statement `[[ expression ]]`
- spaces between expression & square brackets are important

```bash
[[ expression ]]
# 1: FALSE
# 0: TRUE

# supported comparison operators, used to compare strings
[[ $a < $b ]]   # Less than
[[ $a > $b ]]   # Greater than
[[ $a <= $b ]]  # Less than or equal to
[[ $a >= $b ]]  # Greater than or equal to
[[ $a == $b ]]  # Equal
[[ $a != $b ]]  # Not equal

# single or double equal signs can be used to test for equality
[[ "cat" == "cat" ]]
echo $? # 0

[[ "cat" = "dog" ]]
echo $? # 1

[[ 20 > 100 ]]
echo $? # 0 becuase > is comparing numbers as strings and 2, 0 is greater value lexically than 1, 0, 0

# to test numbers as integers, we have the same operations but the operators are different
[[ $a -lt $b ]]   # Less than
[[ $a -gt $b ]]   # Greater than
[[ $a -le $b ]]   # Less than or equal to
[[ $a -ge $b ]]   # Greater than or equal to
[[ $a -eq $b ]]   # Equal
[[ $a -ne $b ]]   # Not equal

[[ 20 -gt 100 ]]
echo $? # 1
```

- lexicographical ordering, If the first digits match, then the second digits will be compared; but it compares like a String - that is 10 comes before 2 but 111 comes after 10 (but also after 1000). Because 0 is less than 1. A lexical sort compares the characters in each string as characters, not integral

## Logical Operators

```bash
[[ $a && $b ]]   # AND
[[ $a || $b ]]   # OR
[[ ! $a ]]       # NOT
```

## String Null

```bash
# other operators to test string null or not
[[ -z $a ]]       # Is null
[[ -n $a ]]       # Is not null

a=""
b="cat"
[[ -z $a && -n $b ]] # check a is null & b is not
echo $? # 0
```

## Conditional Statement

```bash
# given
fruit="apple banana banana cherry"
a="hello"
b="world"
c=$a$b
```

### String Concatination

```bash
echo $c # helloworld

# using expansion parameter
echo ${a} # hello

# use # sign to get length
echo ${#a} # 5
```

### Substring

```bash
# substring from index
echo ${c:3} # loworld

# substring length
echo ${c:3:4} # lowo

# substring count from end, space is required
echo ${c: -4} # orld

# substring count from end, length
echo ${c: -4:3} # orl
```

### String Replacement

```bash
# replace substring
echo ${fruit/banana/durian} # apple durian banana cherry

# replace all substring
echo ${fruit//banana/durian} # apple durian durian cherry

# replace substring only if in beginning
echo ${fruit/#apple/durian} # durian banana banana cherry
echo ${fruit/#banana/durian} # apple banana banana cherry

# replace substring only if in end
echo ${fruit/%cherry/durian} # apple banana banana durian

# replace substring using matching
echo ${fruit/c*/durian} # apple banana banana durian
echo ${fruit/ba*/durian} # apple durian
```

## Text Style

- Style to highlight some output values, there're two ways to style

### ANSI Escape Codes

- `echo -e "\033[34;42mColor Text\033[0m"`
    - `-e` to enable escape sequences, tells bash you'll be using an escaped string following back slash or  escape character
    - `\033[34;42m` escape sequence
        - `\033[` indicate start
        - `34;42` numbers represent various format options, `style;foreground;background`
        - `m` indicates end of sequence
        - `Color Text` string to be formatted
        - `\033[0m` another escape sequence to reset the colors
- [More on text style](https://en.wikipedia.org/wiki/ANSI_escape_code#SGR_(Select_Graphic_Rendition)_parameters)
- [More on foreground & background](https://en.wikipedia.org/wiki/ANSI_escape_code#3-bit_and_4-bit)

```bash
flashred="\033[5;31;40m"
red="\033[31;40m"
none="\033[0m"
echo -e $flashred"ERROR: "$none$red"Something went wrong!"$none
```

### Utility tput

```bash
flashred=$(tput setab 0; tput setaf 1; tput blink)
red=$(tput setab 0; tput setaf 1)
none=$(tput sgr0)
echo -e $flashred"ERROR: "$none$red"Something went wrong!"$none
```

## Date & Printf

- `date` is not part of bash but it's usefull in scripts

```bash
echo $(date)    # Mon Apr 8 12:43:38 PM UTC 2024

# to format, use +"format", check format specifiers in "date" man page
echo $(date +"%d-%m-%Y")    # 08-04-2024
echo $(date +"%H:%M:%S")    # 12:43:38
```

- `printf` is built-in bash, it lets print in sepecifc format, gives more options than `echo`

```bash
# %s for string placeholder
# %04d pad with zero up to 4 digits
printf "Name:\t%s\nID:\t%04d\n" "awad" "123"
# Name:   awad
# ID:     0123
```

- to use both together

```bash
today=$(date +"%d-%m-%Y")
time=$(date +"%H:%M:%S")

# print -v assign output to a variable (d)
printf -v d "Current USER:\t%s\nDate:\t\t%s @ %s\n" $USER $today $time

# quotes are used to preserve the the new lines (\n) because command substiution strips new lines by default
echo "$d"
# Current USER:   vscode
# Date:           08-04-2024 @ 13:03:34
```

## Array

- use arrays to deal with collection of things insstead of one variable
- In bash to declaer a variable is an array, eclose it with parentheses `()`

```bash
a=()    # empty array
b=("apple" "banana" "cherry")   # note no comma's

# to retrieve elements, use zero based index of variable inside curly braces
echo ${b[2]}    # cherry

# you don't have to populate entire elements, you can set any index directly
b[5]="kiwi"     # set index 5
echo ${b[3]}    # 
echo ${b[4]}    # 
echo ${b[5]}    # kiwi

# to print the array
echo ${b[@]}    # apple banana cherry kiwi

# add to end of array
b+=("mango")
echo ${b[@]}    # apple banana cherry kiwi mango

# append to first element
b+="grape"
echo ${b[@]}    # applegrape banana cherry kiwi

# to get last element
echo ${b[@]: -1}    # mango
```

- Bash v4 & above support associative arrays, you can specify a key & value

```bash
declare -A arr
arr[color]=black

# key/value have space, use ""
arr["office address"]="town square"

echo ${arr[color]} ${arr["office address"]}     #black town square
```

## Text File

- `<` & `>` are key operators when working with text files

```bash
# write "some text" to file.txt, create it if not found & override anything in it
echo "some text" > file.txt

# to create file or get rid of it's content
> file.txt

# append to end of file
echo "some other text" >> file.txt

# read a file, example using file as input for loop
i=1
while read f;do
    echo "Line $i: $f"
    ((i+=1))
done < file.txt
# Line 1: some text
# Line 2: some other text

# use file as input for a command
cat < file.txt

# real world example would be to setup some instructions in file & then get it executed one by one, for example connect to an ftp server
ftp -n < fileftp.txt
```

## Here Document

- let's you specify input freely up to specified limit string, it can be handy for displaying long passages of text in script

```bash
# take all input & feed it into the command (cat) until end of string (EndOfText)
# EndOfText should be unique & not repeat in text
# it's usually used for instructions or text that would be tedious to echo out line by line
cat << EndOfText
This is a
multiline
text string
EndOfText

# to make script more readible, use tabs which can be stripped with (-)
cat <<- EndOfText
    This is a
    multiline
    text string
EndOfText
```

- you can use here document as built in way to download or upload documents needed by script, for example below download file (GUTINDEX.01) from ftp server

```bash
# after running script, list files to see "GUTINDEX.01"
ftp -n <<- DoneWithTheUpdate
    open mirros.xmission.com    # connect to ftp
    user anonymous nothinghere  # user & pass
    ascii
    cd gutenberg
    get GUTINDEX.01
    bye     # disconnect ftp
DoneWithTheUpdate
```


## Challenge 1

- write script that print system information to screen & save some information to file
- use variables, text manipulation, string formatting, commadn substitution & redirection to save information to a file

```bash
freespace=$(df -h / | grep -E "\/$" | awk '{print $4}')
bold="\033[1m"
normal="\033[0m"
green="\033[32m"
logdate=$(date +"%Y%m%d")
logfile="$logdate"_report.log

echo -e $bold"Quick system report for "$green"$HOSTNAME"$normal
printf "\tSystem type:\t%s\n" $MACHTYPE
printf "\tBash version:\t%s\n" $BASH_VERSION
printf "\tFree space:\t%s\n" $freespace
printf "\tFiles in dir:\t%s\n" $(ls | wc -l)
printf "\tGenerated on:\t%s\n" $(date +"%m/%d/%y")
echo -e $green"A summary of this report has been saved to $logfile"$normal

cat <<- EOF > $logfile
    This report was authomatically generated by bash script
    It contains some information about the system
EOF

printf "SYS:\t%s\n" $MACHTYPE >> $logfile
printf "BASH:\t%s\n" $BASH_VERSION >> $logfile
```

## If Statement

- the expression usually in single (test) barckets `if [ expression ]` or double (extended test) brackets `if [[ expression ]]`
- It can also be contained within parentheses for integer comparison `if (( expression ))`
- Or have no brackets at all `if expression` whatever is appropriate for the comparison you're doing

```bash
# expression can be followed by semicolon & "then
if expression;then
    echo "True"

# Or you can drop semicolon & put then in new line
if expression
then
    echo "True"
elif expression2;then
    echo "e1 is False, e2 is True"
else
    echo "False"
fi  # close if statement
```

```bash
a=2
if [ $a -gt 4 ]     # if [[ $a -gt 4 ]] also works
then
    echo $a is greater than 4
else 
    echo $a is not greater than 4
fi
```
 	
### [] vs. [[]]
Contrary to [, [[ prevents word splitting of variable values. So, if VAR="var with spaces", you do not need to double quote $VAR in a test - eventhough using quotes remains a good habit. Also, [[ prevents pathname expansion, so literal strings with wildcards do not try to expand to filenames. Using [[, == and != interpret strings to the right as shell glob patterns to be matched against the value to the left, for instance: [[ "value" == val* ]].

```bash
a="This is a string"
if [[ $a =~ [0-9]+ ]];then      # =~ denotes regular expression 
    echo "There're number in string: $a"
else
    echo "There're no numbers in string: $a"
fi
```

## Loops, While & Until

```bash
# while loop
i=0
while [ $i -lt 10 ];do
    echo i:$i
    ((i++))
done

# until loop
j=0
until [ $j -gt 10 ];do
    echo j:$j
    ((j++))
done
```

## For Loop

```bash
# for loop
for i in 1 2 3;do
    echo $i
done

# using expansion curly braces
for j in {1..20};do
    echo $j
done

# in bash v4 & above, you can specify an interval
for j in {1..20..2};do
    echo $j
done

# more familiar syntax
for (( i=0; i<=10; i++ ))
do
    echo $i
done

# loop through an array using expansion parameter
arr=("apple" "banana" "cherry")
for i in ${arr[@]}
do
    echo $i
done

# associative array
declare -A arr
arr["name"]="ali"
arr["id"]="1234"
for i in "${!arr[@]}"   # !arr[@] to access keys instead of values, double quotes are used if keys are strings it might contain an space
do
    echo "$i: ${arr[$i]}"
done

# command substitution 
for i in $(ls)
do
    echo $i
done
```

## Case Statement

```bash
a="dog"
case $a in
    cat) echo "cat";;
    dog|puppy) echo "dog";;
    *) echo "other";;
esac
```

## Function Declaration

- functions can help with repeated code blocks

```bash
function greet {
    echo "Hi $1, what a lovely $2"
}
greet jack morning  # function call, print Hi jack, what a lovely morning

# capture all arguments
# $@ special array variable that repersent all arguments given to a function
function numberthings {
    i=1
    for f in $@;do
        echo $i: $f
        ((i++))
    done
}
numberthings $(ls)
numberthings apple banana cherry kiwi
```

## Arguments, Acquiring User Input

- arguments are used to acquire user input
- arguments are special type of variables that specified by the user when the script is run
- arguments are passedto script by typing them after script name
- argumemnts are represented by numbered variables & assigned in the same order they were provided in command line `echo $1` to print first argument
- arguments just like variables, anything with space in it will need to have quotes around it `script.sh "red apple"`
- bash provides an array of arguments which can be accessed by `$@`, for the number of arguments `$#`

```bash
function args {
    for f in $@;do
        echo $f
    done
    echo "Number of arguments: $#"
}
args apple banana cherry kiwi
```

## Flags, getopts (get options)

- flags are specific options that you can use to pass information to a program, they usually start with a dash
- you can use flags in your scripts with `getopts` function

```bash
# look for options u & b
# colon after option means that flag value is expected (-u username)
while getopts u:p:ab option;do
    case $option in
        u) user=$OPTARG;;
        p) pass=$OPTARG;;
        a) echo "Got a flag";;
        b) echo "Got b flag";;
    esac
done
echo "User: $user / Password: $pass"

./script -u abbas -p wsxedc     # User: abbas / Password: wsxedc

# order is not required for flags
./script -p wsxedc -u abbas     # User: abbas / Password: wsxedc

./script -u abbas -p wsxedc -a  # Got a flag 
                                # User: abbas / Password: wsxedc

# using colon at beginning of opt-string tells bash that i want to know about other options passed, capture them with ?
while getopts :u:p:ab option;do
    case $option in
        u) user=$OPTARG;;
        p) pass=$OPTARG;;
        a) echo "Got a flag";;
        b) echo "Got b flag";;
        ?) echo "I don't know what $OPTARG is";;
    esac
done
echo "User: $user / Password: $pass"

./script -u abbas -p wsxedc -z  # I don't know what z is
                                # User: abbas / Password: wsxedc

```

## Acquiring Input During Execution of Bash Script && Select

- flags allowes to get input before script starts
- to get input during runtime, the word `read name` pause the script to wait for an input & store the value in variable you specify `name`
- `read -s pass` for silent (don't show characters during typing)

```bash
echo "what's your name?"
read name

echo "what's your password?"
read -s pass

read -p "what's your favorite animal?" animal

echo "Name: $name, Pass: $pass, Animal: $animal"
```

- `select` is another way of getting input in handy menu form

```bash
# following will prompt user to select one of four values & store selection in animal variable
select animal in "dog" "cat" "lion" "giraffe"
do
    echo "You selected $animal"
    break   # break otherwise loop will continue forever
done

# with case statement
select option in "cat" "dog" "quit"
do
    case $option in
        cat) echo "Cats liek to sleep!";;
        dog) echo "Dogs like to play catch!";;
        quit) echo "bye!" && break;;
        *) echo "not supported";;
    esac
done
```

## Error Tolerance && Input Validation









