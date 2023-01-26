# Ansible
```
Ansible-Tower API calls" https://docs.ansible.com/ansible-tower/latest/html/towerapi/api_ref.html
```
```
How to  install Ansible-Tower
yum -y update
yum -y install epel-release
yum -y install ansible vim curl
mkdir /tmp/tower && cd  /tmp/tower
curl -k -O https://releases.ansible.com/ansible-tower/setup/ansible-tower-setup-latest.tar.gz
tar xvf ansible-tower-setup-latest.tar.gz
cd ansible-tower-setup*/
$ vim inventory
...........................
[tower]
localhost ansible_connection=local

[database]

[all:vars]
admin_password='AdminPassword'

pg_host=''
pg_port=''

pg_database='awx'
pg_username='awx'
pg_password='PgStrongPassword'

rabbitmq_username=tower
rabbitmq_password='RabbitmqPassword'
rabbitmq_cookie=cookiemonster

# Isolated Tower nodes automatically generate an RSA key for authentication;
# To disable this behavior, set this value to false
# isolated_key_generation=true
sudo ./setup.sh



```
export ANSIBLE_HOST_KEY_CHECKING=False
## Tower rest Calls
```
root@awxdev#  curl -qsku admin:password http://192.168.9.121:8080/api/v2/config/ | jq '{Tower: ."version", Ansible: ."ansible_version"}'
{
  "Tower": "17.1.0",
  "Ansible": "2.9.18"
}

```
