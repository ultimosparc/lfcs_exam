# Test Exam
According to the exercises from the sources, I have created a serie of tasks for every topic. (Task (T))

Die Aufgaben bilden eine Grundlage, um bestimmte Teilaufgaben von Examsaufgaben zu idenzifizieren und zu lösen.  

## 1. Essential Commands

T: _Ersetzen Sie das Wort "Haus" mit dem Wort "Eigenheim" überall in der Textdatei "Immobilien.txt" (Substitution)._
    
T: _Ersetzen Sie in Zeile 200 den Preis von 100.000€ nach 150.000€ in der Textdatei "Immobilien.txt" (Substitution)._
       
T: _Löschen Sie in der Textdatei "Immobilien.txt" die Zeile 202._

T: _Erstelle ein komprimiertes Archiv "test.tar" der Dateien "datei1" und "datei2" und lösche die beiden Dateien "datei1" und "datei2"._

T: _Erstellen Sie einen neuen SSH Schlüssel und sperren Sie den alten Schlüssel._

## 2. User and Group Management

T: _Erstellen Sie einen neuen User "Peter" und geben Sie Ihm "Superuser"-Rechte._

T: _Erstellen Sie eine neue Gruppe "Verkäufer" und fügen Sie den User Peter hinzu_

T: _Erstellen Sie einen Unterordner "Haus" und ändern Sie die Gruppe zu "Verkäufer" und den Besitzer zu root_

T: _Ändern Sie die SU Rechte von Peter, sodass nur nur die Programme cat, tail ohne sudo ausgeführt werden kann_

## 3. Operation of Running Systems

T: _Erstellen Sie einen Softlink und Hardlink auf die Datei "test"._

T: _Geben Sie dem Prozess "Sleep" eine höhre Priorität, damit dieser schneller ausgeführt wird_

T: _Legen Sie einen Chronjob an, der jede Minute das Skript /tmp/test.sh ausführt_

## 4. Service Configuration

## 5. Networking

Kombinieren von Netzwerk speichern und den Zugriff darauf
Install the Apache Package
Change the default directory to /web/html
Create a file named index.html and place it in /web/html
Make sure that anyone on your local subnet can access index.html but no one else.

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
hostn
Mögliche Aufgaben: 
Mögliche Aufgaben wären, den statischen pretty icon, deployment umgebung(prodcution, staging developing usw zu geben), standar wert bei "" eingabe setzen, chassis types wofür werden diese verwendet, 
Wo wird der statische Hostname verwendet, der direkt aus dem Kernel kommt. Man könnte ändern der bootlist im bootmenu, benutzt diese List den statischen hostname
hostnamectl may be used to query and change the system hostname and related settings.

Upon reboot, manually configure grub to boot into runlevel 3
Upon booting, set the machine to permanently boot into runlevel 3
Change the hostname to CENT6
https://phoenixnap.com/kb/how-to-set-or-change-a-hostname-in-centos-7

## 6. Storage Management

T: _Erstellen Sie ein Backup des Heimverzeichnis von User "Peter" und der ganzen Partition/Hard Drive._

S: Folgende Lösungen sind möglich, entweder über sudo oder root ausführen 

    1. Backup des Heimverzeichnis von "Peter"
        1. mkdir /backups (falls noch nicht verhanden)
        2. tar cvfz /backups/user_backup_peter.tar /home/Peter
    
    2. Backup der Partition/Hard Drive
        
 T     Exam's preparing &amp; test exam

S: Prüfen Sie, ob das System eine SWAP Partition hat. Falls nicht, dann erstellen Sie eine SWAP Partition. Größe von RAM ist 1.5 GB. Vergewissern Sie sich, das die SWAP auch nach dem Neustart noch exisitert und aktiviert ist. 

T: Prüfen Sie, ob das System eine SWAP Partition hat. Falls nicht, dann erstellen Sie eine SWAP Partition. Größe von RAM ist 1.5 GB. Vergewissern Sie sich, das die SWAP auch nach dem Neustart noch exisitert und aktiviert ist.

T: Angenommen das System hat zwei SWAP Partition S1 und S2. Prioierensieren die S2, damit Daten immer erst in S2 ausgelagert werden. --> Fstab

T: 


## 7. Solutions & Remarks

Task (T), Solution (S)

T: _Ersetzen Sie das Wort "Haus" mit dem Wort "Eigenheim" überall in der Textdatei "Immobilien.txt" (Substitution)._

S: Es gibt hier zwei mögliche Lösungswege. 

    1. File-Streaming
      
      sed 's/Haus/Eigenheim/g' Immobilien.txt > Immobilien.txt 
        
        Der Befehl packt eine Kopie der Datei "Immobilien.txt" in den Stream und ändert überall das Wort "Haus". 
        Das Ergebnis wird angezeigt, jedoch erst nach Weiterleitung auf sich selbst
        werden die Änderung permant übernommen. 
      
    2. VI-Editor
      
      1. vi Immobilien.txt
      2. : %s/Haus/Eigenheim/g
      3. :wq
      
        In der zweiten Lösung wird ebenfalls eine Substitution durchgeführt, 
        jedoch bietet dieser Lösungsweg den Vorteil, man kann direkt im vi
        Editor sehen, ob die Änderungen erfolgreich ungesetzt wurden. 
      
T: _Ersetzen Sie in Zeile 200 den Preis von 100.000€ nach 150.000€ in der Textdatei "Immobilien.txt" (Substitution)._

S: Mögliche Lösung wäre:
  
    1. vi Immoblien.txt
    2. :set number
    3. :200
    4. i (insert-Mode)
    5. Die entsprechende Stelle ersetzen
    6 :wq
    
       Das Ersetzen von Textstellen in einer Textdatei wird gerne als eine Teilaufgabe 
       verwendet im Zusammenhang mit dem Ersetzen von User-Eigenschaften, z.B. für das Passwort-Datei. 
       
T: _Löschen Sie in der Textdatei "Immobilien.txt" die Zeile 202._

S: Mögliche Lösung wäre:
    
    1. vi Immoblien.txt
    2. :set number
    3. :202
    5. :d 
    6. :wq
    
       Im Vi-Editor gibt es verschiedene Wege eine Zeile zu löschen, jedoch der "delete"-Befehl 
       ist simpel und schnell auszuführen, zusätzlich kann man das Ergebniss gleich im vi-Editor betrachten. 
 
T: _Erstelle ein komprimiertes Archiv "test.tar" der Dateien "datei1" und "datei2" und löschen Sie die beiden Dateien "datei1" und "datei2"._

S: Mögliche Lösung wäre:
    
    tar -cvzf vi test.tar datei1 datei2; rm -rf datei1 datei2;
    
       Um fas Archiv wieder zu extrahieren, muss einfach nur das "c" mit dem "x" ausgetauscht werden,
       "tar -xvf". Falls bei der Archivierung noch die Option p mitgegeben wird, werden die Rechte mit archiviert.
       Um zu prüfen, ob "test.tar" wirklich komprimiert ist, einfach "file test.tar" ausführen. 
       
T: _Erstellen Sie einen neuen User "Peter" und geben Sie Ihm "Superuser"-Rechte._

S: Mögliche Lösung wäre:
    
    1. sudo useradd -m -s /bin/bash Peter;
    2. sudo visudo
    3. Peter ALL=(ALL:ALL) ALL
    4. :wq
    
       Visudo ist ein spezielles Programm, das den vi-Editor benutzt, um die Datei 
       zu öffnen, wo die Superuser-Rechte liegen.
       In dieser Datei kann auch notiert werden, welche Gruppen Superuser-Rechte bekommen sollen. 
       Damit können mehrere User gleichzeitig mit Superuser-Rechten ausgestattet werden.
       Standardmäßig existiert bereits eine solche Gruppe, genannt "wheel".

T: _Erstellen Sie eine neue Gruppe "Verkäufer" und fügen Sie den User Peter hinzu_

S: Mögliche Lösung wäre:
    
    1. groupadd Verkäufer
    2. sudo usermod -aG Verkäufer Peter
        
       Wenn die Optionen -aG nicht verwendet werden, dann wird der User gelöscht von allen Gruppen, 
       die nicht innerhalb der -G Liste stehen. 

T: _Erstellen Sie einen Unterordner "Haus" in /home/Peter und ändern Sie die Gruppe zu "Verkäufer" und den Besitzer zu root_

S: Mögliche Lösung wäre:
    
    1. cd ~
    2. mkdir -p Haus
    3. sudo chown root:Verkäufer Haus
    
       Die Option -p vergewissert, das auch die Parentordner existieren, falls nicht, dann werden diese angelegt. 
      -R setzt den Change rekursiv auf die Unterordner um. 
       
T: _Erstellen Sie einen Softlink, genannt "symlink", und Hardlink, genannt "hardlink", auf die Datei "test"._

S: Mögliche Lösung wäre:
    
    ln -s test symlink; ln test hardlink
    
       Der Unterschied zwischen einen Hardlink und einem Symlink (auch Softlink genannt) besteht darin,
       das Hardlinks und Ursprungsdatei teilen sich die gleiche inode. Bei Softlinks ist das nicht der 
       Fall. 
       
T: _Geben Sie dem Prozess "Sleep" eine höhre Priorität, damit dieser schneller ausgeführt wird_

S: Mögliche Lösung wäre:
    
    1. top -u username --> PID bestimmen
    2. renice '-20' -p 'PID'
    
        Wir der erste Befehl "top -u username" nochmal ausgeführt, dann
        muss sich der NI-Wert des Prozess "Sleep" geändert haben.
        
T: _Legen Sie einen Chronjob an, der jede Minute das Skript /tmp/test.sh ausführt_

S: Mögliche Lösung wäre:
    
    1. chrontab -e
    2. */1 * * * * /tmp/test.sh
    3. chrontab -l
    
       Ein Zeiteintrag kann mit Hilfe der Tabelle "more /etc/crontab" definiert werden (nur unter CentOS).

T: _Konfigurieren Sie die Ip-Adresse der virtuellen Maschine, folgende Werte sind zu setzen: IP 192.168.0.10, Gateway 192.168.0.254, server1.example.com_

S: Mögliche Lösung wäre:
    
    1. ip a s   //Zeigt die aktuelle netzwerk-interne IP Adresse unter eth0 an
    2. nmtui    //Terminal, grafisches Programm, um Netzwerkeinstellungen schnell und easy zu machen.
    3. nmtui    //Neue Einstellungen aktivieren
    4. sudo nmtui    //Hostname einstellen
    5. reboot   //Prüfen ob die Einstellungen gemacht sind mit ip, hostnamectl usw. 
    
        nmtui ist ein terminal, grafische Programm, das verwendet werden kann, um die Änderungen
        schnell und einfach zu machen. Falls vorhanden, auf denen Fall verwenden. 
    
T: _Erstellen Sie eine neue 500 MB Partition unter /mnt/new mit einem 500MB ext4 Datei-Struktur_

S: Mögliche Lösung wäre:
    
    1. lsblk                    //Suche wo genug Platz ist, angenommen sdb
    2. fdisk  /dev/sdb          //einfach den Anweisungen im Programm verfolgen
    3. partprobe /dev/sdb       //Befehl prüft, ob alles Problemlos gelaufen ist
    4. lsblk                    //Neue Parittion sollte sichtbar sein
    5. mkfs.ext4 /dev/sdb1      //Partition erhält FS
    6. mkdir /mnt/new           //Einhängepunkt generieren, falls noch nicht vorhanden
    7. mount /dev/sdb1 /mnt/new //Partition einhängen in das primäre FS
    8. df -h                    //Prüfen, ob das Einhängen funktioniert hat
    9. blkid /dev/sdb1          //UUID bestimmen
    10. vi /etc/fstab           //Änderung der Partitionstabelle permanent machen
    
        Für den 10 Befehl sollte der Eintrag folgendes Format haben: UUID Mountpunt FS Settings 0 0
    
T: _Kopieren Sie die Datei /etc/fstab nach /var/tmp/fstab und machen Sie folgende Änderungen: Ändern Sie den Besitzer in root, ändern Sie die gruppe in root, es nicht für alle (other) ausführbar, User Andrew kann die Datei lesen und schreiben, Susan aber nicht, alle anderen User können die Datei lesen jetzt und in Zukunft_

S: Mögliche Lösung wäre:
    
    1. cp /etc/fstab /var/tmp/fstab
    2. ls -ld
    3. chmod a-x /var/tmp/fstab
    4. setfacl -m u:andrew:rw /var/tmp/fstab
    5. setfacl -m u:susan:x /var/tmp/fstab
    
T: _Geben Sie dem User Andrew folgende Rechte auf die Datei /opt/backup: read, write, execute_

S: Mögliche Lösung wäre:
    
    1. getfacl /opt/backup //Schauen welche Rechte man hat
    2. setfacl -m u:andrew:rwx /opt/backup
    3. getfacl /opt/backup
    
T: _Generieren Sie folgende Users, Gruppen und Mitgliedschaften: Gruppe sysusers, User Andrew mit Mitglied in sysusers, Susan mit Mitglied sysusers, Brad mit keiner Shell zugriff, brad susan andrew haben Passwort passwort_

S: Mögliche Lösung wäre:
    
    1. groupadd sysusers
    2. useradd andrew -G sysusers
    3. passwod andrew 
    4  useradd susan -G sysusers
    5. passwod susan
    6. useradd brad -s/sbin/nologin
    7. passwd brad
    8. cat /etc/passwd //prüfen ob alles geklappt hat
    9. grep sysusers /etc/group 
    
 T: _Generieren Sie einen geteilten Ordner für die Mitglieder des Teams. Folgende Einstellungen sollen erstellt werden: Gruppe sysusers, rwx für Gruppe aber nicht für andere, files in dem Ordner sollen gleiche gruppe haben mit Einstellungen_

S: Mögliche Lösung wäre:
    
    1. mkdir -p /shared/sysusers
    2. chgrp sysusers /shared/sysusers
    3. chmod 770 /shared/sysusers
    4. chmod g+s /shared/sysusers
    
T: _Suchen sie "root" in der Datei /et/passwd und speichern Sie die Zeilen in /file_

S: Mögliche Lösung wäre:
    
    1. grep root /etc/passwd > /file

T: _Ändern Sie die SELinux Einstellung von enforcing nach permissive_

S: Mögliche Lösung wäre:
    
    1. setenforce 2
    
        Als Alternative gibt vim /etc/sysconfig/selinux, eine alternative Aufgabe wäre die Änderung des SELinuxtypes

T: _Generieren Sie eine LVM Partition mit dem Namen /dev/development/engineering der Größe 32 MB_

S: Mögliche Lösung wäre:
    
    1. fdisk /dev/sdb
    2. pvcreate /dev/sdb3
    3. pvs
    4. vgcreate Development /dev/sdb3
    5. vgs
    6. lvcreate -L 32M -n engineering development
    7. mk.xfs /dev/development/engineering
    8. mkdir /engineering
    9 mount /dev/development/engineering /engineering
    10. blkid /dev/sdb3
    11. vim /etc/fstab
    
T: _Finden Sie alle Dateien die dem User andrew gehören und speichern Sie die Resultate in /tmp/results ab_

S: Mögliche Lösung wäre:
    
    find / -name andrew -exec cp -rvp {} /tmp/results
    
T: _Erstellen Sie einen Cronjob der alle 5 Minuten das Skript /etc/test ausführt_

S: Mögliche Lösung wäre:
    
    1. crontab -e
    2. */5 * * * * /etc/test eintragen
    
T: _Vergrößern Sie die LVM Partition auf 350 MB_

S: Mögliche Lösung wäre:
    
    1. fdisk /dev/sdb //Neue Partition erstellen 
    2. partprobe /dev/sdb
    3. pvcreate /dev/sdb4
    4. pvs
    5. vgextend deveolpment /dev/sdb4
    6. vgs
    7. lvextend -L +265M /dev/development/engineering
    8. lvs 
    9. xfs_growfs /dev/development/engineering
    10 lsblk
    
        Als Alternative wäre die Verkleinerung möglich als Aufgabe
    
T: _Vergrößern Sie die SWAP Partition auf 3GB_

S: Mögliche Lösung wäre:

T: _Authenfiziere die Users vom LDAP_

S: Mögliche Lösung wäre:
    
Create the following users with the following details - If the supplemental groups listed do not exist, create them. 
Bob - Supplemental Group Engineering - PW: cent6
Tim - Supplemental Group WebDev - PW: cent6
Ben - Supplemental Group Chemical - PW: cent6
All accounts created must expire one year from today. 
Set Bob's default shell to zsh
Set Tim's default home directory to /websites - if this directory does not exist, create it.
Apply the same SELinux contexts and permissions to /websites that would be found on a user's /home directory such that Tim and only Tim will have access to /websites
Give Ben complete Sudo access to the machine.
Give Tim Sudo access to the following tool:
iptables

Create a directory named /Homework
Allow all users to read this directory. But nothing else. 
Create three folders inside /Homework 
Bob_homework
Tim_homework
Ben_homework
Give Bob (and only Bob) read and write access to /Homework/Bob_homework
Give Tim (and only Tim) read and write access to /Homework/Tim_homework
Give Ben (and only Ben) read and write access to /Homework/Ben_homework

T: _Ändern Sie die SU Rechte von Peter, sodass nur nur die Programme cat, tail ohne sudo ausgeführt werden kann_

S: Mögliche Lösung wäre: 

    Peter ALL=(ALL:ALL) cat, tail -> /etc/sudoers 
    
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
