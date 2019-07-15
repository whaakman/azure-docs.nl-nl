---
title: Zoek de aanvraageenheid (RU) kosten in rekening gebracht in Azure Cosmos DB
description: Meer informatie over het vinden van de aanvraageenheid (RU) kosten in rekening gebracht voor elke bewerking die wordt uitgevoerd op een Azure Cosmos-container.
author: ThomasWeiss
ms.service: cosmos-db
ms.topic: sample
ms.date: 06/14/2019
ms.author: thweiss
ms.openlocfilehash: 3d088da4c771c828db9788817e424c4d89586dd6
ms.sourcegitcommit: 6b41522dae07961f141b0a6a5d46fd1a0c43e6b2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/15/2019
ms.locfileid: "67986140"
---
# <a name="find-the-request-unit-charge-in-azure-cosmos-db"></a>De kosten van de aanvraag-eenheden niet vinden in Azure Cosmos DB

In dit artikel geeft de verschillende manieren waarop u vindt de [aanvraageenheid](request-units.md) (ru/s) van het verbruik voor elke bewerking die wordt uitgevoerd op een container in Azure Cosmos DB. Op dit moment kunt u dit verbruik meten alleen met behulp van de Azure-portal of door het antwoord van het Azure Cosmos DB via een van de SDK's teruggestuurd.

## <a name="sql-core-api"></a>SQL (Core) API

Als u de SQL-API gebruikt, hebt u meerdere opties voor het vinden van de RU-verbruik voor een bewerking op basis van een Azure Cosmos-container.

### <a name="use-the-azure-portal"></a>Azure Portal gebruiken

Op dit moment kunt u de kosten van de aanvraag vinden in Azure portal alleen voor een SQL-query.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

1. [Maak een nieuw Azure Cosmos-account](create-sql-api-dotnet.md#create-account) en deze feed met gegevens, of Selecteer een bestaand Azure-Cosmos-account dat al gegevens bevat.

1. Ga naar de **Data Explorer** deelvenster en selecteer vervolgens de container die u wilt werken.

1. Selecteer **nieuwe SQL-Query**.

1. Voer een geldige query en selecteer vervolgens **Query uitvoeren**.

1. Selecteer **Query statistieken** om weer te geven van de werkelijke kosten in rekening gebracht voor de aanvraag die u hebt uitgevoerd.

![Schermafbeelding van een SQL-query-aanvraag in rekening gebracht in Azure portal](./media/find-request-unit-charge/portal-sql-query.png)

### <a name="use-the-net-sdk"></a>De .NET SDK gebruiken
### <a name="net-v2-sdk"></a>.Net V2 SDK

Objecten die zijn geretourneerd door de [.NET SDK v2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/) blootstellen een `RequestCharge` eigenschap:

```csharp
ResourceResponse<Document> fetchDocumentResponse = await client.ReadDocumentAsync(
    UriFactory.CreateDocumentUri("database", "container", "itemId"),
    new RequestOptions
    {
        PartitionKey = new PartitionKey("partitionKey")
    });
var requestCharge = fetchDocumentResponse.RequestCharge;

StoredProcedureResponse<string> storedProcedureCallResponse = await client.ExecuteStoredProcedureAsync<string>(
    UriFactory.CreateStoredProcedureUri("database", "container", "storedProcedureId"),
    new RequestOptions
    {
        PartitionKey = new PartitionKey("partitionKey")
    });
requestCharge = storedProcedureCallResponse.RequestCharge;

IDocumentQuery<dynamic> query = client.CreateDocumentQuery(
    UriFactory.CreateDocumentCollectionUri("database", "container"),
    "SELECT * FROM c",
    new FeedOptions
    {
        PartitionKey = new PartitionKey("partitionKey")
    }).AsDocumentQuery();
while (query.HasMoreResults)
{
    FeedResponse<dynamic> queryResponse = await query.ExecuteNextAsync<dynamic>();
    requestCharge = queryResponse.RequestCharge;
}
```

### <a name="net-v3-sdk"></a>.Net V3 SDK

Objecten die zijn geretourneerd door de [SDK voor .NET v3](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/) blootstellen een `RequestCharge` eigenschap:

[!code-csharp[](~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos/tests/Microsoft.Azure.Cosmos.Tests/SampleCodeForDocs/CustomDocsSampleCode.cs?name=GetRequestCharge)]

Zie voor meer informatie [Snelstart: Een .NET-web-app bouwen met behulp van een SQL API-account in Azure Cosmos DB](create-sql-api-dotnet.md).

### <a name="use-the-java-sdk"></a>Gebruik de Java SDK

Objecten die zijn geretourneerd door de [Java SDK](https://mvnrepository.com/artifact/com.microsoft.azure/azure-cosmosdb) blootstellen een `getRequestCharge()` methode:

```java
RequestOptions requestOptions = new RequestOptions();
requestOptions.setPartitionKey(new PartitionKey("partitionKey"));

Observable<ResourceResponse<Document>> readDocumentResponse = client.readDocument(String.format("/dbs/%s/colls/%s/docs/%s", "database", "container", "itemId"), requestOptions);
readDocumentResponse.subscribe(result -> {
    double requestCharge = result.getRequestCharge();
});

Observable<StoredProcedureResponse> storedProcedureResponse = client.executeStoredProcedure(String.format("/dbs/%s/colls/%s/sprocs/%s", "database", "container", "storedProcedureId"), requestOptions, null);
storedProcedureResponse.subscribe(result -> {
    double requestCharge = result.getRequestCharge();
});

FeedOptions feedOptions = new FeedOptions();
feedOptions.setPartitionKey(new PartitionKey("partitionKey"));

Observable<FeedResponse<Document>> feedResponse = client
    .queryDocuments(String.format("/dbs/%s/colls/%s", "database", "container"), "SELECT * FROM c", feedOptions);
feedResponse.forEach(result -> {
    double requestCharge = result.getRequestCharge();
});
```

Zie voor meer informatie [Snelstart: Een Java-toepassing ontwikkelen met behulp van een Azure Cosmos DB SQL API-account](create-sql-api-java.md).

### <a name="use-the-nodejs-sdk"></a>De Node.js-SDK gebruiken

Objecten die zijn geretourneerd door de [Node.js SDK](https://www.npmjs.com/package/@azure/cosmos) blootstellen een `headers` weergevem dat is toegewezen alle headers die zijn geretourneerd door de onderliggende HTTP-API. De kosten van de aanvraag is beschikbaar onder de `x-ms-request-charge` sleutel:

```javascript
const item = await client
    .database('database')
    .container('container')
    .item('itemId', 'partitionKey')
    .read();
var requestCharge = item.headers['x-ms-request-charge'];

const storedProcedureResult = await client
    .database('database')
    .container('container')
    .storedProcedure('storedProcedureId')
    .execute({
        partitionKey: 'partitionKey'
    });
requestCharge = storedProcedureResult.headers['x-ms-request-charge'];

const query = client.database('database')
    .container('container')
    .items
    .query('SELECT * FROM c', {
        partitionKey: 'partitionKey'
    });
while (query.hasMoreResults()) {
    var result = await query.executeNext();
    requestCharge = result.headers['x-ms-request-charge'];
}
```

Zie voor meer informatie [Snelstart: Een Node.js-app bouwen met behulp van een Azure Cosmos DB SQL API-account](create-sql-api-nodejs.md). 

### <a name="use-the-python-sdk"></a>Gebruik de Python-SDK

De `CosmosClient` object uit de [Python SDK](https://pypi.org/project/azure-cosmos/) wordt aangegeven dat een `last_response_headers` woordenlijst die is toegewezen alle headers die zijn geretourneerd door de onderliggende HTTP-API voor de laatste bewerking uitgevoerd. De kosten van de aanvraag is beschikbaar onder de `x-ms-request-charge` sleutel:

```python
response = client.ReadItem('dbs/database/colls/container/docs/itemId', { 'partitionKey': 'partitionKey' })
request_charge = client.last_response_headers['x-ms-request-charge']

response = client.ExecuteStoredProcedure('dbs/database/colls/container/sprocs/storedProcedureId', None, { 'partitionKey': 'partitionKey' })
request_charge = client.last_response_headers['x-ms-request-charge']
```

Zie voor meer informatie [Snelstart: Een Python-app bouwen met behulp van een Azure Cosmos DB SQL API-account](create-sql-api-python.md). 

## <a name="azure-cosmos-db-api-for-mongodb"></a>Azure Cosmos DB-API voor MongoDB

De RU-kosten wordt weergegeven met een aangepaste [opdracht database](https://docs.mongodb.com/manual/reference/command/) met de naam `getLastRequestStatistics`. De opdracht retourneert een document met de naam van de laatste bewerking die is uitgevoerd, de kosten van de aanvraag en de duur. Als u de Azure Cosmos DB-API voor MongoDB gebruiken, hebt u meerdere opties voor het ophalen van de RU-kosten in rekening gebracht.

### <a name="use-the-azure-portal"></a>Azure Portal gebruiken

Op dit moment kunt u de kosten van de aanvraag vinden in Azure portal alleen voor een query.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

1. [Maak een nieuw Azure Cosmos-account](create-mongodb-dotnet.md#create-a-database-account) en deze feed met gegevens, of Selecteer een bestaand account die al gegevens bevat.

1. Ga naar de **Data Explorer** deelvenster en selecteer vervolgens de verzameling die u wilt werken.

1. Selecteer **New Query** (Nieuwe query).

1. Voer een geldige query en selecteer vervolgens **Query uitvoeren**.

1. Selecteer **Query statistieken** om weer te geven van de werkelijke kosten in rekening gebracht voor de aanvraag die u hebt uitgevoerd.

![Schermafbeelding van een MongoDB-query-aanvraag in rekening gebracht in Azure portal](./media/find-request-unit-charge/portal-mongodb-query.png)

### <a name="use-the-mongodb-net-driver"></a>Gebruik de MongoDB .NET-stuurprogramma

Wanneer u gebruikt de [officiële MongoDB .NET-stuurprogramma](https://docs.mongodb.com/ecosystem/drivers/csharp/), kunt u opdrachten uitvoeren door het aanroepen van de `RunCommand` methode op een `IMongoDatabase` object. Deze methode moet een implementatie van de `Command<>` abstracte klasse:

```csharp
class GetLastRequestStatisticsCommand : Command<Dictionary<string, object>>
{
    public override RenderedCommand<Dictionary<string, object>> Render(IBsonSerializerRegistry serializerRegistry)
    {
        return new RenderedCommand<Dictionary<string, object>>(new BsonDocument("getLastRequestStatistics", 1), serializerRegistry.GetSerializer<Dictionary<string, object>>());
    }
}

Dictionary<string, object> stats = database.RunCommand(new GetLastRequestStatisticsCommand());
double requestCharge = (double)stats["RequestCharge"];
```

Zie voor meer informatie [Snelstart: Een .NET-web-app bouwen met behulp van een Azure Cosmos DB-API voor MongoDB](create-mongodb-dotnet.md).

### <a name="use-the-mongodb-java-driver"></a>Het stuurprogramma voor MongoDB Java gebruiken


Wanneer u gebruikt de [officiële MongoDB Java-stuurprogramma](https://mongodb.github.io/mongo-java-driver/), kunt u opdrachten uitvoeren door het aanroepen van de `runCommand` methode op een `MongoDatabase` object:

```java
Document stats = database.runCommand(new Document("getLastRequestStatistics", 1));
Double requestCharge = stats.getDouble("RequestCharge");
```

Zie voor meer informatie [Snelstart: Een web-app bouwen met behulp van de Azure Cosmos DB-API voor MongoDB en de Java SDK](create-mongodb-java.md).

### <a name="use-the-mongodb-nodejs-driver"></a>Het stuurprogramma Node.js voor MongoDB gebruiken

Wanneer u gebruikt de [officiële MongoDB Node.js-stuurprogramma](https://mongodb.github.io/node-mongodb-native/), kunt u opdrachten uitvoeren door het aanroepen van de `command` methode op een `db` object:

```javascript
db.command({ getLastRequestStatistics: 1 }, function(err, result) {
    assert.equal(err, null);
    const requestCharge = result['RequestCharge'];
});
```

Zie voor meer informatie [Snelstart: Een bestaande MongoDB Node.js-web-app migreren naar Azure Cosmos DB](create-mongodb-nodejs.md).

## <a name="cassandra-api"></a>Cassandra-API

Wanneer u bewerkingen met de Cassandra-API van Azure Cosmos DB, de RU-kosten in rekening gebracht als een veld met de naam wordt geretourneerd in de nettolading van de binnenkomende `RequestCharge`. U hebt meerdere opties voor het ophalen van de RU-kosten in rekening gebracht.

### <a name="use-the-net-sdk"></a>De .NET SDK gebruiken

Wanneer u gebruikt de [.NET SDK](https://www.nuget.org/packages/CassandraCSharpDriver/), kunt u de nettolading van de binnenkomende onder ophalen de `Info` eigenschap van een `RowSet` object:

```csharp
RowSet rowSet = session.Execute("SELECT table_name FROM system_schema.tables;");
double requestCharge = BitConverter.ToDouble(rowSet.Info.IncomingPayload["RequestCharge"].Reverse().ToArray(), 0);
```

Zie voor meer informatie [Snelstart: Een Cassandra-app bouwen met behulp van de SDK voor .NET en Azure Cosmos DB](create-cassandra-dotnet.md).

### <a name="use-the-java-sdk"></a>Gebruik de Java SDK

Wanneer u gebruikt de [Java SDK](https://mvnrepository.com/artifact/com.datastax.cassandra/cassandra-driver-core), kunt u de nettolading van de binnenkomende ophalen door het aanroepen van de `getExecutionInfo()` methode op een `ResultSet` object:

```java
ResultSet resultSet = session.execute("SELECT table_name FROM system_schema.tables;");
Double requestCharge = resultSet.getExecutionInfo().getIncomingPayload().get("RequestCharge").getDouble();
```

Zie voor meer informatie [Snelstart: Een Cassandra-app bouwen met behulp van Java-SDK en Azure Cosmos DB](create-cassandra-java.md).

## <a name="gremlin-api"></a>Gremlin-API

Wanneer u de Gremlin-API gebruikt, hebt u meerdere opties voor het vinden van de RU-verbruik voor een bewerking op basis van een Azure Cosmos-container. 

### <a name="use-drivers-and-sdk"></a>Gebruik stuurprogramma's en SDK

Headers die wordt geretourneerd door de Gremlin-API worden toegewezen aan de status van de aangepaste kenmerken, die momenteel worden opgehaald door de Gremlin-.NET en Java-SDK. De kosten van de aanvraag is beschikbaar onder de `x-ms-request-charge` sleutel.

### <a name="use-the-net-sdk"></a>De .NET SDK gebruiken

Wanneer u gebruikt de [Gremlin.NET SDK](https://www.nuget.org/packages/Gremlin.Net/), status-kenmerken zijn beschikbaar onder de `StatusAttributes` eigenschap van de `ResultSet<>` object:

```csharp
ResultSet<dynamic> results = client.SubmitAsync<dynamic>("g.V().count()").Result;
double requestCharge = (double)results.StatusAttributes["x-ms-request-charge"];
```

Zie voor meer informatie [Snelstart: Een .NET Framework- of Core-toepassing bouwen met behulp van een Gremlin-API van Azure Cosmos DB-account](create-graph-dotnet.md).

### <a name="use-the-java-sdk"></a>Gebruik de Java SDK

Wanneer u gebruikt de [Gremlin Java SDK](https://mvnrepository.com/artifact/org.apache.tinkerpop/gremlin-driver), kunt u de kenmerken van de status ophalen door het aanroepen van de `statusAttributes()` methode voor het `ResultSet` object:

```java
ResultSet results = client.submit("g.V().count()");
Double requestCharge = (Double)results.statusAttributes().get().get("x-ms-request-charge");
```

Zie voor meer informatie [Snelstart: Een grafiekdatabase maken in Azure Cosmos DB met behulp van de Java SDK](create-graph-java.md).

## <a name="table-api"></a>Tabel-API

Op dit moment de enige SDK die wordt geretourneerd van de RU kosten in rekening gebracht voor tabelbewerkingen is de [.NET Standard-SDK](https://www.nuget.org/packages/Microsoft.Azure.Cosmos.Table). De `TableResult` object toont een `RequestCharge` eigenschap die door de SDK wordt gevuld wanneer u deze op basis van de Azure Cosmos DB Table-API gebruiken:

```csharp
CloudTable tableReference = client.GetTableReference("table");
TableResult tableResult = tableReference.Execute(TableOperation.Insert(new DynamicTableEntity("partitionKey", "rowKey")));
if (tableResult.RequestCharge.HasValue) // would be false when using Azure Storage Tables
{
    double requestCharge = tableResult.RequestCharge.Value;
}
```

Zie voor meer informatie [Snelstart: Een tabel-API-app bouwen met behulp van de SDK voor .NET en Azure Cosmos DB](create-table-dotnet.md).

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over het optimaliseren van uw RU-verbruik, Zie de volgende artikelen:

* [Aanvraageenheden en doorvoer in Azure Cosmos DB](request-units.md)
* [Kosten voor ingerichte doorvoer optimaliseren in Azure Cosmos DB](optimize-cost-throughput.md)
* [Kosten van de query in Azure Cosmos DB optimaliseren](optimize-cost-queries.md)
* [Ingerichte doorvoer wereldwijd schalen](scaling-throughput.md)
* [Doorvoer voor containers en databases inrichten](set-throughput.md)
* [Inrichten doorvoer voor een container](how-to-provision-container-throughput.md)
* [Monitor en fouten opsporen met metrische gegevens in Azure Cosmos DB](use-metrics.md)
