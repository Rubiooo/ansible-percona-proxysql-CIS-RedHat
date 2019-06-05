# ansible-percona-db

## Ansible Dynamic Inventory on Azure - install software and dependencies
$: pip install --upgrade pip --user
$: pip install 'ansible[azure]' --user
$: pip install 'azure>=2.0.0rc5' --upgrade --user

## Update azure_rm.py file in ansible-percona-db/.ansible/
$: wget  https://raw.githubusercontent.com/ansible/ansible/devel/contrib/inventory/azure_rm.py
$ (on Mac) curl -o https://raw.githubusercontent.com/ansible/ansible/devel/contrib/inventory/azure_rm.py
$: chmod +x azure_rm.py

## Update azure_rm.ini file in ansible-percona-db/.ansible/
update resource_groups: <database resource group>
update locations: <database location>
update tags: <in key_value style>

## Update variables in roles/ansible-percona-cluster/defaults/main.yml
xtradb_nodes_group: <key_value> (For example: key_percona, same as percona vm tags in key_value style)

## Update playbook if host groups changed
for example: key_percona, key_proxysql

## Update config file to pick up from the respective host groups
for example: 'key_percona' in ansible-proxysql/config/proxysql_backend_servers.yml
for example: 'key_proxysql' in ansible-proxysql/config/proxysql_proxysql_servers.yml

## Connect to VPN, run playbook
$: ansible-playbook -i ./.ansible/azure_rm.py provision.yml
$: (if only install percona) ansible-playbook -i ./.ansible/azure_rm.py percona.yml
$: (if only install proxysql) ansible-playbook -i ./.ansible/azure_rm.py proxysql.yml
