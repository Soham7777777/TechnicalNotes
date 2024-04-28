# Docker Notes:
---

- <strong>Basically docker is a super light-weight
Virtual machine which has all necessery
programs and dependencies in order to run
your program.
 
- When we setup big application
which has UI, database, and lots of
microservices we need to do a setup and
each of this services need some kind of program
to work like JAVA and so on. And to setup
everything and installing correctly in the
new server probably would take days.
But most of the time versions are not matching
and other problems so you need to debug it
which is very time consuming. So what dockers
is doing, we create image and we are explaining
what exactly is needed for our program. 
So basically we are saying these are the dependencies,
this is our program where it's coming from and
than we we need to create a container with
our program we just pulling from Docker registry.
And container with a program will be running and
it can be bootsrap in few seconds. So we just
download and start in few seconds and
whoever will be requesting that image will
have exactly same setup. Biggest advantage of
dockers is to ability to have exactly same
setup everywhere and also you can very quickly
bootstrap the whole infrastructure.
And containers can build very quickly. 

- Its almost like a virtual environment of linux OS (playground I like to call) for an application, just like python virtual environment for python applications.

</strong>

## Important commands:

- the "docker" commands are used to manage single containers

- the "docker-compose" commands are used to manage setup of multiple containers working together
- docker ps # current containers
- docker run # create and start the container
- docker create # create container
- docker exec # to run commands in container for once
- docker volume # create a docker volume
- docker network # create a docker network
- docker rm # remove container 
- docker images # list the images
- docker rmi # remove image
- docker build # build a new image from dockerfile
- docker push # push your image to docker repo
- docker pull # download an image from docker repo
- docker commit # create an image from container
- docker stop # give container id as input to stop the container

**Anatomy of some docker commands:**
```
sudo docker run -d -p 1024:1024 --rm --user root -it image_name
```
- here -d means run in detached mode, means run container in background
- here -p means map port, the sysntax is host_port:container_port means whichever port is used in container will mapped to host_port in main OS.
- here --rm means automatically remove the container when we remove the container
- the --user root means run the CMD command as root
- here -it means "in open" and "allocated tty" 

```
sudo docker build -t system_health_check .
```
- the "docker build ." will find a Dockerfile in . directory and build it
- the "-t tag_name" to give this container a name

```
sudo docker exec --user root -it [id] /bin/bash
```
- the `docker exec command` will let you execute the command inside the running container 


## About Dockerfile:

- The Dockerfile specifies all commands to configure the container when its building itself, for example :
```
	- RUN apt-get update
	
	- WORKDIR /home/ctf # to set the current working directory
	
	- COPY from_host to_container # copies the file/directory from host (the main OS) to the container OS
	
	- USER user # switch the user in container OS
	
	- CMD command # the command to execute when container is activated, it can be a command to start a server 
```

## Server side:

- If you are accessing a server that is hosted on your local network, you always need to allow the firewall to open the port in order to access that server from a docker container.

- If you are serving a service in docker container then you need to map the container port with machine's port.

 	
