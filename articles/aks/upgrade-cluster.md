---
title: Een cluster Azure Kubernetes Service (AKS) upgraden
description: Meer informatie over het upgraden van een cluster Azure Kubernetes Service (AKS)
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 05/31/2019
ms.author: mlearned
ms.openlocfilehash: dd88b5a044fe495da374178be8774f45bdd30f61
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2019
ms.locfileid: "67614053"
---
# <a name="upgrade-an-azure-kubernetes-service-aks-cluster"></a>Een cluster Azure Kubernetes Service (AKS) upgraden

Als onderdeel van de levenscyclus van een AKS-cluster moet u vaak upgraden naar de nieuwste versie van Kubernetes. Het is belangrijk u de meest recente versies van de Kubernetes-beveiliging toepassen, of een upgrade voor de nieuwste functies. Dit artikel ziet u een upgrade uitvoeren van de master-onderdelen of als één knooppunt standaardgroep in een AKS-cluster.

Zie voor AKS clusters die gebruikmaken van meerdere groepen of Windows Server-knooppunten (zowel momenteel in preview in AKS), [een knooppuntgroep in AKS Upgrade][nodepool-upgrade].

## <a name="before-you-begin"></a>Voordat u begint

In dit artikel is vereist dat u de Azure CLI versie 2.0.65 worden uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Zie [Azure CLI installeren][azure-cli-install] als u de CLI wilt installeren of een upgrade wilt uitvoeren.

## <a name="check-for-available-aks-cluster-upgrades"></a>Controleren op beschikbare upgrades voor AKS-cluster

Als u wilt controleren welke Kubernetes-versies zijn beschikbaar voor uw cluster, gebruikt u de [az aks get-upgrades][az-aks-get-upgrades] opdracht. Het volgende voorbeeld wordt gecontroleerd voor beschikbare upgrades naar het cluster met de naam *myAKSCluster* in de resourcegroep met de naam *myResourceGroup*:

```azurecli-interactive
az aks get-upgrades --resource-group myResourceGroup --name myAKSCluster --output table
```

> [!NOTE]
> Wanneer u een upgrade uitvoert van een AKS-cluster, kunnen niet secundaire versies van Kubernetes worden overgeslagen. Bijvoorbeeld, voert een upgrade tussen *1.11.x* -> *1.12.x* of *1.12.x* -> *1.13.x* zijn toegestaan, maar *1.11.x* -> *1.13.x* niet.
>
> Om bij te werken van *1.11.x* -> *1.13.x*, eerst een upgrade uitvoeren voor *1.11.x* -> *1.12.x*, vervolgens een upgrade uitvoeren van *1.12.x* -> *1.13.x*.

De volgende voorbeelduitvoer ziet u dat het cluster kan worden bijgewerkt naar versie *1.12.7* of *1.12.8*:

```console
Name     ResourceGroup    MasterVersion  NodePoolVersion  Upgrades
-------  ---------------  -------------  ---------------  --------------
default  myResourceGroup  1.11.9         1.11.9           1.12.7, 1.12.8
```

## <a name="upgrade-an-aks-cluster"></a>Een AKS-cluster upgraden

Een lijst met beschikbare versies voor uw AKS-cluster, gebruikt u de [az aks upgrade][az-aks-upgrade] command to upgrade. During the upgrade process, AKS adds a new node to the cluster that runs the specified Kubernetes version, then carefully [cordon and drains][kubernetes-drain] een van de oude knooppunten om onderbreking actieve toepassingen te minimaliseren. Wanneer het nieuwe knooppunt wordt bevestigd dat deze toepassingspods wordt uitgevoerd, wordt het oude knooppunt verwijderd. Dit proces wordt herhaald totdat alle knooppunten in het cluster zijn bijgewerkt.

Het volgende voorbeeld wordt een cluster bijgewerkt naar versie *1.12.8*:

```azurecli-interactive
az aks upgrade --resource-group myResourceGroup --name myAKSCluster --kubernetes-version 1.12.8
```

Het duurt een paar minuten om het cluster, afhankelijk van hoeveel knooppunten die u hebt te upgraden.

Als u wilt controleren of de upgrade voltooid is, gebruikt u de [az aks show][az-aks-show] opdracht:

```azurecli-interactive
az aks show --resource-group myResourceGroup --name myAKSCluster --output table
```

De volgende voorbeelduitvoer ziet u dat wordt nu het cluster uitgevoerd *1.12.8*:

```json
Name          Location    ResourceGroup    KubernetesVersion    ProvisioningState    Fqdn
------------  ----------  ---------------  -------------------  -------------------  ---------------------------------------------------------------
myAKSCluster  eastus      myResourceGroup  1.12.8               Succeeded            myaksclust-myresourcegroup-19da35-90efab95.hcp.eastus.azmk8s.io
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
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool
