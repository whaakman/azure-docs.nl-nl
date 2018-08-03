---
title: Verwijderen van een cluster Azure Kubernetes Service (AKS)
description: Verwijderen en AKS-cluster met de CLI of Azure-portal.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 2/05/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: c8eab17a5c635560d9a5274eb038845238968e02
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39439932"
---
# <a name="delete-an-azure-kubernetes-service-aks-cluster"></a>Verwijderen van een cluster Azure Kubernetes Service (AKS)

Bij het verwijderen van een Azure Kubernetes Service-cluster, blijft de resourcegroep waarin het cluster is geïmplementeerd, maar alle AKS-gerelateerde resources zijn verwijderd. Dit document wordt beschreven hoe u een AKS-cluster met behulp van de Azure CLI en Azure-portal verwijdert.

Naast het cluster te verwijderen, kan de resourcegroep waarin het werd geïmplementeerd worden verwijderd, die ook het AKS-cluster worden verwijderd.

## <a name="azure-cli"></a>Azure-CLI

Gebruik de [az aks verwijderen] [ az-aks-delete] opdracht voor het verwijderen van het AKS-cluster.

```azurecli-interactive
az aks delete --resource-group myResourceGroup --name myAKSCluster
```

De volgende opties zijn beschikbaar met de `az aks delete` opdracht.

| Argument | Beschrijving | Vereist |
|---|---|:---:|
| `--name` `-n` | De naam van de resource voor het beheerde cluster. | ja |
| `--resource-group` `-g` | De naam van de resourcegroep van de Azure Kubernetes Service. | ja |
| `--no-wait` | Wacht niet tot de langdurige bewerking te voltooien. | nee |
| `--yes` `-y` | Niet gevraagd om bevestiging. | nee |

## <a name="azure-portal"></a>Azure Portal

In de Azure portal, blader naar de resourcegroep met de Azure Kubernetes Service (AKS)-bron, selecteert u de resource en klikt u op **verwijderen**. U wordt gevraagd of u de verwijderbewerking te bevestigen.

![AKS-cluster portal verwijderen](media/container-service-delete-cluster/delete-aks-portal.png)

<!-- LINKS - internal -->
[az-aks-delete]: /cli/azure/aks?view=azure-cli-latest#az-aks-delete