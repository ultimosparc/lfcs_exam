# lfcs_exam
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
