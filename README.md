## Ansible Playbook for Debian 11 common config and install NGINX/MySQL

## Limitations

1.	Version 0.001-early-alfa
2. 	Needs huge refactoring - rebuild folder structure, roles and plays split/ decoupling
3. 	Move all vars to vars/main.yml, secure sensitive vars with ansible vault
4.  Automate scripts to updating roles and ansible dependencies
5. 	Needs testing 	

## Usage

This playbook will configure fresh (green field) Debian 11 server with common params, install basic software, 

You need to install couple Ansible collections wich are not included in ansible-core. To check whether it is installed, run "ansible-galaxy collection list".

To install it, use: 

ansible-galaxy collection install community.general

ansible-galaxy collection install community.mysql

ansible-galaxy install geerlingguy.mysql

ansible-galaxy install geerlingguy.ntp

To run playbook use:

ansible-playbook playbook.yml -i development.ini


## TO be done:
Directory layout to be developed

    production.ini            # inventory file for production stage
    development.ini           # inventory file for development stage
    test.ini                  # inventory file for test stage
    vpass                     # ansible-vault password file
                              # This file should not be committed into the repository (.gitignore)
                              # therefore file is in ignored by git
    group_vars/
        all/                  # variables under this directory belongs all the groups
            apt.yml           # ansible-apt role variable file for all groups
        webservers/           # here we assign variables to webservers groups
            apt.yml           # Each file will correspond to a role i.e. apt.yml
            nginx.yml         # ""
        mysql/           	  # here we assign variables to mysql groups
            mysql.yml    	  # Each file will correspond to a role i.e. mysql
            mysql-password.yml# Encrypted password file
    plays/
        ansible.cfg           # Ansible.cfg file that holds all ansible config
        webservers.yml        # playbook for webserver tier
        mysql.yml        	  # playbook for mysql tier

    roles/
        roles_requirements.yml# All the information about the roles
        external/             # All the roles that are in git or ansible galaxy
                              # Roles that are in roles_requirements.yml file will be downloaded into this directory
        internal/             # All the roles that are not public

    extension/
        setup/                 # All the setup files for updating roles and ansible dependencies


## Encrypting Passwords and Certificates
Implement [ansible-vault](http://docs.ansible.com/playbooks_vault.html) to encrypt sensitive data. 

There is also [git-crypt](https://github.com/AGWA/git-crypt) that allow you to work with a key or GPG. It's more transparent on daily work than `ansible-vault`
