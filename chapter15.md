# The Bash Shell and Basic Scripting

`sh` was written by Steve Bourne at AT&T in 1977, and is often known as the Bourne Shell. `bash` is a product of the GNU project and was created in 1987. It was designed as a major upgrade of `sh`; the name stands for Bourne Again Shell. On all Linux systems `sh` is just a link to `bash`, but scripts which are invoked as `sh` will only work without the `bash` extensions. 

Linux provides a wide choice of shells and they are listed in the `/etc/shells` file.

## A Simple bash Script
The first line of the script, `#!/usr/bin/env bash`, gets the path to the command interpreter from the system environment. The special two-character sequence, `#!`, is often called a shebang.

Save it as `hello.sh` and enter `chmod +x hello.sh` to make the file executable by all users:

```bash
#!/usr/bin/env bash
echo "Hello Linux Foundation Student"
```

You can then run the script by typing `./hello.sh` or by doing:

```bash
$ bash hello.sh
```

> You can run a bash script in debug mode either by doing `bash -x ./script_file`, or bracketing parts of the script with `set -x` and `set +x`.

## Interactive Example Using bash Scripts
Create a file named `get_name.sh` with the following content:

```bash
#!/usr/bin/env bash
echo "ENTER YOUR NAME"
read name

# NOT RECOMMENDED: echo The name given was: $name
echo "The name given was: ${name}"    # or echo "The name given was: $name"
```

The user will be prompted to enter a value, which is then displayed on the screen. We can reference the value of a shell variable by using a `$` in front of the variable name, such as `$name`. However, it is preferred to place a pair of quotes around it, as in `"$name"`. The quotes prevent parsing issues when the value of a variable contains spaces. Without the quotation marks, the value can be incorrectly interpreted by the shell as a command followed by its arguments. Besides this, you can also use `"${name}"` to avoid the same problem.

## Return Values
All shell scripts generate a return value upon finishing execution. By convention, success is returned as zero, and failure is returned as any non-zero value. The return value is stored in the environment variable represented by `$?`:

```
$ ls /etc/logrotate.conf
/etc/logrotate.conf

$ echo $?
0
```

In this example, the system is able to locate the file `/etc/logrotate.conf` and `ls` returns a value of 0 to indicate success. When run on a non-existing file, it returns 2.

## Splitting Long Commands Over Multiple Lines
The concatenation operator (`\`), the backslash character, is used to continue long commands over several lines. It causes the shell to concatenate multiple lines and execute them as one single command. Here is an example of a command installing a long list of packages on a system using Debian package management:

```
$ sudo apt install autoconf automake bison build-essential \
  chrpath curl diffstat emacs flex gcc-multilib g++-multilib \ 
  libsdl1.2-dev libtool lzop make mc patch \
  screen socat sudo tar texinfo tofrodos u-boot-tools unzip \
  vim wget xterm zip
```

Also, note that the backslash character has another meaning of interpreting next character literally:

```bash
echo \$HOME # $HOME
``` 

## Putting Multiple Commands on a Single Line
Users sometimes need to combine several commands and statements and even conditionally execute them based on the behavior of operators used in between them. This method is called chaining of commands.

The `;` (semicolon) character is used to separate the commands and execute them sequentially, as if they had been typed on separate lines. Each ensuing command is executed whether or not the preceding one succeeded. Thus, the three commands in the following example will all execute, even if the ones preceding them fail: `make ; make install ; make clean`.

However, you may want to abort subsequent commands when an earlier one fails. You can do this using the `&&` (and) operator as in: `make && make install && make clean`.

Likewise, the `||` (or) operator runs through multiple commands until something succeeds: `cat file1 || cat file2 || cat file3`. 

## Script Parameters
| Parameter | Meaning |
| - | - |
| `$0` | Script name |
| `$1` | First parameter |
| `$2`, `$3`, etc. | Second, third parameter, etc. |
| `$@` | All parameters |
| `$#` | Number of arguments |

```bash
f() {
    echo "The name of this program is: $0"
    echo "The first argument passed from the command line is: $1"
    echo "The second argument passed from the command line is: $2"
    echo "The third argument passed from the command line is: $3"
    echo "All of the arguments passed from the command line are: $@"
}

# The name of this program is: /bin/bash
# The first argument passed from the command line is: one
# The second argument passed from the command line is: two
# The third argument passed from the command line is: three
# All of the arguments passed from the command line are: one two three four five
f one two three four five
```

## Command Substitution
At times, you may need to substitute the result of a command as a portion of another command. It can be done in two ways:
- By enclosing the inner command in `$()`
- By enclosing the inner command with backticks (`` ` ``)

The second form using backticks is deprecated, and its use should be avoided in new scripts and commands. No matter which method is used, the specified command will be executed in a newly launched shell environment, and the standard output of the shell will be inserted where the command substitution is done.

![](./images/15.2.2.png)

## Environment Variables
When referenced, environment variables must be prefixed with the `$` symbol, as in `$HOME`. You can view and set the value of environment variables. However, no prefix is required when setting or modifying the variable value: `MYCOLOR=blue`.

By default, the variables created within a script are available only to the subsequent steps of that script. Any child processes (sub-shells) do not have automatic access to the values of these variables. To make them available to child processes, they must be promoted to environment variables using the export statement: `export VAR=value`.

While child processes are allowed to modify the value of exported variables, the parent will not see any changes; exported variables are not shared, they are only copied and inherited.

## Functions
A function is a code block that implements a set of operations. They are also often called subroutines. 

Note you should use the `local` operator when you want to define a local variable in functions. By default, variables in bash scripts are global variables.

```bash
showmess() {
    local distribution=$1
    echo "My favorite Linux distribution is: ${distribution}"
}

# My favorite Linux distribution is: Ubuntu
# My favorite Linux distribution is: Fedora
# My favorite Linux distribution is: Debian
# My favorite Linux distribution is: openSUSE
showmess Ubuntu
showmess Fedora
showmess Debian
showmess openSUSE
```

> A constant variable is defined like `readonly NAME='value'` in bash. Constant variables can be used just like a regular variable except you cannot change their value after they have been defined.

## Boolean Expressions
| Operator | Operation |
| - | - |
| `&&` | AND |
| `\|\|` | OR |
| `!` | NOT |

## The if Statement
When an if statement is used, the ensuing actions depend on the evaluation of specified conditions.

In the following example, an if statement checks to see if a certain file exists, and if the file is found, it displays a message indicating success or failure:

```bash
if [ -f "$1" ]
then
    echo file "$1" exists 
else
    echo file "$1" does not exist
fi
```

Notice the use of the square brackets (`[]`) to delineate the test condition. 

In modern scripts, you may see doubled brackets as in `[[ -f /etc/passwd ]]`. This is not an error. It is never wrong to do so and it avoids some subtle problems, such as referring to an environment variable without surrounding it in double quotes.

## The elif Statement
You can use the elif statement to perform more complicated tests, and take action appropriate actions.

```bash
give_your_name() {
    echo "Give your name"
    read name

    if [[ "$name" == "John" ]]; then
        echo 'Hello John'
    elif [[ "$name" == "George" ]] || [[ "$name" == "Ringo" ]] || [[ "$name" == "Paul" ]]; then
        echo "Hello ${name}"
    else
        echo "Forget it ${name}, you are not a Beatle"
    fi
}

# Give your name
# Hello John
echo "John" | give_your_name

# Give your name
# Hello George
echo "George" | give_your_name

# Give your name
# Hello Jack
echo "Jack" | give_your_name
```

## Testing for Files
bash provides a set of file conditionals that can be used with the if statement. In the following example,

```bash
if [[ -x /etc/passwd ]]; then
    echo 'Executable'
fi
```

the if statement checks if the file `/etc/passwd` is executable, which it is not.

| Condition | Meaning |
| - | - |
| `-e file` | Checks if the file exists |
| `-d file` | Checks if the file is a directory |
| `-f file` | Checks if the file is a regular file (i.e., not a symbolic link, device node, directory, etc.) |
| `-s file` | Checks if the file is of non-zero size |
| `-r file` | Checks if the file is readable |
| `-w file` | Checks if the file is writable |
| `-x file` | Checks if the file is executable |

## Testing for Strings
You can use the if statement to compare strings using the operator `==` (two equal signs):

```bash
test_string() {
    echo "Please specify window, middle, or aisle for your seat."
    read choice

    if [[ "$choice" == "window" ]]; then
        echo "Window #29A"
    elif [[ "$choice" == "middle" ]]; then
        echo "Middle #29B"
    elif [[ "$choice" == "aisle" ]]; then
        echo "Aisle #29C"
    else
        echo "\"${choice}\" is not valid. Please try again."
    fi
}

# Please specify window, middle, or aisle for your seat.
# Middle #29B
echo "middle" | test_string

# Please specify window, middle, or aisle for your seat.
# Window #29A
echo "window" | test_string

# Please specify window, middle, or aisle for your seat.
# "wing" is not valid. Please try again.
echo "wing" | test_string
```

Note that using one `=` sign will also work, but some consider it deprecated usage. 

## Testing for Numbers
| Operator | Meaning |
| - | - |
| `-eq` | Equal to |
| `-ne` | Not equal to |
| `-gt` | Greater than |
| `-lt` | Less than |
| `-ge` | Greater than or equal to |
| `-le` | Less than or equal to |

```bash
check_age() {
    local age=$1
    if [[ "$age" -ge 20 ]] && [[ "$age" -lt 30 ]]; then
        echo "You are in your 20s"
    elif [[ "$age" -ge 30 ]] && [[ "$age" -lt 40 ]]; then
        echo "You are in your 30s"
    elif [[ "$age" -ge 40 ]] && [[ "$age" -lt 50 ]]; then
        echo "You are in your 40s"
    else
        echo "You are not in the proper range of 21-50"
    fi
}

check_age 33    # You are in your 30s
check_age 21    # You are in your 20s
check_age 18    # You are not in the proper range of 21-50
```

## Arithmetic Expressions
Arithmetic expressions can be evaluated in the following three ways (spaces are important!):

- `expr` is a standard but somewhat deprecated program
    ```bash
    echo $(expr 8 + 8)
    ```

- Using the `$((...))` syntax, the built-in shell format
  - In modern shell scripts, the use of `expr` is better replaced with `x=$((...))` 
  ```bash
  echo $((x+1))
  ```

- Using the built-in shell command `let`
    ```bash
    let x=( 1 + 2 ); echo $x
    ```
