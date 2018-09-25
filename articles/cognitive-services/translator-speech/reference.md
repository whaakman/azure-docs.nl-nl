---
title: Naslaginformatie Translator Speech-API
titleSuffix: Azure Cognitive Services
description: Referentiedocumentatie voor de Translator Speech-API.
services: cognitive-services
author: Jann-Skotdal
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-speech
ms.topic: reference
ms.date: 05/18/2018
ms.author: v-jansko
ROBOTS: NOINDEX
ms.openlocfilehash: 46aeab52014a28d1a962195de802d0e000b62509
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46978706"
---
# <a name="translator-speech-api"></a>Translator Speech-API

Deze service biedt een streaming-API om te transcriberen conversatie spraak in één taal in de tekst van een andere taal. De API voor tekst naar spraak mogelijkheden voor het weer de vertaalde tekst uitspreken kan ook worden geïntegreerd. De Translator Speech-API kunt scenario's zoals realtime omzetting van gesprekken zoals te zien is in Skype Translator.

Met de spraak-API van Translator clienttoepassingen streamen gesproken audio naar de service en het ontvangen van een stream van de resultaten op basis van tekst, waaronder de herkende tekst in de source-taal en de vertaling in de doel-taal. Resultaten van de tekst worden geproduceerd door toe te passen van automatische spraakherkenning Recognition (ASR) mogelijk gemaakt door deep neural networks naar de binnenkomende stream audio. Onbewerkte ASR-uitvoer is verder door de nieuwe techniek TrueText om beter aan bedoeling van gebruikers te worden verbeterd. TrueText verwijdert bijvoorbeeld disfluencies (hmms en coughs) en de juiste interpunctie terugzetten hoofdletters en kleine letters. De mogelijkheid om te maskeren of uitsluiten profanities is ook opgenomen. De opname- en NAT-engines zijn speciaal getraind om af te handelen conversatie spraak. Stiltedetectie de Translator Speech-service gebruikt om te bepalen van het einde van een utterance. Na een onderbreking in de stem-activiteit, wordt de service stream weer een definitieve resultaat voor de voltooide utterance. De service kan ook verzenden terug gedeeltelijke resultaten, zodat tussenliggende herkenningen en vertalingen voor een utterance wordt uitgevoerd. De definitieve resultaten voor biedt de service de mogelijkheid om na te bootsen spraak (tekst naar spraak) van de gesproken tekst in de doel-talen. Text to Speech audio wordt gemaakt in de indeling die is opgegeven door de client. WAV en MP3-indelingen zijn beschikbaar.

Translator Speech-API maakt gebruik van de WebSocket-protocol voor een kanaal full-duplex-communicatie tussen de client en de server. Een toepassing moet deze stappen om de service te gebruiken:

## <a name="1-getting-started"></a>1. Aan de slag
Voor toegang tot de Translator Text-API u moet [zich registreren voor Microsoft Azure](translator-speech-how-to-signup.md).

## <a name="2-authentication"></a>2. Verificatie

De abonnementssleutel gebruiken om te verifiëren. De Translator Speech-API ondersteunt twee modi van verificatie:

* **Met behulp van een toegangstoken:** verkrijgen In uw toepassing, een toegangstoken van de service voor beveiligingstokens. Uw abonnementssleutel Translator Speech-API gebruiken om te verkrijgen van een toegangstoken van de verificatieservice voor Azure Cognitive Services. Het toegangstoken is geldig voor 10 minuten. Een nieuw toegangstoken verkrijgen om de 10 minuten, en houd met behulp van dezelfde toegang token voor herhaalde aanvragen binnen deze 10 minuten.

* **Met behulp van een abonnementssleutel rechtstreeks:** In uw toepassing, kunt u uw abonnementssleutel doorgegeven als een waarde in `Ocp-Apim-Subscription-Key` header.

Uw abonnementssleutel en het toegangstoken behandelen als geheimen die moeten worden verborgen.

## <a name="3-query-languages"></a>3. Querytalen
**Query uitvoeren op de resource van de talen voor de huidige set van ondersteunde talen.** De [talen resource](languages-reference.md) wordt aangegeven dat de set met talen en stemmen beschikbaar voor spraakherkenning, tekst en tekst naar spraak. Elke taal of stem krijgt een id in die de Translator Speech-API wordt gebruikt om de dezelfde taal of stem te identificeren.

## <a name="4-stream-audio"></a>4. Stream audio
**Een verbinding openen en beginnen met streamen van audio naar de service.** De service-URL is `wss://dev.microsofttranslator.com/speech/translate`. Parameters en audio-indelingen werd verwacht door de service worden hieronder beschreven, in de `/speech/translate` bewerking. Een van de parameters wordt gebruikt om door te geven van het toegangstoken uit stap 2 hierboven.

## <a name="5-process-the-results"></a>5. De resultaten verwerken
**De resultaten van de service wordt gestreamd verwerken.** De indeling van gedeeltelijke resultaten, de laatste resultaten en Text to Speech audio segmenten worden beschreven in de documentatie van de `/speech/translate` bewerking hieronder.

Gebruik van de Translator Speech-API aan te tonen codevoorbeelden zijn beschikbaar via de [Microsoft Translator Github-site](https://github.com/MicrosoftTranslator).

## <a name="implementation-notes"></a>Opmerkingen bij de implementatie

/Speech/translate Establishes een sessie voor vertaling van gesproken tekst ophalen

### <a name="connecting"></a>Verbinding maken
Bekijk voordat u verbinding maakt met de service, de lijst met parameters die later in deze sectie worden gegeven. Een van de voorbeeldaanvraag is:

`GET wss://dev.microsofttranslator.com/speech/translate?from=en-US&to=it-IT&features=texttospeech&voice=it-IT-Elsa&api-version=1.0`
`Ocp-Apim-Subscription-Key: {subscription key}`
`X-ClientTraceId: {GUID}`

De aanvraag bevat dat gesproken Engels worden gestreamd naar de service en vertaald naar Italiaans. Elke laatste herkenningsresultaat genereert een Text to Speech audio antwoord met de vrouwelijke stem Elsa met de naam. U ziet dat de aanvraag bevat de referenties in de `Ocp-Apim-Subscription-Key header`. Het verzoek volgt ook een best practice door in te stellen van een globally unique identifier in de header `X-ClientTraceId`. Een clienttoepassing moet de trace-ID aanmelden, zodat het kan worden gebruikt voor het oplossen van problemen wanneer ze zich voordoen.

### <a name="sending-audio"></a>Verzenden van audio
Zodra de verbinding tot stand is gebracht, begint de client streamen van audio naar de service. De client verzendt audio in segmenten. Elk segment wordt verzonden met behulp van een Websocket-bericht van het gegevenstype Binary.

Audio-invoer is in de Wave Audio-indeling (WAVE, of meer bekend als WAV vanwege de bestandsnaamextensie). De clienttoepassing moet één kanaal, 16-bits ondertekende steekproef op 16 kHz PCM-audio streamen. De eerste set met bytes die worden gestreamd door de client wordt de WAV-header bevatten. De header van een 44 bytes is voor één kanaal ondertekend 16 bits PCM stream steekproef op 16 kHz:

|Offset|Waarde|
|:---|:---|
|0 - 3|"RIFF"|
|4 - 7|0|
|8 - 11|"WAVE"|
|12: 15|"fmt"|
|16 - 19|16|
|20 - 21|1|
|22 - 23|1|
|24 - 27|16000|
|28 - 31|32000|
|32 - 33|2|
|34 - 35|16|
|36 - 39|"gegevens"|
|40 - 43|0|

U ziet dat de totale bestandsgrootte (byte 4-7) en de grootte van de "gegevens" (40-43 bytes) zijn ingesteld op nul. Dit is geen probleem voor het streaming-scenario waarin de totale grootte is niet noodzakelijkerwijs bekend kosten vooraf.

Na het verzenden van de header WAV (RIFF), verzendt de client segmenten van de gegevens. De client wordt meestal stream segmenten van verschillende vaste grootte voor een vaste duur (bijvoorbeeld stream 100 MS van audio op een tijdstip).

### <a name="final-result"></a>Laatste resultaat
Een definitieve spraak herkenningsresultaat wordt aan het einde van een utterance gegenereerd. Een resultaat wordt van de service naar de client met behulp van een WebSocket-bericht van het type tekst verzonden. De inhoud van het bericht is de JSON-serialisatie van een object met de volgende eigenschappen:

* `type`: Constante voor het identificeren van het type van het resultaat van de tekenreeks. De waarde is definitief voor de laatste resultaten.
* `id`: De id die is toegewezen aan het herkenningsresultaat tekenreeks.
* `recognition`: De herkende tekst in de source-taal. De tekst is mogelijk een lege tekenreeks in het geval van een false erkenning.
* `translation`: De herkende tekst wordt vertaald in de doel-taal.
* `audioTimeOffset`: Time-offset van het begin van de spraakherkenning in tikken (1 maatstreepjes = 100 nanoseconden). De offset is ten opzichte van het begin van streaming.
* `audioTimeSize`: De duur in tikken (100 nanoseconden) van de opname.
* `audioStreamPosition`: Byte-offset van het begin van de opname. De offset is ten opzichte van het begin van de stroom.
* `audioSizeBytes`: Grootte in bytes van de opname.

Houd er rekening mee dat plaatsing van de opname in de audiostream niet standaard in de resultaten opgenomen is. De `TimingInfo` functie moet worden geselecteerd door de client (Zie `features` parameter).

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
Gedeeltelijk of tussenliggende spraak herkenningsresultaten worden niet gestreamd naar de client standaard. De client kan de queryparameter functies gebruiken om aan te vragen deze.

Een gedeeltelijke resultaten wordt van de service naar de client met behulp van een WebSocket-bericht van het type tekst verzonden. De inhoud van het bericht is de JSON-serialisatie van een object met de volgende eigenschappen:

* `type`: Constante voor het identificeren van het type van het resultaat van de tekenreeks. De waarde is gedeeltelijk voor gedeeltelijke resultaten.
* `id`: De id die is toegewezen aan het herkenningsresultaat tekenreeks.
* `recognition`: De herkende tekst in de source-taal.
* `translation`: De herkende tekst wordt vertaald in de doel-taal.
* `audioTimeOffset`: Time-offset van het begin van de spraakherkenning in tikken (1 maatstreepjes = 100 nanoseconden). De offset is ten opzichte van het begin van streaming.
* `audioTimeSize`: De duur in tikken (100 nanoseconden) van de opname.
* `audioStreamPosition`: Byte-offset van het begin van de opname. De offset is ten opzichte van het begin van de stroom.
* `audioSizeBytes`: Grootte in bytes van de opname.

Houd er rekening mee dat plaatsing van de opname in de audiostream niet standaard in de resultaten opgenomen is. De functie TimingInfo moet worden geselecteerd door de client (Zie functies parameter).

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

### <a name="text-to-speech"></a>Tekst naar spraak
Wanneer de Text to Speech-functie is ingeschakeld (Zie `features` parameter hieronder), een definitieve resultaat wordt gevolgd door de audio van de gesproken vertaalde tekst. Audiogegevens is gesegmenteerde overdrachtscodering en van de service verzonden naar de client als een reeks Websocket-berichten van het type Binary. Een client kan het einde van de stroom detecteren door het controleren van de bit FIN van elk bericht. Het laatste binaire bericht heeft de FIN bit is ingesteld op een aan het einde van de stroom. De indeling van de stroom, is afhankelijk van de waarde van de `format` parameter.

### <a name="closing-the-connection"></a>De verbinding wordt gesloten
Wanneer een clienttoepassing is voltooid met het streamen van audio en het laatste eindresultaat heeft ontvangen, moet deze de verbinding door te starten van de WebSocket-handshake haakje sluiten. Er zijn situaties die zorgt ervoor dat de server de verbinding is beëindigd. De volgende codes voor WebSocket gesloten mogen worden ontvangen door de client:

* `1003 - Invalid Message Type`: De server kan de verbinding wordt beëindigd omdat deze niet het gegevenstype daarop accepteren. Dit gebeurt vaak wanneer binnenkomende audio niet met een correcte header begint.
* `1000 - Normal closure`: De verbinding is verbroken nadat de aanvraag is uitgevoerd. De server verbreekt de verbinding: wanneer er geen audio wordt ontvangen van de client voor een lange periode; Wanneer stilte worden gestreamd voor een lange periode; Wanneer een sessie bereikt de maximale duur toegestaan (ongeveer 90 minuten).
* `1001 - Endpoint Unavailable`: Geeft aan dat de server wordt niet beschikbaar. Clienttoepassing kan proberen opnieuw verbinding maken met een limiet voor het aantal nieuwe pogingen.
* `1011 - Internal Server Error`: De verbinding wordt gesloten door de server vanwege een fout op de server.

### <a name="parameters"></a>Parameters

|Parameter|Waarde|Beschrijving|Parametertype|Gegevenstype|
|:---|:---|:---|:---|:---|
|API-versie|1.0|De versie van de API die is aangevraagd door de client. Toegestane waarden zijn: `1.0`.|query   |tekenreeks|
|uit|(leeg)   |Hiermee geeft u de taal van de binnenkomende spraak. De waarde is een van de taal-id's van de `speech` bereik in het antwoord van de API talen.|query|tekenreeks|
|tot|(leeg)|Hiermee geeft u de taal voor het vertalen van de getranscribeerde tekst in. De waarde is een van de taal-id's van de `text` bereik in het antwoord van de API talen.|query|tekenreeks|
|database|(leeg)   |Door komma's gescheiden reeks functies die door de client is geselecteerd. Beschikbare functies zijn onder andere:<ul><li>`TextToSpeech`: Hiermee wordt aangegeven dat de service moet de vertaalde audio van de laatste vertaalde zin houden.</li><li>`Partial`: Hiermee wordt aangegeven dat de service moet tussenliggende herkenningsresultaten terwijl de audio is streaming naar de service.</li><li>`TimingInfo`: Hiermee wordt aangegeven dat de service moet timinginformatie die is gekoppeld aan elke herkenning.</li></ul>Als u bijvoorbeeld een client geeft `features=partial,texttospeech` voor het ontvangen van gedeeltelijke resultaten en tekst naar spraak, maar er zijn geen klokinformatie. Houd er rekening mee dat de laatste resultaten altijd worden gestreamd naar de client.|query|tekenreeks|
|Stem|(leeg)|Geeft aan welke stem te gebruiken voor de Text to Speech rendering van de vertaalde tekst. De waarde is een van de stem-id van de scope tts in het antwoord van de API talen. Als een stem is niet opgegeven dat het systeem wordt automatisch een kiezen wanneer de Text to Speech-functie is ingeschakeld.|query|tekenreeks|
|Indeling|(leeg)|Hiermee geeft u de indeling van de Text to Speech audiostream geretourneerd door de service. De volgende opties zijn beschikbaar:<ul><li>`audio/wav`: Wave audiostream. Client moet de WAV-header gebruiken om de audio-indeling correct worden geïnterpreteerd. WAV-audio voor tekst naar spraak is 16-bits, één kanaal PCM met een samplefrequentie van 24 of 16kHz.</li><li>`audio/mp3`: Audio MP3-stream.</li></ul>De standaardwaarde is `audio/wav`.|query|tekenreeks|
|ProfanityAction    |(leeg)    |Hiermee geeft u op hoe de service profanities herkend in de gesproken tekst moet verwerken. Geldige acties zijn:<ul><li>`NoAction`: Profanities zijn ongewijzigd worden gelaten.</li><li>`Marked`: Profanities vervangen door een markering. Zie `ProfanityMarker` parameter.</li><li>`Deleted`: Profanities worden verwijderd. Bijvoorbeeld, als het woord `"jackass"` wordt beschouwd als een grof taalgebruik, de woordgroep `"He is a jackass."` wordt `"He is a .".`</li></ul>De standaardwaarde is gemarkeerd.|query|tekenreeks|
|ProfanityMarker|(leeg)    |Hiermee geeft u op hoe gedetecteerde profanities worden afgehandeld wanneer `ProfanityAction` is ingesteld op `Marked`. Geldige opties zijn:<ul><li>`Asterisk`: Profanities zijn vervangen door de tekenreeks `***`. Bijvoorbeeld, als het woord `"jackass"` wordt beschouwd als een grof taalgebruik, de woordgroep `"He is a jackass."` wordt `"He is a ***.".`</li><li>`Tag`: Scheldwoorden worden omringd door een grof taalgebruik XML-code. Bijvoorbeeld, als het woord `"jackass"` wordt beschouwd als een grof taalgebruik, de woordgroep `"He is a jackass."` wordt `"He is a <profanity>jackass</profanity>."`.</li></ul>De standaardwaarde is `Asterisk`.|query|tekenreeks|
|Autorisatie|(leeg)  |Hiermee geeft u de waarde van het bearer-token van de client. Gebruik het voorvoegsel `Bearer` gevolgd door de waarde van de `access_token` waarde die wordt geretourneerd door de token authentication-service.|koptekst   |tekenreeks|
|OCP-Apim-Subscription-Key|(leeg)|Vereist als de `Authorization` -header is niet opgegeven.|koptekst|tekenreeks|
|access_token|(leeg)   |Alternatieve manier om door te geven van een geldige OAuth-toegangstoken. Het bearer-token is meestal voorzien van koptekst `Authorization`. Sommige bibliotheken websocket toegestaan clientcode om in te stellen headers niet. In dit geval is, kan de client gebruiken de `access_token` queryparameter om door te geven van een geldig token. Bij het gebruik van een toegangstoken om te verifiëren, als `Authorization` header niet is ingesteld, klikt u vervolgens `access_token` moet worden ingesteld. Als de kop- en queryparameter zijn ingesteld, wordt de queryparameter genegeerd. Clients moeten een methode alleen gebruiken om het token.|query|tekenreeks|
|abonnement-sleutel|(leeg)   |Alternatieve manier om door te geven abonnementssleutel. Sommige bibliotheken websocket toegestaan clientcode om in te stellen headers niet. In dit geval is, kan de client gebruiken de `subscription-key` queryparameter om door te geven van een sleutel geldig abonnement. Bij het gebruik van de abonnementssleutel van een om te verifiëren, als `Ocp-Apim-Subscription-Key` header is niet ingesteld, wordt abonnement-sleutel moet worden ingesteld. Als de kop- en queryparameter zijn ingesteld, wordt de queryparameter genegeerd. Clients moeten een methode alleen gebruiken om door te geven de `subscription key`.|query|tekenreeks|
|X-ClientTraceId    |(leeg)    |Een client gegenereerde GUID gebruikt voor het traceren van een aanvraag. Voor het juiste oplossen van problemen, moeten clients voorzien van elke aanvraag een nieuwe waarde en aangemeld.<br/>In plaats van een koptekst, deze waarde kan worden doorgegeven met queryparameter `X-ClientTraceId`. Als de kop- en queryparameter zijn ingesteld, wordt de queryparameter genegeerd.|koptekst|tekenreeks|
|X-CorrelationId|(leeg)    |Een client gegenereerde id gebruikt voor het correleren van meerdere kanalen in een gesprek. Meerdere spraak vertaling sessies kunnen worden gemaakt om in te schakelen gesprekken tussen gebruikers. In dergelijke scenario gebruik alle spraak vertaling sessies dezelfde correlatie-ID aan de kanalen met elkaar verbinden. Dit vereenvoudigt het tracering en diagnostische gegevens. De id moet voldoen: `^[a-zA-Z0-9-_.]{1,64}$`<br/>In plaats van een koptekst, deze waarde kan worden doorgegeven met queryparameter `X-CorrelationId`. Als de kop- en queryparameter zijn ingesteld, wordt de queryparameter genegeerd.|koptekst|tekenreeks|
|X-ClientVersion|(leeg)    |Hiermee geeft u de versie van de clienttoepassing. Voorbeeld: '2.1.0.123'.<br/>In plaats van een koptekst, deze waarde kan worden doorgegeven met queryparameter `X-ClientVersion`. Als de kop- en queryparameter zijn ingesteld, wordt de queryparameter genegeerd.|koptekst|tekenreeks|
|X-OsPlatform|(leeg)   |Hiermee geeft u de naam en versie van het besturingssysteem dat op de clienttoepassing wordt uitgevoerd. Voorbeelden: "Android 5.0", 'iOs 8.1.3', ' Windows 8.1 ".<br/>In plaats van een koptekst, deze waarde kan worden doorgegeven met queryparameter `X-OsPlatform`. Als de kop- en queryparameter zijn ingesteld, wordt de queryparameter genegeerd.|koptekst|tekenreeks|

### <a name="response-messages"></a>Berichten met reacties

|HTTP-statuscode|Reden|Het Reactiemodel|Headers|
|:--|:--|:--|:--|
|101    |WebSocket-upgrade.|Voorbeeldwaarde van model <br/> Object {}|X-RequestId<br/>Een waarde van de aanvraag voor het oplossen van problemen te identificeren.<br/>tekenreeks|
|400    |Ongeldige aanvraag. Controleer de invoerparameters om ervoor te zorgen dat geldig zijn. Het antwoordobject bevat een gedetailleerde beschrijving van de fout.|||
|401    |Niet gemachtigd. Zorg ervoor dat referenties zijn ingesteld, of ze geldig zijn en dat uw abonnement op Azure Data markt goede status met een saldo beschikbaar is.|||
|500    |Er is een fout opgetreden. Als de fout zich blijft voordoen, het rapport met trace-client-id (X-ClientTraceId) of aanvraag-id (X-RequestId).|||
|503    |De server is tijdelijk niet beschikbaar. Probeer de aanvraag. Als de fout zich blijft voordoen, het rapport met trace-client-id (X-ClientTraceId) of aanvraag-id (X-RequestId).|||

    


    





    
    




    




    




    

            




        









