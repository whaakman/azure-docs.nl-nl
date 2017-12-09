---
title: Scale-out een Azure SQL database | Microsoft Docs
description: Het gebruik van de ShardMapManager, clientbibliotheek voor elastische database
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: 0e9d647a-9ba9-4875-aa22-662d01283439
ms.service: sql-database
ms.custom: scale out apps
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: ddove
ms.openlocfilehash: fe4c8b7b2a9d199c85faf11fcd35382d586fc009
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="scale-out-databases-with-the-shard-map-manager"></a>Databases met de shard-toewijzing manager uitbreiden
Gebruik gemakkelijk als u wilt uitbreiden databases op SQL Azure, een manager shard-toewijzing. De shard-toewijzing manager is een speciale database waarin gegevens over alle shards (databases) in een set shard globale toewijzing worden bijgehouden. De metagegevens kan een toepassing verbinding maken met de juiste database op basis van de waarde van de **sharding sleutel**. Bovendien elke shard in de set bevat kaarten die bijhouden van de lokale shard-gegevens (ook wel **shardlets**). 

![Beheer van shard-kaart](./media/sql-database-elastic-scale-shard-map-management/glossary.png)

Begrijpen hoe deze toewijzingen worden opgebouwd is essentieel voor beheer van shard-toewijzing. Dit wordt gedaan met behulp van de klasse ShardMapManager ([Java](https://docs.microsoft.com/java/api/com.microsoft.azure.elasticdb.shard.mapmanager._shard_map_manager), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx)) vindt u in de [clientbibliotheek voor elastische Database](sql-database-elastic-database-client-library.md) voor het beheren van shard-kaarten.  

## <a name="shard-maps-and-shard-mappings"></a>Shard-kaarten en shard-toewijzingen
Voor elke shard, moet u het type van shard-toewijzing te maken. De keuze is afhankelijk van de database-architectuur: 

1. Één tenant per database  
2. Meerdere tenants per database (twee typen):
   1. Toewijzing van de lijst
   2. Bereik toewijzing

Voor een model voor één tenant, maakt u een **lijst toewijzing** shard-toewijzing. Het model voor één tenant wijst één database per tenant. Dit is een doeltreffend model voor SaaS-ontwikkelaars aangezien vereenvoudigt het beheer.

![Toewijzing van de lijst][1]

Het model multitenant verschillende tenants toegewezen aan een individuele database (en u kunt groepen van tenants verdelen over meerdere databases). Dit model gebruiken wanneer u verwacht dat elke tenant kleine hoeveelheden gegevens nodig hebben. In dit model tal van tenants aan een database met toewijzen **bereik toewijzing**. 

![Bereik toewijzing][2]

Of u kunt implementeren met een multitenant database model met een *lijst toewijzing* meerdere tenants toewijzen aan een individuele database. Bijvoorbeeld, DB1 wordt gebruikt voor het opslaan van informatie over het tenant-ID 1 en 5 en DB2 slaat gegevens voor de tenant 7- en 10. 

![Meerdere tenants voor één database][3] 

### <a name="supported-types-for-sharding-keys"></a>Ondersteunde typen voor sharding-sleutels
De volgende typen elastisch schalen ondersteunen als sharding sleutels:

| .NET | Java |
| --- | --- |
| geheel getal |geheel getal |
| lang |lang |
| GUID |UUID |
| Byte]  |Byte] |
| Datum/tijd | tijdstempel |
| TimeSpan | Duur|
| DateTimeOffset |offsetdatetime |

### <a name="list-and-range-shard-maps"></a>Lijst met en bereik shard-kaarten
Shard maps kunnen worden opgesteld met **lijsten met afzonderlijke sharding waarden sleutel**, of ze kunnen worden opgesteld met **bereiken van sharding waarden sleutel**. 

### <a name="list-shard-maps"></a>Lijst shard maps
**Shards** bevatten **shardlets** en de toewijzing van shardlets naar shards wordt onderhouden door een shard-toewijzing. Een **lijst shard-toewijzing** is een koppeling tussen de afzonderlijke sleutelwaarden die de shardlets identificeren en de databases die als shards fungeren.  **Lijst met toewijzingen** zijn de expliciete en andere sleutel waarden kunnen worden toegewezen aan dezelfde database. Bijvoorbeeld: sleutel 1 wordt toegewezen aan een Database en sleutelwaarden 3 en 6 verwijst naar Database B.

| Sleutel | Shard-locatie |
| --- | --- |
| 1 |Database_A |
| 3 |Database_B |
| 4 |Database_C |
| 6 |Database_B |
| ... |... |

### <a name="range-shard-maps"></a>Bereik shard maps
In een **bereik shard-toewijzing**, het bereik van de sleutel is beschreven door een combinatie van **[lage waarde, hoogwaardige)** waar de *lage waarde* is de minimale sleutel in het bereik en de *hoog Waarde* is de eerste waarde hoger is dan het bereik. 

Bijvoorbeeld: **[0, 100)** bestaat uit alle gehele getallen groter dan of gelijk 0 en kleiner dan 100. Denk eraan dat meerdere adresbereiken kunnen verwijzen naar dezelfde database en niet-aaneengesloten bereiken worden ondersteund (bijvoorbeeld [100,200) en [400,600) beide verwijzen naar de C-Database in het volgende voorbeeld.)

| Sleutel | Shard-locatie |
| --- | --- |
| [1,50) |Database_A |
| [50,100) |Database_B |
| [100,200) |Database_C |
| [400,600) |Database_C |
| ... |... |

Elk van de bovenstaande tabellen is een conceptueel voorbeeld van een **ShardMap** object. Elke rij is een eenvoudig voorbeeld van een persoon **PointMapping** (voor de lijst shard-toewijzing) of **RangeMapping** (voor de bereik shard-toewijzing) object.

## <a name="shard-map-manager"></a>Beheer van shard-kaart
In de clientbibliotheek is de shard-toewijzing manager een verzameling van shard-kaarten. De gegevens die worden beheerd door een **ShardMapManager** exemplaar wordt opgeslagen op drie locaties: 

1. **Globale Shard-toewijzing (GSM)**: opgeven van een database om te fungeren als de opslagplaats voor alle shard-kaarten en toewijzingen. Speciale tabellen en opgeslagen procedures worden automatisch gemaakt om de gegevens te beheren. Dit is meestal een kleine database en licht geopend en mag niet worden gebruikt voor andere vereisten voor de toepassing. De tabellen in een speciale schema met de naam zijn **__ShardManagement**. 
2. **Lokale Shard-toewijzing (LSM)**: elke database die u opgeeft moet een shard bevat verschillende kleine tabellen en speciale opgeslagen procedures die bevatten en beheren van shard map specifieke informatie over die shard wordt gewijzigd. Deze informatie is redundant met de informatie in de GSM en kan de toepassing in de cache shard kaart om informatie te valideren zonder belasting op de GSM; worden geplaatst de toepassing gebruikt de LSM om te bepalen of een toewijzing in de cache nog steeds geldig is. De tabellen die overeenkomt met de LSM op elke shard bevinden zich ook in het schema **__ShardManagement**.
3. **Toepassingscache**: elke toepassing exemplaar toegang tot een **ShardMapManager** object onderhoudt een lokale cache in het geheugen van de toewijzingen. Routinggegevens die onlangs zijn opgehaald worden opgeslagen. 

## <a name="constructing-a-shardmapmanager"></a>Een ShardMapManager maken
Een **ShardMapManager** -object is gemaakt met behulp van een fabriek ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager._shard_map_manager_factory), [.NET](/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory)) patroon. De **ShardMapManagerFactory.GetSqlShardMapManager** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager._shard_map_manager_factory.getsqlshardmapmanager), [.NET](/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager)) methode heeft referenties (inclusief de servernaam en databasenaam die de GSM) in de vorm van een **ConnectionString** en retourneert een exemplaar van een **ShardMapManager**.  

**Opmerking:** de **ShardMapManager** slechts één keer per app-domein, binnen de van de initialisatiecode voor een toepassing moet worden gemaakt. Maken van een extra exemplaren van ShardMapManager in hetzelfde toepassingsdomein resulteert in meer geheugen en CPU-gebruik van de toepassing. Een **ShardMapManager** mag een onbeperkt aantal shard-kaarten. Bij een enkele shard-toewijzing is mogelijk niet voldoende is voor veel toepassingen, zijn er keer wanneer verschillende sets van databases worden gebruikt voor verschillende schema of voor unieke doeleinden; in deze gevallen mogelijk meerdere shard-kaarten beter. 

In deze code wordt een toepassing probeert te openen met een bestaande **ShardMapManager** met de TryGetSqlShardMapManager ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager._shard_map_manager_factory.trygetsqlshardmapmanager), [.NET](/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager.aspx)) methode.  Als objecten die een Global **ShardMapManager** (GSM) nog niet bestaat in de database, de clientbibliotheek maakt ze er met de CreateSqlShardMapManager ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager._shard_map_manager_factory.createsqlshardmapmanager), [.NET ](/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.createsqlshardmapmanager)) methode.

```Java
// Try to get a reference to the Shard Map Manager in the shardMapManager database.
// If it doesn't already exist, then create it.
ShardMapManager shardMapManager = null;
boolean shardMapManagerExists = ShardMapManagerFactory.tryGetSqlShardMapManager(shardMapManagerConnectionString,ShardMapManagerLoadPolicy.Lazy, refShardMapManager);
shardMapManager = refShardMapManager.argValue;

if (shardMapManagerExists) {
    ConsoleUtils.writeInfo("Shard Map %s already exists", shardMapManager);
}
else {
    // The Shard Map Manager does not exist, so create it
    shardMapManager = ShardMapManagerFactory.createSqlShardMapManager(shardMapManagerConnectionString);
    ConsoleUtils.writeInfo("Created Shard Map %s", shardMapManager);
}
```

```csharp
// Try to get a reference to the Shard Map Manager via the Shard Map Manager database.  
// If it doesn't already exist, then create it. 
ShardMapManager shardMapManager; 
bool shardMapManagerExists = ShardMapManagerFactory.TryGetSqlShardMapManager(
                                        connectionString, 
                                        ShardMapManagerLoadPolicy.Lazy, 
                                        out shardMapManager); 

if (shardMapManagerExists) 
{ 
    Console.WriteLine("Shard Map Manager already exists");
} 
else
{
    // Create the Shard Map Manager. 
    ShardMapManagerFactory.CreateSqlShardMapManager(connectionString);
    Console.WriteLine("Created SqlShardMapManager"); 

    shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager(
            connectionString, 
            ShardMapManagerLoadPolicy.Lazy);

// The connectionString contains server name, database name, and admin credentials for privileges on both the GSM and the shards themselves.
} 
```

Voor de .NET-versie kunt u PowerShell gebruiken voor het maken van een nieuwe Manager van de Shard-toewijzing. Een voorbeeld is beschikbaar [hier](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).

## <a name="get-a-rangeshardmap-or-listshardmap"></a>Een RangeShardMap of ListShardMap ophalen
Na het maken van een shard kaart manager, kunt u de RangeShardMap krijgen ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map._range_shard_map), [.NET](https://msdn.microsoft.com/library/azure/dn807318.aspx)) of ListShardMap ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map._list_shard_map), [.NET](https://msdn.microsoft.com/library/azure/dn807370.aspx)) met behulp van de TryGetRangeShardMap ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager._shard_map_manager.trygetrangeshardmap), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetrangeshardmap.aspx)), de TryGetListShardMap ([Java](https://docs.microsoft.com/java/api/com.microsoft.azure.elasticdb.shard.mapmanager._shard_map_manager.trygetlistshardmap), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetlistshardmap.aspx)), of de GetShardMap ([ Java](https://docs.microsoft.com/java/api/com.microsoft.azure.elasticdb.shard.mapmanager._shard_map_manager.getshardmap), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getshardmap.aspx)) methode.

```Java
// Creates a new Range Shard Map with the specified name, or gets the Range Shard Map if it already exists.
static <T> RangeShardMap<T> createOrGetRangeShardMap(ShardMapManager shardMapManager,
            String shardMapName,
            ShardKeyType keyType) {
    // Try to get a reference to the Shard Map.
    ReferenceObjectHelper<RangeShardMap<T>> refRangeShardMap = new ReferenceObjectHelper<>(null);
    boolean isGetSuccess = shardMapManager.tryGetRangeShardMap(shardMapName, keyType, refRangeShardMap);
    RangeShardMap<T> shardMap = refRangeShardMap.argValue;

    if (isGetSuccess && shardMap != null) {
        ConsoleUtils.writeInfo("Shard Map %1$s already exists", shardMap.getName());
    }
    else {
        // The Shard Map does not exist, so create it
        try {
            shardMap = shardMapManager.createRangeShardMap(shardMapName, keyType);
        }
        catch (Exception e) {
            e.printStackTrace();
        }
        ConsoleUtils.writeInfo("Created Shard Map %1$s", shardMap.getName());
    }

    return shardMap;
}
```

```csharp
// Creates a new Range Shard Map with the specified name, or gets the Range Shard Map if it already exists.
public static RangeShardMap<T> CreateOrGetRangeShardMap<T>(ShardMapManager shardMapManager, string shardMapName)
{
    // Try to get a reference to the Shard Map.
    RangeShardMap<T> shardMap;
    bool shardMapExists = shardMapManager.TryGetRangeShardMap(shardMapName, out shardMap);

    if (shardMapExists)
    {
        ConsoleUtils.WriteInfo("Shard Map {0} already exists", shardMap.Name);
    }
    else
    {
        // The Shard Map does not exist, so create it
        shardMap = shardMapManager.CreateRangeShardMap<T>(shardMapName);
        ConsoleUtils.WriteInfo("Created Shard Map {0}", shardMap.Name);
    }

    return shardMap;
} 
```

### <a name="shard-map-administration-credentials"></a>Beheerdersrechten shard-kaart
Toepassingen beheren en manipuleren van shard-kaarten wijken af van de implementaties die gebruikmaken van de shard-toewijzingen op route-verbindingen. 

Voor het beheren van shard-toewijzingen (toevoegen of wijzigen shards, shard-kaarten, shard-toewijzingen, enz.) u instantiëren moet de **ShardMapManager** met **referenties die lezen/schrijven-bevoegdheden op zowel de GSM-database en op elk database die als een shard fungeert**. De referenties moeten ervoor zorgen dat voor de tabellen in de GSM en de LSM te schrijven shard kaart informatie wordt ingevoerd of gewijzigd, ook als voor het maken van tabellen LSM op nieuwe shards.  

Zie [referenties gebruikt voor toegang tot de clientbibliotheek voor elastische Database](sql-database-elastic-scale-manage-credentials.md).

### <a name="only-metadata-affected"></a>Alleen de metagegevens die van invloed op een
Methoden voor het invullen van of het wijzigen van de **ShardMapManager** gegevens de gegevens van de gebruiker opgeslagen in de shards zelf niet wijzigen. Bijvoorbeeld, methoden zoals **CreateShard**, **DeleteShard**, **UpdateMapping**, enzovoort. de shard kaart metagegevens alleen van invloed zijn op. Ze niet verwijdert, toevoegen of wijzigen van de gebruikersgegevens in de shards. In plaats daarvan deze methoden zijn ontworpen om te worden gebruikt in combinatie met afzonderlijke u bewerkingen uitvoeren om te maken of verwijderen werkelijke databases of die rijen uit één shard naar de andere verplaatsen een shard-omgeving opnieuw verdelen.  (De **gesplitste samenvoegen** hulpprogramma dat wordt geleverd met hulpprogramma's voor elastische database wordt gebruikgemaakt van deze API's samen met het organiseren van de daadwerkelijke gegevensverplaatsing tussen shards.) Zie [schalen met het hulpprogramma voor elastische Database gesplitste samenvoegen](sql-database-elastic-scale-overview-split-and-merge.md).

## <a name="data-dependent-routing"></a>Gegevensafhankelijke routering
De shard-toewijzing manager wordt gebruikt in toepassingen waarvoor databaseverbindingen de bewerkingen van de app-specifieke gegevens uit te voeren. Deze verbindingen moeten worden gekoppeld aan de juiste database. Dit staat bekend als **gegevensafhankelijke routering**. Exemplaar maken van een object shard kaart manager van de fabriek met referenties die alleen-lezen toegang op de database GSM hebben voor deze toepassingen. Afzonderlijke aanvragen voor latere verbindingen Geef referenties op die nodig zijn om verbinding te maken met de juiste shard-database.

Houd er rekening mee dat deze toepassingen (met behulp van **ShardMapManager** geopend met alleen-lezen referenties) geen wijzigingen aanbrengen in de kaarten of toewijzingen. Maak voor die behoeften beheerdersrechten-specifieke toepassingen of PowerShell-scripts die hogere bevoegdheden referenties opgeven, zoals eerder besproken. Zie [referenties gebruikt voor toegang tot de clientbibliotheek voor elastische Database](sql-database-elastic-scale-manage-credentials.md).

Zie voor meer informatie [gegevensafhankelijke routering](sql-database-elastic-scale-data-dependent-routing.md). 

## <a name="modifying-a-shard-map"></a>Wijzigen van een shard-toewijzing
Een shard-toewijzing kan op verschillende manieren worden gewijzigd. Alle van de volgende manieren de metagegevens met een beschrijving van de shards en hun toewijzingen wijzigen maar ze gegevens binnen de shards niet fysiek aanpassen, of komen ze maken of verwijderen van de werkelijke databases.  Sommige van de bewerkingen in de shard-toewijzing die hieronder worden beschreven moet mogelijk worden gecoördineerd met beheeracties die gegevens fysiek verplaatsen of die toevoegen en verwijderen van de databases die fungeren als shards.

Deze methoden samenwerken als de bouwstenen beschikbaar voor het wijzigen van de algemene distributie van gegevens in uw databaseomgeving shard.  

* Toevoegen of verwijderen van shards: Gebruik **CreateShard** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map._shard_map.createshard), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.createshard.aspx)) en **DeleteShard** ([Java](https://docs.microsoft.com/java/api/com.microsoft.azure.elasticdb.shard.map._shard_map.deleteshard), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.deleteshard.aspx)) van de Shardmap ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map._shard_map), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.aspx)) klasse. 
  
    De server en database voor de shard doel moeten al bestaan voor deze bewerkingen uit te voeren. Deze methoden geen invloed heeft op de databases zelf, alleen op metagegevens in de shard-toewijzing.
* Maken of verwijderen van punten of bereiken die zijn toegewezen aan de shards: Gebruik **CreateRangeMapping** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map._range_shard_map.createrangemapping), [.NET](https://msdn.microsoft.com/library/azure/dn841993.aspx)), **DeleteMapping** () [Java](/java/api/com.microsoft.azure.elasticdb.shard.map._range_shard_map.deletemapping), [.NET](https://msdn.microsoft.com/library/azure/dn824200.aspx)) van de RangeShardMapping ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map._range_shard_map), [.NET](https://msdn.microsoft.com/library/azure/dn807318.aspx)) klasse, en **CreatePointMapping**  ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map._list_shard_map.createpointmapping), [.NET](https://msdn.microsoft.com/library/azure/dn807218.aspx)) van de ListShardMap ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map._list_shard_map), [.NET](https://msdn.microsoft.com/library/azure/dn842123.aspx)) klasse.
  
    Aan de dezelfde shard kunnen veel verschillende punten of bereiken worden toegewezen. Deze methoden zijn alleen van invloed op de metagegevens - ze hebben geen invloed op alle gegevens die al aanwezig in shards. Als gegevens moeten worden verwijderd uit de database om het in overeenstemming zijn met **DeleteMapping** -bewerkingen u deze bewerkingen uitvoeren, afzonderlijk, maar in combinatie met behulp van deze methoden.  
* Bestaande bereiken splitsen in twee of aangrenzende bereiken samenvoegen: Gebruik **SplitMapping** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map._range_shard_map.splitmapping), [.NET](https://msdn.microsoft.com/library/azure/dn824205.aspx)) en **MergeMappings** () [Java](/java/api/com.microsoft.azure.elasticdb.shard.map._range_shard_map.mergemappings), [.NET](https://msdn.microsoft.com/library/azure/dn824201.aspx)).  
  
    Let op te splitsen en samenvoegen van bewerkingen **veranderen niet de shard waaraan sleutelwaarden die zijn toegewezen**. Een splitsing een bestaand bereik opgesplitst in twee delen, maar blijft beide zoals toegewezen aan de dezelfde shard. Een samenvoeging is van invloed op twee aangrenzende bereiken die al zijn toegewezen aan de dezelfde shard, ze samenvoegen tot een enkel bereik.  De verplaatsing van punten of adresbereiken zelf tussen shards moet worden gecoördineerd door middel van **UpdateMapping** in combinatie met de daadwerkelijke gegevensverplaatsing.  U kunt de **gesplitste/Merge** service die deel uitmaken van de elastische database-hulpprogramma's voor het coördineren van shard kaart wijzigingen met de verplaatsing van gegevens wanneer verkeer nodig is. 
* Opnieuw toewijzen (of verplaatsen) afzonderlijke punten of bereiken die verschillende shards: Gebruik **UpdateMapping** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map._range_shard_map.updatemapping), [.NET](https://msdn.microsoft.com/library/azure/dn824207.aspx)).  
  
    Omdat gegevens worden verplaatst van één shard naar een andere moeten mogelijk om het in overeenstemming zijn met **UpdateMapping** bewerkingen, die u wilt uitvoeren die verkeer afzonderlijk, maar in combinatie met behulp van deze methoden.
* Te ondernemen toewijzingen online als offline: Gebruik **MarkMappingOffline** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map._range_shard_map.markmappingoffline), [.NET](https://msdn.microsoft.com/library/azure/dn824202.aspx)) en **MarkMappingOnline** ([ Java](/java/api/com.microsoft.azure.elasticdb.shard.map._range_shard_map.markmappingonline), [.NET](https://msdn.microsoft.com/library/azure/dn807225.aspx)) voor het beheren van de online status van een toewijzing. 
  
    Bepaalde bewerkingen op de shard-toewijzingen zijn alleen toegestaan als een toewijzing in een 'offline' staat, met inbegrip van **UpdateMapping** en **DeleteMapping**. Als een toewijzing offline is, wordt er een fout geretourneerd in een gegevens-afhankelijke aanvraag op basis van een sleutel die is opgenomen in de toewijzing. Bovendien wanneer een bereik is eerst offline gehaald, wordt alle verbindingen met de betrokken shard worden automatisch om te voorkomen dat inconsistente of onvolledige resultaten voor query's die zijn gericht tegen bereiken wordt gewijzigd afgebroken. 

Toewijzingen zijn niet-wijzigbaar objecten in .net.  Alle methoden van het bovenstaande die Wijzig toewijzingen ongeldig ook alle verwijzingen naar deze in uw code. Uitvoeren van takenreeksen van bewerkingen die een toewijzing in een statuswijziging te vereenvoudigen retourneren alle methoden die een-toewijzing gewijzigd een nieuwe toewijzing verwijzing zodat bewerkingen kunnen worden gekoppeld. Bijvoorbeeld: voor het verwijderen van een bestaande toewijzing in shardmap sm dat de sleutel 25 bevat, kunt u uitvoeren het volgende: 

```
    sm.DeleteMapping(sm.MarkMappingOffline(sm.GetMappingForKey(25)));
```

## <a name="adding-a-shard"></a>Toevoegen van een shard
Toepassingen moeten vaak toevoegen van nieuwe shards verwerkt gegevens die wordt verwacht van de nieuwe sleutels of sleutelbereiken voor een shard-toewijzing die al bestaat. Bijvoorbeeld, een toepassing shard door Tenant-ID kan het nodig voor het inrichten van een nieuwe shard voor een nieuwe tenant of gegevens shard maandelijks moet mogelijk een nieuwe shard vóór het begin van elke nieuwe maand wordt ingericht. 

Als het nieuwe bereik sleutelwaarden nog geen deel uitmaakt van een bestaande toewijzing en geen gegevensverplaatsing nodig is, is het eenvoudig aan de nieuwe shard toevoegen en de nieuwe sleutel of het bereik op dat shard koppelen. Zie voor meer informatie over het toevoegen van nieuwe shards [toevoegen van een nieuwe shard](sql-database-elastic-scale-add-a-shard.md).

Voor scenario's waarvoor de verplaatsing van gegevens, is het hulpprogramma gesplitste samenvoegen echter vereist voor het indelen van de verplaatsing van gegevens tussen shards in combinatie met de updates nodig shard-toewijzing. Zie voor meer informatie over het gebruik van het hulpprogramma gesplitste samenvoegen [overzicht van de gesplitste samenvoegen](sql-database-elastic-scale-overview-split-and-merge.md) 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-shard-map-management/listmapping.png
[2]: ./media/sql-database-elastic-scale-shard-map-management/rangemapping.png
[3]: ./media/sql-database-elastic-scale-shard-map-management/multipleonsingledb.png
