# getting-started-with-docker
Installing Docker in Ubuntu

Create the docker group if does not exist
sudo groupadd docker
add user to docker group
sudo usermod -aG docker --user <username>


- docker install <image>
- To create a new container - docker run <image>
- To view all containers - docker ps -a
- To Stop a running container - docker stop <container funny name or GUID>
- To stop all running containers - docker stop $(docker ps -aG)
- To start a container docker start <Container funny name or guid>
- To delete a container - docker rm <container funny name or guid>
- To delete all containers - docker rm $(docker ps -aG)
- To view all images in the system - docker images
- To delete a docker image - docker rmi <image name>
- To delete all docker images - docker rmi $(docker images -aG>
