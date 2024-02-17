# CSP451-Azure-Project

### Checkpoint5 Submission

* ##### COURSE INFORMATION: CSP451
* ##### STUDENT’S NAME: Thuan Le
* ##### STUDENT'S NUMBER: 125180182
* ##### GITHUB USER_ID: 125180182-myseneca
* ##### TEACHER’S NAME: Atoosa Nasiri
___

### Table of Contents

1. [An Image](#An-Image)

    
---
### Part A:

1. What is the difference between Windows machine NSG and Linux machine NSG rules? Why? Do not give screenshots just describe the difference? Do you need a rule for ssh or rdp? What happens if you delete specific ssh and rdp rules?

    <p>The difference between Windows machine NSG and Linux machine NSG rules is that Windows machine NSG contains a rule for the remote desktop protocol (RDP), and the Linux machine NSG contains a rule for Secure Shell protocol (SSH). The way you interact with the OS's require separate protocols. We need the rules for ssh or rdp because it allows us to communicate with the machines. If we delete these rules, the virtual machines won't be accessible.</p>

2. Work from Azure Bash CLI in Portal to get a list of your VM, NSG, NIC, and Disks. You can start with the below commands. Make sure the outputs in table format are embedded in your submission.

    1. az group list --output table > rg_list_output
        <details>
        <summary>WC-99-details.json</summary>
        </details>

    2. az vm list -g $RG --output table > vm_list_output
        <details>
        <summary>WC-99-details.json</summary>
        </details>

    3. az network nic list -g $RG --output table > nic_list_output
        <details>
        <summary>WC-99-details.json</summary>
        </details>

    4. az network nsg list -g $RG --output table > nsg_list_output
        <details>
        <summary>WC-99-details.json</summary>
        </details>
    
    5. az disk list -g $RG --output table > disk_list_output
        <details>
        <summary>WC-99-details.json</summary>
        </details>
    
### Part B:
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

### Part C:

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
