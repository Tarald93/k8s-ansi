IMAGE_NAME = "bento/ubuntu-22.04"
NODES = 2

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
    master.vm.network "private_network", ip: "192.168.56.10"

    master.vm.provision "ansible" do |ansible|
      ansible.playbook = "playbooks/master.yml"
      ansible.inventory_path = "inventory/hosts.ini"
      ansible.compatibility_mode = "2.0"
    end
  end

  # WORKERS
  (1..NODES).each do |i|
    config.vm.define "k8s-node-#{i}" do |node|
      node.vm.box = IMAGE_NAME
      node.vm.hostname = "k8s-node-#{i}"
      node.vm.network "private_network", ip: "192.168.56.#{10 + i}"

      node.vm.provision "ansible" do |ansible|
        ansible.playbook = "playbooks/worker.yml"
        ansible.inventory_path = "inventory/hosts.ini"
        ansible.compatibility_mode = "2.0"
      end
    end
  end
end