---
title: De monitoren in duurzame functies - Azure
description: Informatie over het implementeren van een statusmonitor met de extensie duurzame functies voor Azure Functions.
services: functions
author: kashimiz
manager: cfowler
editor: ''
tags: ''
keywords: ''
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/19/2018
ms.author: azfuncdf
ms.openlocfilehash: 7e520429e5f5e219e05a77eb4ca18d0d6b6b3977
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="monitor-scenario-in-durable-functions---weather-watcher-sample"></a>Monitor-scenario in duurzame functies - weer watcher-voorbeeld

Het patroon monitor verwijst naar een flexibel *terugkerende* proces in een werkstroom - bijvoorbeeld polling totdat aan bepaalde voorwaarden wordt voldaan. Dit artikel wordt uitgelegd voor een voorbeeldtoepassing die gebruikmaakt van [duurzame functies](durable-functions-overview.md) voor het implementeren van bewaking.

## <a name="prerequisites"></a>Vereisten

* [Installeren van functies duurzame](durable-functions-install.md).
* Voltooi de [Hello Sequence](durable-functions-sequence.md) scenario.

## <a name="scenario-overview"></a>Overzicht van scenario's

Dit voorbeeld controleert een locatie huidige weer voorwaarden en een gebruiker door SMS waarschuwingen wanneer de skies duidelijk zijn. U kunt een reguliere timer geactiveerd-functie voor het controleren van het weer en waarschuwingen verzenden. Een probleem met deze methode is echter **levensduurbeheer**. Als er slechts één waarschuwing moet worden verzonden, wordt de monitor moet zelf uitschakelen na wissen weer wordt gedetecteerd. Het patroon bewaking kunt beëindigen eigen uitvoering, onder andere voordelen:

* Monitors die worden uitgevoerd op intervallen, is niet gepland: een timertrigger *wordt uitgevoerd* elk uur; een monitor *wacht* één uur tussen acties. Bewerkingen van een monitor wordt niet overlappen tenzij opgegeven, wat kan zijn belangrijk voor langlopende taken worden uitgevoerd.
* Monitors kunnen dynamische intervallen hebben: de wachttijd op basis van een bepaalde voorwaarde kunt wijzigen.
* Monitors kunnen worden beëindigd wanneer een bepaalde voorwaarde wordt voldaan, of worden afgesloten met een ander proces.
* Monitors kunnen duren voordat de parameters. Het voorbeeld toont hoe hetzelfde proces weer bewaking aan een opgegeven locatie en telefoonnummer kan worden toegepast.
* Monitors zijn schaalbaar. Omdat elke monitor een exemplaar van de orchestration is, kunnen meerdere beeldschermen worden gemaakt zonder te hoeven maken van nieuwe functies of meer code definiëren.
* Monitors integreren in grotere werkstromen gemakkelijk. Een monitor kan bestaan uit één deel van een complexere orchestration-functie of een [onderliggende orchestration](https://docs.microsoft.com/en-us/azure/azure-functions/durable-functions-sub-orchestrations).

## <a name="configuring-twilio-integration"></a>Twilio-integratie configureren

[!INCLUDE [functions-twilio-integration](../../includes/functions-twilio-integration.md)]

## <a name="configuring-weather-underground-integration"></a>Weer ondergrondse integratie configureren

Dit voorbeeld wordt de ondergrondse API weer voor huidige weer voorwaarden voor een locatie controleren.

U moet eerst is een weer ondergrondse account. U kunt een gratis maken op [ https://www.wunderground.com/signup ](https://www.wunderground.com/signup). Zodra u een account hebt, moet u een API-sleutel te verkrijgen. U kunt dit doen in via [ https://www.wunderground.com/weather/api ](https://www.wunderground.com/weather/api), en vervolgens de instellingen van de sleutel te selecteren. Het plan Stratus Developer is gratis en voldoende zijn voor het uitvoeren van dit voorbeeld.

Zodra u een API-sleutel hebt, voeg de volgende **app-instelling** aan uw app functie.

| De naam van de App-instelling | Waardebeschrijving |
| - | - |
| **WeatherUndergroundApiKey**  | Uw weer ondergrondse API-sleutel. |

## <a name="the-functions"></a>De functies

Dit artikel wordt uitgelegd dat de volgende functies in de voorbeeld-app:

* `E3_Monitor`: Er is een orchestrator-functie die aanroept `E3_GetIsClear` regelmatig. Roept `E3_SendGoodWeatherAlert` als `E3_GetIsClear` ' true ' geretourneerd.
* `E3_GetIsClear`: Er is een functie van het type activiteit waarmee wordt gecontroleerd of de huidige voorwaarden weer voor een locatie.
* `E3_SendGoodWeatherAlert`: Er is een functie van het type activiteit dat een SMS-bericht via Twilio verzendt.

De volgende secties worden de configuratie en code die worden gebruikt voor het C#-scripts. De code voor het ontwikkelen van Visual Studio wordt weergegeven aan het einde van het artikel.
 
## <a name="the-weather-monitoring-orchestration-visual-studio-code-and-azure-portal-sample-code"></a>De bewaking van orchestration (Visual Studio Code en de Azure portal voorbeeldcode) weer

De **E3_Monitor** functie maakt gebruik van de standaard *function.json* voor orchestrator-functies.

[!code-json[Main](~/samples-durable-functions/samples/csx/E3_Monitor/function.json)]

Dit is de code die de functie implementeert:

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E3_Monitor/run.csx)]

Deze orchestrator-functie worden de volgende acties uitgevoerd:

1. Haalt de **MonitorRequest** die bestaan uit de *locatie* om te controleren en de *telefoonnummer* dat deze wordt een SMS om melding te verzenden.
2. Bepaalt de verlooptijd van de monitor. Het voorbeeld wordt de waarde vastgelegde als beknopt alternatief bevat.
3. Aanroepen **E3_GetIsClear** om te bepalen of er duidelijk skies beschikbaar op de opgegeven locatie zijn.
4. Als het weer ingeschakeld is, roept **E3_SendGoodWeatherAlert** een SMS-melding te verzenden naar het aangevraagde telefoonnummer.
5. Maakt een duurzame timer voor het hervatten van de orchestration op de volgende pollinginterval. Het voorbeeld wordt de waarde vastgelegde als beknopt alternatief bevat.
6. Wordt verder verwerkt totdat de [CurrentUtcDateTime](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CurrentUtcDateTime) geeft de verlooptijd van de monitor of een SMS-bericht verzonden.

Meerdere exemplaren van orchestrator kunnen tegelijkertijd worden uitgevoerd door te sturen meerdere **MonitorRequests**. De locatie voor het bewaken van en het telefoonnummer op dat een SMS-bericht te verzenden kunnen worden opgegeven.

## <a name="strongly-typed-data-transfer"></a>Sterk getypeerde gegevensoverdracht

Vereist verschillende soorten gegevens, zodat de orchestrator [POCO objecten gedeeld](functions-reference-csharp.md#reusing-csx-code) worden gebruikt voor gegevensoverdracht sterk getypeerde: [!code-csharp[Main](~/samples-durable-functions/samples/csx/shared/MonitorRequest.csx)]

[!code-csharp[Main](~/samples-durable-functions/samples/csx/shared/Location.csx)]

## <a name="helper-activity-functions"></a>Activiteit hulpfuncties

Zoals met andere voorbeelden zijn de activiteit hulpfuncties reguliere functies die gebruikmaken van de `activityTrigger` binding activeren. De **E3_GetIsClear** functie opgehaald van de huidige weer voorwaarden met de ondergrondse API weer en wordt bepaald of de lucht wissen. De *function.json* wordt als volgt gedefinieerd:

[!code-json[Main](~/samples-durable-functions/samples/csx/E3_GetIsClear/function.json)]

En hier is de implementatie. Als de POCOs gebruikt voor gegevensoverdracht, logica voor het afhandelen van de API aanroepen en parseren van het antwoord JSON beschouwing in een gedeelde-klasse. U vindt deze als onderdeel van de [Visual Studio voorbeeldcode](#run-the-sample).

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E3_GetIsClear/run.csx)]

De **E3_SendGoodWeatherAlert** functie maakt gebruik van de binding Twilio een SMS-bericht om de eindgebruiker te informeren dat het een goed moment voor een stapsgewijze beschrijving is te verzenden. De *function.json* is eenvoudig:

[!code-json[Main](~/samples-durable-functions/samples/csx/E3_SendGoodWeatherAlert/function.json)]

En dit is de code die door de SMS-bericht verzonden:

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E3_SendGoodWeatherAlert/run.csx)]

## <a name="run-the-sample"></a>De voorbeeldtoepassing uitvoeren

De HTTP-geactiveerde functies opgenomen in de steekproef gebruikt, kunt u de orchestration starten door de volgende HTTP POST-aanvraag te verzenden:

```
POST https://{host}/orchestrators/E3_Monitor
Content-Length: 77
Content-Type: application/json

{ "Location": { "City": "Redmond", "State": "WA" }, "Phone": "+1425XXXXXXX" }
```
```
HTTP/1.1 202 Accepted
Content-Type: application/json; charset=utf-8
Location: https://{host}/admin/extensions/DurableTaskExtension/instances/f6893f25acf64df2ab53a35c09d52635?taskHub=SampleHubVS&connection=Storage&code={SystemKey}
RetryAfter: 10

{"id": "f6893f25acf64df2ab53a35c09d52635", "statusQueryGetUri": "https://{host}/admin/extensions/DurableTaskExtension/instances/f6893f25acf64df2ab53a35c09d52635?taskHub=SampleHubVS&connection=Storage&code={systemKey}", "sendEventPostUri": "https://{host}/admin/extensions/DurableTaskExtension/instances/f6893f25acf64df2ab53a35c09d52635/raiseEvent/{eventName}?taskHub=SampleHubVS&connection=Storage&code={systemKey}", "terminatePostUri": "https://{host}/admin/extensions/DurableTaskExtension/instances/f6893f25acf64df2ab53a35c09d52635/terminate?reason={text}&taskHub=SampleHubVS&connection=Storage&code={systemKey}"}
```

De **E3_Monitor** exemplaar wordt gestart en query's van de huidige weer voorwaarden voor de opgegeven locatie. Als het weer ingeschakeld is, ontvangt een functie van de activiteit voor het verzenden van een waarschuwing; anders wordt een timer ingesteld. Als de timer verloopt, wordt de orchestration hervat.

U ziet de orchestration-activiteit door te kijken naar de functie wordt geregistreerd in de Azure Functions-portal.

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

De orchestration wordt [beëindigen](durable-functions-instance-management.md#terminating-instances) zodra de time-out bereikt of schakel is skies worden gedetecteerd. U kunt ook `TerminateAsync` binnen een ander functioneren of aanroepen de **terminatePostUri** HTTP POST-webhook waarnaar wordt verwezen in het antwoord 202 hierboven vervangen `{text}` met de reden voor beëindiging:

```
POST https://{host}/admin/extensions/DurableTaskExtension/instances/f6893f25acf64df2ab53a35c09d52635/terminate?reason=Because&taskHub=SampleHubVS&connection=Storage&code={systemKey}
```

## <a name="visual-studio-sample-code"></a>Voorbeeldcode voor Visual Studio

Dit is de orchestration als één C#-bestand in Visual Studio-project:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/Monitor.cs)]

## <a name="next-steps"></a>Volgende stappen

Dit voorbeeld heeft aangetoond duurzame functies gebruiken om een externe bron status te controleren met behulp van [duurzame timers](durable-functions-timers.md) en voorwaardelijke logica. Het volgende voorbeeld laat zien hoe u externe gebeurtenissen en [duurzame timers](durable-functions-timers.md) voor het afhandelen van menselijke tussenkomst.

> [!div class="nextstepaction"]
> [De menselijke tussenkomst-voorbeeld uitvoeren](durable-functions-phone-verification.md)
