---
title: Bindingen voor duurzame functies - Azure
description: Het gebruik van triggers en bindingen voor de extensie duurzame Functons voor Azure Functions.
services: functions
author: kashimiz
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 10/23/2018
ms.author: azfuncdf
ms.openlocfilehash: f9bf42e5e20a7d9e861d0c3354040e981bf3ef21
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2018
ms.locfileid: "49987746"
---
# <a name="bindings-for-durable-functions-azure-functions"></a>Bindingen voor duurzame functies (Azure Functions)

De [duurzame functies](durable-functions-overview.md) extensie worden twee nieuwe bindingen van de trigger waarmee de uitvoering van orchestrator en activiteit functies geïntroduceerd. Het bevat ook een Uitvoerbinding die als een client voor de runtime duurzame functies fungeert.

## <a name="orchestration-triggers"></a>Orchestration-triggers

De orchestration-trigger kunt u auteur duurzame orchestrator-functies. Deze trigger ondersteunt nieuwe instanties van de orchestrator-functie wordt gestart en bestaande exemplaren van de orchestrator-functie die in afwachting zijn "van" een taak wordt hervat.

Wanneer u de Visual Studio-hulpprogramma's voor Azure Functions, de orchestration-trigger is geconfigureerd met behulp van de [OrchestrationTriggerAttribute](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.OrchestrationTriggerAttribute.html) .NET-kenmerk.

Bij het schrijven van orchestrator-functies in scripttalen (bijvoorbeeld in Azure portal), de orchestration-trigger wordt gedefinieerd door de volgende JSON-object in de `bindings` matrix van de *function.json* bestand:

```json
{
    "name": "<Name of input parameter in function signature>",
    "orchestration": "<Optional - name of the orchestration>",
    "type": "orchestrationTrigger",
    "direction": "in"
}
```

* `orchestration` is de naam van de indeling. Dit is de waarde die clients gebruiken moeten wanneer ze willen nieuwe exemplaren van deze functie orchestrator worden gestart. Deze eigenschap is optioneel. Indien niet opgegeven, wordt de naam van de functie gebruikt.

Intern pollt deze binding trigger een reeks van wachtrijen in het standaardopslagaccount voor de functie-app. Deze wachtrijen zijn interne implementatiegegevens van de extensie, die is de reden waarom ze niet expliciet worden geconfigureerd in de bindingeigenschappen van de.

### <a name="trigger-behavior"></a>Trigger-gedrag

Hier vindt u informatie over de orchestration-trigger:

* **Single-threading** -een enkel dispatcher-thread wordt gebruikt voor de uitvoering van alle orchestrator-functie op een afzonderlijke host-instantie. Daarom is het belangrijk om ervoor te zorgen dat de orchestrator-functiecode efficiënt is en alle i/o niet uitvoeren. Het is ook belangrijk om ervoor te zorgen dat deze thread wordt niet alle tot het asynchrone werk, behalve wanneer in afwachting van duurzame functies-specifieke taaktypen.
* **Afhandeling van poison-berichten** -er is geen ondersteuning voor beheer van Onverwerkbare berichten in de orchestration-triggers.
* **Bericht zichtbaarheid** -Orchestration triggerberichten worden uit de wachtrij genomen en onzichtbaar gedurende een configureerbare duur. De zichtbaarheid van deze berichten wordt automatisch verlengd, zolang de functie-app uitgevoerd en in orde wordt.
* **Retourwaarden** -waarden worden geserialiseerd naar JSON en persistent gemaakt met de orchestration-geschiedenistabel in Azure Table storage worden geretourneerd. Deze geretourneerde waarden kunnen worden opgevraagd met de orchestration-client-binding, later wordt beschreven.

> [!WARNING]
> Orchestrator-functies moeten nooit gebruikt voor elke invoer of uitvoerbindingen dan de orchestration binding activeren. In dat geval heeft de potentiële problemen met de extensie duurzame taak veroorzaken omdat deze bindingen kunnen zich niet aan de één-threading en i/o-regels.

### <a name="trigger-usage"></a>Gebruik van de trigger

De orchestration-trigger binding ondersteunt zowel invoer en uitvoer. Hier volgen enkele dingen die u moet weten over de invoer en uitvoer verwerking:

* **invoer** -Orchestration-functies ondersteunen alleen [DurableOrchestrationContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html) als een parameter. Deserialisatie van invoer rechtstreeks in de functiehandtekening wordt niet ondersteund. Code moet gebruiken de [GetInput\<T >](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_GetInput__1) methode voor het ophalen van orchestrator-functie-invoer. Deze invoer moet de JSON-serialisatie-typen.
* **uitvoer** -Orchestration triggers ondersteunen zowel uitvoerwaarden als invoer. De geretourneerde waarde van de functie wordt gebruikt voor het toewijzen van de uitvoerwaarde en moet een JSON-geserialiseerd. Als een functie geeft als resultaat `Task` of `void`, een `null` waarde wordt opgeslagen als de uitvoer.

### <a name="trigger-sample"></a>Voorbeeld van de trigger

Hier volgt een voorbeeld van wat de meest eenvoudige "Hallo wereld" orchestrator-functie als volgt uitzien:

#### <a name="c"></a>C#

```csharp
[FunctionName("HelloWorld")]
public static string Run([OrchestrationTrigger] DurableOrchestrationContext context)
{
    string name = context.GetInput<string>();
    return $"Hello {name}!";
}
```

#### <a name="javascript-functions-v2-only"></a>JavaScript (alleen functies v2)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const name = context.df.getInput();
    return `Hello ${name}!`;
});
```

> [!NOTE]
> JavaScript-orchestrators moeten gebruiken `return`. De `durable-functions` bibliotheek aanroepen zorgt de `context.done` methode.

De meeste orchestrator-functies aanroepen activiteitsfuncties, hier is een voorbeeld van een "Hallo wereld" die laat zien hoe u een activiteit functie aanroepen:

#### <a name="c"></a>C#

```csharp
[FunctionName("HelloWorld")]
public static async Task<string> Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    string name = context.GetInput<string>();
    string result = await context.CallActivityAsync<string>("SayHello", name);
    return result;
}
```

#### <a name="javascript-functions-v2-only"></a>JavaScript (alleen functies v2)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const name = context.df.getInput();
    const result = yield context.df.callActivity("SayHello", name);
    return result;
});
```

## <a name="activity-triggers"></a>Activiteit-triggers

De trigger activiteit kunt u op het schrijven van functies die worden aangeroepen door de orchestrator-functies.

Als u Visual Studio, de trigger van de activiteit is geconfigureerd met behulp van de [ActivityTriggerAttribute](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.ActivityTriggerAttribute.html) .NET-kenmerk. 

Als u VS Code of de Azure-portal voor ontwikkeling gebruikt, de activiteit-trigger wordt gedefinieerd door de volgende JSON-object in de `bindings` reeks *function.json*:

```json
{
    "name": "<Name of input parameter in function signature>",
    "activity": "<Optional - name of the activity>",
    "type": "activityTrigger",
    "direction": "in"
}
```

* `activity` is de naam van de activiteit. Dit is de waarde die de orchestrator-functies gebruiken om aan deze activiteit functie aanroepen. Deze eigenschap is optioneel. Indien niet opgegeven, wordt de naam van de functie gebruikt.

Intern pollt deze binding trigger een wachtrij in het standaardopslagaccount voor de functie-app. Deze wachtrij is een interne implementatiedetail van de extensie, is de reden waarom deze niet expliciet is geconfigureerd in de bindingeigenschappen van de.

### <a name="trigger-behavior"></a>Trigger-gedrag

Hier vindt u informatie over de trigger activiteit:

* **Threading** -activiteit-triggers geen in tegenstelling tot de orchestration-trigger, alle beperkingen met betrekking threading of i/o. Ze kunnen worden behandeld als normale functies.
* **Afhandeling van poison-berichten** -er is geen ondersteuning voor beheer van Onverwerkbare berichten in de activiteit triggers.
* **Bericht zichtbaarheid** -activiteit triggerberichten worden uit de wachtrij genomen en onzichtbaar gedurende een configureerbare duur. De zichtbaarheid van deze berichten wordt automatisch verlengd, zolang de functie-app uitgevoerd en in orde wordt.
* **Retourwaarden** -waarden worden geserialiseerd naar JSON en persistent gemaakt met de orchestration-geschiedenistabel in Azure Table storage worden geretourneerd.

> [!WARNING]
> De opslag back-end voor de activiteitsfuncties is een implementatiedetail en gebruikerscode mag geen interactie hebben met deze opslagentiteiten rechtstreeks.

### <a name="trigger-usage"></a>Gebruik van de trigger

De activiteit trigger binding ondersteunt zowel invoer en uitvoer, net als de orchestration-trigger. Hier volgen enkele dingen die u moet weten over de invoer en uitvoer verwerking:

* **invoer** -activiteitsfuncties systeemeigen wijze [DurableActivityContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableActivityContext.html) als een parameter. De functie van een activiteit kan ook worden gedeclareerd met een parameter die JSON-geserialiseerd. Bij het gebruik `DurableActivityContext`, u kunt aanroepen [GetInput\<T >](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableActivityContext.html#Microsoft_Azure_WebJobs_DurableActivityContext_GetInput__1) invoer op te halen en te deserialiseren van de functie van de activiteit.
* **uitvoer** -activiteit-functies ondersteunen zowel uitvoerwaarden als invoer. De geretourneerde waarde van de functie wordt gebruikt voor het toewijzen van de uitvoerwaarde en moet een JSON-geserialiseerd. Als een functie geeft als resultaat `Task` of `void`, een `null` waarde wordt opgeslagen als de uitvoer.
* **metagegevens** -functies van de activiteit verbinding kunnen maken met een `string instanceId` parameter om op te halen van de exemplaar-ID van de bovenliggende-indeling.

### <a name="trigger-sample"></a>Voorbeeld van de trigger

Hier volgt een voorbeeld van wat een eenvoudige 'Hallo wereld'-activiteit-functie als volgt uitzien:

#### <a name="c"></a>C#

```csharp
[FunctionName("SayHello")]
public static string SayHello([ActivityTrigger] DurableActivityContext helloContext)
{
    string name = helloContext.GetInput<string>();
    return $"Hello {name}!";
}
```

#### <a name="javascript-functions-v2-only"></a>JavaScript (alleen functies v2)

```javascript
module.exports = function(context) {
    context.done(null, `Hello ${context.bindings.name}!`);
};
```

Het standaardtype voor de parameter voor de `ActivityTriggerAttribute` binding wordt `DurableActivityContext`. Echter, activiteit triggers ook ondersteuning voor bindende rechtstreeks naar een JSON-serializeable typen (met inbegrip van primitieve typen), zodat dezelfde functie kan worden vereenvoudigd als volgt:

#### <a name="c"></a>C#

```csharp
[FunctionName("SayHello")]
public static string SayHello([ActivityTrigger] string name)
{
    return $"Hello {name}!";
}
```

#### <a name="javascript-functions-v2-only"></a>JavaScript (alleen functies v2)

```javascript
module.exports = function(context, name) {
    context.done(null, `Hello ${name}!`);
};
```

### <a name="passing-multiple-parameters"></a>Meerdere parameters doorgeven 

Het is niet mogelijk meerdere parameters rechtstreeks doorgeven aan een functie van de activiteit. Om door te geven in een matrix met objecten of voor het gebruik van de aanbeveling is in dit geval [ValueTuples](https://docs.microsoft.com/dotnet/csharp/tuples) objecten.

Het volgende voorbeeld wordt met behulp van nieuwe functies van [ValueTuples](https://docs.microsoft.com/dotnet/csharp/tuples) toegevoegd met [C# 7](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-7#tuples):

```csharp
[FunctionName("GetCourseRecommendations")]
public static async Task<dynamic> RunOrchestrator(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    string major = "ComputerScience";
    int universityYear = context.GetInput<int>();

    dynamic courseRecommendations = await context.CallActivityAsync<dynamic>("CourseRecommendations", (major, universityYear));
    return courseRecommendations;
}

[FunctionName("CourseRecommendations")]
public static async Task<dynamic> Mapper([ActivityTrigger] DurableActivityContext inputs)
{
    // parse input for student's major and year in university 
    (string Major, int UniversityYear) studentInfo = inputs.GetInput<(string, int)>();

    // retrieve and return course recommendations by major and university year
    return new {
        major = studentInfo.Major,
        universityYear = studentInfo.UniversityYear,
        recommendedCourses = new []
        {
            "Introduction to .NET Programming",
            "Introduction to Linux",
            "Becoming an Entrepreneur"
        }
    };
}
```

## <a name="orchestration-client"></a>Orchestration-client

De orchestration-client-binding kunt u het schrijven van functies die communiceren met orchestrator-functies. U kunt bijvoorbeeld reageren op orchestration-exemplaren in de volgende manieren:
* Ze worden gestart.
* Query uitvoeren op hun status.
* Deze beëindigen.
* Gebeurtenissen verzenden naar deze terwijl deze nu worden uitgevoerd.

Als u Visual Studio gebruikt, kunt u binden aan de orchestration-client met behulp van de [OrchestrationClientAttribute](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.OrchestrationClientAttribute.html) .NET-kenmerk.

Als u scripttalen (bijvoorbeeld *.csx* of *.js* bestanden) voor de ontwikkeling, de orchestration-trigger wordt gedefinieerd door de volgende JSON-object in de `bindings` reeks  *Function.JSON*:

```json
{
    "name": "<Name of input parameter in function signature>",
    "taskHub": "<Optional - name of the task hub>",
    "connectionName": "<Optional - name of the connection string app setting>",
    "type": "orchestrationClient",
    "direction": "out"
}
```

* `taskHub` -Wordt gebruikt in scenario's waarbij meerdere functie-apps delen hetzelfde opslagaccount, maar moeten worden geïsoleerd van elkaar. Indien niet opgegeven, de standaardwaarde van `host.json` wordt gebruikt. Deze waarde moet overeenkomen met de waarde die wordt gebruikt door de doel-orchestrator-functies.
* `connectionName` -De naam van een app-instelling met de verbindingsreeks van een storage-account. Het opslagaccount dat wordt vertegenwoordigd door deze verbindingsreeks moet hetzelfde account gebruikt door de doel-orchestrator-functies. Indien niet opgegeven, wordt de standaardverbindingsreeks voor storage-account voor de functie-app wordt gebruikt.

> [!NOTE]
> In de meeste gevallen wordt aangeraden dat u deze eigenschappen weglaten en zijn afhankelijk van het standaardgedrag.

### <a name="client-usage"></a>Clientgebruik

In C#-functies, bindt u doorgaans aan `DurableOrchestrationClient`, waardoor u volledige toegang tot alle client-API's ondersteund door duurzame functies. API's op de clientobject zijn onder andere:

* [StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_)
* [GetStatusAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_GetStatusAsync_)
* [TerminateAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_TerminateAsync_)
* [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_)

U kunt ook koppelen aan `IAsyncCollector<T>` waar `T` is [StartOrchestrationArgs](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.StartOrchestrationArgs.html) of `JObject`.

Zie de [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) API-documentatie voor meer informatie over deze bewerkingen.

### <a name="client-sample-visual-studio-development"></a>Clientvoorbeeld (Visual Studio-ontwikkeling)

Hier volgt een voorbeeld van de wachtrij geactiveerde functie waarmee een 'Hallo wereld'-indeling wordt gestart.

```csharp
[FunctionName("QueueStart")]
public static Task Run(
    [QueueTrigger("durable-function-trigger")] string input,
    [OrchestrationClient] DurableOrchestrationClient starter)
{
    // Orchestration input comes from the queue message content.
    return starter.StartNewAsync("HelloWorld", input);
}
```

### <a name="client-sample-not-visual-studio"></a>Clientvoorbeeld (geen Visual Studio)

Als u Visual Studio niet voor het ontwikkelen gebruikt, kunt u de volgende *function.json* bestand. In dit voorbeeld laat zien hoe een wachtrij geactiveerde functie die gebruikmaakt van de binding duurzame orchestration-client configureren:

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "queueTrigger",
      "queueName": "durable-function-trigger",
      "direction": "in"
    },
    {
      "name": "starter",
      "type": "orchestrationClient",
      "direction": "out"
    }
  ],
  "disabled": false
} 
```

Hieronder vindt u taalspecifieke voorbeelden die nieuwe orchestrator-functie-exemplaren worden gestart.

#### <a name="c-sample"></a>Voorbeeld van C#

Het volgende voorbeeld ziet u hoe u de binding met een nieuwe functie-exemplaar starten vanuit een C#-scriptfunctie duurzame orchestration-client:

```csharp
#r "Microsoft.Azure.WebJobs.Extensions.DurableTask"

public static Task<string> Run(string input, DurableOrchestrationClient starter)
{
    return starter.StartNewAsync("HelloWorld", input);
}
```

#### <a name="javascript-sample"></a>JavaScript-voorbeeld

Het volgende voorbeeld laat zien hoe de duurzame orchestration-client binden aan een nieuwe functie-exemplaar starten vanuit een JavaScript-functie gebruiken:

```js
module.exports = function (context, input) {
    var id = generateSomeUniqueId();
    context.bindings.starter = [{
        FunctionName: "HelloWorld",
        Input: input,
        InstanceId: id
    }];

    context.done(null, id);
};
```

Meer informatie over het starten van exemplaren te vinden in [management-exemplaar](durable-functions-instance-management.md).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over het functioneren van het plaatsen van controlepunten en herhaling](durable-functions-checkpointing-and-replay.md)

