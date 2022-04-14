# Workstation automated configuration

Ansible playbook for workstation configuration.  I built this to have an "ephemeral" Ubuntu image on my workstations that would be consistent.  Feel free to use/share!

This playbook will install/update to the most recent versions available of the following components (links to latest for Ubuntu 20.04LTS, but the playbook isn't fixed on that):
- git CLI [latest](https://github.com/git-for-windows/git/releases/latest)
- Openshift and kubectl CLI [latest](https://mirror.openshift.com/pub/openshift-v4/x86_64/clients/ocp/latest/changelog.html)
- Azure CLI [latest](https://github.com/Azure/azure-cli/releases/latest)
- AWS CLI [latest](https://github.com/aws/aws-cli/blob/v2/CHANGELOG.rst)
- Terraform CLI [latest](https://github.com/hashicorp/terraform/releases/latest)
- Packer CLI [latest](https://github.com/hashicorp/packer/releases/latest)
- podman (kubic) [latest](https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/xUbuntu_20.04/)

This playbook will install/update to the most recent version available within the default Ubuntu APT repository: 
- unzip [latest](https://packages.ubuntu.com/focal/unzip)
- ruby [latest](https://packages.ubuntu.com/focal/ruby)
- jq [latest](https://packages.ubuntu.com/focal/jq)
- maven [latest](https://packages.ubuntu.com/focal/maven)
- python3 [latest](https://packages.ubuntu.com/focal/python3)
- openjdk-17-jre-headless [latest](https://packages.ubuntu.com/focal/openjdk-17-jre-headless)

## Install WSL

For a Windows workstation, first install WSL

```
wsl --install
```
This will by default install Ubuntu 20.04LTS.  Also suggest installing [Windows Terminal](https://www.microsoft.com/en-ca/p/windows-terminal/9n0dx20hk701)

## Test and fix DNS if required
Check DNS of the base configuration.  If DNS isn't working, fix the WSL DNS issue if necessary by creating `/etc/wsl.conf` with the following contents:

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

Modify the variables in the `~/workstation/vars.yaml` file.

Then run this playbook with:

```
cd ~/workstation
ansible-playbook playbook.yaml -K
```
