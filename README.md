# Doker Cheatlist
my own docker cheats <br>
please ping me if you find something really stupid here [mail](mailto:alan.dayne@gmail.com)

## get docker
* [mac](https://docs.docker.com/docker-for-mac/install/) 
* [win](https://docs.docker.com/docker-for-windows/install/) 
* [ununtu](https://docs.docker.com/install/linux/docker-ce/ubuntu/)

## iTerm2
*good replacement for the standard mac terminal [link](https://www.iterm2.com/)* <br>

## install command line completion
[link](https://docs.docker.com/compose/completion/) <br>

## install some tools on ubuntu image
*update apt-get index* <br>
`apt-get update` <br>
*install __curl__* <br>
`apt-get install curl` <br>
*install __nslookup__* <br>
`apt-get install dnsutils` <br>
*install __ping__* <br>
`apt-get install iputils-ping` <br>

## basic 
*print version of installed docker client and server* <br>
`docker version` <br>
*print docker configuration* <br>
`docker info` <br>

## container
*run detached container with name __webhost__ from the __nginx__ image and forward __8080__ port of the host to the __80__ port of the container* <br>
`docker container run --publish 8080:80 --detach --name webhost nginx` <br>
`docker container run -p 8080:80 -d --name webhost nginx` <br>
*print published ports for the __webhost__ container* <br>
`docker container port webhost` <br>
*view only running containers* <br>
`docler container ls` <br>
*view all containers* <br>
`docker container ls -a` <br>
*realtime resources monitor for all running containers* <br>
`docker container stats` <br>
*print configuration for the __webhost__ container* <br>
`docker container inspect webhost` <br>
*print only IPAddress for the __webhost__ container (or any another field by specified pattern)* <br>
`docker container inspect --format '{{.NetworkSettings.IPAddress}}' webhost` <br>
*stop container webhost* <br>
`docker container stop webhost` <br>
*print logs of the detached and running container __webhost__* <br>
`docker container logs webhost` <br>
*print process list of the running container __webhost__* <br>
`docker container top webhost` <br>
*remove stopped containers __webhost1__ and  __webhost2__*<br>
`docker container rm webhost1 webhost2` <br>
*stop and remove running containers __webhost1__ and  __webhost2__ (will work on stopped containers as well )* <br>
`docker container rm -f webhost1 webhost2` <br>
*run container with name __my_ubuntu__ from the __ubuntu__ image, and open its __bash__* <br>
`docker container run -it --name my_ubuntu ubuntu bash` <br>
*run container from the __centos__ image, exec __curl__ command on the __google__, than exit and remove container* <br>
`docker container run --rm centos curl https://www.google.com` <br>

## image
*get list of pulled images* <br>
`docker image ls` <br>
*remove cached images __centos__, __ubuntu__ and __nginx__* <br>
`docker image rm centos ubuntu nginx` <br>

## network
*get a list of all networks* <br>
`docker network ls` <br>
*get configuraion of the network with name __bridge__ (in addition contains a list of connected containers)* <br>
`docker network inspect bridge` <br>
*create new network with name __my_network__ and default driver (bridge)* <br>
`docker network create my_network` <br>
*connect container __webhost__ to the __my_network__* <br>
`docker network connect my_network webhost` <br>
*connect container __webhost_1__ to the __my_network__ and assign alias __webhost__ to the container* <br>
`docker network connect --alias webhost my_network webhost_1` <br>
*disconnect container __webhost_1__ from the __my_network__* <br>
`docker network disconnect my_network webhost_1` <br>
*run container __webhost_1__ and connect it to the __my_network__ (will be connected to the __my_network__ only)* <br>
`docker container run -d --name webhost_1 --network my_network nginx:alpine` <br>
`docker container run -d --name webhost_1 --net my_network nginx:alpine` <br>
*ping container __webhost_2__ from the __webhost_1__* <br>
`docker container exec webhost_1 ping webhost_2` <br>

