# -*- mode: ruby -*-
# vi: set ft=ruby :
# we only need one machine 
# -- 
Vagrant.configure(2) do |config|

  config.vm.box = "ubuntu/xenial64"
  config.vm.hostname = 'storage'
  config.vm.define "storage"
  config.vm.network "private_network", ip: "192.168.50.4"
  config.vm.provider "virtualbox" do |vb|
    #   # Display the VirtualBox GUI when booting the machine
    #   vb.gui = true
    #
    #   # Customize the amount of memory on the VM:
    vb.name = "storage"
    vb.memory = "1524"
    unless File.exist?("disk-#0-0.vdi")
      vb.customize ['storagectl', :id,
                    '--name', 'OSD Controller',
                    '--add', 'sas']
    end
    (0..1).each do |d|
      vb.customize ['createhd',
                    '--filename', "disk-#0-#{d}",
                    '--size', '12000'] unless File.exist?("disk-#0-#{d}.vdi")
      vb.customize ['storageattach', :id,
                    '--storagectl', 'OSD Controller',
                    '--port', 3 + d,
                    '--device', 0,
                    '--type', 'hdd',
                    '--medium', "disk-#0-#{d}.vdi"]
    end
    #vb.customize ['modifyvm', :id, '--memory', "#1524"]
  
    config.vm.provision "ansible" do |p|
      p.playbook = "site.yml"
      p.verbose        = "-vvvv"
      p.inventory_path = "../vagrantinventory"
      p.verbose = true
      p.extra_vars =
          {
          }
    end
  end
end
