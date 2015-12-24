# Docker

### Boot2Docker (OS X)
create a new VM. Only need to run this command once.
```sh
$ boot2docker init
```

start VM
```sh
$ boot2docker start
```

set the environment variables in your shell
```sh
$ eval "$(boot2docker shellinit)"
```

run to verify setup
```sh
$ docker run hello-world
$ boot2docker status
$ docker version
```

upgrade
```sh
$ boot2docker upgrade
```

### Containers
running an instance of an image
```sh
$ docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
```

example:
```sh
$ docker run -it --name="test_container" ubuntu:latest /bin/bash
```
  * OPTIONS: `-it` tells docker to make the container interactive and attach a terminal
  * IMAGE: `ubuntu:latest` is the image pulled down
  * COMMAND: `/bin/bash` starts a shell where you can log in

let the docker command know where to find the boot2docker VM
```sh
# Get http:///var/run/docker.sock/v1.19/containers/<username>.local/changes:
# dial unix /var/run/docker.sock: no such file or directory.
# Are you trying to connect to a TLS-enabled daemon without TLS?

$ $(boot2docker shellinit 2> /dev/null)
```

make changes to/inside container
```sh
root@<hash>:/# echo "hello world" > /tmp/hello.txt
```

view info about container
```sh
$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
4afa46473802        ubuntu:14.04        /bin/bash           17 hours ago        Up 39 seconds                           test_container

# show all containers
$ docker ps -a
```

view changes to container
```sh
$ docker diff test_container
C /tmp
A /tmp/hello.txt
```

commit changes to container a new container: `hello_docker`
```sh
$ docker commit -m "Adding hello world" test\_container hello\_docker
```

view history
```sh
$ docker history hello_docker
```

view images
```sh
$ docker images
```

stop and remove running container
```sh
$ docker stop <container>
$ docker rm <container>
```

remove all containers/images
```sh
# containers
$ docker rm $(docker ps -aq)
# images
$ docker rmi $(docker images -q)
```

### Example: Flask app
```sh
# host
$ docker run -it --name="simple_flask" ubuntu:latest /bin/bash

# container
root@<hash>:/# apt-get update
root@<hash>:/# apt-get install -y python python-pip wget
root@<hash>:/# pip install Flask

# host
$ docker commit -m "installed python and flask" simple_flask
$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
<none>              <none>              204c4673b95e        5 seconds ago       355.2 MB
ubuntu              latest              d2a0ecffe6fa        3 days ago          188.4 MB
hello-world         latest              91c95931e552        12 weeks ago        910 B

$ docker tag 204c4673b95e simple_flask
$ docker history simple_flask
IMAGE               CREATED              CREATED BY                                      SIZE                COMMENT
204c4673b95e        About a minute ago   /bin/bash                                       166.8 MB            installed python and flask
d2a0ecffe6fa        3 days ago           /bin/sh -c #(nop) CMD ["/bin/bash"]             0 B
29460ac93442        3 days ago           /bin/sh -c sed -i 's/^#\s*\(deb.*universe\)$/   1.895 kB
b670fb0c7ecd        3 days ago           /bin/sh -c echo '#!/bin/sh' > /usr/sbin/polic   194.5 kB
83e4dde6b9cf        3 days ago           /bin/sh -c #(nop) ADD file:c8f078961a543cdefa   188.2 MB

$ docker run -it -P -p 5000:5000 -w /home simple_flask /bin/bash
# flags:
# -P publishes exposed ports from container to local host
# -p exposes port 5000 on container and routes it to port 5000 on host
# -w /home sets working directory used when container starts

# container
root@<hash>:/home# wget https://raw.githubusercontent.com/odewahn/docker-jumpstart/master/examples/hello.py
root@<hash>:/home# python hello.py
 * Running on http://127.0.0.1:5000/

# host
$ boot2docker ip
192.168.59.103

$ curl 192.168.59.103:5000
Hello World!
```

### Dockerfiles
simple example
```
### "from"
FROM ubuntu:latest
MAINTAINER name <email>

### "localedef"
RUN locale-gen en_US.UTF-8

### "apt-defaults"
RUN apt-get update

### "utils"
RUN apt-get install \
      ack-grep \
      adduser \
      build-essential \
      curl \
      git \
      man-db \
      pkg-config \
      rsync \
      strace \
      sudo \
      tree \
      unzip \
      vim \
      wget \
    ;

### "python"
RUN apt-get install \
      python-dev \
      python-pip \
    ;

WORKDIR /home

### "run"
ADD run.sh /home/run.sh
CMD ["/bin/bash", "/home/run.sh"]
```
