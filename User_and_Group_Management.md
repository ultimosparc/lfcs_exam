# lfcs_exam
Exam's preparing &amp; test exam

    Create, delete, and modify local user accounts
    Create, delete, and modify local groups and group memberships
    Configure startup files (read about /etc/skel)
    Manage user privileges
    Configure custom environment paths for userh
    
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


