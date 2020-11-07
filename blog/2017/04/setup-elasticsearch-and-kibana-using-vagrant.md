Setup Elasticsearh and Kibana 5.3.0 on Ubuntu 16.04 using Parallels 12.x and Vagrant 1.9.3
============================================================================================
**Author:** [Alan Mills]
**Date:** [10 April 2017 22:46]
**Tags:** [macOS Sierra], [Vagrant 1], [Parallels 12], [ElasticSearch 5], [Kibana 5], [Ubuntu 16.04]
**Status**: Draft

I like to use Vagrant to setup development environments for each project that I am working on, that way when I am done I can destroy the VM and I'm left with a set of small text files that enable me to free up nearly all of the used disk space and allowing me to restore the development environment at another time if I need to go back.

Prequsites:
* Have a development machine, I'm using a MacBook Pro running macOS Sierra
* On the development machine have some virtualisation software, I'm using Parallels Desktop Pro 12.1.3
* Have Vagrant 1.x installed, at the time of writing I'm using 1.9.3



Steps:
1. Create a working directory for the project
2. Initalise Vagrant
3. Configure the Vagrantfile for Elasticsearch and Kibana
4. Write the provisioning script
5. vagrant up
6. Check everything is working

Configure the Vagrantfile for Elasticsearch and Kibana
------------------------------------------------------
Elasticsearch and Kibana require significant amounts of system memory to run, during testing I found that I needed a minimum of 4GB of RAM assigned to the Ubuntu 16.04 virtual machine.

Write the provisioning script
-----------------------------
Provisioning needs to:
1. Install Oracle Java 8
2. Configure JAVA_HOME
3. Increase Linux virtual memory
4. Install Elasticsearch
5. Install Kibana
6. Configure Elasticsearch and Kibana
7. Start Elasticsearch
8. Start Kibana

### Install Oracle Java 8
Hazel Virdó at Digital Ocean has written an easy to follow tutorial on [installing Java on Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/how-to-install-java-with-apt-get-on-ubuntu-16-04).  For our purposes we are installing the official Oracle Java 8 using apt-get.

``` bash
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update
sudo apt-get install oracle-java8-installer
JAVA_HOME="/usr/lib/jvm/java-8-oracle"
```

### Increase Linux virtual memory
As outlined in the Elasticsearch reference, [Elasticsearch needs the Linux Virtual memory limits increased](https://www.elastic.co/guide/en/elasticsearch/reference/current/vm-max-map-count.html)
``` bash
sudo sysctl -w vm.max_map_count=262144
```

update /etc/sysctl.conf, add vm.max_map_count = 262144

### Configure Elasticsearch and Kibana
As we want Elasticsearch and Kibana to work from localhost on our development workstation, we need change the default configuration for both.  By default Elasticsearch and Kibana will tie themselves to the localhost loopback adaptor which will not work when called from localhost on your development workstation.

To prevent Kibana showing the error message 'Elasticsearch plugin is red'

#### elasticsearch.yml
``` yml
cluster.name: learn_elasticsearch
network.host: locahost, 10.211.55.171
```

#### kibana.yml
``` yml
server.host: "10.211.55.171"
elasticsearch.url: "http://localhost:9200"
```





``` bash
curl -L -O https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.3.0.tar.gz
tar -xvf elasticsearch-5.3.0.tar.gz
elasticsearch-5.3.0/bin/elasticsearch &
```