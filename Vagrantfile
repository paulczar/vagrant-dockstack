# We'll mount the Chef::Config[:file_cache_path] so it persists between
# Vagrant VMs
host_cache_path = File.expand_path("../.cache", __FILE__)
guest_cache_path = "/tmp/vagrant-cache"

# ensure the cache path exists
FileUtils.mkdir(host_cache_path) unless File.exist?(host_cache_path)

Vagrant.configure("2") do |config|
#  config.berkshelf.enabled = true
#  config.omnibus.chef_version = :latest

  config.vm.provider "virtualbox" do |v|
    v.customize ["modifyvm", :id, "--memory", 1024]
  end

  config.vm.define :dockstack do |dockstack|
    dockstack.vm.box      = 'opscode-ubuntu-12.04'
    dockstack.vm.box_url  = 'https://opscode-vm.s3.amazonaws.com/vagrant/opscode_ubuntu-12.04_provisionerless.box'
    dockstack.vm.hostname = 'docker-ubuntu-1204'
    dockstack.vm.network :private_network, ip: '192.168.50.11'
    dockstack.vm.provider "virtualbox" do |v|
      v.customize ["modifyvm", :id, "--memory", 4096]
    end
    dockstack.vm.provision :shell, :inline => <<-SCRIPT
      apt-get update
      apt-get -y install git socat curl wget
      su vagrant -c "git clone https://github.com/paulczar/devstack.git"
      cd devstack
      useradd docker
      usermod -a -G docker vagrant
      su vagrant -c "touch localrc"
      echo VIRT_DRIVER=docker >> localrc
      echo DATABASE_PASSWORD=dockstack >> localrc
      echo RABBIT_PASSWORD=dockstack >> localrc
      echo SERVICE_TOKEN=dockstack >> localrc
      echo SERVICE_PASSWORD=dockstack >> localrc
      echo ADMIN_PASSWORD=dockstack >> localrc
      su vagrant -c "./tools/docker/install_docker.sh"
      su vagrant -c "./stack.sh"
      #./exercises/docker.sh"
    SCRIPT
  end

  config.vm.synced_folder host_cache_path, guest_cache_path
end