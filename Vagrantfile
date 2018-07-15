# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.box = "nrel/CentOS-6.5-x86_64"

  config.vm.network "private_network", ip: "10.10.10.50"
  #config.vm.network "forwarded_port", guest: 22, host: 2223
  
  config.vm.hostname = "vagrant.local"
  
  config.vm.provision "ansible_local" do |ansible|
    ansible.compatibility_mode = "2.0"
    ansible.playbook = "site.yml"
    # Uncomment next line if you don't want to install Google Authenticator plugin
    ansible.skip_tags = "totp-authenticator"
  end
  
  config.vm.synced_folder ".", "/vagrant", :mount_options => ["dmode=777", "fmode=666"]
  
end
