# Docker

This document describes how to install and use Docker on different platforms.

## Installation

If you are on Mac, Linux or Windows 10 Pro/Student editions, installing Docker
is relatively straightforward:

* [Mac](https://docs.docker.com/docker-for-mac/)
* [Linux official instructions](https://docs.docker.com/install/linux/docker-ce/ubuntu/)
* [Windows 10 Pro/Student](https://docs.docker.com/docker-for-windows/)

It is important to note that, on Mac and Windows, Docker runs under a virtual
machine so it will not use up all of the resources of your machine
(conversely, it'll equate to be working on a more limited machine). This can
be changed. But if you need more firepower, the idea is that you develop on
your laptop and scale to a server (e.g. running out of the lab).

The steps to install Docker include:

- Obtain a copy of Docker and install it:
    - `Windows10 Pro/Enterprise`: [Install Docker Desktop for Windows](https://docs.docker.com/docker-for-windows/install/)
    - `macOS`: [Get started with Docker Desktop for Mac](https://docs.docker.com/docker-for-mac/)
- Once Docker is successfully installed, make sure to enable access to your main drive (e.g. `C:\\`): 
    - `Windows10 Pro/Enterprise`: Open the preferences for Docker and click the
      "Shared Drives" tab; click on the drive you want to add and then "Apply"
    - `macOS`: this feature is automatically enabled

## Image and Container

Docker can be seen as a tool to generate a computer within your computer (the host). This creates a working environment that employs the host's resources but follows its own internal rules. 

**Image** and **Container** are two key concepts to understand Docker processes.

**Images** are files containing all the instructions to build a complete and executable version of an application, relying on the host OS kernel.

**Containers** are instantiations of images, meaning that they are instances of the images running in an isolated environment. The same image can be instantiated in multiple containers.

## Useful Docker Commands

List available images :

```shell
docker image ls
```
Pull a new image:

```shell
docker pull image-tag
```

See what containers are running (this also shows you the ID and which port is occupying):

```shell
docker ps
```
The run command instantiates an image in an isolated container. The generic command is:

```shell
docker run [OPTIONS] IMAGE[:TAG] [COMMAND] [ARG...]
```
Options that are generally added are:

```shell
--detach or -d -> Run container in background and print container ID
```

If you do not add this option to the run command you can detach from a running docker session without exiting the shell, the escape sequence *Ctrl + p* followed by *Ctrl + q* can be used. 

To re-attach the terminal to the docker session enter: 

```shell
docker attach <session_id>
```

Other commonly used flags include:

```shell
--rm  -> Automatically remove the container when it exits
```

```shell
--env or -e  -> Set environment variables.
```
Environment variables in linux act as placeholders for information stored within the system that passes data to programs launched in shells. A common environment variable is *HOME* which is associated with the path of your home directory (*/home/your-user*). By adding *-e* to the run command you pass the environment variable to the container where a certain image is running. Cases when this option is necessary will be discussed.

```shell
--publish or -p -> Publish a container’s port(s) to the host (This is key when running server-based application through Docker. A network port is the communication enpoint in a client-server model, specifying which program a communication is intended for.)

--volume or -v -> Bind mount a volume 
```

All other options are listed [here](https://docs.docker.com/engine/reference/commandline/run/)

To stop the container you have been running you need to detach from the container using the escape sequence (unless you added -d to the run command) and send the following commands: 

```shell
docker ps   to get the Container ID
docker stop <container-ID>  
```

If you have not added -rm option you also need to manually remove the container

```shell
docker rm -f <container-ID>
```

An exemplary run command is the following:

```shell
docker run -d --rm -p 8080:8787 -v ${PWD}:/data rocker/geospatial
```

After the -p option you add the ports mapping: <host port>:<container port>. In this example, docker runs an image with r studio server which uses the port 8787 by default, but the host (the server where you are running docker) has the port 8787 already in use by other services. Therefore, you need to map the port of the image (8787) with a free port of the host (in this example 8080).

After the -v option you add the paths to mount a volume:<host path>:<container path>. In this example, the working directory (the directory where you are running the command from) is mapped to a data directory in the container. ${PWD} stands for **Print Working Directory**.
 

