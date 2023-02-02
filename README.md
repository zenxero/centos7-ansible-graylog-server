# Archival Purposes

# WARNING - This playbook is outdated and no longer works on modern RHEL-Like distros.

This an archive of an Ansible playbook circa 2017 or so. It only targets CentOS 7 and installs the Graylog logging server. It uses an outdated version of Graylog, MongoDB, and Elasticsearch.

# Deploy Graylog Server

This repo contains an Ansible playbook that will automatically install and configure a CentOS 7 system as a Graylog log aggregation server.

## Table of Contents
  * [Requirements](#requirements)
  * [Usage](#usage)
  * [Note](#note)

<a name="requirements"></a>
## Requirements

  * [Ansible Control System Requirements](https://docs.ansible.com/ansible/latest/intro_installation.html#control-machine-requirements)
  * A system with the `ansible` and `git` packages installed.
  * The ability to SSH to the target system as root with an SSH key.
  * The target system that will be the Nagios server needs to be running the CentOS 7 Linux distribution.
  * The target CentOS 7 system should have a valid FQDN (Fully Qualified Domain Name) on your local network.

<a name="usage"></a>
## Usage

This is a standalone playbook, meaning that it includes its own `ansible.cfg` file and does not need a system-level config file.

To use this playbook:

1. On your "control" system with ansible and git installed, clone this git repo.
  
2. In the [hosts](hosts) file, you will want to define the host that you want to make your nagios master server as well as the hosts that you want to monitor with said master server.
  * Define your nagios master server in the `graylog-server` hostgroup
  
3. In the [deploy-graylog-server.yml](deploy-graylog-server.yml) file, configure the following variables:
  * The `password_secret` variable. This is the secret used to secure the other user passwords for Graylog.
  * The `root_password_sha2` variable. This is the root password that you will use to login and set up the web interface of Graylog.

4. Now, run the ansible deploy command with this playbook:

```
ansible-playbook deploy-graylog-server.yml
```

In a few minutes, you should have a working graylog server.

After the playbook is finished, you can access the web interface of your graylog server by navigating to:

`https://your-hostname.example.com/`

Since this just uses a self-signed SSL cert by default, your browser will warn you that it can't validate the cert and that it may not be secure. Just continue through.

To login, use the default credentials:

  * **Username:** admin
  * **Password:** The pasword you set in the `root_password_sha2` variable

<a name="note"></a>
## Note

There is more configuration that needs to be done once the graylog server is up and running.

You need to set up the Syslog TCP input stream via the webserver, otherwise graylog won't display any logs even if you have clients pointed at it.

You will also need to tie the authentication to LDAP/AD via the configuration settings in the webserver.
