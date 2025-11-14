# Vagrant Ansible k8s cluster v1.33

```
vagrant up
```
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