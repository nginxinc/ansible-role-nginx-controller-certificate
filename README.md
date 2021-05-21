NGINX Controller Certificate
============================

Upsert (create and update) certificates to NGINX Controller for use with Gateways and Components.

Formatting certificates in YAML is not always straightforward.  Please see /files for additional examples.

Requirements
------------

[NGINX Controller](https://www.nginx.com/products/nginx-controller/)

Role Variables
--------------

### Required Variables

`controller.fqdn` - The hostname or DNS of the NGINX Controller instance.

`controller.auth_token` - An authentication token for the NGINX Controller API (the nginx_controller_generate_token role outputs this).

`environmentName` - The Environment the certificate is being managed within.

`certificate.metadata.name` - Name of the certificate

`certificate.desiredState.type` - Type of the certificate. Can be PEM | PKCS12 | RemoteFile.

### Template Variables

This role has multiple template related variables. The descriptions and defaults for all these variables can be found in **[vars/main.yml](./vars/main.yml)**

Dependencies
------------

Example Playbook
----------------

To use this role you can create a playbook such as the following (let's name it `nginx_controller_certificate.yaml` for the purposes of this example).

```yaml
- hosts: localhost
  gather_facts: no
  vars:
    nginx_controller_user_email: "user@example.com"
    nginx_controller_user_password: "mySecurePassword"
    nginx_controller_fqdn: "controller.mydomain.com"
    nginx_controller_validate_certs: false

  tasks:
    - name: Retrieve the NGINX Controller auth token
      include_role:
        name: nginxinc.nginx_controller_generate_token

    - name: Create the certificate object
      include_role:
        name: nginxinc.nginx_controller_certificate
      vars:
        # nginx_controller_auth_token: output by previous role in example
        nginx_controller_environmentName: "production-us-west"
        nginx_controller_certificate:
          metadata:
            name: "www.example.com"
            displayName: "Root WWW Cert"
            description: "A certificate"
          desiredState:
            type: "PEM"
            privateKey: |-
              # Contents of cert key as a multiline, unquoted string
            publicCert: |-
              # Contents of cert as a multiline, unquoted string
```

You can then run `ansible-playbook nginx_controller_certificate.yaml` to execute the playbook.

Alternatively, you can also pass/override any variables at run time using the `--extra-vars` or `-e` flag like so `ansible-playbook nginx_controller_certificate.yaml -e "nginx_controller_user_email=user@company.com nginx_controller_user_password=notsecure nginx_controller_fqdn=controller.example.local nginx_controller_validate_certs=false"`

You can also pass/override any variables by passing a `yaml` file containing any number of variables like so `ansible-playbook nginx_controller_certificate.yaml -e "@nginx_controller_certificate_vars.yaml"`

License
-------

[Apache License, Version 2.0](./LICENSE)

Author Information
------------------

[Brian Ehlert](https://github.com/brianehlert)

[Alessandro Fael Garcia](https://github.com/alessfg)

[Daniel Edgar](https://github.com/aknot242)

&copy; [NGINX, Inc.](https://www.nginx.com/) 2020
