# Ansible Starter

This is a starter setup for learning [Ansible](https://www.ansible.com/) on your workstation or laptop. It's intended as a companion to a _Getting started with Ansible_ tutorial. There are many articles and tutorials for getting started, this doesn't try to reinvent that wheel. 

Why this? Ansible has a number of file formats and organizations. The examples can be confusing until the variations are learned. Having a (mostly) complete starting point gives you one less thing to worry about when learning Ansible.

This is the setup I wish I'd started with. 

These instructions assume a working knowledge of the command line and a little bit of knowledge of Ansible terminology. 

## Notes

- This is not intended as a best practice example. It's a starting point for learning and experimentation.
- Ansible defaults to `/etc/ansible/host` for the host inventory. This starter setup places the inventory in a user account directory because:
  - I didn't want to deal with permission issues in /etc
  - I didn't want a global inventory scope on what is essentially a sandbox 
- If you don't want to set up an `.ansible.cfg` file to change the host inventory location, there's an environmental variable available:  https://docs.ansible.com/ansible/latest/reference_appendices/config.html#default-host-list

## Installation

- Install Ansible if you haven't already:  https://docs.ansible.com/ansible/latest/installation_guide/index.html
- Pick a name and location for the starter setup Ansible directory  
  Examples: ~/ansible, ~/devops/ansible
- Place a copy of the [ansible-starter](https://github.com/dale42/ansible-starter) repository files in your Ansible directory
  - Fork your own copy of the repo and clone it
  - Download as zip file
  - Clone a copy of this repo and delete the remote (`git remote rm origin`)  
- Place a copy of the Ansible sample configuration file, https://github.com/ansible/ansible/blob/devel/examples/ansible.cfg, in the file: `.ansible.cfg` in your home directory  
  i.e.: `~/.ansible.cfg`
- Update the `.ansible.cfg` inventory entry to match your configuration:  
  - Edit `~/.ansible.cfg`
  - Uncomment the inventory entry:  
    `[defaults]`  
     `#inventory = /etc/ansible/hosts`  
  - Set it to your host file path:  
  `inventory = {ansible_dir}/hosts`  
  `inventory = /home/dale/ansible/hosts`
- When you start using roles, update the entry: `roles_path`

If the installation has succeeded the command `ansible-inventory --list` will give the following results:

```
$ ansible-inventory --list
{
    "_meta": {
        "hostvars": {
            "host.com": {
                "a_var": "a_var value for host.com",
                "another_var": "another_var value for host.com",
                "ansible_host": "host.com",
                "ansible_user": "host_account",
                "variable_1": "This variable came from group_vars/all.yml"
            },
            "host.localhost": {
                "a_var": "a_var value for host.localhost",
                "another_var": "another_var value for host.localhost",
                "ansible_connection": "local",
                "ansible_host": "localhost",
                "ansible_user": "my_account_name",
                "variable_1": "This variable came from group_vars/all.yml"
            }
        }
    },
    "all": {
        "children": [
            "hosted_sites",
            "local_sites",
            "ungrouped"
        ]
    },
    "hosted_sites": {
        "hosts": [
            "host.com"
        ]
    },
    "local_sites": {
        "hosts": [
            "host.localhost"
        ]
    }
}
```

## Configuring your inventory

- Update the hosts file with real hosts and delete the sample entries.  
  - `ansible_host`, `ansible_user`, are the most important variables, they define how to connect to your host, think:  
   `ssh ansible_user@ansible_host`
  - There's an `ansible_port` variable if your host uses a custom ssh port (.e.g.: 2222) 
  - For localhost sites, the variable entry: `ansible_connection: local` bypasses ssh when connecting
- To test your inventory:  
  `ansible all -m ping`  
  `ansible hostname -m ping`   

## Using your setup

Resources for learning Ansible are available at https://docs.ansible.com/
