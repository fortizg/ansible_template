# Ansible Template

Some handy template for Ansible project development. 

## This repository

Use this content to accelerate, ease and standardize project development.


## Credits and motivation:

- [Ansible Best Practices](https://docs.ansible.com/ansible/latest/user_guide/playbooks_best_practices.html)
- Everyday's work helped by wonderful developers
- Our curiosity that always tends to be quite close to infinity (?)
  
## Directory Structure

    project_name                  # directory where the project resides (MANDATORY)
        README.md                 # instructions on how to run the playbook and general information (MANDATORY)
        inventory                 # folder for inventory collections
            development           # inventory file for development servers
            hosts                 # common inventory file (MANDATORY)
            production            # inventory file for production servers
            staging               # inventory file for staging environment

        group_vars/
           group1.yml             # here we assign variables to particular groups
           group2.yml

        host_vars/
           hostname1.yml          # here we assign variables to particular systems
           hostname2.yml

        library/                  # if any custom modules, put them here (optional)
        module_utils/             # if any custom module_utils to support modules, put them here (optional)
        filter_plugins/           # if any custom filter plugins, put them here (optional)

        site.yml                  # master playbook (MANDATORY)
        webservers.yml            # playbook for webserver tier
        dbservers.yml             # playbook for dbserver tier

        roles/
            common/               # this hierarchy represents a "role"
                tasks/            #
                    main.yml      #  <-- tasks file can include smaller files if warranted
                handlers/         #
                    main.yml      #  <-- handlers file
                templates/        #  <-- files for use with the template resource
                    ntp.conf.j2   #  <------- templates end in .j2
                files/            #
                    bar.txt       #  <-- files for use with the copy resource
                    foo.sh        #  <-- script files for use with the script resource
                vars/             #
                    main.yml      #  <-- variables associated with this role
                defaults/         #
                    main.yml      #  <-- default lower priority variables for this role
                meta/             #
                    main.yml      #  <-- role dependencies
                library/          # roles can also include custom modules
                module_utils/     # roles can also include custom module_utils
                lookup_plugins/   # or other types of plugins, like lookup in this case

            webtier/              # same kind of structure as "common" was above, done for the webtier role
            monitoring/           # ""
            fooapp/               # ""


## Playbook requirements

All playbooks requires the following extra var:

* `target`: the playbook uses this variable to identify the host where is going to be applied, according to inventory file content. Groups and _all_ can also be used.


### Meta and individual playbooks for projects

1. `site.yml`

   * Main playbook. This apply all remediation's to a server

2. `something_todo.yml`

   * This playbook does some something in particular or just an specific subset of actions.


### How to run the playbooks manually

All the playbooks are run in a similar fashion

```console
$ ansible-playbook -u ansible_user -kK --extra-vars="target=targetServer" -i ./inventory site.yml
 ...
```

Where: 

* `ansible_user`: user to log on the remote server using SSH.

* `targetServer`: server or group where this playbook is going to be execute. It should exist in the inventory referenced below.

* `inventory`: inventory file or folder, so as to include multiple inventories, which contains server(s) or group(s) where this playbook is expected to be executed.


## Prerequisites

N/A
