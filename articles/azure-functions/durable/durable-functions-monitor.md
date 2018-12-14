---
title: Monitors in duurzame functies - Azure
description: Informatie over het implementeren van een statusmonitor met behulp van de extensie duurzame functies voor Azure Functions.
services: functions
author: kashimiz
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: azfuncdf
ms.openlocfilehash: 4322841f126e4aa017b4d901cbfb1afd39e5bccf
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2018
ms.locfileid: "53342569"
---
# <a name="monitor-scenario-in-durable-functions---weather-watcher-sample"></a>Monitor-scenario in duurzame functies - weer watcher-voorbeeld

Het patroon monitor verwijst naar een flexibele *terugkerende* proces in een werkstroom - bijvoorbeeld polling totdat aan bepaalde voorwaarden wordt voldaan. Dit artikel wordt uitgelegd voor een voorbeeld dat gebruikmaakt van [duurzame functies](durable-functions-overview.md) voor het implementeren van bewaking.

[!INCLUDE [durable-functions-prerequisites](../../../includes/durable-functions-prerequisites.md)]

## <a name="scenario-overview"></a>Overzicht van scenario's

In dit voorbeeld controleert de huidige weersomstandigheden van een locatie en een gebruiker door SMS een waarschuwing wanneer de skies duidelijk zijn. U kunt een reguliere timer geactiveerde functie gebruiken om te controleren van het weer en verzenden van waarschuwingen. Een probleem met deze methode is echter **beheer van de levensduur**. Als er slechts één waarschuwing moet worden verzonden, de monitor moet zelf uitschakelen na het wissen weer wordt gedetecteerd. Het patroon bewaking kunt stoppen met een eigen uitvoering, onder andere voordelen:

* Monitors die worden uitgevoerd op de intervallen, niet wordt gepland: een timertrigger *wordt uitgevoerd* elk uur; een monitor *wacht* één uur tussen acties. Bewerkingen van een monitor wordt niet overlappen tenzij anders aangegeven, wat belangrijk is voor langlopende taken kan zijn.
* Monitors kunnen dynamische intervallen hebben: de wachttijd op basis van bepaalde voorwaarde kunt wijzigen.
* Monitors kunnen beëindigen wanneer bepaalde voorwaarde wordt voldaan, of worden afgesloten met een ander proces.
* Monitors kunnen duren voordat de parameters. Het voorbeeld laat zien hoe hetzelfde proces weer bewaking kan worden toegepast op alle aangevraagde locatie en telefoonnummer.
* Monitors zijn schaalbaar. Omdat elke monitor een orchestration-exemplaar is, kunnen meerdere beeldschermen worden gemaakt zonder te hoeven maken van nieuwe functies of meer code moet worden gedefinieerd.
* Monitors integreren in grotere werkstromen eenvoudig. Een monitor kan bestaan uit een deel van een meer complexe orchestration-functie of een [onderliggende orchestration](durable-functions-sub-orchestrations.md).

## <a name="configuring-twilio-integration"></a>Twilio-integratie configureren

[!INCLUDE [functions-twilio-integration](../../../includes/functions-twilio-integration.md)]

## <a name="configuring-weather-underground-integration"></a>Weather Underground integratie configureren

In dit voorbeeld gaat over het gebruik van de Underground weer-API om te controleren op huidige weersomstandigheden voor een locatie.

Het eerste wat dat u nodig hebt is een weer Underground account. U kunt een gratis maken op [ https://www.wunderground.com/signup ](https://www.wunderground.com/signup). Zodra u een account hebt, moet u een API-sleutel te verkrijgen. U kunt dit doen door naar de pagina [ https://www.wunderground.com/weather/api ](https://www.wunderground.com/weather/api), en de instellingen van de sleutel te selecteren. Het abonnement Stratus Developer is gratis en voldoende zijn om uit te voeren in dit voorbeeld.

Zodra u een API-sleutel hebt, voeg de volgende **app-instelling** aan uw functie-app.

| Naam van de App-instelling | Waardebeschrijving |
| - | - |
| **WeatherUndergroundApiKey**  | Uw Underground weer-API-sleutel. |

## <a name="the-functions"></a>De functies

Dit artikel wordt uitgelegd dat de volgende functies in de voorbeeld-app:

* `E3_Monitor`: Een orchestrator-functie die worden aangeroepen `E3_GetIsClear` periodiek. Wordt de `E3_SendGoodWeatherAlert` als `E3_GetIsClear` retourneert ' True '.
* `E3_GetIsClear`: Een activiteit-functie waarmee wordt gecontroleerd of de huidige weersomstandigheden voor een locatie.
* `E3_SendGoodWeatherAlert`: Een activiteit-functie waarmee een SMS-bericht via Twilio wordt verzonden.

De volgende secties worden de configuratie en de code die worden gebruikt voor C#-scripts en JavaScript. De code voor het ontwikkelen van Visual Studio wordt weergegeven aan het einde van het artikel.

## <a name="the-weather-monitoring-orchestration-visual-studio-code-and-azure-portal-sample-code"></a>Het weer bewaking orchestration (Visual Studio Code en Azure portal voorbeeldcode)

De **E3_Monitor** functie gebruikmaakt van de standaard *function.json* voor orchestrator-functies.

[!code-json[Main](~/samples-durable-functions/samples/csx/E3_Monitor/function.json)]

Dit is de code die de functie implementeert:

### <a name="c"></a>C#

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E3_Monitor/run.csx)]

### <a name="javascript-functions-2x-only"></a>JavaScript (werkt alleen 2.x)

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E3_Monitor/index.js)]

Deze orchestrator-functie worden de volgende acties uitgevoerd:

1. Haalt de **MonitorRequest** die bestaan uit de *locatie* om te controleren en de *telefoonnummer* aan dat er een SMS-bericht wordt verzonden.
2. Bepaalt de verlooptijd van de monitor. Het voorbeeld wordt een waarde vastgelegde kort te houden.
3. Aanroepen **E3_GetIsClear** om te bepalen of er duidelijk skies beschikbaar op de aangevraagde locatie zijn.
4. Als het weer uitgeschakeld is, roept **E3_SendGoodWeatherAlert** een SMS-bericht verzenden naar het aangevraagde telefoonnummer.
5. Hiermee maakt u een duurzame timer als u wilt doorgaan met de indeling op basis van de volgende pollinginterval. Het voorbeeld wordt een waarde vastgelegde kort te houden.
6. Verder wordt uitgevoerd totdat de [CurrentUtcDateTime](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CurrentUtcDateTime) (C#) of `currentUtcDateTime` (JavaScript) geeft de verlooptijd van de monitor of een SMS-bericht wordt verzonden.

Meerdere exemplaren van orchestrator kunnen tegelijkertijd worden uitgevoerd door het verzenden van meerdere **MonitorRequests**. De locatie voor het bewaken van en het telefoonnummer voor het verzenden van een SMS-bericht om te kunnen worden opgegeven.

## <a name="strongly-typed-data-transfer-net-only"></a>Sterk getypeerde gegevensoverdracht (alleen voor .NET)

De orchestrator vereist verschillende soorten gegevens, dus [POCO objecten gedeeld](../functions-reference-csharp.md#reusing-csx-code) worden gebruikt voor sterk getypeerde gegevensoverdracht in C# en C#-script: [!code-csharp[Main](~/samples-durable-functions/samples/csx/shared/MonitorRequest.csx)]

[!code-csharp[Main](~/samples-durable-functions/samples/csx/shared/Location.csx)]

De JavaScript-voorbeeld maakt gebruik van reguliere JSON-objecten als parameters.

## <a name="helper-activity-functions"></a>Activiteit hulpfuncties

Zoals met andere voorbeelden zijn de functies van de activiteit helper reguliere functies die gebruikmaken van de `activityTrigger` binding activeren. De **E3_GetIsClear** functie haalt de huidige weersomstandigheden met behulp van de Underground weer-API en bepaalt of de lucht uitgeschakeld is. De *function.json* wordt als volgt gedefinieerd:

[!code-json[Main](~/samples-durable-functions/samples/csx/E3_GetIsClear/function.json)]

En dit is de implementatie. Als de POCOs gebruikt voor gegevensoverdracht, logica voor het verwerken van de API aanroepen en parseren van het antwoord JSON abstract in een gedeelde-klasse in C# gemaakt is. U vindt deze als onderdeel van de [Visual Studio-voorbeeldcode](#run-the-sample).

### <a name="c"></a>C#

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E3_GetIsClear/run.csx)]

### <a name="javascript-functions-2x-only"></a>JavaScript (werkt alleen 2.x)

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E3_GetIsClear/index.js)]

De **E3_SendGoodWeatherAlert** -functie maakt gebruik van de Twilio-binding voor het verzenden van een SMS-bericht om de eindgebruiker te informeren dat het een goed moment voor een overzicht wordt is. De *function.json* is eenvoudig:

[!code-json[Main](~/samples-durable-functions/samples/csx/E3_SendGoodWeatherAlert/function.json)]

En hier is de code die de SMS-bericht verzonden:

### <a name="c"></a>C#

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E3_SendGoodWeatherAlert/run.csx)]

### <a name="javascript-functions-2x-only"></a>JavaScript (werkt alleen 2.x)

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E3_SendGoodWeatherAlert/index.js)]

## <a name="run-the-sample"></a>De voorbeeldtoepassing uitvoeren

Met behulp van de HTTP-geactiveerde functies opgenomen in het voorbeeld, kunt u de indeling starten door de volgende HTTP POST-aanvraag te verzenden:

```
POST https://{host}/orchestrators/E3_Monitor
Content-Length: 77
Content-Type: application/json

{ "location": { "city": "Redmond", "state": "WA" }, "phone": "+1425XXXXXXX" }
```

```
HTTP/1.1 202 Accepted
Content-Type: application/json; charset=utf-8
Location: https://{host}/admin/extensions/DurableTaskExtension/instances/f6893f25acf64df2ab53a35c09d52635?taskHub=SampleHubVS&connection=Storage&code={SystemKey}
RetryAfter: 10

{"id": "f6893f25acf64df2ab53a35c09d52635", "statusQueryGetUri": "https://{host}/admin/extensions/DurableTaskExtension/instances/f6893f25acf64df2ab53a35c09d52635?taskHub=SampleHubVS&connection=Storage&code={systemKey}", "sendEventPostUri": "https://{host}/admin/extensions/DurableTaskExtension/instances/f6893f25acf64df2ab53a35c09d52635/raiseEvent/{eventName}?taskHub=SampleHubVS&connection=Storage&code={systemKey}", "terminatePostUri": "https://{host}/admin/extensions/DurableTaskExtension/instances/f6893f25acf64df2ab53a35c09d52635/terminate?reason={text}&taskHub=SampleHubVS&connection=Storage&code={systemKey}"}
```

De **E3_Monitor** exemplaar wordt gestart en de huidige weersomstandigheden voor de aangevraagde locatie een query uitgevoerd. Als het weer uitgeschakeld is, wordt een functie van de activiteit voor het verzenden van een waarschuwing; anders wordt een timer ingesteld. Wanneer de timer verloopt, wordt de indeling wordt hervat.

U kunt zien van de orchestration-activiteit door te kijken naar de functie beschikbaar zijn in de Azure Functions-portal.

```
2018-03-01T01:14:41.649 Function started (Id=2d5fcadf-275b-4226-a174-f9f943c90cd1)
2018-03-01T01:14:42.741 Started orchestration with ID = '1608200bb2ce4b7face5fc3b8e674f2e'.
2018-03-01T01:14:42.780 Function completed (Success, Id=2d5fcadf-275b-4226-a174-f9f943c90cd1, Duration=1111ms)
2018-03-01T01:14:52.765 Function started (Id=b1b7eb4a-96d3-4f11-a0ff-893e08dd4cfb)
2018-03-01T01:14:52.890 Received monitor request. Location: Redmond, WA. Phone: +1425XXXXXXX.
2018-03-01T01:14:52.895 Instantiating monitor for Redmond, WA. Expires: 3/1/2018 7:14:52 AM.
2018-03-01T01:14:52.909 Checking current weather conditions for Redmond, WA at 3/1/2018 1:14:52 AM.
2018-03-01T01:14:52.954 Function completed (Success, Id=b1b7eb4a-96d3-4f11-a0ff-893e08dd4cfb, Duration=189ms)
2018-03-01T01:14:53.226 Function started (Id=80a4cb26-c4be-46ba-85c8-ea0c6d07d859)
2018-03-01T01:14:53.808 Function completed (Success, Id=80a4cb26-c4be-46ba-85c8-ea0c6d07d859, Duration=582ms)
2018-03-01T01:14:53.967 Function started (Id=561d0c78-ee6e-46cb-b6db-39ef639c9a2c)
2018-03-01T01:14:53.996 Next check for Redmond, WA at 3/1/2018 1:44:53 AM.
2018-03-01T01:14:54.030 Function completed (Success, Id=561d0c78-ee6e-46cb-b6db-39ef639c9a2c, Duration=62ms)
```

De indeling wordt [beëindigen](durable-functions-instance-management.md#terminating-instances) zodra de time-out bereikt of wissen is skies worden gedetecteerd. U kunt ook `TerminateAsync` (.NET) of `terminate` (JavaScript) binnen een andere functie of aanroepen van de **terminatePostUri** HTTP POST-webhook waarnaar wordt verwezen in het 202-antwoord vervangen hierboven `{text}` met de reden voor beëindigen:

```
POST https://{host}/admin/extensions/DurableTaskExtension/instances/f6893f25acf64df2ab53a35c09d52635/terminate?reason=Because&taskHub=SampleHubVS&connection=Storage&code={systemKey}
```

## <a name="visual-studio-sample-code"></a>Visual Studio-voorbeeldcode

Hier volgt de indeling als één C#-bestand in een Visual Studio-project:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/Monitor.cs)]

## <a name="next-steps"></a>Volgende stappen

In dit voorbeeld is aangetoond duurzame functies gebruiken om de status van een externe bron te controleren met behulp van [duurzame timers](durable-functions-timers.md) en voorwaardelijke logica. Het volgende voorbeeld ziet u hoe u externe gebeurtenissen en [duurzame timers](durable-functions-timers.md) voor het afhandelen van menselijke tussenkomst.

> [!div class="nextstepaction"]
> [Het voorbeeld menselijke tussenkomst uitvoeren](durable-functions-phone-verification.md)
