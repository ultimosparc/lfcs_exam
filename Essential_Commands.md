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
 
 
_Suchen nach Dateien_ [9]




    Searching files (using find, a lot of it in different levels)
    Compare and manipulate file content (using diff for example)
    Input-output redirection (>,>>,<<,<,|,2&1 etc.)
    Create, manage hard and soft links
    List, set, and change standard file permissions (including SUID/SGID, sticky bit)
    Create, delete, copy, and move files and directories
    Using all other common console utilities
    Read and use system documentation (man, apropos commands are your friends during exam)
