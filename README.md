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


## Step 2: Install And Configure Helm And Tiller

Next, install and configure Helm for your operating system and then create the following Kubernetes objects to make Helm work with Role-Based Access Control (RBAC) in AKS:

* ClusterRoleBinding
* ServiceAccount

Follow these instructions:

* To install Helm, run the following commands:

```bash
curl https://raw.githubusercontent.com/kubernetes/helm/master/scripts/get > get_helm.sh
chmod 700 get_helm.sh
./get_helm.sh
```

* To create a ServiceAccount and associate it with the predefined cluster-admin role, use a ClusterRoleBinding, as below:

```bash
kubectl create serviceaccount -n kube-system tiller
kubectl create clusterrolebinding tiller-cluster-rule --clusterrole=cluster-admin --serviceaccount=kube-system:tiller
```

* Initialize Helm as shown below:
```bash
helm init --service-account tiller
```

If you have previously initialized Helm, execute the following command to upgrade it:

```bash
helm init --upgrade --service-account tiller
```

## Step 3: Deploy The WordPress Helm Chart

Once Helm is installed, you’re ready to deploy WordPress using the Bitnami WordPress Helm Chart.

* Add the Microsoft Azure Marketplace chart repository to Helm:

```bash
helm repo add azure-marketplace https://marketplace.azurecr.io/helm/v1/repo
```

* Install the WordPress Helm Chart by executing the command below.

```bash
helm install azure-marketplace/wordpress
```

You should see something like the output below as the chart is installed. Pay special attention to the NOTES section of the output, as it contains important information to access the application.

* Check pod status until both WordPress and MariaDB are “running”:

```bash
kubectl get pods -w
```

* Check services until you see the load balancer’s external IP address:

```bash
kubectl get svc -w wordpress-chart-wordpress --namespace default
```

* Get the credentials for the application by executing the commands shown in the output of helm install:

![Bitnami Helm Commands](https://docs.bitnami.com/images/img/platforms/azure/aks-tutorial-helm-commands.png)

Browse to the specified URL and you should see WordPress running. Here’s what it should look like:

![Bitnami Helm Commands](https://docs.bitnami.com/images/img/platforms/common/tutorial-check-wordpress.png)
