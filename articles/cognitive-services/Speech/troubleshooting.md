---
title: Probleemoplossing | Microsoft Docs
description: Klik hier voor meer informatie over het oplossen van problemen wanneer met de Service van Microsoft spraak.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/15/2017
ms.author: zhouwang
ms.openlocfilehash: 04f3da19939d523d201d357b2b0293db1508431d
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344675"
---
# <a name="troubleshooting"></a>Problemen oplossen

## <a name="error-http-403-forbidden"></a>Fout `HTTP 403 Forbidden`

Wanneer u spraakherkenning API gebruikt, wordt een `HTTP 403 Forbidden` fout.

### <a name="cause"></a>Oorzaak

Deze fout wordt vaak veroorzaakt door problemen met verificatie. Verbindingsaanvragen zonder geldige `Ocp-Apim-Subscription-Key` of `Authorization` header worden geweigerd door de service met een `HTTP 403 Forbidden` antwoord.

Als u van abonnementssleutel voor verificatie gebruikmaakt, kan de reden zijn

- de abonnementssleutel is ontbreekt of is ongeldig
- het gebruiksquotum voor informatie over het van de abonnementssleutel is overschreden
- de `Ocp-Apim-Subscription-Key` veld is niet ingesteld in de aanvraagheader wanneer REST-API wordt aangeroepen

Als u de verificatietoken voor verificatie gebruikt, kunnen de fout veroorzaakt door de volgende oorzaken hebben.

- de `Authorization` koptekst ontbreekt in de aanvraag bij gebruik van REST
- het verificatietoken dat is opgegeven in de autorisatie-header is ongeldig
- het verificatietoken is verlopen. Het toegangstoken heeft een vervaldatum van 10 minuten

Zie voor meer informatie over verificatie de [verificatie](How-to/how-to-authentication.md) pagina.

### <a name="troubleshooting-steps"></a>Stappen voor probleemoplossing

#### <a name="verify-that-your-subscription-key-is-valid"></a>Controleren of de abonnementssleutel van uw geldig is

U kunt de volgende opdracht voor verificatie uitvoeren. Opmerking voor vervangen *YOUR_SUBSCRIPTION_KEY* met de abonnementssleutel van uw eigen. Als de abonnementssleutel van uw geldig is, ontvangt u in het antwoord het verificatietoken als een JSON Web Token (JWT). Anders krijgt u een fout opgetreden in het antwoord.

> [!NOTE]
> Vervang `YOUR_SUBSCRIPTION_KEY` met de abonnementssleutel van uw eigen.

# <a name="powershelltabpowershell"></a>[Powershell](#tab/Powershell)

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

# <a name="curltabcurl"></a>[cURL](#tab/curl)

Het voorbeeld wordt curl op Linux met bash. Als deze niet beschikbaar op uw platform is, moet u wellicht curl installeren. In het voorbeeld moet ook op Cygwin werken op Windows, Git Bash, zsh en andere shells.

```
curl -v -X POST "https://api.cognitive.microsoft.com/sts/v1.0/issueToken" -H "Content-type: application/x-www-form-urlencoded" -H "Content-Length: 0" -H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY"
```
---

Zorg ervoor dat u dezelfde abonnementssleutel in uw toepassing of in de REST-aanvraag zoals die hierboven wordt gebruikt.

#### <a name="verify-the-authorization-token"></a>Controleer of het verificatietoken

Deze stap is alleen nodig als u verificatietoken voor verificatie gebruiken. Voer de volgende opdracht om te controleren of het verificatietoken nog geldig. De opdracht een POST-aanvraag indient bij de service en een antwoordbericht van de service verwacht. Als u nog steeds HTTP ontvangen `403 Forbidden` fout, Controleer de toegang token is correct ingesteld en niet verlopen.

> [!IMPORTANT]
> Het token heeft een vervaldatum van 10 minuten.
> [!NOTE]
> Vervang `YOUR_AUDIO_FILE` met het pad naar uw vooraf opgenomen audio-bestand en `YOUR_ACCESS_TOKEN` geretourneerd door het verificatietoken in de vorige stap.

# <a name="powershelltabpowershell"></a>[Powershell](#tab/Powershell)

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

# <a name="curltabcurl"></a>[cURL](#tab/curl)

```
curl -v -X POST "https://speech.platform.bing.com/speech/recognition/interactive/cognitiveservices/v1?language=en-us&format=detailed" -H "Transfer-Encoding: chunked" -H "Authorization: Bearer YOUR_ACCESS_TOKEN" -H "Content-type: audio/wav; codec=audio/pcm; samplerate=16000" --data-binary @YOUR_AUDIO_FILE
```

---

## <a name="error-http-400-bad-request"></a>Fout `HTTP 400 Bad Request`

Deze reden wordt meestal aan dat de hoofdtekst van de aanvraag ongeldige audiogegevens bevat. Op dit moment ondersteunen we alleen WAV-bestand.

## <a name="error-http-408-request-timeout"></a>Fout `HTTP 408 Request Timeout`

De fout is hoogstwaarschijnlijk omdat dat geen audio gegevens naar de service verzonden en de service deze fout na time-out retourneert. Voor de REST API, moeten de gegevens worden geplaatst in de aanvraagtekst.

## <a name="the-recognitionstatus-in-the-response-is-initialsilencetimeout"></a>De `RecognitionStatus` in het antwoord is `InitialSilenceTimeout`

Audiogegevens is meestal de reden dat het probleem veroorzaakt. Bijvoorbeeld:

- de audio heeft een lange stilte tijd aan het begin. De service stopt de opname na een aantal seconden en retourneert `InitialSilenceTimeout`.
- niet-ondersteunde codec-indeling, waardoor de gegevens worden behandeld als stilte maakt gebruik van de audio.
