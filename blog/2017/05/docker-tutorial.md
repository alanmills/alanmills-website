Get Started with Docker Tutorial
================================
**Author:** [Alan Mills]
**Date:** [10 May 2017 22:16]
**Tags:** [Docker]
**Status**: Draft

[Getting started with Docker](https://docs.docker.com/get-started/)

Docker service
--------------
docker-compose.yml
``` yml
version: "3"
services:
  web:
    image: alanmills/friendlyhello:latest
    deploy:
      replicas: 5
      resources:
        limits:
          cpus: "0.1"
          memory: 50M
      restart_policy:
        condition: on-failure
    ports:
      - "80:80"
    networks:
      - webnet
networks:
  webnet:
```
This spins up an environment with an network called webnet with 5 replicas of alanmills/firendly:latest, available on port 80 attached to the webnet network.  This is a load-balanced network so if you `curl http://localhost` you will get a response from one of the containers chosen at random.

```bash
docker swarm init
docker stack deploy -c docker-compose.yml getstartedlab
docker stack ps getstartedlab
docker stack rm getstartedlab
```



Docker Swarm using Parallels
----------------------------
To use Parallels, use the [Docker Machine Parallels Driver](https://github.com/Parallels/docker-machine-parallels/)

1. Install the Parallels docker-machine driver
``` bash
brew install docker-machine-parallels
```

2. Create the machines for the Docker Swarm
``` bash
docker-machine create --driver=parallels myvm1
docker-machine create --driver=parallels myvm2
```

3. Setup the first machine to be the swarm manager
``` bash
docker-machine ssh myvm1 "docker swarm init"
```

4. Setup the sedond machine to join the swarm
``` bash
docker-machine ssh myvm2 "docker swarm join \
    --token SWMTKN-1-4vyj8dvyfxrwhisw9wetipbxn7buf9zzagrz7qlqfmtpkh1tdq-5daro206yeb1mv274rdxekvu2 \
    10.211.55.174:2377"
```

5. Copy the definition of your Docker service to the Swarm
``` bash
docker-machine scp docker-componse.yml myvm1:~
```
This copies the `docker-compose.yml` file to the home directory `~` on the swarm manager (myvm1).

6. Deploy the Docker Service to the Swarm
``` bash
docker-machine ssh myvm1 "docker stack deploy -c docker-compose.yml getstartedlab"
docker-machine ssh myvm1 "docker stack ps getstartedlab"
```
7. Test the deployed service
``` bash
docker-machine url myvm1
curl http://#.#.#.#
open http://#.#.#.#
```

Docker Stack
------------
