---
title: Een cluster Azure Kubernetes Service (AKS) upgraden
description: Meer informatie over het upgraden van een cluster Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 02/12/2019
ms.author: iainfou
ms.openlocfilehash: 59d52db8c3f5f8968eae1a544abe1e5c6bbaacca
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56185977"
---
# <a name="upgrade-an-azure-kubernetes-service-aks-cluster"></a>Een cluster Azure Kubernetes Service (AKS) upgraden

Als onderdeel van de levenscyclus van een AKS-cluster moet u vaak upgraden naar de nieuwste versie van Kubernetes. Het is belangrijk u de meest recente versies van de Kubernetes-beveiliging toepassen, of een upgrade voor de nieuwste functies. Dit artikel ziet u hoe u een bestaand AKS-cluster upgraden.

## <a name="before-you-begin"></a>Voordat u begint

In dit artikel is vereist dat u de Azure CLI versie 2.0.56 worden uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren][azure-cli-install].

## <a name="check-for-available-aks-cluster-upgrades"></a>Controleren op beschikbare upgrades voor AKS-cluster

Als u wilt controleren welke Kubernetes-versies zijn beschikbaar voor uw cluster, gebruikt u de [az aks get-upgrades] [ az-aks-get-upgrades] opdracht. Het volgende voorbeeld wordt gecontroleerd voor beschikbare upgrades naar het cluster met de naam *myAKSCluster* in de resourcegroep met de naam *myResourceGroup*:

```azurecli-interactive
az aks get-upgrades --resource-group myResourceGroup --name myAKSCluster --output table
```

> [!NOTE]
> Wanneer u een upgrade uitvoert van een AKS-cluster, kunnen niet secundaire versies van Kubernetes worden overgeslagen. Bijvoorbeeld, voert een upgrade tussen *1.10.x* -> *1.11.x* of *1.11.x* -> *1.12.x* zijn toegestaan, maar *1.10.x* -> *1.12.x* niet.
>
> Om bij te werken van *1.10.x* -> *1.12.x*, eerst een upgrade uitvoeren voor *1.10.x* -> *1.11.x*, vervolgens een upgrade uitvoeren van *1.11.x* -> *1.12.x*.

De volgende voorbeelduitvoer ziet u dat het cluster kan worden bijgewerkt naar versie *1.11.5* of *1.11.6*:

```console
Name     ResourceGroup    MasterVersion    NodePoolVersion    Upgrades
-------  ---------------  ---------------  -----------------  --------------
default  myResourceGroup  1.10.12          1.10.12            1.11.5, 1.11.6
```

## <a name="upgrade-an-aks-cluster"></a>Een AKS-cluster upgraden

Een lijst met beschikbare versies voor uw AKS-cluster, gebruikt u de [az aks upgrade] [ az-aks-upgrade] opdracht uit om te upgraden. Tijdens het upgradeproces AKS voegt een nieuw knooppunt aan het cluster vervolgens zorgvuldig [cordon en afneemt] [ kubernetes-drain] één knooppunt tegelijk om onderbreking actieve toepassingen te minimaliseren. Het volgende voorbeeld wordt een cluster bijgewerkt naar versie *1.11.6*:

```azurecli-interactive
az aks upgrade --resource-group myResourceGroup --name myAKSCluster --kubernetes-version 1.11.6
```

Het duurt een paar minuten om het cluster, afhankelijk van hoeveel knooppunten die u hebt te upgraden.

Als u wilt controleren of de upgrade voltooid is, gebruikt u de [az aks show] [ az-aks-show] opdracht:

```azurecli-interactive
az aks show --resource-group myResourceGroup --name myAKSCluster --output table
```

De volgende voorbeelduitvoer ziet u dat wordt nu het cluster uitgevoerd *1.11.6*:

```json
Name          Location    ResourceGroup    KubernetesVersion    ProvisioningState    Fqdn
------------  ----------  ---------------  -------------------  -------------------  ---------------------------------------------------------------
myAKSCluster  eastus      myResourceGroup  1.11.6               Succeeded            myaksclust-myresourcegroup-19da35-90efab95.hcp.eastus.azmk8s.io
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u een upgrade uitvoeren van een bestaand AKS-cluster. Zie voor meer informatie over het implementeren en beheren van AKS-clusters, de reeks zelfstudies.

> [!div class="nextstepaction"]
> [AKS-zelfstudies][aks-tutorial-prepare-app]

<!-- LINKS - external -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-get-upgrades]: /cli/azure/aks#az-aks-get-upgrades
[az-aks-upgrade]: /cli/azure/aks#az-aks-upgrade
[az-aks-show]: /cli/azure/aks#az-aks-show
