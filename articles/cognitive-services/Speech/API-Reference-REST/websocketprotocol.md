---
title: Bing Speech WebSocket-Protocol | Microsoft Docs
titlesuffix: Azure Cognitive Services
description: Documentatie voor de Bing Speech-protocol op basis van WebSockets
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.subservice: bing-speech
ms.topic: article
ms.date: 09/18/2018
ms.author: zhouwang
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: d6601f57d87b518b2061df64174818432b822755
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58076187"
---
# <a name="bing-speech-websocket-protocol"></a>Bing Speech WebSocket-protocol

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-bing-speech-api-deprecation-note.md)]

Bing Speech is een cloud-platform die mogelijkheden voor de meest geavanceerde algoritmen die beschikbaar zijn voor het converteren van de gesproken audio naar tekst. De Bing Speech-protocol bepaalt de [verbindingsinstellingen](#connection-establishment) tussen toepassingen en de service en die worden uitgewisseld tussen tegenhangers berichten voor de spraakherkenning ([berichten client afkomstig](#client-originated-messages) en [berichten afkomstig is van een service](#service-originated-messages)). Bovendien [telemetrieberichten](#telemetry-schema) en [foutafhandeling](#error-handling) worden beschreven.

## <a name="connection-establishment"></a>Verbinding tot stand brengen

Het protocol Spraakservice volgt de standaard WebSocket-specificatie [IETF RFC 6455](https://tools.ietf.org/html/rfc6455). Een HTTP-aanvraag die HTTP-headers die wijzen op van de client de verbinding een upgrade uitvoert naar een WebSocket in plaats van HTTP-semantiek willen bevat in eerste instantie een WebSocket-verbinding. De server geeft aan dat de bereidheid om deel te nemen in de WebSocket-verbinding door te retourneren van een HTTP `101 Switching Protocols` antwoord. Na de uitwisseling van deze handshake, client- en houd de socket geopend en beginnen met behulp van een protocol op basis van een bericht te verzenden en ontvangen van gegevens.

Als u wilt de WebSocket-handshake, verzendt de clienttoepassing een HTTPS-GET-aanvraag naar de service. Het bevat standaard upgrade WebSocket-headers, samen met andere headers die specifiek voor spraak zijn.

```HTTP
GET /speech/recognition/interactive/cognitiveservices/v1 HTTP/1.1
Host: speech.platform.bing.com
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
Sec-WebSocket-Version: 13
Authorization: t=EwCIAgALBAAUWkziSCJKS1VkhugDegv7L0eAAJqBYKKTzpPZOeGk7RfZmdBhYY28jl&p=
X-ConnectionId: A140CAF92F71469FA41C72C7B5849253
Origin: https://speech.platform.bing.com
```

De service reageert met:

```HTTP
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: upgrade
Sec-WebSocket-Key: 2PTTXbeeBXlrrUNsY15n01d/Pcc=
Set-Cookie: SpeechServiceToken=AAAAABAAWTC8ncb8COL; expires=Wed, 17 Aug 2016 15:39:06 GMT; domain=bing.com; path="/"
Date: Wed, 17 Aug 2016 15:03:52 GMT
```

Alle spraak-aanvragen vereisen de [TLS](https://en.wikipedia.org/wiki/Transport_Layer_Security) versleuteling. Het gebruik van niet-versleutelde gesproken aanvragen wordt niet ondersteund. De volgende TLS-versie wordt ondersteund:

* TLS 1.2

### <a name="connection-identifier"></a>Id van de verbinding

Spraakservice is vereist dat alle clients zijn onder andere een unieke ID voor het identificeren van de verbinding. Clients *moet* bevatten de *X ConnectionId* header wanneer ze een WebSocket-handshake starten. De *X ConnectionId* -header moet een [universele, unieke id](https://en.wikipedia.org/wiki/Universally_unique_identifier) (UUID)-waarde. Upgrade WebSocket-aanvragen die geen bevatten de *X ConnectionId*, Geef een waarde voor de *X ConnectionId* header, niet opnemen van een geldige UUID waarde worden geweigerd door de service met een HTTP- `400 Bad Request` antwoord.

### <a name="authorization"></a>Autorisatie

Naast de standaard WebSocket-handshake-headers, spraak-aanvragen vereisen een *autorisatie* header. Zonder deze header worden geweigerd door de service met een HTTP-aanvragen `403 Forbidden` antwoord.

De *autorisatie* header moet een toegangstoken van JSON Web Token (JWT) bevatten.

Zie voor meer informatie over het abonnement en API-sleutels die worden gebruikt om op te halen ongeldig JWT-toegangstokens te verkrijgen, de [Cognitive Services-abonnement](https://azure.microsoft.com/try/cognitive-services/) pagina.

De API-sleutel wordt doorgegeven aan de service voor beveiligingstokens. Bijvoorbeeld:

``` HTTP
POST https://api.cognitive.microsoft.com/sts/v1.0/issueToken
Content-Length: 0
```

De volgende headerinformatie is vereist voor token-toegang.

| Name | Indeling | Description |
|----|----|----|
| OCP-Apim-Subscription-Key | ASCII | Uw abonnementssleutel |

De service voor beveiligingstokens retourneert het toegangstoken JWT als `text/plain`. En vervolgens de JWT wordt doorgegeven als een `Base64 access_token` op de handshake als een *autorisatie* header voorafgegaan door de tekenreeks `Bearer`. Bijvoorbeeld:

`Authorization: Bearer [Base64 access_token]`

### <a name="cookies"></a>Cookies

Clients *moet* ondersteuning bieden voor HTTP-cookies die zijn opgegeven in [RFC 6265](https://tools.ietf.org/html/rfc6265).

### <a name="http-redirection"></a>HTTP-omleiding

Clients *moet* ondersteuning voor de omleiding van standard-mechanismen opgegeven door de [HTTP protocol specification](https://www.w3.org/Protocols/rfc2616/rfc2616.html).

### <a name="speech-endpoints"></a>Spraak-eindpunten

Clients *moet* een juiste Speech Service-eindpunt gebruiken. Het eindpunt is gebaseerd op de opname-modus en de taal. De tabel ziet u enkele voorbeelden.

| Modus | Pad | Service-URI |
| -----|-----|-----|
| Interactief | /speech/recognition/interactive/cognitiveservices/v1 | https://speech.platform.bing.com/speech/recognition/interactive/cognitiveservices/v1?language=pt-BR |
| Gesprek | /speech/recognition/conversation/cognitiveservices/v1 | https://speech.platform.bing.com/speech/recognition/conversation/cognitiveservices/v1?language=en-US |
| Dicteren | /speech/recognition/dictation/cognitiveservices/v1 | https://speech.platform.bing.com/speech/recognition/dictation/cognitiveservices/v1?language=fr-FR |

Zie voor meer informatie de [URI van de Service](../GetStarted/GetStartedREST.md#service-uri) pagina.

### <a name="report-connection-problems"></a>Problemen met de verbinding van rapport

Clients moeten alle problemen opgetreden bij het maken van een verbinding onmiddellijk rapporteren. De message protocol voor rapportage mislukte verbindingen wordt beschreven in [verbinding mislukt telemetrie](#connection-failure-telemetry).

### <a name="connection-duration-limitations"></a>Duur van de beperkingen van verbinding

Als de vergeleken met HTTP-verbindingen met de standaard web service, WebSocket verbindingen laatst een *lang* tijd. Speech Service plaatst beperkingen op de duur van de WebSocket-verbindingen met de service:

 * De maximale duur voor alle actieve WebSocket-verbinding is 10 minuten. Er is een verbinding actief als de service of de client WebSocket-berichten via deze verbinding verzendt. De service verbreekt de verbinding zonder waarschuwing wanneer de limiet is bereikt. Clients moeten de gebruiker moet worden opgegeven die niet nodig hebt voor de verbinding actief tijdens of kort de levensduur van het maximumaantal verbindingen blijft ontwikkelen.

 * De maximale duur voor een inactieve WebSocket-verbinding is 180 seconden. Er is een verbinding inactief als de service noch de client een WebSocket-bericht via de verbinding verzonden. Nadat de maximale levensduur van een niet-actief is bereikt, beëindigt de service de inactieve WebSocket-verbinding.

## <a name="message-types"></a>Berichttypen

Nadat een WebSocket-verbinding tot stand is gebracht tussen de client en de service, kunnen u berichten met zowel de client en de service verzenden. Deze sectie beschrijft de indeling van deze WebSocket-berichten.

[IETF RFC 6455](https://tools.ietf.org/html/rfc6455) geeft aan dat de gegevens voor WebSocket-berichten kunnen worden verzonden met behulp van een tekst of binaire codering. De twee coderingen gebruiken verschillende indelingen van de kabel. Elke indeling is geoptimaliseerd voor efficiënte codering, overdracht en decodering van de berichtnettolading.

### <a name="text-websocket-messages"></a>WebSocket-SMS-berichten

SMS-berichten voor WebSocket voert een nettolading van tekstgegevens die uit een gedeelte van kopteksten en een instantie van elkaar gescheiden door de combinatie van vertrouwde dubbele regelterugloop newline gebruikt voor HTTP-berichten bestaat. En zoals HTTP-berichten, tekstberichten van de WebSocket kopteksten in *naam: waarde* indeling, gescheiden door een combinatie van één regelterugloop nieuwe regel. Geen tekst is opgenomen in een tekstbericht WebSocket *moet* gebruiken [UTF-8](https://tools.ietf.org/html/rfc3629) codering.

WebSocket-SMS-berichten moeten een pad opgeven in de header *pad*. De waarde van deze header moet een van de spraakherkenning protocol berichttypen gedefinieerd verderop in dit document.

### <a name="binary-websocket-messages"></a>Binaire WebSocket-berichten

Binaire WebSocket berichten voert een binaire-nettolading. In het protocol Spraakservice audio verzonden naar en ontvangen van de service met behulp van binaire WebSocket-berichten. Alle andere berichten zijn berichten WebSocket.

Zoals SMS-berichten voor WebSocket bestaan binaire WebSocket-berichten uit een koptekst en een hoofdtekst-sectie. Geef de eerste 2 bytes van het binaire WebSocket-bericht [big endian](https://en.wikipedia.org/wiki/Endianness) volgorde, de grootte van de 16-bits geheel getal van de sectie header. De grootte van de sectie minimale kop is 0 bytes. De maximale grootte is 8192 bytes. De tekst in de headers van binaire WebSocket berichten *moet* gebruiken [US-ASCII-](https://tools.ietf.org/html/rfc20) codering.

Kopteksten in een binaire WebSocket-bericht zijn gecodeerd in dezelfde indeling als in de SMS-berichten voor WebSocket. De *naam: waarde* indeling wordt gescheiden door een combinatie van één regelterugloop nieuwe regel. Binaire WebSocket berichten moeten een pad opgeven in de header *pad*. De waarde van deze header moet een van de spraakherkenning protocol berichttypen gedefinieerd verderop in dit document.

Tekst en binaire WebSocket-berichten worden gebruikt in de Service voor spraak-protocol.

## <a name="client-originated-messages"></a>Berichten afkomstig is van een client

Nadat de verbinding tot stand is gebracht, worden zowel de client en de service kunnen beginnen met het verzenden van berichten. Deze sectie beschrijft de indeling en de nettolading van de berichten die clienttoepassingen naar spraak-Service verzenden. De sectie [berichten afkomstig is van een Service](#service-originated-messages) geeft de berichten die afkomstig zijn uit Speech Service en worden verzonden naar de clienttoepassingen.

De belangrijkste berichten verzonden door de client naar de services zijn `speech.config`, `audio`, en `telemetry` berichten. Voordat we elk bericht in detail, vereist de gemeenschappelijke berichtkoppen voor al deze berichten worden beschreven.

### <a name="required-message-headers"></a>Vereiste berichtkoppen

De volgende headers zijn vereist voor alle berichten die afkomstig zijn van een client.

| Header | Value |
|----|----|
| Pad | Het pad weergegeven die zijn opgegeven in dit document |
| X-RequestId | UUID in 'niet-dash'-indeling |
| X-Timestamp | Client UTC klok tijdstempel in ISO 8601-notatie |

#### <a name="x-requestid-header"></a>De header X-RequestId

Aanvragen afkomstig is van een client worden onderscheiden op basis van de *X-RequestId* berichtkop. Deze header is vereist voor alle berichten die afkomstig zijn van een client. De *X-RequestId* header-waarde moet een UUID "niet-dash" vorm, bijvoorbeeld *123e4567e89b12d3a456426655440000*. Deze *kan geen* worden in de canonieke vorm *123e4567-e89b-12d3-a456-426655440000*. Aanvragen zonder een *X-RequestId* header of met een headerwaarde die gebruikmaakt van de verkeerde indeling voor UUID's ervoor zorgen dat de service beëindigen van de WebSocket-verbinding.

#### <a name="x-timestamp-header"></a>De header X-Timestamp

Elk bericht dat wordt verzonden naar spraak-Service door een clienttoepassing *moet* bevatten een *X-Timestamp* header. De waarde voor deze header is de tijd wanneer de client wordt het bericht wordt verzonden. Aanvragen zonder een *X-Timestamp* header of leiden dat de service beëindigen van de WebSocket-verbinding met een headerwaarde die gebruikmaakt van de verkeerde indeling.

De *X-Timestamp* headerwaarde moet van het formulier 'yyyy'-'MM'-'dd'T' HH': 'mm':'ss '.' fffffffZ' waar 'fffffff' is een fractie van een seconde. Bijvoorbeeld, '12,5' '12 + 5/10 seconden en '12.526' betekent ' 12 plus 526/1000 seconden'. Deze indeling voldoet aan de [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) en, in tegenstelling tot de standaard-HTTP- *datum* header, kan deze milliseconde oplossing te bieden. Clienttoepassingen kunnen afronden tijdstempels op de dichtstbijzijnde milliseconde. Moeten de clienttoepassingen om ervoor te zorgen dat de apparaatklok nauwkeurig worden bijgehouden tijd met behulp van een [Network Time Protocol (NTP)-server](https://en.wikipedia.org/wiki/Network_Time_Protocol).

### <a name="message-speechconfig"></a>Bericht `speech.config`

Spraakservice moet de kenmerken van uw toepassing voor de best mogelijke spraakherkenning kent. De gegevens van de vereiste kenmerken bevat informatie over het apparaat en het besturingssysteem dat wordt gebruikt door uw toepassing. U opgeven dat deze informatie in de `speech.config` bericht.

Clients *moet* verzendt een `speech.config` bericht zodra ze maken van de verbinding met Spraakservice en voordat ze een verzenden `audio` berichten. U moet voor het verzenden van een `speech.config` bericht slechts één keer per verbinding.

| Veld | Description |
|----|----|
| WebSocket-bericht coderen | Tekst |
| Hoofdtekst | De nettolading als een JSON-structuur |

#### <a name="required-message-headers"></a>Vereiste berichtkoppen

| Headernaam | Value |
|----|----|
| Pad | `speech.config` |
| X-Timestamp | Client UTC klok tijdstempel in ISO 8601-notatie |
| Content-Type | application/json; charset=utf-8 |

Net als bij alle berichten in het protocol Speech Service, die afkomstig is van een client de `speech.config` bericht *moet* bevatten een *X-Timestamp* -header die records van de client UTC clock-tijd waarop het bericht is verzonden met de service. De `speech.config` bericht *niet* vereisen een *X-RequestId* header omdat dit bericht is niet gekoppeld aan een bepaalde spraak-aanvraag.

#### <a name="message-payload"></a>De berichtnettolading van
De nettolading van de `speech.config` bericht is een JSON-structuur die informatie over de toepassing bevat. Het volgende voorbeeld ziet deze informatie. Voor clients en apparaten context-informatie is opgenomen in de *context* element van de JSON-structuur.

```JSON
{
  "context": {
    "system": {
      "version": "2.0.12341",
    },
    "os": {
      "platform": "Linux",
      "name": "Debian",
      "version": "2.14324324"
    },
    "device": {
      "manufacturer": "Contoso",
      "model": "Fabrikan",
      "version": "7.341"
      }
    },
  }
}
```

##### <a name="system-element"></a>Systeemelement

Het element system.version van de `speech.config` bericht bevat de versie van de gesproken tekst SDK-software die worden gebruikt door de clienttoepassing of het apparaat. De waarde in het formulier is *major.minor.build.branch*. U kunt weglaten de *vertakking* onderdeel als deze niet van toepassing is.

##### <a name="os-element"></a>OS-element

| Veld | Description | Gebruik |
|-|-|-|
| os.platform | De OS-platform die als host fungeert van de toepassing, bijvoorbeeld, Windows, Android, iOS- of Linux |Vereist |
| os.name | De productnaam besturingssysteem, bijvoorbeeld Debian of Windows 10 | Vereist |
| OS.Version | De versie van het besturingssysteem in de vorm *major.minor.build.branch* | Vereist |

##### <a name="device-element"></a>Apparaat-element

| Veld | Description | Gebruik |
|-|-|-|
| Device.Manufacturer | De fabrikant van apparaat | Vereist |
| device.model | Het Apparaatmodel | Vereist |
| Device.Version | De versie van het apparaat software is geleverd door de fabrikant van het apparaat. Deze waarde bevat een versie van het apparaat dat kan worden gevolgd door de fabrikant. | Vereist |

### <a name="message-audio"></a>Bericht `audio`

Spraak ingeschakeld clienttoepassingen verzenden audio naar spraak-Service door de audiostream omzetten in een reeks van audio segmenten. Elk segment van de audio wordt een segment van de gesproken audio die moet worden omgezet door de service. De maximale grootte van één audio segment is 8192 bytes. Audiostream berichten zijn *binaire WebSocket berichten*.

Clients gebruiken de `audio` dat een audio segment naar de service verzonden. Clients lezen audio van de microfoon in segmenten en deze segmenten verzenden naar Spraakservice voor transcriptie. De eerste `audio` bericht een grammaticaal correcte header die goed geeft aan dat de audio aan een van de indelingen ondersteund door de service voldoet moet bevatten. Aanvullende `audio` berichten bevatten alleen de binaire audio streamen gegevens lezen van de microfoon.

Clients worden verzonden (optioneel) een `audio` bericht met een lengte van nul-instantie. Dit bericht geeft aan dat de service die de client bekend is dat de gebruiker heeft gestopt spreken, de utterance is voltooid en de microfoon is uitgeschakeld.

Speech Service maakt gebruik van de eerste `audio` -mailbericht met een unieke aanvraag-id op om door te geven het begin van een nieuwe aanvraag/antwoord-cyclus of *inschakelen*. Nadat de service ontvangt een `audio` bericht met een nieuwe aanvraag-id, het in de wachtrij of niet-verzonden berichten die gekoppeld aan alle vorige inschakelen zijn wordt verwijderd.

| Veld | Description |
|-------------|----------------|
| WebSocket-bericht coderen | Binair bestand |
| Hoofdtekst | De binaire gegevens voor de audio chunk. Maximale grootte is 8192 bytes. |

#### <a name="required-message-headers"></a>Vereiste berichtkoppen

De volgende headers zijn vereist voor alle `audio` berichten.

| Header         |  Value     |
| ------------- | ---------------- |
| Pad | `audio` |
| X-RequestId | UUID in 'niet-dash'-indeling |
| X-Timestamp | Client UTC klok tijdstempel in ISO 8601-notatie |
| Content-Type | De audio inhoudstype. Het type moet een *audio/x-wav* (PCM) of *audio/zijde* (ZIJDE). |

#### <a name="supported-audio-encodings"></a>Ondersteunde audio coderingen

Deze sectie beschrijft de codecs audio wordt ondersteund door spraak-Service.

##### <a name="pcm"></a>PCM

Speech Service accepteert niet-gecomprimeerde pulse-code (PCM)-modulatie audio. Audio wordt verzonden naar de service in [WAV](https://en.wikipedia.org/wiki/WAV) -indeling, zodat de eerste audio wordt verdeeld *moet* bevatten een geldige [Resource Interchange bestandsindeling](https://en.wikipedia.org/wiki/Resource_Interchange_File_Format) (RIFF)-header. Als een client een inschakelen met een audio-segment dat doet initieert *niet* een geldige RIFF-header bevatten, de service wordt het verzoek geweigerd en wordt de WebSocket-verbinding beëindigd.

PCM-audio *moet* daarvan worden genomen op 16 kHz met 16 bits per voorbeeld en één kanaal (*riff-16khz-16-bits-mono-pcm*). Speech Service biedt geen ondersteuning voor stereo audiostreams en audiostreams die geen van de opgegeven bitsnelheid, samplefrequentie of aantal kanalen gebruikmaken weigert.

##### <a name="opus"></a>Opus

Opus is een open, royaltyvrije, uiterst veelzijdige audiocodec. Spraakservice ondersteunt Opus tegen een tarief constante bits van `32000` of `16000`. Alleen de `OGG` container voor Opus wordt momenteel ondersteund die is opgegeven door de `audio/ogg` MIME-type.

Voor het gebruik van Opus, wijzigen de [JavaScript voorbeeld](https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript/blob/master/samples/browser/Sample.html#L101) en wijzig de `RecognizerSetup` methode om te retourneren.

```javascript
return SDK.CreateRecognizerWithCustomAudioSource(
            recognizerConfig,
            authentication,
            new SDK.MicAudioSource(
                     new SDK.OpusRecorder(
                     {
                         mimeType: "audio/ogg",
                         bitsPerSecond: 32000
                     }
              )
          ));
```

#### <a name="detect-end-of-speech"></a>Einde van spraak detecteren

Mensen doen niet expliciet geven wanneer ze klaar bent spreken. Elke toepassing die door spraak geaccepteerd als invoer twee opties heeft voor het verwerken van het einde van de gesproken tekst in een audiostream: detectie van end-of-speech en detectie van de end-of-speech client-service. Van deze twee opties biedt detectie van de end-of-speech service gewoonlijk een betere gebruikerservaring.

##### <a name="service-end-of-speech-detection"></a>Detectie van de end-of-speech service

Voor het bouwen van de ideale Handsfree spraak-ervaring, zodat toepassingen de service om te detecteren wanneer de gebruiker spreekt is voltooid. Clients verzenden audio van de microfoon als *audio* segmenten gedownload totdat de service stilte detecteert en met reageert een `speech.endDetected` bericht.

##### <a name="client-end-of-speech-detection"></a>Detectie van de end-of-speech client

Clienttoepassingen die toestaan dat de gebruiker het einde van de gesproken tekst in een bepaalde manier kunnen tevens de service die signaal. Een clienttoepassing zijn mogelijk een 'Stop' of ' ' dempen die kan de gebruiker op. Als u wilt aangeven end-of-speech, het verzenden van clienttoepassingen een *audio* chunk-bericht met een lengte van nul-instantie. Spraakservice wordt dit bericht als het einde van de binnenkomende audiostream geïnterpreteerd.

### <a name="message-telemetry"></a>Bericht `telemetry`

Clienttoepassingen *moet* bevestigen van het einde van elke inschakelen door te verzenden van telemetrie over de inschakelen naar Speech Service. Schakel-end bevestiging kunt Speech-Service om ervoor te zorgen dat alle berichten die nodig zijn voor de voltooiing van de aanvraag en de reactie correct zijn ontvangen door de client. Schakel-end bevestiging kunt ook-spraak-Service om te verifiëren dat de clienttoepassingen worden uitgevoerd zoals verwacht. Deze informatie is zeer waardevol als u informatie over het oplossen van uw toepassing spraak ingeschakelde nodig hebt.

Clients het einde van een inschakelen door te sturen moeten bevestigen een `telemetry` bericht snel na ontvangst een `turn.end` bericht. Clients om te bevestigen moeten proberen de `turn.end` zo snel mogelijk. Als een clienttoepassing mislukt voor het bevestigen van het einde inschakelen, mogelijk Speech Service de verbinding met een fout te beëindigen. Clients kunnen slechts één moeten verzenden `telemetry` bericht voor elke aanvraag en reactie geïdentificeerd door de *X-RequestId* waarde.

| Veld | Description |
| ------------- | ---------------- |
| WebSocket-bericht coderen | Tekst |
| Pad | `telemetry` |
| X-Timestamp | Client UTC klok tijdstempel in ISO 8601-notatie |
| Content-Type | `application/json` |
| Hoofdtekst | Een JSON-structuur met clientinformatie over het inschakelen |

Het schema voor de hoofdtekst van de `telemetry` bericht is gedefinieerd in de [telemetrie schema](#telemetry-schema) sectie.

#### <a name="telemetry-for-interrupted-connections"></a>Telemetrie voor onderbroken verbindingen

Als de netwerkverbinding om welke reden dan ook tijdens een inschakelen mislukt en de client heeft *niet* ontvangen een `turn.end` bericht uit de service, de client verzendt een `telemetry` bericht. In dit bericht wordt de mislukte aanvragen met de volgende keer dat de client een verbinding met de service maakt. Clients hoeven niet onmiddellijk probeert een verbinding voor het verzenden van de `telemetry` bericht. Het bericht kan worden gebufferd op de client en verzonden via een toekomstige gebruiker aangevraagde verbinding. De `telemetry` bericht voor de mislukte aanvragen *moet* gebruiken de *X-RequestId* waarde van de mislukte aanvragen. Deze kan worden verzonden naar de service zodra een verbinding tot stand is gebracht, zonder te wachten op verzenden of ontvangen van andere berichten.

## <a name="service-originated-messages"></a>Berichten afkomstig is van een service

Deze sectie beschrijft de berichten die afkomstig zijn uit Speech Service en worden verzonden naar de client. Speech Service houdt een register van de client-mogelijkheden en genereert de berichten die zijn vereist voor elke client, dus niet dat alle clients ontvangen alle berichten die hier worden beschreven. Beknopt alternatief berichten wordt verwezen door de waarde van de *pad* header. Bijvoorbeeld, verwijzen we naar een WebSocket SMS-bericht met de *pad* waarde `speech.hypothesis` als een bericht speech.hypothesis.

### <a name="message-speechstartdetected"></a>Bericht `speech.startDetected`

De `speech.startDetected` bericht geeft aan dat Spraakservice spraak gedetecteerd in de audio-stream.

| Veld | Description |
| ------------- | ---------------- |
| WebSocket-bericht coderen | Tekst |
| Pad | `speech.startDetected` |
| Content-Type | application/json; charset=utf-8 |
| Hoofdtekst | De JSON-structuur die informatie over de voorwaarden bevat wanneer het begin van spraak is gedetecteerd. De *Offset* veld in deze structuur geeft de verschuiving (in eenheden van 100 nanoseconden) wanneer spraak in de audiostream, ten opzichte van het begin van de stroom is gedetecteerd. |

#### <a name="sample-message"></a>Voorbeeldbericht

```HTML
Path: speech.startDetected
Content-Type: application/json; charset=utf-8
X-RequestId: 123e4567e89b12d3a456426655440000

{
  "Offset": 100000
}
```

### <a name="message-speechhypothesis"></a>Bericht `speech.hypothesis`

Tijdens de spraakherkenning genereert Speech Service regelmatig hypothesen over de woorden die de service wordt herkend. Speech Service verzendt deze hypothesen naar de client ongeveer elke 300 milliseconden. De `speech.hypothesis` geschikt *alleen* voor het verbeteren van de gebruikerservaring van spraak. U moet eventuele afhankelijkheden van de inhoud of de nauwkeurigheid van de tekst niet uitvoeren in deze berichten.

 De `speech.hypothesis` bericht is van toepassing op clients die de mogelijkheden van sommige tekst rendering en feedback wilt geven near-real-time van de erkenning wordt uitgevoerd voor de persoon die spreekt.

| Veld | Description |
| ------------- | ---------------- |
| WebSocket-bericht coderen | Tekst |
| Pad | `speech.hypothesis` |
| X-RequestId | UUID in 'niet-dash'-indeling |
| Content-Type | application/json |
| Hoofdtekst | De spraak-hypothese JSON-structuur |

#### <a name="sample-message"></a>Voorbeeldbericht

```HTML
Path: speech.hypothesis
Content-Type: application/json; charset=utf-8
X-RequestId: 123e4567e89b12d3a456426655440000

{
  "Text": "this is a speech hypothesis",
  "Offset": 0,
  "Duration": 23600000
}
```

De *Offset* element geeft de verschuiving (in eenheden van 100 nanoseconden) wanneer de woordgroep wordt herkend, ten opzichte van het begin van de audio-stream.

De *duur* element Hiermee geeft u de duur (in eenheden van 100 nanoseconden) van deze zin spraak.

Clients moeten niet veronderstellingen over de frequentie, timing of tekst in een hypothese spraak of de consistentie van de tekst in een hypothesen twee spraak. De hypothesen zijn alleen momentopnamen in het proces transcriptie in de service. Ze vertegenwoordigt een stabiele opeenstapeling van transcriptie. Bijvoorbeeld een eerste spraak hypothese bevat mogelijk de woorden "fijn leuk" en de tweede hypothese kan bevatten van de woorden "leuks was vinden." Spraakservice uitvoeren niet na verwerking (bijvoorbeeld gebruik van hoofdletters, leestekens) op de tekst in de hypothese spraak.

### <a name="message-speechphrase"></a>Bericht `speech.phrase`

Wanneer Spraakservice bepaalt dat er voldoende informatie voor het produceren van een herkenningsresultaat dat niet verandert, de service-produceert een `speech.phrase` bericht. Speech Service levert deze resultaten nadat gedetecteerd dat de gebruiker een zin of woordgroep is voltooid.

| Veld | Description |
| ------------- | ---------------- |
| WebSocket-bericht coderen | Tekst |
| Pad | `speech.phrase` |
| Content-Type | application/json |
| Hoofdtekst | De spraak-woordgroep JSON-structuur |

De spraak woordgroep JSON-schema bevat de volgende velden: `RecognitionStatus`, `DisplayText`, `Offset`, en `Duration`. Zie voor meer informatie over deze velden, [transcriptie antwoorden](../concepts.md#transcription-responses).

#### <a name="sample-message"></a>Voorbeeldbericht

```HTML
Path: speech.phrase
Content-Type: application/json; charset=utf-8
X-RequestId: 123e4567e89b12d3a456426655440000

{
  "RecognitionStatus": "Success",
  "DisplayText": "Remind me to buy 5 pencils.",
  "Offset": 0,
  "Duration": 12300000
}
```

### <a name="message-speechenddetected"></a>Bericht `speech.endDetected`

De `speech.endDetected` bericht geeft aan dat de clienttoepassing moet worden stopgezet streamen van audio naar de service.

| Veld | Description |
| ------------- | ---------------- |
| WebSocket-bericht coderen | Tekst |
| Pad | `speech.endDetected` |
| Hoofdtekst | De JSON-structuur met de verschuiving van het einde van spraak is gedetecteerd. De offset wordt weergegeven in eenheden van 100 nanoseconden verschuiving vanaf het begin van de audio die wordt gebruikt voor opname. |
| Content-Type | application/json; charset=utf-8 |

#### <a name="sample-message"></a>Voorbeeldbericht

```HTML
Path: speech.endDetected
Content-Type: application/json; charset=utf-8
X-RequestId: 123e4567e89b12d3a456426655440000

{
  "Offset": 0
}
```

De *Offset* element geeft de verschuiving (in eenheden van 100 nanoseconden) wanneer de woordgroep wordt herkend, ten opzichte van het begin van de audio-stream.

### <a name="message-turnstart"></a>Bericht `turn.start`

De `turn.start` geeft aan het begin van een inschakelen vanuit het perspectief van de service. De `turn.start` bericht is altijd het eerste antwoordbericht u voor elke aanvraag ontvangt. Als u niet hebt ontvangen een `turn.start` ontvangt, wordt ervan uitgegaan dat de status van de serviceverbinding is ongeldig.

| Veld | Description |
| ------------- | ---------------- |
| WebSocket-bericht coderen | Tekst |
| Pad | `turn.start` |
| Content-Type | application/json; charset=utf-8 |
| Hoofdtekst | JSON-structuur |

#### <a name="sample-message"></a>Voorbeeldbericht

```HTML
Path: turn.start
Content-Type: application/json; charset=utf-8
X-RequestId: 123e4567e89b12d3a456426655440000

{
  "context": {
    "serviceTag": "7B33613B91714B32817815DC89633AFA"
  }
}
```

De hoofdtekst van de `turn.start` bericht is een JSON-structuur die context voor het begin van de beurt bevat. De *context* element bevat een *serviceTag* eigenschap. Deze eigenschap geeft u een tagwaarde die de service is gekoppeld aan de beurt. Deze waarde kan worden gebruikt door Microsoft als u informatie over het oplossen van fouten in uw toepassing nodig hebt.

### <a name="message-turnend"></a>Bericht `turn.end`

De `turn.end` geeft aan het einde van een inschakelen vanuit het perspectief van de service. De `turn.end` bericht is altijd het laatste antwoordbericht u voor elke aanvraag ontvangt. Clients kunnen de ontvangst van dit bericht gebruiken als een signaal voor opschoning activiteiten en over te stappen op een niet-actieve status. Als u niet hebt ontvangen een `turn.end` ontvangt, wordt ervan uitgegaan dat de status van de serviceverbinding is ongeldig. In deze gevallen de bestaande verbinding met de service sluit en opnieuw verbinding maken.

| Veld | Description |
| ------------- | ---------------- |
| WebSocket-bericht coderen | Tekst |
| Pad | `turn.end` |
| Hoofdtekst | Geen |

#### <a name="sample-message"></a>Voorbeeldbericht

```HTML
Path: turn.end
X-RequestId: 123e4567e89b12d3a456426655440000
```

## <a name="telemetry-schema"></a>Telemetrie-schema

De hoofdtekst van de *telemetrie* bericht is een JSON-structuur die clientinformatie over een inschakelen of een pogingen tot verbinding bevat. De structuur is opgebouwd uit client tijdstempels die record wanneer clientgebeurtenissen plaatsvinden. Elke tijdstempel moet zich in de ISO 8601-notatie, zoals beschreven in de sectie 'X-Timestamp header'. *Telemetrie* berichten die niet alle vereiste velden in de JSON-structuur opgeeft of die gebruik niet de juiste tijdstempel-indeling kunnen leiden tot de service de verbinding met de client is beëindigd. Clients *moet* geldige waarden voor alle vereiste velden opgeven. Clients *moet* waarden voor optionele velden wanneer dat nodig. De waarden in de voorbeelden in deze sectie zijn uitsluitend ter illustratie.

Telemetrie-schema is onderverdeeld in de volgende onderdelen: ontvangen bericht tijdstempels en metrische gegevens. De indeling en het gebruik van elk onderdeel is opgegeven in de volgende secties.

### <a name="received-message-time-stamps"></a>Ontvangen bericht tijdstempels

-Clients moeten de tijd van ontvangst waarden voor alle berichten die zij krijgen de verbinding is geslaagd met de service zijn. Deze waarden moeten registreren op welk tijdstip wanneer de client *ontvangen* elk bericht van het netwerk. De waarde moet een ander tijdstip niet vastleggen. De client moet bijvoorbeeld niet registreren op welk tijdstip wanneer deze *gehandeld* voor het bericht. De tijdstempels ontvangen bericht worden opgegeven in een matrix met *naam: waarde* paren. Hiermee geeft u op de naam van de combinatie van de *pad* waarde van het bericht. De waarde van het paar Hiermee geeft u de tijd op de client wanneer het bericht is ontvangen. Of, als meer dan één bericht van de opgegeven naam is ontvangen, is de waarde van de combinatie van een matrix van tijdstempels die aangeeft wanneer deze berichten zijn ontvangen.

```JSON
  "ReceivedMessages": [
    { "speech.hypothesis": [ "2016-08-16T15:03:48.172Z", "2016-08-16T15:03:48.331Z", "2016-08-16T15:03:48.881Z" ] },
    { "speech.endDetected": "2016-08-16T15:03:49.721Z" },
    { "speech.phrase": "2016-08-16T15:03:50.001Z" },
    { "turn.end": "2016-08-16T15:03:51.021Z" }
  ]
```

Clients *moet* bevestigt de ontvangst van alle berichten die door de service worden verzonden door tijdstempels voor deze berichten op te nemen in de JSON-hoofdtekst. Als een client niet kan de ontvangst van een bericht te bevestigen, kan de service de verbinding verbreken.

### <a name="metrics"></a>Metrische gegevens

Clients moeten bevatten informatie over gebeurtenissen die hebben plaatsgevonden tijdens de levensduur van een aanvraag. De volgende metrische gegevens worden ondersteund: `Connection`, `Microphone`, en `ListeningTrigger`.

### <a name="metric-connection"></a>Metrische gegevens `Connection`

De `Connection` metrische waarde geeft meer informatie over verbindingspogingen door de client. De metrische gegevens moet tijdstempels bevatten wanneer de WebSocket-verbinding is gestart en is voltooid. De `Connection` metrische gegevens is vereist *alleen voor de eerste inschakelen van een verbinding*. Hiermee schakelt u volgende hoeven niet te deze informatie opnemen. Als een client meerdere verbindingspogingen voordat een verbinding tot stand is gebracht, informatie over *alle* de verbindingspogingen moeten worden opgenomen. Zie voor meer informatie, [verbinding mislukt telemetrie](#connection-failure-telemetry).

| Veld | Description | Gebruik |
| ----- | ----------- | ----- |
| Name | `Connection` | Vereist |
| Id | De verbinding id-waarde die is gebruikt in de *X ConnectionId* -header voor deze aanvraag | Vereist |
| Starten | Het tijdstip waarop de client de verbindingsaanvraag verzonden | Vereist |
| Beëindigen | De tijd bij het ontvangen van de client de verbinding is tot stand is gebracht of, in foutgevallen, afgewezen, geweigerd of is mislukt | Vereist |
| Fout | Een beschrijving van de fout is opgetreden, indien van toepassing. Als de verbinding geslaagd is, moeten clients laat dit veld weg. De maximale lengte van dit veld is 50 tekens. | Vereist voor foutgevallen, anders wordt weggelaten |

De beschrijving van de fout moet maximaal 50 tekens lang zijn en in het ideale geval moet een van de waarden die worden vermeld in de volgende tabel. Als het probleem komt niet overeen met een van deze waarden, clients een beknopte beschrijving van de fout kunnen gebruiken met behulp van [CamelCasing](https://en.wikipedia.org/wiki/Camel_case) zonder spaties bevatten. De mogelijkheid voor het verzenden van een *telemetrie* bericht moet worden verbonden met de service, zodat alleen tijdelijke of tijdelijke fouten kunnen worden gerapporteerd de *telemetrie* bericht. Foutsituaties die *permanent* blok een client van een verbinding te maken met de service te voorkomen dat de client een bericht verzenden naar de service, met inbegrip van *telemetrie* berichten.

| Fout | Gebruik |
| ----- | ----- |
| DNSfailure | De client kon geen verbinding maken met de service vanwege een DNS-fout in de netwerkstack. |
| NoNetwork | De client heeft geprobeerd een verbinding, maar de netwerkstack gemeld dat er geen fysieke netwerk beschikbaar is. |
| NoAuthorization | De clientverbinding is mislukt tijdens het verkrijgen van een verificatietoken voor de verbinding. |
| NoResources | De client heeft te weinig een lokale bron (bijvoorbeeld geheugen) bij een poging om een verbinding te maken. |
| Verboden | De client is geen verbinding maken met de service omdat de service heeft een HTTP geretourneerd `403 Forbidden` statuscode in de aanvraag om een upgrade WebSocket. |
| Niet geautoriseerd | De client is geen verbinding maken met de service omdat de service heeft een HTTP geretourneerd `401 Unauthorized` statuscode in de aanvraag om een upgrade WebSocket. |
| BadRequest | De client is geen verbinding maken met de service omdat de service heeft een HTTP geretourneerd `400 Bad Request` statuscode in de aanvraag om een upgrade WebSocket. |
| ServerUnavailable | De client is geen verbinding maken met de service omdat de service heeft een HTTP geretourneerd `503 Server Unavailable` statuscode in de aanvraag om een upgrade WebSocket. |
| ServerError | De client kan geen verbinding maken met de service omdat de service heeft geretourneerd is een `HTTP 500` statuscode interne fout in de aanvraag om een upgrade WebSocket. |
| Time-out | Aanvraag van de client verbinding zonder een reactie van de service is een time-out. De *End* veld bevat de tijd wanneer de client is een time-out en wachten op de verbinding is gestopt. |
| ClientError | De client verbroken de verbinding vanwege een interne Clientfout. |

### <a name="metric-microphone"></a>Metrische gegevens `Microphone`

De `Microphone` metrische gegevens is vereist voor alle spraak schakelt. Met deze metriek wordt de tijd op de client gedurende welke audio-invoer actief voor een spraak-aanvraag gebruikt wordt.

Gebruik de volgende voorbeelden als richtlijnen voor de opname *Start* tijd van de waarden voor de `Microphone` metrische gegevens in uw clienttoepassing:

* Een clienttoepassing vereist dat een gebruiker moet druk op een fysieke knop om te starten van de microfoon. Na de druk op de knop, de clienttoepassing leest de invoer van de microfoon en verzonden naar spraak-Service. De *Start* waarde voor de `Microphone` metriek legt de tijd na de knop push wanneer de microfoon is geïnitialiseerd en is klaar voor invoer. De *End* waarde voor de `Microphone` metriek legt de tijd wanneer het streamen van audio naar de service nadat het ontvangen van de clienttoepassing wordt gestopt de `speech.endDetected` bericht van de service.

* Een clienttoepassing maakt gebruik van een sleutelwoord spotter die 'altijd' luistert. Nadat het sleutelwoord spotter een woordgroep gesproken trigger detecteert de clienttoepassing de invoer van de microfoon te verzamelen en te verzenden naar Speech Service. De *Start* waarde voor de `Microphone` metriek legt de tijd waarop het sleutelwoord spotter op de hoogte gesteld de client te starten met behulp van de invoer van de microfoon. De *End* waarde voor de `Microphone` metriek legt de tijd wanneer het streamen van audio naar de service nadat het ontvangen van de clienttoepassing wordt gestopt de `speech.endDetected` bericht van de service.

* Een clienttoepassing toegang heeft tot een constante audiostream en stilte/spraak detectie uitvoert op die audiostream in een *spraak detectiemodule*. De *Start* waarde voor de `Microphone` metriek legt de tijd wanneer de *spraak detectiemodule* een melding van de client te starten met behulp van de invoer van de audio-stream. De *End* waarde voor de `Microphone` metriek legt de tijd wanneer het streamen van audio naar de service nadat het ontvangen van de clienttoepassing wordt gestopt de `speech.endDetected` bericht van de service.

* Een clienttoepassing wordt verwerkt door de tweede inschakelen van een aanvraag meerdere inschakelen en op de hoogte gesteld door een antwoordbericht service om in te schakelen op de microfoon voor het verzamelen van invoer voor de tweede inschakelen. De *Start* waarde voor de `Microphone` metriek legt de tijd waarop de clienttoepassing kan de microfoon en wordt gestart met behulp van de invoer van de audio bron. De *End* waarde voor de `Microphone` metriek legt de tijd wanneer het streamen van audio naar de service nadat het ontvangen van de clienttoepassing wordt gestopt de `speech.endDetected` bericht van de service.

De *End* tijd-waarde voor de `Microphone` metriek legt de tijd wanneer het streamen van audio-invoer van de clienttoepassing wordt gestopt. In de meeste gevallen deze gebeurtenis treedt op kort nadat de client ontvangen de `speech.endDetected` bericht van de service. Clienttoepassingen kunnen controleren of dat ze goed die aan het protocol voldoen zijn door ervoor te zorgen dat de *End* tijd-waarde voor de `Microphone` metriek optreedt hoger is dan de ontvangst tijd-waarde voor de `speech.endDetected` bericht. En omdat er meestal een vertraging tussen het einde van een inschakelen en het begin van een andere inschakelen, clients conformiteit van protocol kunnen verifiëren door ervoor te zorgen dat de *Start* tijd van de `Microphone` metrische gegevens voor elke volgende turn correct legt de tijd wanneer de client *gestart* met behulp van de microfoon stream audio-invoer voor de service.

| Veld | Description | Gebruik |
| ----- | ----------- | ----- |
| Name | Microfoon | Vereist |
| Starten | De tijd wanneer de client aan de slag met audio-invoer van de microfoon of andere audiostream of een trigger hebt ontvangen van het sleutelwoord spotter | Vereist |
| Beëindigen | De tijd wanneer de client is gestopt met behulp van de microfoon of audio-stream | Vereist |
| Fout | Een beschrijving van de fout is opgetreden, indien van toepassing. Als de bewerkingen van de microfoon gelukt is, moeten clients laat dit veld weg. De maximale lengte van dit veld is 50 tekens. | Vereist voor foutgevallen, anders wordt weggelaten |

### <a name="metric-listeningtrigger"></a>Metrische gegevens `ListeningTrigger`
De `ListeningTrigger` metriek meet de tijd wanneer de gebruiker voert de actie die spraakinvoer initieert. De `ListeningTrigger` metriek is optioneel, maar de clients die met deze metriek worden uitgevers aangemoedigd om dit te doen.

Gebruik de volgende voorbeelden als richtlijnen voor de opname *Start* en *End* tijd van de waarden voor de `ListeningTrigger` metrische gegevens in uw clienttoepassing.

* Een clienttoepassing vereist dat een gebruiker moet druk op een fysieke knop om te starten van de microfoon. De *Start* voor deze metrische gegevens de tijd van de knop push legt-waarde. De *End* waarde legt de tijd wanneer de knop push-bewerking is voltooid.

* Een clienttoepassing maakt gebruik van een sleutelwoord spotter die 'altijd' luistert. Na het sleutelwoord spotter detecteert een woordgroep gesproken trigger de clienttoepassing leest de invoer van de microfoon en verzonden naar spraak-Service. De *Start* waarde voor deze metrische gegevens legt de tijd waarop het sleutelwoord spotter ontvangen audio die vervolgens de trigger-zin is gedetecteerd. De *End* waarde legt de tijd wanneer het laatste woord van de trigger woordgroep is gesproken door de gebruiker.

* Een clienttoepassing toegang heeft tot een constante audiostream en stilte/spraak detectie uitvoert op die audiostream in een *spraak detectiemodule*. De *Start* waarde voor deze metrische gegevens de tijd legt die de *spraak detectiemodule* ontvangen audio die vervolgens als spraak is gedetecteerd. De *End* waarde legt de tijd wanneer de *spraak detectiemodule* spraak gedetecteerd.

* Een clienttoepassing wordt verwerkt door de tweede inschakelen van een aanvraag meerdere inschakelen en op de hoogte gesteld door een antwoordbericht service om in te schakelen op de microfoon voor het verzamelen van invoer voor de tweede inschakelen. De clienttoepassing moet *niet* bevatten een `ListeningTrigger` metrische gegevens voor deze inschakelen.

| Veld | Description | Gebruik |
| ----- | ----------- | ----- |
| Name | ListeningTrigger | Optioneel |
| Starten | Het tijdstip waarop de luisterende client-trigger is gestart | Vereist |
| Beëindigen | De tijd waarop de luisterende client-trigger is beëindigd | Vereist |
| Fout | Een beschrijving van de fout is opgetreden, indien van toepassing. Als de triggerbewerking voltooid is, moeten clients laat dit veld weg. De maximale lengte van dit veld is 50 tekens. | Vereist voor foutgevallen, anders wordt weggelaten |

#### <a name="sample-message"></a>Voorbeeldbericht

Het volgende voorbeeld toont een telemetrie-bericht met zowel ReceivedMessages en metrische gegevens delen:

```HTML
Path: telemetry
Content-Type: application/json; charset=utf-8
X-RequestId: 123e4567e89b12d3a456426655440000
X-Timestamp: 2016-08-16T15:03:54.183Z

{
  "ReceivedMessages": [
    { "speech.hypothesis": [ "2016-08-16T15:03:48.171Z", "2016-08-16T15:03:48.331Z", "2016-08-16T15:03:48.881Z" ] },
    { "speech.endDetected": "2016-08-16T15:03:49.721Z" },
    { "speech.phrase": "2016-08-16T15:03:50.001Z" },
    { "turn.end": "2016-08-16T15:03:51.021Z" }
  ],
  "Metrics": [
    {
      "Name": "Connection",
      "Id": "A140CAF92F71469FA41C72C7B5849253",
      "Start": "2016-08-16T15:03:47.921Z",
      "End": "2016-08-16T15:03:48.000Z",
    },
    {
      "Name": "ListeningTrigger",
      "Start": "2016-08-16T15:03:48.776Z",
      "End": "2016-08-16T15:03:48.777Z",
    },
    {
      "Name": "Microphone",
      "Start": "2016-08-16T15:03:47.921Z",
      "End": "2016-08-16T15:03:51.921Z",
    },
  ],
}
```

## <a name="error-handling"></a>Foutafhandeling

Deze sectie beschrijft de soorten foutberichten en voorwaarden die uw toepassing nodig heeft om af te handelen.

### <a name="http-status-codes"></a>HTTP-statuscodes

Tijdens de aanvraag van de WebSocket-upgrade, spraak-Service retourneert mogelijk een van de standaard HTTP-statuscodes zoals `400 Bad Request`, enzovoort. Uw toepassing moet de foutvoorwaarden van deze correct verwerken.

#### <a name="authorization-errors"></a>Verificatiefouten

Als het onjuiste autorisatie is opgegeven tijdens de upgrade WebSocket Speech Service een HTTP retourneert `403 Forbidden` statuscode. Tussen de voorwaarden die u kunnen deze foutcode activeren zijn:

* Ontbrekende *autorisatie* koptekst

* Ongeldig verificatietoken

* Verlopen Autorisatietoken

De `403 Forbidden` foutbericht niet duiden op een probleem met de Speech-Service. Dit foutbericht geeft een probleem met de clienttoepassing.

### <a name="protocol-violation-errors"></a>Tot protocolschending

Als Speech Service eventuele protocolschendingen van een client detecteert, de service wordt beëindigd door de WebSocket-verbinding na het retourneren van een *statuscode* en *reden* voor de beëindiging. Clienttoepassingen kunnen deze informatie gebruiken om problemen op te lossen de schendingen.

#### <a name="incorrect-message-format"></a>Onjuiste indeling

Als een client verzendt een tekst of binaire bericht naar de service die is niet gecodeerd in de juiste indeling opgegeven in deze specificatie, de service, sluit u de verbinding met een *1007 Ongeldige nettolading gegevens* statuscode.

De service retourneert deze statuscode voor een aantal redenen, zoals wordt weergegeven in de volgende voorbeelden:

* "Onjuist berichtindeling. Binaire bericht heeft ongeldige header-Groottevoorvoegsel." De client verzonden een binaire bericht dat het voorvoegsel van een ongeldige header-grootte heeft.

* "Onjuist berichtindeling. Binaire bericht heeft ongeldige header-grootte." De client verzonden een binaire bericht dat een ongeldige header-grootte opgegeven.

* "Onjuist berichtindeling. Binaire-mailberichtkoppen in UTF-8-decodering is mislukt." De client verzonden een binaire bericht met kopteksten die zijn niet correct gecodeerd in UTF-8.

* "Onjuist berichtindeling. SMS-bericht bevat geen gegevens." De client verzonden een SMS-bericht dat geen van de hoofdtekstgegevens bevat.

* "Onjuist berichtindeling. SMS-bericht in UTF-8-decodering is mislukt." De client verzonden een SMS-bericht is niet correct gecodeerd in UTF-8.

* "Onjuist berichtindeling. SMS-bericht bevat geen scheidingsteken header." De client verzonden een SMS-bericht dat bevat geen scheidingsteken header of het scheidingsteken verkeerde kop gebruikt.

#### <a name="missing-or-empty-headers"></a>Headers ontbreekt of is leeg

Als een client verzendt een bericht dat u beschikt niet over de vereiste headers *X-RequestId* of *pad*, de service wordt gesloten voor de verbinding met een *1002 protocolfout* statuscode. Het bericht is 'koptekst ontbreekt/leeg. {Headernaam}."

#### <a name="requestid-values"></a>Aanvraag-id-waarden

Als een client een bericht verzendt dat aangeeft een *X-RequestId* koptekst met een onjuiste indeling bevatten, de service de verbinding wordt gesloten en retourneert een *1002 protocolfout* status. Het bericht wordt 'Ongeldige aanvraag. De waarde van de header X-aanvraag-id is niet opgegeven in niet-dash-UUID-opmaak."

#### <a name="audio-encoding-errors"></a>Audio coderingsfouten

Als een client verzendt een audio segment een inschakelen en de audio-indeling initieert of codering niet met de vereiste specificatie overeenkomt, de service de verbinding wordt gesloten en retourneert een *1007 Ongeldige nettolading gegevens* statuscode. Het bericht geeft aan dat de codering van de bron van fout-indeling.

#### <a name="requestid-reuse"></a>Hergebruik van de aanvraag-id

Nadat een inschakelen is voltooid, als een client verzendt een bericht weergegeven waarin wordt gebruikgemaakt van de aanvraag-id van die inschakelen, de service de verbinding wordt gesloten en retourneert een *1002 protocolfout* statuscode. Het bericht wordt 'Ongeldige aanvraag. Hergebruik van aanvraag-id is niet toegestaan."

## <a name="connection-failure-telemetry"></a>Verbinding mislukt telemetrie

Om ervoor te zorgen de best mogelijke gebruikerservaring, clients verwittigt Speech-Service van de tijdstempels voor belangrijke controlepunten binnen een verbinding met behulp van de *telemetrie* bericht. Is het belangrijk dat clients de service van verbindingen die zijn uitgevoerd, maar kan niet informeren.

Voor elke verbindingspoging die niet zijn geslaagd, clients maken een *telemetrie* bericht met een unieke *X-RequestId* header-waarde. Omdat de client kan een verbinding tot stand brengen de *ReceivedMessages* veld in de JSON-hoofdtekst kan worden weggelaten. Alleen de `Connection` vermelding in de *metrische gegevens* veld is opgenomen. Deze vermelding bestaat uit het begin en einde tijdstempels, evenals de foutstatus die is opgetreden.

### <a name="connection-retries-in-telemetry"></a>Verbindingspogingen in telemetrie

Clients dient te onderscheiden *nieuwe pogingen* van *meerdere verbindingspogingen* door de gebeurtenis die wordt geactiveerd de verbindingspoging. Verbindingspogingen die via een programma worden uitgevoerd zonder invoer van de gebruiker zijn nieuwe pogingen. Meerdere verbindingspogingen die worden uitgevoerd in reactie op invoer van de gebruiker zijn meerdere verbindingspogingen. Clients geven elke verbindingspoging die door de gebruiker heeft geactiveerd een unieke *X-RequestId* en *telemetrie* bericht. Clients gebruiken de *X-RequestId* voor programmatische nieuwe pogingen. Als meerdere pogingen zijn gedaan voor een enkele verbindingspoging elke nieuwe poging wordt opgenomen als een `Connection` vermelding in de *telemetrie* bericht.

Stel bijvoorbeeld dat een gebruiker spreekt het sleutelwoord-trigger voor het starten van een verbinding en de eerste verbindingspoging is mislukt vanwege fouten in DNS. Echter, een tweede poging die via een programma is gemaakt door de client is geslaagd. Omdat de client opnieuw geprobeerd de verbinding zonder aanvullende invoer van de gebruiker, de client maakt gebruik van een enkel *telemetrie* bericht met meerdere `Connection` vermeldingen voor het beschrijven van de verbinding.

Als een ander voorbeeld: Stel dat een gebruiker spreekt het sleutelwoord-trigger voor het starten van een verbinding en deze verbindingspoging is mislukt na drie pogingen. De client geeft, probeert verbinding maken met de service stopt, en wordt de gebruiker geïnformeerd dat er iets misgegaan is. De gebruiker spreekt vervolgens de trigger sleutelwoord opnieuw. Deze tijd, Stel dat de client verbinding maakt met de service. Na de verbinding te maken, verzendt de client onmiddellijk een *telemetrie* bericht naar de service die drie bevat `Connection` vermeldingen die de verbindingsfouten beschrijven. Na de `turn.end` bericht, verzendt de client een andere *telemetrie* bericht met een beschrijving van de verbinding is geslaagd.

## <a name="error-message-reference"></a>Naslaginformatie over foutberichten

### <a name="http-status-codes"></a>HTTP-statuscodes

| HTTP-statuscode | Description | Problemen oplossen |
| - | - | - |
| 400-Ongeldige aanvraag | De client verzonden een WebSocket-verbindingsaanvraag onjuist is. | Controleer dat u alle vereiste parameters en HTTP-headers opgegeven en of de waarden correct zijn. |
| 401-niet toegestaan | De client bevat niet de vereiste autorisatie-informatie. | Controleer of u het bericht verzendt de *autorisatie* -header in de WebSocket-verbinding. |
| 403-verboden | Autorisatie-informatie voor de client worden verzonden, maar deze is ongeldig. | Controleer dat u niet de waarde van een verlopen of ongeldig verzendt de *autorisatie* header. |
| 404 – Niet gevonden | De client probeert te krijgen van een URL-pad wordt niet ondersteund. | Controleer dat u de juiste URL voor de WebSocket-verbinding gebruikt. |
| 500-fout | De service is een interne fout opgetreden en kan niet voldoen aan de aanvraag. | Deze fout is tijdelijk in de meeste gevallen. De aanvraag opnieuw. |
| 503 Service niet beschikbaar | De service is niet beschikbaar voor het afhandelen van de aanvraag. | Deze fout is tijdelijk in de meeste gevallen. De aanvraag opnieuw. |

### <a name="websocket-error-codes"></a>WebSocket-foutcodes

| WebSocketsStatus code | Description | Problemen oplossen |
| - | - | - |
| 1000 normale sluiting | De service verbroken de WebSocket-verbinding zonder fouten. | Als de sluiting WebSocket werd niet verwacht, lees opnieuw in de documentatie om ervoor te zorgen dat u begrijpt hoe en wanneer de service de WebSocket-verbinding kunt beëindigen. |
| 1002 Protocol Error | De client kan niet voldoen aan de vereisten voor protocol. | Zorg ervoor dat u inzicht in de documentatie van het protocol en schakel over de vereisten. Raadpleeg de vorige documentatie over de fout redenen om te zien als u vereisten voor protocol bent schenden. |
| 1007 Invalid Payload Data | De client heeft een ongeldige nettolading verzonden in een protocolbericht. | Controleer het laatste bericht dat u hebt verzonden naar de service op fouten. Raadpleeg de vorige documentatie over fouten met nettolading. |
| 1011 Server Error | De service is een interne fout opgetreden en kan niet voldoen aan de aanvraag. | Deze fout is tijdelijk in de meeste gevallen. De aanvraag opnieuw. |

## <a name="related-topics"></a>Verwante onderwerpen

Zie een [JavaScript SDK](https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript) dat wil zeggen een implementatie van de Speech-Service op basis van WebSocket-protocol.
