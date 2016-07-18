Review of Pluralsight course: Introduction to Versioning Environments With Vagrant (2014/10/02)
===============================================================================================
**Author:** Alan Mills  
**Date:** [18 July 2016 15:21](/blog/history/2016-07.md)  
**Tags:** [Pluralsight](/blog/categories/pluralsight.md), [Vagrant](blog/categories/vagrant.md), [Puppet4](blog/categories/puppet4.md)  
**Status**: Publish

The Pluralsight course [Introduction to Versioning Environments With Vagrant](https://app.pluralsight.com/library/courses/vagrant-versioning-environments), published on 2 October 2014 is given by [Wes Higbee](http://www.weshigbee.com).

overview
--------
This is a very good course that covers the Fundamentals of working with Vagrant.  The chapters of the course are:
* Why Vagrant?
* Test Drive Vagrant
* Web Development Environment and Vagrant Fundamentals
* Creating a Hubot Environment
* Hubot in the Cloud
* Windows Guests
* Local Development Databases

Since the course was published a few things have changed which required some working around.  These are:
* [Creating a Hubot Environment](creating-a-hubot-environment)
* [Hubot in the Cloud](hubot-in-the-cloud)
* [Windows Guests](windows-guests)
* [Local Development Databases](local-development-databases)

### Creating a Hubot Environment
The command `hubot --create myhubot` has been deprecated and now [Hubot](https://hubot.github.com) leverages [Yeoman](http://yeoman.io).  Therefore, while the majority of the steps are the same as outlined in the course.  To achieve the same state as the course command, we need to update the Vagrant shell provisioning script (provision.sh) with the code:
``` bash
command -v yo &>/dev/null || {
  npm install -g yo generator-hubot
}
```

Hubot in the Cloud
------------------
Wes is clear that the described deployment of Hubot to [Amazon Web Services](https://aws.amazon.com) as insecure.  Therefore, to deploy to AWS, I took a more secure approach:
* AWS Identity and Access (IAM) account for the project
* Security group restricted to SSH and MyIP address

### AWS Setup
1. Install the [Vagrant AWS plugin](https://github.com/mitchellh/vagrant-aws) `vagrant plugin install vagrant-aws`
2. Add the Vagrant AWS dummy box `vagrant box add aws https://github.com/mitchellh/vagrant-aws/raw/master/dummy.box` **NOTE: I have called this aws and not 'dummy' as shown in the course and AWS github page**
3. Setup the 'default' IAS administrator group and user IAS, follow the steps outlined in the [AWS Identity and Access (IAM) service best practice](http://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html?icmpid=docs_iam_console) documentation.

### Hubot Setup
1. Select the region that you want to use to launch
2. Setup an IAS account called hubot and attached it to the administrator IAS group
3. Setup a new *EC2 > Network & Security > Key Pair* key called *hubot* and save to local `.ssh` folder in the file `hubot.pem`
4. Use the *Default* security as it is restricted to only SSH inbound traffic and from your own IP address (*My IP*).  Copy the Group ID into the *Vagrantfile* `aws.security_groups` configuration item.
5. **NOTE:** make sure the *Vagrantfile* `aws.instance_type` configuration item aligns with a size available to Ubuntu image available to you otherwise, the provisioning will fail.
6. Use the [AWS Marketplace](https://aws.amazon.com/marketplace/) to find the Amazon Machine Image (AMI) that you want to use and update the region settings.  See the table and Vagrantfile snippet below to configure your own needs.


| Amazon EC2 Region Name    | Region         | Endpoint                         | Protocol |
| ------------------------- | -------------- | -------------------------------- | -------- |
| US East (N. Virginia)     | us-east-1      | ec2.us-east-1.amazonaws.com      | HTTPS    |
| US West (N. California)   | us-west-1      | ec2.us-west-1.amazonaws.com      | HTTPS    |
| US West (Oregon)          | us-west-2      | ec2.us-west-2.amazonaws.com      | HTTPS    |
| Asia Pacific (Mumbai)     | ap-south-1     | ec2.ap-south-1.amazonaws.com     | HTTPS    |
| Asia Pacific (Seoul)      | ap-northeast-2 | ec2.ap-northeast-2.amazonaws.com | HTTPS    |
| Asia Pacific (Singapore)  | ap-southeast-1 | ec2.ap-southeast-1.amazonaws.com | HTTPS    |
| Asia Pacific (Sydney)     | ap-southeast-2 | ec2.ap-southeast-2.amazonaws.com | HTTPS    |
| Asia Pacific (Tokyo)      | ap-northeast-1 | ec2.ap-northeast-1.amazonaws.com | HTTPS    |
| EU (Frankfurt)            | eu-central-1   | ec2.eu-central-1.amazonaws.com   | HTTPS    |
| EU (Ireland)              | eu-west-1      | ec2.eu-west-1.amazonaws.com      | HTTPS    |
| South America (São Paulo) | sa-east-1      | ec2.sa-east-1.amazonaws.com      | HTTPS    |


``` ruby
config.vm.provider "aws" do |aws, override|
  aws.region_config "us-east-1", :ami => "ami-7b386c11"
  aws.region_config "us-west-1", :ami => "ami-b085eed0"
  aws.region_config "us-west-2", :ami => "ami-86e0ffe7"
  aws.region_config "ap-southeast-1", :ami => "ami-be5794dd"
  aws.region_config "ap-southeast-2", :ami => "ami-c499c2a7"
  aws.region_config "ap-northeast-1", :ami => "ami-c0586dae"
  aws.region_config "eu-central-1", :ami => "ami-4789952b"
  aws.region_config "eu-west-1", :ami => "ami-5a60c229"
  aws.region_config "sa-east-1", :ami => "ami-a0880fcc"
  aws.region = "eu-west-1"

  aws.tags = {
    'Name' => 'Hubot'
  }

  override.ssh.username = "ubuntu"

  override.ssh.private_key_path = "~/.ssh/hubot.pem"
  aws.keypair_name = "hubot"
  aws.security_groups = "hubot"

  aws.access_key_id = "YOUR ACCESS KEY"
  aws.secret_access_key = "YOUR SECRET KEY"

  aws.instance_type = "t1.micro"
end
```

### Windows Guests
I am using [Parallels](http://www.parallels.com/products/desktop/) and therefore, I created a Windows Server 2012 R2 and Windows 10 box for the Web server and Visual Studio 2015 development workstation respectively.  

The material provided by [GitHub fitzgerald/packer-windows](https://github.com/joefitzgerald/packer-windows) gave an amazing jumpstart.  There were issues that I needed to work through which I plan to write up as subsequent blog posts.

Local Development Databases
---------------------------
In this section of the course MongoDB is installed on Ubuntu including provisioning an dataset to work with.  Following this the provisioning process is modified to create a MongoDB cluster.

There were a number of issues that I ran into with this section of the course:
* Vagrant Puppet 3 vs 4 provisioner assumptions - covered in blog post [Using Vagrant Puppet Apply provisioner with Puppet 4](blog/2016/07/using-vagrant-puppet-apply-provisioner-with-puppet-4.md)
* [MongoDB tools installation] - covered in blog post [Installing MongoDB tools using Vagrant and Puppet on Ubuntu 14.04](blog/2016/07/installing-mongodb-tools-using-vagrant-and-puppet-on-ubuntu-14-04.md)

Final thoughts
--------------
This is an excellent introduction to the extremely useful [Vagrant](https://www.vagrantup.com) tool.

Recommended courses
-------------------
* [Pluralsight: Puppet Fundamentals for System Administrators (2015/02/12)](http://www.pluralsight.com/courses/puppet-system-administrators-fundamentals), [Review](blog/2016/07/pluralsight-puppet-fundamentals-for-system-administrators-2015-02-12.md)
