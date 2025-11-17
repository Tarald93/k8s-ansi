# Vagrant Ansible k8s cluster:
- k8s version 1.33
- helm v3
- cilium CNI

## Requirements:
- ansible
- VirtualBox
- vagrant

```
vagrant up

vagrant ssh k8s-master

#Check if everything works

k get nodes -o wide

crictl ps
```
enjoy
### If error because of network.
```
echo '* 192.168.50.0/24' >> /etc/vbox/networks.conf
```

If error on vagrant up because of KVM services. try 
```
sudo systemctl stop libvirtd
sudo systemctl stop virtlogd

# IF INTEL 
sudo modprobe -r kvm_intel kvm
```

## Teardown: 
```
vagrant destroy -f
```