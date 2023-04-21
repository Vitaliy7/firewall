# -*- mode: ruby -*-
# vim: set ft=ruby :

MACHINES = {
 :inetRouter => {
        :box_name => "centos/7",
        :vm_name => "inetRouter",
        :net => [
                   {ip: '192.168.150.1', adapter: 2, netmask: "255.255.255.0", virtualbox__intnet: "router-net"} # intnet это vlan
               ]
  },

 :inetRouter2 => {
        :box_name => "centos/7",
        :vm_name => "inetRouter2",
        :net => [
                   {ip: '192.168.150.2', adapter: 2, netmask: "255.255.255.0", virtualbox__intnet: "router-net"}
               ]
  },

  :centralRouter => {
        :box_name => "centos/7",
        :vm_name => "centralRouter",
        :net => [
                   {ip: '192.168.150.3', adapter: 2, netmask: "255.255.255.0", virtualbox__intnet: "router-net"},
                   {ip: '192.168.0.1', adapter: 3, netmask: "255.255.255.0", virtualbox__intnet: "central-net"}
                ]
  },
  
  :centralServer => {
        :box_name => "centos/7",
        :vm_name => "centralServer",
        :net => [
                   {ip: '192.168.0.2', adapter: 2, netmask: "255.255.255.0", virtualbox__intnet: "central-net"}
                ]
  }

}

Vagrant.configure("2") do |config|

  MACHINES.each do |boxname, boxconfig|

       config.vm.define boxname do |box|
       box.vm.box = boxconfig[:box_name]
       box.vm.host_name = boxconfig[:vm_name]
       
              if boxconfig[:vm_name] == "centralServer"
                 box.vm.provision "ansible" do |ansible|
                  ansible.playbook = "ansible/provision.yml"
                  ansible.host_key_checking = "false"
                  ansible.limit = "all"
               end
                         
          end
               
       boxconfig[:net].each do |ipconf|
       box.vm.network "private_network", ipconf
       
      end
               
          if boxconfig.key?(:public)
             box.vm.network "public_network", boxconfig[:public]
             
     end
               
          box.vm.provision "shell", inline: <<-SHELL
             mkdir -p ~root/.ssh
              cp ~vagrant/.ssh/auth* ~root/.ssh
             SHELL
     end      
   end
end
