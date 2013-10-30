# Vagrant Dockstack

Docker + Devstack = Dockstack

Uses the Vagrant shell provisioner to set up a Ubuntu 12.04 box running devstack with the docker driver.

### Requirements:

Here's how you can quickly get testing or developing against the cookbook thanks to [Vagrant](http://vagrantup.com/) and [Berkshelf](http://berkshelf.com/).

* Vagrant ( 1.3.5 ) - http://downloads.vagrantup.com/tags/v1.3.5
* VirtualBox ( 4.3 ) - https://www.virtualbox.org/wiki/Downloads

### Running:

    git clone https://github.com/paulczar/vagrant-dockstack.git
    cd vagrant-dockstack
    vagrant up 

* You can then SSH into the running VM using the `vagrant ssh` command.
* Occasionally the new version of vagrant doesn't run the provision scripts,  if that happens simply run `vagrant provision` to kick them off.

### Did it work?

	vagrant ssh
	. ~/devstack/openrc
	nova boot --image "docker-busybox:latest" --flavor m1.tiny test
	nova list
	docker ps

### Cleanup ?

    vagrant destroy
