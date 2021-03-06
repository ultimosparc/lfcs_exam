# Service Configuration and Virtualization


https://www.linux.com/certifications/tecmints-10-lessons-prep-linux-foundation-sysadmin-certification/

https://ibm-learning.udemy.com/course/redhat-linux-administration-ll/learn/lecture/16499752#overview

ethods of bringing down the GUI:
student:/tmp> sudo systemctl stop gdm
student:/tmp> sudo systemctl stop lightdm
student:/tmp> sudo telinit 3
      
Methods of bringing the GUI back up:
student:/tmp> sudo systemctl start gdm
student:/tmp> sudo systemctl start lightdm
student:/tmp> sudo telinit 5


    Manage and configure containers (you’d better know all common and widely used docker commands for this exam)
    Manage Virtual Machines (using virsh)
    Configure SSH servers and clients
    Configure an HTTP server
    Restrict access to a web page
    Configure a database serverhhhhhhhhhhhhhhhhhhhhhhhhhh



-------------------
Bootloader --> Kernel --> Init --> Services (httpd, ftpd, dhcpd usw) sind damoens auch genannt Jobs im Background laufen

If the display manager is not started by default in the default runlevel, you can start the graphical desktop different way, after logging on to a text-mode console, by running startx from the command line. Or, you can start the display manager (gdm, lightdm, kdm, xdm, etc.) manually from the command line. This differs from running startx as the display managers will project a sign in screen. We discuss them next.



