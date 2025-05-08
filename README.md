Provided "as is", without warranty. Use at your own risk.

# guardium-ansible-examples
Set of Guardium Ansible scripts



## Setup: 
Using Guardium API - https://www.ibm.com/docs/en/gdp/12.x?topic=commands-using-guardium-rest-apis

1. update group_vars/vars.yml to include OAuth details. You will need to create a client application to use Guardium REST API. 
   For example: 
   ```
   example.yourdomain.com> grdapi register_oauth_client client_id=client1 grant_types="password"
   ID=0
   {"client_id":"client1","client_secret":"b1f242a2-xxxx-xxxx-xxxx-6298556c2eea","grant_types":"password"}
   ok
   example.yourdomain.com>
   ```
2. You will also need a Guardium user and password to request an OAuth token.
3. add all databases servers to inventory file in root folder - for example inventory.txt
```
  db2.mydemo.com ansible_host=10.100.1.7 ansible_ssh_user=root ansible_ssh_pass= guard_config_path=/usr/local/guardium/modules/STAP/current/guard-config-update
  db1.mydemo.com ansible_host=10.100.1.6 ansible_ssh_user=root ansible_ssh_pass=   guard_config_path=/usr/local/guardium/modules/STAP/current/guard-config-update
```

## STAP scripts 
### Guardium API reference - 
list_staps - https://www.ibm.com/docs/en/gdp/12.x?topic=reference-list-staps 

start/stop stap or gim - https://www.ibm.com/docs/en/gdp/12.x?topic=lustop-linux-unix-start-stop-s-tap-gim-processes-various-os-typesversions

restart_stap - https://www.ibm.com/docs/en/gdp/12.x?topic=reference-restart-stap

 List inactive staps
   
    ansible-playbook --tags always,print   list_inactive_staps.yml  -i inventory.txt
 List inactive STAPs using REST API and automatically start them using SSH to each database host. Your hosts in inventory.txt should match how the STAPS are named in Guardium UI
   ```
   ansible-playbook --tags always,print,start_stopped_staps   list_inactive_staps.yml  -i inventory.txt
   ```
 Restart STAP on db host using REST API to Guardium CM/Collector
 ```
 ansible-playbook restart_stap.yml --extra-vars "stap_host=raptor.gdemo.com"
 ```
 Check status of Guardium services running on database host (using SSH to DB host)
```
ansible-playbook check_staphost.yml --extra-vars "variable_host=mydb3.mydemo.com" -i inventory.txt
```

## Datasources
### Guardium API reference - 
create_datasource - https://www.ibm.com/docs/en/gdp/11.5.0?topic=reference-list-staps

delete_datasource - https://www.ibm.com/docs/en/gdp/12.x?topic=reference-delete-datasource-by-name

test_data_connection - https://www.ibm.com/docs/en/gdp/12.x?topic=reference-test-datasource-connection

Bulk create datasources listed in datasource file defined in vars.yml - for example datasource.json
```   
     ansible-playbook --tags=create  bulk-createdatasource.yml
```
Bulk test datasource connections for datasources listed in datasource file defined in vars.yml - for example datasource.json
```   
     ansible-playbook --tags=test  bulk-createdatasource.yml
```
 Bulk delete datasources listed in datasource file defined in vars.yml - for example datasource.json
```   
     ansible-playbook --tags=delete  bulk-createdatasource.yml
```   

## Upgrade STAP bundle etc
### Guardium API reference
gim_assign_bundle_or_module_to_client_by_version  - https://www.ibm.com/docs/en/gdp/12.x?topic=reference-gim-assign-bundle-module-client-by-version

gim_schedule_install - https://www.ibm.com/docs/en/gdp/12.x?topic=reference-gim-schedule-install

Assign bundle and schedule install. Update the targetted host(client_ip), bundle details in vars section of playbook
```
    ansible-playbook upgrade_stap_bundle.yml 
```
