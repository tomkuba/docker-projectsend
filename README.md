[![linuxserver.io](https://raw.githubusercontent.com/linuxserver/docker-templates/master/linuxserver.io/img/linuxserver_medium.png)](https://linuxserver.io)

The [LinuxServer.io](https://linuxserver.io) team brings you another container release featuring :-

 * regular and timely application updates
 * easy user mappings (PGID, PUID)
 * custom base image with s6 overlay
 * weekly base OS updates with common layers across the entire LinuxServer.io ecosystem to minimise space usage, down time and bandwidth
 * regular security updates

Find us at:
* [Discord](https://discord.gg/YWrKVTn) - realtime support / chat with the community and the team.
* [IRC](https://irc.linuxserver.io) - on freenode at `#linuxserver.io`. Our primary support channel is Discord.
* [Blog](https://blog.linuxserver.io) - all the things you can do with our containers including How-To guides, opinions and much more!

# [linuxserver/projectsend](https://github.com/linuxserver/docker-projectsend)
[![](https://img.shields.io/discord/354974912613449730.svg?logo=discord&label=LSIO%20Discord&style=flat-square)](https://discord.gg/YWrKVTn)
[![](https://images.microbadger.com/badges/version/linuxserver/projectsend.svg)](https://microbadger.com/images/linuxserver/projectsend "Get your own version badge on microbadger.com")
[![](https://images.microbadger.com/badges/image/linuxserver/projectsend.svg)](https://microbadger.com/images/linuxserver/projectsend "Get your own version badge on microbadger.com")
![Docker Pulls](https://img.shields.io/docker/pulls/linuxserver/projectsend.svg)
![Docker Stars](https://img.shields.io/docker/stars/linuxserver/projectsend.svg)
[![Build Status](https://ci.linuxserver.io/buildStatus/icon?job=Docker-Pipeline-Builders/docker-projectsend/master)](https://ci.linuxserver.io/job/Docker-Pipeline-Builders/job/docker-projectsend/job/master/)
[![](https://lsio-ci.ams3.digitaloceanspaces.com/linuxserver/projectsend/latest/badge.svg)](https://lsio-ci.ams3.digitaloceanspaces.com/linuxserver/projectsend/latest/index.html)

[Projectsend](http://www.projectsend.org) is a self-hosted application that lets you upload files and assign them to specific clients that you create yourself. Secure, private and easy. No more depending on external services or e-mail to send those files.

[![projectsend](http://www.projectsend.org/wp-content/themes/projectsend/img/screenshots.png)](http://www.projectsend.org)

## Supported Architectures

Our images support multiple architectures such as `x86-64`, `arm64` and `armhf`. We utilise the docker manifest for multi-platform awareness. More information is available from docker [here](https://github.com/docker/distribution/blob/master/docs/spec/manifest-v2-2.md#manifest-list) and our announcement [here](https://blog.linuxserver.io/2019/02/21/the-lsio-pipeline-project/). 

Simply pulling `linuxserver/projectsend` should retrieve the correct image for your arch, but you can also pull specific arch images via tags.

The architectures supported by this image are:

| Architecture | Tag |
| :----: | --- |
| x86-64 | amd64-latest |
| arm64 | arm64v8-latest |
| armhf | arm32v7-latest |


## Usage

Here are some example snippets to help you get started creating a container.

### docker

```
docker create \
  --name=projectsend \
  -e PUID=1000 \
  -e PGID=1000 \
  -e TZ=Europe/London \
  -e MAX_UPLOAD=<5000> \
  -e DB_HOST=mariadb \
  -e DB_NAME=projectsend \
  -e DB_USER=dbuser \
  -e DB_PASSWORD=dbuser-password \
  -p 80:80 \
  -v <path to data>:/config \
  -v <path to data>:/data \
  --restart unless-stopped \
  linuxserver/projectsend
```


### docker-compose

Compatible with docker-compose v2 schemas.

```
---
version: "2"
services:
  projectsend:
    image: linuxserver/projectsend
    container_name: projectsend
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/London
      - MAX_UPLOAD=<5000>
      - DB_HOST=mariadb
      - DB_NAME=projectsend
      - DB_USER=dbuser
      - DB_PASSWORD=dbuser-password
    volumes:
      - <path to data>:/config
      - <path to data>:/data
    ports:
      - 80:80
    restart: unless-stopped
```

## Parameters

Container images are configured using parameters passed at runtime (such as those above). These parameters are separated by a colon and indicate `<external>:<internal>` respectively. For example, `-p 8080:80` would expose port `80` from inside the container to be accessible from the host's IP on port `8080` outside the container.

| Parameter | Function |
| :----: | --- |
| `-p 80` | WebUI |
| `-e PUID=1000` | for UserID - see below for explanation |
| `-e PGID=1000` | for GroupID - see below for explanation |
| `-e TZ=Europe/London` | Specify a timezone to use EG Europe/London. |
| `-e MAX_UPLOAD=<5000>` | To set maximum upload size (in MB), default if unset is 5000. |
| `-e DB_HOST=mariadb` | Set your database location here using IP,hostname or docker container name |
| `-e DB_NAME=projectsend` | Pre-existing database name, default if unset is projectsend. |
| `-e DB_USER=dbuser` | Database user with permissions on above database, default if unset is root. |
| `-e DB_PASSWORD=dbuser-password` | Password for above user. |
| `-v /config` | Where to store projectsend config files. |
| `-v /data` | Where to store files to share. |

## User / Group Identifiers

When using volumes (`-v` flags) permissions issues can arise between the host OS and the container, we avoid this issue by allowing you to specify the user `PUID` and group `PGID`.

Ensure any volume directories on the host are owned by the same user you specify and any permissions issues will vanish like magic.

In this instance `PUID=1000` and `PGID=1000`, to find yours use `id user` as below:

```
  $ id username
    uid=1000(dockeruser) gid=1000(dockergroup) groups=1000(dockergroup)
```


&nbsp;
## Application Setup

You are required to setup a database in either mysql or mariadb with appropriate permissions before install.
The webui will be available at http://<IP>:<HOST_PORT>/ to complete setup. 
More info at [ProjectSend](http://www.projectsend.org).



## Support Info

* Shell access whilst the container is running: `docker exec -it projectsend /bin/bash`
* To monitor the logs of the container in realtime: `docker logs -f projectsend`
* container version number 
  * `docker inspect -f '{{ index .Config.Labels "build_version" }}' projectsend`
* image version number
  * `docker inspect -f '{{ index .Config.Labels "build_version" }}' linuxserver/projectsend`

## Updating Info

Most of our images are static, versioned, and require an image update and container recreation to update the app inside. With some exceptions (ie. nextcloud, plex), we do not recommend or support updating apps inside the container. Please consult the [Application Setup](#application-setup) section above to see if it is recommended for the image.  
  
Below are the instructions for updating containers:  
  
### Via Docker Run/Create
* Update the image: `docker pull linuxserver/projectsend`
* Stop the running container: `docker stop projectsend`
* Delete the container: `docker rm projectsend`
* Recreate a new container with the same docker create parameters as instructed above (if mapped correctly to a host folder, your `/config` folder and settings will be preserved)
* Start the new container: `docker start projectsend`
* You can also remove the old dangling images: `docker image prune`

### Via Docker Compose
* Update all images: `docker-compose pull`
  * or update a single image: `docker-compose pull projectsend`
* Let compose update all containers as necessary: `docker-compose up -d`
  * or update a single container: `docker-compose up -d projectsend`
* You can also remove the old dangling images: `docker image prune`

### Via Watchtower auto-updater (especially useful if you don't remember the original parameters)
* Pull the latest image at its tag and replace it with the same env variables in one run:
  ```
  docker run --rm \
  -v /var/run/docker.sock:/var/run/docker.sock \
  containrrr/watchtower \
  --run-once projectsend
  ```
* You can also remove the old dangling images: `docker image prune`

## Building locally

If you want to make local modifications to these images for development purposes or just to customize the logic: 
```
git clone https://github.com/linuxserver/docker-projectsend.git
cd docker-projectsend
docker build \
  --no-cache \
  --pull \
  -t linuxserver/projectsend:latest .
```

The ARM variants can be built on x86_64 hardware using `multiarch/qemu-user-static`
```
docker run --rm --privileged multiarch/qemu-user-static:register --reset
```

Once registered you can define the dockerfile to use with `-f Dockerfile.aarch64`.

## Versions

* **12.06.19:** - Rebase to Alpine 3.9 & adapt startup script to changes in ProjectSend r1053 - Thanks to tomkuba
* **23.03.19:** - Switching to new Base images, shift to arm32v7 tag.
* **11.02.19:** - Add pipeline logic and multi arch.
* **11.06.17:** - Fetch version from github.
* **09.12.17:** - Rebase to alpine 3.7.
* **13.06.17:** - Initial Release.
