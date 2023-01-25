## how to enable tower-cli

```
root@awxdev:~# tower-cli config

# Defaults.
format: human
password:
host: 127.0.0.1
verify_ssl: True
color: True
oauth_token:
use_token: False
verbose: False
certificate:
username:
description_on: False
```

```
tower-cli config verify_ssl false
tower-cli config host http://192.168.9.121:8080
tower-cli config username admin
tower-cli config password password
```

```
tower-cli inventory create \
  --organization "Default" \
  --name "Example Inventory" \
  --variables '{
                  "api_awx_url": "http://192.168.9.121:8080",
                  "api_awx_username": "admin",
                  "api_awx_password": "password"
              }'
              
 root@awxdev:~# tower-cli inventory create \
>   --organization "Default" \
>   --name "Example Inventory" \
>   --variables '{
>                   "api_awx_url": "http://192.168.9.121:8080",
>                   "api_awx_username": "admin",
>                   "api_awx_password": "password"
>               }'
Resource changed.
== ================= ============
id       name        organization
== ================= ============
 1 Example Inventory            1
== ================= ============
             
```
