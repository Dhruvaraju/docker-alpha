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
    - [Defining docker image through docker file](#defining-docker-image-through-docker-file)
    - [Docker images are readonly](#docker-images-are-readonly)
    - [Image Layers](#image-layers)
    - [Running Containers in detached or attached mode](#running-containers-in-detached-or-attached-mode)
    - [Interactive terminal](#interactive-terminal)
    - [Removing containers and images](#removing-containers-and-images)
    - [Copying files in and out of a containers](#copying-files-in-and-out-of-a-containers)
    - [Naming a container and images](#naming-a-container-and-images)
    - [Pushing images to docker-hub](#pushing-images-to-docker-hub)
    - [Pulling images from docker-hub](#pulling-images-from-docker-hub)

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
- Gets the code to and fro from a computer.
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

### Defining docker image through docker file

- Take a sample project, A node app is taken as example.
- Create a file with name `Dockerfile` without any extension.
- We need to add few parameters as shown below to the file

```docker
FROM node
WORKDIR /app
COPY . /app
RUN npm install
EXPOSE 80
CMD ["node", "server.js"]
```

- `FROM node` refers that create an image with base as node image.
- `WORKDIR /app` To specify the working directory inside container.
- `COPY . /app` To copy all the code in the folder to app folder in container.
- `RUN npm install` to run npm install while building the container.
- `EXPOSE 80` for documentation purpose only stating which port we are exposing.
- `CMD ["node", "server.js"]` to run `node server.js` command on starting the container

> Once after defining the file run `docker build .` command which will create an image and give its name.

- To expose a port on container that need to run on a local port with `-p` switch
- In the below command we are attaching local port 3000 with container port 80

```dos
docker run -p 3000:80 09cee1f75310aab7bf58ad883454
```

### Docker images are readonly

- Once you run a docker build the created image is readonly.
- If you make changes to the code then you have to rebuilt the images again else the changes will not reflect.
- Every time you rebuild a new image will be created.

### Image Layers

- Every line in the `Dockerfile` is considered as a layer.
- When there is a change in the code the lines which will cause updated result will be run,
- More specifically all the changes that are after the first line which causes an updated result will be run.
  **Example**

```docker
FROM node
WORKDIR /app
COPY . /app
RUN npm install
EXPOSE 80
CMD ["node", "server.js"]
```

When a code change is made while considering the above file, `COPY . /app` line will cause an updated result.
So every line under it will be executed on building the image.
We can fine tune it as:

```docker
FROM node
WORKDIR /app
COPY package.json /app
RUN npm install
COPY . /app
EXPOSE 80
CMD ["node", "server.js"]
```

So every time when there is a code change and an image is build only from the line `COPY . /app`.

> To know more about any docker commands add `--help` at the end of the command.

**Stopping and Restarting containers**

- To restart an exited container use `docker start <<container name>>`
- Run docker ps -a to get the list of containers and their names.
- When we restart a container we will not get the logs terminal back.

### Running Containers in detached or attached mode

**Attached Mode:** When a container is initially started with `docker run` command, we will be able to follow the logs after the container starts.
This is attached mode.

**Detached Mode:** While starting a container using `docker start` command, we will not be able to follow any logs, this is called detached mode.

- `docker run -d <<Image hash>>` to start a container in detached mode.
- `docker start -a <<container name>>` will start a container in attached mode.
- `docker attach <<name_of_running_container>>` will bring up terminal for the container.
- `docker logs <<name_of_running_container>>` will bring up the logs of a container.
- To follow logs use the -f switch `docker logs -f <<name_of_running_container>>`

### Interactive terminal

- To run in attached mode might not be sufficient when you want to pass some info via command line.
- So we use interactive mode `-i` is used as `docker run -i <<image_name>>`
- Generally we attach a terminal also to this as below
  - `docker run -t -i <<image_hash>>`
  - `docker run -ti <<image_hash>>`
- To use the same for starting a container we have to use `docker start -ai <<container_name>>`

### Removing containers and images

- Use `rm` switch to remove container `docker rm <<container_name>>`
- We cannot remove a running container.
- We can remove multiple containers by passing their names as a command line arguments with space

```dos
docker rm <<container_001>> <<container_002>>
```

- `docker container prune` will remove all stopped containers.
- `docker images` will give list of all images
- `docker rmi <<image_hash>>` to remove a specific image.
- `docker image prune` will remove all the images for which there is no container present (running or stopped).
- `docker image prune -a` to remove all the name tagged containers.

> Removing stopped containers automatically can be done by using the `--rm` switch
> `docker run -p 8000:80 --rm <<image_hash>>`

- To inspect and identify additional details about an image use `docker inspect image <<image_id>>`
- An example output will be like this

```
[
    {
        "Id": "sha256:4208f648b801dee96f0ee0434180273601dc60f1266dd1404e8b5075695be014",
        "RepoTags": [],
        "RepoDigests": [],
        "Parent": "",
        "Comment": "buildkit.dockerfile.v0",
        "Created": "2022-05-28T09:11:52.4167184Z",
        "Container": "",
        "ContainerConfig": {
            "Hostname": "",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": null,
            "Cmd": null,
            "Image": "",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": null
        },
        "DockerVersion": "",
        "Author": "",
        "Config": {
            "Hostname": "",
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
                "NODE_VERSION=18.2.0",
                "YARN_VERSION=1.22.19"
            ],
            "Cmd": [
                "node",
                "server.js"
            ],
            "ArgsEscaped": true,
            "Image": "",
            "Volumes": null,
            "WorkingDir": "/app",
            "Entrypoint": [
                "docker-entrypoint.sh"
            ],
            "OnBuild": null,
            "Labels": null
        },
        "Architecture": "amd64",
        "Os": "linux",
        "Size": 1001705909,
        "VirtualSize": 1001705909,
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/j51ghtpdqk2z8b2xtcmlqr1dd/diff:/var/lib/docker/overlay2/z760dwglkpbbj86xdbdexr333/diff:/var/lib/docker/overlay2/8011d33937aa2bd64597c20dee75121f9f73dad13815ce6bdb21da6a2c523aea/diff:/var/lib/docker/overlay2/8dfd99e8e043af08d23f5c5d1043e6f6f3fb86cf534d3f9b1fcebdaca0096156/diff:/var/lib/docker/overlay2/5345a20ead648d33b60c665b7431d22bce5857681ac2de248908bdb97b7d18d7/diff:/var/lib/docker/overlay2/501793094e8cfb0cadae13060143d23132410ccee26aa578f98622b06ab99462/diff:/var/lib/docker/overlay2/67cd528a1cd99258b45fbdfa9b220b83ffa3e458e0215389cb46929755fd2219/diff:/var/lib/docker/overlay2/a378924b426cefeff919a0a94b33e783798c234e36802292b5153a11a497ccdf/diff:/var/lib/docker/overlay2/18942caa53c84169a20e97b77039324372b293a532509f080fb1bf13f14e5ba4/diff:/var/lib/docker/overlay2/40345eaa3e31f477e796160cbf542681dad7be69b2f39208362b920c5a123626/diff:/var/lib/docker/overlay2/bcfb10db7d11649506585b65209b6caea4658703188960afe2b04f8a402a6f15/diff",
                "MergedDir": "/var/lib/docker/overlay2/a6ieu3sn7389a7gp2cfdbqy0b/merged",
                "UpperDir": "/var/lib/docker/overlay2/a6ieu3sn7389a7gp2cfdbqy0b/diff",
                "WorkDir": "/var/lib/docker/overlay2/a6ieu3sn7389a7gp2cfdbqy0b/work"
            },
            "Name": "overlay2"
        },
        "RootFS": {
            "Type": "layers",
            "Layers": [
                "sha256:e7597c345c2eb11bce09b055d7c167c526077d7c65f69a7f3c6150ffe3f557ea",
                "sha256:7dbadf2b9bd82a7447533776d0c8de6687cfcf241d3aa993ed8a86ad1347c6e0",
                "sha256:9177197c67d08b25357b0b5ba8f7b944f321970dddbbe93b36cb726e9bdfd678",
                "sha256:ee509ed6e976cdad5adda963902f78e442ea5fc05f955bd2c2c9026789f84b42",
                "sha256:2fbabeba902e7f7c521f478f855b738d91bd4f2435de223a89fa5f4b2369065a",
                "sha256:9e8a8e4e0b9201a5bf839068744a6ffb1e8f66c26600e5c733a6dead057aa36c",
                "sha256:00a6e2ec123c791ab91d4633df70ac42df7a62aee1c8f1646dbcb308c7bd3d07",
                "sha256:5cd94bddda4ddea4a07cd197ccaed3d758ab9a96f406529a237bd8b7a84a92fb",
                "sha256:9822b42ab2ee535ba505c15eeb5e93a06c8b99bc0c99bfb7bc2d99d51ca49fbb",
                "sha256:6a534ce25f8bd94c2f358261fd108a76717d72dd39f9d2d24df00e23f4f5208e",
                "sha256:b039435553edc7f6edd30870021a7fe71d9e96ad75ddff8e92ce0f21043fc26d",
                "sha256:d93c17da8b7b985731552b2eb357dcce5fe305f055a1ae842d8655573bd39db5"
            ]
        },
        "Metadata": {
            "LastTagTime": "0001-01-01T00:00:00Z"
        }
    }
]
Error: No such object: image
```

### Copying files in and out of a containers

- To copy contents from local location to a running container and vice versa use the `cp` switch
- `docker cp test/abc.txt <<container_name>>:folder_name/` to move file from local to container

### Naming a container and images

- To name a container use `--name` switch while using docker run command
  ```dos
  docker run -p 8000:80 --name goalapp 4208f648b801
  ```
- To name a image we need to add the name while building the image.

```dos
 docker build -t goalapp:latest .
```

- we need to use the name and tag combination.

**Advantages of Sharing Images:**

- Easy to share the environment for installation.
- No need to install or uninstall dependencies.
- To have exact environment for dev and production.
- Same machine can have multiple versions of same software like python or node.
- Effective usage of resources.

> To share environment, we can share docker file or share the built image directly.

### Pushing images to docker-hub

- Create an account in docker hub
- Create a repository, name the repository.
- Now we can push our local images to repository.
- Rename the image which you want to push as `docker-hub-username/repository`
- We can rename images in 2 ways
  - build again with `-t` and use the name as mentioned.
  - `docker tag <<ols_image_name>> <<new_image_name>>`
- Using the tag command will create a new image and keep the old image as it is.
- Now in terminal use `docker login`. Provide username and password.
- Log in is required only once.
- `docker logout` is used to logout from a terminal.
- `docker push <<image_name>>` is used to push an image to docker hub.
- we can also push with tag names `docker push <<username/image-name:tag>>`
- If we do not provide a tag name, it will add latest tag by default.

### Pulling images from docker-hub

- `docker pull <<image_name>>` is used to pull image from docker hub.
- `docker run <<image_name>>` will check if the image is available locally if not it will pull it from docker hub.
- Run command will not fetch the latest changes after the first time.
  - It will only run the available version locally.
- Docker pull will get the most recent version all the time.
