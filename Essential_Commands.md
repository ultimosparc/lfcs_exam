# Essential_Commands

Essential Commands domain might appears more frequently than others in the exam. 

_Archivieren von Dateien_ [9]

Dateien können in Archiven verpackt werden. Das Tool dafür ist _tar_. 
Ein möglicher Befehl zum Generieren eines Archives könnte wie folgt aussehen: 

    tar cvf test.tar datei1 datei2 verzeichnis1

Dieser Befehl generiert ein Archiv "test.tar" (-c) aus den Dateien "datei1" und "datei2" und dem Verzeichnis "verzeichnis1". Dabei wird im Terminal ausgegeben, welche Dateien ins Archiv gepackt werden (-v). 
Das Entpacken eines Archivs kann wie folgt realisiert werden: 

    tar xvf test.tar (Size: 10 KB)
    
Die Option -x steht für Entpacken. Es gibt zusätzlich die Möglichkeit, ein Archiv zu komprimieren. Unteranderem gibt es folgende Möglichkeiten der Kompromierung:

    -z gzip (Size: 131 B)
    -j bzip2/bz (Size: 140 B)
    -J xz (Size: 180 B)
    
Wie im Beispiel beobachtet werden kann, das Archiv kann bis um das 10fache verkleinert werden. Damit wird klar, jedes Archiv sollte komprimiert werden. Gewöhnlich benutzt man gzip, um ein tar Archiv zu komprimieren. 
Weitere Ooptionen: 

    -t Auflisten des Inhaltes des Archives
    -r man fügt Dateien zu einem Archiv
    -d Löschen einzlner Dateien
    -u Austauschen von einzlnen Dateien zw Update
    -p man archiviert Dateien und den Rechten auf den Dateien
    -f muss gesetzt werden, damit Grundbefehle we c,d ausgeführt werden kann
 
 
_Suchen nach Dateien_ [10]

Es gibt verschiedene Befehle zum Suchen von Textdateien. Meist wird der Befehl _find_ genutzt. It can be u
to search by name, file type, timestamp, owner, and many other attributes. Ein möglicher Befehl kann wie folgt aussehen:

    find <folder> -name <file>
    
<folder> und <file> sind Platzhalter. Falls <folder> wegfällt, dann sucht das Programm im lokalen Ordner. Weitere Operationen: 
    
    -iname incasitive Suche nach Dateien
    -type <d> Suche nach File-Typen (d Verzeichnis, f Datei, l Link)
    -user <name> Suche nach Besitzer
    -atime <time> Suche nach Generierungszeit
    -size <size> Suche nach Dateigröße
    -perm <Permissions> Suche nach Dateien durch die Rechte
    
Ein besondere Option des _find_ Befehls zeigt das folgende Beispiel

    find . -perm 777 -exec rm -f '{}' \;
    
Das Beispiel sucht nach Dateien im lokalen Ordner, die mindestens die Berechtigung von 777 haben. Falls gefunden, dann einmal löschen, ohne extra nachzufragen. {} will be substitute with result of find. 
The exec's command must be contained between -exec and \;.
; is treated as end of command character in bash shell. For this I must escape it with \. If escaped it will be interpreted by find and not by bash shell.
Es existieren weitere Befehhle nach Dateien zu suchen. 

    which – returns the location of the command base on the PATH settings
    whereis – returns the location of the binary, source file, and man page, it can return multiple versions of command if they exist.
    type – returns information about the command type and details ba
    on how the command is related to the shell configuration.
    locate sucht wie find nach Datei, jedoch über eine interne Datenbank
    
Ein weiteres Beispiel zeigt, wie man nach Dateien mit einer bestimmten Größe sucht.

    find ~ -maxdepth 1 -type f -size -20c
    
 Sucht im Heimverzeichnis nach Dateien, keine Ordner, von der Größe kleiner oder gleich 20 Bytes. Dateien in Unterordner werden nicht durchsucht (maxdepth).
 c steht für Bytes, es gibt noch: b für 512 Blocksize, w 2 Bytes, k Kilo Bytes, M für Megabytes, G Gigabytes. GroßKleinschreibung muss beachtet werden. Der Ausdruck "-20c" kann auch anders geschrieben werden. =20c Dateien der genauen Größe, +20c Dateien die größer sind 20c genau der größe. 
 Nach rechten kann auch gesucht werden. Beispiel: 
 
    find ~ -maxdepth 1 -type f -perm u=rw

_Bearbeitung von Dateien_[25]

Mit dem Befehl file kann man Information über den Typ der Datei bekommen. Zum Auslesen einer Datei gibt es mehrere Befehle wie cat, less, more, head, tail. 

Die Befehle head/tail geben jeweils die ersten/letzten 10 Zeilen einer Datei aus. Man kann auch die Ausgabe anpassen. Beispiel: 

    head -n 5 testfile
    
gibt die ersten 5 Zeilen der testfile wieder. 
Es gib verschiedene Befehle eine Textdatei zu inhaltlich zu manipulieren. Der einfachste Befehl ist echo, der was in eine Textdatei schreibt. Ein weitere Befehl wäre the cut Funktion. 
The cut command in UNIX is a command for cutting out the sections from each line of files and writing the result to standard output. It can be used to cut parts of a line by byte position, character and field. Basically the cut command slices a line and extracts the text. It is necessary to specify option with command otherwise it gives error. If more than one file name is provided then data from each file is not precedes by its file name.

Syntax:

cut OPTION... [FILE]...

Let us consider two files having name state.txt and capital.txt contains 5 names of the Indian states and capitals respectively.

    $ cat state.txt
    Andhra Pradesh
    Arunachal Pradesh
    Assam
    Bihar
    Chhattisgarh

Without any option specified it displays error.

    $ cut state.txt
    cut: you must specify a list of bytes, characters, or fields
    Try 'cut --help' for more information.

2. -c (column): To cut by character use the -c option. This selects the characters given to the -c option. This can be a list of numbers separated comma or a range of numbers separated by hyphen(-). Tabs and backspaces are treated as a character. It is necessary to specify list of character numbers otherwise it gives error with this option.

Syntax:

$cut -c [(k)-(n)/(k),(n)/(n)] filename

Here,k denotes the starting position of the character and n denotes the ending position of the character in each line, if k and n are separated by “-” otherwise they are only the position of character in each line from the file taken as an input.

$ cut -c 2,5,7 state.txt
nr
rah
sm
ir
hti

Above cut command prints second, fifth and seventh character from each line of the file.

$ cut -c 1-7 state.txt
Andhra
Arunach
Assam
Bihar
Chhatti

Above cut command prints first seven characters of each line from the file.

Cut uses a special form for selecting characters from beginning upto the end of the line:


$ cut -c 1- state.txt
Andhra Pradesh
Arunachal Pradesh
Assam
Bihar
Chhattisgarh

Above command prints starting from first character to end. Here in command only starting
position is specified and the ending position is omitted.

$ cut -c -5 state.txt
Andhr
Aruna
Assam
Bihar
Chhat

Above command prints starting position to the fifth character. Here the starting position
is omitted and the ending position is specified.

3. -f (field): -c option is useful for fixed-length lines. Most unix files doesn’t have fixed-length lines. To extract the useful information you need to cut by fields rather than columns. List of the fields number specified must be separated by comma. Ranges are not described with -f option. cut uses tab as a default field delimiter but can also work with other delimiter by using -d option.
Note: Space is not considered as delimiter in UNIX.

Syntax:

$cut -d "delimiter" -f (field number) file.txt

Like in the file state.txt fields are separated by space if -d option is not used then it prints whole line:

$ cut -f 1 state.txt
Andhra Pradesh
Arunachal Pradesh
Assam
Bihar
Chhattisgarh

If -d option is used then it considered space as a field separator or delimiter:

$ cut -d " " -f 1 state.txt
Andhra
Arunachal
Assam
Bihar
Chhattisgarh

Command prints field from first to fourth of each line from the file.
Command:
$ cut -d " " -f 1-4 state.txt
Output:
Andhra Pradesh
Arunachal Pradesh
Assam
Bihar
Chhattisgarh

SORT command is used to sort a file, arranging the records in a particular order. By default, the sort command sorts file assuming the contents are ASCII. Using options in sort command, it can also be used to sort numerically.

SORT command sorts the contents of a text file, line by line.
sort is a standard command line program that prints the lines of its input or concatenation of all files listed in its argument list in sorted order.
The sort command is a command line utility for sorting lines of text files. It supports sorting alphabetically, in reverse order, by number, by month and can also remove duplicates.
The sort command can also sort by items not at the beginning of the line, ignore case sensitivity and return whether a file is sorted or not. Sorting is done based on one or more sort keys extracted from each line of input.
By default, the entire input is taken as sort key. Blank space is the default field separator.
The sort command follows these features as stated below:

Lines starting with a number will appear before lines starting with a letter.
Lines starting with a letter that appears earlier in the alphabet will appear before lines starting with a letter that appears later in the alphabet.
Lines starting with a lowercase letter will appear before lines starting with the same letter in uppercase.

$ sort filename.txt

Command:
$ sort file.txt

Output :
abhishek
chitransh
divyam
harsh
naveen 
rajan
satish

    -u sortiert und löscht doppelte Einträge
    -n numerisch sortieren
    -k N sortiert die Spalte N 
    -o exportiert Ergbnis in eine neue Datei
    -r zngekehrte Sortierung
    -c checkt ob datei sortiert ist
    
The uniq command in Linux is a command line utility that reports or filters out the repeated lines in a file.
In simple words, uniq is the tool that helps to detect the adjacent duplicate lines and also deletes the duplicate lines. uniq filters out the adjacent matching lines from the input file(that is required as an argument) and writes the filtered data to the output file .

Syntax of uniq Command :

 //...syntax of uniq...// 
$uniq [OPTION] [INPUT[OUTPUT]]
The syntax of this is quite easy to understand. Here, INPUT refers to the input file in which repeated lines need to be filtered out and if INPUT isn’t specified then uniq reads from the standard input. OUTPUT refers to the output file in which you can store the filtered output generated by uniq command and as in case of INPUT if OUTPUT isn’t specified then uniq writes to the standard output.

Now, let’s understand the use of this with the help of an example. Suppose you have a text file named kt.txt which contains repeated lines that needs to be omitted. This can simply be done with uniq.

//displaying contents of kt.txt//

$cat kt.txt
I love music.
I love music.
I love music.

I love music of Kartik.
I love music of Kartik.

Thanks.
Now, as we can see that the above file contains multiple duplicate lines. Now, lets’s use uniq command to remove them:



//...using uniq command.../

$uniq kt.txt
I love music.

I love music of Kartik.

-c – -count : It tells how many times a line was repeated by displaying a number as a prefix with the line.
-d – -repeated : It only prints the repeated lines and not the lines which aren’t repeated.
-D – -all-repeated[=METHOD] : It prints all duplicate lines and METHOD can be any of the following:
none : Do not delimit duplicate lines at all. This is the default.
prepend : Insert a blank line before each set of duplicated lines.
separate : Insert a blank line between each set of duplicated lines.
-f N – -skip-fields(N) : It allows you to skip N fields(a field is a group of characters, delimited by whitespace) of a line before determining uniqueness of a line.
-i – -ignore case : By default, comparisons done are case sensitive but with this option case insensitive comparisons can be made.
-s N – -skip-chars(N) : It doesn’t compares the first N characters of each line while determining uniqueness. This is like the -f option, but it skips individual characters rather than fields.
-u – -unique : It allows you to print only unique lines.
-z – -zero-terminated : It will make a line end with 0 byte(NULL), instead of a newline.
-w N – -check-chars(N) : It only compares N characters in a line.
– – help : It displays a help message and exit.
– – version : It displays version information and exit.

Man Dateien miteinander vergleichen über den Befehl diff beispielsweise. Aufbau: 

    diff PARAMETER FILES
    
Zum Beispiel, man vergleicht zwei Daeien miteinander, dann werden eine Liste von Unterschieden zwischen den zwei Dateien im Terminal ausgeben
The important thing to remember is that diff uses certain special symbols and instructions that are required to make two files identical. It tells you the instructions on how to change the first file to make it match the second file.

Special symbols are:

    a : add
    c : change
    d : delete

Beispiel:

    $ ls
    a.txt  b.txt

    $ cat a.txt
    Gujarat
    Uttar Pradesh
    Kolkata
    Bihar
    Jammu and Kashmir

    $ cat b.txt
    Tamil Nadu
    Gujarat
    Andhra Pradesh
    Bihar
    Uttar pradesh
    
Now, applying diff command without any option we get the following output:

    $ diff a.txt b.txt
    0a1
    > Tamil Nadu
    2,3c3
    < Uttar Pradesh
     Andhra Pradesh
    5c5
     Uttar pradesh
     
Let’s take a look at what this output means. The first line of the diff output will contain:

Line numbers corresponding to the first file,
A special symbol and
Line numbers corresponding to the second file.
Like in our case, 0a1 which means after lines 0(at the very beginning of file) you have to add Tamil Nadu to match the second file line number 1. It then tells us what those lines are in each file preceeded by the symbol:

    Lines preceded by a < are lines from the first file.
    Lines preceded by > are lines from the second file.
    Next line contains 2,3c3 which means from line 2 to line 3 in the first file needs to be changed to match line number 3 in the second file. It then tells us those lines with the above symbols.
    
The three dashes (“—“) merely separate the lines of file 1 and file 2.

SED command in UNIX is stands for stream editor and it can perform lot’s of function on file like, searching, find and replace, insertion or deletion. Though most common use of SED command in UNIX is for substitution or for find and replace. By using SED you can edit files even without opening it, which is much quicker way to find and replace something in file, than first opening that file in VI Editor and then changing it.

SED is a powerful text stream editor. Can do insertion, deletion, search and replace(substitution).
SED command in unix supports regular expression which allows it perform complex pattern matching.
Syntax:

sed OPTIONS... [SCRIPT] [INPUTFILE...] 
Example:
Consider the below text file as an input.

sed -e command <filename>

Specify editing commands at the command line, operate on file and put the output on standard out (e.g. the terminal)
sed -f scriptfile <filename>	Specify a scriptfile containing sed commands, operate on file and put output on standard out
    
Now that you know that you can perform multiple editing and filtering operations with sed, let’s explain some of them in more detail. The table explains some basic operations, where pattern is the current string and replace_string is the new string:

 

Command	Usage
sed s/pattern/replace_string/ file	Substitute first string occurrence in every line
sed s/pattern/replace_string/g file	Substitute all string occurrences in every line
sed 1,3s/pattern/replace_string/g file	Substitute all string occurrences in a range of lines
sed -i s/pattern/replace_string/g file	Save changes for string substitution in the same file
 

You must use the -i option with care, because the action is not reversible. It is always safer to use sed without the –i option and then replace the file yourself, as shown in the following example:

$ sed s/pattern/replace_string/g file1 > file2

The above command will replace all occurrences of pattern with replace_string in file1 and move the contents to file2. The contents of file2 can be viewed with cat file2. If you approve you can then overwrite the original file with mv file2 file1.

Example: To convert 01/02/… to JAN/FEB/…

sed -e 's/01/JAN/' -e 's/02/FEB/' -e 's/03/MAR/' -e 's/04/APR/' -e 's/05/MAY/' \
    -e 's/06/JUN/' -e 's/07/JUL/' -e 's/08/AUG/' -e 's/09/SEP/' -e 's/10/OCT/' \
    -e 's/11/NOV/' -e 's/12/DEC/'
    
AWK awk is used to extract and then print specific contents of a file and is often used to construct reports. It was created at Bell Labs in the 1970s and derived its name from the last names of its authors: Alfred Aho, Peter Weinberger, and Brian Kernighan.

awk has the following features:

It is a powerful utility and interpreted programming language.
It is used to manipulate data files, retrieving, and processing text.
It works well with fields (containing a single piece of data, essentially a column) and records (a collection of fields, essentially a line in a file).
awk is invoked as shown in the following:

As with sed, short awk commands can be specified directly at the command line, but a more complex script can be saved in a file that you can specify using the -f option.

 

Command	Usage
awk ‘command’  file	Specify a command directly at the command line
awk -f scriptfile file	Specify a file that contains the script to be executed

The table explains the basic tasks that can be performed using awk. The input file is read one line at a time, and, for each line, awk matches the given pattern in the given order and performs the requested action. The -F option allows you to specify a particular field separator character. For example, the /etc/passwd file uses ":" to separate the fields, so the -F: option is used with the /etc/passwd file.

The command/action in awk needs to be surrounded with apostrophes (or single-quote (')). awk can be used as follows:

 Suppose you have a file that contains the full name of all employees and another file that lists their phone numbers and Employee IDs. You want to create a new file that contains all the data listed in three columns: name, employee ID, and phone number. How can you do this effectively without investing too much time?

paste can be used to create a single file containing all three columns. The different columns are identified based on delimiters (spacing used to separate two fields). For example, delimiters can be a blank space, a tab, or an Enter. In the image provided, a single space is used as the delimiter in all files.

paste accepts the following options:

-d delimiters, which specify a list of delimiters to be used instead of tabs for separating consecutive values on a single line. Each delimiter is used in turn; when the list has been exhausted, paste begins again at the first delimiter.
-s, which causes paste to append the data in series rather than in parallel; that is, in a horizontal rather than vertical fashion.
 
 paste can be used to combine fields (such as name or phone number) from different files, as well as combine lines from multiple files. For example, line one from file1 can be combined with line one of file2, line two from file1 can be combined with line two of file2, and so on.

To paste contents from two files one can do:

$ paste file1 file2

The syntax to use a different delimiter is as follows:

$ paste -d, file1 file2

Common delimiters are 'space', 'tab', '|', 'comma', etc.

Suppose you have two files with some similar columns. You have saved employees’ phone numbers in two files, one with their first name and the other with their last name. You want to combine the files without repeating the data of common columns. How do you achieve this?

The above task can be achieved using join, which is essentially an enhanced version of paste. It first checks whether the files share common fields, such as names or phone numbers, and then joins the lines in two files based on a common field.

split is used to break up (or split) a file into equal-sized segments for easier viewing and manipulation, and is generally used only on relatively large files. By default, split breaks up a file into 1000-line segments. The original file remains unchanged, and a set of new files with the same name plus an added prefix is created. By default, the x prefix is added. To split a file into segments, use the command split infile.

To split a file into segments using a different prefix, use the command split infile <Prefix>.
    
    We will apply split to an American-English dictionary file of over 99,000 lines:

$ wc -l american-english
99171 american-english

where we have used wc (word count, soon to be discussed) to report on the number of lines in the file. Then, typing:

$ split american-english dictionary

will split the American-English file into 100 equal-sized segments named dictionaryxx. The last one will of course be somewhat smaller.

strings is used to extract all printable character strings found in the file or files given as arguments. It is useful in locating human-readable content embedded in binary files; for text files one can just use grep.

For example, to search for the string my_string in a spreadsheet:

$ strings book1.xls | grep my_string

The screenshot shows a  search of a number of programs to see which ones have GPL licenses of various versions.

The tr utility is used to translate specified characters into other characters or to delete them. The general syntax is as follows:

$ tr [options] set1 [set2]

The items in the square brackets are optional. tr requires at least one argument and accepts a maximum of two. The first, designated set1 in the example, lists the characters in the text to be replaced or removed. The second, set2, lists the characters that are to be substituted for the characters listed in the first argument. Sometimes these sets need to be surrounded by apostrophes (or single-quotes (')) in order to have the shell ignore that they mean something special to the shell. It is usually safe (and may be required) to use the single-quotes around each of the sets as you will see in the examples below.

For example, suppose you have a file named city containing several lines of text in mixed case. To translate all lower case characters to upper case, at the command prompt type cat city | tr a-z A-Z and press the Enter key.

 

Command	Usage
tr abcdefghijklmnopqrstuvwxyz ABCDEFGHIJKLMNOPQRSTUVWXYZ	Convert lower case to upper case
tr '{}' '()' < inputfile > outputfile	Translate braces into parenthesis
echo "This is for testing" | tr [:space:] '\t'	Translate white-space to tabs
echo "This   is   for    testing" | tr -s [:space:]
Squeeze repetition of characters using -s
echo "the geek stuff" | tr -d 't'	Delete specified characters using -d option
echo "my username is 432234" | tr -cd [:digit:]	Complement the sets using -c option
tr -cd [:print:] < file.txt	Remove all non-printable character from a file
tr -s '\n' ' ' < file.txt	Join all the lines in a file into a single line

Here is the basic structure of the case statement:

case expression in
   pattern1) execute commands;;
   pattern2) execute commands;;
   pattern3) execute commands;;
   pattern4) execute commands;;
   * )       execute some default commands or nothing ;;
esac

You can run a bash script in debug mode either by doing bash –x ./script_file, or bracketing parts of the script with set -x and set +x. The debug mode helps identify the error because:

It traces and prefixes each command with the + character.
It displays each command before executing it.
It can debug only selected parts of a script (if desired) with:

set -x    # turns on debugging
...
set +x    # turns off debugging
The screenshot shown here demonstrates a script which runs in debug mode if run with any argument on the command line.

Consider a situation where you want to retrieve 100 records from a file with 10,000 records. You will need a place to store the extracted information, perhaps in a temporary file, while you do further processing on it.

Temporary files (and directories) are meant to store data for a short time. Usually, one arranges it so that these files disappear when the program using them terminates. While you can also use touch to create a temporary file, in some circumstances this may make it easy for hackers to gain access to your data. This is particularly true if the name and the file location of the temporary file are predictable.

The best practice is to create random and unpredictable filenames for temporary storage. One way to do this is with the mktemp utility, as in the following examples.

The XXXXXXXX is replaced by mktemp with random characters to ensure the name of the temporary file cannot be easily predicted and is only known within your program.

 

Command	Usage
TEMP=$(mktemp /tmp/tempfile.XXXXXXXX)	To create a temporary file
TEMPDIR=$(mktemp -d /t

It is often useful to generate random numbers and other random data when performing tasks such as:

Performing security-related tasks
Reinitializing storage devices
Erasing and/or obscuring existing data
Generating meaningless data to be used for tests.
Such random numbers can be generated by using the $RANDOM environment variable, which is derived from the Linux kernel’s built-in random number generator, or by the OpenSSL library function, which uses the FIPS140 (Federal Information Processing Standard) algorithm to generate random numbers for encryption

To learn about FIPS140, read Wikipedia's "FIPS 140-2" article.

The example shows you how to easily use the environmental variable method to generate random numbers.

 

Some servers have hardware random number generators that take as input different types of noise signals, such as thermal noise and photoelectric effect. A transducer converts this noise into an electric signal, which is again converted into a digital number by an A-D converter. This number is considered random. However, most common computers do not contain such specialized hardware and, instead, rely on events created during booting to create the raw data needed.

Regardless of which of these two sources is used, the system maintains a so-called entropy pool of these digital numbers/random bits. Random numbers are created from this entropy pool.

The Linux kernel offers the /dev/random and /dev/urandom device nodes, which draw on the entropy pool to provide random numbers which are drawn from the estimated number of bits of noise in the entropy pool.

/dev/random is used where very high quality randomness is required, such as one-time pad or key generation, but it is relatively slow to provide values. /dev/urandom is faster and suitable (good enough) for most cryptographic purposes.

Furthermore, when the entropy pool is empty, /dev/random is blocked and does not generate any number until additional environmental noise (network traffic, mouse movement, etc.) is gathered, whereas /dev/urandom reuses the internal pool to produce more pseudo-random bits.

 CUPS carries out the printing process with the help of its various components:

Configuration Files
Scheduler
Job Files
Log Files
Filter
Printer Drivers
Backend.
You will learn about each of these components on the next few pages.

he print scheduler reads server settings from several configuration files, the two most important of which are cupsd.conf and printers.conf. These and all other CUPS related configuration files are stored under the /etc/cups/ directory.

cupsd.conf is where most system-wide settings are located; it does not contain any printer-specific details. Most of the settings available in this file relate to network security, i.e. which systems can access CUPS network capabilities, how printers are advertised on the local network, what management features are offered, and so on.

printers.conf is where you will find the printer-specific settings. For every printer connected to the system, a corresponding section describes the printer’s status and capabilities. This file is generated or modified only after adding a printer to the system, and should not be modified by hand.

You can view the full list of configuration files by typing ls -lF /etc/cups.

CUPS stores print requests as files under the /var/spool/cups directory (these can actually be accessed before a document is sent to a printer). Data files are prefixed with the letter d while control files are prefixed with the letter c.

 After a printer successfully handles a job, data files are automatically removed. These data files belong to what is commonly known as the print queue.
 
 Log files are placed in /var/log/cups and are used by the scheduler to record activities that have taken place. These files include access, error, and page records.

To view what log files exist, type:

$ sudo ls -l /var/log/cups

CUPS uses filters to convert job file formats to printable formats. Printer drivers contain descriptions for currently connected and configured printers, and are usually stored under /etc/cups/ppd/. The print data is then sent to the printer through a filter, and via a backend that helps to locate devices connected to the system.

So, in short, when you execute a print command, the scheduler validates the command and processes the print job, creating job files according to the settings specified in the configuration files. Simultaneously, the scheduler records activities in the log files. Job files are processed with the help of the filter, printer driver, and backend, and then sent to the printer.

Assuming CUPS has been installed you'll need to start and manage the CUPS daemon so that CUPS is ready for configuring a printer. Managing the CUPS daemon is simple; all management features can be done with the systemctl utility:

$ systemctl status cups

$ sudo systemctl [enable|disable] cups

$ sudo systemctl [start|stop|restart] cups

Note: The next screen demonstrates this on Ubuntu, but is the same for all major current Linux distributions. 


A fact that few people know is that CUPS also comes with its own web server, which makes a configuration interface available via a set of CGI scripts.

This web interface allows you to:

Add and remove local/remote printers
Configure printers:
– Local/remote printers
– Share a printer as a CUPS server
Control print jobs:
– Monitor jobs
– Show completed or pending jobs
– Cancel or move jobs.
The CUPS web interface is available on your browser at: http://localhost:631.

Some pages require a username and password to perform certain actions, for example to add a printer. For most Linux distributions, you must use the root password to add, modify, or delete printers or classes.

CUPS provides two command-line interfaces, descended from the System V and BSD flavors of UNIX. This means that you can use either lp (System V) or lpr (BSD) to print. You can use these commands to print text, PostScript, PDF, and image files.

These commands are useful in cases where printing operations must be automated (from shell scripts, for instance, which contain multiple commands in one file). 

lp is just a command line front-end to the lpr utility that passes input to lpr. Thus, we will discuss only lp in detail. In the example shown here, the task is to print  $HOME/.emacs .

 lp and lpr accept command line options that help you perform all operations that the GUI can accomplish. lp is typically used with a file name as an argument.

Some lp commands and other printing utilities you can use are listed in the table:

 

Command	Usage
lp <filename>	To print the file to default printer
lp -d printer <filename>	To print to a specific printer (useful if multiple printers are available)
program | lp
echo string | lp	To print the output of a program
lp -n number <filename>	To print multiple copies
lpoptions -d printer	To set the default printer
lpq -a	To show the queue status
lpadmin
    
    lpoptions can be used to set printer options and defaults. Each printer has a set of tags associated with it, such as the default number of copies and authentication requirements. You can type lpoptions help to obtain a list of supported options. lpoptions can also be used to set system-wide values, such as the default printer.
    
    You send a file to the shared printer. But when you go there to collect the printout, you discover another user has just started a 200 page job that is not time sensitive. Your file cannot be printed until this print job is complete. What do you do now?

In Linux, command line print job management commands allow you to monitor the job state as well as managing the listing of all printers and checking their status, and canceling or moving print jobs to another printer.

Some of these commands are listed in the table. 

 

Command	Usage
lpstat -p -d	To get a list of available printers, along with their status
lpstat -a	To check the status of all connected printers, including job numbers
cancel job-id
OR
lprm job-id	To cancel a print job
lpmove job-id newprinter	To move a print job to new printer

PostScript is a standard  page description language. It effectively manages scaling of fonts and vector graphics to provide quality printouts. It is purely a text format that contains the data fed to a PostScript interpreter. The format itself is a language that was developed by Adobe in the early 1980s to enable the transfer of data to printers.

Features of PostScript are:

It can be used on any printer that is PostScript-compatible; i.e. any modern printer
Any program that understands the PostScript specification can print to it
Information about page appearance, etc. is embedded in the page.
Postscript has been for the most part superseded by the PDF format (Portable Document Format) which produces far smaller files in a compressed format for which support has been integrated into many applications. However, one still has to deal with postscript documents, often as an intermediate format on the way to producing final documents.

enscript is a tool that is used to convert a text file to PostScript and other formats. It also supports Rich Text Format (RTF) and HyperText Markup Language (HTML). For example, you can convert a text file to two columns (-2) formatted PostScript using the command:

$ enscript -2 -r -p psfile.ps textfile.txt

This command will also rotate (-r) the output to print so the width of the paper is greater than the height (aka landscape mode) thereby reducing the number of pages required for printing.

The commands that can be used with enscript are listed in the table below (for a file called textfile.txt).

 

Command	Usage
enscript -p psfile.ps textfile.txt	Convert a text file to PostScript (saved to psfile.ps)
enscript -n -p psfile.ps textfile.txt	Convert a text file to n columns where n=1-9 (saved in psfile.ps)
enscript textfile.txt	Print a text file directly to the default printer

Most users today are far more accustomed to working with files in PDF format, viewing them easily either on the Internet through their browser or locally on their machine. The PostScript format is still important for various technical reasons that the general user will rarely have to deal with.

From time to time, you may need to convert files from one format to the other, and there are very simple utilities for accomplishing that task. ps2pdf and pdf2ps are part of the ghostscript package installed on or available on all Linux distributions. As an alternative, there are pstopdf and pdftops which are usually part of the poppler package, which may need to be added through your package manager. Unless you are doing a lot of conversions or need some of the fancier options (which you can read about in the man pages for these utilities), it really does not matter which ones you use.

Another possibility is to use the very powerful convert program, which is part of the ImageMagick package. (Some newer distributions have replaced this with Graphics Magick, and the command to use is gm convert).

Some usage examples:

 

Command	Usage
pdf2ps file.pdf	Converts file.pdf to file.ps
ps2pdf file.ps	Converts file.ps to file.pdf
pstopdf input.ps output.pdf	Converts input.ps to output.pdf
pdftops input.pdf output.ps	Converts input.pdf to output.ps
convert input.ps output.pdf	Converts input.ps to output.pdf
convert input.pdf output.ps	Converts input.pdf to output.ps

Linux has many standard programs that can read PDF files, as well as many applications that can easily create them, including all available office suites, such as LibreOffice.

The most common Linux PDF readers are:

evince is available on virtually all distributions and the most widely used program.
okular is based on the older kpdf and available on any distribution that provides the KDE environment.
ghostView is one of the first open source PDF readers and is universally available.
xpdf is one of the oldest open source PDF readers and still has a good user base. 
All of these open source PDF readers support and can read files following the PostScript standard unlike the proprietary Adobe Acrobat Reader, which was once widely used on Linux systems, but, with the growth of these excellent programs, very few Linux users use it today.

At times, you may want to merge, split, or rotate PDF files; not all of these operations can be achieved while using a PDF viewer. Some of these operations include:

Merging/splitting/rotating PDF documents
Repairing corrupted PDF pages
Pulling single pages from a file
Encrypting and decrypting PDF files
Adding, updating, and exporting a PDF’s metadata
Exporting bookmarks to a text file
Filling out PDF forms.
In order to accomplish these tasks there are several programs available:

qpdf
pdftk
ghostscript.
qpdf is widely available on Linux distributions and is very full-featured. pdftk was once very popular but depends on an obsolete unmaintained package (libgcj) and a number of distributions have dropped it; thus we recommend avoiding it. Ghostscript (often invoked using gs) is widely available and well-maintained. However, its usage is a little complex.

You can accomplish a wide variety of tasks using qpdf including:

 

Command	Usage
qpdf --empty --pages 1.pdf 2.pdf -- 12.pdf	Merge the two documents 1.pdf and 2.pdf. The output will be saved to 12.pdf.
qpdf --empty --pages 1.pdf 1-2 -- new.pdf	Write only pages 1 and 2 of 1.pdf. The output will be saved to new.pdf.
qpdf --rotate=+90:1 1.pdf 1r.pdf

qpdf --rotate=+90:1-z 1.pdf 1r-all.pdf

Rotate page 1 of 1.pdf 90 degrees clockwise and save to 1r.pdf

Rotate all pages of 1.pdf 90 degrees clockwise and save to 1r-all.pdf

qpdf --encrypt mypw mypw 128 -- public.pdf private.pdf	Encrypt with 128 bits public.pdf using as the passwd mypw with output as private.pdf
qpdf --decrypt --password=mypw private.pdf file-decrypted.pdf	Decrypt private.pdf with output as file-decrypted.pdf. 

If you’re working with PDF files that contain confidential information and you want to ensure that only certain people can view the PDF file, you can apply a password to it using the user_pw option. One can do this by issuing a command such as:

$ pdftk public.pdf output private.pdf user_pw PROMPT

When you run this command, you will receive a prompt to set the required password, which can have a maximum of 32 characters. A new file, private.pdf, will be created with the identical content as public.pdf, but anyone will need to type the password to be able to view it.

Ghostscript is widely available as an interpreter for the Postscript and PDF languages. The executable program associated with it is abbreviated to gs.

This utility can do most of the operations pdftk can, as well as many others; see man gs for details. Use is somewhat complicated by the rather long nature of the options. For example:

Combine three PDF files into one:
$ gs -dBATCH -dNOPAUSE -q -sDEVICE=pdfwrite  -sOutputFile=all.pdf file1.pdf file2.pdf file3.pdf
Split pages 10 to 20 out of a PDF file:
$ gs -sDEVICE=pdfwrite -dNOPAUSE -dBATCH -dDOPDFMARKS=false -dFirstPage=10 -dLastPage=20\
-sOutputFile=split.pdf file.pdf

ou can use other tools to work with PDF files, such as:

pdfinfo 
It can extract information about PDF files, especially when the files are very large or when a graphical interface is not available.
flpsed 
It can add data to a PostScript document. This tool is specifically useful for filling in forms or adding short comments into the document.
pdfmod 
It is a simple application that provides a graphical interface for modifying PDF documents. Using this tool, you can reorder, rotate, and remove pages; export images from a document; edit the title, subject, and author; add keywords; and combine documents using drag-and-drop action.
For example, to collect the details of a document, you can use the following command:

$ pdfinfo /usr/share/doc/readme.pdf

Check to see if the enscript package has been installed on your system, and if not, install it.
Using enscript, convert the text file /var/dmesg to PostScript format and name the result /tmp/dmesg.ps. As an alternative, you can use any large text file on your system. Make sure you can read the PostScript file (for example with evince) and compare to the original file.
Note: On some systems, such as RHEL7/CentOS7, evince may have problems with the PostScript file, but the PDF file you produce from it will be fine for viewing.
Convert the PostScript document to PDF format, using ps2pdf. Make sure you can read the resulting PDF file. Does it look identical to the PostScript version?
Is there a way you can go straight to the PDF file without producing a PostScript file on the disk along the way?
Using pdfinfo, determine what is the PDF version used to encode the file, the number of pages, the page size, and other metadata about the file. If you do not have pdfinfo you probably need to install the poppler-utils package.
Click the link below to view a solution to the Lab exercise.



Command	Usage
awk '{ print $0 }' /etc/passwd	Print entire file
awk -F: '{ print $1 }' /etc/passwd	Print first field (column) of every line, separated by a space
awk -F: '{ print $1 $7 }' /etc/passwd	Print first and seventh field of every line
    
$cat > geekfile.txt
unix is great os. unix is opensource. unix is free os.
learn operating system.
unix linux which one you choose.
unix is easy to learn.unix is a multiuser os.Learn unix .unix is a powerful.
Sample Commands

Replacing or substituting string : Sed command is mostly used to replace the text in a file. The below simple sed command replaces the word “unix” with “linux” in the file.
$sed 's/unix/linux/' geekfile.txt
Output :



linux is great os. unix is opensource. unix is free os.
learn operating system.
linux linux which one you choose.
linux is easy to learn.unix is a multiuser os.Learn unix .unix is a powerful.
Here the “s” specifies the substitution operation. The “/” are delimiters. The “unix” is the search pattern and the “linux” is the replacement string.

By default, the sed command replaces the first occurrence of the pattern in each line and it won’t replace the second, third…occurrence in the line.

Replacing the nth occurrence of a pattern in a line : Use the /1, /2 etc flags to replace the first, second occurrence of a pattern in a line. The below command replaces the second occurrence of the word “unix” with “linux” in a line.
$sed 's/unix/linux/2' geekfile.txt
Output:

unix is great os. linux is opensource. unix is free os.
learn operating system.
unix linux which one you choose.
unix is easy to learn.linux is a multiuser os.Learn unix .unix is a powerful.
Replacing all the occurrence of the pattern in a line : The substitute flag /g (global replacement) specifies the sed command to replace all the occurrences of the string in the line.
$sed 's/unix/linux/g' geekfile.txt
Output :

linux is great os. linux is opensource. linux is free os.
learn operating system.
linux linux which one you choose.
linux is easy to learn.linux is a multiuser os.Learn linux .linux is a powerful.

Deleting lines from a particular file : SED command can also be used for deleting lines from a particular file. SED command is used for performing deletion operation without even opening the file
Examples:
1. To Delete a particular line say n in this example
Syntax:
$ sed 'nd' filename.txt
Example:
$ sed '5d' filename.txt

Duplicating the replaced line with /p flag : The /p print flag prints the replaced line twice on the terminal. If a line does not have the search pattern and is not replaced, then the /p prints that line only once.
$sed 's/unix/linux/p' geekfile.txt


_Repitative Ausführung von Befehlen über die Historie des Terminals_

history befehl wiederholen mit !26 !"26" klären was das genau bedeutet

_Ausgabe und Manipulation des Datum und der Zeit_

date cal und in diesem Zusammenhang das Format diskutieren

_Standardausgabe und Weiterleitung_[26]

Es existieren drei inpit/output Channels: stdin (0), stdout (1), stderr (2), die eine Kommunikation zwischen den Programmen und der Umgebung etablieren. 
Die Channels sind alle der Datei /dev/tty0 zugeordnet. Folgende Operatoren zur Weiterleitung existieren: 

    Operator | Channel/File
    <           stdin
    >,>>        stdout
    2>          stderr  leitet Fehlermeldungen um.
    |           stdout -> stdin
    2>&1        stderr -> stdin  


Jeder Kanal ist im Grunde eine eigene Textdatei mit Filedesriptor, der an jeder Datei hangt und die Metadaten enthält.

Ein Beispiel für eine Pipe: 
    
    cat /var/log/syslog | less
    
How To Redirect Standard Output/ Error To One File

    ls -l /root/ >ls-error.log 2>&1
    
Alternativ, die direkte Methode: 

    $ ls -l /root/ &>ls-error.log 
    
Oftmals in Kombination mit dem Befehl xargs: 

    $ echo /home/aaronkilik/test/ /home/aaronkilik/tmp | xargs -n 1 cp -v /home/aaronkilik/bin/sys_info.sh

xargs which is used to build and execute command lines from standard input. Below is an example of a pipeline which uses xargs, this command is used to copy a file into multiple directories in Linux:

_Login und SSH_

Der Zugriff auf eine Maschine kann über Lokal oder Über Fernzugriff erfolgen. Entweder erfolgt der Zugriff über Command Line direkt oder über eine CL innerhalb einer GUI (X--Window System). Fernzugriff erfolgt über den Befehl ssh user@maschine (DNS Name oder IP). Erfolgt ein Zugriff auf die Maschine, dann öffnet sich ein Teletypewriter (TTY). Innerhalb dieses TTY wird eine virtuelle Konsole geöffnet. Innerhalb der Konsole wird dann eine Shell aktiviert, mit der auf das System zugriffen werden kann. Über Programme wie Docker werden dann ab und zu Shell in Shell geöffnet. 

Mit dem Befehl who kann man sehen, wer gerade eingelogt ist in das System. Dabei struktiert sich die Ausgabe wie folgt: 

    |USER|TTY|FROM|LGOIN@|IDLE|....
    
Dabei ist USER der Login name des USers, tty steht für die virtuelle Konsolle, wobei TTY steht für lokaes Einloggen und PTY steht für remote login mit SSH. 

    -b wie lange das System schon aktiv ist
    -q Anzahl der User die eingelogt sind
 
 One virtual terminal (usually number one or seven) is reserved for the graphical environment, and text logins are enabled on the unused VTs. Ubuntu uses VT 7, but CentOS/RHEL and openSUSE use VT 1 for the graphical display.

An example of a situation where using VTs is helpful is when you run into problems with the graphical desktop. In this situation, you can switch to one of the text VTs and troubleshoot.

To switch between VTs, press CTRL-ALT-function key for the VT. For example, press CTRL-ALT-F6 for VT 6. Actually, you only have to press the ALT-F6 key combination if you are in a VT and want to switch to another VT.
_CREATE, DELETE, COPY, AND MOVE FILES AND DIRECTORIES_[28] 

touch command will create a zero bytes empty file. To create a directory, we can use the mkdir command, which stands for making a directory. cp – copy command, we can use this command to rename the file while copying it to the new location. There is scp command – secure copy – to copy files or directory between different Linux servers.
Another command is the mv command which we can use to move the file or directory to a new location, similar we can use this command to rename a file to a different name. 
Sometimes when we create a file or directory, we will like to delete it, we can do this with the help of rm command. rmdir is a command we can use to remove an empty directory if that directory contains anything inside the command will fail. We can use rm -r where -r stands for recursive this will remove the directory and its content. The rm command removes files or directory instantly; there is no bin or trash where we can retrieve what we deleted back. rm -rf, where f stands for force, is a very dangerous command because it will remove everything in the current working directory without warring.

_MAN Pages_[29]

an interface to the on-line reference manuals

man -k print | head -n 5
fmtmsg (3)           - print formatted error messages
isprint (3)          - character classification routines
iswprint (3)         - test for printing wide character
netstat (8)          - Print network connections, routing tables, interface statistics, masquerade connections, and multicast memberships
abrt-action-list-dsos (1) - Prints out DSO from mapped memory regions.

whatis
display manual page descriptions

1
2
3
whatis ip
ip (7)               - Linux IPv4 protocol implementation
ip (8)               - show / manipulate routing, devices, pol...

apropos
search the manual page names and descriptions

1
2
3
4
5
6
7
8
9
10
11
apropos network | tail
teamnl (8)           - team network device Netlink interface tool
tracepath (8)        - traces path to a network host discoveri...
tracepath6 (8)       - traces path to a network host discoveri...
traceroute (8)       - print the route packets trace to networ...
traceroute6 (8)      - print the route packets trace to networ...
tshark (1)           - Dump and analyze network traffic
umount.nfs (8)       - unmount a Network File System
usernetctl (8)       - allow a user to manipulate a network in...
wget (1)             - The non-interactive network downloader.
wireshark (1)        - Interactively dump and analyze network ...

fmt
simple optimal text formatter

u # uniform spacing: one space between words, two after sentences
1
2
3
4
[root@tron ~]# cat test.txt
Test.     Next sentence.   This    doesn't   look       right
[root@tron ~]# fmt -u test.txt
Test.  Next sentence.  This doesn't look right
nl
number lines of files

1
2
3
4
5
6
nl test.txt
     1    First line

     2    Second line

     3    Third line

_Wildcards_

Viel man innerhalb einer Textdatei etwas suchen, jedoch man kennt den String nicht mehr genau, dann kann man über Wildcards nach potentiellen Strings suchen, die ählich dem String sind, den man sucht. 
Folgende Wildcards existieren: 

![IMG_8394](https://user-images.githubusercontent.com/15387251/109890380-7af15e80-7c87-11eb-8b30-960f9c798eb9.jpg)
    
_Hard and Soft Links_[27]

 inode (a data structure that stores everything about a file apart from its name and actual content).

$ ls -l
$ ln topprocs.sh tp
$ ls -l

To make a hard link directly into a soft link, use the -P flag like this.

$ ln -P topprocs.sh tp

To make a soft link, look at the next example: 

$ ln -s ~/bin/topprocs.sh topps.sh
$ ls -l topps.sh

If the symbolic link already exist, you may get an error, to force the operation (remove exiting symbolic link), use the -f option.

$ ln -s ~/bin/topprocs.sh topps.sh
$ ln -sf ~/bin/topprocs.sh topps.sh

To enable verbose mode, add the -v flag to prints the name of each linked file in the output.

$ ln -sfv ~/bin/topprocs.sh topps.sh
$ $ls -l topps.sh


_Creating Backups_ [9]

Creating Backups:
dd - Use this tool to take complete back ups of entire partitions or drives. 
dd if=[device you want to backup] of=[backup.img]
IMPORTANT: This backup will be of the entire extent of the device you mention even if there is space on the drive. It will backup the entire extent. 
rsync - use this tool if you need to perform backups over a network. 
rsync -av [source] [destination]
tar - Standard archiving. 
tar cvf tarball.tar /directory/
Some practical code:
dd if=/dev/sda of=/backups/sda.img
rsync -av /root/ /backups/
tar cvfj rootarchive.bz2 /root/
Restoring Backups: Pretty much straightforward in most cases. 
dd - simply reverse the process
rsync - Same idea as dd - Reverse the process. 
tar - Use the extract directive (x)
Some practical code:
dd if=/backups/sda.img /dev/sda
rsync -v /backups/ /root/
tar xvfj rootarchive.bz2
Make sure you are in the directory you want to the data to be unpacked into. 


man -k "keyword" 


Set up a mail server.
The questions were outcome based, i.e. Set up a mail server.

You were free to use the tools you deemed right as assessment was proficiency based.

I don't know how much has changed but practice was key. i.e. I set up various mail servers before 

ing an approach I was confident with.



  apropos - search the manual page names and descriptions
SYNOPSIS         top
       apropos [-dalv?V] [-e|-w|-r] [-s list] [-m system[,...]] [-M
       path] [-L locale] [-C file] keyword ...
DESCRIPTION         top
       Each manual page has a short description available within it.
       apropos searches the descriptions for instances of keyword.

       keyword is usually a regular expression, as if (-r) was used, or
       may contain wildcards (-w), or match the exact keyword (-e).
       Using these options, it may be necessary to quote the keyword or
       escape (\) the special characters to stop the shell from
       interpreting them.

       The standard matching rules allow matches to be made against the
       page name and word boundaries in the description.

       The database searched by apropos is updated by the mandb program.
       Depending on your installation, this may be run by a periodic
       cron job, or may need to be run manually after new manual pages
       have been installed.
OPTIONS         top
       -d, --debug
              Print debugging information.

       -v, --verbose
              Print verbose warning messages.

       -r, --regex
              Interpret each keyword as a regular expression.  This is
              the default behaviour.  Each keyword will be matched
              against the page names and the descriptions independently.
              It can match any part of either.  The match is not limited
              to word boundaries.

       -w, --wildcard
              Interpret each keyword as a pattern containing shell style
              wildcards.  Each keyword will be matched against the page
              names and the descriptions independently.  If --exact is
              also used, a match will only be found if an expanded
              keyword matches an entire description or page name.
              Otherwise the keyword is also allowed to match on word
              boundaries in the description.

       -e, --exact
              Each keyword will be exactly matched against the page
              names and the descriptions.

       -a, --and
              Only display items that match all the supplied keywords.
              The default is to display items that match any keyword.

       -l, --long
              Do not trim output to the terminal width.  Normally,
              output will be truncated to the terminal width to avoid
              ugly results from poorly-written NAME sections.

       -s list, --sections=list, --section=list
              Search only the given manual sections.  list is a colon-
              or comma-separated list of sections.  If an entry in list
              is a simple section, for example "3", then the displayed
              list of descriptions will include pages in sections "3",
              "3perl", "3x", and so on; while if an entry in list has an
              extension, for example "3perl", then the list will only
              include pages in that exact part of the manual section.

       -m system[,...], --systems=system[,...]
              If this system has access to other operating system's
              manual page descriptions, they can be searched using this
              option.  To search NewOS's manual page descriptions, use
              the option -m NewOS.

              The system specified can be a combination of comma-
              delimited operating system names.  To include a search of
              the native operating system's whatis descriptions, include
              the system name man in the argument string.  This option
              will override the $SYSTEM environment variable.

       -M path, --manpath=path
              Specify an alternate set of colon-delimited manual page
              hierarchies to search.  By default, apropos uses the
              $MANPATH environment variable, unless it is empty or
              unset, in which case it will determine an appropriate
              manpath based on your $PATH environment variable.  This
              option overrides the contents of $MANPATH.

       -L locale, --locale=locale
              apropos will normally determine your current locale by a
              call to the C function setlocale(3) which interrogates
              various environment variables, possibly including
              $LC_MESSAGES and $LANG.  To temporarily override the
              determined value, use this option to supply a locale
              string directly to apropos.  Note that it will not take
              effect until the search for pages actually begins.  Output
              such as the help message will always be displayed in the
              initially determined locale.

       -C file, --config-file=file
              Use this user configuration file rather than the default
              of ~/.manpath.

       -?, --help
              Print a help message and exit.

       --usage
              Print a short usage message and exit.

       -V, --version
              Display version information.  
       
    Create a RAID 0 array using the two spare drives on (5GB) on this machine. 
Size=2048MB
Label=RAID_0
Mount it persistently by Label at /storage
Create a RAID 1 array using LVM using the two spare drives (5GB) on this machine
size 1024MB
Label=RAID_1
Mount it persistently by UUID at /storage2
Using the left over space on those one of those two drives, create a 1GB SWAP partition and add it to the existing SWAP pool - This SWAP space should mount at boot. 
Assume that there is an NFS share somewhere on your network. Write the command you would use to mount the NFS share on /NFS_mount. Append your terminal code to a file named “network_mount.txt” and place it in the /root folder.
The IP or DNS name for the share do not matter - Use whatever you would like. 
Assume that there is an CIFS share somewhere on your network. Write the command you would use to mount the CIFS share on /CIFS_mount. Append your terminal code to a file named “network_mount.txt” and place it in the /root folder.
The IP or DNS name for the share do not matter - Use whatever you would like.

Create a bash script named “Infinite_loop” that does the following:
Continually writes the string "Linux is fun" to the folder /dev/null
Has no exit loop (that is, it will continue to run until told to stop
Place the script in /root
Give bob permissions to run infinite_loop
Switch user to Bob and have him run infinite_loop
Switch back to root
As root, locate the now running infinite_loop and forcefully kill it by PID
Create a backup of the /etc/yum.repos.d folder using tar or rsync; place your backup in /root
Create a local repo using a live dvd iso of Centos
Using your local repo, install the appropriate packages needed for virtualization.

As always, consult man pages for more information. 
