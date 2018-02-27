---
title: Een AKS-cluster (Azure Container Service) upgraden
description: Een AKS-cluster (Azure Container Service) upgraden
services: container-service
author: gabrtv
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 11/15/2017
ms.author: gamonroy
ms.custom: mvc
ms.openlocfilehash: 2ba18f638d3cc56437b659b7ab6754338a2220cf
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2018
---
# <a name="upgrade-an-azure-container-service-aks-cluster"></a>Een AKS-cluster (Azure Container Service) upgraden

Met AKS (Azure Container Service) kunt u eenvoudig algemene beheertaken uitvoeren, inclusief het upgraden van Kubernetes-clusters.

## <a name="upgrade-an-aks-cluster"></a>Een AKS-cluster upgraden

Voordat u een cluster upgradet, gebruikt u de opdracht `az aks get-upgrades` om te controleren welke Kubernetes-releases beschikbaar zijn voor de upgrade.

```azurecli-interactive
az aks get-upgrades --name myAKSCluster --resource-group myResourceGroup --output table
```

Uitvoer:

```console
Name     ResourceGroup    MasterVersion    NodePoolVersion    Upgrades
-------  ---------------  ---------------  -----------------  ----------------------------------
default  myResourceGroup  1.7.9            1.7.9              1.7.12, 1.8.1, 1.8.2, 1.8.6, 1.8.7
```

Er zijn drie versies beschikbaar voor de upgrade: 1.7.9, 1.8.1 en 1.8.2. We kunnen de opdracht `az aks upgrade` gebruiken om te upgraden naar de meest recente beschikbare versie.  Tijdens het upgradeproces knooppunten zijn zorgvuldig [cordoned en geleegd] [ kubernetes-drain] onderbreking van actieve toepassingen minimaliseren.  Voordat u begint met het upgraden van een cluster, moet u controleren of u genoeg compute-capaciteit heeft om de workload te verwerken wanneer klusterknooppunten worden toegevoegd en verwijderd.

```azurecli-interactive
az aks upgrade --name myAKSCluster --resource-group myResourceGroup --kubernetes-version 1.8.2
```

Uitvoer:

```json
{
  "id": "/subscriptions/<Subscription ID>/resourcegroups/myResourceGroup/providers/Microsoft.ContainerService/managedClusters/myAKSCluster",
  "location": "eastus",
  "name": "myAKSCluster",
  "properties": {
    "accessProfiles": {
      "clusterAdmin": {
        "kubeConfig": "..."
      },
      "clusterUser": {
        "kubeConfig": "..."
      }
    },
    "agentPoolProfiles": [
      {
        "count": 1,
        "dnsPrefix": null,
        "fqdn": null,
        "name": "myAKSCluster",
        "osDiskSizeGb": null,
        "osType": "Linux",
        "ports": null,
        "storageProfile": "ManagedDisks",
        "vmSize": "Standard_D2_v2",
        "vnetSubnetId": null
      }
    ],
    "dnsPrefix": "myK8sClust-myResourceGroup-4f48ee",
    "fqdn": "myk8sclust-myresourcegroup-4f48ee-406cc140.hcp.eastus.azmk8s.io",
    "kubernetesVersion": "1.8.2",
    "linuxProfile": {
      "adminUsername": "azureuser",
      "ssh": {
        "publicKeys": [
          {
            "keyData": "..."
          }
        ]
      }
    },
    "provisioningState": "Succeeded",
    "servicePrincipalProfile": {
      "clientId": "e70c1c1c-0ca4-4e0a-be5e-aea5225af017",
      "keyVaultSecretRef": null,
      "secret": null
    }
  },
  "resourceGroup": "myResourceGroup",
  "tags": null,
  "type": "Microsoft.ContainerService/ManagedClusters"
}
```

U kunt nu bevestigen dat de upgrade is geslaagd met de opdracht `az aks show`.

```azurecli-interactive
az aks show --name myAKSCluster --resource-group myResourceGroup --output table
```

Uitvoer:

```json
Name          Location    ResourceGroup    KubernetesVersion    ProvisioningState    Fqdn
------------  ----------  ---------------  -------------------  -------------------  ----------------------------------------------------------------
myAKSCluster  eastus     myResourceGroup  1.8.2                Succeeded            myk8sclust-myresourcegroup-3762d8-2f6ca801.hcp.eastus.azmk8s.io
```

## <a name="next-steps"></a>Volgende stappen

U krijgt meer informatie over het implementeren en beheren van AKS in de AKS-zelfstudies.

> [!div class="nextstepaction"]
> [AKS-zelfstudie][aks-tutorial-prepare-app]

<!-- LINKS - external -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md