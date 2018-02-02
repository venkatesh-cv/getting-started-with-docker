
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
``` docker swarm join --token SWMTKN-1-021njpl9joa3msxr225qmrd4zvs6nwagrd0o11c4l07afoxe5k-alg961c3gwlb35skygjcs8dub 192.168.1.104:2377 --advertise-addr <to-be-joined manager/worker ipa to use:2377> --listen-addr <to-be-joined manager/worker ipa to use:2377>```
When this command is executed, a response as shown below pops up
` this node joined a swarm as a manager'` or as a worker.

#### Viewing the node status ####
`docker node ls` is the command to view the node status it shows something like this
```hs
ID                            HOSTNAME                  STATUS              AVAILABILITY        MANAGER STATUS
sdfh2387sdlkj2lkjp03575jls    < worker host name>       Ready               Active
ncawby86hhlthjn9w9a93hzs4 *   venkatesh-Lenovo-G50-70   Ready               Active              Leader
klsjdf0293jklflsjd0923llsd    < host name goes here>    Ready               Active              Reachable
```
- *note* the asterisk (\*) against the ID `ncawby86hhlthjn9w9a93hzs4 *` shows the node that the user is currently logged on to
-  :heavy_exclamation_mark: The command will run only on managers & **not** workers
- The worker node can be identified by the absence of a manager status.

#### Can i change a worker to a manager- Yes ####
Its possible to mistakingly use the wrong token. So to help smooth things, here is a command to convert a worker to a manager
``` docker node promote <the ID that looks like a GUID>```

### Services ###
We had a brief look at services in the terms and definitions section. Services are a declarative way of running and scaling tasks. We also took a brief look at spinning up a service with a given name. 
Here are the list of possible servicecs related commands
``` docker service <create|ls|ps|inspect|update|rm ```
#### Creating a service ####
``` docker service --name myWebApp -p servicePort:portOnNode --replicas 5 <image name> ```
- This will cause 5 tasks / containers to be spun up in the swarm the current node belongs to.
- Supposing there is only one node in the swarm (say in a developer laptop), the 5 replicas will be spun up in the same node. as shown below

#### Viewing the replica status ####
- `docker services ps <service name> `. 
- For example if we start the service as follows.
```
 docker swarm init --advertise-addr 127.0.0.1:2377 --listen-addr 127.0.0.1:2377
 docker service --name psight1 -p 8080:8080 --replicas 5 nigelpoulton\pluralsight-docker-ci
 ```
- five instancecs will be spun up. Their status will be 
```
 docker services ps psight1
 ID                  NAME                IMAGE                                       NODE                      DESIRED STATE       CURRENT STATE            ERROR               PORTS
fstoe1i0o747        psight1.1           nigelpoulton/pluralsight-docker-ci:latest   myLaptop   Running             Running 13 minutes ago                       
bjb2chs9k86y        psight1.2           nigelpoulton/pluralsight-docker-ci:latest   myLaptop   Running             Running 13 minutes ago                       
3qrj5ppfdgst        psight1.3           nigelpoulton/pluralsight-docker-ci:latest   myLaptop   Running             Running 13 minutes ago                       
dmib30alpvwc        psight1.4           nigelpoulton/pluralsight-docker-ci:latest   myLaptop   Running             Running 13 minutes ago                       
4r04dv0zv7ur        psight1.5           nigelpoulton/pluralsight-docker-ci:latest   myLaptop   Running             Running 13 minutes ago                 
```
- Note that the node name is all myLaptop indicating that the service has created 5 replicas in a single node and the only node in the swarm.
- Note how the service does not care about the infra. It just spins up enough containers to meet desired state.
- When we try out the service in the browser at port 8080 it will work.
- Here is the most interesting thing. When running in a proper swarm, **regardless of the exact nodes in the swarm running the task/container, hitting the 8080 port on any of the nodes in the cluster will always return a proper response**
- **routing-mesh** this is the magic that happens behind the scenes that makes the service a container aware load balancer
- to view detailed information about a docker service key in `docker servie inspect --pretty psight1OrYourDockerServiceNamehere`

### scaling docker services in other words, adding / removing tasks###
- We can increase the number of tasks run by a service with a simple command
- There are two versions of the command
- This one is the preferred one `docker service update --replicas 10 psight1OrYourServiceNamehere`
- The other way is using a command alias `docker service scale psight1OrYourServiceNamehere=10`
- This command works both for scaling **up** or **down**
-  :heavy_exclamation_mark: adding new nodes **may** not guarantee rebalancing of tasks/containers across to include the new node

### removing a docker service ###
- To stop all tasks and remove a service the command is `docker service rm psight1OrYourServiceNameHere`

## creating an overlay network##
- to create an overlay network (is it like a vlan?) type ` docker network create -d overlay ps-netOrYourNetworkNamehere`
- to check the networks list type `docker network ls`

## Rolling updates ##
- Lets first inspect a docker service by keying in `docker service inspect psight1` you will see this
```
ID:		t6szoctp30sitbe0mr41e90db
Name:		psight1
Service Mode:	Replicated
 Replicas:	6
Placement:
UpdateConfig:
 Parallelism:	1
 On failure:	pause
 Monitoring Period: 5s
 Max failure ratio: 0
 Update order:      stop-first
RollbackConfig:
 Parallelism:	1
 On failure:	pause
 Monitoring Period: 5s
 Max failure ratio: 0
 Rollback order:    stop-first
ContainerSpec:
 Image:		nigelpoulton/tu-demo:v1@sha256:9ccc0c67e5c5eaae4bebeeed9b22e0e22f8a35624c1d5c80f2c9623cbcc9b59a
Resources:
Networks: ps-net 
Endpoint Mode:	vip
Ports:
 PublishedPort = 80
  Protocol = tcp
  TargetPort = 80
  PublishMode = ingress 
```
-If you notice the update related settings, parallelism is set to 1. This is the default. There are options to change it when starting a service .
- using switches such as `--update-parallelism 2 --update-delay 10m ` etc.,
### To start the update ###
``` docker service update --image nigelpoulton/tu-demo:v2 --update-parallelism 2 --update-delay 10s psight1```

## stacks and DAB - distributed application bundles ##
- stacks -- defines an application comprising of multiple services
- DAB -- stacks are deployed from distributed application bundles
<This needs to be understood in detail. TBD>
