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


_Network Management_ [19,22,23,24]

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
Die Steuerung des NetzwerkManager erfolgt über die Befehle 

    systemctl start NetworkManager.service
    systemctl enable NetworkManager.service
    systemctl stop NetworkManager.service
    systemctl disable NetworkManager.service

Ein Netzwerkinterface kann über die Befehle 

    ifup <interfacename like eth0>      //gestartet werden
    ifdown <interfacename like eth0>    //deaktiviert werden
    
Wenn das Interface deaktiviert ist, dann ist das senden und emfpagne von Paketen nicht mehr möglich. 
Möchete man Eigenschaften 

NOTE: IP must be inserted with syntax IP/NETMASK (e.t. 192.168.0.2/24)


If there is need to change IP configuration of an interface without using nmtui remember to shutdown interface, change IP, restart interface

    ip link set eth0 down Shutdown interface eth0
    ip addr add 192.168.0.2/24 dev eth0 Assign IP 192.168.0.2/24 to interface eth0
    ip link set eth0 up Restart interface eth0

In /etc/hosts is configured a name resolution that take precedence of DNS

    It contains static DNS entry

    It is possible add hostname to row for 127.0.0.1 resolution, or insert a static IP configured on principal interface equal to hostname

In /etc/resolv.conf there are configured DNS servers entry
It is possible to insert more than one nameserver as backup (primary and secondary)
packet filtering (iptables, no firewalld since it’s not related to RHEL)

Configurations
file 	purpose
id_rsa.pub 	default public key
id_rsa 	default private ky
authorized_keys 	list of identities who can login in this host
known_hosts 	trusted hosts (public keys)
config 	ssh client config

ssh-keygen is to generate identity keys.


festlegen von statischen Ips. DNS, FQDN und Gateways wäre möglich
   ip as / nmtui   
   
to start automatically at boot (using systemctl   

Start, stop, and check the status of network services

rerouting pakete an eine neue Ipadresse die man erstellt hat

_Implement packet filtering_ 

![2021-02-14 15_41_05-lfcs_Networking md at master · ultimosparc_lfcs](https://user-images.githubusercontent.com/15387251/107879777-3457e200-6edb-11eb-929a-31bb0fae24e9.png)

    The firewall is managed by Kernel

    The kernel firewall functionality is Netfilter

    Netfilter will process information that will enter and will exit from system
        For this it has two tables of rules called chains:
            INPUT that contains rules applied to packets that enter in the system
            OUTPUT that contains rules applied to packets that leave the system

    Another chain can be used if system is configured as router: FORWARD

    Finally there are other two chains: PREROUTING, POSTROUTING
    
    

    Picture show the order with which the various chains are valued. The arrows indicate the route of the packages:
        Incoming packets are generated from the outside
        Outgoing packets are either generated by an application or are packets in transit

    The rules inside chains are evaluated in an orderly way.
        When a rule match the other rules are skipped
        If no rules match, default policy will be applied
            Default policy:
                ACCEPT: the packet will be accepted and it will continue its path through the chains
                DROP: the packet will be rejected

    The utility to manage firewall is iptables

    iptables will create rules for chains that will be processed in an orderly way

    firewalld is a service that use iptables to manage firewalls rules

    firewall-cmd is the command to manage firewalld

Firewalld

    firewalld is enabled by default in CentOS

    It works with zone, public is default zone

    The zone is applied to an interface
        The idea is that we can have safe zone, e.g. bound to an internal interface, and unsafe zone, e.g. bound to external interfaces internet facing

    firewall-cmd --list-all show current configuration
        services -> service that are allowed to use interface
        ports -> ports that are allowed to use interface

    firewall-cmd --get-services shows the list of default services
        The services are configured in /urs/lib/firewalld/services
        /urs/lib/firewalld/services contains xml file with service configuration

    firewall-cmd --add-service service add service to current configuration
        NOTE: it isn't a permanent configuration

    firewall-cmd --reload reload firewalld configuration
        NOTE: If a service was added with previous command now it is disappeared

    firewall-cmd --add-service service --permanent add service to configuration as permanent
        NOTE: Now if firewalld configuration is reloaded service it is still present

    firewall-cmd --add-port 4000-4005/tcp Open TCP ports from 4000 to 4005

    firewall-cmd --permanent --direct --add-rule ipv4 filter OUTPUT 1 -p tcp -m tcp --dport 80 -j ACCEPT
        Add a firewall rule using iptables syntax
        This add permanently a rule as first in OUTPUT chain to allow connections to TCP destination port 80

iptables

    The firewalld daemon can be substitute with iptables daemon (the configuration that was in place until recently)
        systemctl stop firewalld
        iptables -L
            More verbose output iptables -L -v
            Show configuration of iptables chains
            Note that policies is set equal to ACCEPT for every chain. This means that no package will be rejected. This is equal to have a shut downed firewall
        systemctl disable firewalld
        yum -y install iptables-services
        systemctl enable iptables

    With this configuration rules must be inserted

    iptables -P INPUT DROP
        Set default policy to DROP for INPUT chain

    iptables rules syntax:
        iptables {-A|I} chain [-i/o interface][-s/d ipaddres] [-p tcp|upd|icmp [--dport|--sport nn…]] -j [LOG|ACCEPT|DROP|REJECTED]
        {-A|I} chain
            -A append as last rule
            -I insert. This require a number after chain that indicate rule position
        [-i/o interface]
            E.g. -i eth0 - the package is received (input) on the interface eth0
        [-s/d ipaddres]
            -s Source address. ipaddres can be an address or a subnet
            -d Destination address. ipaddres can be an address or a subnet
        [-p tcp|upd|icmp [--dport|--sport nn…]]
            -p protocol
            --dport Destination port
            --sport Source port
        -j [LOG|ACCEPT|DROP|REJECTED]
            ACCEPT accept packet
            DROP silently rejected
            REJECTED reject the packet with an ICMP error packet
            LOG log packet. Evaluation of rules isn't blocked.

    E.g.

    iptables -A INPUT -i lo -j ACCEPT
        Accept all inbound loopback traffic

    iptables -A OUTPUT -o lo -j ACCEPT
        Accept all outbound loopback traffic

    iptable -A INPUT -p tcp --dport 22 -j ACCEPT
        Accept all inbound traffic for tcp port 22

    iptable -A OUTPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
        This is a rule that is used to ACCEPT all traffic generated as a response of an inbound connection that was accepted. E.g. if incoming traffic for web server on port 80 was accepted, this rule permits to response traffic to exit from system without inserting specific rules in OUTPUT chain

    NOTE file /etc/services contains a list of well know ports with services name
    
    


    Network services are controlled as other daemon with systemctl command
        systemctl status servicename

    With netstat is it possible list internet port opened by a process
        yum -y install net-tools
        netstat -tln
            Show TCP port opened by processes

Statically route IP traffic

    ip route show
        Print route
        Alternative command route -n

    ip route add 192.0.2.1 via 10.0.0.1 [dev interface]
        Add route to 192.0.2.1 through 10.0.0.1. Optionally interface can be specified

    To make route persistent, create a route-ifname file for the interface through which the subnet is accessed, e.g eth0:
        vi /etc/sysconfig/network-scripts/route-eth0
        Add line 192.0.2.1 via 10.0.0.101 dev eth0
        service network restart to reload file

    ip route add 192.0.2.0/24 via 10.0.0.1 [dev ifname]
        Add a route to subnet 192.0.2.0/24

    To configure system as route forward must be enabled
        echo 1 > /proc/sys/net/ipv4/ip_forward
        To make configuration persistent
            echo net.ipv4.ip_forward = 1 > /etc/sysctl.d/ipv4.conf
Synchronize time using other network peers

    In time synchronization the concept of Stratum define the accuracy of server time.
    A server with Stratum 0 it is the most reliable
    A server synchronized with a Stratum 0 become Stratum 1
    Stratum 10 is reserved for local clock. This means that it is not utilizable
    The upper limit for Stratum is 15
    Stratum 16 is used to indicate that a device is unsynchronized
    Remember that time synchronization between servers is a slowly process

CHRONYD

    Default mechanism to synchronize time in CentOS

    Configuration file /etc/chrony.conf

    server parameters are servers that are used as source of synchronization

    chronyc sources contact server and show them status

    chronyc tracking show current status of system clock

    NOTE: if some of the commands below doesn't work please refer to this bug https://bugzilla.redhat.com/show_bug.cgi?id=1574418
        Simple solution: setenforce 0
        Package selinux-policy-3.13.1-229 should resolve problem

NTP

    The old method of synchronization. To enable it Chronyd must be disabled
    Configuration file /etc/ntp.conf
    server parameters are servers that are used as source of synchronization
    ntpq -p check current status of synchronization

    
