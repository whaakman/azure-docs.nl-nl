---
title: Versiebeheer in duurzame functies - Azure
description: Informatie over het implementeren van versiebeheer in de uitbreiding duurzame functies voor Azure Functions.
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
ms.openlocfilehash: 0a86e4a87f5ec23c284aa4e5cfb2c67622b3ebe9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="versioning-in-durable-functions-azure-functions"></a>Versiebeheer in duurzame functies (Azure-functies)

Het is onvermijdelijk dat functies worden toegevoegd, verwijderd, en gedurende de levensduur van een toepassing worden gewijzigd. [Duurzame functies](durable-functions-overview.md) kunnen samen chaining werkt op een manier die niet eerder mogelijk was en deze koppeling is van invloed op hoe u versiebeheer kan verwerken.

## <a name="how-to-handle-breaking-changes"></a>Het verwerken van grote wijzigingen

Er zijn enkele voorbeelden van grote wijzigingen moet houden. Dit artikel worden de meest voorkomende waarden. De belangrijkste thema achter al deze is dat beide nieuwe en bestaande functie integraties worden beïnvloed door wijzigingen in de functiecode.

### <a name="changing-activity-function-signatures"></a>Activiteit functiehandtekeningen wijzigen

Een wijziging in de handtekening verwijst naar een wijziging in de naam, invoer of uitvoer van een functie. Als u dit soort wijziging is aangebracht in een functie van de activiteit, kan deze de orchestrator-functie die afhankelijk zijn van het verbroken. Als u de orchestrator-functie zodat deze wijziging hebt bijgewerkt, kon u bestaande onderweg exemplaren verbreken.

Stel bijvoorbeeld dat we hebben de volgende functie.

```csharp
[FunctionName("FooBar")]
public static Task Run([OrchestrationTrigger] DurableOrchestrationContext context)
{
    bool result = await context.CallActivityAsync<bool>("Foo");
    await context.CallActivityAsync("Bar", result);
}
```

Deze functie eenvoudig neemt de resultaten van **Foo** en doorgegeven aan **balk**. Stel moeten we wijzigen de retourwaarde van **Foo** van `bool` naar `int` ter ondersteuning van een bredere reeks waarden als resultaat. Het resultaat ziet er als volgt:

```csharp
[FunctionName("FooBar")]
public static Task Run([OrchestrationTrigger] DurableOrchestrationContext context)
{
    int result = await context.CallActivityAsync<int>("Foo");
    await context.CallActivityAsync("Bar", result);
}
```

Deze wijziging werkt goed voor alle nieuwe exemplaren van de orchestrator-functie, maar geen onderweg instanties wordt verbroken. Neem bijvoorbeeld het geval waarbij een exemplaar van de orchestration-aanroepen **Foo**, een Booleaanse waarde en klik vervolgens op controlepunten weer worden opgehaald. Als de wijziging van de handtekening op dit moment is geïmplementeerd, mislukt het controlepunt exemplaar onmiddellijk wanneer deze wordt hervat en opnieuw weergegeven van de aanroep van `context.CallActivityAsync<int>("Foo")`. Dit is omdat het resultaat in de geschiedenistabel `bool` , maar de nieuwe code probeert te deserialiseren naar `int`.

Dit is slechts een van veel verschillende manieren dat een wijziging in de handtekening bestaande exemplaren kan breken. In het algemeen als een orchestrator, moet u de manier wijzigen roept een functie, wordt de wijziging is waarschijnlijk problematisch.

### <a name="changing-orchestrator-logic"></a>Orchestrator-logica wijzigen

De andere klasse van versioning problemen is afkomstig van de code van de functie orchestrator op een manier die de replay-logica voor onderweg exemplaren confuses te wijzigen.

Houd rekening met de volgende orchestrator-functie:

```csharp
[FunctionName("FooBar")]
public static Task Run([OrchestrationTrigger] DurableOrchestrationContext context)
{
    bool result = await context.CallActivityAsync<bool>("Foo");
    await context.CallActivityAsync("Bar", result);
}
```

Nu gaan we wordt ervan uitgegaan dat u wilt maken van een schijnbaar onschuldige wijziging in een andere functieaanroep van toevoegen.

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

Deze wijziging wordt toegevoegd aan een nieuwe functie-aanroep **SendNotification** tussen **Foo** en **balk**. Er zijn geen wijzigingen handtekening. Het probleem zich voordoet als een bestaand exemplaar wordt hervat vanuit de aanroep van **balk**. Tijdens het opnieuw afspelen, als de aanroep van de oorspronkelijke **Foo** geretourneerd `true`, en vervolgens de orchestrator-replay belt in **SendNotification** die zich niet in de geschiedenis van de uitvoering. Als gevolg hiervan het duurzame Framework van de taak is mislukt met een `NonDeterministicOrchestrationException` omdat er een aanroep van opgetreden **SendNotification** wanneer deze verwachtte een aanroep van **balk**.

## <a name="mitigation-strategies"></a>Risicobeperking strategieën

Hier volgen enkele van de strategieën voor het omgaan met versiebeheer uitdagingen:

* Niets doen
* Alle exemplaren van onderweg stoppen
* Side-by-side-implementaties

### <a name="do-nothing"></a>Niets doen

De eenvoudigste manier voor het afhandelen van een belangrijke wijziging is om te laten onderweg orchestration exemplaren mislukken. Nieuwe exemplaren van de gewijzigde code uitgevoerd.

Of dit een probleem is, is afhankelijk van het belang van uw onderweg exemplaren. Als u in de actieve ontwikkeling en geen belang bij onderweg exemplaren, is het mogelijk dat dit goed genoeg. U moet echter omgaan met uitzonderingen en fouten in de pipeline diagnostische gegevens. Als u voorkomen dat deze dingen wilt, kunt u de andere opties voor de systeemversiebeheergeschiedenis.

### <a name="stop-all-in-flight-instances"></a>Alle exemplaren van onderweg stoppen

Een andere optie is niet meer alle onderweg exemplaren. Dit kan worden gedaan door het selectievakje van de inhoud van de interne **besturingselement wachtrij** en **werkitem wachtrij** wachtrijen. De exemplaren worden permanent vastgelopen waar ze zijn, maar ze worden niet door elkaar op uw telemetrie met foutberichten. Dit is ideaal in snelle prototype-ontwikkeling.

> [!WARNING]
> De details van deze wachtrijen kunnen worden gewijzigd gedurende een periode, zodat Vertrouw niet op deze techniek voor productieworkloads.

### <a name="side-by-side-deployments"></a>Side-by-side-implementaties

De meest mislukken bewijs manier om ervoor te zorgen dat grote wijzigingen veilig zijn geïmplementeerd, is door het implementeren van side-by-side met de oudere versies. U kunt dit doen met behulp van een van de volgende technieken:

* De updates als geheel nieuwe functies implementeren (nieuwe namen).
* Alle updates implementeren als een nieuwe functie-app met een ander opslagaccount.
* Implementeren van een nieuw exemplaar van de functie-app, maar met een bijgewerkte `TaskHub` naam. Dit is de aanbevolen methode.

### <a name="how-to-change-task-hub-name"></a>Het wijzigen van de taaknaam hub

De taak hub kan worden geconfigureerd in de *host.json* als volgt:

```json
{
    "durableTask": {
        "HubName": "MyTaskHubV2"
    }
}
```

De standaardwaarde is `DurableFunctionsHub`.

Alle Azure Storage-entiteiten zijn met de naam op basis van de `HubName` configuratiewaarde. De taak hub een nieuwe naam geeft, zorgt u ervoor dat de afzonderlijke wachtrijen en geschiedenistabel voor de nieuwe versie van uw toepassing worden gemaakt.

We raden aan dat u de nieuwe versie van de functie-app naar een nieuwe implementeren [Implementatiesleuf](https://blogs.msdn.microsoft.com/appserviceteam/2017/06/13/deployment-slots-preview-for-azure-functions/). Implementatiesites kunnen u meerdere exemplaren van de functie app side-by-side uitvoeren met slechts één van deze als het actieve *productie* sleuf. Wanneer u klaar om de nieuwe orchestration logica aan uw bestaande infrastructuur weer te geven bent, kan het worden net zo eenvoudig als het wisselen van de nieuwe versie in de productiesite.

> [!NOTE]
> Deze strategie werkt het beste wanneer u HTTP- en webhook triggers voor orchestrator-functies. De definitie van de trigger moet voor niet-HTTP-triggers, zoals wachtrijen of Event Hubs zijn afgeleid van een app-instelling als onderdeel van de wisselbewerking wordt bijgewerkt.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Informatie over het verwerken van de prestaties en schalen van problemen](durable-functions-perf-and-scale.md)
