# User and Group Management

Create, delete, and modify local user accounts
    Create, delete, and modify local groups and group memberships
    Configure startup files (read about /etc/skel)
    Manage user privileges
    Configure custom environment paths for userh
_Change User Privileges_ 

Mit dem Befehl _visudo_ können nicht root Usern, also alle andere User, superuser rechten vergeben werden. Es gibt drei Möglichekeiten, Superuser rechte zu vergeben. Direkt  gebunden an einen User Account, über die Mitgliedschaft zu einer bestimmten Gruppe mit super user privilegien oder über spezielle Programme, die mit superuserrechten ausgestattet werden. 

Mögliche Auszug aus der der Datei /etc/sudoers die durch den Befehl visudo bearbeitet wird: 

	## The COMMANDS section may have other options added to it.
	##
	## Allow root to run any commands anywhere
	root    ALL=(ALL)       ALL

	## Allows members of the 'sys' group to run networking, software,
	## service management apps and more.
	# %sys ALL = NETWORKING, SOFTWARE, SERVICES, STORAGE, DELEGATING, PROCESSES, LOCATE, DRIVERS

	## Allows people in group wheel to run all commands
	%wheel  ALL=(ALL)       ALL

	## Same thing without a password
	# %wheel        ALL=(ALL)       NOPASSWD: ALL

	## Allows members of the users group to mount and unmount the
	## cdrom as root
	# %users  ALL=/sbin/mount /mnt/cdrom, /sbin/umount /mnt/cdrom

	## Allows members of the users group to shutdown this system
	# %users  localhost=/sbin/shutdown -h now

	## Read drop-in files from /etc/sudoers.d (the # here does not mean a comment)
	#includedir /etc/sudoers.d
	ec2-user        ALL=(ALL)       NOPASSWD: ALL

The syntax to configure user privileges is as follows:

	user_list host_list=effective_user_list tag_list command_list
Where:

	user_list – list of users or a user alias that has already been set.
	host_list – list of hosts or a host alias on which users can run sudo.
	effective_user_list – list of users they must be running as or a run as alias.
	tag_list – list of tags such as NOPASSWD.
	command_list – list of commands or a command alias to be run by user(s) using sudo.
    
Beispiele:
	
	aaronkilik ALL=(ALL) NOPASSWD: /bin/kill
	%sys ALL=(ALL) NOPASSWD: /bin/kill, /bin/rm

Für alle Dateien gilt: es existiert mind eine Gruppe, die diese Datei zugeordnet ist. 

Now, you need to create a configuration file to enable your user account to use sudo. Typically, this file is created in the /etc/sudoers.d/ directory with the name of the file the same as your username. For example, for this demo, let’s say your username is student. After doing step 1, you would then create the configuration file for student by doing this:
# echo "student ALL=(ALL) ALL" > /etc/sudoers.d/student
Finally, some Linux distributions will complain if you do not also change permissions on the file by doing:
# chmod 440 /etc/sudoers.d/student

Die Rechte von Dateien und Verzeichnissen innerhalb des Dateiverzeichniss werden über permission bits gesteuert. Man unterscheidet
zwischen drei Schichten beim Rechtemanagement: Besitzer, Gruppe, Allgemein. 
In jeder Schicht kann man eine Datei/Verzeichnis lesen, bearbeiten und ausführen. Die Steuerung der Rechte erfolgt über den Befehl
chmod, chgrp. Die einzelnen Rechten in den drei Schichten sind sogenannte Zahlen zugeordnet, die man als Parameter den Befehlen
chmod chgrp übergeben kann. Folgende Tabelle zeigt eine beispielhafte Übersicht der Rechte in einer Schicht: 

	(R)ead - 4 , (W)rite - 2 , (E)xec - 1 
	|		| Ordner 	| Datei |
	|---------------|---------------|-------|
	|7 (W,R,E)	| 12		| 01	|
	|6 (W,R)	| 11		| 02	|
	|5 (R,E)	| 10		| 03	|
	|3 (W,E)	| 09		| 04	|
	|2 (W)		| 08		| 05	|
	|1 (E)		| 07		| 06	|

	1. File kann beschrieben, gelesen und ausgeführt werden in der Shell
	2. Datei kann im Verzeichnis angezeigt werden, Inhalt kann ausgelesen und modifiziert werden, Ausführung nicht z.B. in Skripten
	3. Datei wird im Verzeichnis angezeigt , kann ausgeführt werden, Inhalt der Datei lesbar im Terminal, kann nicht modifiziert werden
	4. ausführbar, Änderung wäre möglich, jedoch kann der Inhalt der Datei nicht ausgelesen werden
	5. kann modifiziert werden, jedoch Datei kann weer gelesen ausgeführt werden
	6. ausführbar, aber nicht lesbar/modifiizert bar
	7. mit cd kann man ins Verzeichnis wechseln, jedoch sich nicht den inhalt auslesen 
	8. Files können angelegt und gelöcht werden
	9. Es kann ins Verzeichnis gewechselt werden, Dateien könen angelegt und elöscht werden
	10. usw



_Die extra Bits_ 
|		| File		| Ordner
| suid(4)	| Second Header |
| sgid (2) 	| 	------------- |
| sticky (1) 	| Content Cell  |
|    | Content Cell  |

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


