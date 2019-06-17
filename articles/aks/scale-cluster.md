---
title: Een Azure Kubernetes Service (AKS)-cluster schalen
description: Informatie over het schalen van het aantal knooppunten in een cluster Azure Kubernetes Service (AKS).
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 05/31/2019
ms.author: iainfoulds
ms.openlocfilehash: de3f8613c93715aecf7e9e066a8ad1d82e4379e3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66475125"
---
# <a name="scale-the-node-count-in-an-azure-kubernetes-service-aks-cluster"></a>Het aantal knooppunten in een Azure Kubernetes Service (AKS)-cluster schalen

Als de resource moet van uw toepassingen wijzigen, kunt u een AKS-cluster voor het uitvoeren van een ander aantal knooppunten handmatig schalen. Wanneer u omlaag schaalt, worden knooppunten zorgvuldig [afgebakend en geleegd] [ kubernetes-drain] om onderbreking actieve toepassingen te minimaliseren. Wanneer u omhoog schalen, AKS, wordt er gewacht tot er knooppunten zijn gemarkeerd `Ready` door het Kubernetes-cluster voordat schillen zijn gepland op deze.

## <a name="scale-the-cluster-nodes"></a>De clusterknooppunten schalen

Haal eerst de *naam* van uw toepassingen met behulp de [az aks show] [ az-aks-show] opdracht. Het volgende voorbeeld wordt de naam van het knooppunt toepassingen voor het cluster met de naam *myAKSCluster* in de *myResourceGroup* resourcegroep:

```azurecli-interactive
az aks show --resource-group myResourceGroup --name myAKSCluster --query agentPoolProfiles
```

De volgende voorbeelduitvoer ziet u dat de *naam* is *nodepool1*:

```console
$ az aks show --resource-group myResourceGroup --name myAKSCluster --query agentPoolProfiles

[
  {
    "count": 1,
    "maxPods": 110,
    "name": "nodepool1",
    "osDiskSizeGb": 30,
    "osType": "Linux",
    "storageProfile": "ManagedDisks",
    "vmSize": "Standard_DS2_v2"
  }
]
```

Gebruik de [az aks schaal] [ az-aks-scale] opdracht voor het schalen van de clusterknooppunten. Het volgende voorbeeld wordt een cluster met de naam geschaald *myAKSCluster* naar één knooppunt. Geef uw eigen *--nodepool naam* uit de vorige opdracht, zoals *nodepool1*:

```azurecli-interactive
az aks scale --resource-group myResourceGroup --name myAKSCluster --node-count 1 --nodepool-name <your node pool name>
```

De volgende voorbeelduitvoer ziet het cluster heeft is geschaald naar één knooppunt, zoals wordt weergegeven in de *agentPoolProfiles* sectie:

```json
{
  "aadProfile": null,
  "addonProfiles": null,
  "agentPoolProfiles": [
    {
      "count": 1,
      "maxPods": 110,
      "name": "nodepool1",
      "osDiskSizeGb": 30,
      "osType": "Linux",
      "storageProfile": "ManagedDisks",
      "vmSize": "Standard_DS2_v2",
      "vnetSubnetId": null
    }
  ],
  [...]
}
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel, moet u handmatig een AKS-cluster wilt verhogen of verlagen van het aantal knooppunten geschaald. U kunt ook de [automatisch schalen van cluster] [ cluster-autoscaler] (momenteel in preview in AKS) voor het automatisch schalen van uw cluster.

<!-- LINKS - external -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/

<!-- LINKS - internal -->
[aks-tutorial]: ./tutorial-kubernetes-prepare-app.md
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-aks-scale]: /cli/azure/aks#az-aks-scale
[cluster-autoscaler]: cluster-autoscaler.md
