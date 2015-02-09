# rutha_deploy
rutha staging and deployment vagrant environment

### How to

#### Security

NEVER push keys to a public repo. We dearly recommend to clone to a private repo.

#### Playbook settings

Configure app settings

```ruby
# Ports
http_port: 80
https_port: 443

# nginx
domain: disrupting.com
ssl_name: disrupting.com

local_api_port: 3002
local_fe_port: 3005

timestamp: "{{ ansible_date_time.epoch }}"

# rutha
app_name: disrupting_app
app_repo: git@github.com:molekilla/rutha.git
app_branch: release0.1.0
app_version: v0.1.0
app_env: 
  NODE_ENV: production

```

#### SSL

##### Stage local
`sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout domain.key -out domain.crt`

##### Production

Purchase SSL and add cert and private key in provisioning/roles/nginx/files/domain_name.key | .crt

#### Using rutha_deploy with rutha


1. add deploySettings to ui/Gruntfile.js

```javascript
        data: {
            deploySettings: {
              ruthaDeploy: '/home/rutha_user/rutha_deploy',
              playbook: '/home/rutha_user/provisioning/playbook.yml',
              hosts: {
                production: {
                  name: 'aws',
                  sshKey: '/home/rutha_user/site.pem',
                  remoteUser: 'app'
                },
                staging: {
                  name: 'azure',
                  sshKey: '/home/rutha_user/staging.pem',
                  remoteUser: 'app'
                },
                vagrant: {
                  name: 'all',
                  remoteUser: 'vagrant'
                }
              }
            },
        }
```

2. Run command 
 * `grunt stagelocal` provisions a local Vagrant machine
 * `grunt staging` provisions staging environment
 * `grunt deploy` provisions production environment
