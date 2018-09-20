---
title: Aan de slag met de Bing Speech-API met behulp van REST | Microsoft Docs
titlesuffix: Azure Cognitive Services
description: REST gebruiken voor toegang tot de spraak-API in Microsoft Cognitive Services om Converteer gesproken audio naar tekst.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/18/2018
ms.author: zhouwang
ms.openlocfilehash: ed1624648d668f392ed854cccf0843809b7e0e5e
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46365133"
---
# <a name="quickstart-use-the-bing-speech-recognition-rest-api"></a>Snelstartgids: De Bing-spraakherkenning REST-API gebruiken

Met de cloud gebaseerde Bing Speech-Service, kunt u toepassingen ontwikkelen met behulp van de REST-API aan Converteer gesproken audio naar tekst.

## <a name="prerequisites"></a>Vereisten

### <a name="subscribe-to-the-speech-api-and-get-a-free-trial-subscription-key"></a>Abonneer u op de spraak-API en een gratis proefabonnement-sleutel ophalen

De spraak-API maakt deel uit van Cognitive Services (eerder Project Oxford). U krijgt een gratis proefabonnement sleutels uit de [Cognitive Services-abonnement](https://azure.microsoft.com/try/cognitive-services/) pagina. Nadat u de spraak-API selecteert, selecteert u **API-sleutel ophalen** om op te halen van de sleutel. Het resultaat een primaire en secundaire sleutel. Beide sleutels zijn gekoppeld aan dezelfde quota, zodat u beide sleutels kunt gebruiken.

> [!IMPORTANT]
>* De abonnementssleutel van een ophalen. Voordat u krijgt de REST-API tot toegang, moet u beschikken over een [abonnementssleutel](https://azure.microsoft.com/try/cognitive-services/).
>
>* Gebruik uw abonnement. In de volgende REST-voorbeelden, kunt u YOUR_SUBSCRIPTION_KEY vervangen door uw eigen abonnementssleutel. 
>
>* Raadpleeg de [verificatie](../how-to/how-to-authentication.md) pagina voor het ophalen van de abonnementssleutel van een.

### <a name="prerecorded-audio-file"></a>Vooraf opgenomen audiobestand

In dit voorbeeld gebruiken we een opgenomen audiobestand te laten zien hoe u de REST-API gebruikt. Registreer een geluidsbestand van uzelf een korte zin zeggen. Zeg bijvoorbeeld 'Wat is het weer, zoals vandaag?' of "Vinden grappige films." De the spraakherkennings-API biedt ook ondersteuning voor externe microfoon invoer.

> [!NOTE]
> Het voorbeeld is vereist dat audio wordt vastgelegd als een WAV-bestand met **PCM één kanaal (mono), 16 KHz**.

## <a name="build-a-recognition-request-and-send-it-to-the-speech-recognition-service"></a>Een aanvraag voor spraakherkenning maken en te verzenden naar de spraakherkenningsservice

De volgende stap voor spraakherkenning is een POST-aanvraag verzenden naar de spraak-HTTP-eindpunten met de juiste aanvraag koptekst en hoofdtekst.

### <a name="service-uri"></a>Service-URI

De spraakherkenningsservice URI is gedefinieerd op basis van [erkenning modi](../concepts.md#recognition-modes) en [erkenning talen](../concepts.md#recognition-languages):

```HTTP
https://speech.platform.bing.com/speech/recognition/<RECOGNITION_MODE>/cognitiveservices/v1?language=<LANGUAGE_TAG>&format=<OUTPUT_FORMAT>
```

`<RECOGNITION_MODE>` Hiermee geeft u de opname-modus en moet een van de volgende waarden: `interactive`, `conversation`, of `dictation`. Dit is een vereiste bron-pad in de URI. Zie voor meer informatie, [erkenning modi](../concepts.md#recognition-modes).

`<LANGUAGE_TAG>` is een vereiste parameter in de querytekenreeks. De doeltaal voor audio conversie wordt gedefinieerd: bijvoorbeeld `en-US` voor Engels (Verenigde Staten). Zie voor meer informatie, [erkenning talen](../concepts.md#recognition-languages).

`<OUTPUT_FORMAT>` is een optionele parameter in de querytekenreeks. De toegestane waarden zijn `simple` en `detailed`. Standaard retourneert resultaten in de service `simple` indeling. Zie voor meer informatie, [uitvoerindeling](../concepts.md#output-format).

In de volgende tabel vindt u enkele voorbeelden van URI's-service.

| Opname-modus  | Taal | Uitvoerindeling | Service-URI |
|---|---|---|---|
| `interactive` | pt-BR | Standaard | https://speech.platform.bing.com/speech/recognition/interactive/cognitiveservices/v1?language=pt-BR |
| `conversation` | nl-NL | Gedetailleerd |https://speech.platform.bing.com/speech/recognition/conversation/cognitiveservices/v1?language=en-US&format=detailed |
| `dictation` | fr-FR | Eenvoudig | https://speech.platform.bing.com/speech/recognition/dictation/cognitiveservices/v1?language=fr-FR&format=simple |

> [!NOTE]
> De service-URI is alleen nodig wanneer uw toepassing maakt gebruik van REST-API's om aan te roepen de spraakherkenningsservice. Als u een van de [clientbibliotheken](GetStartedClientLibraries.md), doorgaans niet nodig om te weten welke URI wordt gebruikt. De clientbibliotheken mogelijk andere service-URI's, die alleen van toepassing op een specifieke client-bibliotheek gebruiken. Zie voor meer informatie de clientbibliotheek van uw keuze.

### <a name="request-headers"></a>Aanvraagheaders

De volgende velden moeten worden ingesteld in de aanvraagheader:

- `Ocp-Apim-Subscription-Key`: Telkens wanneer die aanroepen van de service, moet u uw abonnementssleutel in doorgeven de `Ocp-Apim-Subscription-Key` header. Speech Service biedt ook ondersteuning voor autorisatie doorgegeven tokens in plaats van abonnementssleutels. Zie voor meer informatie, [verificatie](../How-to/how-to-authentication.md).
- `Content-type`: De `Content-type` veld Beschrijving van de indeling en codec van de audio-stream. Op dit moment alleen WAV-bestand en PCM Mono 16000 codering wordt ondersteund. De waarde van de Content-type voor deze indeling is `audio/wav; codec=audio/pcm; samplerate=16000`.

De `Transfer-Encoding` veld is optioneel. Als u dit veld ingesteld op `chunked`, kunt u de audio afgeknipt in kleine chunks. Zie voor meer informatie, [gesegmenteerde overdrachtscodering overdracht](../How-to/how-to-chunked-transfer.md).

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

Het volgende voorbeeld ziet hoe u een spraak opname-aanvraag verzendt naar spraak REST-eindpunten. Hierbij de `interactive` opname-modus.

> [!NOTE]
> Vervang `YOUR_AUDIO_FILE` met het pad naar uw vooraf opgenomen audiobestand. Vervang `YOUR_SUBSCRIPTION_KEY` door uw eigen abonnementssleutel.

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

# <a name="curltabcurl"></a>[CURL](#tab/curl)

Het voorbeeld wordt curl gebruikt op Linux met bash. Als deze niet beschikbaar is op uw platform, moet u mogelijk curl installeren. Het voorbeeld werkt ook op Cygwin op Windows, Git Bash, zsh en andere shells.

> [!NOTE]
> Houd de `@` vóór de naam van de audio-bestand bij het vervangen van `YOUR_AUDIO_FILE` met het pad naar uw vooraf opgenomen audiobestand, zoals deze wordt aangegeven dat de waarde van `--data-binary` is de bestandsnaam van een in plaats van gegevens.

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

## <a name="process-the-speech-recognition-response"></a>Het antwoord van de spraakherkenning spraak verwerken

Na het verwerken van de aanvraag, retourneert Speech Service de resultaten in een reactie als JSON-indeling.

> [!NOTE]
> Als de vorige code een fout retourneert, Zie [probleemoplossing](../troubleshooting.md) mogelijke oorzaken te vinden.

Het volgende codefragment toont een voorbeeld van hoe u het antwoord van de stroom kunt lezen.

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/Powershell)

```Powershell
# show the response in JSON format
ConvertTo-Json $RecoResponse
```

# <a name="curltabcurl"></a>[CURL](#tab/curl)

In dit voorbeeld retourneert curl rechtstreeks het antwoordbericht in een tekenreeks. Als u weergeven in JSON-indeling wilt, kunt u extra hulpprogramma's, bijvoorbeeld jq.

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

Het volgende voorbeeld wordt een JSON-antwoord:

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

De REST-API heeft enkele beperkingen:

- Het ondersteunt audio-stream maximaal 15 seconden.
- Het biedt geen ondersteuning voor tussenliggende resultaten genereren tijdens de opname. Gebruikers ontvangen het laatste herkenningsresultaat.

Als u wilt verwijderen van deze beperkingen, gebruik spraak [clientbibliotheken](GetStartedClientLibraries.md). Of werkt u direct met de [spraak WebSocket-protocol](../API-Reference-REST/websocketprotocol.md).

## <a name="whats-next"></a>Volgend onderwerp

- Zie voor meer informatie over de REST-API in C#, Java, enzovoort gebruikt, deze [voorbeeldtoepassingen](../samples.md).
- Als u wilt zoeken en oplossen van fouten, Zie [probleemoplossing](../troubleshooting.md).
- Voor het gebruik van meer geavanceerde functies, ziet u hoe u aan de slag met behulp van spraak [clientbibliotheken](GetStartedClientLibraries.md).

### <a name="license"></a>Licentie

Alle Cognitive Services-SDK's en voorbeelden hebben een licentie voor de MIT-licentie. Zie voor meer informatie, [licentie](https://github.com/Microsoft/Cognitive-Speech-STT-JavaScript/blob/master/LICENSE.md).
