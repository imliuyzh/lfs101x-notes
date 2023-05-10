# The Bash Shell and Basic Scripting

## Command Shell Choices
The command interpreter is tasked with executing statements that follow it in the script. Linux provides a wide choice of shells; exactly what is available on the system is listed in /etc/shells:
- /bin/sh
- /bin/bash
- /bin/tcsh
- /bin/csh
- /bin/ksh
- /bin/zsh

## sh and bash
sh was written by Steve Bourne at AT&T in 1977, and is often known as the Bourne Shell. bash is a product of the GNU project and was created in 1987. It was designed as a major upgrade of sh; the name stands for Bourne Again Shell. On all Linux systems sh is just a link to bash, but scripts which are invoked as sh will only work without the bash extensions. 

## Shell Scripts
A shell is simply a command line interpreter which provides the user interface for terminal windows. A command shell can also be used to run scripts, even in non-interactive sessions without a terminal window, as if the commands were being directly typed in.

The first line of the script, `#!/bin/bash`, contains the full path of the command interpreter that is to be used on the file. You have quite a few choices for the scripting language you can use, such as /usr/bin/perl, /bin/csh, /usr/bin/python, etc. The special two-character sequence, #!, is often called a shebang.

## A Simple bash Script
Save it as hello.sh and enter chmod +x hello.sh to make the file executable by all users:

```bash
#!/bin/bash
echo "Hello Linux Foundation Student"
```

You can then run the script by typing `./hello.sh` or by doing:

```bash
$ bash hello.sh
```

NOTE: If you use the second form, you do not have to make the file executable.

## Interactive Example Using bash Scripts
The user will be prompted to enter a value, which is then displayed on the screen. The value is stored in a temporary variable, name. We can reference the value of a shell variable by using a $ in front of the variable name, such as $name. To create this script, you need to create a file named getname.sh in your favorite editor with the following content:

```bash
#!/bin/bash
echo "ENTER YOUR NAME"
read name

# Display variable input
echo The name given was :$name
```

Once again, make it executable by doing `chmod +x getname.sh`.

## Return Values
All shell scripts generate a return value upon finishing execution, which can be explicitly set with the exit statement. Return values permit a process to monitor the exit state of another process, often in a parent-child relationship. 

As a script executes, one can check for a specific value or condition and return success or failure as the result. By convention, success is returned as zero (0), and failure is returned as any non-zero value. An easy way to demonstrate success and failure completion is to execute the ls program on a file that exists as well as one that does not. The return value is stored in the environment variable represented by $?:

```
$ ls /etc/logrotate.conf
/etc/logrotate.conf

$ echo $?
0
```

In this example, the system is able to locate the file /etc/logrotate.conf and ls returns a value of 0 to indicate success. When run on a non-existing file, it returns 2.

## Basic Syntax and Special Characters
| Character | Description |
| - | - |
| # | Used to add a comment, except when used as \\# or #! |
| \ | Used at the end of a line to indicate continuation on to the next line, or to indicate that the next character is to be interpreted literally, as in \$ |
| ; | Used to interpret what follows as a new command to be executed after completion of the current command |
| $ | Indicates what follows is an environment variable |
| > | Redirect output |
| >> | Append output |
| < | Redirect input |
| \| | Used to pipe the result into the next command |

## Splitting Long Commands Over Multiple Lines
The concatenation operator (\\), the backslash character, is used to continue long commands over several lines. It causes the shell to combine (concatenate) multiple lines and execute them as one single command. Here is an example of a command installing a long list of packages on a system using Debian package management:

```
$ sudo apt install autoconf automake bison build-essential \
  chrpath curl diffstat emacs flex gcc-multilib g++-multilib \ 
  libsdl1.2-dev libtool lzop make mc patch \
  screen socat sudo tar texinfo tofrodos u-boot-tools unzip \
  vim wget xterm zip
```

## Putting Multiple Commands on a Single Line
Users sometimes need to combine several commands and statements and even conditionally execute them based on the behavior of operators used in between them. This method is called chaining of commands.

There are several different ways to do this, depending on what you want to do. The ; (semicolon) character is used to separate these commands and execute them sequentially, as if they had been typed on separate lines. Each ensuing command is executed whether or not the preceding one succeeded. Thus, the three commands in the following example will all execute, even if the ones preceding them fail: `make ; make install ; make clean`.

However, you may want to abort subsequent commands when an earlier one fails. You can do this using the && (and) operator as in: `make && make install && make clean`.

If the first command fails, the second one will never be executed. A final refinement is to use the || (or) operator, as in: `cat file1 || cat file2 || cat file3`. In this case, you proceed until something succeeds and then you stop executing any further steps.

## Output Redirection
With shell commands and scripts you can send the output to a file. The process of diverting the output to a file is called output redirection. 

The > character is used to write output to a file. For example, the following command sends the output of free to /tmp/free.out: `free > /tmp/free.out`.

Two > characters (>>) will append output to a file if it exists, and act just like > if the file does not already exist.\

## Input Redirection
Just as the output can be redirected to a file, the input of a command can be read from a file. The process of reading input from a file is called input redirection and uses the < character.

The following three commands (using wc to count the number of lines, words and characters in a file) are entirely equivalent and involve input redirection, and a command operating on the contents of a file:

```bash
# 49  105 2678 /etc/passwd
# 49  105 2678 /etcpasswd
# 49  105 2678
$ wc < /etc/passwd
$ wc /etc/passwd
$ cat /etc/passwd | wc
```

## Script Parameters
Users often need to pass parameter values to a script, such as a filename, date, etc. Scripts will take different paths or arrive at different results according to the parameters (command arguments) that are passed to them.

```bash
$ ./script.sh /tmp
$ ./script.sh 100 200
```

Within a script, the parameter or an argument is represented with a $ and a number or special character:
| Parameter | Meaning |
| - | - |
| $0 | Script name |
| $1 | First parameter |
| $2, $3, etc. | Second, third parameter, etc. |
| $* | All parameters |
| $# | Number of arguments |

![](./images/15.2.1.png)

## Command Substitution
At times, you may need to substitute the result of a command as a portion of another command. It can be done in two ways:
- By enclosing the inner command in $( )
- By enclosing the inner command with backticks (`)

The second form using backticks is deprecated, and its use should be avoided in new scripts and commands. No matter which method is used, the specified command will be executed in a newly launched shell environment, and the standard output of the shell will be inserted where the command substitution is done.

![](./images/15.2.2.png)

## Environment Variables
Most scripts use variables containing a value, which can be used anywhere in the script. These variables can either be user or system-defined. Many applications use such environment variables (already covered in some detail in the User Environment chapter) for supplying inputs, validation, and controlling behavior.

When referenced, environment variables must be prefixed with the $ symbol, as in $HOME. You can view and set the value of environment variables. However, no prefix is required when setting or modifying the variable value: `MYCOLOR=blue`.

By default, the variables created within a script are available only to the subsequent steps of that script. Any child processes (sub-shells) do not have automatic access to the values of these variables. To make them available to child processes, they must be promoted to environment variables using the export statement: `export VAR=value`.

While child processes are allowed to modify the value of exported variables, the parent will not see any changes; exported variables are not shared, they are only copied and inherited.

Typing export with no arguments will give a list of all currently exported environment variables.

![](./images/15.2.3.png)

## Functions
A function is a code block that implements a set of operations. Functions are useful for executing procedures multiple times, perhaps with varying input variables. Functions are also often called subroutines. Using functions in scripts requires two steps:
- Declaring a function
- Calling a function

For example, the following function is named display:

```bash
display () {
   echo "This is a sample function that just displays a string"
}
```

The function can be as long as desired and have many statements. Once defined, the function can be called later as many times as necessary. In the full example shown in the figure, we are also showing an often-used refinement: how to pass an argument to the function. The first argument can be referred to as $1, the second as $2, etc.

![](./images/15.2.4.png)
