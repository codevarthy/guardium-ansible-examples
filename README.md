# guardium-ansible-examples
Set of Guardium Ansible scripts

Setup: 
1. update group_vars/vars.yml to include OAuth details. You will need to create a client application to use Guardium REST API
2. add all databases servers to inventory file in root folder - for example inventory.txt
3. List inactive staps
   
    ansible-playbook --tags always,print   list_inactive_staps.yml  -i inventory.txt
   
4. List inactive STAPs using REST API and automatically start them using SSH to each database host. Your hosts in inventory.txt should match how the STAPS are named in Guardium UI
   
   ansible-playbook --tags always,print,start_stopped_staps   list_inactive_staps.yml  -i inventory.txt

5. Bulk create datasources listed in datasource file defined in vars.yml - for example datasource.json
   
     ansible-playbook --tags=create  bulk-createdatasource.yml
6. Bulk test datasource connections for datasources listed in datasource file defined in vars.yml - for example datasource.json
   
     ansible-playbook --tags=test  bulk-createdatasource.yml
7. Bulk delete datasources listed in datasource file defined in vars.yml - for example datasource.json
   
     ansible-playbook --tags=test  bulk-createdatasource.yml
   
