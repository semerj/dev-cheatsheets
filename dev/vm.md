# Vagrant

### Commands
* `vagrant init`: creates `Vagrantfile` in current directory
* `vagrant box add <environment>`: add box to environment (e.g. `hashicorp/precise32`). Find more boxes at [HashiCorp's Atlas box catalog](https://atlas.hashicorp.com/boxes/search)
* `vagrant ssh`: login to vm
* `vagrant status` and `vagrant global-status`: see which vm's are running locally and globally
* `vagrant reload`: reload vm
* `vagrant suspend`: save the current running state of vm and stop it.
* `vagrant halt`: gracefully shut down OS and power down 
* `vagrant destroy`: remove all traces of the vm from system

### Shared files
Files synced in `/vagrant/` folder on vm and in same directory as `Vagrantfile` on host.

### Configuration
After creating environment and optional shell script (e.g. `bootstrap.sh`), open the `Vagrantfile` and modify the following:
```bash
Vagrant.configure("2") do |config|
  config.vm.box = "<environment>"
  config.vm.provision :shell, path: "bootstrap.sh"
end
```

If the guest machine is already running from a previous step, run:
```bash
$ vagrant reload --provision
```

Update apt-get
```bash
$ sudo apt-get update
```

# VirtualBox

start vm
```bash
$ VBoxHeadless --startvm "vm-name" &
# or
$ VBoxManage startvm "vm-name" --type headless
```

create vm
```bash
$ VBoxManage createvm --name "vm-name"
```

power off vm
```bash
$ VBoxManage controlvm "vm-name" poweroff
```

verify extension pack is installed in order to use VirtualBox headless
```bash
$ VBoxManage list extpacks
```
