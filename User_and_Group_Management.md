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
