Physical server--->vm----->containers

vm solve some problems of pysical server(modern things done by devops),conatiner solve some problems of vm

u can create containes on top of vm  as well as pysical server

why docker containers are lightweight?
Docker containers are lightweight because they share the host operating system's
kernel instead of hardware-virtualizing a full guest operating system

<img width="617" height="354" alt="image" src="https://github.com/user-attachments/assets/fd1c0ed7-1371-4a6b-b1d6-9a2d76fbc0ce" />


**1.Why Docker**
Docker is used because it solves the "it works on my machine" problem by packaging an application and all its dependencies into a standardized, isolated container

**2.Container**

A Docker container is a lightweight, standalone, and executable package of software that includes everything needed to run an application, including the code, runtime, system tools, libraries, and settings. 

**3.Container vs virtual machine**

Docker containers virtualize the operating system to share the host kernel, while Virtual Machines (VMs) virtualize physical hardware to run a completely separate guest operating system

**4.difference between images and containers in docker**
The core difference is that a Docker image is a static, read-only template containing your application code and environment configurations, while a Docker container is a live, executable runtime instance created from that image
<img width="704" height="410" alt="image" src="https://github.com/user-attachments/assets/3f90f92a-edc5-4312-8123-56b15932881c" />

**Commands**
1.ran
#Create and Run a container(attach mode)
Cmd: docker run --name docker-container-name docker-image-name:v1

Essential Flags
-d: Runs the container in detached mode (background).
-it: Connects your terminal to the container for an interactive shell.
-p: Maps a host port to a container port (host:container).
-v: Mounts a storage volume to persist data (host_path:container_path).
--name: Assigns a custom name to the container.
--rm: Automatically removes the container when it exits.
-e: Defines environment variables inside the container

2.#Show running containers
Cmd: docker ps
 
3.#Show all containers (running + stopped)
Cmd: docker ps -a

#Stop docker container
Cmd: docker stop container-name
Note:must now container name or container id
 
#Remove docker container
Cmd: docker rm container-name
output:it will show container name then it is successfully removed

#List docker images:
Cmd: docker images
Note :we will get name id tag size created date here


#Remove docker image
CMD: docker rmi image-name(we can't delete it with image id)
Note: make sure no containers are running on that image before you remove image

docker pull command downloads container images from a registry (like Docker Hub) to your local machine. 

Core Examples
Default Pull: Downloads the image with the :latest tag.bash
docker pull ubuntu
Use code with caution.Specific Version: Downloads a precise version using a tag.bash
docker pull ubuntu:22.04

**docker pull only downloads an image, while docker run downloads, creates, and starts a container from that image.**

#Keep container sleep for 5 seconds
CMD: docker run image-name sleep 5

#How to run a command on already running container(change a config, check a log, run a quick query)
CMD: docker exec container-id cat /etc/hosts

 
#Run a container on Detach mode(it will run on background, directly gives the id .and stops automatocally.so you can able to perform other actions)
CMD: docker run -d container-name or ID

note:if you want to give name to container then run:
CMD: docker run -d --name divya container-name or ID

 
##Run a container on Detach mode(here it won't stop we need to press contrl+c to get out of it or quit)
CMD: docker run container-id

#if you want to attach back to running container
CMD: docker attach container-id
note Lcontainer id is big number like:asdfghjkrtyu so just give asdfg so it will understand because it is unique

The docker inspect command returns low-level, detailed configuration data for virtually any Docker object—such as containers, images, volumes, and networks—in a structured JSON format.
docker inspect <container_name_or_id>

**Image tags**
one image can be used by different cointainers

1.docker rum redis
output :it will give latest version
if you want to go with specific version
cmd docker run redis:7.4

2.now you have 2 images of redis with different version so u want to delete one specific version
cmd:docker rmi redis:7.4

**interactive mode**

#Docker container runs in a  non-interactive mode. You can give input while running, so you must provide before running.
Ex:docker run image-name(non-interactive)
Output: Hello and Welcome!
 
Ex: docker run -i image-name(interactive)
Naga(just we give name)
Output: Hello and Welcome Naga!
 
Ex: docker run -it Image-name(interactive and terminal)
Welcome! Please enter your name: Naga
Output: Hello and Welcome Naga!

#Port mapping
CMD: docker run -d -p 80:5000 image-name
Note:we can use 81:5001.82:5000
but we can't do 80:5001
Description:
80 -> host port or machine port
5000 -> container port
If we want to allow users to access our application: we have to share the host ip and host port
If uses access container ip and port will not access to outside of users, since it will only access within host network.

#Volumes
CMD: docker run -v /opt/datadir:/var/lib/mysql mysql
Description: starts a MySQL container and uses a Docker bind mount to store MySQL data outside the container.
 
HOST MACHINE                 MYSQL CONTAINER
 
/opt/datadir    ←────────→  /var/lib/mysql
                               ↓
                         MySQL database files


Why do we use this?
The main reason is data persistence
Without a mount:
MySQL Container
     ↓
MySQL Data
     ↓
Container deleted
     ↓
Data may be lost ❌
 
 
Ex2:
CMD: docker run --mount type=bind,source=/opt/datadir,target=/var/lib/mysql mysql
Description:
starts a MySQL container and bind-mounts a directory from your host machine into the container.
It is essential
 
--mount vs -v
 
These two commands have a similar purpose:
docker run -v /opt/datadir:/var/lib/mysql mysql
And
docker run --mount type=bind,source=/opt/datadir,target=/var/lib/mysql mysql
The second is simply more explicit and readable:
-v syntax:
HOST_PATH:CONTAINER_PATH
 
--mount syntax:
type=bind,

source=HOST_PATH,
target=CONTAINER_PATH
For scripts and production configurations, --mount can be easier to understand because type, source, and target are clearly specified.
 
#How to check container logs after running container in detach mode
CMD: docker logs container-name or id  


**Docker File**
what docker file contain
A Dockerfile is a plain text file containing a set of instructions used to build a Docker image.
Each instruction defines a step in creating the image, such as selecting a base image ( FROM ), copying files ( COPY ), installing packages ( RUN ), or setting environment variables (ENV).

#Problems with docker builds
Packages re-download every build (for image creation even change only for single line)
Secrets leak into image metadata - docker build --build API_KEY=mysecretkey -t myapp .
Architecture lock-in (windows build(runs amd64a) vs Mac build(arm64)) both can't run on each other
Independent stages run sequentially

Buildkit fixes all above problems using buildx
 
#How to create docker file from scratch
CMD: docker init -> will ask few details

use it:
new project from scratch
skip it:when you have docker file already in use

if you want to build the docker image 

docker build -t container_name .

Build a new smaller docker image by modifying the same Dockerfile and name it webapp-color and tag it lite.
cmd:docker build -t container_name:lite .


**entrypoint vs cmd in dockerfile**

CMD defines default commands or arguments that are easily completely overridden at runtime, while ENTRYPOINT sets a fixed main executable that
cannot be directly overridden by standard CLI arguments. Instead, any arguments passed at runtime are appended to the ENTRYPOINT

<img width="780" height="796" alt="image" src="https://github.com/user-attachments/assets/301ff2e7-3170-47ac-bde9-400740e27a42" />

