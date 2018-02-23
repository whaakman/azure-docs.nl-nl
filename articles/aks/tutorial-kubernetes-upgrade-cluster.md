---
title: 'Zelfstudie voor Kubernetes op Azure: Cluster bijwerken'
description: 'Zelfstudie voor Kubernetes op Azure: Cluster bijwerken'
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 11/15/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: d82232d590bcc5c578ebe8ed7c85d25aebcfe097
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2018
---
# <a name="upgrade-kubernetes-in-azure-container-service-aks"></a>Kubernetes in Azure Container Service (AKS) upgraden

Een AKS-cluster (Azure Container Service) kan worden bijgewerkt met de Azure CLI. Tijdens het upgradeproces worden Kubernetes-knooppunten zorgvuldig [afgebakend en geleegd][kubernetes-drain] om onderbreking van actieve toepassingen te minimaliseren.

In deze zelfstudie, deel acht van de acht, wordt een upgrade uitgevoerd van een Kubernetes-cluster. Taken die u uitvoert, zijn onder andere:

> [!div class="checklist"]
> * Huidige en beschikbare Kubernetes-versies identificeren
> * De Kubernetes-knooppunten upgraden
> * Een geslaagde upgrade valideren

## <a name="before-you-begin"></a>Voordat u begint

In de vorige zelfstudies is een toepassing verpakt in een containerinstallatiekopie, is deze installatiekopie geüpload naar Azure Container Registry en is een Kubernetes-cluster gemaakt. De toepassing is vervolgens in het Kubernetes-cluster uitgevoerd.

Als u deze stappen niet hebt uitgevoerd en deze zelfstudie wilt volgen, gaat u terug naar [Zelfstudie 1: Containerinstallatiekopieën maken][aks-tutorial-prepare-app].


## <a name="get-cluster-versions"></a>Clusterversies verkrijgen

Voordat u een cluster upgradet, gebruikt u de opdracht `az aks get-versions` om te controleren welke Kubernetes-releases beschikbaar zijn voor de upgrade.

```azurecli-interactive
az aks get-versions --name myAKSCluster --resource-group myResourceGroup --output table
```

Hier kunt u zien dat de versie van het huidige knooppunt `1.7.7` is en dat versie `1.7.9`, `1.8.1` en `1.8.2` beschikbaar zijn.

```
Name     ResourceGroup    MasterVersion    MasterUpgrades       NodePoolVersion     NodePoolUpgrades
-------  ---------------  ---------------  -------------------  ------------------  -------------------
default  myAKSCluster     1.7.7            1.8.2, 1.7.9, 1.8.1  1.7.7               1.8.2, 1.7.9, 1.8.1
```

## <a name="upgrade-cluster"></a>Een cluster upgraden

Gebruik de opdracht `az aks upgrade` als u de clusterknooppunten wilt upgraden. In de volgende voorbeelden wordt het cluster bijgewerkt naar versie `1.8.2`.

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

## <a name="validate-upgrade"></a>Upgrade valideren

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