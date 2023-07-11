# -*- mode: ruby -*-
# vi: set ft=ruby :

# Load overrides
require 'yaml'
settings_path = '.vagrant_settings'
settings = {}

if File.exist?(settings_path)
  settings = YAML.load_file(settings_path)
end

VAGRANT_CPUS         = settings["VAGRANT_CPUS"]         || 2
VAGRANT_MEMORY       = settings["VAGRANT_MEMORY"]       || 4096
VAGRANT_NETWORK_NAME = settings["VAGRANT_NETWORK_NAME"] || "vagrant-libvirt"
VAGRANT_NETWORK_ADDR = settings["VAGRANT_NETWORK_ADDR"] || "192.168.121.0/24"

# Vagrant
Vagrant.configure("2") do |config|
  config.vm.box = "debian/bookworm64"
  config.vm.network "private_network", type: "dhcp"
  config.vm.synced_folder ".", "/vagrant", disabled: true

  # Libvirt settings
  config.vm.provider :libvirt do |libvirt|
    libvirt.cpu_mode = "host-model"
    libvirt.cpus = VAGRANT_CPUS
    libvirt.memory = VAGRANT_MEMORY
    libvirt.management_network_name = VAGRANT_NETWORK_NAME
    libvirt.management_network_address = VAGRANT_NETWORK_ADDR
    libvirt.nested = true
  end

  # Boot with a GUI in VirtualBox
  config.vm.provider "virtualbox" do |vbox|
    vbox.customize ["modifyvm", :id, "--vram", "128",
                    "--graphicscontroller", "vboxvga",
                    "--accelerate3d", "on"]
    vbox.memory = VAGRANT_MEMORY
    vbox.cpus = VAGRANT_CPUS
    vbox.gui = true
  end

  # Provision with Ansible
  config.vm.provision "ansible" do |ansible|
    ENV['ANSIBLE_ROLES_PATH'] = File.dirname(__FILE__) + "/roles"
    ansible.compatibility_mode = "2.0"
    ansible.playbook = "site-vagrant.yml"
    ansible.raw_arguments  = ["--diff"]
  end
end
