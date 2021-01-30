# lfcs_exam
Exam's preparing &amp; test exam

    Configure networking and hostname resolution (using hostnamectl)
    Configure network services to start automatically at boot (using systemctl, read about systemd-networkd/systemd-resolved)
    Start, stop, and check the status of network services
    Implement packet filtering (iptables, no firewalld since it’s not related to RHEL)
    Manipulate /etc/hosts file
    
    Aufgabe wäre zuordnung von netwerk eigenschaften und servern
    zum beispiel festlegen von statischen Ips. DNS, FQDN und Gateways
    
    Weitere mögliche Aufgaben ist installieren 
    
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
