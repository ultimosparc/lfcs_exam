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
 
_Bearbeitung von Dateien_

Mit dem Befehl file kann man Information über den Typ der Datei bekommen. Zum Auslesen einer Datei gibt es mehrere Befehle wie cat, less, more, head, tail. 

wc,  

_Repitative Ausführung von Befehlen über die Historie des Terminals_

history befehl wiederholen mit !26 !"26" klären was das genau bedeutet

_Ausgabe und Manipulation des Datum und der Zeit_

date cal und in diesem Zusammenhang das Format diskutieren

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

I don't know how much has changed but practice was key. i.e. I set up various mail servers before finding an approach I was confident with.



    
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
