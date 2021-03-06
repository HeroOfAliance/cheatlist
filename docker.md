# Doker Cheatlist
my own docker cheats <br>

*please ping me if you find something really stupid here* [mail](mailto:alan.dayne@gmail.com)

## get docker

* [mac](https://docs.docker.com/docker-for-mac/install/) 
* [win](https://docs.docker.com/docker-for-windows/install/) 
* [ununtu](https://docs.docker.com/install/linux/docker-ce/ubuntu/)

## iTerm2

good replacement for the standard mac terminal [link](https://www.iterm2.com/) <br>

## install command line completion

absolutely necessary thing [official docker's doc](https://docs.docker.com/compose/completion/) <br>

step by step if standart doc doesn't work for you: <br>
* you should install brew if you have not already done so [link](https://brew.sh)
* install bash completion `brew install bash-completion`
* open bash profile `nano ~/.bash_profile`
* copy this text there:
 ```
 if [ -f $(brew --prefix)/etc/bash_completion ]; then
 . $(brew --prefix)/etc/bash_completion
 fi
 ```
* press `control + O` and `enter`
* restart bash session `source ~/.bash_profile`
* install docker completion `brew install docker-completion`
* Hurray! You did it. Just restart your terminal.

## install some tools on ubuntu image

update apt-get index <br>
`apt-get update` <br>
install __curl__ <br>
`apt-get install curl` <br>
install __nslookup__ <br>
`apt-get install dnsutils` <br>
install __ping__ <br>
`apt-get install iputils-ping` <br>

## basic 

print versions of installed docker client and server <br>
`docker version` <br>
print docker configuration <br>
`docker info` <br>

## container

run detached container with name __webhost__ from the __nginx__ image and forward __8080__ port of the host to the __80__ port of the container <br>
`docker container run --publish 8080:80 --detach --name webhost nginx` <br>
`docker container run -p 8080:80 -d --name webhost nginx` <br>
print published ports for the __webhost__ container <br>
`docker container port webhost` <br>
view only running containers <br>
`docker container ls` <br>
view all containers <br>
`docker container ls -a` <br>
realtime resources monitor for all running containers <br>
`docker container stats` <br>
print configuration for the __webhost__ container <br>
`docker container inspect webhost` <br>
print only IPAddress for the __webhost__ container (or any another field by specified pattern) <br>
`docker container inspect --format '{{.NetworkSettings.IPAddress}}' webhost` <br>
stop container webhost <br>
`docker container stop webhost` <br>
print logs of the detached and running container __webhost__ <br>
`docker container logs webhost` <br>
print process list of the running container __webhost__ <br>
`docker container top webhost` <br>
remove stopped containers __webhost1__ and  __webhost2__<br>
`docker container rm webhost1 webhost2` <br>
stop and remove running containers __webhost1__ and  __webhost2__ (will work on stopped containers as well ) <br>
`docker container rm -f webhost1 webhost2` <br>
run container with name __my_ubuntu__ from the __ubuntu__ image, and open its __bash__ <br>
`docker container run -it --name my_ubuntu ubuntu bash` <br>
run container from the __centos__ image, exec __curl__ command on the __google__, than exit and remove container <br>
`docker container run --rm centos curl https://www.google.com` <br>

## image

get a list of pulled images <br>
`docker image ls` <br>
remove cached images __centos__, __ubuntu__ and __nginx__ <br>
`docker image rm centos ubuntu nginx` <br>
print a list of layers of the __nginx:latest__ image <br>
`docker image history nginx:latest` <br>
print a metadata of the __nginx:latest__ image <br>
`docker image inspect nginx:latest` <br>
tag official image of the __nginx:alpine__ as my own image __avalonxt/nginx:latest__ <br>
`docker image tag nginx:alpine avalonxt/nginx:latest` <br>
tag my image __avalonxt/nginx:latest__ as  __avalonxt/nginx:develop__ <br>
`docker image tag avalonxt/nginx:latest avalonxt/nginx:develop` <br>
login to the docker to be able to push images to the docker hub <br>
`docker login` <br>
push tag __avalonxt/nginx:latest__ to the docker hub (upload the image to my account on docker hub) <br>
`docker image push avalonxt/nginx:latest` <br>
logout from the docker to avoid compromising security <br>
`docker logout` <br>
pull image __avalonxt/nginx:latest__ from the docker hub <br>
`docker pull avalonxt/nginx:latest` <br>

## network

get a list of all networks <br>
`docker network ls` <br>
get configuraion of the network with name __bridge__ (in addition contains a list of connected containers) <br>
`docker network inspect bridge` <br>
create new network with name __my_network__ and default driver (bridge) <br>
`docker network create my_network` <br>
connect container __webhost__ to the __my_network__ <br>
`docker network connect my_network webhost` <br>
connect container __webhost_1__ to the __my_network__ and assign alias __webhost__ to the container <br>
`docker network connect --alias webhost my_network webhost_1` <br>
disconnect container __webhost_1__ from the __my_network__ <br>
`docker network disconnect my_network webhost_1` <br>
run container __webhost_1__ and connect it to the __my_network__ (will be connected to the __my_network__ only) <br>
`docker container run -d --name webhost_1 --network my_network nginx:alpine` <br>
`docker container run -d --name webhost_1 --net my_network nginx:alpine` <br>
run container __webhost_1__ and connect it to the __my_network__ and assign alias __webhost__ to the container <br>
`docker container run -d --name webhost_1 --net my_network --net--alias webhost nginx:alpine` <br>
ping container __webhost_2__ from the __webhost_1__ <br>
`docker container exec webhost_1 ping webhost_2` <br>

## volumes 

clean all detached volumes
`docker volume prune` <br>

## keep everything clean

get docker ussage info <br>
`docker system df` <br>
remove all __\<none\> (dangling)__ images <br>
`docker image prune` <br>
remove all images that are nout in use right now<br>
`docker image prune -a` <br>
remove all __\<none\> (dangling)__ images, unused networks, detached volumes and stopped containers <br>
`docker system prune` <br>
remove all unused images, unused networks, detached volumes, stopped containers and build cache<br>
`docker system prune -a` <br>
remove all stopped containers <br>
`docker container prune` <br>

## docker file 

``` dockerfile
# define base image
FROM debian:jessie 

# setup environment
# this statement is optional
ENV SOME_VAR some_value

# run bash or sh commands 
# each RUN command is a layer, thats wry calls are joined with &&
# this statement is optional
RUN apt-get update \
    && apt-get install curl \
    && echo "curl installed"

# expose port to the local docker network
# you should -p to forward this port on host
# this statement is optional
EXPOSE 80 443

# set current work dir to avoid cd .. stuff 
WORKDIR /usr/src/app

# copy source file to the destination
COPY source.file destination.file

# copy everything from current client folder to the current WORKDIR
COPY . .

# add dirrectory ./public from host to the container
ADD ./public ./public

# this command will be running on container's start
# keep in mind, that only one command will be running
# so docker will exec only the latest one
CMD ["nginx", "-g", "daemon off;"]

```
<br>

## build an image

build custom image from Dockerfile <br>
-f to specify file to build <br>
__dont forget to put dot in the end!__ <br>
`docker build -t path/to/dockerfile .` <br>
