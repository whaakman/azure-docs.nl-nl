---
title: Een shard met behulp van hulpprogramma's voor elastische databases toevoegen | Microsoft Docs
description: Het gebruik van Elastic Scale API's naar de nieuwe shards toevoegen aan een shard is ingesteld.
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
ms.date: 04/01/2018
ms.openlocfilehash: 46580efa697c174743228fcc9eee82e0a67e1912
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52864593"
---
# <a name="adding-a-shard-using-elastic-database-tools"></a>Een shard met behulp van hulpprogramma's voor elastische databases toevoegen
## <a name="to-add-a-shard-for-a-new-range-or-key"></a>Een shard voor een nieuw bereik of een sleutel toevoegen
Toepassingen moeten vaak om toe te voegen nieuwe shards voor het afhandelen van gegevens dat wordt verwacht van nieuwe sleutels of sleutelbereiken, voor een shard-toewijzing die al bestaat. Bijvoorbeeld, een shard toepassing door Tenant-ID mogelijk nodig hebt voor het inrichten van een nieuwe shard voor een nieuwe tenant of shard maandelijkse gegevens moet mogelijk een nieuwe shard ingericht vóór het begin van elke maand nieuwe. 

Als het nieuwe bereik van waarden nog niet is onderdeel van een bestaande toewijzing, is het eenvoudig naar de nieuwe shard toevoegen en koppelen van de nieuwe sleutel of het bereik dat sharden. 

### <a name="example--adding-a-shard-and-its-range-to-an-existing-shard-map"></a>Voorbeeld: een shard en het bereik aan een bestaande shard-toewijzing toevoegen
In dit voorbeeld wordt de TryGetShard ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map._shard_map.trygetshard), [.NET](https://msdn.microsoft.com/library/azure/dn823929.aspx)) de CreateShard ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map._shard_map.createshard), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.createshard.aspx)), CreateRangeMapping ([ Java](/java/api/com.microsoft.azure.elasticdb.shard.map._range_shard_map.createrangemapping), [.NET](https://msdn.microsoft.com/library/azure/dn807221.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeShardMap`1.CreateRangeMapping\(Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeMappingCreationInfo{`0}\))) methoden, en maakt een exemplaar van de ShardLocation ([Java](/java/api/com.microsoft.azure.elasticdb.shard.base._shard_location), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardlocation.shardlocation.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardLocation.)) klasse. In het voorbeeld hieronder, een database met de naam **sample_shard_2** en alle benodigde schemaobjecten daarbinnen zijn gemaakt voor het opslaan van bereik [300, 400).  

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

Voor de versie van .NET kunt u ook PowerShell gebruiken als alternatief voor het maken van een nieuwe Shard-toewijzing. Een voorbeeld is beschikbaar [hier](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).

## <a name="to-add-a-shard-for-an-empty-part-of-an-existing-range"></a>Een shard voor een lege deel uitmaken van een bestaand bereik toevoegen
In sommige gevallen kan u mogelijk hebt al een bereik toegewezen aan een shard en gedeeltelijk ingevuld met gegevens, maar u wilt dat nu toekomstige gegevens om te worden omgeleid naar een andere shard. Bijvoorbeeld, u shard dag breed en 50 dagen al is toegewezen aan een shard hebben, maar op een dag 24 uur per dag, wilt u gegevens uit de toekomst te worden neergezet in een andere shard. De elastische database [hulpprogramma voor splitsen en samenvoegen](sql-database-elastic-scale-overview-split-and-merge.md) kunnen deze bewerking uitvoeren, maar als de verplaatsing van gegevens is niet nodig zijn (bijvoorbeeld gegevens voor het aantal dagen [25, 50), dat wil zeggen, dag 25 tot 50 uit, inclusief nog niet bestaat) kunt u dit uitvoeren volledig rechtstreeks de Shard Map Management API's gebruiken.

### <a name="example-splitting-a-range-and-assigning-the-empty-portion-to-a-newly-added-shard"></a>Voorbeeld: het splitsen van een bereik en de lege gedeelte toewijzen aan een nieuw toegevoegde shard
Een database met de naam 'sample_shard_2' en alle benodigde schemaobjecten daarbinnen zijn gemaakt.  

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

**Belangrijke**: Gebruik deze methode alleen als u zeker weet dat het bereik voor de bijgewerkte toewijzing leeg is.  De vorige methoden controleren niet voor het bereik worden verplaatst, dus het is raadzaam om op te nemen van controles in uw code.  Als rijen bestaan in het bereik worden verplaatst, wordt de verdeling van de werkelijke gegevens niet overeen met de bijgewerkte shard-toewijzing. Gebruik de [hulpprogramma voor splitsen en samenvoegen](sql-database-elastic-scale-overview-split-and-merge.md) voor het uitvoeren van de bewerking in plaats daarvan in dergelijke gevallen.  

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

