## Features

This repository could create two virtual machine(CentOS7.1) by Vagrant.
One's name is **manager**, the other is **server**. They are set up by Ansible provisioner.
They have the following features.

* Their network is configured with Host-only networking.
* They connect to the same internal network.
* **manager** manages multiple ansible versions with pyenv and virtualenv.
You can verify multiple ansibles without changing your local environment.
* **manager** could connect to **server** via SSH. Ansible set simple SSH public key authentication settings to them.
Using only `ssh server` command, you can access "server" from **manager**.
**manager** could deploy **server** easilly wihtout confusing setting.
* **server** has no sepcial features without setting of pulic key authentication.
So it is for testing.

#####according to your own decision and at your own responsibility


## Prerequisite

### Vagrant
Download from [Vagrant's site](https://www.vagrantup.com) and follow as instructed.

### Virtual Box
Download from [Oracle's site](https://www.virtualbox.org) and follow as instructed.

### Python 2.7.x
Confirm [Python site](https://www.python.org/downloads/).

### Ansible
Install Ansible by following instruction in [ansible:GitHub](https://github.com/ansible/ansible#ansible)
You can install via pip or yum or brew or etc...

### Obtaining the source code
Clone from this repository in github.


## Tested Environment

* Mac OS X (El Capitan)
* Vagrant 1.8.1
* Virtual Box 5.0.20
* Python 2.7.10
* ansible (2.1.0.0)

## usage

**First: Go to your local repository directory.**

Boot two virtual environments
```bash
$ vagrant up
```

Basic Vagrant commands
```bash
# shutdown virtual machine
$ vagrant halt

# destroy virtual machine
$ vagrant destroy --force
```

Connect **manager** or **server**
```bash
# to manager from host
$ vagrant ssh manager
# you can connect to server from manager via SSH
$ ssh server

# to server from host
$ vagrant ssh server
```
###### FYI: If you wanna connect to them from your host via SSH, you can do it with port forwarding.

Change ansible version in **manager**
```bash
# execute following commands after connect to manager

# lists now installed python versions
$ pyenv versions

# use ansible1 python-environment
$ pyenv global ansible1

# check your ansible version
$ ansible --version

# use ansible2 python-environment
$ pyenv global ansible2
```

If you need more information about Vagrant and Ansible, please read Official docs.
