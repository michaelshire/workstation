# workstation
Ansible playbook for workstation configuration

## Install WSL and fix DNS
For a Windows workstation, first install WSL

```
wsl --install
```

Fix the WSL DNS issue if necessary by creating /etc/wsl.conf with the following contents:

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
sudo apt install software-properties-common
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible
```

Then run this playbook
