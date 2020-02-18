NGINX Controller Certificate
=========

Upsert (create and update) certificates to NGINX Controller for use with Gateways and Components.

Requirements
------------

Role Variables
--------------

Required:
controller_fqdn | the hostname or DNS of the NGINX Controller instance
environment_name | the Environment the certificate is being managed within
controller_auth_token | an authentication token for the NGINX Controller api (the Role nginx_controller_generate_token outputs this)
certificate_name | the name of the certificate and keys in the provided Environment

Optional:
certificate_displayname | optional friendly display name
certificate_description | optional description

Combinations:
certificate_type: PEM | PEM based certificate, key, CA certs
certificate_privateKey | certificate private key formatted as single string, substituting '/n' for line breaks
certificate_publicCert | certificate public cert formatted as single string, substituting '/n' for line breaks
certificate_password | optional password for certificate
ca_certificates | one or more CA certificates when necessary

certificate_type: PKCS12 | PKCS12 formatted certificate
certificate_pkcs12_data | contents of the PKCS12 formatted file as a string
certificate_password | password of the certificate

certificate_type: LOCAL_FILE | local files already present on the NGINX Plus instance
certificate_privateKey | path to private key on the NGINX Plus instance file system
certificate_publicCert | path to public cert on the NGINX Plus instance file system

Dependencies
------------

Example Playbook
----------------

```yaml
- hosts: localhost
  gather_facts: no

  tasks:
  - include_role:
      name: nginx_controller_generate_token
    vars:
      user_email: "user@example.com"
      user_password: 'secure_password'
      controller_fqdn: "controller.example.local"

  - name: create the certificate object
    include_role:
      name: nginx_controller_certificate
    vars:
      controller_fqdn: "controller.example.local"
      environment_name: "production-us-west"
      # controller_auth_token: output by previous Role in example
      certificate_name: "www.example.com"
      certificate_displayname: "Root WWW Cert"
      certificate_description: "A complete sentence"
      certificate_privateKey: "contents of cert key as a string"
      certificate_publicCert: "contents of cert as a string"
      certificate_type: "PEM"
```

License
-------

Apache-2.0

Author Information
------------------

brianehlert
