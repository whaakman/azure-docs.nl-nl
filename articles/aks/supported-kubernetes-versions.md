---
title: Ondersteunde versies van Kubernetes in Azure Kubernetes Service
description: Informatie over de ondersteuningsbeleid voor Kubernetes-versie en de levenscyclus van clusters in Azure Kubernetes Service (AKS)
services: container-service
author: sauryadas
ms.service: container-service
ms.topic: article
ms.date: 02/15/2018
ms.author: saudas
ms.openlocfilehash: c55a12e6bd5ab57f790e5f161930851be10f5c2a
ms.sourcegitcommit: 1afd2e835dd507259cf7bb798b1b130adbb21840
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2019
ms.locfileid: "56985237"
---
# <a name="supported-kubernetes-versions-in-azure-kubernetes-service-aks"></a>Ondersteunde versies van Kubernetes in Azure Kubernetes Service (AKS)

Ongeveer elke drie maanden wordt een secundaire versie uitgebracht door de Kubernetes-community. Deze releases bevatten nieuwe functies en verbeteringen. Patchreleases worden vaker uitgebracht (soms wekelijks) en zijn alleen bedoeld voor essentiële bugfixes in een secundaire versie. Deze patch-versies bevatten oplossingen voor beveiligingsproblemen of grote bugs die invloed hebben op een groot aantal klanten en producten die worden uitgevoerd in productie op basis van Kubernetes.

Een nieuwe Kubernetes secundaire versie is beschikbaar in [aks-engine] [ aks-engine] dag één. Het SLO-doel (Service Level Objective) voor AKS is om binnen 30 dagen een secundaire versie voor AKS-clusters uit te brengen, afhankelijk van de stabiliteit van de release.

## <a name="kubernetes-version-support-policy"></a>Beleid voor ondersteuning van Kubernetes-versies

AKS ondersteunt vier secundaire versies van Kubernetes:

- De huidige secundaire versie die is uitgebracht upstream (n)
- Die eerdere secundaire versies. Elke ondersteunde secundaire versie biedt ook ondersteuning voor twee stabiele patches.

Bijvoorbeeld, als u AKS introduceert *1.12.x* vandaag de dag wordt ook ondersteund voor *1.11.a* + *1.11.b*, *1.10.c*  +  *1,10 d*, *1.9.e* + *1.9f* (waarbij de versies van de letters patch twee nieuwste stabiele builds zijn).

Wanneer een nieuwe secundaire versie wordt geïntroduceerd, worden de oudste ondersteunde secundaire versie en patchreleases afgeschaft. 15 dagen vóór de release van de nieuwe secundaire versie en de komende versie buiten gebruik stellen, een aankondiging is gemaakt via de [Azure updatekanalen][azure-update-channel]. In het voorbeeld hierboven waar *1.12.x* is uitgebracht, de buiten gebruik gestelde versies zijn *1.8.g* + *1.8.h*.

Wanneer u een AKS-cluster implementeert in de portal of met Azure CLI, wordt het cluster altijd ingesteld op de n-1 secundaire versie en de nieuwste patch. Bijvoorbeeld, als biedt ondersteuning voor AKS *1.12.x*, *1.11.a* + *1.11.b*, *1.10.c*  +   *1,10 d*, *1.9.e* + *1.9f*, is de standaardversie voor nieuwe clusters *1.11.b*.

## <a name="list-currently-supported-versions"></a>Versies weergeven die momenteel worden ondersteund

Als u wilt weten welke versies momenteel beschikbaar voor uw abonnement en regio zijn, gebruikt u de [az aks get-versies] [ az-aks-get-versions] opdracht. Het volgende voorbeeld worden de beschikbare Kubernetes-versies voor de *EastUS* regio:

```azurecli-interactive
az aks get-versions --location eastus --output table
```

De uitvoer is vergelijkbaar met het volgende voorbeeld, waarin die Kubernetes-versie *1.12.5* is de meest recente versie beschikbaar:

```
KubernetesVersion    Upgrades
-------------------  -----------------------
1.12.5               None available
1.12.4               1.12.5
1.11.7               1.12.4, 1.12.5
1.11.6               1.11.7, 1.12.4, 1.12.5
1.10.12              1.11.6, 1.11.7
1.10.9               1.10.12, 1.11.6, 1.11.7
1.9.11               1.10.9, 1.10.12
1.9.10               1.9.11, 1.10.9, 1.10.12
```

## <a name="faq"></a>Veelgestelde vragen

**Wat gebeurt er wanneer een klant wordt bijgewerkt een Kubernetes-cluster met een secundaire versie die niet wordt ondersteund?**

Als u van gebruikmaakt de *n-4* versie, die u niet de SLO zijn. Als de upgrade van versie n-4 naar n-3 is voltooid, bent u in de Serviceniveaudoelstelling. Bijvoorbeeld:

- Als de ondersteunde versies van AKS *1.12.x*, *1.11.a* + *1.11.b*, *1.10.c*  +  *1,10 d*, en *1.9.e* + *1.9f* en u bent op *1.8.g* of *1.8.h*, u niet de SLO zijn.
- Als de upgrade van *1.8.g* of *1.8.h* naar *1.9.e* of *1.9.f* is geslaagd, u bent terug op de Serviceniveaudoelstelling.

Upgrades naar versies die ouder zijn dan *n-4* worden niet ondersteund. In dergelijke gevallen raden wij klanten nieuwe AKS-clusters maken en implementeren van hun workloads.

**Wat gebeurt er wanneer een klant kan worden geschaald van een Kubernetes-cluster met een secundaire versie die niet wordt ondersteund?**

Voor secundaire versies niet wordt ondersteund door AKS, blijft schalen binnen of buiten werken zonder problemen.

**Kan een klant blijven maken van een Kubernetes-versie blijven?**

Ja. Als het cluster zich niet op een van de versies die worden ondersteund door AKS, moet het cluster is echter buiten de SLO AKS. Azure niet automatisch uw cluster upgraden of te verwijderen.

**Welke versie is de ondersteuning voor master als de agent-cluster zich niet in een van de ondersteunde versies van AKS?**

Het model wordt automatisch bijgewerkt naar de nieuwste ondersteunde versie.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het upgraden van uw cluster [een cluster Azure Kubernetes Service (AKS) upgraden][aks-upgrade].

<!-- LINKS - External -->
[aks-engine]: https://github.com/Azure/aks-engine
[azure-update-channel]: https://azure.microsoft.com/updates/?product=kubernetes-service

<!-- LINKS - Internal -->
[aks-upgrade]: upgrade-cluster.md
[az-aks-get-versions]: /cli/azure/aks#az-aks-get-versions
