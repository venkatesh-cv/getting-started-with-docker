# The docker book 
**By James Turnbull**
- February 2018
## Introduction
Virtual Machines | Containers
------------ | -------------
Run on top of hardware over an intermediation layer | Run inside the user's workspace
Complete abstraction | runs on top of the host OS kernel
Hardware Virtualization - presents as independent hardware to each VM running on it | OS virtualization - presents as independent OS (same as host) to processes running on each container
Can run any OS | can run only similar OS. For instance Windows cant be run on Ubutnu Server, but can run RHEL
runs the full OS | Only an abstraction of the host OS. Its similar in  the light weighted ness like that of the unikernel but running on the host OS
Very heavy | super light weight
Very secure | not that secure
Large attack surafce - full OS | small attack surface as containers have only barebones components required for its services to run

There are many types of container technologies such as 
- chroot jail
- OpenVZ
- Solaris zones
- lxc - linux container

### Docker is a linux container 
- It has its own control-group. 
     >cgroups as they are called allow you to allocate resources — such as CPU time, system memory, network bandwidth, or combinations of these resources — among user-defined groups of tasks (processes) running on a system. You can monitor the cgroups you configure, deny cgroups access to certain resources, and even reconfigure your cgroups dynamically on a running system. 
- it has its own namespaces 
    > Namespaces allow processes running in them to see system resources such as NICs, Memory as individual systems. Thus enabling total isolation of processes from one another.  Namespaces are what makes containers possible. They can be imagined to be what makes sandboxing possible via virtualization. 

*In other words, Docker can be considered a namespacing tool*
#### How does Namespacing help containers work?
- Namespaces provide isolation of process, filesystem, network, CPU, memory
- Traditionally each linux kernel has a single process tree.
- A process tree contains reference to all the processes running in the oS in a tree graph .
- A process given sufficient privileges can inspect or kill its other process in the process tree.
- with the introduction of namespaces, it has become possible now to have ***Nested Process trees***
- The other important point is that no matter the privileges, a process cannot monitor or do anything or even know the existence of processes in another sibling or parent process tree. Thus enabling total isolation
- however, the parent process tree is fully aware of the processes running in the child namespace/process tree just like any other process in the parent namespace.
- Each linux process starts by starting a process with id:1 PID:1. This in turn spins up other processes and so on. However, when spinning up a new process tree, it spins up a new namespace and that triggers another process with PID:1 running inside that namespace and thus giving processes running inside it an impression of running on its own host.

## Docker
Docker, in addition to providing a simple to use API to spin up, automate containers, also provides an application deployment engine that helps automate promotion of code through its development pipeline to production

### Docker features
- Incredibly lightweight
- helps ensure consistency of infrastructure configuration across environments from developer laptops to production infrastructure
- docker's high degree of process isolation means, though not a must, it lends itself easily to building distributed applications following microservices architecture or such by running different systems/sub-systems as independent processes within containers
- easy to scale
- easy to isolate for operations
#### use cases
- highly distributed, hyper-scale applications
- creating sandboxes on the fly for 
    - CI / CD pipeline test jobs
    - Development
    - teaching
    - exams / contests
    - QA & various forms of application testing
## Docker components
- Dockr Engine - a docker server or daemon(*dockerd*) & a client(*docker*) that features a full-fledged ReSTful api. Docker client talks to the Docker daemon (it can run on any host)
- Docker images - they are the templates / source-code for spinning up a container. They are built typically using a series of instructions, such as.
    - Copy a file / files
    - Run a command
    - Open a port
- Docker registry - hosts various images. Its also possible to run private/corporate registries
- Docker containers - runtime instances of the images. Each docker container is
    - an image format
    - a set of instructions
    - a runtime environment
## Stacks and Swarms
- **Stacks** are a bundle containers representing a system or an application. For instance a stack could contain a web-server, application-server and a database . Docker-Compose allows you to configure stacks and run them
- **Swarms** are a cluster of containers for high availability purposes. 

## Installing Docker
- follow instructions on the site or in the book
- Note that using uncomplicated firewall (UFW) requires additional config
### Configuring docker (see section 2.9.1 of the book)
- note that its possible to change the default configuration settings for
    - changing the NIC, port #s dockerd listens on
    - authentication can be enabled between client & daemon. see chapter 8
    - proxy settings can also be enabled
## Docker life cycle
- read [getting-started-with-docker](getting-started-with-docker.md)