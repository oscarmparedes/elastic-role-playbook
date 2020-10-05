BOX_IMAGE = "centos/7"
NODE_COUNT = 2
MEMORY = 1024

Vagrant.configure("2") do |config|
  config.vm.define "master" do |subconfig|
    subconfig.vm.box = BOX_IMAGE
    subconfig.vm.hostname = "master"
    subconfig.vm.network :private_network, ip: "192.168.50.20"
	subconfig.hostmanager.enabled = true
    subconfig.hostmanager.manage_host = true
    subconfig.hostmanager.manage_guest = true
    subconfig.hostmanager.ignore_private_ip = false
    subconfig.hostmanager.include_offline = true
    subconfig.vm.provider :virtualbox do |vb|
        vb.memory = 2048
        vb.cpus = 4
    end
  end
  
  (1..NODE_COUNT).each do |i|
    config.vm.define "node#{i}" do |subconfig|
      subconfig.vm.box = BOX_IMAGE
      subconfig.vm.hostname = "node#{i}"
      subconfig.vm.network :private_network, ip: "192.168.50.#{i + 10}"
	  subconfig.hostmanager.enabled = true
      subconfig.hostmanager.manage_host = true
      subconfig.hostmanager.manage_guest = true
      subconfig.hostmanager.ignore_private_ip = false
      subconfig.hostmanager.include_offline = true
      subconfig.vm.provider :virtualbox do |vb|
        vb.memory = MEMORY
        vb.cpus = 4
    end
    end
  end

  # Install ansible and python3 in all vms  
#config.vm.provision "shell", inline: <<-SHELL
#    yum install python3 ansible -y 
#  SHELL
#config.vm.provision "ansible" do |ansible|
#    ansible.playbook = "playbook.yaml"
#    ansible.inventory_path = "inventory"
#end
end
