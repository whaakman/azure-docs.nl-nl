---
title: 'Prestaties van SQL Database: servicelagen | Microsoft Docs'
description: Vergelijk SQL Database-servicelagen.
keywords: databaseopties, prestaties van de database
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: f5c5c596-cd1e-451f-92a7-b70d4916e974
ms.service: sql-database
ms.custom: overview
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 12/14/2016
ms.author: carlrab; janeng
translationtype: Human Translation
ms.sourcegitcommit: a40319d3e53c07a94bc34714ca7393c2747fb50c
ms.openlocfilehash: 340656b896763914c2f6d37c72ce1d5323d1411e


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

Wanneer u de minimale servicelaag hebt bepaald, bent u klaar om het prestatieniveau van de database te bepalen (het aantal DTU's). De Standard-prestatieniveaus S2 en S3 zijn in veel gevallen een goed uitgangspunt. Voor databases met hoge CPU- of I/O-vereisten zijn de Premium-prestatieniveaus een goed uitgangspunt. Premium biedt meer CPU en begint bij 10 x meer I/O vergeleken met het hoogste Standard-prestatieniveau.

Wanneer u een prestatieniveau hebt gekozen, kunt u de [afzonderlijke database](sql-database-scale-up.md) of de [elastische pool](sql-database-elastic-pool-manage-portal.md#change-performance-settings-of-a-pool) dynamisch omhoog of omlaag schalen op basis van het feitelijke gebruik. Als u gaat migreren, gebruik dan de [DTU-rekenmachine](http://dtucalculator.azurewebsites.net/) om het aantal benodigde DTU's te schatten. 

>
> [!VIDEO https://channel9.msdn.com/Blogs/Windows-Azure/Azure-SQL-Database-dynamically-scale-up-or-scale-down/player]
>

## <a name="single-database-service-tiers-and-performance-levels"></a>Servicelagen en prestatieniveaus van afzonderlijke databases
Voor enkele databases zijn er binnen elke servicelaag meerdere prestatieniveaus. U hebt de flexibiliteit om het niveau te kiezen dat het best voldoet aan de eisen van uw workload. Als u omhoog of omlaag wilt schalen, kunt u de lagen van de database eenvoudig wijzigen. Zie [Servicelagen en prestatieniveaus van databases wijzigen](sql-database-scale-up.md) voor meer informatie.

Ongeacht het aantal gehoste databases, krijgt de database steeds een gegarandeerd aantal resources en heeft dit geen invloed op de verwachte prestatiekenmerken van de database.

[!INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

> [!NOTE]
> Voor een gedetailleerde uitleg van de overige rijen in deze tabel met servicelagen raadpleegt u [Mogelijkheden en beperkingen van servicelagen](sql-database-performance-guidance.md#service-tier-capabilities-and-limits).
> 

## <a name="elastic-pool-service-tiers-and-performance-in-edtus"></a>Servicelagen en prestaties in eDTU's van elastische pools

In pools kunnen databases eDTU-resources delen en verbruiken zonder dat er aan elke database in de pool een specifiek prestatieniveau hoeft te worden toegewezen. Zo kan een individuele database in een Standard-pool van 0 eDTU's tot het maximumaantal eDTU's voor de database gaan dat u hebt ingesteld bij de configuratie van de pool. Zo kunnen meerdere databases met verschillende workloads efficiënt gebruikmaken van de eDTU-resources die voor de hele pool beschikbaar zijn. Zie [Prijs- en prestatieoverwegingen voor een elastische pool](sql-database-elastic-pool-guidance.md) voor meer informatie.

In de volgende tabel vindt u een beschrijving van de kenmerken van de servicelagen van de pool.

[!INCLUDE [SQL DB service tiers table for elastic pools](../../includes/sql-database-service-tiers-table-elastic-db-pools.md)]

Elke database in een pool heeft dezelfde kenmerken als die van een enkele database voor deze laag. Zo heeft de Basic-pool een limiet voor het maximumaantal sessies per pool van 4800 tot 28800, maar één database binnen die Basic-pool heeft een databaselimiet van 300 sessies.

## <a name="next-steps"></a>Volgende stappen

* Lees meer over [Pools voor elastische pools](sql-database-elastic-pool-guidance.md) en [Prijs- en prestatieoverwegingen voor elastische pools](sql-database-elastic-pool-guidance.md).
* Lees [Elastische pools controleren, beheren en van formaat veranderen](sql-database-elastic-pool-manage-portal.md) en [De prestaties van individuele databases bewaken](sql-database-single-database-monitor.md).
* Nu u bekend bent met de lagen van SQL Database, kunt u ze uitproberen met een [gratis account](https://azure.microsoft.com/pricing/free-trial/) en leren [hoe u uw eerste SQL-database maakt](sql-database-get-started.md).




<!--HONumber=Dec16_HO3-->


