## Table of contents
- [docker-alpha](#docker-alpha)
  - [What is docker?](#what-is-docker)
  - [What is a container?](#what-is-a-container)
    - [Docker containers that run on Docker Engine:](#docker-containers-that-run-on-docker-engine)
  - [What docker does](#what-docker-does)
  - [Installing docker on a windows machine](#installing-docker-on-a-windows-machine)
  - [Docker images to containers](#docker-images-to-containers)
- [containers to images](#containers-to-images)
  - [basic docker life-cycle](#basic-docker-life-cycle)
  - [Command Reference](#command-reference)

# docker-alpha

learning log for docker
[command reference](#command-reference)

## What is docker?

- Docker is a container management tool.
- Enables users to create, deploy and run applications using containers.

> Containers allow a developer to package up an application with all of the parts it needs, such as libraries and other dependencies, and deploy it as one package. By doing so, thanks to the container, the developer can rest assured that the application will run on any other Linux machine regardless of any customized settings that machine might have that could differ from the machine used for writing and testing the code.

## What is a container?

- A standardized unit of software.

A container is a standard unit of software that packages up code and all its dependencies so the application runs quickly and reliably from one computing environment to another. A Docker container image is a lightweight, standalone, executable package of software that includes everything needed to run an application: code, runtime, system tools, system libraries and settings.

Container images become containers at runtime and in the case of Docker containers - images become containers when they run on Docker Engine. Available for both Linux and Windows-based applications, containerized software will always run the same, regardless of the infrastructure. Containers isolate software from its environment and ensure that it works uniformly despite differences for instance between development and staging.

### Docker containers that run on Docker Engine:

- **Standard**: Docker created the industry standard for containers, so they could be portable anywhere
- **Lightweight**: Containers share the machine’s OS system kernel and therefore do not require an OS per application, driving higher server efficiencies and reducing server and licensing costs
- **Secure**: Applications are safer in containers and Docker provides the strongest default isolation capabilities in the industry

> Container vs virtual machine comparison

![Container vs Virtual Machine](images/container_vs_virtualmachine.svg)

| Container                                                                                                                                                                                                                                                                                                                                                                                                            | Virtual Machine                                                                                                                                                                                                                                                                                                                   |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Containers are an abstraction at the app layer that packages code and dependencies together. Multiple containers can run on the same machine and share the OS kernel with other containers, each running as isolated processes in user space. Containers take up less space than VMs (container images are typically tens of MBs in size), can handle more applications and require fewer VMs and Operating systems. | Virtual machines (VMs) are an abstraction of physical hardware turning one server into many servers. The hypervisor allows multiple VMs to run on a single machine. Each VM includes a full copy of an operating system, the application, necessary binaries and libraries - taking up tens of GBs. VMs can also be slow to boot. |

## What docker does

- Carves up a computer into sealed container that runs a code
- Gets the code to adn fro from a computer.
- Docker builds containers.
- Docker has a social platform to find and share containers which are different from virtual machines.

## Installing docker on a windows machine

- Search for docker desktop and install it from exe
- We need a docker id to download docker desktop so create one.
- It might as to use windows containers instead of linux, do not check that. Linux containers will be light weight.
- Once installed to check if docker is running properly, see if the whale symbol is showing up in system tray if yes go to terminal and type command `docker info`, it will provide bunch of information about docker.
- To run a container locally use command `docker run container-name` like `docker run hello-world`
- This will check if we have docker image locally if not it wil pull from docker hub and then run in.

## Docker images to containers

- Necessary parts of an os required to run your app is called an images
- To list the docker images run `docker images`, It will list images as shown below.

```
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
hello-world         latest              bf756fb1ae65        8 months ago        13.3kB
```

- To run an image locally just use command `docker run <<image-name>>`
- docker run converts an image in to a container example `docker run ubuntu`
- docker run will also download an image if it is not found locally.

`docker run -ti ubuntu:latest bash`

- '-ti' stands for terminal interactive useful while running in terminal
- 'ubuntu:latest' says to run the latest version of ubuntu.
- 'bash' means open bash terminal after starting ubuntu.
- to stop a container terminal use 'Ctrl+D' or exit command

`docker ps`

- used to list the current running images
- after starting ubuntu the terminal will give an output as below

![running-process](images/running-processes.png)

- If we add files or do some work in an image and after completing it, if we close the container the container will not persist the files we edited by default.
- Once we exit a container, docker closes it but it will not destroy it.
- `docker ps -a` to see all the images stopped, available and running
- `docker ps -l` to see the last exited container.

# containers to images

- If we had a container in which we created a file. or some app and we need to use it as a base image, we cna create images from containers.
- `docker commit <<container-id>>` or `docker commit <<container-name>>` will create an images hash which is nothing but a new image created from your container with files in it.
- ` docker tag <<hash-generated-from-docker-commit>> <<new-container-name>>` will assign the name provided as container name for the hash provided.

- we can also use `docker commit <<container-id>> <<new-container-name>>` to generate a new image from an exiting stopped container.

## basic docker life-cycle

![docker life cycle](images/docker-lifecycle.svg)














---

## Command Reference

- `docker images` : to see images that are available locally
- `docker run <<image-name>>` : To run an image, converts an image in to a container, docker run will also download an image if it is not found locally.

- `docker run -ti ubuntu:latest bash`

    - '-ti' stands for terminal interactive useful while running in terminal
    - 'ubuntu:latest' says to run the latest version of ubuntu.
    - 'bash' means open bash terminal after starting ubuntu.

- `docker ps` : used to see running images locally.
- `docker ps -a` to see all the images stopped, available and running
- `docker ps -l` to see the last exited container.
- `docker commit <<container-id>>` or `docker commit <<container-name>>` will create an images hash which is nothing but a new image created from your container with files in it.
- ` docker tag <<hash-generated-from-docker-commit>> <<new-container-name>>` will assign the name provided as container name for the hash provided.
- `docker commit <<container-id>> <<new-container-name>>` to generate a new image from an exiting stopped container with name assigned to it.
