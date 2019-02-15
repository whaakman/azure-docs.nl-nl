---
title: Over het beheren van verbindingen in Azure Functions
description: Leer hoe u om te voorkomen dat problemen met de prestaties in Azure Functions met behulp van statische verbinding-clients.
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: conceptual
ms.date: 11/02/2018
ms.author: glenga
ms.openlocfilehash: 4246259445cf096b5353ab87a9ed83f87332dc78
ms.sourcegitcommit: f863ed1ba25ef3ec32bd188c28153044124cacbc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/15/2019
ms.locfileid: "56299323"
---
# <a name="how-to-manage-connections-in-azure-functions"></a>Over het beheren van verbindingen in Azure Functions

Functies in een functie-app-resources delen en tussen deze gedeelde resources zijn verbindingen &mdash; HTTP-verbindingen, databaseverbindingen en verbindingen met Azure-services zoals opslag. Wanneer veel functies gelijktijdig worden uitgevoerd, is het mogelijk te weinig beschikbare verbindingen. In dit artikel wordt uitgelegd hoe u code van uw functies om te voorkomen met behulp van meer verbindingen dan ze echt nodig hebt.

## <a name="connections-limit"></a>Limiet voor verbindingen

Het aantal beschikbare verbindingen is beperkt, deels omdat een functie-app wordt uitgevoerd in de [Azure App Service-sandbox](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox). Een van de beperkingen die de sandbox opgelegd, uw code is een [bovengrens voor het aantal verbindingen, momenteel 300](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox#numerical-sandbox-limits). Wanneer u deze limiet is bereikt, de functions-runtime wordt een logboekbestand gemaakt met het volgende bericht: `Host thresholds exceeded: Connections`.

De kans op meer dan de limiet gaat wanneer de [schaal controller voegt de functie-app-instanties](functions-scale.md#how-the-consumption-plan-works) om meer aanvragen te verwerken. Elke functie-app-exemplaar kan veel functies tegelijk, die allemaal zijn tellen mee voor de limiet van 300 verbindingen met een worden uitgevoerd.

## <a name="use-static-clients"></a>Gebruik statische clients

Om te voorkomen dat bevat meer verbindingen dan nodig, opnieuw gebruiken clientexemplaren in plaats van het maken van nieuwe kennis met elke functieaanroep. .NET-clients, zoals de [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx), [DocumentClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient
), en Azure Storage-clients verbindingen kunnen beheren als u een enkele, statische-client gebruiken.

Hier vindt u enkele richtlijnen te volgen bij het gebruik van een client servicespecifieke in een Azure Functions-toepassing:

- **GEEN** maken van een nieuwe client met elke functie-aanroep.
- **Voer** maken van een enkele, statische-client die kan worden gebruikt door elke functie-aanroep.
- **Houd rekening met** het maken van een enkele, statische-client in een gedeelde helperklasse als dezelfde service verschillende functies gebruiken.

## <a name="client-code-examples"></a>Client-codevoorbeelden

Deze sectie wordt beschreven aanbevolen procedures voor het maken en gebruiken van clients in uw functiecode aan te geven.

### <a name="httpclient-example-c"></a>Voorbeeld van HttpClient (C#)

Hier volgt een voorbeeld van C# code die wordt gemaakt van een statische functie [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx):

```cs
// Create a single, static HttpClient
private static HttpClient httpClient = new HttpClient();

public static async Task Run(string input)
{
    var response = await httpClient.GetAsync("https://example.com");
    // Rest of function
}
```

Een algemene vraag over de .NET [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx) is "Moet ik worden verwijdering mijn client?" In het algemeen is het verwijderen van objecten die worden geïmplementeerd `IDisposable` wanneer u klaar bent met behulp van deze. Maar u een statische client niet verwijderen omdat u niet worden uitgevoerd met behulp van deze wanneer de functie wordt beëindigd. U wilt dat de statische client live voor de duur van uw toepassing.

### <a name="http-agent-examples-nodejs"></a>Voorbeelden van HTTP-agent (Node.js)

Omdat deze beter verbinding opties voor beheer biedt, moet u de native [ `http.agent` ](https://nodejs.org/dist/latest-v6.x/docs/api/http.html#http_class_http_agent) klasse in plaats van niet-eigen methoden, zoals de `node-fetch` module. Verbindingsparameters zijn geconfigureerd met behulp van de opties op de `http.agent` klasse. Zie [nieuwe-Agent (\[opties\])](https://nodejs.org/dist/latest-v6.x/docs/api/http.html#http_new_agent_options) voor gedetailleerde opties beschikbaar zijn met de HTTP-agent.

De globale `http.globalAgent` die worden gebruikt door `http.request()` heeft al deze waarden ingesteld op hun respectievelijke standaardwaarden ingesteld. De aanbevolen manier om de limieten configureren in de functies is het maximum aantal wereldwijd instellen. Het volgende voorbeeld wordt het maximum aantal sockets voor de functie-app:

```js
http.globalAgent.maxSockets = 200;
```

 Het volgende voorbeeld wordt een nieuwe HTTP-aanvraag met een aangepaste HTTP-agent alleen voor deze aanvraag.

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

## <a name="sqlclient-connections"></a>SqlClient-verbindingen

Uw functiecode aan te geven de .NET Framework Data Provider voor SQL Server kan gebruiken ([SqlClient](https://msdn.microsoft.com/library/system.data.sqlclient(v=vs.110).aspx)) verbinding te maken met een relationele SQL-database. Dit is ook de onderliggende provider voor gegevens-frameworks die afhankelijk van ADO.NET, zoals Entity Framework zijn. In tegenstelling tot [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx) en [DocumentClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient
) verbindingen, ADO.NET implementeert standaard Groepsgewijze verbinding. Omdat u kunt nog steeds worden uitgevoerd buiten-verbindingen, moet u verbindingen met de database optimaliseren. Zie voor meer informatie, [SQL Server-verbinding groeperen (ADO.NET)](https://docs.microsoft.com/dotnet/framework/data/adonet/sql-server-connection-pooling).

> [!TIP]
> Sommige gegevens frameworks, zoals [Entity Framework](https://msdn.microsoft.com/library/aa937723(v=vs.113).aspx), doorgaans downloadt verbindingsreeksen uit de **ConnectionStrings** gedeelte van een configuratiebestand. In dit geval moet u expliciet verbindingsreeksen van de SQL-database naar toevoegen de **verbindingsreeksen** verzameling van de instellingen van uw functie-app en in de [bestand local.settings.json](functions-run-local.md#local-settings-file) in uw lokale project. Als u maakt een [SqlConnection](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection(v=vs.110).aspx) in uw functiecode aan te geven, moet u de connection string-waarde in opslaan **toepassingsinstellingen** met uw andere verbindingen.

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over de reden waarom statische clients worden aanbevolen, Zie [antipatroon onjuiste instantiëring](https://docs.microsoft.com/azure/architecture/antipatterns/improper-instantiation/).

Zie voor meer tips voor Azure Functions-prestaties, [optimaliseren de prestaties en betrouwbaarheid van Azure Functions](functions-best-practices.md).
