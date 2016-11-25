# Docker

## `docker` commands

```sh
# basics
info                              # display system-wide info
images                            # list images
ps                                # list containers

# Dockerfile
build   PATH                      # build an image from a Dockerfile

# container
top     CONTAINER                 # display runing processes of a container
logs    CONTAINER                 # fetch logs of a container
start   CONTAINER                 # start one or more stopped containers
restart CONTAINER                 # start one or more stopped containers
rm      CONTAINER                 # remove one or more containers
exec    CONTAINER CMD             # run command in a running container
inspect CONTAINER|IMAGE|TASK      # return low-level info on container, image or task
port    CONTAINER [PRIVATE_PORT]  # list port mappings for container

# image
run     IMAGE [CMD]               # run a command in a new container
rmi     IMAGE                     # remove one or more images

# registry
pull    NAME[TAG|@DIGET]          # pull an image/repository from a registry
```

### Example: `run`

Daemon:

* `-d` : runs container as daemon
* `-P` : maps any required network ports inside the container to our host
* `-p` : bind container to specific port

```sh
docker run -d -P IMAGE [CMD]

docker run -d -p LOCALHOST-PORT:CONTAINER-PORT IMAGE [CMD]
```

### Example: `exec`

* `-t` : assigns a pseudo-tty or terminal inside the new container
* `-i` : interactive connection by grabbing stdin of the container

Access terminal on running container as root
```sh
$ docker exec -t -i --user root CONTAINER /bin/bash
```

### Example: `rm` and `rmi`

Remove all (`-a`) containers and images based on numeric IDs (`-q`)
```sh
docker rm $(docker ps -a -q)

docker rmi $(docker images -a -q)
```

# Docker Compose

Tool for defining and running multi-container apps by composing a file to configure your app's services. Using a single command, create and start all the services from your configuration.

## `docker-compose` commands

* `-f` : specify an alternateive compose file (default: `docker-compose.yml`)

```sh
logs [SERVICE...]    # view output from containers
ps   [SERVICE...]    # list containers (-q displays on IDs)
rm   [SERVICE...]    # remove stopped containers
stop [SERVICE...]    # stop services
up   [SERVICE...]    # create and start containers
```

### Example: `up`

Run in detached-mode: run containers in the background
```sh
docker-compose up -d
```

# `bash` completion

to activate bash completion (if using `brew`):
```sh
cd /usr/local/etc/bash_completion.d
ln -s /Applications/Docker.app/Contents/Resources/etc/docker.bash-completion
ln -s /Applications/Docker.app/Contents/Resources/etc/docker-machine.bash-completion
ln -s /Applications/Docker.app/Contents/Resources/etc/docker-compose.bash-completion
```
