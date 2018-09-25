---
title: Ondersteunde versies van Kubernetes in Azure Kubernetes Service
description: Informatie over de ondersteuningsbeleid voor Kubernetes-versie en de levenscyclus van clusters in Azure Kubernetes Service (AKS)
services: container-service
author: sauryadas
ms.service: container-service
ms.topic: article
ms.date: 09/21/2018
ms.author: saudas
ms.openlocfilehash: 6b55825107ae8872b146b3ad4fde0ef4b917b71d
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47047576"
---
# <a name="supported-kubernetes-versions-in-azure-kubernetes-service-aks"></a>Ondersteunde versies van Kubernetes in Azure Kubernetes Service (AKS)

De Kubernetes-community releases secundaire versies ongeveer elke drie maanden. Deze releases bevatten onder meer nieuwe functies en verbeteringen. Patch-versies (soms wekelijks) vaker zijn en zijn alleen bedoeld voor essentiële oplossingen voor problemen in een secundaire versie. Deze patch-versies bevatten oplossingen voor beveiligingsproblemen of grote bugs die invloed hebben op een groot aantal klanten en producten die worden uitgevoerd in productie op basis van Kubernetes.

Een nieuwe Kubernetes secundaire versie is beschikbaar in [acs-engine] [ acs-engine] dag één. De doelen voor AKS Service Level Objective (SLO) los van de secundaire versie voor clusters met AKS binnen 30 dagen, afhankelijk van de stabiliteit van de release.

## <a name="kubernetes-version-support-policy"></a>Ondersteuningsbeleid voor Kubernetes-versie

Vier secundaire versies van Kubernetes biedt ondersteuning voor AKS:

- De huidige secundaire versie die is uitgebracht upstream (n)
- Drie vorige secundaire versies. Elke ondersteunde secundaire versie biedt ook ondersteuning voor twee stabiel patches.

Bijvoorbeeld, als u AKS introduceert *1.11.x* vandaag de dag wordt ook ondersteund voor *1.10.a* + *1.10.b*, *1.9.c*  +  *1,9 d*, *1.8.e* + *1.8F* (waarbij de versies van de letters patch twee nieuwste stabiele builds zijn).

Wanneer een nieuwe secundaire versie is geïntroduceerd, zijn de oudste secundaire versie en patch-versies ondersteund buiten gebruik gesteld. 15 dagen vóór de release van de nieuwe secundaire versie en de komende versie buiten gebruik stellen, wordt een aankondiging gemaakt via de Azure-update-kanalen. In het voorbeeld hierboven waar *1.11.x* is uitgebracht, de buiten gebruik gestelde versies zijn *1.7.g* + *1.7.h*.

Wanneer u een AKS-cluster in de portal of met de Azure CLI implementeert, wordt het cluster is altijd ingesteld op de secundaire versie van n-1 en de nieuwste patch. Bijvoorbeeld, als biedt ondersteuning voor AKS *1.11.x*, *1.10.a* + *1.10.b*, *1.9.c* + *1,9 d* , *1.8.e* + *1.8F*, is de standaardversie voor nieuwe clusters *1.10.b*.

## <a name="faq"></a>Veelgestelde vragen

**Wat gebeurt er wanneer een klant wordt bijgewerkt een Kubernetes-cluster met een secundaire versie die niet wordt ondersteund?**

Als u van gebruikmaakt de *n-4* versie, die u niet de SLO zijn. Als de upgrade van versie n-4 naar n-3 is voltooid, bent u in de Serviceniveaudoelstelling. Bijvoorbeeld:

- Als de ondersteunde versies van AKS *1.10.a* + *1.10.b*, *1.9.c* + *1,9 d*,  *1.8.e* + *1.8F* en u bent op *1.7.g* of *1.7.h*, u niet de SLO zijn.
- Als de upgrade van *1.7.g* of *1.7.h* naar *1.8.e* of *1.8.f* is geslaagd, u bent terug op de Serviceniveaudoelstelling.

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
[acs-engine]: https://github.com/Azure/acs-engine

<!-- LINKS - Internal -->
[aks-upgrade]: upgrade-cluster.md