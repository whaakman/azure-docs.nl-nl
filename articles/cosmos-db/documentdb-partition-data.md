---
title: Partitionering en schalen in Azure Cosmos DB | Microsoft Docs
description: Meer informatie over hoe partitionering werkt in Azure Cosmos DB, partitie-sleutels en configureren met het partitioneren en het kiezen van de juiste partitiesleutel voor uw toepassing.
services: cosmos-db
author: arramac
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 702c39b4-1798-48dd-9993-4493a2f6df9e
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2017
ms.author: arramac
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 81010d91ac7fe8fa7149c52ed56af304cf4e83d9
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="partitioning-in-azure-cosmos-db-using-the-documentdb-api"></a>Partitioneren in Azure Cosmos-database met de DocumentDB-API

[Microsoft Azure Cosmos DB](../cosmos-db/introduction.md) is een globale gedistribueerde, meerdere model-database-service waarmee u snel, voorspelbare prestaties en schaalbaarheid naadloos samen met uw toepassing bereiken wanneer deze groeit. 

Dit artikel bevat een overzicht van het werken met het partitioneren van de Cosmos-DB containers met de DocumentDB-API. Zie [partitionerings- en horizontaal schalen](../cosmos-db/partition-data.md) voor een overzicht van de concepten en aanbevolen procedures voor het partitioneren van Azure Cosmos DB API. 

Download het project uit om te beginnen met code [Github](https://github.com/Azure/azure-documentdb-dotnet/tree/a2d61ddb53f8ab2a23d3ce323c77afcf5a608f52/samples/documentdb-benchmark). 

Na het lezen van dit artikel kunt u zich de volgende vragen beantwoorden:   

* Hoe werkt partitionering in Azure Cosmos DB?
* Hoe configureer partitioneren in Azure Cosmos DB
* Wat partitiesleutels zijn en hoe ik de juiste partitiesleutel kiezen voor de toepassing?

Download het project uit om te beginnen met code [Azure Cosmos DB prestaties testen stuurprogramma Sample](https://github.com/Azure/azure-documentdb-dotnet/tree/a2d61ddb53f8ab2a23d3ce323c77afcf5a608f52/samples/documentdb-benchmark). 

<!-- placeholder until we have a permanent solution-->
<a name="partition-keys"></a>
<a name="single-partition-and-partitioned-collections"></a>
<a name="migrating-from-single-partition"></a>

## <a name="partition-keys"></a>Partitiesleutels

In de DocumentDB-API geeft u de definitie van de partitie sleutel in de vorm van een JSON-pad. De volgende tabel ziet u voorbeelden van partitie basisdefinities en de waarden die overeenkomen met elke. De partitiesleutel is opgegeven als een pad, bijvoorbeeld `/department` vertegenwoordigt de eigenschap afdeling. 

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>Partitiesleutel</strong></p></td>
            <td valign="top"><p><strong>Beschrijving</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>/Department</p></td>
            <td valign="top"><p>Komt overeen met de waarde van doc.department doc waar het item is.</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Eigenschappen/naam</p></td>
            <td valign="top"><p>Komt overeen met de waarde van doc.properties.name waar doc is voor het item (de geneste eigenschap).</p></td>
        </tr>
        <tr>
            <td valign="top"><p>/ID</p></td>
            <td valign="top"><p>Komt overeen met de waarde van doc.id (sleutel-id en partitie zijn dezelfde eigenschap).</p></td>
        </tr>
        <tr>
            <td valign="top"><p>/ 'afdeling name'</p></td>
            <td valign="top"><p>Komt overeen met de waarde van doc ["Afdelingsnaam '] doc waar het item is.</p></td>
        </tr>
    </tbody>
</table>

> [!NOTE]
> De syntaxis voor de partitiesleutel is vergelijkbaar met het opgegeven pad voor indexeren beleid paden met het belangrijkste verschil is dat het pad overeenkomt met de eigenschap in plaats van de waarde, dat wil zeggen er zich geen jokerteken aan het einde. Bijvoorbeeld, geeft u afdeling /? de waarden onder afdeling indexeert, maar u /department opgeven als de definitie van de partitie. De partitiesleutel is impliciet geïndexeerd en kan niet worden uitgesloten van het indexeren met indexering beleid onderdrukkingen.
> 
> 

Bekijk hoe de keuze van de partitiesleutel heeft impact op de prestaties van uw toepassing.

## <a name="working-with-the-azure-cosmos-db-sdks"></a>Werken met de SDK's van Azure Cosmos DB
Azure Cosmos DB is ondersteuning toegevoegd voor automatische partitionering met [REST API-versie 2015-12-16](/rest/api/documentdb/). Om te kunnen gepartitioneerde containers te maken, moet u de SDK-versies 1.6.0 downloaden of hoger in een van de ondersteunde platforms SDK (.NET, Node.js, Java, Python, MongoDB). 

### <a name="creating-containers"></a>Containers maken
Het volgende voorbeeld ziet u een .NET-codefragment voor het maken van een container voor het opslaan van apparaat telemetriegegevens van 20.000 aanvraageenheden per seconde van doorvoer. De SDK stelt u de waarde OfferThroughput (die op zijn beurt stelt de `x-ms-offer-throughput` aanvraag-header in de REST-API). Hier stelt de `/deviceId` als de partitiesleutel. De keuze van de partitiesleutel wordt opgeslagen samen met de rest van de container-metagegevens, zoals naam en het indexeringsbeleid.

Voor dit voorbeeld wordt gekozen `deviceId` Aangezien we dat weten (a) omdat er een groot aantal apparaten, schrijfbewerkingen kunnen meerdere partities gelijkmatig worden verdeeld en ons toestemming te schalen van de database voor het opnemen van grote hoeveelheden gegevens en (b) veel van de aanvragen, zoals het ophalen van de meest recente lezen voor een apparaat zijn gericht op één apparaat-id en kunnen worden opgehaald uit een enkele partitie.

```csharp
DocumentClient client = new DocumentClient(new Uri(endpoint), authKey);
await client.CreateDatabaseAsync(new Database { Id = "db" });

// Container for device telemetry. Here the property deviceId will be used as the partition key to 
// spread across partitions. Configured for 10K RU/s throughput and an indexing policy that supports 
// sorting against any number or string property.
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 20000 });
```

Deze methode maakt een REST-API-aanroep om Cosmos-database en de service wordt een aantal partities op basis van de aangevraagde doorvoer inrichten. U kunt de doorvoer van een container te wijzigen wanneer de prestaties van uw behoeften ontwikkelen. 

### <a name="reading-and-writing-items"></a>Lezen en schrijven van items
Nu gaan we gegevens invoegen in Cosmos-DB. Hier volgt een voorbeeldklasse met een apparaat lezen en een aanroep naar CreateDocumentAsync invoegen van een nieuw apparaat lezen in een container. Dit is een voorbeeld gebruik van de DocumentDB-API:

```csharp
public class DeviceReading
{
    [JsonProperty("id")]
    public string Id;

    [JsonProperty("deviceId")]
    public string DeviceId;

    [JsonConverter(typeof(IsoDateTimeConverter))]
    [JsonProperty("readingTime")]
    public DateTime ReadingTime;

    [JsonProperty("metricType")]
    public string MetricType;

    [JsonProperty("unit")]
    public string Unit;

    [JsonProperty("metricValue")]
    public double MetricValue;
  }

// Create a document. Here the partition key is extracted as "XMS-0001" based on the collection definition
await client.CreateDocumentAsync(
    UriFactory.CreateDocumentCollectionUri("db", "coll"),
    new DeviceReading
    {
        Id = "XMS-001-FE24C",
        DeviceId = "XMS-0001",
        MetricType = "Temperature",
        MetricValue = 105.00,
        Unit = "Fahrenheit",
        ReadingTime = DateTime.UtcNow
    });
```

Laten we het item door de partitiesleutel en -id niet lezen, bijwerken en als laatste stap, verwijdert u deze door partitiesleutel en -id. Opmerking dat gelezen gegevens een PartitionKey-waarde bevatten (die overeenkomt met de `x-ms-documentdb-partitionkey` aanvraag-header in de REST-API).

```csharp
// Read document. Needs the partition key and the ID to be specified
Document result = await client.ReadDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });

DeviceReading reading = (DeviceReading)(dynamic)result;

// Update the document. Partition key is not required, again extracted from the document
reading.MetricValue = 104;
reading.ReadingTime = DateTime.UtcNow;

await client.ReplaceDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  reading);

// Delete document. Needs partition key
await client.DeleteDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });
```

### <a name="querying-partitioned-containers"></a>Gepartitioneerde containers doorzoeken
Als u query's gegevens in gepartitioneerde containers, routeert Cosmos DB automatisch de query naar de partities die overeenkomt met de partitie sleutelwaarden die zijn opgegeven in het filter (wanneer aanwezig). Deze query wordt bijvoorbeeld doorgestuurd naar partitie met de partitiesleutel 'XMS-0001'.

```csharp
// Query using partition key
IQueryable<DeviceReading> query = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"))
    .Where(m => m.MetricType == "Temperature" && m.DeviceId == "XMS-0001");
```
    
De volgende query heeft geen een filter voor de partitiesleutel (DeviceId) en is fanned uit op alle partities waarop deze wordt uitgevoerd op basis van de index van de partitie. Opmerking die u hebt voor het opgeven van de EnableCrossPartitionQuery (`x-ms-documentdb-query-enablecrosspartition` in de REST-API) om de SDK voor het uitvoeren van een query meerdere partities.

```csharp
// Query across partition keys
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"), 
    new FeedOptions { EnableCrossPartitionQuery = true })
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100);
```

Biedt ondersteuning voor cosmos DB [statistische functies](documentdb-sql-query.md#Aggregates) `COUNT`, `MIN`, `MAX`, `SUM` en `AVG` over gepartitioneerd containers met behulp van SQL wordt gestart met de SDK's 1.12.0 en hoger. Query's moeten een enkel samengestelde operator bevatten en moeten één waarde bevatten in de projectie.

### <a name="parallel-query-execution"></a>Parallelle queryuitvoering
De Cosmos-DB SDK's 1.9.0 en hoger ondersteuning parallelle uitvoering queryopties, waarmee u kunt uitvoeren lage latentie query's voor gepartitioneerde verzamelingen, zelfs als ze nodig hebben om een groot aantal partities touch. De volgende query is bijvoorbeeld geconfigureerd voor meerdere partities parallel worden uitgevoerd.

```csharp
// Cross-partition Order By Queries
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"), 
    new FeedOptions { EnableCrossPartitionQuery = true, MaxDegreeOfParallelism = 10, MaxBufferedItemCount = 100})
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100)
    .OrderBy(m => m.MetricValue);
```
    
U kunt parallelle queryuitvoering beheren door het afstemmen van de volgende parameters:

* Door in te stellen `MaxDegreeOfParallelism`, u kunt de mate van parallelle uitvoering, dat wil zeggen, het maximum aantal gelijktijdige netwerkverbindingen op de container partities beheren. Als u deze instelt op-1, de mate van parallelle uitvoering wordt beheerd door de SDK. Als de `MaxDegreeOfParallelism` is niet opgegeven of is ingesteld op 0, wat de standaardwaarde is, zal er een netwerk met één verbinding met de partities van de container.
* Door in te stellen `MaxBufferedItemCount`, kunt u handelt uit query latentie en client-side geheugengebruik. Als u deze parameter of stel deze optie in op-1, het aantal items in de buffer opgeslagen tijdens parallelle queryuitvoering wordt beheerd door de SDK.

Gezien de dezelfde status van de verzameling, retourneert een parallelle query resultaten in dezelfde volgorde als in seriële uitvoering. Bij het uitvoeren van een query voor cross-partitie met sorteren (ORDER BY en/of boven) geeft de query parallel meerdere partities uit Azure Cosmos DB SDK en gedeeltelijk gesorteerde resulteert in de clientzijde levert geen resultaten globaal geordende worden samengevoegd.

### <a name="executing-stored-procedures"></a>Opgeslagen procedures uitvoeren
U kunt ook atomische transacties tegen documenten met dezelfde apparaat-ID, zoals uitvoeren als u statistische functies of de meest recente toestand van een apparaat in één item onderhoudt. 

```csharp
await client.ExecuteStoredProcedureAsync<DeviceReading>(
    UriFactory.CreateStoredProcedureUri("db", "coll", "SetLatestStateAcrossReadings"),
    new RequestOptions { PartitionKey = new PartitionKey("XMS-001") }, 
    "XMS-001-FE24C");
```
   
In het volgende gedeelte kijken we hoe u naar een gepartitioneerde containers uit één partitie containers verplaatsen kunt.

## <a name="next-steps"></a>Volgende stappen
In dit artikel wordt een overzicht van het werken met partitionering van Azure DB die Cosmos-containers met de DocumentDB-API opgegeven. Zie ook [partitionerings- en horizontaal schalen](../cosmos-db/partition-data.md) voor een overzicht van de concepten en aanbevolen procedures voor het partitioneren van Azure Cosmos DB API. 

* Schaal en prestaties testen met Azure Cosmos DB uitvoeren. Zie [prestaties en schaal testen met Azure Cosmos DB](performance-testing.md) voor een voorbeeld.
* Aan de slag programmeren met de [SDK's](documentdb-sdk-dotnet.md) of de [REST-API](/rest/api/documentdb/)
* Meer informatie over [ingerichte doorvoer in Azure Cosmos-DB](request-units.md)

