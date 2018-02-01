# getting-started-with-docker
## Installing Docker in Ubuntu
- Follow the instructions here [link to Installation guide @ Docker](https://docs.docker.com/install/linux/docker-ce/ubuntu/)

## Post installation setup a docker group and add the user to the group

- Create the docker group if does not exist
``` sudo groupadd docker ```
- add user to docker group
``` sudo usermod -aG docker --user <username> ```

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
- To create a new container - ```docker run <image>```
- To run a container in daemon mode - ``` docker run -d <image>``` This is the default mode even if the switch is not specified
- To give a container your own funny name - ``` docker run image --name <myFunnyName> ```
- To run a container with port mapping - ``` docker run image -p hostPort:containerPort```
- To run a container in an interactive mode (opposite of daemon mode) - ``` docker run -i image name ``` Not recommended
- To list all containers - ```docker ps -a ```
- To list all containers' GUIDs - ``` docker ps -aq```
- To start a container ```docker start <Container funny name or guid> ```
- to start a container in interactive mode ``` docker start -i <container funny name or guid> ```


### Stopping and deleting containers
- To Stop a running container - ```docker stop <container funny name or GUID>```
- To stop all running containers - ``` docker stop $(docker ps -aq)``` 
- To delete a container - ``` docker rm <container funny name or guid> ```
- To delete all containers - ```docker rm $(docker ps -aq) ```

