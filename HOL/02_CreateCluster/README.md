Create Cluster
========================================
In this section you will create a Private Container Regsitry as well as spinning up a Container Orchestrator using Azure Container Services (ACS).

![](./images/00_workshop_overview.png)

Configure Azure Environment
-----------------------

### Create Azure Resource Group
Everything in Azure needs to reside in a Resource Group (RG). Think of this as a way to group all like resources together.

```bash
# Create new RG in Azure.
az group create --name myResourceGroup --location eastus

# Once RG has been setup you cna configure defaults if you choose. This helps avoid adding
RG parameters to subsequent commands, but sometimes can be confusing.
az account set --subscription="<Subscrition Goes Here>"
az configure -d group=myResourceGroup location=eastus
```

### Create Azure Container Registry
This section will take us through the steps of creating a Private Container Registry using Azure Container Regsitry, also known as ACR. The registry is what holds the Docker images we will be using to run workloads.

![](./images/01_azure_container_services.png)


```bash
az acr create --resource-group myResourceGroup --name myContainerRegistry --sku Basic --admin-enabled true
```

The following commands are examples of how to interact with ACR:
``` bash
# Find ACR login server.
az acr show --name <acrName> --query loginServer

# Find ACR password. 
az acr credential show --name <acrName> --query passwords[0].value

# Login to ACR, it is similar to logging into Docker Hub.
docker login --username=<acrName> --password=<acrPassword> <acrLoginServer>

# Test to see that the login was successful, query the registry for a list of images.
docker images

# List Images in the registry.
az acr repository list --name <acrName> --username <acrName> --password <acrPassword> --output table
```

### Create Container Orchestrator
This section will take us through the steps of creating a new Container Cluster with Kubernetes as the Orchestrator using Azure Container Service, also known as ACS.

```bash
# Create Cluster Command.
az acs create --orchestrator-type kubernetes --resource-group myResourceGroup --name myK8sCluster-<alias> --generate-ssh-keys

# Install K8s CLI.
az acs kubernetes install-cli

# Connect to Cluster.
az acs kubernetes get-credentials --resource-group=myResourceGroup --name=myK8sCluster-<alias>

# Verify Connection to K8s.
kubectl get nodes

# Other useful K8s commands.
kubectl config get-contexts
kubectl config use-context <Context_From_Above>
kubectl config set-context <Context_From_Above> --namespace=default
```