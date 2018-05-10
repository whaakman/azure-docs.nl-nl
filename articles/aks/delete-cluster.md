---
title: Een Azure Kubernetes Service (AKS)-cluster verwijderen
description: Cluster verwijderen en AKS met CLI of Azure portal.
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 2/05/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: e006466d1450471900a8635c49d3bc6c3a73d476
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="delete-an-azure-kubernetes-service-aks-cluster"></a>Een Azure Kubernetes Service (AKS)-cluster verwijderen

Bij het verwijderen van een Azure Kubernetes Service-cluster, blijft de resourcegroep waarin het cluster is geïmplementeerd, maar alle AKS-gerelateerde resources zijn verwijderd. Dit document wordt beschreven hoe een AKS-cluster met behulp van de Azure CLI en Azure-portal te verwijderen.

Naast het verwijderen van de cluster, kan de resourcegroep waarin deze is geïmplementeerd worden verwijderd, die ook het cluster AKS wist.

## <a name="azure-cli"></a>Azure-CLI

Gebruik de [az aks verwijderen] [ az-aks-delete] opdracht voor het verwijderen van het cluster AKS.

```azurecli-interactive
az aks delete --resource-group myResourceGroup --name myAKSCluster
```

De volgende opties zijn beschikbaar met de `az aks delete` opdracht.

| Argument | Beschrijving | Vereist |
|---|---|:---:|
| `--name` `-n` | De bronnaam voor het beheerde cluster. | ja |
| `--resource-group` `-g` | Naam van de resourcegroep Azure Kubernetes Service. | ja |
| `--no-wait` | Wacht niet totdat de langlopende bewerking te voltooien. | nee |
| `--yes` `-y` | Niet vragen om bevestiging. | nee |

## <a name="azure-portal"></a>Azure Portal

Blader naar de resourcegroep met de Azure Kubernetes Service (AKS)-bron in de Azure-portal, selecteert u de resource en op **verwijderen**. U wordt gevraagd om te bevestigen dat de bewerking delete.

![AKS cluster portal verwijderen](media/container-service-delete-cluster/delete-aks-portal.png)

<!-- LINKS - internal -->
[az-aks-delete]: /cli/azure/aks?view=azure-cli-latest#az_aks_delete