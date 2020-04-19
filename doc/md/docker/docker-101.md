## Basics
Install [Docker](https://www.docker.com/), by following the instructions relevant to your OS / distribution, and start the service.


### Docker cheatsheet

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


### References

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


--------------------------------------

<!-- TODO REAFCTOR EVERYTHING BELOW/MERGE WITH OTHER DOCKER DOCS -->


### Running dockerized Shaarli as a systemd service

It is possible to start a dockerized Shaarli instance as a systemd service (systemd is the service management tool of choice on several distributions). After installing Docker, use the following steps to run your shaarli container Shaarli to run on system start.

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

Another solution for automatic restarts is to deploy Shaarli from a docker-compose file as a service in a [Docker stack](https://docs.docker.com/engine/reference/commandline/stack_deploy/). This requires setting up [Swarm](https://docs.docker.com/engine/reference/commandline/swarm_init/) mode.
