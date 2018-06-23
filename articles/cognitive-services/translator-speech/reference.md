---
title: Microsoft Translator Speech-API-referentiemateriaal | Microsoft Docs
titleSuffix: Cognitive Services
description: Documentatie voor de Microsoft Translator Speech-API.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.technology: microsoft translator
ms.topic: article
ms.date: 05/18/2018
ms.author: v-jansko
ms.openlocfilehash: be8faddf56158de3399713c41638c0b913b4627e
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345827"
---
# <a name="microsoft-translator-speech-api"></a>Spraak-API van Microsoft Translator

Deze service biedt een streaming-API om transcriberen met gebruikmaking van eigen spraak van één taal in de tekst van een andere taal. De API kan ook worden geïntegreerd-naar-spraak mogelijkheden voor de vertaalde tekst terug te laten spreken. Microsoft Translator Speech-API maakt scenario's zoals realtime vertaling van conversaties zoals Skype conversieprogramma.

Met Microsoft Translator Speech-API clienttoepassingen stream spraak geluid om de service en ontvangen van een stream van de resultaten op basis van tekst, waaronder de herkende tekst in de taal van de bron en de vertaling in de doel-taal. Resultaten van de tekst worden geproduceerd door het toepassen van automatische Speech Recognition (ASR) aangedreven door diep neural netwerken voor de binnenkomende audiostroom. Onbewerkte ASR uitvoer is verder verbeterd door een nieuwe techniek TekstAlsWaar aangeroepen om aan te geven nauwkeuriger gebruiker bedoeld. Bijvoorbeeld, verwijdert TekstAlsWaar disfluencies (hmms en coughs) en de juiste leestekens terugzetten hoofdletters en kleine letters. De mogelijkheid om te maskeren of uitsluiten profanities is ook opgenomen. De opname- en NAT-engines zijn specifiek voor het afhandelen van eigen spraak getraind. De vertaling van spraak-service gebruikt Stiltedetectie om te bepalen van het einde van een utterance. Na een onderbreking in de stem activiteit, wordt de service stream terug uiteindelijke resultaat van de voltooide utterance. De service kunt terug gedeeltelijke resultaten, die tussenliggende erkenning en vertalingen voor een utterance in voortgang geeft ook verzenden. De definitieve resultaten voor biedt de service de mogelijkheid om na te bootsen speech (spraak) uit de gesproken tekst in de doel-talen. Audio-naar-spraak wordt gemaakt in de indeling die is opgegeven door de client. WAV en MP3 indelingen zijn beschikbaar.

Microsoft Translator Speech-API maakt gebruik van het WebSocket-protocol om op te geven van een kanaal full-duplex-communicatie tussen de client en de server. Een toepassing moet deze stappen om de service te gebruiken:

## <a name="1-getting-started"></a>1. Aan de slag
Voor toegang tot de Microsoft Translator tekst-API wordt gebruikt, u moet [aanmelden voor Microsoft Azure](translator-speech-how-to-signup.md).

## <a name="2-authentication"></a>2. Authenticatie

De abonnement-toets gebruiken om te verifiëren. Microsoft Translator Speech-API ondersteunt twee modi van verificatie:

* **Met behulp van een toegangstoken:** verkrijgen In uw toepassing een toegangstoken van de service voor beveiligingstokens. Gebruik uw abonnementssleutel Microsoft Translator Speech-API een toegangstoken ophalen uit de verificatieservice cognitieve Services. Het toegangstoken is geldig voor 10 minuten. Vraag een nieuw toegangstoken elke 10 minuten en blijven gebruiken dezelfde toegang token voor herhaalde aanvragen binnen deze 10 minuten.

* **Met behulp van een abonnementssleutel rechtstreeks:** In uw toepassing, geeft u de abonnementssleutel van uw als een waarde in `Ocp-Apim-Subscription-Key` header.

De abonnementssleutel van uw en het toegangstoken behandelen als geheimen die moeten worden verborgen.

## <a name="3-query-languages"></a>3. Querytalen
**Query uitvoeren op de bron van de talen voor de huidige set van ondersteunde talen.** De [talen resource](languages-reference.md) beschrijft de set met talen en stemmen beschikbaar voor spraakherkenning, de tekstvertaling van en spraak. Elke taal of gesproken krijgt een id die Microsoft Translator Speech-API wordt gebruikt om de dezelfde taal of stem te identificeren.

## <a name="4-stream-audio"></a>4. Stroom audio
**Een verbinding openen en beginnen streaming geluid om de service.** De URL van de service is `wss://dev.microsofttranslator.com/speech/translate`. Parameters en audio-indelingen werd verwacht door de service worden hieronder beschreven, in de `/speech/translate` bewerking. Een van de parameters wordt gebruikt voor het doorsturen van het toegangstoken van stap 2 hierboven.

## <a name="5-process-the-results"></a>5. De resultaten verwerken
**De resultaten weer van de service gestreamd verwerken.** De indeling van gedeeltelijke resultaten, eindresultaat en -naar-spraak audio segmenten worden beschreven in de documentatie van de `/speech/translate` bewerking hieronder.

Gebruik van Microsoft Translator Speech-API aan te tonen codevoorbeelden zijn beschikbaar via de [Microsoft Translator Github site](https://github.com/MicrosoftTranslator).

## <a name="implementation-notes"></a>Opmerkingen bij de implementatie

/Speech/translate Establishes een sessie voor spraak vertaling ophalen

### <a name="connecting"></a>Verbinding maken
Controleer voordat u verbinding maakt met de service, de lijst met parameters die later in deze sectie worden gegeven. Er is een voorbeeld van aanvraag:

`GET wss://dev.microsofttranslator.com/speech/translate?from=en-US&to=it-IT&features=texttospeech&voice=it-IT-Elsa&api-version=1.0`
`Ocp-Apim-Subscription-Key: {subscription key}`
`X-ClientTraceId: {GUID}`

De aanvraag bevat dat gesproken Engels worden gestreamd naar de service en Italiaans vertaald. Elk resultaat definitieve erkenning genereert een-naar-spraak audio antwoord met de vrouwelijke stem Elsa met de naam. U ziet dat de aanvraag bevat de referenties in de `Ocp-Apim-Subscription-Key header`. De aanvraag volgt ook aanbevolen door in te stellen van een globally unique identifier in de header `X-ClientTraceId`. Een clienttoepassing zich moet aanmelden traceer-ID zodat deze kan worden gebruikt voor het oplossen van problemen wanneer deze optreden.

### <a name="sending-audio"></a>Verzenden van audio
Wanneer de verbinding is gemaakt, begint de client streaming geluid om de service. De client stuurt audio in stukken verdeeld. Elk segment wordt verzonden met een Websocket-bericht van het type Binary.

Audio-invoer is in de Wave Audio-indeling (WAVE of meer bekend als WAV vanwege de bestandsnaamextensie). De clienttoepassing moet één kanaal, 16-bits ondertekende PCM-audio op 16 kHz door actieve streamen. De eerste reeks bytes gestreamd door de client bevat de WAV-header. Een header 44 bytes is voor één kanaal ondertekend 16 bits PCM stream op 16 kHz door actieve:

|Offset|Waarde|
|:---|:---|
|0 - 3|'RIFF'|
|4 - 7|0|
|8 - 11|'WAVE'|
|12 - 15|'fmt'|
|16 - 19|16|
|20 - 21|1|
|22 - 23|1|
|24 - 27|16000|
|28 - 31|32000|
|32 - 33|2|
|34 - 35|16|
|36 - 39|"gegevens"|
|40 - 43|0|

U ziet dat de totale bestandsgrootte (in bytes 4-7) en de grootte van de "gegevens" (in bytes 40 43) zijn ingesteld op nul. Dit is geen probleem voor het streaming-scenario waarin de totale grootte is niet noodzakelijkerwijs bekend tevoren betaalt.

Na het verzenden van de header WAV (RIFF), verzendt de client gegevenssegmenten van de audio. De client wordt doorgaans stream vaste grootte segmenten die een vaste duur (bijvoorbeeld stream 100ms van audio tegelijk) vertegenwoordigt.

### <a name="final-result"></a>Eindresultaat
Een definitieve speech recognition resultaat wordt aan het einde van een utterance gegenereerd. Een resultaat is van de service verzonden naar de client met een WebSocket-bericht van het type Text. Inhoud van het bericht is de JSON-serialisatie van een object met de volgende eigenschappen:

* `type`: Tekenreeksconstante voor het identificeren van het type resultaat van. De waarde is definitief voor de laatste resultaten.
* `id`: De id die is toegewezen aan het resultaat erkenning tekenreeks.
* `recognition`: Herkende tekst in de taal van de bron. De tekst is mogelijk een lege tekenreeks in het geval van een false herkenning.
* `translation`: Herkende tekst wordt vertaald in de doel-taal.
* `audioTimeOffset`: Tijd verschuiving van het begin van de opname in ticks (1 maatstreepjes = 100 nanoseconden). De offset is ten opzichte van het begin van streaming.
* `audioTimeSize`: De duur in ticks (100 nanoseconden) van de erkenning.
* `audioStreamPosition`: Byte-offset van het starten van de erkenning. De offset is ten opzichte van het begin van de stroom.
* `audioSizeBytes`: De grootte in bytes van de erkenning.

Houd er rekening mee dat plaatsing van de opname in de audio-stroom niet standaard in de resultaten opgenomen is. De `TimingInfo` functie moet worden geselecteerd door de client (Zie `features` parameter).

Het uiteindelijke resultaat van een voorbeeld is als volgt:

```
{
  type: "final"
  id: "23",
  recognition: "what was said", 
  translation: "translation of what was said",
  audioStreamPosition: 319680,
  audioSizeBytes: 35840,
  audioTimeOffset: 2731600000,
  audioTimeSize: 21900000
}
```

### <a name="partial-result"></a>Gedeeltelijke resultaten
Gedeeltelijke of tussenliggende spraak herkenningsresultaten worden niet gestreamd naar de client standaard. De client kan de queryparameter functies gebruiken om aan te vragen deze.

Een gedeeltelijke resultaat is van de service verzonden naar de client met een WebSocket-bericht van het type Text. Inhoud van het bericht is de JSON-serialisatie van een object met de volgende eigenschappen:

* `type`: Tekenreeksconstante voor het identificeren van het type resultaat van. De waarde is gedeeltelijk voor gedeeltelijke resultaten.
* `id`: De id die is toegewezen aan het resultaat erkenning tekenreeks.
* `recognition`: Herkende tekst in de taal van de bron.
* `translation`: Herkende tekst wordt vertaald in de doel-taal.
* `audioTimeOffset`: Tijd verschuiving van het begin van de opname in ticks (1 maatstreepjes = 100 nanoseconden). De offset is ten opzichte van het begin van streaming.
* `audioTimeSize`: De duur in ticks (100 nanoseconden) van de erkenning.
* `audioStreamPosition`: Byte-offset van het starten van de erkenning. De offset is ten opzichte van het begin van de stroom.
* `audioSizeBytes`: De grootte in bytes van de erkenning.

Houd er rekening mee dat plaatsing van de opname in de audio-stroom niet standaard in de resultaten opgenomen is. De functie TimingInfo moet worden geselecteerd door de client (Zie functies parameter).

Het uiteindelijke resultaat van een voorbeeld is als volgt:

```
{
  type: "partial"
  id: "23.2",
  recognition: "what was", 
  translation: "translation of what was",
  audioStreamPosition: 319680,
  audioSizeBytes: 25840,
  audioTimeOffset: 2731600000,
  audioTimeSize: 11900000
}
```

### <a name="text-to-speech"></a>Spraak
Wanneer de tekst-naar-functie is ingeschakeld (Zie `features` parameter hieronder), een definitieve resultaat wordt gevolgd door de audio van de gesproken vertaalde tekst. Audiogegevens is chunked en van de service verzonden naar de client als een reeks Websocket-berichten van het type Binary. Een client kan het einde van de stroom detecteren door het controleren van de bit FIN van elk bericht. Het laatste binaire bericht heeft de FIN bit is ingesteld op een aan het einde van de stroom. De indeling van de stroom is afhankelijk van de waarde van de `format` parameter.

### <a name="closing-the-connection"></a>De verbinding wordt gesloten
Wanneer een clienttoepassing streaming audio is voltooid en het laatste eindresultaat heeft ontvangen, moet deze de verbinding verbreken door de WebSocket sluiten handshake initiëren. Er zijn voorwaarden waardoor de server de verbinding is beëindigd. De volgende codes voor WebSocket gesloten kunnen worden ontvangen door de client:

* `1003 - Invalid Message Type`: De server kan de verbinding wordt beëindigd omdat dit niet het gegevenstype dat is ontvangen accepteren. Dit gebeurt meestal wanneer binnenkomende audio niet met een juiste header begint.
* `1000 - Normal closure`: De verbinding is gesloten nadat de aanvraag is voldaan. De server verbreekt de verbinding: wanneer geen audio wordt ontvangen van de client gedurende langere tijd; Wanneer stilte gestreamd gedurende langere tijd; Wanneer een sessie bereikt voor de maximale duur toegestaan (ongeveer 90 minuten).
* `1001 - Endpoint Unavailable`: Dit geeft aan dat de server is niet beschikbaar. Clienttoepassing kan proberen opnieuw verbinding maken met een limiet van het aantal nieuwe pogingen.
* `1011 - Internal Server Error`: De verbinding wordt gesloten door de server vanwege een fout op de server.

### <a name="parameters"></a>Parameters

|Parameter|Waarde|Beschrijving|Parametertype|Gegevenstype|
|:---|:---|:---|:---|:---|
|API-versie|1.0|De versie van de API die door de client wordt aangevraagd. Toegestane waarden zijn: `1.0`.|query   |tekenreeks|
|uit|(leeg)   |Hiermee geeft u de taal van de binnenkomende spraak. De waarde is een van de taal-id van de `speech` bereik in het antwoord van de API talen.|query|tekenreeks|
|tot|(leeg)|Hiermee geeft u de taal voor het vertalen van de transcribed tekst in. De waarde is een van de taal-id van de `text` bereik in het antwoord van de API talen.|query|tekenreeks|
|database|(leeg)   |Door komma's gescheiden reeks functies geselecteerd door de client. De beschikbare functies zijn:<ul><li>`TextToSpeech`: Hiermee geeft u aan dat de service de vertaalde audio van de laatste vertaalde zin moet retourneren.</li><li>`Partial`: Hiermee geeft u aan dat de service een tussenliggende herkenningsresultaten moet worden geretourneerd terwijl de audio is streaming aan de service.</li><li>`TimingInfo`: Hiermee geeft u aan dat de service timinginformatie die is gekoppeld aan elke herkenning moet retourneren.</li></ul>Als u bijvoorbeeld een client geeft `features=partial,texttospeech` voor het ontvangen van gedeeltelijke resultaten en spraak, maar er is geen timinginformatie. Houd er rekening mee dat de definitieve resultaten altijd worden gestreamd naar de client.|query|tekenreeks|
|Voice-over|(leeg)|Identificeert welke voice moet worden gebruikt voor tekst-naar-rendering van de vertaalde tekst. De waarde is een van de voice-id uit het bereik tts in het antwoord van de API talen. Als een stem is niet opgegeven dat het systeem wordt automatisch een kiezen wanneer de tekst-naar-functie is ingeschakeld.|query|tekenreeks|
|Indeling|(leeg)|Hiermee geeft u de indeling van de-naar-spraak audiostroom geretourneerd door de service. Beschikbare opties zijn:<ul><li>`audio/wav`: Wave audiostroom. Client moet de header WAV gebruiken voor het interpreteren van de audio-indeling. WAV audio voor TTS-is 16-bits, één kanaal PCM met een samplefrequentie van 24 of 16kHz.</li><li>`audio/mp3`: MP3 audiostroom.</li></ul>De standaardwaarde is `audio/wav`.|query|tekenreeks|
|ProfanityAction    |(leeg)    |Hiermee geeft u op hoe de service profanities herkend in de spraak moet verwerken. Geldige acties zijn:<ul><li>`NoAction`: Profanities zijn ongewijzigd worden gelaten.</li><li>`Marked`: Profanities worden vervangen door een markering. Zie `ProfanityMarker` parameter.</li><li>`Deleted`: Profanities worden verwijderd. Bijvoorbeeld, als het woord `"jackass"` wordt behandeld als een taalgebruik, de woordgroep `"He is a jackass."` worden `"He is a .".`</li></ul>De standaardwaarde is gemarkeerd.|query|tekenreeks|
|ProfanityMarker|(leeg)    |Hiermee geeft u op hoe gedetecteerde profanities worden verwerkt wanneer `ProfanityAction` is ingesteld op `Marked`. Geldige opties zijn:<ul><li>`Asterisk`: Profanities zijn vervangen door de tekenreeks `***`. Bijvoorbeeld, als het woord `"jackass"` wordt behandeld als een taalgebruik, de woordgroep `"He is a jackass."` worden `"He is a ***.".`</li><li>`Tag`: Taalgebruik worden omgeven door een taalgebruik XML-code. Bijvoorbeeld, als het woord `"jackass"` wordt behandeld als een taalgebruik, de woordgroep `"He is a jackass."` worden `"He is a <profanity>jackass</profanity>."`.</li></ul>De standaardwaarde is `Asterisk`.|query|tekenreeks|
|Autorisatie|(leeg)  |Hiermee geeft u de waarde van de client bearer-token. Gebruik het voorvoegsel `Bearer` gevolgd door de waarde van de `access_token` waarde die door de verificatieservice token geretourneerd.|koptekst   |tekenreeks|
|OCP-Apim-Subscription-Key|(leeg)|Vereist als de `Authorization` -header is niet opgegeven.|koptekst|tekenreeks|
|access_token|(leeg)   |Andere manier om door te geven van een geldige OAuth-toegangstoken. De bearer-token is meestal voorzien van header `Authorization`. Sommige websocket-bibliotheken toegestaan clientcode instellen headers niet. In dit geval kan de client gebruiken de `access_token` queryparameter doorgeven van een geldig token. Bij gebruik van een toegangstoken voor de verificatie als `Authorization` header niet is ingesteld, klikt u vervolgens `access_token` moet worden ingesteld. Als de kop- en queryparameter zijn ingesteld, wordt de queryparameter genegeerd. Clients moeten één methode alleen gebruiken bij het doorgeven van het token.|query|tekenreeks|
|abonnement-sleutel|(leeg)   |Andere methode voor het abonnementssleutel doorgeven. Sommige websocket-bibliotheken toegestaan clientcode instellen headers niet. In dit geval kan de client gebruiken de `subscription-key` queryparameter doorgeven van een geldig abonnement-sleutel. Wanneer u een abonnementssleutel voor de verificatie als `Ocp-Apim-Subscription-Key` header is niet ingesteld, wordt abonnement-sleutel moet worden ingesteld. Als de kop- en queryparameter zijn ingesteld, wordt de queryparameter genegeerd. Clients moeten één methode alleen gebruiken om door te geven de `subscription key`.|query|tekenreeks|
|X-ClientTraceId    |(leeg)    |Een client gegenereerde GUID gebruikt voor het traceren van een aanvraag. Clients moeten voor juiste het oplossen van problemen, geeft u een nieuwe waarde aan elke aanvraag en meld u deze.<br/>In plaats van een koptekst, deze waarde kan worden doorgegeven met queryparameter `X-ClientTraceId`. Als de kop- en queryparameter zijn ingesteld, wordt de queryparameter genegeerd.|koptekst|tekenreeks|
|X-CorrelationId|(leeg)    |Een client gegenereerde id gebruikt voor het correleren van meerdere kanalen in een gesprek. Meerdere spraak vertaling sessies kunnen worden gemaakt om in te schakelen conversaties tussen gebruikers. In dergelijke scenario gebruiken alle spraak vertaling sessies dezelfde correlatie-ID te koppelen van de kanalen. Dit vereenvoudigt het tracering en diagnostische gegevens. De id moet voldoen aan: `^[a-zA-Z0-9-_.]{1,64}$`<br/>In plaats van een koptekst, deze waarde kan worden doorgegeven met queryparameter `X-CorrelationId`. Als de kop- en queryparameter zijn ingesteld, wordt de queryparameter genegeerd.|koptekst|tekenreeks|
|X-ClientVersion|(leeg)    |De versie van de clienttoepassing identificeert. Voorbeeld: '2.1.0.123'.<br/>In plaats van een koptekst, deze waarde kan worden doorgegeven met queryparameter `X-ClientVersion`. Als de kop- en queryparameter zijn ingesteld, wordt de queryparameter genegeerd.|koptekst|tekenreeks|
|X-OsPlatform|(leeg)   |Geeft de naam en versie van het besturingssysteem dat op de clienttoepassing wordt uitgevoerd. Voorbeelden: 'Android 5.0', 'iOs 8.1.3', ' Windows 8.1 ".<br/>In plaats van een koptekst, deze waarde kan worden doorgegeven met queryparameter `X-OsPlatform`. Als de kop- en queryparameter zijn ingesteld, wordt de queryparameter genegeerd.|koptekst|tekenreeks|

### <a name="response-messages"></a>Antwoordberichten

|HTTP-statuscode|Reden|Antwoord-Model|Headers|
|:--|:--|:--|:--|
|101    |WebSocket-upgrade.|Voorbeeldwaarde model <br/> object {}|X-RequestId<br/>Een waarde van de aanvraag voor het oplossen van problemen te identificeren.<br/>tekenreeks|
|400    |Ongeldige aanvraag. Controleer de invoerparameters zodat ze geldig zijn. Het antwoordobject bevat een gedetailleerde beschrijving van de fout.|||
|401    |Niet gemachtigd. Zorg ervoor dat referenties zijn ingesteld, of ze geldig zijn en dat uw abonnement op Azure gegevens markt goede status met een saldo beschikbaar is.|||
|500    |Er is een fout opgetreden. Als de fout zich blijft voordoen, het rapport met client trace-id (X-ClientTraceId) of aanvraag-id (X-RequestId).|||
|503    |De server is tijdelijk niet beschikbaar. Probeer de aanvraag. Als de fout zich blijft voordoen, het rapport met client trace-id (X-ClientTraceId) of aanvraag-id (X-RequestId).|||

    


    





    
    




    




    




    

            




        









