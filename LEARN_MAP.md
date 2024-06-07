# Ansible Learning Map

## Introduction to Ansible
- What is Ansible?
  - Configuration management
  - Automation tool
  - Agentless architecture
- History of Ansible
- Installation
  - Using pip
  - Using package manager (apt, yum, etc.)
  - Installing on different OS (Linux, Windows, MacOS)

## Basic Concepts
- Ansible Architecture
  - Control node
  - Managed nodes
  - Inventory
- YAML Basics
- Ansible Playbooks
  - Structure
  - Tasks
  - Handlers
  - Variables

## Inventory Management
- Static inventory
- Dynamic inventory
- Inventory file format
- Host and group variables

## Modules
- Core modules
  - Command
  - Shell
  - Copy
  - File
  - Package management (yum, apt)
- Extras modules
- Custom modules

## Playbooks
- Writing Playbooks
- Playbook structure
  - Hosts
  - Tasks
  - Handlers
  - Variables
  - Roles
- Debugging Playbooks
- Tags
- Conditionals
- Loops

## Roles and Galaxy
- Introduction to roles
- Creating roles
- Role directory structure
- Using roles in playbooks
- Ansible Galaxy
  - Finding roles
  - Installing roles

## Variables
- Defining variables
  - Inventory variables
  - Playbook variables
  - Role variables
  - Extra variables
- Variable precedence
- Fact variables

## Templates
- Introduction to Jinja2 templates
- Creating templates
- Using templates in playbooks

## Advanced Topics
- Ansible Vault
  - Encrypting and decrypting files
  - Using vault in playbooks
- Dynamic Inventory
- Ansible Tower / AWX
  - Features
  - Installation
  - Using Tower/AWX
- Ansible Plugins
  - Callback plugins
  - Connection plugins
  - Inventory plugins
  - Lookup plugins

## Best Practices
- Directory structure
- Modular playbooks
- Using version control (Git)
- Idempotency
- Documentation

## Testing and Debugging
- Syntax check
- Dry run (Check mode)
- Debugging tasks
  - Debug module
  - Verbose mode
- Testing with Molecule

## Integrations
- Ansible and Cloud providers
  - AWS
  - Azure
  - Google Cloud
- Ansible and Docker
- Ansible and Kubernetes

## Resources
- Official Documentation
- Tutorials and Courses
  - Ansible documentation
  - Ansible for DevOps by Jeff Geerling
  - Udemy
  - Coursera
- Books
  - "Ansible for DevOps" by Jeff Geerling
  - "Ansible: Up & Running" by Lorin Hochstein
- Online Communities
  - Ansible mailing list
  - Stack Overflow
  - Reddit (r/ansible)
- Practice Projects
  - Setting up a web server
  - Automating database backups
  - Deploying applications

## Conclusion
- Keeping up-to-date
  - Ansible blog
  - Release notes
  - Community events

