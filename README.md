# learn-ansible

Ansible is developed using python.
To install Ansible on linux, we use a python package manager call PIP. 

# dnf install ansible  ( offers ansible core which is ansible-7 )

Latest and grestest version of ansible is available here :  ( 11.3.0 )
https://pypi.org/project/ansible/

# To install ansible:

```
    $ sudo pip3.11 install ansible
```

### What is inventory ?
    Inventory is a file that has the list of all the vm ip's that needs to be managed by ansible
    all is default group on this file that includes every thing on the file.

### Running ansible commands manually:

> If we execute the commands manually, we can execture one command at a time.

    $ ansible -i inv all  -e ansible_user=ec2-user -e ansible_password=DevOps321  -m ansible.builtin.shell -a uptime
    $ ansible -i inv all  -e ansible_user=ec2-user -e ansible_password=DevOps321  -m ansible.builtin.ping


Ansible is all about modules ( from version 2.8, we are referring them as collections ),

If we go with the manual way of running these commands, we can run one command at a time.

> In our case, we neeed to unstasll nginx, start the service and download the packages

    $ ansible -i inv all  -e ansible_user=ec2-user -e ansible_password=DevOps321  -m ansible.builtin.package nginx
    $ ansible -i inv all  -e ansible_user=ec2-user -e ansible_password=DevOps321  -m ansible.builtin.systemd_service nginx
    $ ansible -i inv all  -e ansible_user=ec2-user -e ansible_password=DevOps321  -m  ansible.builtin.get_url http://syxzz/d

Running manually is not a great way and not a right approach to handle. 

The right approach to handle ansible is using PLAYBOOKS ( ansible playbook: ansible scripts )

To do automation using ANSIBLE, we achieve it using PLAYBOOKS. Playbooks are written using YAML. 

> Leanring YAML is very Easy
    YAML is all about 
        * Dictionary: A key with single value is referred as Dictionary
                        a: 10 
        * List: A key with multiple values is referred as a List 
                        courses: 
                           - python
                           - java
                           - nodejs

> In ansible, what is a playbook ?
    1) A playbook is nothing but a list of plays 
    2) A play is nothing but a list of tasks
    3) A task is nothing but a list of actions 

All the playbooks or yaml files should end with an extension as `.yml or .yaml`

### How to run an ansible playbook ?

    $ ansible-playbook -i inv  -e ansible_user=ec2-user -e ansible_password=DevOps321 playBookName.yml
    $ ansible-playbook -i inv  -e ansible_user=ec2-user -e ansible_password=DevOps321 003-fileVars.yaml -e env=prod
    $ ansible-playbook -i inv  -e ansible_user=ec2-user -e ansible_password=DevOps321 007-tags.yaml --tags frontend

### Variable precendence in ansible

    task variables > play variables 

In ansible, if you attempt to use a variable which is note decalred, then it returns an error.
    > CLIURL' is undefined < 

How to gather the facts of the nodes mentioned on the inventory:
    $ ansible -i inv all  -e ansible_user=ec2-user -e ansible_password=DevOps321 -m ansible.builtin.gather_facts

>>> If a playbook has 5 tasks, task 1,2,3,4,5 in order, what will happen if the task 3 fails? Will the test of the tasks get's executed? 