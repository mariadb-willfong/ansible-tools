# Ansible Tools

Using Ansible to automate standardized processes for our day to day work.

**Warning: Ansible does not support Windows natively**

## Requirements and Assumptions

1. Currently have only been tested on Rocky Linux 9 on ARM
1. VMs should be set up to allow `root` user to log in via SSH key

## Getting Started

1. Install Ansible
1. Set up VMs
1. Set up an `inventory.ini` file to pass to `ansible-playbook`. Example given below.
1. Run a playbook: `ansible-playbook -i <inventory> <playbook>`, example: `ansible-playbook -i inventory-async.ini install_mariadb_async.yml`

## Example `~/.ssh/config`

This configuration automatically configures your VMs to ignore the host fingerprints, so you can re-create VMs easier.

```sh
Host 10.211.55.*
    User root
    StrictHostKeyChecking no
    UserKnownHostsFile=/dev/null
```

## Example `inventory.ini`

This file should be configured for your environment. It should not be checked in, as it is only relevant to you.

You can create separate `.ini` files for each type of cluster, like an `inventory-async.ini` or `inventory-3node.ini` file.

```sh
[primary]
mariadb-primary ansible_host=10.211.55.23 server_id=1

[replica]
mariadb-replica ansible_host=10.211.55.24 server_id=2

[all:vars]
mariadb_version="mariadb-10.11"

[macos:children]
primary
replica

[macos:vars]
ansible_python_interpreter=/usr/bin/python3

[linux:children]
primary
replica

[linux:vars]
ansible_python_interpreter=/usr/bin/python3

mariadb_root_password=root_pass
replication_password=repl_pass
```

## Enterprise Download Token

If you need to use the Enterprise Repo, please include your token into a file called `enterprise_token.yml`. It will not be commited into the repository due to `.gitignore`.

Example format:

```
download_token: c48...e42
```

## Scripts Overview

These are the base playbooks. For ticket-specific tests, please create your own playbook. If you want to make any changes to these playbooks, please file a pull request.

#### `shutdown.yml`

This shutdowns all the VMs in the specified inventory file. This is used when we need to re-create the VM from scratch.

#### `uninstall_mariadb.yml`

This should remove everything related to MariaDB for all hosts in the inventory file. This should ensure the VM is a "fresh start". It does not need to revert OS level settings, like firewall or hostname changes.

#### `install_mariadb_async.yml`

Install a 2-node async replication cluster with GTID.

#### `install_mariadb_galera.yml`

Install a 3-node Galera cluster.

#### `install_mariadb_10.6_es.yml`

WORK IN PROGRESS

Install MariaDB 10.6 Enterprise.
