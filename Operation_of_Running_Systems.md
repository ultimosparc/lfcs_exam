# Operation_of_Running_Systems

Der Linux Kernel ist eine API mit verschiedenen Entry points. Ein Entrypoint ist ein Zugang zu einem Service, der vom Kernel angeboten wird. Genauer definiert, es handelt sich um eine Methode mit Parameter, die bestimmte Werte zurückliefert. Beim Booten wird der Kernel in den Systemspace hochgeladen und bleibt dort bis das System wieder runterfährt.  
The kernel provides many services to user programs, including

         process scheduling and management,
         I/O handling,
         physical and virtual memory management,
         device management,
         Idle management,
         signaling and inter-process communication,
         multi-threading,
         multi-tasking,
         real-time signaling and scheduling, and
         networking services.
         
Network services include protocols such as HTTP, NIS, NFS, X.25, SSH, SFTP, TCP/IP, and Java.
Es gibt zwei Arten, wie ein Service angefragt werden kann. 

        1. by making a system call to a function (i.e., entry point) built directly into the kernel
        2. by calling a higher-level library (i.q. system programs) routine that makes use of this call.

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

_Limits of Processes_[31]

How to Set Limits on User Running Processes in Linux. One of the Linux’s beauties is that you can control almost everything about it. This gives a system administrator a great control over his system and better utilization of the system resources.

While some might have never thought about doing this, it is important to know that in Linux you can limit how much resource a single user may use and for how long.
In this short topic, we will show you how to limit the number of processes started by user and how to check the current limits and modify them.

Before we go any further there are two things we need to point:

You need root access to your system to modify the user limits
You must be extremely careful if you plan to modify these limits
To setup user limits, we will need to edit the following file:
/etc/security/limits.conf
This file is used to apply ulimit created by the pam_module. 

The file has the following syntax:

<domain> <type> <item> <value>
Here we will stop to discuss each of the options:

Domain – this includes usernames, groups, guid ranges etc
Type – soft and hard limits
Item – the item that will be limited – core size, file size,  nproc etc
Value – this is the value for the given limit
A good sample for a limit is:

@student          hard           nproc                20
The above line sets a hard limit of maximum 20 processes on the "student" group.

If you want to see the limits of a certain process has you can simply “cat” the limits file like this:

# cat /proc/PID/limits
Where PID is the actual process ID, you can find out process id by using ps command. For more detailed explanation, read our article that says – Find Running Linux Processes and Set Process Limits Per-User Level

So here is an example:

# cat /proc/2497/limits
Sample Output
Limit                     Soft Limit           Hard Limit           Units     
Max cpu time              unlimited            unlimited            seconds   
Max file size             unlimited            unlimited            bytes     
Max data size             unlimited            unlimited            bytes     
Max stack size            8388608              unlimited            bytes     
Max core file size        0                    unlimited            bytes     
Max resident set          unlimited            unlimited            bytes     
Max processes             32042                32042                processes 
Max open files            1024                 4096                 files     
Max locked memory         65536                65536                bytes     
Max address space         unlimited            unlimited            bytes     
Max file locks            unlimited            unlimited            locks     
Max pending signals       32042                32042                signals   
Max msgqueue size         819200               819200               bytes     
Max nice priority         0                    0                    
Max realtime priority     0                    0                    
Max realtime timeout      unlimited            unlimited            us   
All of the lines are pretty much self explanatory. However if you want to find more the settings you can input in limits.conf file, you can have a look at the manual provided here.

_Find Out Installed or Removed Packages Info_[32]

YUM is an interactive, rpm based, high level package manager, it perform queries on the installed packages and/or available packages.
It possible to view history of YUM transactions in order to find out information about installed packages and those that where removed/erased from a system.
To view a full history of YUM transactions,

        # yum history oder # yum history list all
        
Es zeigt: transaction id, login user who executed the particular action, date and time when the operation happened, the actual action and additional information about any thing wrong with the operation
You can view details of transactions concerning a given package such as httpd web server with the info command as follows:

        # yum history info httpd
      
To get a summary of the transactions concerning httpd package, we can issue the following command:

        # yum history summary httpd

It is also possible to use a transaction ID, the command below will display details of the transaction ID 15.

        # yum history info 15
   
There are sub-commands that print out transaction details of a specific package or group of packages. We can use package-list or package_info to view more info about httpd package like so:

        # yum history package-list httpd oder # yum history package-info httpd
        
 To get history about multiple packages, we can run:

# yum history package-list httpd epel-release
OR
# yum history packages-list httpd epel-release

Furthermore, there are certain history sub-commands that enable us to: undo/redo/rollback transactions.

Undo – will undo a specified transaction.
redo – repeat the work of a specified transaction
rollback – will undo all transactions up to the point of the specified transaction.
They take either a single transaction id or the keyword last and an offset from the last transaction.

For example, assuming we’ve done 60 transactions, “last” refers to transaction 60, and “last-4” points to transaction 56.

This is how the sub-commands above work: If we have 5 transactions: V, W, X, Y and Z, where packages where installed respectively.

# yum history undo 2    #will remove package W
# yum history redo 2    #will  reinstall package W
# yum history rollback 2    #will remove packages from X, Y, and Z. 
In the following example, transaction 2 was a update operation, as seen below, the redo command that follows will repeat transaction 2 upgrading all the packages updated by that time:

# yum history | grep -w "2"

 yum history redo 2
 
 The redo sub-command can also take some optional arguments before we specify a transaction:

force-reinstall – reinstalls any packages that were installed in that transaction (via yum install, upgrade or downgrade).
force-remove – removes any packages that were updated or downgraded.
# yum history redo force-reinstall 16

Find Yum History Database and Sources Info
These sub-commands provide us information about the history DB and additional info sources:

addon-info – will provide sources of additional information.
stats – displays statistics about the current history DB.
sync – enables us to alter the the rpmdb/yumdb data stored for any installed packages.
Consider the commands below to understand how these sub-commands practically work:

# yum history addon-info
# yum history stats
# yum history sync
To set a new history file, use the new sub-command:

# yum history new
We can find a complete information about YUM history command and several other commands in the yum man page:

# man yum
 
_Boot Process_[33]

![boot_porcess](https://user-images.githubusercontent.com/15387251/110189827-d30f9880-7e20-11eb-9d2b-dc42ebed3e38.png)

The boot loader loads both the kernel and an initial RAM–based file system (initramfs) into memory, so it can be used directly by the kernel.  

When the kernel is loaded in RAM, it immediately initializes and configures the computer’s memory and also configures all the hardware attached to the system. This includes all processors, I/O subsystems, storage devices, etc. The kernel also loads some necessary user space applications.
Once the kernel has set up all its hardware and mounted the root filesystem, the kernel runs /sbin/init. This then becomes the initial process, which then starts other processes to get the system running. Most other processes on the system trace their origin ultimately to init; exceptions include the so-called kernel processes. These are started by the kernel directly, and their job is to manage internal operating system details.

Besides starting the system, init is responsible for keeping the system running and for shutting it down cleanly. One of its responsibilities is to act when necessary as a manager for all non-kernel processes; it cleans up after them upon completion, and restarts user login services as needed when users log in and out, and does the same for other background system services.

Traditionally, this process startup was done using conventions that date back to the 1980s and the System V variety of UNIX. This serial process has the system passing through a sequence of runlevels containing collections of scripts that start and stop services. Each runlevel supports a different mode of running the system. Within each runlevel, individual services can be set to run, or to be shut down if running.

However, all major recent distributions have moved away from this sequential runlevel method of system initialization, although they usually support the System V conventions for compatibility purposes. Next, we discuss the newer methods, systemd and Upstart.
SysVinit viewed things as a serial process, divided into a series of sequential stages. Each stage required completion before the next could proceed. Thus, startup did not easily take advantage of the parallel processing that could be done on multiple processors or cores.

Furthermore, shutdown and reboot was seen as a relatively rare event; exactly how long it took was not considered important. This is no longer true, especially with mobile devices and embedded Linux systems. Some modern methods, such as the use of containers, can require almost instantaneous startup times. Thus, systems now require methods with faster and enhanced capabilities. Finally, the older methods required rather complicated startup scripts, which were difficult to keep universal across distribution versions, kernel versions, architectures, and types of systems. The two main alternatives developed were:

Upstart

Developed by Ubuntu and first included in 2006
Adopted in Fedora 9 (in 2008) and in RHEL 6 and its clones.
systemd

Adopted by Fedora first (in 2011)
Adopted by RHEL 7 and SUSE 
Replaced Upstart in Ubuntu 16.04
While the migration to systemd was rather controversial, it has been adopted by the major distributions, and so we will not discuss the older System V method or Upstart, which has become a dead end. Regardless of how one feels about the controversies or the technical methods of systemd, almost universal adoption has made learning how to work on Linux systems simpler, as there are fewer differences among distributions. We enumerate systemd features next.
ystems with systemd start up faster than those with earlier init methods. This is largely because it replaces a serialized set of steps with aggressive parallelization techniques, which permits multiple services to be initiated simultaneously.

Complicated startup shell scripts are replaced with simpler configuration files, which enumerate what has to be done before a service is started, how to execute service startup, and what conditions the service should indicate have been accomplished when startup is finished. One thing to note is that /sbin/init now just points to /lib/systemd/systemd; i.e. systemd takes over the init process.

One systemd command (systemctl) is used for most basic tasks. While we have not yet talked about working at the command line, here is a brief listing of its use:

Starting, stopping, restarting a service (using nfs as an example) on a currently running system:
$ sudo systemctl start|stop|restart nfs.service
Enabling or disabling a system service from starting up at system boot:
$ sudo systemctl enable|disable nfs.service
In most cases, the .service can be omitted. There are many technical differences with older methods that lie beyond the scope of our discussion.  

Process Type	Description	Example
Interactive Processes	Need to be started by a user, either at a command line or through a graphical interface such as an icon or a menu selection.	bash, firefox, top
Batch Processes	Automatic processes which are scheduled from and then disconnected from the terminal. These tasks are queued and work on a FIFO (First-In, First-Out) basis.	updatedb, ldconfig
Daemons	Server processes that run continuously. Many are launched during system startup and then wait for a user or system request indicating that their service is required.	httpd, sshd, libvirtd
Threads	Lightweight processes. These are tasks that run under the umbrella of a main process, sharing memory and other resources, but are scheduled and run by the system on an individual basis. An individual thread can end without terminating the whole process and a process can create new threads at any time. Many non-trivial programs are multi-threaded.	firefox, gnome-terminal-server
Kernel Threads	Kernel tasks that users neither start nor terminate and have little control over. These may perform actions like moving a thread from one CPU to another, or making sure input/output operations to disk are completed.	kthreadd, migration, ksoftirqd

A critical kernel function called the scheduler constantly shifts processes on and off the CPU, sharing time according to relative priority, how much time is needed and how much has already been granted to a task.

When a process is in a so-called running state, it means it is either currently executing instructions on a CPU, or is waiting to be granted a share of time (a time slice) so it can execute. All processes in this state reside on what is called a run queue and on a computer with multiple CPUs, or cores, there is a run queue on each.

However, sometimes processes go into what is called a sleep state, generally when they are waiting for something to happen before they can resume, perhaps for the user to type something. In this condition, a process is said to be sitting on a wait queue.

There are some other less frequent process states, especially when a process is terminating. Sometimes, a child process completes, but its parent process has not asked about its state. Amusingly, such a process is said to be in a zombie state; it is not really alive, but still shows up in the system's list of processes.

A critical kernel function called the scheduler constantly shifts processes on and off the CPU, sharing time according to relative priority, how much time is needed and how much has already been granted to a task.

When a process is in a so-called running state, it means it is either currently executing instructions on a CPU, or is waiting to be granted a share of time (a time slice) so it can execute. All processes in this state reside on what is called a run queue and on a computer with multiple CPUs, or cores, there is a run queue on each.

However, sometimes processes go into what is called a sleep state, generally when they are waiting for something to happen before they can resume, perhaps for the user to type something. In this condition, a process is said to be sitting on a wait queue.

There are some other less frequent process states, especially when a process is terminating. Sometimes, a child process completes, but its parent process has not asked about its state. Amusingly, such a process is said to be in a zombie state; it is not really alive, but still shows up in the system's list of processes.

At any given time, there are always multiple processes being executed. The operating system keeps track of them by assigning each a unique process ID (PID) number. The PID is used to track process state, CPU usage, memory use, precisely where resources are located in memory, and other characteristics.

New PIDs are usually assigned in ascending order as processes are born. Thus, PID 1 denotes the init process (initialization process), and succeeding processes are gradually assigned higher numbers.

The table explains the PID types and their descriptions:

 

ID Type	Description
Process ID (PID)	Unique Process ID number
Parent Process ID (PPID)	Process (Parent) that started this process. If the parent dies, the PPID will refer to an adoptive parent; on recent kernels, this is kthreadd which has PPID=2.
Thread ID (TID)	Thread ID number. This is the same as the PID for single-threaded processes. For a multi-threaded process, each thread shares the same PID, but has a unique TID.

At some point, one of your applications may stop working properly. How do you eliminate it?

To terminate a process, you can type kill -SIGKILL <pid> or kill -9 <pid>.

Note, however, you can only kill your own processes; those belonging to another user are off limits, unless you are root.

The load average is the average of the load number for a given period of time. It takes into account processes that are:

Actively running on a CPU
Considered runnable, but waiting for a CPU to become available
Sleeping: i.e. waiting for some kind of resource (typically, I/O) to become available.
Note: Linux differs from other UNIX-like operating systems in that it includes the sleeping processes. Furthermore, it only includes so-called uninterruptible sleepers, those which cannot be awakened easily.

The load average can be viewed by running w, top or uptime. We will explain the numbers on the next page.

The load average is displayed using three numbers (0.45, 0.17, and 0.12) in the below screenshot. Assuming our system is a single-CPU system, the three load average numbers are interpreted as follows:

0.45: For the last minute the system has been 45% utilized on average.
0.17: For the last 5 minutes utilization has been 17%.
0.12: For the last 15 minutes utilization has been 12%.
If we saw a value of 1.00 in the second position, that would imply that the single-CPU system was 100% utilized, on average, over the past 5 minutes; this is good if we want to fully use a system. A value over 1.00 for a single-CPU system implies that the system was over-utilized: there were more processes needing CPU than CPU was available.

If we had more than one CPU, say a quad-CPU system, we would divide the load average numbers by the number of CPUs. In this case, for example, seeing a 1 minute load average of 4.00 implies that the system as a whole was 100% (4.00/4) utilized during the last minute.

Short-term increases are usually not a problem. A high peak you see is likely a burst of activity, not a new level. For example, at start up, many processes start and then activity settles down. If a high peak is seen in the 5 and 15 minute load averages, it may be cause for concern.

 pstree displays the processes running on the system in the form of a tree diagram showing the relationship between a process and its parent process and any other processes that it created. Repeated entries of a process are not displayed, and threads are displayed in curly braces.
 
 cp can only copy files to and from destinations on the local machine (unless you are copying to or from a filesystem mounted using NFS), but rsync can also be used to copy files from one machine to another. Locations are designated in the target:path form, where target can be in the form of someone@host. The someone@ part is optional and used if the remote user is different from the local user.

rsync is very efficient when recursively copying one directory tree to another, because only the differences are transmitted over the network. One often synchronizes the destination directory tree with the origin, using the -r option to recursively walk down the directory tree copying all files and directories below the one listed as the source.

rsync is a very powerful utility. For example, a very useful way to back up a project directory might be to use the following command:

$ rsync -r project-X archive-machine:archives/project-X

Note that rsync can be very destructive! Accidental misuse can do a lot of harm to data and programs, by inadvertently copying changes to where they are not wanted. Take care to specify the correct options and paths. It is highly recommended that you first test your rsync command using the -dry-run option to ensure that it provides the results that you want.

To use rsync at the command prompt, type rsync sourcefile destinationfile, where either file can be on the local machine or on a networked machine; The contents of sourcefile will be copied to destinationfile.

A good combination of options is shown in:

$ rsync --progress -avrxH  --delete sourcedir destdir

The dd program is very useful for making copies of raw disk space. For example, to back up your Master Boot Record (MBR) (the first 512-byte sector on the disk that contains a table describing the partitions on that disk), you might type:

dd if=/dev/sda of=sda.mbr bs=512 count=1

WARNING!

Typing:

dd if=/dev/sda of=/dev/sdb

to make a copy of one disk onto another, will delete everything that previously existed on the second disk.

An exact copy of the first disk device is created on the second disk device.

The standard prescription is that when you first login to Linux, /etc/profile is read and evaluated, after which the following files are searched (if they exist) in the listed order:

~/.bash_profile
~/.bash_login
~/.profile 
where  ~/. denotes the user's home directory. The Linux login shell evaluates whatever startup file that it comes across first and ignores the rest. This means that if it finds ~/.bash_profile, it ignores ~/.bash_login and ~/.profile. Different distributions may use different startup files. 

However, every time you create a new shell, or terminal window, etc., you do not perform a full system login; only a file named ~/.bashrc file is read and evaluated. Although this file is not read and evaluated along with the login shell, most distributions and/or users include the ~/.bashrc file from within one of the three user-owned startup files.

Most commonly, users only fiddle with ~/.bashrc, as it is invoked every time a new command line shell initiates, or another program is launched from a terminal window, while the other files are read and executed only when the user first logs onto the system.

Recent distributions sometimes do not even have .bash_profile  and/or .bash_login , and some just do little more than include .bashrc.

 

Order of the Startup Files

Order of the Startup Files
Environment variables are quantities that have specific values which may be utilized by the command shell, such as bash, or other utilities and applications. Some environment variables are given preset values by the system (which can usually be overridden), while others are set directly by the user, either at the command line or within startup and other scripts. 

An environment variable is actually just a character string that contains information used by one or more applications. There are a number of ways to view the values of currently set environment variables; one can type set, env, or export. Depending on the state of your system, set may print out many more lines than the other two methods.

By default, variables created within a script are only available to the current shell; child processes (sub-shells) will not have access to values that have been set or modified. Allowing child processes to see the values requires use of the export command.

 

Task	Command
Show the value of a specific variable	echo $SHELL
Export a new variable value	export VARIABLE=value (or VARIABLE=value; export VARIABLE)
Add a variable permanently	
Edit ~/.bashrc and add the line export VARIABLE=value
Type source ~/.bashrc or just . ~/.bashrc (dot ~/.bashrc); or just start a new shell by typing  bash
 

You can also set environment variables to be fed as a one shot to a command as in:

$ SDIRS=s_0* KROOT=/lib/modules/$(uname -r)/build make modules_install

which feeds the values of the SDIRS and KROOT environment variables to the command make modules_install.

Prompt Statement (PS) is used to customize your prompt string in your terminal windows to display the information you want. 

PS1 is the primary prompt variable which controls what your command line prompt looks like. The following special characters can be included in PS1:

\u - User name
\h - Host name
\w - Current working directory
\! - History number of this command
\d - Date

They must be surrounded in single quotes when they are used, as in the following example:

$ echo $PS1
$
$ export PS1='\u@\h:\w$ '
student@example.com:~$ # new prompt

To revert the changes:

student@example.com:~$ export PS1='$ '
$

An even better practice would be to save the old prompt first and then restore, as in:

$ OLD_PS1=$PS1

change the prompt, and eventually change it back with:

$ PS1=$OLD_PS1

bash keeps track of previously entered commands and statements in a history buffer. You can recall previously used commands simply by using the Up and Down cursor keys. To view the list of previously executed commands, you can just type history at the command line.

The list of commands is displayed with the most recent command appearing last in the list. This information is stored in ~/.bash_history. If you have multiple terminals open, the commands typed in each session are not saved until the session terminates.

Several associated environment variables can be used to get information about the history file. 

HISTFILE
The location of the history file. 
HISTFILESIZE
The maximum number of lines in the history file (default 500). 
HISTSIZE 
The maximum number of commands in the history file. 
HISTCONTROL
How commands are stored. 
HISTIGNORE
Which command lines can be unsaved.
For a complete description of the use of these environment variables, see man bash.

Up/Down arrow keys	Browse through the list of commands previously executed
!! (Pronounced as bang-bang)	Execute the previous command
CTRL-R	Search previously used commands
 

If you want to recall a command in the history list, but do not want to press the arrow key repeatedly, you can press CTRL-R to do a reverse intelligent search.

As you start typing, the search goes back in reverse order to the first command that matches the letters you have typed. By typing more successive letters, you make the match more and more specific.

The following is an example of how you can use the CTRL-R command to search through the command history:

$ ^R                                                                      (This all happens on 1 line)
(reverse-i-search)'s': sleep 1000   (Searched for 's'; matched "sleep")
$ sleep 1000                                                   (Pressed Enter to execute the searched command)
$

The table describes the syntax used to execute previously used commands:

 

Syntax	Task
!	Start a history substitution
!$	Refer to the last argument in a line
!n	Refer to the nth command line
!string	Refer to the most recent command starting with string
 

All history substitutions start with !. When typing the command: ls -l /bin /etc will refer to /var, the last argument to the command.

Here are more examples:

$ history

echo $SHELL
echo $HOME
echo $PS1
ls -a
ls -l /etc/ passwd
sleep 1000
history
$ !1                                    (Execute command #1 above)
echo $SHELL
/bin/bash
$ !sl                           (Execute the command beginning with "sl")
sleep 1000
$

The Linux kernel allows properly authenticated users to access files and applications. While each user is identified by a unique integer (the user id or UID), a separate database associates a username with each UID. Upon account creation, new user information is added to the user database and the user's home directory must be created and populated with some essential files. Command line programs such as useradd and userdel as well as GUI tools are used for creating and removing accounts. 

For each user, the following seven fields are maintained in the /etc/passwd file: 

 

Field Name	Details	Remarks
Username	User login name	Should be between 1 and 32 characters long
Password	User password (or the character x if the password is stored in the /etc/shadow file) in encrypted format	Is never shown in Linux when it is being typed; this stops prying eyes
User ID (UID)	Every user must have a user id (UID)	
UID 0 is reserved for root user
UID's ranging from 1-99 are reserved for other predefined accounts
UID's ranging from 100-999 are reserved for system accounts and groups
Normal users have UID's of 1000 or greater
Group ID (GID)	The primary Group ID (GID); Group Identification Number stored in the /etc/group file	Is covered in detail in the chapter on Processes
User Info	This field is optional and allows insertion of extra information about the user such as their name	For example: Rufus T. Firefly
Home Directory	The absolute path location of user's home directory	For example: /home/rtfirefly
Shell	The absolute location of a user's default shell 	For example: /bin/bash

By default, Linux distinguishes between several account types in order to isolate processes and workloads. Linux has four types of accounts:

root
System
Normal
Network
For a safe working environment, it is advised to grant the minimum privileges possible and necessary to accounts, and remove inactive accounts. The last utility, which shows the last time each user logged into the system, can be used to help identify potentially inactive accounts which are candidates for system removal.

Keep in mind that practices you use on multi-user business systems are more strict than practices you can use on personal desktop systems that only affect the casual user. This is especially true with security. We hope to show you practices applicable to enterprise servers that you can use on all systems, but understand that you may choose to relax these rules on your own personal system.

root is the most privileged account on a Linux/UNIX system. This account has the ability to carry out all facets of system administration, including adding accounts, changing user passwords, examining log files, installing software, etc. Utmost care must be taken when using this account. It has no security restrictions imposed upon it.

When you are signed in as, or acting as root, the shell prompt displays '#' (if you are using bash and you have not customized the prompt, as we have discussed previously). This convention is intended to serve as a warning to you of the absolute power of this account.

root privileges are required to perform operations such as:

Creating, removing and managing user accounts
Managing software packages
Removing or modifying system files
Restarting system services.
Regular account users of Linux distributions might be allowed to install software packages, update some settings, use some peripheral devices, and apply various kinds of changes to the system. However, root privilege is required for performing administration tasks such as restarting most services, manually installing packages and managing parts of the filesystem that are outside the normal user’s directories.

A regular account user can perform some operations requiring special permissions; however, the system configuration must allow such abilities to be exercised.

SUID (Set owner User ID upon execution - similar to the Windows "run as" feature) is a special kind of file permission given to a file. Use of SUID provides temporary permissions to a user to run a program with the permissions of the file owner (which may be root) instead of the permissions held by the user.

The table provides examples of operations which do not require root privileges:

 

Operations that do not require Root privilege	Examples of this operation
Running a network client	Sharing a file over the network
Using devices such as printers	Printing over the network
Operations on files that the user has proper permissions to access	Accessing files that you have access to or sharing data over the network
Running SUID-root applications	Executing programs such as passwd

In Linux you can use either su or sudo to temporarily grant root access to a normal user. However, these methods are actually quite different. Listed below are the differences between the two commands:

 

su	sudo
When elevating privilege, you need to enter the root password. Giving the root password to a normal user should never, ever be done.	When elevating privilege, you need to enter the user’s password and not the root password.
Once a user elevates to the root account using su, the user can do anything that the root user can do for as long as the user wants, without being asked again for a password. 	Offers more features and is considered more secure and more configurable. Exactly what the user is allowed to do can be precisely controlled and limited. By default the user will either always have to keep giving their password to do further operations with sudo, or can avoid doing so for a configurable time interval.
The command has limited logging features.	The command has detailed logging features.

sudo has the ability to keep track of unsuccessful attempts at gaining root access. Users' authorization for using sudo is based on configuration information stored in the /etc/sudoers file and in the /etc/sudoers.d directory.

A message such as the following would appear in a system log file (usually /var/log/secure) when trying to execute sudo bash without successfully authenticating the user:

authentication failure; logname=op uid=0 euid=0 tty=/dev/pts/6 ruser=op rhost= user=op
conversation failed
auth could not identify password for [op]
op : 1 incorrect password attempt ;
TTY=pts/6 ; PWD=/var/log ; USER=root ; COMMAND=/bin/bash

Whenever sudo is invoked, a trigger will look at /etc/sudoers and the files in /etc/sudoers.d to determine if the user has the right to use sudo and what the scope of their privilege is. Unknown user requests and requests to do operations not allowed to the user even with sudo are reported. The basic structure of entries in these files is:

who where = (as_whom) what

/etc/sudoers contains a lot of documentation in it about how to customize. Most Linux distributions now prefer you add a file in the directory /etc/sudoers.d with a name the same as the user. This file contains the individual user's sudo configuration, and one should leave the master configuration file untouched except for changes that affect all users.

You should edit any of these configuration files by using visudo, which ensures that only one person is editing the file at a time, has the proper permissions, and refuses to write out the file and exit if there are syntax errors in the changes made. The editing can be accomplished by doing a command such as the following ones:

# visudo /etc/sudoers
# visudo -f /etc/sudoers.d/student

The actual specific editor invoked will depend on the setting of your EDITOR environment variable.

By default, sudo commands and any failures are logged in /var/log/auth.log under the Debian distribution family, and in /var/log/messages and/or /var/log/secure on other systems. This is an important safeguard to allow for tracking and accountability of sudo use. A typical entry of the message contains:

Calling username
Terminal info
Working directory
User account invoked
Command with arguments.
Running a command such as sudo whoami results in a log file entry such as:

Dec 8 14:20:47 server1 sudo: op : TTY=pts/6 PWD=/var/log USER=root COMMAND=/usr/bin/whoami

Linux is considered to be more secure than many other operating systems because processes are naturally isolated from each other. One process normally cannot access the resources of another process, even when that process is running with the same user privileges. Linux thus makes it difficult (though certainly not impossible) for viruses and security exploits to access and attack random resources on a system.

More recent additional security mechanisms that limit risks even further include:

Control Groups (cgroups)
Allows system administrators to group processes and associate finite resources to each cgroup.
Containers
Makes it possible to run multiple isolated Linux systems (containers) on a single system by relying on cgroups.
Virtualization
Hardware is emulated in such a way that not only processes can be isolated, but entire systems are run simultaneously as isolated and insulated guests (virtual machines) on one physical host.

Linux limits user access to non-networking hardware devices in a manner that is extremely similar to regular file access. Applications interact by engaging the filesystem layer (which is independent of the actual device or hardware the file resides on). This layer will then open a device special file (often called a device node) under the /dev directory that corresponds to the device being accessed. Each device special file has standard owner, group and world permission fields. Security is naturally enforced just as it is when standard files are accessed.

Hard disks, for example, are represented as /dev/sd*. While a root user can read and write to the disk in a raw fashion, for example, by doing something like:

 # echo hello world > /dev/sda1

The standard permissions as shown in the figure, make it impossible for regular users to do so. Writing to a device in this fashion can easily obliterate the filesystem stored on it in a way that cannot be repaired without great effort, if at all. The normal reading and writing of files on the hard disk by applications is done at a higher level through the filesystem, and never through direct access to the device node.

When security problems in either the Linux kernel or applications and libraries are discovered, Linux distributions have a good record of reacting quickly and pushing out fixes to all systems by updating their software repositories and sending notifications to update immediately. The same thing is true with bug fixes and performance improvements that are not security related.

However, it is well known that many systems do not get updated frequently enough and problems which have already been cured are allowed to remain on computers for a long time; this is particularly true with proprietary operating systems where users are either uninformed or distrustful of the vendor's patching policy as sometimes updates can cause new problems and break existing operations. Many of the most successful attack vectors come from exploiting security holes for which fixes are already known but not universally deployed.

So the best practice is to take advantage of your Linux distribution's mechanism for automatic updates and never postpone them. It is extremely rare that such an update will cause new problems.

Protecting passwords has become a crucial element of security. Most Linux distributions rely on a modern password encryption algorithm called SHA-512 (Secure Hashing Algorithm 512 bits), developed by the U.S. National Security Agency (NSA) to encrypt passwords.

The SHA-512 algorithm is widely used for security applications and protocols. These security applications and protocols include TLS, SSL, PHP, SSH, S/MIME and IPSec. SHA-512 is one of the most tested hashing algorithms.

For example, if you wish to experiment with SHA-512 encoding, the word "test" can be encoded using the program sha512sum to produce the SHA-512 form (see graphic):

T professionals follow several good practices for securing the data and the password of every user.

Password aging is a method to ensure that users get prompts that remind them to create a new password after a specific period. This can ensure that passwords, if cracked, will only be usable for a limited amount of time. This feature is implemented using chage, which configures the password expiry information for a user.
Another method is to force users to set strong passwords using Pluggable Authentication Modules (PAM). PAM can be configured to automatically verify that a password created or modified using the passwd utility is sufficiently strong. PAM configuration is implemented using a library called pam_cracklib.so, which can also be replaced by pam_passwdqc.so to take advantage of more options.
One can also install password cracking programs, such as John The Ripper, to secure the password file and detect weak password entries. It is recommended that written authorization be obtained before installing such tools on any system that you do not own.
 
 You can secure the boot process with a secure password to prevent someone from bypassing the user authentication step. This can work in conjunction with password protection for the BIOS. Note that while using a bootloader password alone will stop a user from editing the bootloader configuration during the boot process, it will not prevent a user from booting from an alternative boot media such as optical disks or pen drives. Thus, it should be used with a BIOS password for full protection.

For the older GRUB 1 boot method, it was relatively easy to set a password for grub. However, for the GRUB 2 version, things became more complicated. However, you have more flexibility, and can take advantage of more advanced features, such as user-specific passwords (which can be their normal login ones.)

Furthermore, you never edit grub.cfg directly; instead, you can modify the configuration files in /etc/grub.d and /etc/defaults/grub, and then run update-grub, or grub2-mkconfig and save the new configuration file.

To learn more, read the following post: "GRUB 2 Password Protection".

When hardware is physically accessible, security can be compromised by:

Key logging
Recording the real time activity of a computer user including the keys they press. The captured data can either be stored locally or transmitted to remote machines.
Network sniffing
Capturing and viewing the network packet level data on your network.
Booting with a live or rescue disk
Remounting and modifying disk content.
Your IT security policy should start with requirements on how to properly secure physical access to servers and workstations. Physical access to a system makes it possible for attackers to easily leverage several attack vectors, in a way that makes all operating system level recommendations irrelevant.

The guidelines of security are:

Lock down workstations and servers.
Protect your network links such that it cannot be accessed by people you do not trust.
Protect your keyboards where passwords are entered to ensure the keyboards cannot be tampered with.
Ensure a password protects the BIOS in such a way that the system cannot be booted with a live or rescue DVD or USB key.
For single user computers and those in a home environment some of the above features (like preventing booting from removable media) can be excessive, and you can avoid implementing them. However, if sensitive information is on your system that requires careful protection, either it shouldn't be there or it should be better protected by following the above guidelines.

Like all software, hackers occasionally find weaknesses in the Linux ecosystem. The strength of the Linux (and open source community in general) is the speed with which such vulnerabilities are exposed and remediated. While specific discussion of vulnerabilities is beyond the scope of this course, we have seeded a couple relevant topics to the Discussion Board (under the "Local Security" topic) to encourage further discussion.


