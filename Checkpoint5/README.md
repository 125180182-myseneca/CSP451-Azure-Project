# CSP451-Azure-Project

### Checkpoint5 Submission

* ##### COURSE INFORMATION: CSP451
* ##### STUDENT’S NAME: Thuan Le
* ##### STUDENT'S NUMBER: 125180182
* ##### GITHUB USER_ID: 125180182-myseneca
* ##### TEACHER’S NAME: Atoosa Nasiri
___

### Table of Contents

1. [Part A - Creating & Configuring VMs - Using Portal](#part-a---creating--configuring-vms---using-portal)
2. [Part B - Enable IP_Forwarding - Using Portal](#part-b---enable-ip_forwarding---using-portal)
3. [Part C - Basic Connectivity - VM Configuration](#part-c---basic-connectivity---vm-configuration)
4. [Part D - Creating & Configuring VM Images - Using Portal](#part-d---creating--configuring-vm-images---using-portal)
5. [Part E - Azure Cost Analysis Charts](#part-e---azure-cost-analysis-charts)
6. [Part F - Create Customized Azure Dashboard](#part-f---create-customized-azure-dashboard)

---
### Part A - Creating & Configuring VMs - Using Portal

1. What is the difference between Windows machine NSG and Linux machine NSG rules? Why? Do not give screenshots just describe the difference? Do you need a rule for ssh or rdp? What happens if you delete specific ssh and rdp rules?

    <p>The difference between Windows machine NSG and Linux machine NSG rules is that Windows machine NSG contains a rule for the remote desktop protocol (RDP), and the Linux machine NSG contains a rule for Secure Shell protocol (SSH). The way you interact with the OS's require separate protocols. We need the rules for ssh or rdp because it allows us to communicate with the machines. If we delete these rules, the virtual machines won't be accessible.</p>

2. Work from Azure Bash CLI in Portal to get a list of your VM, NSG, NIC, and Disks. You can start with the below commands. Make sure the outputs in table format are embedded in your submission.

    1. az group list --output table > rg_list_output
    
        [rg_list_output.txt](/Checkpoint5/CP5-Outputs/rg_list_output)
        <details>
        <summary>rg_list_output</summary>
        
        ```
        Name                Location       Status
        ------------------  -------------  ---------
        Bastion_RG          canadacentral  Succeeded
        NetworkWatcherRG    canadacentral  Succeeded
        Student-RG-1202818  canadacentral  Succeeded

        ```
        
        </details>

    2. az vm list -g $RG --output table > vm_list_output
    
        [vm_list_output.txt](/Checkpoint5/CP5-Outputs/vm_list_output)
        <details>
        <summary>vm_list_output</summary>

        ```
        Name    ResourceGroup       Location       Zones
        ------  ------------------  -------------  -------
        LR-27   Student-RG-1202818  canadacentral  1
        LS-27   Student-RG-1202818  canadacentral  1
        WC-27   Student-RG-1202818  canadacentral  1
        WS-27   Student-RG-1202818  canadacentral  1

        ```

        </details>

    3. az network nic list -g $RG --output table > nic_list_output
        
        [nic_list_output.txt](/Checkpoint5/CP5-Outputs/nic_list_output)
        <details>
        <summary>nic_list_output</summary>

        ```
        AuxiliaryMode    AuxiliarySku    DisableTcpStateTracking    EnableAcceleratedNetworking    EnableIPForwarding    Location       MacAddress         Name         NicType    Primary    ProvisioningState    ResourceGroup       ResourceGuid                          VnetEncryptionSupported
        ---------------  --------------  -------------------------  -----------------------------  --------------------  -------------  -----------------  -----------  ---------  ---------  -------------------  ------------------  ------------------------------------  -------------------------
        None             None            False                      False                          False                 canadacentral  60-45-BD-5D-0F-3C  lr-27644_z1  Standard   True       Succeeded            Student-RG-1202818  8ee39a11-7b1a-4e09-b9aa-360016c33ea4  False
        None             None            False                      False                          False                 canadacentral  60-45-BD-5D-B0-2D  ls-27487_z1  Standard   True       Succeeded            Student-RG-1202818  64d29a5f-c48f-4201-8860-6879fcdf6af3  False
        None             None            False                      False                          False                 canadacentral  00-22-48-3C-BF-78  wc-27277_z1  Standard   True       Succeeded            Student-RG-1202818  ef8033b3-fb8a-4fc1-ad73-1564c03d6d8a  False
        None             None            False                      False                          False                 canadacentral  60-45-BD-60-76-58  ws-27435_z1  Standard   True       Succeeded            Student-RG-1202818  c07ed42b-ad1a-4bbf-9786-af7308a39061  False

        ```

        </details>

    4. az network nsg list -g $RG --output table > nsg_list_output

        [nsg_list_output.txt](/Checkpoint5/CP5-Outputs/nsg_list_output)
        <details>
        <summary>nsg_list_output</summary>

        ```
        Location       Name        ProvisioningState    ResourceGroup       ResourceGuid
        -------------  ----------  -------------------  ------------------  ------------------------------------
        canadacentral  LR-27-nsg   Succeeded            Student-RG-1202818  21a33ac3-709c-4328-b342-da65c63c1ca0
        canadacentral  LS-27-nsg   Succeeded            Student-RG-1202818  aa75a725-4ba9-4159-8e31-203bf33c11f4
        canadacentral  WC-27-nsg   Succeeded            Student-RG-1202818  f145ff6a-5c50-4d82-b09b-f7d132a34dc5
        canadacentral  WS-27-nsg   Succeeded            Student-RG-1202818  90080557-9cce-4e8c-86c3-123eca8fe27d
        canadacentral  WS27nsg300  Succeeded            Student-RG-1202818  0e276da9-48a4-4593-bef4-8af13b9958a6

        ```

        </details>
    
    5. az disk list -g $RG --output table > disk_list_output

        [disk_list_output.txt](/Checkpoint5/CP5-Outputs/disk_list_output)

        <details>
        <summary>disk_list_output</summary>

        ```
        HyperVGeneration    Location       Name             ProvisioningState    ResourceGroup
        ------------------  -------------  ---------------  -------------------  ------------------
        V2                  canadacentral  lr-27-ver-0.0.1  Succeeded            Student-RG-1202818
        V1                  canadacentral  lr-27-ver-2      Succeeded            Student-RG-1202818
        V2                  canadacentral  ls-27-ver-0.0.1  Succeeded            Student-RG-1202818
        V1                  canadacentral  ls-27-ver-2      Succeeded            Student-RG-1202818
        V2                  canadacentral  wc-27-ver-0.0.1  Succeeded            Student-RG-1202818
        V1                  canadacentral  wc-27-ver-2      Succeeded            Student-RG-1202818
        V2                  canadacentral  ws-27-ver-0.0.1  Succeeded            Student-RG-1202818
        V1                  canadacentral  ws-27-ver-2      Succeeded            Student-RG-1202818

        ```

        </details>
    

----    
### Part B - Enable IP_Forwarding - Using Portal
1. Check the status of ip-forwarding using the command az network nic ip-config show with output format as json. Include only the command not output including the --quey you used in your submission.

    `az network nic show -g $RG -n lr-27644_z1 --query "enableIPForwarding" --output json`

2. When your output format is json, which property shows the status of the ip-forwarding attribute? Embed only the property that shows the status of ip-forwarding.
**<p> The "enableIPForwarding" property shows the status of the ip-forwarding attribute.</p>**
    <details>
        <summary>az network nic show -g $RG -n lr-27644_z1 --output json</summary>

    `"enableIPForwarding": true,`
    </details>

    <details>
        <summary>az network nic show -g $RG -n lr-27644_z1 --query "enableIPForwarding" --output json</summary>
    true
    </details>

----

### Part C - Basic Connectivity - VM Configuration

1. In configuring your Linux VMs, for the step "Remove the firewalld service", which command will you be using?
    
    I will be using the `sudo yum remove firewalld` command to remove the firewalld service.

2. In configuring your Linux VMs, what command do you use to check the status of iptabels?

    I will be using the `systemctl status iptables` command to check the status of iptables.

3. How can you make iptables service start automatically after reboot on CenOS/RHEL8? 👉 Hint: RHEL7: How to disable `Firewalld`` and use Iptables instead.

    I will be using the `systemctl enable iptables` command to enable iptables to start automatically upon reboot.

4. Run a command in LR-xx that shows all iptables chains with their order number. What is the default setting? Include both the command and its output in your submission. How could you improve these settings to be less vulnerable to attacks?

    <details>
    <p>The default setting are 5 input rules 1 forward rule, and 0 rules in output. In order to improve these settings to be less vulnerable to attacks, we can add a rule to reject all other protocols that we don't need.</p>

    <summary>[tle53@LR-27 ~]$ sudo iptables -L -n --line-numbers</summary>

    ```
    Chain INPUT (policy ACCEPT)
    num  target     prot opt source               destination
    1    ACCEPT     all  --  0.0.0.0/0            0.0.0.0/0            state RELATED,ESTABLISHED
    2    ACCEPT     icmp --  0.0.0.0/0            0.0.0.0/0
    3    ACCEPT     all  --  0.0.0.0/0            0.0.0.0/0
    4    ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0            state NEW tcp dpt:22
    5    REJECT     all  --  0.0.0.0/0            0.0.0.0/0            reject-with icmp-host-prohibited

    Chain FORWARD (policy ACCEPT)
    num  target     prot opt source               destination
    1    REJECT     all  --  0.0.0.0/0            0.0.0.0/0            reject-with icmp-host-prohibited

    Chain OUTPUT (policy ACCEPT)
    num  target     prot opt source               destination
    ```
    </details>

5. Run a command that shows the hostname in LR-XX and LX-XX. Embed the output in your submission.

![LR_SS](/Checkpoint5/LR_SS.png)
![LS_SS](/Checkpoint5/LS_SS.png)

----

### Part D - Creating & Configuring VM Images - Using Portal

1. Run a command in CLI that lists all your Custom Images. Hint: az image list .... Change the output format to table format and embed the answer in your submission.
[image_list_output.txt](/Checkpoint5/CP5-Outputs/image_list_output)
<details>
<summary>az image list -g Student-RG-1202818 --output table > image_list_output</summary>

```
HyperVGeneration    Location       Name             ProvisioningState    ResourceGroup
------------------  -------------  ---------------  -------------------  ------------------
V2                  canadacentral  lr-27-ver-0.0.1  Succeeded            Student-RG-1202818
V1                  canadacentral  lr-27-ver-2      Succeeded            Student-RG-1202818
V2                  canadacentral  ls-27-ver-0.0.1  Succeeded            Student-RG-1202818
V1                  canadacentral  ls-27-ver-2      Succeeded            Student-RG-1202818
V2                  canadacentral  wc-27-ver-0.0.1  Succeeded            Student-RG-1202818
V1                  canadacentral  wc-27-ver-2      Succeeded            Student-RG-1202818
V2                  canadacentral  ws-27-ver-0.0.1  Succeeded            Student-RG-1202818
V1                  canadacentral  ws-27-ver-2      Succeeded            Student-RG-1202818
```
</details>

2. Delete your VMs after your work is completed. Run a command in CLI that lists all your VMs. Hint: az vm list .... Change the output format to table format and embed the answer in your submission.

az vm list -g Student-RG-1202818 --output table > vm_list_output1

[vm_list_output1.txt](/Checkpoint5/CP5-Outputs/vm_list_output1)
<details>
<summary>az_vm_list</summary>

</details>

3. Recreate all VMs from your images, and establish basic connectivity. How long the entire process took? How can you do this more efficiently?

    This was a tedious process as you have to go to the individual images one by one in the portal, and still must configure the VM's. I think to do this more effectively, we could use ARM templates, or even create a script which will automatically use the images to create the VM's.

----

 ### Part E - Azure Cost Analysis Charts

 | No.|Scope              | Chart Type      | VIEW TYPE      | Date Range  | Group By     | Granularity | Example                                 |
 |----|-------------------|-----------------|----------------|-------------|--------------|-------------|-----------------------------------------|
 | 1  |Student-RG-1202818 |Column (Stacked) |DailyCosts      |Last 7 Days  |Resource      |Daily        |![image-1](/Checkpoint5/CP5_images/1.png)|
 | 2  |Student-RG-1202818 |Column (Stacked) |DailyCosts      |Last 7 Days  |Service       |Daily        |![image-2](/Checkpoint5/CP5_images/2.png)|
 | 3  |Student-RG-1202818 |Area             |AccumulatedCosts|Last 7 Days  |Resource      |Accumulated  |![image-3](/Checkpoint5/CP5_images/3.png)|
 | 4  |Student-RG-1202818 |Pie Chart        |NA              |Last Month   |Service Name  |NA           |![image-4](/Checkpoint5/CP5_images/4.png)|
 | 5  |Student-RG-1202818 |Pie Chart        |NA              |Last Month   |Service Family|NA           |![image-5](/Checkpoint5/CP5_images/5.png)|
 | 6  |Student-RG-1202818 |Pie Chart        |NA              |Last Month   |Product       |NA           |![image-6](/Checkpoint5/CP5_images/6.png)|

----

### Part F - Create Customized Azure Dashboard

![image-7](/Checkpoint5/CP5_images/7.png)