---
title: Aan de slag met de Microsoft Speech Recognition API met behulp van REST | Microsoft Docs
description: Met REST toegang tot de erkenning Speech-API in Microsoft cognitieve Services gesproken audio naar tekst wilt converteren.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/15/2017
ms.author: zhouwang
ms.openlocfilehash: 53785cdfd75c23910802f2be20e6305817b3b097
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344703"
---
# <a name="get-started-with-speech-recognition-by-using-the-rest-api"></a>Aan de slag met spraakherkenning met behulp van de REST-API

Met cloud-gebaseerde spraak-Service, kunt u toepassingen ontwikkelen met behulp van de REST-API aan gesproken audio naar tekst wilt converteren.

## <a name="prerequisites"></a>Vereisten

### <a name="subscribe-to-the-speech-api-and-get-a-free-trial-subscription-key"></a>Abonneer u op de Speech-API en de sleutel voor een gratis proefabonnement ophalen

De Speech-API maakt deel uit van cognitieve Services (eerder Project Oxford). Krijgt u gratis proefabonnement sleutels van de [abonnement cognitieve Services](https://azure.microsoft.com/try/cognitive-services/) pagina. Nadat u de Speech-API selecteert, selecteert u **API-sleutel ophalen** ophalen van de sleutel. Het resultaat een primaire en secundaire sleutel. Beide sleutels zijn gekoppeld aan hetzelfde contingent, zodat u van sleutel kunt gebruiken.

> [!IMPORTANT]
>* De abonnementssleutel voor een ophalen. Voordat u toegang hebt tot de REST-API, moet u beschikken over een [abonnementssleutel](https://azure.microsoft.com/try/cognitive-services/).
>
>* De abonnementssleutel van uw worden gebruikt. Vervang in de volgende voorbeelden van de REST YOUR_SUBSCRIPTION_KEY met de abonnementssleutel van uw eigen. 
>
>* Raadpleeg de [verificatie](../how-to/how-to-authentication.md) pagina voor het ophalen van de abonnementssleutel van een.

### <a name="prerecorded-audio-file"></a>Vooraf opgenomen audio-bestand

In dit voorbeeld gebruiken we een opgenomen audiobestand ter illustratie van het gebruik van de REST-API. Leg een audiobestand van uzelf een korte zin gezegd. Bijvoorbeeld, 'Wat is het weer zoals vandaag?' of 'Grappige films vinden'. Spraakherkenning API ondersteunt ook externe microfoon invoer.

> [!NOTE]
> In het voorbeeld vereist dat audio wordt vastgelegd als een WAV-bestand met **PCM één kanaal (mono) 16 KHz**.

## <a name="build-a-recognition-request-and-send-it-to-the-speech-recognition-service"></a>Een aanvraag voor bouwen en verzenden naar de erkenning spraak service

De volgende stap voor spraakherkenning is een POST-aanvraag verzenden naar de spraak HTTP-eindpunten met de juiste aanvraag-header en de hoofdtekst.

### <a name="service-uri"></a>Service-URI

De spraak voor service-URI is gedefinieerd op basis van [erkenning modi](../concepts.md#recognition-modes) en [talen](../concepts.md#recognition-languages):

```HTTP
https://speech.platform.bing.com/speech/recognition/<RECOGNITION_MODE>/cognitiveservices/v1?language=<LANGUAGE_TAG>&format=<OUTPUT_FORMAT>
```

`<RECOGNITION_MODE>` Hiermee wordt de opname-modus en u moet een van de volgende waarden: `interactive`, `conversation`, of `dictation`. Dit is een vereiste bronpad in de URI. Zie voor meer informatie [erkenning modi](../concepts.md#recognition-modes).

`<LANGUAGE_TAG>` is een vereiste parameter in de queryreeks. Hiermee definieert u de taal van het doel voor audio conversie: bijvoorbeeld `en-US` voor Engels (Verenigde Staten). Zie voor meer informatie [talen](../concepts.md#recognition-languages).

`<OUTPUT_FORMAT>` is een optionele parameter in de queryreeks. De toegestane waarden zijn `simple` en `detailed`. Standaard stuurt de service resulteert in een `simple` indeling. Zie voor meer informatie [uitvoerindeling](../concepts.md#output-format).

In de volgende tabel vindt u enkele voorbeelden van service URI's.

| Opname-modus  | Taal | De indeling van uitvoer | Service-URI |
|---|---|---|---|
| `interactive` | pt-BR | Standaard | https://speech.platform.bing.com/speech/recognition/interactive/cognitiveservices/v1?language=pt-BR |
| `conversation` | nl-NL | Gedetailleerd |https://speech.platform.bing.com/speech/recognition/conversation/cognitiveservices/v1?language=en-US&format=detailed |
| `dictation` | fr-FR | Eenvoudig | https://speech.platform.bing.com/speech/recognition/dictation/cognitiveservices/v1?language=fr-FR&format=simple |

> [!NOTE]
> De service-URI is alleen nodig wanneer de toepassing gebruikmaakt van REST-API's de erkenning spraak service aanroepen. Als u een van de [clientbibliotheken](GetStartedClientLibraries.md), hoeft u niet te weten welke URI wordt gebruikt. De clientbibliotheken mogelijk andere service URI's, gebruiken die alleen voor een specifieke client-bibliotheek gelden. Zie voor meer informatie de clientbibliotheek van uw keuze.

### <a name="request-headers"></a>Aanvraagheaders

De volgende velden moeten worden ingesteld in de aanvraagheader:

- `Ocp-Apim-Subscription-Key`: Elke keer dat u de service aanroepen moet u de abonnementssleutel van uw in doorgeven de `Ocp-Apim-Subscription-Key` header. Spraak Service biedt ook ondersteuning voor autorisatie doorgeven tokens in plaats van abonnement sleutels. Zie voor meer informatie [verificatie](../How-to/how-to-authentication.md).
- `Content-type`: De `Content-type` veld beschrijft de indeling en codec van audiostroom. Op dit moment alleen WAV-bestand en PCM Mono 16000 codering wordt ondersteund. De waarde Content-type voor deze indeling is `audio/wav; codec=audio/pcm; samplerate=16000`.

De `Transfer-Encoding` veld is optioneel. Als u dit veld ingesteld op `chunked`, kunt u de audio afgeknipt in kleine stukken verdeeld. Zie voor meer informatie [Chunked overdracht](../How-to/how-to-chunked-transfer.md).

Hier volgt een voorbeeld-aanvraagheader:

```HTTP
POST https://speech.platform.bing.com/speech/recognition/interactive/cognitiveservices/v1?language=en-US&format=detailed HTTP/1.1
Accept: application/json;text/xml
Content-Type: audio/wav; codec=audio/pcm; samplerate=16000
Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY
Host: speech.platform.bing.com
Transfer-Encoding: chunked
Expect: 100-continue
```

### <a name="send-a-request-to-the-service"></a>Een aanvraag verzenden naar de service

Het volgende voorbeeld laat zien hoe een aanvraag te verzenden spraak opname naar-spraak REST-eindpunten. Dit maakt gebruik van de `interactive` opname-modus.

> [!NOTE]
> Vervang `YOUR_AUDIO_FILE` met het pad naar uw vooraf opgenomen audio-bestand. Vervang `YOUR_SUBSCRIPTION_KEY` met de abonnementssleutel van uw eigen.

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/Powershell)

```Powershell

$SpeechServiceURI =
'https://speech.platform.bing.com/speech/recognition/interactive/cognitiveservices/v1?language=en-us&format=detailed'

# $OAuthToken is the authorization token returned by the token service.
$RecoRequestHeader = @{
  'Ocp-Apim-Subscription-Key' = 'YOUR_SUBSCRIPTION_KEY';
  'Transfer-Encoding' = 'chunked';
  'Content-type' = 'audio/wav; codec=audio/pcm; samplerate=16000'
}

# Read audio into byte array
$audioBytes = [System.IO.File]::ReadAllBytes("YOUR_AUDIO_FILE")

$RecoResponse = Invoke-RestMethod -Method POST -Uri $SpeechServiceURI -Headers $RecoRequestHeader -Body $audioBytes

# Show the result
$RecoResponse

```

# <a name="curltabcurl"></a>[cURL](#tab/curl)

Het voorbeeld wordt curl op Linux met bash. Als deze niet beschikbaar op uw platform is, moet u mogelijk curl installeren. In het voorbeeld werkt ook op Cygwin op Windows, Git Bash, zsh en andere shells.

> [!NOTE]
> Houd de `@` voordat de audio bestandsnaam tijdens het vervangen van `YOUR_AUDIO_FILE` met het pad naar uw vooraf opgenomen audio-bestand, zoals deze geeft aan dat de waarde van `--data-binary` is een bestandsnaam in plaats van gegevens.

```
curl -v -X POST "https://speech.platform.bing.com/speech/recognition/interactive/cognitiveservices/v1?language=en-us&format=detailed" -H "Transfer-Encoding: chunked" -H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY" -H "Content-type: audio/wav; codec=audio/pcm; samplerate=16000" --data-binary @YOUR_AUDIO_FILE
```

# <a name="ctabcsharp"></a>[C#](#tab/CSharp)

```cs
HttpWebRequest request = null;
request = (HttpWebRequest)HttpWebRequest.Create(requestUri);
request.SendChunked = true;
request.Accept = @"application/json;text/xml";
request.Method = "POST";
request.ProtocolVersion = HttpVersion.Version11;
request.ContentType = @"audio/wav; codec=audio/pcm; samplerate=16000";
request.Headers["Ocp-Apim-Subscription-Key"] = "YOUR_SUBSCRIPTION_KEY";

// Send an audio file by 1024 byte chunks
using (FileStream fs = new FileStream(YOUR_AUDIO_FILE, FileMode.Open, FileAccess.Read))
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

---

## <a name="process-the-speech-recognition-response"></a>Het antwoord van de erkenning spraak verwerken

Nadat de aanvraag is verwerkt, retourneert spraak-Service de resultaten in een antwoord als JSON-indeling.

> [!NOTE]
> Als de vorige code een fout retourneert, Zie [probleemoplossing](../troubleshooting.md) vinden van de mogelijke oorzaak.

Het volgende codefragment toont een voorbeeld van hoe u het antwoord van de stroom kan lezen.

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/Powershell)

```Powershell
# show the response in JSON format
ConvertTo-Json $RecoResponse
```

# <a name="curltabcurl"></a>[cURL](#tab/curl)

In dit voorbeeld retourneert curl rechtstreeks van het antwoordbericht in een tekenreeks. Als u weergeven in JSON-indeling wilt, kunt u extra hulpprogramma's, bijvoorbeeld jq.

```
curl -X POST "https://speech.platform.bing.com/speech/recognition/interactive/cognitiveservices/v1?language=en-us&format=detailed" -H "Transfer-Encoding: chunked" -H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY" -H "Content-type: audio/wav; codec=audio/pcm; samplerate=16000" --data-binary @YOUR_AUDIO_FILE | jq
```

# <a name="ctabcsharp"></a>[C#](#tab/CSharp)

```cs
/*
* Get the response from the service.
*/
Console.WriteLine("Response:");
using (WebResponse response = request.GetResponse())
{
    Console.WriteLine(((HttpWebResponse)response).StatusCode);

    using (StreamReader sr = new StreamReader(response.GetResponseStream()))
    {
        responseString = sr.ReadToEnd();
    }

    Console.WriteLine(responseString);
    Console.ReadLine();
}
```

---

Het volgende voorbeeld is een JSON-antwoord:

```json
OK
{
  "RecognitionStatus": "Success",
  "Offset": 22500000,
  "Duration": 21000000,
  "NBest": [{
    "Confidence": 0.941552162,
    "Lexical": "find a funny movie to watch",
    "ITN": "find a funny movie to watch",
    "MaskedITN": "find a funny movie to watch",
    "Display": "Find a funny movie to watch."
  }]
}
```

## <a name="limitations"></a>Beperkingen

De REST-API heeft een aantal beperkingen:

- Het ondersteunt audiostroom maximaal 15 seconden.
- Deze biedt geen ondersteuning voor tussenliggende resultaten tijdens herkenning. Gebruikers ontvangen alleen het resultaat van de laatste herkenning.

U kunt deze beperkingen verwijderen met spraak [clientbibliotheken](GetStartedClientLibraries.md). Of u kunt werken rechtstreeks met de [spraak WebSocket-protocol](../API-Reference-REST/websocketprotocol.md).

## <a name="whats-next"></a>Volgend onderwerp

- Zie informatie over het gebruik van de REST-API in C#, Java, enz., deze [voorbeeldtoepassingen](../samples.md).
- Als u wilt zoeken en corrigeer de fouten, Zie [probleemoplossing](../troubleshooting.md).
- Zie voor het gebruik van meer geavanceerde functies, hoe u aan de slag met behulp van spraak [clientbibliotheken](GetStartedClientLibraries.md).

### <a name="license"></a>Licentie

Alle cognitieve Services SDK's en voorbeelden worden in licentie gegeven aan de MIT-licentie. Zie voor meer informatie [licentie](https://github.com/Microsoft/Cognitive-Speech-STT-JavaScript/blob/master/LICENSE.md).
