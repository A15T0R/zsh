# Install OhMyZsh with Ansible Engine :

This is a basic Ansible Engine playbook to install OhMyZsh, configure and enable ZSH on many Linux distributions. This README is made for persons who wants to change the configuration of this playbook.


# Change the connection type :

In this playbook the connection is made locally. To run it on a remote server, you must correctly fill in the 'hosts' section (see below). And this line must be deleted.


# Configure SSH keys :

To connect to a remote server and run this playbook, you must set up the SSH keys.

Generate an SSH key :
```bash
$ ssh-keygen -t rsa -b 4096
```
If an SSH key is already present, it is recommended to make one specific for Ansible. To identify it, you must give it a meaningful name during configuration.


Then copy the key to each server : 
```bash
$ ssh-copy-id 192.168.0.1 -i [key_name]
$ ssh-copy-id 192.168.0.2 -i [key_name]
```

Check correct operation by connecting to each of the machines.


# Variable details :

| Variable name  | Default value | Description |
|----------------|---------------|-------------|
|     'user'     |     [rick]     |Indicates the name (s) of the user for whom the playbook will be run. Also used to have the correct path to write configuration files.|
|    'script'    |'/tmp/ohmyzsh.sh'|Indicates the path that will be used to download the 'ohmyzsh' installation script.|


# Put a list in the 'user' variable :
```yaml
- name: Install and configure ZSH
  connection: local
  vars:
    - user:
        - rick
        - morty
    - script: /tmp/ohmyzsh.sh
  hosts: 127.0.0.1
```

# List several hosts :
```yaml
- name: Install and configure ZSH
  connection: local
  vars:
    - user: rick
    - script: /tmp/ohmyzsh.sh
  hosts: [127.0.0.1,192.168.0.1,192.168.0.2]
```

# Remark :

Here, with this configuration it's not necessary to use "ZSH_THEME_RANDOM_CANDIDATES" because we have only one theme. The better solution here should be integrate the theme in "ZSH_THEME". But i let "ZSH_THEMES_RANDOM_CANDIDATES" for example. If you want add multiple themes, separate them with a double quotes like : `( "theme1" "theme2" )`.

# Execution :

To run this playbook you have to use the command :
```yaml
ansible-playbook playbook.yml --ask-become-pass
```	
The "ask-become-pass" option is mandatory since there are 'become' and 'become_method' entries in the playbook to indicate that it should run the task as sudo.
