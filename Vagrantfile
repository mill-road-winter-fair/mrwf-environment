require 'yaml'

settings = YAML.load_file 'config/vagrant.yml'

Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-16.04"
  # vm resources
  config.vm.provider "virtualbox" do |vb|
    vb.customize ["modifyvm", :id, "--cpuexecutioncap", "50", "--cpus", "1", "--cableconnected1", "on"]
    vb.memory = 1024
  end
  # network
  config.vm.network "private_network", ip: settings['ip']
  # forward ssh agent
  config.ssh.forward_agent = true
  # shared dirs
  config.vm.synced_folder '.', '/vagrant', disabled: true
  config.vm.synced_folder "./ansible", "/ansible", create: true, owner: "vagrant", group: "vagrant", mount_options: ["dmode=777,fmode=777"]
  config.vm.synced_folder settings['theme_dir'], "/mrwf/themes", create: true, owner: "vagrant", group: "www-data", mount_options: ["dmode=777,fmode=777"]
  config.vm.synced_folder settings['core_dir'], "/mrwf/core", create: true, owner: "vagrant", group: "www-data", mount_options: ["dmode=777,fmode=777"]
  config.vm.synced_folder settings['module_dir'], "/mrwf/modules", create: true, owner: "vagrant", group: "www-data", mount_options: ["dmode=777,fmode=777"]
  # ansible provisioner
  config.vm.provision "ansible_local" do |ansible|
    ansible.install  = true
    ansible.install_mode = :default
    ansible.playbook = "/ansible/init.yml"
    ansible.provisioning_path = "/ansible"
    ansible.inventory_path = "/ansible/hosts"
    ansible.galaxy_role_file = "/ansible/requirements.yml"
  end

end
