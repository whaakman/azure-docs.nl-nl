---
title: Aanbevolen procedures operator - geavanceerde functies van scheduler in Azure Kubernetes Services (AKS)
description: Meer over de best practices uit de cluster-operator voor het gebruik van scheduler geavanceerde functies zoals taints en tolerations, knooppunt selectoren en affiniteit, of tussen pod-affiniteit en anti-affiniteit in Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: conceptual
ms.date: 11/26/2018
ms.author: iainfou
ms.openlocfilehash: 27c9c872f4dfb82b4a1389189d62c4e1f06ee272
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58175978"
---
# <a name="best-practices-for-advanced-scheduler-features-in-azure-kubernetes-service-aks"></a>Aanbevolen procedures voor geavanceerde scheduler-functies in Azure Kubernetes Service (AKS)

Wanneer u clusters in Azure Kubernetes Service (AKS) beheert, moet u vaak teams en workloads te isoleren. De Kubernetes-scheduler biedt geavanceerde functies waarmee kunt die u bepalen welke schillen kunnen worden gepland op bepaalde knooppunten of hoe meerdere pod toepassingen op de juiste wijze kunnen worden verdeeld over het cluster. 

Deze aanbevolen procedures voor richt zich op geavanceerde Kubernetes planningsfuncties voor clusteroperators. In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Gebruik taints en tolerations om te beperken welke schillen kunnen worden gepland op knooppunten
> * De voorkeur geeft aan schillen uitvoeren op bepaalde knooppunten met het selectoren knooppunt- of -knooppuntsaffiniteit
> * Split elkaar of gezamenlijk schillen van de groep met affiniteit tussen pod of anti-affiniteit

## <a name="provide-dedicated-nodes-using-taints-and-tolerations"></a>Toegewezen knooppunten met behulp van taints en tolerations bieden

**Aanbevolen procedurerichtlijn** -toegang voor resource-intensieve toepassingen, zoals controllers voor inkomend verkeer, om specifieke knooppunten te beperken. Houd knooppunt bronnen beschikbaar zijn voor workloads die ze nodig hebben en planning van andere werkbelastingen op de knooppunten niet toestaan.

Wanneer u uw AKS-cluster maakt, kunt u knooppunten met GPU-ondersteuning of een groot aantal krachtige CPU's kunt implementeren. Deze knooppunten worden vaak gebruikt voor workloads met grote gegevensverwerking zoals machine learning-(ML) of kunstmatige intelligentie (AI). Als dit type hardware doorgaans een dure knooppunt bron om te implementeren is, beperken van de werklasten die kunnen worden gepland op deze knooppunten. U kunt in plaats daarvan desgewenst besteden aan het aantal knooppunten in het cluster uit te voeren van ingress-services en te voorkomen dat andere werkbelastingen.

De scheduler Kubernetes kunt taints en tolerations gebruiken om te beperken welke workloads kunnen uitvoeren op de knooppunten.

* Een **taint** wordt toegepast op een knooppunt dat geeft aan dat alleen bepaalde schillen op deze kunnen worden gepland.
* Een **toleration** wordt vervolgens toegepast op een schil waarmee ze *tolereren* beïnvloeding van een knooppunt.

Wanneer u een schil op een AKS-cluster implementeert, plant Kubernetes alleen schillen op knooppunten waar een toleration wordt uitgelijnd met de beïnvloeding. Als voorbeeld wordt ervan uitgegaan dat u hebt een nodepool in uw AKS-cluster voor knooppunten met GPU ondersteunt. Definieert u de naam, zoals *gpu*, klikt u vervolgens een waarde voor de planning. Als u deze waarde instelt op *NoSchedule*, de Kubernetes-scheduler schillen op het knooppunt kan niet worden gepland als de schil bevat geen definitie van de juiste toleration.

```console
kubectl taint node aks-nodepool1 sku=gpu:NoSchedule
```

Met een beïnvloeding toegepast op de knooppunten, definieert u een toleration vervolgens in de schil-specificatie waarmee plannen op de knooppunten. Het volgende voorbeeld definieert de `sku: gpu` en `effect: NoSchedule` te tolereren de beïnvloeding toegepast op het knooppunt in de vorige stap:

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: tf-mnist
spec:
  containers:
  - name: tf-mnist
    image: microsoft/samples-tf-mnist-demo:gpu
  resources:
    requests:
      cpu: 0.5
      memory: 2Gi
    limits:
      cpu: 4.0
      memory: 16Gi
  tolerations:
  - key: "sku"
    operator: "Equal"
    value: "gpu"
    effect: "NoSchedule"
```

Wanneer deze pod wordt geïmplementeerd, zoals het gebruik van `kubectl apply -f gpu-toleration.yaml`, Kubernetes is de schil op de knooppunten kunt plannen met de beïnvloeding toegepast. Deze logische isolatie kunt u toegang tot bronnen binnen een cluster beheren.

Wanneer u taints toepast, kunt u werken met uw ontwikkelaars van toepassingen en eigenaren zodat ze voor het definiëren van de vereiste tolerations in hun implementaties.

Zie voor meer informatie over taints en tolerations [toe te passen taints en tolerations][k8s-taints-tolerations].

## <a name="control-pod-scheduling-using-node-selectors-and-affinity"></a>Besturingselement pod plannen met behulp van knooppunt selectoren en affiniteit

**Aanbevolen procedurerichtlijn** - bepalen de planning van de pods op knooppunten met behulp van knooppunt selectoren: knooppuntsaffiniteit, of tussen pod affiniteit. Deze instellingen kunnen de Kubernetes-scheduler logisch om workloads te isoleren, zoals door hardware in het knooppunt.

Taints en tolerations worden gebruikt voor het isoleren van logisch resources met een vaste deadline - als de schil niet van een knooppunt beïnvloeding tolereren, deze op het knooppunt is niet gepland. Een alternatieve methode is om te gebruiken knooppunt selectoren. U knooppunten, zoals een label geven lokaal gekoppelde SSD-opslag of een grote hoeveelheid geheugen en vervolgens definiëren in de specificatie pod een selector knooppunt. Vervolgens plant u Kubernetes de schillen in een overeenkomende knooppunt. In tegenstelling tot tolerations, kunnen de schillen zonder een overeenkomende knooppunt selector op gelabelde knooppunten worden gepland. Dit gedrag kunt u niet-gebruikte resources op de knooppunten om te gebruiken, maar prioriteit geeft aan schillen die de lijst met overeenkomende knooppunt definiëren.

We bekijken een voorbeeld van knooppunten met een grote hoeveelheid geheugen. Deze knooppunten kunnen de voorkeur geeft aan schillen die aanvragen van een grote hoeveelheid geheugen. Om ervoor te zorgen dat de resources niet zitten niet-actieve, kunnen ze ook andere schillen om uit te voeren.

```console
kubectl label node aks-nodepool1 hardware:highmem
```

Voegt een pod-specificatie vervolgens de `nodeSelector` eigenschap voor het definiëren van een knooppunt selector die overeenkomt met het label is ingesteld op een knooppunt:

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: tf-mnist
spec:
  containers:
  - name: tf-mnist
    image: microsoft/samples-tf-mnist-demo:gpu
    resources:
      requests:
        cpu: 0.5
        memory: 2Gi
      limits:
        cpu: 4.0
        memory: 16Gi
    nodeSelector:
      hardware: highmem
```

Wanneer u deze opties scheduler, werken met uw ontwikkelaars van toepassingen en eigenaren zodat ze voor het correct definiëren van hun pod-specificaties.

Zie voor meer informatie over het gebruik van selectoren knooppunt [schillen toe te wijzen aan knooppunten][k8s-node-selector].

### <a name="node-affinity"></a>Knooppuntsaffiniteit

De selectie van een knooppunt is een eenvoudige manier schillen toewijzen aan een bepaald knooppunt. Meer flexibiliteit is beschikbaar met *knooppuntsaffiniteit*. Met knooppuntaffiniteitsmasker definieert u wat gebeurt er als u de schil komt niet overeen met een knooppunt. U kunt *vereisen* dat Kubernetes scheduler komt overeen met een schil met een host met het label. U kunt *liever* een overeenkomst toestaan, maar de schil moet worden gepland op een andere host als dat niet overeenkomst beschikbaar is.

Het volgende voorbeeld wordt het knooppuntaffiniteitsmasker ingesteld op *requiredDuringSchedulingIgnoredDuringExecution*. Deze relatie is vereist voor het Kubernetes-schema voor het gebruik van een knooppunt met een bijbehorende label. Als er geen knooppunt beschikbaar is, heeft de schil na afloop van de planning om door te gaan. Als u wilt toestaan dat de schil moet worden gepland op een ander knooppunt, kunt u in plaats daarvan de waarde ingesteld op *preferredDuringScheduledIgnoreDuringExecution*:

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: tf-mnist
spec:
  containers:
  - name: tf-mnist
    image: microsoft/samples-tf-mnist-demo:gpu
    resources:
      requests:
        cpu: 0.5
        memory: 2Gi
      limits:
        cpu: 4.0
        memory: 16Gi
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
      nodeSelectorTerms:
      - matchExpressions:
        - key: hardware
          operator: In
          values: highmem
```

De *IgnoredDuringExecution* deel uit van de instelling geeft aan dat als het knooppunt labels wijzigen, de schil mag niet worden verwijderd uit het knooppunt. De Kubernetes-scheduler wordt alleen gebruikt voor het bijgewerkte knooppunt labels voor nieuwe pods wordt gepland, niet schillen al op de knooppunten worden gepland.

Zie voor meer informatie, [affiniteit en anti-affiniteit][k8s-affinity].

### <a name="inter-pod-affinity-and-anti-affinity"></a>Affiniteit tussen pod en anti-affiniteit

Laatste benadering voor de scheduler Kubernetes logisch om workloads te isoleren maakt gebruik van de affiniteit tussen pod of anti-affiniteit. De instellingen definiëren die schillen *mag niet* worden gepland op een knooppunt dat een bestaande overeenkomende pod bevat, of dat ze *moet* worden gepland. Standaard probeert de Kubernetes-scheduler meerdere schillen in een replica die is ingesteld op knooppunten te plannen. U kunt meer specifieke regels om dit gedrag definiëren.

Een goed voorbeeld is een webtoepassing die ook gebruikmaakt van een Azure-Cache voor Redis. U kunt pod anti-affiniteit regels gebruiken om aan te vragen die de Kubernetes-scheduler wordt replica's verspreid over de knooppunten. U kunt regels voor processoraffiniteit vervolgens gebruiken om ervoor te zorgen dat elk onderdeel van web-app op dezelfde host als een bijbehorende cache is gepland. De distributie van schillen over knooppunten ziet eruit als in het volgende voorbeeld:

| **Node 1** | **Knooppunt 2** | **Knooppunt 3** |
|------------|------------|------------|
| webapp-1   | webapp-2   | webapp-3   |
| cache-1    | cache-2    | cache-3    |

In dit voorbeeld is een meer complexe implementatie dan het gebruik van selectoren knooppunt of knooppuntsaffiniteit. De implementatie geeft u controle over hoe Kubernetes de schillen op knooppunten plant en kunt het logische isolatie van resources. Zie voor een compleet voorbeeld van deze webtoepassing met Azure Cache voor Redis-voorbeeld, [schillen op hetzelfde knooppunt plaatsen][k8s-pod-affinity].

## <a name="next-steps"></a>Volgende stappen

In dit artikel is gericht op geavanceerde functies van Kubernetes scheduler. Zie voor meer informatie over de bewerkingen voor een cluster in AKS, de volgende aanbevolen procedures:

* [Multitenancy en cluster-isolatie][aks-best-practices-scheduler]
* [Kubernetes scheduler basisfuncties][aks-best-practices-scheduler]
* [Verificatie en autorisatie][aks-best-practices-identity]

<!-- EXTERNAL LINKS -->
[k8s-taints-tolerations]: https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/
[k8s-node-selector]: https://kubernetes.io/docs/concepts/configuration/assign-pod-node/
[k8s-affinity]: https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#affinity-and-anti-affinity
[k8s-pod-affinity]: https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#always-co-located-in-the-same-node

<!-- INTERNAL LINKS -->
[aks-best-practices-scheduler]: operator-best-practices-scheduler.md
[aks-best-practices-cluster-isolation]: operator-best-practices-cluster-isolation.md
[aks-best-practices-identity]: operator-best-practices-identity.md
