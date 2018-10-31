---
title: Woordenlijst voor elastische Database-hulpprogramma's | Microsoft Docs
description: Uitleg van termen die worden gebruikt voor hulpprogramma's voor elastische databases
services: sql-database
ms.service: sql-database
ms.subservice: elastic-scale
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 09/14/2018
ms.openlocfilehash: 7af8e25fa46155d5054d494485de336c68caa7f2
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50239063"
---
# <a name="elastic-database-tools-glossary"></a>Woordenlijst voor elastische Database-hulpprogramma 's
De volgende voorwaarden zijn gedefinieerd voor de [hulpmiddelen voor Elastic Database](sql-database-elastic-scale-introduction.md), een functie van Azure SQL Database. De hulpprogramma's worden gebruikt voor het beheren van [shard-kaarten](sql-database-elastic-scale-shard-map-management.md), en omvatten de [-clientbibliotheek](sql-database-elastic-database-client-library.md), wordt de [hulpprogramma voor splitsen en samenvoegen](sql-database-elastic-scale-overview-split-and-merge.md), [elastische pools](sql-database-elastic-pool.md), en [query's](sql-database-elastic-query-overview.md). 

Deze voorwaarden worden gebruikt in [toe te voegen een shard met behulp van hulpprogramma's voor elastische databases](sql-database-elastic-scale-add-a-shard.md) en [problemen van shardtoewijzingen met de RecoveryManager-klasse](sql-database-elastic-database-recovery-manager.md).

![Elastische schaal-voorwaarden][1]

**Database**: een Azure SQL-database. 

**Gegevensafhankelijke routering**: de functionaliteit waarmee een toepassing verbinding maken met een shard gegeven van een specifieke sharding-sleutel. Zie [gegevensafhankelijke routering](sql-database-elastic-scale-data-dependent-routing.md). Vergelijken met  **[multi-shard Query](sql-database-elastic-scale-multishard-querying.md)**.

**Globale shard-toewijzing**: de toewijzing tussen sleutels voor sharding en hun respectieve shards binnen een **shard set**. De globale shard-toewijzing is opgeslagen in de **shard-Toewijzingsbeheer**. Vergelijken met **lokale shard-toewijzing**.

**Lijst-shard-toewijzing**: een shard-toewijzing in welke sharding sleutels afzonderlijk worden toegewezen. Vergelijken met **bereik-Shard-toewijzing**.   

**Lokale shard-toewijzing**: opgeslagen op een shard, de lokale shard-toewijzing toewijzingen voor de shardlets die zich op de shard bevinden bevat.

**Multi-shard query**: de mogelijkheid om uit te geven van een query op meerdere shards; resultatensets worden geretourneerd met behulp van UNION ALL semantiek (ook wel bekend als ' fanout-query'). Vergelijken met **gegevensafhankelijke routering**.

**Multitenant** en **één tenant**: u ziet een één tenant-database en een multitenant-database:

![Databases van één of meerdere tenants](./media/sql-database-elastic-scale-glossary/multi-single-simple.png)

Hier is een weergave van **shard** databases van één of meerdere tenants. 

![Databases van één of meerdere tenants](./media/sql-database-elastic-scale-glossary/shards-single-multi.png)

**Bereik-shard-toewijzing**: een shard-toewijzing waarin de shard-distributiestrategie is gebaseerd op meerdere bereiken met opeenvolgende waarden. 

**Verwijzen naar tabellen**: tabellen die niet zijn shard, maar in verschillende shards worden gerepliceerd. Postcodes kunnen bijvoorbeeld worden opgeslagen in een tabel. 

**Shard**: een Azure SQL-database waarin gegevens uit een shard verzameling. 

**Shardflexibiliteit**: de mogelijkheid om uit te voeren beide **horizontaal schalen** en **verticaal schalen**.

**Shard tabellen**: tabellen die zijn shard, dat wil zeggen, waarvan de gegevens is verdeeld over shards op basis van hun waarden van sharding-sleutel. 

**Sharding-sleutel**: een waarde in de kolom waarmee wordt bepaald hoe gegevens worden gedistribueerd over verschillende shards. Het waardetype kan een van de volgende zijn: **int**, **bigint**, **varbinary**, of **uniqueidentifier**. 

**Shard-set**: de verzameling van shards die worden toegeschreven aan de dezelfde shard-toewijzing in de shard-toewijzing.  

**Shardlet**: alle gegevens die zijn gekoppeld aan een enkele waarde van een sharding-sleutel op een shard. Een shardlet is de kleinste eenheid van de verplaatsing van gegevens mogelijk wanneer shard tabellen opnieuw distribueren. 

**Shard-toewijzing**: de verzameling toewijzingen tussen sharding sleutels en hun respectieve shards.

**Shard-Toewijzingsbeheer**: een store, beheer van objecten en gegevens die de shard map(s), shard locaties en voor een of meer sets met shard-toewijzingen bevat.

![Toewijzingen][2]

## <a name="verbs"></a>Termen
**Horizontaal schalen**: de handeling van het schalen van (of in) een verzameling van shards toevoegen of verwijderen van shards aan een shard-toewijzing, zoals hieronder wordt weergegeven.

![Horizontaal en verticaal schalen][3]

**Samenvoegen**: de handeling van shardlets uit twee shards verplaatsen naar één shard en dienovereenkomstig bijwerken van de shard-toewijzing.

**Shardlet verplaatsen**: de handeling van een enkele shardlet verplaatsen naar een andere shard. 

**Shard**: de handeling van het partitioneren van horizontaal identiek gestructureerde gegevens met meerdere databases op basis van een sharding-sleutel.

**Gesplitste**: de handeling van verschillende shardlets uit één shard verplaatsen naar een andere (meestal nieuwe) shard. Een sharding-sleutel wordt verstrekt door de gebruiker de splitspunt.

**Verticale schaling**: de handeling van schaal omhoog (of omlaag) de compute-grootte van een afzonderlijke shard. Bijvoorbeeld, wijzigen van een shard van Standard naar Premium (die resulteert in meer bronnen). 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-glossary/glossary.png
[2]: ./media/sql-database-elastic-scale-glossary/mappings.png
[3]: ./media/sql-database-elastic-scale-glossary/h_versus_vert.png

