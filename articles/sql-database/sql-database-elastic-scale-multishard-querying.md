---
title: Query uitvoeren op Azure SQL-databases shard | Microsoft Docs
description: Query's uitvoeren via shards met behulp van de clientbibliotheek voor elastische database.
services: sql-database
documentationcenter: 
manager: jhubbard
author: torsteng
editor: 
ms.assetid: a4379c15-f213-4026-ab6f-a450ee9d5758
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: torsteng
ms.openlocfilehash: 33128357bd5b2bd744c5c1c3032f658ebe865d49
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2017
---
# <a name="multi-shard-querying"></a>Meerdere shard opvragen
## <a name="overview"></a>Overzicht
Met de [hulpmiddelen voor elastische databases](sql-database-elastic-scale-introduction.md), u kunt shard databaseoplossingen maken. **Uitvoeren van query's met meerdere shard** wordt gebruikt voor taken zoals verzameling/rapportage gegevens waarvoor het uitvoeren van een query die zich uitstrekt over meerdere shards. (Deze optie om te vergelijken [gegevensafhankelijke routering](sql-database-elastic-scale-data-dependent-routing.md), die al het werk wordt uitgevoerd op een enkele shard.) 

1. Ophalen van een **RangeShardMap** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map._range_shard_map), [.NET](https://msdn.microsoft.com/library/azure/dn807318.aspx)) of **ListShardMap** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map._list_shard_map), [.NET ](https://msdn.microsoft.com/library/azure/dn807370.aspx)) met behulp van de **TryGetRangeShardMap** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager._shard_map_manager.trygetrangeshardmap), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetrangeshardmap.aspx)), wordt de **TryGetListShardMap** ([ Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager._shard_map_manager.trygetlistshardmap), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetlistshardmap.aspx)), of de **GetShardMap** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager._shard_map_manager.getshardmap), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getshardmap.aspx)) methode. Zie  **[samenstellen van een ShardMapManager](sql-database-elastic-scale-shard-map-management.md#constructing-a-shardmapmanager)**  en  **[ophalen van een RangeShardMap of ListShardMap](sql-database-elastic-scale-shard-map-management.md#get-a-rangeshardmap-or-listshardmap)**.
2. Maak een **MultiShardConnection** ([Java](/java/api/com.microsoft.azure.elasticdb.query.multishard._multi_shard_connection), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardconnection.aspx)) object.
3. Maak een **MultiShardStatement of MultiShardCommand** ([Java](/java/api/com.microsoft.azure.elasticdb.query.multishard._multi_shard_statement), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.aspx)). 
4. Stel de **eigenschap CommandText** ([Java](/java/api/com.microsoft.azure.elasticdb.query.multishard._multi_shard_statement), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.commandtext.aspx#P:Microsoft.Azure.SqlDatabase.ElasticScale.Query.MultiShardCommand.CommandText)) aan een T-SQL-opdracht.
5. Voer de opdracht door het aanroepen van de **ExecuteQueryAsync of ExecuteReader** ([Java](), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.executereader.aspx)) methode.
6. Bekijk de resultaten met behulp van de **MultiShardResultSet of MultiShardDataReader** ([Java](/java/api/com.microsoft.azure.elasticdb.query.multishard._multi_shard_result_set), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multisharddatareader.aspx)) klasse. 

## <a name="example"></a>Voorbeeld
De volgende code ziet u het gebruik van meerdere shard uitvoeren van query's met behulp van een bepaalde **ShardMap** met de naam *myShardMap*. 

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

Een belangrijk verschil is het maken van meerdere shard-verbindingen. Waar **SqlConnection** is van invloed op een individuele database de **MultiShardConnection** duurt een ***verzameling shards*** als invoer. Vul de verzameling van shards uit een shard-toewijzing. De query wordt vervolgens uitgevoerd op de verzameling van shards met **UNION ALL** semantiek voor het samenstellen van een enkele algemene resultaat. U kunt desgewenst de naam van de shard waar de rij is afkomstig uit kan worden toegevoegd aan de uitvoer met behulp de **ExecutionOptions** eigenschap op de opdracht. 

Let op de aanroep van **myShardMap.GetShards()**. Deze methode haalt alle shards uit de shard-toewijzing en een eenvoudige manier om een query uitvoeren voor alle relevante databases. De verzameling van shards voor een query meerdere shard kan verfijnd verder door het uitvoeren van een LINQ-query via de verzameling geretourneerd van de aanroep aan **myShardMap.GetShards()**. In combinatie met het beleid gedeeltelijke resultaten, is de huidige mogelijkheden in meerdere shard-query ontworpen om te werken goed bij tientallen tot honderden shards.

Een beperking voor het uitvoeren van query's met meerdere shard is momenteel het gebrek aan validatie voor shards en shardlets die worden opgevraagd. Terwijl gegevensafhankelijke routering, bevestigt dat een bepaalde shard deel uitmaakt van de shard-toewijzing op het moment van query's op, Voer meerdere shard-query's niet deze controle uit. Dit kan leiden tot meerdere shard-query uitvoeren op databases die zijn verwijderd uit de shard-toewijzing.

## <a name="multi-shard-queries-and-split-merge-operations"></a>Meerdere shard-query's en gesplitste samenvoegbewerkingen
Meerdere shard-query's controleren niet of shardlets op de aangevraagde database lopende gesplitste merge-bewerkingen deelnemen. (Zie [schalen met het hulpprogramma voor elastische Database gesplitste samenvoegen](sql-database-elastic-scale-overview-split-and-merge.md).) Dit kan leiden tot inconsistenties waar rijen uit de dezelfde shardlet voor meerdere databases in dezelfde query meerdere shard weergeven. Hoogte zijn van deze beperkingen en overweeg verwerkingsstop lopende gesplitste merge-bewerkingen en wijzigingen in de shard-toewijzing tijdens het uitvoeren van query's met meerdere shard.

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]


