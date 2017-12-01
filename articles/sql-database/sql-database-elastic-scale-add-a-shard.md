---
title: Toevoegen van een shard met hulpprogramma's van elastische database | Microsoft Docs
description: Hoe Elastic Scale API's met nieuwe shards toevoegen aan een shard ingesteld.
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: 62a349db-bebe-406f-a120-2f1986f2b286
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: ddove
ms.openlocfilehash: c3abbae20772cfc37fb8c58dc97209ac42dde358
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2017
---
# <a name="adding-a-shard-using-elastic-database-tools"></a>Toevoegen van een shard met hulpprogramma's van elastische Database
## <a name="to-add-a-shard-for-a-new-range-or-key"></a>Een shard voor een nieuw bereik of een sleutel toevoegen
Toepassingen moeten vaak toevoegen van nieuwe shards verwerkt gegevens die wordt verwacht van de nieuwe sleutels of sleutelbereiken voor een shard-toewijzing die al bestaat. Bijvoorbeeld, een toepassing shard door Tenant-ID kan het nodig voor het inrichten van een nieuwe shard voor een nieuwe tenant of gegevens shard maandelijks moet mogelijk een nieuwe shard vóór het begin van elke nieuwe maand wordt ingericht. 

Als het nieuwe bereik sleutelwaarden nog niet is onderdeel van een bestaande toewijzing, is het eenvoudig aan de nieuwe shard toevoegen en de nieuwe sleutel of het bereik op dat shard koppelen. 

### <a name="example--adding-a-shard-and-its-range-to-an-existing-shard-map"></a>Voorbeeld: een shard en het bereik toevoegen aan een bestaande shard-toewijzing
In dit voorbeeld gebruikt de TryGetShard ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map._shard_map.trygetshard), [.NET](https://msdn.microsoft.com/library/azure/dn823929.aspx)) de CreateShard ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map._shard_map.createshard), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.createshard.aspx)), CreateRangeMapping ([ Java](/java/api/com.microsoft.azure.elasticdb.shard.map._range_shard_map.createrangemapping), [.NET](https://msdn.microsoft.com/library/azure/dn807221.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeShardMap`1.CreateRangeMapping\(Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeMappingCreationInfo{`0}\))) methoden, en maakt een exemplaar van de ShardLocation ([Java](/java/api/com.microsoft.azure.elasticdb.shard.base._shard_location), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardlocation.shardlocation.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardLocation.)) klasse. In het voorbeeld hieronder een database met naam **sample_shard_2** en alle benodigde schemaobjecten daarbinnen zijn gemaakt voor het bereik [300, 400).  

```csharp
// sm is a RangeShardMap object.
// Add a new shard to hold the range being added. 
Shard shard2 = null; 

if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2)) 
{ 
    shard2 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_2"));  
} 

// Create the mapping and associate it with the new shard 
sm.CreateRangeMapping(new RangeMappingCreationInfo<long> 
                            (new Range<long>(300, 400), shard2, MappingStatus.Online)); 
```

Voor de .NET-versie kunt u ook PowerShell gebruiken als alternatief voor het maken van een nieuwe Manager van de Shard-toewijzing. Een voorbeeld is beschikbaar [hier](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).

## <a name="to-add-a-shard-for-an-empty-part-of-an-existing-range"></a>Toevoegen van een shard voor een leeg deel uitmaken van een bestaand bereik
In sommige gevallen u mogelijk hebt al een bereik toegewezen aan een shard en gedeeltelijk ingevuld met gegevens, maar u nu toekomstige gegevens moeten worden omgeleid naar een andere shard. Bijvoorbeeld, u shard door dagenbereik en 50 dagen al is toegewezen aan een shard hebben, maar op een dag 24, die u wilt toekomstige gegevens in een andere shard land. De elastische database [gesplitste merge tool](sql-database-elastic-scale-overview-split-and-merge.md) kunnen deze bewerking uitvoeren, maar als verplaatsing van gegevens niet nodig (bijvoorbeeld gegevens voor het bereik van dagen [25, 50), dat wil, dag 25 tot 50 exclusieve, inclusief nog niet bestaat) kunt u dit uitvoeren geheel met behulp van de Shard-toewijzing Management API's direct.

### <a name="example-splitting-a-range-and-assigning-the-empty-portion-to-a-newly-added-shard"></a>Voorbeeld: splitsen van een bereik en het lege gedeelte toewijzen aan een nieuw toegevoegde shard
Een database met naam 'sample_shard_2' en alle benodigde schemaobjecten daarbinnen zijn gemaakt.  

```csharp
// sm is a RangeShardMap object.
// Add a new shard to hold the range we will move 
Shard shard2 = null; 

if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2)) 
{ 
    shard2 = sm.CreateShard(new ShardLocation(shardServer,"sample_shard_2"));  
} 

// Split the Range holding Key 25 
sm.SplitMapping(sm.GetMappingForKey(25), 25); 

// Map new range holding [25-50) to different shard: 
// first take existing mapping offline 
sm.MarkMappingOffline(sm.GetMappingForKey(25)); 

// now map while offline to a different shard and take online 
RangeMappingUpdate upd = new RangeMappingUpdate(); 
upd.Shard = shard2; 
sm.MarkMappingOnline(sm.UpdateMapping(sm.GetMappingForKey(25), upd)); 
```

**Belangrijke**: Gebruik deze techniek alleen als u zeker weet dat het bereik voor de bijgewerkte toewijzing leeg is.  De voorgaande methoden controleren niet gegevens voor het bereik worden verplaatst, is het verstandig om op te nemen van controles in uw code.  Als rijen bestaan in het bereik worden verplaatst, wordt de werkelijke gegevensdistributie niet overeen met de bijgewerkte shard-toewijzing. Gebruik de [gesplitste merge tool](sql-database-elastic-scale-overview-split-and-merge.md) om de bewerking in plaats daarvan in dergelijke gevallen.  

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

