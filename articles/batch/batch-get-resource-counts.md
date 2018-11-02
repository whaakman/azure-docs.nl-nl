---
title: Aantal statussen voor taken en knooppunten - Azure Batch | Microsoft Docs
description: Aantal van de status van Azure Batch-taken en rekenknooppunten om te beheren en controleren van de Batch-oplossingen.
services: batch
author: dlepow
manager: jeconnoc
ms.service: batch
ms.topic: article
ms.date: 09/07/2018
ms.author: danlep
ms.openlocfilehash: 5c65ee3ecc851b289c32a480ecad8f2df3aaad95
ms.sourcegitcommit: 799a4da85cf0fec54403688e88a934e6ad149001
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2018
ms.locfileid: "50913883"
---
# <a name="monitor-batch-solutions-by-counting-tasks-and-nodes-by-state"></a>Batch-oplossingen controleren door te tellen van taken en knooppunten per staat

Als u wilt bewaken en beheren van grootschalige Azure Batch-oplossingen, moet u nauwkeurige tellingen van resources in verschillende statussen. Azure Batch biedt een efficiënte werking voor deze telt voor Batch *taken* en *rekenknooppunten*. Gebruik deze bewerkingen in plaats van potentieel tijdrovende lijstquery's die gedetailleerde gegevens over grote verzamelingen van taken of knooppunten retourneren.

* [Ophalen van de taak wordt geteld] [ rest_get_task_counts] opgehaald van een statistische count van actieve, uitgevoerd en voltooide taken in een job en taken die geslaagd of mislukt. 

  Door de taken in elke status worden geteld, kunt u eenvoudiger taakvoortgang weergeven voor een gebruiker of onverwachte vertragingen of fouten die mogelijk van invloed zijn op de taak niet detecteren. Get-taak telt is beschikbaar vanaf de API voor Batch-Service versie 2017-06-01.5.1 en verwante SDK's en hulpprogramma's.

* [Lijst met toepassingen knooppunt telt] [ rest_get_node_counts] haalt het aantal toegewezen en met lage prioriteit rekenknooppunten in elke groep die zich in verschillende statussen: maken, niet-actieve, offline, verschoven, opnieuw opstarten, installatiekopie terugzetten, starten en anderen. 

  Door het tellen van knooppunten in elke status, kunt u bepalen wanneer u hebt voldoende rekenresources uw taken uit te voeren en identificeren van mogelijke problemen met uw pools. Lijst met toepassingen knooppunt telt is beschikbaar vanaf de API voor Batch-Service versie 2018-03-01.6.1 en verwante SDK's en hulpprogramma's.

Als u een versie van de service biedt geen ondersteuning voor de taak count of knooppunt aantal bewerkingen, gebruikt u een lijst met query voor het tellen van deze resources. Een lijst met query's ook gebruiken voor informatie over andere Batch-resources, zoals toepassingen, bestanden en taken. Zie voor meer informatie over het toepassen van filters op de lijst met query's, [query's maken om weer te geven Batch-resources efficiënt](batch-efficient-list-queries.md).

## <a name="task-state-counts"></a>Taakstatus wordt geteld

De bewerking ophalen voor de taak wordt geteld telt taken door de volgende statussen:

- **Actieve** -een taak die wordt uitgevoerd in de wachtrij en kan, maar momenteel niet naar een rekenknooppunt is toegewezen. Een taak is ook `active` als het [afhankelijk zijn van een bovenliggende taak](batch-task-dependencies.md) die nog niet is voltooid. 
- **Met** -een taak die is toegewezen aan een compute-knooppunt, maar nog niet is voltooid. Een taak wordt geteld als `running` wanneer de status is `preparing` of `running`, zoals aangegeven door de [informatie ophalen over een taak] [ rest_get_task] bewerking.
- **Voltooid** -een taak die niet langer in aanmerking komende uit te voeren, omdat het ofwel voltooid is, of zonder succes is voltooid en ook de limiet voor opnieuw proberen bereikt. 
- **Geslaagd** -een taak waarvan u het resultaat van uitvoering van de taak is `success`. Batch wordt bepaald of een taak is geslaagd of door het controleren van mislukt de `TaskExecutionResult` eigenschap van de [executionInfo] [ rest_get_exec_info] eigenschap.
- **Kan geen** een taak waarvan u het resultaat van uitvoering van de taak is `failure`.

De volgende .NET-codevoorbeeld laat zien hoe taak aantallen per status ophalen: 

```csharp
var taskCounts = await batchClient.JobOperations.GetJobTaskCountsAsync("job-1");

Console.WriteLine("Task count in active state: {0}", taskCounts.Active);
Console.WriteLine("Task count in preparing or running state: {0}", taskCounts.Running);
Console.WriteLine("Task count in completed state: {0}", taskCounts.Completed);
Console.WriteLine("Succeeded task count: {0}", taskCounts.Succeeded);
Console.WriteLine("Failed task count: {0}", taskCounts.Failed);
```

U kunt een soortgelijk patroon voor REST en andere ondersteunde talen aantallen van de taak voor een taak ophalen. 

> [!NOTE]
> Batch-Service-API-versies voordat 2018-08-01.7.0 ook retourneren een `validationStatus` eigenschap in het antwoord ophalen voor de taak wordt geteld. Deze eigenschap geeft aan of Batch de status van de aantallen voor consistentie met de statussen gerapporteerd in de lijst met taken API ingeschakeld. Een waarde van `validated` alleen geeft aan dat de Batch gecontroleerd op consistentie ten minste één keer voor de taak. De waarde van de `validationStatus` eigenschap wordt niet aangegeven of de aantallen die u voor de taak wordt geteld als resultaat geeft op dat moment up-to-date zijn.
>

## <a name="node-state-counts"></a>Status van knooppunt wordt geteld

De bewerking lijst Pool knooppunt telt aantallen compute-knooppunten op basis van de volgende statussen in elke groep. Afzonderlijke cumulatieve tellingen zijn beschikbaar voor toegewezen knooppunten en knooppunten met lage prioriteit in elke groep.

- **Het maken van** -een Azure-toegewezen virtuele machine die nog niet is gestart voor deelname aan een groep.
- **Inactieve** -een beschikbare compute-knooppunt dat een taak niet wordt uitgevoerd.
- **LeavingPool** -een knooppunt dat is verlaten van de groep, omdat de gebruiker expliciet hebt verwijderd of omdat de pool vergroten of verkleinen of automatisch schalen niet actief is.
- **Offline** -een knooppunt dat Batch niet gebruiken om nieuwe taken te plannen.
- **Verschoven** -een knooppunt met lage prioriteit die uit de pool is verwijderd, omdat Azure de virtuele machine vrijgemaakt. Een `preempted` knooppunt kan opnieuw worden geïnitialiseerd als vervanging met lage prioriteit VM-capaciteit beschikbaar is.
- **Opnieuw opstarten** -een knooppunt dat opnieuw wordt opgestart.
- **Installatiekopie terugzetten** -een knooppunt waarop het besturingssysteem opnieuw wordt geïnstalleerd.
- **Met** -een knooppunt dat een of meer taken (met uitzondering van de begintaak) wordt uitgevoerd.
- **Vanaf** -een knooppunt waarop de Batch-service wordt gestart. 
- **StartTaskFailed** -een knooppunt waarop de [begintaak] [ rest_start_task] is mislukt en alle nieuwe pogingen, verbruikt en waarvoor `waitForSuccess` is ingesteld op de begintaak opgegeven. Het knooppunt kan niet worden gebruikt voor het uitvoeren van taken.
- **Onbekende** : een knooppunt verloren die contact met de Batch-service en waarvan u de status is niet bekend.
- **Onbruikbaar** -een knooppunt dat voor de uitvoering van de taak kan niet worden gebruikt omdat er fouten zijn.
- **WaitingForStartTask** -een knooppunt waarop de begintaak is gestart, maar `waitForSuccess` is ingesteld en het begin taak is niet voltooid.

De volgende C#-codefragment laat zien hoe om knooppunt aantallen voor alle groepen in het huidige account weer te geven:

```csharp
foreach (var nodeCounts in batchClient.PoolOperations.ListPoolNodeCounts())
{
    Console.WriteLine("Pool Id: {0}", nodeCounts.PoolId);

    Console.WriteLine("Total dedicated node count: {0}", nodeCounts.Dedicated.Total);

    // Get dedicated node counts in Idle and Offline states; you can get additional states.
    Console.WriteLine("Dedicated node count in Idle state: {0}", nodeCounts.Dedicated.Idle);
    Console.WriteLine("Dedicated node count in Offline state: {0}", nodeCounts.Dedicated.Offline);

    Console.WriteLine("Total low priority node count: {0}", nodeCounts.LowPriority.Total);

    // Get low-priority node counts in Running and Preempted states; you can get additional states.
    Console.WriteLine("Low-priority node count in Running state: {0}", nodeCounts.LowPriority.Running);
    Console.WriteLine("Low-priority node count in Preempted state: {0}", nodeCounts.LowPriority.Preempted);
}
```
Het volgende C#-fragment toont hoe u knooppunt aantallen voor een bepaalde groep in het huidige account.

```csharp
foreach (var nodeCounts in batchClient.PoolOperations.ListPoolNodeCounts(new ODATADetailLevel(filterClause: "poolId eq 'testpool'")))
{
    Console.WriteLine("Pool Id: {0}", nodeCounts.PoolId);

    Console.WriteLine("Total dedicated node count: {0}", nodeCounts.Dedicated.Total);

    // Get dedicated node counts in Idle and Offline states; you can get additional states.
    Console.WriteLine("Dedicated node count in Idle state: {0}", nodeCounts.Dedicated.Idle);
    Console.WriteLine("Dedicated node count in Offline state: {0}", nodeCounts.Dedicated.Offline);

    Console.WriteLine("Total low priority node count: {0}", nodeCounts.LowPriority.Total);

    // Get low-priority node counts in Running and Preempted states; you can get additional states.
    Console.WriteLine("Low-priority node count in Running state: {0}", nodeCounts.LowPriority.Running);
    Console.WriteLine("Low-priority node count in Preempted state: {0}", nodeCounts.LowPriority.Preempted);
}
```
U kunt een soortgelijk patroon voor REST en andere ondersteunde talen gebruiken knooppunt telt voor groepen ophalen.
 
## <a name="next-steps"></a>Volgende stappen

* Zie [Overzicht van Batch-functies](batch-api-basics.md) voor meer informatie over de concepten en functies van de Batch-service. Het artikel worden de primaire Batch-resources zoals pools, rekenknooppunten, jobs en taken besproken en biedt een overzicht van de servicefuncties.

* Zie voor meer informatie over het toepassen van filters op query's met Batch-resources [query's maken om weer te geven Batch-resources efficiënt](batch-efficient-list-queries.md).


[rest_get_task_counts]: /rest/api/batchservice/job/gettaskcounts
[rest_get_node_counts]: /rest/api/batchservice/account/listpoolnodecounts
[rest_get_task]: /rest/api/batchservice/task/get
[rest_list_tasks]: /rest/api/batchservice/task/list
[rest_get_exec_info]: /rest/api/batchservice/task/get#executionInfo
[rest_start_task]: /rest/api/batchservice/pool/add#starttask

