# doker_cheatlist
my own docker cheats

## get docker
mac:    https://docs.docker.com/docker-for-mac/install/ <br>
win:    https://docs.docker.com/docker-for-windows/install/ <br>
ununtu: https://docs.docker.com/install/linux/docker-ce/ubuntu/ <br>

## iTerm2
*good replacement for the standard mac terminal* <br>
https://www.iterm2.com/


## install command line completion
https://docs.docker.com/compose/completion/ <br>

## basic 
*print version of the installed docker client and server* <br>
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
