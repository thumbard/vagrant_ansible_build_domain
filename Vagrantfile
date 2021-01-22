Vagrant.configure("2") do |config|
  config.vm.provider :virtualbox do |v|
    v.memory = 2048
    v.cpus = 2
    v.linked_clone = true
  end

  config.vm.define "dc1" do |node|
    node.vm.box = "gusztavvargadr/windows-server"
    node.vm.hostname = "dc1"
    node.vm.guest = :windows
    node.vm.communicator = :winrm
    node.winrm.username = "vagrant"
    node.winrm.password = "vagrant"

    node.vm.network "private_network", ip: "10.3.10.11"

    node.vm.provider :virtualbox do |vb|
      vb.name = "dc1"
    end

    node.vm.provision "shell", path: "ConfigureRemotingForAnsible.ps1"
    node.vm.provision "shell", inline: "netsh advfirewall set allprofiles state off"
  end

  config.vm.define "dc2" do |node|
    node.vm.box = "gusztavvargadr/windows-server"
    node.vm.hostname = "dc2"
    node.vm.guest = :windows
    node.vm.communicator = :winrm
    node.winrm.username = "vagrant"
    node.winrm.password = "vagrant"

    node.vm.network "private_network", ip: "10.3.10.12"

    node.vm.provider :virtualbox do |vb|
      vb.name = "dc2"
    end

    node.vm.provision "shell", path: "ConfigureRemotingForAnsible.ps1"
    node.vm.provision "shell", inline: "netsh advfirewall set allprofiles state off"
  end

  config.vm.define "server1" do |node|
    node.vm.box = "gusztavvargadr/windows-server"
    node.vm.hostname = "server1"
    node.vm.guest = :windows
    node.vm.communicator = :winrm
    node.winrm.username = "vagrant"
    node.winrm.password = "vagrant"

    node.vm.network "private_network", ip: "10.3.10.13"

    node.vm.provider :virtualbox do |vb|
      vb.name = "server1"
    end

    node.vm.provision "shell", path: "ConfigureRemotingForAnsible.ps1"
    node.vm.provision "shell", inline: "netsh advfirewall set allprofiles state off"
  end

  config.vm.define "ansible" do |node|
    node.vm.box = "hashicorp/bionic64"
    node.vm.hostname = "ansible"

    node.vm.network "private_network", ip: "10.3.10.10"

    node.vm.synced_folder "ansible_build_domain", "/home/vagrant/ansible_build_domain"
    node.vm.provider :virtualbox do |vb|
      vb.name = "ansible"
    end

    node.vm.provision "shell", path: "update_install_ansible.sh"
    node.vm.provision "file", source: "win_search_updates.yml", destination: "/home/vagrant/win_search_updates.yml"
    node.vm.provision "file", source: "win_install_updates.yml", destination: "/home/vagrant/win_install_updates.yml"
    node.vm.provision "shell", inline: "echo '[win]
    10.3.10.11
    10.3.10.12
    10.3.10.13
    [win:vars]
    ansible_user=vagrant
    ansible_password=vagrant
    ansible_connection=winrm
    ansible_winrm_server_cert_validation=ignore' >> /etc/ansible/hosts"
    node.vm.provision "shell", inline: "ansible win -m win_ping"
  end
end