# Checkpoint8 Submission

- **COURSE INFORMATION: CSP451**
- **STUDENT’S NAME: Thuan Le**
- **STUDENT'S NUMBER: 125180182**
- **GITHUB USER ID: 125180182-myseneca**
- **TEACHER’S NAME: Atoosa Nasiri**

### Table of Contents
1. [Part A - Clean Network Resources](#part-a---clean-network-resources-follow-the-instructions-and-answer-all-questions)
2. [Part B - Create Azure Cluster Resources](#part-b---create-azure-cluster-resources-follow-the-instructions-and-answer-all-questions)
3. [Part C - Configure Host VM](#part-c---configure-host-vm-follow-the-instructions-and-answer-all-questions)
4. [Part D - Running Multi-Tier Python-Flask App Locally](#part-d---running-multi-tier-python-flask-app-locally-follow-the-instructions-and-answer-all-questions)
5. [Part E - Containerizing Multi-Tier Python-Flask App](#part-e---containerizing-multi-tier-python-flask-app-follow-the-instructions-and-answer-all-questions)


## Part A - Clean Network Resources: Follow the instructions and answer all questions.

1. ### What is the CLI command to list all NICs in table format?
    <p>The CLI command to list all NICs in table format is: `az network nic list --output table`</p>

2. ### What is the CLI command to list all NSGs in table format?
    <p>The CLI command to list all NSGs in table format is: `az network nsg list --output table`</p>

3. ### What is the CLI command to list all Routes in table format?
    <p>The CLI command to list all Routes in table format is: `az network route-table route list --resource-group MyResourceGroup --route-table-name MyRouteTable --output table`</p>

4. ### What is the CLI command to list all Route-Tables in table format?
    <p>The CLI command to list all Route-Tables in table format is: `az network route-table list --output table`

5. ### What is the CLI command to list all VNETs in table format?
    <p>The CLI command to list all VNETs in table format is: `az network vnet list --output table`
---

<details>

<summary>$ ./delete_all_nic.sh</summary>

```                                                         
 .d8888b.   .d8888b.  8888888b.     d8888  888888888  d888       
d88P  Y88b d88P  Y88b 888   Y88b   d8P888  888       d8888       
888    888 Y88b.      888    888  d8P 888  888         888       
888         "Y888b.   888   d88P d8P  888  8888888b.   888       
888            "Y88b. 8888888P" d88   888       "Y88b  888       
888    888       "888 888       8888888888        888  888       
Y88b  d88P Y88b  d88P 888             888  Y88b  d88P  888       
 "Y8888P"   "Y8888P"  888             888   "Y8888P" 8888888      


Loaded variabes without error

---------------------------------------------------
Deleting Network Interface Cards
---------------------------------------------------

NIC: wc-99 
Check if it  exists ---

Doesn't exist! Nothing to do ...

NIC: lr-99 
Check if it  exists ---

Doesn't exist! Nothing to do ...

NIC: ws-99 
Check if it  exists ---

Doesn't exist! Nothing to do ...

NIC: ls-99 
Check if it  exists ---

Doesn't exist! Nothing to do ...
```
</details>


<details>

<summary>$ ./delete_all_nsg.sh</summary>

```
 .d8888b.   .d8888b.  8888888b.     d8888  888888888  d888       
d88P  Y88b d88P  Y88b 888   Y88b   d8P888  888       d8888       
888    888 Y88b.      888    888  d8P 888  888         888       
888         "Y888b.   888   d88P d8P  888  8888888b.   888       
888            "Y88b. 8888888P" d88   888       "Y88b  888       
888    888       "888 888       8888888888        888  888       
Y88b  d88P Y88b  d88P 888             888  Y88b  d88P  888       
 "Y8888P"   "Y8888P"  888             888   "Y8888P" 8888888      


Loaded variabes without error

---------------------------------------------------
Delting Netwrok Security Groups
---------------------------------------------------

NSG: WC-NSG-99 
Check if it  exists ---

Doesn't exist! Nothing to do ...

NSG: LR-NSG-99 
Check if it  exists ---

Doesn't exist! Nothing to do ...

NSG: LS-NSG-99 
Check if it  exists ---

Doesn't exist! Nothing to do ...

NSG: WS-NSG-99 
Check if it  exists ---

Doesn't exist! Nothing to do ...
```
</details>

<details>

<summary>$ ./delete_all_rts.sh</summary>

```
                                                                 
                                                                 
 .d8888b.   .d8888b.  8888888b.     d8888  888888888  d888       
d88P  Y88b d88P  Y88b 888   Y88b   d8P888  888       d8888       
888    888 Y88b.      888    888  d8P 888  888         888       
888         "Y888b.   888   d88P d8P  888  8888888b.   888       
888            "Y88b. 8888888P" d88   888       "Y88b  888       
888    888       "888 888       8888888888        888  888       
Y88b  d88P Y88b  d88P 888             888  Y88b  d88P  888       
 "Y8888P"   "Y8888P"  888             888   "Y8888P" 8888888      


Loaded variabes without error

---------------------------------------------------
Deleting Routes
---------------------------------------------------

Route: Route-to-Server 
Check if it  exists ---

Doesn't exist! Nothing to do ...

Route: Route-to-Desktop 
Check if it  exists ---

Doesn't exist! Nothing to do ...


---------------------------------------------------
Deleting Subnet Associations
---------------------------------------------------


Deleting Route table Associatin for: Virtual-Desktop-Client

Deleting Route table Associatin for: SN1

---------------------------------------------------
Deleting Route Table: RT-99
---------------------------------------------------

Route Table: Route-to-Desktop 
Check if it  exists ---

Doesn't exist! Nothing to do ...


---------------------------------------------------
DONE!
---------------------------------------------------
```
</details>

<details>

<summary>$ ./delete_all_vnet.sh</summary>

```
                                                                 
                                                                 
 .d8888b.   .d8888b.  8888888b.     d8888  888888888  d888       
d88P  Y88b d88P  Y88b 888   Y88b   d8P888  888       d8888       
888    888 Y88b.      888    888  d8P 888  888         888       
888         "Y888b.   888   d88P d8P  888  8888888b.   888       
888            "Y88b. 8888888P" d88   888       "Y88b  888       
888    888       "888 888       8888888888        888  888       
Y88b  d88P Y88b  d88P 888             888  Y88b  d88P  888       
 "Y8888P"   "Y8888P"  888             888   "Y8888P" 8888888      


Loaded variabes without error

---------------------------------------------------
Deleting VNET Peering
---------------------------------------------------


RoutertoStudent in VNET: Router-99 ...

Check if it exists ---

Doesn't exist! Nothing to do ...


StudenttoRouter in VNET: Student-1202253-vnet ...

Check if it exists ---

Doesn't exist! Nothing to do ...


RoutertoServer in VNET: Router-99 ...

Check if it exists ---

Doesn't exist! Nothing to do ...


ServertoRouter in VNET: Server-99 ...

Check if it exists ---

Doesn't exist! Nothing to do ...


---------------------------------------------------
Deleting Sunbets and VNETs
---------------------------------------------------

---------------------------------------------------
VNET: Router-99
---------------------------------------------------

Subnet: SN1
Check if it exists ---

Doesn't exist! Nothing to do ...


Subnet: SN2
Check if it exists ---

Doesn't exist! Nothing to do ...


Subnet: SN3
Check if it exists ---

Doesn't exist! Nothing to do ...


Subnet: SN4
Check if it exists ---

Doesn't exist! Nothing to do ...


VNET: Router-99
Check if it exists ---

Doesn't exist! Nothing to do ...

---------------------------------------------------
VNET: Server-99
---------------------------------------------------

Subnet: SN1
Check if it exists ---

Doesn't exist! Nothing to do ...


Subnet: SN2
Check if it exists ---

Doesn't exist! Nothing to do ...


Subnet: SN3
Check if it exists ---

Doesn't exist! Nothing to do ...


Subnet: SN4
Check if it exists ---

Doesn't exist! Nothing to do ...


VNET: Server-99
Check if it exists ---

Doesn't exist! Nothing to do ...


---------------------------------------------------
DONE!
---------------------------------------------------
```
</details>








## Part B - Create Azure Cluster Resources: Follow the instructions and answer all questions.
## Part C - Configure Host VM: Follow the instructions and answer all questions.
## Part D - Running Multi-Tier Python-Flask App Locally: Follow the instructions and answer all questions.
## Part E - Containerizing Multi-Tier Python-Flask App: Follow the instructions and answer all questions.
