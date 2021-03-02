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


