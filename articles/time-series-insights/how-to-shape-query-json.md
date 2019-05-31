---
title: Aanbevolen procedures voor het vormgeven van JSON in Azure Time Series Insights-query's | Microsoft Docs
description: Leer hoe u uw Azure Time Series Insights-query-efficiëntie te verbeteren.
services: time-series-insights
author: ashannon7
manager: cshankar
ms.service: time-series-insights
ms.topic: article
ms.date: 05/09/2019
ms.author: dpalled
ms.custom: seodec18
ms.openlocfilehash: 089285637bb740fea47f1fd07de0906dfe46662b
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66244461"
---
# <a name="shape-json-to-maximize-query-performance"></a>Shape JSON naar het maximaliseren van de prestaties van query 's 

Dit artikel bevat richtlijnen over het vorm JSON naar de efficiëntie van uw Azure Time Series Insights-query's te maximaliseren.

## <a name="video"></a>Video

### <a name="learn-best-practices-for-shaping-json-to-meet-your-storage-needsbr"></a>Informatie over aanbevolen procedures voor het vormgeven van JSON om te voldoen aan uw opslagbehoeften.</br>

> [!VIDEO https://www.youtube.com/embed/b2BD5hwbg5I]

## <a name="best-practices"></a>Aanbevolen procedures
Denk na over hoe u gebeurtenissen naar de Time Series Insights verzenden. Dat wil zeggen u altijd:

1. gegevens zo efficiënt mogelijk via het netwerk verzenden.
1. Zorg ervoor dat uw gegevens worden opgeslagen op een manier, zodat u aggregaties geschikt is voor uw scenario kan uitvoeren.
1. Zorg ervoor dat u de grenzen van de eigenschap maximum Time Series Insights van niet bereiken:
   - 600 eigenschappen (kolommen) voor S1-omgevingen.
   - 800 eigenschappen (kolommen) voor S2-omgevingen.

De volgende richtlijnen geven om de best mogelijke query-prestaties:

1. Gebruik geen dynamische eigenschappen, zoals een label-ID als naam van een eigenschap. Dit draagt bij aan de eigenschappen van de maximale limiet is bereikt.
1. Geen overbodige eigenschappen niet verzenden. Als de eigenschap van een query is niet vereist, is het raadzaam om niet te verzenden. Zo voorkomt u beperkingen voor opslag.
1. Gebruik [verwijzen naar gegevens](time-series-insights-add-reference-data-set.md) om te voorkomen dat statische gegevens verzenden via het netwerk.
1. Delen tussen meerdere gebeurtenissen voor het verzenden van gegevens via het netwerk is het efficiënter een dimensie-eigenschappen.
1. Gebruik geen grondige matrix nesten. Time Series Insights biedt ondersteuning voor maximaal twee niveaus van geneste matrices die objecten bevatten. Time Series Insights effent matrices in berichten in meerdere gebeurtenissen met eigenschap-waardeparen.
1. Als er slechts een paar metingen bestaan voor alle of de meeste gebeurtenissen, is het beter om het verzenden van deze metingen als afzonderlijke eigenschappen binnen hetzelfde object. Afzonderlijk naar hen vermindert het aantal gebeurtenissen en prestaties van query's kan verbeteren omdat minder gebeurtenissen moeten worden verwerkt. Wanneer er meerdere metingen, minimaliseert ze worden verzonden als waarden in één eigenschap, de mogelijkheid om de limiet voor de eigenschap maximum bereikt.

## <a name="example-overview"></a>Voorbeeld-overzicht

De volgende twee voorbeelden laten zien hoe u voor het verzenden van gebeurtenissen te markeren van de bovenstaande aanbevelingen. De volgende elk voorbeeld ziet u hoe de aanbevelingen zijn toegepast.

De voorbeelden zijn gebaseerd op een scenario waar meerdere apparaten metingen of signalen verzenden. Metingen of signalen mag Flow snelheid, Engine oliedruk, temperatuur en vochtigheid. In het eerste voorbeeld vindt enkele metingen voor alle apparaten. Het tweede voorbeeld heeft een groot aantal apparaten en elk apparaat verzendt veel unieke metingen.

## <a name="scenario-one-only-a-few-measurements-exist"></a>Scenario 1: Er bestaan slechts een paar metingen

> [!TIP]
> Het is raadzaam dat u elke meting of het signaal als een afzonderlijke eigenschap of kolom verzenden.

In het volgende voorbeeld is er een enkel Azure IoT Hub-bericht wanneer de buitenste matrix een gedeelde sectie van de algemene dimensiewaarden bevat. De buitenste matrix wordt referentiegegevens worden gebruikt om de efficiëntie van het bericht. Referentiegegevens bevat metagegevens van apparaten die niet worden gewijzigd met elke gebeurtenis, maar het biedt nuttige eigenschappen voor data-analyse. Algemene dimensiewaarden batchverwerking en die gebruikmaakt van verwijzing gegevens worden opgeslagen op via de kabel verzonden bytes, waardoor het bericht efficiënter.

Voorbeeld van JSON-nettolading:

```json
[
    {
        "deviceId": "FXXX",
        "timestamp": "2018-01-17T01:17:00Z",
        "series": [
            {
                "Flow Rate ft3/s": 1.0172575712203979,
                "Engine Oil Pressure psi ": 34.7
            },
            {
                "Flow Rate ft3/s": 2.445906400680542,
                "Engine Oil Pressure psi ": 49.2
            }
        ]
    },
    {
        "deviceId": "FYYY",
        "timestamp": "2018-01-17T01:18:00Z",
        "series": [
            {
                "Flow Rate ft3/s": 0.58015072345733643,
                "Engine Oil Pressure psi ": 22.2
            }
        ]
    }
]
```

* Tabel met referentiegegevens waarvoor de sleuteleigenschap **deviceId**:

   | deviceId | messageId | deviceLocation |
   | --- | --- | --- |
   | FXXX | REGEL\_GEGEVENS | EU |
   | BJJJ | REGEL\_GEGEVENS | VS |

* Time Series Insights Gebeurtenistabel, na het plat maken:

   | deviceId | messageId | deviceLocation | timestamp | reeks. Stroom tarief ft3/s | reeks. Engine oliedruk psi |
   | --- | --- | --- | --- | --- | --- |
   | FXXX | REGEL\_GEGEVENS | EU | 2018-01-17T01:17:00Z | 1.0172575712203979 | 34,7 |
   | FXXX | REGEL\_GEGEVENS | EU | 2018-01-17T01:17:00Z | 2.445906400680542 | 49,2 |
   | BJJJ | REGEL\_GEGEVENS | VS | 2018-01-17T01:18:00Z | 0.58015072345733643 | 22.2 |

Opmerkingen over deze twee tabellen:

- De **deviceId** kolom fungeert als de kop van de kolom voor de verschillende apparaten in een hele vloot. Waardoor de apparaat-id-waarde beperkt een eigen eigenschapsnaam het totale aantal apparaten 595 (voor S1-omgevingen) of 795 (voor S2-omgevingen) met de vijf kolommen.
- Onnodige eigenschappen worden vermeden, voor bijvoorbeeld het merk en model informatie. Omdat de eigenschappen wordt niet in de toekomst worden opgevraagd, kunt zodat ze u beter netwerk en efficiëntie van opslag.
- Referentiegegevens wordt gebruikt voor het verminderen van het aantal bytes dat wordt overgedragen via het netwerk. De twee kenmerken **messageId** en **deviceLocation** worden gekoppeld met behulp van de sleuteleigenschap **deviceId**. Deze gegevens wordt samengevoegd met de telemetrische gegevens gelijktijdig met inkomend verkeer en wordt vervolgens opgeslagen in Time Series Insights voor het uitvoeren van query's.
- Twee lagen van geneste worden gebruikt, is de maximale hoeveelheid nesten ondersteund door de Time Series Insights. Het is essentieel om te voorkomen dat diep geneste matrices.
- Metingen worden verzonden als afzonderlijke eigenschappen binnen hetzelfde object, omdat er enkele metingen. Hier **reeks. Flow tarief psi** en **reeks. Oliedruk ft3/s-engine** unieke kolommen zijn.

## <a name="scenario-two-several-measures-exist"></a>Scenario 2: Er bestaan meerdere metingen

> [!TIP]
> Het is raadzaam om metingen als "type", 'eenheid' en 'waarde' tuples te sturen.

Voorbeeld van JSON-nettolading:

```json
[
    {
        "deviceId": "FXXX",
        "timestamp": "2018-01-17T01:17:00Z",
        "series": [
            {
                "tagId": "pumpRate",
                "value": 1.0172575712203979
            },
            {
                "tagId": "oilPressure",
                "value": 49.2
            },
            {
                "tagId": "pumpRate",
                "value": 2.445906400680542
            },
            {
                "tagId": "oilPressure",
                "value": 34.7
            }
        ]
    },
    {
        "deviceId": "FYYY",
        "timestamp": "2018-01-17T01:18:00Z",
        "series": [
            {
                "tagId": "pumpRate",
                "value": 0.58015072345733643
            },
            {
                "tagId": "oilPressure",
                "value": 22.2
            }
        ]
    }
]
```

* Tabel met referentiegegevens waarvoor de sleuteleigenschappen **deviceId** en **series.tagId**:

   | deviceId | series.tagId | messageId | deviceLocation | type | Eenheid |
   | --- | --- | --- | --- | --- | --- |
   | FXXX | pumpRate | REGEL\_GEGEVENS | EU | Stroom | ft3/s |
   | FXXX | oilPressure | REGEL\_GEGEVENS | EU | Engine oliedruk | psi |
   | BJJJ | pumpRate | REGEL\_GEGEVENS | VS | Stroom | ft3/s |
   | BJJJ | oilPressure | REGEL\_GEGEVENS | VS | Engine oliedruk | psi |

* Time Series Insights Gebeurtenistabel, na het plat maken:

   | deviceId | series.tagId | messageId | deviceLocation | type | Eenheid | timestamp | series.value |
   | --- | --- | --- | --- | --- | --- | --- | --- |
   | FXXX | pumpRate | REGEL\_GEGEVENS | EU | Stroom | ft3/s | 2018-01-17T01:17:00Z | 1.0172575712203979 | 
   | FXXX | oilPressure | REGEL\_GEGEVENS | EU | Engine oliedruk | psi | 2018-01-17T01:17:00Z | 34,7 |
   | FXXX | pumpRate | REGEL\_GEGEVENS | EU | Stroom | ft3/s | 2018-01-17T01:17:00Z | 2.445906400680542 | 
   | FXXX | oilPressure | REGEL\_GEGEVENS | EU | Engine oliedruk | psi | 2018-01-17T01:17:00Z | 49,2 |
   | BJJJ | pumpRate | REGEL\_GEGEVENS | VS | Stroom | ft3/s | 2018-01-17T01:18:00Z | 0.58015072345733643 |
   | BJJJ | oilPressure | REGEL\_GEGEVENS | VS | Engine oliedruk | psi | 2018-01-17T01:18:00Z | 22.2 |

Opmerkingen over deze twee tabellen:

- De kolommen **deviceId** en **series.tagId** fungeren als de kolomkoppen bevat voor de verschillende apparaten en labels in een hele vloot. Gebruik van elk een eigen kenmerk beperkt de query voor het 594 (voor S1-omgevingen) of 794 (voor S2-omgevingen) totaal aantal apparaten met de zes kolommen.
- Eigenschappen van onnodige zijn vermeden, om de reden dat wordt vermeld in het eerste voorbeeld.
- Referentiegegevens wordt gebruikt om te beperken van het aantal bytes dat via het netwerk wordt overgedragen door de introductie van **deviceId**, die wordt gebruikt voor de unieke combinatie van **messageId** en **deviceLocation**. De samengestelde sleutel **series.tagId** wordt gebruikt voor de unieke combinatie van **type** en **eenheid**. De samengestelde sleutel kan de **deviceId** en **series.tagId** paar moet worden gebruikt om te verwijzen naar vier waarden: **messageId, deviceLocation, typt,** en **eenheid**. Deze gegevens wordt samengevoegd met de telemetrische gegevens gelijktijdig met inkomend verkeer. Er wordt vervolgens opgeslagen in Time Series Insights voor het uitvoeren van query's.
- twee lagen van geneste worden gebruikt om de reden dat wordt vermeld in het eerste voorbeeld.

### <a name="for-both-scenarios"></a>Voor beide scenario 's

Voor een eigenschap met een groot aantal van de mogelijke waarden is het raadzaam te verzenden als afzonderlijke waarden binnen één kolom in plaats van het maken van een nieuwe kolom voor elke waarde. Uit de vorige twee voorbeelden:

  - Enkele eigenschappen hebben verschillende waarden in het eerste voorbeeld, dus is het geschikt is voor elk een afzonderlijke eigenschap maken.
  - De metingen worden niet in het tweede voorbeeld wordt opgegeven als afzonderlijke eigenschappen. In feite in plaats daarvan een matrix met waarden of maatregelen op grond van een algemene reeks-eigenschap. De nieuwe sleutel **tagId** wordt verzonden, die wordt gemaakt met de nieuwe kolom **series.tagId** in de platte tabel. De eigenschappen van nieuwe **type** en **eenheid** worden gemaakt met behulp van referentiegegevens zodat de eigenschap niet is bereikt.

## <a name="next-steps"></a>Volgende stappen

- Lezen [Azure Time Series Insights-querysyntaxis](/rest/api/time-series-insights/ga-query-syntax) voor meer informatie over de query-syntaxis voor de Time Series Insights data access-REST-API.
- Informatie over [hoe vorm gebeurtenissen](./time-series-insights-send-events.md).
