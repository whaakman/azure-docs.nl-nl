---
title: Microsoft-spraakherkenning WebSocket-protocol | Microsoft Docs
description: Documentatie voor spraak-Service op basis van WebSockets protocol
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/15/2017
ms.author: zhouwang
ms.openlocfilehash: 17954536e8bdb49c09204c2e522586b79cb1bef5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344719"
---
# <a name="speech-service-websocket-protocol"></a>Spraak Service WebSocket-protocol

  Spraak-Service is een cloud-gebaseerde platform met functionaliteit voor de meest geavanceerde algoritmen die beschikbaar zijn voor converteren gesproken audio naar tekst. De Service voor spraak-protocol bepaalt de [verbinding instellen](#connection-establishment) tussen clienttoepassingen en de service en de berichten voor spraakherkenning uitgewisseld tussen collega's ([client afkomstig berichten](#client-originated-messages)en [berichten afkomstig is van een service](#service-originated-messages)). Bovendien [telemetrieberichten](#telemetry-schema) en [foutafhandeling](#error-handling) worden beschreven.

## <a name="connection-establishment"></a>Verbinding tot stand brengen

Het protocol spraak Service volgens de specificatie van WebSocket-standaard [IETF RFC 6455](https://tools.ietf.org/html/rfc6455). Een HTTP-aanvraag met HTTP-headers die wijzen op de client de verbinding een upgrade uitvoert naar een WebSocket in plaats van HTTP-semantiek wil in eerste instantie een WebSocket-verbinding. De server geeft aan dat de bereid om deel te nemen in de WebSocket-verbinding door te retourneren van een HTTP `101 Switching Protocols` antwoord. Nadat de uitwisseling van deze handshake, zowel de client als de service de socket open houden en beginnen met een protocol op basis van een bericht te verzenden en ontvangen van gegevens.

Om te beginnen met de WebSocket-handshake, verzendt de clienttoepassing een HTTPS-GET-aanvraag naar de service. Dit omvat standaard upgrade WebSocket-headers samen met andere headers die specifiek voor spraak zijn.

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

Alle spraak-aanvragen vereisen de [TLS](https://en.wikipedia.org/wiki/Transport_Layer_Security) versleuteling. Het gebruik van niet-versleutelde spraak aanvragen wordt niet ondersteund. TLS-versie wordt ondersteund:

* TLS 1.2

### <a name="connection-identifier"></a>Id van de verbinding

Spraak Service vereist dat alle clients zijn onder andere een unieke ID voor het identificeren van de verbinding. Clients *moet* omvatten de *X ConnectionId* header wanneer ze een WebSocket-handshake starten. De *X ConnectionId* header moet een [Universeel unieke id](https://en.wikipedia.org/wiki/Universally_unique_identifier) (UUID)-waarde. Upgrade WebSocket-aanvragen die geen bevatten de *X ConnectionId*, Geef een waarde op voor de *X ConnectionId* -kop, of geen geldige UUID-waarde worden geweigerd door de service met een HTTP- `400 Bad Request` antwoord.

### <a name="authorization"></a>Autorisatie

Naast de standaard WebSocket-handshake headers spraak-aanvragen vereisen een *autorisatie* header. Verbinding zonder deze header worden geweigerd door de service met een HTTP-aanvragen `403 Forbidden` antwoord.

De *autorisatie* header moet een toegangstoken JSON Web Token (JWT) bevatten.

Zie voor informatie over het abonneren en de API-sleutels die worden gebruikt voor het ophalen van de toegang van geldige JWT-tokens verkrijgen, de [abonnement cognitieve Services](https://azure.microsoft.com/try/cognitive-services/) pagina.

De API-sleutel wordt doorgegeven aan de service voor beveiligingstokens. Bijvoorbeeld:

``` HTTP
POST https://api.cognitive.microsoft.com/sts/v1.0/issueToken
Content-Length: 0
```

De volgende headerinformatie is vereist voor token-toegang.

| Naam | Indeling | Beschrijving |
|----|----|----|
| OCP-Apim-Subscription-Key | ASCII | Uw abonnementssleutel |

Retourneert de token service de toegang JWT-token als `text/plain`. En vervolgens de JWT wordt doorgegeven als een `Base64 access_token` naar de handshake als een *autorisatie* header voorafgegaan door de tekenreeks `Bearer`. Bijvoorbeeld:

`Authorization: Bearer [Base64 access_token]`

### <a name="cookies"></a>Cookies

Clients *moet* ondersteuning bieden voor cookies zoals opgegeven in de HTTP- [RFC 6265](https://tools.ietf.org/html/rfc6265).

### <a name="http-redirection"></a>HTTP-omleiding

Clients *moet* ondersteuning voor de omleiding van standaard mechanismen opgegeven door de [HTTP protocol specification](http://www.w3.org/Protocols/rfc2616/rfc2616.html).

### <a name="speech-endpoints"></a>Spraak-eindpunten

Clients *moet* een juiste spraak Service-eindpunt te gebruiken. Het eindpunt is gebaseerd op de opname- en taalinstellingen. De tabel ziet u enkele voorbeelden.

| Modus | Pad | Service-URI |
| -----|-----|-----|
| Interactief | /Speech/Recognition/Interactive/cognitiveservices/V1 |https://speech.platform.bing.com/speech/recognition/interactive/cognitiveservices/v1?language=pt-BR |
| Gesprek | /Speech/Recognition/CONVERSATION/cognitiveservices/V1 |https://speech.platform.bing.com/speech/recognition/conversation/cognitiveservices/v1?language=en-US |
| dicteren | /Speech/Recognition/Dictation/cognitiveservices/V1 |https://speech.platform.bing.com/speech/recognition/dictation/cognitiveservices/v1?language=fr-FR |

Zie voor meer informatie de [Service URI](../GetStarted/GetStartedREST.md#service-uri) pagina.

### <a name="report-connection-problems"></a>Problemen met de verbinding

Clients moeten alle problemen aangetroffen tijdens het maken van een verbinding onmiddellijk rapporteren. De message protocol voor rapportage mislukte verbindingen wordt beschreven in [verbinding mislukt telemetrie](#connection-failure-telemetry).

### <a name="connection-duration-limitations"></a>Duur van de beperkingen van verbinding

In vergelijking met typische web service HTTP-verbindingen, WebSocket-verbindingen laatste een *lang* tijd. Spraak Service plaatst beperkingen op de duur van de WebSocket-verbindingen met de service:

 * De maximale tijdsduur voor een actieve WebSocket-verbinding is 10 minuten. Er is een verbinding actief als de service of de client WebSocket-berichten via deze verbinding verzendt. De service verbreekt de verbinding zonder waarschuwing wanneer de limiet is bereikt. Clients moeten gebruikersscenario's die niet is vereist voor de verbinding actief is op of in de buurt van de levensduur van het maximumaantal verbindingen blijft ontwikkelen.

 * De maximale tijdsduur voor een inactieve WebSocket-verbinding is 180 seconden. Er is een verbinding inactief als noch de client als de service een WebSocket-bericht via de verbinding verzonden. Nadat de maximale inactieve levensduur is bereikt, beëindigt de service de inactieve WebSocket-verbinding.

## <a name="message-types"></a>Berichttypen

Nadat een WebSocket-verbinding tot stand is gebracht tussen de client en de service, kunnen u berichten met zowel de client als de service verzenden. Deze sectie beschrijft de indeling van deze WebSocket-berichten.

[IETF RFC 6455](https://tools.ietf.org/html/rfc6455) geeft aan dat de gegevens voor WebSocket-berichten kunnen worden verzonden met behulp van een tekst of binaire codering. De twee coderingen gebruiken verschillende indelingen voor de kabel. Elke indeling is geoptimaliseerd voor efficiënte codering, verzending en decoderen van de nettolading van het bericht.

### <a name="text-websocket-messages"></a>WebSocket-SMS-berichten

Tekstberichten WebSocket voert een nettolading van tekstinformatie die uit een gedeelte van de kop- en een instantie van elkaar gescheiden door het vertrouwde dubbele regelterugloop newline-paar gebruikt voor HTTP-berichten bestaat. En, zoals HTTP-berichten WebSocket tekstberichten kopteksten in opgeven *naam: waarde* indeling gescheiden door een combinatie van één regelterugloop newline. De tekst die wordt opgenomen in een tekstbericht WebSocket *moet* gebruiken [UTF-8](https://tools.ietf.org/html/rfc3629) codering.

Tekstberichten WebSocket moeten een berichtpad opgeven in de header *pad*. De waarde van deze header moet een van de spraak protocol berichttypen gedefinieerd verderop in dit document.

### <a name="binary-websocket-messages"></a>Binaire WebSocket-berichten

Binaire WebSocket-berichten voert een binaire nettolading. In het protocol spraak Service audio verzonden naar en ontvangen van de service met behulp van de binaire WebSocket-berichten. Alle andere berichten zijn WebSocket tekstberichten. 

Zoals WebSocket tekstberichten bestaan binaire WebSocket-berichten uit een header en een hoofdtekst sectie. Geef de eerste 2-bytes van het binaire WebSocket-bericht [big endian](https://en.wikipedia.org/wiki/Endianness) volgorde, de grootte van de 16-bits geheel getal van de headersectie. De grootte van de sectie minimale header is 0 bytes. De maximale grootte is 8192 bytes. De tekst in de headers van binaire WebSocket-berichten *moet* gebruiken [US-ASCII-](https://tools.ietf.org/html/rfc20) codering.

Kopteksten in een binaire WebSocket-bericht worden in dezelfde indeling als in de WebSocket-tekstberichten gecodeerd. De *name-value* indeling wordt gescheiden door een combinatie van één regelterugloop newline. Binaire WebSocket-berichten moeten een berichtpad opgeven in de header *pad*. De waarde van deze header moet een van de spraak protocol berichttypen gedefinieerd verderop in dit document.

Tekst- en binaire WebSocket-berichten worden gebruikt in het protocol voor spraak-Service. 

## <a name="client-originated-messages"></a>Berichten afkomstig is van een client

Nadat de verbinding tot stand is gebracht, worden zowel de client als de service kunnen starten om berichten te verzenden. Deze sectie beschrijft de indeling en de nettolading van berichten die clienttoepassingen naar-spraak-Service verzenden. De sectie [berichten afkomstig is van een Service](#service-originated-messages) geeft de berichten die afkomstig zijn uit spraak-Service en worden verzonden naar de clienttoepassingen.

De belangrijkste berichten is verzonden door de client naar de services zijn `speech.config`, `audio`, en `telemetry` berichten. Voordat we elk bericht in detail beschouwen, vereist de algemene berichtkoppen voor alle berichten in deze worden beschreven.

### <a name="required-message-headers"></a>Vereiste berichtkoppen

De volgende headers zijn vereist voor alle berichten afkomstig is van een client.

| Koptekst | Waarde |
|----|----|
| Pad | Het pad weergegeven zoals opgegeven in dit document |
| X-RequestId | UUID in de indeling 'Nee-dash' |
| X-tijdstempel | Client UTC klok tijdstempel in ISO 8601-notatie |

#### <a name="x-requestid-header"></a>De header X-RequestId

Aanvragen die afkomstig is van een client zijn uniek geïdentificeerd door de *X RequestId* berichtkop. Deze header is vereist voor alle berichten afkomstig is van een client. De *X RequestId* headerwaarde moet een UUID 'Nee-dash' vorm, bijvoorbeeld *123e4567e89b12d3a456426655440000*. Deze *kan niet* worden in de canonieke vorm *123e4567-e89b-12d3-a456-426655440000*. Aanvragen zonder een *X RequestId* koptekst of een headerwaarde die gebruikmaakt van de verkeerde indeling voor UUID's ervoor zorgen dat de service de WebSocket-verbinding is beëindigd.

#### <a name="x-timestamp-header"></a>De header X-tijdstempel

Elk bericht naar-spraak-Service is verzonden door een clienttoepassing *moet* bevatten een *X tijdstempel* header. De waarde voor deze header is de tijd wanneer de client het bericht verzendt. Aanvragen zonder een *X tijdstempel* header of met een headerwaarde die gebruikmaakt van de verkeerde indeling ervoor zorgen dat de service de WebSocket-verbinding is beëindigd.

De *X tijdstempel* headerwaarde moet van het formulier 'yyyy'-'MM'-'dd'T' HH': 'mm':'ss '.' fffffffZ' waar 'fffffff' is een fractie van een seconde. Bijvoorbeeld, '12,5' '12 + 5/10 seconden en '12.526' betekent 12 plus 526/1000 seconden'. Deze indeling voldoet aan [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) en, in tegenstelling tot de standaard HTTP *datum* koptekst kan milliseconde resolutie te geven. Clienttoepassingen mogelijk ronde van tijdstempels naar de dichtstbijzijnde milliseconde. Clienttoepassingen nodig hebben om ervoor te zorgen dat de apparaatklok nauwkeurig tijd met behulp van houdt een [Network Time Protocol (NAP) server](https://en.wikipedia.org/wiki/Network_Time_Protocol).

### <a name="message-speechconfig"></a>Bericht `speech.config`

Spraak-Service moet de kenmerken van uw toepassing om de best mogelijke spraakherkenning kent. De gegevens van de vereiste kenmerken bevat informatie over het apparaat en het besturingssysteem die ook door uw toepassing. Geef van deze informatie in de `speech.config` bericht.

Clients *moet* verzenden een `speech.config` onmiddellijk nadat ze bij het maken van de verbinding met spraak-Service en voordat ze een verzendt bericht `audio` berichten. Verzending van een `speech.config` bericht slechts eenmaal per verbinding.

| Veld | Beschrijving |
|----|----|
| WebSocket-berichtcodering | Tekst |
| Hoofdtekst | De nettolading als een JSON-structuur |

#### <a name="required-message-headers"></a>Vereiste berichtkoppen

| Header-naam | Waarde |
|----|----|
| Pad | `speech.config` |
| X-tijdstempel | Client UTC klok tijdstempel in ISO 8601-notatie |
| Inhoudstype | Application/json; charset = utf-8 |

Net als bij alle afkomstig is van een client berichten in het protocol spraak-Service de `speech.config` bericht *moet* bevatten een *X tijdstempel* header die legt de client UTC kloktijd waarop het bericht is verzonden met de service. De `speech.config` bericht *heeft geen* vereisen een *X RequestId* header omdat dit bericht is niet gekoppeld aan een bepaalde spraak-aanvraag.

#### <a name="message-payload"></a>Bericht-nettolading
De nettolading van de `speech.config` bericht is een JSON-structuur die informatie over de toepassing bevat. Het volgende voorbeeld ziet deze informatie. Client- en context-informatie is opgenomen in de *context* element van de JSON-structuur. 

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

Het element system.version van de `speech.config` bericht bevat de versie van de spraak SDK-software die wordt gebruikt door de clienttoepassing of het apparaat. De waarde is in het formulier *major.minor.build.branch*. U kunt weglaten de *vertakking* onderdeel als niet van toepassing is.

##### <a name="os-element"></a>OS-element

| Veld | Beschrijving | Gebruik |
|-|-|-|
| OS.platform | De OS-platform waarop de toepassing, bijvoorbeeld Windows, Android, iOS of Linux |Vereist |
| OS.name | De OS-productnaam, bijvoorbeeld Debian of Windows 10 | Vereist |
| OS.Version | De versie van het besturingssysteem in het formulier *major.minor.build.branch* | Vereist |

##### <a name="device-element"></a>Apparaat-element

| Veld | Beschrijving | Gebruik |
|-|-|-|
| Device.Manufacturer | De fabrikant van de hardware | Vereist |
| Device.model | Het model | Vereist |
| Device.Version | De versie van het apparaat software is geleverd door de fabrikant van het apparaat. Deze waarde wordt een versie van het apparaat dat kan worden gevolgd door de fabrikant. | Vereist |

### <a name="message-audio"></a>Bericht `audio`

Spraak ingeschakeld clienttoepassingen verzenden via de audiostroom converteren naar een reeks audio segmenten audio naar-spraak-Service. Elk segment van audio voert een segment van de gesproken audio die moet worden omgezet door de service. De maximale grootte van een enkele audio segment is 8192 bytes. Audiostroom berichten zijn *binaire WebSocket berichten*.

Clients gebruiken de `audio` bericht naar een audio chunk verzenden naar de service. Clients lezen van de microfoon in segmenten audio en deze segmenten verzenden naar-spraak-Service voor schrijffouten. De eerste `audio` bericht moet een goed ingedeelde kop die goed aangeeft dat de audio aan een van de codering indelingen ondersteund door de service voldoet bevatten. Aanvullende `audio` berichten alleen de binaire audio stream gegevens lezen uit de microfoon bevatten.

Clients kunnen eventueel verzenden een `audio` bericht met een lengte van nul-instantie. Dit bericht geeft aan dat de service die de client bekend is dat de gebruiker spreken gestopt de utterance is voltooid en de microfoon is uitgeschakeld.

Spraak-Service gebruikt de eerste `audio` bericht met een unieke aanvraag-id om het begin van een nieuwe aanvraag/antwoord-cyclus signaal of *schakelen*. Nadat de service ontvangt een `audio` bericht met een nieuwe aanvraag-id wordt verwijderd in de wachtrij of niet-verzonden berichten die gekoppeld aan een eerdere inschakelen zijn.

| Veld | Beschrijving |
|-------------|----------------|
| WebSocket-berichtcodering | Binair bestand |
| Hoofdtekst | De binaire gegevens voor de audio chunk. Maximale grootte is 8192 bytes. |

#### <a name="required-message-headers"></a>Vereiste berichtkoppen

De volgende headers zijn vereist voor alle `audio` berichten.

| Koptekst         |  Waarde     |
| ------------- | ---------------- |
| Pad | `audio` |
| X-RequestId | UUID in de indeling 'Nee-dash' |
| X-tijdstempel | Client UTC klok tijdstempel in ISO 8601-notatie |
| Inhoudstype | Het type van het audio-inhoud. Het type moet een *x-audio-wav* (PCM) of *audio/zijde* (ZIJDE). |

#### <a name="supported-audio-encodings"></a>Ondersteunde audio coderingen

Deze sectie beschrijft de audio-codecs ondersteund door de Service spraak.

##### <a name="pcm"></a>PCM

Spraak Service accepteert niet-gecomprimeerde pulse code modulatie (PCM) audio. Audio wordt verzonden naar de service in [WAV](https://en.wikipedia.org/wiki/WAV) -indeling, zodat de eerste audiofragment Segmentselectie *moet* bevatten een geldige [Resource Interchange File Format](https://en.wikipedia.org/wiki/Resource_Interchange_File_Format) (RIFF)-header. Als een client een inschakelen met een audio-segment dat doet initieert *niet* een geldige RIFF koptekst omvatten, maar de service wordt de aanvraag geweigerd en wordt de WebSocket-verbinding verbroken.

PCM-audio *moet* actieve op 16 kHz met 16-bits per voorbeeld en één kanaal (*riff-16khz-16-bits-mono-pcm*). Spraak Service biedt geen ondersteuning voor aansluiting audio stromen en audio-stromen die geen van de opgegeven bitsnelheid, samplefrequentie of aantal kanalen gebruikmaken weigert.

##### <a name="opus"></a>Opus

Opus is een open, royaltyvrije, uiterst veelzijdige audio codec. Spraak-Service ondersteunt Opus met een constante bitsnelheid van `32000` of `16000`. Alleen de `OGG` container voor Opus wordt momenteel ondersteund die is opgegeven door de `audio/ogg` MIME-type.

Voor het gebruik van Opus wijzigen de [JavaScript voorbeeld](https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript/blob/master/samples/browser/Sample.html#L101) en wijzig de `RecognizerSetup` methode om te retourneren.

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

Mensen doen niet expliciet signaleren wanneer ze klaar bent met spreken. Alle toepassingen die spraak geaccepteerd als invoer twee opties heeft voor het verwerken van het einde van spraak in een audiostroom: service einde van spraak detectie en detectie van de client-einde van spraak. Service-end van spraak detectie levert doorgaans een betere gebruikerservaring van deze twee opties.

##### <a name="service-end-of-speech-detection"></a>Service-end van spraak detectie

De ervaring voor de ideale Handsfree spraak bouwen, zodat toepassingen de service om te detecteren wanneer de gebruiker heeft voltooid spreken. Clients verzenden audio van de microfoon als *audio* segmenten gedownload totdat de service stilte detecteert en reageert op een `speech.endDetected` bericht.

##### <a name="client-end-of-speech-detection"></a>Client-end van spraak detectie

Clienttoepassingen die toestaan dat de gebruiker het einde van spraak op een bepaalde manier kunnen geeft ook de service die signaal. Een clienttoepassing zijn wellicht een 'Stop' of ' ' uitknop waarmee kan de gebruiker op. Als u wilt aangeven einde van spraak, clienttoepassingen verzenden een *audio* chunk bericht met een lengte van nul-instantie. Spraak-Service wordt dit bericht als het einde van de binnenkomende audiostroom geïnterpreteerd.

### <a name="message-telemetry"></a>Bericht `telemetry`

Clienttoepassingen *moet* Bevestig het einde van elke inschakelen door telemetrie over de Schakel verzenden naar-spraak-Service. Schakel-end bevestiging kan spraak-Service om ervoor te zorgen dat alle berichten die nodig zijn voor de voltooiing van de aanvraag en de reactie daarop correct zijn ontvangen door de client. Schakel-end bevestiging kan ook spraak-Service om te controleren dat de clienttoepassingen worden uitgevoerd zoals verwacht. Deze informatie is bijzonder handig als u het oplossen van uw toepassing met spraak hulp nodig hebt.

Clients het einde van een beurt moeten bevestigen via een `telemetry` bericht snel na ontvangst een `turn.end` bericht. -Clients om te bevestigen moeten proberen de `turn.end` zo snel mogelijk. Als een clienttoepassing mislukt voor het bevestigen van het einde inschakelen, kan de verbinding met een fout spraak-Service worden beëindigd. Clients kunnen slechts één moeten verzenden `telemetry` bericht voor elke aanvraag en -antwoord geïdentificeerd door de *X RequestId* waarde.

| Veld | Beschrijving |
| ------------- | ---------------- |
| WebSocket-berichtcodering | Tekst |
| Pad | `telemetry` |
| X-tijdstempel | Client UTC klok tijdstempel in ISO 8601-notatie |
| Inhoudstype | `application/json` |
| Hoofdtekst | Een JSON-structuur met clientinformatie over het inschakelen |

Het schema voor de hoofdtekst van de `telemetry` bericht is gedefinieerd in de [telemetrie schema](#telemetry-schema) sectie.

#### <a name="telemetry-for-interrupted-connections"></a>Telemetrie voor onderbroken verbindingen

Als de netwerkverbinding tijdens een beurt uitvalt en de client heeft *niet* ontvangen een `turn.end` bericht uit de service, de client verzendt een `telemetry` bericht. Dit bericht wordt de mislukte aanvragen met de volgende keer dat de client verbinding met de service maakt. Clients hoeven niet meteen probeert een verbinding met het verzenden van de `telemetry` bericht. Het bericht kan de buffer opgeslagen op de client en verzonden via een toekomstige verbinding van de gebruiker heeft aangevraagd. De `telemetry` bericht voor de mislukte aanvragen *moet* gebruiken de *X RequestId* waarde van de mislukte aanvragen. Deze kan worden verzonden naar de service zodra een verbinding tot stand is gebracht, zonder te wachten op verzenden of ontvangen voor andere berichten.

## <a name="service-originated-messages"></a>Berichten afkomstig is van een service

Deze sectie beschrijft de berichten die afkomstig zijn uit spraak-Service en worden verzonden naar de client. Spraak Service houdt een register van de client-mogelijkheden en genereert de berichten die zijn vereist voor elke client, dus niet dat alle clients ontvangen alle berichten die hier worden beschreven. Voor beknopt alternatief bevat berichten wordt verwezen door de waarde van de *pad* header. Bijvoorbeeld, is een WebSocket-SMS-bericht met de *pad* waarde `speech.hypothesis` als een speech.hypothesis-bericht.

### <a name="message-speechstartdetected"></a>Bericht `speech.startDetected`

De `speech.startDetected` bericht geeft aan dat spraak Service spraak gedetecteerd in de audiostroom.

| Veld | Beschrijving |
| ------------- | ---------------- |
| WebSocket-berichtcodering | Tekst |
| Pad | `speech.startDetected` |
| Inhoudstype | Application/json; charset = utf-8 |
| Hoofdtekst | De JSON-structuur die informatie over de voorwaarden bevat wanneer het begin van spraak is gedetecteerd. De *Offset* veld in deze structuur geeft de verschuiving (in eenheden van 100 nanoseconden) wanneer spraak is gedetecteerd in de audiostroom, ten opzichte van het begin van de stroom. |

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

Tijdens spraakherkenning genereert spraak Service periodiek hypothesen de woorden die de service die wordt herkend. Spraak Service verzendt deze hypothesen naar de client ongeveer elke 300 milliseconden. De `speech.hypothesis` geschikt *alleen* voor het verbeteren van de gebruikerservaring van spraak. U moet eventuele afhankelijkheden van de inhoud of de nauwkeurigheid van de tekst niet in deze berichten nemen.

 De `speech.hypothesis` bericht is van toepassing op clients die de mogelijkheden van sommige tekst rendering en feedback wilt geven near-realtime van de erkenning wordt uitgevoerd voor de persoon die is spreken.

| Veld | Beschrijving |
| ------------- | ---------------- |
| WebSocket-berichtcodering | Tekst |
| Pad | `speech.hypothesis` |
| X-RequestId | UUID in de indeling 'Nee-dash' |
| Inhoudstype | application/json |
| Hoofdtekst | De spraak hypothese JSON-structuur |

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

De *Offset* element geeft de verschuiving (in eenheden van 100 nanoseconden) wanneer de woordgroep wordt herkend, ten opzichte van het begin van de audiostroom.

De *duur* element geeft de duur (in eenheden van 100 nanoseconden) van deze zin spraak.

Clients moeten veronderstellingen over de frequentie, timing of tekst in een hypothese spraak of de consistentie van de tekst in een hypothesen twee spraak niet maken. De hypothesen zijn alleen momentopnamen in het proces schrijffouten in de service. Deze weergeven een stabiele opeenstapeling van schrijffouten niet. Bijvoorbeeld een eerste spraak hypothese bevat mogelijk de woorden 'fijn plezier' en de tweede hypothese bevat mogelijk de woorden 'vreemd uitzien vinden.' Spraak-Service uitvoeren niet na verwerking (bijvoorbeeld hoofdlettergebruik, leestekens) op de tekst in de hypothese spraak.

### <a name="message-speechphrase"></a>Bericht `speech.phrase`

Wanneer spraak Service bepaalt dat er onvoldoende informatie om het resultaat een herkenning die niet verandert, het resultaat van de service een `speech.phrase` bericht. Spraak Service produceert deze resultaten nadat wordt gedetecteerd dat de gebruiker een zin of woordgroep is voltooid.

| Veld | Beschrijving |
| ------------- | ---------------- |
| WebSocket-berichtcodering | Tekst |
| Pad | `speech.phrase` |
| Inhoudstype | application/json |
| Hoofdtekst | De woordgroep spraak JSON-structuur |

De JSON-schema van spraak woordgroep bevat de volgende velden: `RecognitionStatus`, `DisplayText`, `Offset`, en `Duration`. Zie voor meer informatie over deze velden [schrijffouten antwoorden](../concepts.md#transcription-responses).

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

De `speech.endDetected` bericht geeft aan dat de clienttoepassing moet worden stopgezet streaming geluid om de service.

| Veld | Beschrijving |
| ------------- | ---------------- |
| WebSocket-berichtcodering | Tekst |
| Pad | `speech.endDetected` |
| Hoofdtekst | De JSON-structuur met de verschuiving als het einde van spraak is gedetecteerd. De offset wordt weergegeven in eenheden van 100 nanoseconden verschuiving vanaf het begin van audio die wordt gebruikt voor herkenning. |
| Inhoudstype | Application/json; charset = utf-8 |

#### <a name="sample-message"></a>Voorbeeldbericht

```HTML
Path: speech.endDetected
Content-Type: application/json; charset=utf-8
X-RequestId: 123e4567e89b12d3a456426655440000

{
  "Offset": 0
}
```

De *Offset* element geeft de verschuiving (in eenheden van 100 nanoseconden) wanneer de woordgroep wordt herkend, ten opzichte van het begin van de audiostroom.

### <a name="message-turnstart"></a>Bericht `turn.start`

De `turn.start` geeft aan het begin van een inschakelen vanuit het perspectief van de service. De `turn.start` bericht is altijd het eerste response-bericht wordt weergegeven voor elke aanvraag. Als u geen ontvangt een `turn.start` bericht wordt weergegeven, wordt ervan uitgegaan dat de status van de serviceverbinding is ongeldig.

| Veld | Beschrijving |
| ------------- | ---------------- |
| WebSocket-berichtcodering | Tekst |
| Pad | `turn.start` |
| Inhoudstype | Application/json; charset = utf-8 |
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

De hoofdtekst van de `turn.start` bericht is een JSON-structuur die context voor het begin van de beurt bevat. De *context* element bevat een *serviceTag* eigenschap. Deze eigenschap geeft u een tagwaarde dat de service aan de beurt is gekoppeld. Deze waarde kan worden gebruikt door Microsoft bij twijfel over het oplossen van fouten in uw toepassing.

### <a name="message-turnend"></a>Bericht `turn.end`

De `turn.end` geeft het einde van een inschakelen vanuit het perspectief van de service. De `turn.end` bericht is altijd het laatste response-bericht wordt weergegeven voor elke aanvraag. Clients kunnen de ontvangst van dit bericht gebruiken als een signaal voor het opruimen van activiteiten en overstappen naar een niet-actieve status. Als u geen ontvangt een `turn.end` bericht wordt weergegeven, wordt ervan uitgegaan dat de status van de serviceverbinding is ongeldig. In deze gevallen kan de bestaande verbinding met de service te sluiten en opnieuw verbinding.

| Veld | Beschrijving |
| ------------- | ---------------- |
| WebSocket-berichtcodering | Tekst |
| Pad | `turn.end` |
| Hoofdtekst | Geen |

#### <a name="sample-message"></a>Voorbeeldbericht

```HTML
Path: turn.end
X-RequestId: 123e4567e89b12d3a456426655440000
```

## <a name="telemetry-schema"></a>Telemetrie-schema

De hoofdtekst van de *telemetrie* bericht is een JSON-structuur die clientinformatie over een inschakelen of een poging verbinding bevat. De structuur is opgebouwd uit client tijdstempels die record wanneer clientgebeurtenissen plaatsvinden. Elke tijdstempel moet zich in de ISO 8601-notatie zoals beschreven in het gedeelte "-header X-Timestamp." *Telemetrie* berichten die niet de vereiste velden in de JSON-structuur opgeeft of gebruik die niet de juiste tijdstempel indeling kunnen ertoe leiden dat de service te beëindigen van de verbinding met de client. Clients *moet* geldige waarden voor alle vereiste velden opgeven. Clients *moet* voor optionele velden wanneer de juiste waarden opgeven. De waarden in de voorbeelden in deze sectie zijn uitsluitend ter illustratie.

Telemetrie-schema is onderverdeeld in de volgende onderdelen: ontvangen bericht tijdstempels en metrische gegevens. De indeling en het gebruik van elk deel is opgegeven in de volgende secties.

### <a name="received-message-time-stamps"></a>Tijdstempels van ontvangen bericht

Clients moeten tijd van ontvangst waarden voor alle berichten die zij krijgen na correct verbinding maakt met de service zijn. Deze waarden moeten de tijd registreren wanneer de client *ontvangen* elk bericht vanaf het netwerk. De waarde moet een ander tijdstip geen vastgelegd. De client moet bijvoorbeeld niet registreren wanneer wanneer deze *gehandeld* voor het bericht. Het ontvangen bericht tijdstempels zijn opgegeven in een matrix van *name-value* paren. De naam van het paar bevat de *pad* waarde van het bericht. De waarde van het paar geeft de tijd van de client wanneer het bericht is ontvangen. Of als meer dan een bericht met de opgegeven naam is ontvangen, is de waarde van de combinatie van een matrix met tijdstempels die aangeeft wanneer deze berichten werden ontvangen.

```JSON
  "ReceivedMessages": [
    { "speech.hypothesis": [ "2016-08-16T15:03:48.172Z", "2016-08-16T15:03:48.331Z", "2016-08-16T15:03:48.881Z" ] },
    { "speech.endDetected": "2016-08-16T15:03:49.721Z" },
    { "speech.phrase": "2016-08-16T15:03:50.001Z" },
    { "turn.end": "2016-08-16T15:03:51.021Z" }
  ]
```

Clients *moet* bevestigt de ontvangst van alle berichten die door de service is verzonden door tijdstempels voor die berichten in de JSON-hoofdtekst. Als een client, mislukt de ontvangst van een bericht te bevestigen, kan de service kan de verbinding verbreken.

### <a name="metrics"></a>Metrische gegevens

Clients moeten bevatten informatie over gebeurtenissen die is opgetreden tijdens de levensduur van een aanvraag. De volgende metrische gegevens worden ondersteund: `Connection`, `Microphone`, en `ListeningTrigger`.

### <a name="metric-connection"></a>Metrische gegevens `Connection`

De `Connection` metriek geeft details over verbindingspogingen door de client. De meetwaarde moet tijdstempels bevatten wanneer de WebSocket-verbinding is gestart en is voltooid. De `Connection` metriek is vereist *alleen voor de eerste inschakelen van een verbinding*. Hiermee schakelt u latere zijn niet vereist voor deze informatie opnemen. Als een client meerdere verbindingspogingen doet voordat een verbinding tot stand is gebracht, informatie over *alle* de verbindingspogingen moeten worden opgenomen. Zie voor meer informatie [verbinding mislukt telemetrie](#connection-failure-telemetry).

| Veld | Beschrijving | Gebruik |
| ----- | ----------- | ----- |
| Naam | `Connection` | Vereist |
| Id | De verbinding id-waarde die is gebruikt in de *X ConnectionId* -header voor deze aanvraag | Vereist |
| Starten | Het tijdstip waarop de client de verbindingsaanvraag verzonden | Vereist |
| Einde | De tijd wanneer de client een melding dat de verbinding tot stand is gebracht is ontvangen of, in foutgevallen, afgewezen, geweigerd of is mislukt | Vereist |
| Fout | Een beschrijving van de volgende fout is opgetreden, indien van toepassing. Als de verbinding voltooid is, moeten clients laat dit veld weg. De maximumlengte van dit veld is 50 tekens. | Vereist voor foutgevallen, anders wordt weggelaten |

De foutbeschrijving moet maximaal 50 tekens lang zijn en in het ideale geval moet een van de waarden die worden vermeld in de volgende tabel. Als de foutvoorwaarde komt niet overeen met een van deze waarden, clients een beknopte beschrijving van de fout kunnen gebruiken met behulp van [CamelCasing](https://en.wikipedia.org/wiki/Camel_case) zonder spaties. De mogelijkheid om te verzenden van een *telemetrie* bericht moet een verbinding met de service, zodat alleen tijdelijke of tijdelijke fouten worden gerapporteerd de *telemetrie* bericht. Fout voorwaarden die *permanent* blok een client tot stand brengen van een verbinding met de service te voorkomen dat de client bericht verzenden naar de service, inclusief *telemetrie* berichten.

| Fout | Gebruik |
| ----- | ----- |
| DNSfailure | De client kon geen verbinding maken met de service vanwege een fout in de netwerkstack van DNS. |
| NoNetwork | Poging een verbinding van de client, maar de netwerkstack meldt dat er geen fysieke netwerk beschikbaar is. |
| NoAuthorization | Verbinding met de client is mislukt tijdens het verkrijgen van een verificatietoken voor de verbinding. |
| NoResources | De client heeft te weinig een lokale bron (bijvoorbeeld geheugen) tijdens een poging om een verbinding te maken. |
| Verboden | De client kon geen verbinding maken met de service omdat de service heeft een HTTP geretourneerd `403 Forbidden` op de upgradeaanvraag van WebSocket-statuscode geregistreerd. |
| Niet geautoriseerd | De client kon geen verbinding maken met de service omdat de service heeft een HTTP geretourneerd `401 Unauthorized` op de upgradeaanvraag van WebSocket-statuscode geregistreerd. |
| BadRequest | De client kon geen verbinding maken met de service omdat de service heeft een HTTP geretourneerd `400 Bad Request` op de upgradeaanvraag van WebSocket-statuscode geregistreerd. |
| ServerUnavailable | De client kon geen verbinding maken met de service omdat de service heeft een HTTP geretourneerd `503 Server Unavailable` op de upgradeaanvraag van WebSocket-statuscode geregistreerd. |
| ServerError | De client kan geen verbinding maken met de service, omdat de service is geretourneerd is een `HTTP 500` statuscode interne fout op de upgradeaanvraag van WebSocket. |
| Time-out | De client-verbindingsaanvraag zonder reactie van de service is een time-out. De *End* veld bevat de tijd wanneer de client is een time-out en wachten op de verbinding is gestopt. |
| ClientError | De client verbroken de verbinding vanwege een interne client-fout. | 

### <a name="metric-microphone"></a>Metrische gegevens `Microphone`

De `Microphone` metriek is vereist voor alle spraak schakelt. Met deze metriek wordt de tijd op de client gedurende welke audio-invoer actief wordt gebruikt voor een aanvraag spraak.

De volgende voorbeelden gebruiken als richtlijnen voor de opname *Start* tijdwaarden voor de `Microphone` metrische in uw clienttoepassing:

* Een clienttoepassing vereist dat een gebruiker moet op een fysieke knop starten van de microfoon drukt. Na de knop de clienttoepassing leest de invoer van de microfoon en verzendt het naar-spraak-Service. De *Start* waarde voor de `Microphone` metriek legt de tijd nadat de knop push wanneer de microfoon is geïnitialiseerd en is gereed om te bieden invoer. De *End* waarde voor de `Microphone` metriek legt de tijd waarop de clienttoepassing streaming geluid om de service gestopt nadat deze ontvangen het `speech.endDetected` bericht van de service.

* Een clienttoepassing maakt gebruik van een sleutelwoord spotter die 'altijd' luistert. Pas nadat het sleutelwoord spotter een wachtwoordzin gesproken trigger detecteert de clienttoepassing de invoer van de microfoon verzamelen en verzenden naar-spraak-Service. De *Start* waarde voor de `Microphone` metriek legt de tijd waarop het sleutelwoord spotter melding voor de client aan de slag met invoer van de microfoon. De *End* waarde voor de `Microphone` metriek legt de tijd waarop de clienttoepassing streaming geluid om de service gestopt nadat deze ontvangen het `speech.endDetected` bericht van de service.

* Een clienttoepassing toegang heeft tot een constante audiostroom en voert stilte/spraak detectie op die audiostroom in een *spraak detectiemodule*. De *Start* waarde voor de `Microphone` metriek legt de tijd wanneer de *spraak detectiemodule* melding van de client aan de slag met invoer van de audiostroom. De *End* waarde voor de `Microphone` metriek legt de tijd waarop de clienttoepassing streaming geluid om de service gestopt nadat deze ontvangen het `speech.endDetected` bericht van de service.

* Een clienttoepassing wordt verwerkt door de tweede inschakelen van een aanvraag met meerdere inschakelen en kennis wordt gesteld door een service-antwoordbericht inschakelen van de microfoon voor het verzamelen van invoer voor de tweede inschakelen. De *Start* waarde voor de `Microphone` metriek legt de tijd wanneer de clienttoepassing kan de microfoon en wordt gestart met behulp van de invoer van die bron audio. De *End* waarde voor de `Microphone` metriek legt de tijd waarop de clienttoepassing streaming geluid om de service gestopt nadat deze ontvangen het `speech.endDetected` bericht van de service.

De *End* tijd-waarde voor de `Microphone` metriek legt de tijd waarop de clienttoepassing gestopt streaming audio-invoer. In de meeste situaties deze gebeurtenis treedt op kort nadat de client ontvangen de `speech.endDetected` bericht van de service. Clienttoepassingen kunnen controleren of ze correct die aan het protocol voldoen zijn door ervoor te zorgen dat de *End* tijd-waarde voor de `Microphone` metriek treedt later dan de waarde voor ontvangst op de `speech.endDetected` bericht. En omdat er meestal een vertraging tussen het einde van een inschakelen en het begin van een andere inschakelen, clients protocol overeenstemming kunnen controleren door ervoor te zorgen dat de *Start* tijd van de `Microphone` metric voor alle daaropvolgende Schakel correct legt de tijd wanneer de client *gestart* met behulp van de microfoon stroom audio-invoer met de service.

| Veld | Beschrijving | Gebruik |
| ----- | ----------- | ----- |
| Naam | Microfoon | Vereist |
| Starten | De tijd wanneer de client met audio-invoer van de microfoon of andere audiostroom gestart of ontvangen van een trigger van het sleutelwoord spotter | Vereist |
| Einde | De tijd wanneer de client is gestopt met behulp van de microfoon of audio-stroom | Vereist |
| Fout | Een beschrijving van de volgende fout is opgetreden, indien van toepassing. Als de microfoon-bewerkingen met succes zijn uitgevoerd, moeten clients dit veld weglaten. De maximumlengte van dit veld is 50 tekens. | Vereist voor foutgevallen, anders wordt weggelaten |

### <a name="metric-listeningtrigger"></a>Metrische gegevens `ListeningTrigger`
De `ListeningTrigger` metriek meet de tijd wanneer de gebruiker voert de actie die spraakinvoer start. De `ListeningTrigger` metriek is optioneel, maar de clients die deze metrische gegevens kunnen opgeven, wordt aangeraden om dit te doen.

De volgende voorbeelden gebruiken als richtlijnen voor de opname *Start* en *End* tijdwaarden voor de `ListeningTrigger` metrische in uw clienttoepassing.

* Een clienttoepassing vereist dat een gebruiker moet op een fysieke knop starten van de microfoon drukt. De *Start* waarde met deze metriek legt de tijd van de knop push. De *End* waarde legt de tijd wanneer de knop push-bewerking is voltooid.

* Een clienttoepassing maakt gebruik van een sleutelwoord spotter die 'altijd' luistert. Na het sleutelwoord spotter detecteert een wachtwoordzin gesproken trigger de clienttoepassing leest de invoer van de microfoon en verzendt het naar-spraak-Service. De *Start* waarde met deze metriek legt de tijd waarop het sleutelwoord spotter audio die vervolgens gedetecteerd als de trigger woordgroep ontvangen. De *End* waarde legt de tijd wanneer het laatste woord van de trigger woordgroep is gesproken door de gebruiker.

* Een clienttoepassing toegang heeft tot een constante audiostroom en voert stilte/spraak detectie op die audiostroom in een *spraak detectiemodule*. De *Start* waarde met deze metriek legt de tijd die de *spraak detectiemodule* ontvangen audio die vervolgens als spraak is gedetecteerd. De *End* waarde legt de tijd wanneer de *spraak detectiemodule* spraak gedetecteerd.

* Een clienttoepassing wordt verwerkt door de tweede inschakelen van een aanvraag met meerdere inschakelen en kennis wordt gesteld door een service-antwoordbericht inschakelen van de microfoon voor het verzamelen van invoer voor de tweede inschakelen. De clienttoepassing moet *niet* bevatten een `ListeningTrigger` metric voor deze inschakelen.

| Veld | Beschrijving | Gebruik |
| ----- | ----------- | ----- |
| Naam | ListeningTrigger | Optioneel |
| Starten | Het tijdstip waarop de luisterende client-trigger worden gestart | Vereist |
| Einde | Het tijdstip waarop de luisterende client-trigger is voltooid | Vereist |
| Fout | Een beschrijving van de volgende fout is opgetreden, indien van toepassing. Als de trigger-bewerking voltooid is, moeten clients laat dit veld weg. De maximumlengte van dit veld is 50 tekens. | Vereist voor foutgevallen, anders wordt weggelaten |

#### <a name="sample-message"></a>Voorbeeldbericht

Het volgende voorbeeld toont een bericht telemetrie met ReceivedMessages en de metrische gegevens delen:

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

Deze sectie beschrijft de soorten foutberichten en voorwaarden die uw toepassing moet om af te handelen.

### <a name="http-status-codes"></a>HTTP-statuscodes

Tijdens de upgradeaanvraag van WebSocket spraak-Service retourneert mogelijk een van de standaard HTTP-statuscodes, zoals `400 Bad Request`, enzovoort. Uw toepassing moet de foutvoorwaarden van deze correct verwerken.

#### <a name="authorization-errors"></a>Verificatiefouten

Als autorisatie onjuist is opgegeven tijdens de upgrade WebSocket spraak Service een HTTP retourneert `403 Forbidden` statuscode. Zijn tussen de condities die deze foutcode kunnen activeren:

* Ontbrekende *autorisatie* koptekst

* Ongeldige verificatietoken

* Verlopen verificatietoken

De `403 Forbidden` foutbericht over een probleem met spraak-Service wordt niet aangegeven. Dit foutbericht geeft een probleem met de clienttoepassing.

### <a name="protocol-violation-errors"></a>Protocolfouten schending

Als spraak Service eventuele schendingen van het protocol van een client detecteert, de service wordt beëindigd door de WebSocket-verbinding na het retourneren van een *statuscode* en *reden* voor de beëindiging. Client-toepassingen kunnen u deze informatie gebruiken om problemen op te lossen schendingen van het.

#### <a name="incorrect-message-format"></a>Onjuiste berichtindeling

Als een client verzendt een tekst of binaire bericht naar de service die is niet gecodeerd in de juiste indeling gegeven in deze specificatie, de service, sluit u de verbinding met een *1007 Ongeldige nettolading gegevens* statuscode. 

De service retourneert deze statuscode voor een aantal redenen, zoals wordt weergegeven in de volgende voorbeelden:

* 'Onjuist berichtindeling. Binaire bericht heeft ongeldige header-Groottevoorvoegsel." De client een binaire bericht verzonden dat het voorvoegsel van een ongeldige header-grootte heeft.

* 'Onjuist berichtindeling. Binaire bericht heeft ongeldige header-grootte." De client een binaire bericht verzonden dat een ongeldige header-grootte opgegeven.

* 'Onjuist berichtindeling. Binaire berichtkoppen decodering in UTF-8 is mislukt." De client een binaire bericht verzonden dat headers die zijn niet correct gecodeerd in UTF-8 bevat.

* 'Onjuist berichtindeling. SMS-bericht bevat geen gegevens." De client verzonden een SMS-bericht dat geen hoofdtekst gegevens bevat.

* 'Onjuist berichtindeling. SMS-bericht in UTF-8 decoderen is mislukt." De client verzonden een SMS-bericht is niet correct gecodeerd in UTF-8.

* 'Onjuist berichtindeling. SMS-bericht bevat geen scheidingsteken header." De client verzonden een SMS-bericht dat bevat geen scheidingsteken koptekst of het scheidingsteken verkeerde kop wordt gebruikt.

#### <a name="missing-or-empty-headers"></a>Headers ontbreekt of is leeg

Als een client verzendt een bericht dat de vereiste headers geen *X RequestId* of *pad*, de service wordt gesloten voor de verbinding met een *1002 protocolfout* statuscode. Het bericht is 'header ontbreekt/leeg. {Header name}'.

#### <a name="requestid-values"></a>Aanvraag-id-waarden

Als een client een bericht verzendt dat Hiermee geeft u een *X RequestId* koptekst met een onjuiste indeling, de service wordt de verbinding wordt gesloten en retourneert een *1002 protocolfout* status. Het bericht wordt 'Ongeldige aanvraag. Waarde van de header X-aanvraag-id is niet opgegeven in Nee-dash-UUID-opmaak."

#### <a name="audio-encoding-errors"></a>Audio-codering fouten

Als een client verzendt een audio-segment waarnaar een inschakelen en de audio-indeling initieert of codering niet aan de vereiste specificatie voldoet, de service wordt de verbinding wordt gesloten en retourneert een *1007 Ongeldige nettolading gegevens* statuscode. Het bericht geeft aan dat de indeling Foutbron codering.

#### <a name="requestid-reuse"></a>Aanvraag-id opnieuw gebruiken

Nadat een inschakelen is voltooid, als een client verzendt een bericht waarin wordt gebruikgemaakt van de aanvraag-id van die inschakelen, de service wordt de verbinding wordt gesloten en retourneert een *1002 protocolfout* statuscode. Het bericht wordt 'Ongeldige aanvraag. Hergebruik van aanvraag-id is niet toegestaan.'

## <a name="connection-failure-telemetry"></a>Verbinding mislukt telemetrie

De best mogelijke gebruikerservaring, zodat clients verwittigt spraak-Service van de tijdstempels voor belangrijke controlepunten binnen een verbinding met behulp van de *telemetrie* bericht. Is het belangrijk dat clients informeren over de service voor verbindingen die heeft geprobeerd zijn, maar is mislukt.

Bij elke verbinding die niet zijn geslaagd, clients maken een *telemetrie* bericht met een unieke *X RequestId* headerwaarde. Omdat de client kon geen verbinding de *ReceivedMessages* veld in de JSON-hoofdtekst kan worden weggelaten. Alleen de `Connection` vermelding in de *metrische gegevens* veld is opgenomen. Deze vermelding bestaat uit het begin en einde tijdstempels, evenals de fout die is opgetreden.

### <a name="connection-retries-in-telemetry"></a>Verbindingspogingen in telemetrie

Clients moeten worden onderscheiden *pogingen* van *meerdere verbindingspogingen* door de gebeurtenis die de verbindingspoging activeert. Verbindingspogingen die programmatisch zonder invoer van de gebruiker worden uitgevoerd, zijn nieuwe pogingen. Meerdere verbindingspogingen die worden uitgevoerd in reactie op invoer van de gebruiker zijn meerdere verbindingspogingen. Clients geven elke gebruiker geactiveerd verbindingspoging een unieke *X RequestId* en *telemetrie* bericht. Clients gebruiken de *X RequestId* voor programmatische nieuwe pogingen. Als er meerdere pogingen voor een enkele verbindingspoging zijn aangebracht, elke nieuwe poging is opgenomen als een `Connection` vermelding in de *telemetrie* bericht.

Stel bijvoorbeeld dat een gebruiker spreekt de trigger trefwoord om een verbinding wordt gestart en de eerste verbindingspoging is mislukt vanwege fouten in DNS. Echter, lukt de een tweede poging die door de client via een programma is gemaakt. Omdat de client opnieuw geprobeerd de verbinding zonder aanvullende invoer van de gebruiker, gebruikt de client een enkel *telemetrie* bericht met meerdere `Connection` vermeldingen om de verbinding te beschrijven.

Als een ander voorbeeld: Stel dat een gebruiker spreekt de trigger sleutelwoord voor het starten van een verbinding mislukt deze verbindingspoging na drie pogingen. De client geeft, probeert verbinding maken met de service wordt gestopt, en wordt de gebruiker geïnformeerd dat is iets verkeerd gegaan. De trigger trefwoord wordt in de gebruiker opnieuw spreekt. Deze tijd, Stel dat de client verbinding maakt met de service. Nadat u verbinding maakt, de client onmiddellijk een *telemetrie* bericht aan de service dat drie bevat `Connection` vermeldingen die het verbindingsfouten beschrijven. Na ontvangst de `turn.end` bericht verzendt de client een andere *telemetrie* bericht met een beschrijving van de verbinding is geslaagd.

## <a name="error-message-reference"></a>Naslaginformatie over foutberichten

### <a name="http-status-codes"></a>HTTP-statuscodes

| HTTP-statuscode | Beschrijving | Problemen oplossen |
| - | - | - |
| 400 onjuiste aanvraag | De client verzonden een WebSocket-verbindingsaanvraag onjuist is. | Controleer dat u alle vereiste parameters en HTTP-headers hebt opgegeven en of de waarden correct zijn. |
| 401-niet toegestaan | De client hebt niet de vereiste verificatie-informatie opgenomen. | Controleer of u verzendt de *autorisatie* -header in de WebSocket-verbinding. |
| 403-verboden | De client verzonden autorisatie-informatie, maar deze is ongeldig. | Controleer dat u niet de waarde van een verlopen of ongeldig verzendt de *autorisatie* header. |
| 404 – Niet gevonden | De client toegang probeert te krijgen van een URL-pad wordt niet ondersteund. | Controleer dat u de juiste URL voor de WebSocket-verbinding gebruikt. |
| 500 Serverfout | De service is een interne fout opgetreden en kan niet voldoen aan de aanvraag. | Deze fout wordt in de meeste gevallen tijdelijke. De aanvraag opnieuw proberen. |
| 503 Service niet beschikbaar | De service is niet beschikbaar om de aanvraag te verwerken. | Deze fout wordt in de meeste gevallen tijdelijke. De aanvraag opnieuw proberen. |

### <a name="websocket-error-codes"></a>WebSocket-foutcodes

| WebSocketsStatus code | Beschrijving | Problemen oplossen |
| - | - | - |
| 1000 normale sluiting | De service de WebSocket-verbinding zonder fouten gesloten. | Als de WebSocket-sluiting werd niet verwacht, lees opnieuw in de documentatie om ervoor te zorgen dat u begrijpt hoe en wanneer de service de WebSocket-verbinding kunt beëindigen. |
| Protocolfout 1002 | De client kan niet voldoen aan vereisten protocol. | Zorg ervoor dat u inzicht in de documentatie van het protocol en duidelijk over de vereisten zijn. Raadpleeg de vorige documentatie over fout redenen om te zien als u protocol vereisten zijn overtreden. |
| 1007 Ongeldige nettolading | De client verzonden een ongeldige nettolading in een protocolbericht. | Controleer het laatste bericht dat u hebt verzonden naar de service op fouten. Raadpleeg de vorige documentatie over de nettolading van fouten. |
| Serverfout 1011 | De service is een interne fout opgetreden en kan niet voldoen aan de aanvraag. | Deze fout wordt in de meeste gevallen tijdelijke. De aanvraag opnieuw proberen. |

## <a name="related-topics"></a>Verwante onderwerpen

Zie een [JavaScript SDK](https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript) is een implementatie van het spraak-Service op basis van WebSocket-protocol.
