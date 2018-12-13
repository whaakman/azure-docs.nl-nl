---
title: Speech Services REST-API's - spraakservices
titleSuffix: Azure Cognitive Services
description: Informatie over het gebruik van de spraak-naar-tekst en spraak REST-API's. In dit artikel leert u over de opties voor autorisatie, opties voor query's, het structureren van een aanvraag en antwoord heeft ontvangen.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: 5a3c160fcb550fc4f0c92145733aa993b95bd112
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53089341"
---
# <a name="speech-service-rest-apis"></a>Speech Service REST API 's

Als alternatief voor de [spraak SDK](speech-sdk.md), de Speech-Service kunt u Converteer spraak naar tekst en tekst naar spraak met een set REST-API's. Elk eindpunt dat toegankelijk is, is gekoppeld aan een regio. Uw toepassing is een abonnementssleutel vereist voor het eindpunt dat u van plan bent te gebruiken.

Voordat u de REST-API's te begrijpen:
* De spraak-naar-tekst-aanvragen met behulp van de REST-API kunnen slechts tien seconden opgenomen audio bevatten.
* De spraak-naar-tekst REST-API retourneert alleen de laatste resultaten. Gedeeltelijke resultaten zijn niet opgegeven.
* De Text to Speech REST-API vereist autorisatie-header. Dit betekent dat u voltooien van de uitwisseling van een token moet voor toegang tot de service. Zie [Verificatie](#authentication) voor meer informatie.

## <a name="authentication"></a>Verificatie

Elke aanvraag aan een van beide de spraak-naar-tekst of tekst naar spraak REST-API vereist autorisatie-header. Deze tabel ziet u welke headers voor elke service worden ondersteund:

| Ondersteunde autorisatie-header | Spraak-naar-tekst | Tekst naar spraak |
|------------------------|----------------|----------------|
| OCP-Apim-Subscription-Key | Ja | Nee |
| Autorisatie: Bearer | Ja | Ja |

Wanneer u de `Ocp-Apim-Subscription-Key` header, u hoeft zich alleen voor de abonnementssleutel van uw. Bijvoorbeeld:

```
'Ocp-Apim-Subscription-Key': 'YOUR_SUBSCRIPTION_KEY'
```

Wanneer u de `Authorization: Bearer` header, u bent vereist voor het maken van een aanvraag naar de `issueToken` eindpunt. In deze aanvraag, moet u uw abonnementssleutel voor een toegangstoken dat is geldig voor 10 minuten uitwisselen. In de volgende gedeelten leert u hoe u een token verkrijgen, gebruikt u een token en vernieuwen van een token.

### <a name="how-to-get-an-access-token"></a>Over het verkrijgen van een toegangstoken

Voor een toegangstoken, moet u een aanvraag naar de `issueToken` eindpunt met behulp van de `Ocp-Apim-Subscription-Key` en de abonnementssleutel van uw.

Deze regio's en -eindpunten worden ondersteund:

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-token-service.md)]

Gebruik deze voorbeelden om u te maken van uw aanvraag voor een toegangstoken.

#### <a name="http-sample"></a>HTTP-voorbeeld

In dit voorbeeld is een eenvoudige HTTP-aanvraag voor een token verkrijgen. Vervang `YOUR_SUBSCRIPTION_KEY` met uw abonnementssleutel Speech-Service. Als uw abonnement komt niet in de regio VS-West, vervangt u de `Host` -header met de hostnaam van uw regio.

```http
POST /sts/v1.0/issueToken HTTP/1.1
Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY
Host: westus.api.cognitive.microsoft.com
Content-type: application/x-www-form-urlencoded
Content-Length: 0
```

De hoofdtekst van het antwoord bevat het toegangstoken in de indeling van de Java Web Token (JWT).

#### <a name="powershell-sample"></a>Voorbeeld van PowerShell

In dit voorbeeld is een eenvoudige PowerShell-script voor een toegangstoken. Vervang `YOUR_SUBSCRIPTION_KEY` met uw abonnementssleutel Speech-Service. Zorg ervoor dat u het juiste eindpunt voor de regio die overeenkomt met uw abonnement. In dit voorbeeld is momenteel ingesteld op VS-West.

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

#### <a name="curl-sample"></a>cURL voorbeeld

cURL is een opdrachtregelprogramma beschikbaar in Linux (en in de Windows-subsysteem voor Linux). Deze cURL-opdracht laat zien hoe een toegangstoken. Vervang `YOUR_SUBSCRIPTION_KEY` met uw abonnementssleutel Speech-Service. Zorg ervoor dat u het juiste eindpunt voor de regio die overeenkomt met uw abonnement. In dit voorbeeld is momenteel ingesteld op VS-West.

```cli
curl -v -X POST
 "https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken" \
 -H "Content-type: application/x-www-form-urlencoded" \
 -H "Content-Length: 0" \
 -H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY"
```

#### <a name="c-sample"></a>C#-voorbeeld

Dit C# klasse ziet u hoe u een toegangstoken. Uw abonnementssleutel Speech Service doorgeven als u een exemplaar maken van de klasse. Als uw abonnement komt niet in de regio VS-West, wijzigt u de waarde van `FetchTokenUri` zodat deze overeenkomen met de regio voor uw abonnement.

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

### <a name="how-to-use-an-access-token"></a>Het gebruik van een toegangstoken

Het toegangstoken moet worden verzonden naar de service als de `Authorization: Bearer <TOKEN>` header. Elke toegangstoken is geldig voor 10 minuten. Krijgt u een nieuw token op elk gewenst moment, om te beperken van netwerkverkeer en de latentie, raden we echter aan met dezelfde token negen minuten.

Hier volgt een voorbeeld-HTTP-aanvraag naar de Text to Speech REST-API:

```http
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

### <a name="how-to-renew-an-access-token-using-c"></a>Het vernieuwen van een access token gebruikenC#

Dit C# code is een vervanging drop-in voor de klasse die eerder is weergegeven. De `Authentication` klasse ontvangt automatisch een nieuw toegangstoken om 9 minuten met behulp van een timer. Deze aanpak zorgt ervoor dat een geldig token altijd beschikbaar is terwijl uw programma wordt uitgevoerd.

> [!NOTE]
> In plaats van een timer te gebruiken, kunt u een timestamp van wanneer de laatste token is verkregen opslaan. U kunt vervolgens een nieuwe aanvragen alleen als het bijna verloopt. Deze aanpak voorkomt u onnodig aanvragen van de nieuwe tokens en mogelijk beter geschikt voor programma's die incidentele spraak-aanvragen.

Als voorheen, zorg ervoor dat de `FetchTokenUri` waarde komt overeen met de regio van uw abonnement. Uw abonnementssleutel doorgeven als u een exemplaar maken van de klasse.

```cs
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

## <a name="speech-to-text-api"></a>Speech to text-API

De spraak-naar-tekst REST-API biedt alleen ondersteuning voor korte uitingen. Aanvragen kunnen maximaal 10 seconden van audio met een totale duur van 14 seconden bevatten. De REST-API retourneert alleen de eindresultaten, niet geheel of gedeeltelijk tussentijdse resultaten.

Als verzendt langer audio is vereist voor uw toepassing, kunt u overwegen de [spraak SDK](speech-sdk.md) of [batch transcriptie](batch-transcription.md).

### <a name="regions-and-endpoints"></a>Regio's en -eindpunten

Deze regio's worden ondersteund voor transcriptie van spraak-naar-tekst met behulp van de REST-API. Zorg ervoor dat u het eindpunt dat overeenkomt met de regio van uw abonnement selecteert.

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-speech-to-text.md)]

### <a name="query-parameters"></a>Queryparameters

Deze parameters kunnen worden opgenomen in de query-tekenreeks van de REST-aanvraag.

| Parameter | Description | Vereiste / optioneel |
|-----------|-------------|---------------------|
| `language` | Hiermee geeft u de gesproken taal die wordt herkend. Zie [ondersteunde talen](language-support.md#speech-to-text). | Vereist |
| `format` | Hiermee geeft u de resultaatindeling. Geaccepteerde waarden zijn `simple` en `detailed`. Eenvoudige tot de resultaten behoren `RecognitionStatus`, `DisplayText`, `Offset`, en `Duration`. Gedetailleerde antwoorden bevatten meerdere resultaten met vertrouwen waarden en vier verschillende manieren. De standaardinstelling is `simple`. | Optioneel |
| `profanity` | Geeft aan hoe grof taalgebruik in herkenningsresultaten worden verwerkt. Geaccepteerde waarden zijn `masked`, die grof taalgebruik vervangen door sterretjes, `removed`, die alle grof taalgebruik verwijderen uit het resultaat, of `raw`, waaronder de grof taalgebruik in het resultaat. De standaardinstelling is `masked`. | Optioneel |

### <a name="request-headers"></a>Aanvraagheaders

Deze tabel bevat de vereiste en optionele headers voor spraak-naar-tekst-aanvragen.

|Header| Description | Vereiste / optioneel |
|------|-------------|---------------------|
| `Ocp-Apim-Subscription-Key` | Uw abonnementssleutel Speech-Service. | Een van beide deze header of `Authorization` is vereist. |
| `Authorization` | Een verificatietoken voorafgegaan door het woord `Bearer`. Zie [Verificatie](#authentication) voor meer informatie. | Een van beide deze header of `Ocp-Apim-Subscription-Key` is vereist. |
| `Content-type` | Beschrijft de indeling en codec van de opgegeven gegevens. Geaccepteerde waarden zijn `audio/wav; codec=audio/pcm; samplerate=16000` en `audio/ogg; codec=audio/pcm; samplerate=16000`. | Vereist |
| `Transfer-Encoding` | Hiermee geeft u op of gesegmenteerde audiogegevens wordt verzonden, in plaats van één bestand. Gebruik alleen deze header als audiogegevens logische groepen te verdelen. | Optioneel |
| `Expect` | Als u gesegmenteerde overdracht, stuurt u `Expect: 100-continue`. De Spraakservice erkent de eerste aanvraag en wacht op de aanvullende gegevens.| Vereist als het gesegmenteerde audiogegevens verzenden. |
| `Accept` | Indien opgegeven, moet deze `application/json`. De spraak-Service bevat de resultaten in JSON. Sommige Web aanvraag frameworks bieden een niet-compatibele standaardwaarde als u niets opgeeft, dus het is raadzaam om altijd opnemen `Accept`. | Optioneel maar aanbevolen. |

### <a name="audio-formats"></a>Audio-indelingen

Audio wordt verzonden in de hoofdtekst van de HTTP `POST` aanvraag. Het moet zich binnen een van de indelingen in deze tabel:

| Indeling | Codec | Bitrate | Samplefrequentie |
|--------|-------|---------|-------------|
| WAV | PCM | 16-bits | 16 kHz, mono |
| OGG | OPUS | 16-bits | 16 kHz, mono |

>[!NOTE]
>De bovenstaande indelingen worden ondersteund via REST-API en WebSocket in de Speech-Service. De [spraak SDK](speech-sdk.md) momenteel alleen ondersteunt de WAV opmaken met PCM codec.

### <a name="sample-request"></a>Voorbeeld van een aanvraag

Dit is een typische HTTP-aanvraag. Het onderstaande voorbeeld bevat de hostnaam en de vereiste headers. Het is belangrijk te weten dat de service ook audiogegevens, die niet is opgenomen in dit voorbeeld wordt verwacht. Zoals vermeld eerder, wordt logische groepen te verdelen aanbevolen, maar niet vereist.

```HTTP
POST speech/recognition/conversation/cognitiveservices/v1?language=en-US&format=detailed HTTP/1.1
Accept: application/json;text/xml
Content-Type: audio/wav; codec=audio/pcm; samplerate=16000
Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY
Host: westus.stt.speech.microsoft.com
Transfer-Encoding: chunked
Expect: 100-continue
```

### <a name="http-status-codes"></a>HTTP-statuscodes

De HTTP-statuscode voor elke reactie geeft aan dat het slagen of veelvoorkomende fouten.

| HTTP-statuscode | Description | Mogelijke oorzaak |
|------------------|-------------|-----------------|
| 100 | Doorgaan | De eerste aanvraag is geaccepteerd. Doorgaan met het verzenden van de rest van de gegevens. (Met gesegmenteerde overdracht gebruikt.) |
| 200 | OK | De aanvraag is uitgevoerd. de antwoordtekst is een JSON-object. |
| 400 | Ongeldig verzoek | De taalcode die niet opgegeven of is niet een ondersteunde taal; Ongeldig audio-bestand. |
| 401 | Niet geautoriseerd | Abonnementssleutel of autorisatie-token is ongeldig in de regio is opgegeven of ongeldig eindpunt. |
| 403 | Verboden | Ontbrekende abonnementssleutel of autorisatie token. |

### <a name="chunked-transfer"></a>Gesegmenteerde overdracht

Gesegmenteerde overdrachtscodering overdracht (`Transfer-Encoding: chunked`) kunt u Verminder de latentie van de spraakherkenning omdat hierdoor de Speech-Service om te beginnen met het audiobestand verwerken terwijl deze wordt verzonden. De REST-API biedt geen tijdelijke of gedeeltelijke resultaten. Deze optie is bedoeld uitsluitend het reactievermogen verbeteren.

Dit codevoorbeeld laat zien hoe voor het verzenden van audio in segmenten. Alleen de eerste chunk moet de audio bestands-header bevatten. `request` is een HTTPWebRequest-object dat is verbonden met het juiste REST-eindpunt. `audioFile` is het pad naar een audio-bestand op schijf.

```csharp
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

### <a name="response-parameters"></a>Antwoord-parameters

Resultaten worden gegeven als JSON. De `simple` indeling omvat deze velden op het hoogste niveau.

| Parameter | Description  |
|-----------|--------------|
|`RecognitionStatus`|Status, zoals `Success` voor geslaagde opname. Zie de volgende tabel.|
|`DisplayText`|De herkende tekst na het hoofdlettergebruik, interpunctie, inverse tekst normalisering (conversie van de gesproken tekst voor kortere formulieren, zoals 200 voor '200' of 'Dr. Smith' voor 'doctor smith'), en grof taalgebruik maskeren. Alleen op succes is geïnstalleerd.|
|`Offset`|De tijd (in eenheden van 100 nanoseconden) waarmee de herkende spraak in de audiostream begint.|
|`Duration`|De duur (in eenheden van 100 nanoseconden) van de herkende spraak in de audio-stream.|

De `RecognitionStatus` veld bevat deze waarden mogelijk:

| Status | Description |
|--------|-------------|
| `Success` | De opname is voltooid en de `DisplayText` veld niet aanwezig is. |
| `NoMatch` | Spraak is gedetecteerd in de audiostream, maar er zijn geen woorden uit de doeltaal zijn afgestemd. Betekent meestal dat de opname-taal is een andere taal dan het account dat de gebruiker spreekt. |
| `InitialSilenceTimeout` | Het begin van de audiostream bevat alleen stilte en de time-out voor spraak-service. |
| `BabbleTimeout` | Het begin van de audiostream bevat alleen ruis, en de time-out voor spraak-service. |
| `Error` | De opname-service is een interne fout opgetreden en kan niet worden voortgezet. Probeer het opnieuw, indien mogelijk. |

> [!NOTE]
> Als de audio alleen uit grof taalgebruik bestaat, en de `profanity` queryparameter is ingesteld op `remove`, de service heeft geen spraak resultaat geretourneerd.

De `detailed` indeling bevat dezelfde gegevens als de `simple` opmaken, samen met `NBest`, een lijst met alternatieve interpretaties van hetzelfde resultaat van de herkenning van spraak. Deze resultaten worden geclassificeerd van meest waarschijnlijke te minste waarschijnlijk de eerste vermelding is hetzelfde als de belangrijkste herkenningsresultaat.  Wanneer u de `detailed` indeling `DisplayText` wordt geleverd als `Display` voor elk resultaat in de `NBest` lijst.

Elk object in de `NBest` lijst bevat:

| Parameter | Description |
|-----------|-------------|
| `Confidence` | De betrouwbaarheidsscore van de vermelding van 0,0 (geen vertrouwen) 1.0 (volledig vertrouwen) |
| `Lexical` | De lexicale vorm van de herkende tekst: de werkelijke woorden herkend. |
| `ITN` | De inverse-normalized-tekst ("canonieke") vorm van de herkende tekst, met telefoon getallen, getallen, afkortingen ("doctor smith' op 'dr smith') en andere transformaties toegepast. |
| `MaskedITN` | Het formulier toevoegen met grof taalgebruik maskering toegepast, indien aangevraagd. |
| `Display` | Het formulier weergegeven van de herkende tekst, met interpunctie hoofdletters en kleine letters toegevoegd. Deze parameter is hetzelfde als `DisplayText` opgegeven als indeling is ingesteld op `simple`. |

### <a name="sample-responses"></a>Voorbeeld van reacties

Dit is een typische antwoord voor `simple` herkenning.

```json
{
  "RecognitionStatus": "Success",
  "DisplayText": "Remind me to buy 5 pencils.",
  "Offset": "1236645672289",
  "Duration": "1236645672289"
}
```

Dit is een typische antwoord voor `detailed` herkenning.

```json
{
  "RecognitionStatus": "Success",
  "Offset": "1236645672289",
  "Duration": "1236645672289",
  "NBest": [
      {
        "Confidence" : "0.87",
        "Lexical" : "remind me to buy five pencils",
        "ITN" : "remind me to buy 5 pencils",
        "MaskedITN" : "remind me to buy 5 pencils",
        "Display" : "Remind me to buy 5 pencils.",
      },
      {
        "Confidence" : "0.54",
        "Lexical" : "rewind me to buy five pencils",
        "ITN" : "rewind me to buy 5 pencils",
        "MaskedITN" : "rewind me to buy 5 pencils",
        "Display" : "Rewind me to buy 5 pencils.",
      }
  ]
}
```

## <a name="text-to-speech-api"></a>Text to Speech-API

Deze regio's worden ondersteund voor tekst naar spraak met behulp van de REST-API. Zorg ervoor dat u het eindpunt dat overeenkomt met de regio van uw abonnement selecteert.

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-text-to-speech.md)]

De spraak-Service ondersteunt 24-KHz-audio-uitvoer, samen met de uitvoer van de 16-Khz die werden ondersteund door Bing Speech. Vier 24-KHz uitvoerindeling op te geven en twee 24-KHz stemmen worden ondersteund.

### <a name="voices"></a>Stemmen

| Landinstelling | Taal   | Geslacht | Toewijzing |
|--------|------------|--------|---------|
| en-US  | Amerikaans-Engels | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (en-US, Jessa24kRUS)" |
| en-US  | Amerikaans-Engels | Man   | "Microsoft Server spraak tekst en spraak, spraak (en-US, Guy24kRUS)" |

Zie voor een volledige lijst met beschikbare stemmen [ondersteunde talen](language-support.md#text-to-speech).

### <a name="request-headers"></a>Aanvraagheaders

Deze tabel bevat de vereiste en optionele headers voor spraak-naar-tekst-aanvragen.

| Header | Description | Vereiste / optioneel |
|--------|-------------|---------------------|
| `Authorization` | Een verificatietoken voorafgegaan door het woord `Bearer`. Zie voor meer informatie, [verificatie](#authentication). | Vereist |
| `Content-Type` | Hiermee geeft u het type inhoud voor de opgegeven tekst. Waarde geaccepteerd: `application/ssml+xml`. | Vereist |
| `X-Microsoft-OutputFormat` | Hiermee geeft u de indeling van de audio-uitvoer. Zie voor een volledige lijst met geaccepteerde waarden [audio-uitvoer](#audio-outputs). | Vereist |
| `User-Agent` | De naam van de toepassing. Het moet minder dan 255 tekens. | Vereist |

### <a name="audio-outputs"></a>Audio-uitvoer

Dit is een lijst met ondersteunde audio-indelingen die worden verzonden in elke aanvraag als de `X-Microsoft-OutputFormat` header. Elk omvat een bitrate en type codering.

|||
|-|-|
| `raw-16khz-16bit-mono-pcm` | `raw-8khz-8bit-mono-mulaw` |
| `riff-8khz-8bit-mono-mulaw` | `riff-16khz-16bit-mono-pcm` |
| `audio-16khz-128kbitrate-mono-mp3` | `audio-16khz-64kbitrate-mono-mp3` |
| `audio-16khz-32kbitrate-mono-mp3`  | `raw-24khz-16bit-mono-pcm` |
| `riff-24khz-16bit-mono-pcm`        | `audio-24khz-160kbitrate-mono-mp3` |
| `audio-24khz-96kbitrate-mono-mp3`  | `audio-24khz-48kbitrate-mono-mp3` |

> [!NOTE]
> Als uw geselecteerde spraak- en de indeling van uitvoer hebt verschillende bitsnelheden, de audio nieuw voorbeeld wordt gemaakt zo nodig. Echter, 24khz stemmen bieden geen ondersteuning voor `audio-16khz-16kbps-mono-siren` en `riff-16khz-16kbps-mono-siren` uitvoerindelingen.

### <a name="request-body"></a>Aanvraagbody

Tekst wordt verzonden als de instantie van een HTTP `POST` aanvraag. Kan het zijn tekst zonder opmaak (ASCII- of UTF-8) of [spraak synthese Markup Language](speech-synthesis-markup.md) (SSML)-indeling (UTF-8). Tekst zonder opmaak aanvragen gebruiken de Spraakservice standaard spraak en taal. Met SSML kunt u de spraak- en taal.

### <a name="sample-request"></a>Voorbeeld van een aanvraag

Deze HTTP-aanvraag maakt gebruik van SSML om op te geven van de spraak- en taal. De hoofdtekst van het niet langer zijn dan 1000 tekens.

```http
POST /cognitiveservices/v1 HTTP/1.1

X-Microsoft-OutputFormat: raw-16khz-16bit-mono-pcm
Content-Type: application/ssml+xml
Host: westus.tts.speech.microsoft.com
Content-Length: 225
Authorization: Bearer [Base64 access_token]

<speak version='1.0' xml:lang='en-US'><voice xml:lang='en-US' xml:gender='Female'
    name='Microsoft Server Speech Text to Speech Voice (en-US, ZiraRUS)'>
        Microsoft Speech Service Text-to-Speech API
</voice></speak>
```

### <a name="http-status-codes"></a>HTTP-statuscodes

De HTTP-statuscode voor elke reactie geeft aan dat het slagen of veelvoorkomende fouten.

| HTTP-statuscode | Description | Mogelijke oorzaak |
|------------------|-------------|-----------------|
| 200 | OK | De aanvraag is uitgevoerd. de antwoordtekst is een geluidsbestand. |
| 400 | Onjuiste aanvraag | Er ontbreekt een vereiste parameter ontbreekt, is leeg of null zijn. Of de waarde die wordt doorgegeven aan een vereiste of optionele parameter is ongeldig. Een veelvoorkomend probleem is een header die te lang is. |
| 401 | Niet geautoriseerd | De aanvraag is niet gemachtigd. Controleer of dat uw abonnementssleutel of token geldig is en in de juiste regio. |
| 413 | Aanvraagentiteit te groot | De invoer SSML is langer dan 1024 tekens. |
| 429 | Te veel aanvragen | U hebt het quotum of het aantal aanvragen dat is toegestaan voor uw abonnement overschreden. |
| 502 | Ongeldige gateway | Netwerk- of serverzijde probleem. Kan ook duiden op ongeldige kopteksten. |

Als de HTTP-status `200 OK`, de hoofdtekst van het antwoord bevat een audio-bestand in de gewenste indeling. Dit bestand kan worden afgespeeld, zoals deze is overgedragen, in een buffer opgeslagen of opgeslagen in een bestand.

## <a name="next-steps"></a>Volgende stappen

- [Uw proefabonnement voor Speech ophalen](https://azure.microsoft.com/try/cognitive-services/)
- [Akoestische modellen aanpassen](how-to-customize-acoustic-models.md)
- [Taalmodellen aanpassen](how-to-customize-language-model.md)
