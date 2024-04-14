Vagrant.configure("2") do |config|

  ###############################################

  # name the VMs
  config.vm.define "rke2-server" do |node|

    # which image to use
    node.vm.box = "opensuse/Leap-15.5.x86_64"

    # sizing of the VMs
    node.vm.provider "libvirt" do |lv|
      lv.random_hostname = false
      lv.memory = 4096
      lv.cpus = 2
    end

    # set the hostname
    node.vm.hostname = "rke2-server"

    node.vm.provision "ansible" do |ansible|
      ansible.compatibility_mode = "2.0"
      ansible.limit = "all"
      ansible.groups = {
        "rke2_servers"  => [ "rke2-server" ],
      }
      ansible.playbook = "ansible/playbook-vagrant.yml"
    end # node.vm.provision

    node.trigger.after :destroy do |trigger|
      trigger.warn = "Removing ansible/rke2-kubeconfig"
      trigger.run = {inline: "rm -vf ansible/rke2-kubeconfig"}
    end # node.trigger.after

  end # config.vm.define servers

end # Vagrant.configure
