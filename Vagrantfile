require 'yaml'

vars = YAML.load_file("inventory/group_vars/all.yml")

BASE_NODE_IP = vars["base_node_ip"]
MASTER_IP = "#{BASE_NODE_IP}.#{vars["master_ip_last"]}"
WORKER_IPS = vars["worker_ip_last"].map { |n| "#{BASE_NODE_IP}.#{n}" }

IMAGE_NAME = "bento/ubuntu-22.04"
NODES      = WORKER_IPS.length

Vagrant.configure("2") do |config|
  config.ssh.insert_key = false

  config.vm.provider "virtualbox" do |v|
    v.memory = 2048
    v.cpus = 2
  end

  # MASTER
  config.vm.define "k8s-master" do |master|
    master.vm.box = IMAGE_NAME
    master.vm.hostname = "k8s-master"
    master.vm.network "private_network", ip: MASTER_IP

    master.vm.provision "ansible" do |ansible|
      ansible.playbook = "playbooks/master.yml"
      ansible.inventory_path = "inventory/hosts.yml"
      ansible.compatibility_mode = "2.0"
    end
  end

  # WORKERS
  (1..NODES).each do |i|
    config.vm.define "k8s-node-#{i}" do |node|
      node.vm.box = IMAGE_NAME
      node.vm.hostname = "k8s-node-#{i}"
      
      worker_ip = WORKER_IPS[i - 1]
      node.vm.network "private_network", ip: worker_ip

      node.vm.provision "ansible" do |ansible|
        ansible.playbook = "playbooks/worker.yml"
        ansible.inventory_path = "inventory/hosts.yml"
        ansible.compatibility_mode = "2.0"
      end
    end
  end
end