---
title: Webhooks - spraakservices
titlesuffix: Azure Cognitive Services
description: Webhooks zijn ideaal voor het optimaliseren van uw oplossing tijdens het afhandelen van langdurige HTTP terugbellen actieve processen, zoals invoer, aanpassing, nauwkeurigheidstests of transcripties van langlopende bestanden.
services: cognitive-services
author: PanosPeriorellis
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/11/2019
ms.author: panosper
ms.custom: seodec18
ms.openlocfilehash: 7b47d4fc3aa4a1a50e441e668a856703c67045ae
ms.sourcegitcommit: 48a41b4b0bb89a8579fc35aa805cea22e2b9922c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/15/2019
ms.locfileid: "59581007"
---
# <a name="webhooks-for-speech-services"></a>Webhooks voor spraakservices

Webhooks zijn HTTP callbacks waarmee uw toepassing voor de acceptatie van gegevens van de spraakservices wanneer deze beschikbaar. Met behulp van webhooks, kunt u uw gebruik van onze REST API's optimaliseren door hoeft u niet steeds te peilen op een reactie. In de volgende gedeelten leert u hoe u webhooks gebruiken met de Speech Services.

## <a name="supported-operations"></a>Ondersteunde bewerkingen

Webhooks ondersteuning de spraakservices voor alle langlopende bewerkingen. Elk van de hieronder vermelde bewerkingen kan resulteren in een HTTP-terugbeloproep na voltooiing. 

* DataImportCompletion
* ModelAdaptationCompletion
* AccuracyTestCompletion
* TranscriptionCompletion
* EndpointDeploymentCompletion
* EndpointDataCollectionCompletion

Vervolgens maken we een webhook.

## <a name="create-a-webhook"></a>Een webhook maken

Laten we een webhook voor een offline transcriptie maken. Het scenario: een gebruiker heeft een lange actief audiobestand die ze graag asynchroon met de Batch-API voor transcriptie transcriberen. 

Parameters voor de configuratie voor de aanvraag worden gegeven als JSON:

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
Alle POST-verzoeken naar de Batch-API voor transcriptie vereisen een `name`. De `description` en `properties` parameters zijn optioneel.

De `Active` eigenschap wordt gebruikt om over te schakelen zonder te verwijderen en opnieuw maken van de webhookregistratie terug naar de URL in of uit aanroepen. Als u alleen voor de terugbelfunctie eenmaal moet nadat het proces is voltooid, verwijdert de webhook en de switch de `Active` eigenschap in op onwaar.

Het gebeurtenistype `TranscriptionCompletion` is opgegeven in de matrix gebeurtenissen. Deze wordt aangeroepen terug naar het eindpunt wanneer een transcriptie opgehaald in een definitieve status heeft (`Succeeded` of `Failed`). Bij het aanroepen van terug naar de geregistreerde URL, de aanvraag bevat een `X-MicrosoftSpeechServices-Event` koptekst met een van de geregistreerde gebeurtenis-typen. Er is één aanvraag per geregistreerde gebeurtenistype. 

Er is een gebeurtenistype dat u zich niet abonneren op. Het is de `Ping` gebeurtenistype. Een aanvraag met dit type wordt verzonden naar de URL wanneer u klaar bent het maken van een webhook bij het gebruik van de ping-URL (Zie hieronder).  

In de configuratie, de `url` eigenschap is vereist. POST-aanvragen worden verzonden naar deze URL. De `secret` wordt gebruikt om u te maken van een SHA256-hash van de nettolading aan de geheime sleutel als een HMAC-sleutel. De hash is ingesteld als de `X-MicrosoftSpeechServices-Signature` header bij het aanroepen van terug naar de geregistreerde URL. Deze header is Base64-gecodeerd.

In dit voorbeeld laat zien hoe u een nettolading met valideren C#:

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
In dit codefragment de `secret` wordt ontsleuteld en gevalideerd. U ziet ook dat de webhook-gebeurtenistype is overgeschakeld. Er is momenteel één gebeurtenis per voltooide transcriptie. De code nieuwe pogingen vijf keer voor elke gebeurtenis (met een vertraging van één seconde) voordat geeft.

### <a name="other-webhook-operations"></a>Andere bewerkingen webhook

Alle geregistreerde webhooks ophalen: TOEVOEGEN https://westus.cris.ai/api/speechtotext/v2.1/transcriptions/hooks

Een specifieke webhook ophalen: TOEVOEGEN https://westus.cris.ai/api/speechtotext/v2.1/transcriptions/hooks/:id

Een specifieke webhook verwijderen: VERWIJDEREN https://westus.cris.ai/api/speechtotext/v2.1/transcriptions/hooks/:id

> [!Note] 
> In het bovenstaande voorbeeld is de regio 'westus'. Dit moet worden vervangen door de regio waar u uw resource Speech Services in Azure portal hebt gemaakt.

POST https://westus.cris.ai/api/speechtotext/v2.1/transcriptions/hooks/:id/ping hoofdtekst: leeg zijn

Een POST-aanvraag verzendt naar de geregistreerde URL. De aanvraag bevat een `X-MicrosoftSpeechServices-Event` -header met een waarde-ping. Als de webhook is geregistreerd met een geheim, bevat deze een `X-MicrosoftSpeechServices-Signature` -header met een SHA256-hash van de nettolading aan de geheime sleutel als HMAC-sleutel. De hash is Base64-gecodeerd. 

POST https://westus.cris.ai/api/speechtotext/v2.1/transcriptions/hooks/:id/test hoofdtekst: leeg zijn

Een POST-aanvraag verzendt naar de geregistreerde URL als een entiteit voor het geabonneerde gebeurtenistype (transcriptie) aanwezig in het systeem is en de juiste status heeft. De nettolading wordt worden gegenereerd op basis van de laatste entiteit die de webhook zou hebben aangeroepen. Als er is geen entiteit aanwezig is, wordt het bericht reageren met 204. Als een testaanvraag kan worden gemaakt, wordt deze reageren met 200. De aanvraagtekst is van de dezelfde vorm zoals in de GET-aanvraag voor een specifieke entiteit dat de webhook is geabonneerd voor (bijvoorbeeld transcriptie). De aanvraag heeft de `X-MicrosoftSpeechServices-Event` en `X-MicrosoftSpeechServices-Signature` headers zoals hierboven beschreven.

### <a name="run-a-test"></a>Een test uitvoeren

Een snelle test kan worden gedaan met behulp van de website https://bin.webhookrelay.com. Van daaruit kunt u aanroep back-URL's worden doorgegeven als parameter aan de HTTP-POST voor het maken van een webhook die eerder in het document wordt beschreven.

## <a name="next-steps"></a>Volgende stappen

* [Uw proefabonnement voor Speech ophalen](https://azure.microsoft.com/try/cognitive-services/)
