---
title: Cognitive Services Speech SDK oplossen van problemen
description: Probleemoplossing Cognitive Services Speech SDK
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: wolfma61
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: wolfma
ms.openlocfilehash: ff8aba562cfd2d6d54c708ee7fdc4c6ca7185f29
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/27/2018
ms.locfileid: "39284119"
---
# <a name="troubleshooting-speech-services-sdk"></a>Het oplossen van problemen Speech Services SDK

In dit artikel bevat informatie om u te helpen bij het oplossen van problemen die optreden bij het gebruik van de spraak-SDK.

## <a name="error-websocket-upgrade-failed-with-an-authentication-error-403"></a>Fout `WebSocket Upgrade failed with an authentication error (403).`

Mogelijk hebt u het juiste eindpunt voor uw regio of de service. Controleer de URI om te controleren of dat deze juist is. Zie ook de volgende sectie, als dit wordt mogelijk ook een probleem met uw abonnementssleutel of autorisatie token.

## <a name="error-http-403-forbidden-or-error-http-401-unauthorized"></a>Fout `HTTP 403 Forbidden` of fout `HTTP 401 Unauthorized`

Deze fout wordt vaak veroorzaakt door verificatieproblemen met. Verbindingsaanvragen zonder een geldig `Ocp-Apim-Subscription-Key` of `Authorization` header met de status 401- of 403 worden afgewezen.

* Als u een abonnementssleutel voor verificatie gebruikt, kan de oorzaak kan zijn:

    - de abonnementssleutel is ongeldig of ontbreekt
    - u hebt overschreden quotum voor het gebruik van uw abonnement

* Als u van een verificatietoken voor de verificatie gebruikmaakt, kan de oorzaak zijn:

    - het verificatietoken is ongeldig
    - het verificatietoken is verlopen

### <a name="validate-your-subscription-key"></a>Uw abonnementssleutel valideren

U kunt controleren om ervoor te zorgen dat u beschikt over een geldig abonnement-sleutel door het uitvoeren van een van de onderstaande opdrachten.

> [!NOTE]
> Vervang `YOUR_SUBSCRIPTION_KEY` en `YOUR_REGION` met uw eigen abonnementssleutel en een gekoppelde regio, respectievelijk.

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

Als u een verificatietoken voor de verificatie gebruikt, voert u een van de volgende opdrachten om te verifiëren dat het Autorisatietoken nog geldig is. Tokens zijn geldig voor 10 minuten.

> [!NOTE]
> Vervang `YOUR_AUDIO_FILE` met het pad naar uw vooraf opgenomen audiobestand `YOUR_ACCESS_TOKEN` door het Autorisatietoken geretourneerd in de vorige stap en `YOUR_REGION` met de juiste regio.

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

Deze fout treedt meestal op wanneer de aanvraagtekst ongeldige audiogegevens bevat. Alleen `WAV` indeling wordt ondersteund. Controleer ook of de aanvraagheaders om ervoor te zorgen dat u een geschikte opgeeft `Content-Type` en `Content-Length`.

## <a name="error-http-408-request-timeout"></a>Fout `HTTP 408 Request Timeout`

De fout is het waarschijnlijk dat er geen audiogegevens worden verzonden naar de service. Deze fout kan ook worden veroorzaakt door netwerkproblemen.

## <a name="the-recognitionstatus-in-the-response-is-initialsilencetimeout"></a>De `RecognitionStatus` in het antwoord is `InitialSilenceTimeout`

Audiogegevens is meestal de reden dat het probleem veroorzaakt. Bijvoorbeeld:

* Er is een lange stretch stilte aan het begin van de audio. De service de opname stoppen na een paar seconden en retourneren `InitialSilenceTimeout`.
* Een niet-ondersteunde codec-indeling, die ervoor zorgt dat de gegevens worden behandeld als stilte maakt gebruik van de audio.

## <a name="next-steps"></a>Volgende stappen

* [Releaseopmerkingen](releasenotes.md)

