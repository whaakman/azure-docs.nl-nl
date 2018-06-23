---
title: Spraak cognitieve Services SDK probleemoplossing | Microsoft Docs
description: Probleemoplossing cognitieve spraak SDK-Services
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: wolfma
ms.openlocfilehash: 16eaebcf9494ab57521068a9418ccf2ac7f5a8fe
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345463"
---
# <a name="troubleshooting-speech-services-sdk"></a>Problemen met spraakservices SDK oplossen

Dit artikel bevat informatie om u te helpen bij het oplossen van problemen die u kunt tegenkomen wanneer u de SDK-spraak.

## <a name="error-websocket-upgrade-failed-with-an-authentication-error-403"></a>Fout `WebSocket Upgrade failed with an authentication error (403).`

Mogelijk hebt u het juiste eindpunt voor uw regio of de service. Controleer de URI te controleren of dat deze juist is. Zie ook de volgende sectie, omdat dit kan ook een probleem met uw abonnementssleutel of autorisatie worden token.

## <a name="error-http-403-forbidden-or-error-http-401-unauthorized"></a>Fout `HTTP 403 Forbidden` of fout `HTTP 401 Unauthorized`

Deze fout wordt vaak veroorzaakt door problemen met verificatie. Verbindingsaanvragen zonder een geldig `Ocp-Apim-Subscription-Key` of `Authorization` header met de status 401- of 403 worden afgewezen.

* Als u een abonnementssleutel voor verificatie gebruikt, is de oorzaak kan zijn:

    - de abonnementssleutel is ontbreekt of is ongeldig
    - gebruiksgegevens van uw abonnement is overschreden

* Als u van een verificatietoken voor verificatie gebruikmaakt, is de oorzaak kan zijn:

    - het verificatietoken is ongeldig
    - het verificatietoken is verlopen

### <a name="validate-your-subscription-key"></a>Valideren van de abonnementssleutel van uw

U kunt controleren om ervoor te zorgen dat u de sleutel van een geldig abonnement hebt met een van de onderstaande opdrachten uitgevoerd.

> [!NOTE]
> Vervang `YOUR_SUBSCRIPTION_KEY` en `YOUR_REGION` met uw eigen abonnementssleutel en de bijbehorende regio, respectievelijk.

* PowerShell

    ```Powershell
    $FetchTokenHeader = @{
      'Content-type'='application/x-www-form-urlencoded'
      'Content-Length'= '0'
      'Ocp-Apim-Subscription-Key' = 'YOUR_SUBSCRIPTION_KEY'
    }
    $OAuthToken = Invoke-RestMethod -Method POST -Uri https://YOUR_REGION.api.cognitive.microsoft.com/sts/v1.0/issueToken -Headers $FetchTokenHeader
    $OAuthToken
    ```

* cURL

    ```
    curl -v -X POST "https://YOUR_REGION.api.cognitive.microsoft.com/sts/v1.0/issueToken" -H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY" -H "Content-type: application/x-www-form-urlencoded" -H "Content-Length: 0"
    ```

### <a name="validate-an-authorization-token"></a>Een verificatietoken valideren

Als u een verificatietoken voor verificatie gebruikt, voert u een van de volgende opdrachten om te controleren of het verificatietoken nog geldig. Tokens zijn geldig voor 10 minuten.

> [!NOTE]
> Vervang `YOUR_AUDIO_FILE` met het pad naar uw vooraf opgenomen audio-bestand `YOUR_ACCESS_TOKEN` door het verificatietoken geretourneerd in de vorige stap en `YOUR_REGION` met de juiste regio.

* PowerShell

    ```Powershell
    $SpeechServiceURI =
    'https://YOUR_REGION.stt.speech.microsoft.com/speech/recognition/interactive/cognitiveservices/v1?language=en-US'
    
    # $OAuthToken is the authorization token returned by the token service.
    $RecoRequestHeader = @{
      'Authorization' = 'Bearer '+ $OAuthToken
      'Transfer-Encoding' = 'chunked'
      'Content-type' = 'audio/wav; codec=audio/pcm; samplerate=16000'
    }
    
    # Read audio into byte array
    $audioBytes = [System.IO.File]::ReadAllBytes("YOUR_AUDIO_FILE")
    
    $RecoResponse = Invoke-RestMethod -Method POST -Uri $SpeechServiceURI -Headers $RecoRequestHeader -Body $audioBytes
    
    # Show the result
    $RecoResponse
    ```

* cURL

    ```
    curl -v -X POST "https://YOUR_REGION.stt.speech.microsoft.com/speech/recognition/interactive/cognitiveservices/v1?language=en-US" -H "Authorization: Bearer YOUR_ACCESS_TOKEN" -H "Transfer-Encoding: chunked" -H "Content-type: audio/wav; codec=audio/pcm; samplerate=16000" --data-binary @YOUR_AUDIO_FILE
    ```

---

## <a name="error-http-400-bad-request"></a>Fout `HTTP 400 Bad Request`

Deze fout treedt meestal op wanneer de aanvraagtekst ongeldige audiogegevens bevat. Alleen `WAV` -indeling wordt ondersteund. Controleer ook de aanvraagheaders om ervoor te zorgen dat u een geschikte opgeeft `Content-Type` en `Content-Length`.

## <a name="error-http-408-request-timeout"></a>Fout `HTTP 408 Request Timeout`

De fout is hoogstwaarschijnlijk omdat er zijn geen audiogegevens wordt verzonden naar de service. Deze fout kan ook worden veroorzaakt door problemen met netwerken.

## <a name="the-recognitionstatus-in-the-response-is-initialsilencetimeout"></a>De `RecognitionStatus` in het antwoord is `InitialSilenceTimeout`

Audiogegevens is meestal de reden dat het probleem veroorzaakt. Bijvoorbeeld:

* Er is een lange stretch stilte aan het begin van de audio. De service stop de opname na een paar seconden en retourneren `InitialSilenceTimeout`.
* De audio maakt gebruik van een niet-ondersteunde codec-indeling, dit zorgt ervoor dat de gegevens worden behandeld als stilte.

## <a name="next-steps"></a>Volgende stappen

* [Releaseopmerkingen](releasenotes.md)

