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
ms.openlocfilehash: 92c4850f623aea331e9834b5c8da66a7de34107f
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="upgrade-an-azure-container-service-aks-cluster"></a>Een AKS-cluster (Azure Container Service) upgraden

Met AKS (Azure Container Service) kunt u eenvoudig algemene beheertaken uitvoeren, inclusief het upgraden van Kubernetes-clusters.

## <a name="upgrade-an-aks-cluster"></a>Een AKS-cluster upgraden

Voordat u een cluster upgradet, gebruikt u de opdracht `az aks get-versions` om te controleren welke Kubernetes-releases beschikbaar zijn voor de upgrade.

```azurecli-interactive
az aks get-versions --name myK8sCluster --resource-group myResourceGroup --output table
```

Uitvoer:

```console
Name     ResourceGroup    MasterVersion    MasterUpgrades       NodePoolVersion     NodePoolUpgrades
-------  ---------------  ---------------  -------------------  ------------------  -------------------
default  myResourceGroup  1.7.7            1.8.2, 1.7.9, 1.8.1  1.7.7               1.8.2, 1.7.9, 1.8.1
```

Er zijn drie versies beschikbaar voor de upgrade: 1.7.9, 1.8.1 en 1.8.2. We kunnen de opdracht `az aks upgrade` gebruiken om te upgraden naar de meest recente beschikbare versie.  Tijdens het upgradeproces knooppunten zijn zorgvuldig [cordoned en geleegd] [ kubernetes-drain] onderbreking van actieve toepassingen minimaliseren.  Voordat u begint met het upgraden van een cluster, moet u controleren of u genoeg compute-capaciteit heeft om de workload te verwerken wanneer klusterknooppunten worden toegevoegd en verwijderd.

```azurecli-interactive
az aks upgrade --name myK8sCluster --resource-group myResourceGroup --kubernetes-version 1.8.2
```

Uitvoer:

```json
{
  "id": "/subscriptions/4f48eeae-9347-40c5-897b-46af1b8811ec/resourcegroups/myResourceGroup/providers/Microsoft.ContainerService/managedClusters/myK8sCluster",
  "location": "eastus",
  "name": "myK8sCluster",
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
        "name": "myK8sCluster",
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
az aks show --name myK8sCluster --resource-group myResourceGroup --output table
```

Uitvoer:

```json
Name          Location    ResourceGroup    KubernetesVersion    ProvisioningState    Fqdn
------------  ----------  ---------------  -------------------  -------------------  ----------------------------------------------------------------
myK8sCluster  eastus     myResourceGroup  1.8.2                Succeeded            myk8sclust-myresourcegroup-3762d8-2f6ca801.hcp.eastus.azmk8s.io
```

## <a name="next-steps"></a>Volgende stappen

U krijgt meer informatie over het implementeren en beheren van AKS in de AKS-zelfstudies.

> [!div class="nextstepaction"]
> [AKS-zelfstudie][aks-tutorial-prepare-app]

<!-- LINKS - external -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md