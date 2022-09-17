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
  - [Managing Data and working with Volumes](#managing-data-and-working-with-volumes)
    - [Types of data](#types-of-data)
    - [Volumes](#volumes)
    - [How to add a volume to a container](#how-to-add-a-volume-to-a-container)
    - [Named volumes](#named-volumes)
    - [Bind mount](#bind-mount)
    - [Merging multiple volumes](#merging-multiple-volumes)
    - [Windows, WSL 2 & Bind Mounts](#windows-wsl-2--bind-mounts)
      - [There are three main workarounds & solutions:](#there-are-three-main-workarounds--solutions)
    - [Making volumes readonly](#making-volumes-readonly)
      - [Managing docker volumes](#managing-docker-volumes)
    - [COPY vs Bind Mounts](#copy-vs-bind-mounts)
    - [Docker ignore file](#docker-ignore-file)
    - [Environment Variables](#environment-variables)
    - [Build Args](#build-args)
  - [Networking with containers](#networking-with-containers)
    - [Container to internet communication](#container-to-internet-communication)
    - [Container to host machine](#container-to-host-machine)
    - [Container to container communication](#container-to-container-communication)
    - [Based on Ip address:](#based-on-ip-address)
    - [Based on Network](#based-on-network)
    - [Docker Network Drivers](#docker-network-drivers)
    - [How do docker parse ip addresses](#how-do-docker-parse-ip-addresses)
  - [Building Multi Container apps with docker](#building-multi-container-apps-with-docker)
    - [Adding networks to the apps](#adding-networks-to-the-apps)
    - [Persisting data with volumes](#persisting-data-with-volumes)
  - [Docker-compose](#docker-compose)
    - [What is docker compose](#what-is-docker-compose)
    - [What is not docker-compose](#what-is-not-docker-compose)
    - [Creating a compose file](#creating-a-compose-file)
    - [Installing docker-compose on a linux machine](#installing-docker-compose-on-a-linux-machine)

# docker-alpha

learning log for docker

### What is docker?

- Docker is a container management tool.
- Enables users to create, deploy and run applications using containers.

> Containers allow a developer to package up an application with all of the parts it needs, such as libraries and other dependencies, and deploy it as one package. By doing so, thanks to the container, the developer can rest assured that the application will run on any other Linux machine regardless of any customized settings that machine might have that could differ from the machine used for writing and testing the code.

### What is a container?

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

### What docker does

- Carves up a computer into sealed container that runs a code
- Gets the code to and fro from a computer.
- Docker builds containers.
- Docker has a social platform to find and share containers which are different from virtual machines.

### Installing docker on a windows machine

- Search for docker desktop and install it from exe
- We need a docker id to download docker desktop so create one.
- It might as to use windows containers instead of linux, do not check that. Linux containers will be light weight.
- Once installed to check if docker is running properly, see if the whale symbol is showing up in system tray if yes go to terminal and type command `docker info`, it will provide bunch of information about docker.
- To run a container locally use command `docker run container-name` like `docker run hello-world`
- This will check if we have docker image locally if not it wil pull from docker hub and then run in.

### Docker images to containers

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

## containers to images

- If we had a container in which we created a file. or some app and we need to use it as a base image, we cna create images from containers.
- `docker commit <<container-id>>` or `docker commit <<container-name>>` will create an images hash which is nothing but a new image created from your container with files in it.
- ` docker tag <<hash-generated-from-docker-commit>> <<new-container-name>>` will assign the name provided as container name for the hash provided.

- we can also use `docker commit <<container-id>> <<new-container-name>>` to generate a new image from an exiting stopped container.

### basic docker life-cycle

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

## Managing Data and working with Volumes

### Types of data

- **Application** Code and environment hence stored in images. Generally Read-only data.
- **Temporary App Data** User input provided during runtime. Generally read and write data.
  - Not required once container is stopped.
  - Hence stored in container.
- **Permanent data** App data like profile of user, log in information. Read and write data.
  - Should be available even after closing container.
  - Data that should be stored in database.
  - Usually stored in containers and volumes.

### Volumes

- Volumes are folders on host machine which will mounted(made available or mapped) into containers.
- Volumes persists even after container is shut down.
- When a container is restarted, if a volume is mounted, container will have access to the data in volume.
- Container have read-write access to the volume.

### How to add a volume to a container

- Add `VOLUME` array in the Dockerfile
- the `VOLUME` array takes the folders in the container, which need to be made as volumes

**example dockerfile**

```
FROM node:14

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

EXPOSE 80

VOLUME [ "/app/feedback" ]

CMD [ "node", "server.js" ]
```

- In the above docker file "/app/feedback" folder will be mapped to a folder in host machine.
- We will not know which folder in host machine, as it is mapped by docker.
- Even after adding a volume section the files might not be persistance, because every time we start the container a new volume will be created.
- To avoid the above issue we used named volumes.
- Volume mentioned in the dockerfile is called as anonymous volume. This will be detached from the container once stops.
- These are also called as unnamed volumes.
- We saw, that anonymous volumes are removed automatically, when a container is removed.
- This happens when you start / run a container with the --rm option.
- Command to see all volumes is `docker volume ls`
- To remove a volume use command `docker volume rm <<volume_name>>` or `docker volume prune`

If you start a container without that option, the anonymous volume would NOT be removed, even if you remove the container (with docker rm ...).
Still, if you then re-create and re-run the container (i.e. you run docker run ... again), a new anonymous volume will be created. So even though the anonymous volume wasn't removed automatically, it'll also not be helpful because a different anonymous volume is attached the next time the container starts (i.e. you removed the old container and run a new one).
Now you just start piling up a bunch of unused anonymous volumes - you can clear them via docker volume rm VOL_NAME or docker volume prune.

### Named volumes

- A volume with a name assigned to it is a named volume.
- We will not add this in docker file
- A named docker volume can be created with command `docker run -v <<volume_name>>:<<folder_path_in_container>> <<image_name>>`

```docker
docker run -p 8000:80 --name example-docker --rm -v volume_example:/app/feedback node-app:latest
# a volume will be created with name volume_example
docker run -p 3000:80 --rm --name volume-eg -v feedback:/app/feedback node-app # feedback is volume name
```

- A named volume can be attached to other containers also

### Bind mount

- Volumes can persist data but the folder to which they will be mapped cannot be managed by developers.
- Bind mounts allow user to choose which folder the mapping need to be done on host system.
- This is more suitable for persistent editable data, like source code.
- A named Volume will help with persistent data but not for editing because we do not know which path the folder is saved to.
- Bind mount is not specific to image, it is specific to container.
- We cannot add a bind mount using Dockerfile.

Adding bind mount

```
docker run -p 8000:80 --name <<container_name>> -v feedback:/app/feedback -v <<absolute-path-host-system-path>>:<<path-on-container>> <<image_name>>

# Example
docker run -p 8000:80 --name eg-app -v feedback:/app/feedback -v "D:\devworks\docker\docker-alpha\":/app node-image
```

While using folder names if the folder name has any spaces place it in between double quotes.

> If you don't always want to copy and use the full path, you can use these shortcuts:

macOS / Linux: -v $(pwd):/app

Windows: -v "%cd%":/app

> I a linux or mac machine while using bind mounts make sure the parent folder is checked under resources file sharing.

### Merging multiple volumes

Consider a node app, in the docker file we have done a `npm install`, then copied the code to code in your current working directory to the `/app` location in container.

While starting the application you have added a named volume and bind mount to `/app` folder to the current working directory like below.

```commandline
docker run -p 8000:80 --rm -d --name feedback-app -v feedback:/app/feedback -v "C:/user/node-app":/app feedback:volume
```

The app might not start because the node modules folder will be overwritten by contents of `C:/user/node-app`. Because docker executes commands in docker file then it will overwrite them with the volumes.

To avoid the above problem we can make use of an unnamed volume like below:

```commandline
docker run -p 8000:80 --rm -d --name feedback-app -v feedback:/app/feedback -v "C:/user/node-app":/app -v /app/nodemodules feedback:volume
```

now node-modules folder will not be deleted. Which ever volume will have a deep routed folder path that path will be finally kept or persists.

This is how multiple modules can be merged.

### Windows, WSL 2 & Bind Mounts

- While using a windows machine file changes might not propagate as docker runs on a wsl(windows subsystem for linux)
- Few changes need to be made to avoid this

- When working with Windows and WSL2, you can encounter issues when it comes to working with "Bind Mounts".

> Please note, that these instructions only apply if you are using Windows with WSL2 and Docker. NOT if you're not using WSL2!

- Specifically, file change events might not be propagated to the container - i.e. processes running inside of the container (like "nodemon") are not made aware of file changes.

#### There are three main workarounds & solutions:

1. Simple Solution
   This solution is "quick and dirty" but it does the job.
   For Nodemon, you can switch from the "notify me" mode to the "I look for changes
   myself mode" by changing the starting script in the package.json file like this:

```javascript
{
"scripts": "nodemon -L server.js"
}
```

For React apps (where we also want to listen to file changes), you can pull a similar trick.
Simply add an environment variable to your docker run command:

```commandline
docker run -e CHOKIDAR_USEPOLLING=true ...other-options <react-image-id>
```

2. Mount Your Windows Paths Into WSL2
   This solution might require an update to the latest Windows Insiders version - to be
   precise, you need at least this version: https://blogs.windows.com/windows-insider/202
   0/09/10/announcing-windows-10-insider-preview-build-20211/
   Once you got that, you can follow the steps outlined in this article to mount your
   Windows path / folder into WSL2: https://devblogs.microsoft.com/commandline/accesslinux-
   filesystems-in-windows-and-wsl-2/
   As a result, file changes will be forwarded into the container.

3. Embrace WSL2
   This is theoretically the best solution, but it's also an advanced solution.
   "Embrace WSL2" means, that you should use WSL2 as your default terminal / command
   prompt (instead of the regular Windows command prompt).
   Do all your work from inside WSL2 (and therefore from inside Linux) and you'll not face
   any issues.
   What does that mean?
   Launch your Linux distribution (and therefore WSL2) via the Windows starting menu.
   This opens up a Linux shell which allows you to work with the Linux distribution which
   was installed on your Windows system. You also got full access to its internal file system
   there.
   Navigate around via the ls (list content of the current directory) and cd (change
   directory) commands.
   You can create a new directory via the mkdir command.
   For example, you could create a docker-practice directory like this:

```commandline
cd ~
mkdir docker-practice
cd docker-practice
```

Now when it comes to working with these folders (e.g. to create your Dockerfile and your
project files in there), the following article can be a great next step: https://www.docker.c
om/blog/docker-desktop-wsl-2-best-practices/.

### Making volumes readonly

- Volumes are by default read and write.
- We can restrict it to be read only by adding `:ro` at the end of declaring a bind mount

```commandline
# Example bind mount
docker run -v C:/temp:/app <<image_name>>
# this can be made read only by
docker run -v C:/temp:/app:ro <<image_name>>
```

- In the above example entire app folder is made as readonly
- we can make sub folders to read write by mentioning them as another anonymous volumes.

```
docker run -v C:/temp:/app:ro -v /app/temp <<image_name>>
```

- Now the `/app/temp` folder becomes read & write as it has a much deeper folder specified.

#### Managing docker volumes

- We can create a docker volume by using `docker volume create <<name>>`
- We can inspect a volume by using `docker volume inspect <<volume name>>`
- Individual volume can be removed by using `rm` flag.
- For removing multiple volumes use `prune` flag as `docker volume prune`

example info from a volume inspect:

```json
[
  {
    "CreatedAt": "2022-07-02T13:55:24Z",
    "Driver": "local",
    "Labels": null,
    "Mountpoint": "/var/lib/docker/volumes/feedback/_data",
    "Name": "feedback",
    "Options": null,
    "Scope": "local"
  }
]
```

### COPY vs Bind Mounts

- Bind Mounts are good for development environment as we do not want to changes things in production.
- Avoid using bind mounts in production.
- In a dev environment we don't have to use `COPY . .` in docker file if you are replacing entire code with bind mount.
- It would be a better choice to leave it there in docker file.

### Docker ignore file

- Just like git ignore file docker ignore file is also used to exclude files and folders from docker image.
- `.dockerignore` will be the name of file with no extensions
- Generally we will add unused files like `.git` folder, `Dockerfile` itself if we do not want to include it.

```
# dockerignore file
  Dockerfile
  .git
```

### Environment Variables

- Used to set variables in docker file or through commandline while starting container.
- Accessible in code.
- In a Dockerfile use the following syntax `ENV <<PARAMETER_NAME>> <<VALUE>>`
- example `ENV PORT 80`
- Now this can be assigned to other section in docker file as `$PORT`

Example Docker File:

```dockerfile
FROM node
WORKDIR /app
COPY package.json .
RUN npm install
COPY . .
EXPOSE 80
CMD ["npm", "start"]

# The above we can add env vars as
FROM node
WORKDIR /app
COPY package.json .
RUN npm install
COPY . .
ENV PORT 80
EXPOSE $PORT
CMD ["npm", "start"]
# Port number will be set by default to 80
```

- If we want to change the env value we can do it while creating a container by using `--env` switch.
- example: `docker run -p 8000:3000 --env PORT=3000 --env ENVIRONMENT=dev feedback-app`
- In the above example port and environment are environment variables.
- We can create a `.env` file and set all environment variables in it, then pass it as a parameter.
- example: `docker run -p 8000:3000 --env-file ./.env feedback-app`

```envfile
PORT=8000
ENVIRONMENT=dev
```

### Build Args

- These are se inside a docker file not accessible in application or CMD section.
- set on docker build using `--build-arg` or `ARG` section in dockerfile.

Example docker file with build args:

```dockerfile
FROM node
WORKDIR /app
COPY package.json .
RUN npm install
COPY . .
ARG DEFAULT_PORT=80
ENV PORT $DEFAULT_PORT
EXPOSE $PORT
CMD ["npm", "start"]
```

- During image building we can use: `docker run --build-arg DEFAULT_PORT=8000 feedback-app:buildargs`

## Networking with containers

There are three types of network interactions with containers.

- Container to internet, where a container is trying to connect some external api.
- Container to host machine, Where app in the container is trying to connect to something on the host machine.
- Container to container communication, where one container is trying to connect to another container.

### Container to internet communication

- Docker doesn't need any additional configuration or variable specification for internet communication.
- An app hosted in docker container will be able to connect to internet out of the box.

### Container to host machine

- When we locally use some apps or services we access them by using `http://localhost` as domain.
- To have communications with host machine we need to replace the domain name `localhost` with `host.docker.internal`.
- So if we want to connect to `http://localhost:9090` it should be as `http://host.docker.internal:9090`.

### Container to container communication

- Consider an app `star-wars-node-app` which is a node application, which requires a mongodb installation as database.
- We will create a container for node app and another container for mongodb
- The official mongo docker image is `mongo`
- We can run it in detached mode with name as mongodb using `docker run -d --name mongodb mongo`
- In `app.js` node app is trying to connect to mongodb by using the following line `mongodb://localhost:27017/swfavorites`

As mongodb is running on a different container, we can connect in 2 ways.

### Based on Ip address:

- Inspect the mongodb container with `docker inspect mongodb`
- an ip address can be found under network settings, it will look something like `"IPAddress": "172.17.0.2"`
- Place the ip address value in place of local host and build node app container and start it.
- change `mongodb://localhost:27017/swfavorites` to `mongodb://172.17.0.2:27017/swfavorites`, it will work as expected.
- Every time there is a change in ip address we need to update the image.
- Start mongodb and node (on port 8000) containers then using following curl commands.

**To save a favorite**

```curl
curl --location --request POST 'localhost:8000/favorites' \
--header 'Content-Type: application/json' \
--data-raw '{
    "name":"emperor strikes back",
    "type":"movie",
    "url":"https://swapi.dev/api/films/2/"
}'
```

**To get favorites**

```curl
curl --location --request GET 'localhost:8000/favorites'
```

The above requests should work without any issue.

### Based on Network

- We can place all the containers that need to connect to each other in a docker network.
- We can create a docker network by using `docker network create <<network-name>>`
- We don't have to give any additional info, it will create a network.
- Now we can use the created network to add containers into it.
- To add a container to network while starting a container use `--network` switch and provide the network name.

> Note: We need to create network on our own, docker will not create it if we just give the `--network` switch during starting container.

- Create a network with name `net-01` using `docker network create net-01`
- Now add this to mongodb container by using command `docker run -d --name mongodb --network net-01 mongo`
- Now other containers in same network can connect to a container services by using it's name as domain name.
- So in the node app update `mongodb://172.17.0.2:27017/swfavorites` to `mongodb://mongodb:27017/swfavorites`.
- Build a new image and start a container from it using `--network net-01` to add it to same container
- Build the image with command `docker build -t starwars-nod:for-nt .`
- Start node app with command `docker run -d --name starwars-nt --network net-01 -p 8000:3000 starwars-nod:for-nt`

The curl commands which are mentioned under based on ip should work.

### Docker Network Drivers

- Docker Networks actually support different kinds of "Drivers" which influence the behavior of the Network.
- The default driver is the "bridge" driver - it provides the behavior shown in this module (i.e. Containers can find each other by name if they are in the same Network).
- The driver can be set when a Network is created, simply by adding the --driver option.

```
 docker network create --driver bridge my-net
```

> Of course, if you want to use the "bridge" driver, you can simply omit the entire option since "bridge" is the default anyways.

- Docker also supports these alternative drivers - though you will use the "bridge" driver in most cases:
  - **host:** For standalone containers, isolation between container and host system is removed (i.e. they share localhost as a network)
  - **overlay:** Multiple Docker daemons (i.e. Docker running on different machines) are able to connect with each other. Only works in "Swarm" mode which is a dated / almost deprecated way of connecting multiple containers
  - **macvlan:** You can set a custom MAC address to a container - this address can then be used for communication with that container
  - **none:** All networking is disabled.
  - **Third-party plugins:** You can install third-party plugins which then may add all kinds of behaviors and functionalities

> As mentioned, the "bridge" driver makes most sense in the vast majority of scenarios.

### How do docker parse ip addresses

Docker do not replace the container name in source code, once a call is made from app in a container it will pass through docker environment.
Then docker will replace the container name provided in the domain name with ip address.

## Building Multi Container apps with docker

- Generally apps are build in multi containers.
- We will consider an app with nodejs back-end, React frontend and mongodb as database.
- The source code of the app available at `multi-container-app-05` folder.

**Creating a mongodb container**

- Use the official mongo image and run the following command" `docker run -d --rm --name mongodb -p 27017:27017 mongo`
- Which will expose mongodb on port `27017`

**Creating a backend container**

- Add a Dockerfile to which can be used to build image.

```dockerfile
FROM node
WORKDIR /app
COPY package.json .
RUN npm install
COPY . .
EXPOSE 80
CMD ["node","app.js"]
```

- build the docker image `docker build -t goals-backend . `
- use `docker run -d --rm -p 80:80 --name goals-backend goals-backend` to start the container.
- In instead of `localhost` use `host.docker.internal` to get server address

**Creating front-end container**

- Create a docker file with the following content:

```dockerfile
FROM node
WORKDIR /app
COPY package.json .
RUN npm install
COPY . .
EXPOSE 3000
CMD ["npm","start"]
```

- Build the app using `docker build -t goals-react .` in the front-end folder.
- To start the app use `docker run --name goals-front-end -it -p 3000:3000 goals-react`
- We have to run in terminal interactive mode else react app will stop working after it starts.

### Adding networks to the apps

- Create a new network using `docker network create goals-network`
- To add a container to the network while starting a container use `--network goals-network`
- Use the container name to connect to the container.
- For the React app we still need to expose the node app's ports.
- As the code for react will be built in the browser instead of the server with in the container.

### Persisting data with volumes

- mongodb uses `/data/db` location to store database related information.
- So we can add this as a named volume. `-v mongo-data:/data/db`.
- To add security we can add username, password for authentication.
- These can be added by using environment variables.
- `-e MONGO_INITDB_ROOT_USERNAME` for adding a username.
- `-e MONGO_INITDB_ROOT_PASSWORD` for adding a password.

```
docker run -d -p 27017:27017 --rm --name mongodb -e MONGO_INITDB_ROOT_USERNAME=admin e MONGO_INITDB_ROOT_PASSWORD=admin --network local-network mongo
```

- Now to connect to mongo db http://localhost:27017 will not work
- We should add the username and password in the as shown `http://[username:password@]localhost:27017/database?authSource=admin`
- example: `http://admin:admin@localhost:27017/goalsdb?authSource=admin`
- To get environment variables in nodejs applications use `${process.env.ENVIRONMENT_VARIABLE_NAME}`
- To add environment variables in Dockerfile `ENV MONGO-USERNAME=admin`

> Code live reload can be done using bind-mounts

## Docker-compose

### What is docker compose

Docker Compose is a tool that allows you to replace, Docker build and Docker run commands.
Not just one Docker build and one Docker run command, but potentially multiple Docker build and Docker run commands with just one configuration file.

The configuration file will contain other orchestration commands, to start all the containers at once, also build all necessary images.

We can use a one command to bring all those container up and running. we can also use one command to bring down all containers.

Docker compose can be used for single container apps but it will have more features when we use multi container apps are used.

### What is not docker-compose

- Docker-compose will not replace Dockerfiles for custom images.
- Docker-compose does not replace docker containers or images.
- Docker-compose is not suitable for managing multiple containers in multiple hosts.

> To start with docker compose we use a docker compose file.

In a docker compose file we will add

- Services(Containers)
  - Published ports
  - Environment Variables
  - Volumes
  - Networks

### Creating a compose file
#TODO

### Installing docker-compose on a linux machine

On macOS and Windows, you should already have Docker Compose installed - it's set up together with Docker there. On Linux machines, you need to install it separately.

These steps should get you there:

1. `sudo curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose`
2. `sudo chmod +x /usr/local/bin/docker-compose`
3. `sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose`
4. to verify: `docker-compose --version`

Also see: https://docs.docker.com/compose/install/
