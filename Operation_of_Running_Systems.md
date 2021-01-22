# Operation_of_Running_Systems

    Locate and analyse system log files and manipulate it
    Limit the number of user processes for a specific account or group
    Install/update packages, get information about installed packages (depends on your distribution)
    Schedule tasks to run at a set date and time (using cron)
    Use scripting to automate system maintenance tasks
    Diagnose and manage processes (find process ID for example)



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


 _Ausführung von Jobs auf einem Linux-System_ [6,7,8]


Das Programm "echo" schreibt beispielsweise eine Nachricht in die Standardausgabe "stdout". Führt man das Programm "echo" aus, dann generiert man
einen Prozess. D.h. die Bezeichnung Prozess steht für "ausführbares Programm". Wird nun der Befehl "echo test &" im Terminal eingegeben, 
dann startet man den Prozess im Vordergrund und schiebt den Prozess in den Hintergrund vom Terminal. Das bedeutet, 
während der Ausführungszeit des Prozesses bleibt das Terminal "ansprechbar", d.h. es können weitere Befehle eingeben werden. 
Prozesse im Hintergrund nennt man Job. Jobs haben wie Prozesse einen Parentprozess. Jobs, dessen Parentprozess den "init-Prozess" ist, nennt man "Demoans". 
Bei allen Begriffen, die oben genannt werden, handelt es sich um abstrakte Begriffe, die von einander abhängen. Die folgende Darstellung illustriert noch einmal
diese Abhängigkeiten: 
    
    File (sourcecode) ->  program (executable file) ->  

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
