---
title: Kubernetes op Azure zelfstudie - cluster bijwerken
description: Kubernetes op Azure zelfstudie - cluster bijwerken
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 11/15/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: aa457c97292fc9f97d3bc4769ca45d55dd5829a6
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2017
---
# <a name="upgrade-kubernetes-in-azure-container-service-aks"></a>Upgrade Kubernetes in Azure Containerservice (AKS)

Een Azure Container Service (AKS)-cluster kan worden bijgewerkt met de Azure CLI. Tijdens het upgradeproces Kubernetes knooppunten zijn zorgvuldig [cordoned en geleegd](https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/) onderbreking van actieve toepassingen minimaliseren.

In deze zelfstudie deel acht acht, wordt een cluster Kubernetes bijgewerkt. Taken die u uitvoert, zijn onder andere:

> [!div class="checklist"]
> * Huidige en de beschikbare Kubernetes-versie identificeren
> * Upgrade van de knooppunten Kubernetes
> * Een geslaagde upgrade valideren

## <a name="before-you-begin"></a>Voordat u begint

In vorige zelfstudies is een toepassing worden verpakt in een installatiekopie van een container, deze installatiekopie geüpload naar het register van Azure-Container en een Kubernetes-cluster gemaakt. De toepassing is vervolgens op het cluster Kubernetes uitgevoerd.

Als u deze stappen nog niet hebt gedaan en u wilt volgen, terug naar de [zelfstudie 1 – installatiekopieën van de container maken](./tutorial-kubernetes-prepare-app.md).


## <a name="get-cluster-versions"></a>Clusterversies ophalen

Vóór de upgrade van een cluster, gebruikt u de `az aks get-versions` opdracht om te controleren die Kubernetes versies zijn beschikbaar voor de upgrade.

```azurecli-interactive
az aks get-versions --name myK8sCluster --resource-group myResourceGroup --output table
```

Hier kunt u zien dat de versie van het huidige knooppunt is `1.7.7` en die versie `1.7.9`, `1.8.1`, en `1.8.2` beschikbaar zijn.

```
Name     ResourceGroup    MasterVersion    MasterUpgrades       NodePoolVersion     NodePoolUpgrades
-------  ---------------  ---------------  -------------------  ------------------  -------------------
default  myAKSCluster     1.7.7            1.8.2, 1.7.9, 1.8.1  1.7.7               1.8.2, 1.7.9, 1.8.1
```

## <a name="upgrade-cluster"></a>Upgraden van cluster

Gebruik de `az aks upgrade` opdracht voor upgrade de clusterknooppunten. De volgende voorbeelden bijwerken naar versie van het cluster `1.8.2`.

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

## <a name="validate-upgrade"></a>Upgrade valideren

U kunt nu Bevestig de upgrade is geslaagd met de `az aks show` opdracht.

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

In deze zelfstudie maakt u Kubernetes in een cluster AKS bijgewerkt. De volgende taken zijn voltooid:

> [!div class="checklist"]
> * Huidige en de beschikbare Kubernetes-versie identificeren
> * Upgrade van de knooppunten Kubernetes
> * Een geslaagde upgrade valideren

Volg deze koppeling voor meer informatie over AKS.

> [!div class="nextstepaction"]
> [AKS-overzicht](./intro-kubernetes.md)