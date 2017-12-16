---
title: Controlepunten en replayaanvallen in duurzame functies - Azure
description: Meer informatie over hoe het plaatsen van controlepunten en de antwoord-werkt in de uitbreiding duurzame functies voor Azure Functions.
services: functions
author: cgillum
manager: cfowler
editor: 
tags: 
keywords: 
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: b1bca62e256c1ede5df6888dd7c47ce2aa816bb9
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2017
---
# <a name="checkpoints-and-replay-in-durable-functions-azure-functions"></a>Controlepunten en replayaanvallen in duurzame functies (Azure-functies)

Een van de belangrijkste kenmerken van duurzame functies is **betrouwbare uitvoering**. Orchestrator-functies en functies van de activiteit wordt mogelijk uitgevoerd op andere virtuele machines binnen een datacenter en deze virtuele machines of de onderliggende netwerkinfrastructuur is niet 100% betrouwbaar.

Altijd een beroep dit garandeert duurzame functies betrouwbare uitvoering van integraties. Dit gebeurt met behulp van opslagwachtrijen voor station functie aanroepen en door regelmatig uitvoergeschiedenis van het plaatsen van controlepunten in opslagtabellen (met behulp van een cloud-ontwerppatroon bekend als [gebeurtenis bron](https://docs.microsoft.com/azure/architecture/patterns/event-sourcing)). Die geschiedenis kan vervolgens worden cookies voor het automatisch opnieuw opbouwen van de status in het geheugen van een orchestrator-functie.

## <a name="orchestration-history"></a>Orchestration-geschiedenis

Stel dat u hebt de volgende orchestrator-functie.

```csharp
[FunctionName("E1_HelloSequence")]
public static async Task<List<string>> Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    var outputs = new List<string>();

    outputs.Add(await context.CallActivityAsync<string>("E1_SayHello", "Tokyo"));
    outputs.Add(await context.CallActivityAsync<string>("E1_SayHello", "Seattle"));
    outputs.Add(await context.CallActivityAsync<string>("E1_SayHello", "London"));

    // returns ["Hello Tokyo!", "Hello Seattle!", "Hello London!"]
    return outputs;
}
```

Op elk `await` instructie, de controlepunten duurzame Framework van de taak de uitvoeringsstatus van de functie naar de tabelopslag. Deze status is wat wordt aangeduid als de *orchestration geschiedenis*.

## <a name="history-table"></a>Geschiedenistabel

Normaal gesproken doet de duurzame taak Framework het volgende in elk controlepunt:

1. Slaat de uitvoergeschiedenis in Azure Storage-tabellen.
2. Enqueues berichten voor functies in de orchestrator wil aanroepen.
3. Enqueues berichten voor de orchestrator zelf &mdash; bijvoorbeeld duurzame timer berichten.

Zodra het controlepunt voltooid is, is de orchestrator-functie moet worden verwijderd uit het geheugen tot er meer werk voor te doen is gratis.

> [!NOTE]
> Azure Storage biedt geen transactionele garanties tussen het opslaan van gegevens in de tabelopslag en wachtrijen. Voor het afhandelen van fouten gebruikmaakt van de opslagprovider duurzame functies *uiteindelijke consistentie* patronen. Deze patronen Zorg ervoor dat gegevens niet verloren als er een crash of verlies van verbinding in het midden van een controlepunt.

Na voltooiing, de geschiedenis van de eerder vermelde functie ziet er ongeveer als volgt in Azure Table Storage (afgekort ter illustratie):

| PartitionKey (InstanceId)                     | EventType             | Timestamp               | Invoer | Naam             | Resultaat                                                    | Status | 
|----------------------------------|-----------------------|----------|--------------------------|-------|------------------|-----------------------------------------------------------|---------------------| 
| eaee885b | OrchestratorStarted   | 2017-05-05T18:45:32.362Z |       |                  |                                                           |                     | 
| eaee885b | ExecutionStarted      | 2017-05-05T18:45:28.852Z | null  | E1_HelloSequence |                                                           |                     | 
| eaee885b | TaskScheduled         | 2017-05-05T18:45:32.670Z |       | E1_SayHello      |                                                           |                     | 
| eaee885b | OrchestratorCompleted | 2017-05-05T18:45:32.670Z |       |                  |                                                           |                     | 
| eaee885b | OrchestratorStarted   | 2017-05-05T18:45:34.232Z |       |                  |                                                           |                     | 
| eaee885b | TaskCompleted         | 2017-05-05T18:45:34.201Z |       |                  | "" 'Hello Tokio!"" "                                        |                     | 
| eaee885b | TaskScheduled         | 2017-05-05T18:45:34.435Z |       | E1_SayHello      |                                                           |                     | 
| eaee885b | OrchestratorCompleted | 2017-05-05T18:45:34.435Z |       |                  |                                                           |                     | 
| eaee885b | OrchestratorStarted   | 2017-05-05T18:45:34.857Z |       |                  |                                                           |                     | 
| eaee885b | TaskCompleted         | 2017-05-05T18:45:34.763Z |       |                  | "" 'Hello Seattle!"" "                                      |                     | 
| eaee885b | TaskScheduled         | 2017-05-05T18:45:34.857Z |       | E1_SayHello      |                                                           |                     | 
| eaee885b | OrchestratorCompleted | 2017-05-05T18:45:34.857Z |       |                  |                                                           |                     | 
| eaee885b | OrchestratorStarted   | 2017-05-05T18:45:35.032Z |       |                  |                                                           |                     | 
| eaee885b | TaskCompleted         | 2017-05-05T18:45:34.919Z |       |                  | "" 'Hello Londen!"" "                                       |                     | 
| eaee885b | ExecutionCompleted    | 2017-05-05T18:45:35.044Z |       |                  | "[""Hello Tokio!" ",""Hello Seattle!" ', "' Hello Londen!" "]" | Voltooid           | 
| eaee885b | OrchestratorCompleted | 2017-05-05T18:45:35.044Z |       |                  |                                                           |                     | 

Enkele opmerkingen bij de kolomwaarden:
* **PartitionKey**: bevat de exemplaar-ID van de orchestration.
* **EventType**: vertegenwoordigt het type van de gebeurtenis. Mogelijk een van de volgende typen:
    * **OrchestrationStarted**: de orchestrator-functie hervat vanaf een await of voor het eerst wordt uitgevoerd. De `Timestamp` kolom wordt gebruikt voor het vullen van de deterministische waarde voor de [CurrentUtcDateTime](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CurrentUtcDateTime) API.
    * **ExecutionStarted**: de orchestrator-functie gestart voor de eerste keer wordt uitgevoerd. Deze gebeurtenis bevat ook de functie invoer in de `Input` kolom.
    * **TaskScheduled**: de functie van een activiteit is gepland. De naam van de functie van de activiteit is opgenomen de `Name` kolom.
    * **TaskCompleted**: een functie activiteit is voltooid. Het resultaat van de functie is in de `Result` kolom.
    * **TimerCreated**: een duurzame timer is gemaakt. De `FireAt` kolom bevat de geplande UTC-tijd waarop de timer verloopt.
    * **TimerFired**: een duurzame timer gestart.
    * **EventRaised**: een externe gebeurtenis is verzonden naar de orchestration-exemplaar. De `Name` kolom de naam van de gebeurtenis worden vastgelegd en de `Input` kolom de nettolading van de gebeurtenis worden vastgelegd.
    * **OrchestratorCompleted**: de orchestrator-functie wordt afgewacht.
    * **ContinueAsNew**: de orchestrator-functie is voltooid en zelf opnieuw gestart met de nieuwe status. De `Result` kolom bevat de waarde die wordt gebruikt als de invoer in het exemplaar opnieuw gestart.
    * **ExecutionCompleted**: de orchestrator-functie voor voltooiing worden uitgevoerd (of mislukt). De uitvoer van de functie of de foutgegevens worden opgeslagen in de `Result` kolom.
* **Tijdstempel**: de UTC-timestamp van de geschiedenisgebeurtenis.
* **Naam**: de naam van de functie die is aangeroepen.
* **Invoer**: invoer van de functie de JSON-indeling.
* **Resultaat**: de uitvoer van de functie; dat wil zeggen, de retourwaarde.

> [!WARNING]
> Het is nuttig zijn als een foutopsporingsprogramma, geen eventuele afhankelijkheden onderneemt in deze tabel. Deze mogelijk wijzigen, omdat de extensie duurzame functies zich verder ontwikkelen.

Telkens wanneer de functie wordt hervat vanuit een `await`, het duurzame taak Framework de orchestrator-functie helemaal opnieuw worden uitgevoerd. Op elke opnieuw uitvoeren die wordt de uitvoergeschiedenis om te bepalen of de huidige asynchrone bewerking heeft gehouden raadpleegt plaatsen.  Als de bewerking plaatsgevonden heeft, het framework de uitvoer van die bewerking onmiddellijk opnieuw weergegeven en gaat verder met de volgende `await`. Dit proces wordt vervolgd totdat de hele geschiedenis is cookies, waarna alle lokale variabelen in de orchestrator-functie worden teruggezet op hun vorige waarden.

## <a name="orchestrator-code-constraints"></a>Beperkingen van orchestrator-code

Het gedrag voor opnieuw afspelen maakt beperkingen voor het type code dat kan worden geschreven in een orchestrator-functie:

* Orchestrator-code moet **deterministische**. Het wordt meerdere keren worden cookies en hetzelfde resultaat telkens moet produceren. Er zijn geen directe wordt bijvoorbeeld ophalen van de huidige datum en tijd, ophalen van willekeurige getallen, genereren van willekeurige GUID's of externe eindpunten aanroepen.

  Als er moet een orchestrator-code ophalen van de huidige datum en tijd, gebruiken deze de [CurrentUtcDateTime](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CurrentUtcDateTime) API voor opnieuw afspelen veilig is.

  Niet-deterministisch operations moeten worden uitgevoerd in de functies van de activiteit. Dit omvat interactie met andere invoer- of -bindingen. Dit zorgt ervoor dat een niet-deterministische waarden worden gegenereerd eenmaal op de eerste uitvoering en in de geschiedenis van de uitvoering opgeslagen. Bij een volgende uitvoering wordt automatisch de opgeslagen waarde gebruikt.

* Orchestrator-code moet **niet-blokkerende**. Dat betekent bijvoorbeeld geen i/o- en geen aanroepen naar `Thread.Sleep` of gelijkwaardige API's.

  Als een orchestrator uitstellen moet, kunt gebruiken de [CreateTimer](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CreateTimer_) API.

* Orchestrator-code moet **nooit gestart een asynchrone bewerking** behalve met behulp van de [DurableOrchestrationContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html) API. Bijvoorbeeld: Er is geen `Task.Run`, `Task.Delay` of `HttpClient.SendAsync`. Het duurzame taak Framework orchestrator-code op een enkele thread wordt uitgevoerd en kan niet communiceren met andere threads die door andere async-API's kunnen worden gepland.

* **Oneindige lus moeten worden vermeden** in de orchestrator-code. Omdat het duurzame taak Framework wordt opgeslagen uitvoergeschiedenis van de voortgang van de orchestration-functie, kan een oneindige lus ervoor zorgen dat een orchestrator-exemplaar onvoldoende geheugen. Gebruik voor scenario's met oneindige lus API's, zoals [ContinueAsNew](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_ContinueAsNew_) opnieuw opstarten van de functie wordt uitgevoerd en uitvoergeschiedenis van de vorige negeren.

Terwijl deze beperkingen op eerst in de praktijk die ze niet moeilijk afschrikken kunnen zijn te volgen. Het duurzame taak Framework probeert te detecteren schendingen van de bovenstaande regels en genereert een `NonDeterministicOrchestrationException`. Dit gedrag detectie is echter best effort, en u dient niet afhankelijk zijn.

> [!NOTE]
> Deze regels alleen van toepassing op functies die zijn geactiveerd door de `orchestrationTrigger` binding. Functies van de activiteit wordt geactiveerd door de `activityTrigger` binding en functies die gebruikmaken van de `orchestrationClient` geen beperkingen voor deze binding, hebben.

## <a name="durable-tasks"></a>Duurzame taken

> [!NOTE]
> Deze sectie beschrijft de interne implementatiegegevens van duurzame taak Framework. U kunt duurzame functies zonder te weten van deze informatie gebruiken. Dit is uitsluitend bedoeld om te begrijpen van het gedrag voor opnieuw afspelen.

Taken die veilig kunnen worden afgewacht in orchestrator-functies worden soms aangeduid als *duurzame taken*. Dit zijn taken die worden gemaakt en beheerd door de duurzame taak Framework. Voorbeelden zijn de taken die zijn geretourneerd door `CallActivityAsync`, `WaitForExternalEvent`, en `CreateTimer`.

Deze *duurzame taken* intern worden beheerd met behulp van een lijst met `TaskCompletionSource` objecten. Tijdens het opnieuw afspelen, deze taken worden gemaakt als onderdeel van de uitvoering van de orchestrator-code en zijn voltooid, omdat de dispatcher de overeenkomende geschiedenisgebeurtenissen inventariseert. Dit is alles synchroon één thread gebruiken totdat de geschiedenis is replay gedaan. Een duurzame taken, die niet zijn voltooid door het einde van de geschiedenis replay heeft juiste acties worden uitgevoerd. Zo mogelijk een bericht in de wachtrij om aan te roepen, een functie van de activiteit.

De hier beschreven uitvoeringsgedrag moet u helpen begrijpen waarom orchestrator functiecode moet nooit `await` een taak niet duurzame: de dispatcher-thread niet kan wachten deze is voltooid en een retouraanroep door deze taak kan de tracering beschadigen de status van de orchestrator-functie. Enkele runtime-controles zijn geïmplementeerd om te proberen om dit te voorkomen.

Als u meer informatie over hoe het duurzame taak Framework orchestrator-functies uitvoert, het beste doen te raadplegen is de [duurzame taak broncode op GitHub](https://github.com/Azure/durabletask). In het bijzonder Zie [TaskOrchestrationExecutor.cs](https://github.com/Azure/durabletask/blob/master/src/DurableTask.Core/TaskOrchestrationExecutor.cs) en [TaskOrchestrationContext.cs](https://github.com/Azure/durabletask/blob/master/src/DurableTask.Core/TaskOrchestrationContext.cs)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over instantiebeheer](durable-functions-instance-management.md)
