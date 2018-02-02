# getting-started-with-docker
## Installing Docker in Ubuntu
- Follow the instructions here [link to Installation guide @ Docker](https://docs.docker.com/install/linux/docker-ce/ubuntu/)

## Post installation setup a docker group and add the user to the group

- Create the docker group if does not exist
``` sudo groupadd docker ```
- add user to docker group
``` sudo usermod -aG docker --user <username> ```

## Verifying the installation
- If you had added the user properly to the user group you should be able to run ``` docker version ``` and see something similar to this. Instead if you get permission denied, its probably because you missed logging off and logging back in for your `groupadd and usermod` to work or your `groupadd or usermod` has some error. To verify if the command works with `sudo` if it does, revisit your groupadd and usermod.
 ``` Client:
 Version:	17.12.0-ce
 API version:	1.35
 Go version:	go1.9.2
 Git commit:	c97c6d6
 Built:	Wed Dec 27 20:11:19 2017
 OS/Arch:	linux/amd64

 Server:
  Engine:
   Version:	17.12.0-ce
   API version:	1.35 (minimum version 1.12)
   Go version:	go1.9.2
   Git commit:	c97c6d6
   Built:	Wed Dec 27 20:09:53 2017
   OS/Arch:	linux/amd64
   Experimental:	false

```
- To get an overall status on the docker server. run `docker info`. You will see something similar to this
```
Containers: 1
 Running: 0
 Paused: 0
 Stopped: 1
Images: 2
Server Version: 17.12.0-ce
Storage Driver: overlay2
 Backing Filesystem: extfs
 Supports d_type: true
 Native Overlay Diff: true
Logging Driver: json-file
Cgroup Driver: cgroupfs
Plugins:
 Volume: local
 Network: bridge host macvlan null overlay
 Log: awslogs fluentd gcplogs gelf journald json-file logentries splunk syslog
Swarm: inactive
Runtimes: runc
Default Runtime: runc
Init Binary: docker-init
containerd version: 89623f28b87a62344d4b785663257362d1658a729
runc version: b2567b37d7b75eb4cf37289325b77297b140ea686ce8f
init version: 72290e6fa
Security Options:
 apparmor
 seccomp
  Profile: default
Kernel Version: 4.13.0-32-generic
Operating System: Ubuntu 16.04.3 LTS
OSType: linux
Architecture: x86_64
CPUs: 4
Total Memory: 7.684GiB
Name: Mighty-Eagle
ID: IMWQ:H5SO:UFQ2:YHBF:OJS4:3ZEZ:36BI:OSEJ:M7E4:LSA5:7B6V:HBI6
Docker Root Dir: /var/lib/docker
Debug Mode (client): false
Debug Mode (server): false
Registry: https://index.docker.io/v1/
Labels:
Experimental: false
Insecure Registries:
 127.0.0.0/8
Live Restore Enabled: false

WARNING: No swap limit support

```
***Docker Engine Port : 2375 Secured Engine Port : 2376***
## Images
### Download and installing images
- To download an image from docker hub - ``` docker pull <imageName>```
- To install a new docker image ```docker install <image>```
- To view all images in the system - ```docker images```
- To view all images' GUIDs - ``` docker images -aq ```

### Deleting images
- To delete a docker image - ```docker rmi <image name>```
- To delete all docker images - ```docker rmi $(docker images -aq)```

## Containers
:large_orange_diamond: Container funny names are names automatically assigned to running containers when a name is not explicitly specified
- To create a new container - ```docker run <image>```. This automatically pulls the image from the hub if its not available locally, and spawns a container off it and starts it. 
- :heavy_exclamation_mark: The `docker run` spawns a new container each time its run. Use `docker start|stop` to bounce an existing container 
- To run a container in daemon mode - ``` docker run -d <image>``` This is the default mode even if the switch is not specified
- To give a container your own funny name - ``` docker run image --name <myFunnyName> ```
- To run a container with port mapping - ``` docker run image -p hostPort:containerPort```
- To run a container in an interactive mode (opposite of daemon mode) - ``` docker run -it image name ``` Not recommended. 
  - the ```-it``` switch makes it run interactively and assigns it to the default terminal.
  - To quit interactive mode without killing the container, use ``` ctrl + p + q```
- To list all containers - ```docker ps -a ```
- To list all containers' GUIDs - ``` docker ps -aq```
- To start a container ```docker start <Container funny name or guid> ```
- to start a container in interactive mode ``` docker start -i <container funny name or guid> ```

### Stopping and deleting containers
- To Stop a running container - ```docker stop <container funny name or GUID>```
- To stop all running containers - ``` docker stop $(docker ps -aq)``` 
- If a container has only one running process, the container will automatically stop when the solitary process stops.
- To delete a container - ``` docker rm <container funny name or guid> ```
- To delete all containers - ```docker rm $(docker ps -aq) ```

## Images repository
All docker images are hosted here at [Docker hub](https://hub.docker.com#

## Dockers and swarms
### Terms and definitions
- **Swarm** - a cluster comprsing of docker engines is a swarm. Note - the word container is not being used here. just the engine
- **Swarm mode** - In a swarm, the docker engines are said to run in a *swarm mode*. This mode is essential for docker to run in a swarm. The default mode that we have seen so far is called the *Single engine mode*
- **Manager node** 
  - A swarm can have one or more manager node that is responsible for looking after the state of the swarm. 
  - They are responsible for despatching tasks and commands to the *worker nodes* in the swarm. 
  - ***note*** - Just like how the puropse of the swarm is high availability, the manager nodes also support high availability and hence more than one manager(3 or 5) node and even if one of them goes down the others will keep the swarm running
  - Its recommended that an odd number of managers be configured 3 or 5(recommended) per swarm. Higher numbers are not encouraged as it becomes difficult to arrive at consensus with higher numbers
  - All managers maintain the state of the cluster, but only one is the leader.
  - All manager nodes can receive commands. However, they will be executed by only the leader. The others will just act as proxies and forward the commands to the leader
  -  :heavy_exclamation_mark: Managers can be spread across high availability zones, data centers etc.,
  - Managers use a [Raft Protocol](https://raft.github.io/raft.pdf) implementation to keep commands synchronized and to elect leaders.
- ** Worker node** - each docker engine running in a swarm is called a worker node
- While all manager nodes are also worker nodes, its not a recommended practice to use manager nodes as workers
- **Services** - They are a declarative way of running & scaling *tasks* . In other words, services are told the desired end state and they take the resopnsibility of ensuring that the desired state is maintained. Read on.
  - For example here is a service `docker service create --name webFrontEndOrSuchName --replicas 5`
    - This tells docker to create a task called WebFrontEnd and create five replicas of it.
    - What is this task? Think of it as a container. 
    - Docker will spin up five containers on various worker nodes across the cluster 
    - Service will also ensure that there are always 5 replicas up and running. So, if one goes down, it spins up another one right away. This is the *delcarative* emphasis that was laid in the definition
 - **Task** task is an atomic unit of work assigned to a worker node in a swarm. 
  - They also have metadata about initiating the container and some other runtime stuff. 
  - But in abstract terms one can think of task as a container
  - However, its possible that Tasks can be used to represent other things in the future. Such as a unikernel, for instance
  
### Setting up a swarm
***Swarm Port : 2377***
- To start a swarm, some planning is required. 
- Identify three servers that will act as managers. For production it should be 5
- Identify the servers that shall be the worker nodes.
- Now the first manager that spins up the swarm shall be the leader.
- Other managers will need to be spun up and joined with the leader
- The worker nodes subsequently

#### To start a swarm####
- `docker swarm init --advertise-addr <ipa:2377> --listen-addr <ipa:2377>` 
  - the `--advertise-addr command allows the exact network interface card to be used for communications. `
  - the listen address `--listen-addr` also has similar behavior
  - The port can be a port of your choice. But 2377 is the convention
  - ** It is strongly recommended that the --advertise-addr & --listen-addr switches be used in all docker swarm spooling commands**
  - Here is what happens when the command is run
  ```
  docker swarm init --advertise-addr 192.168.1.104:2377 --listen-addr 192.168.1.104:2377
  Swarm initialized: current node (ncawby86hhlthjn9w9a93hzs4) is now a manager.
  To add a worker to this swarm, run the following command:
  docker swarm join --token SWMTKN-1-021njpl9joa3msxr225qmrd4zvs6nwagrd0o11c4l07afoxe5k-alg961c3gwlb35skygjcs8dub 192.168.1.104:2377
  To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.
  ```
  - **Note** the commands given in the reponse are to be followed to the letter.
#### To join a manager or worker to the swarm ####
To recover the syntax for joining the worker or the manager to the swarm simply type the following command.
` docker swarm join-token manager`
To retrieve
```
To add a manager to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-021njpl9joa3msxr225qmrd4zvs6nwagrd0o11c4l07afoxe5k-3rs69fr9a9ucj462lozjp5t46 192.168.1.104:2377

```
Likewise to retrieve the join token to join a worker to the swarm, key in 
` docker swarm join-token worker`
to retrieve
```
To add a worker to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-021njpl9joa3msxr225qmrd4zvs6nwagrd0o11c4l07afoxe5k-alg961c3gwlb35skygjcs8dub 192.168.1.104:2377
```
If you notice carefully, the commands with the join tokens are almost same except for the last portion of the join-token. This join-token is what tells the manager whether the node joining it is a manager or a worker.

#### Joining a manager or worker ####
In the earlier section we noted that the `join-token` can be pulled up using a simple commmand. 
Use the command verbatim , but with the following additions , in the new manager / worker being joined to the swarm
` docker swarm join --token SWMTKN-1-021njpl9joa3msxr225qmrd4zvs6nwagrd0o11c4l07afoxe5k-alg961c3gwlb35skygjcs8dub 192.168.1.104:2377 --advertise-addr <to-be-joined manager/worker ipa to use:2377> --listen-addr <to-be-joined manager/worker ipa to use:2377>`
