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
- `docker run -p 3000:80 abcdefgh` to run docker image `abcdefgh` on local port 3000 where container port 80 is exposed.
- `docker stop <<image-name>>` to stop a docker container.
- `docker run -d <<Image hash>>` to start a container in detached mode.
- `docker start -a <<container name>>` will start a container in attached mode.
- `docker attach <<name_of_running_container>>` will bring up interactive terminal for the container.
- `docker logs <<name_of_running_container>>` will bring up the logs of a container.
- To follow logs use the -f switch `docker logs -f <<name_of_running_container>>`
