---
title: Het beheren van de verbindingen in de Azure Functions
description: Informatie over het prestatieproblemen in Azure Functions vermijden met behulp van verbinding met statische-clients.
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
ms.openlocfilehash: 4ea2b033d8d67dd3c921fb833462605ba0aeb687
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34654918"
---
# <a name="how-to-manage-connections-in-azure-functions"></a>Het beheren van de verbindingen in de Azure Functions

Functies in een functie-app resources delen en tussen deze gedeelde bronnen zijn verbindingen &mdash; HTTP-verbindingen, databaseverbindingen en verbindingen met de Azure-services zoals opslag. Het is mogelijk te weinig beschikbare verbindingen wanneer er veel functies gelijktijdig worden uitgevoerd. In dit artikel wordt uitgelegd hoe uw functies om te voorkomen dat meer verbindingen dan ze daadwerkelijk nodig met een code.

## <a name="connections-limit"></a>Verbindingen beperken

Het aantal beschikbare verbindingen is beperkt, deels omdat een functie-app wordt uitgevoerd in de [Azure App Service sandbox](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox). Een van de beperkingen die de sandbox opgelegd, uw code is een [initiaal van het aantal verbindingen, momenteel 300](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox#numerical-sandbox-limits). Wanneer u deze limiet bereikt, de runtime van functions wordt een logboekbestand gemaakt met het volgende bericht: `Host thresholds exceeded: Connections`.

Kans om de limiet overschreden vergroot wanneer de [scale controller voegt de functie app-exemplaren](functions-scale.md#how-the-consumption-plan-works). Elke functie app-exemplaar kan worden aangeroepen functies vaak tegelijk, en al deze functies verbindingen die voor de limiet van 300 meetellen gebruikt.

## <a name="use-static-clients"></a>Statische clients gebruiken

Om te voorkomen dat bevat meer verbindingen dan nodig, opnieuw gebruiken exemplaren van de client in plaats van door nieuwe te maken met elke functie-aanroep. .NET-clients, zoals de `HttpClient`, `DocumentClient`, en Azure Storage-clients verbindingen kunnen beheren als u een enkele, statische client gebruiken.

Hier volgen enkele richtlijnen te volgen wanneer u een specifieke client in een Azure Functions-toepassing:

- **GEEN** maken van een nieuwe client met elke functie-aanroep.
- **Voer** maken van een enkele, statische client die kan worden gebruikt door elke functie-aanroep.
- **Houd rekening met** maken van een enkele, statische client in een gedeelde helperklasse als verschillende functies dezelfde service gebruiken.

## <a name="httpclient-code-example"></a>HttpClient codevoorbeeld

Hier volgt een voorbeeld van functiecode die wordt gemaakt van een statische `HttpClient`:

```cs
// Create a single, static HttpClient
private static HttpClient httpClient = new HttpClient();

public static async Task Run(string input)
{
    var response = await httpClient.GetAsync("http://example.com");
    // Rest of function
}
```

Een algemene vraag over de .NET `HttpClient` is 'Moet ik worden verwijdering mijn client?' In het algemeen, het verwijderen van objecten die implementeren `IDisposable` wanneer u klaar bent met deze. Maar u een statische client niet verwijderen omdat u niet worden uitgevoerd met behulp van deze wanneer de functie wordt beëindigd. U wilt de statische client live voor de duur van uw toepassing.

## <a name="documentclient-code-example"></a>DocumentClient codevoorbeeld

`DocumentClient` maakt verbinding met een Cosmos-DB-exemplaar. De documentatie van de Cosmos-DB adviseert u [een singleton Azure DB die Cosmos-client gebruiken voor de levensduur van uw toepassing](https://docs.microsoft.com/en-us/azure/cosmos-db/performance-tips#sdk-usage). Het volgende voorbeeld ziet een patroon voor het uitvoeren van die in een functie.

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

Voor meer informatie over de reden waarom statische clients worden aanbevolen, Zie [onjuiste instantiëring antipattern](https://docs.microsoft.com/azure/architecture/antipatterns/improper-instantiation/).

Zie voor meer tips voor Azure Functions-prestaties, [optimaliseren van de prestaties en betrouwbaarheid van Azure Functions](functions-best-practices.md).