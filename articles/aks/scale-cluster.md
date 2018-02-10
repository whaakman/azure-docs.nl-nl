---
title: Een AKS-cluster (Azure Container Service) schalen
description: Lees hier alles over het schalen van een AKS-cluster (Azure Container Service).
services: container-service
author: gabrtv
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 11/15/2017
ms.author: gamonroy
ms.custom: mvc
ms.openlocfilehash: fbbc24c958152806964412b426aff81a894d4412
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2018
---
# <a name="scale-an-azure-container-service-aks-cluster"></a>Een AKS-cluster (Azure Container Service) schalen

Het is niet moeilijk om een AKS-cluster naar een ander aantal knooppunten te schalen.  Selecteer het gewenste aantal knooppunten en voer de opdracht `az aks scale` uit.  Tijdens het omlaag schalen, knooppunten zorgvuldig zich [cordoned en geleegd] [ kubernetes-drain] onderbreking van actieve toepassingen minimaliseren.  Als u omhoog gaat schalen, wacht de opdracht `az` totdat de knooppunten zijn gemarkeerd `Ready` door het Kubernetes-cluster.

## <a name="scale-the-cluster-nodes"></a>De clusterknooppunten schalen

Gebruik de opdracht `az aks scale` voor het schalen van de clusterknooppunten. Het volgende voorbeeld wordt een cluster met de naam geschaald *myAKSCluster* naar één knooppunt.

```azurecli-interactive
az aks scale --name myAKSCluster --resource-group myResourceGroup --node-count 1
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
    "kubernetesVersion": "1.7.7",
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

## <a name="next-steps"></a>Volgende stappen

U krijgt meer informatie over het implementeren en beheren van AKS in de AKS-zelfstudies.

> [!div class="nextstepaction"]
> [AKS-zelfstudie][aks-tutorial]

<!-- LINKS - external -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/

<!-- LINKS - internal -->
[aks-tutorial]: ./tutorial-kubernetes-prepare-app.md