# Bash

## Navigation Commands

| Key or key combination          	| Function                                                                                                                                                                                                      	|
|---------------------------------	|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------	|
| Ctrl+A                          	| Move cursor to the beginning of the command line.                                                                                                                                                             	|
| Ctrl+C                          	| End a running program and return the prompt, see Chapter 4.                                                                                                                                                   	|
| Ctrl+D                          	| Log out of the current shell session, equal to typing exit or logout.                                                                                                                                         	|
| Ctrl+E                          	| Move cursor to the end of the command line.                                                                                                                                                                   	|
| Ctrl+H                          	| Generate backspace character.                                                                                                                                                                                 	|
| Ctrl+L                          	| Clear this terminal.                                                                                                                                                                                          	|
| Ctrl+R                          	| Search command history, see Section 3.3.3.4.                                                                                                                                                                  	|
| Ctrl+Z                          	| Suspend a program, see Chapter 4.                                                                                                                                                                             	|
| ArrowLeft and ArrowRight        	| Move  the cursor one place to the left or right on the command line, so that  you can insert characters at other places than just at the beginning and  the end.                                              	|
| ArrowUp and ArrowDown           	| Browse history.  Go to the line that you want to repeat, edit details if necessary, and press Enter to save time.                                                                                             	|
| Shift+PageUp and Shift+PageDown 	| Browse terminal buffer (to see text that has "scrolled off" the screen).                                                                                                                                      	|
| Tab                             	| Command  or filename completion; when multiple choices are possible, the system  will either signal with an audio or visual bell, or, if too many choices  are possible, ask you if you want to see them all. 	|
| Tab Tab                         	| Shows file or command completion possibilities.                                                                                                                                                               	|


---

## Bash Scripting Tips and Tricks

### 1. Use `set -euo pipefail` for Robust Scripts

- **`set -e`**: Exit on any error.
- **`set -u`**: Treat unset variables as an error.
- **`set -o pipefail`**: Fail if any command in a pipeline fails, not just the last one.

```bash
# Ensure the script exits on error, undefined variables, and pipeline failures.

set -euo pipefail
```

### 2. Command Substitution Using $()

- Use $(command) instead of backticks `command` for readability and nesting.

```
today=$(date +%Y-%m-%d)
```

### 3. Use Functions for Code Reusability

- Modularize your script using functions to improve readability and reusability.

```
greet() {
  echo "Hello, $1!"
}

greet "John"
```

### 4. Use Arrays for Complex Data

- Bash arrays are powerful for handling lists of data.

```
fruits=("apple" "banana" "cherry")
echo "I like ${fruits[0]}"  # Outputs "I like apple"
```

### 5. Use $(...) to Capture Command Output

- Store the output of a command in a variable using $(...).

```
current_directory=$(pwd)
echo "Current Directory: $current_directory"
```

### 6. Loop Through Files or Command Outputs

- Use loops to process multiple files or outputs from a command.

```
for file in *.txt; do
  echo "Processing $file..."
done
```

### 7. Use [[ ]] for Safer Comparisons

- Prefer [[ ... ]] over [ ... ] for tests to avoid issues with operators and enable regex matching.

```
if [[ "$name" =~ ^[A-Z] ]]; then
  echo "Name starts with an uppercase letter."
fi
```

### 8. Check Command Results Using $?

- Check the exit status of the last command with $?. A value of 0 indicates success, while non-zero indicates failure.

```
ls /nonexistent_directory
if [[ $? -ne 0 ]]; then
  echo "Directory not found."
fi
```

### 9. Use Here Documents for Multi-Line Strings

- Use a Here Document to pass multi-line input to a command.

```
cat <<EOF
This is a multi-line string.
EOF
```

### 10. Avoid Useless Use of cat (UUOC)

- Use input redirection instead of unnecessary cat commands.

#### Bad

```
cat file.txt | grep "pattern"
```

#### Good

```
grep "pattern" file.txt
```

### 11. Handle Script Arguments with $@ and $#

- Use $@ to access all arguments and $# for the count of arguments.

```
echo "You passed $# arguments."
for arg in "$@"; do
  echo "Argument: $arg"
done
```

### 12. Use trap to Handle Script Exit or Errors

- Trap signals and errors to perform cleanup or logging before exiting.

```
trap 'echo "Error on line $LINENO"; exit 1' ERR
```

### 13. Use String Manipulation

- Use Bash parameter expansion to manipulate strings efficiently.

```
filename="document.txt"
echo "${filename%.txt}.pdf"  # Converts "document.txt" to "document.pdf"
```

### 14. Use getopts for Handling Options

- Use getopts to parse command-line options in a structured way.

```
while getopts ":a:b:" opt; do
  case $opt in
    a) echo "Option -a with value $OPTARG";;
    b) echo "Option -b with value $OPTARG";;
    \?) echo "Invalid option: -$OPTARG";;
  esac
done
```

### 15. Use case for Multiple Condition Checks

- Use case instead of a long list of if-elif statements for better readability.

```
case $action in
  start) echo "Starting...";;
  stop) echo "Stopping...";;
  *) echo "Unknown action";;
esac
```

### 16. Avoid Glob Expansion Pitfalls

- Use double quotes to prevent unintended glob expansion and splitting.

```
file_list="*.txt"
echo "$file_list"  # Will not expand "*.txt" to filenames
```

### 17. Use declare for Typed Variables and Arrays

- Use **`declare`** to specify variable types, arrays, and constants.

```
declare -r PI=3.14159  # Read-only variable
declare -a fruits=("apple" "banana" "cherry")
```

### 18. Check for File Existence Before Operating

- Use file test operators like -e, -f, and -d to check file existence.

```
if [[ -e "file.txt" ]]; then
  echo "file.txt exists."
fi
```

### 19. Use read for Interactive Input

- Capture user input with **`read`** in interactive scripts.

```
echo "Enter your name:"
read name
echo "Hello, $name!"
```

### 20. Use exec to Redirect Input/Output

- Use **`exec`** to redirect input/output for efficiency and clarity in long-running scripts.

```
exec > output.log 2>&1  # Redirect both stdout and stderr to output.log
```