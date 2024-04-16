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
6. [Scale application](#header)
7. [Upgrade cluster](#header)

----

## Prepare application for AKS

**PS C:\Users\lemon\Desktop\Winter 2024\CSP451\aks-store-demo> docker container list**

1.

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

---

**PS C:\Users\lemon\Desktop\Winter 2024\CSP451\aks-store-demo> docker images**

2.

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

---

**PS C:\Users\lemon\Desktop\Winter 2024\CSP451\aks-store-demo> docker ps**

3. 

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



4.

---


## Create container registry

**az acr create --resource-group Student-RG-1202818 --name tle53acr --sku Basic**

5.

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

6.

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

7.

## Deploy containerized application

**PS C:\Users\lemon\Desktop\Winter 2024\CSP451\aks-store-demo> kubectl get pods**

8.

---


**PS C:\Users\lemon\Desktop\Winter 2024\CSP451\aks-store-demo> kubectl get service store-front --watch**

<details>
<summary>kubectl get service store-front --watch</summary>

```
NAME          TYPE           CLUSTER-IP   EXTERNAL-IP      PORT(S)        AGE
store-front   LoadBalancer   10.0.3.9     20.175.207.146   80:31570/TCP   10m
```

</details>

9.

---

10.

11.