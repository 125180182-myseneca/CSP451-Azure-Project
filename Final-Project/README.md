# Final Project Submission

- **COURSE INFORMATION: CSP451**
- **STUDENT’S NAME: Thuan Le**
- **STUDENT'S NUMBER: 125180182**
- **GITHUB USER ID: 125180182-myseneca**
- **TEACHER’S NAME: Atoosa Nasiri**

----

### Table of Contents
1. [Prepare application for AKS](#prepare-application-for-aks)
2. [Create container registry](#create-container-registry)
3. [Create Kubernetes cluster](#create-kubernetes-cluster)
4. [Deploy containerized application](#header)
5. (Optional) [Use PaaS services](#header)
6. [Scale application](#scale-application)
7. [Upgrade cluster](#upgrade-cluster)

----

## Prepare application for AKS

**PS C:\Users\lemon\Desktop\Winter 2024\CSP451\aks-store-demo> docker container list**

<details>

<summary>docker container list</summary>
```
CONTAINER ID   IMAGE                                COMMAND                  CREATED          STATUS                      PORTS                                                                                                         NAMES
f27a8d758fc1   aks-store-demo-store-front           "/docker-entrypoint.…"   23 minutes ago   Up 22 minutes (unhealthy)   80/tcp, 0.0.0.0:8080->8080/tcp                                                                                store-front
97ce857fb4f6   aks-store-demo-order-service         "docker-entrypoint.s…"   23 minutes ago   Up 22 minutes (healthy)     0.0.0.0:3000->3000/tcp                                                                                        order-service
5a08f3665187   rabbitmq:3.11.17-management-alpine   "docker-entrypoint.s…"   23 minutes ago   Up 23 minutes (healthy)     4369/tcp, 5671/tcp, 0.0.0.0:5672->5672/tcp, 15671/tcp, 15691-15692/tcp, 25672/tcp, 0.0.0.0:15672->15672/tcp   rabbitmq
033d488028e8   aks-store-demo-product-service       "./product-service"      23 minutes ago   Up 23 minutes (healthy)     0.0.0.0:3002->3002/tcp                                                                                        product-service                                                                                       product-service
```

</details>

![1](/Final-Project/termproject-images/1.png)

---

**PS C:\Users\lemon\Desktop\Winter 2024\CSP451\aks-store-demo> docker images**

<details>

<summary>docker images</summary>

```
REPOSITORY                       TAG                         IMAGE ID       CREATED          SIZE
aks-store-demo-store-front       latest                      80b08a1382d2   29 minutes ago   45.8MB
aks-store-demo-product-service   latest                      595587bdf020   29 minutes ago   133MB
aks-store-demo-order-service     latest                      69e40e9c8360   33 minutes ago   193MB
rabbitmq                         3.11.17-management-alpine   79a570297657   12 months ago    179MB
```

</details>

![2](/Final-Project/termproject-images/2.png)

---

**PS C:\Users\lemon\Desktop\Winter 2024\CSP451\aks-store-demo> docker ps**


<details>

<summary>docker ps</summary>

```
CONTAINER ID   IMAGE                                COMMAND                  CREATED          STATUS                      PORTS                                                                                                         NAMES
f27a8d758fc1   aks-store-demo-store-front           "/docker-entrypoint.…"   34 minutes ago   Up 33 minutes (unhealthy)   80/tcp, 0.0.0.0:8080->8080/tcp                                                                                store-front
97ce857fb4f6   aks-store-demo-order-service         "docker-entrypoint.s…"   34 minutes ago   Up 33 minutes (healthy)     0.0.0.0:3000->3000/tcp                                                                                        order-service
5a08f3665187   rabbitmq:3.11.17-management-alpine   "docker-entrypoint.s…"   34 minutes ago   Up 34 minutes (healthy)     4369/tcp, 5671/tcp, 0.0.0.0:5672->5672/tcp, 15671/tcp, 15691-15692/tcp, 25672/tcp, 0.0.0.0:15672->15672/tcp   rabbitmq
033d488028e8   aks-store-demo-product-service       "./product-service"      34 minutes ago   Up 34 minutes (healthy)     0.0.0.0:3002->3002/tcp                                                                                        product-service   
```

</details>

![3](/Final-Project/termproject-images/3.png) 

---

![4](/Final-Project/termproject-images/4.png)

---


## Create container registry

**az acr create --resource-group Student-RG-1202818 --name tle53acr --sku Basic**

![5](/Final-Project/termproject-images/5.png)

<details>
<summary>az acr create --resource-group Student-RG-1202818 --name tle53acr --sku Basic</summary>

```
{
  "adminUserEnabled": false,
  "anonymousPullEnabled": false,
  "creationDate": "2024-04-16T07:33:15.408852+00:00",
  "dataEndpointEnabled": false,
  "dataEndpointHostNames": [],
  "encryption": {
    "keyVaultProperties": null,
    "status": "disabled"
  },
  "id": "/subscriptions/71d310bf-1718-4d11-87d1-99a7d4e2053f/resourceGroups/Student-RG-1202818/providers/Microsoft.ContainerRegistry/registries/tle53acr",
  "identity": null,
  "location": "canadacentral",
  "loginServer": "tle53acr.azurecr.io",
  "metadataSearch": "Disabled",
  "name": "tle53acr",
  "networkRuleBypassOptions": "AzureServices",
  "networkRuleSet": null,
  "policies": {
    "azureAdAuthenticationAsArmPolicy": {
      "status": "enabled"
    },
    "exportPolicy": {
      "status": "enabled"
    },
    "quarantinePolicy": {
      "status": "disabled"
    },
    "retentionPolicy": {
      "days": 7,
      "lastUpdatedTime": "2024-04-16T08:07:12.855911+00:00",
      "status": "disabled"
    },
    "softDeletePolicy": {
      "lastUpdatedTime": "2024-04-16T08:07:12.855972+00:00",
      "retentionDays": 7,
      "status": "disabled"
    },
    "trustPolicy": {
      "status": "disabled",
      "type": "Notary"
    }
  },
  "privateEndpointConnections": [],
  "provisioningState": "Succeeded",
  "publicNetworkAccess": "Enabled",
  "resourceGroup": "Student-RG-1202818",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "status": null,
  "systemData": {
    "createdAt": "2024-04-16T07:33:15.408852+00:00",
    "createdBy": "odl_user_1202818@seneca-csp451nia.cloudlabs.ai",
    "createdByType": "User",
    "lastModifiedAt": "2024-04-16T08:07:12.697854+00:00",
    "lastModifiedBy": "odl_user_1202818@seneca-csp451nia.cloudlabs.ai",
    "lastModifiedByType": "User"
  },
  "tags": {
    "DeploymentId": "1202818",
    "LaunchId": "38011",
    "LaunchType": "ON_DEMAND_LAB",
    "TemplateId": "7633",
    "TenantId": "353"
  },
  "type": "Microsoft.ContainerRegistry/registries",
  "zoneRedundancy": "Disabled"
}
```

</details>

---

**PS C:\Users\lemon\Desktop\Winter 2024\CSP451\aks-store-demo> az acr repository list --name tle53acr --output table**

<details>

<summary>az acr repository list --name tle53acr --output table</summary>

```
Result
------------------------------
aks-store-demo/order-service
aks-store-demo/product-service
aks-store-demo/store-front
```

</details>

![6](/Final-Project/termproject-images/6.png)

---

## Create Kubernetes cluster

**PS C:\Users\lemon\Desktop\Winter 2024\CSP451\aks-store-demo> kubectl get nodes**

<details>

<summary>kubectl get nodes</summary>

```
NAME                                STATUS   ROLES   AGE   VERSION
aks-nodepool1-40236670-vmss000000   Ready    agent   22m   v1.28.5
aks-nodepool1-40236670-vmss000001   Ready    agent   17m   v1.28.5
```

</details>

![7](/Final-Project/termproject-images/7.png)

## Deploy containerized application

**PS C:\Users\lemon\Desktop\Winter 2024\CSP451\aks-store-demo> kubectl get pods**

![8](/Final-Project/termproject-images/8.png)

---


**PS C:\Users\lemon\Desktop\Winter 2024\CSP451\aks-store-demo> kubectl get service store-front --watch**

<details>
<summary>kubectl get service store-front --watch</summary>

```
NAME          TYPE           CLUSTER-IP   EXTERNAL-IP      PORT(S)        AGE
store-front   LoadBalancer   10.0.3.9     20.175.207.146   80:31570/TCP   10m
```

</details>

![9](/Final-Project/termproject-images/9.png)

---

![10](/Final-Project/termproject-images/10.png)

![11](/Final-Project/termproject-images/11.png)

## Scale application

**PS C:\Users\lemon\Desktop\Winter 2024\CSP451\aks-store-demo> kubectl get nodes**

<details>
<summary>kubectl get nodes</summary>

```
NAME                                STATUS   ROLES   AGE   VERSION
aks-nodepool1-40236670-vmss000000   Ready    agent   64m   v1.28.5
aks-nodepool1-40236670-vmss000001   Ready    agent   59m   v1.28.5
```

</details>

---

**PS C:\Users\lemon\Desktop\Winter 2024\CSP451\aks-store-demo> az aks scale --resource-group Student-RG-1202818 --name myAKSCluster --node-count 3**

<details>

<summary>az aks scale --resource-group Student-RG-1202818 --name myAKSCluster --node-count 3</summary>

```
{
  "aadProfile": null,
  "addonProfiles": null,
  "agentPoolProfiles": [
    {
      "availabilityZones": null,
      "capacityReservationGroupId": null,
      "count": 3,
      "creationData": null,
      "currentOrchestratorVersion": "1.28.5",
      "enableAutoScaling": false,
      "enableEncryptionAtHost": false,
      "enableFips": false,
      "enableNodePublicIp": false,
      "enableUltraSsd": false,
      "gpuInstanceProfile": null,
      "hostGroupId": null,
      "kubeletConfig": null,
      "kubeletDiskType": "OS",
      "linuxOsConfig": null,
      "maxCount": null,
      "maxPods": 110,
      "minCount": null,
      "mode": "System",
      "name": "nodepool1",
      "networkProfile": null,
      "nodeImageVersion": "AKSUbuntu-2204gen2containerd-202404.01.0",
      "nodeLabels": null,
      "nodePublicIpPrefixId": null,
      "nodeTaints": null,
      "orchestratorVersion": "1.28.5",
      "osDiskSizeGb": 128,
      "osDiskType": "Managed",
      "osSku": "Ubuntu",
      "osType": "Linux",
      "podSubnetId": null,
      "powerState": {
        "code": "Running"
      },
      "provisioningState": "Succeeded",
      "proximityPlacementGroupId": null,
      "scaleDownMode": null,
      "scaleSetEvictionPolicy": null,
      "scaleSetPriority": null,
      "spotMaxPrice": null,
      "tags": null,
      "type": "VirtualMachineScaleSets",
      "upgradeSettings": {
        "drainTimeoutInMinutes": null,
        "maxSurge": "10%",
        "nodeSoakDurationInMinutes": null
      },
      "vmSize": "Standard_DS2_v2",
      "vnetSubnetId": null,
      "workloadRuntime": null
    }
  ],
  "apiServerAccessProfile": null,
  "autoScalerProfile": null,
  "autoUpgradeProfile": {
    "nodeOsUpgradeChannel": "NodeImage",
    "upgradeChannel": null
  },
  "azureMonitorProfile": null,
  "azurePortalFqdn": "myaksclust-student-rg-12028-71d310-xmzn8nux.portal.hcp.canadacentral.azmk8s.io",
  "currentKubernetesVersion": "1.28.5",
  "disableLocalAccounts": false,
  "diskEncryptionSetId": null,
  "dnsPrefix": "myAKSClust-Student-RG-12028-71d310",
  "enablePodSecurityPolicy": null,
  "enableRbac": true,
  "extendedLocation": null,
  "fqdn": "myaksclust-student-rg-12028-71d310-xmzn8nux.hcp.canadacentral.azmk8s.io",
  "fqdnSubdomain": null,
  "httpProxyConfig": null,
  "id": "/subscriptions/71d310bf-1718-4d11-87d1-99a7d4e2053f/resourcegroups/Student-RG-1202818/providers/Microsoft.ContainerService/managedClusters/myAKSCluster",
  "identity": {
    "delegatedResources": null,
    "principalId": "020efac4-f6fe-4227-b892-a0853b64fd3b",
    "tenantId": "ed27b597-cea0-4942-8c6f-40e6a78bf47d",
    "type": "SystemAssigned",
    "userAssignedIdentities": null
  },
  "identityProfile": {
    "kubeletidentity": {
      "clientId": "b20dc99f-05e5-4929-a14d-3aeb3f100372",
      "objectId": "06f08594-a218-49e1-a7cb-369f21e75e7d",
      "resourceId": "/subscriptions/71d310bf-1718-4d11-87d1-99a7d4e2053f/resourcegroups/MC_Student-RG-1202818_myAKSCluster_canadacentral/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myAKSCluster-agentpool"    
    }
  },
  "kubernetesVersion": "1.28",
  "linuxProfile": {
    "adminUsername": "azureuser",
    "ssh": {
      "publicKeys": [
        {
          "keyData": "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCxHkkjhN1QxpRZORfM4T2acy/chUJFSVYUIDLWQwUv81kar9ZklDbkAPVn/p2uFwXo86Nu2nIflu+3noa8ra6HQsiVteEax/J1oBrFl7NRyXiiKX4i2GQJiHszg4w7+aS/liSIkCsZRgOtQrK+JWU7PV2ASXX02mL9UjlyDh4x0IEVpzxlKQACGBxWeRJk7Qzd0xZHNDFL3LYGZyoCcTquyjDLwfd/XfyyFehOQ3AyiWZFpfHddqklAwPWf6sMYV7cGy8iPqtSO4StohXmJ2tLtid8A1+d54pRCl13Kz+3cwILarWy0jtslRaQ4oR28HPaqHbHuB7991ICkAD9u20r"
        }
      ]
    }
  },
  "location": "canadacentral",
  "maxAgentPools": 100,
  "name": "myAKSCluster",
  "networkProfile": {
    "dnsServiceIp": "10.0.0.10",
    "ipFamilies": [
      "IPv4"
    ],
    "loadBalancerProfile": {
      "allocatedOutboundPorts": null,
      "backendPoolType": "nodeIPConfiguration",
      "effectiveOutboundIPs": [
        {
          "id": "/subscriptions/71d310bf-1718-4d11-87d1-99a7d4e2053f/resourceGroups/MC_Student-RG-1202818_myAKSCluster_canadacentral/providers/Microsoft.Network/publicIPAddresses/6dba39e2-d7ca-41d0-928f-bcdbc1b28372",      
          "resourceGroup": "MC_Student-RG-1202818_myAKSCluster_canadacentral"
        }
      ],
      "enableMultipleStandardLoadBalancers": null,
      "idleTimeoutInMinutes": null,
      "managedOutboundIPs": {
        "count": 1,
        "countIpv6": null
      },
      "outboundIPs": null,
      "outboundIpPrefixes": null
    },
    "loadBalancerSku": "standard",
    "natGatewayProfile": null,
    "networkDataplane": null,
    "networkMode": null,
    "networkPlugin": "kubenet",
    "networkPluginMode": null,
    "networkPolicy": null,
    "outboundType": "loadBalancer",
    "podCidr": "10.244.0.0/16",
    "podCidrs": [
      "10.244.0.0/16"
    ],
    "serviceCidr": "10.0.0.0/16",
    "serviceCidrs": [
      "10.0.0.0/16"
    ]
  },
  "nodeResourceGroup": "MC_Student-RG-1202818_myAKSCluster_canadacentral",
  "oidcIssuerProfile": {
    "enabled": false,
    "issuerUrl": null
  },
  "podIdentityProfile": null,
  "powerState": {
    "code": "Running"
  },
  "privateFqdn": null,
  "privateLinkResources": null,
  "provisioningState": "Succeeded",
  "publicNetworkAccess": null,
  "resourceGroup": "Student-RG-1202818",
  "resourceUid": "661e39b8f15ceb0001842359",
  "securityProfile": {
    "azureKeyVaultKms": null,
    "defender": null,
    "imageCleaner": null,
    "workloadIdentity": null
  },
  "serviceMeshProfile": null,
  "servicePrincipalProfile": {
    "clientId": "msi",
    "secret": null
  },
  "sku": {
    "name": "Base",
    "tier": "Free"
  },
  "storageProfile": {
    "blobCsiDriver": null,
    "diskCsiDriver": {
      "enabled": true
    },
    "fileCsiDriver": {
      "enabled": true
    },
    "snapshotController": {
      "enabled": true
    }
  },
  "supportPlan": "KubernetesOfficial",
  "systemData": null,
  "tags": {
    "DeploymentId": "1202818",
    "LaunchId": "38011",
    "LaunchType": "ON_DEMAND_LAB",
    "TemplateId": "7633",
    "TenantId": "353"
  },
  "type": "Microsoft.ContainerService/ManagedClusters",
  "upgradeSettings": null,
  "windowsProfile": null,
  "workloadAutoScalerProfile": {
    "keda": null,
    "verticalPodAutoscaler": null
  }
}
```

</details>

---

**PS C:\Users\lemon\Desktop\Winter 2024\CSP451\aks-store-demo> kubectl get nodes**

<details>
<summary>kubectl get nodes</summary>

```
NAME                                STATUS   ROLES   AGE    VERSION
aks-nodepool1-40236670-vmss000000   Ready    agent   70m    v1.28.5
aks-nodepool1-40236670-vmss000001   Ready    agent   65m    v1.28.5
aks-nodepool1-40236670-vmss000002   Ready    agent   2m3s   v1.28.5
```

</details>

![12](/Final-Project/termproject-images/12.png)

---

**PS C:\Users\lemon\Desktop\Winter 2024\CSP451\aks-store-demo> az aks scale --resource-group Student-RG-1202818 --name myAKSCluster --node-count 2**

<details>
<summary>az aks scale --resource-group Student-RG-1202818 --name myAKSCluster --node-count 2</summary>

```
{
  "aadProfile": null,
  "addonProfiles": null,
  "agentPoolProfiles": [
    {
      "availabilityZones": null,
      "capacityReservationGroupId": null,
      "count": 2,
      "creationData": null,
      "currentOrchestratorVersion": "1.28.5",
      "enableAutoScaling": false,
      "enableEncryptionAtHost": false,
      "enableFips": false,
      "enableNodePublicIp": false,
      "enableUltraSsd": false,
      "gpuInstanceProfile": null,
      "hostGroupId": null,
      "kubeletConfig": null,
      "kubeletDiskType": "OS",
      "linuxOsConfig": null,
      "maxCount": null,
      "maxPods": 110,
      "minCount": null,
      "mode": "System",
      "name": "nodepool1",
      "networkProfile": null,
      "nodeImageVersion": "AKSUbuntu-2204gen2containerd-202404.01.0",
      "nodeLabels": null,
      "nodePublicIpPrefixId": null,
      "nodeTaints": null,
      "orchestratorVersion": "1.28.5",
      "osDiskSizeGb": 128,
      "osDiskType": "Managed",
      "osSku": "Ubuntu",
      "osType": "Linux",
      "podSubnetId": null,
      "powerState": {
        "code": "Running"
      },
      "provisioningState": "Succeeded",
      "proximityPlacementGroupId": null,
      "scaleDownMode": null,
      "scaleSetEvictionPolicy": null,
      "scaleSetPriority": null,
      "spotMaxPrice": null,
      "tags": null,
      "type": "VirtualMachineScaleSets",
      "upgradeSettings": {
        "drainTimeoutInMinutes": null,
        "maxSurge": "10%",
        "nodeSoakDurationInMinutes": null
      },
      "vmSize": "Standard_DS2_v2",
      "vnetSubnetId": null,
      "workloadRuntime": null
    }
  ],
  "apiServerAccessProfile": null,
  "autoScalerProfile": null,
  "autoUpgradeProfile": {
    "nodeOsUpgradeChannel": "NodeImage",
    "upgradeChannel": null
  },
  "azureMonitorProfile": null,
  "azurePortalFqdn": "myaksclust-student-rg-12028-71d310-xmzn8nux.portal.hcp.canadacentral.azmk8s.io",
  "currentKubernetesVersion": "1.28.5",
  "disableLocalAccounts": false,
  "diskEncryptionSetId": null,
  "dnsPrefix": "myAKSClust-Student-RG-12028-71d310",
  "enablePodSecurityPolicy": null,
  "enableRbac": true,
  "extendedLocation": null,
  "fqdn": "myaksclust-student-rg-12028-71d310-xmzn8nux.hcp.canadacentral.azmk8s.io",
  "fqdnSubdomain": null,
  "httpProxyConfig": null,
  "id": "/subscriptions/71d310bf-1718-4d11-87d1-99a7d4e2053f/resourcegroups/Student-RG-1202818/providers/Microsoft.ContainerService/managedClusters/myAKSCluster",
  "identity": {
    "delegatedResources": null,
    "principalId": "020efac4-f6fe-4227-b892-a0853b64fd3b",
    "tenantId": "ed27b597-cea0-4942-8c6f-40e6a78bf47d",
    "type": "SystemAssigned",
    "userAssignedIdentities": null
  },
  "identityProfile": {
    "kubeletidentity": {
      "clientId": "b20dc99f-05e5-4929-a14d-3aeb3f100372",
      "objectId": "06f08594-a218-49e1-a7cb-369f21e75e7d",
      "resourceId": "/subscriptions/71d310bf-1718-4d11-87d1-99a7d4e2053f/resourcegroups/MC_Student-RG-1202818_myAKSCluster_canadacentral/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myAKSCluster-agentpool"    
    }
  },
  "kubernetesVersion": "1.28",
  "linuxProfile": {
    "adminUsername": "azureuser",
    "ssh": {
      "publicKeys": [
        {
          "keyData": "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCxHkkjhN1QxpRZORfM4T2acy/chUJFSVYUIDLWQwUv81kar9ZklDbkAPVn/p2uFwXo86Nu2nIflu+3noa8ra6HQsiVteEax/J1oBrFl7NRyXiiKX4i2GQJiHszg4w7+aS/liSIkCsZRgOtQrK+JWU7PV2ASXX02mL9UjlyDh4x0IEVpzxlKQACGBxWeRJk7Qzd0xZHNDFL3LYGZyoCcTquyjDLwfd/XfyyFehOQ3AyiWZFpfHddqklAwPWf6sMYV7cGy8iPqtSO4StohXmJ2tLtid8A1+d54pRCl13Kz+3cwILarWy0jtslRaQ4oR28HPaqHbHuB7991ICkAD9u20r"
        }
      ]
    }
  },
  "location": "canadacentral",
  "maxAgentPools": 100,
  "name": "myAKSCluster",
  "networkProfile": {
    "dnsServiceIp": "10.0.0.10",
    "ipFamilies": [
      "IPv4"
    ],
    "loadBalancerProfile": {
      "allocatedOutboundPorts": null,
      "backendPoolType": "nodeIPConfiguration",
      "effectiveOutboundIPs": [
        {
          "id": "/subscriptions/71d310bf-1718-4d11-87d1-99a7d4e2053f/resourceGroups/MC_Student-RG-1202818_myAKSCluster_canadacentral/providers/Microsoft.Network/publicIPAddresses/6dba39e2-d7ca-41d0-928f-bcdbc1b28372",      
          "resourceGroup": "MC_Student-RG-1202818_myAKSCluster_canadacentral"
        }
      ],
      "enableMultipleStandardLoadBalancers": null,
      "idleTimeoutInMinutes": null,
      "managedOutboundIPs": {
        "count": 1,
        "countIpv6": null
      },
      "outboundIPs": null,
      "outboundIpPrefixes": null
    },
    "loadBalancerSku": "standard",
    "natGatewayProfile": null,
    "networkDataplane": null,
    "networkMode": null,
    "networkPlugin": "kubenet",
    "networkPluginMode": null,
    "networkPolicy": null,
    "outboundType": "loadBalancer",
    "podCidr": "10.244.0.0/16",
    "podCidrs": [
      "10.244.0.0/16"
    ],
    "serviceCidr": "10.0.0.0/16",
    "serviceCidrs": [
      "10.0.0.0/16"
    ]
  },
  "nodeResourceGroup": "MC_Student-RG-1202818_myAKSCluster_canadacentral",
  "oidcIssuerProfile": {
    "enabled": false,
    "issuerUrl": null
  },
  "podIdentityProfile": null,
  "powerState": {
    "code": "Running"
  },
  "privateFqdn": null,
  "privateLinkResources": null,
  "provisioningState": "Succeeded",
  "publicNetworkAccess": null,
  "resourceGroup": "Student-RG-1202818",
  "resourceUid": "661e39b8f15ceb0001842359",
  "securityProfile": {
    "azureKeyVaultKms": null,
    "defender": null,
    "imageCleaner": null,
    "workloadIdentity": null
  },
  "serviceMeshProfile": null,
  "servicePrincipalProfile": {
    "clientId": "msi",
    "secret": null
  },
  "sku": {
    "name": "Base",
    "tier": "Free"
  },
  "storageProfile": {
    "blobCsiDriver": null,
    "diskCsiDriver": {
      "enabled": true
    },
    "fileCsiDriver": {
      "enabled": true
    },
    "snapshotController": {
      "enabled": true
    }
  },
  "supportPlan": "KubernetesOfficial",
  "systemData": null,
  "tags": {
    "DeploymentId": "1202818",
    "LaunchId": "38011",
    "LaunchType": "ON_DEMAND_LAB",
    "TemplateId": "7633",
    "TenantId": "353"
  },
  "type": "Microsoft.ContainerService/ManagedClusters",
  "upgradeSettings": null,
  "windowsProfile": null,
  "workloadAutoScalerProfile": {
    "keda": null,
    "verticalPodAutoscaler": null
  }
}
```
</details>

---

**PS C:\Users\lemon\Desktop\Winter 2024\CSP451\aks-store-demo> kubectl get nodes**

<details>
<summary>kubectl get nodes</summary>

```
NAME                                STATUS   ROLES   AGE   VERSION
aks-nodepool1-40236670-vmss000000   Ready    agent   77m   v1.28.5
aks-nodepool1-40236670-vmss000001   Ready    agent   71m   v1.28.5
```

</details>

![13](/Final-Project/termproject-images/13.png)

## Upgrade cluster

**PS C:\Users\lemon\Desktop\Winter 2024\CSP451\aks-store-demo> az aks get-upgrades --resource-group Student-RG-1202818 --name myAKSCluster**

<details>
<summary>az aks get-upgrades --resource-group Student-RG-1202818 --name myAKSCluster</summary>

```
{
  "agentPoolProfiles": null,
  "controlPlaneProfile": {
    "kubernetesVersion": "1.28.5",
    "name": null,
    "osType": "Linux",
    "upgrades": [
      {
        "isPreview": null,
        "kubernetesVersion": "1.29.2"
      },
      {
        "isPreview": null,
        "kubernetesVersion": "1.29.0"
      }
    ]
  },
  "id": "/subscriptions/71d310bf-1718-4d11-87d1-99a7d4e2053f/resourcegroups/Student-RG-1202818/providers/Microsoft.ContainerService/managedClusters/myAKSCluster/upgradeprofiles/default",
  "name": "default",
  "resourceGroup": "Student-RG-1202818",
  "type": "Microsoft.ContainerService/managedClusters/upgradeprofiles"
}
```

</details>

---

**PS C:\Users\lemon\Desktop\Winter 2024\CSP451\aks-store-demo> az aks upgrade --resource-group Student-RG-1202818 --name myAKSCluster --kubernetes-version 1.29.2**

<details>
<summary>az aks upgrade --resource-group Student-RG-1202818 --name myAKSCluster --kubernetes-version 1.29.2</summary>

```
Kubernetes may be unavailable during cluster upgrades.
 Are you sure you want to perform this operation? (y/N): y
Since control-plane-only argument is not specified, this will upgrade the control plane AND all nodepools to version 1.29.2. Continue? (y/N): y

{
  "aadProfile": null,
  "addonProfiles": null,
  "agentPoolProfiles": [
    {
      "availabilityZones": null,
      "capacityReservationGroupId": null,
      "count": 2,
      "creationData": null,
      "currentOrchestratorVersion": "1.29.2",
      "enableAutoScaling": false,
      "enableEncryptionAtHost": false,
      "enableFips": false,
      "enableNodePublicIp": false,
      "enableUltraSsd": false,
      "gpuInstanceProfile": null,
      "hostGroupId": null,
      "kubeletConfig": null,
      "kubeletDiskType": "OS",
      "linuxOsConfig": null,
      "maxCount": null,
      "maxPods": 110,
      "minCount": null,
      "mode": "System",
      "name": "nodepool1",
      "networkProfile": null,
      "nodeImageVersion": "AKSUbuntu-2204gen2containerd-202404.01.0",
      "nodeLabels": null,
      "nodePublicIpPrefixId": null,
      "nodeTaints": null,
      "orchestratorVersion": "1.29.2",
      "osDiskSizeGb": 128,
      "osDiskType": "Managed",
      "osSku": "Ubuntu",
      "osType": "Linux",
      "podSubnetId": null,
      "powerState": {
        "code": "Running"
      },
      "provisioningState": "Succeeded",
      "proximityPlacementGroupId": null,
      "scaleDownMode": null,
      "scaleSetEvictionPolicy": null,
      "scaleSetPriority": null,
      "spotMaxPrice": null,
      "tags": null,
      "type": "VirtualMachineScaleSets",
      "upgradeSettings": {
        "drainTimeoutInMinutes": null,
        "maxSurge": "10%",
        "nodeSoakDurationInMinutes": null
      },
      "vmSize": "Standard_DS2_v2",
      "vnetSubnetId": null,
      "workloadRuntime": null
    }
  ],
  "apiServerAccessProfile": null,
  "autoScalerProfile": null,
  "autoUpgradeProfile": {
    "nodeOsUpgradeChannel": "NodeImage",
    "upgradeChannel": "patch"
  },
  "azureMonitorProfile": null,
  "azurePortalFqdn": "myaksclust-student-rg-12028-71d310-xmzn8nux.portal.hcp.canadacentral.azmk8s.io",
  "currentKubernetesVersion": "1.29.2",
  "disableLocalAccounts": false,
  "diskEncryptionSetId": null,
  "dnsPrefix": "myAKSClust-Student-RG-12028-71d310",
  "enablePodSecurityPolicy": null,
  "enableRbac": true,
  "extendedLocation": null,
  "fqdn": "myaksclust-student-rg-12028-71d310-xmzn8nux.hcp.canadacentral.azmk8s.io",
  "fqdnSubdomain": null,
  "httpProxyConfig": null,
  "id": "/subscriptions/71d310bf-1718-4d11-87d1-99a7d4e2053f/resourcegroups/Student-RG-1202818/providers/Microsoft.ContainerService/managedClusters/myAKSCluster",
  "identity": {
    "delegatedResources": null,
    "principalId": "020efac4-f6fe-4227-b892-a0853b64fd3b",
    "tenantId": "ed27b597-cea0-4942-8c6f-40e6a78bf47d",
    "type": "SystemAssigned",
    "userAssignedIdentities": null
  },
  "identityProfile": {
    "kubeletidentity": {
      "clientId": "b20dc99f-05e5-4929-a14d-3aeb3f100372",
      "objectId": "06f08594-a218-49e1-a7cb-369f21e75e7d",
      "resourceId": "/subscriptions/71d310bf-1718-4d11-87d1-99a7d4e2053f/resourcegroups/MC_Student-RG-1202818_myAKSCluster_canadacentral/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myAKSCluster-agentpool"    
    }
  },
  "kubernetesVersion": "1.29.2",
  "linuxProfile": {
    "adminUsername": "azureuser",
    "ssh": {
      "publicKeys": [
        {
          "keyData": "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCxHkkjhN1QxpRZORfM4T2acy/chUJFSVYUIDLWQwUv81kar9ZklDbkAPVn/p2uFwXo86Nu2nIflu+3noa8ra6HQsiVteEax/J1oBrFl7NRyXiiKX4i2GQJiHszg4w7+aS/liSIkCsZRgOtQrK+JWU7PV2ASXX02mL9UjlyDh4x0IEVpzxlKQACGBxWeRJk7Qzd0xZHNDFL3LYGZyoCcTquyjDLwfd/XfyyFehOQ3AyiWZFpfHddqklAwPWf6sMYV7cGy8iPqtSO4StohXmJ2tLtid8A1+d54pRCl13Kz+3cwILarWy0jtslRaQ4oR28HPaqHbHuB7991ICkAD9u20r"
        }
      ]
    }
  },
  "location": "canadacentral",
  "maxAgentPools": 100,
  "name": "myAKSCluster",
  "networkProfile": {
    "dnsServiceIp": "10.0.0.10",
    "ipFamilies": [
      "IPv4"
    ],
    "loadBalancerProfile": {
      "allocatedOutboundPorts": null,
      "backendPoolType": "nodeIPConfiguration",
      "effectiveOutboundIPs": [
        {
          "id": "/subscriptions/71d310bf-1718-4d11-87d1-99a7d4e2053f/resourceGroups/MC_Student-RG-1202818_myAKSCluster_canadacentral/providers/Microsoft.Network/publicIPAddresses/6dba39e2-d7ca-41d0-928f-bcdbc1b28372",      
          "resourceGroup": "MC_Student-RG-1202818_myAKSCluster_canadacentral"
        }
      ],
      "enableMultipleStandardLoadBalancers": null,
      "idleTimeoutInMinutes": null,
      "managedOutboundIPs": {
        "count": 1,
        "countIpv6": null
      },
      "outboundIPs": null,
      "outboundIpPrefixes": null
    },
    "loadBalancerSku": "standard",
    "natGatewayProfile": null,
    "networkDataplane": null,
    "networkMode": null,
    "networkPlugin": "kubenet",
    "networkPluginMode": null,
    "networkPolicy": null,
    "outboundType": "loadBalancer",
    "podCidr": "10.244.0.0/16",
    "podCidrs": [
      "10.244.0.0/16"
    ],
    "serviceCidr": "10.0.0.0/16",
    "serviceCidrs": [
      "10.0.0.0/16"
    ]
  },
  "nodeResourceGroup": "MC_Student-RG-1202818_myAKSCluster_canadacentral",
  "oidcIssuerProfile": {
    "enabled": false,
    "issuerUrl": null
  },
  "podIdentityProfile": null,
  "powerState": {
    "code": "Running"
  },
  "privateFqdn": null,
  "privateLinkResources": null,
  "provisioningState": "Succeeded",
  "publicNetworkAccess": null,
  "resourceGroup": "Student-RG-1202818",
  "resourceUid": "661e39b8f15ceb0001842359",
  "securityProfile": {
    "azureKeyVaultKms": null,
    "defender": null,
    "imageCleaner": null,
    "workloadIdentity": null
  },
  "serviceMeshProfile": null,
  "servicePrincipalProfile": {
    "clientId": "msi",
    "secret": null
  },
  "sku": {
    "name": "Base",
    "tier": "Free"
  },
  "storageProfile": {
    "blobCsiDriver": null,
    "diskCsiDriver": {
      "enabled": true
    },
    "fileCsiDriver": {
      "enabled": true
    },
    "snapshotController": {
      "enabled": true
    }
  },
  "supportPlan": "KubernetesOfficial",
  "systemData": null,
  "tags": {
    "DeploymentId": "1202818",
    "LaunchId": "38011",
    "LaunchType": "ON_DEMAND_LAB",
    "TemplateId": "7633",
    "TenantId": "353"
  },
  "type": "Microsoft.ContainerService/ManagedClusters",
  "upgradeSettings": null,
  "windowsProfile": null,
  "workloadAutoScalerProfile": {
    "keda": null,
    "verticalPodAutoscaler": null
  }
}


```

</details>

---

**PS C:\Users\lemon\Desktop\Winter 2024\CSP451\aks-store-demo> az aks show --resource-group Student-RG-1202818 --name myAKSCluster --output table**

<details>
<summary>az aks show --resource-group Student-RG-1202818 --name myAKSCluster --output table</summary>
</details>