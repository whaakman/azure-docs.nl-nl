---
title: 'Zelfstudie voor Kubernetes op Azure: Een cluster bijwerken'
description: In deze zelfstudie Azure Kubernetes Service (AKS) leert u hoe u een bestaand AKS-cluster kunt bijwerken naar de nieuwste Kubernetes-versie.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: tutorial
ms.date: 08/14/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 1c0710be11b95b66d16661b5aff9cbf739ccda92
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/10/2018
ms.locfileid: "48901926"
---
# <a name="tutorial-upgrade-kubernetes-in-azure-kubernetes-service-aks"></a>Zelfstudie: Kubernetes bijwerken in AKS (Azure Kubernetes Service)

Gedurende de levenscyclus van de toepassing en het cluster, kunt u desgewenst Kubernetes bijwerken naar de nieuwste versie, en nieuwe functies gebruiken. Een AKS-cluster (Azure Kubernetes Service) kan worden bijgewerkt met de Azure CLI. Om onderbreking van actieve toepassingen te minimaliseren, worden Kubernetes-knooppunten tijdens het upgradeproces zorgvuldig [afgebakend en geleegd][kubernetes-drain].

In deze zelfstudie, deel zeven, wordt een upgrade uitgevoerd van een Kubernetes-cluster. In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * Huidige en beschikbare Kubernetes-versies identificeren
> * De Kubernetes-knooppunten upgraden
> * Een geslaagde upgrade valideren

## <a name="before-you-begin"></a>Voordat u begint

In de vorige zelfstudies is een toepassing verpakt in een containerinstallatiekopie, is deze installatiekopie geüpload naar Azure Container Registry en is een Kubernetes-cluster gemaakt. De toepassing is vervolgens in het Kubernetes-cluster uitgevoerd. Als u deze stappen niet hebt uitgevoerd en deze zelfstudie wilt volgen, gaat u terug naar [Zelfstudie 1: Containerinstallatiekopieën maken][aks-tutorial-prepare-app].

Voor deze zelfstudie moet u Azure CLI versie 2.0.44 of hoger uitvoeren. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren][azure-cli-install].

## <a name="get-available-cluster-versions"></a>Beschikbare clusterversies verkrijgen

Voordat u een cluster bijwerkt, gebruikt u de opdracht [az aks get-upgrades][] om te controleren welke Kubernetes-releases beschikbaar zijn voor de upgrade:

```azurecli
az aks get-upgrades --resource-group myResourceGroup --name myAKSCluster --output table
```

In het volgende voorbeeld is de huidige versie *1.9.6* en worden de beschikbare versies weergegeven in de kolom *Upgrades*.

```
Name     ResourceGroup    MasterVersion    NodePoolVersion    Upgrades
-------  ---------------  ---------------  -----------------  ----------------------
default  myResourceGroup  1.9.9            1.9.9              1.10.3, 1.10.5, 1.10.6
```

## <a name="upgrade-a-cluster"></a>Een cluster upgraden

Gebruik de opdracht [az aks upgrade][] als u het AKS-cluster wilt bijwerken. In de volgende voorbeelden wordt het cluster bijgewerkt naar Kubernetes-versie *1.10.6*.

> [!NOTE]
> U kunt slechts één secundaire versie per keer bijwerken. U kunt bijvoorbeeld een upgrade uitvoeren van *1.9.6* naar *1.10.3*, maar niet rechtstreeks van *1.9.6* naar *1.11.x*. Als u een upgrade wilt uitvoeren van *1.9.6* naar *1.11.x*, moet u eerst een upgrade uitvoeren van *1.9.6* naar *1.10.3*, en vervolgens nog een upgrade van *1.10.3* naar *1.11.x* uitvoeren.

```azurecli
az aks upgrade --resource-group myResourceGroup --name myAKSCluster --kubernetes-version 1.10.6
```

De volgende verkorte voorbeelduitvoer toont dat de *kubernetesVersion* nu *1.10.6* meldt:

```json
{
  "agentPoolProfiles": [
    {
      "count": 3,
      "maxPods": 110,
      "name": "nodepool1",
      "osType": "Linux",
      "storageProfile": "ManagedDisks",
      "vmSize": "Standard_DS1_v2",
    }
  ],
  "dnsPrefix": "myAKSClust-myResourceGroup-19da35",
  "enableRbac": false,
  "fqdn": "myaksclust-myresourcegroup-19da35-bd54a4be.hcp.eastus.azmk8s.io",
  "id": "/subscriptions/<Subscription ID>/resourcegroups/myResourceGroup/providers/Microsoft.ContainerService/managedClusters/myAKSCluster",
  "kubernetesVersion": "1.10.6",
  "location": "eastus",
  "name": "myAKSCluster",
  "type": "Microsoft.ContainerService/ManagedClusters"
}
```

## <a name="validate-an-upgrade"></a>Een upgrade valideren

Controleer als volgt of de upgrade is geslaagd met de opdracht [az aks show][]:

```azurecli
az aks show --resource-group myResourceGroup --name myAKSCluster --output table
```

De volgende voorbeelduitvoer laat zien dat *KubernetesVersion 1.10.6* door het AKS-cluster wordt uitgevoerd:

```
Name          Location    ResourceGroup    KubernetesVersion    ProvisioningState    Fqdn
------------  ----------  ---------------  -------------------  -------------------  ----------------------------------------------------------------
myAKSCluster  eastus      myResourceGroup  1.10.6               Succeeded            myaksclust-myresourcegroup-19da35-bd54a4be.hcp.eastus.azmk8s.io
```

## <a name="delete-the-cluster"></a>Het cluster verwijderen

Omdat dit het laatste deel is van de reeks zelfstudies, kunt u het AKS-cluster verwijderen. Aangezien de Kubernetes-knooppunten worden uitgevoerd op virtuele Azure machines (VM's), worden er nog steeds kosten in rekening gebracht, zelfs als u het cluster niet gebruikt. Gebruik de opdracht [az group delete][az-group-delete] om de resourcegroep, de containerservice en alle gerelateerde resources te verwijderen.

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

> [!NOTE]
> Wanneer u het cluster verwijdert, wordt de Azure Active Directory-service-principal die door het AKS-cluster wordt gebruikt niet verwijderd. Zie [Overwegingen voor en verwijdering van AKS service-principal][sp-delete] voor stappen voor het verwijderen van de service-principal.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een upgrade van Kubernetes in een AKS-cluster uitgevoerd. U hebt geleerd hoe u:

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
[az aks show]: /cli/azure/aks#az-aks-show
[az aks get-upgrades]: /cli/azure/aks#az-aks-get-upgrades
[az aks upgrade]: /cli/azure/aks#az-aks-upgrade
[azure-cli-install]: /cli/azure/install-azure-cli
[az-group-delete]: /cli/azure/group#az-group-delete
[sp-delete]: kubernetes-service-principal.md#additional-considerations
