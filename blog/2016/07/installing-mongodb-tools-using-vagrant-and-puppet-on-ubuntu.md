Installing MongoDB tools using Vagrant and Puppet on Ubuntu 14.04
=================================================================
**Author:** Alan Mills  
**Date:** [18 July 2016 17:48](/blog/history/2016-07.md)  
**Tags:** [Vagrant](blog/categories/vagrant.md), [Puppet4](blog/categories/puppet4.md), [MongoDB](blog/categories/mongodb.md), [Ubuntu 14.04](blog/categories/ubuntu-14-04.md)  
**Status**: Publish

The [Puppet Labs, MongoDB module](https://github.com/puppetlabs/puppetlabs-mongodb) is an excellent way to install and configure the MongoDB Client and Server.  Unfortunately the module does not allow you to install the [MongoDB tools package](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/) which includes:
* [mongoimport](https://docs.mongodb.com/manual/reference/program/mongoimport/#bin.mongoimport)
* [bsondump](https://docs.mongodb.com/manual/reference/program/bsondump/#bin.bsondump)
* [mongodump](https://docs.mongodb.com/manual/reference/program/mongodump/#bin.mongodump)
* [mongoexport](https://docs.mongodb.com/manual/reference/program/mongoexport/#bin.mongoexport)
* [mongofiles](https://docs.mongodb.com/manual/reference/program/mongofiles/#bin.mongofiles)
* [mongooplog](https://docs.mongodb.com/manual/reference/program/mongooplog/#bin.mongooplog)
* [mongoperf](https://docs.mongodb.com/manual/reference/program/mongoperf/#bin.mongoperf)
* [mongorestore](https://docs.mongodb.com/manual/reference/program/mongorestore/#bin.mongorestore)
* [mongostat](https://docs.mongodb.com/manual/reference/program/mongostat/#bin.mongostat)
* [mongotop](https://docs.mongodb.com/manual/reference/program/mongotop/#bin.mongotop)

The approached outline is to use two provisioning scripts:
1. Bash script to add the Mongo repo to APT
2. Puppet script to install MongoDB Server, Client and Tools

The steps are:
* [Setup Vagrantfile](setup-vagrantfile)
* [Define bash provision script](define-bash-provision-script)
* [Define puppet provision script](define-puppet-provision-script)
* [Import puppetlabs/mongodb Puppet module](import-puppetlabs/mongodb-puppet-module)
* [Start the VM](start-the-vm)

Setup Vagrantfile
-----------------
`Vagrantfile` - install MongoDB on Ubuntu 14.04 using Parallels.  Just substitue the `config.vm.box` to another Ubuntu 14.04 box that works for your provider.
``` ruby
Vagrant.configure(2) do |config|
  config.vm.box = "parallels/ubuntu-14.04"
  config.vm.hostname = "standalone"
  config.vm.network "forwarded_port", host: 27017, guest: 27017, id: "mongodb"

  config.vm.provision :shell, path: "bash/provision.sh"

  config.vm.provision :puppet do |puppet|
    puppet.environment_path = "puppet/environments"
    puppet.environment = "production"
    puppet.manifests_path = "puppet/environments/production/manifests"
    puppet.manifest_file = "default.pp"
  end
end
```

Note the Vagrant file makes the assumption that the folder is structured
``` bash
$ tree
.
|-- bash/
|   |-- provision.sh
|-- puppet/
|   |-- environments/
|   |    |-- production/
|   |    |    |-- manifests/
|   |    |    |   |-- default.pp
|   |    |    |-- modules/
|-- Vagrantfile
```

Define bash provision script
----------------------------
`bash/provision.sh` - adds:
* Adds Puppet package - needed for the parallels/ubuntu-14.04 box, you many not need this if you use a different base image.
* Adds MongoDB sources list to APT - steps taken from the MongoDB documentation: [Install MongoDB Community Edition on Ubuntu](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/)
* Update the Packages
* Install the Puppet Agent - needed for the parallels/ubuntu-14.04 box, you many not need this if you use a different base image.

``` bash
#!/usr/bin/env bash

echo "Get Puppet Release"
# For the correct package for the OS you are using, see: https://docs.puppet.com/puppet/latest/reference/puppet_collections.html
wget https://apt.puppetlabs.com/puppetlabs-release-pc1-trusty.deb
dpkg -i puppetlabs-release-pc1-trusty.deb

echo "Add Mongo to the APT list"
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv EA312927
echo "deb http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.2 multiverse" | tee /etc/apt/sources.list.d/mongodb-org-3.2.list

echo "Update Packages"
apt-get -y update

echo "Install Puppet Agent"
apt-get -y install puppet-agent
```

Import puppetlabs/mongodb Puppet module
---------------------------------------
We need to import the [puppetlabs/mongodb Puppet module](https://forge.puppet.com/puppetlabs/mongodb), these steps assume that you have the [Puppet Agent](https://docs.puppet.com/puppet/latest/reference/man/agent.html) installed.
1. Move to the Modules folder `cd puppet/environments/production/modules`
2. Import the puppetlabs/mongodb module `puppet module install puppetlabs-mongodb`

Define puppet provision script
------------------------------
This script makes use of the [puppetlabs/mongodb Puppet module](https://forge.puppet.com/puppetlabs/mongodb) to install the MongoDB Client and Server through the ::mongodb:: classes.  The package 'mongodb-org-tools' step installs the MongoDB tools referenced by APT (added in the provision.sh).
`puppet\environments\production\manifests\default.pp` -
``` ruby
node 'standalone' {
  class { '::mongodb::globals':
    manage_package_repo => true,
  }
  -> class { '::mongodb::client': }
     -> class {'::mongodb::server':
          ensure => 'present',
          port    => 27017,
          bind_ip => ["0.0.0.0"],
          verbose => true,
        }

  package { 'mongodb-org-tools':
    ensure => 'present',
  }
}
```

Start the VM
------------
All that is left is to start the VM
1. Navigate back to the project root
2. Start the VM `vagrant up`
