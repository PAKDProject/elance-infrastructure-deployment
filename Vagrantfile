# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  config.vm.box = "ubuntu/xenial64"
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
  end

  config.vm.define "app", primary: true do |app|
    app.vm.hostname = "dev.elance.site"
    app.vm.network "private_network", ip: "10.0.0.10"
    app.vm.provision "ansible" do |ansible|
      ansible.inventory_path = "development/inventory"
      ansible.playbook = "bootstrap-server.yml"
      ansible.limit = "all"
      ansible.verbose = "vvv"
      ansible.extra_vars = {
        ansible_user: "vagrant",
        ansible_python_interpreter: "/usr/bin/python3"
      }
    end
  end
end