---
title: "Zakelijke continuïteit en herstel na noodgevallen (BCDR): Gekoppelde Azure-regio's | Microsoft Docs"
description: Meer informatie over het Azure regioparen, om ervoor te zorgen dat toepassingen veerkrachtig tijdens het datacenterfouten zijn.
author: rayne-wiselman
ms.service: multiple
ms.topic: article
ms.date: 12/23/2018
ms.author: raynew
ms.openlocfilehash: d27db03977b84002b59d58327af7d14fbdc713c2
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/27/2018
ms.locfileid: "53792309"
---
# <a name="business-continuity-and-disaster-recovery-bcdr-azure-paired-regions"></a>Zakelijke continuïteit en herstel na noodgevallen (BCDR): Gekoppelde Azure-regio 's

## <a name="what-are-paired-regions"></a>Wat zijn gekoppelde regio's?

Azure is beschikbaar in meerdere regio's over de hele wereld. Een Azure-Geografie is een bepaald gebied van de hele wereld met ten minste één Azure-regio. Een Azure-regio is een gebied in een geografisch gebied, met een of meer datacenters.

Elke Azure-regio is gekoppeld aan een andere regio binnen hetzelfde geografische gebied, een regionaal paar samen te maken. De uitzondering is Brazilië-Zuid, die is gekoppeld aan een regio buiten de geografische locatie. In de gekoppelde regio's Azure serialiseert platform updates (gepland onderhoud), zodat slechts één gekoppelde regio tegelijk wordt bijgewerkt. Na een storing die betrekking hebben op meerdere regio's, ten minste één regio's in elk paar prioriteit moeten krijgen voor herstel.

![AzureGeography](./media/best-practices-availability-paired-regions/GeoRegionDataCenter.png)

Afbeelding 1: Azure regioparen

| Geografie | Gekoppelde regio's |  |
|:--- |:--- |:--- |
| Azië |Azië - oost |Azië - zuidoost |
| Australië |Australië - oost |Australië - zuidoost |
| Australië |Australië - centraal |Australië - centraal 2 |
| Brazilië |Brazilië - zuid |US - zuid-centraal |
| Canada |Canada - centraal |Canada - oost |
| China |China - noord |China East|
| China |China - noord 2 |China - oost 2|
| Europa |Europa - noord |Europa -west |
| Frankrijk |Frankrijk - centraal|Frankrijk - zuid|
| Duitsland |Duitsland - centraal |Duitsland - noordoost |
| India |India - centraal |India - zuid |
| India |India - west |India - zuid |
| Japan |Japan - oost |Japan - west |
| Korea |Korea - centraal |Korea - zuid |
| Noord-Amerika |US - oost |US - west |
| Noord-Amerika |US - oost 2 |US - centraal |
| Noord-Amerika |US - noord-centraal |US - zuid-centraal |
| Noord-Amerika |US - west 2 |US - west-centraal 
| VK |Verenigd Koninkrijk West |Verenigd Koninkrijk Zuid |
| het Ministerie van Defensie in de VS |US DoD - oost |US DoD - centraal |
| Amerikaanse overheid |VS (overheid) - Arizona |VS (overheid) - Texas |
| Amerikaanse overheid |US Gov - Iowa |VS (overheid) - Virginia |
| Amerikaanse overheid |VS (overheid) - Virginia |VS (overheid) - Texas |

Tabel 1 - toewijzing van Azure regioparen

- West-India is verschillend, omdat deze is gekoppeld aan een andere regio in één richting. De secundaire regio West-India van Zuid-India is, maar de secundaire regio Zuid-India centraal-India.
- Brazilië-Zuid is uniek omdat deze is gekoppeld aan een regio buiten de eigen Geografie. Secundaire regio Brazilië-Zuid van is Zuid-centraal VS, maar er is geen Zuid-centraal VS van secundaire regio Brazilië-Zuid.
- De secundaire regio VS (overheid)-Iowa is VS (overheid) Virginia, maar de secundaire regio VS (overheid)-Virginia is niet VS (overheid)-Iowa.
- VS (overheid) Virginia secundaire regio VS (overheid)-Texas is, maar de secundaire regio VS (overheid)-Texas is niet VS (overheid) Virginia.


Wordt aangeraden dat u business continuity noodherstel (BCDR configureert) over regioparen profiteren van de beleidsregels isolatie en beschikbaarheid van Azure. Voor toepassingen die ondersteuning bieden voor meerdere actieve regio's, wordt u aangeraden beide regio's in een paar van de regio waar mogelijk. Dit zorgt ervoor dat optimale beschikbaarheid voor toepassingen en geminimaliseerd hersteltijd in het geval van een noodgeval. 

## <a name="an-example-of-paired-regions"></a>Een voorbeeld van gekoppelde regio 's
Afbeelding 2 hieronder ziet u een hypothetische toepassing die gebruikmaakt van de regionaal paar voor herstel na noodgevallen. De groene getallen Markeer de regio-overschrijdende activiteiten van drie Azure-services (Azure compute, opslag en database) en hoe ze zijn geconfigureerd om te repliceren tussen regio's. De unieke voordelen van de implementatie van gekoppelde regio's zijn gemarkeerd door de oranje getallen.

![Overzicht van de voordelen van gekoppelde regio](./media/best-practices-availability-paired-regions/PairedRegionsOverview2.png)

Afbeelding 2: hypothetische Azure regionaal paar

## <a name="cross-region-activities"></a>Regio-overschrijdende activiteiten
Zoals in afbeelding 2 genoemd.

![IaaS](./media/best-practices-availability-paired-regions/1Green.png) **Azure Compute (IaaS)** – moet u extra rekenresources van tevoren om ervoor te zorgen resources zijn beschikbaar in een andere regio tijdens een noodgeval inrichten. Zie voor meer informatie, [technische richtlijnen voor Azure flexibiliteit](resiliency/resiliency-technical-guidance.md).

![Opslag](./media/best-practices-availability-paired-regions/2Green.png) **Azure Storage** -geografisch redundante opslag (GRS) is standaard geconfigureerd als een Azure Storage-account is gemaakt. Met GRS worden uw gegevens automatisch driemaal gerepliceerd binnen de primaire regio en driemaal in de gekoppelde regio. Zie voor meer informatie, [opslagopties van Azure voor redundantie](storage/common/storage-redundancy.md).

![Azure SQL](./media/best-practices-availability-paired-regions/3Green.png) **Azure SQL Database** – met Azure SQL Database Geo-replicatie, kunt u asynchrone replicatie van transacties in elke regio ter wereld; we raden u echter aan implementeren van deze resources in een gekoppelde regio voor de meeste herstel na noodgevallen. Zie voor meer informatie, [Geo-replicatie in Azure SQL Database](sql-database/sql-database-geo-replication-overview.md).

![Resource Manager](./media/best-practices-availability-paired-regions/4Green.png) **Azure Resource Manager** -Resource Manager biedt inherent logische isolatie van onderdelen in regio's. Dit betekent dat logische fouten in één regio waarschijnlijk minder invloed op een andere.

## <a name="benefits-of-paired-regions"></a>Voordelen van gekoppelde regio 's
Zoals in afbeelding 2 genoemd.  

![Isolatie](./media/best-practices-availability-paired-regions/5Orange.png)
**fysieke isolatie** – wanneer mogelijk, Azure geeft de voorkeur aan ten minste 300 mijl van scheiding tussen datacenters in een regionaal paar, hoewel dit niet praktisch of mogelijk in alle regio's. Scheiding van fysieke datacenter vermindert de kans natuurrampen, civiele onrusten, stroomstoringen of fysieke netwerkstoringen dat beide regio's in één keer. Isolatie is onderworpen aan de beperkingen die binnen het geografische gebied (Geografie grootte, de beschikbaarheid van de power/network-infrastructuur, voorschriften, enzovoort).  

![Replicatie](./media/best-practices-availability-paired-regions/6Orange.png)
**Platform geleverde replicatie** -sommige services, zoals geografisch redundante opslag bieden automatische replicatie naar de gekoppelde regio.

![Herstel](./media/best-practices-availability-paired-regions/7Orange.png)
**regio recovery volgorde** : In het geval van een grootschalige storing wordt één regio is de twee prioriteit gegeven. Toepassingen die worden geïmplementeerd op verschillende gekoppelde regio's worden gegarandeerd beschikken over een van de regio's met prioriteit wordt hersteld. Als een toepassing wordt geïmplementeerd in regio's die niet zijn gekoppeld, kan herstel zijn vertraagd: in het ergste geval dat de gekozen regio's mogelijk de laatste twee moeten worden hersteld.

![Updates](./media/best-practices-availability-paired-regions/8Orange.png)
**opeenvolgende updates** : geplande Azure-systeemupdates worden uitgerold voor gekoppelde regio's sequentieel worden verwerkt (niet op hetzelfde moment) om te minimaliseren van downtime, het effect van bugs en logische fouten in het zeldzame geval van een onjuiste Update.

![Gegevens](./media/best-practices-availability-paired-regions/9Orange.png)
**gegevensresidentie** – een regio bevindt zich binnen hetzelfde geografische gebied als het paar (met uitzondering van Brazilië-Zuid) om te voldoen aan gegevenslocatievereisten voor juridische en belastingdoeleinden.
