---
title: Verklarende woordenlijst voor elastische Database extra | Microsoft Docs
description: Uitleg van termen die worden gebruikt voor hulpprogramma's van elastische database
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: a23a4e81-6706-452d-afc1-a550e5e47af9
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: db8ce257479888db63758e681393c0244af01ce7
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="elastic-database-tools-glossary"></a>Elastische Database extra verklarende woordenlijst
De volgende voorwaarden zijn gedefinieerd voor de [hulpmiddelen voor elastische databases](sql-database-elastic-scale-introduction.md), een functie van Azure SQL Database. De hulpprogramma's worden gebruikt voor het beheren van [shard toegewezen](sql-database-elastic-scale-shard-map-management.md), en bevatten de [clientbibliotheek](sql-database-elastic-database-client-library.md), wordt de [gesplitste merge tool](sql-database-elastic-scale-overview-split-and-merge.md), [elastische pools](sql-database-elastic-pool.md), en [query's](sql-database-elastic-query-overview.md). 

Deze termen worden gebruikt [toevoegen van een shard met hulpprogramma's van elastische Database](sql-database-elastic-scale-add-a-shard.md) en [shard kaart problemen op te lossen met behulp van de klasse RecoveryManager](sql-database-elastic-database-recovery-manager.md).

![Elastische Scale voorwaarden][1]

**Database**: een Azure SQL-database. 

**Gegevensafhankelijke routering**: de functionaliteit waarmee een toepassing verbinding maken met een shard krijgt een specifieke sharding-sleutel. Zie [gegevensafhankelijke routering](sql-database-elastic-scale-data-dependent-routing.md). Vergelijken met  **[Query meerdere Shard](sql-database-elastic-scale-multishard-querying.md)**.

**Globale shard-toewijzing**: de toewijzing tussen sharding-sleutels en hun respectieve shards binnen een **shard set**. De globale shard-toewijzing is opgeslagen de **shard kaart manager**. Vergelijken met **lokale shard-toewijzing**.

**Lijst shard-toewijzing**: een shard-toewijzing in welke sharding sleutels afzonderlijk zijn toegewezen. Vergelijken met **Shard-toewijzing bereik**.   

**Lokale shard-toewijzing**: opgeslagen op een shard, de lokale shard-toewijzing toewijzingen voor de shardlets die zich op de shard bevinden bevat.

**Meerdere shard query**: de mogelijkheid om uit te geven van een query op meerdere shards; resultatensets worden geretourneerd met UNION ALL semantiek (ook wel bekend als ' fan-out query'). Vergelijken met **gegevensafhankelijke routering**.

**Multitenant** en **één tenant**: dit betekent dat een single-tenant-database en een multitenant-database:

![Één en multitenant-databases](./media/sql-database-elastic-scale-glossary/multi-single-simple.png)

Hier volgt een weergave van **shard** één en multitenant-databases. 

![Één en multitenant-databases](./media/sql-database-elastic-scale-glossary/shards-single-multi.png)

**Bereik shard-toewijzing**: een shard-toewijzing waarin de shard-distributie-strategie is gebaseerd op meerdere bereiken met opeenvolgende waarden. 

**Verwijzen naar tabellen**: tabellen die zijn niet shard maar zijn gerepliceerd in shards. Zip-codes kunnen bijvoorbeeld worden opgeslagen in een tabel. 

**Shard**: een Azure SQL-database waarin gegevens uit een gegevensset shard wordt opgeslagen. 

**Elasticiteit shard**: de mogelijkheid om uit te voeren beide **horizontaal schalen** en **verticaal schalen**.

**Shard tabellen**: tabellen die zijn shard, dat wil zeggen, waarvan de gegevens is verdeeld over de shards op basis van hun sharding-sleutelwaarden. 

**Sharding sleutel**: een waarde in de kolom die bepaalt hoe de gegevens is verdeeld over shards. Het waardetype kan een van de volgende zijn: **int**, **bigint**, **varbinary**, of **uniqueidentifier**. 

**Shard set**: de verzameling van shards dat worden toegeschreven aan dezelfde shard-toewijzing in de shard-toewijzing manager.  

**Shardlet**: alle gegevens die zijn gekoppeld aan een enkele waarde van een sharding-sleutel op een shard. Een shardlet is de kleinste eenheid van de verplaatsing van gegevens mogelijk wanneer herdistribueren shard tabellen. 

**Shard-toewijzing**: de verzameling toewijzingen tussen sharding-sleutels en hun respectieve shards.

**Beheer van shard-toewijzing**: een store, management object en gegevens die de shard-toewijzing(en), shard-locaties en toewijzingen voor een of meer sets van shard bevat.

![Toewijzingen][2]

## <a name="verbs"></a>Termen
**Horizontaal schalen**: de handeling schalen uit (of in) een verzameling van shards door toe te voegen of te verwijderen van shards naar een shard-kaart, zoals hieronder wordt weergegeven.

![Horizontale en verticale schalen][3]

**Samenvoegen**: de handeling waarbij shardlets van twee shards verplaatst naar één shard en de shard-toewijzing dienovereenkomstig bijgewerkt.

**Shardlet verplaatsen**: de act van een enkele shardlet verplaatsen naar een andere shard. 

**Shard**: de handeling horizontaal partitioneren identiek gestructureerde gegevens over meerdere databases op basis van een sharding-sleutel.

**Gesplitste**: de handeling waarbij verschillende shardlets van één shard verplaatst naar een andere (meestal nieuwe) shard. Een sharding-sleutel wordt opgegeven door de gebruiker als de splitspunt.

**Verticale schaal**: de handeling schaal omhoog (of omlaag) het prestatieniveau van een afzonderlijke shard. Bijvoorbeeld, het wijzigen van een shard van standaard naar Premium (die resulteert in meer bronnen). 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-glossary/glossary.png
[2]: ./media/sql-database-elastic-scale-glossary/mappings.png
[3]: ./media/sql-database-elastic-scale-glossary/h_versus_vert.png

