# ansible+vagrant-notebook.md

-----

https://docs.ansible.com/ansible/latest/scenario_guides/guide_vagrant.html


## 1.1 Vagrant Setup

The first step once you’ve installed Vagrant is to create a Vagrantfile and customize it to suit your needs. This is covered in detail in the Vagrant documentation, but here is a quick example that includes a section to use the Ansible provisioner to manage a single machine:

# This guide is optimized for Vagrant 1.8 and above.
# Older versions of Vagrant put less info in the inventory they generate.
Vagrant.require_version ">= 1.8.0"

Vagrant.configure(2) do |config|

  config.vm.box = "ubuntu/bionic64"

  config.vm.provision "ansible" do |ansible|
    ansible.verbose = "v"
    ansible.playbook = "playbook.yml"
  end
end

## 1.2 Vagrant Setup


Notice the config.vm.provision section that refers to an Ansible playbook called playbook.yml in the same directory as the Vagrantfile. Vagrant runs the provisioner once the virtual machine has booted and is ready for SSH access.

There are a lot of Ansible options you can configure in your Vagrantfile. Visit the Ansible Provisioner documentation for more information.

$ vagrant up

This will start the VM, and run the provisioning playbook (on the first VM startup).

To re-run a playbook on an existing VM, just run:

$ vagrant provision

This will re-run the playbook against the existing VM.

Note that having the ansible.verbose option enabled will instruct Vagrant to show the full ansible-playbook command used behind the scene, as illustrated by this example:

$ PYTHONUNBUFFERED=1 ANSIBLE_FORCE_COLOR=true ANSIBLE_HOST_KEY_CHECKING=false ANSIBLE_SSH_ARGS='-o UserKnownHostsFile=/dev/null -o IdentitiesOnly=yes -o ControlMaster=auto -o ControlPersist=60s' ansible-playbook --connection=ssh --timeout=30 --limit="default" --inventory-file=/home/someone/coding-in-a-project/.vagrant/provisioners/ansible/inventory -v playbook.yml

This information can be quite useful to debug integration issues and can also be used to manually execute Ansible from a shell, as explained in the next section.
Running Ansible Manually

Sometimes you may want to run Ansible manually against the machines. This is faster than kicking vagrant provision and pretty easy to do.

With our Vagrantfile example, Vagrant automatically creates an Ansible inventory file in .vagrant/provisioners/ansible/inventory/vagrant_ansible_inventory. This inventory is configured according to the SSH tunnel that Vagrant automatically creates. A typical automatically-created inventory file for a single machine environment may look something like this:

# Generated by Vagrant

default ansible_host=127.0.0.1 ansible_port=2222 ansible_user='vagrant' ansible_ssh_private_key_file='/home/someone/coding-in-a-project/.vagrant/machines/default/virtualbox/private_key'

If you want to run Ansible manually, you will want to make sure to pass ansible or ansible-playbook commands the correct arguments, at least for the inventory.

$ ansible-playbook -i .vagrant/provisioners/ansible/inventory/vagrant_ansible_inventory playbook.yml
