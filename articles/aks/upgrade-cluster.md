---
title: Een AKS-cluster (Azure Container Service) upgraden
description: Een AKS-cluster (Azure Container Service) upgraden
services: container-service
author: gabrtv
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 04/05/2018
ms.author: gamonroy
ms.custom: mvc
ms.openlocfilehash: 5f5cfc49bd7b5e1c06adf97bc6f6ba88fc81f77a
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
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
-------  ---------------  ---------------  -----------------  -------------------
default  mytestaks007     1.8.10           1.8.10             1.9.1, 1.9.2, 1.9.6
```

Er zijn drie versies beschikbaar voor de upgrade: 1.9.1, 1.9.2 en 1.9.6. We kunnen de opdracht `az aks upgrade` gebruiken om te upgraden naar de meest recente beschikbare versie.  Tijdens het upgradeproces knooppunten zijn zorgvuldig [cordoned en geleegd] [ kubernetes-drain] onderbreking van actieve toepassingen minimaliseren.  Voordat u begint met het upgraden van een cluster, moet u controleren of u genoeg compute-capaciteit heeft om de workload te verwerken wanneer klusterknooppunten worden toegevoegd en verwijderd.

> [!NOTE]
> Bij de upgrade van een cluster AKS kunnen niet Kubernetes secundaire versies worden overgeslagen. Bijvoorbeeld, upgrades tussen 1.7.x > 1.8.x of 1.8.x > 1.9.x zijn toegestaan, maar 1,7 > 1,9 is niet.

```azurecli-interactive
az aks upgrade --name myAKSCluster --resource-group myResourceGroup --kubernetes-version 1.9.6
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
    "kubernetesVersion": "1.9.6",
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

Bevestig dat de upgrade is voltooid met de `az aks show` opdracht.

```azurecli-interactive
az aks show --name myAKSCluster --resource-group myResourceGroup --output table
```

Uitvoer:

```json
Name          Location    ResourceGroup    KubernetesVersion    ProvisioningState    Fqdn
------------  ----------  ---------------  -------------------  -------------------  ----------------------------------------------------------------
myAKSCluster  eastus     myResourceGroup  1.9.6                 Succeeded            myk8sclust-myresourcegroup-3762d8-2f6ca801.hcp.eastus.azmk8s.io
```

## <a name="next-steps"></a>Volgende stappen

U krijgt meer informatie over het implementeren en beheren van AKS in de AKS-zelfstudies.

> [!div class="nextstepaction"]
> [AKS-zelfstudie][aks-tutorial-prepare-app]

<!-- LINKS - external -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md