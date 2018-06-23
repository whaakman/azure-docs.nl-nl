---
title: Aanmelden bij de Service Microsoft Speech | Microsoft Docs
description: Aanvraag-verificatie te gebruiken Microsoft Speech-API
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/15/2017
ms.author: zhouwang
ms.openlocfilehash: e36168cf3ff938af44f1028c2d26fd475d60b148
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344683"
---
# <a name="authenticate-to-the-speech-api"></a>Authenticatie bij de Speech-API

Spraak Service ondersteunt verificatie met behulp van:

- De abonnementssleutel van een.
- Een verificatietoken.

## <a name="use-a-subscription-key"></a>Een abonnementssleutel wordt gebruikt

Spraak als Service wilt gebruiken, moet u eerst abonneren op de Speech-API die deel uitmaakt van cognitieve Services (eerder Project Oxford). Krijgt u gratis proefabonnement sleutels van de [abonnement cognitieve Services](https://azure.microsoft.com/try/cognitive-services/) pagina. Nadat u de Speech-API selecteert, selecteert u **API-sleutel ophalen** ophalen van de sleutel. Het resultaat een primaire en secundaire sleutel. Beide sleutels zijn gekoppeld aan hetzelfde contingent, zodat u van sleutel kunt gebruiken.

Voor de lange termijn gebruiken of een grotere quota, zich aanmelden voor een [Azure-account](https://azure.microsoft.com/free/).

De REST-API-spraak, hebt u nodig om door te geven van de abonnementssleutel in de `Ocp-Apim-Subscription-Key` veld in de aanvraagheader.

Naam| Indeling| Beschrijving
----|-------|------------
OCP-Apim-Subscription-Key | ASCII | YOUR_SUBSCRIPTION_KEY

Hier volgt een voorbeeld van een aanvraagheader:

```HTTP
POST https://speech.platform.bing.com/speech/recognition/interactive/cognitiveservices/v1?language=en-US&format=detailed HTTP/1.1
Accept: application/json;text/xml
Content-Type: audio/wav; codec=audio/pcm; samplerate=16000
Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY
Host: speech.platform.bing.com
Transfer-Encoding: chunked
Expect: 100-continue
```

> [!IMPORTANT]
> Als u [clientbibliotheken](../GetStarted/GetStartedClientLibraries.md) in uw toepassing controleren dat u het verificatietoken met de abonnementssleutel van uw krijgen kunt, zoals beschreven in de volgende sectie. De abonnement-toets de clientbibliotheken gebruiken een verificatietoken ophalen en het token vervolgens gebruiken voor verificatie.

## <a name="use-an-authorization-token"></a>Een verificatietoken gebruiken

U kunt ook een verificatietoken voor verificatie gebruiken als bewijs van verificatie/autorisatie. Als u dit token, moet u eerst aanvragen de abonnementssleutel van een van de Speech-API, zoals beschreven in de [voorgaande sectie](#use-a-subscription-key).

### <a name="get-an-authorization-token"></a>Een verificatietoken ophalen

Nadat u de sleutel van een geldig abonnement hebt, moet u een POST-aanvraag verzenden naar de service voor beveiligingstokens van cognitieve Services. In het antwoord verschijnt het verificatietoken als een JSON Web Token (JWT).

> [!NOTE]
> Het token heeft een vervaldatum van 10 minuten. Zie de volgende sectie voor het vernieuwen van het token.

De token service URI zich hier bevindt:

```
https://api.cognitive.microsoft.com/sts/v1.0/issueToken
```

De volgende voorbeeldcode laat zien hoe een toegangstoken ophalen. Vervang `YOUR_SUBSCRIPTION_KEY` met de abonnementssleutel van uw eigen:

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/Powershell)

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

Het voorbeeld wordt curl op Linux met bash. Als deze niet beschikbaar op uw platform is, moet u mogelijk curl installeren. In het voorbeeld werkt ook op Cygwin op Windows, Git Bash, zsh en andere shells.

```
curl -v -X POST "https://api.cognitive.microsoft.com/sts/v1.0/issueToken" -H "Content-type: application/x-www-form-urlencoded" -H "Content-Length: 0" -H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY"
```

# <a name="ctabcsharp"></a>[C#](#tab/CSharp)

```cs
    /*
     * This class demonstrates how to get a valid access token.
     */
    public class Authentication
    {
        public static readonly string FetchTokenUri = "https://api.cognitive.microsoft.com/sts/v1.0";
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
                uriBuilder.Path += "/issueToken";

                var result = await client.PostAsync(uriBuilder.Uri.AbsoluteUri, null);
                Console.WriteLine("Token Uri: {0}", uriBuilder.Uri.AbsoluteUri);
                return await result.Content.ReadAsStringAsync();
            }
        }
    }
```

---

Hier volgt een voorbeeld van POST-aanvraag:

```HTTP
POST https://api.cognitive.microsoft.com/sts/v1.0/issueToken HTTP/1.1
Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY
Host: api.cognitive.microsoft.com
Content-type: application/x-www-form-urlencoded
Content-Length: 0
Connection: Keep-Alive
```

Als u niet een autorisatie token van de service voor beveiligingstokens, Controleer of de abonnementssleutel van uw nog geldig is. Als u een gratis proefversie sleutel gebruikt, gaat u naar de [abonnement cognitieve Services](https://azure.microsoft.com/try/cognitive-services/) pagina, klikt u op 'Aanmelden' om te melden met behulp van het account dat u gebruikt voor het toepassen van de gratis proefversie sleutel en controleer of de abonnementssleutel is verlopen of is groter dan de quota.

### <a name="use-an-authorization-token-in-a-request"></a>Een verificatietoken gebruiken in een aanvraag

Elke keer dat u de Speech-API aanroepen, u moet doorgeven van de Autorisatietoken in de `Authorization` header. De `Authorization` header moet een toegangstoken JWT bevatten.

Het volgende voorbeeld laat zien hoe een verificatietoken gebruiken wanneer u de spraak REST-API aanroept.

> [!NOTE]
> Vervang `YOUR_AUDIO_FILE` met het pad naar uw vooraf opgenomen audio-bestand. Vervang `YOUR_ACCESS_TOKEN` door het verificatietoken dat u in de vorige stap verkregen [een verificatietoken ophalen](#get-an-authorization-token).

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/Powershell)

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

# <a name="ctabcsharp"></a>[C#](#tab/CSharp)

```cs
HttpWebRequest request = null;
request = (HttpWebRequest)HttpWebRequest.Create(requestUri);
request.SendChunked = true;
request.Accept = @"application/json;text/xml";
request.Method = "POST";
request.ProtocolVersion = HttpVersion.Version11;
request.Host = @"speech.platform.bing.com";
request.ContentType = @"audio/wav; codec=audio/pcm; samplerate=16000";
request.Headers["Authorization"] = "Bearer " + token;

// Send an audio file by 1024 byte chunks
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

---

### <a name="renew-an-authorization-token"></a>Een verificatietoken vernieuwen

Het verificatietoken verloopt na een bepaalde periode (momenteel 10 minuten). U moet het verificatietoken te verlengen voordat deze verloopt.

De volgende code is een voorbeeld van de implementatie in C# van het vernieuwen van het verificatietoken:

```cs
    /*
     * This class demonstrates how to get a valid O-auth token.
     */
    public class Authentication
    {
        public static readonly string FetchTokenUri = "https://api.cognitive.microsoft.com/sts/v1.0";
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
                uriBuilder.Path += "/issueToken";

                var result = await client.PostAsync(uriBuilder.Uri.AbsoluteUri, null);
                Console.WriteLine("Token Uri: {0}", uriBuilder.Uri.AbsoluteUri);
                return await result.Content.ReadAsStringAsync();
            }
        }
    }
```
