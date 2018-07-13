---
title: Maken van een interne load balancer van Azure Kubernetes Service (AKS)
description: Informatie over het maken en gebruiken van een interne load balancer om uw services met Azure Kubernetes Service (AKS) zichtbaar te maken.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 07/12/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 123fc08995416e0ff9c7e12a526deadc34b3a4a2
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39001392"
---
# <a name="use-an-internal-load-balancer-with-azure-kubernetes-service-aks"></a>Een interne load balancer gebruiken met Azure Kubernetes Service (AKS)

Interne taakverdeling, wordt een Kubernetes-service toegankelijk voor toepassingen die worden uitgevoerd in hetzelfde virtuele netwerk bevinden als het Kubernetes-cluster. Dit artikel leest u hoe het maken en gebruiken van een interne load balancer met Azure Kubernetes Service (AKS). Azure Load Balancer is beschikbaar in twee SKU's: Basic en Standard. AKS maakt gebruik van de basis-SKU.

## <a name="create-an-internal-load-balancer"></a>Een interne load balancer maken

Voor het maken van een interne load balancer, door een servicemanifest te maken met het servicetype *LoadBalancer* en de *azure-load-balancer-interne* aantekening zoals wordt weergegeven in het volgende voorbeeld:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
  annotations:
    service.beta.kubernetes.io/azure-load-balancer-internal: "true"
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-vote-front
```

Eenmaal is geïmplementeerd met `kubetctl apply`, een Azure load balancer is gemaakt en beschikbaar gemaakt in hetzelfde virtuele netwerk bevinden als het AKS-cluster.

![Afbeelding van AKS interne load balancer](media/internal-lb/internal-lb.png)

Wanneer u de details van de service bekijkt, het IP-adres de *externe IP-adres* kolom is het IP-adres van de interne load balancer, zoals wordt weergegeven in het volgende voorbeeld:

```
$ kubectl get service azure-vote-front

NAME               TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   LoadBalancer   10.0.248.59   10.240.0.7    80:30555/TCP   10s
```

## <a name="specify-an-ip-address"></a>Geef een IP-adres

Als u gebruiken van een specifiek IP-adres met de interne load balancer wilt, voegt u toe de *loadBalancerIP* eigenschap in op de load balancer-specificaties. Het opgegeven IP-adres moet zich bevinden in hetzelfde subnet als het AKS-cluster en moet niet worden toegewezen aan een resource.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
  annotations:
    service.beta.kubernetes.io/azure-load-balancer-internal: "true"
spec:
  type: LoadBalancer
  loadBalancerIP: 10.240.0.25
  ports:
  - port: 80
  selector:
    app: azure-vote-front
```

Wanneer u de details van de service bekijkt, het IP-adres op de *externe IP-adres* weerspiegelt het opgegeven IP-adres:

```
$ kubectl get service azure-vote-front

NAME               TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   LoadBalancer   10.0.184.168   10.240.0.25   80:30225/TCP   4m
```

## <a name="use-private-networks"></a>Particuliere netwerken gebruikt

Wanneer u uw AKS-cluster maakt, kunt u geavanceerde netwerkinstellingen. Deze aanpak kunt u het cluster implementeren in een bestaande Azure-netwerk en subnetten. Een scenario is het implementeren van uw AKS-cluster in een particulier netwerk verbonden met uw on-premises omgeving en services die alleen toegankelijk zijn intern uitvoeren. Zie voor meer informatie, [geavanceerde netwerkconfiguratie in AKS][advanced-networking].

Geen wijzigingen aangebracht in de vorige stappen zijn nodig voor het implementeren van een interne load balancer in een AKS-cluster dat gebruik maakt van een particulier netwerk. De load balancer is gemaakt in dezelfde resourcegroep bevinden als uw AKS-cluster, maar verbonden met uw persoonlijke virtueel netwerk en subnet, zoals wordt weergegeven in het volgende voorbeeld:

```
$ kubectl get service azure-vote-front

NAME               TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   LoadBalancer   10.1.15.188   10.0.0.35     80:31669/TCP   1m
```

> [!NOTE]
> Mogelijk moet u de service-principal voor uw AKS-cluster verlenen de *Inzender voor netwerken* rol aan de resourcegroep waar uw virtuele netwerk van Azure-resources worden geïmplementeerd. Weergeven van de service-principal met [az aks show][az-aks-show], zoals `az aks show --resource-group myResourceGroup --name myAKSCluster --query "servicePrincipalProfile.clientId"`. Gebruik voor het maken van een roltoewijzing de [az roltoewijzing maken] [ az-role-assignment-create] opdracht.

## <a name="specify-a-different-subnet"></a>Geef een ander subnet

Als u een subnet voor de load balancer, voeg de *azure-load-balancer-interne-subnet* aantekening met uw service. Het opgegeven subnet moet zich in hetzelfde virtuele netwerk bevinden als uw AKS-cluster. Bij de implementatie door de load balancer *externe IP-adres* adres deel uitmaakt van het opgegeven subnet.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
  annotations:
    service.beta.kubernetes.io/azure-load-balancer-internal: "true"
    service.beta.kubernetes.io/azure-load-balancer-internal-subnet: "apps-subnet"
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-vote-front
```

## <a name="delete-the-load-balancer"></a>De load balancer verwijderen

Wanneer alle services die gebruikmaken van de interne load balancer zijn verwijderd, wordt de load balancer zelf ook verwijderd.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over Kubernetes-services op de [documentatie voor Kubernetes services][kubernetes-services].

<!-- LINKS - External -->
[kubernetes-services]: https://kubernetes.io/docs/concepts/services-networking/service/

<!-- LINKS - Internal -->
[advanced-networking]: networking-overview.md
[deploy-advanced-networking]: networking-overview.md#configure-networking---cli
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create