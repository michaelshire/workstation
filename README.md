# Workstation automated configuration

Ansible playbook for workstation configuration.  I built this to have an "ephemeral" Ubuntu image on my workstations that would be consistent.  Feel free to use/share!

This playbook will install/update to the most recent versions available of the following components: git CLI, Openshift and kubectl CLI, Azure CLI, AWS CLI, Terraform CLI, Packer CLI, and podman.
This playbook will install/update to the most recent version available within the default Ubunto APT repository: unzip, ruby, jq, maven, python3
This playbook will install specific versions of the following components: openjdk-17-jre-headless

## Install WSL

For a Windows workstation, first install WSL

```
wsl --install
```

## Test and fix DNS if required
Check DNS of the base configuration.  If DNS isn't working, fix the WSL DNS issue if necessary by creating /etc/wsl.conf with the following contents:

```
[network]
generateResolvConf = false
```

Shutdown the ubuntu image from the windows command line:

```
wsl --shutdown
```

Restart the ubuntu image, then remove the symbolic link:
```
sudo rm /etc/resolve.conf
```

Then create a new resolve.conf file with the following contents:
```
nameserver 1.1.1.1
```

## Install Ansible
Run the following commands to install ansible:

```
sudo apt update
sudo apt upgrade -y
sudo add-apt-repository --yes --update ppa:ansible/ansible -y
sudo apt install ansible -y
```

Note: there is an issue with the playbook getting the gpg key from Microsoft while connected to some VPNs.  Disconnect from the VPN before running this playbook.

Clone this repository:

```
cd ~
git clone https://github.com/michaelshire/workstation.git
```

Modify the variables in the ~/workstation/vars.yaml file.

Then run this playbook with:

```
cd ~/workstation
ansible-playbook playbook.yaml -K
```
