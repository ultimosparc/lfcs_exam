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


