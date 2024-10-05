# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'yaml'

def get_config()
    config_yaml = YAML.load_file("infra.yaml")
    return config_yaml
end

def vagrant_main(config)
    Vagrant.configure("2") do |vagrant_config|
        vagrant_config.vm.box_check_update = false

        config["nodes"].each do |host, node_config|
            vagrant_config.vm.define host do |nodeconfig|
                nodeconfig.vm.hostname = host
                nodeconfig.vm.box = node_config["image"]

                nodeconfig.vm.network :private_network, ip: node_config["ip"], virtualbox__intnet: "plumbernet"
                nodeconfig.vm.network :forwarded_port, guest: 22, host: node_config["port"], id: 'ssh'

                nodeconfig.vm.provider :virtualbox do |vb|
                    vb.name = "plumber_" + host
                    vb.gui = false
                    vb.memory = 2000
                    vb.cpus = 1
                end
            end
        end
    end
end

config = get_config()
vagrant_main(config)
