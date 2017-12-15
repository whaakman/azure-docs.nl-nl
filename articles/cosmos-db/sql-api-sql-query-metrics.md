---
title: SQL-query metrische gegevens voor Azure Cosmos DB SQL-API | Microsoft Docs
description: Meer informatie over het instrumenteren en de prestaties van de SQL-query's van Azure DB die Cosmos-aanvragen voor foutopsporing.
keywords: SQL-syntaxis, sql-query, sql-query's, json-querytaal, database-concepten en sql-query's, statistische functies
services: cosmos-db
documentationcenter: 
author: arramac
manager: jhubbard
editor: monicar
ms.assetid: b2fa8e8f-7291-45a3-9bd1-7284ed9077f8
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/02/2017
ms.author: arramac
ms.openlocfilehash: a2a42fd65ba4344f703ca423dc451802f3f0ac76
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="tuning-query-performance-with-azure-cosmos-db"></a>Prestaties van query's met Azure Cosmos DB afstemmen

[!INCLUDE [cosmos-db-sql-api](../../includes/cosmos-db-sql-api.md)]

Azure Cosmos DB biedt een [SQL-API voor het opvragen van gegevens](sql-api-sql-query.md), zonder schema of secundaire indexen. In dit artikel biedt de volgende informatie voor ontwikkelaars:

* Details op hoog niveau over de werking van Azure Cosmos DB SQL-query-uitvoering
* Meer informatie over de aanvraag- en reactieheaders query en opties voor client-SDK
* Tips en aanbevolen procedures voor prestaties van query 's
* Voorbeelden van hoe u kunt gebruikmaken van SQL-Uitvoeringsstatistieken voor foutopsporing van de prestaties van query 's

## <a name="about-sql-query-execution"></a>Over het uitvoeren van de SQL-query

In Azure Cosmos DB, slaat u de gegevens in de containers die kunnen worden uitgebreid naar een [grootte of aanvraag opslagdoorvoer](partition-data.md). Azure Cosmos DB schaalt naadloos gegevens meerdere fysieke partities achter de Gegevensgroei verwerken of worden verhoogd met ingerichte doorvoer. U kunt de SQL-query's naar een container met de REST-API of een van de ondersteunde opdracht [SQL-SDK's](sql-api-sdk-dotnet.md).

Een kort overzicht van het partitioneren: definieert u een partitiesleutel zoals 'plaats', waarmee wordt bepaald hoe gegevens zijn verdeeld over fysieke partities. Gegevens die behoren tot een enkele partitie-sleutel (bijvoorbeeld 'city' == "Seattle") zijn opgeslagen in een fysieke partitie, maar één fysieke partitie heeft doorgaans meerdere partitiesleutels. Wanneer een partitie zijn opslaggrootte bereikt, de service naadloos de partitie worden gesplitst in twee nieuwe partities en de partitiesleutel gelijkmatig wordt verdeeld over deze partities. Aangezien partities tijdelijke zijn, gebruiken de API's een abstractie van een 'partitiesleutelbereik', waarmee de bereiken van de belangrijkste hashes partitie. 

Wanneer u een query voor Azure Cosmos DB uitgeeft, voert de SDK deze logische stappen:

* Parseren van de SQL-query om te bepalen van het queryplan kan worden uitgevoerd. 
* Als de query een filter voor de partitiesleutel bevat, zoals `SELECT * FROM c WHERE c.city = "Seattle"`, wordt deze doorgestuurd naar één partitie. Als de query heeft geen een filter voor de partitiesleutel en vervolgens deze wordt uitgevoerd in alle partities en de resultaten worden samengevoegd clientzijde.
* De query is uitgevoerd binnen elke partitie in de reeks of parallel, op basis van de clientconfiguratie. Binnen elke partitie kan in de query maken een of meer retouren afhankelijk van de complexiteit query paginagrootte geconfigureerd en ingerichte doorvoer van de verzameling. Elke uitvoering retourneert het aantal [aanvraageenheden](request-units.md) verbruikt door uitvoering van de query, en desgewenst Uitvoeringsstatistieken query. 
* De SDK voert een samenvatting van de queryresultaten meerdere partities. Bijvoorbeeld als de query een ORDER BY meerdere partities moet, zijn vervolgens resultaten uit afzonderlijke partities voor het retourneren van resultaten in een globaal gesorteerde volgorde gesorteerd samenvoegen. Als de query een aggregatie zoals is `COUNT`, het aantal uit afzonderlijke partities voor het produceren van het totale aantal worden opgeteld.

De SDK's bieden verschillende opties voor het uitvoeren van de query. Bijvoorbeeld in .NET deze opties zijn beschikbaar in de `FeedOptions` klasse. De volgende tabel beschrijft deze opties en hoe ze invloed hebben op uitvoeringstijd van de query. 

| Optie | Beschrijving |
| ------ | ----------- |
| `EnableCrossPartitionQuery` | Moet worden ingesteld op true voor een query die moet worden uitgevoerd op meer dan één partitie. Dit is een expliciete vlag zodat u deze kunt u weloverwogen prestatie-en nadelen tijdens de ontwikkeling. |
| `EnableScanInQuery` | Moet worden ingesteld op true als u ervoor hebt gekozen buiten het indexeren, maar de query toch uitvoeren via een scan. Alleen is van toepassing als indexering voor de aangevraagde filterpad uitgeschakeld. | 
| `MaxItemCount` | Het maximum aantal items per round trip terugkeren naar de server. Door de instelling op-1 kunt u de server die het aantal items beheren. Of u kunt deze waarde voor het ophalen van een klein aantal items per round trip verlagen. 
| `MaxBufferedItemCount` | Dit is een optie voor de clientzijde, en gebruikt voor het beperken van het geheugenverbruik bij het uitvoeren van cross-partitie ORDER BY. Een hogere waarde vermindert de latentie van cross-partitie sorteren. |
| `MaxDegreeOfParallelism` | Opgehaald of ingesteld van het aantal gelijktijdige bewerkingen clientzijde tijdens parallelle queryuitvoering in de database-service van Azure DB die Cosmos uitgevoerd. Een positief eigenschapswaarde beperkt het aantal gelijktijdige bewerkingen op de waarde ingesteld. Als deze is ingesteld op minder dan 0, besluit het systeem automatisch het aantal gelijktijdige bewerkingen om uit te voeren. |
| `PopulateQueryMetrics` | Hiermee gedetailleerde logboekregistratie van statistieken voor de tijd die is doorgebracht in verschillende fasen van de uitvoering van de query zoals compileertijd en index Lustijd document laden tijd. U kunt uitvoer van de query statistieken delen met Azure-ondersteuning voor het vaststellen van prestatieproblemen met de query. |
| `RequestContinuation` | U kunt de queryuitvoering hervatten door door te geven in het ondoorzichtige vervolgtoken geretourneerd door een query. Het vervolgtoken ingekapseld alle staat is vereist voor uitvoering van de query. |
| `ResponseContinuationTokenLimitInKb` | U kunt de maximale grootte van het vervolgtoken geretourneerd door de server kunt beperken. Mogelijk moet u dit instellen als uw toepassingshost limieten op antwoord header-grootte. In te stellen, kan de totale duur en RUs gebruikt voor de query verhogen.  |

Bijvoorbeeld, u gaat nu een voorbeeldquery op partitiesleutel is aangevraagd op een verzameling met `/city` als de partitie sleutel en ingericht met 100.000 RU/s doorvoer. U deze query met aanvragen `CreateDocumentQuery<T>` in .NET als volgt:

```cs
IDocumentQuery<dynamic> query = client.CreateDocumentQuery(
    UriFactory.CreateDocumentCollectionUri(DatabaseName, CollectionName), 
    "SELECT * FROM c WHERE c.city = 'Seattle'", 
    new FeedOptions 
    { 
        PopulateQueryMetrics = true, 
        MaxItemCount = -1, 
        MaxDegreeOfParallelism = -1, 
        EnableCrossPartitionQuery = true 
    }).AsDocumentQuery();

FeedResponse<dynamic> result = await query.ExecuteNextAsync();
```

De SDK-codefragment hierboven, overeenkomt met de volgende REST-API-aanvraag:

```
POST https://arramacquerymetrics-westus.documents.azure.com/dbs/db/colls/sample/docs HTTP/1.1
x-ms-continuation: 
x-ms-documentdb-isquery: True
x-ms-max-item-count: -1
x-ms-documentdb-query-enablecrosspartition: True
x-ms-documentdb-query-parallelizecrosspartitionquery: True
x-ms-documentdb-query-iscontinuationexpected: True
x-ms-documentdb-populatequerymetrics: True
x-ms-date: Tue, 27 Jun 2017 21:52:18 GMT
authorization: type%3dmaster%26ver%3d1.0%26sig%3drp1Hi83Y8aVV5V6LzZ6xhtQVXRAMz0WNMnUuvriUv%2b4%3d
x-ms-session-token: 7:8,6:2008,5:8,4:2008,3:8,2:2008,1:8,0:8,9:8,8:4008
Cache-Control: no-cache
x-ms-consistency-level: Session
User-Agent: documentdb-dotnet-sdk/1.14.1 Host/32-bit MicrosoftWindowsNT/6.2.9200.0
x-ms-version: 2017-02-22
Accept: application/json
Content-Type: application/query+json
Host: arramacquerymetrics-westus.documents.azure.com
Content-Length: 52
Expect: 100-continue

{"query":"SELECT * FROM c WHERE c.city = 'Seattle'"}
```

Elke pagina van de uitvoering van query komt overeen met een REST-API `POST` met de `Accept: application/query+json` header en de SQL-query in de hoofdtekst. Elke query kunnen een of meer retouren naar de server met de `x-ms-continuation` token gekopieerd tussen de client en server uitvoering te hervatten. De configuratieopties in FeedOptions worden doorgegeven aan de server in de vorm van de aanvraagheaders. Bijvoorbeeld: `MaxItemCount` komt overeen met `x-ms-max-item-count`. 

De aanvraag de volgende (afgekapt voor leesbaarheid) antwoord geretourneerd:

```
HTTP/1.1 200 Ok
Cache-Control: no-store, no-cache
Pragma: no-cache
Transfer-Encoding: chunked
Content-Type: application/json
Server: Microsoft-HTTPAPI/2.0
Strict-Transport-Security: max-age=31536000
x-ms-last-state-change-utc: Tue, 27 Jun 2017 21:01:57.561 GMT
x-ms-resource-quota: documentSize=10240;documentsSize=10485760;documentsCount=-1;collectionSize=10485760;
x-ms-resource-usage: documentSize=1;documentsSize=884;documentsCount=2000;collectionSize=1408;
x-ms-item-count: 2000
x-ms-schemaversion: 1.3
x-ms-alt-content-path: dbs/db/colls/sample
x-ms-content-path: +9kEANVq0wA=
x-ms-xp-role: 1
x-ms-documentdb-query-metrics: totalExecutionTimeInMs=33.67;queryCompileTimeInMs=0.06;queryLogicalPlanBuildTimeInMs=0.02;queryPhysicalPlanBuildTimeInMs=0.10;queryOptimizationTimeInMs=0.00;VMExecutionTimeInMs=32.56;indexLookupTimeInMs=0.36;documentLoadTimeInMs=9.58;systemFunctionExecuteTimeInMs=0.00;userFunctionExecuteTimeInMs=0.00;retrievedDocumentCount=2000;retrievedDocumentSize=1125600;outputDocumentCount=2000;writeOutputTimeInMs=18.10;indexUtilizationRatio=1.00
x-ms-request-charge: 604.42
x-ms-serviceversion: version=1.14.34.4
x-ms-activity-id: 0df8b5f6-83b9-4493-abda-cce6d0f91486
x-ms-session-token: 2:2008
x-ms-gatewayversion: version=1.14.33.2
Date: Tue, 27 Jun 2017 21:59:49 GMT
```

De belangrijkste antwoordheaders geretourneerd van de query omvatten het volgende:

| Optie | Beschrijving |
| ------ | ----------- |
| `x-ms-item-count` | Het aantal items in het antwoord geretourneerd. Dit is afhankelijk van de opgegeven `x-ms-max-item-count`, het aantal items dat binnen de maximale reactiegrootte nettolading, de ingerichte doorvoer en de uitvoeringstijd van de query past. |  
| `x-ms-continuation:` | Het vervolgtoken uitvoering van de query te hervatten als extra resultaten beschikbaar zijn. | 
| `x-ms-documentdb-query-metrics` | De statistieken van de query voor de uitvoering. Dit is een tekenreeks met scheidingstekens met statistieken van de tijd besteed aan de verschillende fasen van de uitvoering van de query. Geretourneerde als `x-ms-documentdb-populatequerymetrics` is ingesteld op `True`. | 
| `x-ms-request-charge` | Het aantal [aanvraageenheden](request-units.md) verbruikt door de query. | 

Zie voor meer informatie over de REST-API-headers voor aanvraag en de opties [opvragen van resources met behulp van de REST-API](https://docs.microsoft.com/rest/api/documentdb/querying-documentdb-resources-using-the-rest-api).

## <a name="best-practices-for-query-performance"></a>Aanbevolen procedures voor prestaties van query 's
Hieronder vindt u de meest voorkomende factoren die van invloed op prestaties van Azure DB die Cosmos-query's. We verdiepen in elk van deze onderwerpen in dit artikel.

| Factor | Tip | 
| ------ | -----| 
| Ingerichte doorvoer | Meet RU per query en zorg ervoor dat u de vereiste ingerichte doorvoer voor uw query's. | 
| Partitionering en partitiesleutels | Voorkeur voor query's met de waarde voor de partitiesleutel in de filtercomponent voor lage latentie. |
| SDK en query-opties | Volg de aanbevolen procedures van de SDK zoals directe verbinding en clientzijde uitvoering queryopties afstemmen. |
| Netwerklatentie | Account voor netwerk overhead in meting en multihoming API's gebruiken om te lezen uit de dichtstbijzijnde regio. |
| Indexeringsbeleid | Zorg ervoor dat u de vereiste paden/indexeringsbeleid voor de query. |
| Query-uitvoering metrische gegevens | Analyseer de query-uitvoering metrische gegevens om te identificeren mogelijke regeneraties van query's en vormen.  |

### <a name="provisioned-throughput"></a>Ingerichte doorvoer
In de Cosmos-DB maakt u containers voor gegevens, elk met gereserveerde doorvoer, uitgedrukt in de aanvraag-eenheden (RU) per de seconde. Het lezen van een document van 1 KB is 1 RU en elke bewerking (met inbegrip van query's) op een vast aantal RUs op basis van de complexiteit is genormaliseerd. Bijvoorbeeld, als er 1000 RU/s ingericht voor de container en u hebt een query zoals `SELECT * FROM c WHERE c.city = 'Seattle'` die 5 RUs verbruikt en vervolgens kunt u uitvoeren (1000 RU/s) / (5 RU/query) = 200 query/s dergelijke query's per seconde. 

Als u meer dan 200 query's per seconde indient, start de service binnenkomende aanvragen frequentiebeperkende hierboven 200/s. In dit geval de SDK's automatisch verwerkt door het uitvoeren van een backoff/nieuwe poging, dus u wellicht opgevallen dat een hogere latentie voor deze query's. De ingerichte doorvoer voor de vereiste waarde verhogen verbetert uw Querylatentie en de doorvoer. 

Zie voor meer informatie over aanvraageenheden, [Aanvraageenheden](request-units.md).

### <a name="partitioning-and-partition-keys"></a>Partitionering en partitiesleutels
Met Azure Cosmos DB doorgaans uitvoeren query's in de volgende volgorde van de snelste/meest efficiënte naar tragere/minder efficiënt. 

* Op een enkele partitie en item sleutel ophalen
* Query's uitvoeren met een filter-component op een enkele partitiesleutel
* Query uitvoeren zonder dat een gelijkheid of bereik filter-component op een eigenschap
* Een query uitvoeren zonder filters

Query's die u moeten alle partities Raadpleeg moeten hogere latentie, en hogere RUs kunnen gebruiken. Aangezien elke partitie automatisch indexeren tegen alle eigenschappen heeft, kan de query worden geleverd efficiënt vanaf de index in dit geval. U kunt query's die partities sneller omvatten met behulp van de opties voor parallelle uitvoering.

Zie voor meer informatie over partitioneren en partitiesleutels, [partitioneren in Azure Cosmos DB](partition-data.md).

### <a name="sdk-and-query-options"></a>SDK en query-opties
Zie [Tips voor betere prestaties](performance-tips.md) en [prestatietests](performance-testing.md) voor het ophalen van de beste prestaties voor de clientzijde van Azure Cosmos DB. Dit omvat met de nieuwste SDK's, platform-specifieke netwerkconfiguraties als standaardaantal verbindingen, frequentie van garbagecollection, configureren en lightweight connectiviteitsopties zoals Direct/TCP. 


#### <a name="max-item-count"></a>Maximum aantal items
Voor query's, de waarde van `MaxItemCount` kan een aanzienlijke invloed hebben op het moment dat de end-to-end-query. Elke retouren naar de server resulteert niet vaker dan het aantal items in `MaxItemCount` (standaard van 100 objecten). Als u dit op een hogere waarde (-1 is maximum- en aanbevolen) worden uw totale duur van de query verbeteren door het aantal retouren tussen server en client, met name voor query's met grote resultatensets te beperken.

```cs
IDocumentQuery<dynamic> query = client.CreateDocumentQuery(
    UriFactory.CreateDocumentCollectionUri(DatabaseName, CollectionName), 
    "SELECT * FROM c WHERE c.city = 'Seattle'", 
    new FeedOptions 
    { 
        MaxItemCount = -1, 
    }).AsDocumentQuery();
```

#### <a name="max-degree-of-parallelism"></a>Maximale mate van parallelle uitvoering
Stemmen voor query's, de `MaxDegreeOfParallelism` voor het identificeren van de aanbevolen configuraties voor uw toepassing, met name als u cross-partitie query's (zonder een filter op de partitie-sleutelwaarde) uitvoeren. `MaxDegreeOfParallelism`Hiermee bepaalt u het maximum aantal parallelle taken, dat wil zeggen, het maximum van partities parallel wordt bezocht. 

```cs
IDocumentQuery<dynamic> query = client.CreateDocumentQuery(
    UriFactory.CreateDocumentCollectionUri(DatabaseName, CollectionName), 
    "SELECT * FROM c WHERE c.city = 'Seattle'", 
    new FeedOptions 
    { 
        MaxDegreeOfParallelism = -1, 
        EnableCrossPartitionQuery = true 
    }).AsDocumentQuery();
```

Stel dat
* D = standaard maximale aantal parallelle taken (= totaal aantal processor in de client-computer)
* P = gebruiker opgegeven maximale aantal parallelle taken
* N het aantal partities die behoeften voor het beantwoorden van een query wordt bezocht =

Hieronder vindt u de implicaties van hoe de parallelle query's voor verschillende waarden van P. zou werken
* (P == 0) = > seriële modus
* (P == 1) = > maximaal één taak
* (P > 1) = > parallelle taken Min (P, N) 
* (P < 1) = > parallelle taken Min (N, D)

Voor SDK-releaseopmerkingen en Zie voor meer informatie over geïmplementeerde klassen en methoden [SQL-SDK's](sql-api-sdk-dotnet.md)

### <a name="network-latency"></a>Netwerklatentie
Zie [Azure Cosmos DB globale distributie](tutorial-global-distribution-sql-api.md) voor het instellen van de algemene distributie en verbinding maken met de dichtstbijzijnde regio. Netwerklatentie heeft een aanzienlijke invloed op prestaties van query's als u wilt meerdere retouren maken of een grote resultatenset van de query worden opgehaald. 

De sectie over metrische gegevens query-uitvoering wordt uitgelegd hoe u de tijd van de server uitvoeren van query's niet ophalen ( `totalExecutionTimeInMs`), zodat u tijd besteed aan het uitvoeren van de query en -tijd besteed aan het netwerk onderweg kunt onderscheiden.

### <a name="indexing-policy"></a>Indexeringsbeleid
Zie [indexeringsbeleid configureren](indexing-policies.md) voor indexeren paden, soorten, en modi en invloed op uitvoering van de query. Het indexeringsbeleid gebruikt standaard hash-indexering voor tekenreeksen, die geldt voor gelijkheid query's, maar niet voor query's bereik/order by-query's. Als u een bereik query's nodig voor tekenreeksen, wordt u aangeraden het indextype bereik voor alle tekenreeksen geven. 

## <a name="query-execution-metrics"></a>Query-uitvoering metrische gegevens
U kunt gedetailleerde metrische gegevens op de uitvoering van de query verkrijgen door door te geven in de optionele `x-ms-documentdb-populatequerymetrics` header (`FeedOptions.PopulateQueryMetrics` in de .NET SDK). De waarde die in `x-ms-documentdb-query-metrics` heeft de volgende sleutel-waardeparen bedoeld voor geavanceerde probleemoplossing van de uitvoering van de query. 

```cs
IDocumentQuery<dynamic> query = client.CreateDocumentQuery(
    UriFactory.CreateDocumentCollectionUri(DatabaseName, CollectionName), 
    "SELECT * FROM c WHERE c.city = 'Seattle'", 
    new FeedOptions 
    { 
        PopulateQueryMetrics = true, 
    }).AsDocumentQuery();

FeedResponse<dynamic> result = await query.ExecuteNextAsync();

// Returns metrics by partition key range Id
IReadOnlyDictionary<string, QueryMetrics> metrics = result.QueryMetrics;

```

| Gegevens | Eenheid | Beschrijving | 
| ------ | -----| ----------- |
| `totalExecutionTimeInMs` | milliseconden | Uitvoeringstijd van de query | 
| `queryCompileTimeInMs` | milliseconden | Query-compilatie  | 
| `queryLogicalPlanBuildTimeInMs` | milliseconden | Tijd voor het bouwen van logische queryplan | 
| `queryPhysicalPlanBuildTimeInMs` | milliseconden | Tijd voor het bouwen van fysieke queryplan | 
| `queryOptimizationTimeInMs` | milliseconden | Tijd besteed aan de query te optimaliseren | 
| `VMExecutionTimeInMs` | milliseconden | Tijd besteed aan de query-runtime | 
| `indexLookupTimeInMs` | milliseconden | Tijd besteed aan fysieke index laag | 
| `documentLoadTimeInMs` | milliseconden | Tijd besteed aan het laden van documenten  | 
| `systemFunctionExecuteTimeInMs` | milliseconden | Totale tijd besteed aan het uitvoerende system (ingebouwde) functies in milliseconden  | 
| `userFunctionExecuteTimeInMs` | milliseconden | Totale tijd besteed worden uitgevoerd door de gebruiker gedefinieerde functies in milliseconden | 
| `retrievedDocumentCount` | aantal | Totaal aantal opgehaalde documenten  | 
| `retrievedDocumentSize` | bytes | Totale grootte van de opgehaalde documenten in bytes  | 
| `outputDocumentCount` | aantal | Het aantal uitvoerdocumenten | 
| `writeOutputTimeInMs` | milliseconden | Uitvoeringstijd van de query in milliseconden | 
| `indexUtilizationRatio` | verhouding (< = 1) | Verhouding tussen aantal documenten dat overeenkomt met het filter aan het aantal documenten die worden geladen  | 

De client-SDK's kunnen meerdere querybewerkingen voor het uitvoeren van de query binnen elke partitie intern stellen. Maakt de client meer dan één aanroep per partitie als de totale resultaten overschrijdt `x-ms-max-item-count`, als de query de ingerichte doorvoer voor de partitie overschrijdt als de nettolading van de query de maximale grootte per pagina bereikt of als de query het systeem die zijn toegewezen bereikt time-outlimiet. Elke gedeeltelijke queryuitvoering retourneert een `x-ms-documentdb-query-metrics` voor die pagina. 

Hier volgen enkele voorbeeldquery's en het interpreteren van de metrische gegevens die door uitvoering van de query zijn geretourneerd: 

| Query’s uitvoeren | Voorbeeld metrische gegevens | Beschrijving | 
| ------ | -----| ----------- |
| `SELECT TOP 100 * FROM c` | `"RetrievedDocumentCount": 101` | Het aantal documenten opgehaald is 100 + 1 overeenkomen met de TOP-component. Querytijd voornamelijk die is doorgebracht in `WriteOutputTime` en `DocumentLoadTime` omdat het een scan. | 
| `SELECT TOP 500 * FROM c` | `"RetrievedDocumentCount": 501` | RetrievedDocumentCount is nu hogere (500 + 1 overeenkomen met de TOP-component). | 
| `SELECT * FROM c WHERE c.N = 55` | `"IndexLookupTime": "00:00:00.0009500"` | Ongeveer 0,9 ms is besteed aan IndexLookupTime voor een sleutel-lookup, omdat deze het opzoeken van een index op `/N/?`. | 
| `SELECT * FROM c WHERE c.N > 55` | `"IndexLookupTime": "00:00:00.0017700"` | Iets meer tijd (1,7 ms) die is doorgebracht in IndexLookupTime via een scan bereik omdat dit het opzoeken van een index op `/N/?`. | 
| `SELECT TOP 500 c.N FROM c` | `"IndexLookupTime": "00:00:00.0017700"` | Dezelfde tijd besteed aan `DocumentLoadTime` als de voorgaande query's, maar lagere `WriteOutputTime` omdat we bij het projecteren van slechts één eigenschap. | 
| `SELECT TOP 500 udf.toPercent(c.N) FROM c` | `"UserDefinedFunctionExecutionTime": "00:00:00.2136500"` | Punt 213 ms is die is doorgebracht in `UserDefinedFunctionExecutionTime` de UDF worden uitgevoerd op elke waarde van `c.N`. |
| `SELECT TOP 500 c.Name FROM c WHERE STARTSWITH(c.Name, 'Den')` | `"IndexLookupTime": "00:00:00.0006400", "SystemFunctionExecutionTime": "00:00:00.0074100"` | Er is ongeveer 0,6 ms gespendeerd aan het `IndexLookupTime` op `/Name/?`. Van de query uitvoeringstijd (ms ~ 7) in de meeste `SystemFunctionExecutionTime`. |
| `SELECT TOP 500 c.Name FROM c WHERE STARTSWITH(LOWER(c.Name), 'den')` | `"IndexLookupTime": "00:00:00", "RetrievedDocumentCount": 2491,  "OutputDocumentCount": 500` | Query als een scan wordt uitgevoerd, omdat deze gebruikmaakt van `LOWER`, en 500 buiten 2491 opgehaalde documenten worden geretourneerd. |


## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over de ondersteunde SQL-query's en trefwoorden, [SQL-query](sql-api-sql-query.md). 
* Zie voor meer informatie over aanvraageenheden, [aanvraageenheden](request-units.md).
* Zie voor meer informatie over indexeringsbeleid, [indexeren van beleid](indexing-policies.md) 


