# Ansible Tools

## Inventory File

```
[mariadb]
db106 ansible_host=your_ip_address ansible_user=root ansible_password=password ansible_ssh_common_args='-o StrictHostKeyChecking=no'
```

## Enterprise Download Token

Create a file called `enterprise_token.yml` and include your token:

```
download_token: c48...e42
```
