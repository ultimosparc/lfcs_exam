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

Es gibt verschiedene Befehle zum Suchen von Textdateien. Meist wird der Befehl _find_ genutzt. It can be used to search by name, file type, timestamp, owner, and many other attributes. Ein möglicher Befehl kann wie folgt aussehen:

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
    type – returns information about the command type and details based on how the command is related to the shell configuration.
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

_Standardausgabe und Weiterleitung_

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
    
_Wildcards_

Viel man innerhalb einer Textdatei etwas suchen, jedoch man kennt den String nicht mehr genau, dann kann man über Wildcards nach potentiellen Strings suchen, die ählich dem String sind, den man sucht. 
Folgende Wildcards existieren: 

![IMG_8394](https://user-images.githubusercontent.com/15387251/109890380-7af15e80-7c87-11eb-8b30-960f9c798eb9.jpg)
    

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



    
    Compare and manipulate file content (using diff for example)
    Input-output redirection (>,>>,<<,<,|,2&1 etc.)
    Create, manage hard and soft links
    List, set, and change standard file permissions (including SUID/SGID, sticky bit)
    Create, delete, copy, and move files and directories
    Using all other common console utilities
    Read and use system documentation (man, apropos commands are your friends during exam)
    
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
