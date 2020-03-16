# Docker

Docker is a tool for containerizing applications, such that they are flexible, lightweight, and portable.

## Key feature of Docker

- Each container interact with its own filesystem, provided by a Docker image.

## Common workflow of Docker

1. Create and test individual containers for each component of the application by first creating Docker images.
2. Assemble the containers and supporting infrastructure into a complete application, expressed either as *Docker stack file* or in *Kubernetes YAML*.
3. Test, share and deploy the containerized application.


# Docker get-started example

https://docs.docker.com/get-started/part2/

## Step 1: Docker image creation

Recall that docker image captures a private filesystem. When we first start our project, we seeks to build a docker image with just what we need.

### `Dockerfile` file

Dockfile is a configuration file describing how to assemble the private filesystem. Basic structure:

```bash
FROM node:6.11.5
WORKDIR /usr/src/app
COPY package.json
RUN npm install
COPY . .

CMD [ "npm", "start" ]
```

Explanation:

- `FROM node:6.11.5`: Start the pre-existing official image by `node.js` vendors, containing the node 6.11.5 interpreter and dependencies.
- `WORKDIR /usr/src/app`: Specify the working directory in the **image** system
- `COPY package.json`: Copy the file `package.json` from host to image location (`/usr/src/app`)
- `RUN npm install`: Run the command `npm install` inside the image filesystem, which will read from the `package.json` to determine dependencies.
- `COPY . .`: Copy the rest of the source code to the image filesystem
- `CMD ... `: A metadata specifying how to run a container based on this image. In this example it shows that the container aims to support `npm start`.

## Step 2: Assemble the container

In the directorry of the project, run the following command to build the image:

```cmd
docker image build -t bulletinboard:1.0 .
```

## Step 3: Start the container

Run the following command to start the container based on the image:

```cmd
docker container run --publish 8000:8080 --detach --name bb bulletinboard:1.0
```

Explanation:

- `--publish`: Ask docker to foward traffic incoming on host posrt `8000` to container's port `8080`. (Otherwise, firewall rules will block all incoming traffic)
- `--detach`: Run this container in the background
- `--name` : Specify a name such that it can be referenced later, in this case, `bb`.

With that, we can access the container with browser in `localhost:8000`!

After that, we can delete the container with:

```cmd
docker container rm --force bb
```

# Docker network

## Step 1: Create a new network bridge

Creating a new network bridge has two main advanrage: 1) More secure as only specified container are in this network 2) Allow resolution of container name to IP address.

```cmd
docker network create <net_name>
```

## Step 2: Link that network to a container during its start

```cmd
docker run --net <net_name> ...
```