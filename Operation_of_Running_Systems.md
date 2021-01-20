# Operation_of_Running_Systems

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
und in den Background gepackt, z.B. mit _programm &_, dann 

he cron daemon on Linux runs tasks in the background at specific times; it’s like the Task Scheduler on Windows. Add tasks to your system’s crontab files using the appropriate syntax and cron will automatically run them for you.

Crontab files can be used to automate backups, system maintenance and other repetitive tasks. The syntax is powerful and flexible, so you can have a task run every fifteen minutes or at a specific minute on a specific day every year.
