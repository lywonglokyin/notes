# Useful commands for Docker

`docker pull <image>` : Fetch an image from docker's repository.

---

`docker push <username>/<project_name>`: Push to docker hub.

---

`docker images` : List all images in your system.

---

`docker run [OPTIONS] <image> [cmd]` : Launch the specified image, then run the command.

Useful OPTIONS:

- `-d` : Detached mode
- `-it` : Interactive mode (for individual usage of `-i` and `-t` see https://docs.docker.com/engine/reference/run/)
- `-rm` : removes the container when it exits

Networking options:
- `-p <port>` : Expose port to client. If unspecified then assign random port.

Image functionality:
- `e <option>`: Set environment variable

  e.g. `docker run ... -e "discovery.type=single-node" elasticsearch`



---

`docker ps [OPTIONS]` : Show the containers (currently) running

Useful OPTIONS:

- `-a`: include running history

Return formmating options:
- `-q`: return only the IDs
- `-f <condition>` : filter the output

Alternatives:
- `docker container ls`

---

`docker stop <container>`: Stop the specified container

---

`docker rm <image>` : Remove the selected image

Useful example:

`docker rm $(docker ps -a -q -f status=exited)` : Remove all exited images (alternatively, you can use `docker container prune`.)

---

`docker rmi <image>` : Remove image that are no longer needed

---

`docker build [OPTIONS] <file_location>` : Build the docker image.

Useful OPTIONS:

- `-t <tag>`: Link the image with the tag, e.g. `-tag kevinwong/test`

---

`docker network <CMDS>` : Network commands.

Useful CMDS:

- `-ls` : List networks
- `inspect <network_name>`: Inspect the specified network
- `create <name>`: Create a new network

---

# Docker-compose Useful commands

`docker-compose [PRE-OPTIONS] up [OPTIONS]`: Boot up a docker-compose project

Useful PRE-OPTIONS:
- `-f` : specify the docker-compose file to be used.

Useful OPTIONS:
- `-d` : detached mode

---

`docker-compose down [OPTIONS]`: Shutdown docker-compose project

Useful OPTIONS:
- `-v`: Also destroy clusters and data volume

---

`docker-compose ps` : List running containers
