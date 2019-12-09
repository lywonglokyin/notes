# Docker compose

Docker compose is basically a tool to bind all running configuration, and allow user to easily run built docker images.

## docker-compose.yml File

See an example first:

```yml
version: '3'
services:
  web:
    build: .
    ports:
      - "5000:5000"
  redis:
    image: "redis:alpine"
```
The above compose file defines two services: `web` and `redis`.

The `web` service uses an image that is built from `Dockerfile` in the current directory. It then binds host machine to the exposed port `5000`.

## A more detailed look to compose-file


1. `version:` : Specify the version of docker-compose. See https://docs.docker.com/compose/compose-file/ for compatible versions.

2. `services:` : Specifies the list of containers you wish to compose.

    1. `<name>:` : Name of the service wished to invoke
        1. `build:` 