# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.

misc_host_ips = {
    "loadbalancer" => "192.168.2.10",
}

controller_host_ips = {
    "controller-0" => "192.168.2.20",
    "controller-1" => "192.168.2.21",
    "controller-2" => "192.168.2.22",
}

worker_host_ips = {
    "worker-0" => "192.168.2.30",
    "worker-1" => "192.168.2.31",
    "worker-2" => "192.168.2.32",
}

Vagrant.configure("2") do |main|
  if Vagrant.has_plugin? "vagrant-vbguest"
   main.vbguest.auto_update = false
  end
  main.vm.box = "debian-kubernetes"
  main.ssh.private_key_path = "./http-serve/id_vagrant"
  main.vm.synced_folder ".", "/home/vagrant/shared"
  main.vm.box_check_update = false

  misc_host_ips.each do |name, ip | 
   main.vm.define name do |loadbalancer|
     loadbalancer.vm.hostname = name
     loadbalancer.vm.network "private_network", ip: ip
     loadbalancer.vm.provider "virtualbox" do |vb|
        vb.cpus = 1
        vb.memory = 512
     end
     loadbalancer.vm.provision "shell", path: "./vagrant-scripts/setup-hosts"
     loadbalancer.vm.provision "shell", path: "./vagrant-scripts/setup-haproxy"
   end
 end

 controller_host_ips.each do |name, ip|
  main.vm.define name do |controller|
     controller.vm.hostname = name
     controller.vm.network "private_network", ip: ip
     controller.vm.provider "virtualbox" do |vb|
       #vb.gui = true
       vb.cpus = 2
       vb.memory = 2048
     end
     controller.vm.provision "shell", path: "./vagrant-scripts/setup-hosts"
     controller.vm.provision "shell", path: "./vagrant-scripts/load-images"
     #controller.vm.provision "shell", path: "./vagrant-scripts/set-controller-firewalld"
     controller.vm.provision "shell", path: "./vagrant-scripts/odds-and-ends"
  end
 end

worker_host_ips.each do |name, ip|
  main.vm.define name do |worker|
     worker.vm.hostname = name
     worker.vm.network "private_network", ip: ip
     worker.vm.provider "virtualbox" do |vb|
        vb.cpus = 1
        vb.memory = 1024
     end
     worker.vm.provision "shell", path: "./vagrant-scripts/setup-hosts"
     worker.vm.provision "shell", path: "./vagrant-scripts/load-images"
     #worker.vm.provision "shell", path: "./vagrant-scripts/set-worker-firewalld"
     worker.vm.provision "shell", path: "./vagrant-scripts/odds-and-ends"
  end
 end
end
