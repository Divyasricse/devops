``` text
Physical server--->vm----->containers
-vm solve some problems of pysical server(modern things done by devops),conatiner solve some problems of vm
-u can create containes on top of vm  as well as pysical server

```
<img width="617" height="354" alt="image" src="https://github.com/user-attachments/assets/fd1c0ed7-1371-4a6b-b1d6-9a2d76fbc0ce" />

```



**2.Container**

A Docker container is a lightweight, standalone, and executable package of software that includes everything needed to run an application, including the code, runtime, system tools, libraries, and settings. 

**3.Container vs virtual machine**
Containers and virtual machines are both technologies used to isolate applications and their dependencies, but they have some key differences:

Docker containers virtualize the operating system to share the host kernel, while Virtual Machines (VMs) virtualize physical hardware to run a completely separate guest operating system.when comes to security vm are secure because it is using independent os.

**4.why docker containers are lightweight?**
Docker containers are lightweight because they didn't have a full operating system and they use the resources from base  operating system.Additionally, Docker containers are designed to be minimal, only including what is necessary for the application to run, further reducing their size.
or
Containers are lightweight in nature. As they don't have complete operating systemContainer have minimal operating system that is base image. Container is a package/bundle of application that has all the application library and system dependencies like python. 
Very easy to ship and transport as they are lightweight in nature. As they use resources from base operating system.
or 
they share the host operating system's kernel instead of hardware-virtualizing a full guest operating system

**4.difference between images and containers in docker**
The core difference is that a Docker image is a static, read-only template containing your application code and environment configurations, while a Docker container is a live, executable runtime instance created from that image
<img width="704" height="410" alt="image" src="https://github.com/user-attachments/assets/3f90f92a-edc5-4312-8123-56b15932881c" />


```

<img width="1383" height="721" alt="image" src="https://github.com/user-attachments/assets/5171809b-dd32-4229-a6a2-65fc6f43bc83" />

```

The above picture, clearly indicates that Docker Deamon is brain of Docker. If Docker Deamon is killed, stops working for some reasons, Docker is brain dead :p (sarcasm intended).

**Docker LifeCycle**
We can use the above Image as reference to understand the lifecycle of Docker.

There are three important things,

docker build -> builds docker images from Dockerfile
docker run -> runs container from docker images
docker push -> push the container image to public/private regestries to share the docker images.
```


<img width="1589" height="830" alt="image" src="https://github.com/user-attachments/assets/c067271d-5b56-40d8-963a-0ab76506256c" />

```

- docker file (we will give some commands) from these it will bulid image ,by using image it will run conatainer.these all commands is handles by docker engine

**Commands**
1.Docker run :Runs a Docker container.
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

2.Lists running containers on the host machine.
Cmd: docker ps
 
3.Show all containers (running + stopped)
Cmd: docker ps -a

4.Stop running docker container
Cmd: docker stop container-name
Note:must use container name or container id
 
5.Remove docker container
Cmd: docker rm container-name
output:it will show container name then it is successfully removed

6.List docker images:
Cmd: docker images
Note :we will get name id tag size created date here

7.Remove docker image
CMD: docker rmi image-name(we can't delete it with image id)
Note: make sure no containers are running on that image before you remove image

docker pull command downloads container images from a registry (like Docker Hub) to your local machine. 

Core Examples
Default Pull: Downloads the image with the :latest tag.bash
docker pull ubuntu
Use code with caution.Specific Version: Downloads a precise version using a tag.bash
docker pull ubuntu:22.04

**docker pull only downloads an image, while docker run downloads, creates, and starts a container from that image.**

8.Keep container sleep for 5 seconds
CMD: docker run image-name sleep 5

9.How to run a command on already running container(change a config, check a log, run a quick query)
CMD: docker exec container-id cat /etc/hosts

 
10.Run a container on Detach mode(it will run on background, directly gives the id .and stops automatocally.so you can able to perform other actions)
CMD: docker run -d container-name or ID

note:if you want to give name to container then run:
CMD: docker run -d --name divya container-name or ID

 
11.Run a container on attach mode(here it won't stop we need to press contrl+c to get out of it or quit)
CMD: docker run container-id

12.if you want to attach back to running container
CMD: docker attach container-id
note Lcontainer id is big number like:asdfghjkrtyu so just give asdfg so it will understand because it is unique

The docker inspect command returns low-level, detailed configuration data for virtually any Docker object—such as containers, images, volumes, and networks—in a structured JSON format.
docker inspect <container_name_or_id>

**Image tags**
what is image:Lists docker images on the host machine.
-one image can be used by different cointainers

1.docker run redis
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


difference between docker hub and github
GitHub is a code hosting platform used for storing source code and tracking revisions, whereas Docker Hub is a container registry used for storing, sharing, and managing compiled Docker 
container images

To check the status of docker wheather it is running or not
cmd:sudo systemctl status docker
output:active

#Set environment variable
CMD: docker run -e APP_COLOR=red --name myapp myapp-image
 
#Docker Compose: when we have multiple docker container to run at once(will there a connnectivity between this files), will go for docker compose
CMD: docker compose up - latest
CMD: docker-compose - old legacy - end of life

CMD: docker compose -f compose.yaml up
 
Compose.yml
Services:
Web(these are names given to image):ex:docker  run -d --name=web imagename
Image:  "imagename"
Database:
Image: "db image name"
Messaging:
Image: "redis:alpine"
links:-Database
Orchestration:
Image: "ansible"

if u r linking one to other use
cmd:docker run -d --name=web --network database web

First, create a Docker network named clicknet. This should be a bridge network
docker network  create --driver bridge  clicknet

Create a redis database container named redis using the image redis:alpine, running in the background.
Attach it to the clicknet Docker network that you created in the previous task.
cmd:docker run -d --name=redis --network clicknet redis:alpine


Next, create a simple container named clickcounter using the image kodekloud/click-counter, attach it to the clickcounter network from the earlier task, and then expose it on host port 8085.
The clickcounter app runs on port 5000.
cmd:docker run -d --name=clickcounter --network clicknet -p 8085:5000 kodekloud/click-counter


Delete the redis and clickcounter containers, alongwith the clicknet network you had created
root@docker-host ~ ➜  docker stop f3d1b5f07032(stop the container)
f3d1b5f07032

root@docker-host ~ ➜  docker rm f3d1b5f07032(remove the container)
f3d1b5f07032

root@docker-host ~ ➜  docker rmi kodekloud/click-counter:latest(remove image)
Untagged: kodekloud/click-counter:latest
Deleted: sha256:530e4532a718e8f5cbda05844a6c0638ebe8898fa4c4307ee6afbdd5d1f213db

root@docker-host ~ ➜  docker network rm clicknet(to remove network)
clicknet


Create a compose.yaml file in the /root/clickcounter directory. Once done, bring up the stack using Docker Compose.
The compose file should have the following exact specification:
redis service - Use redis:alpine image name
clickcounter service - Use the kodekloud/click-counter image name; the app runs on port 5000 and should be exposed on host port 8085.

vi compose.yaml
services:
 redis:
   image: redis:alpine
 clickcounter:
   image: kodekloud/click-counter
   ports:
     - "8085:5000"


**Docker Engine**
When we install docker on linux host we actually installing 3 different components:
 
1.Dockerd (daemon ): docker daemon is a background process that managers docker objects
Images
Containers
Volumes
networks
 
2.Rest API (server): is the API interface that programs can use to talk to the daemon and send instructions.
 
3.Docker (cli): The CLI talks to the daemon through the REST API server: docker run, docker stop, docker rmi
 
Underneath the daemon: Architecture
Docker cli
Dockerd daemon
Containerd runtime
Runc oci
Linux kernel . Namespaces . Cgroups
 
Kubernetes directly taking to containerd, it won't talk to dockerd
 
Cgroups . Resource limits
CMD: docker run --cpus=0.5 ubuntu -> container doesn't take up more than 50% of the host CPU at any given time.
CMD: docker run --memory=100m ubuntu -> container limited to 100 megabytes
 
Docker follows layered architecture -> line by line it adds
 
Persistent Volumes:
we have 2 types of mounds (volume(from any volume) and bind(from any host))
Volume mount: from /var/lib/docker/volumes
Create Volume: docker volume create datavolume
CMD: docker run -v datavolume:/var/lib/mysql mysql(if u want to create my sql inside the datavolumes)
Bind mount: from any host path(if ur mysql is present in other place(/data/mysql) and now u want to send to other path(/var/lib/mysql mysql) use below cmd)
Cmd: docker run -v /data/mysql:/var/lib/mysql mysql
-v is old formate
--mount is latest more readable
 
CMD: docker run --mount type=bind, source=/data/mysql, target=/var/lib/mysql mysql
 
Storage Driver:
Manage - file system
Create - writable layer
Implement - copy-on-write
 
Available:
Overlay2
Btrfs
Zfs
Fuse-overlayfs
vfs


1.Run a mysql container named mysql-db using the mysql image.
Set the database password to db_pass123
cmd:docker run -d --name mysql-db -e MYSQL_ROOT_PASSWORD=db_pass123  mysql
-e-refer to env variable

2.Run a mysql container again, but this time map a volume to the container so that the data stored by the container is stored at /opt/data on the host.
Use the same name : mysql-db and same password: db_pass123 as before.
Note: Mysql stores data at /var/lib/mysql inside the container.
cmd:docker run -d --name mysql-db -e MYSQL_ROOT_PASSWORD=db_pass123 -v /opt/data:/var/lib/mysql mysql


**Networking**

-2 types of networking
1.container 1 talks with conatainer 2
2.container 1 isolates with conatainer 2
 
1.bridge networking--default networking 
2.host network
3.overlay networking(complex)


Run a container named alpine-2 using the alpine image and attach it to the none network.
cmd:docker run -d --name alpine-2 --network=none alpine

Create a new network named wp-mysql-network using the bridge driver. Allocate subnet as 182.18.0.0/24 and configure gateway as 182.18.0.1.
cmd:docker network create --driver bridge --subnet 182.18.0.0/24 --gateway 182.18.0.1 wp-mysql-network 

Deploy a mysql database using the mysql:8.4 image and name it mysql-db. Attach it to the newly created network wp-mysql-network.Set the database password to db_pass123 using the MYSQL_ROOT_PASSWORD environment variable.
cmd:docker run -d --name mysql-db --network wp-mysql-network -e MYSQL_ROOT_PASSWORD=db_pass123 mysql:8.4

Deploy a web application named webapp using the kodekloud/latest-webapp-mysql image.Expose the container’s port 8080 to port 38080 on the host.The application makes use of two environment variable:
DB_Host with the value mysql-db.DB_Password with the value db_pass123.Make sure to attach it to the newly created network called wp-mysql-network.Note: We need to link the MySQL and webapp containers.
cmd:docker run -d --name webapp --network wp-mysql-network 
-e DB_Host=mysql-db -e DB_Password=db_pass123 -p 38080:8080 kodekloud/latest-webapp-mysql 

**Networking**
CMD: docker network ls
CMD: docker network create \
--driver bridge \
--subnet 182.18.0.0/16 \
custom-isolated-network

to delete network 
cmd:docker network rm network_name
 
DNS: this is also why service name DNS works in docker compose always creates a user-defined
CMD: docker network create --driver bridge my-app
CMD: docker run -d --name=mysql --network=my-app mysql
CMD: docker run -d --name=web --network=my-app my-webapp
 
Network Namespaces: separate namespaces for each container
Virtual Ethernet Pairs: Connect containers together


A Docker registry :is a centralized storage and distribution system designed to manage, share, and version container 
images

diff bet docker hub( cloud SaaS platform provided directly by Docker.) and registory(open source)
The core difference is that a Docker Registry is a software application used to store and distribute container images, while Docker Hub is a specific, cloud-based public implementation of a registry managed by Docker Inc

Let's practice deploying a registry server on our own.
Run a registry server with name equals to my-registry using registry:2 image with host port set to 5000, and restart policy set to always.
cmd:docker run -d --name my-registry -p 5000:5000 --restart always registry:2


Now, it's time to push some images to our registry server. Let's push two images for now: nginx:latest and httpd:latest.
Note: Don't forget to pull them first.
ans:pulling the base image and then pushing it to your private registry.
<img width="713" height="599" alt="image" src="https://github.com/user-attachments/assets/9bbc5706-6f9b-42d1-9496-d14e12c291e6" />
<img width="698" height="614" alt="image" src="https://github.com/user-attachments/assets/ee90bb4b-a0fb-4b07-b079-92d92af1bc16" />

Dockerfile
Dockerfile is a file where you provide the steps to build your Docker Image.

```
