---
title: Een cluster Azure Kubernetes Service (AKS) upgraden
description: Een cluster Azure Kubernetes Service (AKS) upgraden
services: container-service
author: gabrtv
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 07/18/2018
ms.author: gamonroy
ms.custom: mvc
ms.openlocfilehash: 4ff2b56afc4496b6344735b4e3c813b06cee17e3
ms.sourcegitcommit: f057c10ae4f26a768e97f2cb3f3faca9ed23ff1b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/17/2018
ms.locfileid: "42058375"
---
# <a name="upgrade-an-azure-kubernetes-service-aks-cluster"></a>Een cluster Azure Kubernetes Service (AKS) upgraden

Azure Kubernetes Service (AKS) kunt eenvoudig algemene beheertaken uitvoeren, inclusief het upgraden van Kubernetes-clusters.

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

Er zijn drie versies beschikbaar voor de upgrade: 1.9.1, 1.9.2 en 1.9.6. We kunnen de opdracht `az aks upgrade` gebruiken om te upgraden naar de meest recente beschikbare versie.  Tijdens het upgradeproces AKS wordt een nieuw knooppunt toevoegen aan het cluster, klikt u vervolgens zorgvuldig [cordon en leegmaken] [ kubernetes-drain] één knooppunt tegelijk om onderbreking actieve toepassingen te minimaliseren.

> [!NOTE]
> Bij het upgraden van een AKS-cluster, kunnen niet secundaire versies van Kubernetes worden overgeslagen. Bijvoorbeeld, upgrades tussen 1.8.x -> 1.9.x of 1.9.x -> 1.10.x zijn toegestaan, maar 1.8 -> 1.10 is niet. Als u wilt upgraden, van 1.8 1.10 ->, moet u eerst een upgrade uitvoert van 1.8 1.9 -> en klik vervolgens nog een andere doen 1.10-upgrade van 1.9 >

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

Bevestig dat de upgrade is geslaagd met de opdracht `az aks show`.

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
