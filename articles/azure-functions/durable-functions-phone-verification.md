---
title: Menselijke tussenkomst en time-outs in duurzame functies - Azure
description: Informatie over het afhandelen van menselijke tussenkomst en time-outs in de uitbreiding duurzame functies voor Azure Functions.
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
ms.openlocfilehash: 1763c63b37c5e6b326c3623dc058974f718ac990
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
# <a name="human-interaction-in-durable-functions---phone-verification-sample"></a>Menselijke tussenkomst in duurzame functies - Phone verificatie voorbeeld

Dit voorbeeld laat zien hoe bouwt een [duurzame functies](durable-functions-overview.md) orchestration waarbij een menselijke tussenkomst. Wanneer een echte persoon wordt gebruikt bij een geautomatiseerde proces zijn betrokken, is het proces moet kunnen meldingen verzenden naar de persoon en antwoorden asynchroon ontvangen. Het moet ook de mogelijkheid de mogelijkheid om de persoon is niet beschikbaar. (Dit laatste deel is waar time-outs worden steeds belangrijker.)

Dit voorbeeld implementeert een telefoon op basis van SMS-verificatiesysteem. Deze typen stromen worden vaak gebruikt bij het verifiëren van het telefoonnummer van een klant of voor multi-factor authentication (MFA). Dit is een voorbeeld van een krachtige omdat de volledige implementatie is uitgevoerd met behulp van een paar kleine functies. Er zijn geen externe gegevensarchief, zoals een database is vereist.

## <a name="prerequisites"></a>Vereisten

* Volg de instructies in [duurzame functies installeren](durable-functions-install.md) voor het instellen van het voorbeeld.
* In dit artikel wordt ervan uitgegaan dat u al hebt doorlopen de [Hello Sequence](durable-functions-sequence.md) voorbeeld scenario.

## <a name="scenario-overview"></a>Overzicht van scenario's

Verificatie via de telefoon wordt gebruikt om te controleren of de eindgebruikers van uw toepassing geen afzenders van spam hebben en dat ze zijn die zij beweren te zijn. Multi-factor authentication-server is een algemene gebruiksvoorbeeld voor gebruikersaccounts beveiligen tegen hackers. De uitdaging met de implementatie van uw eigen verificatie via de telefoon is dat een **stateful interactie** met fotograferen. De gebruiker wordt meestal verzorgd door code (bijvoorbeeld een 4 cijfers) en moet reageren **binnen redelijke tijd**.

Gewone Azure Functions staatloze zijn (zoals zijn veel andere cloud-eindpunten op andere platforms), zodat deze typen interacties eruit ziet expliciet beheren status extern in een database of enige andere permanente opslaan. Bovendien moet de interactie worden verdeeld in meerdere functies die gecoördineerd samen worden kunnen. Bijvoorbeeld, moet u ten minste één functie voor het kiezen van een code en verzending aan telefoonnummer van de gebruiker ergens permanent. Bovendien moet u ten minste één andere functie reactie ontvangen van de gebruiker en enigszins terug naar de oorspronkelijke functieaanroep hiervoor de codevalidatie toewijzen. Er is een time-out is ook een belangrijk aspect om beveiliging te waarborgen. Dit kan krijgen relatief complex vrij snel.

De complexiteit van dit scenario is aanzienlijk verminderd wanneer u duurzame functies gebruiken. Zoals u in dit voorbeeld ziet, kan een orchestrator-functie de stateful interactie heel eenvoudig en beheren zonder tussenkomst van een externe gegevensarchieven. Omdat de orchestrator-functies zijn *duurzame*, ook deze interactieve stromen maximaal betrouwbaar zijn.

## <a name="configuring-twilio-integration"></a>Twilio-integratie configureren

Dit voorbeeld ook met behulp van de [Twilio](https://www.twilio.com/) SMS-berichten verzenden naar een mobiele telefoon-service. Azure Functions heeft al ondersteuning voor Twilio via de [Twilio-binding](https://docs.microsoft.com/azure/azure-functions/functions-bindings-twilio), en het voorbeeld maakt gebruik van deze functie.

U moet eerst is een Twilio-account. Kunt u een gratis op https://www.twilio.com/try-twilio. Zodra u een account hebt, voeg de volgende drie **appinstellingen** aan uw app functie.

| De naam van de App-instelling | Waardebeschrijving |
| - | - |
| **TwilioAccountSid**  | De SID voor uw Twilio-account |
| **TwilioAuthToken**   | De Auth-token voor uw Twilio-account |
| **TwilioPhoneNumber** | Het telefoonnummer dat is gekoppeld aan uw Twilio-account. Dit wordt gebruikt om SMS-berichten te verzenden. |

## <a name="the-functions"></a>De functies

Dit artikel begeleidt u bij de volgende functies in de voorbeeld-app:

* **E4_SmsPhoneVerification**
* **E4_SendSmsChallenge**

De volgende secties worden de configuratie en code die worden gebruikt voor het ontwikkelen van Azure portal. De code voor het ontwikkelen van Visual Studio wordt weergegeven aan het einde van het artikel.
 
## <a name="the-sms-verification-orchestration-visual-studio-code-and-azure-portal-sample-code"></a>De SMS-verificatie orchestration (Visual Studio Code en de Azure portal voorbeeldcode) 

De **E4_SmsPhoneVerification** functie maakt gebruik van de standaard *function.json* voor orchestrator-functies.

[!code-json[Main](~/samples-durable-functions/samples/csx/E4_SmsPhoneVerification/function.json)]

Dit is de code die de functie implementeert:

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E4_SmsPhoneVerification/run.csx)]

Eenmaal gestart, is deze functie orchestrator doet het volgende:

1. Een telefoonnummer waarop deze wordt opgehaald *verzenden* de SMS-melding.
2. Aanroepen **E4_SendSmsChallenge** SMS-bericht te verzenden de gebruiker en geeft weer de uitdaging van verwachte 4-cijferige code.
3. Maakt een duurzame timer die triggers 90 seconden van de huidige tijd.
4. In combinatie met de timer wordt gewacht op een **SmsChallengeResponse** gebeurtenis van de gebruiker.

De gebruiker ontvangt een SMS-bericht met een code van vier cijfers. Ze hebben 90 seconden te verzenden die dezelfde 4-cijferige code terug naar het orchestrator-functie-exemplaar voor de verificatie te voltooien. Als ze een onjuiste code indienen, krijgen ze een extra drie probeert te krijgen (binnen hetzelfde 90 tweede venster).

> [!NOTE]
> Deze mogelijk niet duidelijk is op de eerste, maar deze orchestrator functie volledig deterministisch is. Dit komt doordat de `CurrentUtcDateTime` eigenschap wordt gebruikt voor het berekenen van de verlooptijd van de timer en deze eigenschap geeft dezelfde waarde op elke opnieuw afspelen op dit moment in de orchestrator-code. Dit is belangrijk om ervoor te zorgen dat hetzelfde `winner` resulteert vanuit elke herhaalde aanroep `Task.WhenAny`.

> [!WARNING]
> Het is belangrijk dat [annuleren timers met behulp van een CancellationTokenSource](durable-functions-timers.md) als u deze niet meer nodig om te verlopen, zoals in het voorbeeld hierboven wanneer een challenge-antwoord is geaccepteerd.

## <a name="send-the-sms-message"></a>De SMS-bericht verzenden

De **E4_SendSmsChallenge** functie maakt gebruik van de Twilio-binding voor het verzenden van de SMS-bericht met de 4-cijferige code voor de eindgebruiker. De *function.json* wordt als volgt gedefinieerd:

[!code-json[Main](~/samples-durable-functions/samples/csx/E4_SendSmsChallenge/function.json)]

En dit is de code die de uitdaging 4-cijferige code genereert en het SMS-bericht verzonden:

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E4_SendSmsChallenge/run.csx)]

Dit **E4_SendSmsChallenge** functie slechts opgehaald eenmaal aangeroepen, zelfs als het proces vastloopt cookies opgehaald. Dit is geschikt omdat u niet wilt dat de eindgebruiker ophalen van meerdere SMS-berichten. De `challengeCode` waarde automatisch wordt behouden, zodat de orchestrator-functie altijd weet wat de juiste code is geretourneerd.

## <a name="run-the-sample"></a>Het voorbeeld uitvoert

De HTTP-geactiveerde functies opgenomen in de steekproef gebruikt, kunt u de orchestration starten door de volgende HTTP POST-aanvraag te verzenden.

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

De orchestrator-functie ontvangt van het opgegeven telefoonnummer en verzendt onmiddellijk een SMS-bericht met een willekeurig gegenereerde 4-cijferige verificatiecode &mdash; bijvoorbeeld *2168*. De functie wacht vervolgens 90 seconden reageert.

Te beantwoorden met de code, kunt u `RaiseEventAsync` binnen een ander functioneren of aanroepen de **sendEventUrl** HTTP POST-webhook waarnaar wordt verwezen in het antwoord 202 hierboven vervangen `{eventName}` met de naam van de gebeurtenis `SmsChallengeResponse`:

```
POST http://{host}/admin/extensions/DurableTaskExtension/instances/741c65651d4c40cea29acdd5bb47baf1/raiseEvent/SmsChallengeResponse?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}
Content-Length: 4
Content-Type: application/json

2168
```

Als u dit verzendt voordat de timer verloopt, de orchestration is voltooid en de `output` veld is ingesteld op `true`, die wijzen op een geslaagde verificatie.

```
GET http://{host}/admin/extensions/DurableTaskExtension/instances/741c65651d4c40cea29acdd5bb47baf1?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}
```
```
HTTP/1.1 200 OK
Content-Length: 144
Content-Type: application/json; charset=utf-8

{"runtimeStatus":"Completed","input":"+1425XXXXXXX","output":true,"createdTime":"2017-06-29T19:10:49Z","lastUpdatedTime":"2017-06-29T19:12:23Z"}
```

Als u kunt de timer voor het verlopen of als u de verkeerde code vier keer invoert, kunt u zoeken naar de status en Zie een `false` orchestration functie uitvoer, die aangeeft dat de verificatie via de telefoon is mislukt.

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Content-Length: 145

{"runtimeStatus":"Completed","input":"+1425XXXXXXX","output":false,"createdTime":"2017-06-29T19:20:49Z","lastUpdatedTime":"2017-06-29T19:22:23Z"}
```

## <a name="visual-studio-sample-code"></a>Voorbeeldcode voor Visual Studio

Dit is de orchestration als één C#-bestand in Visual Studio-project:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/PhoneVerification.cs)]

## <a name="next-steps"></a>Volgende stappen

Dit voorbeeld heeft aangetoond dat sommige van de geavanceerde mogelijkheden van duurzame functies, met name `WaitForExternalEvent` en `CreateTimer`. U hebt gezien hoe deze kunnen worden gecombineerd met `Task.WaitAny` voor het implementeren van een betrouwbare time-out-systeem is het vaak nuttig voor interactie met echte mensen. U kunt meer informatie over het gebruik van duurzame functies door te lezen van een reeks van artikelen die gedetailleerde beschrijving van bepaalde onderwerpen bieden.

> [!div class="nextstepaction"]
> [Ga naar het eerste artikel in de reeks](durable-functions-bindings.md)
