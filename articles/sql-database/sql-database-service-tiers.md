---
title: 'Prestaties van SQL Database: servicelagen | Microsoft Docs'
description: Vergelijk SQL Database-servicelagen.
keywords: databaseopties, prestaties van de database
services: sql-database
documentationcenter: 
author: janeng
manager: jhubbard
editor: 
ms.assetid: f5c5c596-cd1e-451f-92a7-b70d4916e974
ms.service: sql-database
ms.custom: overview
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 02/09/2017
ms.author: janeng
translationtype: Human Translation
ms.sourcegitcommit: 8ed4b3b30df6756c4e99e77476bc4c3a21bba90e
ms.openlocfilehash: e93d54910d8ed64879e66542c4c795101bc19a41


---
# <a name="sql-database-options-and-performance-understand-whats-available-in-each-service-tier"></a>Opties en prestaties van SQL Database: inzicht in wat er beschikbaar is in elke servicelaag
[Azure SQL Database](sql-database-technical-overview.md) biedt drie servicelagen (**Basic**, **Standard** en **Premium**) met meerdere prestatieniveaus om verschillende werkbelastingen te verwerken. Met hogere prestatieniveaus krijgt u toegang tot meer resources die zijn ontworpen om een steeds snellere doorvoer te leveren. U kunt [servicelagen en prestatieniveaus dynamisch wijzigen](sql-database-scale-up.md) zonder downtime. De Basic-, Standard- en Premium-servicelagen hebben alle een SLA van 99,99% beschikbaarheid, flexibele opties voor bedrijfscontinuïteit, beveiligingsfuncties en facturering per uur. 

U kunt individuele databases met eigen resources maken voor het geselecteerde [prestatieniveau](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels). U kunt ook meerdere databases beheren in een [elastische pool](sql-database-service-tiers.md#elastic-pool-service-tiers-and-performance-in-edtus) waarin de resources door de databases worden gedeeld. De resources voor individuele databases worden DTU's genoemd (Database Transaction Units) en de resources voor elastische pools worden elastische DTU's of eDTU's genoemd. Raadpleeg [Wat is een DTU?](sql-database-what-is-a-dtu.md) voor meer informatie over DTU's en eDTU's. 

In beide gevallen is er een servicelaag **Basic**, **Standard** en **Premium**. 

## <a name="choosing-a-service-tier"></a>Het kiezen van een servicelaag
In de volgende tabel vindt u voorbeelden van de lagen die het meest geschikt zijn voor verschillende toepassingsworkloads.

| Servicelaag | Beoogde workloads |
| :--- | --- |
| **Basic** | Het meest geschikt voor een kleine database, die doorgaans één actieve bewerking tegelijk ondersteunt. Voorbeelden zijn onder meer databases die worden gebruikt voor ontwikkeling of testen, of kleinschalige, onregelmatig gebruikte toepassingen. |
| **Standard** |Dit is de beste optie voor cloudtoepassingen met lage tot gemiddelde I/O-prestatievereisten die ondersteuning bieden voor meerdere gelijktijdige query's. Voorbeelden zijn onder meer werkgroep- of webtoepassingen. |
| **Premium** | Ontwikkeld voor gebruik bij een hoog transactievolume met hoge I/O-prestatievereisten, waarbij ondersteuning wordt geboden voor veel gelijktijdige gebruikers. Voorbeelden zijn databases die bedrijfskritieke toepassingen ondersteunen. |

Besluit eerst of u een individuele database wilt gebruiken of een groep databases met gedeelde resources. Bekijk de overwegingen voor het gebruik van een [elastische pool](sql-database-elastic-pool-guidance.md). Als u een beslissing gaat nemen over een servicelaag, bepaalt u eerst wat de minimaal vereiste databasekenmerken zijn:

* Max. databasegrootte voor afzonderlijke databases (max. 2 GB voor Basic, max. 250 GB voor Standard en max. 500 GB tot 1 TB voor Premium bij het hoogste prestatieniveau)
* Max. totale opslag bij een elastische pool (117 GB voor Basic, 1200 voor Standard en 750 voor Premium)
* Max. aantal databases per pool (400 voor Basic, 400 voor Standard en 500 voor Premium)
* Bewaarperiode van de databaseback-ups (7 dagen voor Basic, 35 dagen voor Standard en 35 dagen voor Premium)

Wanneer u de minimale servicelaag hebt bepaald, bent u klaar om het prestatieniveau van de database te bepalen (het aantal DTU's). De Standard-prestatieniveaus S2 en S3 zijn vaak een goed uitgangspunt. Voor databases met hoge CPU- of I/O-vereisten zijn de Premium-prestatieniveaus een goed uitgangspunt. Premium biedt meer CPU en begint bij 10 x meer I/O vergeleken met het hoogste Standard-prestatieniveau.

## <a name="single-database-service-tiers-and-performance-levels"></a>Servicelagen en prestatieniveaus van afzonderlijke databases
Voor enkele databases zijn er binnen elke servicelaag meerdere prestatieniveaus. U hebt de flexibiliteit om het niveau te kiezen dat het best voldoet aan de eisen van uw workload. Als u omhoog of omlaag wilt schalen, kunt u de lagen van de database eenvoudig wijzigen. Zie [Servicelagen en prestatieniveaus van databases wijzigen](sql-database-scale-up.md) voor meer informatie.

Ongeacht het aantal gehoste databases, krijgt de database steeds een gegarandeerd aantal resources en heeft dit geen invloed op de verwachte prestatiekenmerken van de database.

[!INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

> [!NOTE]
> Voor een gedetailleerde uitleg van de overige rijen in deze tabel met servicelagen raadpleegt u [Mogelijkheden en beperkingen van servicelagen](sql-database-performance-guidance.md#service-tier-capabilities-and-limits).
> 

## <a name="scaling-up-or-scaling-down-a-single-database"></a>Omhoog of omlaag schalen in een individuele database

Wanneer u een servicelaag en prestatieniveau hebt gekozen, kunt u een individuele database dynamisch omhoog of omlaag schalen op basis van het feitelijke gebruik. 

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-dynamically-scale-up-or-scale-down/player]
>

Als u de servicelaag en/of het prestatieniveau van een database wijzigt, wordt er een replica van de oorspronkelijke database gemaakt op het nieuwe prestatieniveau en worden vervolgens de verbindingen overgeschakeld op de replica. Er gaan geen gegevens verloren tijdens dit proces, maar tijdens het korte moment waarop we overschakelen naar de replica, worden verbindingen met de database uitgeschakeld, zodat sommige actieve transacties kunnen worden teruggedraaid. Deze tijdsduur varieert, maar is gemiddeld korter dan 4 seconden en in meer dan 99% van de gevallen minder dan 30 seconden. Als er veel transacties actief zijn op het moment dat de verbindingen worden uitgeschakeld, wordt deze tijdsduur mogelijk ook langer.  

De duur van het volledige proces voor omhoog schalen is afhankelijk van de grootte en de servicelaag van de database vóór en na de wijziging. Een 250GB-database die wordt gewijzigd naar, van of binnen een Standard-servicelaag, zou bijvoorbeeld binnen zes uur voltooid moeten zijn. Een database van dezelfde grootte die van prestatieniveau wisselt binnen de Premium-servicelaag, zou binnen drie uur voltooid moeten zijn.

* Om een database te downgraden, moet de database kleiner zijn dan de maximaal toegestane grootte van de gewenste servicelaag. 
* Als u een database upgradet waarbij [geo-replicatie](sql-database-geo-replication-portal.md) is ingeschakeld, dient u eerst de secundaire databases te upgraden naar de gewenste prestatielaag voordat u de primaire database upgradet.
* Wanneer u een downgrade uitvoert vanaf een Premium-servicelaag, dient u eerst alle geo-replicatierelaties te beëindigen. U kunt de stappen beschreven in het onderwerp [Herstellen na een storing](sql-database-disaster-recovery.md) volgen om het replicatieproces tussen de primaire en de actieve secundaire databases te beëindigen.
* De mogelijkheden om de service te herstellen verschillen voor de verschillende servicelagen. Als u een downgrade uitvoert, kunt u de mogelijkheid verliezen om naar een bepaald tijdstip te herstellen of een kortere retentieperiode hebben voor back-ups. Zie [Azure SQL Database Backup and Restore](sql-database-business-continuity.md) (Back-up maken en terugzetten van Azure SQL Database) voor meer informatie.
* De nieuwe eigenschappen voor de database worden pas toegepast nadat de wijzigingen zijn voltooid.

> [!IMPORTANT]
> Ga voor gedetailleerde stappen naar [Managing single databases with the Azure portal](sql-database-manage-single-databases-portal.md) (Individuele database beheren met Azure Portal), [Managing single databases with Powershell](sql-database-manage-single-databases-powershell.md) (Individuele database beheren met Powershell) of [Managing single databases with Transact-SQL](sql-database-manage-single-databases-tsql.md) (Individuele database beheren met Transact-SQL).
>

## <a name="elastic-pool-service-tiers-and-performance-in-edtus"></a>Servicelagen en prestaties in eDTU's van elastische pools

In pools kunnen databases eDTU-resources delen en verbruiken zonder dat er aan elke database in de pool een specifiek prestatieniveau hoeft te worden toegewezen. Zo kan een individuele database in een Standard-pool van 0 eDTU's tot het maximumaantal eDTU's voor de database gaan dat u hebt ingesteld bij de configuratie van de pool. Zo kunnen meerdere databases met verschillende workloads efficiënt gebruikmaken van de eDTU-resources die voor de hele pool beschikbaar zijn. Zie [Prijs- en prestatieoverwegingen voor een elastische pool](sql-database-elastic-pool-guidance.md) voor meer informatie.

In de volgende tabel vindt u een beschrijving van de kenmerken van de servicelagen van de pool.

[!INCLUDE [SQL DB service tiers table for elastic pools](../../includes/sql-database-service-tiers-table-elastic-pools.md)]

Elke database in een pool heeft dezelfde kenmerken als die van een enkele database voor deze laag. Zo heeft de Basic-pool een limiet voor het maximumaantal sessies per pool van 4800 tot 28800, maar één database binnen die Basic-pool heeft een databaselimiet van 300 sessies.

## <a name="scaling-up-or-scaling-down-an-elastic-pool"></a>Omhoog of omlaag schalen in een elastische pool

Wanneer u een prestatieniveau hebt gekozen, kunt u de elastische pool dynamisch omhoog of omlaag schalen op basis van het feitelijke gebruik. 

* Het wijzigen van het minimumaantal eDTU's per database of maximumaantal eDTU's per database duurt doorgaans vijf minuten of minder.
* De duur van het wijzigen van de poolgrootte (eDTU's) is afhankelijk van de gecombineerde grootte van alle databases in de pool. Wijzigingen duren gemiddeld 90 minuten of minder per 100 GB. Als bijvoorbeeld de totale ruimte van alle databases in de groep 200 GB is, is de verwachte latentie voor het wijzigen van de pool-eDTU's 3 uur of minder per pool.

> [!IMPORTANT]
> Zie [Managing elastic pools with the Azure portal](sql-database-elastic-pool-manage-portal.md) (Elastische pools beheren met Azure Portal), [Managing elastic pools with Powershell](sql-database-elastic-pool-manage-powershell.md) (Elastische pools beheren met Powershell), [Managing elastic pools with Transact-SQL](sql-database-elastic-pool-manage-tsql.md) (Elastische pools beheren met Transact-SQL) of [Managing elastic pools with C#](sql-database-elastic-pool-manage-csharp.md) (Elastische pools beheren met C#) voor gedetailleerde stappen.
>

## <a name="next-steps"></a>Volgende stappen

* Lees meer over [Pools voor elastische pools](sql-database-elastic-pool-guidance.md) en [Prijs- en prestatieoverwegingen voor elastische pools](sql-database-elastic-pool-guidance.md).
* Lees [Elastische pools controleren, beheren en van formaat veranderen](sql-database-elastic-pool-manage-portal.md) en [De prestaties van individuele databases bewaken](sql-database-single-database-monitor.md).
* Nu u bekend bent met de lagen van SQL Database, kunt u ze uitproberen met een [gratis account](https://azure.microsoft.com/pricing/free-trial/) en leren [hoe u uw eerste SQL-database maakt](sql-database-get-started.md).
* Als u gaat migreren, kunt u ook de [DTU-rekenmachine](http://dtucalculator.azurewebsites.net/) gebruiken om het aantal benodigde DTU's te schatten. 




<!--HONumber=Feb17_HO2-->


