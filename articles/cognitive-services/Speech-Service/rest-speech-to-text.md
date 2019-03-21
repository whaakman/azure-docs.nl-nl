---
title: Spraak-naar-tekst API reference (REST) - spraakservices
titleSuffix: Azure Cognitive Services
description: Informatie over het gebruik van de REST-API voor spraak-naar-tekst. In dit artikel leert u over de opties voor autorisatie, opties voor query's, het structureren van een aanvraag en antwoord heeft ontvangen.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: baaa7b1068e13863293e0968cb0bf1ffb198882b
ms.sourcegitcommit: b8f9200112cae265155b8877f7e1621c4bcc53fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/14/2019
ms.locfileid: "57894964"
---
# <a name="speech-to-text-rest-api"></a>Spraak-naar-tekst REST-API

Als alternatief voor de [spraak SDK](speech-sdk.md), Speech Services kunt u Converteer spraak naar tekst met behulp van een REST-API. Elk eindpunt dat toegankelijk is, is gekoppeld aan een regio. Uw toepassing is een abonnementssleutel vereist voor het eindpunt dat u van plan bent te gebruiken.

Voordat u de spraak-naar-tekst REST-API, begrijpen:
* Aanvragen die gebruikmaken van de REST-API kunnen slechts tien seconden opgenomen audio bevatten.
* De spraak-naar-tekst REST-API retourneert alleen de laatste resultaten. Gedeeltelijke resultaten zijn niet opgegeven.

Als verzendt langer audio is vereist voor uw toepassing, kunt u overwegen de [spraak SDK](speech-sdk.md) of [batch transcriptie](batch-transcription.md).

[!INCLUDE [](../../../includes/cognitive-services-speech-service-rest-auth.md)]

## <a name="regions-and-endpoints"></a>Regio's en -eindpunten

Deze regio's worden ondersteund voor transcriptie van spraak-naar-tekst met behulp van de REST-API. Zorg ervoor dat u het eindpunt dat overeenkomt met de regio van uw abonnement selecteert.

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-speech-to-text.md)]

## <a name="query-parameters"></a>Queryparameters

Deze parameters kunnen worden opgenomen in de query-tekenreeks van de REST-aanvraag.

| Parameter | Description | Vereiste / optioneel |
|-----------|-------------|---------------------|
| `language` | Hiermee geeft u de gesproken taal die wordt herkend. Zie [ondersteunde talen](language-support.md#speech-to-text). | Vereist |
| `format` | Hiermee geeft u de resultaatindeling. Geaccepteerde waarden zijn `simple` en `detailed`. Eenvoudige tot de resultaten behoren `RecognitionStatus`, `DisplayText`, `Offset`, en `Duration`. Gedetailleerde antwoorden bevatten meerdere resultaten met vertrouwen waarden en vier verschillende manieren. De standaardinstelling is `simple`. | Optioneel |
| `profanity` | Geeft aan hoe grof taalgebruik in herkenningsresultaten worden verwerkt. Geaccepteerde waarden zijn `masked`, die grof taalgebruik vervangen door sterretjes, `removed`, die alle grof taalgebruik verwijderen uit het resultaat, of `raw`, waaronder de grof taalgebruik in het resultaat. De standaardinstelling is `masked`. | Optioneel |

## <a name="request-headers"></a>Aanvraagheaders

Deze tabel bevat de vereiste en optionele headers voor spraak-naar-tekst-aanvragen.

|Header| Description | Vereiste / optioneel |
|------|-------------|---------------------|
| `Ocp-Apim-Subscription-Key` | Uw abonnementssleutel Speech Services. | Een van beide deze header of `Authorization` is vereist. |
| `Authorization` | Een verificatietoken voorafgegaan door het woord `Bearer`. Zie [Verificatie](#authentication) voor meer informatie. | Een van beide deze header of `Ocp-Apim-Subscription-Key` is vereist. |
| `Content-type` | Beschrijft de indeling en codec van de opgegeven gegevens. Geaccepteerde waarden zijn `audio/wav; codecs=audio/pcm; samplerate=16000` en `audio/ogg; codecs=opus`. | Vereist |
| `Transfer-Encoding` | Hiermee geeft u op of gesegmenteerde audiogegevens wordt verzonden, in plaats van één bestand. Gebruik alleen deze header als audiogegevens logische groepen te verdelen. | Optioneel |
| `Expect` | Als u gesegmenteerde overdracht, stuurt u `Expect: 100-continue`. De spraakservices erkent de eerste aanvraag en wacht op de aanvullende gegevens.| Vereist als het gesegmenteerde audiogegevens verzenden. |
| `Accept` | Indien opgegeven, moet deze `application/json`. De spraakservices bieden resultaten in JSON. Sommige Web aanvraag frameworks bieden een niet-compatibele standaardwaarde als u niets opgeeft, dus het is raadzaam om altijd opnemen `Accept`. | Optioneel maar aanbevolen. |

## <a name="audio-formats"></a>Audio-indelingen

Audio wordt verzonden in de hoofdtekst van de HTTP `POST` aanvraag. Het moet zich binnen een van de indelingen in deze tabel:

| Indeling | Codec | Bitrate | Samplefrequentie |
|--------|-------|---------|-------------|
| WAV | PCM | 16-bits | 16 kHz, mono |
| OGG | OPUS | 16-bits | 16 kHz, mono |

>[!NOTE]
>De bovenstaande indelingen worden ondersteund via REST-API en WebSocket in de spraakservices. De [spraak SDK](speech-sdk.md) momenteel alleen ondersteunt de WAV opmaken met PCM codec.

## <a name="sample-request"></a>Voorbeeld van een aanvraag

Dit is een typische HTTP-aanvraag. Het onderstaande voorbeeld bevat de hostnaam en de vereiste headers. Het is belangrijk te weten dat de service ook audiogegevens, die niet is opgenomen in dit voorbeeld wordt verwacht. Zoals vermeld eerder, wordt logische groepen te verdelen aanbevolen, maar niet vereist.

```HTTP
POST speech/recognition/conversation/cognitiveservices/v1?language=en-US&format=detailed HTTP/1.1
Accept: application/json;text/xml
Content-Type: audio/wav; codecs=audio/pcm; samplerate=16000
Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY
Host: westus.stt.speech.microsoft.com
Transfer-Encoding: chunked
Expect: 100-continue
```

## <a name="http-status-codes"></a>HTTP-statuscodes

De HTTP-statuscode voor elke reactie geeft aan dat het slagen of veelvoorkomende fouten.

| HTTP-statuscode | Description | Mogelijke oorzaak |
|------------------|-------------|-----------------|
| 100 | Doorgaan | De eerste aanvraag is geaccepteerd. Doorgaan met het verzenden van de rest van de gegevens. (Met gesegmenteerde overdracht gebruikt.) |
| 200 | OK | De aanvraag is uitgevoerd. de antwoordtekst is een JSON-object. |
| 400 | Ongeldig verzoek | De taalcode die niet opgegeven of is niet een ondersteunde taal; Ongeldig audio-bestand. |
| 401 | Niet geautoriseerd | Abonnementssleutel of autorisatie-token is ongeldig in de regio is opgegeven of ongeldig eindpunt. |
| 403 | Verboden | Ontbrekende abonnementssleutel of autorisatie token. |

## <a name="chunked-transfer"></a>Gesegmenteerde overdracht

Gesegmenteerde overdrachtscodering overdracht (`Transfer-Encoding: chunked`) kunt u Verminder de latentie van de spraakherkenning omdat hierdoor de Speech Services om te beginnen met het audiobestand verwerken terwijl deze wordt verzonden. De REST-API biedt geen tijdelijke of gedeeltelijke resultaten. Deze optie is bedoeld uitsluitend het reactievermogen verbeteren.

Dit codevoorbeeld laat zien hoe voor het verzenden van audio in segmenten. Alleen de eerste chunk moet de audio bestands-header bevatten. `request` is een HTTPWebRequest-object dat is verbonden met het juiste REST-eindpunt. `audioFile` is het pad naar een audio-bestand op schijf.

```csharp

    HttpWebRequest request = null;
    request = (HttpWebRequest)HttpWebRequest.Create(requestUri);
    request.SendChunked = true;
    request.Accept = @"application/json;text/xml";
    request.Method = "POST";
    request.ProtocolVersion = HttpVersion.Version11;
    request.Host = host;
    request.ContentType = @"audio/wav; codecs=audio/pcm; samplerate=16000";
    request.Headers["Ocp-Apim-Subscription-Key"] = args[1];
    request.AllowWriteStreamBuffering = false;

using (fs = new FileStream(audioFile, FileMode.Open, FileAccess.Read))
{
    /*
    * Open a request stream and write 1024 byte chunks in the stream one at a time.
    */
    byte[] buffer = null;
    int bytesRead = 0;
    using (Stream requestStream = request.GetRequestStream())
    {
        /*
        * Read 1024 raw bytes from the input audio file.
        */
        buffer = new Byte[checked((uint)Math.Min(1024, (int)fs.Length))];
        while ((bytesRead = fs.Read(buffer, 0, buffer.Length)) != 0)
        {
            requestStream.Write(buffer, 0, bytesRead);
        }

        // Flush
        requestStream.Flush();
    }
}
```

## <a name="response-parameters"></a>Antwoord-parameters

Resultaten worden gegeven als JSON. De `simple` indeling omvat deze velden op het hoogste niveau.

| Parameter | Description  |
|-----------|--------------|
|`RecognitionStatus`|Status, zoals `Success` voor geslaagde opname. Zie de volgende tabel.|
|`DisplayText`|De herkende tekst na het hoofdlettergebruik, interpunctie, inverse tekst normalisering (conversie van de gesproken tekst voor kortere formulieren, zoals 200 voor '200' of 'Dr. Smith' voor 'doctor smith'), en grof taalgebruik maskeren. Alleen op succes is geïnstalleerd.|
|`Offset`|De tijd (in eenheden van 100 nanoseconden) waarmee de herkende spraak in de audiostream begint.|
|`Duration`|De duur (in eenheden van 100 nanoseconden) van de herkende spraak in de audio-stream.|

De `RecognitionStatus` veld bevat deze waarden mogelijk:

| Status | Description |
|--------|-------------|
| `Success` | De opname is voltooid en de `DisplayText` veld niet aanwezig is. |
| `NoMatch` | Spraak is gedetecteerd in de audiostream, maar er zijn geen woorden uit de doeltaal zijn afgestemd. Betekent meestal dat de opname-taal is een andere taal dan het account dat de gebruiker spreekt. |
| `InitialSilenceTimeout` | Het begin van de audiostream bevat alleen stilte en de time-out voor spraak-service. |
| `BabbleTimeout` | Het begin van de audiostream bevat alleen ruis, en de time-out voor spraak-service. |
| `Error` | De opname-service is een interne fout opgetreden en kan niet worden voortgezet. Probeer het opnieuw, indien mogelijk. |

> [!NOTE]
> Als de audio alleen uit grof taalgebruik bestaat, en de `profanity` queryparameter is ingesteld op `remove`, de service heeft geen spraak resultaat geretourneerd.

De `detailed` indeling bevat dezelfde gegevens als de `simple` opmaken, samen met `NBest`, een lijst met alternatieve interpretatie van het herkenningsresultaat dezelfde. Deze resultaten worden geclassificeerd van meest waarschijnlijke te minste waarschijnlijk de eerste vermelding is hetzelfde als de belangrijkste herkenningsresultaat.  Wanneer u de `detailed` indeling `DisplayText` wordt geleverd als `Display` voor elk resultaat in de `NBest` lijst.

Elk object in de `NBest` lijst bevat:

| Parameter | Description |
|-----------|-------------|
| `Confidence` | De betrouwbaarheidsscore van de vermelding van 0,0 (geen vertrouwen) 1.0 (volledig vertrouwen) |
| `Lexical` | De lexicale vorm van de herkende tekst: de werkelijke woorden herkend. |
| `ITN` | De inverse-normalized-tekst ("canonieke") vorm van de herkende tekst, met telefoon getallen, getallen, afkortingen ("doctor smith' op 'dr smith') en andere transformaties toegepast. |
| `MaskedITN` | Het formulier toevoegen met grof taalgebruik maskering toegepast, indien aangevraagd. |
| `Display` | Het formulier weergegeven van de herkende tekst, met interpunctie hoofdletters en kleine letters toegevoegd. Deze parameter is hetzelfde als `DisplayText` opgegeven als indeling is ingesteld op `simple`. |

## <a name="sample-responses"></a>Voorbeeld van reacties

Dit is een typische antwoord voor `simple` herkenning.

```json
{
  "RecognitionStatus": "Success",
  "DisplayText": "Remind me to buy 5 pencils.",
  "Offset": "1236645672289",
  "Duration": "1236645672289"
}
```

Dit is een typische antwoord voor `detailed` herkenning.

```json
{
  "RecognitionStatus": "Success",
  "Offset": "1236645672289",
  "Duration": "1236645672289",
  "NBest": [
      {
        "Confidence" : "0.87",
        "Lexical" : "remind me to buy five pencils",
        "ITN" : "remind me to buy 5 pencils",
        "MaskedITN" : "remind me to buy 5 pencils",
        "Display" : "Remind me to buy 5 pencils.",
      },
      {
        "Confidence" : "0.54",
        "Lexical" : "rewind me to buy five pencils",
        "ITN" : "rewind me to buy 5 pencils",
        "MaskedITN" : "rewind me to buy 5 pencils",
        "Display" : "Rewind me to buy 5 pencils.",
      }
  ]
}
```

## <a name="next-steps"></a>Volgende stappen

- [Uw proefabonnement voor Speech ophalen](https://azure.microsoft.com/try/cognitive-services/)
- [Akoestische modellen aanpassen](how-to-customize-acoustic-models.md)
- [Taalmodellen aanpassen](how-to-customize-language-model.md)
