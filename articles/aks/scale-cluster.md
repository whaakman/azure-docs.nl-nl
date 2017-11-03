---
title: Een Azure Container Service (AKS)-cluster schalen | Microsoft Docs
description: Een Azure Container Service (AKS)-cluster schalen.
services: container-service
documentationcenter: 
author: gabrtv
manager: timlt
editor: 
tags: aks, azure-container-service
keywords: Kubernetes, Docker, containers, microservices, Azure
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/24/2017
ms.author: gamonroy
ms.custom: mvc
ms.openlocfilehash: d380c593ebecd18af7dcca190d8c4134cbdfd63c
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2017
---
# <a name="scale-an-azure-container-service-aks-cluster"></a>Een Azure Container Service (AKS)-cluster schalen

Het is gemakkelijk om te schalen van een cluster AKS naar een ander aantal knooppunten.  Selecteer het gewenste aantal knooppunten en voer de `az aks scale` opdracht.  Tijdens het omlaag schalen, knooppunten zorgvuldig zich [cordoned en geleegd](https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/) onderbreking van actieve toepassingen minimaliseren.  Tijdens het schalen van de `az` opdracht wacht totdat de knooppunten zijn gemarkeerd `Ready` door het cluster Kubernetes.

## <a name="scale-the-cluster-nodes"></a>Schalen van de clusterknooppunten

Gebruik de `az aks scale` opdracht voor het schalen van de clusterknooppunten. Het volgende voorbeeld wordt een cluster met de naam geschaald *myK8SCluster* naar één knooppunt.

```azurecli-interactive
az aks scale --name myK8sCluster --resource-group myResourceGroup --agent-count 1
```

Uitvoer:

```json
{
  "id": "/subscriptions/4f48eeae-9347-40c5-897b-46af1b8811ec/resourcegroups/myResourceGroup/providers/Microsoft.ContainerService/managedClusters/myK8sCluster",
  "location": "westus2",
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
    "fqdn": "myk8sclust-myresourcegroup-4f48ee-406cc140.hcp.westus2.azmk8s.io",
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

Meer informatie over het implementeren en beheren van AKS met de zelfstudies AKS.

> [!div class="nextstepaction"]
> [AKS-zelfstudie](./tutorial-kubernetes-prepare-app.md)
