---
title: Translator Speech-API talen methode
titleSuffix: Azure Cognitive Services
description: Gebruik de talen van Translator Speech-API-methode.
services: cognitive-services
author: Jann-Skotdal
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-speech
ms.topic: conceptual
ms.date: 05/18/2018
ms.author: v-jansko
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 7498ba08b9ce7b6aae10f38a393eb8cba37f3f4e
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57435759"
---
# <a name="translator-speech-api-languages"></a>Translator Speech-API: Languages

[!INCLUDE [Deprecation note](../../../includes/cognitive-services-translator-speech-deprecation-note.md)]

Translator Speech wordt voortdurend uitgebreid voor de lijst met talen die worden ondersteund door de services. Gebruik deze API voor het detecteren van de reeks talen die momenteel beschikbaar voor gebruik met de Translator Speech-service.

Voorbeelden van code aan te tonen gebruik van de API om op te halen van de beschikbare talen zijn beschikbaar via de [Microsoft Translator GitHub-site](https://github.com/MicrosoftTranslator).

## <a name="implementation-notes"></a>Opmerkingen bij de implementatie

### <a name="get-languages"></a>/Languages ophalen

Een groot aanbod van talen is beschikbaar voor spraak, de getranscribeerde tekst vertalen, en voor het produceren van kunstmatige spraak van de vertaling transcriberen.

Een client gebruikt de `scope` queryparameter om te definiëren welke sets met talen die het is geïnteresseerd in.

* **Spraak-naar-tekst:** Gebruik van de queryparameter `scope=speech` om de set beschikbare spraak naar tekst te transcriberen talen te halen.
* **Vertalen van tekst:** Gebruik van de queryparameter `scope=text` om op te halen van de set met talen die beschikbaar zijn voor de omzetting van getranscribeerde tekst.
* **Tekst naar spraak:**  Gebruik van de queryparameter `scope=tts` om op te halen van de reeks talen en stemmen beschikbaar om na te bootsen vertaalde tekst naar spraak.

Een client kan meerdere sets tegelijk ophalen door een door komma's gescheiden lijst met opties op te geven. Bijvoorbeeld `scope=speech,text,tts`.

Een geslaagde reactie is een JSON-object met een eigenschap voor elke set aangevraagd.

```
{
    "speech": {
        //... Set of languages supported for speech-to-text
    },
    "text": {
        //... Set of languages supported for text translation
    },
    "tts": {
        //... Set of languages supported for text-to-speech
    }
}
```

Nadat een client kan gebruiken de `scope` queryparameter selecteren welke sets van ondersteunde talen moet worden geretourneerd, een echte reactie omvat mogelijk alleen een subset van alle eigenschappen die hierboven worden weergegeven.

De waarde die is opgegeven met elke eigenschap is als volgt.

### <a name="speech-to-text-speech"></a>Spraak-naar-tekst (spraak)

De waarde die is gekoppeld met de spraak-naar-tekst-eigenschap, `speech`, is een dictionary van (sleutel, waarde) paren. Elke sleutel identificeert een taal die wordt ondersteund voor spraak-naar-tekst. De sleutel is de id die client wordt doorgegeven aan de API. De waarde die is gekoppeld aan de sleutel is een object met de volgende eigenschappen:

* `name`: Weergavenaam van de taal.
* `language`: De taalcode van de bijbehorende taal geschreven. Zie 'Tekst transactie' hieronder.
Er is een voorbeeld:

```
{
    "speech": {
        "en-US": { name: "English", language: "en" }
    }
}
```

### <a name="text-translation-text"></a>Tekstomzetting (tekst)

De waarde die is gekoppeld aan de `text` eigenschap is ook een woordenlijst waar elke sleutel een taal voor tekstvertaling wordt ondersteund identificeert. De waarde die is gekoppeld aan de sleutel worden beschreven van de taal:

* `name`: Weergavenaam van de taal.
* `dir`: Richting is `rtl` voor talen van rechts naar links of `ltr` voor talen van links naar rechts.

Er is een voorbeeld:

```
{
    "text": {
        "en": { name: "English", dir: "ltr" }
    }
}
```

### <a name="text-to-speech-tts"></a>Spraak

De waarde die is gekoppeld aan de Text to Speech eigenschap tts, is ook een woordenlijst waar elke sleutel een ondersteunde stem identificeert. Kenmerken van een stem-object zijn:

* `displayName`: Weergavenaam voor de stem.
* `gender`: Het geslacht van de stem (mannelijke of vrouwelijke).
* `locale`: De taalcode van de stem met subcode van de primaire taal en regio subcode.
* `language`: De taalcode van de bijbehorende taal geschreven.
* `languageName`: Weergavenaam van de taal.
* `regionName`: De weergavenaam van de regio voor deze taal.

Er is een voorbeeld:

```
{
    "tts": {
        "en-US-Zira": {
            "displayName": "Zira",
            "gender": "female",
            "locale": "en-US",
            "languageName": "English",
            "regionName": "United States",
            "language": "en"
        }
}
```

### <a name="localization"></a>Lokalisatie
De service retourneert alle namen in de taal van de header 'Accept-Language' voor alle talen die worden ondersteund in tekst.

### <a name="response-class-status-200"></a>Antwoord-klasse (Status 200)
Object met een beschrijving van de set met ondersteunde talen.

ModelExample waarde:

Langagues {spraak (object, optioneel), tekst (object, optioneel), tts (object, optioneel)}

### <a name="headers"></a>Headers

|Header|Description|Type|
:--|:--|:--|
X-RequestId|Waarde die worden gegenereerd door de server voor het identificeren van de aanvraag en gebruikt voor het oplossen van problemen.|string|

### <a name="parameters"></a>Parameters

|Parameter|Description|Parametertype|Gegevenstype|
|:--|:--|:--|:--|
|API-versie    |De versie van de API die is aangevraagd door de client. Toegestane waarden zijn: `1.0`.|query|string|
|scope  |Sets met ondersteunde talen of stemmen om terug te keren naar de client. Deze parameter is opgegeven als een door komma's gescheiden lijst met trefwoorden. De volgende trefwoorden zijn beschikbaar:<ul><li>`speech`: Biedt de talen die worden ondersteund voor spraak transcriberen.</li><li>`tts`: Biedt de stemmen voor tekst-speech conversie wordt ondersteund.</li><li>`text`: Biedt de talen die worden ondersteund voor het omzetten van tekst.</li></ul>Als een waarde niet opgegeven is, de waarde van `scope` standaard ingesteld op `text`.|query|string|
|X-ClientTraceId    |Een client gegenereerde GUID gebruikt voor het traceren van een aanvraag. Om te kunnen oplossen van problemen met, moeten clients voorzien van elke aanvraag een nieuwe waarde en aangemeld.|koptekst|string|
|Accept-Language    |Enkele van de velden in het antwoord zijn namen van talen of regio's. Gebruik deze parameter voor het definiëren van de taal waarin de namen worden geretourneerd. De taal die is opgegeven door op te geven van een opgemaakte BCP-47-taalcode. Selecteer een label in de lijst van taal-id's die zijn geretourneerd met de `text` bereik. Voor niet-ondersteunde talen vindt u de namen in het Engels.<br/>Gebruik bijvoorbeeld de waarde `fr` aan te vragen van namen in het Frans of gebruikt u de waarde `zh-Hant` naar namen van de aanvraag in een traditioneel Chinees.|koptekst|string|

### <a name="response-messages"></a>Berichten met reacties

|HTTP-statuscode|Reden|
|:--|:--|
|400|Ongeldige aanvraag. Controleer de invoerparameters om ervoor te zorgen dat geldig zijn. Het antwoordobject bevat een gedetailleerde beschrijving van de fout.|
|429|Te veel aanvragen.|
|500|Er is een fout opgetreden. Als de fout zich blijft voordoen, het rapport met trace-client-id (X-ClientTraceId) of aanvraag-id (X-RequestId).|
|503|De server is tijdelijk niet beschikbaar. Probeer de aanvraag. Als de fout zich blijft voordoen, het rapport met trace-client-id (X-ClientTraceId) of aanvraag-id (X-RequestId).|
