## TUTORIAL NOTES


## RESOURCES

List of resources:

- https://www.youtube.com/watch?v=BS0GLQaSGPo (Ansible tutorial).


## INTRO

Ansible helps automating tasks when we have a set of
multiple servers and we want to automate
Deployments and keep consistancy accross our servers.

In the domain of sync conf we have two global mode of doing so:
    - **PULL CONFIGURATION**: each nodes checks the server for updates on the config and sync
               client
                    \
        client -> server <- client
    - **PUSH CONFIGURATION**: the server push the config when it got updated to sync with clients
               client
                    \
        client <- server -> client

> ANSIBLE is a **PUSH CONFIGURATION** type.


## HOW DOES IT WORKS

The local machine ssh into all the nodes clients and has a list of playbooks.

playbooks are the core of ansible
Example of a yaml ansible configuration file :

```yaml

- name: player1
  hosts: webserver
  tasks:
    - name: install postgresql
      yum:
        name: postgres
        state: present
    - name: start postgresql
      service:
        name: postgres
        state: start

- name player2
  hosts: syncserver
  tasks:
    # We run rsync
    - name: install rsync
        shell:
            name: rsync
            state: present
```
We have in our example two playbooks (player1 and player2).

Ansible works with an inventory file that list all the hosts and their ip addresses
and it's where we manage all our network management.
In this case we have two hosts  :
- webserver
- syncserver

The contains of the inventory can looks like :
```txt
[webserver]
web1.manachine
web2.manachine

[syncserver]
db1.manachine
```

## USE CASE

**NOTE:** Don't forget to `export TERM=xterm`

We're going to play on CentOs

To install ansible:

```bash
sudo yum update -y
sudo yum install epel-release
sudo yum install ansible -y
sudo yum install vim -y
```

Then update the /etc/ansible/hosts
```bash
[pgservers]
192.168.1.129 ansible_ssh_user=root
# the password will be asked on CLI mode
```

then we can create our playbook :
```yaml
- name: player1
  hosts: pgservers
  tasks:
    - name: install postgresql
      yum:
        name: postgres
        state: present
    - name: start postgresql
      service:
        name: postgres
        state: start
```

Before running, we can first check for the playbook linting with :
`ansible-playbook ./playbook.yml --syntax-check`

Then to run ansible we can run :
`ansible-playbook ./playbook.yml`

Or we can also specify the inventory file with this command :
`ansible-playbook -i inventory.ini playbook.yml`

To allow ports to be opens :
```
sudo yum install firewalld

sudo systemctl start firewalld
sudo systemctl enable firewalld

sudo firewall-cmd --permanent --add-service=ssh
sudo firewall-cmd --reload
sudo firewall-cmd --list-all
```


TO generate the ansible vault password, we just need to run :
```bash
echo "root" > password.txt
ansible-vault encrypt ./password.txt # to encrypt the password
```
This will encrypt the content of the password and save in txt.

Now, we have this updated version :
```yaml
---

- name: player1
  hosts: pgservers
  remote_user: root
  become: true
  tasks:
  - name: check reachable on ping
    ping:
  - name: Ensure a file with the secret password is present
    copy:
      content: "{{ my_secret_password }}"
      dest: ./password.txt
    when: my_secret_password is defined
  - name: install postgresql
    yum:
      name: postgresql-server
      state: latest

  - name: install postgresql-contrib
    yum:
      name: postgresql-contrib
      state: latest

  - name: run postgresql
    service:
      name: postgresql
      state: start
```

The content of pasword.yml:
```yaml
servers:
  hosts:
    192.168.1.129:
      ansible_user: root
      ansible_password: !vault |
        $ANSIBLE_VAULT;1.1;AES256
        33373166653239303231396465616664623262323432316238313235653437666338313935323663
        3534623364633534353539346239653131313632323631660a663537656235303163616234383932
        30356332343637353536616637626238316136353631313561623761313366366566633839653032
        3334366464313963360a623639613832353930616235333937663530343035613833373331656630
        6262
```
the ansible_password is the content of password.txt

# with this command, we should provide ssh password asked to run the playbook
ansible-playbook -i ./inventory.ini --ask-pass ./playbook.yml

# Complete playbook for postgresql for example:
```yaml
---
- name: Install and configure PostgreSQL on CentOS
  hosts: pgservers
  remote_user: root
  become: true  # Run tasks with sudo

  tasks:
    - name: Install PostgreSQL, Server and Contrib
      yum:
        name: [postgresql, postgresql-server, postgresql-contrib]
        state: present

    - name: Initialize PostgreSQL database
      command: postgresql-setup initdb
      args:
        creates: /var/lib/pgsql/data/postgresql.conf

    - name: Start PostgreSQL service
      service:
        name: postgresql
        state: started
        enabled: yes  # Ensure service starts on boot

    - name: Ensure PostgreSQL is listening on port 5432
      wait_for:
        port: 5432
        state: started

- name: Check PostgreSQL status on CentOS
  hosts: postgresql_servers
  gather_facts: no

  tasks:
    - name: Check PostgreSQL service status
      service_facts:
      register: service_facts
    - debug:
        msg: "PostgreSQL service is {{ 'running' if service_facts.ansible_facts.services['postgresql.service'].state == 'running' else 'not running' }}"
```

In case of the error :
```
FAILED! => {"msg": "Using a SSH password instead of a key is not possible because Host Key checking is enabled and sshpass does not support this.  Please add this host's fingerprint to your known_hosts file to manage this host."}
```
we need to ssh root@.... first to set keys that ansible is going to use.
