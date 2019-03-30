---
title: Text to Speech API reference (REST) - spraakservices
titleSuffix: Azure Cognitive Services
description: Informatie over het gebruik van de Text to Speech REST-API. In dit artikel leert u over de opties voor autorisatie, opties voor query's, het structureren van een aanvraag en antwoord heeft ontvangen.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/26/2019
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: 71710cd940aad3a56dae6c19d4d52a5b141b3d80
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58660963"
---
# <a name="text-to-speech-rest-api"></a>Text to Speech REST-API

De spraak-Services kunt u [tekst converteren naar kunstmatige spraak](#convert-text-to-speech) en [ophalen van een lijst met ondersteunde stemmen](#get-a-list-of-voices) voor een regio met behulp van een set REST-API's. Elk beschikbaar eindpunt is gekoppeld aan een regio. Er is een abonnementssleutel voor het eindpunt/de regio die u van plan bent te gebruiken vereist.

De Text to Speech REST-API biedt ondersteuning voor neurale en standard-naar-spraak stemmen, die ondersteuning biedt voor een specifieke taal en een dialect, geïdentificeerd door de landinstelling.

* Zie voor een volledige lijst van stemmen, [taalondersteuning](language-support.md#text-to-speech).
* Zie voor meer informatie over regionale beschikbaarheid [regio's](regions.md#text-to-speech).

> [!IMPORTANT]
> Kosten variëren voor standaard, aangepaste en neurale stemmen. Zie voor meer informatie, [prijzen](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

Voordat u deze API gebruikt, begrijpt:

* De Text to Speech REST-API vereist autorisatie-header. Dit betekent dat u voltooien van de uitwisseling van een token moet voor toegang tot de service. Zie [Verificatie](#authentication) voor meer informatie.

[!INCLUDE [](../../../includes/cognitive-services-speech-service-rest-auth.md)]

## <a name="get-a-list-of-voices"></a>Een lijst van stemmen

De `voices/list` eindpunt kunt u een volledige lijst van stemmen voor een specifieke regio/het eindpunt.

### <a name="regions-and-endpoints"></a>Regio's en -eindpunten

| Regio | Eindpunt |
|--------|----------|
| Australië - oost | https://australiaeast.tts.speech.microsoft.com/cognitiveservices/voices/list |
| Brazilië - zuid | https://brazilsouth.tts.speech.microsoft.com/cognitiveservices/voices/list |
| Canada - midden | https://canadacentral.tts.speech.microsoft.com/cognitiveservices/voices/list |
| US - centraal | https://centralus.tts.speech.microsoft.com/cognitiveservices/voices/list |
| Azië - oost | https://eastasia.tts.speech.microsoft.com/cognitiveservices/voices/list |
| US - oost | https://eastus.tts.speech.microsoft.com/cognitiveservices/voices/list |
| US - oost 2 | https://eastus2.tts.speech.microsoft.com/cognitiveservices/voices/list |
| Frankrijk - centraal | https://francecentral.tts.speech.microsoft.com/cognitiveservices/voices/list |
| India - centraal | https://centralindia.tts.speech.microsoft.com/cognitiveservices/voices/list |
| Japan - oost | https://japaneast.tts.speech.microsoft.com/cognitiveservices/voices/list |
| Korea - centraal | https://koreacentral.tts.speech.microsoft.com/cognitiveservices/voices/list |
| US - noord-centraal | https://northcentralus.tts.speech.microsoft.com/cognitiveservices/voices/list |
| Europa - noord | https://northeurope.tts.speech.microsoft.com/cognitiveservices/voices/list |
| US - zuid-centraal | https://southcentralus.tts.speech.microsoft.com/cognitiveservices/voices/list |
| Azië - zuidoost | https://southeastasia.tts.speech.microsoft.com/cognitiveservices/voices/list |
| Verenigd Koninkrijk Zuid | https://uksouth.tts.speech.microsoft.com/cognitiveservices/voices/list |
| Europa -west | https://westeurope.tts.speech.microsoft.com/cognitiveservices/voices/list |
| US - west | https://westus.tts.speech.microsoft.com/cognitiveservices/voices/list |
| US - west 2 | https://westus2.tts.speech.microsoft.com/cognitiveservices/voices/list |

### <a name="request-headers"></a>Aanvraagheaders

Deze tabel bevat de vereiste en optionele headers voor Text to Speech-aanvragen.

| Header | Description | Vereiste / optioneel |
|--------|-------------|---------------------|
| `Authorization` | Een verificatietoken voorafgegaan door het woord `Bearer`. Zie [Verificatie](#authentication) voor meer informatie. | Vereist |

### <a name="request-body"></a>Aanvraagbody

Er is een instantie niet vereist voor `GET` aanvragen naar dit eindpunt.

### <a name="sample-request"></a>Voorbeeld van een aanvraag

Deze aanvraag is alleen vereist voor een autorisatie-header.

```http
GET /cognitiveservices/voices/list HTTP/1.1

Host: westus.tts.speech.microsoft.com
Authorization: Bearer [Base64 access_token]
```

### <a name="sample-response"></a>Voorbeeldantwoord

Deze reactie is ingekort ter illustratie van de structuur van een reactie.

> [!NOTE]
> Stem beschikbaarheid varieert per regio/het eindpunt.

```json
[
    {
        "Name": "Microsoft Server Speech Text to Speech Voice (ar-EG, Hoda)",
        "ShortName": "ar-EG-Hoda",
        "Gender": "Female",
        "Locale": "ar-EG"
    },
    {
        "Name": "Microsoft Server Speech Text to Speech Voice (ar-SA, Naayf)",
        "ShortName": "ar-SA-Naayf",
        "Gender": "Male",
        "Locale": "ar-SA"
    },
    {
        "Name": "Microsoft Server Speech Text to Speech Voice (bg-BG, Ivan)",
        "ShortName": "bg-BG-Ivan",
        "Gender": "Male",
        "Locale": "bg-BG"
    },
    {
        "Name": "Microsoft Server Speech Text to Speech Voice (ca-ES, HerenaRUS)",
        "ShortName": "ca-ES-HerenaRUS",
        "Gender": "Female",
        "Locale": "ca-ES"
    },
    {
        "Name": "Microsoft Server Speech Text to Speech Voice (cs-CZ, Jakub)",
        "ShortName": "cs-CZ-Jakub",
        "Gender": "Male",
        "Locale": "cs-CZ"
    },

    ...

]
```

### <a name="http-status-codes"></a>HTTP-statuscodes

De HTTP-statuscode voor elke reactie geeft aan dat het slagen of veelvoorkomende fouten.

| HTTP-statuscode | Description | Mogelijke oorzaak |
|------------------|-------------|-----------------|
| 200 | OK | De aanvraag is voltooid. |
| 400 | Onjuiste aanvraag | Er ontbreekt een vereiste parameter ontbreekt, is leeg of null zijn. Of de waarde die wordt doorgegeven aan een vereiste of optionele parameter is ongeldig. Een veelvoorkomend probleem is een header die te lang is. |
| 401 | Niet geautoriseerd | De aanvraag is niet gemachtigd. Controleer of dat uw abonnementssleutel of token geldig is en in de juiste regio. |
| 429 | Te veel aanvragen | U hebt het quotum of het aantal aanvragen dat is toegestaan voor uw abonnement overschreden. |
| 502 | Ongeldige gateway | Netwerk- of serverzijde probleem. Kan ook duiden op ongeldige kopteksten. |


## <a name="convert-text-to-speech"></a>Tekst naar spraak converteren

De `v1` eindpunt kunt u converteren met behulp van tekst naar spraak [spraak synthese Markup Language (SSML)](speech-synthesis-markup.md).

### <a name="regions-and-endpoints"></a>Regio's en -eindpunten

Deze regio's worden ondersteund voor tekst naar spraak met behulp van de REST-API. Zorg ervoor dat u het eindpunt dat overeenkomt met de regio van uw abonnement selecteert.

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-text-to-speech.md)]

### <a name="request-headers"></a>Aanvraagheaders

Deze tabel bevat de vereiste en optionele headers voor Text to Speech-aanvragen.

| Header | Description | Vereiste / optioneel |
|--------|-------------|---------------------|
| `Authorization` | Een verificatietoken voorafgegaan door het woord `Bearer`. Zie [Verificatie](#authentication) voor meer informatie. | Vereist |
| `Content-Type` | Hiermee geeft u het type inhoud voor de opgegeven tekst. Waarde geaccepteerd: `application/ssml+xml`. | Vereist |
| `X-Microsoft-OutputFormat` | Hiermee geeft u de indeling van de audio-uitvoer. Zie voor een volledige lijst met geaccepteerde waarden [audio-uitvoer](#audio-outputs). | Vereist |
| `User-Agent` | De naam van de toepassing. De opgegeven waarde moet minder dan 255 tekens. | Vereist |

### <a name="audio-outputs"></a>Audio-uitvoer

Dit is een lijst met ondersteunde audio-indelingen die worden verzonden in elke aanvraag als de `X-Microsoft-OutputFormat` header. Elk omvat een bitrate en type codering. De Speech Services biedt ondersteuning voor 24 KHz, 16 KHz en 8 KHz audio weergeeft.

|||
|-|-|
| `raw-16khz-16bit-mono-pcm` | `raw-8khz-8bit-mono-mulaw` |
| `riff-8khz-8bit-mono-alaw` | `riff-8khz-8bit-mono-mulaw` |
| `riff-16khz-16bit-mono-pcm` | `audio-16khz-128kbitrate-mono-mp3` |
| `audio-16khz-64kbitrate-mono-mp3` | `audio-16khz-32kbitrate-mono-mp3` |
| `raw-24khz-16bit-mono-pcm` | `riff-24khz-16bit-mono-pcm` |
| `audio-24khz-160kbitrate-mono-mp3` | `audio-24khz-96kbitrate-mono-mp3` |
| `audio-24khz-48kbitrate-mono-mp3` | |

> [!NOTE]
> Als uw geselecteerde spraak- en de indeling van uitvoer hebt verschillende bitsnelheden, de audio nieuw voorbeeld wordt gemaakt zo nodig. Echter, 24khz stemmen bieden geen ondersteuning voor `audio-16khz-16kbps-mono-siren` en `riff-16khz-16kbps-mono-siren` uitvoerindelingen.

### <a name="request-body"></a>Aanvraagbody

De hoofdtekst van elk `POST` aanvraag wordt verzonden als [spraak synthese Markup Language (SSML)](speech-synthesis-markup.md). SSML kunt u de spraak- en taal van de kunstmatige spraak geretourneerd door de Text to Speech-service kiezen. Zie voor een volledige lijst van ondersteunde stemmen, [taalondersteuning](language-support.md#text-to-speech).

> [!NOTE]
> Als u een aangepaste spraak, kan de hoofdtekst van een aanvraag kan worden verzonden als tekst zonder opmaak (ASCII- of UTF-8).

### <a name="sample-request"></a>Voorbeeld van een aanvraag

Deze HTTP-aanvraag maakt gebruik van SSML om op te geven van de spraak- en taal. De hoofdtekst van het niet langer zijn dan 1000 tekens.

```http
POST /cognitiveservices/v1 HTTP/1.1

X-Microsoft-OutputFormat: raw-16khz-16bit-mono-pcm
Content-Type: application/ssml+xml
Host: westus.tts.speech.microsoft.com
Content-Length: 225
Authorization: Bearer [Base64 access_token]

<speak version='1.0' xml:lang='en-US'><voice xml:lang='en-US' xml:gender='Female'
    name='Microsoft Server Speech Text to Speech Voice (en-US, ZiraRUS)'>
        Microsoft Speech Service Text-to-Speech API
</voice></speak>
```

Zie onze snelstartgidsen om snel voor taal-specifieke voorbeelden:

* [.NET Core, C#](quickstart-dotnet-text-to-speech.md)
* [Python](quickstart-python-text-to-speech.md)
* [Node.js](quickstart-nodejs-text-to-speech.md)

### <a name="http-status-codes"></a>HTTP-statuscodes

De HTTP-statuscode voor elke reactie geeft aan dat het slagen of veelvoorkomende fouten.

| HTTP-statuscode | Description | Mogelijke oorzaak |
|------------------|-------------|-----------------|
| 200 | OK | De aanvraag is uitgevoerd. de antwoordtekst is een geluidsbestand. |
| 400 | Onjuiste aanvraag | Er ontbreekt een vereiste parameter ontbreekt, is leeg of null zijn. Of de waarde die wordt doorgegeven aan een vereiste of optionele parameter is ongeldig. Een veelvoorkomend probleem is een header die te lang is. |
| 401 | Niet geautoriseerd | De aanvraag is niet gemachtigd. Controleer of dat uw abonnementssleutel of token geldig is en in de juiste regio. |
| 413 | Aanvraagentiteit te groot | De invoer SSML is langer dan 1024 tekens. |
| 429 | Te veel aanvragen | U hebt het quotum of het aantal aanvragen dat is toegestaan voor uw abonnement overschreden. |
| 502 | Ongeldige gateway | Netwerk- of serverzijde probleem. Kan ook duiden op ongeldige kopteksten. |

Als de HTTP-status `200 OK`, de hoofdtekst van het antwoord bevat een audio-bestand in de gewenste indeling. Dit bestand kan worden afgespeeld, zoals deze is overgedragen, in een buffer opgeslagen of opgeslagen in een bestand.

## <a name="next-steps"></a>Volgende stappen

- [Uw proefabonnement voor Speech ophalen](https://azure.microsoft.com/try/cognitive-services/)
- [Akoestische modellen aanpassen](how-to-customize-acoustic-models.md)
- [Taalmodellen aanpassen](how-to-customize-language-model.md)
