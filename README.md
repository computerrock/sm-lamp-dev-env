
# Vagrant LAMP stack for project development

This is a pre-configured Vagrant LAMP Stack that is meant to be used as a project template. 

Once project is set, it is quickly replicated to any team member's workstation. Code is modified locally whether you're 
working on Linux, Mac OS X, or Windows, but it is running in the same virtual environment created in Virtualbox and configured by Vagrant.

### What is in the stack? ###

* Ubuntu 14.04 LTS (Trusty Tahr)
* Apache 2.4.x
* MySQL server 5.5.x
* PHP 5.6.x
* PHP-FPM
* Xdebug
* Composer
* Symfony installer

## How to use? ##

### Host setup ###

1. Install [Oracle VM Virtualbox](https://www.virtualbox.org/)
2. Install [Vagrant](https://www.vagrantup.com/)
3. Install ansible if on Linux/Mac OS X (eg. `brew install ansible`)

### Project setup ###

1. Download and unpack latest version (master) to your projects location
2. Rename `sm-lamp-dev-env` directory according to new project name
3. Modify IP address in `ansible/inventories/dev` so it doesn't collide with other project VMs (read [Network setup](#network_setup))
4. Configure your VM in `ansible/box/definition.yml` by modifying all entries marked with `SETUP`
5. Edit your `hosts` file so you can access project from browser (eg. `192.168.10.2 sm-test.dev`)
6. Start VM by running `vagrant up` in project root

### Vagrant environment manipulation ###

**Boot** virtual machine by going to environment folder (eg. ie8-win7) and running `vagrant up`.

**Suspending** the virtual machine by calling `vagrant suspend` will save the current running state of the machine and stop it.

**Provisioning** the virtual machine by calling `vagrant provision` will apply changes from **ansible** roles. 

**Access** virtual machine by calling `vagrant ssh`.

**Halting** the virtual machine by calling `vagrant halt` will gracefully shut down the guest operating system and power down the guest machine.

**Destroying** the virtual machine by calling `vagrant destroy` will remove all traces of the guest machine from your system. It'll stop the guest machine, power it down, and remove all of the guest hard disks.

### Network setup<a name="network_setup"></a> ###

Boxes should be set to have NAT IPs in range 192.168.10.2-255. When choosing IP consult with internal Spoiled Milk document of available local IP addresses for your project. 

Consult project box definition file to check which IP it is set to. 
Boxes also have bridged mode networks enabled, so they can also be accessed by other team members. During box startup you may be asked which host network card to use for bridge.
