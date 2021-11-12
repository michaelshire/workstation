# workstation
Ansible playbook for workstation configuration

For a Windows workstation, first install WSL

```
wsl --install
```

Then install ansible on the ubuntu image

```
sudo apt update
sudo apt install software-properties-common
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible
```

Then run this playbook
