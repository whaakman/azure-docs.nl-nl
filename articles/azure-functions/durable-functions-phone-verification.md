---
title: Menselijke tussenkomst en time-outs in duurzame functies - Azure
description: Leer hoe u voor het afhandelen van menselijke tussenkomst en time-outs in de extensie duurzame functies voor Azure Functions.
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
ms.date: 07/11/2018
ms.author: azfuncdf
ms.openlocfilehash: a62baf64e35dfad55f76138e2f1aaef65dd434be
ms.sourcegitcommit: 04fc1781fe897ed1c21765865b73f941287e222f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39036302"
---
# <a name="human-interaction-in-durable-functions---phone-verification-sample"></a>Menselijke tussenkomst in duurzame functies - voorbeeld van de telefoon-verificatie

In dit voorbeeld wordt gedemonstreerd hoe u een [duurzame functies](durable-functions-overview.md) orchestration dat betrekking heeft op menselijke tussenkomst. Wanneer een echte persoon betrokken bij een geautomatiseerd proces is, is het proces moet kunnen verzenden van meldingen naar de persoon en asynchroon antwoord ontvangen. Ook moeten zijn toegestaan voor de mogelijkheid dat de persoon die niet beschikbaar is. (Deze laatste gedeelte is waar time-outs steeds belangrijker.)

In dit voorbeeld implementeert een telefoonnummer op basis van SMS-verificatiesysteem. Deze typen stromen worden vaak gebruikt bij het controleren van het telefoonnummer van een klant of voor multi-factor authentication (MFA). Dit is een voorbeeld van een krachtige, omdat de volledige implementatie wordt uitgevoerd met behulp van een paar kleine functies. Er zijn geen externe gegevensarchief, zoals een database is vereist.

## <a name="prerequisites"></a>Vereisten

* [Duurzame functies installeren](durable-functions-install.md).
* Voltooi de [Hello reeks](durable-functions-sequence.md) scenario.

## <a name="scenario-overview"></a>Overzicht van scenario's

Verificatie via de telefoon wordt gebruikt om te controleren dat eindgebruikers van uw toepassing niet spammers zijn en dat ze zijn wie ze beweren te zijn. Multi-factor authentication is een algemene use-case voor gebruikersaccounts beveiligen tegen hackers. De uitdaging bij de implementatie van uw eigen verificatie via de telefoon is dat een **stateful interactie** met een mens. Een eindgebruiker wordt meestal verzorgd door code (bijvoorbeeld, een getal van 4 cijfers) en moet reageren **in een redelijk tijdsbestek**.

Gewone Azure Functions zijn staatloos (zoals zijn veel andere cloudeindpunten op andere platforms), zodat deze typen van interacties die betrekking hebben op expliciet beheer extern in een database of enige andere permanente Statusopslag. Bovendien moet de interactie worden verdeeld in meerdere functies die gecoördineerd samen worden kunnen. Bijvoorbeeld, moet u ten minste één functie voor het bepalen van een code, ergens opslaan en deze worden verzonden naar het telefoonnummer van de gebruiker. Daarnaast moet u ten minste één andere functie op een reactie ontvangen van de gebruiker en wijs deze aanpakt toe terug naar de oorspronkelijke aanroep van de functie om de code-validatie. Er is een time-out is ook een belangrijk aspect om beveiliging te waarborgen. Dit kan relatief complex Ga snel aan.

De complexiteit van dit scenario is aanzienlijk verminderd wanneer u duurzame functies gebruikt. Zoals u in dit voorbeeld ziet, kan een orchestrator-functie de stateful interactie eenvoudig en beheren zonder tussenkomst van een externe gegevensarchieven. Omdat de orchestrator-functies zijn *duurzame*, deze interactieve stromen ook uiterst betrouwbaar zijn.

## <a name="configuring-twilio-integration"></a>Twilio-integratie configureren

[!INCLUDE [functions-twilio-integration](../../includes/functions-twilio-integration.md)]

## <a name="the-functions"></a>De functies

Dit artikel helpt bij de volgende functies in de voorbeeld-app:

* **E4_SmsPhoneVerification**
* **E4_SendSmsChallenge**

De volgende secties worden de configuratie en de code die worden gebruikt voor C#-scripts en JavaScript. De code voor het ontwikkelen van Visual Studio wordt weergegeven aan het einde van het artikel.
 
## <a name="the-sms-verification-orchestration-visual-studio-code-and-azure-portal-sample-code"></a>De SMS-verificatie-indeling (Visual Studio Code en Azure portal voorbeeldcode) 

De **E4_SmsPhoneVerification** functie gebruikmaakt van de standaard *function.json* voor orchestrator-functies.

[!code-json[Main](~/samples-durable-functions/samples/csx/E4_SmsPhoneVerification/function.json)]

Dit is de code die de functie implementeert:

### <a name="c"></a>C#

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E4_SmsPhoneVerification/run.csx)]

### <a name="javascript-functions-v2-only"></a>JavaScript (alleen functies v2)

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E4_SmsPhoneVerification/index.js)]

Eenmaal is gestart, is deze functie orchestrator doet het volgende:

1. Een telefoonnummer waarop deze wordt opgehaald *verzenden* de SMS-bericht.
2. Aanroepen **E4_SendSmsChallenge** voor het verzenden van een SMS-bericht naar de gebruiker en retourneert de uitdaging van verwachte 4-cijferige code back.
3. Maakt een duurzame timer die triggers 90 seconden van de huidige tijd.
4. Wacht parallel met de timer voor een **SmsChallengeResponse** gebeurtenis van de gebruiker.

De gebruiker ontvangt een SMS-bericht met een code van vier cijfers. Ze hebben 90 seconden te verzenden die dezelfde 4-cijferige code naar het exemplaar van de orchestrator-functie om de verificatieproces te voltooien. Als ze een verkeerde code indient, krijgen ze een extra drie pogingen zodat deze direct (in het venster met dezelfde 90 seconden).

> [!NOTE]
> Kan niet liggen is de eerste, maar deze orchestrator-functie is volledig deterministisch. Dit komt doordat de `CurrentUtcDateTime` eigenschap wordt gebruikt voor het berekenen van de verlooptijd van de timer en deze eigenschap retourneert de dezelfde waarde op elke herhaling op dit moment in de orchestrator-code. Dit is belangrijk om ervoor te zorgen dat hetzelfde `winner` resultaat is van een voor elke herhaalde aanroep naar `Task.WhenAny`.

> [!WARNING]
> Het is belangrijk om [annuleren timers](durable-functions-timers.md) als u niet meer nodig laten verlopen hebt, zoals in het voorbeeld hierboven wanneer een vraag-antwoord wordt geaccepteerd.

## <a name="send-the-sms-message"></a>De SMS-bericht verzenden

De **E4_SendSmsChallenge** -functie maakt gebruik van de Twilio-binding voor het verzenden van de SMS-bericht met de 4-cijferige code voor de eindgebruiker. De *function.json* wordt als volgt gedefinieerd:

[!code-json[Main](~/samples-durable-functions/samples/csx/E4_SendSmsChallenge/function.json)]

En hier is de code die de uitdaging van 4-cijferige code gegenereerd en wordt het SMS-bericht verzonden:

### <a name="c"></a>C#

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E4_SendSmsChallenge/run.csx)]

### <a name="javascript-functions-v2-only"></a>JavaScript (alleen functies v2)

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E4_SendSmsChallenge/index.js)]

Dit **E4_SendSmsChallenge** functie alleen wordt aangeroepen, zelfs als het proces vastloopt of opnieuw wordt afgespeeld. Dit is goed omdat u niet wilt dat de gebruiker aan meerdere SMS-berichten. De `challengeCode` waarde automatisch wordt vastgehouden, zodat de orchestrator-functie altijd weet wat de juiste code is geretourneerd.

## <a name="run-the-sample"></a>De voorbeeldtoepassing uitvoeren

Met behulp van de HTTP-geactiveerde functies opgenomen in het voorbeeld, kunt u de indeling starten door de volgende HTTP POST-aanvraag te verzenden:

```
POST http://{host}/orchestrators/E4_SmsPhoneVerification
Content-Length: 14
Content-Type: application/json

"+1425XXXXXXX"
```
```
HTTP/1.1 202 Accepted
Content-Length: 695
Content-Type: application/json; charset=utf-8
Location: http://{host}/admin/extensions/DurableTaskExtension/instances/741c65651d4c40cea29acdd5bb47baf1?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}

{"id":"741c65651d4c40cea29acdd5bb47baf1","statusQueryGetUri":"http://{host}/admin/extensions/DurableTaskExtension/instances/741c65651d4c40cea29acdd5bb47baf1?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}","sendEventPostUri":"http://{host}/admin/extensions/DurableTaskExtension/instances/741c65651d4c40cea29acdd5bb47baf1/raiseEvent/{eventName}?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}","terminatePostUri":"http://{host}/admin/extensions/DurableTaskExtension/instances/741c65651d4c40cea29acdd5bb47baf1/terminate?reason={text}&taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}"}
```

   > [!NOTE]
   > JavaScript orchestration starter functies kunnen niet op dit moment instantiebeheer URI's retourneren. Deze mogelijkheid wordt toegevoegd in een latere versie.

De orchestrator-functie ontvangt van het opgegeven telefoonnummer en verzendt onmiddellijk een SMS-bericht met een willekeurig gegenereerde 4-cijferige verificatiecode &mdash; bijvoorbeeld *2168*. De functie wordt vervolgens gewacht 90 seconden een reactie.

Te beantwoorden met de code, kunt u `RaiseEventAsync` binnen een andere functie of aanroepen van de **sendEventUrl** HTTP POST-webhook waarnaar wordt verwezen in het 202-antwoord vervangen hierboven `{eventName}` met de naam van de gebeurtenis `SmsChallengeResponse`:

```
POST http://{host}/admin/extensions/DurableTaskExtension/instances/741c65651d4c40cea29acdd5bb47baf1/raiseEvent/SmsChallengeResponse?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}
Content-Length: 4
Content-Type: application/json

2168
```

Als u deze verzenden voordat de timer verloopt, de indeling is voltooid en de `output` veld is ingesteld op `true`, die wijzen op een geslaagde verificatie.

```
GET http://{host}/admin/extensions/DurableTaskExtension/instances/741c65651d4c40cea29acdd5bb47baf1?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}
```
```
HTTP/1.1 200 OK
Content-Length: 144
Content-Type: application/json; charset=utf-8

{"runtimeStatus":"Completed","input":"+1425XXXXXXX","output":true,"createdTime":"2017-06-29T19:10:49Z","lastUpdatedTime":"2017-06-29T19:12:23Z"}
```

Als u toestaan dat de timer verloopt, of als u de verkeerde code vier keer invoert, kunt u query's uitvoeren voor de status en Zie een `false` orchestration functie uitvoer, die aangeeft dat de verificatie via de telefoon is mislukt.

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Content-Length: 145

{"runtimeStatus":"Completed","input":"+1425XXXXXXX","output":false,"createdTime":"2017-06-29T19:20:49Z","lastUpdatedTime":"2017-06-29T19:22:23Z"}
```

## <a name="visual-studio-sample-code"></a>Visual Studio-voorbeeldcode

Hier volgt de indeling als één C#-bestand in een Visual Studio-project:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/PhoneVerification.cs)]

## <a name="next-steps"></a>Volgende stappen

In dit voorbeeld is aangetoond dat enkele van de geavanceerde mogelijkheden van duurzame functies, met name `WaitForExternalEvent` en `CreateTimer`. U hebt gezien hoe deze kunnen worden gecombineerd met `Task.WaitAny` voor het implementeren van een betrouwbare time-systeem, wat vaak nuttig is voor interactie met echte mensen. U kunt meer informatie over het gebruik van duurzame functies door te lezen van een reeks artikelen die worden geboden door uitgebreide dekking van specifieke onderwerpen.

> [!div class="nextstepaction"]
> [Ga naar het eerste artikel in de reeks](durable-functions-bindings.md)
