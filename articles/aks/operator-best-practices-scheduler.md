---
title: Operator aanbevolen procedures - functies van Basic scheduler in Azure Kubernetes Services (AKS)
description: Meer over de best practices uit de cluster-operator voor het gebruik van eenvoudige scheduler-functies zoals resourcequota en pod wordt onderbroken, budgetten in Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: conceptual
ms.date: 11/26/2018
ms.author: iainfou
ms.openlocfilehash: 8233330973946e552e36a85a11bdbbfb06c739f0
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58178132"
---
# <a name="best-practices-for-basic-scheduler-features-in-azure-kubernetes-service-aks"></a>Aanbevolen procedures voor basic scheduler-functies in Azure Kubernetes Service (AKS)

Wanneer u clusters in Azure Kubernetes Service (AKS) beheert, moet u vaak teams en workloads te isoleren. De Kubernetes-scheduler biedt functies die u kunnen de distributie van compute-resources beheren of beperken van de invloed van onderhoudsgebeurtenissen.

Deze aanbevolen procedures voor richt zich op basic Kubernetes planningsfuncties voor clusteroperators. In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Resourcequota gebruiken voor een vaste hoeveelheid resources aan teams of werkbelastingen
> * De gevolgen van het geplande onderhoud met behulp van de schil wordt onderbroken budgetten beperken
> * Controleren op ontbrekende pod resourceaanvragen en beperkingen met behulp van de `kube-advisor` hulpprogramma

## <a name="enforce-resource-quotas"></a>Resourcequota afdwingen

**Aanbevolen procedurerichtlijn** -plannen en resource-quota op het niveau van de naamruimte. Schillen niet definiëren resourceaanvragen en -limieten, dan wordt de implementatie geweigerd. Resourcegebruik te bewaken en quota's naar wens aanpassen.

Resourceaanvragen en -limieten worden geplaatst in de schil-specificatie. Deze limieten worden gebruikt door de scheduler Kubernetes tijdens de implementatie voor een beschikbaar knooppunt niet vinden in het cluster. Deze beperkingen en aanvragen werkt op afzonderlijke pod-niveau. Zie voor meer informatie over het definiëren van deze waarden [definiëren pod resourceaanvragen en -limieten][resource-limits]

Als u wilt bieden een manier om te reserveren en resources voor een ontwikkelingsteam of project beperken, moet u *resourcequota*. Deze quota zijn gedefinieerd in een naamruimte, en kunnen worden gebruikt voor het instellen van quota op basis van de volgende:

* **Rekenresources**, zoals CPU en geheugen of GPU's.
* **Storage-resources**, bevat het totale aantal volumes of de hoeveelheid schijfruimte voor een bepaalde opslagklasse.
* **Aantal objecten**, zoals het maximum aantal geheimen, services of taken kunnen worden gemaakt.

Kubernetes biedt geen resources overcommit. Zodra het cumulatieve totaal van resourceaanvragen of limieten voor het toegewezen quotum is geslaagd, is geen verdere implementaties zijn geslaagd.

Wanneer u resourcequota definieert, moeten alle schillen gemaakt in de naamruimte limieten of de aanvragen in hun pod-specificaties opgeven. Als ze deze waarden niet opgeeft, kunt u de implementatie kunt weigeren. In plaats daarvan kunt u [configureren standaard aanvragen en -limieten voor een naamruimte][configure-default-quotas].

Het volgende voorbeeld YAML-manifest met de naam *dev-app-team-quotas.yaml* Hiermee stelt u een vaste limiet van een totaal van *10* CPU's, *20Gi* van geheugen en *10*schillen:

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: dev-app-team
spec:
  hard:
    cpu: "10"
    memory: 20Gi
    pods: "10"
```

Is dit resourcequotum voor kan worden toegepast door het opgeven van de naamruimte, zoals *dev-apps*:

```console
kubectl apply -f dev-app-team-quotas.yaml --namespace dev-apps
```

Werken met uw ontwikkelaars van toepassingen en -eigenaren aan de hand van hun behoeften en de juiste resourcequota toe te passen.

Zie voor meer informatie over beschikbare resourceobjecten, bereiken en prioriteiten [resourcequota in Kubernetes][k8s-resource-quotas].

## <a name="plan-for-availability-using-pod-disruption-budgets"></a>Plan voor beschikbaarheid pod onderbreking budgetten gebruiken

**Aanbevolen procedurerichtlijn** : als u wilt onderhouden van de beschikbaarheid van toepassingen, het definiëren van de schil wordt onderbroken budgetten (PDB-bestanden) om ervoor te zorgen dat een minimum aantal schillen beschikbaar in het cluster zijn.

Er zijn twee verstorende gebeurtenissen die ertoe leiden schillen dat moet worden verwijderd:

* *Aantal onvrijwillige onderbrekingen* zijn gebeurtenissen buiten de normale controle van de cluster-operator of de eigenaar van toepassing.
  * Deze onopzettelijk onderbrekingen bevatten een hardwarestoring optreedt op de fysieke machine, een kernelprobleem of het verwijderen van een VM-knooppunt
* *Vrijwillige onderbrekingen* zijn gebeurtenissen die worden aangevraagd door de operator cluster of de eigenaar van toepassing.
  * Deze vrijwillig onderbrekingen bevatten upgraden van clusters, een sjabloon voor de bijgewerkte implementatie of per ongeluk verwijderen van een schil.

Het aantal onvrijwillige onderbrekingen kunnen worden opgelost door het gebruik van meerdere replica's van uw schillen in een implementatie. Meerdere knooppunten in het AKS-cluster uitgevoerd helpt ook bij met deze onopzettelijk onderbrekingen. Kubernetes biedt voor vrijwillig onderbrekingen, *pod onderbreking budgetten* waarmee de cluster-operator een aantal minimale beschikbaar of maximaal niet beschikbaar resources definiëren. Deze onderbreking van de schil budgetten kunnen u plannen hoe implementaties of replicasets reageert wanneer een gebeurtenis in het vrijwillig wordt onderbroken.

Als een cluster moeten worden bijgewerkt of een sjabloon voor de implementatie bijgewerkt, de Kubernetes-scheduler kunt u ervoor dat aanvullende schillen zijn volgens de planning op andere knooppunten voordat de gebeurtenissen vrijwillig onderbreking kunnen worden voortgezet. De scheduler wordt gewacht voordat een knooppunt opnieuw wordt opgestart nadat het opgegeven aantal schillen zijn is gepland op andere knooppunten in het cluster.

We bekijken een voorbeeld van een replica die is ingesteld op vijf schillen waarop NGINX wordt uitgevoerd. De schillen in de replica instellen als het label toegewezen `app: nginx-frontend`. Tijdens een onderbreking van de vrijwillig-gebeurtenis, zoals de clusterupgrade van een, die u wilt controleren of ten minste drie schillen worden uitgevoerd. De volgende YAML-manifest voor een *PodDisruptionBudget* object definieert deze vereisten:

```yaml
apiVersion: policy/v1beta1
kind: PodDisruptionBudget
metadata:
   name: nginx-pdb
spec:
   minAvailable: 3
   selector:
   matchLabels:
      app: nginx-frontend
```

U kunt ook een percentage, zoals definiëren *60%*, omhoog schalen van het aantal schillen waarmee u automatisch compenseren voor de replica is ingesteld.

U kunt een maximum aantal exemplaren is niet beschikbaar in een replicaset definiëren. Percentage voor de maximale pods die niet beschikbaar kan opnieuw, ook worden gedefinieerd. Het volgende pod onderbreking budget YAML-manifest dat niet meer dan twee schillen in de replica definieert set niet beschikbaar:

```yaml
apiVersion: policy/v1beta1
kind: PodDisruptionBudget
metadata:
   name: nginx-pdb
spec:
   maxUnavailable: 2
   selector:
   matchLabels:
      app: nginx-frontend
```

Als uw budget van de onderbreking van de schil is gedefinieerd, maakt u deze in uw AKS-cluster net als bij elk ander Kubernetes-object:

```console
kubectl apply -f nginx-pdb.yaml
```

Werken met uw ontwikkelaars van toepassingen en eigenaren van inzicht in hun behoeften en de onderbreking van de juiste pod budgetten toepassen.

Zie voor meer informatie over het gebruik van de schil wordt onderbroken budgetten [opgeven van een onderbreking van de budget voor uw toepassing][k8s-pdbs].

## <a name="regularly-check-for-cluster-issues-with-kube-advisor"></a>Regelmatig controleren op problemen met kube-advisor cluster

**Aanbevolen procedurerichtlijn** -Voer regelmatig de nieuwste versie van `kube-advisor` open source-hulpprogramma om problemen te detecteren in uw cluster. Als u resourcequota op een bestaand AKS-cluster toepast, voert u `kube-advisor` eerst te vinden schillen waarvoor geen resourceaanvragen en -limieten die zijn gedefinieerd.

De [kube-advisor] [ kube-advisor] hulpprogramma is een bijbehorende AKS open source-project, die een Kubernetes-cluster wordt gescand en rapporten weergeven over problemen die worden gevonden. Een nuttige controle is het identificeren van schillen die niet over een bron aanvragen en -limieten in plaats.

In een AKS-cluster die als host fungeert voor meerdere teams voor ontwikkeling en -toepassingen, kan het lastig zijn om bij te houden van schillen zonder deze resource-aanvragen en set beperkt. Als een best practice regelmatig uitgevoerd `kube-advisor` op uw AKS-clusters, met name als u geen resourcequota aan naamruimten toewijst.

## <a name="next-steps"></a>Volgende stappen

In dit artikel is gericht op Kubernetes scheduler basisfuncties. Zie voor meer informatie over de bewerkingen voor een cluster in AKS, de volgende aanbevolen procedures:

* [Multitenancy en cluster-isolatie][aks-best-practices-cluster-isolation]
* [Geavanceerde functies voor Kubernetes-scheduler][aks-best-practices-advanced-scheduler]
* [Verificatie en autorisatie][aks-best-practices-identity]

<!-- EXTERNAL LINKS -->
[k8s-resource-quotas]: https://kubernetes.io/docs/concepts/policy/resource-quotas/
[configure-default-quotas]: https://kubernetes.io/docs/tasks/administer-cluster/manage-resources/memory-default-namespace/
[kube-advisor]: https://github.com/Azure/kube-advisor
[k8s-pdbs]: https://kubernetes.io/docs/tasks/run-application/configure-pdb/

<!-- INTERNAL LINKS -->
[resource-limits]: developer-best-practices-resource-management.md#define-pod-resource-requests-and-limits
[aks-best-practices-cluster-isolation]: operator-best-practices-cluster-isolation.md
[aks-best-practices-advanced-scheduler]: operator-best-practices-advanced-scheduler.md
[aks-best-practices-identity]: operator-best-practices-identity.md
