---
title: Bing Speech verifiëren | Microsoft Docs
titlesuffix: Azure Cognitive Services
description: Verificatie van de aanvraag naar de Bing Speech-API gebruiken
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/18/2018
ms.author: zhouwang
ms.openlocfilehash: 93171dba435172565215560e437dc56c9436baac
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46366451"
---
# <a name="authenticate-to-the-speech-api"></a>Verifiëren bij de spraak-API

Bing Speech biedt ondersteuning voor verificatie met behulp van:

- De abonnementssleutel van een.
- Een verificatietoken.

## <a name="use-a-subscription-key"></a>Gebruik een abonnementssleutel

Voor het gebruik van spraak-Service, moet u eerst abonneren op de spraak-API die deel uitmaakt van Cognitive Services (eerder Project Oxford). U krijgt een gratis proefabonnement sleutels uit de [Cognitive Services-abonnement](https://azure.microsoft.com/try/cognitive-services/) pagina. Nadat u de spraak-API selecteert, selecteert u **API-sleutel ophalen** om op te halen van de sleutel. Het resultaat een primaire en secundaire sleutel. Beide sleutels zijn gekoppeld aan dezelfde quota, zodat u beide sleutels kunt gebruiken.

Voor langdurig gebruik of een verbeterd quotum, zich aanmelden voor een [Azure-account](https://azure.microsoft.com/free/).

Voor het gebruik van de REST-API voor spraakherkenning, moet u de abonnementssleutel in doorgeven de `Ocp-Apim-Subscription-Key` veld in de aanvraagheader.

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
> Als u [clientbibliotheken](../GetStarted/GetStartedClientLibraries.md) in uw toepassing, Controleer of dat u het Autorisatietoken met de abonnementssleutel van uw krijgen kunt, zoals beschreven in de volgende sectie. De clientbibliotheken gebruiken de abonnementssleutel een Autorisatietoken verkrijgen en het token vervolgens gebruiken voor verificatie.

## <a name="use-an-authorization-token"></a>Gebruik een verificatietoken

U kunt ook een verificatietoken voor verificatie gebruiken als bewijs van verificatie/autorisatie. Als u dit token, moet u eerst een abonnementssleutel bij aanschaffen de spraak-API, zoals beschreven in de [voorgaande sectie](#use-a-subscription-key).

### <a name="get-an-authorization-token"></a>Een verificatietoken ophalen

Nadat u de sleutel van een geldig abonnement hebt, moet u een POST-aanvraag verzenden naar de service voor beveiligingstokens van Cognitive Services. In het antwoord ontvangt u het Autorisatietoken als een JSON Web Token (JWT).

> [!NOTE]
> Het token heeft een vervaldatum van 10 minuten. Als u wilt vernieuwen het token, Zie de volgende sectie.

De token service-URI bevindt zich hier:

```
https://api.cognitive.microsoft.com/sts/v1.0/issueToken
```

Het volgende codevoorbeeld laat zien hoe u een toegangstoken. Vervang `YOUR_SUBSCRIPTION_KEY` door uw eigen abonnementssleutel:

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

# <a name="curltabcurl"></a>[CURL](#tab/curl)

Het voorbeeld wordt curl gebruikt op Linux met bash. Als deze niet beschikbaar is op uw platform, moet u mogelijk curl installeren. Het voorbeeld werkt ook op Cygwin op Windows, Git Bash, zsh en andere shells.

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

Als u niet een autorisatie token van de service voor beveiligingstokens, moet u controleren of uw abonnementssleutel nog geldig is. Als u een gratis proefversie sleutel gebruikt, gaat u naar de [Cognitive Services-abonnement](https://azure.microsoft.com/try/cognitive-services/) pagina, klikt u op 'Aanmelden' als u wilt zich aanmelden met het account dat u gebruikt voor het toepassen van de gratis proefversie sleutel en controleer of de abonnementssleutel is verlopen of is groter dan de quotum.

### <a name="use-an-authorization-token-in-a-request"></a>Een verificatietoken gebruiken in een aanvraag

Telkens wanneer u de spraak-API aanroept, moet u het autorisatie-token in de `Authorization` header. De `Authorization` header moet een JWT-toegangstoken bevatten.

Het volgende voorbeeld ziet hoe u een verificatietoken gebruikt wanneer u de Speech REST-API aanroept.

> [!NOTE]
> Vervang `YOUR_AUDIO_FILE` met het pad naar uw vooraf opgenomen audiobestand. Vervang `YOUR_ACCESS_TOKEN` door het Autorisatietoken u in de vorige stap hebt verkregen [een verificatietoken ophalen](#get-an-authorization-token).

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

# <a name="curltabcurl"></a>[CURL](#tab/curl)

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

### <a name="renew-an-authorization-token"></a>Een verificatietoken te vernieuwen

Het verificatietoken is verlopen na een bepaalde periode (momenteel 10 minuten). U moet het verificatietoken te vernieuwen voordat het verloopt.

De volgende code is een voorbeeld van de implementatie in C# van hoe u om de Autorisatietoken te verlengen:

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
