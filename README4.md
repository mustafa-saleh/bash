
# HackerRank Problems

## Arithemtic Operations

### Problem

- To make numerical calculations in bash, wrapping the expression in double parenthesis $((..)) evaluates it. To evaluate expressions involving decimal places (floating points) "bc -l" is very useful
- bc is a command-line, arbitrary-precision calculator with a Turing-complete
language.
- control decimals by specifying the "scale" (number of decimal points). Note that the ordering of the numbers matters in this case, as demonstrated below.
- `expr` is another way to accomplish such tasks
- scale truncates rather than rounds. Instead, the -l option to bc should be used to return a full-precision result, then use printf formatting to round the result

```bash
echo $((5+5))       # 10
echo $((3/4))       # 0

echo "3/4" | bc -l  # 0.75000000000000000000

echo "scale = 2; 3/4*100" | bc -l   # 0.75

echo $(expr 5 - 5 + 2 / 1 )         # 2

echo '5+5*3/20+(19*2)/7' | bc -l    # 11.17857142857142857142
printf "%.3f" $(echo '5+5*3/20+(19*2)/7' |bc -l)    # 11.179
```

### Resources

#### [A quick guide to writing bash scripts using bash shell](https://www.panix.com/~elflord/unix/bash-tute.html)

##### Using Quotes to enclose your variables 

- most important if your variables value either (a) contains spaces or (b) is the empty string. An example is as follows:

```bash
Y="hello World"
X=""
if [ -n $X ]; then 	# -n tests to see if the argument is non empty
	echo "the variable X is not the empty string"
fi
# output: the variable X is not the empty string
# because the shell expands $X to the empty string. The expression [ -n ] returns true (since it is not provided with an argument). A better script would have been:
X=""
if [ -n "$X" ]; then 	# -n tests to see if the argument is non empty
	echo "the variable X is not the empty string"
fi
```

##### Using Braces to Protect Your Variables

- Suppose you want to echo the value of the variable X, followed immediately by the letters "abc". Question: how do you do this ?

```bash
X=ABC
echo "$Xabc"
# THis gives no output. shell thought variable Xabc. put braces around X to seperate it from the other characters.
echo "${X}abc"  # ABCabc
```

##### The Test Command and Operators

- The command used in conditionals nearly all the time is the test command
- format `test operand1 operator operand2` which abbreviated as `[ operand1 operator operand2 ]`
- for some tests, there need be only one operand (operand2)
- for more `man test`

```bash
X=3
Y=4
empty_string=""
if [ $X -lt $Y ]	# is $X less than $Y ? 
then
	echo "\$X=${X}, which is smaller than \$Y=${Y}"
fi

if [ -n "$empty_string" ]; then
	echo "empty string is non_empty"
fi

if [ -e "${HOME}/.fvwmrc" ]; then 			# test to see if ~/.fvwmrc exists
	echo "you have a .fvwmrc file"
	if [ -L "${HOME}/.fvwmrc" ]; then 		# is it a symlink ?  
		echo "it's a symbolic link
	elif [ -f "${HOME}/.fvwmrc" ]; then 	# is it a regular file ?
		echo "it's a regular file"
	fi
else
	echo "you have no .fvwmrc file"
fi
```

- The test command needs to be in the form `operand1<space>operator<space>operand2` or `operator<space>operand2`. shell considers the first block containing no spaces to be either an operator (if it begins with a '-') or an operand (if it doesn't)

```bash
if [ 1=2 ]; then 
	echo "hello"
fi
# output: hello
```






