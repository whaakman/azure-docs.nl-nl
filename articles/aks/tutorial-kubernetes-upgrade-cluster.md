---
title: 'Zelfstudie voor Kubernetes op Azure: Cluster bijwerken'
description: 'Zelfstudie voor Kubernetes op Azure: Cluster bijwerken'
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: tutorial
ms.date: 06/29/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: d66197b69a0804a49fabb72e9b97c77e000bdf88
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37131640"
---
# <a name="tutorial-upgrade-kubernetes-in-azure-kubernetes-service-aks"></a>Zelfstudie: Kubernetes bijwerken in AKS (Azure Kubernetes Service)

Een AKS-cluster (Azure Kubernetes Service) kan worden bijgewerkt met de Azure CLI. Om onderbreking van actieve toepassingen te minimaliseren, worden Kubernetes-knooppunten tijdens het upgradeproces zorgvuldig [afgebakend en geleegd][kubernetes-drain].

In deze zelfstudie, deel zeven, wordt een upgrade uitgevoerd van een Kubernetes-cluster. Taken die u uitvoert, zijn onder andere:

> [!div class="checklist"]
> * Huidige en beschikbare Kubernetes-versies identificeren
> * De Kubernetes-knooppunten upgraden
> * Een geslaagde upgrade valideren

## <a name="before-you-begin"></a>Voordat u begint

In de vorige zelfstudies is een toepassing verpakt in een containerinstallatiekopie, is deze installatiekopie geüpload naar Azure Container Registry en is een Kubernetes-cluster gemaakt. De toepassing is vervolgens in het Kubernetes-cluster uitgevoerd.

Als u deze stappen niet hebt uitgevoerd en deze zelfstudie wilt volgen, gaat u terug naar [Zelfstudie 1: Containerinstallatiekopieën maken][aks-tutorial-prepare-app].

## <a name="get-cluster-versions"></a>Clusterversies verkrijgen

Voordat u een cluster upgradet, gebruikt u de opdracht [az aks get-upgrades][] om te controleren welke Kubernetes-releases beschikbaar zijn voor de upgrade.

```azurecli
az aks get-upgrades --name myAKSCluster --resource-group myResourceGroup --output table
```

In het volgende voorbeeld is de versie van het huidige knooppunt *1.9.6* en worden de beschikbare versies weergegeven in de kolom *Upgrades*.

```
Name     ResourceGroup    MasterVersion    NodePoolVersion    Upgrades
-------  ---------------  ---------------  -----------------  ----------
default  myResourceGroup  1.9.6            1.9.6              1.10.3
```

## <a name="upgrade-cluster"></a>Een cluster upgraden

Gebruik de opdracht [az aks upgrade][] als u de clusterknooppunten wilt upgraden. In de volgende voorbeelden wordt het cluster bijgewerkt naar versie *1.10.3*.

```azurecli
az aks upgrade --name myAKSCluster --resource-group myResourceGroup --kubernetes-version 1.10.3
```

De volgende verkorte voorbeelduitvoer toont dat de *kubernetesVersion* nu *1.10.3* meldt:

```json
{
  "agentPoolProfiles": [
    {
      "count": 3,
      "maxPods": 110,
      "name": "nodepool1",
      "osType": "Linux",
      "storageProfile": "ManagedDisks",
      "vmSize": "Standard_DS1_v2",
    }
  ],
  "dnsPrefix": "myAKSClust-myResourceGroup-19da35",
  "enableRbac": false,
  "fqdn": "myaksclust-myresourcegroup-19da35-bd54a4be.hcp.eastus.azmk8s.io",
  "id": "/subscriptions/<Subscription ID>/resourcegroups/myResourceGroup/providers/Microsoft.ContainerService/managedClusters/myAKSCluster",
  "kubernetesVersion": "1.10.3",
  "location": "eastus",
  "name": "myAKSCluster",
  "type": "Microsoft.ContainerService/ManagedClusters"
}
```

## <a name="validate-upgrade"></a>Upgrade valideren

Controleer of de upgrade is geslaagd met de opdracht [az aks show][].

```azurecli
az aks show --name myAKSCluster --resource-group myResourceGroup --output table
```

Uitvoer:

```json
Name          Location    ResourceGroup    KubernetesVersion    ProvisioningState    Fqdn
------------  ----------  ---------------  -------------------  -------------------  ----------------------------------------------------------------
myAKSCluster  eastus      myResourceGroup  1.10.3               Succeeded            myaksclust-myresourcegroup-19da35-bd54a4be.hcp.eastus.azmk8s.io
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een upgrade van Kubernetes in een AKS-cluster uitgevoerd. De volgende taken zijn uitgevoerd:

> [!div class="checklist"]
> * Huidige en beschikbare Kubernetes-versies identificeren
> * De Kubernetes-knooppunten upgraden
> * Een geslaagde upgrade valideren

Volg deze koppeling voor meer informatie over AKS.

> [!div class="nextstepaction"]
> [Overzicht van AKS][aks-intro]

<!-- LINKS - external -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/

<!-- LINKS - internal -->
[aks-intro]: ./intro-kubernetes.md
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[az aks show]: /cli/azure/aks#az-aks-show
[az aks get-upgrades]: /cli/azure/aks#az-aks-get-upgrades
[az aks upgrade]: /cli/azure/aks#az-aks-upgrade