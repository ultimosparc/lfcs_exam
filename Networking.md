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


_Configure network services_ [19,22,23,24]

Ein Computer mit einer Netzwerkkarte (Netzwerk-Interface) besitzt regulär zwei IP Adressen, IP4 und IP6. Um sich die IP Adresse einmal anzuzeigen gibt es verschiedene Möglichkeiten: ip, ifconfig, nmcli usw.. Eine einfache Möglichkeit wäre wie folgt: 

    ip a s 

Die Ausgabe könnte wie folgt aussehen: 

    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
    2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 9001 qdisc fq_codel state UP group default qlen 1000
    link/ether 0a:0b:7c:65:5a:ee brd ff:ff:ff:ff:ff:ff
    inet 172.31.15.74/20 brd 172.31.15.255 scope global dynamic noprefixroute eth0
       valid_lft 2143sec preferred_lft 2143sec
    inet6 fe80::80b:7cff:fe65:5aee/64 scope link
       valid_lft forever preferred_lft forever

Es werden zwei Geräte angezeigt. lo und eth0. Das erste Geräte ist die virtuelle Netzwerkkarte "loopback", die es ermöglicht, auf den maschineninternen Server "localhost" zuzugreifen. inet steht für ip4 und inet6 für ip6. 

    In der Datei /etc/localhost stehen die Adressen des localhost, womit dieser im Browser angesprochen werden kann. 

The loopback device is a special, virtual network interface that your computer uses to communicate with itself. It is used mainly for diagnostics and troubleshooting, and to connect to servers running on the local machine. The Purpose of Loopback: when a network interface is disconnected--for example, when an Ethernet port is unplugged or Wi-Fi is turned off or not associated with an access point--no communication on that interface is possible, not even communication between your computer and itself. The loopback interface does not represent any actual hardware, but exists so applications running on your computer can always connect to servers on the same machine. This is important for troubleshooting (it can be compared to looking in a mirror). The loopback device is sometimes explained as purely a diagnostic tool. But it is also helpful when a server offering a resource you need is running on your own machine. For example, if you run a web server, you have all your web documents and could examine them file by file. You may be able to load the files in your browser too, though with server-side active content, it won't work the way it does when someone accesses it normally.So if you want to experience the same site others do, the best course is usually to connect to your own server. The loopback interface facilitates that. Addresses on Loopback: For IPv4, the loopback interface is assigned all the IPs in the 127.0.0.0/8 address block. That is, 127.0.0.1 through 127.255.255.254 all represent your computer. For most purposes, though, it is only necessary to use one IP address, and that is 127.0.0.1. This IP has the hostname of localhost mapped to it.
Der zweite Eintrag  eth0 steht für die physikalische Netzwerkkarte, die vom Rechner benutzt wird, um auf ein externes Netzwerk zuzugreifen. Bemerkung: Die dargestellte IP Adresse ist nur im internen Netzwerk sichtbar, will man über das Internet, außerhalb des Netzwerkes, auf den Rechner zugreifen, dann wird der Rechner eine andere IP Adresse zugeordnet haben. 
Man sieht also immer mindestens zwei Netzwerkinterfaces, wenn man mit dem Internet verbunden ist. Netzwerkinterfaces in einem Linuxsystem werden über Dateien, die im Verzeichnis /etc/sysconfig/network-scripts/ liegen, konfiguriert. D.h. Jedes Interface hat eine eigene Datei dort liegen. Es gibt verschiedene Wege eine solche Datei zu manipulieren. Entweder über den Texteditor direkt in der Datei oder über ein Userinterface wie nmtui bzw. nmcli. Anfragen über die Userinterfaces werden vom internen Netzwerkmanager verwaltet. D.h. wenn der Netzwerkmanager deaktiviert ist, dann funktionieren die Userinterfaces nicht mehr, aber man hat immer noch Zugriff auf das Internet. 


packet filtering (iptables, no firewalld since it’s not related to RHEL)
      
festlegen von statischen Ips. DNS, FQDN und Gateways wäre möglich
   ip as / nmtui   
   
to start automatically at boot (using systemctl   

Start, stop, and check the status of network services

rerouting pakete an eine neue Ipadresse die man erstellt hat

Stellt sich die Frage, was ist die private IP Adresse und was ist die öffentliche IP Adresse
Es zeigt sich, das Virtuelle Maschine die Bezeichnung eth0 für die Netzwerkkarte verwenden, dies dann private /interne IPaddresse zugeordnert bekommen, diese Docker und AWS. Tune2 ist dann die öffentliche IP Adresse
    
