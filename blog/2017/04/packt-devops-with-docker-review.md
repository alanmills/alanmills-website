Packt Learning Path: DevOps with Docker Review
==============================================
**Author:** [Alan Mills]
**Date:** [30 April 2017 21:46]
**Tags:** [Docker], [Packt], [Safari Books Online]
**Status**: Draft

This course is made up of two Packt video training courses:
* [Beginning Docker](https://www.packtpub.com/virtualization-and-cloud/beginning-docker-video) by [Donald Simpson](http://www.donaldsimpson.co.uk)
* [Docker for Web Developers](https://www.packtpub.com/virtualization-and-cloud/docker-web-developers-video) by [Ian Miell](https://zwischenzugs.wordpress.com)

Beginning Docker
================

``` bash
docker run ubuntu --rm ls
docker ps -a
docker run ubuntu -i -t bash
docker run ubuntu -d \etc\bash -c "apt-get update && ..."
docker ps -lq
docker logs `docker ps -lq`
docker ps -aq | xargs docker rm -f
```

Committing Chages to a Container Image
--------------------------------------
Interactively install the Open SSH Server on an Ubuntu container image
``` bash
docker run ubuntu -i -t bash
apt-get update
apt-get install -y openssh-server
mkdir /var/run/sshd
echo 'root:demo' | chpasswd
sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/g' /etc/ssh/sshd_config
sed -ri 's/UsePAM yes/UsePAM no/g' /etc/ssh/sshd_config
exit
```

**View file changes**  The docker image is stopped and still on your host system.  You can view all of the file changes by running the command ``` docker diff `docker ps -lq` | less ```

**Save the modified docker image** You can save the modified container by running the command ``` docker commit `docker ps -lq` cadamus/sshd ```

**Create a new docker machine from the saved image** run the command ``` docker run -d -p 2222:22 cadamus/sshd /usr/sbin/sshd -D ```

**Connect to new SSH docker machine**  run the command ``` ssh root@localhost -p 2222 ```

**Clean up the docker machine** ``` docker -rm `docker ps -lq` ```

Sharing a Container on the Index (Docker Hub)
---------------------------------------------
[https://hub.docker.com](https://hub.docker.com)

As long as you setup your Docker Hub username the same as the username you used to save the SSHD image e.g. cadamus/sshd, then you can push the image to the Docker Hub using the commands:
```
docker login
docker commit `docker ps -lq cadamus/sshd_demo`
docker push cadamus/sshd_demo
```
When we commit the docker image to cadamus/sshd_demo, the command line arguments that we used to run the last image are saved to the image too, these are ```-d -p 2222:22 cadamus/sshd /usr/sbin/sshd -D``` instead of ```bash``` which is what we used to create the cadamus/sshd image.

**Test pulling the new image down to local host**
1. First make sure no images are running usig your cadamus/sshd image ``` docker ps -a ```
2. Kills any images running using the cadamus/sshd or cadamus/sshd_demo image ``` docker kill ID ```
3. Pull and run the image
```
docker pull cadamus/sshd_demo
docker run -d -p 2223:22 cadamus/sshd_demo
```
This last run command overrights the port mapping that we used to create the image.  We also do not need run the pull command as when we run a docker image, if it's not stored locally, docker will download it.

Finding and Using Third-party Containers
----------------------------------------
Using redis - https://hub.docker.com/_/redis/
```
docker pull redis
docker run --name don-redis -d redis
docker logs don-redis
docker run --rm -i -t --entrypoint="bash" --link don-redis:redis redis -c 'redis-cli -h $REDIS_PORT_6379_TCP_ADDR'
set foobar Hello
```

Writing and Building a Dockerfile
=================================
```
mkdir sshd-example
cd sshd-example
touch Dockerfile
vim Dockerfile
```
Dockerfile
```
FROM ubuntu

MAINTAINER Alan Mills <alan@alanmills.info>

RUN apt-get update && apt-get install -y openssh-server

RUN mkdir -p /var/run/sshd
```

1. Build the docker image `docker build -t cadamus/sshd-example .`
2. Now we are going to ADD the sshd_config to the container.  First off, we need to get the configuraiton file, so we will pull this from our image and place it into a new `sshd_config` local file `docker run --rm -it cadamus/sshd-example cat /etc/ssh/sshd_config > sshd_config`.
3. Add to the end of the Dockerfile `ADD sshd_config /etc/ssh/sshd_config`
4. Re-build the docker image `docker build -t cadamus/sshd-example .`
5. Inspect the new config file added to the updated docker image `docker run --rm -it cadamus/sshd-example cat /etc/ssh/sshd_config`

Within the **Dockerfile** we can specify the default command to run when the docker image is started.  This can be done with:
* **CMD** This can be changed by the user running the image
* **ENTRYPOINT**  This can only be appended by the user running the image and therefore should only be used if you really know what you are doing...

Now append the Dockerfile with `echo CMD /usr/sbin/sshd -D >> Dockerfile`

Lets tests this is working
``` bash
docker build -t cadamus/sshd-example .
docker run -d cadamus/sshd-example
docker ps
docker rm -f `docker ps -lq`
```

We can now change some of the default properties
``` bash
echo USER nobody >> Dockerfile
echo WORKDIR /tmp >> Dockerfile
echo ENV foobar "Hello World" >> Dockerfile
docker build -t cadamus/sshd-example .

docker run --rm -it cadamus/sshd-example id
docker run --rm -it cadamus/sshd-example pwd
docker run --rm -it cadamus/sshd-example env
```

We are also going to provide some metadata that the Container exposes a service, in this case on port 2222
``` bash
echo EXPOSE 2222 >> Dockerfile
docker build -t cadamus/sshd-example .
docker run -d cadamus/sshd-example
docker ps
docker rm -f `docker ps -lq`
```
We can see from the docker ps command that the image is communicating on the port 2222.

Building on existing containers
-------------------------------
As in the last section, we are going to build a new Docker template, this time called **sshd-custom**.

``` bash
mkdir sshd-custom
touch Dockerfile
vim Dockerfile
```
Dockerfile
``` bash
FROM cadamus/sshd-example
MAINTAINER Alan Mills <alan@alanmills.info>
USER root
```
``` bash
docker build -t cadamus\sshd-custom .
docker run --rm -it cadamus\sshd-custom id
```

We now want to enable the sshd-custom container to provide its own version of the sshd_config file.  To do this we go back to the **cadamus/sshd-example** template and change the Dockerfile line `ADD sshd_config /etc/ssh/sshd_config` and add **ONBUILD** to the start of the line, therefore it becomes `ONBUILD ADD sshd_config /etc/ssh/sshd_config`.  After making this change, rebuild the the image `docker build -t cadamus/sshd-example .`

Copy the sshd_conf from sshd-example to sshd-custom
Build sshd-custom and we will see that the sshd_conf in the sshd-custom directory is pulled into the build for sshd-custom even through the sshd-custom Dockerfile does not specify to ADD this file.

Trusted Builds
--------------
These are the ideal way to manage public docker images as you do not have to rember to push built images to the Docker Hub.  The Docker Hub will watch your GitHub or BitBucket account and automatically build the Container when you push updates to VCS.

To do this we will need a Public [Github](https://github.com) repository.
1. Create a new github project called sshd-example, I set this up adding a README.md and LICENSE file ([MIT](https://choosealicense.com/licenses/mit/))
2. I like to work with SSH keys, under the **Clone or download button** copy the **Clone with SSH** string
3. Run the following commands
``` bash
git init
git remote add origin git@github.com:alanmills/sshd-example.git
git add .
git branch --set-upstream-to=origin/master master
git fetch origin
git merge origin/master --allow-unrelated-histories
git push
```
4. Link your Github account to your [Docker Hub account](https://hub.docker.com/account/authorized-services/)

Contraining Container Resources
-------------------------------
### CPU priority
``` bash
docker ps -aq | xargs docker rm -f
docker run --rm donaldsimpson/primesum 9000
```
Running on it's own on my laptop this took 19.02s

Then run five in the background, to see competing resources.
``` bash
docker ps -aq | xargs docker rm -f
docker run -d donaldsimpson/primesum 9000
docker run -d donaldsimpson/primesum 9000
docker run -d donaldsimpson/primesum 9000
docker run -d donaldsimpson/primesum 9000
docker run -d donaldsimpson/primesum 9000
docker logs `docker ps -aq`
```
On my laptop, the last instance took 26.82s

``` bash
docker ps -aq | xargs rm -f
docker run -d donaldsimpson/primesum 9000
docker run -d donaldsimpson/primesum 9000
docker run -d donaldsimpson/primesum 9000
docker run -d donaldsimpson/primesum 9000
docker run -d -c=10 donaldsimpson/primesum 9000
docker logs `docker ps -aq`
```
On my laptop, the last instance took 25.14s

So we can see that running multiple docker images at the same time takes longer than a single image however, we can use the relative weighting `-c=10` to priortise one or more images over the other images.

### Memory
``` bash
docker ps -aq | xargs docker rm -f
docker run --rm donaldsimpson/memalloc 256
ps aux | grep [m]em
docker rm -f $(docker ps -lq)
```

Now run docker telling it how much memory to allocate
``` bash
docker run -m=256mb donaldsimpson/memalloc 512 512
```

Override dockerfile defaults
----------------------------
To override the entrypoint when we tell docker to run.
``` bash
docker run -ti --entrypoing=/bin/sh donaldsimpson/memalloc -s
```

We can also override the user as using root is bad practice and can cause bugs.
``` bash
docker run --rm -it -u=nobody ubuntu bash
id
exit
```

Most common thing to do is to provide environment variables
``` bash
docker run --rm -it ubuntu bash 
env
exit
docker run --rm -it -e VAR1=HelloWorld -e VAR2=foobar ubuntu bash
env
exit
```

Docker volumes and mounts
-------------------------
### Mounts
When using docker for DBs and other persistent storage needs.  We can do this by mounting folders from the host system to the client system.

``` bash
mkdir mounts
cd mounts
echo Hello from the host system >> hostfile
docker run --rm -it -v $(pwd):/mnt ubuntu bash
cd mnt
cat hostfile
```

By default these mounts are read/write however, these can be made readonly.

### Volumes
Volumes are really only for consumption by containers and will only exist while a container is using it.

``` bash
docker run --rm -it -v /var/volume1 ubuntu bash
cd /var/volume1
touch file
```

We can connect one containers volume to another container
``` bash
docker run --name alanmem -d -v /var/volume1 donaldsimpson/memalloc 256 256
docker run --rm -it --volumes-from alanmem ubuntu bash
cd /var/volume1
ls
touch file
ls
exit
docker run --rm -it --volumes-from alanmem ubuntu bash
cd /var/volume1
ls
exit
```

Ports and Networking
--------------------
The `-P` option will publish the Exposed docker ports.  This will map the container port to an available port on the host.  To get the host port, you can use the `docker port` command to get the host port.
``` bash
docker run -d -P redis
docker port $(docker ps -lq) 6379
docker rm -f $(docker ps -lq)
docker run -d -p 6379:6379 redis
```

More common is to use the `-p public port:private port` option as it gives us more control.  We can also further control this by ensuring thatit is only available from localhost `docker run -d -p 127.0.0.1:6379:6379`.

Each docker host runs in its own network and uses NAT to publish the ports.

By default all outbound networking is allowed, this can be changed by providing the `--net="none"` option. 
``` bash
docker run --rm -it --net="none" ubuntu bash
ping google.com
```

Linking containers together
---------------------------
We can connect containers together using port mapping however, like for volumes there is another way to `--link` two containers together.
``` bash
docker run -d --name redis redis
docker run --rm -it --link redis:db ubuntu bash
env
```
We can see that the `ENV` and `EXPOSE` statements from the Redis Dockerfile have been exposed in the linked ubuntu environment variables.

db has been placed in hosts file and can be pinged.
```
cat /etc/hosts
apt-get update
apt-get install -y iputils-ping
ping -c 3 db
exit
```

Now lets use it
``` bash
docker run --rm -it --link redis:redis redis bash
redis-cli -h $REDIS_PORT_6379_TCP_ADDR
set foo bar
get foo
exit
```

Writing a simple application
----------------------------
``` bash
mkdir helloworld
cd helloworld
vim Dockerfile
vim hello.py
vim Makefile
make
docker run -d --name redis redis
docker run -d --link redis:db -p 8000:8000 alanmills/helloworld
curl http://locahost:8000
```

Makefile
``` make
build:
    docker build -t alanmills/helloworld .
```

Dockerfile
``` docker
FROM ubuntu
MAINTAINER Alan Mills <alan@alanmills.info>

RUN apt-get update
RUN apt-get install -y python python-pip
RUN pip install redis flask

ADD ./hello.py /hello.py

EXPOSE 8000
CMD ["python", "/hello.py"]
```

hello.py
``` python
from flask import Flask
import redis, os
app = Flask(__name__)
db = redis.Redis(host=os.environ.get("DB_PORT_6379_TCP_ADDR", "localhost"))

@app.route("/")
def hello():
    db.incr("counter")
    return "Hello, World! {0}\n".format(db.get("counter"))

if __name__ == "__main__":
    app.run(Port=8000, host="0.0.0.0")
```

Setting up Production
---------------------
In the video, Donald leverages Cloudera, I am going to use Amazon.

1. Setup an EC2 instance using Ubuntu 16.04 LTS AMI
    * I setup the EC2 using the free tier (t2.micro)
    * Setup a security group with SSH access to myIP
    * Setup a security group with HTTPS to myIP for the Docker Registry
    * Enabled the use of an SSH key that I already had setup
2. SSH to the EC2 instance
``` bash
aws ec2 describe-instances
ssh -i ~/.ssh/aws-ec2-ssh.pem ubuntu@ec2-52-211-161-228.eu-west-1.compute.amazonaws.com
```
3. Get packages updated and install Docker, following the [docker installation instructions](https://store.docker.com/editions/community/docker-ce-server-ubuntu?tab=description)
``` bash
sudo apt-get update
sudo apt-get -y install apt-transport-https ca-certificates curl
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt-get update
sudo apt-get install -y docker-ce
sudo docker run --rm hello-world
sudo docker rmi hello-world
```

4. Setup Redis 
```bash
sudo docker run -d --name redis redis
sudo docker logs redis
```

5. Setup Let's Encrypt needed for Docker Hub
    * Install [Certbot](https://certbot.eff.org/#ubuntuxenial-other)
    ``` bash
    sudo add-apt-repository ppa:certbot/certbot
    sudo apt-get update
    sudo apt-get install -y certbot
    ```

5. Setup [Docker Registry](https://hub.docker.com/_/registry/)
```bash 
sudo docker run -d --name registry -p 443:5000 --restart always -v /tmp/registry:/tmp/registry -e REGISTRY_HTTP_TLS_LETSENCRYPT_CACHEFILE=/tmp/letencryptcachefile -e REGISTRY_HTTP_TLS_LETSENCRYPT_EMAIL=alan@alanmills.info registry:2
sudo docker logs registry
```
* **Look at the documentation for using Let's Encrypt**https://docs.docker.com/registry/deploying/#get-a-certificate
* **Look at the AWS documentation for giving our server a more 'friendly' name**

6. Publish my application to the production server.  On my local workstation
``` bash
docker tag alanmills/helloworld ec2-52-211-161-228.eu-west-1.compute.amazonaws.com:5000/alanmills/helloworld
docker push ec2-52-211-161-228.eu-west-1.compute.amazonaws.com:5000/alanmills/helloworld
```

7. Run the applicaiton on the production server
``` bash
sudo docker run -d --name helloworld --link redis:db ec2-52-211-161-228.eu-west-1.compute.amazonaws.com:5000/alanmills/helloworld
curl http://localhost:8000
```


[Insecure docker registry](https://docs.docker.com/registry/insecure/)

``` bash
docker run --rm -it -v $(pwd):/helloworld ubuntu bash
apt-get update
apt-get install -y software-properties-common lsb-release vim apt-transport-https make
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -
add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
apt-get update
apt-get install -y docker-ce
service docker start
make
docker run -d --name redis redis
docker run -d --name hello --link redis:db -p 8000:8000 alanmills/helloworld
curl http://localhost:8000
docker rm -f hello
docker rum -r redis
```

https://docs.docker.com/registry/deploying/
https://docs.docker.com/registry/insecure/#deploying-a-plain-http-registry

deploy-app.sh
```
vim deploy-app.sh

#!/bin/bash
set -e
ip="$(curl icanhazip.com -s)"
name="$1"
version="$2"
port=8000
registry=""

echo "Pulling $version from registry..."
sudo docker pull $name:$version > /dev/null
sudo docker tag $name:$version $name:$version
echo "Stopping existing versions..."
sudo docker rm -f $(docker ps | grep $name | cut -d ' ' -f 1) > /dev/null 2>&1 || true
echo "Starting version $version..."
sudo docker run -d -p $port:$port --link redis:db $name:$version > /dev/null
echo "$name deployed:"
echo "    $(sudo docker port 'docker ps -lq' $port | sed s/0.0.0.0/$ip/)"

chmod +x deploy-app.sh
```

### Deploy and tag as a specific version
`make VERSION=v1`


Using the docker API
--------------------
bash
```
curl --unix-socket /var/run/docker.sock -X POST http:/v1.24/containers/5355c0badcf4/start
curl --unix-socket /var/run/docker.sock -X POST http:/v1.24/containers/5355c0badcf4/wait
curl --unix-socket /var/run/docker.sock "http:/v1.24/containers/5355c0badcf4/logs?stdout=1"
```

### From Python
``` bash
docker run --rm -it -v /var/run/docker.sock:/var/run/docker.sock ubunut bash
apt-get update
apt-get install -y python python-pip
pip install docker-py
python
import docker
client = docker.from_env()
client.containers() # List the existing containers
container = client.create_container("ubuntu", "echo Hello world")
client.start(container)
client.logs(container)
```

### From node
This makes use of [apocas/dockerode](https://github.com/apocas/dockerode)
``` bash
docker run --rm -it -v `pwd`:/code -v /var/etc/docker.sock:/var/etc/docker.sock ubuntu bash
apt-get update
apt-get install -y curl vim
curl -sL https://deb.nodesource.com/setup_6.x | bash -
apt-get install -y nodejs
npm install npm@latest -g
cd /code
mkdir runcontainer && cd runcontainer
npm init - except all defaults
npm install dockerrode --save

vim listcontainers.js
nodejs listcontainers.js

vim runcontainer.js
nodejs runcontainer.js
```

listcontainers.js
``` javascript
var Docker = require('dockerode');
var docker = new Docker({socketPath: '/var/run/docker.sock'});

docker.listContainers(function(err, containers) {
    containers.forEach(function(containerInfo) {
        console.log(containerInfo);
    });
});
```

runcontainer.js  **this does not output the stdout to the console - find out about node streams**
``` javascript
var Docker = require('dockerode');
var docker = new Docker({socketPath: '/var/run/docker.sock'});

docker.createContainer({Image: 'ubuntu', Cmd: ['echo', 'Hello, World!'],
}).then(function(container) {
  return container.start();
}).then(function(container) {
  container.attach({stream: true, stdout: true, stderr: true}, function(err, stream) {
    stream.pipe(process.stdout);
  });
  return container;
}).then(function(container) {
  return container.stop();
}).then(function(container) {
  return container.remove();
}).then(function(data) {
  console.log('container removed');
}).catch(function(err) {
  console.log(err);
});
```

Container management within a Container
---------------------------------------
We can manage the docker server from within a docker container.  In the following script snippet we install docker into the docker container.  This is to get the docker tools into the container.  By sharing the docker named pipe with the container, when we use the docker tools, the commands are running on the docker host.

``` bash
docker run --rm -it -v /var/run/docker.soc ubuntu bash
apt-get update
apt-get update
apt-get install -y software-properties-common lsb-release vim apt-transport-https make
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -
add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
apt-get update
apt-get install -y docker-ce

docker ps -a
docker run --rm -it ubuntu bash
echo Hello, World!
exit

exit
```


Managing Docker Logs with logspout
----------------------------------
[gliderlabs/logspout](https://github.com/gliderlabs/logspout) allows us to export docker logs to syslog.  Docker only captures stdout and stderr.

In this example we run a few docker containers that push output to these outputs.

### Output locally

``` bash
docker run -d --name log1 ubuntu bash -c "while true; do echo \$RANDOM; sleep 1; done"
docker run -d --name log2 ubuntu bash -c "while true; do echo \$RANDOM; sleep 1; done"
docker run -d --name log3 ubuntu bash -c "while true; do echo \$RANDOM; sleep 1; done"
docker run -d --name logs -P -v /var/run/docker.sock:/tmp/docker.sock gliderlabs/logspout

curl `docker port logs 80 | sed s/0.0.0.0/localhost/`/logs
curl 'docker port logs 80 | sed s/0.0.0.0/localhost/'/logs/name:log1
```

### Output to Papertrail
This section makes the assumption that the docker containers crating random numbers is still running.

``` bash
docker rm -f logs
docker run -d --name logs -v /var/run/docker.sock:/tmp/docker.sock gliderlabs/logspout syslog://logs5.papertrailapp.com:26626
# look at the Papertrail Dashboard, when done.
docker ps -aq | xargs docker rm -f
```


Creating your Own PaaS with Dokku
---------------------------------
[progrium/dokku](https://github.com/dokku/dokku)

In the video, Digital Ocean is used.  I am going to continue to use AWS.  the Dokku bootstrapper assumes that Docker is not installed and therefore, you should create a new EC2 instances with SSH and HTTP access.

1. Setup an EC2 instance using Ubuntu 16.04 LTS AMI
    * I setup the EC2 using the free tier (t2.micro)
    * Setup a security group with SSH access to myIP
    * Setup a security group with HTTP to myIP for the Dokku web interface
    * Enabled the use of an SSH key that I already had setup

2. SSH to the EC2 instance
``` bash
aws ec2 describe-instances
ssh -i ~/.ssh/aws-cadamus-ec2-key.pem ubuntu@ec2-34-253-44-42.eu-west-1.compute.amazonaws.com
```

3. Install Dokku
``` bash
wget https://raw.githubusercontent.com/dokku/dokku/v0.9.4/bootstrap.sh
sudo DOKKU_TAG=v0.9.4 bash bootstrap.sh 
exit
```

4. Get the Heroku Node.JS sample project to deploy
``` bash
mkdir dokku && cd dokku
git clone https://github.com/heroku/node-js-sample.git
```

5. Now attach our new Dokku server as the remote for the node-js-sample git project
``` bash
cd node-js-sample.git
git remote add nodejsdemo ubuntu@34.253.44.42:nodejsdemo
git push nodejsdemo master
```

6. Lanch node-js-sample in the browser, the IP:Port will be output once git has pushed the code to the Dokku server.

Docker for Web Developers
=========================
This starts by repeating the introduction material that was covered by [Beginning Docker](https://www.packtpub.com/virtualization-and-cloud/beginning-docker-video) e.g.
* What is Docker
* Installing Docker
* Getting Ready with Docker - with the exception of setting up a bitbucket account
* Working with Containers
* Diving Deep into Containers and Images
* sort of for Playing 2048 with Docker and Saving the state
* Uploading Images to Docker Hub
* Managing the Containers
* Understanding Dockerfiles
* Iterating Our App to Succeed
* Setting Up OUr BitBucket Project
* Setting Up and Triggering a DockerHub Build
* Iterating to Success
* Opening Up Our Containers to the World
* Linking Containers to Each Other
* Interacting with the Host Environment



Playing 2048 with Docker
------------------------

``` bash
docker run -d --name win2048 -p 5901:5901 -p 6080:6080 imiell/win2048 /bin/bash -c '/root/start_win2048.sh && sleep infinity'
docker logs win2048
docker top win2048
vncviewer localhost:1
```

Saving the state
----------------
``` bash
docker pause win2048
docker tag `docker commit win2048` alanmills/myfirst2048image
docker rm -f win2048
docker run -d --name win2048 -p 5901:5901 -p 6080:6080 alanmills/myfirst2048image /bin/bash -c 'root/start_win2048.sh && sleep infinity'
vncviewer localhost:1
```

Managing the Images
-------------------
`docker history alanmills/myfirst2048image` will give you the history of file system layers used to make up the latest version of the image.

`docker events --since=0` will query for events on your systems docker demon.

Managing the Containers
-----------------------
You can start a stopped docker container by using the `docker start id` command and restarted using the `docker restart id` commands.

You can copy files from a container e.g. 
``` bash
docker cp id:/etc/passwd /tmp
cat /tmp/passwd
```

You can modify the docker container with the `docker exec id` command view the changes with the `docker diff id` commands e.g.
``` bash
docker exec id touch /tmp/hello
docker diff id
```

Understanding Dockerfiles
-------------------------
The course makes reference to a website [The Mortgage Meter](www.themortgagemeter.com), this site was down when I went throug this course!

``` bash
git clone https://github.com/ianmiell/simple-dockerfile.git
cd simple-dockerfile
cat Dockerfile
docker build -t alanmills/simple .
docker run --rm alanmills/simple
cd ..
```

``` bash
curl -LO https://github.com/ianmiell/themortgagemeter/archive/master.zip
unzip master.zip
cd themortgagemeter - master/
docker build -t alanmills/themortgagemeter .
docker build -t --no-cache alanmills/themortgagemeter . # if you want to rebuild ignoring the previous cached layers
```

``` bash
docker run -d -p 80:80 -p 2222:22 alanmills/themortgagemeter
```

If you want to run your docker container as though you were running your project on the docker host (your workstation), then run the command 
``` bash
docker run -d --net=host alanmills/themortgagemeter
```

Building a project workflow
---------------------------
1. bitbucket, create a new project
2. Clone the new project to your local workstation
3. Write your application
4. Push the changes to master
5. Connect your BitBucket repo to Docker Hub and trigger a build on change.


Orchestrating Containers with Docker Compose
--------------------------------------------
In this scenario Ian uses Docker Compose, as Docker Swarm is built into Docker I:
* Watched the videos
* Completed the Docker Swarm 

docker-compose.yml
``` yml
one:
 image: imiell/sqlite
 command: nc -l 12345
two:
 image: imeiell/sqlite
 command: /bin/bash -c 'sleep 3 && socat FILE:/etc/issue TCP:one:12345'
 link:
  - one:one
```

Complex Orchestration with Docker Compose
-----------------------------------------
``` bash
git clone https://github.com/ianmiell/docker-componse-example.git
./setup_dbs.sh
docker-compose up &
telnet localhost 12346
select * from t1;
```

docker-componse.yml
``` yml
one:
 command: socate TCP-L:12345,fork,reuaseaddr EXEC:'sqlite /opt/sqllite/live',pty
 image: imiell/sqlite
 volumes:
- /tmp/sqlitedbs/live:/opt/sqlite/live
two
 command: socat TCP-L:12346,fork,reuseaddr TCP:one:12345
 image: imiell/sqlite
 links:
 - one:one
 ports:
 - 12346:12346
```
