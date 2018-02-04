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
## Docker images
- Docker image is a layered file system as shown in the image below
![Docker's layered image fs](./docker-images/dockerImage.png)
*image courtesy - The docker book and the [gitbook](https://washraf.gitbooks.io/the-docker-ecosystem/content/Chapter%201/Section%203/union_file_system.html) site*

- Now remember, images are the templates and not their runtime counterparts (containers). 
- At the base is the *bootfs* or the boot file system that behaves like the linux/unix file system comprising of the kernel and the containerization kernel essentials such as *cgroups* and *namespaces*
- The next is the actual OS file system called the *rootfs*. This contains the OS related files. This is mounted as *read-only* in a docker container
- Many more file systems are mounted on top of the *rootfs* leveraging a concept called as [Union File system](#Union-File-System). 

- This allows for cherry picking the libraries and software required on the image. Such as say *vim* or *apache* or *nginx* and so on. The Union file system allows for layering the directories related to the desired libraries and software on top of the *rootfs* to create the final image. Though these are layered directories, Docker calls these as individual images.
- An composed image is called a parent image and is used to spin up a container.
- Finally, when a container is launched, Docker spins up a thin read-write layer on top of the image inside which our desired application(s) or services run.

## Docker Containers
- When images are spun up as containers, Docker adds a thin read-write layer on top of the file systems in the image.
- This read-write layer is used to maintain the container state.
- Also, when a change is made to a file , say a config file in the underlying image file system, a copy of it is made into the read-write layer where the changes are applied.
- This ensures that the original image is retained in-tact and the delta changes are in the read-write layer.
- The process of creating a copy into the read write layer on change is called copy-on-write.

### Union File system
Union File systems have been around since the 1980s in different forms. Many unpopular implementations later, *OverlayFS* implementation was added to the Linux Kernel
- Union file system essentially allows directories to be layered on top of each other to make them appear as a single direcotry tree. 
- This can be imagined to be similar to the layers concepts followed in OHP (slide projector) where a slide on top of another can be added to project their combined view as single picture. 
- Even photoshop uses layers approach to apply different styles on images to envision the final state as a union of all layers. All the while without changing the actual original image. 

##Docker Registry and Repository
- [Docker Hub](http://hub.docker.com) is the public registry
- The same can be availed for on premise enterprise use as [Docker Trusted Registry](https://docs.docker.com/datacenter/dtr/2.4/guides/)
- Docker repository can be imagined to be similar to a git repo.
- Images in a repo are identified by their name and tags.
- Tags, similar to the public concept used all over the web are used to identify a particular image
- Tags can be used to identify a specific version of image to be pulled from repo. For instace ` git pull ubuntu:14.04`
    - Here the tag `14.04` has been identified after the image name `ubuntu` using a colon *(:)*
- see [getting-started-with-docker](getting-started-with-docker.md) for docker image related commands