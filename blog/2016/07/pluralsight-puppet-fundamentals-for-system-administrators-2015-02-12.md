Review of Pluralsight course: Puppet Fundamentals for System Administrators (2015/02/12)
========================================================================================
**Author:** Alan Mills  
**Date:** [15 July 2016 00:29](/blog/history/2016-07.md)  
**Tags:** [Pluralsight](/blog/categories/pluralsight.md), [Puppet4](blog/categories/puppet4.md), [Vagrant](blog/categories/vagrant.md)
**Status**: Publish

The Pluralsight course [Puppet Fundamentals for System Administrators](https://app.pluralsight.com/library/courses/puppet-system-administrators-fundamentals/table-of-contents), published on 12 February 2015 is given by [Ben Piper](http://benpiper.com).

Overview
--------
Ben Piper gives a reasonable overview of the subjects:
* Introduction to Puppet
* Installing and Configuring the Puppet Master
* Installing and Configuring the Puppet Agent
* Creating Manifests
* Creating and Using Modules
* Creating File Templates
* Configuring Hiera
* Windows Configuration Management
* Version Control

Given that I have completed this course during July 2016 the course material is still relevant and all the course steps work as you follow along.  However, I was concerned with a number of areas:
* [Throw away complexity not followed up](throw-away-complexity-not-followed-up)
* [Unable to automate a re-build of the solution from scratch](unable-to-automate-a-re-build-of-the-solution-from-scratch)
* [Windows chapter](windows-chapter)

### Throw away complexity not followed up
The course makes use of and makes references to:
* Vagrant
* Multiple environments (test and production)
* SELinux
* Git

#### Vagrant
While Vagrant is used to setup the environments (CentOS, Ubuntu & Windows). Ben sets up each environment in its own folder (Vagrant default VM) and does not make use of a Vagrant multi-machine environment despite this being a 4 machine multi-machine environment.  Instead this appears to be a way to leverage the simple distribution of virtal machine (VM) images (Vagrant boxes) and the use of the Vagrantfile to easily setup a private, static network between the environments.  There are other concerns that I have with the use of Vagrant and Puppet in this course, which is covered in the following section: [Unable to automate a re-build of the solution from scratch](unable-to-automate-a-re-build-of-the-solution-from-scratch).

#### Multiple environments (test and production)
While the premise of the course is to setup a lab environment that includes both a test environment and production deployment of a complex application ([MediaWiki](https://www.mediawiki.org/wiki/MediaWiki)), the majority of the course is manually configuring MediaWiki in the production environment.  The main purpose of these two environments seems to be to point out a few minor setup differences between CentOS and Ubuntu.  While this is interesting, for me this misses the opportunity to show a development workflow going from test to production.

#### SELinux
Ben near the beginning of the course discusses SELinux, and tells you to put it in to permissive mode however, Ben never comes back to this either as a tool to outline environment changes that would not be accepted without extra Puppet steps or to put the environment back in to a more secure setup once the Puppet Configuration Management configuration is finalised.  

#### Git
Right at the end of the course there is a very quick introduction of Git as a way to manage a Puppet environment as everything is in configuration files.  It is great that Git is introduced as Puppet uses files for all configuration however, there are much better courses covering Git and is too brief to give participants of this course the skills necessary to make effective and safe use of Git in their own environments.

### Unable to automate a re-build of the solution from scratch
The course takes an easy to follow, mixture of manual and Puppet steps to install MediaWiki and its dependencies however, at the end of the course MediaWiki will not work if you clear away the environments and attempt to reprovision them.  This is down to a number of issues:
* Vagrant usage
* Puppet certificates
* MediaWiki installation

#### Vagrant usage
The key strength of Vagrant is the ability to run a provisioning process as part of the creation of a new virtual machine (VM) which can be used to configure the OS, install software, etc.  Unfortunately in this course, Vagrant is used as an alternative to the VirtualBox UI and misses the opportunity to show how Vagrant supports the use of Puppet for the provisioning process. Therefore, there are large parts of this course where the student is manually setting up Puppet and MediaWiki.

#### Puppet certificates
Puppet certificates are manually managed between the environments which I do not believe is how this is done in a large scale DevOps environment and Ben makes no reference to how this is done in a scaleable way.

#### MediaWiki installation
The MediaWiki installation requires a DB to be setup and during the course this is achieved by running a setup wizard.  This leads of a configuration file being produced that you then add to a custom Puppet Module.  Unfortunately, this file does not allow MediaWiki to setup the DB if it is missing.  There is an install and upgrade script that need to be run depending on the configuration of the service and this is completely skipped over.

### Windows chapter
I appreciate that the Windows chapter is in this course and does provide the basics of how to use Puppet to install simple applications to Windows.  Unfortunately I was left feeling that this chapter was an after thought and did not really tie-in with the rest of the course e.g.
* there are admin tools installed but not configured to access the rest of the course environment
* No users are setup to use the environment and instead everything is done through the default Vagrant account

Final thoughts
--------------
As a complete beginner to Puppet, I feel much more comfortable with what Puppet is and how to use it and therefore I would recommend this course if you are new to Puppet too.

However I would also recommend that while following the course you too put in the additional effort automating the provisioning of the whole environment (including the Puppet Server) by running `vagrant up`.  With the knowledge that I have of Vagrant and Puppet, I was not able to get this to be a one step process; with manual and broken (automated) provisioning steps.  Therefore I will need to seek out more advanced material to fill-in the gaps left by this course.

Recommended courses
-------------------
* [Pluralsight: Introduction to Versioning Environments With Vagrant (2014/10/02)](https://app.pluralsight.com/library/courses/vagrant-versioning-environments), [Review](blog/2016/07/pluralsight-introduction-to-versioning-environments-with-vagrant-2014-10-02.md)
* [Pluralsight: Git Fundamentals (2012/05/23)](https://app.pluralsight.com/library/courses/git-fundamentals)
