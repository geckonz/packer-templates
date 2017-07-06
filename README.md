# Bryce's Packer templates

## Boxes description

The current and only template was created for Debian 8.8.0 VM boxes.
The boxes are "vanilla" with a minimal setup, 512MB RAM, 1 CPU. These settings can be easily changed in vagrant.

## Prerequisites

* Packer (>= 0.5.0)(http://www.packer.io/downloads.html)
* Vagrant (>= 1.2.4)(http://downloads.vagrantup.com/)
* Vmware or Virtualbox
* Vagrant vmware plugin if you're using vmware (http://www.vagrantup.com/vmware)

## Configure the vagrant box

Edit the debian-8.json (or the other one you prefer) and check the variables at the beginning of the file.

*Note*:

The debian iso file name contains the version number and, as soon as a new release will be out and the 770 will be removed from the debian servers, the debian-770-wheezy.json file will be outdated and you'll get the "ISO download failed" error after running the build command.
To fix the issue go on http://cdimage.debian.org/debian-cd/current/amd64/iso-cd/, check which is the latest net-inst version and copy its checksum from the MD5SUMS file. Then edit the .json file and update these variables at the beginning of the .json file:
* "iso_url": update the link to the iso file
* "iso_checksum": insert the new MD5 checksum
* "vm_name": update the version

## Build vagrant box

```bash
$ packer build debian-8.json
```

or optionnaly, select only one provider, for example ```VirtualBox```:

```bash
$ packer build -only virtualbox-iso debian-8.json
```

### Install your new box

```bash
$ vagrant box add debian-8 ./packer_vmware-iso_vmware.box
```

or

```bash
$ vagrant box add debian-8 ./packer_virtualbox-iso_virtualbox.box
```

The VM image has been imported to vagrant, it's now available on your system.

## Vagrant

### Getting Started

To use this image with Vagrant, create a vagrant file (```vagrant init```), and use the newly created box:

```ruby
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  # All Vagrant configuration is done here. The most common configuration
  # options are documented and commented below. For a complete reference,
  # please see the online documentation at vagrantup.com.

  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box = "debian-8"

  # Make ssh login secure
  # config.ssh.private_key_path = '~/.ssh/id_rsa'
  #
  # [...]
end
```

And initialize the vm:

```bash
$ vagrant up --provider=virtualbox
```

The ```--provider``` option is only needed if another vagrant provider is available, like vmware_fusion.

### Ignore vagrant boxes in git

```bash
$ echo ".vagrant" >> ~/.gitignore
```
