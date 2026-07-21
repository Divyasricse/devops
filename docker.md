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
CMD: docker rmi image-name
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
 
##Run a container on Detach mode(here it won't stop we need to press contrl+c to get out of it or quit)
CMD: docker run container-id

#if you want to attach back to running container
CMD: docker attach container-id
note Lcontainer id is big number like:asdfghjkrtyu so just give asdfg so it will understand because it is unique
