---
title: Zoek de aanvraageenheid (RU) kosten in rekening gebracht in Azure Cosmos DB
description: Meer informatie over het vinden van de kosten van de aanvraag-eenheden voor elke bewerking die wordt uitgevoerd op een Azure Cosmos-container
author: ThomasWeiss
ms.service: cosmos-db
ms.topic: sample
ms.date: 04/15/2019
ms.author: thweiss
ms.openlocfilehash: 7afa815f81e2a61db8ac83623baafb97cb986b2c
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/30/2019
ms.locfileid: "64925364"
---
# <a name="find-the-request-unit-ru-charge-in-azure-cosmos-db"></a>Zoek de aanvraageenheid (RU) kosten in rekening gebracht in Azure Cosmos DB

In dit artikel geeft de verschillende manieren om te zoeken de [aanvraageenheid](request-units.md) verbruik voor elke bewerking die wordt uitgevoerd op een Azure Cosmos-container. Het is momenteel mogelijk is voor het meten van dit verbruik met behulp van Azure portal of door het antwoord van het Azure Cosmos DB via een van de SDK's teruggestuurd.

## <a name="sql-core-api"></a>SQL (Core) API

### <a name="use-the-azure-portal"></a>Azure Portal gebruiken

De Azure-portal kunt op dit moment u de kosten van de aanvraag voor een SQL-query alleen vinden.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

1. [Maak een nieuw Azure Cosmos-account](create-sql-api-dotnet.md#create-account) en deze feed met gegevens, of Selecteer een bestaand Azure-Cosmos-account dat al gegevens bevat.

1. Open de **Data Explorer** deelvenster en selecteer de container die u wilt werken.

1. Klik op **nieuwe SQL-Query**.

1. Voer een geldige query en klik vervolgens op **Query uitvoeren**.

1. Klik op **Query statistieken** om weer te geven van de werkelijke kosten in rekening gebracht voor de aanvraag die u zojuist hebt uitgevoerd.

![Schermafbeelding van de SQL-query aanvraag kosten in rekening gebracht in Azure portal](./media/find-request-unit-charge/portal-sql-query.png)

### <a name="use-the-net-sdk-v2"></a>Gebruik de .NET SDK-V2

Objecten die zijn geretourneerd door de [.NET SDK v2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/) (Zie [in deze snelstartgids](create-sql-api-dotnet.md) met betrekking tot het gebruik ervan) beschikbaar maken een `RequestCharge` eigenschap.

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

### <a name="use-the-java-sdk"></a>Gebruik de Java SDK

Objecten die zijn geretourneerd door de [Java SDK](https://mvnrepository.com/artifact/com.microsoft.azure/azure-cosmosdb) (Zie [in deze snelstartgids](create-sql-api-java.md) met betrekking tot het gebruik ervan) beschikbaar maken een `getRequestCharge()` methode.

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

### <a name="use-the-nodejs-sdk"></a>De Node.js-SDK gebruiken

Objecten die zijn geretourneerd door de [Node.js SDK](https://www.npmjs.com/package/@azure/cosmos) (Zie [in deze snelstartgids](create-sql-api-nodejs.md) met betrekking tot het gebruik ervan) beschikbaar maken een `headers` onderliggende object dat is toegewezen alle headers die zijn geretourneerd door de onderliggende HTTP-API. De kosten van de aanvraag is beschikbaar onder de `x-ms-request-charge` sleutel.

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

### <a name="use-the-python-sdk"></a>Gebruik de Python-SDK

De `CosmosClient` object uit de [Python SDK](https://pypi.org/project/azure-cosmos/) (Zie [in deze snelstartgids](create-sql-api-python.md) met betrekking tot het gebruik ervan) wordt aangegeven dat een `last_response_headers` woordenlijst die is toegewezen alle headers die zijn geretourneerd door de onderliggende HTTP-API voor de laatste bewerking uitgevoerd. De kosten van de aanvraag is beschikbaar onder de `x-ms-request-charge` sleutel.

```python
response = client.ReadItem('dbs/database/colls/container/docs/itemId', { 'partitionKey': 'partitionKey' })
request_charge = client.last_response_headers['x-ms-request-charge']

response = client.ExecuteStoredProcedure('dbs/database/colls/container/sprocs/storedProcedureId', None, { 'partitionKey': 'partitionKey' })
request_charge = client.last_response_headers['x-ms-request-charge']
```

## <a name="azure-cosmos-dbs-api-for-mongodb"></a>Azure Cosmos DB-API voor MongoDB

Kosten voor aanvraag-eenheden wordt weergegeven met een aangepaste [opdracht database](https://docs.mongodb.com/manual/reference/command/) met de naam `getLastRequestStatistics.` met deze opdracht retourneert een document met de naam van de laatste bewerking die is uitgevoerd, de kosten van de aanvraag en de duur.

### <a name="use-the-azure-portal"></a>Azure Portal gebruiken

De Azure-portal kunt op dit moment u de kosten van de aanvraag voor een query alleen vinden.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

1. [Maak een nieuw Azure Cosmos-account](create-mongodb-dotnet.md#create-a-database-account) en deze feed met gegevens, of Selecteer een bestaand account die al gegevens bevat.

1. Open de **Data Explorer** deelvenster en selecteer de verzameling die u wilt werken.

1. Klik op **nieuwe Query**.

1. Voer een geldige query en klik op **Query uitvoeren**.

1. Klik op **Query statistieken** om weer te geven van de werkelijke kosten in rekening gebracht voor de aanvraag die u zojuist hebt uitgevoerd.

![Schermafbeelding van MongoDB query aanvraag kosten in rekening gebracht in Azure portal](./media/find-request-unit-charge/portal-mongodb-query.png)

### <a name="use-the-mongodb-net-driver"></a>Gebruik de MongoDB .NET-stuurprogramma

Wanneer u de [officiële MongoDB .NET-stuurprogramma](https://docs.mongodb.com/ecosystem/drivers/csharp/) (Zie [in deze snelstartgids](create-mongodb-dotnet.md) met betrekking tot het gebruik ervan), opdrachten kunnen worden uitgevoerd door het aanroepen van de `RunCommand` methode op een `IMongoDatabase` object. Deze methode moet een implementatie van de `Command<>` abstracte klasse.

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

### <a name="use-the-mongodb-java-driver"></a>Het stuurprogramma voor MongoDB Java gebruiken

Wanneer u de [officiële MongoDB Java-stuurprogramma](https://mongodb.github.io/mongo-java-driver/) (Zie [in deze snelstartgids](create-mongodb-java.md) met betrekking tot het gebruik ervan), opdrachten kunnen worden uitgevoerd door het aanroepen van de `runCommand` methode op een `MongoDatabase` object.

```java
Document stats = database.runCommand(new Document("getLastRequestStatistics", 1));
Double requestCharge = stats.getDouble("RequestCharge");
```

### <a name="use-the-mongodb-nodejs-driver"></a>Het stuurprogramma Node.js voor MongoDB gebruiken

Wanneer u de [officiële MongoDB Node.js-stuurprogramma](https://mongodb.github.io/node-mongodb-native/) (Zie [in deze snelstartgids](create-mongodb-nodejs.md) met betrekking tot het gebruik ervan), opdrachten kunnen worden uitgevoerd door het aanroepen van de `command` methode op een `db` object.

```javascript
db.command({ getLastRequestStatistics: 1 }, function(err, result) {
    assert.equal(err, null);
    const requestCharge = result['RequestCharge'];
});
```

## <a name="cassandra-api"></a>Cassandra-API

Bij het uitvoeren van bewerkingen op Azure Cosmos DB Cassandra-API, aanvraag eenheid kosten in rekening gebracht als een veld met de naam wordt geretourneerd in de nettolading van de binnenkomende `RequestCharge`.

### <a name="use-the-net-sdk"></a>De .NET SDK gebruiken

Wanneer u de [.NET SDK](https://www.nuget.org/packages/CassandraCSharpDriver/) (Zie [in deze snelstartgids](create-cassandra-dotnet.md) met betrekking tot het gebruik ervan), de nettolading van de binnenkomende kan worden opgehaald bij de `Info` eigenschap van een `RowSet` object.

```csharp
RowSet rowSet = session.Execute("SELECT table_name FROM system_schema.tables;");
double requestCharge = BitConverter.ToDouble(rowSet.Info.IncomingPayload["RequestCharge"], 0);
```

### <a name="use-the-java-sdk"></a>Gebruik de Java SDK

Wanneer u de [Java SDK](https://mvnrepository.com/artifact/com.datastax.cassandra/cassandra-driver-core) (Zie [in deze snelstartgids](create-cassandra-java.md) met betrekking tot het gebruik ervan), de nettolading van de binnenkomende kan worden opgehaald door het aanroepen van de `getExecutionInfo()` methode op een `ResultSet` object.

```java
ResultSet resultSet = session.execute("SELECT table_name FROM system_schema.tables;");
Double requestCharge = resultSet.getExecutionInfo().getIncomingPayload().get("RequestCharge").getDouble();
```

## <a name="gremlin-api"></a>Gremlin-API

### <a name="use-drivers-and-sdk"></a>Gebruik stuurprogramma's en SDK

Headers die wordt geretourneerd door de Gremlin-API worden toegewezen aan de status van de aangepaste kenmerken, die momenteel worden opgehaald door de Gremlin-.NET en Java-SDK. De kosten van de aanvraag is beschikbaar onder de `x-ms-request-charge` sleutel.

### <a name="use-the-net-sdk"></a>De .NET SDK gebruiken

Bij het gebruik van de [Gremlin.NET SDK](https://www.nuget.org/packages/Gremlin.Net/) (Zie [in deze snelstartgids](create-graph-dotnet.md) met betrekking tot het gebruik ervan), status-kenmerken zijn beschikbaar onder de `StatusAttributes` eigenschap van de `ResultSet<>` object.

```csharp
ResultSet<dynamic> results = client.SubmitAsync<dynamic>("g.V().count()").Result;
double requestCharge = (double)results.StatusAttributes["x-ms-request-charge"];
```

### <a name="use-the-java-sdk"></a>Gebruik de Java SDK

Bij het gebruik van de [Gremlin Java SDK](https://mvnrepository.com/artifact/org.apache.tinkerpop/gremlin-driver) (Zie [in deze Quick Start](create-graph-java.md) met betrekking tot het gebruik ervan), status-kenmerken kunnen worden opgehaald door het aanroepen van de `statusAttributes()` methode voor de `ResultSet` object.

```java
ResultSet results = client.submit("g.V().count()");
Double requestCharge = (Double)results.statusAttributes().get().get("x-ms-request-charge");
```

## <a name="table-api"></a>Tabel-API

De enige SDK momenteel retourneren aanvraag eenheid kosten in rekening gebracht voor tabelbewerkingen is de [.NET Standard-SDK](https://www.nuget.org/packages/Microsoft.Azure.Cosmos.Table) (Zie [in deze snelstartgids](create-table-dotnet.md) met betrekking tot het gebruik ervan). De `TableResult` object toont een `RequestCharge` eigenschap die door de SDK wanneer die wordt gebruikt op basis van Azure Cosmos DB Table-API wordt gevuld.

```csharp
CloudTable tableReference = client.GetTableReference("table");
TableResult tableResult = tableReference.Execute(TableOperation.Insert(new DynamicTableEntity("partitionKey", "rowKey")));
if (tableResult.RequestCharge.HasValue) // would be false when using Azure Storage Tables
{
    double requestCharge = tableResult.RequestCharge.Value;
}
```

## <a name="next-steps"></a>Volgende stappen

Zie de volgende artikelen voor meer informatie over het optimaliseren van uw gebruik van de eenheid aanvraag:

* [Aanvraageenheden en doorvoer in Azure Cosmos DB](request-units.md)
* [Kosten van de ingerichte doorvoer in Azure Cosmos DB optimaliseren](optimize-cost-throughput.md)
* [Kosten van de query in Azure Cosmos DB optimaliseren](optimize-cost-queries.md)
* [Ingerichte doorvoer wereldwijd schalen](scaling-throughput.md)
* [Doorvoer voor containers en databases inrichten](set-throughput.md)
* [Doorvoer inrichten voor een container](how-to-provision-container-throughput.md)
