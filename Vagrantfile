# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.user.defaults = {
    "boxes" => {
      "default" => {
        "ipaddress"                    => "10.10.10.50",
        "install_google_authenticator" => "false",
        "hostname"                     => "vagrant.local",
        "ssh_port"                     => "2222",
        "gui"                          => "false"
      }
    }
  }

  config.vm.box = "nrel/CentOS-6.5-x86_64"

  require 'json'

  # Loop through the boxes and setup
  config.user.boxes.each do |box, details|

    config.vm.define "#{box}" do |layer|

      layer.vm.network :forwarded_port, guest: 22, host: details.ssh_port, id: "ssh_#{box}", auto_correct: true
      layer.vm.network :private_network, ip: details.ipaddress
      layer.vm.provider :virtualbox do |vb|
      
        if details.gui == "false"
          vb.gui = false
        else
          vb.gui = true
        end

        layer.vm.hostname = details.hostname
        layer.vm.synced_folder ".", "/vagrant", :mount_options => ["dmode=777", "fmode=666"]

        layer.vm.provision "ansible_local" do |ansible|
           ansible.compatibility_mode = "2.0"
           ansible.become = true
           ansible.playbook = "site.yml"
           if details.install_google_authenticator != "true"
            ansible.skip_tags = "totp-authenticator"
           end
           ansible.extra_vars = {
             hostname: details.hostname
           }
         end
      end
     end
  end
end
