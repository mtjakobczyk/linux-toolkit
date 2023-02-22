## Virtualization 


### Using Vagrant to automate a local VirtualBox-based lab creation 
- **VirtualBox** (Oracle) is a Type 2 Hypervisor that let us host guest VMs. It can be managed with GUI and `VBoxManage` CLI utility.
- **Vagrant** (Hashicorp) allows to control VirtualBox in an automated way using declarative configurations and pre-existing compute images (called *Vagrant Boxes*). Various companies and organizations publish their official images to the [Vagrant Cloud (Box Registry)](https://app.vagrantup.com/boxes/search). Custom Vagrant Boxes can be built as well.

Steps:
1. Install **VirtualBox**
1. Install **Vagrant**
1. Find a proper VirtualBox-compliant Vagrant Box:   
    https://app.vagrantup.com/boxes/search?provider=virtualbox  
    For example, the official Ubuntu 20.04 LTS (Focal Fossa) is available under the Box name `ubuntu/focal64` 
1. Create some new working directory.
    ```
    mkdir local-lab
    cd local-lab
    ```
    You will store there a so called `Vagrantfile` (more [here](https://www.vagrantup.com/docs/vagrantfile))
1. Create a new `Vagrantfile`.
    ```
    vi Vagrantfile
    ```
    The file will describe the desired configuration of the VMs. Example below:
    ```ruby
    Vagrant.configure("2") do |config|
        config.vm.box = "ubuntu/focal64" # Ubuntu 20.04 LTS Focal Fossa
        config.vm.box_check_update = true
        config.vm.provider "virtualbox" do |vb|
            vb.gui = false
        end

        # Development VM 1
        config.vm.define "dev1" do |c1|
            c1.vm.hostname = "dev-1"
            c1.vm.network "private_network", ip: "192.168.57.11", name: "vboxnet1"
            c1.vm.provider "virtualbox" do |vb|
                vb.name = "my-dev-1"
                vb.cpus = 2
                vb.memory = "4096"
                vb.customize ['modifyvm', :id, '--graphicscontroller', 'vmsvga']
            end
        end
    end
    ```
1. Move to the new working directory. You will store there the so called `Vagrantfile`
    ```
    vagrant up
    ```
1. You can then watch the new VM appearing in the VirtualBox GUI.  
   Alternatively, you can list it with `VBoxManage` utility (more [here](https://www.oracle.com/technical-resources/articles/it-infrastructure/admin-manage-vbox-cli.html)):
   ```
   VBoxManage list vms
   ```
1. To connect to the VM, you can use Vagrant as well:
    ```
    vagrant ssh dev1
    ```
1. The VM can be destroyed using `vagrant destroy` command
