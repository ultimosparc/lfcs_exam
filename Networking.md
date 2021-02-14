# Networking 

_Konfiguration des Hostname_ [19,22]

Es kann sein, das in der Prüfung eine Aufgabe gibt den Hostname zu ändern. Es gibt zwei Möglichkeiten, dynamisch und statisch. Dynamisch kann der Hostname eines laufenden Systems geändert werden und statisch bedeutet, dass der Hostname permanent geändert wird, das nach Reboot das System immer noch den neuen Hostnamen hat. 
Ein System hat drei verschiedene Hostnames. The high-level "pretty" hostname which might include all kinds of special characters (e.g. "Lennart's Laptop"), the static hostname which is used to initialize the kernel hostname at boot (e.g. "lennarts-laptop"), and the transient hostname which is a fallback value received from network configuration. If a static hostname is set, and is valid (something other than localhost), then the transient hostname is not used. 
Note that the pretty hostname has little restrictions on the characters and length used, while the static and transient hostnames are limited to the usually accepted characters of Internet domain names, and 64 characters at maximum (the latter being a Linux limitation).
In DNS, hostnames are appended with a dot and domain name to qualify FQDN, e.g. debian.karakays.com
Statische und transient hostname unterliegen den Internet Regel bei der Namensgebung, also werden diese genommen um in Netzwerken die rechner bezeichnet
Es gibt verschiedene Befehle zum Bearbeiten des Hostnames. Grundsätzlich aber kann der Befehl honstnamectl verwendet werden. Angenommen es soll der hostname gesetzt werden, dann kann der Befehl wie folgt aussehen: 

    hostnamectl set-hostname "test" --pretty //--static -- transient 
    
aussehen. Grafische Oberflächen benutzten manchmal den Icon-Hostname, es kann zudem der Deployment-Name der Umgebung wie Staging, Production, Developing usw gesetzt werden mit selben Befehl gesetzt werden. 
Der --pretty Hostname ist der Name, der im Terminal gesehen werden kann, wenn man sich über ssh anmeldet. Der --static Hostname ist der DNS Name und unterliegt somit dem DNS Namensregeln. Der --transient Hostname ist ein Backup Name, falls der statische Name nicht gesetzt ist oder nicht als DNS Name akzepziert wird. Auslesen kann man einen Hostname wie folgt: 

    hostnamectl --static //--pretty --transient
    
Um permanent den Hostname zu ändern, muss der Eintrag der Datei /etc/hostname entsprechend angepasst werden. Als Alternative kann mit nmtui der Hostname geändert werden. 


_Configure network services_ [19,22,23]

packet filtering (iptables, no firewalld since it’s not related to RHEL)
      
festlegen von statischen Ips. DNS, FQDN und Gateways wäre möglich
   ip as / nmtui   
   
to start automatically at boot (using systemctl   

Start, stop, and check the status of network services

Stellt sich die Frage, was ist die private IP Adresse und was ist die öffentliche IP Adresse
Es zeigt sich, das Virtuelle Maschine die Bezeichnung eth0 für die Netzwerkkarte verwenden, dies dann private /interne IPaddresse zugeordnert bekommen, diese Docker und AWS. Tune2 ist dann die öffentliche IP Adresse
    
