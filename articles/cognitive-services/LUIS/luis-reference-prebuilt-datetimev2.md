---
title: Vooraf gedefinieerde LUIS entiteiten datetimeV2 referentie - Azure | Microsoft Docs
titleSuffix: Azure
description: In dit artikel is datetimeV2 vooraf gedefinieerde entiteitsgegevens in Language Understanding (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: v-geberr
ms.openlocfilehash: 261f6f27c39c280efdcd070888d735374a473c85
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2018
ms.locfileid: "36321888"
---
# <a name="datetimev2-entity"></a>DatetimeV2 entiteit

De **datetimeV2** entiteit vooraf gedefinieerde waarden voor datum en tijd opgehaald. Deze waarden worden omgezet in een gestandaardiseerde indeling voor client-programma's te gebruiken. Wanneer een utterance heeft een datum of tijd die niet worden voltooid, LUIS omvat _zowel verstreken en toekomstige waarden_ in het antwoord van het eindpunt. Omdat deze entiteit wordt al getraind, hoeft u geen voorbeeld utterances datetimeV2 naar de toepassing die met toevoegen. 

## <a name="types-of-datetimev2"></a>Typen datetimeV2
DatetimeV2 wordt beheerd vanaf de [kenmerken tekst](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-DateTime.yaml) Github-opslagplaats

## <a name="example-json"></a>JSON-voorbeeld 
Het volgende voorbeeld JSON-antwoord heeft een `datetimeV2` entiteit met een subtype van `datetime`. Zie voor voorbeelden van andere typen entiteiten datetimeV2 [subtypen van datetimeV2](#subtypes-of-datetimev2)</a>.

```JSON
"entities": [
  {
    "entity": "8am on may 2nd 2017",
    "type": "builtin.datetimeV2.datetime",
    "startIndex": 15,
    "endIndex": 30,
    "resolution": {
      "values": [
        {
          "timex": "2017-05-02T08",
          "type": "datetime",
          "value": "2017-05-02 08:00:00"
        }
      ]
    }
  }
]
  ```

## <a name="json-property-descriptions"></a>Beschrijvingen van de JSON-eigenschap

|Naam van eigenschap |Eigenschap van het type en beschrijving|
|---|---|
|Entiteit|**tekenreeks** -tekst opgehaald uit de utterance met het type datum, tijd, datumbereik of tijdsbereik.|
|type|**tekenreeks** : één van de [subtypen van datetimeV2](#subtypes-of-datetimev2)
|startIndex|**int** -de index in de utterance waarop de entiteit begint.|
|endIndex|**int** -de index in de utterance die de entiteit wordt beëindigd.|
|oplossing|Heeft een `values` matrix een, twee of vier heeft [waarden van resolutie](#values-of-resolution).|
|einde|De eindwaarde van een tijd of datumbereik in dezelfde indeling als `value`. Alleen gebruikt als `type` is `daterange`, `timerange`, of `datetimerange`|

## <a name="subtypes-of-datetimev2"></a>Subtypen van datetimeV2

De **datetimeV2** vooraf gedefinieerde entiteit heeft de volgende subtypen en voorbeelden van elke vindt u in de tabel die volgt op:
* `date`
* `time`
* `daterange`
* `timerange`
* `datetimerange`
* `duration`
* `set`

## <a name="values-of-resolution"></a>Waarden van resolutie
* De matrix heeft een element als de datum of tijd in de utterance volledig is opgegeven en niet-ambigue.
* De matrix heeft twee elementen als de waarde datetimeV2 niet eenduidig is. Dubbelzinnigheid omvat gebrek aan specifieke jaar, tijd of tijdsbereik. Zie [dubbelzinnige datums](#ambiguous-dates) voor voorbeelden. Wanneer de tijd is dubbelzinnig voor uur. of het uur, worden beide waarden zijn opgenomen.
* De matrix heeft vier elementen als de utterance twee elementen met dubbelzinnigheid heeft. Deze dubbelzinnigheid bevat elementen die hebben:
  * Een datum of datumbereik dat is niet eenduidig garantie jaar
  * Een tijd of het tijdsbereik dat is niet eenduidig garantie uur. of PM Bijvoorbeeld: 3:00 April 3e.

Elk element van de `values` matrix heeft mogelijk de volgende velden: 

|Naam van eigenschap|Beschrijving van de eigenschap|
|--|--|
|Timex|tijd, datum of datumbereik uitgedrukt in TIMEX-indeling dat volgt op de [ISO 8601-norm](https://en.wikipedia.org/wiki/ISO_8601) en de kenmerken TIMEX3 voor aantekening met de taal TimeML. Deze aantekening wordt beschreven in de [TIMEX richtlijnen](http://www.timeml.org/tempeval2/tempeval2-trial/guidelines/timex3guidelines-072009.pdf).|
|type|Het subtype, van de volgende items een: datum/tijd, datum, tijd, daterange, timerange, datetimerange, duur, set.|
|waarde|**Optioneel.** Een datum/tijd-object in de indeling yyyy:MM:dd (datum) yyyy:MM:dd: mm: SS (tijd): mm: SS (datetime). Als `type` is `duration`, de waarde is het aantal seconden (duur) <br/> Alleen gebruikt als `type` is `datetime` of `date`, `time`, of ' duur.|

## <a name="valid-date-values"></a>Geldige date-waarden

De **datetimeV2** ondersteunt datums tussen de volgende bereiken:

| Minimum | Maximum |
|----------|-------------|
| 1 januari 1900   | 31 December 2099 |

## <a name="ambiguous-dates"></a>Niet-eenduidige datums

Als de datum in het verleden of in de toekomst, biedt LUIS beide waarden. Een voorbeeld is een utterance met de maand en datum zonder het jaar.  

Bijvoorbeeld, de 'Mei 2e' utterance gegeven:
* Als de datum van vandaag 3e 2017 mogelijk is, biedt LUIS '2017-05-02' en '2018-05-02' als waarden. 
* Wanneer de datum van vandaag 1e 2017 mogelijk is, biedt LUIS '2016-05-02' en '2017-05-02' als waarden.

Het volgende voorbeeld ziet de resolutie van de entiteit 'mei 2e'. Deze oplossing wordt ervan uitgegaan dat de datum van vandaag een datum tussen mei 2e 2017 en mei 1e 2018.
Velden met `X` in de `timex` veld zijn onderdelen van de datum die expliciet zijn niet opgegeven in de utterance.

```JSON
  "entities": [
    {
      "entity": "may 2nd",
      "type": "builtin.datetimeV2.date",
      "startIndex": 0,
      "endIndex": 6,
      "resolution": {
        "values": [
          {
            "timex": "XXXX-05-02",
            "type": "date",
            "value": "2017-05-02"
          },
          {
            "timex": "XXXX-05-02",
            "type": "date",
            "value": "2018-05-02"
          }
        ]
      }
    }
  ]
```

## <a name="date-range-resolution-examples-for-numeric-date"></a>Datum bereik resolutie voorbeelden voor numerieke datum

De `datetimeV2` entiteit extraheert datum- en tijdbereiken. De `start` en `end` velden geven het begin en einde van het bereik. Voor de 'Mei 2e naar mei 5e' utterance LUIS biedt **daterange** waarden voor het huidige jaar en het volgende jaar. In de `timex` veld de `XXXX` waarden geven de dubbelzinnigheid van het jaar. `P3D` Geeft de periode is drie dagen lang.

```JSON
"entities": [
    {
      "entity": "may 2nd to may 5th",
      "type": "builtin.datetimeV2.daterange",
      "startIndex": 0,
      "endIndex": 17,
      "resolution": {
        "values": [
          {
            "timex": "(XXXX-05-02,XXXX-05-05,P3D)",
            "type": "daterange",
            "start": "2017-05-02",
            "end": "2017-05-05"
          },
          {
            "timex": "(XXXX-05-02,XXXX-05-05,P3D)",
            "type": "daterange",
            "start": "2018-05-02",
            "end": "2018-05-05"
          }
        ]
      }
    }
  ]
```

## <a name="date-range-resolution-examples-for-day-of-week"></a>Datum bereik resolutie voorbeelden voor dag van week

Het volgende voorbeeld ziet u hoe LUIS gebruikt **datetimeV2** omzetten van de utterance "Dinsdag donderdag". In dit voorbeeld is de huidige datum 19 juni. LUIS omvat **daterange** waarden voor zowel de datumbereiken die voorafgaan aan en volgt u de huidige datum.

```JSON
  "entities": [
    {
      "entity": "tuesday to thursday",
      "type": "builtin.datetimeV2.daterange",
      "startIndex": 0,
      "endIndex": 19,
      "resolution": {
        "values": [
          {
            "timex": "(XXXX-WXX-2,XXXX-WXX-4,P2D)",
            "type": "daterange",
            "start": "2017-06-13",
            "end": "2017-06-15"
          },
          {
            "timex": "(XXXX-WXX-2,XXXX-WXX-4,P2D)",
            "type": "daterange",
            "start": "2017-06-20",
            "end": "2017-06-22"
          }
        ]
      }
    }
  ]
```
## <a name="ambiguous-time"></a>Niet-eenduidige tijd
De matrix values heeft twee tijdselementen als de tijd of het tijdsbereik niet eenduidig is. Wanneer er een niet-eenduidige tijd, hebben waarden beide het uur. en uur. tijden.

## <a name="time-range-resolution-example"></a>Voorbeeld van de tijd range resolutie

Het volgende voorbeeld ziet u hoe LUIS gebruikt **datetimeV2** omzetten van de utterance met een tijdsbereik.

```
  "entities": [
    {
      "entity": "6pm to 7pm",
      "type": "builtin.datetimeV2.timerange",
      "startIndex": 0,
      "endIndex": 9,
      "resolution": {
        "values": [
          {
            "timex": "(T18,T19,PT1H)",
            "type": "timerange",
            "start": "18:00:00",
            "end": "19:00:00"
          }
        ]
      }
    }
  ]
```

## <a name="deprecated-prebuilt-datetime"></a>Afgeschafte vooraf gedefinieerde datum/tijd

De `datetime` vooraf gedefinieerde entiteit is afgeschaft en wordt vervangen door [ `datetimeV2` ](#builtindatetimev2). 

Ter vervanging van `datetime` met `datetimeV2` in uw app LUIS de volgende stappen uitvoeren:

1. Open de **entiteiten** deelvenster van de LUIS webinterface. 
2. Verwijder de **datetime** vooraf gedefinieerde entiteit.
3. Klik op **vooraf gedefinieerde entiteit toevoegen**
4. Selecteer **datetimeV2** en klik op **opslaan**.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de [dimensie](luis-reference-prebuilt-dimension.md), [e](luis-reference-prebuilt-email.md) entiteiten, en [nummer](luis-reference-prebuilt-number.md). 

