# Installing additional apps in containers

- Login to container as root user using the following:
  
```shell
docker exec -it -u root <<container_name or container_id>> /bin/bash
```
- It will provide access to bin folder.
- We can run terminal commands.
- update apt by using `apt-get update`
- Then install other apps by using install.
- For python `apt-get install python3`
- For pip install use `apt-get install pip`
- Similar for other commands.