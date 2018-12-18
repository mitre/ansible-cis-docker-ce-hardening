ansible-cis-docker-ce-hardening
=========

This Role configures Docker Daemon as per CIS Docker Community Edition Benchmark
Tested with Docker 18.06

## Versioning and State of Development
This project uses the [Semantic Versioning Policy](https://semver.org/). 

### Branches
The master branch contains the latest version of the software leading up to a new release. 

Other branches contain feature-specific updates. 

### Tags
Tags indicate official releases of the project.

Please note 0.x releases are works in progress (WIP) and may change at any time.   

Requirements
------------

If you must expose the Docker daemon via a network socket, TLS authentication for the daemon need to be configured.

Please Generate CA cert and CA Key and place it in the files directory and provide the following aruguments.

```
dockerd_via_network: true

dockerd_ip: 0.0.0.0
dockerd_port: 2376

ca_cert: ca.pem
ca_key: ca-key.pem
ca_key_passphrase: changeme

host_ip: 10.10.10.20

server_cert_path: /etc/docker/tls/server_certs
server_cert: server-cert.pem
server_cert_key: server-key.pem

client_cert_path: /etc/docker/tls/client_certs
client_cert: cert.pem
client_cert_key: key.pem
```

Role Variables
--------------
```
tursted_users:
  - vagrant

config_file: /etc/docker/daemon.json

dockerd_via_network: false

default_ulimits_nofile_soft: 100
default_ulimits_nofile_hard: 200
default_ulimits_nproc_soft: 1024
default_ulimits_nproc_hard: 2048

syslog_address: ''
seccomp_profile: ''
authorization_plugins: []
```

Issues
------

Control `2.8 Enable user namespace support` currently disabled since this interferes with `selinux-enabled` configuration.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: username.rolename, x: 42 }


Author Information
------------------

- Rony Xavier [rx294](https://github.com/rx294)

## NOTICE 

© 2018 The MITRE Corporation.

Approved for Public Release; Distribution Unlimited. Case Number 18-3678.  

## NOTICE
MITRE hereby grants express written permission to use, reproduce, distribute, modify, and otherwise leverage this software to the extent permitted by the licensed terms provided in the LICENSE.md file included with this project.

## NOTICE

This software was produced for the U. S. Government under Contract Number HHSM-500-2012-00008I, and is subject to Federal Acquisition Regulation Clause 52.227-14, Rights in Data-General.  

No other use other than that granted to the U. S. Government, or to those acting on behalf of the U. S. Government under that Clause is authorized without the express written permission of The MITRE Corporation. 

For further information, please contact The MITRE Corporation, Contracts Management Office, 7515 Colshire Drive, McLean, VA  22102-7539, (703) 983-6000.

## NOTICE

CIS Benchmarks are published by the Center for Internet Security (CIS), see: https://www.cisecurity.org/.
