# -*- mode: ruby -*-
# vi: set ft=ruby :

$default_if=`ip route | grep -E "^default" | awk '{printf "%s", $5; exit 0}'`

Vagrant.configure("2") do |config|

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "centos/7"

  config.vm.define "consul-node-1" do |r1|
    r1.vm.network "public_network", ip: "192.168.1.201", bridge: "enp8s0"
  end

  config.vm.define "consul-node-2" do |r2|
    r2.vm.network "public_network", ip: "192.168.1.202", bridge: "enp8s0"
  end

  config.vm.define "consul-node-3" do |r3|
    r3.vm.network "public_network", ip: "192.168.1.203", bridge: "enp8s0"
  end

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  config.vm.provider "virtualbox" do |vb|
     # Customize the amount of memory on the VM:
     vb.memory = "1800"
  end

  # Enable provisioning with a shell script.
  config.vm.provision "shell", inline: <<-SHELL
     cat /etc/hosts | grep mihail.ideapad || echo -e "192.168.1.101		mihail.ideapad\n" >> /etc/hosts
     yum -y update > /dev/null 2>&1
     yum install -y yum-utils unzip lvm2 > /dev/null
     [[ -f /tmp/consul.zip ]] || curl -o /tmp/consul.zip https://releases.hashicorp.com/consul/1.7.0/consul_1.7.0_linux_amd64.zip
  SHELL

  # Enable provisioning with a shell script.
  config.vm.provision "ansible" do |ansible|
    ansible.compatibility_mode = "2.0"
    ansible.playbook = "consul-cluster-deploy.yml"
  end

end
