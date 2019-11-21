# Leand and Do: Azure Kubernetes Service
Laboratorio AKS

Here are the steps you’ll follow in this tutorial:
1. Provision a Kubernetes cluster
2. Install and configure Helm
3. Deploy the WordPress Helm chart
4. Access the Kubernetes dashboard
5. Log in and start using WordPress


## Step 1: Provision A Kubernetes Cluster
The first step is to provision a new Kubernetes cluster using the Microsoft Azure CLI. Follow these steps:

* Log in to Microsoft Azure:

```bash
az login
```

If you have multiple subscriptions, you can optionally set the subscription you wish to use in the SUBSCRIPTION-NAME placeholder.

```bash
az account set --subscription "SUBSCRIPTION-NAME"
```

* Create a resource group and cluster. The cluster creation process can take up to 20 minutes.

```bash
az group create --name aks-resource-group --location eastus
az aks create --name aks-cluster --resource-group aks-resource-group --node-count 3 --generate-ssh-keys
```

In this example, the resource group is named aks-resource-group and the cluster is named aks-cluster and is provisioned in the eastus location. If you choose different names or a different location, update the following steps to use the correct information.

* Install kubectl, the Kubernetes command-line interface, and configure it with the credentials for the new AKS cluster:

```bash
sudo az aks install-cli
az aks get-credentials --name aks-cluster --resource-group aks-resource-group
```
