# -*- mode: ruby -*-
# vim: set ft=ruby :
home = ENV['HOME']

MACHINES = {
  :HLbalancer01 => {
    :box_name => "centos/7",
    :net => [
               {ip: '10.51.21.51', adapter: 2, netmask: "255.255.255.0", virtualbox__intnet: "pgsql-net"},
            ]
  },
  :HLbalancer02 => {
    :box_name => "centos/7",
    :net => [
               {ip: '10.51.21.52', adapter: 2, netmask: "255.255.255.0", virtualbox__intnet: "pgsql-net"},
            ]
  },
  :HLpgConpool01 => {
    :box_name => "centos/7",
    :net => [
               {ip: '10.51.21.54', adapter: 2, netmask: "255.255.255.0", virtualbox__intnet: "pgsql-net"},
            ]
  },
  :HLpgConpool02 => {
    :box_name => "centos/7",
    :net => [
               {ip: '10.51.21.55', adapter: 2, netmask: "255.255.255.0", virtualbox__intnet: "pgsql-net"},
            ]
  },
  :HLdcs01 => {
    :box_name => "centos/7",
    :net => [
               {ip: '10.51.21.61', adapter: 2, netmask: "255.255.255.0", virtualbox__intnet: "pgsql-net"},
            ]
  },
  :HLdcs02 => {
    :box_name => "centos/7",
    :net => [
               {ip: '10.51.21.62', adapter: 2, netmask: "255.255.255.0", virtualbox__intnet: "pgsql-net"},
            ]
  },
  :HLdcs03 => {
    :box_name => "centos/7",
    :net => [
               {ip: '10.51.21.63', adapter: 2, netmask: "255.255.255.0", virtualbox__intnet: "pgsql-net"},
            ]
  },
  :HLpg01 => {
    :box_name => "centos/7",
    :net => [
               {ip: '10.51.21.65', adapter: 2, netmask: "255.255.255.0", virtualbox__intnet: "pgsql-net"},
            ]
  },
  :HLpg02 => {
    :box_name => "centos/7",
    :net => [
               {ip: '10.51.21.66', adapter: 2, netmask: "255.255.255.0", virtualbox__intnet: "pgsql-net"},
            ]
  },
  :HLpg03 => {
    :box_name => "centos/7",
    :net => [
               {ip: '10.51.21.67', adapter: 2, netmask: "255.255.255.0", virtualbox__intnet: "pgsql-net"},
            ]
  },
  :HLzabbix01 => {
    :box_name => "centos/7",
    :net => [
               {ip: '10.51.21.57', adapter: 2, netmask: "255.255.255.0", virtualbox__intnet: "pgsql-net"},
            ]
  },
  :HLzabbix02 => {
    :box_name => "centos/7",
    :net => [
               {ip: '10.51.21.58', adapter: 2, netmask: "255.255.255.0", virtualbox__intnet: "pgsql-net"},
            ]
  },
  :HLclient => {
    :box_name => "centos/7",
    :net => [
               {ip: '10.51.21.70', adapter: 2, netmask: "255.255.255.0", virtualbox__intnet: "pgsql-net"},
            ]
  },
}

Vagrant.configure("2") do |config|

  config.vm.define "HLclient" do |c|
    c.vm.hostname = "hl-client"
    c.vm.network "forwarded_port", adapter: 1, guest: 22, host: 2232, id: "ssh", host_ip: '127.0.0.1'
  end
  config.vm.define "HLbalancer01" do |c|
    c.vm.hostname = "hl-balancer01"
    c.vm.network "forwarded_port", adapter: 1, guest: 22, host: 2231, id: "ssh", host_ip: '127.0.0.1'
    c.vm.network "forwarded_port", adapter: 1, guest: 8080, host: 4001, host_ip: '127.0.0.1'
  end
  config.vm.define "HLbalancer02" do |c|
    c.vm.hostname = "hl-balancer02"
    c.vm.network "forwarded_port", adapter: 1, guest: 22, host: 2241, id: "ssh", host_ip: '127.0.0.1'
    c.vm.network "forwarded_port", adapter: 1, guest: 8080, host: 4002, host_ip: '127.0.0.1'
  end
  config.vm.define "HLpgConpool01" do |c|
    c.vm.hostname = "hl-pg-conpool01"
    c.vm.network "forwarded_port", adapter: 1, guest: 22, host: 2233, id: "ssh", host_ip: '127.0.0.1'
  end
  config.vm.define "HLpgConpool02" do |c|
    c.vm.hostname = "hl-pg-conpool02"
    c.vm.network "forwarded_port", adapter: 1, guest: 22, host: 2234, id: "ssh", host_ip: '127.0.0.1'
  end
  config.vm.define "HLdcs01" do |c|
    c.vm.hostname = "hl-dcs01"
    c.vm.network "forwarded_port", adapter: 1, guest: 22, host: 2251, id: "ssh", host_ip: '127.0.0.1'
    c.vm.network "forwarded_port", adapter: 1, guest: 8500, host: 4003, host_ip: '127.0.0.1'
  end
  config.vm.define "HLdcs02" do |c|
    c.vm.hostname = "hl-dcs02"
    c.vm.network "forwarded_port", adapter: 1, guest: 22, host: 2252, id: "ssh", host_ip: '127.0.0.1'
    c.vm.network "forwarded_port", adapter: 1, guest: 8500, host: 4004, host_ip: '127.0.0.1'
  end
  config.vm.define "HLdcs03" do |c|
    c.vm.hostname = "hl-dcs03"
    c.vm.network "forwarded_port", adapter: 1, guest: 22, host: 2253, id: "ssh", host_ip: '127.0.0.1'
    c.vm.network "forwarded_port", adapter: 1, guest: 8500, host: 4005, host_ip: '127.0.0.1'
  end
  config.vm.define "HLpg01" do |c|
    c.vm.hostname = "hl-pg01"
    c.vm.network "forwarded_port", adapter: 1, guest: 22, host: 2521, id: "ssh", host_ip: '127.0.0.1'
  end
  config.vm.define "HLpg02" do |c|
    c.vm.hostname = "hl-pg02"
    c.vm.network "forwarded_port", adapter: 1, guest: 22, host: 2621, id: "ssh", host_ip: '127.0.0.1'
  end
  config.vm.define "HLpg03" do |c|
    c.vm.hostname = "hl-pg03"
    c.vm.network "forwarded_port", adapter: 1, guest: 22, host: 2721, id: "ssh", host_ip: '127.0.0.1'
  end
  config.vm.define "HLzabbix02" do |c|
    c.vm.hostname = "hl-zabbix02"
    c.vm.network "forwarded_port", adapter: 1, guest: 22, host: 2921, id: "ssh", host_ip: '127.0.0.1'
    c.vm.network "forwarded_port", adapter: 1, guest: 8080, host: 4007, host_ip: '127.0.0.1'
  end
  config.vm.define "HLzabbix01" do |c|
    c.vm.hostname = "hl-zabbix01"
    c.vm.network "forwarded_port", adapter: 1, guest: 22, host: 2821, id: "ssh", host_ip: '127.0.0.1'
    c.vm.network "forwarded_port", adapter: 1, guest: 8080, host: 4006, host_ip: '127.0.0.1'
  end

  MACHINES.each do |boxname, boxconfig|

    config.vm.define boxname do |box|

        box.vm.box = boxconfig[:box_name]
        box.vm.box_check_update = false

        boxconfig[:net].each do |ipconf|
          box.vm.network "private_network", ipconf
        end

        if boxconfig.key?(:public)
          box.vm.network "public_network", boxconfig[:public]
        end

        box.vm.provider "virtualbox" do |v|
          v.customize ["modifyvm", :id, "--audio", "none"]
          v.memory = "768"
          v.cpus = "1"
        end

        box.vm.provision "shell", inline: <<-SHELL
                mkdir -p ~root/.ssh
                cp ~vagrant/.ssh/auth* ~root/.ssh
                sed -i 's/^PasswordAuthentication no/#PasswordAuthentication no/g' /etc/ssh/sshd_config
                sed -i 's/^#PasswordAuthentication yes/PasswordAuthentication yes/g' /etc/ssh/sshd_config
                systemctl restart sshd
        SHELL

        box.vm.provision "ansible" do |ansible|
          ansible.verbose = "v"
          ansible.playbook = "provisioning/00_all.yml"
          ansible.inventory_path = "provisioning/hosts_vagrant"
          ansible.extra_vars = "provisioning/variables"
          ansible.become = "true"
          #ansible.tags = "update_hosts"
          #ansible.limit = "web"
          #ansible.config_file = "provisioning/ansible.cfg"
        end

      end
  end
end
