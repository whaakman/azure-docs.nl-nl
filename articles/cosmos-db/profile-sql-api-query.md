---
title: SQL query performance & uitvoering van metrische gegevens ophalen
description: Informatie over het ophalen van de SQL-query uitvoering van metrische gegevens en prestaties van de SQL-query's van Azure Cosmos DB-aanvragen-profiel.
author: ginamr
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 05/17/2019
ms.author: girobins
ms.openlocfilehash: b4017666956d0e01ea19781fb4f1ce2dde15fff5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66481562"
---
# <a name="get-sql-query-execution-metrics-and-analyze-query-performance-using-net-sdk"></a>Metrische gegevens voor uitvoering van SQL-query ophalen en analyseren van prestaties van query's met .NET SDK

In dit artikel geeft het profiel van de prestaties van de SQL-query's op Azure Cosmos DB. Deze profielen kan worden gedaan met `QueryMetrics` opgehaald uit de .NET SDK en hier wordt beschreven. [QueryMetrics](https://msdn.microsoft.com/library/microsoft.azure.documents.querymetrics.aspx) is een sterk getypeerde-object met informatie over de uitvoering van de back-end-query. Deze metrische gegevens worden in meer detail gedocumenteerd in de [queryprestaties afstemmen](https://docs.microsoft.com/azure/cosmos-db/documentdb-sql-query-metrics) artikel.

## <a name="set-the-feedoptions-parameter"></a>Stel de parameter FeedOptions

De overloads voor [DocumentClient.CreateDocumentQuery](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentquery.aspx) nemen in een optionele [FeedOptions](https://msdn.microsoft.com/library/microsoft.azure.documents.client.feedoptions.aspx) parameter. Deze optie is kunt uitvoering van de query worden afgestemd op en parameters. 

Voor het verzamelen van de Sql-query uitvoering van metrische gegevens, moet u de parameter instellen [PopulateQueryMetrics](https://msdn.microsoft.com/library/microsoft.azure.documents.client.feedoptions.populatequerymetrics.aspx#P:Microsoft.Azure.Documents.Client.FeedOptions.PopulateQueryMetrics) in de [FeedOptions](https://msdn.microsoft.com/library/microsoft.azure.documents.client.feedoptions.aspx) naar `true`. Instellen van `PopulateQueryMetrics` aan true maakt het zodat de `FeedResponse` bevat de relevante `QueryMetrics`. 

## <a name="get-query-metrics-with-asdocumentquery"></a>Query metrische gegevens met AsDocumentQuery() ophalen
Het volgende codevoorbeeld laat zien hoe u kunt metrische gegevens ophalen bij het gebruik van [AsDocumentQuery()](https://msdn.microsoft.com/library/microsoft.azure.documents.linq.documentqueryable.asdocumentquery.aspx) methode:

```csharp
// Initialize this DocumentClient and Collection
DocumentClient documentClient = null;
DocumentCollection collection = null;

// Setting PopulateQueryMetrics to true in the FeedOptions
FeedOptions feedOptions = new FeedOptions
{
    PopulateQueryMetrics = true
};

string query = "SELECT TOP 5 * FROM c";
IDocumentQuery<dynamic> documentQuery = documentClient.CreateDocumentQuery(Collection.SelfLink, query, feedOptions).AsDocumentQuery();

while (documentQuery.HasMoreResults)
{
    // Execute one continuation of the query
    FeedResponse<dynamic> feedResponse = await documentQuery.ExecuteNextAsync();
    
    // This dictionary maps the partitionId to the QueryMetrics of that query
    IReadOnlyDictionary<string, QueryMetrics> partitionIdToQueryMetrics = feedResponse.QueryMetrics;
    
    // At this point you have QueryMetrics which you can serialize using .ToString()
    foreach (KeyValuePair<string, QueryMetrics> kvp in partitionIdToQueryMetrics)
    {
        string partitionId = kvp.Key;
        QueryMetrics queryMetrics = kvp.Value;
        
        // Do whatever logging you need
        DoSomeLoggingOfQueryMetrics(query, partitionId, queryMetrics);
    }
}
```
## <a name="aggregating-querymetrics"></a>QueryMetrics aggregeren

In de vorige sectie, zoals u ziet dat er meerdere aanroepen naar [ExecuteNextAsync](https://msdn.microsoft.com/library/azure/dn850294.aspx) methode. Geretourneerd voor elke aanroep van een `FeedResponse` -object dat een van woordenlijst `QueryMetrics`; één voor elke voortzetting van de query. Het volgende voorbeeld laat zien hoe deze statistische `QueryMetrics` met behulp van LINQ:

```csharp
List<QueryMetrics> queryMetricsList = new List<QueryMetrics>();

while (documentQuery.HasMoreResults)
{
    // Execute one continuation of the query
    FeedResponse<dynamic> feedResponse = await documentQuery.ExecuteNextAsync();
    
    // This dictionary maps the partitionId to the QueryMetrics of that query
    IReadOnlyDictionary<string, QueryMetrics> partitionIdToQueryMetrics = feedResponse.QueryMetrics;
    queryMetricsList.AddRange(partitionIdToQueryMetrics.Values);
}

// Aggregate the QueryMetrics using the + operator overload of the QueryMetrics class.
QueryMetrics aggregatedQueryMetrics = queryMetricsList.Aggregate((curr, acc) => curr + acc);
Console.WriteLine(aggregatedQueryMetrics);
```

## <a name="grouping-query-metrics-by-partition-id"></a>Groepering query metrische per partitie-ID

U kunt groeperen de `QueryMetrics` door de partitie-ID. Groepering met partitie-ID kunt u zien als een specifieke partitie wordt veroorzaakt door prestatieproblemen met in vergelijking met anderen. Het volgende voorbeeld ziet u hoe u wilt groeperen `QueryMetrics` met LINQ:

```csharp
List<KeyValuePair<string, QueryMetrics>> partitionedQueryMetrics = new List<KeyValuePair<string, QueryMetrics>>();
while (documentQuery.HasMoreResults)
{
    // Execute one continuation of the query
    FeedResponse<dynamic> feedResponse = await documentQuery.ExecuteNextAsync();
    
    // This dictionary is maps the partitionId to the QueryMetrics of that query
    IReadOnlyDictionary<string, QueryMetrics> partitionIdToQueryMetrics = feedResponse.QueryMetrics;
    partitionedQueryMetrics.AddRange(partitionIdToQueryMetrics.ToList());
}

// Now we are able to group the query metrics by partitionId
IEnumerable<IGrouping<string, KeyValuePair<string, QueryMetrics>>> groupedByQueryMetrics = partitionedQueryMetrics
    .GroupBy(kvp => kvp.Key);

// If we wanted to we could even aggregate the groupedby QueryMetrics
foreach(IGrouping<string, KeyValuePair<string, QueryMetrics>> grouping in groupedByQueryMetrics)
{
    string partitionId = grouping.Key;
    QueryMetrics aggregatedQueryMetricsForPartition = grouping
        .Select(kvp => kvp.Value)
        .Aggregate((curr, acc) => curr + acc);
    DoSomeLoggingOfQueryMetrics(query, partitionId, aggregatedQueryMetricsForPartition);
}
```

## <a name="linq-on-documentquery"></a>LINQ op DocumentQuery

U krijgt ook de `FeedResponse` vanuit een LINQ-Query met de `AsDocumentQuery()` methode:

```csharp
IDocumentQuery<Document> linqQuery = client.CreateDocumentQuery(collection.SelfLink, feedOptions)
    .Take(1)
    .Where(document => document.Id == "42")
    .OrderBy(document => document.Timestamp)
    .AsDocumentQuery();
FeedResponse<Document> feedResponse = await linqQuery.ExecuteNextAsync<Document>();
IReadOnlyDictionary<string, QueryMetrics> queryMetrics = feedResponse.QueryMetrics;
```

## <a name="expensive-queries"></a>Dure query 's

U kunt de basis van aanvraageenheden gebruikt door elke query voor het onderzoeken van dure query's of query's die hoge doorvoer verbruiken vastleggen. Krijgt u de aanvraag kosten worden berekend met behulp van de [RequestCharge](https://msdn.microsoft.com/library/azure/dn948712.aspx) eigenschap in `FeedResponse`. Zie voor meer informatie over het ophalen van de aanvraag kosten in rekening gebracht met de Azure portal en de verschillende SDK's, [vinden van de kosten voor aanvraag eenheden](find-request-unit-charge.md) artikel.

```csharp
string query = "SELECT * FROM c";
IDocumentQuery<dynamic> documentQuery = documentClient.CreateDocumentQuery(Collection.SelfLink, query, feedOptions).AsDocumentQuery();

while (documentQuery.HasMoreResults)
{
    // Execute one continuation of the query
    FeedResponse<dynamic> feedResponse = await documentQuery.ExecuteNextAsync();
    double requestCharge = feedResponse.RequestCharge
    
    // Log the RequestCharge how ever you want.
    DoSomeLogging(requestCharge);
}
```

## <a name="get-the-query-execution-time"></a>Ophalen van de query-uitvoeringstijd

Bij het berekenen van de tijd die nodig is een client-side '-query uit te voeren, zorg ervoor dat u alleen de tijd om aan te roepen bevatten de `ExecuteNextAsync` methode en geen andere onderdelen van uw codebasis. Alleen deze aanroepen helpen u bij het berekenen hoe lang duurde van de queryuitvoering zoals wordt weergegeven in het volgende voorbeeld:

```csharp
string query = "SELECT * FROM c";
IDocumentQuery<dynamic> documentQuery = documentClient.CreateDocumentQuery(Collection.SelfLink, query, feedOptions).AsDocumentQuery();
Stopwatch queryExecutionTimeEndToEndTotal = new Stopwatch();
while (documentQuery.HasMoreResults)
{
    // Execute one continuation of the query
    queryExecutionTimeEndToEndTotal.Start();
    FeedResponse<dynamic> feedResponse = await documentQuery.ExecuteNextAsync();
    queryExecutionTimeEndToEndTotal.Stop();
}

// Log the elapsed time
DoSomeLogging(queryExecutionTimeEndToEndTotal.Elapsed);
```

## <a name="scan-queries-commonly-slow-and-expensive"></a>Scannen van query's (meestal traag en duur)

Een scan query verwijst naar een query die niet is geleverd door de index, omdat dat veel documenten zijn geladen voordat de resultatenset geretourneerd.

Hieronder volgt een voorbeeld van een scan-query:

```sql
SELECT VALUE c.description 
FROM   c 
WHERE UPPER(c.description) = "BABYFOOD, DESSERT, FRUIT DESSERT, WITHOUT ASCORBIC ACID, JUNIOR"
```

Filter van deze query maakt gebruik van de systeemfunctie hoofdletters, die niet wordt uitgevoerd vanuit de index. De volgende metrische gegevens van de query uit voor de eerste voortzetting die deze query uitvoert op basis van een grote verzameling worden geproduceerd:

```
QueryMetrics

Retrieved Document Count                 :          60,951
Retrieved Document Size                  :     399,998,938 bytes
Output Document Count                    :               7
Output Document Size                     :             510 bytes
Index Utilization                        :            0.00 %
Total Query Execution Time               :        4,500.34 milliseconds
  Query Preparation Times
    Query Compilation Time               :            0.09 milliseconds
    Logical Plan Build Time              :            0.05 milliseconds
    Physical Plan Build Time             :            0.04 milliseconds
    Query Optimization Time              :            0.01 milliseconds
  Index Lookup Time                      :            0.01 milliseconds
  Document Load Time                     :        4,177.66 milliseconds
  Runtime Execution Times
    Query Engine Times                   :          322.16 milliseconds
    System Function Execution Time       :           85.74 milliseconds
    User-defined Function Execution Time :            0.00 milliseconds
  Document Write Time                    :            0.01 milliseconds
Client Side Metrics
  Retry Count                            :               0
  Request Charge                         :        4,059.95 RUs
```

Houd rekening met de volgende waarden van de query-uitvoer voor metrische gegevens:

```
Retrieved Document Count                 :          60,951
Retrieved Document Size                  :     399,998,938 bytes
```

Deze query geladen 60,951 documenten, waarop 399,998,938 bytes getotaliseerd. Het laden van dit aantal bytes resulteert in hoge kosten maakt of een aanvraag eenheid kosten in rekening gebracht. Het duurt ook lang de query die uitgeschakeld met de eigenschap van de totale tijd besteed aan het is uit te voeren:

```
Total Query Execution Time               :        4,500.34 milliseconds
```

Wat betekent dat de query duurde 4.5 seconden om uit te voeren (en dit is slechts één voortzetting).

Vermijd het gebruik van hoofdletters in het filter voor het optimaliseren van dit voorbeeld van een query. In plaats daarvan wanneer documenten worden gemaakt of bijgewerkt, de `c.description` waarden moeten worden ingevoegd in hoofdletters. De query wordt vervolgens: 

```sql
SELECT VALUE c.description 
FROM   c 
WHERE c.description = "BABYFOOD, DESSERT, FRUIT DESSERT, WITHOUT ASCORBIC ACID, JUNIOR"
```

Deze query kan nu worden uitgevoerd vanuit de index.

Zie voor meer informatie over het afstemmen van prestaties van query's, de [queryprestaties afstemmen](https://docs.microsoft.com/azure/cosmos-db/documentdb-sql-query-metrics) artikel.

## <a id="References"></a>Naslaginformatie

- [SQL-specificatie voor Azure Cosmos DB](https://go.microsoft.com/fwlink/p/?LinkID=510612)
- [ANSI SQL 2011](https://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681)
- [JSON](https://json.org/)
- [LINQ](/previous-versions/dotnet/articles/bb308959(v=msdn.10)) 

## <a name="next-steps"></a>Volgende stappen

- [Queryprestaties afstemmen](sql-api-query-metrics.md)
- [Indexering-overzicht](index-overview.md)
- [.NET-voorbeelden voor Azure Cosmos DB](https://github.com/Azure/azure-cosmosdb-dotnet)
