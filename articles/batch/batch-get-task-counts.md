---
title: Voortgang van een taak door de taken op status - Azure Batch worden geteld | Microsoft Docs
description: De voortgang van een taak door het aanroepen van de bewerking taak telt ophalen om te tellen van taken voor een job. U krijgt een telling van actieve, uitgevoerd en voltooide taken en door de taken die u hebt is geslaagd of mislukt.
services: batch
author: tamram
manager: timlt
ms.service: batch
ms.topic: article
ms.date: 08/02/2017
ms.author: tamram
ms.openlocfilehash: ceff59d7063b60a1344a47489d3d73e0e8ee07df
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="count-tasks-by-state-to-monitor-a-jobs-progress-preview"></a>Aantal taken per status voor het bewaken van de voortgang van een taak (Preview)

Azure Batch biedt een efficiënte manier om de voortgang van een taak als het bijbehorende taken wordt uitgevoerd. U kunt aanroepen de [ophalen taak telt] [ rest_get_task_counts] bewerking om erachter te komen hoeveel taken in een status actief, actief of voltooid zijn en hoeveel hebt is geslaagd of mislukt. Door telt het aantal taken in elke staat, kunt u gemakkelijker voortgang van de taak weergeven voor een gebruiker of onverwachte vertragingen of fouten die mogelijk van invloed zijn op de taak niet detecteren.

> [!IMPORTANT]
> De taak telt ophalen-bewerking is momenteel in preview en is nog niet beschikbaar in Azure Government, Azure China en Duitse Azure. 
>
>

## <a name="how-tasks-are-counted"></a>Hoe taken worden geteld

De bewerking ophalen taak telt telt taken op status, als volgt:

- Een taak wordt beschouwd als **active** wanneer deze is in de wachtrij en kunnen uitvoeren, maar momenteel niet aan een rekenknooppunt is toegewezen. Een taak ook wordt beschouwd als **active** als dit is afhankelijk van een bovenliggende taak die nog niet is voltooid. Zie voor meer informatie over taakafhankelijkheden [Maak taakafhankelijkheden taken die afhankelijk van andere taken zijn uitvoeren](batch-task-dependencies.md). 
- Een taak wordt beschouwd als **met** wanneer deze is toegewezen aan een rekenknooppunt, maar is nog niet voltooid. Een taak wordt beschouwd als **met** wanneer de status is `preparing` of `running`, zoals aangegeven door de [informatie ophalen over een taak] [ rest_get_task] bewerking.
- Een taak wordt beschouwd als **voltooid** wanneer het is niet meer kunnen worden uitgevoerd. Een taak geteld als **voltooid** heeft meestal is voltooid, of succes is voltooid en heeft de limiet voor opnieuw proberen ook uitgeput. 

De bewerking ophalen taak telt rapporten ook hoeveel taken hebt voltooid of mislukt. Batch bepaalt of een taak is geslaagd of door het controleren van mislukt de **resultaat** eigenschap van de eigenschap [executionInfo] [https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task#executionInfo]:

    - Een taak wordt beschouwd als **geslaagd** als het resultaat van uitvoering van de taak is `success`.
    - Een taak wordt beschouwd als **mislukt** als het resultaat van uitvoering van de taak is `failure`.

Zie voor meer informatie over taakstatuswaarden [informatie ophalen over een taak][rest_get_task].

Het volgende .NET-codevoorbeeld ziet u hoe taak aantallen per status ophalen: 

```csharp
var taskCounts = await batchClient.JobOperations.GetTaskCountsAsync("job-1");

Console.WriteLine("Task count in active state: {0}", taskCounts.Active);
Console.WriteLine("Task count in preparing or running state: {0}", taskCounts.Running);
Console.WriteLine("Task count in completed state: {0}", taskCounts.Completed);
Console.WriteLine("Succeeded task count: {0}", taskCounts.Succeeded);
Console.WriteLine("Failed task count: {0}", taskCounts.Failed);
Console.WriteLine("ValidationStatus: {0}", taskCounts.ValidationStatus);
```

> [!NOTE]
> U kunt een vergelijkbaar patroon voor REST en andere ondersteunde talen taak tellingen voor een taak ophalen. 
> 
> 

## <a name="consistency-checking-for-task-counts"></a>Consistentiecontroles op taak aantallen

De Batch-service aggregeert taak aantallen door het verzamelen van gegevens uit meerdere delen van een asynchrone gedistribueerde systeem. Gecontroleerd om ervoor te zorgen dat de taak aantallen juist zijn, Batch biedt een extra validatie voor status aantallen door het uitvoeren van consistentiecontrole op basis van meerdere onderdelen van het systeem. Batch voert deze consistentiecontroles, zolang er minder dan 200.000 taken in de taak zijn. In het onwaarschijnlijke geval dat de consistentiecontrole zoeken naar fouten, corrigeert Batch het resultaat van de bewerking taken telt ophalen op basis van de resultaten van de consistentiecontrole. De consistentiecontrole is een extra beveiligingsmaatregel om ervoor te zorgen dat klanten die afhankelijk van de bewerking ophalen taak telt zijn de juiste informatie die ze nodig hebben om hun oplossing krijgen.

De **validationStatus** eigenschap in het antwoord geeft aan of Batch de consistentiecontrole is uitgevoerd. Als Batch is niet controleren van de status in mindering gebracht op de werkelijke statussen ondergebracht in het systeem, wordt de **validationStatus** eigenschap is ingesteld op `unvalidated`. Uit prestatieoverwegingen Batch wordt geen consistentiecontrole uitvoeren als de taak meer dan 200.000 taken bevat, zodat de **validationStatus** eigenschap kan worden ingesteld op `unvalidated` in dit geval. Het aantal taken is echter niet per se verkeerde in dit geval zelfs zeer beperkt gegevensverlies is zeer onwaarschijnlijk. 

Wanneer een taak de status verandert, verwerkt de pijplijn aggregatie van de wijziging binnen enkele seconden. De bewerking ophalen taak telt reflecteert het aantal bijgewerkte taak binnen die periode. Echter, als de pijplijn aggregatie Cachemissers een wijziging in de taakstatus van een, vervolgens die wijziging is niet geregistreerd totdat de volgende validatie op te geven. Gedurende deze tijd taak tellingen kunnen onnauwkeurig enigszins vanwege de gemiste gebeurtenis, maar ze zijn gecorrigeerd in de volgende stap van de validatie.

## <a name="best-practices-for-counting-a-jobs-tasks"></a>Aanbevolen procedures voor een job taken worden geteld

Aanroepen van de taak telt ophalen-bewerking is de meest efficiënte manier om terug te keren basic aantal taken van een taak op status. Als u een versie van de service Batch 2017-06-01.5.1 gebruikt, raden wij schrijven of bijwerken van uw code voor het gebruik van de taak telt ophalen.

De bewerking taak telt ophalen is niet beschikbaar in Batch-versies eerder dan 2017-06-01.5.1. Als u van een oudere versie van de service gebruikmaakt, gebruikt u een lijst met query voor het tellen van taken in een job in plaats daarvan. Zie voor meer informatie [query's maken voor een lijst met Batch-resources efficiënt](batch-efficient-list-queries.md).

## <a name="next-steps"></a>Volgende stappen

* Zie [Overzicht van Batch-functies](batch-api-basics.md) voor meer informatie over de concepten en functies van de Batch-service. Het artikel worden de primaire Batch-resources zoals pools, rekenknooppunten, jobs en taken besproken en biedt een overzicht van de functies van de service.
* Lees de basisbeginselen van het ontwikkelen van een voor Batch geschikte toepassing met behulp van de [clientbibliotheek Batch .NET](batch-dotnet-get-started.md) of [Python](batch-python-tutorial.md). Deze inleidende artikelen helpen u bij een werkende toepassing die gebruikmaakt van de Batch-service voor het uitvoeren van een workload op meerdere rekenknooppunten.


[rest_get_task_counts]: https://docs.microsoft.com/rest/api/batchservice/get-the-task-counts-for-a-job
[rest_get_task]: https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task
[rest_list_tasks]: https://docs.microsoft.com/rest/api/batchservice/list-the-tasks-associated-with-a-job
