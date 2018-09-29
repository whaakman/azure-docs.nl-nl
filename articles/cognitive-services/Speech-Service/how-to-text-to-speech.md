---
title: Tekst naar spraak met behulp van spraakservices gebruiken
description: Informatie over het gebruik van tekst naar spraak in de Speech-service.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 09/08/2018
ms.author: v-jerkin
ms.openlocfilehash: 5822c313cf0fb5848726cf85c46cda2a3a408264
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/28/2018
ms.locfileid: "47434616"
---
# <a name="use-text-to-speech-in-speech-service"></a>'Tekst naar spraak' in de spraak-service gebruiken

De spraak-service biedt de functionaliteit van tekst naar spraak via een eenvoudige HTTP-aanvraag. U `POST` de tekst die moet worden gesproken naar het juiste eindpunt en de service retourneert een geluidsbestand (`.wav`) spraak die worden gemaakt. Uw toepassing kunt vervolgens deze audio gebruiken, zoals het leuk vinden.

Aanvraag voor de hoofdtekst van het bericht voor tekst naar spraak mogelijk tekst zonder opmaak (ASCII of UTF8) of een [SSML](speech-synthesis-markup.md) document. Tekst zonder opmaak aanvragen worden met een standaardstem gesproken. In de meeste gevallen die u wilt gebruiken een SSML-instantie. De HTTP-aanvraag moet bevatten een [autorisatie](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis#authentication) token.

De regionale tekst naar spraak-eindpunten worden hier weergegeven. Gebruik een geschikt is voor uw abonnement.

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-text-to-speech.md)]

## <a name="specify-a-voice"></a>Geef een stem

Als u wilt een stem opgeven, gebruikt u de `<voice>` [SSML](speech-synthesis-markup.md) tag. Bijvoorbeeld:

```xml
<speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
  <voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'>
    Hello, world!
  </voice>
</speak>
```

Zie [tekst naar spraak stemmen](language-support.md#text-to-speech) voor een lijst van de beschikbare stemmen en hun namen.

## <a name="make-a-request"></a>Een aanvraag doen

Een tekst naar spraak HTTP-aanvraag wordt gedaan in de POST-modus met de tekst op die in de hoofdtekst van de aanvraag worden gesproken. De maximale lengte van de hoofdtekst van de HTTP-aanvraag is 1024 tekens. De aanvraag moet de volgende headers hebben:

Koptekst|Waarden|Opmerkingen
-|-|-
|`Content-Type` | `application/ssml+xml` | De indeling van de ingevoerde tekst.
|`X-Microsoft-OutputFormat`|     `raw-16khz-16bit-mono-pcm`<br>`riff-16khz-16bit-mono-pcm`<br>`raw-8khz-8bit-mono-mulaw`<br>`riff-8khz-8bit-mono-mulaw`<br>`audio-16khz-128kbitrate-mono-mp3`<br>`audio-16khz-64kbitrate-mono-mp3`<br>`audio-16khz-32kbitrate-mono-mp3`<br>`raw-24khz-16bit-mono-pcm`<br>`riff-24khz-16bit-mono-pcm`<br>`audio-24khz-160kbitrate-mono-mp3`<br>`audio-24khz-96kbitrate-mono-mp3`<br>`audio-24khz-48kbitrate-mono-mp3` | De uitvoer audio-indeling.
|`User-Agent`   |De naam van de toepassing | De toepassingsnaam is vereist en moet minder dan 255 tekens.
| `Authorization`   | Het verificatietoken is opgehaald door middel van uw abonnementssleutel voor de service voor beveiligingstokens. Elke token is geldig gedurende tien minuten. Zie [REST-API's: verificatie](rest-apis.md#authentication).

> [!NOTE]
> Als uw geselecteerde spraak- en de indeling van uitvoer hebt verschillende bitsnelheden, de audio nieuw voorbeeld wordt gemaakt zo nodig.

Hieronder ziet u een voorbeeld van een aanvraag.

```xml
POST /cognitiveservices/v1
HTTP/1.1
Host: westus.tts.speech.microsoft.com
X-Microsoft-OutputFormat: riff-24khz-16bit-mono-pcm
Content-Type: application/ssml+xml
User-Agent: Test TTS application
Authorization: (authorization token)

<speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'>
    Hello, world!
</voice> </speak>
```

De hoofdtekst van het antwoord met de status van 200 bevat audio in de indeling van de opgegeven uitvoer.

```
HTTP/1.1 200 OK
Content-Length: XXX
Content-Type: audio/x-wav

Response audio payload
```

Als er een fout optreedt, worden de onderstaande statuscodes gebruikt. De hoofdtekst van het antwoord voor de fout bevat ook een beschrijving van het probleem.

|Code|Beschrijving|Probleem|
|-|-|-|
400 |Onjuiste aanvraag |Er ontbreekt een vereiste parameter ontbreekt, is leeg of null zijn. Of de waarde die wordt doorgegeven aan een vereiste of optionele parameter is ongeldig. Een veelvoorkomend probleem is een header die te lang is.
401|Niet geautoriseerd |De aanvraag is niet gemachtigd. Controleer of dat uw abonnement of token geldig is.
413|Aanvraagentiteit te groot|De invoer SSML is te groot of bevat meer dan 3 `<voice>` elementen.
429|Te veel aanvragen|U hebt het quotum of het aantal aanvragen dat is toegestaan voor uw abonnement overschreden.
|502|Ongeldige gateway    | Netwerk- of serverzijde probleem. Kan ook duiden op ongeldige kopteksten.

Zie voor meer informatie over de tekst naar spraak REST-API, [REST-API's](rest-apis.md#text-to-speech).

## <a name="next-steps"></a>Volgende stappen

- [Uw proefabonnement voor Speech ophalen](https://azure.microsoft.com/try/cognitive-services/)
- [Water in C++](quickstart-cpp-windows.md)
- [Water in C#](quickstart-csharp-dotnet-windows.md)
- [Water in Java](quickstart-java-android.md)
