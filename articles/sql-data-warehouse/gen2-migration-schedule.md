---
title: Migreren van uw bestaande Azure SQL Data Warehouse naar Gen2 | Microsoft Docs
description: Instructies voor het migreren van een bestaande datawarehouse Gen2 en de migratie plannen per regio.
services: sql-data-warehouse
author: mlee3gsd
ms.author: anumjs
ms.reviewer: jrasnick
manager: craigg
ms.assetid: 04b05dea-c066-44a0-9751-0774eb84c689
ms.service: sql-data-warehouse
ms.topic: article
ms.date: 02/09/2019
ms.openlocfilehash: d21731549c55f8e3fe90711838fcd21e6770eefe
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2019
ms.locfileid: "56960697"
---
# <a name="upgrade-your-data-warehouse-to-gen2"></a>Uw datawarehouse een upgrade uitvoert naar Gen2
Microsoft helpt oppervlaktegebied de op instapniveau kosten van het uitvoeren van een datawarehouse kan verwerken, query's door toe te voegen lagere rekenlagen voor Azure SQL Data Warehouse To veeleisende Lees de volledige aankondiging over [kleine compute laag ondersteuning voor Gen2](https://azure.microsoft.com/blog/azure-sql-data-warehouse-gen2-now-supports-lower-compute-tiers/). De nieuwe aanbieding is al beschikbaar in de regio's die u hebt genoteerd in de onderstaande tabel. Voor ondersteunde regio's, kunnen bestaande Gen1 datawarehouses worden bijgewerkt naar Gen2 via een:
- **Automatische tijdens de upgrade:** Automatische updates worden niet gestart zodra de service beschikbaar in een regio is.  Wanneer automatische upgrades in een bepaalde regio start, hebben afzonderlijke upgrades van DW plaatsvinden tijdens de geselecteerde onderhoudsplanning. 
- **Zelf een upgrade naar Gen2:** Als u liever om te bepalen wanneer een upgrade uitvoert, kunt u een zelf upgraden naar Gen2 kunt uitvoeren. Als uw regio wordt nog niet ondersteund, kunt u uw DW migreren naar een regio die wordt ondersteund en voert u vervolgens een automatische upgrade naar generatie 2.

## <a name="automated-schedule-and-region-availability-table"></a>Geautomatiseerde schema en tabel van de beschikbaarheid van regio
De volgende tabel geeft een overzicht van per regio als de lagere Gen2 compute-laag beschikbaar zal zijn en wanneer automatische upgrades start. De datums zijn onderhevig aan wijzigingen. Controleren op terug als u wilt zien wanneer uw regio beschikbaar.

\* Geeft aan dat een specifiek schema voor de regio is momenteel niet beschikbaar.

| **Regio** | **Lagere Gen2 beschikbaar** | **Beginnen met automatische upgrades** |
|:--- |:--- |:--- |
| Australië - oost |Beschikbaar |1 mei 2019 |
| Australië - zuidoost |1 maart 2019 |15 juni 2019 |
| Brazilië - zuid |\* |\* |
| Canada - midden |Beschikbaar |1 mei 2019 |
| Canada - oost |\* |\* |
| US - centraal |Beschikbaar |1 mei 2019 |
| China East |\* |\* |
| China - oost 2 |\* |\* |
| China - noord |\* |\* |
| China - noord 2 |\* |\* |
| Azië - oost |Beschikbaar |1 mei 2019 |
| US - oost |Beschikbaar |16 maart 2019 |
| US - oost 2 |Beschikbaar |16 maart 2019 |
| Frankrijk - centraal |1 maart 2019 |1 mei 2019 |
| Duitsland - centraal |\* |\* |
| India - centraal |Beschikbaar |1 mei 2019 |
| India - zuid |1 maart 2019 |15 juni 2019 |
| Japan - oost |Beschikbaar |1 mei 2019 |
| Japan - west |Beschikbaar |15 juni 2019 |
| Korea - centraal |1 maart 2019 |1 mei 2019 |
| Korea - zuid |1 maart 2019 |15 juni 2019 |
| US - noord-centraal |1 maart 2019 |15 juni 2019 |
| Europa - noord |Beschikbaar |16 maart 2019 |
| US - zuid-centraal |Beschikbaar |1 mei 2019 |
| Azië - zuidoost |Beschikbaar |16 maart 2019 |
| Verenigd Koninkrijk Zuid |1 maart 2019 |1 mei 2019 |
| Verenigd Koninkrijk West |1 maart 2019 |15 juni 2019 |
| US - west-centraal |\* |\* |
| Europa -west |Beschikbaar |16 maart 2019 |
| US - west |1 maart 2019 |15 juni 2019 |
| US - west 2 |Beschikbaar |16 maart 2019 |

## <a name="automatic-upgrade-process"></a>Automatische upgradeproces

Vanaf 16 maart 2019, zullen we beginnen geautomatiseerde upgrades voor uw instanties Gen1 plannen. Om te voorkomen van onverwachte onderbrekingen van de beschikbaarheid van het datawarehouse, wordt de automatische upgrades gepland tijdens uw onderhoudsplanning. Zie voor meer informatie over planningen [een onderhoudsplanning weergeven](viewing-maintenance-schedule.md)

Het upgradeproces omvatten een korte afname in de verbinding (ongeveer 5 min.) als we uw datawarehouse opnieuw opstarten.  Nadat uw datawarehouse opnieuw is opgestart, deze worden volledig beschikbaar voor gebruik echter, een afname van prestaties kunnen optreden tijdens het upgradeproces blijft de gegevensbestanden op de achtergrond bijwerken. De totale tijd voor de prestaties achteruitgaan variëren afhankelijk van de grootte van uw gegevensbestanden.

U kunt ook het upgradeproces van de gegevens bestand versnellen door uit te voeren [Alter Index opnieuw opbouwen](sql-data-warehouse-tables-index.md) op alle primaire columnstore-tabellen met behulp van een grotere SLO en resource-klasse na het opnieuw opstarten.

> [!NOTE]
> Het herbouwen van ALTER Index is een offline bewerking en de tabellen zijn niet beschikbaar totdat het herstellen is voltooid.

## <a name="self-upgrade-to-gen2"></a>Zelf een upgrade uitvoert naar Gen2

(Optioneel) als u nu een upgrade uitvoeren wilt, kunt u zelf bijwerken door de volgende stappen op een bestaande Gen1 datawarehouse. Als u deze optie kiest, moet u voltooien voor de automatische upgrade voordat het proces voor automatische upgrade wordt gestart in deze regio. Dit zorgt ervoor dat u voorkomen dat er kosten in rekening van de automatische upgrades een conflict veroorzaken.

Er zijn twee opties bij het uitvoeren van een zelf-upgrade.  Kunt u uw huidige datawarehouse in-place upgraden of u kunt een datawarehouse Gen1 herstellen naar een exemplaar Gen2.

- [In-place upgrade](upgrade-to-latest-generation.md) -deze optie werkt uw bestaande Gen1 datawarehouse bij naar Gen2. Het upgradeproces omvatten een korte afname in de verbinding (ongeveer 5 min.) als we uw datawarehouse opnieuw opstarten.  Wanneer uw datawarehouse opnieuw is gestart, worden deze volledig beschikbaar voor gebruik. Als voor een bepaalde reden die u herstellen naar Gen1 van een exemplaar Gen2 wilt, opent u een [ondersteuningsaanvraag](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-get-started-create-support-ticket).
- [Een upgrade uitvoeren voor herstelpunt](sql-data-warehouse-restore.md) : een door de gebruiker gedefinieerde herstelpunt maken op uw huidige Gen1 datawarehouse en herstel vervolgens rechtstreeks naar een exemplaar Gen2. De bestaande Gen1 datawarehouse blijft aanwezig. Wanneer het herstel is voltooid, is uw datawarehouse Gen2 worden volledig beschikbaar voor gebruik.  Nadat u hebt alle testen en valideren processen uitgevoerd op de herstelde Gen2-exemplaar, kan het oorspronkelijke Gen1 exemplaar kan worden verwijderd.
    - Stap 1: Vanuit de Azure-portal, [maken van een door de gebruiker gedefinieerde herstelpunt](sql-data-warehouse-restore.md#create-a-user-defined-restore-point-using-the-azure-portal).
    - Stap 2: Bij het herstellen van een door de gebruiker gedefinieerde herstelpunt, de 'prestaties niveau' instellen voor de gewenste Gen2-laag.

Een periode van verminderde prestaties kan optreden tijdens het upgradeproces blijft de gegevensbestanden op de achtergrond bijwerken. De totale tijd voor de prestaties achteruitgaan variëren afhankelijk van de grootte van uw gegevensbestanden.

Als u wilt versnellen het achtergrondproces voor de migratie van gegevens, kunt u de verplaatsing van gegevens onmiddellijk forceren door uit te voeren [Alter Index opnieuw opbouwen](sql-data-warehouse-tables-index.md) op alle primaire columnstore-tabellen zou u query's op een grotere SLO en resource-klasse.

> [!NOTE]
> Het herbouwen van ALTER Index is een offline bewerking en de tabellen zijn niet beschikbaar totdat het herstellen is voltooid.

Als u problemen ondervindt met uw datawarehouse, maak een [ondersteuningsaanvraag](sql-data-warehouse-get-started-create-support-ticket.md) en verwijzen naar "Gen2 upgrade' als mogelijke oorzaak.

Zie voor meer informatie, [upgraden naar Gen2](upgrade-to-latest-generation.md).

## <a name="migration-frequently-asked-questions"></a>Veelgestelde vragen over migratie

**V: Kost Gen2 gelijk zijn aan Gen1?**
- A: Ja.

**V: Hoe de upgrades invloed is op mijn automatiseringsscripts?**
- A: Elk automatiseringsscript dat verwijst naar een Serviceniveaudoelstelling moet worden gewijzigd dat deze overeenkomen met het equivalent Gen2.  Zie de details [hier](upgrade-to-latest-generation.md#sign-in-to-the-azure-portal).

**V: Hoe lang een zelf-upgrade gewoonlijk duurt?**
- A: U kunt een upgrade uitvoert in plaats of een upgrade uitvoert van een herstelpunt.  
   - Een upgrade uitvoeren in plaats zorgt ervoor dat uw datawarehouse tijdelijk onderbreken en hervatten.  Een achtergrondproces blijft terwijl het datawarehouse online is.  
   - Het duurt langer als u een upgrade via een herstelpunt uitvoert, omdat de upgrade wordt via het volledige herstelproces.

**V: Hoe lang duurt de automatische upgrade?**
- A: De werkelijke downtime voor de upgrade is alleen de tijd die nodig is om te onderbreken en hervatten van de service, die tussen 5 tot 10 minuten. Na de korte uitvaltijd, wordt een opslagmigratie worden uitgevoerd door een achtergrondproces. De tijdsduur voor het achtergrondproces is afhankelijk van de grootte van uw datawarehouse.

**V: Wanneer wordt deze automatisch bijwerken van het plaatsvinden?**
- A: Tijdens uw onderhoudsplanning. Gebruik te maken van uw gekozen onderhoudsplanning te minimaliseren onderbreking van uw bedrijf.

**V: Wat moet ik doen als mijn upgrade achtergrondproces lijkt te zijn vastgelopen?**
 - A: Een vliegende start een opnieuw indexeren van de Columnstore-tabellen. Houd er rekening mee dat opnieuw indexeren van de tabel offline tijdens deze bewerking worden.

**V: Wat gebeurt er als Gen2 geen de Serviceniveaudoelstelling die ik heb op Gen1?**
- A: Als u een DW600 of DW1200 op Gen1 uitvoert, is het aanbevolen DW500c of DW1000c respectievelijk gebruiken omdat Gen2 meer geheugen, resources en betere prestaties dan Gen1 biedt.

**V: Kan ik geo-back-up uitschakelen?**
- A: Nee. Geo-back-up is een enterprise-functie om uw gegevens te behouden datawarehouse-beschikbaarheid in het geval van een regio niet beschikbaar. Open een [ondersteuningsaanvraag](sql-data-warehouse-get-started-create-support-ticket.md) als u nog meer opmerkingen hebt.

**V: Is er een verschil in T-SQL-syntaxis tussen Gen1 en Gen2?**
- A: Er is geen wijziging in de syntaxis van de T-SQL-taal van Gen1 naar Gen2.

**V: Gen2 biedt ondersteuning voor onderhoud Windows?**
- A: Ja.

**V: Kan ik worden om te maken van een nieuw exemplaar van de Gen1 na de upgrade van mijn regio?**
- A: Nee. Nadat de upgrade van een regio worden, het maken van nieuwe Gen1-instanties uitgeschakeld.

## <a name="next-steps"></a>Volgende stappen

- [Upgradestappen](upgrade-to-latest-generation.md)
- [Onderhoudsvensters](maintenance-scheduling.md)
- [Statusmonitor voor de resource](https://docs.microsoft.com/azure/service-health/resource-health-overview)
- [Controleer voordat u een migratie](upgrade-to-latest-generation.md#before-you-begin)
- [In-place upgrade en upgrade van een herstelpunt](upgrade-to-latest-generation.md)
- [Een door de gebruiker gedefinieerde herstelpunt maken](sql-data-warehouse-restore.md#restore-through-the-azure-portal)
- [Meer informatie over het herstellen naar Gen2](sql-data-warehouse-restore.md#restore-an-active-or-paused-database-using-the-azure-portal)
- [Een ondersteuningsaanvraag voor SQL Data Warehouse](https://go.microsoft.com/fwlink/?linkid=857950)
