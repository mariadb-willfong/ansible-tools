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

```sh
# List the primary DB
[primary]
node1 ansible_host=192.168.41.17 server_id=1

# List the replica DBs
[replica]
node2 ansible_host=192.168.41.19 server_id=2
node3 ansible_host=192.168.41.21 server_id=3

[async:children]
primary
replica

# List the nodes you want in Galera
[galera]
node1 ansible_host=192.168.41.17
node2 ansible_host=192.168.41.19
node3 ansible_host=192.168.41.21

[all:vars]
mariadb_version="mariadb-10.11"
ansible_python_interpreter=/usr/bin/python3
mariadb_root_password=root_pass
replication_password=repl_pass
galera_cluster_name=dev_cluster
mariabackup_password=backup_pass
```

## Enterprise Download Token

If you need to use the Enterprise Repo, please include your token into a file called `enterprise_token.yml`. It will not be commited into the repository due to `.gitignore`.

Example format:

```yml
download_token: c48...e42
```

You can get your token here: https://id.mariadb.com/account/

## Scripts Overview

These are the base playbooks. For ticket-specific tests, please create your own playbook. If you want to make any changes to these playbooks, please file a pull request.

#### `shutdown.yml`

This shutdowns all the VMs in the specified inventory file. This is used when we need to re-create the VM from scratch.

#### `make-fresh.yml`

This playbook resets the VM to the original state. If we make any changes to the VMs via the playbooks, we should update this script to remove it.

#### `install_mariadb_async.yml`

Install a 2-node async replication cluster with GTID.

#### `install_mariadb_galera.yml`

Install a 3-node Galera cluster.

#### `install_mariadb_10.6_es.yml`

WORK IN PROGRESS

Install MariaDB 10.6 Enterprise.
