# Operation_of_Running_Systems

Priviligierte und nicht priviligierte Prozesse und userspace und systemspace. 
Unix OS Kernel ist eine API mit verschiedenen Entry points, ein Entry point liefern service das der Kernel anbietet. 
Kernel Ansammlung von Funktionen mit Api wo die Funktionen fest definiert sind mit Parametern und Datentypen, die dann als Entrypunkt für den Service dienen
Kernel wird in den Systemspace geladen nach dem booten und bleibt dort bis zum zum shutdown
user können über user prozesse mit dem Api mit dem Kernel kommunizieren, der die geforderten Resoucen verwaltet und ensprechende Werte zurückliefert.
The kernel provides many services to user programs, including

 process scheduling and management,
 I/O handling,
 physical and virtual memory management,
 device management,
 le management,
 signaling and inter-process communication,
 multi-threading,
 multi-tasking,
 real-time signaling and scheduling, and
 networking services.
Network services include protocols such as HTTP, NIS, NFS, X.25, SSH, SFTP, TCP/IP, and Java.
Exactly which protocols are supported is not important; what is important is for you to understand
that the kernel provides the means by which a user program can make requests for these services.

There are two dierent methods by which a program can make requests for services from the kernel:
 by making a system call to a function (i.e., entry point) built directly into the kernel, or
 by calling a higher-level library (i.q. system programs) routine that makes use of this call.

Ein System Programm kann man beispielweise unter /bin, /usr/bin gefunden werden
_Priorisierung von Prozessen_ [5]

Linux can run a lot of processes at a time, which can slow down the speed of some high priority processes and result in poor performance.
To avoid this, you can tell your machine to prioritize processes as per your requirements.
Dies erfolgt über die Vergabe von sogenannte nice-Werten. Anhand dieser Werte
werden die Prozesse unterschiedlich schnell ausgeführt. 
This priority is called Niceness in Linux, and it has a value between -20 to 19. The lower the Niceness index, the higher would be a priority given to that task.
The default value of all the processes is 0.
To start a process with a niceness value other than the default value use the following syntax

        nice -n 'Nice value' process name
        
Den Nice-Werte von laufende Prozesse können mit dem Befehl _renice_ modifiziert werden. Z.B. könnte 
ein Befehl wie folgt aussehen:

        renice 'nice value' -p 'PID'
        

Mit dem Programm _top_ können alle aktuell laufenden Prozesse vom ganzem System angezeigt werden.
Zum Beispiel könnte die Ausgabe wie folgt aussehen: 

    top - 11:37:22 up 5 min,  0 users,  load average: 0.52, 0.58, 0.59
    Tasks:   4 total,   1 running,   3 sleeping,   0 stopped,   0 zombie
    %Cpu(s):  3.4 us,  3.1 sy,  0.0 ni, 93.0 id,  0.0 wa,  0.5 hi,  0.0 si,  0.0 st
    KiB Mem : 33389320 total, 20049992 free, 13109976 used,   229352 buff/cache
    KiB Swap: 61776380 total, 61669808 free,   106572 used. 20145612 avail Mem

      PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
        1 root      20   0    8936    316    272 S   0.0  0.0   0:00.14 init
        6 root      20   0    8936    224    180 S   0.0  0.0   0:00.00 init
        7 constan+  20   0   16784   3432   3344 S   0.0  0.0   0:00.09 bash
       35 constan+  20   0   17648   2100   1504 R   0.0  0.0   0:00.03 top
       
 In der Spalte _NI_ sieht man, alle Prozesse haben einen Nice-Wert von 0. 


 _Ausführung von Jobs auf einem Linux-System_ [6,7,8,9]
Das Programm "echo" schreibt beispielsweise eine Nachricht in die Standardausgabe "stdout". Führt man das Programm "echo" aus, dann generiert man
einen Prozess. D.h. die Bezeichnung Prozess steht für "ausführbares Programm". Wird nun der Befehl "echo test &" im Terminal eingegeben, 
dann startet man den Prozess im Vordergrund und schiebt den Prozess in den Hintergrund vom Terminal. Das bedeutet, 
während der Ausführungszeit des Prozesses bleibt das Terminal "ansprechbar", d.h. es können weitere Befehle eingeben werden. 
Prozesse im Hintergrund nennt man Job. Jobs haben wie Prozesse einen Parentprozess. Jobs, dessen Parentprozess den "init-Prozess" ist, nennt man "Demoans". 
Bei allen Begriffen, die oben genannt werden, handelt es sich um abstrakte Begriffe, die von einander abhängen. Die folgende Darstellung illustriert noch einmal
diese Abhängigkeiten: 
    
    file (sourcecode) ->  program (executable file) -> process (running instance of a program)
        -> job (process in background) -> demoan (job having init as parent) 

Linux bietet die Möglichkeit, Jobs zu einer bestimmten Zeit ausführen. Solche Jobs nennt man Chron-Jobs (Chronjobs). Es gibt zwei Befehle, um solche Chronjobs zu erstellen. Der erste Befehl ist "at". Ein möglicher Aufruf könnte wie folgt aussehen

        at 8PM
        
Damit können einzeilige Jobs zu bestimmten Zeiten ausgeführt werden. Die zweite Möglichkeit um Chronjobs zu erstellen ergibt sich durch den Befehl "chrontab". 
Crontab files can be used to automate backups, system maintenance and other repetitive tasks. The syntax is powerful and flexible, so you can have a task run every fifteen minutes or at a specific minute on a specific day every year.
Um einen Chromjob zu erstellen, man muss den Befehl

        chrontab -e
        
benutzen. Dieser Job öffnet die sogenannte "Chrontable". In dieser Tabelle werden nun die Chronjobs definiert. Zum Beispiel könnte ein Eintrag wie folgt aussehen: 

        * * * * * /tmp/test.sh
        
Ein solcher Eintrag besteht aus einem Zeiteintrag " * * * * * " und dem Befehl/Skript. Anhand der Systemlogs kann geprüft werden, ob ein Chronjob ausgeführt wurde.
An den folgenden Stellen im Dateisystem befinden sich Logs:

        /etc/rsyslog.conf
        /etc/syslog.conf
        /var/log

Folgende Beispiele zeigen Zugriff auf Logs, um die Ausführung von Chronjobs zu prüfen:

        grep -ic cron /var/log/* | grep -v :0
        grep cron /etc/rsyslog.conf
        grep -i debian-sa1 syslog | tail -1

jede inode enthält drei Zeiten: 
mtime ->  the time the le was last modied
ctime -> the time the le attributes were last modied
atime ->  the time the le was last read

_System Logs_[30]

Es gibt verschiende Logs auf einer Linux Maschine. 

        /etc/rsyslog.conf
        /etc/syslog.conf
        /var/log
       
lastlog
last
lastb
logrotate
journalctl

Other commands may be used to show the logs; or more specifically, portions of the logs.

Lastlog (last)

The ‘lastlog’ program is used to show the last time a user logged into the system. The log file is located at ‘/var/log/lastlog’.

NOTE: The 'lastlog' command works well on CentOS, but on Ubuntu, you may need to use the command 'last'. On Ubuntu, the GDM will not register the login information when logging into the GUI. You can use the 'last' command to accomplish the same thing as 'lastlog'.
For Ubuntu, you get a better listing of the last logged in information from the command ‘last’. The ‘last’ command will show the TTY into which a user logged into the system. It also shows the kernel version used by the user. The date and time are given for each login. An entry is also made for when a shutdown or reboot occurs. An example of the ‘last’ command is shown in Figure 3. The information from which the ‘last’ command gets data comes from ‘/var/log/wtmp’.

 The ‘last’ command also works in CentOS.

When using the ‘last’ command you can use the command ‘last reboot’ to see the listing of when the system was booted. If you want to see when a specific user logged into the system, you can use the command 'last username'. For example, on my system, I would use the command 'last jarret' to see when I logged in last.

logrotate

If log files are continuously used with new entries being added daily, the files would get quite large. Depending on your file system, you could end up making the file too large to be supported. For example, if you were to use FAT32 to format your hard drive, you could only have log files up to 4 GB in size.

The logs are rotated once a day by default. The script is found at ‘/etc/cron.daily/logrotate’ and is as follows:

#!/bin/sh
# Clean non existent log file entries from status file
cd /var/lib/logrotate
test -e status || touch status
head -1 status > status.clean
sed 's/"//g' status | while read logfile date
do
[ -e "$logfile" ] && echo "\"$logfile\" $date"
done >> status.clean
mv status.clean status
test -x /usr/sbin/logrotate || exit 0
/usr/sbin/logrotate /etc/logrotate.conf

To see the script working, you can run ‘logrotate -d /etc/logrotate.conf’. The parameter ‘-d’ will perform a test. The screen output will show what could/should be performed, but nothing is changed. The second parameter ‘/etc/logrotate.conf’ specifies the location and name of the configuration file to use for the test.

NOTE: If you want to perform a log rotation, then remove the '-d' option.

Now, let’s look at the configuration file ‘logrotate.conf’:

# see "man logrotate" for details
# rotate log files weekly
weekly
# use the syslog group by default, since this is the owning group
# of /var/log/syslog.
su root syslog
# keep 4 weeks worth of backlogs
rotate 4
# create new (empty) log files after rotating old ones
create
# uncomment this if you want your log files compressed
#compress
# packages drop log rotation information into this directory
include /etc/logrotate.d
# no packages own wtmp, or btmp -- we'll rotate them here
/var/log/wtmp {
missingok
monthly
create 0664 root utmp
rotate 1
}
/var/log/btmp {
missingok
monthly
create 0660 root utmp
rotate 1
}
# system-specific logs may be configured here

We can see the configuration information specifies that the log rotations are performed weekly. Log files are kept for four weeks. A new empty file is created when the logs are rotated. The log files are not compressed when backed up. Log information is kept in ‘/etc/logrotate.d’. Finally, two logs are backed up - ‘wtmp’ and ‘btmp’. The two backed up log files have a different rotation plan than the default.

Create a Rule

Sometimes, the default logging is not what you need. You may need more information to be logged.

You can log information for specific services, or priorities, for the system itself. The priorities are as follows:

auth/authprivate: security/authorization messages
cron: cron messages
daemon: system daemons
kern: kernel messages
local0-local7: local use messages
lpr: line printer messages
mail: mail system messages
news: USENET News system messages
syslog: system log daemon messages
user: user level messages
uucp: user and group subsystem messages

The other part you need to determine is the amount of logging. The amount of logging, or the priority, is as follows:

debug: Debug information
info: Informational message
notice: Condition that might require intervention
warn: Warning errors
err: Error condition
crit: Critical error
alert: Condition that needs immediate attention
emerg: Emergency is
If we wanted to create logging for the system, we could use the ‘local#’ facility (where # is 0-7). We then need to know which priority to use for logging. When you pick a priority, logging will occur on all other priorities below it in the list. If you chose ‘info’, it would also log ‘info’, ‘notice’, ‘warn’, ‘err’, ‘crit’, ‘alert’ and ‘emerg’. So the logging would be labeled as ‘local0.crit’. If you would want to only log ‘info’ and nothing below it in the list, the priority would be ‘local0.=info’.

To enable the logging you need to edit the file ‘/etc/rsyslog.conf’ For the last line you need to add the following line:

local0.info /var/log/local0-log

The second parameter is the location of the log file used for the specific entry, which can be any valid file name you want to use. After saving the file you will need to restart the ‘rsyslog’ service to include the new logging entry. To restart the service, use the command:

systemctl restart rsyslog

If you wanted to test the logging you can force a message to the log file by issuing the command:

logger -p local0.info “Information message issued by me!”
logger -p local0.warn “Warning message!”

NOTE: You can specify the priority of the error message after the period (.).

Now, you can list the contents of the log file with the command:

cat /etc/log/local0-log

journalctl

Now that we have created logs and know the placement of the logs, it is beneficial to be able to easily read them.

The command ‘journalctl’ will combine all of the logs located at ‘/var/log/*’ into one listing on the screen. Displaying the content on the screen is the default. It is also default that the list is a complete list, oldest entries first, with all of the log entries sorted by date and time.

NOTE: If you include your own special log, as discussed above, the contents are included in the listing.

The listing is made using the 'less' command. You can use the 'Page Up' and 'Page Down' key to move around. Use the left and right arrow keys to see lines that go off the screen. Use 'Home' and 'End' keys to move to the beginning and end of the listing. To exit the listing, press 'CTRL+C'.

If you only want to see the last 10 entries of the file, the most recent ten entries, you use the command ‘journalctl -n’. If you want more entries listed then you include the number of entries you desire to see. If you wanted to see 20 entries the command would be ‘journalctl -n 20’

Since all services are included in the listing it is possible to see only entries from a specific service. If you only wanted to see all of the ‘cron’ service entries, the command is ‘journalctl -u cron’. The listing may quite a long one and you can include the ‘-n’ just to see the most current ten entries for ‘cron’.

Look into the man page for ‘journalctl’ to see more available options. Practice all of these commands, as well as their other parameters, to make sure you understand their capabilities.

You most likely will get a listing of a lot of accounts that have ‘Never logged in’. Figure 1 shows a screenshot of the ‘lastlog’ command in CentOS. The same command is shown in Figure 2 for Ubuntu.
