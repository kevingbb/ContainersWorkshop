Run Your App
========================================
In this section we will run an image that is in our own Private Registry versus pulling it down from a Public Repo.

How do we do that?
-----------------------

### Copy Image from Public Repo
This section will take us through the steps of setting up the environment so that the K8s cluster can connect to our private repo in ACR and pull down images.

```bash
# Pull Image from Public Repo.
docker pull microsoft/azure-vote-front:redis-v1
# What is different about this command versus the one above?
docker pull redis
```

### Tag Image
Think of this like renaming.

```bash
# Tag Image.
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:redis-v1

# Check to see image is there.
docker images
```

### Push Image to Private Repo
Think of this like copy and paste.

```bash
docker push <acrLoginServer>/azure-vote-front:redis-v1

# List images in private registry.
az acr repository list --name <acrName> --username <acrName> --password <acrPassword> --output table

# To see the specific tags.
az acr repository show-tags --name <acrName> --username <acrName> --password <acrPassword> --repository azure-vote-front --output table
```

### Deploy App from Private Repo
Create a new K8s configuration file (yaml file) that now uses your new images from your private repository.