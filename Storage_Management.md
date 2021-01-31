# Storage Management

_Creating Backups of Filesystems_ [19, 20]

Daten in modernen Computern werden auf bestimmter Hardware wie HDD, USB oder SSD längerfristig gespeichert. Verbindungen zum Datenaustausch, die interne Elemente in
einem Computer Daten austauschen lassen, werden "Bus" bezeichnet. Ein Beispiel für eine solche Verbindung wäre zwischen Festplatte und Motherboard. Ein "Bus" ist also eine Art Interface für den Datenaustausch zwischen interne Elemente eines Computers. Beispiele für ein "Bus" wären SATA und SCSI. 
Phyische Geräte wie Speichermedien (HDD,USB,SSD) werden in Linux als Textdatei im Verzeichnis /dev zur Verfügung gestellt. 
Der Kernel in Unix greift auf Geräte (Drucker, Maus usw.) über sogenannte "Slots" oder "Känäle" zu. Solche "Slots"/"Kanäle" sind nummeriert und enthalten ein Interface für den Datenaustausch (Gerätetreiber). D.h. es gibt ebenfalls eine Nummerierung der Gerätetreiber, die analog zur Nummerierung der "Slots"/"Kanäle" und als Hauptgerätenummer (Major Device Number) gekannt ist. Ein Gerätetreiber kann mehrere Geräte des gleichen Typs verwalten. Die einzelnen Geräte sind ebenfalls nummeriert. Die Nummerierung der Geräte wird als Untergerätenummer (Minor Device Number) bezeichnet. Die einzelnen Geräte werden geordnet nach blockorientierten Geräten mit direktem Zugriff, wie z. B. Disketten oder Festplatten, und zeichenorientierten sequentiellen Geräte, wie Drucker, Terminal oder Maus.   
Damit hat jede Gerätedatei drei "Koordinaten", mit der sie vom Kernel, unabhängig von ihrem Namen, eindeutig identifiziert werden kann.
Entsprechend diesem Konzept findet man oft die folgende Namensgebung für Speichergeräte: 

    Device naming scheme /dev/sd{order}{partition}

Alternativ, oft findet man auch die Bezeichnung hd{order}{partition}. Dabei ist "order" ein beliebiger Buchstabe und "partition" eine beliebige Zahl. Für SSD findet man auch das folgende Namensschema

    SSD device naming scheme /dev/nvme{order}n{ns}p{part}
    
{order} represents order, /dev/nvme0n1 is the first entire disk device, /dev/nvm1n1 the second etc. {ns} is the namespace and {part} is the partition number, e.g. /dev/nvme0n1p1 represents device nvme0, with namespace 1 and partition 1.

/dev/sd\* is the entire disk device file which could be of type SCSI, SATA or USB. Following letter identifies the order, /dev/sda is the first device, /dev/sdb the second etc. The number refers to partition number, /dev/sda2 is the second partition on /dev/sda.

Auf einem Speichermedium, wie beispielsweise /dev/sda1, sind die Daten in Partitionen organisiert. A partition can be identified by device-node, a label or a UUID.



blkid prints block device attributes and requires root access.

blkid -k # list all known filesystems

lsblk lists block devices and mount points for each.

lsblk -f # list with filesystem type and uuid
Disk geometry

Rotational disks consist of platters, each of which is read by heads as disk spins. Tracks are divided into sectors in 512 byte size. SSDs are not rotational and have no moving parts.
Partitioning

Disk is divided into contiguous groups of sectors called partitions. Partitioning promotes:

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

The content of hardware disk starts disk metadata, e.g. partition tables.

MBR

    Dates back to early days of MSDOS. In some tools, aka, dos or msdos. Table is stored in the first 512 bytes of the disk. Up to 4 primary partitions of which one as an extended partition.
    Table has 4 entries and each 16 bytes size. Entry in the table contains active bit, file system code (xfs, ext4, swap etc.) and number of sectors.

mbr-scheme

GPT

    modern. disk starts with the GPT header (and also proactive MBR for backwards compatibility)
    Up to 128 partitions in the table and each 128 bytes of size.

gpt-scheme

    The partition table and filesystem comes with the vendor and it's possible to migrate it from MBR to GPT.

Backup partition tables

To backup MBR, copy MBR table sudo dd if=/dev/sda of=mbrbackup bs=512 count=1

To restore MBR write it back to the disk sudo dd if=mbrbackupk of=/dev/sda bs=512 count=1

To backup GPT, use sgdisk sudo sgdisk --backup=sdabackup /dev/sda
Partition table editors

Tools below at hardware device level. No filesystems need to be mounted ahead.
tool 	purpose
fdisk 	most standard interactive tool, works for MBR and GPT
sfdisk 	non-interactive fdisk for scripting
parted 	GNU version, interactive tool, works for MBR and GPT
gdisk 	guid partition table manipulator
sgdisk 	script interface for gdisk
fdisk

fdisk -l # list all partitions fdisk <device> # go to interactive mode for device
parted

parted [options] [device [command]]

parted -l # list partition tables on all devices parted /dev/sdb print # partition table for /dev/sdb
parted interactive mode

(parted) mktable gpt # create a partition table (destroys data) (parted) mkpart primary 0 8000 # create part

    /proc/partitions is what kernel is aware of partitions.

    losetup to associate a file or block device with a loop device. A loop device is pseudo device which makes a file to be accessed as a block device. Certain commands like lsblk work only with block devices.





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
