# rke2_singlenode_with_vagrant_libvirt

Vagrant setup for building a RKE2 singlenode cluster using the vagrant libvirt provider 

This setup creates a singlenode RKE2 "cluster".

Default OS is openSUSE Leap 15.3, but that can be changed in the Vagrantfile. Same holds true for the sizing of the machines.

## Vagrant

1. You need vagrant obviously. And libvirt. And ansible.
2. Fetch the box, per default this is `opensuse/Leap-15.3.x86_64`, using `vagrant box add opensuse/Leap-15.3.x86_64`.
3. Make sure the machine, where you want to run this on, has enough RAM to allow running the VM. If not, adjust the size in the `Vagrantfile`.
4. Run `vagrant up`
5. Run `kubectl --kubeconfig ansible/rke2-kubeconfig get nodes` and you should see your single node.
6. Party!

## Adjusting size in the Vagrantfile

In case you want or need to change the VM sizing, change the following lines in the `Vagrantfile`:
```
        lv.memory = 4096
        lv.cpus = 2
```

## Cleaning up

You can remove your vagrant VMs using `vagrant destroy`, but you need to manually remove the following files before starting again:
```bash
ansible/rke2-kubeconfig
```
