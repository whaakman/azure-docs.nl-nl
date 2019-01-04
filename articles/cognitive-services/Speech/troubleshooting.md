---
title: Het oplossen van Bing Speech | Microsoft Docs
titlesuffix: Azure Cognitive Services
description: Over het oplossen van problemen bij het gebruik van de Bing Speech.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/18/2018
ms.author: zhouwang
ms.openlocfilehash: 2da6795e938f1a0c0ed0495fb169007126501eea
ms.sourcegitcommit: 7862449050a220133e5316f0030a259b1c6e3004
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/22/2018
ms.locfileid: "53753657"
---
# <a name="troubleshooting-bing-speech"></a>Bing Speech oplossen

[!INCLUDE [Deprecation note](../../../includes/cognitive-services-bing-speech-api-deprecation-note.md)]

## <a name="error-http-403-forbidden"></a>Fout `HTTP 403 Forbidden`

Wanneer u de the spraakherkennings-API, het resultaat een `HTTP 403 Forbidden` fout.

### <a name="cause"></a>Oorzaak

Deze fout wordt vaak veroorzaakt door verificatieproblemen met. Verbindingsaanvragen zonder geldige `Ocp-Apim-Subscription-Key` of `Authorization` header worden geweigerd door de service met een `HTTP 403 Forbidden` antwoord.

Als u de abonnementssleutel voor verificatie gebruikt, kan de reden zijn

- de abonnementssleutel is ongeldig of ontbreekt
- het quotum voor het gebruik van de abonnementssleutel is overschreden
- de `Ocp-Apim-Subscription-Key` veld is niet ingesteld in de aanvraagheader als REST-API wordt aangeroepen

Als u van de verificatietoken voor de verificatie gebruikmaakt, kunnen de fout veroorzaakt door de volgende oorzaken hebben.

- de `Authorization` koptekst ontbreekt in de aanvraag bij het gebruik van REST
- het verificatietoken dat is opgegeven in de autorisatie-header is ongeldig
- het verificatietoken is verlopen. Het toegangstoken heeft een vervaldatum van 10 minuten

Zie voor meer informatie over verificatie, de [verificatie](How-to/how-to-authentication.md) pagina.

### <a name="troubleshooting-steps"></a>Stappen voor probleemoplossing

#### <a name="verify-that-your-subscription-key-is-valid"></a>Controleer of de abonnementssleutel van uw geldig is

U kunt de volgende opdracht voor verificatie kunt uitvoeren. Houd er rekening mee te vervangen *YOUR_SUBSCRIPTION_KEY* door uw eigen abonnementssleutel. Als de abonnementssleutel van uw geldig is, ontvangt u in het antwoord het Autorisatietoken als een JSON Web Token (JWT). Anders wordt er een fout optreedt in het antwoord.

> [!NOTE]
> Vervang `YOUR_SUBSCRIPTION_KEY` door uw eigen abonnementssleutel.

# <a name="powershelltabazure-powershell"></a>[Powershell](#tab/azure-powershell)

```Powershell
$FetchTokenHeader = @{
  'Content-type'='application/x-www-form-urlencoded';
  'Content-Length'= '0';
  'Ocp-Apim-Subscription-Key' = 'YOUR_SUBSCRIPTION_KEY'
}

$OAuthToken = Invoke-RestMethod -Method POST -Uri https://api.cognitive.microsoft.com/sts/v1.0/issueToken -Headers $FetchTokenHeader

# show the token received
$OAuthToken

```

# <a name="curltabcurl"></a>[CURL](#tab/curl)

Het voorbeeld wordt curl gebruikt op Linux met bash. Als deze niet beschikbaar is op uw platform, moet u mogelijk curl installeren. Het voorbeeld moet ook op Cygwin werken op Windows, Git Bash, zsh en andere shells.

```
curl -v -X POST "https://api.cognitive.microsoft.com/sts/v1.0/issueToken" -H "Content-type: application/x-www-form-urlencoded" -H "Content-Length: 0" -H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY"
```
---

Zorg ervoor dat u dezelfde abonnementssleutel in uw toepassing of in de REST-aanvraag zoals die hierboven wordt gebruikt.

#### <a name="verify-the-authorization-token"></a>Controleer of het Autorisatietoken

Deze stap is alleen nodig als u de verificatietoken voor verificatie gebruiken. Voer de volgende opdracht om te verifiëren dat het Autorisatietoken nog geldig is. De opdracht maakt een POST-aanvraag naar de service en wordt verwacht dat een antwoordbericht van de service. Als u nog steeds HTTP `403 Forbidden` fout, Controleer de toegang tot de token is correct ingesteld en niet verlopen.

> [!IMPORTANT]
> Het token heeft een vervaldatum van 10 minuten.
> [!NOTE]
> Vervang `YOUR_AUDIO_FILE` met het pad naar uw vooraf opgenomen audiobestand en `YOUR_ACCESS_TOKEN` geretourneerd door het Autorisatietoken in de vorige stap.

# <a name="powershelltabazure-powershell"></a>[Powershell](#tab/azure-powershell)

```Powershell

$SpeechServiceURI =
'https://speech.platform.bing.com/speech/recognition/interactive/cognitiveservices/v1?language=en-us&format=detailed'

# $OAuthToken is the authrization token returned by the token service.
$RecoRequestHeader = @{
  'Authorization' = 'Bearer '+ $OAuthToken;
  'Transfer-Encoding' = 'chunked'
  'Content-type' = 'audio/wav; codec=audio/pcm; samplerate=16000'
}

# Read audio into byte array
$audioBytes = [System.IO.File]::ReadAllBytes("YOUR_AUDIO_FILE")

$RecoResponse = Invoke-RestMethod -Method POST -Uri $SpeechServiceURI -Headers $RecoRequestHeader -Body $audioBytes

# Show the result
$RecoResponse

```

# <a name="curltabcurl"></a>[CURL](#tab/curl)

```
curl -v -X POST "https://speech.platform.bing.com/speech/recognition/interactive/cognitiveservices/v1?language=en-us&format=detailed" -H "Transfer-Encoding: chunked" -H "Authorization: Bearer YOUR_ACCESS_TOKEN" -H "Content-type: audio/wav; codec=audio/pcm; samplerate=16000" --data-binary @YOUR_AUDIO_FILE
```

---

## <a name="error-http-400-bad-request"></a>Fout `HTTP 400 Bad Request`

Daarom is meestal dat de aanvraagtekst ongeldige audiogegevens bevat. We ondersteunen momenteel alleen WAV-bestand.

## <a name="error-http-408-request-timeout"></a>Fout `HTTP 408 Request Timeout`

De fout is hoogstwaarschijnlijk omdat dat geen audio gegevens worden verzonden naar de service en de service na de time-out voor deze fout geeft als resultaat. Voor de REST-API, moeten de gegevens worden geplaatst in de aanvraagtekst.

## <a name="the-recognitionstatus-in-the-response-is-initialsilencetimeout"></a>De `RecognitionStatus` in het antwoord is `InitialSilenceTimeout`

Audiogegevens is meestal de reden dat het probleem veroorzaakt. Bijvoorbeeld:

- de audio heeft een lange stilte tijd aan het begin. De service stopt de opname na een aantal seconden en retourneert `InitialSilenceTimeout`.
- niet-ondersteunde codec indeling, waardoor de gegevens worden behandeld als stilte maakt gebruik van de audio.
