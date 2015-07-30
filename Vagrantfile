##################################################
#  Spoiled Milk Dev Environment
##################################################

require 'yaml'

# Read YAML file with box details
box = YAML.load_file('./ansible/box/definition.yml')
vm = box["vagrant_local"]["vm"]

# remove line below only after providing the config.vm.box_url parameter!
Vagrant.require_version ">= 1.5"

# Check if we are on a windows or linux/os-x host, to launch ansible in the supported way
# source: https://stackoverflow.com/questions/2108727/which-in-ruby-checking-if-program-exists-in-path-from-ruby
def which(cmd)
    exts = ENV['PATHEXT'] ? ENV['PATHEXT'].split(';') : ['']
    ENV['PATH'].split(File::PATH_SEPARATOR).each do |path|
        exts.each { |ext|
            exe = File.join(path, "#{cmd}#{ext}")
            return exe if File.executable? exe
        }
    end
    return nil
end

Vagrant.configure("2") do |config|

    config.vm.provider :virtualbox do |v|
        v.name = vm["hostname"]
        v.customize [
            "modifyvm", :id,
            "--name", vm["hostname"],
            "--memory", vm["memory"],
            "--natdnshostresolver1", "on",
            "--cpus", 1,
        ]
    end

    config.vm.box = vm["base_box"]

    config.vm.network :public_network
    config.vm.network :private_network, ip: vm["ip"]
    config.ssh.forward_agent = true

    #############################################################
    # Ansible provisioning (you need to have ansible installed)
    #############################################################
    
    if which('ansible-playbook')
        config.vm.provision "ansible" do |ansible|
            ansible.playbook = "ansible/playbook.yml"
            ansible.inventory_path = "ansible/inventories/dev"
            ansible.limit = 'all'
        end
    else
        config.vm.provision :shell, path: "ansible/windows.sh", args: [vm["hostname"]]
    end

    config.vm.synced_folder ".", "/vagrant", disabled: true
    config.vm.synced_folder "./ansible", "/vagrant/ansible"
    config.vm.synced_folder "./www", "/var/www", :owner => "www-data", :group => "www-data", :mount_options => ["dmode=755,fmode=644"]
end
