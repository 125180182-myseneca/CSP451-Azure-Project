Part A:

network_config.sh:

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
1. In network_config_test.sh what does if [[ ! $(az group list -o tsv --query "[?name=='$RG_NAME']") ]] do? Explain your answer.

    To summarize, all this statement does is it checks whether if the Resource Group exists.
    
    For the expression in the square brackets, it lists the resource groups, outputting to tsv output format, and then queries the output for resource groups that equal to the variable RG_NAME.
    The if statement only runs if there is no resource groups that are equal to the variable RG_NAME because of the ! symbol.

2. Why is it crucial to check if a resource exist before creating it? What bash syntax do you use to test this? How do you check if a vnet exists in vnet_create.sh?

    It is crucial to check whether a resource exists before creating it because creating new resources can take a significant amount of time. For example, Azure Bastion takes about 10 minutes to deploy. The bash syntax you can use to test this is through a if statement with an expression. You can check whether if a vnet exists in vnet_create.sh by the following statement: `if [[ $(az network vnet list -g $RG_NAME -o tsv --query "[?name=='$vnet']") ]]`.

3. What is the Azure CLI command to create vnet? Give the specific command as per your environment and unique ID configuration. What are the required and what are the optional parameters that you need to pass to it?

    The Azure CLI command to create a vnet is `"az network vnet create -n "VnetName". -g "resourceGroup"`. The required parameters are vnet name and resource group. The optional parameters are address-prefixes and --subnet-name. In the context of my environment, my example CLI command would be: `az network vnet create -n "Router-27" -g "Student-RG-1202818" --address-prefixes "172.17.27.0/24" --subnet-name "SN1" --subnet-prefixes "172.17.27.32/27"`

4. What is the Azure CLI command to create subnet? Give the specific command as per your environment and unique ID configuration. What are the required and what are the optional parameters that you need to pass to it?
    The Azure CLI command to create a subnet is `az network vnet subnet create -n "name" -g "resourceGroup" --vnet-name "vnetName"`. The required parameters are subnet name, resource group and vnet name. The optional parameters is address-prefixes. In the context of my environment, my example CLI command would be: `az network vnet subnet create -n "SN1" -g "Student-RG-1202818" --vnet-name "Router-27" --address-prefixes 172.17.27.32/27"`

---
Part B: Working 

1.
`az network vnet list --out json > vnet_list.json`

2.
`az network vnet show -g Student-RG-1202818 -n Student-1202818-vnet > student_vnet.json`

3.
`az network vnet peering list -g Student-RG-1202818 --vnet-name Router-27 --output table >> peerings.tbl`

`az network vnet peering list -g Student-RG-1202818 --vnet-name Server-27 --output table >> peerings.tbl`

`az network vnet peering list -g Student-RG-1202818 --vnet-name Student-1202818-vnet --output table >> peerings.tbl`


4.
`az network vnet subnet show -g Student-RG-1202818 -n SN1 --vnet-name Router-27 --output json`

"
{
  "addressPrefix": "192.168.27.32/27",
  "delegations": [],
  "etag": "W/\"7ed3101f-9550-459e-9dcf-680d42f5aeb8\"",
  "id": "/subscriptions/71d310bf-1718-4d11-87d1-99a7d4e2053f/resourceGroups/Student-RG-1202818/providers/Microsoft.Network/virtualNetworks/Router-27/subnets/SN1",
  "name": "SN1",
  "privateEndpointNetworkPolicies": "Disabled",
  "privateLinkServiceNetworkPolicies": "Enabled",
  "provisioningState": "Succeeded",
  "resourceGroup": "Student-RG-1202818",
  "type": "Microsoft.Network/virtualNetworks/subnets"
}
"
5.
az network route-table route list -g Student-RG-1202818 --route-table-name RT-27 --output table > route_list.tbl

6.
az network route-table route show --resource-group Student-RG-1202818 --route-table-name RT-27 -n Route-to-Server >> route_details.json
az network route-table route show --resource-group Student-RG-1202818 --route-table-name RT-27 -n Route-to-Desktop >> route_details.json

7.
az network vnet subnet show -g resourcegroup -n subnet --vnet-name vnet

Part C: Network Review Questions

1. What is Azure Virtual Network (VNET)? Elaborate in your own words, you may use diagrams if drawn by yourself.

    An Azure Virtual Network is similar to a network at home, but instead, the network environment is in the cloud. This allows the consumer to create Azure Resources within the particular network.

2. In the context of Hybrid Cloud architecture. How on-prem computers can access resources inside Azure virtual network?

    There are many ways, but the easiest and less difficult method is to implement a virtual private gateway on the Azure virtual network.

3. What are the most important benefits of Azure Virtual Networks? Elaborate in your own words. Do not copy/paste from Azure Documentation. Itemized list of just benefit without proper elaboration will not receive any marks

    One of the most important benefits of Azure Virtual Networks is that it gives you flexibilty on where you want to deploy your Network.

4. What is the difference between Network Security Group (NSG) and Route-Tables?
5. What is the difference between NSG and Firewalls?
6. What is a hob-and-spoc network topology and how be deployed in Azure Cloud?
7. In working with Azure VNETs, do you need o to define gateways for Azure to route traffic between subnets?
8. When do you need to configure and use Virtual Network Gateways?

Part D:

1. List all VMs and send the output in table format to vm_list.tbl file. What command did you use?

    `az vm list --output table > vm_list.tbl`

2. Get the details of your WC-99 using az show command and send the output in json format to WC-99-details.json file. What command did you use?
    
    `az vm show --name WC-27 -g Student-RG-1202818 --output json > WC-99-details.json`

3. List all NSG using az list command and send the output in table format to nsg_list.tblfile. What command did you use?
    
    `az network nsg list --output table > nsg_list.tbl`

4. Provide screenshot of auto shutdown configuration for LS_XX. Is there any command to show this? What is the time-zone? What should be the correct time settings considering the time zone differences?

    I couldn't find any Azure CLI command to show the auto shutdown configuration of a VM. The time-zone is Coordinated Universal Time. The correct time settings should be 12:00 a.m EST.

5. Why auto shutdown configuration is not done in vm_create code? Why is it a separate scripts? Is it possible to configure auto shutdown at the same time you are creating the VM?
    
    Auto shutdown configuration is not done in vm_create code and they are separate scripts because it is not possible to configure auto shutdown at the same time you are creating the VM. The VM needs to be created before you can configure auto shutdown on the VMs.

Part E:

