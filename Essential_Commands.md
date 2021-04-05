# Essential_Commands

Essential Commands domain might appears more frequently than others in the exam. 

_Generieren, Lesen, Kopieren, Verschieben, Archivieren, Komprimieren, Backup und Lschen von Dateien_[9,25]

Dateien können in Archiven verpackt werden. Das Tool dafür ist _tar_. Beispiel:  

    tar cvf test.tar datei1 datei2 verzeichnis1

Dieser Befehl generiert ein Archiv "test.tar" (-c) aus den Dateien "datei1" und "datei2" und dem Verzeichnis "verzeichnis1". Dabei wird im Terminal ausgegeben, welche Dateien ins Archiv gepackt werden (-v). Beispiel für das Entpacken: 

    tar xvf test.tar (Size: 10 KB)
    
Die Option -x steht für Entpacken. Es gibt zusätzlich die Möglichkeit, ein Archiv zu komprimieren. Es gibt folgende Möglichkeiten der Kompromierung:

    -z gzip (Size: 131 B)
    -j bzip2/bz (Size: 140 B)
    -J xz (Size: 180 B)
    
Wie im Beispiel beobachtet werden kann, das Archi
v kann bis um das 10fache verkleinert werden. Damit wird klar, jedes Archiv sollte komprimiert werden. Gewöhnlich benutzt man gzip, um ein tar Archiv zu komprimieren. 
Weitere Ooptionen: 

    -t Auflisten des Inhaltes des Archives
    -r man fügt Dateien zu einem Archiv
    -d Löschen einzlner Dateien
    -u Austauschen von einzlnen Dateien zw Update
    -p man archiviert Dateien und den Rechten auf den Dateien
    -f muss gesetzt werden, damit Grundbefehle we c,d ausgeführt werden kann
 
Mit den obigen Beispielen kann man ein einfaches Backup erstellen. Will man nun ein Backup für eine Partition/Drives anlegen, dann kann man den Befehl _dd_ verwenden. 

    dd if=[device you want to backup] of=[backup.img]  --> dd if=/dev/sda of=/backups/sda.img  --> dd if=/backups/sda.img /dev/sda

Ein weiteres Tool für Backing Up wäre _rsync_, mit dem auch Backups über das Netzwerk erstellt werden. 

    rsync -av [source] [destination]  --> rsync -av /root/ /backups/  --> rsync -v /backups/ /root/
 
 
Mit dem Befehl _FILE_ können Metadaten einer Datei ausgegben werden. Zum Auslesen einer Datei gibt es mehrere Befehle wie cat, less, more, head, tail. 

Die Befehle _HEAD/TAIL_ geben jeweils die ersten/letzten 10 Zeilen einer Datei aus. Man kann auch die Ausgabe anpassen. Beispiel: 

    head -n 5 testfile
    
gibt die ersten 5 Zeilen der testfile wieder. 
Es gib verschiedene Befehle eine Textdatei zu inhaltlich zu manipulieren. Der einfachste Befehl ist _ECHO_, der Strings in eine Textdatei schreibt. Ein weitere Befehl wäre the cut Funktion. 
The cut command in UNIX is a command for cutting out the sections from each line of files and writing the result to standard output. Basically the cut command slices a line and extracts the text. It is necessary to specify option with command otherwise it gives error. If more than one file name is provided then data from each file is not precedes by its file name. Let us consider two files having name state.txt and capital.txt contains 5 names of the Indian states and capitals respectively.

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

Folgende Optionen stehen unteranderem zur Verfügung: 

     -c (column): $cut -c [(k)-(n)/(k),(n)/(n)] filename

Here,k denotes the starting position of the character and n denotes the ending position of the character in each line, if k and n are separated by “-” otherwise they are only the position of character in each line from the file taken as an input. Beispiel: 

    $ cut -c 2,5,7 state.txt
    nr
    rah
    sm
    ir
    hti
     
    -f (field): $cut -d "delimiter" -f (field number) file.txt
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

Der Befehl _SORT_ kann den Inhalt einer Datei sortieren command is used to sort a file, arranging the records in a particular order. By default, the sort command sorts file assuming the contents are ASCII.

    $ sort file.txt
    abhishek
    chitransh
    divyam
    harsh
    naveen 
    rajan
    satish
    
Weitere Optionen sind:

    -u sortiert und löscht doppelte Einträge
    -n numerisch sortieren
    -k N sortiert die Spalte N 
    -o exportiert Ergbnis in eine neue Datei
    -r zngekehrte Sortierung
    -c checkt ob datei sortiert ist
    
The _UNIQ_ command in Linux is a command line utility that reports or filters out the repeated lines in a file.

    $uniq [OPTION] [INPUT[OUTPUT]]

    $cat kt.txt
    I love music.
    I love music.
    I love music.

    $uniq kt.txt
    I love music.

Weitere Optionen sind: 

    -c zählt die Lines, die mehrfach vorkammen
    -d gibt die Lines aus, die mehrfach vorkammen
    -f N It allows you to skip N fields(a field is a group of characters, delimited by whitespace) of a line before determining uniqueness of a line.
    -i By default, comparisons done are case sensitive but with this option case insensitive comparisons can be made.
    -s N It doesn’t compares the first N characters of each line while determining uniqueness. 
    -u It allows you to print only unique lines.
    -z It will make a line end with 0 byte(NULL), instead of a newline.
    -w N It only compares N characters in a line.
    – – help It displays a help message and exit.
    – – version It displays version information and exit.

Man Dateien miteinander vergleichen über den Befehl _DIFF_ beispielsweise. Aufbau: 

    diff PARAMETER FILES
    
The important thing to remember is that diff uses certain special symbols and instructions that are required to make two files identical. It tells you the instructions on how to change the first file to make it match the second file.
Special symbols are: a : add c : change d : delete

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
     
Like in our case, 0a1 which means after lines 0(at the very beginning of file) you have to add Tamil Nadu to match the second file line number 1. It then tells us what those lines are in each file preceeded by the symbol: Lines preceded by a < are lines from the first file. Lines preceded by > are lines from the second file.
Next line contains 2,3c3 which means from line 2 to line 3 in the first file needs to be changed to match line number 3 in the second file. It then tells us those lines with the above symbols. The three dashes (“—“) merely separate the lines of file 1 and file 2.

Ein weiterer Kommando ist der _SED_ command in UNIX is stands for stream editor and it can perform lot’s of function on file like, searching, find and replace, insertion or deletion. sed OPTIONS... [SCRIPT] [INPUTFILE...] Z.B. sed -e command <filename> spezifiziert on file and put the output on standard out (e.g. the terminal)
sed -f scriptfile <filename>	Specify a scriptfile containing sed commands, operate on file and put output on standard out. Andere Beispiele: 
    
    sed s/pattern/replace_string/ file	Substitute first string occurrence in every line
    sed s/pattern/replace_string/g file	Substitute all string occurrences in every line
    sed 1,3s/pattern/replace_string/g file	Substitute all string occurrences in a range of lines
    sed -i s/pattern/replace_string/g file	Save changes for string substitution in the same file
    $ sed s/pattern/replace_string/g file1 > file2

The above command will replace all occurrences of pattern with replace_string in file1 and move the contents to file2. The contents of file2 can be viewed with cat file2. If you approve you can then overwrite the original file with mv file2 file1.
Das Kommando _AWK_ to extract and then print specific contents of a file and is often used to construct reports. awk has the following features:

    It is a powerful utility and interpreted programming language.
    It is used to manipulate data files, retrieving, and processing text.
    It works well with fields (containing a single piece of data, essentially a column) and records (a collection of fields, essentially a line in a file).

Ein weiteres Kommando ist der _SPLIT_ Befehl, der den Inhalt in verschiedene Datei teilt, ohne die Ursprungsdatei zu löschen oder zu manipulieren.  By default, split breaks up a file into 1000-line segments. 
Ein anderes Kommando ist _TR_, der den Inhalt der Datei übersetzt oder löscht. 

    $ tr [options] set1 [set2]

Beispiele:

    tr abcdefghijklmnopqrstuvwxyz ABCDEFGHIJKLMNOPQRSTUVWXYZ	Convert lower case to upper case
    tr '{}' '()' < inputfile > outputfile	Translate braces into parenthesis
    echo "This is for testing" | tr [:space:] '\t'	Translate white-space to tabs
    echo "This   is   for    testing" | tr -s [:space:]
    Squeeze repetition of characters using -s
    echo "the geek stuff" | tr -d 't'	Delete specified characters using -d option
    echo "my username is 432234" | tr -cd [:digit:]	Complement the sets using -c option
    tr -cd [:print:] < file.txt	Remove all non-printable character from a file
    tr -s '\n' ' ' < file.txt	Join all the lines in a file into a single line

Nehmen den Befehlen cat, more, read für die Ausgabe von Testdatei gibt es noch die Befehle, die einen Ausdruck einer Datei erstellen. 
Ausdrucke für Dateien werden in PDF oder Postscript angegeben, PDF ist kleiner als PostScript, jedoch PostScript bietet Eigenschaften für die längere Aufbewahrung. 
Beispiele: 

    enscript -2 -r -p psfile.ps textfile.txt
    enscript -p psfile.ps textfile.txt	Convert a text file to PostScript (saved to psfile.ps)
    enscript -n -p psfile.ps textfile.txt	Convert a text file to n columns where n=1-9 (saved in psfile.ps)
    enscript textfile.txt	Print a text file directly to the default printer

Manchmal muss zwischen PostScript und PDF convertiert werden. Dafür gibt es die Befehle ps2pdf and pdf2ps are part of the ghostscript package installed on or available on all Linux distributions. As an alternative, there are pstopdf and pdftops which are usually part of the poppler package, which may need to be added through your package manager. Ein anderes Tool ist ImageMagick package. 

    pdf2ps file.pdf	Converts file.pdf to file.ps
    ps2pdf file.ps	Converts file.ps to file.pdf
    pstopdf input.ps output.pdf	Converts input.ps to output.pdf
    pdftops input.pdf output.ps	Converts input.pdf to output.ps
    convert input.ps output.pdf	Converts input.ps to output.pdf
    convert input.pdf output.ps	Converts input.pdf to output.ps

Terminalbasierte Programme für PDF sind 

    qpdf
    pdftk
    ghostscript

Beispiele: 

    qpdf --empty --pages 1.pdf 2.pdf -- 12.pdf	Merge the two documents 1.pdf and 2.pdf. The output will be saved to 12.pdf.
    qpdf --empty --pages 1.pdf 1-2 -- new.pdf	Write only pages 1 and 2 of 1.pdf. The output will be saved to new.pdf.
    qpdf --rotate=+90:1 1.pdf 1r.pdf
    qpdf --rotate=+90:1-z 1.pdf 1r-all.pdf
    Rotate page 1 of 1.pdf 90 degrees clockwise and save to 1r.pdf
    Rotate all pages of 1.pdf 90 degrees clockwise and save to 1r-all.pdf
    qpdf --encrypt mypw mypw 128 -- public.pdf private.pdf	Encrypt with 128 bits public.pdf using as the passwd mypw with output as private.pdf
    qpdf --decrypt --password=mypw private.pdf file-decrypted.pdf	Decrypt private.pdf with output as file-decrypted.pdf. 
    pdftk public.pdf output private.pdf user_pw PROMPT
    Combine three PDF files into one:
    $ gs -dBATCH -dNOPAUSE -q -sDEVICE=pdfwrite  -sOutputFile=all.pdf file1.pdf file2.pdf file3.pdf
    Split pages 10 to 20 out of a PDF file:
    $ gs -sDEVICE=pdfwrite -dNOPAUSE -dBATCH -dDOPDFMARKS=false -dFirstPage=10 -dLastPage=20\
    -sOutputFile=split.pdf file.pdf

_Repitative Ausführung von Befehlen über die Historie des Terminals_

history befehl wiederholen mit !26 !"26" klären was das genau bedeutet

_Ausgabe und Manipulation des Datum und der Zeit_

date cal und in diesem Zusammenhang das Format diskutieren
 
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


_MAN Pages_[29]

man -k "keyword" 

man -k print | head -n 5
fmtmsg (3)           - print formatted error messages
isprint (3)          - character classification routines
iswprint (3)         - test for printing wide character
netstat (8)          - Print network connections, routing tables, interface statistics, masquerade connections, and multicast memberships
abrt-action-list-dsos (1) - Prints out DSO from mapped memory regions.

whatis
display manual page descriptions

whatis ip
ip (7)               - Linux IPv4 protocol implementation
ip (8)               - show / manipulate routing, devices, pol...

apropos
 apropos - search the manual page names and descriptions
SYNOPSIS         top
       apropos [-dalv?V] [-e|-w|-r] [-s list] [-m system[,...]] [-M
       path] [-L locale] [-C file] keyword ...

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
