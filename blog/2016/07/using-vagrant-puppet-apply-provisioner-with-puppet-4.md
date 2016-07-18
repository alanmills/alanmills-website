Using Vagrant Puppet Apply provisioner with Puppet 4
====================================================
**Author:** Alan Mills  
**Date:** [18 July 2016 17:16](/blog/history/2016-07.md)  
**Tags:** [Vagrant](blog/categories/vagrant.md), [Puppet4](blog/categories/puppet4.md)  
**Status**: Publish

When using the [Vagrant Puppet Apply provisioner](https://www.vagrantup.com/docs/provisioning/puppet_apply.html) with [Puppet environments](https://docs.puppet.com/puppet/latest/reference/environments.html) and Puppet 4, as stated in the Vagrant documentation <cite>If manifests_path and manifest_file are specified without environments, the old non-environment mode will be used (which will fail on Puppet 4+)</cite>.

The documentation is not clear on what settings you need to provide for each Vagrant Puppet option.  Given a project folder layout as

``` bash
$ tree
.
|-- puppet\
|   |-- environments\
|   |    |-- production\
|   |    |    |-- manifests\
|   |    |    |   |-- default.pp
|   |    |    |-- modules\
|-- Vagrantfile
```

The Vagrant configuration required is:
``` ruby
config.vm.provision :puppet do |puppet|
  puppet.environment_path = "puppet/environments"
  puppet.environment = "production"
  puppet.manifests_path = "puppet/environments/production/manifests"
  puppet.manifest_file = "default.pp"
end
```

There are a number of things to note:
* Both `puppet.environment_path` and `puppet.manifests_path` need to be specified relative to the project root
* Both `puppet.environment` and `puppet.manifest_file` are both relative to their respective `path` properties
* `puppet.module_path` is not required in this instance because any installed modules for the Production environment are stored in the default location (puppet\environments\production\modules).
