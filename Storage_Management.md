# Storage Management

_Filesystems, Partitions, Mounting and Backups_ [19, 20]

Daten in modernen Computern werden auf bestimmter Hardware wie HDD, USB oder SSD längerfristig gespeichert. Verbindungen zum Datenaustausch, die interne Elemente in einem Computer Daten austauschen lassen, werden "Bus" bezeichnet. Ein Beispiel für eine solche Verbindung wäre zwischen Festplatte und Motherboard. Ein "Bus" ist also eine Art Interface für den Datenaustausch zwischen interne Elemente eines Computers. Beispiele für ein "Bus" wären SATA und SCSI.

Auf einem physischen Speichermedium werden die Daten in Sektoren eingeteilt. Bei alten Speichermedien hatten die Sektoren eine Größe von 512 KByte, bei modernen Systemen gibt es eine solche festgelegt Größe grundsätzlich nicht. Grundsätzlich wird ein Speichermedium in sogenannte "contiguous groups of sectors" eingeteilt. Ein anderer Begriff für eine solche Gruppe ist "Partition". Neben dem Speicherbereich für den Boot-Vorgang werden die Daten einer Partition in sogenannte Daten-Zylinder eingeteilt. Jeder der Daten-Zylinder besitzt Daten, Verzeichnisse und die Inode-Tabelle, die jeder Datei/Verzeichnis eine eindeutige Inode # zuordnet. Die Inode-Tabelle verweist auf die Blöcke im physischen Speicher, wo die Dateien/Verzeichnisse abgelegt sind. Auf der Inode-Tabelle drauf gibt es eine sogenannte "Verzeichnis-Tabelle". Anhand der Verzeichnis-Tabelle kann man über den Kernel auf die einzelnen Verzeichnisse und Dateien zugreifen. Diese Tabelle entspricht abstrakt einem Filesystem. Demnach kann Filesystem wie folgt skizziert werden. Es ist eine abstrakte, hierarche geordnete Struktur, die aus Verzeichnissen und Dateien besteht. Die Urpsrung eines Filesystem in Unix beginnt mit dem sogenannten "root-Verzeichnis"  ("/"). Vereinfacht gesagt, das root-Verzeichnis ist die Wurzel vom Verzeichnisbaum.   

![Unix_Storage_Concept](https://user-images.githubusercontent.com/15387251/106927311-52bb2200-6712-11eb-81d6-8582dc38094c.png)

Gewöhnlich hat ein Computer eine primäre Partition, die auf einem primären Speichermedium abgelegt ist und das OS enthält. In vielen Fällen werden zusätzliche sekundäre Speichermedien wie USB, externe Festplatte usw. benutzen, die alle selbst eine eigene Partition mit einem eigenen Filesystem und Typ haben. Anders als wie bei Windows, wo die verschiedenen Filesysteme in Koexistenz existieren, werden in UNIX-ähnlichen Betriebssystemen solche "sekundären Partitionen" als Unterverzeichnisse in das primäre Filesystem asselimiert (eingebunden).
Das sogenannte "Mounting" kann in Unix mit dem Befehl mount ausgeführt werden. Ohne Parameter ausgeführt, zeigt der Befehl alles eingehängten Partitionen, dessen Typen und deren Einhängepunkte auf.  
Neben anderen phyischen Geräten wie Drucker usw. werden Speichermedien bzw. deren Partitionen im Verzeichnis /dev "eingehängt", d.h. deren Filesystem wird nun ein Unterverzeichnisbaum vom primären Filesystem und ist somit für alle User zugänglich. D.h. ein physisches Speichermedium wird in UNIX durch das sekundäre Filesystem präsentiert.

Bei der Namengebung der Dateien und Verzeichnisse in /dev muss folgendes Konzept beachtet werden. Der Kernel in Unix greift auf Geräte (Drucker, Maus usw.) über sogenannte "Slots" oder "Känäle" zu. Solche "Slots"/"Kanäle" sind nummeriert und enthalten ein Interface für den Datenaustausch (Gerätetreiber). D.h. es gibt ebenfalls eine Nummerierung der Gerätetreiber, die analog zur Nummerierung der "Slots"/"Kanäle" und als Hauptgerätenummer (Major Device Number) gekannt ist. Ein Gerätetreiber kann mehrere Geräte des gleichen Typs verwalten. Die einzelnen Geräte sind ebenfalls nummeriert. Die Nummerierung der Geräte wird als Untergerätenummer (Minor Device Number) bezeichnet. Die einzelnen Geräte werden geordnet nach blockorientierten Geräten mit direktem Zugriff, wie z. B. Disketten oder Festplatten, und zeichenorientierten sequentiellen Geräte, wie Drucker, Terminal oder Maus. Damit wird jedes Objekt im Unterverzeichnis /dev mit drei Koordinaten vom Kernel eindeutig identifiziert werden.
Entsprechend diesem Konzept findet man oft die folgende Namensgebung für Speichergeräte: 

    Device naming scheme /dev/sd{order}{partition}

Alternativ, oft findet man auch die Bezeichnung hd{order}{partition}. Dabei ist "order" ein beliebiger Buchstabe und "partition" eine beliebige Zahl. Für SSD findet man auch das folgende Namensschema

    SSD device naming scheme /dev/nvme{order}n{ns}p{part}
    
{order} represents order, /dev/nvme0n1 is the first entire disk device, /dev/nvm1n1 the second etc. {ns} is the namespace and {part} is the partition number, e.g. /dev/nvme0n1p1 represents device nvme0, with namespace 1 and partition 1.

/dev/sd\* is the entire disk device file which could be of type SCSI, SATA or USB. Following letter identifies the order, /dev/sda is the first device, /dev/sdb the second etc. The number refers to partition number, /dev/sda2 is the second partition on /dev/sda.

Angenommen, /dev/sda1 ist eine "eingehängte" Partition. A solche partition can be identified by device-node, a label or a UUID.
Auflistung aller block devices und derren Anhängepunkte kann mit 

    lsblk -f
    
aufgelistet werden. Es listet alle Partition in Baumstruktur auf und gibt die UUID, Label usw. an, die man dann ändern kann. 
Vorteile Partitionskonzept:  

    separation of user space from system space.
    easier backups
    performance and security enhancement on certain parts
    swap can be isolated

A partition is associated with

    type: primary, extended or logical
    start: start sector
    end: end sector
    filesystem

Partition table schemes
Im ersten Sektor eines Speichermedium liegen die Metadaten des Speicherdaten. Bei alten Computersystemen spricht man vom sogeannten Master Boot Record (MBR). Dieser enthält unteranderem: 

    Partitionstabelle
    Größenangaben der Festplatte
    Programmcode
    Hilfsprogramme

Damit das Betriebssystem gestartet werden kann, wird das MBR nach starten des Computers in den Arbeitsspeicher geladen und dieser veranlasst, das der Bootsektor von der Festplatte abgearbeitet wird und letztlich das Betriebssystem startet. Die Partitionstabelle is stored in the first 512 bytes of the disk. 

![Ext2](https://user-images.githubusercontent.com/15387251/106944119-830cbb80-6726-11eb-88b4-55e2505e65bd.png)

Bei neuen Computersystemen wurde das alte BIOS durch den UEFI-Standard abgelöst. Damit wurde auch ein neues Layout für die Partionstabellen entwickelt, der GBT-Standard.

![gbt](https://user-images.githubusercontent.com/15387251/106944315-b8b1a480-6726-11eb-91b3-0121f180d534.png)

The GUID Partition Table (GPT) is a standard for the layout of partition tables of a physical computer storage device, such as a hard disk drive or solid-state drive, using universally unique identifiers, which are also known as globally unique identifiers (GUIDs). 

Entsprechend dem GPT-Schema besteht ein Datenträger aus den folgenden Bereichen:

    Master Boot Record (MBR) in Sektor 0 (dem ersten, 512 Bytes großen Datenblock), dessen spezielle Konfiguration den Einsatz der Platte auch unter MBR-Betriebssystemen erlaubt und vor Veränderungen durch MBR-Partitionierungstools schützt (Schutz-MBR; von englisch protective MBR)
    primäre GUID-Partitionstabelle (GPT), bestehend aus Header und Partitionseinträgen
    Partitionen
    sekundäre GPT, bestehend aus Header und Partitionseinträgen

Die sekundäre GUID-Partitionstabelle am Ende des Datenträgers ist teilweise eine Kopie der primären GPT am Anfang des Datenträgers: Die Inhalte der Felder für die Positionen des eigenen und des alternativen GPT Headers sind vertauscht und die Adresse der Partitionstabelle verweist auf die Kopie der Partitionstabelle am Ende der Platte vor dem alternativen Header. Damit haben beide GPT-Header auch eine unterschiedliche CRC32-Prüfsumme. Durch die enthaltene Redundanz kann im Fehlerfall die Partitionstabelle wiederhergestellt werden. Da in der GPT eine Prüfsumme eingetragen wird, kann festgestellt werden, ob beide bzw. welche der beiden GPT fehlerhaft sind. 

Um eine Partition anzulegen, muss neuer Speicherbereich festgelegt und in der Partitionstabelle eingetragen werden. Standardmäßig erstellt man eine Partition mit dem Befehl fdisk. 

_Configuration of the SWAP Partition_ [21]


Im Fall, der Arbeitsspeicher RAM eines Computersystems reicht nicht aus, UNIX benutzt ein Teil des Langzeitspeichers (SWAP Partition), um die operativen Prozesse auszuführen, um den Betrieb aufrechtzuhalten. D.h. nicht wichtige Daten werden in einer Auslagersdatei auf dem Langzeitspeicher wie eine Festplatte ziwschenzeitlich abgelegt. Swap should usually equal 2x physical RAM for up to 2 GB of physical RAM, and then an additional 1x physical RAM for any amount above 2 GB, but never less than 32 MB. 
Gewöhnlich wird bereits beim AnlegenUm nun eine solche Auslagerungsdatei anzulegen müssen folgende Punkte bearbeitet werden
pürfen ob schon swap Partitionen existieren
Falls es keine SWAP Parition oder man eine weitere SWAP Parition anlegen möchte müssen folgende Punkte durchgegangen. Angenommen RAM ist 1.5 GB, dann sollte die SWAP Partitioin ein Größe von 2x 1.5 GB= 3 GB haben: 

    SWAP Parition erstellen
    
       sudo dd if=/dev/zero of=/swapfile bs=1M count=3072   //Auslagerungsdatei anlegen
       sudo chmod 0600 /dev/swapfile                            //SWAP Partition sollte nicht von jedem gelesen werden, die Rechte müssen gesetzt werden
       sudo mkswap /dev/swapfile                                //Formatieren der SWAP Partition
       sudo swapon /dev/swapfile                                //SWAP Partition aktivieren
       
       /dev/zero is a special file in Unix-like operating systems that provides as many null characters (ASCII NUL, 0x00) as are read from it.[1] One of the typical uses is to             
       provide a character stream for initializing data storage. Bei 3072 Blöcken ist die Blockgröße 1MB. 

Falls man die Partition bzw. die Änderung der Partitionstabelle persitieren will, dann muss ein entsprechender Eintrag 

    UUID=99e34f44-b366-4805-b290-614717f9058b     swap     swap     defaults     0 0
    
in die Datei /etc/fstab gemacht werden, wobei UUID referenziert auf die SWAP Partition. 
Alternativ kann man das Programm fdisk werden, um eine SWAP Partition zu erstellen. Zunächst sucht man die Partition aus, wo noch Platz ist unter /dev, dann führt man den Befehl 
fdisk /dev/sda1 zum Beispiel aus. Dann geht man durch das Programm n -> 82 -> p -> w und man dann einen entsprechenden Eintrag. Danach folgende Befehle

    partprobe           //Aktuallisieren der Partitionstabelle
    mkswap /dev/sda1    //Formatieren der Swap Partition
    swapon -a           // Swap partition aktivieren
    swapon -s           // Prüfen ob die Aktivierung geklappt hat
    
ausführen. 

Die drei extra Bits. 
set-user-id bit --> erlaubt Programme wie passwd mit superuserrechten auszustatten, programm wird mit root userid ausgeführt
			anstatt der eigenen userid
set-group-id bit --> programm mit diesem bit werden ausgeführt als root group z.B. write befehl
sticky bit --> swap partition war eine extra disk wo prozessbilder ausgeswapt und wieder eingeswapt wurden um den multiprogramming level
aufrecht zu erhalten, d.h. ausführbare code andere Daten wurden in der Swappartition gepackt, damit das schreiben und lesen schneller
gemacht wurde programme die von vielen user jeden genutzt wurde wurden auf die swap partition gepackt als ganzes stück womit das Lesen und Schreiben
schneller gemacht wurde, da man das programm nicht im normalen Speicher zusammen suchen muss. der gesetzte Stickybit wurde gesetzt um das 
löschen des Programms auf der Swap partition zu vermeiden
Auf File ebene wie oben beschrieben, auf Verzeichnis Ebene gesetzt, nur der Besitzer des Verzeichnis darf files löschen ansonst niemand
beispiel /tmp order. schreibbar und ausführbar und lesbar für alle, nur nicht löschen

_Struktur eines Linux Verzeichnisbaumes_[19]

Ein FS besteht aus einem Verzeichnisbaum. Dieses Interface besteht aus Ordnern und Dateien, wobei Ordner nur sDateien mit besonderen Eigenschaften sind: 

	1. niemals leer wegen "." und ".."
	2. Können nicht durch unpriviligierte Programme manipuliert werden, nur durch Systemscalls
	3. enthalten nur Einträge der Form (name, inode #), Tabelle mit hashwerten
    
Im Verzeichnisbaum unterscheidet man 4 Typen von Inhalten: 

	1. Statitscher Inhalt: belibet unverändert bis bearitet bzw. neu konfiguriert wird.
	2. Dynamischer/Variabler Inhalt: kann von aktiven Prozessen geändert / ergänzt werden
	3. persistenter Inhalt: bleib auch nach dem Neubooten erhalten
	4. Laufzeit Inhalt: Prozess/systemspezifischer Inhalt wird nach dem Booten gelöscht. 

Die veschiedenen Ordner eines Verzeichnisbaumes können nach den vier Typen eingeordnert werden. Die Struktur eines Verzeichnisbaumes hat sich leider über die Zeit verändert, es sind Ordner hinzugekommen, die andere Ordner ergänzen, und es sind Ordner aus dem Baum hinausgenommen worden. Zudem kommt hinzu, je nach Betriebssystem/Version von Linux gibt es Änderungen im Verzeichnisbaum. Folgende Übersicht gibt eine möglichen Aufbau des Verzeichnisbaums an: 

	/bin
	Essential binaries required when no other filesystems have yet been mounted.
	Also in single userm ode or recover mode. Basisbefehle für Users und Systemadminstratoren. 

	/sbin
	System binaries for booting, recover or restore. Also capable of mounting /usr, /home etc.
	
	/usr
	A secondary hierarchy that is not needed for system booting. It contains multi-user applications. 	  Package managers touch here. /usr/bin, /usr/lib, /usr/local, /usr/sbin, /usr/bin, /usr/share, 	/usr/src (kernel source), 
	
	/boot
	Contains compressed kernel image, initrd, GRUB (Statitischer Inhalt für das Booten)

	/dev
	Enthält die Dateien, die physische Geräte darstellen wie Partitionen, CD-Rom Laufwerke usw. (Geräte die permant angeschlossen sind)
	
	/opt
	Isolated installation directory (not scattered in multiple directories). Helpful for proprietary sw 	     or packages downloaded without package managers

	/proc
	Pseudo-fs. Persists only in memory for processes to keep internal state. Each process has its place 	    as a subdirectory

        /root
	Home of root user

	/var
	For variable data that changes frequently. Logs, spool directories (for mail and cron), transient 	  data for cache (e.g. packages) , lock files (linked to /run/lock), Datenbanken , loffile usw

	/run
	Pseudo-fs. For transient data that contains runtime information as lock files

	/media
	Mount filesystems for removable media such as USBs

	/run
	A temporary place to mount filesystems such as NFS
	
	/tmp
	Daten werden 10 T aufbewahrt in diesem Ordner, in /var/tmp Daten werden 30 Tage aufbewahrt
	
	/etc 
	Spezifische Dateien zur Konfiguration des Host Systems, ent/init.d enthält Skripte die beim Start ausgeführt werden
	
	/lib
	Esentzielle geteilte Bibs und Kernelmodule
	
	/opt
	Add-ons für Softwarepakete
	
	/srv
	Daten von Services, die vom System generiert werden
	
![dirtree](https://user-images.githubusercontent.com/15387251/110190172-406ff900-7e22-11eb-9d4d-6534618d1671.jpg)
	
	
