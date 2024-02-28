# Ansible role: labocbz.install_phpmyadmin

![Licence Status](https://img.shields.io/badge/licence-MIT-brightgreen)
![CI Status](https://img.shields.io/badge/CI-success-brightgreen)
![Testing Method](https://img.shields.io/badge/Testing%20Method-Ansible%20Molecule-blueviolet)
![Testing Driver](https://img.shields.io/badge/Testing%20Driver-docker-blueviolet)
![Language Status](https://img.shields.io/badge/language-Ansible-red)
![Compagny](https://img.shields.io/badge/Compagny-Labo--CBZ-blue)
![Author](https://img.shields.io/badge/Author-Lord%20Robin%20Crombez-blue)

## Description

![Tag: Ansible](https://img.shields.io/badge/Tech-Ansible-orange)
![Tag: Debian](https://img.shields.io/badge/Tech-Debian-orange)
![Tag: SSL/TLS](https://img.shields.io/badge/Tech-SSL%2FTLS-orange)
![Tag: PhpMyAdmin](https://img.shields.io/badge/Tech-PhpMyAdmin-orange)

An Ansible role to install and configure PhpMyAdmin on your host.

The Ansible role aims to automate the installation process of the latest phpMyAdmin version. It seamlessly integrates phpMyAdmin with PHP and various database servers, enabling flexible configurations. The role accommodates diverse database server setups, such as SSL and mTLS modes, and allows customization of specific users for these databases, including roles for creating the required phpMyAdmin database and managing phpMyAdmin itself. The role takes care of the database creation process for each specified server, ensuring a streamlined deployment of phpMyAdmin with optimal security and functionality.

## Folder structure

By default Ansible will look in each directory within a role for a main.yml file for relevant content (also man.yml and main):

```PYTHON
.
├── README.md  # Contains an overview of the role and its purpose.
├── defaults
│   ├── main.yml  # Contains default variables for the role that can be overridden by users.
│   └── README.md  # Contains documentation for the default variables.
├── files
│   └── README.md  # Contains documentation for the files in the directory.
├── handlers
│   ├── main.yml  # Contains handlers that can be called by tasks within the role.
│   └── README.md  # Contains documentation for the handlers.
├── meta
│   ├── main.yml  # Contains metadata about the role, including dependencies and supported platforms.
│   └── README.md  # Contains documentation for the role metadata.
├── tasks
│   ├── main.yml  # Contains tasks to be executed by the role on the managed nodes.
│   └── README.md  # Contains documentation for the tasks.
├── templates
│   └── README.md  # Contains documentation for the templates.
└── vars
    ├── main.yml  # Contains variables that are specific to the role and are not meant to be overridden.
    └── README.md  # Contains documentation for the role variables.
```

## Tests and simulations

### Basics

You have to run multiples tests. *tests with an # are mandatory*

```MARKDOWN
# lint
# syntax
# converge
# idempotence
# verify
side_effect
```

Executing theses test in this order is called a "scenario" and Molecule can handle them.

Molecule use Ansible and pre configured playbook to create containers, prepare them, converge (run the role) and verify its execution.
You can manage multiples scenario with multiples tests in order to get a 100% code coverage.

This role contains a ./tests folder. In this folder you can use the inventory or the tower folder to create a simualtion of a real inventory and a real AWX / Tower job execution.

### Command reminder

```SHELL
# Check your YAML syntax
yamllint -c ./.yamllint .

# Check your Ansible syntax and code security
ansible-lint --config=./.ansible-lint .

# Execute and test your role
molecule lint
molecule create
molecule list
molecule converge
molecule verify
molecule destroy

# Execute all previous task in one single command
molecule test
```

## Installation

To install this role, just copy/import this role or raw file into your fresh playbook repository or call it with the "include_role/import_role" module.

## Usage

### Vars

Some vars a required to run this role:

```YAML
---
install_phpmyadmin__blowfish: "T_HFR8yM8NzybY5nkRNkmVYhktYsw7vB"
install_phpmyadmin__install_dir: "/usr/share/phpmyadmin"
install_phpmyadmin__tempdir: "/var/lib/phpmyadmin/tmp"
install_phpmyadmin__ssl_dir: "{{ install_phpmyadmin__install_dir }}/ssl"

install_phpmyadmin__php_version: "8.2"

install_phpmyadmin__user: "www-data"
install_phpmyadmin__group: "www-data"

install_phpmyadmin__dbservers:
  - db_host: "molecule-local-instance-1-install-phpmyadmin"
    db_port: 3306
    db_user: "root"
    db_password: "PN$^L8zP*wm@3q"
    db_remote: false
    db_ssl: true
    db_ssl_ca: "{{ install_phpmyadmin__ssl_dir }}/my-phpmyadmin-cluster.domain.tld/ca-chain.pem.crt"
    db_ssl_client_auth: true
    db_ssl_client_key: "{{ install_phpmyadmin__ssl_dir }}/my-phpmyadmin-cluster.domain.tld/my-phpmyadmin-cluster.domain.tld.pem.key"
    db_ssl_client_cert: "{{ install_phpmyadmin__ssl_dir }}/my-phpmyadmin-cluster.domain.tld/my-phpmyadmin-cluster.domain.tld.pem.crt"
    pma_user: "phpmyadmin"
    pma_password: "aQ&SR77mp@D&FF"

```

The best way is to modify these vars by copy the ./default/main.yml file into the ./vars and edit with your personnals requirements.

You can set vars in the template model in Ansible AWX / Tower or just surchage them during the playbook call.

In order to surchage vars, you have multiples possibilities but for mains cases you have to put vars in your inventory and/or on your AWX / Tower interface.

```YAML
# From inventory
---

inv_install_phpmyadmin__blowfish: "T_HFR8yM8NzybY5nkRNkmVYhktYsw7vB"
inv_install_phpmyadmin__install_dir: "/usr/share/phpmyadmin"
inv_install_phpmyadmin__tempdir: "/var/lib/phpmyadmin/tmp"
inv_install_phpmyadmin__ssl_dir: "{{ inv_install_phpmyadmin__install_dir }}/ssl"

inv_install_phpmyadmin__php_version: "8.2"

inv_install_phpmyadmin__user: "www-data"
inv_install_phpmyadmin__group: "www-data"

inv_install_phpmyadmin__dbservers:
  - db_host: "molecule-local-instance-1-install-phpmyadmin"
    db_port: 3306
    db_user: "root"
    db_password: "PN$^L8zP*wm@3q"
    db_remote: false
    db_ssl: true
    db_ssl_ca: "{{ install_phpmyadmin__ssl_dir }}/my-phpmyadmin-cluster.domain.tld/ca-chain.pem.crt"
    db_ssl_client_auth: true
    db_ssl_client_key: "{{ inv_install_phpmyadmin__ssl_dir }}/my-phpmyadmin-cluster.domain.tld/my-phpmyadmin-cluster.domain.tld.pem.key"
    db_ssl_client_cert: "{{ inv_install_phpmyadmin__ssl_dir }}/my-phpmyadmin-cluster.domain.tld/my-phpmyadmin-cluster.domain.tld.pem.crt"
    pma_user: "phpmyadmin"
    pma_password: "aQ&SR77mp@D&FF"

```

```YAML
# From AWX / Tower
---
all vars from to put/from AWX / Tower
```

### Run

To run this role, you can copy the molecule/default/converge.yml playbook and add it into your playbook:

```YAML
---
- name: "Include labocbz.install_phpmyadmin"
    tags:
    - "labocbz.install_phpmyadmin"
    vars:
      install_phpmyadmin__blowfish: "{{ inv_install_phpmyadmin__blowfish }}"
      install_phpmyadmin__install_dir: "{{ inv_install_phpmyadmin__install_dir }}"
      install_phpmyadmin__tempdir: "{{ inv_install_phpmyadmin__tempdir }}"
      install_phpmyadmin__ssl_dir: "{{ inv_install_phpmyadmin__ssl_dir }}"
      install_phpmyadmin__php_version: "{{ inv_install_phpmyadmin__php_version }}"
      install_phpmyadmin__user: "{{ inv_install_phpmyadmin__user }}"
      install_phpmyadmin__group: "{{ inv_install_phpmyadmin__group }}"
      install_phpmyadmin__dbservers: "{{ inv_install_phpmyadmin__dbservers }}"
    ansible.builtin.include_role:
    name: "labocbz.install_phpmyadmin"
```

## Architectural Decisions Records

Here you can put your change to keep a trace of your work and decisions.

### 2023-08-11: First Init

* First init of this role with the bootstrap_role playbook by Lord Robin Crombez
* Role install PhpMyAdmin in the latest version
* Upgrade is possible
* Role handle databases and user creation for PhpMyAdmin
* Multiple server is possible
* Multiple mode is possible (TLS/mTLS, Plain text)
* Role handle the import on the phpmyamdin database and the creation of the user for PhpMyAdmin
* The user is customizable

### 2023-10-06: New CICD, new Images

* New CI/CD scenario name
* Molecule now use remote Docker image by Lord Robin Crombez
* Molecule now use custom Docker image in CI/CD by env vars
* New CICD with needs and optimization

### 2023-11-18: mTLS

* Role REALLY handle mTLS auth

### 2023-12-14: System users

* Role can now use system users and address groups

## Authors

* Lord Robin Crombez

## Sources

* [Ansible role documentation](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html)
* [Ansible Molecule documentation](https://molecule.readthedocs.io/)
* [PWgen.io](https://pwgen.io/fr/)
* [How To Install phpMyAdmin From Source on Debian 10](https://www.digitalocean.com/community/tutorials/how-to-install-phpmyadmin-from-source-debian-10)
