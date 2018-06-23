---
title: Microsoft Translator Speech API talen methode | Microsoft Docs
titleSuffix: Cognitive Services
description: Gebruik de methode Microsoft Translator Speech-API talen.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.technology: microsoft translator
ms.topic: article
ms.date: 05/18/18
ms.author: v-jansko
ms.openlocfilehash: 5396e3be17345c3c36197a9b6cbace86e1f574c1
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345847"
---
# <a name="speech-api-languages"></a>Speech-API: talen

Microsoft Translator breidt voortdurend de lijst met talen die worden ondersteund door de services. Deze API gebruiken voor het detecteren van de set van talen die momenteel beschikbaar voor gebruik met de vertaalservice spraak.

Codevoorbeelden demonstreren gebruik van de API om op te halen van de beschikbare talen beschikbaar zijn via de [Microsoft Translator Github site](https://github.com/MicrosoftTranslator).

## <a name="implementation-notes"></a>Opmerkingen bij de implementatie

/Languages ophalen 

Er is een groot aantal talen beschikbaar transcriberen spraak, om de transcribed tekst te vertalen en voor het produceren van kunstmatige spraak van de vertaling.

Een client gebruikt de `scope` queryparameter om te definiëren welke sets van talen die het is geïnteresseerd in.

* **Spraak naar tekst:** gebruiken queryparameter `scope=speech` voor het ophalen van het aantal talen beschikbaar transcriberen spraak naar tekst.
* **Tekst voor vertaling:** gebruiken queryparameter `scope=text` voor het ophalen van de reeks talen die beschikbaar zijn om tekst omgezet te vertalen.
* **Spraak:** gebruiken queryparameter `scope=tts` voor het ophalen van de reeks talen en stemmen beschikbaar om na te bootsen vertaalde tekst naar spraak.

Een client kan tegelijkertijd meerdere sets ophalen door te geven van een door komma's gescheiden lijst met opties. Bijvoorbeeld `scope=speech,text,tts`.

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

Nadat een client gebruikt de `scope` queryparameter te selecteren welke sets van ondersteunde talen moet worden geretourneerd, een werkelijke antwoord kan alleen een subset van alle eigenschappen die hierboven weergegeven bevatten.

De waarde die is opgegeven met elke eigenschap is als volgt.

### <a name="speech-to-text-speech"></a>Spraak-naar-tekst (spraak)

De waarde die is gekoppeld aan de eigenschap spraak naar tekst `speech`, is een dictionary van (sleutel, waarde) paren. Elke sleutel identificeert een taal die wordt ondersteund voor spraak-naar-tekst. De sleutel is de id die client wordt doorgegeven aan de API. De waarde die is gekoppeld aan de sleutel is een object met de volgende eigenschappen:

* `name`: De naam van de taal weergegeven.
* `language`: Label taal van de bijbehorende taal geschreven. Zie 'Tekst line transactie' hieronder.
Er is een voorbeeld:

```
{
    "speech": {
        "en-US": { name: "English", language: "en" }
    }
}
```

### <a name="text-translation-text"></a>Tekst voor vertaling (tekst)

De waarde die is gekoppeld aan de `text` eigenschap is ook een woordenboek waar elke sleutel een taal die wordt ondersteund voor tekstvertaling identificeert. De waarde die is gekoppeld aan de sleutel wordt de taal beschreven:

* `name`: De naam van de taal weergegeven.
* `dir`: Richtinggevoeligheid die `rtl` voor talen van rechts naar links of `ltr` voor talen van links naar rechts.

Er is een voorbeeld:

```
{
    "text": {
        "en": { name: "English", dir: "ltr" }
    }
}
```

### <a name="text-to-speech-tts"></a>Spraak

De waarde die is gekoppeld met de tekst-naar-eigenschap, tts, is ook een woordenboek waarbij elke sleutel wordt aangeduid met een ondersteunde stem. Kenmerken van een object stem zijn:

* `displayName`: Weergavenaam voor de stem.
* `gender`: Geslacht van de stem (man of vrouw).
* `locale`:-Code van de stem met primaire taal subcode en regio subcode taal.
* `language`: Label taal van de bijbehorende taal geschreven.
* `languageName`: De naam van de taal weergegeven.
* `regionName`: De naam van het gebied voor deze taal weergegeven.

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
De service retourneert alle namen in de taal van de header Accept-Language voor alle talen die worden ondersteund in de tekstvertaling van.

### <a name="response-class-status-200"></a>Antwoord klasse (Status 200)
Object met een beschrijving van de set van ondersteunde talen.

ModelExample waarde: 

Langagues {spraak (object, optioneel), tekst (object, optioneel), tts (object, optioneel)}

### <a name="headers"></a>Headers

|Koptekst|Beschrijving|Type|
:--|:--|:--|
X-RequestId|Waarde gegenereerd door de server voor het identificeren van de aanvraag en gebruikt voor het oplossen van problemen.|tekenreeks|

### <a name="parameters"></a>Parameters

|Parameter|Beschrijving|Parametertype|Gegevenstype|
|:--|:--|:--|:--|
|API-versie    |De versie van de API die door de client wordt aangevraagd. Toegestane waarden zijn: `1.0`.|query|tekenreeks|
|scope  |Sets met ondersteunde talen of stemmen om terug te keren naar de client. Deze parameter wordt opgegeven als een door komma's gescheiden lijst met trefwoorden. De volgende sleutelwoorden zijn beschikbaar:<ul><li>`speech`: Bevat het aantal talen ondersteund transcriberen met gebruikmaking van spraak.</li><li>`tts`: Bevat de set met stemmen voor conversie van tekst spraak ondersteund.</li><li>`text`: Bevat de set met talen die worden ondersteund voor het omzetten van tekst.</li></ul>Als een waarde niet is opgegeven, de waarde van `scope` wordt standaard ingesteld op `text`.|query|tekenreeks|
|X-ClientTraceId    |Een client gegenereerde GUID gebruikt voor het traceren van een aanvraag. Clients moeten vergemakkelijkt het oplossen van problemen, geeft u een nieuwe waarde aan elke aanvraag en meld u deze.|koptekst|tekenreeks|
|Accepteer taal    |Sommige velden in het antwoord zijn namen van talen of regio's. Gebruik deze parameter voor het definiëren van de taal waarin de namen zijn geretourneerd. De taal is opgegeven door een goed ingedeelde BCP 47 taallabel. Selecteer een label in de lijst van taal-id geretourneerd met de `text` bereik. De namen zijn opgegeven voor niet-ondersteunde talen in het Engels.<br/>Gebruik bijvoorbeeld de waarde `fr` aan te vragen van de namen in het Frans of gebruik de waarde `zh-Hant` naar namen van de aanvraag in traditioneel Chinees.|koptekst|tekenreeks|
    
### <a name="response-messages"></a>Antwoordberichten

|HTTP-statuscode|Reden|
|:--|:--|
|400|Ongeldige aanvraag. Controleer de invoerparameters zodat ze geldig zijn. Het antwoordobject bevat een gedetailleerde beschrijving van de fout.|
|429|Te veel aanvragen.|
|500|Er is een fout opgetreden. Als de fout zich blijft voordoen, het rapport met client trace-id (X-ClientTraceId) of aanvraag-id (X-RequestId).|
|503|De server is tijdelijk niet beschikbaar. Probeer de aanvraag. Als de fout zich blijft voordoen, het rapport met client trace-id (X-ClientTraceId) of aanvraag-id (X-RequestId).|
