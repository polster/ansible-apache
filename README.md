Apache
======

[Apache](http://httpd.apache.org/) role for Ansible.

## Prerequisites

* Add the role to the project's Ansible galaxy requirements file and install it via the ansible-galaxy command

## User Manual

### How to use

* Add the following line to your Ansible playbook's role section as follows (sample):
```
roles:
    - role: apache
```

### Virtual Host File Configuration

* As soon as the apache_vhost_file_list variable has been defined, this role will copy the given vhost files to the target environment specific Apache configuration dir
* Additionally, any given source vhost file can be used as parameterized template

#### Sample

```
# Specify the placeholder vars within the vhost file
<VirtualHost {{ vhost_ip }}:443>

  ServerName {{ vhost_server_name }}
  ...
</VirtualHost>
```

```
# Add some local vhost files
my_vhost_file_list:
  - 'app-config/apache/conf.d/devops-jenkins-master.conf'

# Define some vhost template variables
vhost_ip: '192.168.5.35'
vhost_server_name: 'devops-jenkins-master'

roles:
  - role: apache
    apache_vhost_file_list: '{{ my_vhost_file_list }}'
```

### SSL

* In situations where SSL is needed, related files to be copied can be specified as follows (example):
```
apache_ssl_file_list
  - "app-config/dev/all/apache/ssl/server-certificate.pem"
  - "app-config/dev/all/apache/ssl/server-key.pem"
```
* Where the code snippet within your vhost file may look like this:
```
SSLEngine on
SSLCipherSuite ECDH+AESGCM:DH+AESGCM:ECDH+AES256:DH+AES256:ECDH+AES128:DH+AES:ECDH+3DES:DH+3DES:RSA+AESGCM:RSA+AES:RSA+3DES:!aNULL:!MD5
SSLProtocol All -SSLv2 -SSLv3
SSLHonorCipherOrder On
SSLCompression Off
SSLCertificateFile /etc/httpd/ssl/server-certificate.pem
SSLCertificateKeyFile /etc/httpd/ssl/server-key.pem
```

## Configuration

### <a name="configVariables"></a>Configurable variables

See [defaults](defaults/main.yml)
