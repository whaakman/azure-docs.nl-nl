---
title: Scale-out van een Azure SQL database | Microsoft Docs
description: Het gebruik van de ShardMapManager, clientbibliotheek voor elastic database
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
ms.openlocfilehash: cb39426e68b87a314336724a2e40cf78f888010d
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2019
ms.locfileid: "54191578"
---
# <a name="scale-out-databases-with-the-shard-map-manager"></a>Uitschalen-databases met de shard-Toewijzingsbeheer

Eenvoudig opschalen databases op SQL Azure, een shard-Toewijzingsbeheer gebruiken De shard-Toewijzingsbeheer is een speciale database die wordt onderhouden door van algemene toewijzingsinformatie over alle shards (databases) in een shard-verzameling. De metagegevens kan een toepassing verbinding maakt met de juiste database op basis van de waarde van de **sharding-sleutel**. Bovendien elke shard in de set bevat kaarten die de lokale gegevens bijhouden (ook wel **shardlets**).

![Shard-Toewijzingsbeheer](./media/sql-database-elastic-scale-shard-map-management/glossary.png)

Informatie over hoe deze kaarten zijn samengesteld is essentieel voor shard-Toewijzingsbeheer. Dit wordt gedaan met behulp van de klasse ShardMapManager ([Java](https://docs.microsoft.com/java/api/com.microsoft.azure.elasticdb.shard.mapmanager.shardmapmanager), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager)vindt u in de [Elastic Database-clientbibliotheek](sql-database-elastic-database-client-library.md) voor het beheren van shard-toewijzingen.  

## <a name="shard-maps-and-shard-mappings"></a>Shard-toewijzingen en de shard-toewijzingen

Voor elke shard, moet u het type van de shard-toewijzing te maken. De keuze is afhankelijk van de database-architectuur:

1. Met één tenant per database  
2. Meerdere tenants per database (twee typen):
   1. Toewijzing van de lijst
   2. Toewijzing van bereik

Voor een model met één tenant, maakt u een **lijst-toewijzing** shard-toewijzing. Het model met één tenant wordt één database per tenant toegewezen. Dit is een doeltreffend model voor SaaS-ontwikkelaars omdat dit vereenvoudigt het beheer.

![Toewijzing van de lijst][1]

Het model met meerdere tenants verschillende tenants toegewezen aan een individuele database (en u kunt groepen van tenants verdelen over meerdere databases). Dit model gebruiken wanneer u verwacht elke tenant dat hebben kleine Gegevensbehoeften. In dit model kunt u een bereik van tenants toewijzen voor het gebruik van een database **bereik toewijzing**.

![Toewijzing van bereik][2]

Of u kunt implementeren met een multitenant-database-model met een *lijst toewijzing* meerdere tenants toewijzen aan een individuele database. Bijvoorbeeld, DB1 wordt gebruikt voor het opslaan van informatie over de tenant-ID 1 en 5 en DB2 slaat gegevens voor de tenant 7- en 10.

![Meerdere tenants in één DB][3]

### <a name="supported-types-for-sharding-keys"></a>Ondersteunde typen voor sharding-sleutels

Elastisch schalen ondersteuning van de volgende typen als sharding sleutels:

| .NET | Java |
| --- | --- |
| geheel getal |geheel getal |
| lang |lang |
| GUID |uuid |
| byte[]  |byte[] |
| datum/tijd | tijdstempel |
| TimeSpan | duur|
| datetimeoffset |offsetdatetime |

### <a name="list-and-range-shard-maps"></a>Lijst met en het bereik-shard-kaarten

Shard-toewijzingen kunnen worden opgesteld met **lijsten met afzonderlijke sharding waarden sleutel**, of ze kunnen worden opgesteld met **bereiken van sharding-sleutel waarden**.

### <a name="list-shard-maps"></a>Lijst-shard-kaarten

**Shards** bevatten **shardlets** en de toewijzing van shardlets naar shards wordt onderhouden door een shard-toewijzing. Een **lijst-shard-toewijzing** is een koppeling tussen de afzonderlijke sleutelwaarden die de shardlets identificeren en de databases die als shards fungeren.  **Lijst met toewijzingen** zijn de expliciete en verschillende sleutel waarden kunnen worden toegewezen aan dezelfde database. Bijvoorbeeld, waarde van sleutel 1 toegewezen aan een Database en sleutelwaarden 3 en 6 beide toegewezen aan Database B.

| Sleutel | Shard-locatie |
| --- | --- |
| 1 |Database_A |
| 3 |Database_B |
| 4 |Database_C |
| 6 |Database_B |
| ... |... |

### <a name="range-shard-maps"></a>Bereik-shard-kaarten

In een **bereik-shard-toewijzing**, het bereik van de sleutel wordt beschreven door een paar **[lage waarde, hoge waarde)** waar de *lage waarde* is de minimale sleutel in het bereik en de *hoog Waarde* is de eerste waarde die hoger is dan het bereik.

Bijvoorbeeld, **[0, 100)** bestaat uit alle gehele getallen groter dan of gelijk zijn aan 0 en kleiner dan 100. Houd er rekening mee dat meerdere adresbereiken kunnen verwijzen naar dezelfde database en niet-aaneengesloten bereiken worden ondersteund (bijvoorbeeld [100,200) en [400,600) verwijzen beide naar Database C in het volgende voorbeeld.)

| Sleutel | Shard-locatie |
| --- | --- |
| [1,50) |Database_A |
| [50,100) |Database_B |
| [100,200) |Database_C |
| [400,600) |Database_C |
| ... |... |

Elk van de bovenstaande tabellen is een voorbeeld van een concept van een **ShardMap** object. Elke rij is een eenvoudig voorbeeld van een individu **PointMapping** (voor de lijst-shard-toewijzing) of **RangeMapping** (voor de bereik-shard-toewijzing) object.

## <a name="shard-map-manager"></a>Shard-Toewijzingsbeheer

In de clientbibliotheek is de shard-Toewijzingsbeheer een verzameling van shard-toewijzingen. De gegevens die worden beheerd door een **ShardMapManager** exemplaar wordt opgeslagen op drie locaties:

1. **Globale Shard-toewijzing (GSM)**: U opgeven een database om te fungeren als de opslagplaats voor alle van de shard-toewijzingen en toewijzingen. Speciale tabellen en opgeslagen procedures worden automatisch gemaakt om de gegevens te beheren. Dit is doorgaans een kleine database en licht toegankelijk is, en mag niet worden gebruikt voor andere behoeften van de toepassing. De tabellen zijn in een speciale schema met de naam **__ShardManagement**.
2. **Lokale Shard-toewijzing (LSM)**: Elke database die u opgeeft als een shard worden gewijzigd voor het aantal kleine tabellen en speciale opgeslagen procedures die bevatten en beheren van specifieke shard voor shard kaart informatie bevatten. Deze informatie is redundant zijn met de informatie in de GSM en hierdoor kan de toepassing in de cache shard-kaart om informatie te valideren zonder een elke belasting door op de GSM; worden geplaatst de toepassing gebruikt de LSM om te bepalen of er een toewijzing in de cache nog steeds geldig is. De tabellen die overeenkomt met de LSM op elke shard zich ook in het schema **__ShardManagement**.
3. **Toepassingscache**: Elke toepassing exemplaar toegang tot een **ShardMapManager** object onderhoudt een lokale cache in het geheugen van de toewijzingen. Informatie over de routering die onlangs zijn opgehaald worden opgeslagen.

## <a name="constructing-a-shardmapmanager"></a>Maken van een ShardMapManager

Een **ShardMapManager** object is gemaakt met behulp van een factory ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager.shardmapmanagerfactory), [.NET](/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory)) patroon. De **ShardMapManagerFactory.GetSqlShardMapManager** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager.shardmapmanagerfactory.getsqlshardmapmanager), [.NET](/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager)) methode gebruikt de referenties (met inbegrip van de servernaam en databasenaam die de GSM) in de vorm van een **ConnectionString** en retourneert een exemplaar van een **ShardMapManager**.  

**Opmerking:** De **ShardMapManager** slechts één keer per app-domein, in de van initialisatiecode voor een toepassing moet worden gemaakt. Het maken van extra exemplaren van ShardMapManager in hetzelfde toepassingsdomein resulteert in meer geheugen en CPU-gebruik van de toepassing. Een **ShardMapManager** kan een onbeperkt aantal shard-toewijzingen bevatten. Het is mogelijk dat een enkele shard-toewijzing voldoende is voor veel toepassingen, maar er zijn tijden wanneer verschillende sets databases worden gebruikt voor een ander schema of voor unieke doeleinden; in deze gevallen kunnen de voorkeur worden meerdere shard-toewijzingen.

In deze code wordt een toepassing probeert te openen met een bestaande **ShardMapManager** met de TryGetSqlShardMapManager ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager.shardmapmanagerfactory.trygetsqlshardmapmanager), [.NET](/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager) methode. Als objecten die een globale **ShardMapManager** (GSM) nog niet bestaat in de database, de clientbibliotheek maakt met behulp van de CreateSqlShardMapManager ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager.shardmapmanagerfactory.createsqlshardmapmanager), [.NET](/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.createsqlshardmapmanager)) methode.

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

U kunt PowerShell gebruiken om te maken van een nieuwe Shard-toewijzing voor de versie van .NET. Een voorbeeld is beschikbaar [hier](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).

## <a name="get-a-rangeshardmap-or-listshardmap"></a>Een RangeShardMap of ListShardMap ophalen

Na het maken van een shard-Toewijzingsbeheer, krijgt u de RangeShardMap ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map.rangeshardmap), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.rangeshardmap-1)) of ListShardMap ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map.listshardmap), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.listshardmap-1)) met behulp van de TryGetRangeShardMap ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager.shardmapmanager.trygetrangeshardmap), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetrangeshardmap)), de TryGetListShardMap ([Java](https://docs.microsoft.com/java/api/com.microsoft.azure.elasticdb.shard.mapmanager.shardmapmanager.trygetlistshardmap), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetlistshardmap)), of de GetShardMap ([ Java](https://docs.microsoft.com/java/api/com.microsoft.azure.elasticdb.shard.mapmanager.shardmapmanager.getshardmap), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getshardmap)) methode.

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

### <a name="shard-map-administration-credentials"></a>Shard-kaart beheer van referenties

Toepassingen beheren en manipuleren van shard-toewijzingen zijn anders dan de opdrachten die gebruikmaken van de shard-toewijzingen op route-verbindingen.

Voor het beheer van shard-toewijzingen (toevoegen of wijzigen van shards, shard-toewijzingen, shard-toewijzingen, enz.) u moet exemplaar maken van de **ShardMapManager** met behulp van **lezen/schrijven referenties met bevoegdheden op zowel de GSM-database en op elk database die als een shard fungeert**. De referenties moeten toestaan voor schrijfbewerkingen ten opzichte van de tabellen in de GSM en LSM informatie voor shard-toewijzing is ingevoerd of gewijzigd, ook als u voor het maken van LSM tabellen op de nieuwe shards.  

Zie [referenties gebruikt voor toegang tot de clientbibliotheek voor Elastic Database](sql-database-elastic-scale-manage-credentials.md).

### <a name="only-metadata-affected"></a>Alleen de metagegevens die zijn beïnvloed

Methoden die worden gebruikt voor het invullen van of het wijzigen van de **ShardMapManager** gegevens de gegevens die zijn opgeslagen in de shards zelf niet wijzigen. Bijvoorbeeld, methoden, zoals **CreateShard**, **DeleteShard**, **UpdateMapping**, enz. de shard map metagegevens alleen van invloed zijn op. Ze niet verwijderen, toevoegen of wijzigen van de gebruikersgegevens in de shards. In plaats daarvan deze methoden zijn ontworpen om te worden gebruikt in combinatie met afzonderlijke bewerkingen die u uitvoeren als u wilt maken of verwijderen werkelijke databases, of die rijen uit een shard naar de andere verplaatsen opnieuw verdelen van een gedeelde omgeving.  (De **splitsen en samenvoegen** hulpprogramma dat wordt geleverd met hulpmiddelen voor elastic database maakt gebruik van deze API's, samen met het organiseren van de daadwerkelijke gegevensverplaatsing tussen shards.) Zie [schalen met het Elastic Database-hulpprogramma voor splitsen en samenvoegen](sql-database-elastic-scale-overview-split-and-merge.md).

## <a name="data-dependent-routing"></a>Gegevensafhankelijke routering

De shard-Toewijzingsbeheer wordt gebruikt in toepassingen waarvoor databaseverbindingen de bewerkingen van de app-specifieke gegevens uit te voeren. Deze verbindingen moeten worden gekoppeld aan de juiste database. Dit staat bekend als **gegevensafhankelijke routering**. Exemplaar maken van een object shard map manager van de gegevensfactory op basis van de referenties die alleen-lezen toegang op de GSM-database hebben voor deze toepassingen. Afzonderlijke aanvragen voor latere verbindingen opgeven de referenties die nodig zijn om verbinding te maken met de juiste shard-database.

Houd er rekening mee dat deze toepassingen (met behulp van **ShardMapManager** geopend met alleen-lezen referenties) geen wijzigingen aanbrengen in de kaarten of -toewijzingen. Voor deze behoeften maken met beheerdersrechten-specifieke toepassingen of PowerShell-scripts die hogere bevoegdheden van referenties, zoals eerder is besproken. Zie [referenties gebruikt voor toegang tot de clientbibliotheek voor Elastic Database](sql-database-elastic-scale-manage-credentials.md).

Zie voor meer informatie, [gegevensafhankelijke routering](sql-database-elastic-scale-data-dependent-routing.md).

## <a name="modifying-a-shard-map"></a>Een shard-toewijzing wijzigen

Een shard-toewijzing kan op verschillende manieren worden gewijzigd. Alle van de volgende methoden de metagegevens met een beschrijving van de shards en hun toewijzingen, wijzigen, maar deze gegevens in de shards niet fysiek wijzigen, noch doen ze maken of verwijderen van de werkelijke databases.  Enkele van de bewerkingen op de shard-toewijzing die hieronder worden beschreven moet mogelijk worden gecoördineerd met beheeracties fysiek verplaatsen van gegevens of die toevoegen en verwijderen van databases die fungeren als shards.

Deze methoden samenwerken als de bouwstenen die beschikbaar zijn voor het wijzigen van de algehele verdeling van gegevens in uw omgeving shard-database.  

* Toevoegen of verwijderen van shards: Gebruik **CreateShard** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map.shardmap.createshard), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.createshard)) en **DeleteShard** ([Java](https://docs.microsoft.com/java/api/com.microsoft.azure.elasticdb.shard.map.shardmap.deleteshard), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.deleteshard)) van de shardmap ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map.shardmap), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap)) klasse.
  
    De server en database voor de doel-shard moeten al bestaan voor deze bewerkingen om uit te voeren. Deze methoden geen invloed heeft op de databases zelf, alleen op metagegevens in de shard-toewijzing.
* Maken of verwijderen van punten of bereiken die zijn toegewezen aan de shards: Gebruik **CreateRangeMapping** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map._rangeshardmap.createrangemapping), [.NET](https://docs.microsoft.com/previous-versions/azure/dn841993(v=azure.100))), **DeleteMapping** () [Java](/java/api/com.microsoft.azure.elasticdb.shard.map.rangeshardmap.deletemapping), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.rangeshardmap-1)) van de RangeShardMapping ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map.rangeshardmap), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.rangeshardmap-1)) klasse en **CreatePointMapping**  ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map.listshardmap.createpointmapping), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.listshardmap-1)) van de ListShardMap ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map.listshardmap), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.listshardmap-1)) klasse.
  
    Veel verschillende punten of bereiken kunnen worden toegewezen aan de dezelfde shard. Deze methoden zijn alleen van invloed op metagegevens: ze hebben geen invloed op alle gegevens die al aanwezig in shards. Als de gegevens moeten worden verwijderd uit de database om te worden consistent zijn met **DeleteMapping** bewerkingen u deze bewerkingen uitvoeren, afzonderlijk maar in combinatie met behulp van deze methoden.  
* Bestaande bereiken splitsen in twee of aaneengesloten bereiken samenvoegen in één: Gebruik **SplitMapping** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map.rangeshardmap.splitmapping), [.NET](https://msdn.microsoft.com/library/azure/dn824205.aspx)) en **MergeMappings** () [Java](/java/api/com.microsoft.azure.elasticdb.shard.map.rangeshardmap.mergemappings), [.NET](https://msdn.microsoft.com/library/azure/dn824201.aspx)).  
  
    Houd er rekening mee dat splitsen en samenvoegen van bewerkingen **veranderen niet de shard waaraan sleutelwaarden zijn toegewezen**. Een splitsing een bestaand bereik opgesplitst in twee delen, maar blijft beide als toegewezen aan de dezelfde shard. Een samenvoeging van invloed op twee aaneengesloten bereiken die al zijn toegewezen aan de dezelfde shard, ze samenvoegen tot een enkel bereik.  De verplaatsing van punten of bereiken zelf tussen shards moet worden gecoördineerd met behulp van **UpdateMapping** in combinatie met de daadwerkelijke gegevensverplaatsing.  U kunt de **splitsen ensamenvoegen/** service deel uit van de hulpmiddelen voor elastic database voor de coördinatie van shard map wijzigingen met de gegevens worden verplaatst, wanneer verkeer nodig is.
* Opnieuw toewijzen (of verplaatsen) afzonderlijke punten of bereiken aan verschillende shards: Gebruik **UpdateMapping** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map.rangeshardmap.updatemapping), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.rangeshardmap-1)).  
  
    Omdat gegevens mogelijk moet uit één shard worden verplaatst naar een andere om te worden consistent zijn met **UpdateMapping** bewerkingen, die u wilt uitvoeren die verkeer afzonderlijk maar in combinatie met behulp van deze methoden.

* Toewijzingen online en offline uitvoeren: Gebruik **MarkMappingOffline** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map.rangeshardmap.markmappingoffline), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.rangeshardmap-1)) en **MarkMappingOnline** ([ Java](/java/api/com.microsoft.azure.elasticdb.shard.map.rangeshardmap.markmappingonline), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.rangeshardmap-1)) voor het beheren van de online status van een toewijzing.
  
    Bepaalde bewerkingen op de shard-toewijzingen zijn alleen toegestaan als een toewijzing 'offline' status is, met inbegrip van **UpdateMapping** en **DeleteMapping**. Wanneer een toewijzing offline is, wordt er een fout geretourneerd in een gegevens-afhankelijke aanvraag op basis van een sleutel die is opgenomen in de toewijzing. Bovendien wanneer een bereik eerst offline is gehaald, wordt alle verbindingen met de betrokken shard worden automatisch om te voorkomen dat inconsistente of onvolledige resultaten voor query's die zijn gericht tegen bereiken gewijzigd beëindigd.

Toewijzingen zijn onveranderd objecten in .net.  Alle methoden van het bovenstaande die toewijzingen wijzigen ook alle verwijzingen naar deze in uw code ongeldig te maken. Als u wilt maken het gemakkelijker om uit te voeren reeksen van bewerkingen die invloed op de status van een toewijzing, retourneert alle methoden die een toewijzing wijzigen een verwijzing voor nieuwe toewijzing, zodat bewerkingen kunnen worden gekoppeld. Bijvoorbeeld, als u wilt verwijderen van een bestaande toewijzing in shardmap sm dat de sleutel 25 bevat, kunt u uitvoeren de volgende:

```
    sm.DeleteMapping(sm.MarkMappingOffline(sm.GetMappingForKey(25)));
```

## <a name="adding-a-shard"></a>Een shard toevoegen

Toepassingen moeten vaak om toe te voegen nieuwe shards voor het afhandelen van gegevens dat wordt verwacht van nieuwe sleutels of sleutelbereiken, voor een shard-toewijzing die al bestaat. Bijvoorbeeld, een shard toepassing door Tenant-ID mogelijk nodig hebt voor het inrichten van een nieuwe shard voor een nieuwe tenant of shard maandelijkse gegevens moet mogelijk een nieuwe shard ingericht vóór het begin van elke maand nieuwe.

Als het nieuwe bereik van de sleutelwaarden die zijn nog geen deel uitmaakt van een bestaande toewijzing en geen gegevensverplaatsing nodig is, is het eenvoudig naar de nieuwe shard toevoegen en koppelen van de nieuwe sleutel of het bereik dat sharden. Zie voor meer informatie over het toevoegen van nieuwe shards [toe te voegen een nieuwe shard](sql-database-elastic-scale-add-a-shard.md).

Voor scenario's waarvoor de verplaatsing van gegevens, is het hulpprogramma voor splitsen en samenvoegen echter vereist voor het indelen van de gegevensverplaatsing tussen shards in combinatie met de updates nodig shard-kaart. Zie voor meer informatie over het gebruik van het hulpprogramma voor splitsen en samenvoegen [overzicht van splitsen en samenvoegen](sql-database-elastic-scale-overview-split-and-merge.md)

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-shard-map-management/listmapping.png
[2]: ./media/sql-database-elastic-scale-shard-map-management/rangemapping.png
[3]: ./media/sql-database-elastic-scale-shard-map-management/multipleonsingledb.png
