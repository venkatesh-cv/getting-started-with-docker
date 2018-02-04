# Getting-started-with-docker. Based on a pluralsight course by Nigel Poulton and on the Docker Book
Table of Contents
=================
   * [Getting-started-with-docker. Based on a pluralsight course by Nigel Poulton and](#getting-started-with-docker-based-on-a-pluralsight-course-by-nigel-poulton)
   * [Table of Contents](#table-of-contents)
   * [getting-started-with-docker](#getting-started-with-docker)
      * [Installing Docker in Ubuntu](#installing-docker-in-ubuntu)
      * [Post installation setup a docker group and add the user to the group](#post-installation-setup-a-docker-group-and-add-the-user-to-the-group)
      * [Verifying the installation](#verifying-the-installation)
      * [Images](#images)
         * [Download and installing images](#download-and-installing-images)
         * [Deleting images](#deleting-images)
      * [Containers](#containers)
         * [Stopping and deleting containers](#stopping-and-deleting-containers)
         * [restarting failed containers](#restarting-failed-containers)
         * [inspecting docker configuration](#inspecting-docker-configuration)
         * [Docker logs](#docker-logs)
         * [Inspecting processes running inside a container](#inspecting-processes-running-inside-a-container)
         * [Adding another process to a running container](#adding-another-process-to-a-running-container)
      * [Images repository](#images-repository)
      * [Dockers and swarms](#dockers-and-swarms)
         * [Terms and definitions](#terms-and-definitions)
         * [Setting up a swarm](#setting-up-a-swarm)
            * [To start a swarm####](#to-start-a-swarm)
            * [To join a manager or worker to the swarm](#to-join-a-manager-or-worker-to-the-swarm)
            * [Joining a manager or worker](#joining-a-manager-or-worker)
            * [Viewing the node status](#viewing-the-node-status)
            * [Can i change a worker to a manager- Yes](#can-i-change-a-worker-to-a-manager--yes)
         * [Services](#services)
            * [Creating a service](#creating-a-service)
            * [Viewing the replica status](#viewing-the-replica-status)
         * [scaling docker services in other words, adding / removing tasks###](#scaling-docker-services-in-other-words-adding--removing-tasks)
         * [removing a docker service](#removing-a-docker-service)
      * [creating an overlay network##](#creating-an-overlay-network)
      * [Rolling updates](#rolling-updates)
         * [To start the update](#to-start-the-update)
      * [stacks and DAB - distributed application bundles](#stacks-and-dab---distributed-application-bundles)



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

### seaching for images
- The easiest is to visit [Docker hub](http://hub.docker.com) on the browser and check
- Alternatively, `docker search <imageName>` can be used. For instance `docker search puppet` will bring up
    ```
    NAME                             DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
    puppet/puppetserver              A Docker Image for running Puppet Server. Wi…   49                                      
    devopsil/puppet                  Dockerfile for a container with puppet insta…   25                                      [OK]
    macadmins/puppetmaster           Simple puppetmaster based on CentOS 6           25                                      [OK]

    ... and many more             

    ```

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
- docker run command has the following syntax
  > Usage:	docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
  - for instance we could run ` docker run -it ubuntu /bin/bash` to run the bash command inside ubuntu container thats run in an interactive mode
    - when you run this container. the container will run only as long as the bash terminal runs. The moment you exit the bash, the container will stop.
    **gotcha** if you want to attach to a  container running in detached / daemon mode to an interactive terminal session. use `docker attach <containerFunnName or guid>`
    - Here is another example `docker run -it ubuntu /bin/bash -c "ls -l"` will print the following output and terminate the container
    ```
    total 64
    drwxr-xr-x   2 root root 4096 Jan 23 22:49 bin
    drwxr-xr-x   2 root root 4096 Apr 12  2016 boot
    drwxr-xr-x   5 root root  360 Feb  3 14:25 dev
    ....

    ```
- To list all containers - `docker ps -a `
- To list n instance of running or stopped previous containers - ` docker ps -n 10`
- To list all containers' GUIDs - ` docker ps -aq`
- To start a container `docker start <Container funny name or guid> `
- to start a container in interactive mode ` docker start -i <container funny name or guid> `

### Stopping and deleting containers
- To Stop a running container - `docker stop <container funny name or GUID>`
- To stop all running containers - ` docker stop $(docker ps -aq)`
- If a container has only one running process, the container will automatically stop when the solitary process stops.
- To delete a container - ` docker rm <container funny name or guid> `
- To delete all containers - `docker rm $(docker ps -aq) `

### restarting failed containers
- containers can be configured to automatically restart in the event of a container crash.
- By default, containers are configured ***not*** to restart in the event of a failure and possibly with a good reason.
- Should one desire to configure an automatic restart, its advisable to limit it to a lower number.
- restart configuration is a part of container spin up using the `docker run` command. its done as follows
- `docker run --restart=on-failure:5 ..... ` followed by the other options. Here docker has been advised to restart the container in the event of a catastrophic failure. But no more than 5 times.

### inspecting docker configuration
- run `docker inspect <containerFunnyNameOrGuid>`
- **note** it throws up an huge JSON.
- to query the JSON selectively, we can use `-f ` switch that is a **Go template  driven switch**
- this allows us to write commands such as `docker inspect -f {{.NetworkSettings.IPAddress}} elClassico` to bring up
  > 172.17.0.2
- Refer the [Go Template link](https://golang.org/pkg/text/template/) for more information leveraging teh Go Template model for formatted queries
- here is another on `docker inspect -f "{{.Name}} {{.State.Running}}" elClassico` to bring up
  > elClassico true
- Here is a sample of a full battery of attributes that can be used as a guide to prepare the `-f` switch
```
{
        "Id": "ca2bdbd443c326c66dec0d19ce3c8e093b24af6be631e3ac84e8decbf56dc42d",
        "Created": "2018-02-03T17:25:58.511212109Z",
        "Path": "python",
        "Args": [
            "app.py"
        ],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 7096,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2018-02-03T17:25:59.508061749Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
 "Image": "sha256:9b915a241e29dc2767980445e3109412b1905b6f1617aea7098e7ac1e5837ae2",
        "ResolvConfPath": "/var/lib/docker/containers/ca2bdbd443c326c66dec0d19ce3c8e093b24af6be631e3ac84e8decbf56dc42d/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/ca2bdbd443c326c66dec0d19ce3c8e093b24af6be631e3ac84e8decbf56dc42d/hostname",
        "HostsPath": "/var/lib/docker/containers/ca2bdbd443c326c66dec0d19ce3c8e093b24af6be631e3ac84e8decbf56dc42d/hosts",
        "LogPath": "/var/lib/docker/containers/ca2bdbd443c326c66dec0d19ce3c8e093b24af6be631e3ac84e8decbf56dc42d/ca2bdbd443c326c66dec0d19ce3c8e093b24af6be631e3ac84e8decbf56dc42d-json.log",
        "Name": "/relaxed_meninsky",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "docker-default",
        "ExecIDs": null,
 "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "default",
            "PortBindings": {
                "80/tcp": [
                    {
                        "HostIp": "",
                        "HostPort": "8080"
                    }
                ]
            },
            "RestartPolicy": {
                "Name": "on-failure",
                "MaximumRetryCount": 5
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "CapAdd": null,
            "CapDrop": null,
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "shareable",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "ConsoleSize": [
                0,
                0
            ],
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": null,
            "BlkioDeviceWriteBps": null,
            "BlkioDeviceReadIOps": null,
            "BlkioDeviceWriteIOps": null,
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DiskQuota": 0,
            "KernelMemory": 0,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": false,
            "PidsLimit": 0,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0
        },
"GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/3e174bafdd3a1dfbeffb8ca1a7d71ffc2865a03363cbd1fd00ce5a3b73c009b9-init/diff:/var/lib/docker/overlay2/c42089f145b89e8f0fafadcf7b39109a9d029fc605e3cd9fc0d2a0e7afb90981/diff:/var/lib/docker/overlay2/06d18781ea4eb4e406340bd390dfdfdaf09ae55addecb5c118f2ae1cd3668624/diff:/var/lib/docker/overlay2/30b04eb80670879718566a63cc8b562bb9492cc68cd0ba2008a505352110001d/diff:/var/lib/docker/overlay2/c4546f7a4c43c4573c670a4a0c87efabbdd351187c96e1a133ebd2be9a61b9ce/diff:/var/lib/docker/overlay2/2133d743ac143b57a3afa599af868c81a1e0b692dfe84df8b87306e93e49d528/diff:/var/lib/docker/overlay2/164b90569cdf37a3ebca0ac2106ee0e61792f2573d68afab7e656967445a8350/diff:/var/lib/docker/overlay2/dee9e01abb5370a93244563e127ddd205f67d6ac0d83e0a8ac06c32e3e8b2e17/diff:/var/lib/docker/overlay2/bcf6372e441a2739aeeb314858fa01240224199d676f4158c64b00e5674890c7/diff:/var/lib/docker/overlay2/1a50ba6e3ac42f0b7b76fb5706ddf2c260c126fe465d272c7c37511f62eb092e/diff:/var/lib/docker/overlay2/387402ba698b8ad95477eb2a19c52e98232651b1012c435a27b5f70cd19ad672/diff:/var/lib/docker/overlay2/8f6461b8fef1818600239623d91e575c2ad299b6c13d036c723959fd05fa677f/diff:/var/lib/docker/overlay2/5a40bbd74ddea5041a0de66d035319579455b8715cb214a423feb725f3a2098d/diff:/var/lib/docker/overlay2/d301264c24e2b003498c58e68dba729cfd7f6de0bf2c4ca5056da8f33533bb81/diff:/var/lib/docker/overlay2/8cbc654afb425c290849a0a8137ef804cf67f869bff9d878390ceaabb8b02f4d/diff:/var/lib/docker/overlay2/fbdcf82f3ba45ba58cc31fa7d4072178a48071d290e2d302b7c43346b12508e4/diff:/var/lib/docker/overlay2/e3ce5818bcb328395fe2012279755230eb933d469e602f6517390ab2251eb536/diff:/var/lib/docker/overlay2/5236e61144d16a7b054576af0873f015b0e9223a1e07ca434f80c9e4241838a4/diff:/var/lib/docker/overlay2/238da80ec95ca82859670cde56f5eff534181d026c57efe9ddebdafacd79d176/diff",
                "MergedDir": "/var/lib/docker/overlay2/3e174bafdd3a1dfbeffb8ca1a7d71ffc2865a03363cbd1fd00ce5a3b73c009b9/merged",
                "UpperDir": "/var/lib/docker/overlay2/3e174bafdd3a1dfbeffb8ca1a7d71ffc2865a03363cbd1fd00ce5a3b73c009b9/diff",
                "WorkDir": "/var/lib/docker/overlay2/3e174bafdd3a1dfbeffb8ca1a7d71ffc2865a03363cbd1fd00ce5a3b73c009b9/work"
            },
            "Name": "overlay2"
        },
 "Mounts": [],
        "Config": {
            "Hostname": "ca2bdbd443c3",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "80/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "LANG=C.UTF-8",
                "PYTHON_VERSION=2.7.10",
                "PYTHON_PIP_VERSION=7.1.2"
            ],
            "Cmd": [
                "python",
                "app.py"
            ],
            "Image": "nigelpoulton/tu-demo:v1",
            "Volumes": null,
            "WorkingDir": "/app",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {}
        },
 "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "86aa2cddcdefeb04f3a988d3817ce432c5663ffb9d52bf830c0ec8cd3d2c481a",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {
                "80/tcp": [
                    {
                        "HostIp": "0.0.0.0",
                        "HostPort": "8080"
                    }
                ]
            },
            "SandboxKey": "/var/run/docker/netns/86aa2cddcdef",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "5b618325777b46587878e8768b57a34deac6ea41730ca8295593938cf1d8fab5",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.2",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:02",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "f92df2ef25d1c842e2cc7b55632c642c0066967f16969bb57b88a6c2f4e4aff8",
                    "EndpointID": "5b618325777b46587878e8768b57a34deac6ea41730ca8295593938cf1d8fab5",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null
                }
            }
        }
    }
]

```

### Docker logs
- This helps understand whats happening in a container as it runs
- use the command `docker logs -f <containerFunnyName or guid>` this is somewhat similar to the `tail -f ` command on a log file
- or tail specific number of lines with this command `docker logs --tail 10 -f <containerFunnyNameOrGuid>` to specify a follow of the last 10 lines
- additionally we can print timestamps to help debugging ` docker logs -ft <containerFunnyNameOrGuid>` to print
  ```
  2018-02-03T14:30:24.082610889Z total 64
  2018-02-03T14:30:24.082676045Z drwxr-xr-x   2 root root 4096 Jan 23 22:49 bin
  2018-02-03T14:30:24.082685440Z drwxr-xr-x   2 root root 4096 Apr 12  2016 boot
  ...
  ```
- there are docker log drivers available to direct the logs to different channels.  logs can also be directed to the hosts' log channel using the `--log-driver="syslog" ` command. 
- Alternatively , logging can be altogether disabled using the log-driver="none" switch
### Inspecting processes running inside a container
- for instance lets run a container by running `docker run -p 80:80 nigelpoulton/tu-demo:v1 --name elClassico`
- now running `docker top elClassico` will yield
  ```
  UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
  root                30259               30241               0                   20:21               ?                   00:00:00            python app.py

  ```
- to get the resource utilization stats type `docker stats elClassico`  or the respective container name

### Adding another process to a running container
- additional processes can be added to a running container in an interactive mode ` -it` or in a detached mode as a background process within the container using the `-d` switch.
- To do this use the command `docker exec -d <containerFunnyNameOrGuid> command arguments`
- for example `docker exec -it elClassico /bin/bash`
- this will allow you to login to an image and see whats going on inside of it with the help of a bash shell and maybe add more components to the container for say a PoC! or more!

## Images repository
- All docker images are hosted here at [Docker hub](https://hub.docker.com)
- To log into the docker hub from terminal use `docker login`
- likewise use `docker logout` to end the session

## Building your own Docker Image
- The recommended way of building your own docker image is using a *dockerfile* & the `docker build` command
- Generally building images involves using an existing base image such as an Ubuntu or a Fedora image to add layers on top
- However, it is possible to build your own base image from the scratch. This link shows how  to create your own [base image](https://docs.docker.com/develop/develop-images/baseimages/)

### using commit command to build your own image
- though this is not the recommended way of building your own image, it is good to use this for learning purposes.
- this approach helps customize a container to suit our need and commit it as a new image.
- for instance, we can attach a bash session to a running container using 'docker exec -it , install vim and commit it as a new image. see below
    ```
    docker run -d -p 80:80 nigelpoulton/tu-demo:v1 --name elClassico
    docker exec -it elClassico /bin/bash
    in the newly opened bash session
    apt-get update
    apt-get install vim
    vim
    exit -- to log out of attached bash session in the container
    docker commit -m "tu-demo with vim installed" -a "venkatesh" elClassico venkatesh/tu-demo-vim"
    ```
- now when we check images we will see ours
    ```
    docker images
    REPOSITORY               TAG                 IMAGE ID            CREATED             SIZE
    venkatesh/tu-demo-vim   latest              f389bd3c855d        10 minutes ago      251MB
    nigelpoulton/tu-demo     v1                  9b915a241e29        21 months ago       212MB

    ```
- Once this is done, the newly commimted image can be used as any other image
- to push this to your repo, use the git like command
    ```
    docker push venkatesh/tu-demo-vim
    ```
- finally refer this [link](https://docs.docker.com/engine/reference/commandline/commit/#description) for more switches for the commit command
    ```
    docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]
    ```
### Building your own images using Dockerfile
- This approach involves building a script file that specifies the steps to customize a base image to our needs
- to emulate the same example discussed earlier, the following steps are specified in the script
    ```
    FROM nigelpoulton/tu-demo
    MAINTAINER venkatesh "someemail@gmail.com"
    RUN apt-get update;apt-get install -y vim
    ```
- this pretty much does the same job as spooling up a tu-demo container, logging into it using an attached bash shell and running the apt-get commands to install the vim editor.
- Dockerfile approach also is incremental in the sense that each step is an independent image that is in turn used recursively to build the next step. This way, it is easier to understand where a build breaks.
- This approach of caching an image for each step is useful for debugging, but not a healthy one for reuse. So, in devops pipelines its always healthy to instruct docker not to cache anything using the `--no-cache` switch
- To run the build process run the following command
    ```
    docker build -t="venkatesh/tu-demo-vim-build" --file build/DockerfileOrOtherFile ./build
    Sending build context to Docker daemon  2.048kB
    Step 1/3 : FROM nigelpoulton/tu-demo ---> 9b915a241e29
    Step 2/3 : MAINTAINER venkatesh ---> Running in 0f75d0872881
    Removing intermediate container 0f75d0872881 ---> f7d22e43862e
    Step 3/3 : RUN apt-get update; apt-get install -y vim
    ---> Running in 828fde4bf336
    Get:1 http://security.debian.org jessie/updates InRelease [63.1 kB]
    Ign http://httpredir.debian.org jessie InRelease

    ```
- the file can also be hosted out of github and the url used in the build command --file switch
- to see how an image was built start with the `docker history <containerFunnyNameOrGuid>`
- Here is another example where an ubuntu image is used to 
    - build another one with Nginx installed 
    - Create an index.html file 
    - open port 80 on the container
    - run nginx on startup in an imperative way 
    - since nginx needs port 80, it becomes accessible on that.
    ```
    # version 0.0.1
    FROM ubuntu:16.04
    MAINTAINER venkatesh
    RUN apt-get update; apt-get install -y nginx
    RUN echo 'Hi Im in our container' > /var/www/html/index.html
    EXPOSE 80
    ENTRYPOINT ["usr/sbin/nginx", "-g", "daemon off;"]

    ```
    - if we spin up the container by say
        ```
        docker run -d --name elClassico -p 80:80 cvenkatesh/ubuntu-with-nginx
        ```
    - We will be able to access the *localhost* in the browser to see
    > Hi Im in our container 
#### Dockerfile CMD switch
- this is used to specify the command that shall be run when a container is spun up from the image being built
```
CMD ["usr/sbin/nginx", "-g", "daemon off;"]
```
- but this can be overridden in the `docker run ` command when a command and args are present as the last argument
- say if we run the container as `docker run -it newContainer /bin/bash` it will run the bash shell instead of the nginx startup command

#### ENTRYPOINT switch
- this is similar to the CMD switch only that this cannot be overridden from the docker run command line
- any command and args given to the docker run command line will in turn be passed to the command specified in the ENTRYPOINT switch.
- see example above.
- if required entrypoint can be overridden explicitly with a `--entrypoint` flag

#### WORKDIR switch
- this is simple. This is used to configure working directory inside the image to run the Dockerfile commands
- multiple WORKDIR can be specified. Simply use it like a `cd` command.

#### Other switches
##### ENV
used to set an environment variable for executing a RUN command or such

##### USER
used to set the user / group context in which a subsequent command(s) has to be run.

##### VOLUME
This is used to map a network drive or folder to an image without adding / packaging it into the image. This helps map source code, resource folders, data folders etc.,
- These exist outside of a container / image's life cycle
- They are akin to any host folder that is mapped to another system
```
VOLUME ["/opt/project/src", "opt/project/build"]
```
##### ADD
Thi is like *a copy into image* command. This is different from VOLUME in the aspect that all mapped directories and their content are copied into the image. 
- as a bonus if the source is a tarball archive, it will be automatically extracted into the targt directory
- useful for packing static content or such resources into the
```
ADD /opt/project/src/images /root/build/images
ADD /opt/project/vendor/vendor.tar.gz /root/build/scripts/vendor
```

##### COPY
This is similar to add only without the magic auto-extraction features

##### LABEL
helps add metadata to docker image.
These are created as label=value format.
```
LABEL build="beta" role="web server"
```
 *IMPORTANT* its strongly recommended that multiple tags be added under one LABEL to avoid docker from creating a separate image for each tag (remember that Docker creates a separate image for each step)

##### ARG
This identifies the variables that can be passed as build-time arguments in the Docker build command. For example if we wish to change port numbers or such
```
ARG port=80
```
allows specifying the port number to use when building the image. If not specified the above syntax will default it to 80.
- These arguments are passed using the --build-arg switch
    ```
    docker build --build-arg port=8080 -t myrepo/myimage ./buildDirectory
    ```
- There are a standard list of ARG variables available that do not require to be explicitly declared . 
    - HTTP_PROXY
    - HTTPS_PROXY
    - FTP_PROXY
    - NO_PROXY
##### SHELL
Helps run a command in a specific shell
##### HEALTHCHECK
Somewhat like an assert command for the container.Helps run commands that will verify whether the container started properly and is running properly
- failing healthchecks will lead to containers being marked as unhealthy.
- here is a simple command
```
HEALTHCHECK --interval=10s --timeout=1m --retries=5 CMD curl http://localhost || exit 1
```
to view the health of a container run
```
docker inspect --format '{{.State.Health.Status}}' containerName
```
##### ONBUILD
This is a trigger for build event on an image. This is used when an image is used as a base image for another image. This might necessitate say a VOLUME to be ADDed into the image etc., This can be used for anything appropriate
#### Automated Builds
This allows syncing our Bitbucket or Github repos containing the Dockerfiles to Dockerhub in such a way that any updates to the repo will trigger an image to be built and published to the Dockerhub.
This is useful in enterprise situations where CI/CD is in play

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
