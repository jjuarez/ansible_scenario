# -*- mode: ruby -*-
# vi: set ft=ruby :
require 'yaml'

scenario_config_file = File.expand_path('config/default.yml', __dir__)
scenario_config      = YAML.load_file(scenario_config_file)

Vagrant.configure('2') do |config|
  config.vm.define scenario_config['master']['hostname'] do |n|
    n.vm.box      = scenario_config['master']['box']
    n.vm.hostname = scenario_config['master']['hostname']
    n.vm.network :private_network, ip: scenario_config['master']['ip']

    n.vm.provider :virtualbox do |vb|
    end

    n.vm.provision :file, source: 'provision/files/ssh/id_rsa.ansible', destination: '/home/vagrant/.ssh/id_rsa.ansible'
    n.vm.provision :file, source: 'provision/files/ssh/id_rsa.ansible.pub', destination: '/home/vagrant/.ssh/id_rsa.ansible.pub'
    n.vm.provision :file, source: 'provision/files/ssh/config', destination: '/home/vagrant/.ssh/config'
    n.vm.provision :shell, path: 'provision/shell/install_ansible.sh'
    n.vm.provision :shell, inline: 'cat /home/vagrant/.ssh/id_rsa.ansible.pub >> /home/vagrant/.ssh/authorized_keys'
    n.vm.provision :hosts do |p|
      p.add_host scenario_config['master']['ip'], [scenario_config['master']['hostname']]
    end
  end

  scenario_config['nodes'].each do |node|
    config.vm.define node['hostname'] do |n|
      n.vm.box      = node['box']
      n.vm.hostname = node['hostname']
      n.vm.network :private_network, ip: node['ip']

      n.vm.provider :virtualbox do |vb|
        vb.memory = '1024'
      end

    n.vm.provision :file, source: 'provision/files/ssh/id_rsa.ansible', destination: '/home/vagrant/.ssh/id_rsa.ansible'
    n.vm.provision :file, source: 'provision/files/ssh/id_rsa.ansible.pub', destination: '/home/vagrant/.ssh/id_rsa.ansible.pub'
    n.vm.provision :file, source: 'provision/files/ssh/config', destination: '/home/vagrant/.ssh/config'
    n.vm.provision :shell, inline: 'cat /home/vagrant/.ssh/id_rsa.ansible.pub >> /home/vagrant/.ssh/authorized_keys'
      n.vm.provision :shell, path: 'provision/shell/install_ansible.sh'
      n.vm.provision :hosts do |p|
        p.add_host node['ip'], [node['hostname']]
      end
    end
  end
end
