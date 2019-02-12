# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # vagrant up tower --provider virtualbox
  config.vm.define "tower" do |tower|
      config.vm.hostname = "tower.local"
      tower.vm.box = "ansible/tower"
      tower.vm.network :private_network, ip: "192.168.56.2"
      tower.vm.provider :virtualbox do |v|
         v.gui = false
         v.memory = 2048
         v.cpus = 2
      end
  end

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "centos/7"

  ################ VBox #############################
  # let's use vbox
  # TODO: let's refactor and build a function for god's sake
  config.vm.define "jenkins_box" do |jenkins|
      config.vm.hostname = "jenkins.local"
      jenkins.vm.network :private_network, ip: "192.168.56.3"
      jenkins.vm.provider :virtualbox do |v|
         v.gui = false
         v.memory = 2048
      end
  end
    
  config.vm.define "sonar_box" do |sonar|
    config.vm.hostname = "sonar.local"
    sonar.vm.network :private_network, ip: "192.168.56.4"
    sonar.vm.provider :virtualbox do |v|
        v.gui = false
        v.memory = 3000
    end
 end

  config.vm.define "nexus_box", primary: true do |nexus|
    config.vm.hostname = "nexus.local"
    nexus.vm.network :private_network, ip: "192.168.56.5"
    nexus.vm.provider :virtualbox do |v|
        v.gui = false
        v.memory = 1024   
    end
  end

  config.vm.define "app_box", primary: true do |app|
      config.vm.hostname = "app.local"
      app.vm.network :private_network, ip: "192.168.56.11"
      app.vm.provider :virtualbox do |v|
        v.gui = false
        v.memory = 512  
      end
  end

  config.vm.define "app2_box", primary: true do |app2|
      config.vm.hostname = "app2.local"
      app2.vm.network :private_network, ip: "192.168.56.7"
      app2.vm.provider :virtualbox do |v|
         v.gui = false
         v.memory = 512
      end
  end
 
  ################ LIB VIRT #########################

  config.vm.define "jenkins" do |jenkins|
      config.vm.hostname = "jenkins.local"
      jenkins.vm.network :private_network, ip: "172.16.10.100"
      jenkins.vm.provider :libvirt do |lb|
          lb.memory = 2048
      end
  end

  config.vm.define "sonar" do |sonar|
      config.vm.hostname = "sonar.local"
      sonar.vm.network :private_network, ip: "172.16.10.110"
      sonar.vm.provider :libvirt do |lb|
          lb.memory = 2048
      end
  end

  config.vm.define "nexus", primary: true do |nexus|
      config.vm.hostname = "nexus.local"
      nexus.vm.network :private_network, ip: "172.16.10.120"
      nexus.vm.provider :libvirt do |lb|
        lb.memory = 1024
      end
  end

  config.vm.define "app", primary: true do |app|
    config.vm.hostname = "app.local"
    app.vm.network :private_network, ip: "172.16.10.130"
    app.vm.provider :libvirt do |lb|
        lb.memory = 512
    end
  end

  config.vm.define "app2", primary: true do |app2|
    config.vm.hostname = "app2.local"
    app2.vm.network :private_network, ip: "172.16.10.140"
    app2.vm.provider :libvirt do |lb|
        lb.memory = 512
    end
  end

  config.vm.provider "libvirt" do |libvirt|
      libvirt.storage_pool_name = "ext_storage"
  end

  config.vm.provision "ansible" do |ansible|
      ansible.playbook = "ansible/alm.yml"
      ansible.groups = {
          "jenkins_server" => ["jenkins", "jenkins_box"],
          "sonar_server" => ["sonar", "sonar_box"],
          "nexus_server" => ["nexus", "nexus_box"],
          "app_server" => ["app", "app2", "app_box", "app2_box"],
      }
  end

  if Vagrant.has_plugin?("vagrant-hostmanager")
      config.hostmanager.enabled = true
      config.hostmanager.manage_host = true
      config.hostmanager.manage_guest = true
  end
end
