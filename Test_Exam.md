# Test Exam
According to the exercises from the sources, I have created a serie of tasks for every topic. (Task (T))

Die Aufgaben bilden eine Grundlage, um bestimmte Teilaufgaben von Examsaufgaben zu idenzifizieren und zu lösen.  

## 1. Essential Commands

T1: _Ersetzen Sie das Wort "Haus" mit dem Wort "Eigenheim" überall in der Textdatei "Immobilien.txt" (Substitution)._
    
T2: _Ersetzen Sie in Zeile 200 den Preis von 100.000€ nach 150.000€ in der Textdatei "Immobilien.txt" (Substitution)._
       
T3: _Löschen Sie in der Textdatei "Immobilien.txt" die Zeile 202._

T4: _Erstelle ein komprimiertes Archiv "test.tar" der Dateien "datei1" und "datei2" und lösche die beiden Dateien "datei1" und "datei2"._

## 2. User and Group Management

T5: _Erstellen Sie einen neuen User "Peter" und geben Sie Ihm "Superuser"-Rechte._

T6: _Erstellen Sie eine neue Gruppe "Verkäufer" und fügen Sie den User Peter hinzu_

T7: _Erstellen Sie einen Unterordner "Haus" und ändern Sie die Gruppe zu "Verkäufer" und den Besitzer zu root_

## 3. Operation of Running Systems

T8: _Erstellen Sie einen Softlink und Hardlink auf die Datei "test"._

T9: _Geben Sie dem Prozess "Sleep" eine höhre Priorität, damit dieser schneller ausgeführt wird_

T10: _Legen Sie einen Chronjob an, der jede Minute das Skript /tmp/test.sh ausführt_

## 4. Service Configuration

## 5. Networking

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
        
      Exam's preparing &amp; test exam

Mit welche Befehl kann SWAP deaktiviert werden
swapoff, /sbin/swapoff
What is the difference between the i and a commands of the vi editor?
 i (insert) inserts text before the current cursor position whereas a (append) inserts text after the cursor.
 Umgebungsvariablen wiederholen
 find methode . -maxdepth Optionen sollte man kennen
 Was passiert mit einen Programm das chrashed, es wird eine corefile geniert,
 ulimit sollte man kennen
 ssh-agent speichert die Schlüssel für die Zugriff
RAID, LVM, partitioning and fs manipulations are the must. As well as fsck.
    List, create, delete, and modify physical storage partitions, including GPT (using fdisk, gdisk utilities)
    Manage and configure LVM partitions
    Create and manage software RAID (using mdadm)
    Mount file systems during boot (using systemctl mount target)
    Configure user and group disk quotas for filesystem
    Configure and manage swap partitions or files
    Be familiar with storage encryption (using LUKS)  
    Been going over software RAID in CentOS 6.5 for my LFCS exam that is next week. Here are my notes so that I don't forget. 
mdadm 
This is the tool you want to use to create software RAID
/etc/mdadm.conf (Main configuration file)
Syntax
mdadm -C -v <device name> --level=<raid level> --raid-devices=<#> <dev files>
EX: mdadm -C -v /dev/rd0 --level=raid0 --raid-devices=2 /dev/sdb /dev/sdc
This will create a raid array named /dev/rd0 out of two arbitrary HDs
You will need to format those two drives the same way you normally would with mkfs and whatnot. 
EX: mdadm --stop /dev/md0        #Stops the Array
EX: mdadm --remove /dev/md0  #Disassembles the array
You can use mdadm to do some other cool things. Here are a couple:
Set up monitoring services
Run the following command to append the appropriate info to the /etc/mdadm.conf 
mdadm --detail --scan >> /etc/mdadm.conf
This will tell the mdadm service to monitor any raid arrays.
The "mdadm --detail --scan" command will give details about the drive.
Verify that the mdadm monitor service is running and that it is set to start at boot
sysvinit - #service mdmonitor start
sysvinit - #chkconfig mdmonitor on
systemd - #systemctl start mdmonitor
systemd - #systemctl enable mdmonitor
Setup mail alerts
echo "MAILADDR root" >> /etc/mdadm.conf
This will append the local root host as the address
Check array details
cat /proc/mdstat (less detailed summary)
mdadm --detail /dev/md0 (more detailed summary
So there's my rundown - As always, this is all valid for CentOS - I can't attest to other distros. It's quick. It's dirty. Most of all, it's efficient. I like it.   

## 7. Solutions & Remarks

Task (T), Solution (S)

T1: _Ersetzen Sie das Wort "Haus" mit dem Wort "Eigenheim" überall in der Textdatei "Immobilien.txt" (Substitution)._

S1: Es gibt hier zwei mögliche Lösungswege. 

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
      
T2: _Ersetzen Sie in Zeile 200 den Preis von 100.000€ nach 150.000€ in der Textdatei "Immobilien.txt" (Substitution)._

S2: Mögliche Lösung wäre:
  
    1. vi Immoblien.txt
    2. :set number
    3. :200
    4. i (insert-Mode)
    5. Die entsprechende Stelle ersetzen
    6 :wq
    
       Das Ersetzen von Textstellen in einer Textdatei wird gerne als eine Teilaufgabe 
       verwendet im Zusammenhang mit dem Ersetzen von User-Eigenschaften, z.B. für das Passwort-Datei. 
       
T3: _Löschen Sie in der Textdatei "Immobilien.txt" die Zeile 202._

S3: Mögliche Lösung wäre:
    
    1. vi Immoblien.txt
    2. :set number
    3. :202
    5. :d 
    6. :wq
    
       Im Vi-Editor gibt es verschiedene Wege eine Zeile zu löschen, jedoch der "delete"-Befehl 
       ist simpel und schnell auszuführen, zusätzlich kann man das Ergebniss gleich im vi-Editor betrachten. 
 
T4: _Erstelle ein komprimiertes Archiv "test.tar" der Dateien "datei1" und "datei2" und löschen Sie die beiden Dateien "datei1" und "datei2"._

S4: Mögliche Lösung wäre:
    
    tar -cvzf vi test.tar datei1 datei2; rm -rf datei1 datei2;
    
       Um fas Archiv wieder zu extrahieren, muss einfach nur das "c" mit dem "x" ausgetauscht werden,
       "tar -xvf". Falls bei der Archivierung noch die Option p mitgegeben wird, werden die Rechte mit archiviert.
       Um zu prüfen, ob "test.tar" wirklich komprimiert ist, einfach "file test.tar" ausführen. 
       
T5: _Erstellen Sie einen neuen User "Peter" und geben Sie Ihm "Superuser"-Rechte._

S5: Mögliche Lösung wäre:
    
    1. sudo useradd -m -s /bin/bash Peter;
    2. sudo visudo
    3. Peter ALL=(ALL:ALL) ALL
    4. :wq
    
       Visudo ist ein spezielles Programm, das den vi-Editor benutzt, um die Datei 
       zu öffnen, wo die Superuser-Rechte liegen.
       In dieser Datei kann auch notiert werden, welche Gruppen Superuser-Rechte bekommen sollen. 
       Damit können mehrere User gleichzeitig mit Superuser-Rechten ausgestattet werden.
       Standardmäßig existiert bereits eine solche Gruppe, genannt "wheel".

T6: _Erstellen Sie eine neue Gruppe "Verkäufer" und fügen Sie den User Peter hinzu_

S6: Mögliche Lösung wäre:
    
    1. groupadd Verkäufer
    2. sudo usermod -aG Verkäufer Peter
        
       Wenn die Optionen -aG nicht verwendet werden, dann wird der User gelöscht von allen Gruppen, 
       die nicht innerhalb der -G Liste stehen. 

T7: _Erstellen Sie einen Unterordner "Haus" in /home/Peter und ändern Sie die Gruppe zu "Verkäufer" und den Besitzer zu root_

S7: Mögliche Lösung wäre:
    
    1. cd ~
    2. mkdir -p Haus
    3. sudo chown root:Verkäufer Haus
    
       Die Option -p vergewissert, das auch die Parentordner existieren, falls nicht, dann werden diese angelegt. 
      -R setzt den Change rekursiv auf die Unterordner um. 
       
T8: _Erstellen Sie einen Softlink, genannt "symlink", und Hardlink, genannt "hardlink", auf die Datei "test"._

S8: Mögliche Lösung wäre:
    
    ln -s test symlink; ln test hardlink
    
       Der Unterschied zwischen einen Hardlink und einem Symlink (auch Softlink genannt) besteht darin,
       das Hardlinks und Ursprungsdatei teilen sich die gleiche inode. Bei Softlinks ist das nicht der 
       Fall. 
       
T9: _Geben Sie dem Prozess "Sleep" eine höhre Priorität, damit dieser schneller ausgeführt wird_

S9: Mögliche Lösung wäre:
    
    1. top -u username --> PID bestimmen
    2. renice '-20' -p 'PID'
    
        Wir der erste Befehl "top -u username" nochmal ausgeführt, dann
        muss sich der NI-Wert des Prozess "Sleep" geändert haben.
        
T10: _Legen Sie einen Chronjob an, der jede Minute das Skript /tmp/test.sh ausführt_

S10: Mögliche Lösung wäre:
    
    1. chrontab -e
    2. */1 * * * * /tmp/test.sh
    3. chrontab -l
    
       Ein Zeiteintrag kann mit Hilfe der Tabelle "more /etc/crontab" definiert werden (nur unter CentOS).
