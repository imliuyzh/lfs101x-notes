# Finding Linux Documentation

## The man pages
The man pages are the most often-used source of Linux documentation. They are present on all Linux distributions and are always at your fingertips. Typing man with a topic name as an argument retrieves the information stored in the topic's man pages.

The man pages infrastructure was first introduced in the early UNIX versions, at the beginning of the 1970s. The name man is just an abbreviation for manual.

### man
The man program searches, formats, and displays the information contained in the man page system. Because many topics have copious amounts of relevant information, output is piped through a pager program (such as less) to be viewed one page at a time. At the same time, the information is formatted for a good visual display.

A given topic may have multiple pages associated with it and there is a default order determining which one is displayed when no options or section number is specified. To list all pages on the topic, use the -f option. To list all pages that discuss a specific topic (even if the specified subject is not present in the name), use the –k option.

The default order is specified in /etc/man_db.conf and is roughly in ascending numerical order by section.

![](images/8.1.1.png)

### Manual Chapters
The man pages are divided into chapters numbered 1 through 9. In some cases, a letter is appended to the chapter number to identify a specific topic. For example, many pages describing part of the X Window API are in chapter 3X.

The chapter number can be used to force man to display the page from a particular chapter. It is common to have multiple pages across multiple chapters with the same name, especially for names of library functions or system calls.

With the -a parameter, man will display all pages with the given name in all chapters, one after the other: `$ man -a socket`.

## The GNU Info System
This is the GNU project's standard documentation format, which it prefers as an alternative to man. The Info System is basically free-form, and supports linked subsections.

Functionally, info resembles man in many ways. However, topics are connected using links (even though its design predates the World Wide Web). Information can be viewed through either a command line interface, a graphical help utility, printed or viewed online.

### Using info from the Command Line
Typing info with no arguments in a terminal window displays an index of available topics. You can browse through the topic list using the regular movement keys: arrows, Page Up, and Page Down.

You can view help for a particular topic by typing info <topic name>. The system then searches for the topic in all available info files.

Some useful keys are: q to quit, h for help, and Enter to select a menu item.

### info Page Structure
The topic which you view in an info page is called a node. Nodes are essentially sections and subsections in the documentation. You can move between nodes or view each node sequentially. Each node may contain menus and linked subtopics, or items.

Items function like browser links and are identified by an asterisk (*) at the beginning of the item name. Named items (outside a menu) are identified with double-colons (::) at the end of the item name. Items can refer to other nodes within the file or to other files. 

| Key | Function |
| --- | ----------------------------- |
|  n  | Go to next node |
|  p  | Go to previous node |
|  u  | Move one node up in the index |

## The --help Option and help Command
Most commands have an available short description which can be viewed using the --help or the -h option along with the command or application. For example, to learn more about the man command, you can type: `$ man --help`.

When run within a bash command shell, some popular commands actually run especially built-in bash versions of the commands rather than the usual binaries found on the file system, say under /bin or /usr/bin. It is more efficient to do so as execution is faster because fewer resources are used. To view a synopsis of these built-in commands, you can simply type help. 

## Other Documentation Sources

### Graphical Help Systems
All Linux desktop systems have a graphical help application. This application is usually displayed as a question-mark icon or an image of a ship's life preserver, and can also always be found within the menu system. These programs usually contain custom help for the desktop itself and some of its applications, and will sometimes also include graphically-rendered info and man pages. You can also start the graphical help system from a terminal window or command prompt by using one of the following utility programs:

- GNOME: gnome-help or yelp
- KDE: khelpcenter

### Package Documentation
Linux documentation is also available as part of the package management system. Usually, this documentation is directly pulled from the upstream source code, but it can also contain information about how the distribution packaged and set up the software. Such information is placed under the /usr/share/doc directory, grouped in subdirectories named after each package.
