Part A: Creating Network Resources using Azure CLI

Modified network_config.sh
```
RG_NAME="Student-RG-1202818"     # your student group
LOCATION="CanadaCentral"    # your location
ID="27"          #unique ID assigned to you

Student_vnet_name="Student-1202818-vnet"
Student_vnet_address="10.28.178.0/24"
Client_Subnet_name="Virtual-Desktop-Client"
Client_Subnet_address="10.28.178.0/24"
```
---
Part A Questions:
1. In network_config_test.sh what does if [[ ! $(az group list -o tsv --query "[?name=='$RG_NAME']") ]] do? Explain your answer.
    
    To summarize, all this statement does is it checks whether if the Resource Group exists.
    
    For the expression in the square brackets, it lists the resource groups, outputting to tsv output format, and then queries the output for resource groups that equal to the variable RG_NAME.
    The if statement only runs if there is no resource groups that are equal to the variable RG_NAME because of the ! symbol.

2. Why is it crucial to check if a resource exist before creating it? What bash syntax do you use to test this? How do you check if a vnet exists in vnet_create.sh?

    It is crucial to check whether a resource exists before creating it. One of the reasons is because creating new resources can take a significant amount of time. For example, Azure Bastion takes about 10 minutes to deploy. The bash syntax you can use to test this is through a if statement with an expression. You can check whether if a vnet exists in vnet_create.sh by the following statement: 
    
    `if [[ $(az network vnet list -g $RG_NAME -o tsv --query "[?name=='$vnet']") ]]`
    
3. What is the Azure CLI command to create vnet? Give the specific command as per your environment and unique ID configuration. What are the required and what are the optional parameters that you need to pass to it?

    The Azure CLI command to create a vnet is:
    
    `"az network vnet create -n "VnetName". -g "resourceGroup"`. 
    
    The required parameters are vnet name and resource group. The optional parameters are address-prefixes and --subnet-name. In the context of my environment, my example CLI command would be: 
    
    `az network vnet create -n "Router-27" -g "Student-RG-1202818" --address-prefixes "172.17.27.0/24" --subnet-name "SN1" --subnet-prefixes "172.17.27.32/27"`

    
4. What is the Azure CLI command to create subnet? Give the specific command as per your environment and unique ID configuration. What are the required and what are the optional parameters that you need to pass to it?

    The Azure CLI command to create a subnet is: 

    `az network vnet subnet create -n "name" -g "resourceGroup" --vnet-name "vnetName"`

    The required parameters are subnet name, resource group and vnet name. The optional parameters is address-prefixes. In the context of my environment, my example CLI command would be: 
    
    `az network vnet subnet create -n "SN1" -g "Student-RG-1202818" --vnet-name "Router-27" --address-prefixes 172.17.27.32/27"`

Part B: Working with Azure CLI Bash

1. az network vnet list --output json > vnet_list.json
2. az network vnet show -g Student-RG-1202818 -n Student-1202818-vnet --output json > student_vnet.json
3. 
    az network vnet peering list --resource-group "Student-RG-1202818" --vnet-name Router-27 --output table >> peerings.tbl

    az network vnet peering list --resource-group "Student-RG-1202818" --vnet-name Server-27 --output table >> peerings.tbl

    az network vnet peering list --resource-group "Student-RG-1202818" --vnet-name Student-1202818-vnet --output table >> peerings.tbl

4. Note: Router-27, SN1 does not contain any route associations, therefore I will use Server-27

    Server-27 SN1 Details:

    `az network vnet subnet show -g Student-RG-1202818 -n SN1 --vnet-name Server-27`
    ```
    {
    "addressPrefix": "172.17.27.32/27",
    "delegations": [],
    "etag": "W/\"c5ca55bf-3e33-4e57-b15f-d6011de8fbe3\"",
    "id": "/subscriptions/71d310bf-1718-4d11-87d1-99a7d4e2053f/resourceGroups/Student-RG-1202818/providers/Microsoft.Network/virtualNetworks/Server-27/subnets/SN1",
    "name": "SN1",
    "privateEndpointNetworkPolicies": "Disabled",
    "privateLinkServiceNetworkPolicies": "Enabled",
    "provisioningState": "Succeeded",
    "resourceGroup": "Student-RG-1202818",
    "routeTable": {
        "id": "/subscriptions/71d310bf-1718-4d11-87d1-99a7d4e2053f/resourceGroups/Student-RG-1202818/providers/Microsoft.Network/routeTables/RT-27",
        "resourceGroup": "Student-RG-1202818"
    },
    "type": "Microsoft.Network/virtualNetworks/subnets"
    }

    ```
    Server-27 SN1 Routes:

    `az network vnet subnet show -g Student-RG-1202818 -n SN1 --vnet-name Server-27 --query routeTable`
    ```
    {"id": "/subscriptions/71d310bf-1718-4d11-87d1-99a7d4e2053f/resourceGroups/Student-RG-1202818/providers/Microsoft.Network/routeTables/RT-27",
    "resourceGroup": "Student-RG-1202818"
    }
    ```

5. az network route-table route list --resource-group Student-RG-1202818 --route-table RT-27 --output table > route_list.tbl

6. az network route-table route show --resource-group Student-RG-1202818 -n Route-to-Desktop --route-table-name RT-27 --output json >> route_details.json

    az network route-table route show --resource-group Student-RG-1202818 -n Route-to-Server --route-table-name RT-27 --output json >> route_details.json

7. az network vnet subnet show -g Student-RG-1202818 -n SN1 --vnet-name Server-27
