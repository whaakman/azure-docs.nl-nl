---
title: Versiebeheer in duurzame functies - Azure
description: Informatie over het implementeren van versiebeheer in de extensie duurzame functies voor Azure Functions.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: 9d628f48f4958e4e763ed0064462a5fb2ed398bf
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44094329"
---
# <a name="versioning-in-durable-functions-azure-functions"></a>Versiebeheer in duurzame functies (Azure Functions)

Het is onvermijdelijk dat functies zal worden toegevoegd, verwijderd en gewijzigd gedurende de levensduur van een toepassing. [Duurzame functies](durable-functions-overview.md) staat samen koppeling werkt op een manier die niet eerder mogelijk was zijn, en deze koppeling is van invloed op hoe u versiebeheer kunt verwerken.

## <a name="how-to-handle-breaking-changes"></a>Verwerken van wijzigingen die fouten veroorzaken

Er zijn enkele voorbeelden van belangrijke wijzigingen rekening mee moet houden. Dit artikel worden de meest voorkomende. De belangrijkste thema achter alle hiervan is dat beide indelingen nieuwe en bestaande functie worden beïnvloed door wijzigingen in de functiecode aan te geven.

### <a name="changing-activity-function-signatures"></a>Activiteit functiehandtekeningen wijzigen

Een wijziging handtekening verwijst naar een wijziging in de naam, invoer of uitvoer van een functie. Als dit soort wijziging is aangebracht in de functie van een activiteit, kan deze de orchestrator-functie die afhankelijk zijn van het verbroken. Als u bijwerkt naar de orchestrator-functie om te voldoen aan deze wijziging, zou u bestaande actieve exemplaren kunnen worden verbroken.

Stel bijvoorbeeld dat we hebben de volgende functie.

```csharp
[FunctionName("FooBar")]
public static Task Run([OrchestrationTrigger] DurableOrchestrationContext context)
{
    bool result = await context.CallActivityAsync<bool>("Foo");
    await context.CallActivityAsync("Bar", result);
}
```

Deze eenvoudig functie haalt de resultaten van **Foo** en doorgegeven aan **balk**. Stel dat we moeten de geretourneerde waarde van **Foo** van `bool` naar `int` ter ondersteuning van een groter aantal waarden als resultaat. Het resultaat ziet er zo uit:

```csharp
[FunctionName("FooBar")]
public static Task Run([OrchestrationTrigger] DurableOrchestrationContext context)
{
    int result = await context.CallActivityAsync<int>("Foo");
    await context.CallActivityAsync("Bar", result);
}
```

Deze wijziging werkt prima voor alle nieuwe instanties van de orchestrator-functie, maar alle actieve exemplaren wordt verbroken. Neem bijvoorbeeld het geval waarbij een orchestration-instantie aanroepen **Foo**, wordt in een Booleaanse waarde en vervolgens de controlepunten. Als de wijziging van de handtekening op dit moment wordt geïmplementeerd, mislukt het controlepunt exemplaar onmiddellijk wanneer deze wordt hervat en de aanroep opnieuw weergegeven `context.CallActivityAsync<int>("Foo")`. Dit is omdat het resultaat in de geschiedenistabel `bool` , maar de nieuwe code probeert te deserialiseren naar `int`.

Dit is slechts een van veel verschillende manieren dat een wijziging handtekeningen bestaande exemplaren kan breken. In het algemeen als een orchestrator moet worden gewijzigd van de manier waarop roept een functie, wordt de wijziging is waarschijnlijk problematisch zijn.

### <a name="changing-orchestrator-logic"></a>Orchestrator-logica wijzigen

De andere klasse van versiebeheer problemen afkomstig van het wijzigen van de orchestrator-functiecode op een manier die de logica voor opnieuw afspelen voor actieve exemplaren confuses.

Houd rekening met de volgende orchestrator-functie:

```csharp
[FunctionName("FooBar")]
public static Task Run([OrchestrationTrigger] DurableOrchestrationContext context)
{
    bool result = await context.CallActivityAsync<bool>("Foo");
    await context.CallActivityAsync("Bar", result);
}
```

Nu gaan we wordt ervan uitgegaan dat u wilt een wijziging schijnbaar onschuldig uitziet om toe te voegen een andere aanroep van de functie.

```csharp
[FunctionName("FooBar")]
public static Task Run([OrchestrationTrigger] DurableOrchestrationContext context)
{
    bool result = await context.CallActivityAsync<bool>("Foo");
    if (result)
    {
        await context.CallActivityAsync("SendNotification");
    }

    await context.CallActivityAsync("Bar", result);
}
```

Deze wijziging wordt toegevoegd aan een nieuwe aanroep van de functie **SendNotification** tussen **Foo** en **balk**. Er zijn geen wijzigingen handtekening. Het probleem zich voordoet wanneer een bestaand exemplaar van de aanroep aan **balk**. Tijdens het opnieuw afspelen, als de oorspronkelijke aanroep aan **Foo** geretourneerd `true`, de orchestrator-herhaling wordt aangeroepen in **SendNotification** die zich niet in de uitvoeringsgeschiedenis. Als gevolg hiervan de duurzame Framework van de taak is mislukt met een `NonDeterministicOrchestrationException` omdat er een aanroep van **SendNotification** wanneer het waarschijnlijk om te zien van een aanroep van **balk**.

## <a name="mitigation-strategies"></a>Strategieën voor risicobeperking

Hier volgen enkele van de strategieën voor het omgaan met uitdagingen voor versiebeheer:

* Niets doen
* Stop alle actieve exemplaren
* Side-by-side-implementaties

### <a name="do-nothing"></a>Niets doen

De eenvoudigste manier om af te handelen van een belangrijke wijziging is om te laten die onderweg zijn orchestration exemplaren mislukken. Nieuwe exemplaren de gewijzigde code met succes is uitgevoerd.

Of dit een probleem is, is afhankelijk van het belang van uw instanties die onderweg zijn. Als u in de ontwikkeling en geen belang bij die onderweg zijn exemplaren, is het mogelijk dat dit goed genoeg. U moet echter omgaan met uitzonderingen en fouten in de pijplijn diagnostische gegevens. Als u voorkomen dat deze toepassingen wilt, kunt u de andere opties voor versiebeheer.

### <a name="stop-all-in-flight-instances"></a>Stop alle actieve exemplaren

Een andere optie is om te stoppen alle actieve exemplaren. Dit kan worden gedaan door te wissen van de inhoud van de interne **besturingselement wachtrij** en **werkitem wachtrij** wachtrijen. De exemplaren worden altijd vastgelopen waar ze zijn, maar ze worden niet bruikbaar blijft uw telemetrie met foutberichten. Dit is ideaal prototype snelle ontwikkeling.

> [!WARNING]
> De details van deze wachtrijen kunnen worden gewijzigd na verloop van tijd, dus niet afhankelijk zijn van deze techniek voor productieworkloads.

### <a name="side-by-side-deployments"></a>Side-by-side-implementaties

De meest negatief serviceplatform geschikt voor de manier om ervoor te zorgen dat wijzigingen die fouten veroorzaken veilig zijn geïmplementeerd, is door het implementeren van side-by-side met uw oudere versies. Dit kan worden gedaan met behulp van een van de volgende technieken:

* De updates als geheel nieuwe functies implementeren (nieuwe namen).
* Alle updates implementeren als een nieuwe functie-app met een ander opslagaccount.
* Implementeren van een nieuwe kopie van de functie-app, maar met een bijgewerkte `TaskHub` naam. Dit is de aanbevolen methode.

### <a name="how-to-change-task-hub-name"></a>Naam van de hub taak wijzigen

De taak hub kan worden geconfigureerd de *host.json* bestand als volgt:

```json
{
    "durableTask": {
        "HubName": "MyTaskHubV2"
    }
}
```

De standaardwaarde is `DurableFunctionsHub`.

Alle Azure Storage-entiteiten zijn met de naam op basis van de `HubName` configuratiewaarde. Door de taak hub een nieuwe naam geven, zorgt u ervoor dat apart wachtrijen en -tabel met de geschiedenis voor de nieuwe versie van uw toepassing worden gemaakt.

Het is raadzaam dat u de nieuwe versie van de functie-app op een nieuwe implementeren [Deployment Slot](https://blogs.msdn.microsoft.com/appserviceteam/2017/06/13/deployment-slots-preview-for-azure-functions/). Implementatiesites kunnen u meerdere kopieën van uw functie-app side-by-side uitvoeren met slechts één provider als de actieve *productie* sleuf. Wanneer u klaar om beschikbaar te stellen de nieuwe indeling logica voor uw bestaande infrastructuur bent, kan het zijn net zo eenvoudig als het wisselen van de nieuwe versie in de productiesite.

> [!NOTE]
> Deze strategie werkt het beste wanneer u HTTP- en webhook-triggers voor orchestrator-functies. Voor niet-HTTP-triggers, zoals wachtrijen of Event Hubs, moet de definitie van de trigger zijn afgeleid van een app-instelling die wordt bijgewerkt als onderdeel van de wisselbewerking opnieuw uit.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over het afhandelen van prestaties en schaal van problemen](durable-functions-perf-and-scale.md)
