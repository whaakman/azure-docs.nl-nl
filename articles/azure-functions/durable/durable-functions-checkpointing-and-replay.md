---
title: Controlepunten en herhaling in duurzame functies - Azure
description: Meer informatie over hoe het plaatsen van controlepunten en antwoord werkt in de extensie duurzame functies voor Azure Functions.
services: functions
author: kashimiz
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 10/23/2018
ms.author: azfuncdf
ms.openlocfilehash: ce930adc4cb2c635b54b3d41ea4a3ac272541698
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/30/2018
ms.locfileid: "52643164"
---
# <a name="checkpoints-and-replay-in-durable-functions-azure-functions"></a>Controlepunten en herhaling in duurzame functies (Azure Functions)

Een van de belangrijkste kenmerken van duurzame functies is **betrouwbare uitvoering**. Orchestrator-functies en activiteitsfuncties mogelijk op andere virtuele machines in een datacenter worden uitgevoerd en deze virtuele machines of de onderliggende netwerkinfrastructuur is niet 100% betrouwbaar.

Duurzame functies zorgt er, betrouwbare uitvoering van indelingen. Dit gebeurt met behulp van storage-wachtrijen op station functieaanroepen en door regelmatig uitvoeringsgeschiedenis voor het plaatsen van controlepunten in opslagtabellen (met behulp van een cloud-ontwerppatroon wel [' Event sourcing '](https://docs.microsoft.com/azure/architecture/patterns/event-sourcing)). Die geschiedenis kan vervolgens opnieuw om de status in het geheugen van een orchestrator-functie automatisch opnieuw te worden afgespeeld.

## <a name="orchestration-history"></a>Orchestration-geschiedenis

Stel dat u hebt de volgende orchestrator-functie:

#### <a name="c"></a>C#

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

#### <a name="javascript-functions-v2-only"></a>JavaScript (alleen functies v2)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const output = [];
    output.push(yield context.df.callActivity("E1_SayHello", "Tokyo"));
    output.push(yield context.df.callActivity("E1_SayHello", "Seattle"));
    output.push(yield context.df.callActivity("E1_SayHello", "London"));

    return output;
});
```

Op elke `await` (C#) of `yield` instructie (JavaScript), de controlepunten duurzame taak Framework de uitvoeringsstatus van de functie table-opslag. Deze status is wat wordt aangeduid als de *orchestration geschiedenis*.

## <a name="history-table"></a>Tabel met de geschiedenis

In het algemeen doet de duurzame taak Framework het volgende in bij elk controlepunt:

1. Slaat de uitvoeringsgeschiedenis in Azure Storage-tabellen.
2. Enqueues berichten voor de orchestrator wil aanroepen.
3. Enqueues berichten voor de orchestrator zelf &mdash; bijvoorbeeld timer voor duurzame berichten.

Zodra het controlepunt voltooid is, is de orchestrator-functie moet worden verwijderd uit het geheugen tot er meer werk te doen is gratis.

> [!NOTE]
> Azure Storage biedt geen garanties transactionele tussen het opslaan van gegevens in table storage en wachtrijen. Voor het afhandelen van fouten, gebruikmaakt van de opslagprovider duurzame functies *uiteindelijke consistentie* patronen. Deze patronen Zorg ervoor dat er geen gegevens verloren gaan als er een crash of verlies van connectiviteit in het midden van een controlepunt.

Na voltooiing de geschiedenis van de functie hierboven ziet er ongeveer als volgt in Azure Table-opslag (afgekort ter illustratie):

| PartitionKey (InstanceId)                     | Type gebeurtenis             | Tijdstempel               | Invoer | Naam             | Resultaat                                                    | Status | 
|----------------------------------|-----------------------|----------|--------------------------|-------|------------------|-----------------------------------------------------------|---------------------| 
| eaee885b | OrchestratorStarted   | 2017-05-05T18:45:32.362Z |       |                  |                                                           |                     | 
| eaee885b | ExecutionStarted      | 2017-05-05T18:45:28.852Z | Null  | E1_HelloSequence |                                                           |                     | 
| eaee885b | TaskScheduled         | 2017-05-05T18:45:32.670Z |       | E1_SayHello      |                                                           |                     | 
| eaee885b | OrchestratorCompleted | 2017-05-05T18:45:32.670Z |       |                  |                                                           |                     | 
| eaee885b | OrchestratorStarted   | 2017-05-05T18:45:34.232Z |       |                  |                                                           |                     | 
| eaee885b | TaskCompleted         | 2017-05-05T18:45:34.201Z |       |                  | "" "Hallo Tokio!" ""                                        |                     | 
| eaee885b | TaskScheduled         | 2017-05-05T18:45:34.435Z |       | E1_SayHello      |                                                           |                     | 
| eaee885b | OrchestratorCompleted | 2017-05-05T18:45:34.435Z |       |                  |                                                           |                     | 
| eaee885b | OrchestratorStarted   | 2017-05-05T18:45:34.857Z |       |                  |                                                           |                     | 
| eaee885b | TaskCompleted         | 2017-05-05T18:45:34.763Z |       |                  | "" "Hallo Seattle!" ""                                      |                     | 
| eaee885b | TaskScheduled         | 2017-05-05T18:45:34.857Z |       | E1_SayHello      |                                                           |                     | 
| eaee885b | OrchestratorCompleted | 2017-05-05T18:45:34.857Z |       |                  |                                                           |                     | 
| eaee885b | OrchestratorStarted   | 2017-05-05T18:45:35.032Z |       |                  |                                                           |                     | 
| eaee885b | TaskCompleted         | 2017-05-05T18:45:34.919Z |       |                  | "" "Hallo Londen!" ""                                       |                     | 
| eaee885b | ExecutionCompleted    | 2017-05-05T18:45:35.044Z |       |                  | ' ["" Hallo Tokio!"", "" Hallo Seattle!"", "" Londen Hallo!""] ' | Voltooid           | 
| eaee885b | OrchestratorCompleted | 2017-05-05T18:45:35.044Z |       |                  |                                                           |                     | 

Enkele opmerkingen bij de kolomwaarden:
* **PartitionKey**: bevat de exemplaar-ID van de indeling.
* **Type gebeurtenis**: Hiermee geeft u het type van de gebeurtenis. Mogelijk een van de volgende typen:
    * **OrchestrationStarted**: de orchestrator-functie hervat vanaf een await of voor de eerste keer wordt uitgevoerd. De `Timestamp` kolom wordt gebruikt voor het vullen van de deterministische waarde voor de [CurrentUtcDateTime](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CurrentUtcDateTime) API.
    * **ExecutionStarted**: de orchestrator-functie gestart voor de eerste keer wordt uitgevoerd. Deze gebeurtenis bevat ook de functie-invoer in de `Input` kolom.
    * **TaskScheduled**: de functie van een activiteit is gepland. De naam van de functie van de activiteit is vastgelegd in de `Name` kolom.
    * **TaskCompleted**: de functie van een activiteit is voltooid. Het resultaat van de functie wordt in de `Result` kolom.
    * **TimerCreated**: een duurzame timer is gemaakt. De `FireAt` kolom bevat de geplande UTC-tijd waarop de timer verloopt.
    * **TimerFired**: een duurzame timer gestart.
    * **EventRaised**: een externe gebeurtenis is verzonden naar de orchestration-exemplaar. De `Name` kolom bevat de naam van de gebeurtenis en de `Input` kolom bevat de nettolading van de gebeurtenis.
    * **OrchestratorCompleted**: de orchestrator-functie gestopt.
    * **ContinueAsNew**: de orchestrator-functie voltooid en zichzelf opnieuw gestart met de nieuwe status. De `Result` kolom bevat de waarde die wordt gebruikt als invoer in de sessie opnieuw gestart.
    * **ExecutionCompleted**: de orchestrator-functie is tot voltooiing worden uitgevoerd (of mislukt). De uitvoer van de functie of de foutgegevens worden opgeslagen in de `Result` kolom.
* **Tijdstempel**: de UTC-timestamp van de geschiedenisgebeurtenis.
* **Naam**: de naam van de functie is aangeroepen.
* **Invoer**: invoer van de functie de JSON-indeling.
* **Resultaat**: de uitvoer van de functie; dat wil zeggen, de geretourneerde waarde.

> [!WARNING]
> Het is nuttig als u een hulpprogramma voor foutopsporing, nemen niet elke afhankelijkheid voor deze tabel. Het verschilt mogelijk als de extensie duurzame functies zich verder ontwikkelt.

Telkens wanneer de functie wordt geactiveerd vanuit een `await`, het duurzame Framework van de taak de orchestrator-functie helemaal opnieuw worden uitgevoerd. Op elke opnieuw uitvoeren die wordt de uitvoeringsgeschiedenis om te bepalen of de huidige asynchrone bewerking heeft genomen consults plaatsen.  Als de bewerking plaatsgevonden heeft, het framework de uitvoer van deze bewerking onmiddellijk opnieuw weergegeven en gaat verder met de volgende `await`. Dit proces gaat door totdat de volledige geschiedenis is onderschept, waarna alle lokale variabelen in de orchestrator-functie zijn teruggezet naar de vorige waarden.

## <a name="orchestrator-code-constraints"></a>Beperkingen voor orchestrator-code

Het gedrag voor opnieuw afspelen maakt beperkingen met betrekking tot het type van de code die kan worden geschreven in een orchestrator-functie:

* Orchestrator-code moet **deterministische**. Het wordt meerdere keren opnieuw worden afgespeeld en hetzelfde resultaat telkens moet opleveren. Bijvoorbeeld, er zijn geen directe-aanroepen naar ophalen van de huidige datum en tijd, willekeurige nummers ophalen, genereren van willekeurige GUID's of aanroepen naar externe eindpunten.

  Als de orchestrator-code nodig heeft om op te halen van de huidige datum en tijd, moet deze gebruiken de [CurrentUtcDateTime](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CurrentUtcDateTime) API, die veilig voor opnieuw afspelen is.

  Als er moet een orchestrator-code voor het genereren van een willekeurige GUID, moet deze gebruiken de [NewGuid](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_NewGuid) API, die veilig voor opnieuw afspelen is.

  Niet-deterministisch bewerkingen moeten worden uitgevoerd in de activiteitsfuncties. Dit omvat elke interactie met andere invoer- of -bindingen. Dit zorgt ervoor dat alle niet-deterministisch waarden worden één keer op de eerste uitvoering worden gegenereerd en in de uitvoeringsgeschiedenis opgeslagen. Volgende uitvoeringen wordt automatisch de opgeslagen waarde gebruikt.

* Orchestrator-code moet **niet-blokkerende**. Bijvoorbeeld: dit betekent dat geen i/o en er worden geen aanroepen naar `Thread.Sleep` of vergelijkbare API's.

  Als een orchestrator uitstellen moet, kunt deze gebruiken de [CreateTimer](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CreateTimer_) API.

* Orchestrator-code moet **nooit gestart een asynchrone bewerking** behalve met behulp van de [DurableOrchestrationContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html) API. Bijvoorbeeld: Nee `Task.Run`, `Task.Delay` of `HttpClient.SendAsync`. De duurzame taak Framework orchestrator-code op een enkele thread wordt uitgevoerd en kan niet communiceren met andere threads die kunnen worden gepland door andere asynchrone API's.

* **Oneindige lus moeten worden vermeden** in orchestrator-code. Omdat de duurzame Framework van de taak wordt de uitvoeringsgeschiedenis als de orchestration-functie vordert opgeslagen, kan een oneindige lus ervoor zorgen dat een orchestrator-exemplaar te weinig geheugen. Voor scenario's met oneindige lus, zoals API's gebruiken [ContinueAsNew](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_ContinueAsNew_) opnieuw opstarten van de functie-uitvoering en vorige uitvoeringsgeschiedenis negeren.

Terwijl deze beperkingen op eerst in de praktijk die ze niet moeilijk afschrikken kunnen te volgen. De duurzame taak Framework probeert te detecteren schendingen van de bovenstaande regels en genereert een `NonDeterministicOrchestrationException`. Dit gedrag detectie is best-effort echter en u mag niet afhankelijk zijn van deze.

> [!NOTE]
> Alle regels gelden alleen voor de functies die zijn geactiveerd door de `orchestrationTrigger` binding. Activiteitsfuncties wordt geactiveerd door de `activityTrigger` binding en -functies die gebruikmaken van de `orchestrationClient` verbinding maakt, hebben geen dergelijke beperkingen.

## <a name="durable-tasks"></a>Duurzame taken

> [!NOTE]
> Deze sectie beschrijft interne implementatiegegevens van duurzame taak Framework. U kunt duurzame functies gebruiken zonder de wetenschap dat deze informatie. Dit is uitsluitend bedoeld om te begrijpen van het gedrag voor opnieuw afspelen.

Taken die veilig kunnen worden gestopt in de orchestrator-functies worden soms aangeduid als *duurzame taken*. Dit zijn taken die worden gemaakt en beheerd door de duurzame Framework van de taak. Voorbeelden zijn de taken die zijn geretourneerd door `CallActivityAsync`, `WaitForExternalEvent`, en `CreateTimer`.

Deze *duurzame taken* intern worden beheerd met behulp van een lijst van `TaskCompletionSource` objecten. Tijdens het opnieuw afspelen, worden deze taken gemaakt als onderdeel van orchestrator tot uitvoering van code en worden uitgevoerd als de functie voor berichtverzending de bijbehorende van geschiedenisgebeurtenissen inventariseert. Dit wordt gedaan synchroon met een enkele thread totdat de volledige geschiedenis is opnieuw is afgespeeld. Duurzame taken, die niet zijn voltooid door het einde van de geschiedenis opnieuw afspelen heeft de juiste acties worden uitgevoerd. Bijvoorbeeld mogelijk een bericht in de wachtrij voor het aanroepen van een functie van de activiteit.

De hier beschreven werking voor uitvoering kunt u te begrijpen waarom orchestrator-functiecode moet nooit `await` een taak niet-duurzame: de dispatcher-thread niet kunt wachten op deze is voltooid en een retouraanroep door deze taak kan de tracering mogelijk beschadigd status van de orchestrator-functie. Aantal controles runtime zijn aanwezig om te proberen om dit te voorkomen.

Als u meer informatie wilt over hoe orchestrator-functies in de duurzame Framework van de taak wordt uitgevoerd, het beste om te raadplegen is de [duurzame taak broncode van GitHub](https://github.com/Azure/durabletask). In het bijzonder Zie [TaskOrchestrationExecutor.cs](https://github.com/Azure/durabletask/blob/master/src/DurableTask.Core/TaskOrchestrationExecutor.cs) en [TaskOrchestrationContext.cs](https://github.com/Azure/durabletask/blob/master/src/DurableTask.Core/TaskOrchestrationContext.cs)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over het beheer van exemplaar](durable-functions-instance-management.md)
