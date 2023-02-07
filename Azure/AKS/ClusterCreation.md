# Overview
This section walks through the process of creating an Azure Kubernetes Service (AKS) cluster using Azure CLI. I have used Azure Cloud Shell for all the commands listed below.

1. Create a resource group to house everything related to AKS
```bash
az group create --name aajolly-aks-rg --location australiaeast
```

2. Create a service principal for AKS cluster to interact with various Azure services. Think of eksClusterRole
```bash
az ad sp create-for-rbac --skip-assignment
```

3. Create an Azure Container Registry (ACR) for storing container images.
```bash
az acr create --resource-group aajolly-aks-rg --name aajollyacr02022023 --sku Basic --admin-enabled true
```

**Note:** The name of the registry must be globally unique. For the service tier I'm using basic, for more on service tiers, see [here](https://learn.microsoft.com/en-us/azure/container-registry/container-registry-skus)

4. Assign an role (least priviledge principal) to the service principal we created in earlier
```bash
az role assignment create `
--assignee <appID of Service Principal created in Step2> `
--role acrpull ` --> To pull images from ACR
--scope <ACR ID created in Step3>
```

5. I use a demo container app for testing, clone the repo
```bash
git clone https://github.com/aajolly/nyan-cat.git
cd nyan-cat
```

6. Use ACR tasks to build & push the container image to ACR without installing docker
```bash
az acr build `
 --image demo/nyancat:v1 `
 --registry aajollyacr02022023 `
 --file Dockerfile .
```

7. Register the following resource provider if not already registered
Microsoft.OperationsManagement
```powershell
Register-AzResourceProvider -ProviderNamespace Microsoft.OperationsManagement
```

8. Create AKS Cluster. If the network plugin is not specified, the default is Kubenet
```bash
az aks create `
 --resource-group aajolly-aks-rg `
 --name aajollyAKSCluster `
 --node-count 1 `
 --enable-addons monitoring `
 --generate-ssh-keys `
 --service-principal "<appID of Service Principal created in step2>" `
 --client-secret "<password of Service Principal created in step3>"
 ```

 Below are some commands that can be used for creating an AKS cluster with more details
 
 ```bash
 az network vnet create `
 --name aajolly-aks-vnet `
 --resource-group aajolly-aks-azcni-rg `
 --address-prefixes 10.0.0.0/23 100.64.0.0/16
 ```
 ```bash
 az network vnet subnet create `    
 --name aajolly-aks-subnet1 `
 --resource-group aajolly-aks-azcni-rg ` 
 --vnet-name aajolly-aks-vnet `
 --address-prefixes 100.64.0.0/20
 ```
 ```bash
 az aks create `
 --resource-group aajolly-aks-rg2 `
 --name kubenetCluster `
 --node-count 3 `
 --network-plugin kubenet `
 --service-cidr 172.0.0.0/16 `
 --dns-service-ip 172.0.0.10 `
 --pod-cidr 100.64.0.0/20 `
 --docker-bridge-address 172.99.0.1/16 `
 --vnet-subnet-id <subnetID for a subnet> `
 --service-principal "<appID of Service Principal created in step2>" `
 --client-secret "<password of Service Principal created in step3>"
 ```

 **Note:** Please note that `service-cidr`, `docker-bridge-address` could be any CIDR blocks. The `dns-service-ip` needs to be from the `services-cidr` block though, hence I've selected 172.0.0.10. 
 The `vnet-subnet-id` is the subnetID from which the nodes are assigned IPs. The `pod-cidr` block needs to be big enough as every node will be assigned a /24 from this block.

 **Note:** With kubenet, only the nodes receive an IP address in the virtual network subnet. Pods can't communicate directly with each other. Instead, User Defined Routing (UDR) and IP forwarding is used for connectivity between pods across nodes. By default, UDRs and IP forwarding configuration is created and maintained by the AKS service, but you have the option to bring your own route table for custom route management.
 Azure supports a maximum of 400 routes in a UDR, so you can't have an AKS cluster larger than 400 nodes. AKS Virtual Nodes and Azure Network Policies aren't supported with kubenet. You can use Calico Network Policies, as they are supported with kubenet.

 9. Configure the CLI to access AKS cluster
 ```bash
 az aks get-credentials `
  --resource-group aajolly-aks-rg `
  --name aajollyAKSCluster
```

10. Create a deployment using the container image uploaded to ACR previously, followed by exposing the service using a load balancer
```bash
kubectl create deployment nodeapp `
 --image=aajollyacr02022023.azurecr.io/demo/nyancat:v1 `
 --replicas=1 `
 --port=80
```
```yaml
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:
    app: nodeapp
  name: nodeapp
spec:
  type: LoadBalancer
  ports:
  - port: 80
    protocol: TCP
    targetPort: 80
  selector:
    app: nodeapp
```

11. Validate the demo app works by browsing the app via the loadbalancer IP shown in `kubectl get svc` command.