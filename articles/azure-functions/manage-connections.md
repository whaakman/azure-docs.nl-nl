---
title: Verbindingen beheren in Azure Functions
description: Leer hoe u om te voorkomen dat problemen met de prestaties in Azure Functions met behulp van statische verbinding-clients.
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: conceptual
ms.date: 02/25/2018
ms.author: glenga
ms.openlocfilehash: 30d578f130985548c431dea8b68ee291325b5c99
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58893212"
---
# <a name="manage-connections-in-azure-functions"></a>Verbindingen beheren in Azure Functions

Functies in een functie-app resources delen. Zijn de verbindingen tussen deze gedeelde bronnen: HTTP-verbindingen, databaseverbindingen en verbindingen met services zoals Azure Storage. Wanneer veel functies gelijktijdig worden uitgevoerd, is het mogelijk te weinig beschikbare verbindingen. In dit artikel wordt uitgelegd hoe u code van uw functies om te voorkomen met behulp van meer verbindingen dan ze nodig hebben.

## <a name="connection-limit"></a>Limiet voor verbindingen

Het aantal beschikbare verbindingen is beperkt, deels omdat een functie-app wordt uitgevoerd in een [sandbox-omgeving](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox). Een van de beperkingen die de sandbox opgelegd, uw code is een [bovengrens voor het aantal verbindingen (dat zich momenteel in 600 actieve verbindingen en 1200 totaal aantal verbindingen)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox#numerical-sandbox-limits) per exemplaar. Wanneer u deze limiet is bereikt, de functions-runtime wordt een logboekbestand gemaakt met het volgende bericht: `Host thresholds exceeded: Connections`.

Deze limiet is per exemplaar.  Wanneer de [schaal controller voegt de functie-app-instanties](functions-scale.md#how-the-consumption-and-premium-plans-work) voor het afhandelen van meer aanvragen, heeft elk exemplaar een onafhankelijke verbindingslimiet. Dit betekent dat er is geen limiet globale verbinding, en u kunt veel meer dan 600 actieve verbindingen voor alle actieve exemplaren.

## <a name="static-clients"></a>Statische clients

Om te voorkomen dat bevat meer verbindingen dan nodig, opnieuw gebruiken clientexemplaren in plaats van het maken van nieuwe kennis met elke functieaanroep. Het is raadzaam om hergebruik van clientverbindingen voor elke taal die u mogelijk uw functie schrijven in. Bijvoorbeeld, .NET-clients, zoals de [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx), [DocumentClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient
), en Azure Storage-clients verbindingen kunnen beheren als u een enkele, statische-client gebruiken.

Hier vindt u enkele richtlijnen te volgen als u een client servicespecifieke in een Azure Functions-toepassing:

- *Geen* maken van een nieuwe client met elke functie-aanroep.
- *Voer* een enkele, statische-client die gebruikmaken van elke functie-aanroep kunt maken.
- *Houd rekening met* het maken van een enkele, statische-client in een gedeelde helperklasse als dezelfde service verschillende functies gebruiken.

## <a name="client-code-examples"></a>Client-codevoorbeelden

Deze sectie wordt beschreven aanbevolen procedures voor het maken en gebruiken van clients in uw functiecode aan te geven.

### <a name="httpclient-example-c"></a>Voorbeeld van HttpClient (C#)

Hier volgt een voorbeeld van C# code die wordt gemaakt van een statische functie [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx) exemplaar:

```cs
// Create a single, static HttpClient
private static HttpClient httpClient = new HttpClient();

public static async Task Run(string input)
{
    var response = await httpClient.GetAsync("https://example.com");
    // Rest of function
}
```

Een algemene vraag over [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx) in .NET is "Moet ik buitengebruikstelling van mijn klant?" In het algemeen de buitengebruikstelling van objecten die worden geïmplementeerd `IDisposable` wanneer u klaar bent met behulp van deze. Maar u geen buitengebruikstelling van een statische client, omdat u niet worden uitgevoerd met behulp van deze wanneer de functie wordt beëindigd. U wilt dat de statische client live voor de duur van uw toepassing.

### <a name="http-agent-examples-javascript"></a>Voorbeelden van HTTP-agent (JavaScript)

Omdat deze beter verbinding opties voor beheer biedt, moet u de native [ `http.agent` ](https://nodejs.org/dist/latest-v6.x/docs/api/http.html#http_class_http_agent) klasse in plaats van niet-eigen methoden, zoals de `node-fetch` module. Verbindingsparameters worden geconfigureerd via de opties op de `http.agent` klasse. Zie voor gedetailleerde opties beschikbaar voor de HTTP-agent, [nieuwe-Agent (\[opties\])](https://nodejs.org/dist/latest-v6.x/docs/api/http.html#http_new_agent_options).

De globale `http.globalAgent` klasse die wordt gebruikt door `http.request()` heeft al deze waarden ingesteld op hun respectievelijke standaardwaarden ingesteld. De aanbevolen manier om de limieten configureren in de functies is het maximum aantal wereldwijd instellen. Het volgende voorbeeld wordt het maximum aantal sockets voor de functie-app:

```js
http.globalAgent.maxSockets = 200;
```

 Het volgende voorbeeld wordt een nieuwe HTTP-aanvraag met een aangepaste HTTP-agent alleen voor deze aanvraag:

```js
var http = require('http');
var httpAgent = new http.Agent();
httpAgent.maxSockets = 200;
options.agent = httpAgent;
http.request(options, onResponseCallback);
```

### <a name="documentclient-code-example-c"></a>Voorbeeld van de DocumentClient-code (C#)

[DocumentClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient
) verbinding maakt met een Azure Cosmos DB-exemplaar. De documentatie van Azure Cosmos DB wordt aanbevolen dat u [een singleton-Azure Cosmos DB-client gebruiken voor de levensduur van uw toepassing](https://docs.microsoft.com/azure/cosmos-db/performance-tips#sdk-usage). Het volgende voorbeeld ziet u een patroon voor het uitvoeren van die in een functie:

```cs
#r "Microsoft.Azure.Documents.Client"
using Microsoft.Azure.Documents.Client;

private static Lazy<DocumentClient> lazyClient = new Lazy<DocumentClient>(InitializeDocumentClient);
private static DocumentClient documentClient => lazyClient.Value;

private static DocumentClient InitializeDocumentClient()
{
    // Perform any initialization here
    var uri = new Uri("example");
    var authKey = "authKey";
    
    return new DocumentClient(uri, authKey);
}

public static async Task Run(string input)
{
    Uri collectionUri = UriFactory.CreateDocumentCollectionUri("database", "collection");
    object document = new { Data = "example" };
    await documentClient.UpsertDocumentAsync(collectionUri, document);
    
    // Rest of function
}
```

### <a name="cosmosclient-code-example-javascript"></a>Het codevoorbeeld CosmosClient (JavaScript)
[CosmosClient](/javascript/api/@azure/cosmos/cosmosclient) verbinding maakt met een Azure Cosmos DB-exemplaar. De documentatie van Azure Cosmos DB wordt aanbevolen dat u [een singleton-Azure Cosmos DB-client gebruiken voor de levensduur van uw toepassing](../cosmos-db/performance-tips.md#sdk-usage). Het volgende voorbeeld ziet u een patroon voor het uitvoeren van die in een functie:

```javascript
const cosmos = require('@azure/cosmos');
const endpoint = process.env.COSMOS_API_URL;
const masterKey = process.env.COSMOS_API_KEY;
const { CosmosClient } = cosmos;

const client = new CosmosClient({ endpoint, auth: { masterKey } });
// All function invocations also reference the same database and container.
const container = client.database("MyDatabaseName").container("MyContainerName");

module.exports = async function (context) {
    const { result: itemArray } = await container.items.readAll().toArray();
    context.log(itemArray);
}
```

## <a name="sqlclient-connections"></a>SqlClient-verbindingen

Uw functiecode aan te geven de .NET Framework Data Provider voor SQL Server kunt gebruiken ([SqlClient](https://msdn.microsoft.com/library/system.data.sqlclient(v=vs.110).aspx)) verbinding te maken met een relationele SQL-database. Dit is ook de onderliggende provider voor gegevens-frameworks die afhankelijk van ADO.NET, zoals zijn [Entity Framework](https://msdn.microsoft.com/library/aa937723(v=vs.113).aspx). In tegenstelling tot [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx) en [DocumentClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient
) verbindingen, ADO.NET implementeert standaard Groepsgewijze verbinding. Maar omdat u kunt nog steeds worden uitgevoerd buiten-verbindingen, moet u verbindingen met de database optimaliseren. Zie voor meer informatie, [SQL Server-verbinding groeperen (ADO.NET)](https://docs.microsoft.com/dotnet/framework/data/adonet/sql-server-connection-pooling).

> [!TIP]
> Doorgaans downloadt verbindingsreeksen van sommige gegevens-frameworks, zoals Entity Framework, de **ConnectionStrings** gedeelte van een configuratiebestand. In dit geval moet u expliciet verbindingsreeksen van de SQL-database naar toevoegen de **verbindingsreeksen** verzameling van de instellingen van uw functie-app en in de [bestand local.settings.json](functions-run-local.md#local-settings-file) in uw lokale project. Als u een exemplaar van maakt [SqlConnection](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection(v=vs.110).aspx) in uw functiecode aan te geven, moet u de connection string-waarde in opslaan **toepassingsinstellingen** met uw andere verbindingen.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over waarom het beste statische clients [antipatroon onjuiste instantiëring](https://docs.microsoft.com/azure/architecture/antipatterns/improper-instantiation/).

Zie voor meer tips voor Azure Functions-prestaties, [optimaliseren de prestaties en betrouwbaarheid van Azure Functions](functions-best-practices.md).
