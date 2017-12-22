# Ansible Role: Logstash
An Ansible Role that installs Logstash on RedHat/CentOS.

Note that this role installs a syslog grok pattern by default; if you want to add more filters, please add them inside the /etc/logstash/conf.d/ directory. 

## Requirements

Requires Java (Preferred Java 8+). See [`avinash6784.oracle-java`](https://github.com/avinash6784/ansible-oracle-java) role to install Java 8.

This role is made Elastisearch as a backend for storing the logs. See [`avinash6784.elasticsearch`](https://github.com/avinash6784/ansible-role-elasticsearch) role to install Elasticsearch

## Role Variables
Available variables are listed below, along with default values (see `defaults/main.yml`):
```yml

# Lggstash major version
logstash_major_version: 5.x

# Lggstash port
logstash_port: 5044

# Elasticsearch hostname, where logstash ship logs.
logstash_elasticsearch_host:
  - http://localhost:9200

# Local paths to the SSL certificate and key files.
logstash_ssl_dir: /etc/pki/logstash
logstash_ssl_certificate_file: logstash-forwarder-example.crt
logstash_ssl_key_file: logstash-forwarder-example.key

To generate a self-signed certificate/key pair, you can use following role to generate ssl certs.
See [`avinash6784.openssl-certs`](https://github.com/avinash6784/ansible-role-openssl-certs) role to install and generate OpenSSL certs

**NOTE**: filebeat and logstash may not work correctly with self-signed certificates unless you also have the full chain of trust (including the Certificate Authority for your self-signed certs) added on your server. 
See: https://github.com/elastic/logstash/issues/4926#issuecomment-203936891

# Configuration for local syslog file
logstash_local_syslog_path: /var/log/syslog
logstash_monitor_local_syslog: true
```

## Dependencies

This role depends on avinash6784.oracle-java role. This is configured for ansible-galaxy install in requirements.yml.
Also you may need to install elasticsearch, because in this role uses elasticsearch as a backend to stores the logs.(avinash6784.ansible-role-elasticsearch role) 

**NOTE**: Requirements are installed as virtual user avinash6784 (avinash6784.oracle-java).

Be sure to install required roles with
```
ansible-galaxy install --role-file requirements.yml
```

## Usage and Example Playbook

Install from Ansible Galaxy
```
$ ansible-galaxy install avinash6784.logstash
```
Or download manually
```
$ git clone https://github.com/avinash6784/ansible-role-logstash.git
```
The code should reside in the roles directory of ansible ( See ansible documentation for more information on roles ), in a folder ansible-role-logstash.

## Run the playbook

First create a playbook including the git role, naming it test.yml.
```yml
- name: Install logstash
  hosts: localhost
  become: true
  roles:
    - { role: ansible-role-elasticsearch }
    - { role: ansible-role-logstash }

$ ansible-playbook -i hosts test.yml
```

## Author Informations

This role was created by [Avinash Pawar](http://devopstechie.com).

