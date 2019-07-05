---
title: Status van de indexeerfunctie en resultaten - Azure Search controleren
description: Controleer de status, voortgang en resultaten van Azure Search-indexeerfuncties in Azure portal, met behulp van de REST-API of de .NET SDK.
ms.date: 06/28/2019
author: RobDixon22
manager: HeidiSteen
ms.author: v-rodixo
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.custom: seodec2018
ms.openlocfilehash: 07b4fe2ef830c3ce09b655cf4b433d14923229a9
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67486283"
---
# <a name="how-to-monitor-azure-search-indexer-status-and-results"></a>Status van de indexeerfunctie Azure Search en de resultaten controleren

Azure Search biedt status- en informatie over de huidige en historische uitvoeringen van elke indexeerfunctie bewaking.

Bewaking van de indexeerfunctie is handig als u wilt:

* Spoor de voortgang van een indexeerfunctie tijdens een doorlopend worden uitgevoerd.
* Bekijk de resultaten van doorlopende of vorige uitvoerbewerking van de indexeerfunctie.
* Op het hoogste niveau indexeerfunctie identificeren fouten, en fouten of waarschuwingen over afzonderlijke documenten worden geïndexeerd.

## <a name="find-indexer-status-and-history-details"></a>Indexeerfunctie status en geschiedenis van meer informatie te vinden

U kunt toegang tot bewakingsgegevens op verschillende manieren, met inbegrip van indexeerfunctie:

* In [Azure Portal](#portal)
* Met behulp van de [REST-API](#restapi)
* Met behulp van de [.NET SDK](#dotnetsdk)

Beschikbare indexeerfunctie controlegegevens bevat alle volgende (Hoewel de gegevens indelingen verschillen op basis van de access-methode gebruikt):

* Statusinformatie over de indexeerfunctie zelf
* Informatie over de meest recente uitvoering van de indexeerfunctie, met inbegrip van de status, begin en einde keren, en gedetailleerde fouten en waarschuwingen.
* Een lijst met historische indexeerfunctie wordt uitgevoerd, en hun status, resultaten, fouten en waarschuwingen.

Indexeerfuncties die grote hoeveelheden gegevens verwerken kunnen erg lang duren om uit te voeren. Indexeerfuncties die verwerking van miljoenen brondocumenten kunnen bijvoorbeeld 24 uur uitgevoerd en vervolgens bijna onmiddellijk opnieuw opgestart. De status van hoge volumes indexeerfuncties altijd mogelijk staat **In voortgang** in de portal. Zelfs als een indexeerfunctie wordt uitgevoerd, wordt informatie over de voortgang van de actieve en eerdere uitvoeringen vindt.

<a name="portal"></a>

## <a name="monitor-indexers-in-the-portal"></a>Monitor indexeerfuncties in de portal

U ziet de huidige status van al uw indexeerfuncties in de **indexeerfuncties** lijst op de pagina overzicht van uw zoekservice.

   ![Lijst met indexeerfuncties](media/search-monitor-indexers/indexers-list.png "lijst met indexeerfuncties")

Wanneer een indexeerfunctie wordt uitgevoerd, de status in de lijst wordt aangegeven **In voortgang**, en de **Docs is geslaagd** waarde geeft het aantal documenten dat tot nu toe is verwerkt. Duurt het enkele minuten duren voordat de portal voor het bijwerken van de indexeerfunctie statuswaarden en document telt.

Een indexeerfunctie waarvan de meest recente uitvoering geslaagd toont is **succes**. Uitvoerbewerking van de indexeerfunctie kan worden uitgevoerd, zelfs als afzonderlijke documenten fouten bevatten, als het aantal fouten kleiner dan van de indexeerfunctie is **maximaal aantal mislukt items** instelling.

Als de meest recente uitvoering is beëindigd met een fout, ziet u de status **mislukt**. De status van **opnieuw** betekent dat staat voor bijhouden van de indexeerfunctie is opnieuw ingesteld.

Klik op een indexeerfunctie in de lijst voor meer informatie over van de indexeerfunctie huidige en recente wordt uitgevoerd.

   ![De geschiedenis van de samenvatting en uitvoering van de indexeerfunctie](media/search-monitor-indexers/indexer-summary.png "de geschiedenis van de samenvatting en uitvoering van de indexeerfunctie")

De **indexeerfunctie samenvatting** grafiek geeft een grafiek van het aantal documenten in de meest recente uitvoerbewerkingen verwerkt.

De **uitvoeringsdetails** lijst bevat maximaal 50 van de meest recente uitvoering resultaten.

Klik op een resultaat kan worden uitgevoerd in de lijst om te zien van specifieke informatie over die worden uitgevoerd. Dit omvat de start en eindtijd ervan, en eventuele fouten en waarschuwingen die zijn opgetreden.

   ![Details van uitvoering van indexeerfunctie](media/search-monitor-indexers/indexer-execution.png "details van uitvoering van indexeerfunctie")

Als er document-specifieke problemen tijdens de uitvoering waren, wordt deze weergegeven in de velden fouten en waarschuwingen.

   ![Details van de indexeerfunctie met fouten](media/search-monitor-indexers/indexer-execution-error.png "details van de indexeerfunctie met fouten")

Waarschuwingen met bepaalde typen indexeerfuncties gemeenschappelijk zijn en niet altijd duidt op een probleem. Indexeerfuncties die gebruikmaken van cognitive services kunnen bijvoorbeeld waarschuwingen rapporteren wanneer de installatiekopie of een PDF-bestanden bevatten geen tekst om te verwerken.

Zie voor meer informatie over het onderzoeken van indexeerfunctie fouten en waarschuwingen [het oplossen van veelvoorkomende problemen met indexeerfunctie in Azure Search](search-indexer-troubleshooting.md).

<a name="restapi"></a>

## <a name="monitor-indexers-using-the-rest-api"></a>Bewaken met behulp van de REST-API indexeerfuncties

U kunt de geschiedenis van status en de uitvoering van het gebruik van een indexeerfunctie ophalen de [indexeerfunctie Status ophalen-opdracht](https://docs.microsoft.com/rest/api/searchservice/get-indexer-status):

    GET https://[service name].search.windows.net/indexers/[indexer name]/status?api-version=2019-05-06
    api-key: [Search service admin key]

Het antwoord bevat de algemene status van de indexeerfunctie, het laatste (of in uitvoering) indexer-aanroepen en de geschiedenis van recente indexeerfunctie aanroepen.

    {
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2018-11-26T03:37:18.853Z",
            "endTime":"2018-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
         },
        "executionHistory":[ {
            "status":"success",
             "errorMessage":null,
            "startTime":"2018-11-26T03:37:18.853Z",
            "endTime":"2018-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
        }]
    }

Uitvoeringsgeschiedenis bevat maximaal de 50 meest recente uitgevoerd die zijn gesorteerd in omgekeerde volgorde (meest recente eerst).

Let op: Er zijn twee verschillende statussen. De status op het hoogste niveau is voor de indexeerfunctie zelf. De status van een indexeerfunctie van **met** betekent dat de indexeerfunctie juist is ingesteld en worden uitgevoerd, maar niet dat deze de momenteel uitgevoerd.

Elke uitvoering van de indexeerfunctie heeft ook een eigen status die aangeeft of deze specifieke uitvoering lopende (**met**), of al is voltooid met een **succes**, **transientFailure**, of **persistentFailure** status. 

Wanneer een indexeerfunctie opnieuw wordt ingesteld om te vernieuwen van de status van de bijhouden, een uitvoering van afzonderlijke vermelding wordt toegevoegd met een **opnieuw** status.

Zie voor meer informatie over statuscodes en indexeerfunctie bewakingsgegevens [GetIndexerStatus](https://docs.microsoft.com/rest/api/searchservice/get-indexer-status).

<a name="dotnetsdk"></a>

## <a name="monitor-indexers-using-the-net-sdk"></a>Bewaken met behulp van de .NET SDK indexeerfuncties

U kunt het schema voor een indexeerfunctie met behulp van de Azure Search .NET SDK definiëren. Om dit te doen, bevatten de **planning** eigenschap bij het maken of bijwerken van een indexeerfunctie.

De volgende C# voorbeeld schrijft u informatie over de status van een indexeerfunctie en de resultaten van de meest recente (of lopende) worden uitgevoerd in de console.

```csharp
static void CheckIndexerStatus(Indexer indexer, SearchServiceClient searchService)
{
    try
    {
        IndexerExecutionInfo execInfo = searchService.Indexers.GetStatus(indexer.Name);

        Console.WriteLine("Indexer has run {0} times.", execInfo.ExecutionHistory.Count);
        Console.WriteLine("Indexer Status: " + execInfo.Status.ToString());

        IndexerExecutionResult result = execInfo.LastResult;

        Console.WriteLine("Latest run");
        Console.WriteLine("  Run Status: {0}", result.Status.ToString());
        Console.WriteLine("  Total Documents: {0}, Failed: {1}", result.ItemCount, result.FailedItemCount);

        TimeSpan elapsed = result.EndTime.Value - result.StartTime.Value;
        Console.WriteLine("  StartTime: {0:T}, EndTime: {1:T}, Elapsed: {2:t}", result.StartTime.Value, result.EndTime.Value, elapsed);

        string errorMsg = (result.ErrorMessage == null) ? "none" : result.ErrorMessage;
        Console.WriteLine("  ErrorMessage: {0}", errorMsg);
        Console.WriteLine("  Document Errors: {0}, Warnings: {1}\n", result.Errors.Count, result.Warnings.Count);
    }
    catch (Exception e)
    {
        // Handle exception
    }
}
```

De uitvoer in de console ziet er ongeveer als volgt:

    Indexer has run 18 times.
    Indexer Status: Running
    Latest run
      Run Status: Success
      Total Documents: 7, Failed: 0
      StartTime: 10:02:46 PM, EndTime: 10:02:47 PM, Elapsed: 00:00:01.0990000
      ErrorMessage: none
      Document Errors: 0, Warnings: 0

Let op: Er zijn twee verschillende statussen. De status van de op het hoogste niveau is de status van de indexeerfunctie zelf. De status van een indexeerfunctie van **met** betekent dat de indexeerfunctie juist is ingesteld en beschikbaar voor uitvoering, maar niet dat deze wordt momenteel uitgevoerd.

Elke uitvoering van de indexeerfunctie heeft ook een eigen status voor of die specifieke uitvoering momenteel wordt (**met**), of al is voltooid met een **succes** of **TransientError** de status. 

Wanneer een indexeerfunctie opnieuw wordt ingesteld om te vernieuwen van de status van de bijhouden, een afzonderlijke vermelding wordt toegevoegd met een **opnieuw** status.

Zie voor meer informatie over statuscodes en indexeerfunctie controlegegevens [GetIndexerStatus](https://docs.microsoft.com/rest/api/searchservice/get-indexer-status) in de REST-API.

Meer informatie over de document-specifieke fouten of waarschuwingen kunnen worden opgehaald met het inventariseren van de lijsten `IndexerExecutionResult.Errors` en `IndexerExecutionResult.Warnings`.

Zie voor meer informatie over de .NET SDK-klassen worden gebruikt voor het bewaken van indexeerfuncties [IndexerExecutionInfo](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexerexecutioninfo?view=azure-dotnet) en [IndexerExecutionResult](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexerexecutionresult?view=azure-dotnet).
