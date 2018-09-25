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
ms.openlocfilehash: 6758cd658daf75beeea93bf9c719508cd271c8be
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47032424"
---
# <a name="speech-service-rest-apis"></a>Spraakservice REST-API 's

De REST-API's van de Azure Cognitive Services Speech service zijn vergelijkbaar met de API's ontwerpgereedschappen van unified de [Bing Speech-API](https://docs.microsoft.com/azure/cognitive-services/Speech). De eindpunten afwijken van de eindpunten die worden gebruikt door de Bing Speech-service. Regionale eindpunten beschikbaar zijn en moet u een abonnementssleutel die overeenkomt met het eindpunt dat u gebruikt.

## <a name="speech-to-text"></a>Spraak naar tekst

De eindpunten voor de spraak-naar-tekst REST-API worden in de volgende tabel weergegeven. Gebruik de naam die overeenkomt met de regio van uw abonnement.

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-speech-to-text.md)]

> [!NOTE]
> Als u het akoestisch model of de taalmodel of de uitspraak van aangepast, kunt u uw aangepast eindpunt gebruiken.

Deze API ondersteunt alleen korte uitingen. Aanvragen kunnen maximaal 10 seconden audio bevatten en een maximum van 14 seconden algehele laatste. De REST-API retourneert alleen de laatste resultaten, geen tijdelijke of gedeeltelijke resultaten. De spraak-service heeft ook een [batch transcriptie](batch-transcription.md) API die langer audio kunt transcriberen.

### <a name="query-parameters"></a>Queryparameters

De volgende parameters kunnen worden opgenomen in de querytekenreeks van de REST-aanvraag.

|Parameternaam|Vereist/optioneel|Betekenis|
|-|-|-|
|`language`|Vereist|De id van de taal die moet worden herkend. Zie [ondersteunde talen](supported-languages.md#speech-to-text).|
|`format`|Optioneel<br>Standaard: `simple`|Resultaat opmaken, `simple` of `detailed`. Eenvoudige tot de resultaten behoren `RecognitionStatus`, `DisplayText`, `Offset`, en de duur. Gedetailleerde resultaten bevatten meerdere kandidaten met vertrouwen waarden en vier verschillende manieren.|
|`profanity`|Optioneel<br>Standaard: `masked`|Klik hier voor meer informatie over het afhandelen van grof taalgebruik in herkenningsresultaten. Mogelijk `masked` (grof taalgebruik vervangen door sterretjes), `removed` (Hiermee verwijdert u alle grof taalgebruik,) of `raw` (inclusief grof taalgebruik).

### <a name="request-headers"></a>Aanvraagheaders

De volgende velden worden in de HTTP-aanvraagheader verzonden.

|Koptekst|Betekenis|
|------|-------|
|`Ocp-Apim-Subscription-Key`|Uw abonnementssleutel van spraak-service. Een van beide deze header of `Authorization` moet worden opgegeven.|
|`Authorization`|Een verificatietoken voorafgegaan door het woord `Bearer`. Een van beide deze header of `Ocp-Apim-Subscription-Key` moet worden opgegeven. Zie [verificatie](#authentication).|
|`Content-type`|Beschrijft de indeling en codec van de gegevens. Op dit moment deze waarde moet `audio/wav; codec=audio/pcm; samplerate=16000`.|
|`Transfer-Encoding`|Optioneel. Indien opgegeven, moet `chunked` om toe te staan van audiogegevens in meerdere kleine segmenten in plaats van één bestand worden verzonden.|
|`Expect`|Als u gesegmenteerde overdracht, stuurt u `Expect: 100-continue`. De spraakservice erkent de eerste aanvraag en wacht op de aanvullende gegevens.|
|`Accept`|Optioneel. Indien opgegeven, moet de bevatten `application/json`, zoals de Speech-service de resultaten in JSON-indeling bevat. (Sommige Web aanvraag frameworks bieden een niet-compatibele standaardwaarde als u niets opgeeft, dus het is raadzaam om altijd opnemen `Accept`.)|

### <a name="audio-format"></a>Audio-indeling

De audio wordt verzonden in de hoofdtekst van de HTTP `PUT` aanvraag. Deze moet 16-bits WAV-indeling met één PCM-kanaal (mono) op 16 KHz.

### <a name="chunked-transfer"></a>Gesegmenteerde overdracht

Gesegmenteerde overdrachtscodering overdracht (`Transfer-Encoding: chunked`) kunt u Verminder de latentie van de spraakherkenning omdat hierdoor de Speech-service om te beginnen met het audiobestand verwerken terwijl deze wordt verzonden. De REST-API biedt geen tijdelijke of gedeeltelijke resultaten. Deze optie is bedoeld uitsluitend het reactievermogen verbeteren.

De volgende code ziet u hoe u verzendt audio in segmenten. `request` is een HTTPWebRequest-object dat is verbonden met het juiste REST-eindpunt. `audioFile` is het pad naar een audio-bestand op schijf.

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

### <a name="example-request"></a>Van de voorbeeldaanvraag

Hier volgt een typische aanvraag.

```HTTP
POST speech/recognition/conversation/cognitiveservices/v1?language=en-US&format=detailed HTTP/1.1
Accept: application/json;text/xml
Content-Type: audio/wav; codec=audio/pcm; samplerate=16000
Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY
Host: westus.stt.speech.microsoft.com
Transfer-Encoding: chunked
Expect: 100-continue
```

### <a name="http-status"></a>HTTP-status

De HTTP-status van het antwoord geeft aan dat het slagen of algemene fouten.

HTTP-code|Betekenis|Mogelijke oorzaak
-|-|-|
100|Doorgaan|De eerste aanvraag is geaccepteerd. Doorgaan met het verzenden van de rest van de gegevens. (Met gesegmenteerde overdracht gebruikt.)
200|OK|De aanvraag is uitgevoerd. de antwoordtekst is een JSON-object.
400|Ongeldig verzoek|De taalcode die niet opgegeven of is niet een ondersteunde taal; Ongeldig audio-bestand.
401|Niet geautoriseerd|Abonnementssleutel of autorisatie-token is ongeldig in de regio is opgegeven of ongeldig eindpunt.
403|Verboden|Ontbrekende abonnementssleutel of autorisatie token.

### <a name="json-response"></a>JSON-antwoord

Resultaten worden geretourneerd in JSON-indeling. De `simple` indeling bevat alleen de volgende op het hoogste niveau velden.

|Veldnaam|Inhoud|
|-|-|
|`RecognitionStatus`|Status, zoals `Success` voor geslaagde opname. Zie de volgende tabel.|
|`DisplayText`|De herkende tekst na het hoofdlettergebruik, interpunctie, inverse tekst normalisering (conversie van de gesproken tekst voor kortere formulieren, zoals 200 voor '200' of 'Dr. Smith' voor 'doctor smith'), en grof taalgebruik maskeren. Alleen op succes is geïnstalleerd.|
|`Offset`|De tijd (in eenheden van 100 nanoseconden) waarmee de herkende spraak in de audiostream begint.|
|`Duration`|De duur (in eenheden van 100 nanoseconden) van de herkende spraak in de audio-stream.|

De `RecognitionStatus` veld kan de volgende waarden bevatten.

|Statuswaarde|Beschrijving
|-|-|
| `Success` | De opname is voltooid en het veld weergavetekst aanwezig is. |
| `NoMatch` | Spraak is gedetecteerd in de audiostream, maar er zijn geen woorden uit de doeltaal zijn afgestemd. Betekent meestal dat de opname-taal is een andere taal dan het account dat de gebruiker spreekt. |
| `InitialSilenceTimeout` | Het begin van de audiostream bevat alleen stilte en de time-out voor spraak-service. |
| `BabbleTimeout` | Het begin van de audiostream bevat alleen ruis, en de time-out voor spraak-service. |
| `Error` | De opname-service is een interne fout opgetreden en kan niet worden voortgezet. Probeer het opnieuw, indien mogelijk. |

> [!NOTE]
> Als de audio alleen uit grof taalgebruik bestaat, en de `profanity` queryparameter is ingesteld op `remove`, de service heeft geen spraak resultaat geretourneerd. 


De `detailed` indeling bevat dezelfde velden als de `simple` opmaken, samen met een `NBest` veld. De `NBest` veld is een lijst met alternatieve een perfecte ervaring bij van de dezelfde spraak, geclassificeerd van meest waarschijnlijke te minste waarschijnlijk. De eerste vermelding is hetzelfde als de belangrijkste herkenningsresultaat. Elke vermelding bevat de volgende velden:

|Veldnaam|Inhoud|
|-|-|
|`Confidence`|De betrouwbaarheidsscore van de vermelding van 0,0 (geen vertrouwen) 1.0 (volledig vertrouwen)
|`Lexical`|De lexicale vorm van de herkende tekst: de werkelijke woorden herkend.
|`ITN`|De inverse-normalized-tekst ("canonieke") vorm van de herkende tekst, met telefoon getallen, getallen, afkortingen ("doctor smith' op 'dr smith') en andere transformaties toegepast.
|`MaskedITN`| Het formulier toevoegen met grof taalgebruik maskering toegepast, indien aangevraagd.
|`Display`| Het formulier weergegeven van de herkende tekst, met interpunctie hoofdletters en kleine letters toegevoegd. Hetzelfde als `DisplayText` in het resultaat op het hoogste niveau.

### <a name="sample-responses"></a>Voorbeeld van reacties

Hieronder volgt een typische antwoord voor `simple` herkenning.

```json
{
  "RecognitionStatus": "Success",
  "DisplayText": "Remind me to buy 5 pencils.",
  "Offset": "1236645672289",
  "Duration": "1236645672289"
}
```

Hieronder volgt een typische antwoord voor `detailed` herkenning.

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

## <a name="text-to-speech"></a>Tekst naar spraak

Hieronder vindt u de REST-eindpunten voor de spraakservice Text to Speech-API. Gebruik het eindpunt dat overeenkomt met de regio van uw abonnement.

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-text-to-speech.md)]

> [!NOTE]
> Als u een aangepaste spraakstijl hebt gemaakt, kunt u de bijbehorende aangepast eindpunt gebruiken.

De spraak-service ondersteunt 24-KHz audio-uitvoer naast de 16-Khz uitvoer door Bing Speech ondersteund. Vier 24-KHz uitvoerindelingen zijn beschikbaar voor gebruik in de `X-Microsoft-OutputFormat` HTTP-header, twee 24-KHz stemmen, zijn `Jessa24kRUS` en `Guy24kRUS`.

Landinstelling | Taal   | Geslacht | De toewijzing van service
-------|------------|--------|------------
nl-NL  | Amerikaans-Engels | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (en-US, Jessa24kRUS)" 
nl-NL  | Amerikaans-Engels | Man   | "Microsoft Server spraak tekst en spraak, spraak (en-US, Guy24kRUS)"

Een volledige lijst met beschikbare stemmen is beschikbaar in [ondersteunde talen](supported-languages.md#text-to-speech).

### <a name="request-headers"></a>Aanvraagheaders

De volgende velden worden in de HTTP-aanvraagheader verzonden.

|Koptekst|Betekenis|
|------|-------|
|`Authorization`|Een verificatietoken voorafgegaan door het woord `Bearer`. Vereist. Zie [verificatie](#authentication).|
|`Content-Type`|De invoer inhoudstype: `application/ssml+xml`.|
|`X-Microsoft-OutputFormat`|De uitvoer audio-indeling. Zie de volgende tabel.|
|`User-Agent`|De naam van de toepassing. Vereist. moet minder dan 255 tekens bevatten.|

De beschikbare audio uitvoerindelingen (`X-Microsoft-OutputFormat`) een bitsnelheid en een codering.

|||
|-|-|
`raw-16khz-16bit-mono-pcm`         | `raw-8khz-8bit-mono-mulaw`
`riff-8khz-8bit-mono-mulaw`     | `riff-16khz-16bit-mono-pcm`
`audio-16khz-128kbitrate-mono-mp3` | `audio-16khz-64kbitrate-mono-mp3`
`audio-16khz-32kbitrate-mono-mp3`  | `raw-24khz-16bit-mono-pcm`
`riff-24khz-16bit-mono-pcm`        | `audio-24khz-160kbitrate-mono-mp3`
`audio-24khz-96kbitrate-mono-mp3`  | `audio-24khz-48kbitrate-mono-mp3`

> [!NOTE]
> Als uw geselecteerde spraak- en de indeling van uitvoer hebt verschillende bitsnelheden, de audio nieuw voorbeeld wordt gemaakt zo nodig. Echter, 24khz stemmen bieden geen ondersteuning voor `audio-16khz-16kbps-mono-siren` en `riff-16khz-16kbps-mono-siren` uitvoerindelingen. 

### <a name="request-body"></a>Aanvraagtekst

De tekst die moet worden geconverteerd naar spraak wordt verzonden als de instantie van een HTTP `POST` aanvragen in een tekst zonder opmaak (ASCII- of UTF-8) of [spraak synthese Markup Language](speech-synthesis-markup.md) (SSML)-indeling (UTF-8). Tekst zonder opmaak aanvragen gebruiken de standaardstem en de taal van de service. SSML gebruik van een andere stem verzenden.

### <a name="sample-request"></a>Voorbeeld van een aanvraag

De volgende HTTP-aanvraag gebruikt een SSML-instantie voor het kiezen van de stem. De hoofdtekst van het mag niet langer zijn dan 1000 tekens zijn.

```xml
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

### <a name="http-response"></a>HTTP-antwoord

De HTTP-status van het antwoord geeft aan dat het slagen of algemene fouten.

HTTP-code|Betekenis|Mogelijke oorzaak
-|-|-|
200|OK|De aanvraag is uitgevoerd. de antwoordtekst is een geluidsbestand.
400 |Onjuiste aanvraag |Er ontbreekt een vereiste parameter ontbreekt, is leeg of null zijn. Of de waarde die wordt doorgegeven aan een vereiste of optionele parameter is ongeldig. Een veelvoorkomend probleem is een header die te lang is.
401|Niet geautoriseerd |De aanvraag is niet gemachtigd. Controleer of dat uw abonnementssleutel of token geldig is en in de juiste regio.
413|Aanvraagentiteit te groot|De invoer SSML is langer dan 1024 tekens.
|502|Ongeldige gateway    | Netwerk- of serverzijde probleem. Kan ook duiden op ongeldige kopteksten.

Als de HTTP-status `200 OK`, de hoofdtekst van het antwoord bevat een audio-bestand in de gewenste indeling. Dit bestand kan worden afgespeeld, overgedragen of opgeslagen in een buffer of het bestand later afspelen of ander gebruik.

## <a name="authentication"></a>Verificatie

Een aanvraag verzenden naar de spraakservice REST-API is een abonnementssleutel of een toegangstoken vereist. In het algemeen is het eenvoudigste de abonnementssleutel om rechtstreeks te verzenden. De spraakservice haalt het toegangstoken vervolgens voor u. Als u wilt responstijd te minimaliseren, kunt u in plaats daarvan een toegangstoken gebruiken.

Aanwezig als u wilt een token verkrijgen, uw abonnementssleutel met een regionale spraakservice `issueToken` eindpunt, zoals wordt weergegeven in de volgende tabel. Gebruik het eindpunt dat overeenkomt met de regio van uw abonnement.

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-token-service.md)]

Elke toegangstoken is geldig voor 10 minuten. Op elk gewenst moment kunt u een nieuw token verkrijgen. Als u dat wilt, kunt u een token vlak voor elke aanvraag Speech REST-API. Om te beperken van netwerkverkeer en de latentie, wordt u aangeraden het dezelfde token negen minuten.

De volgende secties laten zien hoe u een token verkrijgen en het gebruik ervan in een aanvraag.

### <a name="get-a-token-http"></a>Een token verkrijgen: HTTP

Het volgende voorbeeld wordt een voorbeeld van een HTTP-aanvraag voor het verkrijgen van een token. Vervang `YOUR_SUBSCRIPTION_KEY` met uw abonnementssleutel voor spraak-service. Als uw abonnement komt niet in de regio VS-West, vervangt u de `Host` -header met de hostnaam van uw regio.

```
POST /sts/v1.0/issueToken HTTP/1.1
Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY
Host: westus.api.cognitive.microsoft.com
Content-type: application/x-www-form-urlencoded
Content-Length: 0
```

De hoofdtekst van het antwoord op deze aanvraag is het toegangstoken in de indeling van de Java Web Token (JWT).

### <a name="get-a-token-powershell"></a>Een token verkrijgen: PowerShell

De volgende Windows PowerShell-script laat zien hoe een toegangstoken verkrijgen. Vervang `YOUR_SUBSCRIPTION_KEY` met uw abonnementssleutel voor spraak-service. Als uw abonnement komt niet in de regio VS-West, kunt u de hostnaam van de opgegeven URI gewijzigd.

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

### <a name="get-a-token-curl"></a>Een token verkrijgen: cURL

cURL is een opdrachtregelprogramma beschikbaar in Linux (en in de Windows-subsysteem voor Linux). De volgende cURL-opdracht laat zien hoe een toegangstoken verkrijgen. Vervang `YOUR_SUBSCRIPTION_KEY` met uw abonnementssleutel voor spraak-service. Als uw abonnement komt niet in de regio VS-West, kunt u de hostnaam van de opgegeven URI gewijzigd.

> [!NOTE]
> De opdracht wordt weergegeven op meerdere regels voor de leesbaarheid, maar deze invoeren op één regel op een shell-prompt.

```
curl -v -X POST 
 "https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken" 
 -H "Content-type: application/x-www-form-urlencoded" 
 -H "Content-Length: 0" 
 -H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY"
```

### <a name="get-a-token-c"></a>Een token verkrijgen: C#

De volgende C#-klasse ziet u hoe u een toegangstoken verkrijgen. Uw abonnementssleutel voor spraak-service doorgeven als u een exemplaar maken van de klasse. Als uw abonnement komt niet in de regio VS-West, wijzigt u de hostnaam van `FetchTokenUri` op de juiste wijze.

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

### <a name="use-a-token"></a>Een token gebruiken

Voor het gebruik van een token in een REST-API-aanvraag, bieden deze in de `Authorization` header, volgen op het woord `Bearer`. Hier volgt een voorbeeld van tekst naar spraak REST-aanvraag die een token bevat. Vervangen door uw werkelijke token voor `YOUR_ACCESS_TOKEN`. Gebruik de juiste hostnaam in de `Host` header.

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

### <a name="renew-authorization"></a>Autorisatie vernieuwen

Het verificatietoken verloopt na tien minuten. Autorisatie vernieuwen door het verkrijgen van een nieuw token voordat deze verloopt. U kunt bijvoorbeeld een nieuw token verkrijgen na 9 minuten.

De volgende C#-code is een vervanging drop-in voor de klasse die eerder is weergegeven. De `Authentication` klasse automatisch verkrijgt een nieuw toegangstoken om 9 minuten door een timer te gebruiken. Deze aanpak zorgt ervoor dat een geldig token altijd beschikbaar is terwijl uw programma wordt uitgevoerd.

> [!NOTE]
> In plaats van een timer te gebruiken, kunt u een timestamp van wanneer de laatste token is verkregen opslaan. U kunt vervolgens een nieuwe aanvragen alleen als het bijna verloopt. Deze aanpak voorkomt u onnodig aanvragen van de nieuwe tokens en mogelijk beter geschikt voor programma's die incidentele spraak-aanvragen.

Als voorheen, zorg ervoor dat de `FetchTokenUri` waarde komt overeen met de regio van uw abonnement. Uw abonnementssleutel doorgeven als u een exemplaar maken van de klasse.

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

