---
title: Ondersteunde versies van Kubernetes in Azure Kubernetes Service
description: Informatie over de ondersteuningsbeleid voor Kubernetes-versie en de levenscyclus van clusters in Azure Kubernetes Service (AKS)
services: container-service
author: sauryadas
ms.service: container-service
ms.topic: article
ms.date: 05/20/2019
ms.author: saudas
ms.openlocfilehash: 2d555908007f4e43a38b6d0eff909ef5050878ea
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67069681"
---
# <a name="supported-kubernetes-versions-in-azure-kubernetes-service-aks"></a>Ondersteunde versies van Kubernetes in Azure Kubernetes Service (AKS)

Ongeveer elke drie maanden wordt een secundaire versie uitgebracht door de Kubernetes-community. Deze releases bevatten nieuwe functies en verbeteringen. Patchreleases worden vaker uitgebracht (soms wekelijks) en zijn alleen bedoeld voor essentiële bugfixes in een secundaire versie. Deze patch-versies bevatten oplossingen voor beveiligingsproblemen of grote bugs die invloed hebben op een groot aantal klanten en producten die worden uitgevoerd in productie op basis van Kubernetes.

AKS is erop gericht te certificeren en nieuwe Kubernetes-versies binnen 30 dagen van een upstream-release, afhankelijk van de stabiliteit van de release.

## <a name="kubernetes-versions"></a>Kubernetes-versies

Kubernetes gebruikt de standaard [semantisch versiebeheer](https://semver.org/) versiebeheerschema. Dit betekent dat elke versie van Kubernetes deze nummering volgt:

```
[major].[minor].[patch]

Example:
  1.12.14
  1.12.15
  1.13.7
```

Elk getal uit de versie geeft aan dat de algemene compatibiliteit met de vorige versie:

* Primaire versies worden gewijzigd wanneer niet-compatibele API wordt gewijzigd of achterwaartse compatibiliteit kan worden verbroken.
* Secundaire versies wijzigen wanneer functionaliteitswijzigingen zijn aangebracht die achterwaarts compatibel met de andere secundaire versies zijn.
* Patch-versies worden gewijzigd wanneer neerwaarts compatibele oplossingen worden gemaakt.

In het algemeen gebruikers moeten inspannen om uit te voeren van de nieuwste patch-versie van de secundaire versie van deze worden uitgevoerd, bijvoorbeeld als uw productiecluster op *1.13.6* en *1.13.7* is de nieuwste patch versie die beschikbaar zijn voor de *1.13* reeks, moet u upgraden naar *1.13.7* zodra u bent er zeker van uw cluster is volledig gepatcht en ondersteund.

## <a name="kubernetes-version-support-policy"></a>Beleid voor ondersteuning van Kubernetes-versies

AKS ondersteunt vier secundaire versies van Kubernetes:

* De huidige secundaire versie die is uitgebracht in AKS (N)
* Die eerdere secundaire versies. Elke ondersteunde secundaire versie biedt ook ondersteuning voor twee stabiele patches.

Dit staat bekend als 'N-3'-(N (meest recente versie) - 3 (secundaire versies)).

Bijvoorbeeld, als u AKS introduceert *1.13.x* vandaag de dag wordt ondersteuning geboden voor de volgende versies:

Nieuwe secundaire versie van de lijst met ondersteunde
-----------------        ----------------------
1.13.x 1.12.a, 1.12.b, 1.11.a, 1.11.b, 1.10.a, 1.10.b

Waarbij 'x' en 'een' en ".b" representatieve patch-versies.

Zie voor meer informatie over communicatie met betrekking tot wijzigingen in versie en -verwachtingen, "Communicatie" hieronder.

Wanneer een nieuwe secundaire versie is geïntroduceerd, zijn de oudste secundaire versie en patch-versies ondersteund afgeschaft en verwijderd. Bijvoorbeeld als de huidige lijst met ondersteunde is:

<a name="supported-version-list"></a>Lijst met ondersteunde versie
----------------------
1.12.a, 1.12.b, 1.11.a, 1.11.b, 1.10.a, 1.10.b, 1.9.a, 1.9.b

En AKS releases 1.13.x, betekent dit dat de 1.9.x-versies (alle versies 1,9) wordt verwijderd en ondersteuning vervalt.

> [!NOTE]
> Houd er rekening mee dat als klanten een niet-ondersteunde versie van Kubernetes, ze wordt gevraagd om bij te werken bij het aanvragen van ondersteuning voor het cluster. Clusters met niet-ondersteunde Kubernetes-versies zijn niet inbegrepen bij de [AKS ondersteuningsbeleid](https://docs.microsoft.com/azure/aks/support-policies).


Naast de bovenstaande op secundaire versies, ondersteunt AKS de twee meest recente *patch** versies van een bepaalde secundaire versie. Bijvoorbeeld, krijgt de volgende ondersteunde versies:

<a name="supported-version-list"></a>Lijst met ondersteunde versie
----------------------
1.12.1, 1.12.2, 1.11.4, 1.11.5

Als upstream Kubernetes 1.12.3 en 1.11.6 uitgebracht en AKS releases die vullen van de versies, de oudste patch-versies zijn afgeschaft en wordt verwijderd en wordt de lijst met ondersteunde versie:

<a name="supported-version-list"></a>Lijst met ondersteunde versie
----------------------
1.12.*2*, 1.12.*3*, 1.11.*5*, 1.11.*6*

### <a name="communications"></a>Communicatie

* Voor nieuwe **kleine** versies van Kubernetes
  * Alle gebruikers worden geïnformeerd over de nieuwe versie en welke versie wordt verwijderd.
  * Klanten met de versie **moet worden verwijderd** wordt op de hoogte dat ze hebben **60 dagen** om te upgraden naar een ondersteunde versie (bijvoorbeeld secundaire versie).
* Voor nieuwe **patch** versies van Kubernetes
  * Alle gebruikers zijn op de hoogte gesteld van de nieuwe patchversie wordt uitgebracht en om bij te werken naar de meest recente versie van de patch.
  * Gebruikers hebben **30 dagen** om te upgraden naar een nieuwere, ondersteunde patch-release. Gebruikers hebben **30 dagen** om te upgraden naar een ondersteunde patch-release voordat de oudste wordt verwijderd.

AKS definieert "uitgebracht' als algemene beschikbaarheid, ingeschakeld in alle SLO / kwaliteit van de Service metingen en beschikbaar in alle regio's.

> [!NOTE]
> Klanten worden op de hoogte gesteld van Kubernetes versie versies en afschaffingen ontvangt wanneer een secundaire versie afgeschaft of verwijderd gebruikers is krijgen 60 dagen om te upgraden naar een ondersteunde versie. In het geval van de patch-releases, zijn klanten beschikken over 30 dagen om bij te werken naar een ondersteunde versie.

Meldingen worden verzonden:

* [Opmerkingen bij de Release van AKS](https://aka.ms/aks/releasenotes)
* Azure-portal meldingen
* [Kanaal Azure bijwerken][azure-update-channel]

### <a name="policy-exceptions"></a>Beleidsuitzonderingen

AKS behoudt zich het recht toevoegen of verwijderen van nieuwe/bestaande versies die zijn geïdentificeerd als een of meer kritieke productie die invloed hebben op fouten of beveiligingsproblemen met zonder vooraf een kennisgeving.

Versies van het patchbestand kunnen worden overgeslagen of implementatie versneld, afhankelijk van de ernst van het probleem bug of beveiliging.

### <a name="azure-portal-and-cli-default-versions"></a>Azure portal en CLI-standaard-versies

Wanneer u een AKS-cluster in de portal of met de Azure CLI implementeert, wordt het cluster is altijd ingesteld op de secundaire versie N-1 en de nieuwste patch. Bijvoorbeeld, als biedt ondersteuning voor AKS *1.13.x*, *1.12.a* + *1.12.b*, *1.11.a*  +   *1.11.b*, *1.10.a* + *1.10b*, is de standaardversie voor nieuwe clusters *1.12.b*.

AKS standaard ingesteld op N-1 (minor.latestPatch, bijvoorbeeld 1.12.b) voor klanten een bekende, stabiele en versie standaard een patch uitgevoerd.

## <a name="list-currently-supported-versions"></a>Versies weergeven die momenteel worden ondersteund

Als u wilt weten welke versies momenteel beschikbaar voor uw abonnement en regio zijn, gebruikt u de [az aks get-versies] [ az-aks-get-versions] opdracht. Het volgende voorbeeld worden de beschikbare Kubernetes-versies voor de *EastUS* regio:

```azurecli-interactive
az aks get-versions --location eastus --output table
```

De uitvoer is vergelijkbaar met het volgende voorbeeld, waarin die Kubernetes-versie *1.13.5* is de meest recente versie beschikbaar:

```
KubernetesVersion    Upgrades
-------------------  ------------------------
1.13.5               None available
1.12.7               1.13.5
1.12.6               1.12.7, 1.13.5
1.11.9               1.12.6, 1.12.7
1.11.8               1.11.9, 1.12.6, 1.12.7
1.10.13              1.11.8, 1.11.9
1.10.12              1.10.13, 1.11.8, 1.11.9
```

## <a name="faq"></a>Veelgestelde vragen

**Wat gebeurt er wanneer een klant wordt bijgewerkt een Kubernetes-cluster met een secundaire versie die niet wordt ondersteund?**

Als u van gebruikmaakt de *n-4* versie, op u zijn buiten ondersteuning en wordt gevraagd om bij te werken. Als de upgrade van versie n-4 naar n-3 is geslaagd, bent u nu in ons ondersteuningsbeleid. Bijvoorbeeld:

- Als de ondersteunde versies van AKS *1.13.x*, *1.12.a* + *1.12.b*, *1.11.c*  +  *1.11d*, en *1.10.e* + *1.10F* en u bent op *1.9.g* of *1.9.h*, u zich buiten de ondersteuning.
- Als de upgrade van *1.9.g* of *1.9.h* naar *1.10.e* of *1.10.f* is geslaagd, u bent terug op de binnen ons ondersteuningsbeleid.

Upgrades naar versies die ouder zijn dan *n-4* worden niet ondersteund. In dergelijke gevallen raden wij klanten nieuwe AKS-clusters maken en implementeren van hun workloads.

**Wat betekent van ondersteuning**

'Buiten ondersteuning' betekent dat de versie die u uitvoert, buiten de lijst met ondersteunde versies is en u wordt gevraagd naar het cluster upgraden naar een ondersteunde versie bij het aanvragen van ondersteuning. AKS maakt bovendien geen een runtime- of andere garanties voor clusters buiten de lijst met ondersteunde versies.

**Wat gebeurt er wanneer een klant kan worden geschaald van een Kubernetes-cluster met een secundaire versie die niet wordt ondersteund?**

Voor secundaire versies niet wordt ondersteund door AKS, blijft schalen binnen of buiten werken zonder problemen.

**Kan een klant blijven maken van een Kubernetes-versie blijven?**

Ja. Als het cluster zich niet op een van de versies die worden ondersteund door AKS, moet het cluster is echter buiten het ondersteuningsbeleid van AKS. Azure niet automatisch uw cluster upgraden of te verwijderen.

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
