---
title: Partitioneren en schalen in Azure Cosmos DB | Microsoft Docs
description: Meer informatie over hoe partitionering werkt in Azure Cosmos DB, over het configureren van de partitionering en partitioneren van sleutels en hoe de juiste partitiesleutel voor uw toepassing kiest.
services: cosmos-db
author: rafats
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: na
ms.topic: conceptual
ms.date: 05/24/2017
ms.author: rafats
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7d44d3933a132158a6dfca8201919eb72541f276
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2018
ms.locfileid: "52163242"
---
# <a name="partitioning-in-azure-cosmos-db-using-the-sql-api"></a>Partitionering in Azure Cosmos DB met behulp van de SQL-API

[Microsoft Azure Cosmos DB](../cosmos-db/introduction.md) is een wereldwijd gedistribueerde, multi-model databaseservice waarmee u kunt snelle en voorspelbare prestaties en schaal naadloos samen met uw toepassing bereiken als ze groeien. 

Dit artikel bevat een overzicht van het werken met het partitioneren van Cosmos DB-containers met de SQL-API. Zie [partitionering en horizontaal schalen](../cosmos-db/partition-data.md) voor een overzicht van concepten en aanbevolen procedures voor het partitioneren van met een Azure Cosmos DB-API. 

Als u wilt aan de slag met code, downloadt u het project uit [Github](https://github.com/Azure/azure-documentdb-dotnet/tree/a2d61ddb53f8ab2a23d3ce323c77afcf5a608f52/samples/documentdb-benchmark). 

Na het lezen van dit artikel, kunt u zich de volgende vragen beantwoorden:   

* Hoe werkt partitionering in Azure Cosmos DB?
* Hoe configureer ik partitionering in Azure Cosmos DB
* Wat partitiesleutels zijn en hoe ik de juiste partitiesleutel kiezen voor mijn toepassing?

Als u wilt aan de slag met code, downloadt u het project uit [Azure Cosmos DB prestaties testen stuurprogramma Sample](https://github.com/Azure/azure-documentdb-dotnet/tree/a2d61ddb53f8ab2a23d3ce323c77afcf5a608f52/samples/documentdb-benchmark). 

<!-- placeholder until we have a permanent solution-->
<a name="partition-keys"></a>
<a name="single-partition-and-partitioned-collections"></a>
<a name="migrating-from-single-partition"></a>

## <a name="partition-keys"></a>Partitiesleutels

In de SQL-API geeft u de definitie van de partitie sleutel in de vorm van een JSON-pad. De volgende tabel ziet u voorbeelden van de partitie belangrijke definities en de waarden die overeenkomen met elke. De partitiesleutel is opgegeven als een pad, bijvoorbeeld `/department` Hiermee geeft u de eigenschap afdeling. 

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>Partitiesleutel</strong></p></td>
            <td valign="top"><p><strong>Beschrijving</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>/Department</p></td>
            <td valign="top"><p>Komt overeen met de waarde van doc.department doc-bestand waar het item is.</p></td>
        </tr>
        <tr>
            <td valign="top"><p>/ Eigenschappen/naam</p></td>
            <td valign="top"><p>Komt overeen met de waarde van doc.properties.name waar doc-bestand is voor het item (geneste eigenschap).</p></td>
        </tr>
        <tr>
            <td valign="top"><p>/ID</p></td>
            <td valign="top"><p>Komt overeen met de waarde van doc.id (dezelfde eigenschap-id en de partitie zijn).</p></td>
        </tr>
        <tr>
            <td valign="top"><p>/ "afdelingsnaam"</p></td>
            <td valign="top"><p>Komt overeen met de waarde van doc ["afdelingsnaam"] doc-bestand waar het item is.</p></td>
        </tr>
    </tbody>
</table>

> [!NOTE]
> De syntaxis voor de partitiesleutel is vergelijkbaar met het opgegeven pad voor het beleid voor paden met het belangrijkste verschil is dat het pad overeenkomt met de eigenschap in plaats van de waarde indexeren, dat wil zeggen er zich geen jokerteken aan het einde. Bijvoorbeeld, geeft u/afdeling /? Als u wilt de waarden onder afdeling indexeren, maar u /department als de definitie van de partitie opgeven. De partitiesleutel is impliciet worden geïndexeerd en kan niet worden uitgesloten van het indexeren met indexering beleid negeren.
> 
> 

We bekijken hoe de keuze van de partitiesleutel van invloed is op de prestaties van uw toepassing.

## <a name="working-with-the-azure-cosmos-db-sdks"></a>Werken met de Azure Cosmos DB SDK 's
Azure Cosmos DB ondersteuning toegevoegd voor automatische partitioneren met [REST API-versie 2015-12-16](/rest/api/cosmos-db/). Om te kunnen gepartitioneerde containers te maken, moet u de SDK-versies 1.6.0 downloaden of hoger in een van de ondersteunde platformen SDK (.NET, Node.js, Java, Python, MongoDB). 

### <a name="creating-containers"></a>Het maken van containers
Het volgende voorbeeld ziet u een .NET-codefragment om te maken van een container voor het opslaan van apparaat telemetriegegevens van 20.000 aanvraageenheden aan doorvoer per seconde. De SDK stelt u de waarde OfferThroughput (die op zijn beurt Hiermee stelt u de `x-ms-offer-throughput` aanvraagheader in de REST-API). Hier stelt u de `/deviceId` als de partitiesleutel. De keuze van de partitiesleutel wordt opgeslagen samen met de rest van de containermetagegevens van de, zoals naam en het indexeringsbeleid.

U hebt geselecteerd voor dit voorbeeld `deviceId` omdat u weet dat (a) er een groot aantal apparaten zijn, schrijft gelijkmatig over meerdere partities kan worden verdeeld en zodat u kunt voor het schalen van de database voor opname van grote hoeveelheden gegevens en (b) veel van de aanvragen, zoals ophalen de meest recente lezen voor een apparaat zijn gericht op een enkel apparaat-id en kan worden opgehaald uit een enkele partitie.

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

Deze methode maakt een REST API-aanroep met Cosmos DB en een aantal partities op basis van de aangevraagde doorvoer, de service wordt ingericht. U kunt de doorvoer van een container of een set van containers wijzigen als de prestaties van uw bedrijfsbehoeften zich ontwikkelen. 

### <a name="reading-and-writing-items"></a>Lezen en schrijven van artikelen
Nu gaan we gegevens invoegen in Cosmos DB. Hier volgt een voorbeeldklasse met lezen apparaatgegevens en een aanroep naar CreateDocumentAsync om in te voegen een nieuwe lezen apparaatgegevens in een container. Hier volgt een voorbeeld van een codeblok die gebruikmaakt van de SQL-API:

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

Laten we het item met de partitiesleutel en -id niet lezen, bijwerken en vervolgens als laatste stap, verwijdert u deze door de partitiesleutel en -id. De leesbewerkingen een PartitionKey-waarde bevatten (die overeenkomt met de `x-ms-documentdb-partitionkey` aanvraagheader in de REST-API).

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

### <a name="querying-partitioned-containers"></a>Een query uitvoeren op gepartitioneerde containers
Wanneer u gegevens in gepartitioneerde containers op te vragen, Cosmos DB de query automatisch omgeleid naar de partities die overeenkomt met de partitiesleutelwaarden die zijn opgegeven in het filter (wanneer aanwezig). Deze query wordt bijvoorbeeld doorgestuurd naar de partitie met de partitiesleutel 'XMS-0001'.

```csharp
// Query using partition key
IQueryable<DeviceReading> query = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"))
    .Where(m => m.MetricType == "Temperature" && m.DeviceId == "XMS-0001");
```
    
De volgende query heeft geen een filter voor de partitiesleutel (DeviceId) en wordt verspreid over alle partities waarop deze wordt uitgevoerd op basis van de index van de partitie. U moet opgeven, moet de EnableCrossPartitionQuery (`x-ms-documentdb-query-enablecrosspartition` in de REST-API) om de SDK voor het uitvoeren van een query over meerdere partities.

```csharp
// Query across partition keys
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"), 
    new FeedOptions { EnableCrossPartitionQuery = true })
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100);
```

Cosmos DB biedt ondersteuning voor [statistische functies](how-to-sql-query.md#Aggregates) `COUNT`, `MIN`, `MAX`,, en `AVG` via gepartitioneerd containers met behulp van SQL starten met SDK's 1.12.0 en hoger. Query's een enkel aggregatieoperator moeten bevatten en moeten één waarde bevatten in de projectie.

### <a name="parallel-query-execution"></a>Parallelle queryuitvoering
De Cosmos DB SDK's 1.9.0 en hoger ondersteuning parallelle uitvoering queryopties, waarmee u kunt het uitvoeren van query's met lage latentie op gepartitioneerde verzamelingen, zelfs als ze nodig hebben om een groot aantal partities. De volgende query is bijvoorbeeld geconfigureerd voor parallelle uitvoering in meerdere partities.

```csharp
// Cross-partition Order By Queries
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"), 
    new FeedOptions { EnableCrossPartitionQuery = true, MaxDegreeOfParallelism = 10, MaxBufferedItemCount = 100})
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100)
    .OrderBy(m => m.MetricValue);
```
    
U kunt parallelle queryuitvoering beheren door de volgende parameters af te stemmen:

* Door in te stellen `MaxDegreeOfParallelism`, kunt u de mate van parallelle uitvoering bepalen dat wil zeggen, het maximum aantal gelijktijdige netwerkverbindingen met de partities van de container. Als u deze eigenschap instelt op -1, wordt de mate van parallelle uitvoering beheerd door de SDK. Als de `MaxDegreeOfParallelism` is niet opgegeven of is ingesteld op 0, wat de standaardwaarde is, wordt er één netwerkverbinding met de partities van de container.
* Door `MaxBufferedItemCount` in te stellen, kunt u de querylatentie en het geheugengebruik aan clientzijde uitruilen. Als u deze parameter weglaat of stel deze eigenschap in op-1, wordt het aantal items buffer wordt opgeslagen tijdens parallelle queryuitvoering beheerd door de SDK.

Gezien dezelfde status van de verzameling, retourneert een parallelle query resultaten in dezelfde volgorde als bij seriële uitvoering. Wanneer u een partitieoverkoepelende query uitvoert met sortering (sorteren en/of bovenaan) uitvoert, wordt de Azure Cosmos DB SDK problemen met de query's parallel uitvoeren over meerdere partities en worden gedeeltelijk gesorteerde resultaten aan de clientzijde om globaal geordende resultaten samengevoegd.

### <a name="executing-stored-procedures"></a>Opgeslagen procedures uitvoeren
U kunt ook atomische transacties uitvoeren voor documenten met dezelfde apparaat-ID, bijvoorbeeld, als u statistische functies of de meest recente status van een apparaat in één item. 

```csharp
await client.ExecuteStoredProcedureAsync<DeviceReading>(
    UriFactory.CreateStoredProcedureUri("db", "coll", "SetLatestStateAcrossReadings"),
    new RequestOptions { PartitionKey = new PartitionKey("XMS-001") }, 
    "XMS-001-FE24C");
```
   
U bekijken hoe u naar een gepartitioneerde containers van één partitie containers verplaatsen kunt in de volgende sectie.

## <a name="next-steps"></a>Volgende stappen
In dit artikel een overzicht van het werken met het partitioneren van Azure Cosmos DB-containers met de SQL API opgegeven. Zie ook [partitionering en horizontaal schalen](../cosmos-db/partition-data.md) voor een overzicht van concepten en aanbevolen procedures voor het partitioneren van met een Azure Cosmos DB-API. 

* Schaal en prestaties testen met Azure Cosmos DB uitvoeren. Zie [prestaties en schaal testen met Azure Cosmos DB](performance-testing.md) voor een voorbeeld.
* Aan de slag met code de [SDK's](sql-api-sdk-dotnet.md) of de [REST-API](/rest/api/cosmos-db/)
* Meer informatie over [ingerichte doorvoer in Azure Cosmos DB](request-units.md)

