# Printing

To manage printers and print either directly from a local computer or across a networked environment, you need to know how to configure and install a printer. Printing itself requires software that converts information from the application you are using to a language your printer can understand. The Linux standard for printing software is the Common UNIX Printing System (CUPS).

## CUPS Overview
CUPS is the underlying software Linux systems use to print from applications. It interprets page descriptions produced by your application and then sends the information to the printer. It acts as a print server for both local and network printers.

Printers manufactured by different companies may use their own particular print languages and formats. CUPS uses a modular printing system that accommodates a wide variety of printers and also processes various data formats. This makes the printing process simpler; you can concentrate more on printing and less on how to print.

CUPS carries out the printing process with the help of its various components:

![](./images/17.1.1.png)

In short, when you execute a print command, the scheduler validates the command and processes the print job, creating job files according to the settings specified in the configuration files. Simultaneously, the scheduler records activities in the log files. Job files are processed with the help of the filter, printer driver, and backend, and then sent to the printer.

### Scheduler
CUPS is designed around a print scheduler that manages print jobs, handles administrative commands, allows users to query the printer status, and manages the flow of data through all CUPS components.

### Configuration Files
The print scheduler reads server settings from several configuration files, the two most important of which are cupsd.conf and printers.conf. These and all other CUPS-related configuration files are stored under the /etc/cups/ directory.

cupsd.conf is where most system-wide settings are located; it does not contain any printer-specific details. Most of the settings available in this file relate to network security, i.e., which systems can access CUPS network capabilities, how printers are advertised on the local network, what management features are offered, and so on.

printers.conf is where you will find the printer-specific settings. For every printer connected to the system, a corresponding section describes the printer's status and capabilities. This file is generated or modified only after adding a printer to the system and should not be modified by hand.

You can view the full list of configuration files by typing `ls -lF /etc/cups`.

### Job Files
CUPS stores print requests as files under the /var/spool/cups directory (these can actually be accessed before a document is sent to a printer). Data files are prefixed with the letter d, while control files are prefixed with the letter c. After a printer successfully handles a job, data files are automatically removed. These data files belong to what is commonly known as the print queue.

![](./images/17.1.2.png)

### Log Files
Log files are placed in /var/log/cups and are used by the scheduler to record activities that have taken place. These files include access, error, and page records. To view the log files, type `$ sudo ls -l /var/log/cups`.

### Filters, Printer Drivers, and Backends
CUPS uses filters to convert job file formats to printable formats. Printer drivers contain descriptions for currently connected and configured printers, and are usually stored under /etc/cups/ppd/. The print data is then sent to the printer through a filter, and via a backend that helps to locate devices connected to the system.

## Managing CUPS
Assuming CUPS has been installed you'll need to start and manage the CUPS daemon so that CUPS is ready for configuring a printer. Managing the CUPS daemon is simple; all management features can be done with the systemctl utility:

```
$ systemctl status cups
$ sudo systemctl [enable|disable] cups
$ sudo systemctl [start|stop|restart] cups
```

## Adding Printers from the CUPS Web Interface
Many users do not know CUPS comes with its own web server, which makes a configuration interface available via a set of CGI scripts. It is available at localhost:631.

Some pages require a username and password to perform certain actions, for example to add a printer. For most Linux distributions, you must use the root password to add, modify, or delete printers or classes.

![](./images/17.1.3.png)

## Printing from the Command-Line Interface
CUPS provides two command-line interfaces, descended from the System V and BSD flavors of UNIX. This means that you can use either lp (System V) or lpr (BSD) to print. 

lp is just a command line front-end to the lpr utility that passes input to lpr. In the example shown here, the task is to print $HOME/.emacs.

![](./images/17.2.1.png)

### Using lp
lp and lpr accept command line options that help you perform all operations that the GUI can accomplish. lp is typically used with a file name as an argument.

lpoptions can be used to set printer options and defaults. Each printer has a set of tags associated with it, such as the default number of copies and authentication requirements. You can type lpoptions help to obtain a list of supported options. lpoptions can also be used to set system-wide values, such as the default printer.

| Command | Usage |
| - | - |
| lp filename | To print the file to default printer |
| lp -d printer filename | To print to a specific printer |
| program \| lp <br> echo string \| lp | To print the output of a program |
| lp -n number filename | To print multiple copies |
| lpoptions -d printer | To set the default printer |
| lpq -a | To show the queue status |
| lpadmin | To configure printer queues |

### Managing Print Jobs
You send a file to the shared printer. But when you go there to collect the printout, you discover another user has just started a 200 page job that is not time sensitive. Your file cannot be printed until this print job is complete. What do you do now?

In Linux, command line print job management commands allow you to monitor the job state as well as managing the listing of all printers and checking their status, and canceling or moving print jobs to another printer.

| Command | Usage |
| - | - |
| lpstat -p -d | To get a list of available printers, along with their status |
| lpstat -a | To check the status of all connected printers, including job numbers |
| cancel job-id <br> lprm job-id | To cancel a print job |
| lpmove job-id newprinter | To move a print job to new printer |

## Working with PostScript and PDF
PostScript is a standard page description language. It effectively manages scaling of fonts and vector graphics to provide quality printouts. It is purely a text format that contains the data fed to a PostScript interpreter. The format itself is a language that Adobe developed in the early 1980s to enable the transfer of data to printers.

Postscript has been, for the most part, superseded by the PDF format (Portable Document Format), which produces far smaller files in a compressed format for which support has been integrated into many applications. 

From time to time, you may need to convert files from one format to the other, and there are very simple utilities for accomplishing that task. ps2pdf and pdf2ps are part of the ghostscript package installed on or available on all Linux distributions. As an alternative, there are pstopdf and pdftops which are usually part of the poppler package, which may need to be added through your package manager. Unless you are doing a lot of conversions or need some of the fancier options (which you can read about in the man pages for these utilities), it really does not matter which ones you use.

Another possibility is to use the very powerful convert program, which is part of the ImageMagick package. Some newer distributions have replaced this with Graphics Magick, and the command to use is gm convert.

| Command | Usage |
| - | - |
| pdf2ps file.pdf | Converts file.pdf to file.ps |
| ps2pdf file.ps | Converts file.ps to file.pdf |
| pstopdf input.ps output.pdf | Converts input.ps to output.pdf |
| pdftops input.pdf output.ps | Converts input.pdf to output.ps |
| convert input.ps output.pdf | Converts input.ps to output.pdf |
| convert input.pdf output.ps | Converts input.pdf to output.ps |

## Manipulating PDFs
At times, you may want to merge, split, or rotate PDF files; not all of these operations can be achieved while using a PDF viewer. In order to accomplish these tasks, there are several programs available: qpdf, pdftk, and ghostscript.

qpdf is widely available on Linux distributions and is very full-featured. pdftk was once very popular but depended on an obsolete unmaintained package (libgcj), and a number of distributions dropped it. However, it has now been reimplemented in Java and is available again on most distributions under the name pdftk-java. Ghostscript (often invoked using gs) is widely available and well-maintained. However, its usage is a little complex.

### Using qpdf
| Command | Usage |
| - | - |
| qpdf --empty --pages 1.pdf 2.pdf -- 12.pdf | Merge the two documents 1.pdf and 2.pdf and save to 12.pdf. |
| qpdf --empty --pages 1.pdf 1-2 -- new.pdf | Write only pages 1 and 2 of 1.pdf and save to new.pdf. |
| qpdf --rotate=+90:1 1.pdf 1r.pdf | Rotate page 1 of 1.pdf 90 degrees clockwise and save to 1r.pdf. |
| qpdf --rotate=+90:1-z 1.pdf 1r-all.pdf | Rotate all pages of 1.pdf 90 degrees clockwise and save to 1r-all.pdf |
| qpdf --encrypt mypw mypw 128 -- public.pdf private.pdf | Encrypt with 128 bits public.pdf using as the passwd mypw with output as private.pdf. |
| qpdf --decrypt --password=mypw private.pdf file-decrypted.pdf | Decrypt private.pdf with output as file-decrypted.pdf. |

### Using pdftk
As mentioned earlier, Marc Vinyals has developed and maintained a port to Java for pdftk which can be found on GitLab. Some distributions, such as Ubuntu, may install this version only. 

| Command | Usage |
| - | - |
| pdftk 1.pdf 2.pdf cat output 12.pdf | Merge the two documents 1.pdf and 2.pdf. The output will be saved to 12.pdf. |
| pdftk A=1.pdf cat A1-2 output new.pdf | Write only pages 1 and 2 of 1.pdf. The output will be saved to new.pdf. |
| pdftk A=1.pdf cat A1-endright output new.pdf | Rotate all pages of 1.pdf 90 degrees clockwise and save result in new.pdf. |

If you’re working with PDF files that contain confidential information and you want to ensure that only certain people can view the PDF file, you can apply a password to it using the user_pw option: `$ pdftk public.pdf output private.pdf user_pw PROMPT`.

### Using Ghostscript
Ghostscript is widely available as an interpreter for the Postscript and PDF languages. The executable program associated with it is abbreviated to gs. Use is somewhat complicated by the rather long nature of the options. For example:
- Combine three PDF files into one
  ```
  $ gs -dBATCH -dNOPAUSE -q -sDEVICE=pdfwrite  -sOutputFile=all.pdf file1.pdf file2.pdf file3.pdf
  ```
- Split pages 10 to 20 out of a PDF file
  ```
  $ gs -sDEVICE=pdfwrite -dNOPAUSE -dBATCH -dDOPDFMARKS=false -dFirstPage=10 -dLastPage=20 -sOutputFile=split.pdf file.pdf
  ```
