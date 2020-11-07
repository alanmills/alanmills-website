Review of Pluralsight course: Getting Started with Docker (2016/08/02)
======================================================================

**Author:** Alan Mills  
**Date:** [2 December 2016 10:25](/blog/history/2016-12.md)  
**Tags:** [Pluralsight](/blog/categories/pluralsight.md), [Docker](blog/categories/docker.md)
**Status**: Draft

The Pluralsight course [Getting Started with Docker](https://app.pluralsight.com/library/courses/docker-getting-started/table-of-contents), published on 2 August 2016 is given by [Nigel Poulton](http://blog.nigelpoulton.com).


Overview
--------
* Course Overview
* Course Introduction
* Installing Docker
* Working with Containers
* Swarm Mode and Microservices
* What Next!

Course Overview
---------------
Course aims to cover:
* Native Orchestration
* Declarative service Model
* Scale services
* Rolling application updates
* Stacks and bundles

Course Introduction
-------------------
Course is based on Docker 1.12 (released 29 July 2016) and includes the clustering and orchestration features introduced as part of Docker 1.12.

Installing Docker
-----------------
In this section,
* Windows 10, 64-bit, clean install, no existing Docker installs.
* Docker for Mac, much better than boot2docker

### Docker for Windows is running on-top of Hyper-V

### Docker for Mac is running on-top of macOS Yosemite (10.10.03) using a number of libraries
* [HyperKit](https://github.com/docker/hyperkit) - is providing a lightweight hypervisor based on X-Hive with some extensions
* [DataKit](https://github.com/docker/datakit) -
* [Moby Linux](https://github.com/docker/moby) - A super small linux distro based on Alpine Linux focused on security and speed.
  * Includes binfmt_misc for supporting ARM and PowerPC

### Docker for Windows Server 2016
This is currently showing the process for Windows Server 2016 Tech preview 5.  As running docker on Windows is not an area taht I'm focused on then I'm only partially interested.

### Tagging Docker images
There are a number
``` bash
$ docker images
REPOSITORY          TAG                IMAGES ID      CREATED        SIZE
windowsservercore   10.0.14300.1000    5bc36a335344   8 weeks ago    9.354 GB

$ docker tag 5bc36a335344 windowsservercore:latest

$ docker images
REPOSITORY          TAG                IMAGES ID      CREATED        SIZE
windowsservercore   10.0.14300.1000    5bc36a335344   8 weeks ago    9.354 GB
windowsservercore   latest             5bc36a335344   8 weeks ago    9.354 GB
```

``` bash
$ docker run -it windowsservercore cmd
ctrl + pq
$ docker ps
```

### Installing Docker on Linux (Ubuntu)
``` bash
$ wget -q0- https://get.docker.com/ | sh
```

Working with Containers
-----------------------
Docker Engine is normally the combination of the Docker Client and Docker Daemon however, some people will just mean the Docker Daemon part.

The Docker Client makes API calls to the Docker Dameon, the Docker Daemon implements the Docker Remote API.

### Useful Docker commands
`docker version` - Client and server versions that you are running locally
`docker info` - View what is happening on your docker host
`docker run hello-world` - This command got the hello-world container from the Docker hub, it then runs the docker image streaming the output to the Terminal.
`docker pa -a` - Lists the docker processes run and when they ran.
`docker images` - Outputs the locally stored docker images
`docker pull ubuntu` - Pulls the latest image from Docker Hub, in this case the latest Ubuntu image.
`docker pull ubuntu:14.04` - Pulls the specified image from Docker Hub, in this case Ubuntu 14.04
`docker rmi ubuntu:14.04` - Removes the specified image from the local host.

### Running a Docker hosted web server
`docker run -d --name web -p 80:8080 nigelpoulton/pluralsight-docker-ci`
* run the docker image **nigelpoulton/pluralsight-docker-ci**; this is a second level image, in this case managed by nigelpulton - potentially this means it is less secure.
* -d run in detached mode, run it in the background not attached to the terminal
* --name web is give the container a unique name, in this case web
* -p 80:8080 is map the container port 8080 to port 80 on the docker host

You can access the website that this docker container is hosting through [http://localhost](http://localhost).

#### Start/Stop the docker container
We can see that we can start/stop the image by using the browser and the Terminal.
* `docker stop web`, refresh browser; and your browser should show that it cannot connect to [http://localhost](http://localhost)
* `docker start web`, refresh the browser; and your browser should re-show the website hosted by the container.

### Running an interactive Docker hosted Ubuntu container
`docker run -it --name temp ubuntu:latest /bin/bash`

If you run ps -elf, you'll see that there is really only bash running.  This is because containers are designed to be **single process constructs**.  You can use them for managing environments but it's not normally done; see **Vagrant** as an alternative.

If you run `exit`, bash will stop and as the container is only running *bash* then the container will stop.  If you want to leave the interactive container but leave it running, run <kbd>Ctrl</kbd>+<kbd>CQ</kbd>

### Cleaning up Docker Containers
* `docker ps -aq` - returns all docker containers that are on the Docker Host, the `-q` option brings back just the *Container ID* for each docker container.
* `docker stop $(docker ps -aq)` - stops all of the docker containers on the Docker Host
* `docker rm $(docker ps -aq)` - removes all of the docker containers on the Docker Host

### Cleaning up Docker images
* `docker rmi $(docker images -q)`

**image is reference in one or more repositories** and **image has dependent child images** I am getting these errors from the serverless conference; need to find out how to resolve.

Swarm Mode and Microservices
----------------------------
Native container orchestration from Docker, things like:
* Swarm Mode
* Swarms
* Services and tasks
* Stacks & bundles

Swarm mode is built into Docker 1.12+ and you can optionally turn swarm mode on/off.  Therefore if you are using another clustering solution, then you can leave Swarm mode off and everything works as it did before.

Manager nodes maintain the Swarm
* H/A - recommend 3 or 5
* Only one is the **leader**
* **Raft** is the protocol and algorithm used to bring consensus to status of the swarm.

Worker nodes execute tasks

Services (new to Docker 1.12 and requires Swarm mode)
* Declarative & scaleable way to run tasks
* `Docker service create --name web-fe --replicas 5 ...`
* `--replicas 5` is a desired state of having 5 instances of the service *web-fe*; should one or more of them fail, then Docker will make sure that it will recover all 5 replicas to a working state.

**Tasks** Today tasks is essentially a *container* with some other meta data but in the past this could be *unikernals*, *lambdas*, etc.

### Build a Swarm
We are going to build a swarm with:
* 3x Managers
* 3x Workers

From a docker manager: `docker swarm init --advertise-addr 172.31.12.161:2377 --listen-addr 172.31.12.161:2377`
* `--advertise-addr 172.31.12.161:2377` and `--listen-addr 172.31.12.161:2377` this tells Docker Swarm which NIC address to communicate with and which Port
* Any other node that wants to be part of the swarm has to be on the same network and or routes to traverse the network.
* Native Docker engine port is *2375*
* The secured Docker engine port is *2376*
* The Swarm port is *2377*; Docker are working with IANA to get this officially registered

* When the manager initialises, the command will kick out a command to join the new swarm created by the manager.  It will be something like ``
