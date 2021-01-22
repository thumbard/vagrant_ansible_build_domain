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
    git clone
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
    ansible: 10.3.10.12 | SUCCESS => {
    ansible:     "changed": false,
    ansible:     "ping": "pong"
    ansible: }
    ansible: 10.3.10.13 | SUCCESS => {
    ansible:     "changed": false,
    ansible:     "ping": "pong"
    ansible: }
    ansible: 10.3.10.11 | SUCCESS => {
    ansible:     "changed": false,
    ansible:     "ping": "pong"
    ansible: }
```

### Build a Windows domain

Once reloaded, you can run the following command to build a domain with two domain controllers and a member server.

```bash
vagrant ssh ansible
cd /home/vagrant/ansible_build_domain/
ansible-playbook -i hosts.yml build_domain.yml
```
