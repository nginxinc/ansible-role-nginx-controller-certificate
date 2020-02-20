NGINX Controller Certificate
============================

Upsert (create and update) certificates to NGINX Controller for use with Gateways and Components.

Requirements
------------

[NGINX Controller](https://www.nginx.com/products/nginx-controller/)

Role Variables
--------------

### Required Variables

`controller_fqdn` - The hostname or DNS of the NGINX Controller instance
`environment_name` - The Environment the certificate is being managed within
`controller_auth_token` - An authentication token for the NGINX Controller API (the nginx_controller_generate_token role outputs this)
`certificate_name` - The name of the certificate and keys in the provided environment

### Optional Variables

`certificate_displayname` - Optional friendly display name
`certificate_description` - Optional description

### Template Variables

#### PEM Certificates

`certificate_type: PEM` - PEM based certificate, key, CA certs
`certificate_privateKey` - Certificate private key formatted as a single string, substituting '/n' for line breaks
`certificate_publicCert` - Certificate public cert formatted as a single string, substituting '/n' for line breaks
`certificate_password` - Optional password for the certificate
`ca_certificates` - One or more CA certificates if necessary

#### PKCS12 Certificates

`certificate_type: PKCS12` - PKCS12 formatted certificate
`certificate_pkcs12_data` - Contents of the PKCS12 formatted file as a string
`certificate_password` - Certificate password

#### LOCAL_FILE Certificates

`certificate_type: LOCAL_FILE` - Local files already present on the NGINX Plus instance
`certificate_privateKey` - Path to the private key on the NGINX Plus instance file system
`certificate_publicCert` - Path to the public cert on the NGINX Plus instance file system

Dependencies
------------

Example Playbook
----------------

```yaml
- hosts: localhost
  gather_facts: no

  tasks:
    - name: Retrieve the NGINX Controller auth token
      include_role:
        name: nginx_controller_generate_token
      vars:
        user_email: "user@example.com"
        user_password: "mySecurePassword"
        controller_fqdn: "controller.mydomain.com"

    - name: Create the certificate object
      include_role:
        name: nginx_controller_certificate
      vars:
        controller_fqdn: "controller.mydomain.com"
        environment_name: "production-us-west"
        # controller_auth_token: output by previous role in example
        certificate_name: "www.example.com"
        certificate_displayname: "Root WWW Cert"
        certificate_description: "A complete sentence"
        certificate_privateKey: "contents of cert key as a string"
        certificate_publicCert: "contents of cert as a string"
        certificate_type: "PEM"
```

License
-------

[Apache License, Version 2.0](./LICENSE)

Author Information
------------------

brianehlert
