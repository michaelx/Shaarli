# Docker

## Install Docker

Install [Docker](https://www.docker.com/), by following the instructions relevant to your OS / distribution, and start the service.


## Get and run a Shaarli image

Shaarli images are available on [DockerHub](https://hub.docker.com/r/shaarli/shaarli/):

- `latest`: latest branch
- `master`: master branch

These images are built automatically on DockerHub and rely on:

- [Alpine Linux](https://www.alpinelinux.org/)
- [PHP7-FPM](http://php-fpm.org/)
- [Nginx](http://nginx.org/)

Additional Dockerfiles are provided for the `arm32v7` platform, relying on [Linuxserver.io Alpine armhf images](https://hub.docker.com/r/lsiobase/alpine.armhf/). These images must be built using [`docker build`](https://docs.docker.com/engine/reference/commandline/build/) on an `arm32v7` machine or using an emulator such as [qemu](https://resin.io/blog/building-arm-containers-on-any-x86-machine-even-dockerhub/).

```bash
# download the 'latest' image from dockerhub
docker pull shaarli/shaarli

# create persistent data volumes/directories on the host
docker volume create shaarli-data
docker volume create shaarli-cache

# create a new container from the image
# --detach: run the container in background
# --name: name of the created container/instance
# --publish: map the host's :8000 port to the container's :80 port
# --rm: automatically remove the container when it exits
# --volume: mount persistent volumes in the container ($volume_name:$volume_mountpoint)
docker run --detach \
           --name myshaarli \
           --publish 8000:80 \
           --rm \
           --volume shaarli-data:/var/www/shaarli/data \
           --volume shaarli-cache:/var/www/shaarli/cache \
           shaarli/shaarli

# verify that the container is running
docker ps | grep myshaarli

# to stop the container
docker stop myshaarli

# check the container is stopped
docker ps | grep myshaarli
docker ps -a | grep myshaarli

# destroy the container
docker rm myshaarli
docker ps -a | grep myshaarli

```

## Compose file

A `docker-compose.yml` file can be used to run a persistent/autostarted shaarli service using [docker-compose](https://docs.docker.com/compose/) or in a [Docker stack](https://docs.docker.com/engine/reference/commandline/stack_deploy/)/[Swarm](https://docs.docker.com/engine/reference/commandline/swarm_init/).

```yaml

services:
  shaarli:

  TODO

```



### Running dockerized Shaarli as a systemd service

It is possible to start a dockerized Shaarli instance as a systemd service (systemd is the service management tool on several distributions). After installing Docker, use the following steps to run your shaarli container Shaarli to run on system start.

As root, create `/etc/systemd/system/docker.shaarli.service`:

```ini
[Unit]
Description=Shaarli Bookmark Manager Container
After=docker.service
Requires=docker.service


[Service]
Restart=always

# Put any environment you want in an included file, like $host- or $domainname in this example
EnvironmentFile=/etc/sysconfig/box-environment

# It's just an example..
ExecStart=/usr/bin/docker run \
  -p 28010:80 \
  --name ${hostname}-shaarli \
  --hostname shaarli.${domainname} \
  -v /srv/docker-volumes-local/shaarli-data:/var/www/shaarli/data:rw \
  -v /etc/localtime:/etc/localtime:ro \
  shaarli/shaarli:latest

ExecStop=/usr/bin/docker rm -f ${hostname}-shaarli

[Install]
WantedBy=multi-user.target
```

```bash
# reload systemd services definitions
systemctl daemon-reload
# start the servie and enable it a boot time
systemctl enable docker.shaarli.service --now
# verify that the service is running
systemctl status docker.*
# inspect system log if needed
journalctl -f
```



## Docker cheatsheet

```bash
# pull/update an image
$ docker pull shaarli:release
# run a container from an image
$ docker run shaarli:latest
# list available images
$ docker images ls
# list running containers
$ docker ps
# list running AND stopped containers
$ docker ps -a
# run a command in a running container
$ docker exec -ti <container-name-or-first-letters-of-id> bash
# follow logs of a running container
$ docker logs -f <container-name-or-first-letters-of-id>
# delete unused images to free up disk space
$ docker system prune --images
# delete unused volumes to free up disk space (CAUTION all data in unused volumes will be lost)
$ docker system prunt --volumes
# delete unused containers
$ docker system prune
```


## References

- [docker pull](https://docs.docker.com/engine/reference/commandline/pull/)
- [docker images](https://docs.docker.com/engine/reference/commandline/images/)
- [docker run](https://docs.docker.com/engine/reference/commandline/run/)
- [docker ps](https://docs.docker.com/engine/reference/commandline/ps/)
- [docker create](https://docs.docker.com/engine/reference/commandline/create/)
- [docker exec](https://docs.docker.com/engine/reference/commandline/exec/)
- [docker logs](https://docs.docker.com/engine/reference/commandline/logs/)
- [Interactive Docker training portal](https://www.katacoda.com/courses/docker/) on [Katakoda](https://www.katacoda.com/)
- [Where are Docker images stored?](http://blog.thoward37.me/articles/where-are-docker-images-stored/)
- [Dockerfile reference](https://docs.docker.com/reference/builder/)
- [Dockerfile best practices](https://docs.docker.com/articles/dockerfile_best-practices/)
- [Volumes](https://docs.docker.com/userguide/dockervolumes/)
- [DockerHub: Repositories](https://docs.docker.com/userguide/dockerrepos/)
- [DockerHub: Teams and organizations](https://docs.docker.com/docker-hub/orgs/)
- [DockerHub: GitHub automated build](https://docs.docker.com/docker-hub/github/)
- [Service management: Using supervisord](https://docs.docker.com/articles/using_supervisord/)
- [Service management: Nginx in the foreground](http://nginx.org/en/docs/ngx_core_module.html#daemon)
- [Docker: using volumes](https://docs.docker.com/storage/volumes/)

