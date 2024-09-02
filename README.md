# Ansible Tools

## Enterprise Download Token

Create a file called `enterprise_token.yml` and include your token:

```
download_token: c48...e42
```

## `install_mariadb_async.yml`

Install an async replication 2-node cluster with GTID.

Example `.ini` file:

```
[primary]
mariadb_primary ansible_host=10.211.55.23 server_id=1

[replica]
mariadb_replica ansible_host=10.211.55.24 server_id=2

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
