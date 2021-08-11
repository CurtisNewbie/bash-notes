# Scripting in Bash

## 1. Shell Declaration

We should always make it clear that which shell we are using as follows:

```
#!/bin/bash

...
```

## 2. Variable and command substitution

Use **`${}`** or **`$variable_name`** to get value of variable. Use **`$()`** to do command substitution

```
#!/bin/bash

greeting="welcome"

user=$(whoami)

echo "$greeting back ${user}"
```

## 3. Comments

Add comments using **`#`**

```
# this variable calls 'whoami'
name=$(whoami)
```

## 4. Parameter Expansion

If we want to read the value of a variable we use **`$var_name`**.

e.g.,

```
name="yongj.zhuang"
echo "My name is $name"
```

However, sometimes the variable name may be followed by some characters fak things up.

e.g., this will not work

```
echo "You_$name_okay?"
```

Parameter expansion is essentially **`${...}`**.

```
echo "You_${name}_okay?"
```

## 5. Input/Output Redirection

For I/O, there are following concepts:

1. Standard Input (**`<`**)
2. Standard Output (**`>`**)
3. Standard Error Output (**`2>`**)
4. Both standard output and Error Output (**`&>`**)

We use the above syntax to do output redirection.

e.g.,

```
# output redirection
echo "something important" > note.txt

# input redirection
cat < note.txt
```

## 6. Data Sink

Data Sink is basically **`/dev/null`**. When we want to discard the output, we simply redirect the output to this data sink.

e.g.,

```
echo "useless message" > /dev/null
```

## 7. Function

Function must start with the keyword **`function`**. It doesn't need the bracket like most langauges. It uses **`$n`** to refer to the n-1th parameter.

E.g.,

```
function totalFiles {
    find $1 -type f | wc -L
}

# we call the function like this:
totalFiles /home/zhuangyongj/exec

# then $1 refers to '/home/zhuangyongj/exec'
```

## 8. Numeric & String Comparison

**String cannot be compared with numbers.**

### 8.1 Numeric Comparison

- less than **`-lt`**
- greater than **`-ge`**
- equal **`-eq`**
- not equal **`-ne`**
- less or equal **`-le`**
- greater or equal **`-ge`**

e.g.,

```
n=10

if [ $n -lt 100 ]
then
    echo "$n is less than 100"
fi
```

### 8.2 String Comparison

- less than **`<`**
- greater than **`>`**
- equal **`=`**
- not equal **`!=`**

e.g.,

```
a="apple"
b="banna"

if [ $a != $b ]
then
    echo "$a is not equal to $b"
fi
```

## 9. Result of Previous Statement

**`$?`** is the result of previous statement, **`0`** means true, **`1`** means false. I.e., if previous statement is correctly executed, than **`$?`** should be **0**.

e.g.,

```
echo "I am okay"
echo "$?" # this is 0
```

## 10. Conditional Statements

### 10.1 if

```
if [ 1 -eq 1 ]
then
    echo "1 is 1"
else
    echo "Never happens"
fi
```

## 11 Positional Parameters

Positional parameters are arguments from CLI or methods calls, they are 1-based arguments. We retrieve them by calling **`$1`**, **`$2`** and so on. We can also use **`$#`** to get the total number of supplied arguments. To get all the positional arguments, use **`$*`**.

e.g.,

```
function donothing {
    echo "You hava provided $# arguments"
    echo "These are $*"
    echo "The first one is $1"
}

# call method
donothing apple juice orange juice

# result:
# You hava provided 4 arguments
# These are apple juice orange juice
# The first one is apple
```

## 12. Check if variable has value

Use **`-z`** to check if the variable has any value

e.g,

```
function say {
    if [ -z $1 ]
    then
        echo "Nothing to say"
    else
        echo "$1"
    fi
}
```

## 13. Check if a direction exists

Use **`-d`** to check if a directory exists.

```
function dir_exists {
    if [ -d $1 ]
    then
        echo "dir exists"
    else
        echo "dir not found"
    fi
}
```

## 14. Check if a file exists

Use **`-f`** to check if a file exists.

```
function file_exists {
    if [ -f $1 ]
    then
        echo "file exists"
    else
        echo "file not found"
    fi
}
```

## 15. Exit Script

Use **`exit`** with a code to exit the program, code other than 0 is an abnormal exit.

```
function read_file {
    if [ ! -f $1 ]
    then
        echo "file not found, aborting"
        exit 1
    fi

    echo "Reading file..."
}
```

## 16. For Loop

```
for i in 1,2,3
do
    echo $i
done
```

A string with words delimited by spaces can also be iterated in for loop.

```
parah="Apple banana juice is good"
for s in $parah
do
    echo $s
done

# result:
# Apple
# banana
# juice
# is
# good
```

## 17. While Loop

```
i=0
while [ $i -lt 3 ]
do
    i=$(($i+1))
    echo i=$i
done

# result:
# i=1
# i=2
# i=3
```

## 18. Array

Array can be created using **`declare`** command. Notice that the items are delimited by space, not comma, simply space.

e.g.,

```
declare -a memo=("apple" "juice")
```

To get a size of an array, use **`${#arrayName[@]}`**

e.g.,

```
echo "Size: ${#memo[@]}"
```

To get alll items, use **`${arrayName[@]}`**

```
echo "All items ${memo[@]}"
```

To get a specific item by index, use **`${arrayName[1]}`**, it's 1-based, so 1 returns the first item.

```
echo "First item: ${memo[0]}"
```

To append one or more items, use **`arrayName+=(...)`**

```
memo+=("orange" "juice")
```

To remove item at specified index, this command is 0-based.

```
unset memo[1]
```

## 19. Declare

**`Declare`** is used to supply attributes to a variable.

- array **`-a`**
- read-only **`-r`**
- associative array/map **`-A`**
- integer **`-i`**

## 20. Until loop

```
c=4
until [ $c -lt 1 ]
do
    let "c--"
    echo $c
done
```

## 21. Let commend

**`let`** is use for artimatic operation. We can also use **`$((...))`**.

e.g.,

```
let "1+1"
let "i++"
let "i*=5"
```
