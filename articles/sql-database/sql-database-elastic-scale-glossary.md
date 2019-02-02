---
title: Woordenlijst voor elastische Database-hulpprogramma's | Microsoft Docs
description: Uitleg van termen die worden gebruikt voor hulpprogramma's voor elastische databases
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 12/04/2018
ms.openlocfilehash: 446203b45744a95c32cd41d9ded26fd960ac8a22
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2019
ms.locfileid: "55561430"
---
# <a name="elastic-database-tools-glossary"></a>Woordenlijst voor elastische Database-hulpprogramma 's

De volgende voorwaarden zijn gedefinieerd voor de [hulpmiddelen voor Elastic Database](sql-database-elastic-scale-introduction.md), een functie van Azure SQL Database. De hulpprogramma's worden gebruikt voor het beheren van [shard-kaarten](sql-database-elastic-scale-shard-map-management.md), en omvatten de [-clientbibliotheek](sql-database-elastic-database-client-library.md), wordt de [hulpprogramma voor splitsen en samenvoegen](sql-database-elastic-scale-overview-split-and-merge.md), [elastische pools](sql-database-elastic-pool.md), en [query's](sql-database-elastic-query-overview.md). 

Deze voorwaarden worden gebruikt in [toe te voegen een shard met behulp van hulpprogramma's voor elastische databases](sql-database-elastic-scale-add-a-shard.md) en [problemen van shardtoewijzingen met de RecoveryManager-klasse](sql-database-elastic-database-recovery-manager.md).

![Elastische schaal-voorwaarden][1]

**Database**: Een Azure SQL-database. 

**Gegevensafhankelijke routering**: De functionaliteit waarmee een toepassing verbinding maken met een shard gegeven van een specifieke sharding-sleutel. Zie [gegevensafhankelijke routering](sql-database-elastic-scale-data-dependent-routing.md). Vergelijken met  **[multi-shard Query](sql-database-elastic-scale-multishard-querying.md)**.

**Globale shard-toewijzing**: De toewijzing tussen sleutels voor sharding en hun respectieve shards binnen een **shard set**. De globale shard-toewijzing is opgeslagen in de **shard-Toewijzingsbeheer**. Vergelijken met **lokale shard-toewijzing**.

**Lijst-shard-toewijzing**: Een shard-toewijzing in welke sharding sleutels afzonderlijk worden toegewezen. Vergelijken met **bereik-Shard-toewijzing**.   

**Lokale shard-toewijzing**: De lokale shard-toewijzing is opgeslagen op een shard, bevat toewijzingen voor de shardlets die zich op de shard bevinden.

**Multi-shard query**: De mogelijkheid om uit te geven van een query op meerdere shards; resultatensets worden geretourneerd met behulp van UNION ALL semantiek (ook wel bekend als ' fanout-query'). Vergelijken met **gegevensafhankelijke routering**.

**Multitenant** en **één tenant**: U ziet een één tenant-database en een multitenant-database:

![Databases van één of meerdere tenants](./media/sql-database-elastic-scale-glossary/multi-single-simple.png)

Hier is een weergave van **shard** databases van één of meerdere tenants. 

![Databases van één of meerdere tenants](./media/sql-database-elastic-scale-glossary/shards-single-multi.png)

**Bereik-shard-toewijzing**: Een shard-toewijzing waarin de shard-distributiestrategie is gebaseerd op meerdere bereiken met opeenvolgende waarden. 

**Verwijzen naar tabellen**: De tabellen die niet shard, maar in verschillende shards worden gerepliceerd. Postcodes kunnen bijvoorbeeld worden opgeslagen in een tabel. 

**Shard**: Een Azure SQL-database waarin gegevens uit een shard verzameling. 

**Shardflexibiliteit**: De mogelijkheid om uit te voeren beide **horizontaal schalen** en **verticaal schalen**.

**Shard tabellen**: Tabellen die zijn shard, dat wil zeggen, waarvan de gegevens is verdeeld over shards op basis van hun waarden van sharding-sleutel. 

**Sharding-sleutel**: Een kolomwaarde die bepaalt hoe gegevens worden gedistribueerd over verschillende shards. Het waardetype kan een van de volgende zijn: **int**, **bigint**, **varbinary**, of **uniqueidentifier**. 

**Shard-set**: De verzameling van shards die worden toegeschreven aan de dezelfde shard-toewijzing in de shard-toewijzing.  

**Shardlet**: Alle gegevens die zijn gekoppeld aan een enkele waarde van een sharding-sleutel op een shard. Een shardlet is de kleinste eenheid van de verplaatsing van gegevens mogelijk wanneer shard tabellen opnieuw distribueren. 

**Shard-toewijzing**: De verzameling toewijzingen tussen sharding sleutels en hun respectieve shards.

**Shard-Toewijzingsbeheer**: Een object en de gegevens beheerarchief waarin de shard map(s), shard locaties en voor een of meer sets met shard-toewijzingen.

![Toewijzingen][2]

## <a name="verbs"></a>Termen
**Horizontaal schalen**: De handeling van het schalen van (of in) een verzameling van shards toevoegen of verwijderen van shards aan een shard-toewijzing, zoals hieronder wordt weergegeven.

![Horizontaal en verticaal schalen][3]

**Merge**: De handeling shardlets uit twee shards verplaatsen naar één shard en dienovereenkomstig bijwerken van de shard-toewijzing.

**Shardlet verplaatsen**: De handeling van een enkele shardlet verplaatsen naar een andere shard. 

**Shard**: De handeling van het partitioneren van horizontaal identiek gestructureerde gegevens met meerdere databases op basis van een sharding-sleutel.

**Gesplitste**: De handeling van verschillende shardlets uit één shard verplaatsen naar een andere (meestal nieuwe) shard. Een sharding-sleutel wordt verstrekt door de gebruiker de splitspunt.

**Verticaal schalen**: De handeling van schaal omhoog (of omlaag) de compute-grootte van een afzonderlijke shard. Bijvoorbeeld, wijzigen van een shard van Standard naar Premium (die resulteert in meer bronnen). 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-glossary/glossary.png
[2]: ./media/sql-database-elastic-scale-glossary/mappings.png
[3]: ./media/sql-database-elastic-scale-glossary/h_versus_vert.png

