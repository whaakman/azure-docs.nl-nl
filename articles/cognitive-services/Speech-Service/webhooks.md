---
title: Webhooks-spraak service
titleSuffix: Azure Cognitive Services
description: Webhooks zijn HTTP-aanroepen die ideaal zijn voor het optimaliseren van uw oplossing bij het verwerken van langlopende processen, zoals het importeren, aanpassen, nauw keurig testen of transcripties van langlopende bestanden.
services: cognitive-services
author: PanosPeriorellis
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: panosper
ms.openlocfilehash: 3d07e540bf88c956f61b5d3b2a98702cad616985
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68558794"
---
# <a name="webhooks-for-speech-services"></a>Webhooks voor spraak Services

Webhooks zijn net als HTTP-Call backs waarmee uw toepassing gegevens van de spraak Services kan accepteren wanneer deze beschikbaar wordt. Met webhooks kunt u uw gebruik van de REST-Api's optimaliseren door te elimineren dat er voortdurend moet worden gereageerd op een reactie. In de volgende gedeelten leert u hoe u webhooks gebruikt met de spraak Services.

## <a name="supported-operations"></a>Ondersteunde bewerkingen

De spraak Services ondersteunen webhooks voor alle langlopende bewerkingen. Met elk van de onderstaande bewerkingen kan een HTTP-call back worden geactiveerd na voltooiing.

* DataImportCompletion
* ModelAdaptationCompletion
* AccuracyTestCompletion
* TranscriptionCompletion
* EndpointDeploymentCompletion
* EndpointDataCollectionCompletion

Vervolgens gaan we een webhook maken.

## <a name="create-a-webhook"></a>Een webhook maken

We gaan een webhook maken voor een offline-transcriptie. Het scenario: een gebruiker beschikt over een lang actief audio bestand dat ze asynchroon willen transcriberen met de batch transcriptie-API.

Webhooks kunnen worden gemaakt door een post-aanvraag naar https://\<Region\>. Cris.ai/API/speechtotext/v2.1/Transcriptions/hooks te maken.

Configuratie parameters voor de aanvraag worden opgegeven als JSON:

```json
{
  "configuration": {
    "url": "https://your.callback.url/goes/here",
    "secret": "<my_secret>"
  },
  "events": [
    "TranscriptionCompletion"
  ],
  "active": true,
  "name": "TranscriptionCompletionWebHook",
  "description": "This is a Webhook created to trigger an HTTP POST request when my audio file transcription is completed.",
  "properties": {
      "Active" : "True"
  }

}
```
Alle POST-aanvragen voor de batch-transcriptie- `name`API vereisen een. De `description` para `properties` meters en zijn optioneel.

De `Active` eigenschap wordt gebruikt om te scha kelen naar uw URL in-of uitschakelen zonder dat u de webhook-registratie hoeft te verwijderen en opnieuw te maken. Als u slechts één keer hoeft te bellen nadat het proces is voltooid, verwijdert u de webhook en schakelt u `Active` de eigenschap over naar onwaar.

Het gebeurtenis type `TranscriptionCompletion` wordt gegeven in de matrix Events. Er wordt teruggebeld naar uw eind punt wanneer een transcriptie wordt opgehaald in een Terminal`Succeeded` status `Failed`(of). De aanvraag bevat een `X-MicrosoftSpeechServices-Event` header met een van de geregistreerde gebeurtenis typen wanneer u terugkeert naar de geregistreerde URL. Er is één aanvraag per geregistreerd gebeurtenis type.

Er is één gebeurtenis type waarop u zich niet kunt abonneren. Het is het `Ping` gebeurtenis type. Er wordt een aanvraag met dit type verzonden naar de URL wanneer u klaar bent met het maken van een webhook bij het gebruik van de ping-URL (zie hieronder).  

In de configuratie is de `url` eigenschap vereist. POST-aanvragen worden verzonden naar deze URL. De `secret` wordt gebruikt voor het maken van een sha256-hash van de payload, met het geheim als een HMAC-sleutel. De hash wordt ingesteld als de `X-MicrosoftSpeechServices-Signature` header bij het aanroepen van de geregistreerde URL. Deze header is base64-gecodeerd.

In dit voor beeld ziet u hoe u een Payload C#kunt valideren met:

```csharp

private const string EventTypeHeaderName = "X-MicrosoftSpeechServices-Event";
private const string SignatureHeaderName = "X-MicrosoftSpeechServices-Signature";

[HttpPost]
public async Task<IActionResult> PostAsync([FromHeader(Name = EventTypeHeaderName)]WebHookEventType eventTypeHeader, [FromHeader(Name = SignatureHeaderName)]string signature)
{
    string body = string.Empty;
    using (var streamReader = new StreamReader(this.Request.Body))
    {
        body = await streamReader.ReadToEndAsync().ConfigureAwait(false);
        var secretBytes = Encoding.UTF8.GetBytes("my_secret");
        using (var hmacsha256 = new HMACSHA256(secretBytes))
        {
            var contentBytes = Encoding.UTF8.GetBytes(body);
            var contentHash = hmacsha256.ComputeHash(contentBytes);
            var storedHash = Convert.FromBase64String(signature);
            var validated = contentHash.SequenceEqual(storedHash);
        }
    }

    switch (eventTypeHeader)
    {
        case WebHookEventType.Ping:
            // Do your ping event related stuff here (or ignore this event)
            break;
        case WebHookEventType.TranscriptionCompletion:
            // Do your subscription related stuff here.
            break;
        default:
            break;
    }

    return this.Ok();
}

```
In dit code fragment wordt de `secret` code gedecodeerd en gevalideerd. U ziet ook dat het gebeurtenis type webhook is geswitcheerd. Er is momenteel één gebeurtenis per voltooide transcriptie. De code wordt vijf keer opnieuw geprobeerd voor elke gebeurtenis (met een vertraging van één seconde) voordat deze wordt opgeteld.

### <a name="other-webhook-operations"></a>Andere webhook-bewerkingen

Alle geregistreerde webhooks ophalen: TOEVOEGEN https://westus.cris.ai/api/speechtotext/v2.1/transcriptions/hooks

Een specifieke webhook ophalen: TOEVOEGEN https://westus.cris.ai/api/speechtotext/v2.1/transcriptions/hooks/:id

Eén specifieke webhook verwijderen: VERWIJDERD https://westus.cris.ai/api/speechtotext/v2.1/transcriptions/hooks/:id

> [!Note]
> In het bovenstaande voor beeld is de regio westus. Dit moet worden vervangen door de regio waar u uw speech Services-resource hebt gemaakt in de Azure Portal.

https://westus.cris.ai/api/speechtotext/v2.1/transcriptions/hooks/:id/ping Hoofd tekst: leeg

Verzendt een POST-aanvraag naar de geregistreerde URL. De aanvraag bevat een `X-MicrosoftSpeechServices-Event` header met de waarde ping. Als de webhook is geregistreerd met een geheim, bevat deze een `X-MicrosoftSpeechServices-Signature` header met een sha256-hash van de payload met het geheim als HMAC-sleutel. De hash is base64-gecodeerd.

https://westus.cris.ai/api/speechtotext/v2.1/transcriptions/hooks/:id/test Hoofd tekst: leeg

Verzendt een POST-aanvraag naar de geregistreerde URL als een entiteit voor het gebeurtenis type geabonneerd (transcriptie) aanwezig is in het systeem en de juiste status heeft. De payload wordt gegenereerd op basis van de laatste entiteit die de webhook zou hebben aangeroepen. Als er geen entiteit aanwezig is, reageert het bericht met 204. Als er een test aanvraag kan worden gedaan, reageert deze met 200. De aanvraag tekst bevindt zich op dezelfde vorm als in de GET-aanvraag voor een specifieke entiteit waarop de webhook is geabonneerd (transcriptie). De aanvraag bevat de `X-MicrosoftSpeechServices-Event` en `X-MicrosoftSpeechServices-Signature` -headers zoals hierboven wordt beschreven.

### <a name="run-a-test"></a>Een test uitvoeren

Een snelle test kan worden uitgevoerd met behulp https://bin.webhookrelay.com van de website. Van daaruit kunt u aanroepen van Url's voor door geven als para meter naar het HTTP POST-bericht voor het maken van een webhook dat eerder in het document is beschreven.

Klik op Bucket maken en volg de instructies op het scherm om een hook te verkrijgen. Gebruik vervolgens de informatie die op deze pagina wordt weer gegeven om de Hook bij de spraak service te registreren. De payload van een doorstuur bericht: als reactie op het volt ooien van een transcriptie – ziet er als volgt uit:

```json
{
    "results": [],
    "recordingsUrls": [
        "my recording URL"
    ],
    "models": [
        {
            "modelKind": "AcousticAndLanguage",
            "datasets": [],
            "id": "a09c8c8b-1090-443c-895c-3b1cf442dec4",
            "createdDateTime": "2019-03-26T12:48:46Z",
            "lastActionDateTime": "2019-03-26T14:04:47Z",
            "status": "Succeeded",
            "locale": "en-US",
            "name": "v4.13 Unified",
            "description": "Unified",
            "properties": {
                "Purpose": "OnlineTranscription,BatchTranscription,LanguageAdaptation",
                "ModelClass": "unified-v4"
            }
        }
    ],
    "statusMessage": "None.",
    "id": "d41615e1-a60e-444b-b063-129649810b3a",
    "createdDateTime": "2019-04-16T09:35:51Z",
    "lastActionDateTime": "2019-04-16T09:38:09Z",
    "status": "Succeeded",
    "locale": "en-US",
    "name": "Simple transcription",
    "description": "Simple transcription description",
    "properties": {
        "PunctuationMode": "DictatedAndAutomatic",
        "ProfanityFilterMode": "Masked",
        "AddWordLevelTimestamps": "True",
        "AddSentiment": "True",
        "Duration": "00:00:02"
    }
}
```
Het bericht bevat de opname-URL en modellen die worden gebruikt voor het transcriberen van die opname.

## <a name="next-steps"></a>Volgende stappen

* [Uw proefabonnement voor Speech ophalen](https://azure.microsoft.com/try/cognitive-services/)
