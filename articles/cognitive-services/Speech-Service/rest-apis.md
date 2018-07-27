---
title: Spraakservice REST-API 's
description: Naslaginformatie voor REST-API's voor de Speech-service.
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.technology: speech
ms.topic: article
ms.date: 05/09/2018
ms.author: v-jerkin
ms.openlocfilehash: 9991e0a1fc54e6aa4a99b8bfbd93abdfe974b01b
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/27/2018
ms.locfileid: "39283066"
---
# <a name="speech-service-rest-apis"></a>Spraakservice REST-API 's

De REST-API's van de uniforme spraakherkenning-service zijn vergelijkbaar met de API's uit de [Speech-API](https://docs.microsoft.com/azure/cognitive-services/Speech) (voorheen bekend als de Bing Speech-Service). De eindpunten afwijken van de eindpunten die worden gebruikt door de vorige Speech-service.

## <a name="speech-to-text"></a>Spraak naar tekst

In de Speech to Text-API, wordt alleen de eindpunten die worden gebruikt verschillen van de vorige spraakservice spraak Recognition-API. De nieuwe eindpunten worden weergegeven in de onderstaande tabel. Gebruik de naam die overeenkomt met de regio van uw abonnement.

[!include[](../../../includes/cognitive-services-speech-service-endpoints-speech-to-text.md)]

De spraak-naar-tekst-API is anders is vergelijkbaar met de [REST-API](https://docs.microsoft.com/azure/cognitive-services/speech/getstarted/getstartedrest) voor de vorige Speech-API.

De spraak-naar-tekst REST-API ondersteunt alleen korte uitingen. Aanvragen kunnen maximaal 10 seconden audio bevatten en een maximum van 14 seconden algehele laatste. De REST-API retourneert alleen de laatste resultaten, geen tijdelijke of gedeeltelijke resultaten.

> [!NOTE]
> Als u het akoestisch model of de taalmodel of de uitspraak van aangepast, kunt u uw aangepast eindpunt gebruiken.

## <a name="text-to-speech"></a>Tekst naar spraak

De nieuwe Text to Speech-API biedt ondersteuning voor 24-KHz audio-uitvoer. De `X-Microsoft-OutputFormat` header kan nu de volgende waarden bevatten.

|||
|-|-|
`raw-16khz-16bit-mono-pcm`         | `audio-16khz-16kbps-mono-siren`
`riff-16khz-16kbps-mono-siren`     | `riff-16khz-16bit-mono-pcm`
`audio-16khz-128kbitrate-mono-mp3` | `audio-16khz-64kbitrate-mono-mp3`
`audio-16khz-32kbitrate-mono-mp3`  | `raw-24khz-16bit-mono-pcm`
`riff-24khz-16bit-mono-pcm`        | `audio-24khz-160kbitrate-mono-mp3`
`audio-24khz-96kbitrate-mono-mp3`  | `audio-24khz-48kbitrate-mono-mp3`

De spraak-service biedt nu twee 24-KHz stemmen:

Landinstelling | Taal   | Geslacht | De toewijzing van service
-------|------------|--------|------------
nl-NL  | Amerikaans-Engels | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (en-US, Jessa24kRUS)" 
nl-NL  | Amerikaans-Engels | Man   | "Microsoft Server spraak tekst en spraak, spraak (en-US, Guy24kRUS)"

Hieronder vindt u de REST-eindpunten voor de uniforme spraakherkenning service Text to Speech-API. Gebruik het eindpunt dat overeenkomt met de regio van uw abonnement.

[!include[](../../../includes/cognitive-services-speech-service-endpoints-text-to-speech.md)]

Deze verschillen in waarmee u rekening moet houden wanneer u naar verwijst de [REST API-documentatie](https://docs.microsoft.com/azure/cognitive-services/speech/api-reference-rest/bingvoiceoutput) voor de vorige Speech-API.

## <a name="authentication"></a>Verificatie

Een aanvraag verzenden naar de spraakservice REST-API vereist dat een toegangstoken. U een token verkrijgen door te geven van uw abonnementssleutel voor een regionale spraakservice `issueToken` eindpunt, wordt weergegeven in de onderstaande tabel. Gebruik het eindpunt dat overeenkomt met de regio van uw abonnement.

[!include[](../../../includes/cognitive-services-speech-service-endpoints-token-service.md)]

Elke toegangstoken is geldig voor 10 minuten. U kunt een nieuw token verkrijgen op elk gewenst moment, met inbegrip van, als u dat wilt, net voordat elk verzoek Speech REST-API. Om te beperken van netwerkverkeer en de latentie, maar wordt u aangeraden het dezelfde token negen minuten.

De volgende secties laten zien hoe u een token verkrijgen en het gebruik ervan in een aanvraag.

### <a name="getting-a-token-http"></a>Ophalen van een token: HTTP

Hieronder volgt een voorbeeld van een HTTP-aanvraag voor het verkrijgen van een token. Vervang `YOUR_SUBSCRIPTION_KEY` met uw abonnementssleutel voor spraak-service. Als uw abonnement niet in de regio VS-West is, vervangt u de `Host` header met de hostnaam van uw regio.

```
POST /sts/v1.0/issueToken HTTP/1.1
Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY
Host: westus.api.cognitive.microsoft.com
Content-type: application/x-www-form-urlencoded
Content-Length: 0
```

De hoofdtekst van het antwoord op deze aanvraag is het toegangstoken in de indeling van de Java Web Token (JWT).

### <a name="getting-a-token-powershell"></a>Ophalen van een token: PowerShell

De onderstaande Windows PowerShell-script laat zien hoe een toegangstoken verkrijgen. Vervang `YOUR_SUBSCRIPTION_KEY` met uw abonnementssleutel voor spraak-service. Als uw abonnement niet in de regio VS-West is, kunt u de hostnaam van de opgegeven URI gewijzigd.

```Powershell
$FetchTokenHeader = @{
  'Content-type'='application/x-www-form-urlencoded';
  'Content-Length'= '0';
  'Ocp-Apim-Subscription-Key' = 'YOUR_SUBSCRIPTION_KEY'
}

$OAuthToken = Invoke-RestMethod -Method POST -Uri https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken
 -Headers $FetchTokenHeader

# show the token received
$OAuthToken

```

### <a name="getting-a-token-curl"></a>Ophalen van een token: cURL

cURL is een opdrachtregelprogramma beschikbaar in Linux (en in de Windows-subsysteem voor Linux). De volgende cURL-opdracht laat zien hoe u kunt een toegangstoken verkrijgen. Vervang `YOUR_SUBSCRIPTION_KEY` met uw abonnementssleutel voor spraak-service. Als uw abonnement niet in de regio VS-West is, kunt u de hostnaam van de opgegeven URI gewijzigd.

> [!NOTE]
> De opdracht wordt weergegeven op meerdere regels voor de leesbaarheid, maar op één regel op een shell-prompt moet worden ingevoerd.

```
curl -v -X POST 
 "https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken" 
 -H "Content-type: application/x-www-form-urlencoded" 
 -H "Content-Length: 0" 
 -H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY"
```

### <a name="getting-a-token-c"></a>Ophalen van een token: C#

De C#-klasse hieronder ziet u hoe u een toegangstoken verkrijgen. Geef uw abonnementssleutel voor spraak-service door bij het instantiëren van de klasse. Als uw abonnement niet in de regio VS-West is, wijzigt u de hostnaam van `FetchTokenUri` op de juiste wijze.

```cs
    /*
     * This class demonstrates how to get a valid access token.
     */
    public class Authentication
    {
        public static readonly string FetchTokenUri =
            "https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken";
        private string subscriptionKey;
        private string token;

        public Authentication(string subscriptionKey)
        {
            this.subscriptionKey = subscriptionKey;
            this.token = FetchTokenAsync(FetchTokenUri, subscriptionKey).Result;
        }

        public string GetAccessToken()
        {
            return this.token;
        }

        private async Task<string> FetchTokenAsync(string fetchUri, string subscriptionKey)
        {
            using (var client = new HttpClient())
            {
                client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", subscriptionKey);
                UriBuilder uriBuilder = new UriBuilder(fetchUri);

                var result = await client.PostAsync(uriBuilder.Uri.AbsoluteUri, null);
                Console.WriteLine("Token Uri: {0}", uriBuilder.Uri.AbsoluteUri);
                return await result.Content.ReadAsStringAsync();
            }
        }
    }
```

### <a name="using-a-token"></a>Met behulp van een token

Voor het gebruik van een token in een REST-API-aanvraag, bieden deze in de `Authorization` header, volgen op het woord `Bearer`. Bijvoorbeeld als volgt een voorbeeld van tekst naar spraak REST-aanvraag met een token. Vervangen door uw werkelijke token voor `YOUR_ACCESS_TOKEN` en gebruikt u de juiste hostnaam in de `Host` header.

```xml
POST /cognitiveservices/v1 HTTP/1.1
Authorization: Bearer YOUR_ACCESS_TOKEN
Host: westus.tts.speech.microsoft.com
Content-type: application/ssml+xml
Content-Length: 199
Connection: Keep-Alive

<speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice name='Microsoft Server Speech Text to Speech Voice (en-US, Jessa24kRUS)'>
    Hello, world!
</voice></speak>
```

### <a name="renewing-authorization"></a>Autorisatie vernieuwen

Het verificatietoken verloopt na tien minuten. Autorisatie vernieuwen door het verkrijgen van een nieuw token voordat het verloopt, bijvoorbeeld na 9 minuten. 

De volgende C#-code is een vervanging drop-in voor de klasse die eerder is weergegeven. De `Authentication` klasse automatisch verkrijgt een nieuw toegangstoken om 9 minuten een timer te gebruiken. Deze aanpak zorgt ervoor dat een geldig token altijd beschikbaar is terwijl uw programma wordt uitgevoerd.

> [!NOTE]
> In plaats van een timer te gebruiken, kunt u een timestamp van wanneer het huidige token is verkregen, is een nieuwe sleutel aanvragen alleen als het huidige token is bijna verloopt opslaan. Deze aanpak voorkomt u onnodig aanvragen van de nieuwe tokens en is mogelijk beter geschikt voor programma's die incidentele spraak-aanvragen.

Als voorheen, zorg ervoor dat de `FetchTokenUri` waarde komt overeen met de regio van uw abonnement. Uw abonnementssleutel doorgegeven bij het instantiëren van de klasse.

```cs
    /*
     * This class demonstrates how to maintain a valid access token.
     */
    public class Authentication
    {
        public static readonly string FetchTokenUri = 
            "https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken";
        private string subscriptionKey;
        private string token;
        private Timer accessTokenRenewer;

        //Access token expires every 10 minutes. Renew it every 9 minutes.
        private const int RefreshTokenDuration = 9;

        public Authentication(string subscriptionKey)
        {
            this.subscriptionKey = subscriptionKey;
            this.token = FetchToken(FetchTokenUri, subscriptionKey).Result;

            // renew the token on set duration.
            accessTokenRenewer = new Timer(new TimerCallback(OnTokenExpiredCallback),
                                           this,
                                           TimeSpan.FromMinutes(RefreshTokenDuration),
                                           TimeSpan.FromMilliseconds(-1));
        }

        public string GetAccessToken()
        {
            return this.token;
        }

        private void RenewAccessToken()
        {
            this.token = FetchToken(FetchTokenUri, this.subscriptionKey).Result;
            Console.WriteLine("Renewed token.");
        }

        private void OnTokenExpiredCallback(object stateInfo)
        {
            try
            {
                RenewAccessToken();
            }
            catch (Exception ex)
            {
                Console.WriteLine(string.Format("Failed renewing access token. Details: {0}", ex.Message));
            }
            finally
            {
                try
                {
                    accessTokenRenewer.Change(TimeSpan.FromMinutes(RefreshTokenDuration), TimeSpan.FromMilliseconds(-1));
                }
                catch (Exception ex)
                {
                    Console.WriteLine(string.Format("Failed to reschedule the timer to renew access token. Details: {0}", ex.Message));
                }
            }
        }

        private async Task<string> FetchToken(string fetchUri, string subscriptionKey)
        {
            using (var client = new HttpClient())
            {
                client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", subscriptionKey);
                UriBuilder uriBuilder = new UriBuilder(fetchUri);

                var result = await client.PostAsync(uriBuilder.Uri.AbsoluteUri, null);
                Console.WriteLine("Token Uri: {0}", uriBuilder.Uri.AbsoluteUri);
                return await result.Content.ReadAsStringAsync();
            }
        }
    }
```

## <a name="next-steps"></a>Volgende stappen

- [Uw proefabonnement voor Speech ophalen](https://azure.microsoft.com/try/cognitive-services/)
- [Akoestische modellen aanpassen](how-to-customize-acoustic-models.md)
- [Taalmodellen aanpassen](how-to-customize-language-model.md)

