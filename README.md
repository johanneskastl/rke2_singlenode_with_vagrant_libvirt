# rke2_singlenode_with_vagrant_libvirt

Vagrant setup for building a RKE2 singlenode cluster using the vagrant-libvirt provider.

This setup creates a singlenode RKE2 "cluster".

Default OS is openSUSE Leap 15.5, but that can be changed in the Vagrantfile. Same holds true for the sizing of the machines.

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

## Adjusting the options for the rke2 installation

rke2 has a gazillion of options that you can set. In this setup, you need to provide them as Ansible variables in the playbook.

The playbook `playbook-install_rke2-disable_ingress-nginx.yml` shows an example:
```
[...]
  roles:
    - role: 'install_rke2'
      vars:
        disable_something: 'rke2-ingress-nginx'
[...]
```

You can either adjust the playbook `ansible/playbook-vagrant.yml` that Vagrant runs during node provisioning, or create a new playbook and called it like this:
```
ansible-playbook -i .vagrant/provisioners/ansible/inventory/vagrant_ansible_inventory ansible/playbook-my_new_playbook.yml
```

The list of available options is shown in both the [RKE2 documentation](https://docs.rke2.io/install/install_options/server_config/) as well as in the [Ansible role](https://github.com/johanneskastl/ansible-role-install_rke2)

## Cleaning up

You can remove your vagrant VMs using `vagrant destroy`, but you need to manually remove the following files before starting again:
```bash
ansible/rke2-kubeconfig
```
