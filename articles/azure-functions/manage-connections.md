---
title: Over het beheren van verbindingen in Azure Functions
description: Leer hoe u om te voorkomen dat problemen met de prestaties in Azure Functions met behulp van statische verbinding-clients.
services: functions
documentationcenter: ''
author: tdykstra
manager: cfowler
editor: ''
ms.service: functions
ms.workload: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2018
ms.author: tdykstra
ms.openlocfilehash: 6c0af8f6f7e1d4aea8880a7af311aaa21f474f7e
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38969001"
---
# <a name="how-to-manage-connections-in-azure-functions"></a>Over het beheren van verbindingen in Azure Functions

Functies in een functie-app-resources delen en tussen deze gedeelde resources zijn verbindingen &mdash; HTTP-verbindingen, databaseverbindingen en verbindingen met Azure-services zoals opslag. Het is mogelijk te weinig beschikbare verbindingen wanneer er veel functies gelijktijdig worden uitgevoerd. In dit artikel wordt uitgelegd hoe u code van uw functies om te voorkomen met behulp van meer verbindingen dan ze echt nodig hebt.

## <a name="connections-limit"></a>Limiet voor verbindingen

Het aantal beschikbare verbindingen is beperkt, deels omdat een functie-app wordt uitgevoerd in de [Azure App Service-sandbox](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox). Een van de beperkingen die de sandbox opgelegd, uw code is een [bovengrens voor het aantal verbindingen, momenteel 300](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox#numerical-sandbox-limits). Wanneer u deze limiet is bereikt, de functions-runtime wordt een logboekbestand gemaakt met het volgende bericht: `Host thresholds exceeded: Connections`.

Kans op meer dan de limiet verhogen als de [schaal controller voegt de functie-app-instanties](functions-scale.md#how-the-consumption-plan-works). Elke functie-app-exemplaar kan worden aanroepen van functies vaak in één keer, en al deze functies mee voor de limiet van 300 tellen verbindingen gebruikt.

## <a name="use-static-clients"></a>Gebruik statische clients

Om te voorkomen dat bevat meer verbindingen dan nodig, opnieuw gebruiken clientexemplaren in plaats van het maken van nieuwe kennis met elke functieaanroep. .NET-clients, zoals de `HttpClient`, `DocumentClient`, en Azure Storage-clients verbindingen kunnen beheren als u een enkele, statische-client gebruiken.

Hier vindt u enkele richtlijnen te volgen bij het gebruik van een client servicespecifieke in een Azure Functions-toepassing:

- **GEEN** maken van een nieuwe client met elke functie-aanroep.
- **Voer** maken van een enkele, statische-client die kan worden gebruikt door elke functie-aanroep.
- **Houd rekening met** het maken van een enkele, statische-client in een gedeelde helperklasse als dezelfde service verschillende functies gebruiken.

## <a name="httpclient-code-example"></a>Codevoorbeeld httpclient maakt

Hier volgt een voorbeeld van de functiecode die wordt gemaakt van een statische `HttpClient`:

```cs
// Create a single, static HttpClient
private static HttpClient httpClient = new HttpClient();

public static async Task Run(string input)
{
    var response = await httpClient.GetAsync("http://example.com");
    // Rest of function
}
```

Een algemene vraag over de .NET `HttpClient` is "Moet ik worden verwijdering mijn client?" In het algemeen is het verwijderen van objecten die worden geïmplementeerd `IDisposable` wanneer u klaar bent met behulp van deze. Maar u een statische client niet verwijderen omdat u niet worden uitgevoerd met behulp van deze wanneer de functie wordt beëindigd. U wilt dat de statische client live voor de duur van uw toepassing.

## <a name="documentclient-code-example"></a>Voorbeeld van de DocumentClient-code

`DocumentClient` maakt verbinding met een Cosmos DB-exemplaar. De Cosmos DB-documentatie wordt aanbevolen dat u [een singleton-Azure Cosmos DB-client gebruiken voor de levensduur van uw toepassing](https://docs.microsoft.com/azure/cosmos-db/performance-tips#sdk-usage). Het volgende voorbeeld ziet een patroon voor het uitvoeren van die in een functie.

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

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over de reden waarom statische clients worden aanbevolen, Zie [antipatroon onjuiste instantiëring](https://docs.microsoft.com/azure/architecture/antipatterns/improper-instantiation/).

Zie voor meer tips voor Azure Functions-prestaties, [optimaliseren de prestaties en betrouwbaarheid van Azure Functions](functions-best-practices.md).