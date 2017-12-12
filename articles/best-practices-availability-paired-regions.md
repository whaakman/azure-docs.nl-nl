---
title: "Zakelijke continuïteit en herstel na noodgevallen (BCDR): Azure-gebieden gekoppeld | Microsoft Docs"
description: Meer informatie over het Azure regionale koppelen, om ervoor te zorgen dat toepassingen robuuste tijdens data center-fouten.
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: cfreeman
editor: 
ms.assetid: c2d0a21c-2564-4d42-991a-bc31723f61a4
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/11/2017
ms.author: raynew
ms.openlocfilehash: 394f353837433e241e4da6f4accdb5eaa24bae46
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="business-continuity-and-disaster-recovery-bcdr-azure-paired-regions"></a>Zakelijke continuïteit en herstel na noodgevallen (BCDR): Azure-gebieden gekoppeld

## <a name="what-are-paired-regions"></a>Wat zijn gekoppeld regio's?

Azure werkt op meerdere locaties over de hele wereld. Een Azure Geografie is een bepaald gebied van de wereld met ten minste één Azure-regio. Een Azure-regio is een gebied binnen een Geografie, met een of meer datacenters.

Elke Azure-regio is gekoppeld aan een andere regio binnen de dezelfde Geografie, een combinatie van regionale samen te maken. De uitzondering is Brazilië-Zuid is gekoppeld aan buiten de geografische regio.

![AzureGeography](./media/best-practices-availability-paired-regions/GeoRegionDataCenter.png)

Afbeelding 1: Azure regionale paar diagram

| Geografie | Gekoppelde regio's |  |
|:--- |:--- |:--- |
| Azië |Oost-Azië |Zuidoost-Azië |
| Australië |Australië - oost |Australië - zuidoost |
| Canada |Canada - centraal |Canada - oost |
| China |China - noord |China - oost|
| India |Centraal-India |Zuid-India |
| India |West, India (1) |Zuid-India |
| Japan |Japan - oost |Japan - west |
| Korea |Korea - centraal |Korea - zuid |
| Noord-Amerika |Noord-centraal VS |Zuid-centraal VS |
| Noord-Amerika |VS - oost |VS - west |
| Noord-Amerika |VS - oost 2 |VS - midden |
| Noord-Amerika |VS - west 2 |West-centraal VS |
| Europa |Noord-Europa |West-Europa |
| Japan |Japan - oost |Japan - west |
| Brazilië |Brazilië-Zuid (2) |Zuid-centraal VS |
| Amerikaanse overheid |VS Gov Iowa (3) |VS (overheid) - Virginia |
| Amerikaanse overheid |VS Gov Virginia (4) |VS (overheid) - Texas |
| Amerikaanse overheid |VS (overheid) - Arizona |VS (overheid) - Texas |
| Amerikaanse ministerie van defensie |US DoD - oost |US DoD - centraal |
| VERENIGD KONINKRIJK |Verenigd Koninkrijk West |Verenigd Koninkrijk Zuid |
| Duitsland |Duitsland - centraal |Duitsland - noordoost |

Tabel 1 - toewijzing van Azure regionale paren

- > (1) India West is verschillend, omdat deze is gekoppeld aan een andere regio in één richting. Secundaire regio West India Zuid, India is, maar de secundaire regio Zuid, India centraal, India is.
- > (2) Brazilië-Zuid is uniek omdat deze is gekoppeld aan een regio buiten de eigen Geografie. Brazilië-Zuid secundaire regio is Zuid-centraal VS, maar er is geen Zuid-centraal VS van secundaire regio Brazilië-Zuid.
- > (3) VS Gov Iowa secundaire regio is Gov ons Virginia, maar de Gov ons Virginia secundaire regio is niet Gov ons Iowa.
- > (4) de secundaire regio VS Gov Virginia Gov ons Texas is, maar Gov ons-Texas secundaire regio is niet Gov ons Virginia.


Het is raadzaam dat u workloads over regionale paren repliceren profiteren van Azure isolatie en beschikbaarheid van beleid. Bijvoorbeeld, geplande Azure systeemupdates sequentieel worden geïmplementeerd (en niet op hetzelfde moment) tussen de gekoppelde regio's. Dat betekent dat zelfs in het zeldzame geval van een defecte update beide regio's worden niet beïnvloed tegelijkertijd. Bovendien, in het onwaarschijnlijke geval van een brede onderbreking herstel van ten minste één regio buiten elk paar voorrang krijgt.

## <a name="an-example-of-paired-regions"></a>Een voorbeeld van een gekoppelde regio 's
Afbeelding 2 hieronder toont een hypothetische toepassing die gebruikmaakt van het regionale paar voor herstel na noodgevallen. De groene getallen Markeer de regio-overschrijdende activiteiten van drie Azure-services (Azure compute, storage, en database) en hoe deze zijn geconfigureerd om te repliceren tussen regio's. De unieke voordelen van de implementatie in gekoppelde regio's worden gemarkeerd met de oranje getallen.

![Overzicht van de voordelen van de gekoppelde regio](./media/best-practices-availability-paired-regions/PairedRegionsOverview2.png)

Afbeelding 2: hypothetische Azure regionale paar

## <a name="cross-region-activities"></a>Regio-overschrijdende activiteiten
In afbeelding 2 bedoelde.

![PaaS](./media/best-practices-availability-paired-regions/1Green.png) **Azure Compute (PaaS)** – moet u extra Reken-bronnen van tevoren om ervoor te zorgen bronnen beschikbaar zijn in een andere regio tijdens een noodgeval inrichten. Zie voor meer informatie [technische richtlijnen voor Azure tolerantie](resiliency/resiliency-technical-guidance.md).

![Opslag](./media/best-practices-availability-paired-regions/2Green.png) **Azure Storage** -geografisch redundante opslag (GRS) is standaard geconfigureerd als een Azure Storage-account is gemaakt. Met GRS worden uw gegevens automatisch driemaal gerepliceerd binnen de primaire regio en driemaal in de gekoppelde regio. Zie voor meer informatie [Azure redundantie opslagopties](storage/common/storage-redundancy.md).

![Azure SQL](./media/best-practices-availability-paired-regions/3Green.png) **Azure SQL-Databases** – met Azure SQL standaard Geo-replicatie, kunt u asynchrone replicatie van transacties met een gekoppelde regio. Met premium geo-replicatie, kunt u replicatie voor elke regio configureren in de wereld; we raden echter implementeren van deze bronnen in een gekoppelde regio voor de meeste herstel na noodgevallen. Zie voor meer informatie [Geo-replicatie in Azure SQL Database](sql-database/sql-database-geo-replication-overview.md).

![Resource Manager](./media/best-practices-availability-paired-regions/4Green.png) **Azure Resource Manager** -Resource Manager biedt inherent logische isolatie van onderdelen van service management tussen regio's. Dit betekent dat logische fouten in één regio is het minder waarschijnlijk invloed zijn op een andere.

## <a name="benefits-of-paired-regions"></a>Voordelen van het gekoppelde regio 's
In afbeelding 2 bedoelde.  

![Isolatie](./media/best-practices-availability-paired-regions/5Orange.png)
**fysieke isolatie** – wanneer mogelijk uit Azure geeft de voorkeur aan ten minste 300 mijl van de scheiding tussen datacentra in de combinatie van een regionale, hoewel dit praktisch of mogelijk niet in alle regio's. Fysieke datacenter scheiding vermindert de kans op natuurramp, civiele unrest, stroomstoringen of fysieke netwerkstoringen tegelijk die invloed hebben op beide regio's. Isolatie is onderworpen aan de beperkingen in de Geografie (Geografie grootte, power/netwerkbeschikbaarheid infrastructuur, voorschriften, enzovoort).  

![Replicatie](./media/best-practices-availability-paired-regions/6Orange.png)
**Platform geleverde replicatie** -sommige services zoals geografisch redundante opslag bieden automatische replicatie naar de gekoppelde regio.

![Herstel](./media/best-practices-availability-paired-regions/7Orange.png)
**regio herstel volgorde** : In het geval van een storing brede herstel van één regio buiten elk paar is geplaatst. Toepassingen die zijn geïmplementeerd op gekoppelde regio's zijn gegarandeerd een van de regio's met prioriteit is hersteld. Als een toepassing wordt geïmplementeerd in regio's die niet zijn gekoppeld, kan herstel worden vertraagd: in het ergste geval dat de gekozen regio's mogelijk is de laatste twee wilt herstellen.

![Updates](./media/best-practices-availability-paired-regions/8Orange.png)
**opeenvolgende updates** – systeemupdates geplande Azure worden geïmplementeerd voor gekoppelde regio's worden na elkaar (en niet op hetzelfde moment) uitvaltijd, het effect van de fouten en logische fouten in het zeldzame geval van een onjuiste minimaliseren Update.

![Gegevens](./media/best-practices-availability-paired-regions/9Orange.png)
**gegevens hand vestigingsplaats** – een regio deel uitmaakt van de dezelfde Geografie als de paar (met uitzondering van Brazilië-Zuid) om te voldoen aan de hand van vestigingsplaats eisen voor btw en het recht afdwinging bevoegdheid doeleinden.
