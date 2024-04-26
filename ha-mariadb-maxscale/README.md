# High Availability MariaDB + Maxscale

This is a four node configuration.

## Setup

This will create the necessary AWS resources:

```sh
AWS_PROFILE=support ansible-playbook setup_aws.yml
```

When it is completed, it will create two inventory files containing the host EC2 instances:

- mariadb.ini
- maxscale.ini
