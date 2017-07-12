Vagrant.configure(2) do |config|
  config.vm.box = "bento/ubuntu-16.04"
  
  config.vm.network "private_network", ip: "192.168.200.10"

  config.vm.provider "virtualbox" do |v|
    v.memory = 1024
    v.cpus = 2
    v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
  end

  config.vm.provision "shell", path: "install.sh"
end
