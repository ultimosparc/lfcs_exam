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

_Virtualization_[19,34]

Wie werden Containertechnologie ermöglicht? 

      - cgroups für Resourcen Management
      - Namespaces for process isolation
      - SeLinux and SeComp for Secure boundaries

Containers werden mit sogenannten Container Images erstellt. Diese sind Packete die zusammen mit allen Abhängigkeiten einer Anwendung enthalten wie

      System Archive
      Programmiersprachen runtimes
      Programmiersprache libs
      Konfigurations Einstellen
      Statische Daten
      
Images sollen unveränderbar und nicht vererbar sein. Zum managen von containers gibt es Programme wie podman, skopeo und Buildah. diese können genutet werden um OCI Conatiner konforme wie Docker container zu managen. 
Container registry ist eine Repo zum speichern und abrufen von container images. You can pull und pshed  containers auf dieses Repo. Z.B. regristry.redhat.com

Virtualization is about replacement of physical resource by an abstraction layer. In terms of virtualizing an entire OS, it's a virtual machine (VM) running under a hypervisor. A VM is a virtualized instance of an entire OS to create an isolated environment.

Rationale
Efficient use of hardware
Software isolation
Isolated development environment
Run multiple OS without rebooting
Other types of virtualization

Network - Software Defined Networking.
Storage - Network Attached Storage.
Application - Container
Terms
Host is the physical OS managing one or more VMs. Guest is the VM that is an instance of a complete OS.

Emulator is an application running in an OS that appears to another OS or application as a specific hardware. It's a software that replaces hardware constructs, it simulates the behaviour of a HW.

Hypervisor is responsible for initiating, managing and terminating guests. A VM is a self-container computer packed into a single file. But something needs to run that file. It sits between the physical hardware and VM to virtualize the hardware.

Types of virtualization
Hardware virtualization (full virtualization): Guest is not aware it's running in a virtual environment. It requires CPUs with virtualization support.
Para-virtualization: Guest is aware it's running in a virtualized environment and is modified to work with it. There is no full-fledged hardware virtualization.
Hypervisor types
type i - bare-metal (native): It runs directly on the hardware, no host OS inbetween. As a result, VMs can access hardware directly so better performance. Examples: VMware vSphere, kvm, Microsoft Hyper-V

type ii - hosted hypervisor: It runs on the host OS as a process. VM have no direct accesss to hardware, it goes through host system which lowers VM performance. Examples: Oracle VirtualBox, VMware Workstation

libvirt is the library that provides management of virtual machines, virtual networks, virtual storage. Tools such as virt-manager, virt-viewer, virt-install, virsh` use this library.

QEMU
Quick EMulator is hypervisor also capable of CPU emulation of different archictectures. QEMU can be used together with third party hypervisor to boost performance.

KVM
KVM is a kernel module which works as a hypervisor, linux turned into hypervisor. It's of type i with full-virtualization capabilities.

Container vs VM
VM is a virtual copy of both OS and hardware. It takes up a lot of resources.
It takes VM minutes to start up.
Containers are lightweight because no virtualization is needed as it runs directly on the host OS.
It takes container seconds to start up.
Containers are portable.
VM can run many services and applications. Container runs one application only.
Docker
Docker daemon (engine) replaces hypervisor. It ensures each container is isolated from each other and the host OS.

base image, bins, libs and application code are contained altogether in the docker image, all bundled together. Dockerfile tells Docker how to build the image. A container is created and started with running a docker image

Docker image file
You can create a Docker image in two ways

docker run -it <base-image> # manually interactive shell into base image
docker run <image> # build an image from Dockerfile
# create container from image `ubuntu`
# checks local repository for image named `ubuntu` if not exists
# check central repository
docker run --name my-container -it ubuntu:latest bash
# pulls ubuntu:latest from docker hub, starts a docker container and shell login into it
# install some packages with package managers or manually here
exit
# docker container stopped at this point
# turn container into an image with versioning
docker commit -m "my-container-init" -a "Selcuk" my-container karakays/my-container:latest
Dockerfile is a declarative way of for creating a Docker image.

# Dockerfile
FROM ubuntu:latest
RUN apt-get update
# make some installations
EXPOSE 6379
ENTRYPOINT  ["app.sh"]
Run a container from an image

docker build -t my-image <path>
# start a container from Dockerfile in <path>
# -d to detach (let container run in bg)
# -p to publish port to the hostj
docker run -d -p 8888:6379 <my-image>




