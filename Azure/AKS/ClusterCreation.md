# Overview
This section walks through the process of creating an Azure Kubernetes Service (AKS) cluster using Azure CLI. I have used Azure Cloud Shell for all the commands listed below.

1. Create a resource group to house everything related to AKS
```
az group create --name aajolly-aks-rg --location australiaeast
```

2. Create a service principal for AKS cluster to interact with various Azure services. Think of eksClusterRole
```
az ad sp create-for-rbac --skip-assignment
```

3. Create an Azure Container Registry (ACR) for storing container images.
```
az acr create --resource-group aajolly-aks-rg --name aajollyacr02022023 --sku Basic --admin-enabled true
```

**Note:** The name of the registry must be globally unique. For the service tier I'm using basic, for more on service tiers, see [here](https://learn.microsoft.com/en-us/azure/container-registry/container-registry-skus)

4. Assign an role (least priviledge principal) to the service principal we created in earlier
```
az role assignment create `
--assignee <appID of Service Principal created in Step2> `
--role acrpull ` --> To pull images from ACR
--scope <ACR ID created in Step3>
```

5. I use a demo container app for testing, clone the repo
```
git clone https://github.com/aajolly/nyan-cat.git
cd nyan-cat
```

6. Use ACR tasks to build & push the container image to ACR without installing docker
```
az acr build `
 --image demo/nyancat:v1 `
 --registry aajollyacr02022023 `
 --file Dockerfile .
```

7. Register the following resource provider if not already registered
Microsoft.OperationsManagement
```
Register-AzResourceProvider -ProviderNamespace Microsoft.OperationsManagement
```

8. Create AKS Cluster
```
az aks create `
 --resource-group aajolly-aks-rg `
 --name aajollyAKSCluster `
 --node-count 1 `
 --enable-addons monitoring `
 --generate-ssh-keys `
 --service-principal "<appID of Service Principal created in step2" `
 --client-secret "<password of Service Principal created in step3"
 ```

 9. Configure the CLI to access AKS cluster
 ```
 az aks get-credentials `
  --resource-group aajolly-aks-rg `
  --name aajollyAKSCluster
```

10. Create a deployment using the container image uploaded to ACR previously, followed by exposing the service using a load balancer
```
kubectl create deployment nodeapp --image=aajollyacr02022023.azurecr.io/demo/nyancat:v1 --replicas=1 --port=80
```
```
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