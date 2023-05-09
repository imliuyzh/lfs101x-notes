# Manipulating Text
Irrespective of the role you play with Linux (system administrator, developer, or user), you often need to browse through and parse text files and/or extract data from them. Thus, it is essential for Linux users functioning in any of these capacities to become adept at performing these file manipulation operations. Most of the time, such file manipulation is done at the command line, which allows users to perform tasks more efficiently than while using a GUI. Furthermore, the command line is more suitable for automating often-executed tasks.

## cat
cat is short for concatenate and is one of the most frequently used Linux command line utilities. It is often used to read and print files, as well as for simply viewing file contents. To view a file, use `$ cat <filename>`.

The tac command (cat spelled backwards) prints the lines of a file in reverse order. Each line remains the same, but the order of lines is inverted:

```
$ tac file
$ tac file1 file2 > newfile
```

| Command | Usage |
| - | - |
| cat file1 file2 | Concatenate multiple files and display the output |
| cat file1 file2 > newfile | Combine multiple files and save the output into a new file |
| cat file >> existingfile | Append a file to the end of an existing file |
| cat > file | Any subsequent lines typed will go into the file until CTRL-D is typed |
| cat >> file | Any subsequent lines are appended to the file until CTRL-D is typed |

## echo
echo simply displays (echoes) text: `$ echo string`. It can be used to display a string on standard output, place in a new file, or append to an already existing file.

The –e option, along with the following switches, is used to enable special character sequences, such as the newline character or horizontal tab:
- \n represents newline
- \t represents horizontal tab

| Command | Usage |
| - | - |
| echo string > newfile | The specified string is placed in a new file |
| echo string >> existingfile | The specified string is appended to the end of an already existing file |
| echo $variable | The contents of the specified environment variable are displayed |

## Working with Large Files
System administrators need to work with configuration files, text files, documentation files, and log files. Some of these files may be large or become quite large as they accumulate data with time. For example, a system might maintain one simple large log file to record details of all system warnings and errors. Due to a security attack or a malfunction, the administrator might be forced to check for some data by navigating within the file. In such cases, directly opening the file in an editor will probably be inefficient (due to high memory utilization) because most text editors usually try to read the whole file into memory first. Instead, one can use less to view the contents of such a large file, scrolling up and down page by page, without the system having to place the entire file in memory before starting. This is much faster than using a text editor.

Viewing somefile can be done by typing either of the two following commands: `$ less somefile`.

By default, man pages are sent through the less command. You may have encountered the older, more utility, which has the same basic function but fewer capabilities: i.e., less is more!

## head
head reads the first few lines of each named file (10 by default) and displays it on standard output. You can give a different number of lines in an option.

For example, if you want to print the first 5 lines from /etc/default/grub, use the following commands:

```
$ head –n 5 /etc/default/grub
$ head -5 /etc/default/grub
```

## tail
tail prints the last few lines of each named file and displays it on standard output. By default, it displays the last 10 lines. tail is especially useful when you are troubleshooting any issue using log files, as you probably want to see the most recent lines of output.

For example, to display the last 15 lines of somefile.log, use the following commands:

```
$ tail -n 15 somefile.log
$ tail -15 somefile.log
```

To continually monitor new output in a growing log file: `$ tail -f somefile.log`. This command will continuously display any new lines of output in somefile.log as soon as they appear.

## Viewing Compressed Files
When working with compressed files, many standard commands cannot be used directly. For many commonly-used file and text manipulation programs, there is also a version especially designed to work directly with compressed files. These associated utilities often have the letter "z" prefixed to their name. 

| Command | Description |
| - | - |
| zcat compressed-file.txt.gz | To view a compressed file |
| zless somefile.gz <br> zmore somefile.gz | To page through a compressed file |
| zgrep -i less somefile.gz | To search inside a compressed file |
| zdiff file1.txt.gz file2.txt.gz | To compare two compressed files |

There are also equivalent utility programs for other compression methods besides gzip (e.g., xz or bzip2); we have xzcat, xzless, and xzdiff associated with xz and bzcat, bzless and bzdiff associated with bzip2.

## sed
sed is a powerful text processing tool and is one of the oldest, earliest and most popular UNIX utilities. Its name is an abbreviation for stream editor. Data from an input source/file (or stream) is taken and moved to a working space. The entire list of operations/modifications is applied over the data in the working space and the final contents are moved to the standard output space (or stream).

| Command | Usage |
| - | - |
| sed -e command file | Specify editing commands at the command line, process input from a file, and put the output on standard output |
| sed -f scriptfile filename | Specify a script file containing sed commands, operate on file, and put output on standard out |
| echo "I hate you" \| sed s/hate/love/ | Use sed to filter standard input, putting output on standard out |

The -e option allows you to specify multiple editing commands simultaneously at the command line. It is unnecessary if you only have one operation invoked.

![](./images/13.3.1.png)

| Command | Usage |
| - | - |
| sed s/pattern/replace_string/ file | Substitute first string occurrence in every line |
| sed s/pattern/replace_string/g file | Substitute all string occurrences in every line |
| sed 1,3s/pattern/replace_string/g file | Substitute all string occurrences in a range of lines |
| sed -i s/pattern/replace_string/g file | Save changes for string substitution in the same file |

## awk
awk is used to extract and then print specific contents of a file and is often used to construct reports. It was created at Bell Labs in the 1970s and derived its name from the last names of its authors: Alfred Aho, Peter Weinberger, and Brian Kernighan.

The input file is read one line at a time, and, for each line, awk matches the given pattern in the given order and performs the requested action. The -F option allows you to specify a particular field separator character.

The command/action in awk needs to be surrounded with apostrophes (or single-quote (')):

| Command | Usage |
| - | - |
| awk 'command' file | Specify a command directly at the command line |
| awk -f scriptfile file | Specify a file that contains the script to be executed |
| awk '{ print $0 }' /etc/passwd | Print entire file |
| awk -F: '{ print $1 }' /etc/passwd | Print first field (column) of every line, separated by a colon |
| awk -F: '{ print $1 $7 }' /etc/passwd | Print first and seventh field of every line |

![](./images/13.3.2.png)