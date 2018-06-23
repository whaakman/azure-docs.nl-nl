---
title: Gebruik Text to Speech met behulp van spraakservices | Microsoft Docs
description: Informatie over het gebruik van tekst naar spraak gebruiken in de service spraak.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: 1ed2ee73b32f71d2e1ca34c6de9d1cb2649d7f0c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345828"
---
# <a name="use-text-to-speech-in-speech-service"></a>Gebruik 'Text to Speech' in spraak-service

De service spraak biedt functionaliteit voor tekst-naar-spraak via een eenvoudige HTTP-aanvraag. U de tekst die moet worden gesproken eindpunt van de juiste plaatsen en de service retourneert een audiobestand (`.wav`) spraak die worden gemaakt. Uw toepassing deze audio kan vervolgens worden gebruikt als het graag.

De hoofdtekst van de POST-aanvragen voor tekst-naar-spraak mogelijk tekst zonder opmaak (ASCII of UTF8) of een [SSML](speech-synthesis-markup.md) document. Tekst zonder opmaak verzoeken worden met een standaardstem gesproken. In de meeste gevallen die u wilt gebruiken een SSML-instantie. De HTTP-aanvraag moet een verificatietoken bevatten. 

Hier worden de regionale Text to Speech-eindpunten weergegeven. Gebruik een geschikt is voor uw abonnement.

Gebied| Eindpunt
-|-
VS - westen| `https://westus.tts.speech.microsoft.com/cognitiveservices/v1`
Azië - oost| `https://eastasia.tts.speech.microsoft.com/cognitiveservices/v1`
Europa - noord| `https://northeurope.tts.speech.microsoft.com/cognitiveservices/v1`

> [!NOTE]
> Als u een aangepaste gesproken lettertype hebt gemaakt, gebruikt u het eindpunt dat u hebt gemaakt voor het in plaats van de bovenstaande waarden.

## <a name="specify-a-voice"></a>Geef een stem

Gebruik het opgeven van een stem de `<voice>` [SSML](speech-synthesis-markup.md) label. Bijvoorbeeld:

```xml
<speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
  <voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'>
    Hello, world!
  </voice>
</speak>
```

Zie [Text to Speech stemmen](supported-languages.md#text-to-speech) voor een lijst met beschikbare stemmen en hun namen.

## <a name="make-a-request"></a>Maak een aanvraag

Een Text to Speech HTTP-aanvraag is aangebracht in de POST-modus met de tekst die in de hoofdtekst van de aanvraag worden gesproken. De maximale lengte van het hoofdgedeelte van de HTTP-aanvraag is 1024 tekens. De aanvraag moet de volgende headers hebben: 

Koptekst|Waarden|Opmerkingen
-|-|-
|`Content-Type` | `application/ssml+xml` | De ingevoerde tekst-indeling.
|`X-Microsoft-OutputFormat`|     `raw-16khz-16bit-mono-pcm`<br>`audio-16khz-16kbps-mono-siren`<br>`riff-16khz-16kbps-mono-siren`<br>`riff-16khz-16bit-mono-pcm`<br>`audio-16khz-128kbitrate-mono-mp3`<br>`audio-16khz-64kbitrate-mono-mp3`<br>`audio-16khz-32kbitrate-mono-mp3`<br>`raw-24khz-16bit-mono-pcm`<br>`riff-24khz-16bit-mono-pcm`<br>`audio-24khz-160kbitrate-mono-mp3`<br>`audio-24khz-96kbitrate-mono-mp3`<br>`audio-24khz-48kbitrate-mono-mp3` | De uitvoer audio-indeling.
|`User-Agent`   |Toepassingsnaam | Naam van de toepassing is vereist en moet minder dan 255 tekens.
| `Authorization`   | Verificatietoken is verkregen in de vorm van uw abonnementssleutel voor de service voor beveiligingstokens. Elke token is tien minuten geldig. Zie [REST-API's: verificatie](rest-apis.md#authentication).

> [!NOTE]
> Als uw geselecteerde stem en de indeling van uitvoer hebt verschillende bitsnelheden, de audio nieuw voorbeeld wordt gemaakt als nodig. 24khz stemmen bieden geen ondersteuning voor `audio-16khz-16kbps-mono-siren` en `riff-16khz-16kbps-mono-siren` uitvoerindelingen. 

Een voorbeeld van een aanvraag wordt hieronder weergegeven.

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

De hoofdtekst van de reactie met de status van 200 bevat audio in de indeling van de opgegeven uitvoer.

```
HTTP/1.1 200 OK
Content-Length: XXX
Content-Type: audio/x-wav

Response audio payload
```

Als een fout optreedt, worden de onderstaande statuscodes gebruikt. De hoofdtekst van de reactie voor de fout bevat ook een beschrijving van het probleem.

|Code|Beschrijving|Probleem|
|-|-|-|
400 |Ongeldig verzoek |Er ontbreekt een vereiste parameter ontbreekt, is leeg of null. Of de waarde die is doorgegeven aan een vereiste of optionele parameter is ongeldig. Een veelvoorkomend probleem is een header die is te lang.
401|Niet-gemachtigd |De aanvraag is niet gemachtigd. Controleer of de abonnementssleutel van uw of token geldig is.
413|De entiteit van de aanvraag is te groot|De SSML-invoer is meer dan 1024 tekens.
|502|Ongeldige gateway    | Netwerk- of -serverzijde probleem. Kan ook duiden op ongeldige headers.

Zie voor meer informatie over de tekst die moet spraak REST-API's [REST-API's](rest-apis.md#text-to-speech).

## <a name="next-steps"></a>Volgende stappen

- [Uw proefabonnement spraak ophalen](https://azure.microsoft.com/try/cognitive-services/)
- [Herkent spraak in C#](quickstart-csharp-windows.md)
