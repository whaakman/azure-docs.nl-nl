---
title: Query uitvoeren op shard Azure SQL-databases | Microsoft Docs
description: Query's uitvoeren voor shards met behulp van de clientbibliotheek van elastische database.
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
ms.date: 01/03/2019
ms.openlocfilehash: a7a27d8447a306bc7a3440d64cc6216ca20d2528
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54038142"
---
# <a name="multi-shard-querying-using-elastic-database-tools"></a>Multi-shard query's uitvoeren met behulp van hulpprogramma's voor elastische databases

## <a name="overview"></a>Overzicht

Met de [hulpmiddelen voor Elastic Database](sql-database-elastic-scale-introduction.md), kunt u oplossingen voor shard-database maken. **Meerdere shards uitvoeren van query's** wordt gebruikt voor taken zoals verzameling/rapportage gegevens waarvoor een query uit te voeren die zich uitstrekt over meerdere shards. (Deze contrast [gegevensafhankelijke routering](sql-database-elastic-scale-data-dependent-routing.md), die al het werk wordt uitgevoerd op een enkele shard.)

1. Krijgen een **RangeShardMap** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map.rangeshardmap), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.rangeshardmap-1)) of **ListShardMap** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map.listshardmap), [.NET ](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.listshardmap-1)) met behulp van de **TryGetRangeShardMap** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager.shardmapmanager.trygetrangeshardmap), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetrangeshardmap.aspx)), wordt de **TryGetListShardMap** ([ Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager.shardmapmanager.trygetlistshardmap), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetlistshardmap.aspx)), of de **GetShardMap** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager.shardmapmanager.getshardmap), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getshardmap.aspx)) methode. Zie [maken van een ShardMapManager](sql-database-elastic-scale-shard-map-management.md#constructing-a-shardmapmanager) en [een RangeShardMap of ListShardMap](sql-database-elastic-scale-shard-map-management.md#get-a-rangeshardmap-or-listshardmap).
2. Maak een **MultiShardConnection** ([Java](/java/api/com.microsoft.azure.elasticdb.query.multishard.multishardconnection), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.query.multishardconnection)) object.
3. Maak een **MultiShardStatement of MultiShardCommand** ([Java](/java/api/com.microsoft.azure.elasticdb.query.multishard.multishardstatement), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand)).
4. Stel de **eigenschap CommandText** ([Java](/java/api/com.microsoft.azure.elasticdb.query.multishard.multishardstatement), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand)) aan een T-SQL-opdracht.
5. Hiermee geeft u de opdracht door het aanroepen van de **ExecuteQueryAsync of ExecuteReader** ([Java](/java/api/com.microsoft.azure.elasticdb.query.multishard.multishardstatement.executeQueryAsync), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand)) methode.
6. Bekijk de resultaten met behulp van de **MultiShardResultSet of MultiShardDataReader** ([Java](/java/api/com.microsoft.azure.elasticdb.query.multishard.multishardresultset), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.query.multisharddatareader)) klasse.

## <a name="example"></a>Voorbeeld

De volgende code illustreert het gebruik van meerdere shards uitvoeren van query's met behulp van een bepaalde **ShardMap** met de naam *myShardMap*.

```csharp
using (MultiShardConnection conn = new MultiShardConnection(myShardMap.GetShards(), myShardConnectionString))
{
    using (MultiShardCommand cmd = conn.CreateCommand())
    {
        cmd.CommandText = "SELECT c1, c2, c3 FROM ShardedTable";
        cmd.CommandType = CommandType.Text;
        cmd.ExecutionOptions = MultiShardExecutionOptions.IncludeShardNameColumn;
        cmd.ExecutionPolicy = MultiShardExecutionPolicy.PartialResults;

        using (MultiShardDataReader sdr = cmd.ExecuteReader())
        {
            while (sdr.Read())
            {
                var c1Field = sdr.GetString(0);
                var c2Field = sdr.GetFieldValue<int>(1);
                var c3Field = sdr.GetFieldValue<Int64>(2);
            }
        }
    }
}
```

Een belangrijk verschil is het maken van meerdere shard-verbindingen. Waar **SqlConnection** is van invloed op een individuele database, de **MultiShardConnection** duurt een ***verzameling van shards*** als invoer. Vul de verzameling van shards van een shard-toewijzing. De query wordt vervolgens uitgevoerd op de verzameling van shards met behulp van **UNION ALL** semantiek voor het samenstellen van een enkel het uiteindelijke resultaat. (Optioneel) de naam van de shard waar de rij is afkomstig uit kan worden toegevoegd aan de uitvoer met behulp van de **ExecutionOptions** eigenschap van de opdracht.

Houd er rekening mee de aanroep van **myShardMap.GetShards()**. Deze methode haalt alle shards van de shard-toewijzing en biedt een eenvoudige manier om een query uitvoeren voor alle relevante databases. De verzameling van shards voor een query meerdere shards verfijnd worden kan verder door het uitvoeren van een LINQ-query op de verzameling geretourneerd van de aanroep aan **myShardMap.GetShards()**. In combinatie met het beleid voor gedeeltelijke resultaten, is de huidige capaciteit in meerdere shards uitvoeren van query's zijn ontworpen om goed voor tientallen tot honderden shards.

Een beperking met meerdere shards uitvoeren van query's is momenteel het ontbreken van validatie voor shards en shardlets die zijn opgevraagd. Terwijl gegevensafhankelijke routering heeft geverifieerd dat een bepaalde shard deel van de shard-toewijzing op het moment van het uitvoeren van query's uitmaakt, voer multi-shard query's niet deze controle uit. Dit kan leiden tot meerdere shard-query's die worden uitgevoerd op databases die zijn verwijderd uit de shard-toewijzing.

## <a name="multi-shard-queries-and-split-merge-operations"></a>Multi-shard query's en bewerkingen van splitsen en samenvoegen

Multi-shard query's controleren niet of shardlets in de opgevraagde database lopende bewerkingen voor splitsen en samenvoegen deelneemt. (Zie [schalen met het Elastic Database-hulpprogramma voor splitsen en samenvoegen](sql-database-elastic-scale-overview-split-and-merge.md).) Dit kan leiden tot inconsistenties wanneer rijen uit hetzelfde shardlet weergeven voor meerdere databases in dezelfde query meerdere shards. Houd rekening met deze beperkingen en rekening houden met verwerkingsstop van verbindingen lopende bewerkingen voor splitsen en samenvoegen en wijzigingen in de shard-toewijzing tijdens het uitvoeren van meerdere shard-query's.

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]