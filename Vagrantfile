Vagrant.configure("2") do |config|
  config.vm.box = "debian/bookworm64"
  config.vm.network "private_network", type: "dhcp"
  config.vm.synced_folder ".", "/vagrant", disabled: true

  # Libvrit settings
  config.vm.provider :libvirt do |domain|
    domain.memory = 4096
    domain.cpus = 2
  end

  # Boot with a GUI in VirtualBox
  config.vm.provider "virtualbox" do |vbox|
    vbox.customize ["modifyvm", :id, "--vram", "128",
                    "--graphicscontroller", "vboxvga",
                    "--accelerate3d", "on"]
    vbox.memory = 4096
    vbox.cpus = 2
    vbox.gui = true
  end

  # Provision with Ansible
  config.vm.provision "ansible" do |ansible|
    ENV['ANSIBLE_ROLES_PATH'] = File.dirname(__FILE__) + "/roles"
    ansible.compatibility_mode = "2.0"
    ansible.playbook = "site-vagrant.yml"
  end
end
