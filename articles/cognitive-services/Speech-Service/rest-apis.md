---
title: Spraak service REST-API's | Microsoft Docs
description: Documentatie voor REST-API's voor de service spraak.
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.technology: speech
ms.topic: article
ms.date: 05/09/2018
ms.author: v-jerkin
ms.openlocfilehash: e80c69657dfb7cbab7d29c94d3dd3c56574de7b7
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2018
ms.locfileid: "36321993"
---
# <a name="speech-service-rest-apis"></a>Spraak service REST-API 's

De REST-API's van de geïntegreerde spraak-service zijn vergelijkbaar met de API's geleverd door de [Speech-API](https://docs.microsoft.com/azure/cognitive-services/Speech) (voorheen bekend als de Bing Speech-Service). De eindpunten verschillen van de eindpunten die wordt gebruikt door de vorige spraakherkenningsprogramma-service.

## <a name="speech-to-text"></a>Spraak naar tekst

In de spraak tekst API verschillen alleen de eindpunten die gebruikt van de vorige spraak service erkenning Speech-API. De nieuwe eindpunten worden weergegeven in de onderstaande tabel. Gebruik de naam die overeenkomt met de regio van uw abonnement.

Regio| Spraak naar tekst eindpunt
-|-
VS - west| `https://westus.stt.speech.microsoft.com/speech/recognition/conversation/cognitiveservices/v1`
Oost-Azië| `https://eastasia.stt.speech.microsoft.com/speech/recognition/conversation/cognitiveservices/v1`
Noord-Europa| `https://northeurope.stt.speech.microsoft.com/speech/recognition/conversation/cognitiveservices/v1`

> [!NOTE]
> U moet toevoegen aan de gewenste taal in de URI om te voorkomen dat een 401 http-fout. Dus voor en-US moet de juiste URI zou zijn: https://westus.stt.speech.microsoft.com/speech/recognition/conversation/cognitiveservices/v1?language=en-US

De spraak tekst API anders is vergelijkbaar met de [REST-API](https://docs.microsoft.com/azure/cognitive-services/speech/getstarted/getstartedrest) voor de vorige Speech-API.

De spraak tekst REST-API ondersteunt alleen korte utterances. Aanvragen kunnen maximaal 10 seconden audio bevatten en een maximum van 14 seconden algehele laatste. De REST-API retourneert alleen de laatste resultaten niet geheel of gedeeltelijk tussentijdse resultaten.

> [!NOTE]
> Als u de akoestisch model of taalmodel of uitspraak hebt aangepast, kunt u uw aangepaste eindpunt gebruiken.

## <a name="text-to-speech"></a>Tekst naar spraak

De nieuwe Text to Speech API biedt ondersteuning voor 24-KHz audio-uitvoer. De `X-Microsoft-OutputFormat` header kan nu de volgende waarden bevatten.

|||
|-|-|
`raw-16khz-16bit-mono-pcm`         | `audio-16khz-16kbps-mono-siren`
`riff-16khz-16kbps-mono-siren`     | `riff-16khz-16bit-mono-pcm`
`audio-16khz-128kbitrate-mono-mp3` | `audio-16khz-64kbitrate-mono-mp3`
`audio-16khz-32kbitrate-mono-mp3`  | `raw-24khz-16bit-mono-pcm`
`riff-24khz-16bit-mono-pcm`        | `audio-24khz-160kbitrate-mono-mp3`
`audio-24khz-96kbitrate-mono-mp3`  | `audio-24khz-48kbitrate-mono-mp3`

De service spraak biedt nu twee 24 KHz stemmen:

Landinstelling | Taal   | Geslacht | De toewijzing van service
-------|------------|--------|------------
nl-NL  | Nederlands | Vrouw | 'Microsoft Server Speech tekst stem (en-US, Jessa24kRUS)' 
nl-NL  | Nederlands | Man   | 'Microsoft Server Speech tekst stem (en-US, Guy24kRUS)'

Hieronder vindt u de REST-eindpunten voor de geïntegreerde Speech service Text to Speech API. Gebruik het eindpunt dat overeenkomt met de regio van uw abonnement.

Regio| Text to Speech-eindpunt
-|-
VS - west|    `https://westus.tts.speech.microsoft.com/cognitiveservices/v1`
Oost-Azië|  `https://eastasia.tts.speech.microsoft.com/cognitiveservices/v1`
Noord-Europa|   `https://northeurope.tts.speech.microsoft.com/cognitiveservices/v1`

> [!NOTE]
> Als u een aangepaste gesproken-lettertype gemaakt, kunt u uw aangepaste eindpunt gebruiken.

Houd deze verschillen in rekening wanneer u naar verwijst de [REST API-documentatie](https://docs.microsoft.com/azure/cognitive-services/speech/api-reference-rest/bingvoiceoutput) voor de vorige Speech-API.

## <a name="authentication"></a>Verificatie

Verzenden van een aanvraag van de service spraak REST API vereist dat een toegangstoken. U een token verkrijgen door uw abonnementssleutel naar een regionaal spraak service `issueToken` eindpunt, weergegeven in de onderstaande tabel. Gebruik het eindpunt dat overeenkomt met de regio van uw abonnement.

Regio| Token service-eindpunt
-|-
VS - west|    `https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken`
Oost-Azië|  `https://eastasia.api.cognitive.microsoft.com/sts/v1.0/issueToken`
Noord-Europa|   `https://northeurope.api.cognitive.microsoft.com/sts/v1.0/issueToken`

Elke toegangstoken is geldig voor 10 minuten. U kunt een nieuw token verkrijgen op elk gewenst moment, inclusief, indien gewenst, net voor elke aanvraag spraak REST-API. Om te beperken netwerkverkeer en de latentie, echter aangeraden hetzelfde token voor negen minuten.

De volgende secties laten zien hoe een token verkrijgen en het gebruik ervan in een aanvraag.

### <a name="getting-a-token-http"></a>Ophalen van een token: HTTP

Hieronder volgt een voorbeeld HTTP-aanvraag voor het verkrijgen van een token. Vervang `YOUR_SUBSCRIPTION_KEY` met uw abonnementssleutel voor spraak-service. Als uw abonnement niet in de regio VS-West is, vervangt u de `Host` header met de hostnaam van uw regio.

```
POST /sts/v1.0/issueToken HTTP/1.1
Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY
Host: westus.api.cognitive.microsoft.com
Content-type: application/x-www-form-urlencoded
Content-Length: 0
```

De hoofdtekst van het antwoord op deze aanvraag is het toegangstoken in Java Web Token (JWT)-indeling.

### <a name="getting-a-token-powershell"></a>Ophalen van een token: PowerShell

De onderstaande Windows PowerShell-script laat zien hoe een toegangstoken te verkrijgen. Vervang `YOUR_SUBSCRIPTION_KEY` met uw abonnementssleutel voor spraak-service. Als uw abonnement niet in de regio VS-West is, wijzigt u de hostnaam van de opgegeven URI dienovereenkomstig in.

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

cURL is een opdrachtregelprogramma beschikbaar in Linux (en in het Windows-subsysteem voor Linux). De volgende cURL-opdracht laat zien hoe een toegangstoken te verkrijgen. Vervang `YOUR_SUBSCRIPTION_KEY` met uw abonnementssleutel voor spraak-service. Als uw abonnement niet in de regio VS-West is, wijzigt u de hostnaam van de opgegeven URI dienovereenkomstig in.

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

De C#-klasse hieronder ziet u het verkrijgen van een toegangstoken. Uw spraak service-abonnementssleutel doorgeven bij het instantiëren van de klasse. Als uw abonnement niet in de regio VS-West is, wijzigt u de hostnaam van `FetchTokenUri` op de juiste wijze.

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

Geef voor het gebruik van een token in een REST-API-aanvraag, in de `Authorization` -kop, volgen op het woord `Bearer`. Hier is bijvoorbeeld een steekproef tekst spraak REST-aanvraag met een token. Vervang uw werkelijke token voor `YOUR_ACCESS_TOKEN` en gebruik de juiste hostnaam in de `Host` header.

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

### <a name="renewing-authorization"></a>Vernieuwen van autorisatie

Het verificatietoken verloopt na 10 minuten. Vernieuwen van uw autorisatie door een nieuw token verkrijgen voordat het verloopt, bijvoorbeeld na negen minuten. 

De volgende C#-code is een onverwachte vervanging voor de klasse die eerder is weergegeven. De `Authentication` klasse automatisch verkrijgt een nieuw toegangstoken om negen minuten met behulp van een timer. Deze aanpak zorgt ervoor dat een geldig token altijd beschikbaar is terwijl het programma wordt uitgevoerd.

> [!NOTE]
> In plaats van een timer, kunt u een tijdstempel van wanneer token van de huidige is verkregen en vervolgens een nieuwe aanvraag alleen als het huidige token is bijna verloopt opslaan. Deze aanpak voorkomt nieuwe tokens onnodig aanvragen en is mogelijk beter geschikt voor programma's die incidentele spraak aanvragen indienen.

Als voorheen, zorg ervoor dat de `FetchTokenUri` waarde komt overeen met de regio van uw abonnement. De abonnementssleutel van uw doorgeven bij het instantiëren van de klasse.

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

* [Uw proefabonnement spraak ophalen](https://azure.microsoft.com/try/cognitive-services/)
* [Informatie over het aanpassen van een model spraak](how-to-customize-speech-models.md)
