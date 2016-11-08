# Docker

## Commands

### Basic

```sh
docker info
       ps
       images
       logs
       top
       start [CONTAINER NAME]
```

### getting a new image

```sh
docker pull [IMAGE]
```

### run program in container

#### daemon

* `-d` : runs container as daemon
* `-P` : maps any required network ports inside the container to our host
* `-p` : bind container to specific port

```sh
docker run -d -P [IMAGE] [PROGRAM]

docker run -d -p [LOCAL HOST PORT]:[CONTAINER PORT] [IMAGE] [PROGRAM]
```

#### interactively

* `-t` : assigns a pseudo-tty or terminal inside the new container
* `-i` : interactive connection by grabbing stdin of the container

E.g. to run commands inside bash shell, replace `[PROGRAM]` with `/bin/bash`
```sh
docker run -t -i [IMAGE] [PROGRAM]
```

run commands against already running container
```sh
docker exec -t -i [CONTAINER NAME] [PROGRAM]
```

### view application container

view details of the last container started
```sh
docker ps -l
```

### network port shortcut

```sh
docker port [CONTAINER NAME] [PORT]
```

### view app's logs

```sh
docker logs -f [CONTAINER NAME]
```

### look at container's processes

```sh
docker top [CONTAINER NAME]
```

### inspect container

```sh
docker inspect [CONTAINER NAME]
```

### stop container

```sh
docker stop [CONTAINER NAME]
```

### remove container

```sh
docker rm [CONTAINER NAME]
```

### remove image

```sh
docker rmi [CONTAINER NAME]
```

# Docker Compose

* tool for defining and running multi-container apps
* compose a file to configure your app's services
* using a single command, create and start all the services from your configuration

```sh
docker-compose --version
```

# Docker Machine

```sh
docker-machine --version
```
