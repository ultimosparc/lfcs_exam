# Essential_Commands

Essential Commands domain might appears more frequently than others in the exam. 

_Archivieren von Dateien_ [9]

Dateien können in Archiven verpackt werden. Das Tool dafür ist _tar_. 
Ein möglicher Befehl zum Generieren eines Archives könnte wie folgt aussehen: 

    tar -cvf test.tar datei1 datei2

Dieser Befehl generiert ein Archiv "test.tar" (-c) aus den Dateien "datei1" und "datei2". Dabei wird im Terminal ausgegeben, welche Dateien ins Archiv gepackt werden (-v). 
Das Entpacken eines Archivs kann wie folgt realisiert werden: 

    tar -xvf test.tar (Size: 10 KB)
    
Die Option -x steht für Entpacken. Es gibt zusätzlich die Möglichkeit, ein Archiv zu komprimieren. Unteranderem gibt es folgende Möglichkeiten der Kompromierung:

    -z gzip (Size: 131 B)
    -j bzip2/bz (Size: 140 B)
    -J xz (Size: 180 B
    
Wie im Beispiel beobachtet werden kann, das Archiv kann bis um das 10fache verkleinert werden. Damit wird klar, jedes Archiv sollte komprimiert werden. Gewöhnlich benutzt man gzip, um ein tar Archiv zu komprimieren. 
Weitere Ooptionen: 

    -t Auflisten des Inhaltes des Archives
    -r Erweitern des Archives
    -d Löschen einzlner Dateien
    -u Updaten von einzlnen Dateien 
 
 
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
Es existieren weitere Befehhhhhhhhle nach Dateien zu suchen. 

    which – returns the location of the command base on the PATH settings
    whereis – returns the location of the binary, source file, and man page, it can return multiple versions of command if they exist.
    type – returns information about the command type and details based on how the command is related to the shell configuration.
    locate sucht wie find nach Datei, jedoch über eine interne Datenbank
 


man -k "keyword" 






    Searching files (using find, a lot of it in different levels)
    Compare and manipulate file content (using diff for example)
    Input-output redirection (>,>>,<<,<,|,2&1 etc.)
    Create, manage hard and soft links
    List, set, and change standard file permissions (including SUID/SGID, sticky bit)
    Create, delete, copy, and move files and directories
    Using all other common console utilities
    Read and use system documentation (man, apropos commands are your friends during exam)
