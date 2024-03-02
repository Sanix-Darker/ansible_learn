## TUTORIAL NOTES


## RESOURCES

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
- name player1
  hosts: webserver
  tasks:
    # We install postgres
    - name: install postgresql
        yum:
            name: postgres
            state: present
    # We start postgres with service
    - name: start postgresql
        service:
            name: postgres
            state: start
```
