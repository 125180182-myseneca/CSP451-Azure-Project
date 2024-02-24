# Checkpoint6 Submission

- **COURSE INFORMATION: CSP451
- **STUDENT’S NAME: Thuan Le
- **STUDENT'S NUMBER: 125180182
- **GITHUB USER ID: 125180182-myseneca
- **TEACHER’S NAME: Atoosa Nasiri

 
### Table of Contents

1. [Part A - Basic Connectivity - Linux VMs Firewall Setting](#part-a---basic-connectivity---linux-vms-firewall-setting)
2. [Part B - Azure Cost Analysis Charts](#part-b---azure-cost-analysis-charts)

### Part A - Basic Connectivity - Linux VMs Firewall Setting:

1. The command to check all the chains is `sudo iptables -nvL`. The default setting is ACCEPT. We can improve these settings by modifying some of the rules to drop or reject certain packets.
2. 
    <detail>
    <summary>LR-27</summary>

    `LR-27.CSP451-2241.com`
    
    [LR_27_hostname.txt](/Checkpoint6/Files/LR-27_hostname.txt)
    ![LR_SS](/Checkpoint6/CP6_images//1.png)


    </detail>

    <detail>
    <summary>LS-27</summary>

    `LS-27.CSP451-2241.com`

    [LS_27_hostname.txt](/Checkpoint6/Files/LS-27_hostname.txt)
    ![LR_SS](/Checkpoint6/CP6_images/2.png)

    </detail>

3.
    [LR-iptables.txt](/Checkpoint6/Files/lr-iptables.txt)

    [LS-iptables.txt](/Checkpoint6/Files/ls-iptables.txt)

### Part B - Azure Cost Analysis Charts
1.

 | No.|Scope              | Chart Type      | VIEW TYPE      | Date Range  | Group By     | Granularity | Example                                 |
 |----|-------------------|-----------------|----------------|-------------|--------------|-------------|-----------------------------------------|
 | 1  |Student-RG-1202818 |Column (Stacked) |DailyCosts      |Last 7 Days  |Resource      |Daily        |![image-1](/Checkpoint6/CP6_images/3.png)|
 | 2  |Student-RG-1202818 |Column (Stacked) |DailyCosts      |Last 7 Days  |Service       |Daily        |![image-2](/Checkpoint6/CP6_images/4.png)|
 | 3  |Student-RG-1202818 |Area             |AccumulatedCosts|Last 7 Days  |Resource      |Accumulated  |![image-3](/Checkpoint6/CP6_images/5.png)|
 | 4  |Student-RG-1202818 |Pie Chart        |NA              |Last Month   |Service Name  |NA           |![image-4](/Checkpoint6/CP6_images/6.png)|
 | 5  |Student-RG-1202818 |Pie Chart        |NA              |Last Month   |Service Family|NA           |![image-5](/Checkpoint6/CP6_images/7.png)|
 | 6  |Student-RG-1202818 |Pie Chart        |NA              |Last Month   |Product       |NA           |![image-6](/Checkpoint6/CP6_images/8.png)|

 
 2.
![image-6](/Checkpoint6/CP6_images/8.png)
