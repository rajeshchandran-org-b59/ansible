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

### Variable precendence in ansible

    local variables > play variables 

In ansible, if you attempt to use a variable which is node decalred, then it returns an error.
    > CLIURL' is undefined < 

How to gather the facts of the nodes mentioned on the inventory:
    $ ansible -i inv all  -e ansible_user=ec2-user -e ansible_password=DevOps321 -m ansible.builtin.gather_facts

>>> If a playbook has 5 tasks, task 1,2,3,4,5 in order, what will happen if the task 3 fails? Will the test of the tasks get's executed? 

Running Playbooks organized through ROLES is the pattern and this way we know whihc playbook owns which files and variables and this ROLES has a specific structure which holds the files.

Roles Structure : 

```
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
```

Here are the top five reasons to use Ansible roles in simple terms:

    1) Keeps Things Organized – Roles break big, messy playbooks into smaller, cleaner parts, making them easier to manage.
    2) Easy to Reuse – You can use the same role in multiple projects, saving time and effort.
    3) Makes Scaling Simple – Roles help apply the same setup to many servers without extra work.
    4) Team-Friendly – Different team members can work on different roles separately without causing issues.
    5) Pre-Made Roles Exist – You can find ready-to-use roles online (like on Ansible Galaxy) instead of building everything from scratch.

With multi-environments, here is how we run the playbooks: 

```
 $ ansible-playbook -i inv-dev  -e ansible_user=ec2-user -e ansible_password=DevOps321 -e env=dev -e component=mysql expense.yaml
```

> What is role dependency in ansible ?

    When we are calling a role - x and before the start of the role if you want y - role to be executed first then we can define that as a dependent role.

    When you call something using role-dependency main.yml of that particular roles task will be executed first.