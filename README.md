# Introduction

This builds a small ansible environment, using Vagrant and Virtualbox.

## Requirements

1. [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
2. [Vagrant](https://www.vagrantup.com/downloads.html)
3. Ample Disk space (mine ended up at ~20GB)
4. CPU / Memory for 4 VMs
    - as configured, 2CPU and 2GB RAM each
5. Internet access
    - for downloading updates and installing Ansible

## Run

1. Clone to your local machine

    ```bash
    git clone https://github.com/thumbard/vagrant_ansible_build_domain.git
    ```

2. Edit resources in Vagrantfile
    - Lines 3 and 4. The more your machine can spare, the faster it will likely go.
3. Open a cmd prompt / terminal in the cloned directory.
4. Run one of the commands below and watch the VMs build
    - PowerShell

    ```powershell
    vagrant up; vagrant reload ansible
    ```

    - bash

    ```bash
    vagrant up && vagrant reload ansible
    ```

5. If everything ends well, you should see a snippet similar to below:

    ```bash
    ==> ansible: Running provisioner: shell...
        ansible: Running: inline script
        ansible: 10.3.10.11 | SUCCESS => {
        ansible:     "changed": false,
        ansible:     "ping": "pong"
        ansible: }
        ansible: 10.3.10.13 | SUCCESS => {
        ansible:     "changed": false,
        ansible:     "ping": "pong"
        ansible: }
        ansible: 10.3.10.12 | SUCCESS => {
        ansible:     "changed": false,
        ansible:     "ping": "pong"
        ansible: }
    ==> ansible: Attempting graceful shutdown of VM...
    ==> ansible: Checking if box 'hashicorp/bionic64' version '1.0.282' is up to date...
    ==> ansible: Clearing any previously set forwarded ports...
    ==> ansible: Fixed port collision for 22 => 2222. Now on port 2208.
    ==> ansible: Clearing any previously set network interfaces...
    ==> ansible: Preparing network interfaces based on configuration...
        ansible: Adapter 1: nat
        ansible: Adapter 2: hostonly
    ==> ansible: Forwarding ports...
        ansible: 22 (guest) => 2208 (host) (adapter 1)
    ==> ansible: Running 'pre-boot' VM customizations...
    ==> ansible: Booting VM...
    ==> ansible: Waiting for machine to boot. This may take a few minutes...
        ansible: SSH address: 127.0.0.1:2208
        ansible: SSH username: vagrant
        ansible: SSH auth method: private key
    ==> ansible: Machine booted and ready!
    ==> ansible: Checking for guest additions in VM...
        ansible: The guest additions on this VM do not match the installed version of
        ansible: VirtualBox! In most cases this is fine, but in rare cases it can
        ansible: prevent things such as shared folders from working properly. If you see
        ansible: shared folder errors, please make sure the guest additions within the
        ansible: virtual machine match the version of VirtualBox you have installed on
        ansible: your host and reload your VM.
        ansible:
        ansible: Guest Additions Version: 5.2.8_KernelUbuntu r120774
        ansible: VirtualBox Version: 6.1
    ==> ansible: Setting hostname...
    ==> ansible: Configuring and enabling network interfaces...
    ==> ansible: Mounting shared folders...
        ansible: /vagrant => C:/Users/kthum/OneDrive/git/vagrant_testing_ansible
        ansible: /home/vagrant/ansible_build_domain => C:/Users/kthum/OneDrive/git/vagrant_testing_ansible/ansible_build_domain
    ==> ansible: Machine already provisioned. Run `vagrant provision` or use the `--provision`
    ==> ansible: flag to force provisioning. Provisioners marked to run always will still run.
    ```

## Build a Windows domain

Once reloaded, you can run the following command to build a domain with two domain controllers and a member server.

```bash
vagrant ssh ansible
cd /home/vagrant/ansible_build_domain/
ansible-playbook -i hosts.yml build_domain.yml
```
