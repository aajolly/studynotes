This section walks through the process of creating an Azure Kubernetes Service (AKS) cluster using Azure CLI. I have used Azure Cloud Shell for all the commands listed below.

1. Create a resource group to house everything related to AKS
```
az group create --name aajolly-aks-rg --location australiaeast
```