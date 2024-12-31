#linux #unix #bash

**Bash script** is a text file containing a series of commands written for the **Bash shell**.

- **Shebang (`#!/bin/bash`)**: The first line specifying using bash to run this.
- **Make executable**: Run `chmod +x myscript.sh` to make it executable
- **Execute**: Run with `./myscript.sh` or `bash myscript.sh`
- [[bashrc vs bash_profile]]
	- `bashrc` for all bash script
	- `bash_profile` for login shell only

```bash
#!/bin/bash

ls # do something
```

# Bash Variables

- creating: `variable_name="value"` (no space)
- accessing: `$variable_name`
- printing: `echo $variable_name`
- arithmetic: `$(( num1 + num2 ))` (+-*/)
- string concatenation: `result="$string1 $string2"`

### Special Variables

|**Variable**|**Description**|**Example**|
|---|---|---|
|`$0`|Name of the script being executed.|`echo "This script is called $0"`|
|`$1`, `$2`, ... `$N`|Positional arguments passed to the script.|`echo "First argument: $1"`|
|`$#`|The number of positional parameters passed to the script.|`echo "Number of arguments: $#"`|
|`$@`|All positional parameters, quoted as **separate words**.|`echo "Arguments: $@"`|
|`$*`|All positional parameters, quoted as a **single word**.|`echo "Arguments: $*"`|
|`$?`|Exit status of the last command executed. A value of `0` indicates success, non-zero indicates failure.|`echo "Exit status: $?"`|
|`$$`|Process ID (PID) of the currently running script or shell.|`echo "Script's PID: $$"`|
|`$!`|Process ID (PID) of the last background command.|`sleep 10 & echo "PID of last background process: $!"`|
|`$-`|Current shell options (e.g., `-x`, `-e` for debugging or error handling).|`echo "Shell options: $-"`|

### Other Tips:

- `${variable:-default_value}` If `variable` is unset or empty, `default_value` is used.
- get filename or extension
```bash
FILE=example.tar.gz
echo "${FILE%%.*}" # example
echo "${FILE%.*}"  # example.tar
echo "${FILE#*.}"  # tar.gz
echo "${FILE##*.}" # gz
```
- get script location
```bash
script_dir="$(dirname "${BASH_SOURCE[0]}")"
```

# Bash Control Flow

## `if` `elif` `else`

```bash
if [ condition1 ]; then
    # Commands if condition1 is true
elif [ condition2 ]; then
    # Commands if condition2 is true
else
    # Commands if neither condition1 nor condition2 are true
fi
```

## `case` switch-case

```bash
case $variable in
    pattern1 | pattern2)
        # Commands if pattern1 or pattern2 match
        ;;
    pattern3)
        # Commands if pattern3 matches
        ;;
    *)
        # Commands if no pattern matches
        ;;
esac

```

## `for` loop

The `for` loop is used to iterate over a series of values, such as a list or a range of numbers.

```bash
for variable in "value1 value2 value3"; do
    # Commands to be executed
done

for arg in $@; do
	echo Arguments $arg
done

for i in {1..5}; do
    echo "Number: $i"
done
```

## **`for` Loop with C-style Syntax**

You can also use a `for` loop with a C-style syntax, where you explicitly define the initialization, condition, and increment/decrement.

Syntax:

```bash
for (( initialization; condition; increment )); do
    # Commands to be executed
done
```

```bash
for (( i=1; i<=5; i++ )); do
    echo "Number: $i"
done
```

## **`while` Loop**

The `while` loop executes as long as a condition is true. It checks the condition before executing the commands inside the loop.

```bash
while [ condition ]; do
    # Commands to be executed
done
```

## `break` and `continue`

```bash
#!/bin/bash

for i in {1..10}; do
    if [ $i -eq 5 ]; then
        echo "Breaking the loop at i=$i"
        break
    elif [ $i -eq 3 ]; then
        continue  # Skip iteration 3
    fi
    echo "i=$i"
done
```

## Bash Logical Conditions

In **Bash**, you can use various **conditions** to compare values, check for patterns, and make decisions based on logical conditions.

**Always use double quote if you are using bash.**

### **Comparisons**

For comparing **numbers**, you typically use the `[` (or `test`) command with operators like `-gt`, `-lt`, `-ge`, `-le`, `-eq`, and `-ne`.

|**Operator**|**Meaning**|**Example**|
|---|---|---|
|`-eq`|Equal to|`if [ $a -eq $b ]; then ...`|
|`-ne`|Not equal to|`if [ $a -ne $b ]; then ...`|
|`-gt`|Greater than|`if [ $a -gt $b ]; then ...`|
|`-lt`|Less than|`if [ $a -lt $b ]; then ...`|
|`-ge`|Greater than or equal to|`if [ $a -ge $b ]; then ...`|
|`-le`|Less than or equal to|`if [ $a -le $b ]; then ...`|

But in bash you can also use **double square** `[[ ]]` for extended test command.

|**Operator**|**Meaning**|**Example**|
|---|---|---|
|`=`|Equal to|`if [[ "$a" = "$b" ]]; then ...`|
|`!=`|Not equal to|`if [[ "$a" != "$b" ]]; then ...`|
|`>`|Greater than|`if [[ "$a" > "$b" ]]; then ...`|
|`<`|Less than|`if [[ "$a" < "$b" ]]; then ...`|
|`>=`|Greater than or equal to|`if [[ "$a" >= "$b" ]]; then ...`|
|`<=`|Less than or equal to|`if [[ "$a" <= "$b" ]]; then ...`|

### **String Comparisons**

For comparing **strings**, you use different operators such as `=`, `!=`, `-z`, and `-n`.

|**Operator**|**Meaning**|**Example**|
|---|---|---|
|`=`|Equal to (string)|`if [ "$a" = "$b" ]; then ...`|
|`!=`|Not equal to (string)|`if [ "$a" != "$b" ]; then ...`|
|`-z`|String is empty (zero length)|`if [ -z "$a" ]; then ...`|
|`-n`|String is not empty (non-zero length)|`if [ -n "$a" ]; then ...`|

### **Pattern Matching (Regular Expressions)**

To match strings against **regular expressions**, use the `[[ ... ]]` test with the `=~` operator.

|**Operator**|**Meaning**|**Example**|
|---|---|---|
|`=~`|Regular expression match|`if [[ "$string" =~ regex ]]; then ...`|

```bash
#!/bin/bash

string="hello123"

if [[ "$string" =~ ^hello[0-9]+$ ]]; then
    echo "String matches the regex"
else
    echo "String does not match the regex"
fi

```

### **Logical Operators**

To combine multiple conditions, use logical operators like `-a` (AND), `-o` (OR), and `!` (NOT).

|**Operator**|**Meaning**|**Example**|
|---|---|---|
|`-a`|AND (both conditions must be true)|`if [ $a -gt 5 -a $b -lt 10 ]; then ...`|
|`-o`|OR (one condition must be true)|`if [ $a -gt 5 -o $b -lt 10 ]; then ...`|
|`!`|NOT (negates the condition)|`if [ ! -z "$a" ]; then ...`|

But in bash you can also use **double square** `[[ ]]` for extended test command.

|**Operator**|**Meaning**|**Example**|
|---|---|---|
|`&&`|AND (both conditions must be true)|`if [[ $a > 5 && $b < 10 ]]; then ...`|
|`||`|
|`!`|NOT (negates the condition)|`if [[ ! -z "$a" ]]; then ...`|

### 5. **File Tests**

Bash also supports file-related conditions. These can check if files exist, are readable, writable, or executable.

|**Operator**|**Meaning**|**Example**|
|---|---|---|
|`-e`|File exists|`if [ -e "$file" ]; then ...`|
|`-f`|File is a regular file|`if [ -f "$file" ]; then ...`|
|`-d`|File is a directory|`if [ -d "$dir" ]; then ...`|
|`-r`|File is readable|`if [ -r "$file" ]; then ...`|
|`-w`|File is writable|`if [ -w "$file" ]; then ...`|
|`-x`|File is executable|`if [ -x "$file" ]; then ...`|
|`-s`|File has a non-zero size|`if [ -s "$file" ]; then ...`|
|`-L`|File is a symbolic link|`if [ -L "$file" ]; then ...`|

### 6. **Combining Multiple Conditions**

You can combine conditions using `&&` (AND) and `||` (OR) in an `if` statement.

| **Operator** | **Meaning**     | **Example**                                     |
| ------------ | --------------- | ----------------------------------------------- |
| `&&`         | AND (both true) | `if [ condition1 ] && [ condition2 ]; then ...` |
| `\|`         |                 | `                                               |

# Bash Function

|**Feature**|**Description**|
|---|---|
|**Definition**|`function_name() { commands; }` or `function function_name { commands; }`|
|**Calling**|`function_name` or `function_name arg1 arg2`|
|**Arguments**|`$1`, `$2`, etc., to access passed arguments|
|**Returning data**|Use `echo` or `printf` to return data (not `return`)|
|**Exit status**|Use `return` for exit status (0 for success, non-zero for error)|
|**Local variables**|Use `local` to declare local variables within the function|
|**Parameters**|Use `$@` or `$1`, `$2`, etc., to handle multiple parameters|

Example

```bash
#!/bin/bash

# Function definition
calculate() {
    local result=$(( $1 * $2 ))  # Local variable
    echo $result  # Return value via echo
}

# Function call with arguments
result=$(calculate 5 10)  # Capture return value
echo "Result is: $result"

```