---
title: Een Azure Kubernetes Service (AKS)-cluster schalen
description: Informatie over het schalen van het aantal knooppunten in een cluster Azure Kubernetes Service (AKS).
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 01/10/2019
ms.author: iainfoulds
ms.openlocfilehash: 558a3b6dc15293ab9a0895aa4f9f709ba2d0a51f
ms.sourcegitcommit: e7312c5653693041f3cbfda5d784f034a7a1a8f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2019
ms.locfileid: "54214620"
---
# <a name="scale-the-node-count-in-an-azure-kubernetes-service-aks-cluster"></a>Het aantal knooppunten in een Azure Kubernetes Service (AKS)-cluster schalen

Als de resource moet van uw toepassingen wijzigen, kunt u een AKS-cluster voor het uitvoeren van een ander aantal knooppunten handmatig schalen. Wanneer u omlaag schaalt, worden knooppunten zorgvuldig [afgebakend en geleegd] [ kubernetes-drain] om onderbreking actieve toepassingen te minimaliseren. Wanneer u schaalt, de `az` opdracht wordt er gewacht tot knooppunten zijn gemarkeerd `Ready` door het Kubernetes-cluster.

## <a name="scale-the-cluster-nodes"></a>De clusterknooppunten schalen

Haal eerst de *naam* van het gebruik van uw nodepool de [az aks show] [ az-aks-show] opdracht. Het volgende voorbeeld wordt de naam van de nodepool voor het cluster met de naam *myAKSCluster* in de *myResourceGroup* resourcegroep:

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

Gebruik de opdracht `az aks scale` voor het schalen van de clusterknooppunten. Het volgende voorbeeld wordt een cluster met de naam geschaald *myAKSCluster* naar één knooppunt. Geef uw eigen *--nodepool naam* uit de vorige opdracht, zoals *nodepool1*:

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
  "dnsPrefix": "myAKSClust-myResourceGroup-19da35",
  "enableRbac": true,
  "fqdn": "myaksclust-myresourcegroup-19da35-0d60b16a.hcp.eastus.azmk8s.io",
  "id": "/subscriptions/<guid>/resourcegroups/myResourceGroup/providers/Microsoft.ContainerService/managedClusters/myAKSCluster",
  "kubernetesVersion": "1.9.11",
  "linuxProfile": {
    "adminUsername": "azureuser",
    "ssh": {
      "publicKeys": [
        {
          "keyData": "[...]"
        }
      ]
    }
  },
  "location": "eastus",
  "name": "myAKSCluster",
  "networkProfile": {
    "dnsServiceIp": "10.0.0.10",
    "dockerBridgeCidr": "172.17.0.1/16",
    "networkPlugin": "kubenet",
    "networkPolicy": null,
    "podCidr": "10.244.0.0/16",
    "serviceCidr": "10.0.0.0/16"
  },
  "nodeResourceGroup": "MC_myResourceGroup_myAKSCluster_eastus",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "servicePrincipalProfile": {
    "clientId": "[...]",
    "secret": null
  },
  "tags": null,
  "type": "Microsoft.ContainerService/ManagedClusters"
}
```

## <a name="next-steps"></a>Volgende stappen

U krijgt meer informatie over het implementeren en beheren van AKS in de AKS-zelfstudies.

> [!div class="nextstepaction"]
> [AKS-zelfstudie][aks-tutorial]

<!-- LINKS - external -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/

<!-- LINKS - internal -->
[aks-tutorial]: ./tutorial-kubernetes-prepare-app.md
[az-aks-show]: /cli/azure/aks#az-aks-show
