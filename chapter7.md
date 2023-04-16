# Command Line Operations

A terminal emulator program emulates (simulates) a standalone terminal within a window on the desktop. By this, we mean it behaves essentially as if you were logging into the machine at a pure text terminal with no running graphical interface. Most terminal emulator programs support multiple terminal sessions by opening additional tabs or windows.

By default, on GNOME desktop environments, the gnome-terminal application is used to emulate a text-mode terminal in a window. 

## The Command Line
Most input lines entered at the shell prompt have three basic elements: command, options, and arguments.

The command is the name of the program or script you are executing. It may be followed by one or more options (or switches) that modify what the command may do. Options usually start with one or two dashes, for example, -p or --print, in order to differentiate them from arguments, which represent what the command operates on.

However, plenty of commands have no options, no arguments, or neither. In addition, other elements (such as setting environment variables) can also appear on the command line when launching a task.

### sudo
All the demonstrations created have a user configured with sudo capabilities to provide the user with administrative (admin) privileges when required. sudo allows users to run programs using the security privileges of another user, generally root (superuser). 

If your system does not already have sudo set up and enabled:

1. You will need to make modifications as the administrative, or superuser, root. While sudo will become the preferred method of doing this, we do not have it set up yet, so we will need to use su instead. At the command line prompt, type su and press Enter. You will then be prompted for the root password, so enter it and press Enter. You will notice that nothing is printed; this is so others cannot see the password on the screen. You should end up with a different looking prompt, often ending with ‘#’.
    ```
    $ su Password:
    #
    ```
2. Now, you need to create a configuration file to enable your user account to use sudo. Typically, this file is created in the /etc/sudoers.d/ directory with the name of the file the same as your username. For example, for this demo, let’s say your username is student. After doing step 1, you would then create the configuration file for student by doing this:
    ```
    # echo "student ALL=(ALL) ALL" > /etc/sudoers.d/student
    ```
   - Some Linux distributions will complain if you do not also change permissions on the file by doing: `# chmod 440 /etc/sudoers.d/student`

## Virtual Terminals
Virtual Terminals (VT) are console sessions that use the entire display and keyboard outside of a graphical environment. Such terminals are considered "virtual" because, although there can be multiple active terminals, only one terminal remains visible at a time. A VT is not the same as a command line terminal window; you can have many of those visible simultaneously on a graphical desktop.

One virtual terminal (usually VT 1 or VT 7) is reserved for the graphical environment, and text logins are enabled on the unused VTs. 

To switch between VTs, press CTRL-ALT-function key for the VT. For example, press CTRL-ALT-F6 for VT 6. Actually, you only have to press the ALT-F6 key combination if you are in a VT and want to switch to another VT.

## Turning Off the Graphical Desktop
Linux distributions can start and stop the graphical desktop in various ways. The exact method differs among distributions and between versions. For the newer system-based distributions, the display manager is run as a service, and you can stop the GUI desktop with the systemctl utility. In addition, most distributions will also work with the telinit command, as in:

`$ sudo systemctl stop gdm (or sudo telinit 3)`

and restart it (after logging into the console) with:

`$ sudo systemctl start gdm (or sudo telinit 5)`

## Logging In and Out
An available text terminal will prompt for a username (with the string login:) and password. When typing your password, nothing is displayed on the terminal (not even a * to indicate that you typed in something), to prevent others from seeing your password. After you have logged into the system, you can perform basic operations.

Once your session is started (either by logging into a text terminal or via a graphical terminal program), you can also connect and log into remote systems by using Secure SHell (SSH). For example, by typing `ssh student@remote-server.com`, SSH would connect securely to the remote machine (remote-server.com) and give student a command line terminal window, using either a password (as with regular logins) or cryptographic key to sign in without providing a password to verify the identity.

## Rebooting and Shutting Down
The preferred method to shut down or reboot the system is to use the shutdown command. This sends a warning message, and then prevents further users from logging in. The init process will then control shutting down or rebooting the system. It is important to always shut down properly; failure to do so can result in damage to the system and/or loss of data.

The halt and poweroff commands issue shutdown -h to halt the system; reboot issues shutdown -r and causes the machine to reboot instead of just shutting down. Both rebooting and shutting down from the command line requires superuser (root) access.

When administering a multi-user system, you have the option of notifying all users prior to shutdown: `$ sudo shutdown -h 10:00 "Shutting down for scheduled maintenance."`

## Locating Applications
Depending on the specifics of your particular distribution's policy, programs and software packages can be installed in various directories. In general, executable programs and scripts should live in the /bin, /usr/bin, /sbin, /usr/sbin directories, or somewhere under /opt. They can also appear in /usr/local/bin and /usr/local/sbin, or in a directory in a user's account space, such as /home/student/bin.

One way to locate programs is to employ the which utility. For example, to find out exactly where the diff program resides on the filesystem:

```
$ which diff
/usr/bin/diff
```

If which does not find the program, whereis is a good alternative because it looks for packages in a broader range of system directories:

```
$ whereis diff
diff: /usr/bin/diff /usr/share/man/man1/diff.1.gz /usr/share/man/man1p/diff.1p.gz
```

as well as locating source and man files packaged with the program.

## Accessing Directories
When you first log into a system or open a terminal, the default directory should be your home directory. You can see the exact location by typing echo $HOME. However, most Linux distributions open new graphical terminals in $HOME/Desktop instead.

| Command    | Result |
| ---------   | -------- |
| pwd        | Displays the present working directory |
| cd ~ or cd | Change to your home directory; shortcut name is ~ (tilde) |
| cd ..      | Change to parent directory (..) |
| cd -       | Change to previous working directory |

## Understanding Absolute and Relative Paths
An absolute pathname begins with the root directory (/) and follows the tree, branch by branch, until it reaches the desired directory or file. Absolute paths always start with /. A relative pathname starts from the present working directory. Relative paths never start with /.

Multiple slashes (/) between directories and files are allowed, but all but one slash between elements in the pathname is ignored by the system. While ////usr//bin is valid, it is seen as just /usr/bin by the system.

For example, suppose you are currently working in /home/fred and wish to move to the /usr/bin directory:

- Absolute pathname method: `$ cd /usr/bin`
- Relative pathname method: `$ cd ../../usr/bin`

![](images/7.2.1.png)

## Exploring the Filesystem
| Command    | Result |
| ---------   | -------- |
| ls | List the contents of the present working directory |
| ls –a | List all files, including hidden files and directories (those whose name start with . ) |
| df -h | Report file system disk space usage with sizes in powers of 1024 |

## Navigating the Directory History
The cd command remembers where you were last, and lets you get back there with cd -. For remembering more than just the last directory visited, use pushd to change the directory instead of cd; this pushes your starting directory onto a list. Using popd will then send you back to those directories, walking in reverse order (the most recent directory will be the first one retrieved with popd). The entire list is displayed with the dirs command.

![](images/7.2.2.png)

## Hard Links
The ln utility is used to create hard links and soft links, also known as symbolic links or symlinks. 

Suppose that file1 already exists. A hard link, called file2, is created with the command: `$ ln file1 file2`

Note that two files now appear to exist. However, a closer inspection of the file listing shows that this is not quite true: `$ ls -li file1 file2`. The -i option to ls prints out in the first column the inode number, which is a unique quantity for each file object. This field is the same for both of these files; what is really going on here is that it is only one file, but it has more than one name associated with it, as is indicated by the 2 that appears in the ls output. Thus, there was already another object linked to file1 before the command was executed.

![](images/7.2.3.png)

Hard links are very useful and they save space, but you have to be careful with their use, sometimes in subtle ways. For one thing, if you remove either file1 or file2 in the example, the inode object (and the remaining file name) will remain, which might be undesirable, as it may lead to subtle errors later if you recreate a file of that name.

If you edit one of the files, exactly what happens depends on your editor; most editors, including vi and gedit, will retain the link by default, but it is possible that modifying one of the names may break the link and result in the creation of two objects.

## Soft Links
Soft (or Symbolic) links are created with the -s option, as in:

```
$ ln -s file1 file3
$ ls -li file1 file3
```

Notice file3 no longer appears to be a regular file, and it clearly points to file1 and has a different inode number.

![](images/7.2.4.png)

Symbolic links take no extra space on the filesystem (unless their names are very long). They are extremely convenient, as they can easily be modified to point to different places. An easy way to create a shortcut from your home directory to long pathnames is to create a symbolic link.

Unlike hard links, soft links can point to objects even on different filesystems, partitions, and/or disks and other media, which may or may not be currently available or even exist. In the case where the link does not point to a currently available or existing object, you obtain a dangling link.

## Viewing Files
| Command    | Usage |
| ---------   | -------- |
| cat | Used for viewing files that are not very long; it does not provide any scroll-back |
| tac | Used to look at a file backwards, starting with the last line |
| less | Used to view larger files - it pauses at each screen full of text, provides scroll-back capabilities, and lets you search and navigate within the file |
| tail | Used to print the last 10 lines of a file by default. You can change the number of lines by doing -n 15 or just -15 if you wanted to look at the last 15 lines instead of the default |
| head | The opposite of tail; by default, it prints the first 10 lines of a file |

## touch
touch is often used to set or update the access, change, and modify times of files. By default, it resets a file's timestamp to match the current time. However, you can also create an empty file using touch: `$ touch <filename>`.

touch provides several useful options. For example, the -t option allows you to set the date and timestamp of the file to a specific value: `$ touch -t 12091600 myfile`. This sets the myfile file's timestamp to 4 p.m., December 9th (12 09 1600).

## mkdir
mkdir is used to create a directory:
- mkdir sampdir
  - It creates a sample directory named sampdir under the current directory
- mkdir /usr/sampdir
  - It creates a sample directory called sampdir under /usr
- mkdir -p /tmp/somedir/subdir
  - It creates a sample directory named subdir if somedir/ or tmp/ does not exist

## Moving, Renaming or Removing a File
Note that mv does double duty, in that it can:
- Simply rename a file
- Move a file to another location, while possibly changing its name at the same time.

| Command    | Usage |
| ---------   | -------- |
| mv | Rename a file |
| rm | Remove a file |
| rm –f | Forcefully remove a file |
| rm –i | Interactively remove a file |

## Renaming or Removing a Directory
rmdir works only on empty directories; otherwise you get an error. 

| Command    | Usage |
| ---------   | -------- |
| mv | Rename a directory |
| rmdir | Remove an empty directory |
| rm -rf | Forcefully remove a directory recursively |

## Modifying the Command Line Prompt
The PS1 variable is the character string that is displayed as the prompt on the command line. Some system administrators require the user and the host system name to show up on the command line as in: `student@r9 $`. This could prove useful if you are working in multiple roles and want to be always reminded of who you are and what machine you are on.

```
$ echo $PS1
\$
$ PS1="\u@\h \$ "
student@r9 $ echo $PS1
\u@\h \$
student@r9 $
```

By convention, most systems are set up so that the root user has a pound sign (#) as their prompt.

## Package Management Systems on Linux
The core parts of a Linux distribution and most of its add-on software are installed via the Package Management System. Each package contains the files and other instructions needed to make one software component work well and cooperate with the other components that comprise the entire system. Packages can depend on each other. For example, a package for a web-based application written in Python will require the appropriate Python packages to be installed first.

There are two broad families of package managers widely deployed: those based on Debian and those which use RPM as their low-level package manager. The two systems are incompatible but, broadly speaking, provide the same essential features and satisfy the same needs. In addition, there are some other systems used by more specialized Linux distributions.