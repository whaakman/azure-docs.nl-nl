---
title: Aanbevolen procedures voor het vormgeven van JSON in Azure Time Series Insights-query's | Microsoft Docs
description: Leer hoe u uw Azure Time Series Insights-query-efficiëntie te verbeteren.
services: time-series-insights
author: ashannon7
manager: cshankar
ms.service: time-series-insights
ms.topic: article
ms.date: 05/09/2019
ms.author: anshan
ms.custom: seodec18
ms.openlocfilehash: 535fe9a7920db89e434b4cc1b66aa38bf72a7829
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65790065"
---
# <a name="how-to-shape-json-to-maximize-query-performance"></a>Hoe u kunt vormgeven JSON naar het maximaliseren van de prestaties van query 's 

In dit artikel bevat richtlijnen voor het vormgeven van JSON naar de efficiëntie van uw Azure Time Series Insights-query's te maximaliseren.

## <a name="video"></a>Video

### <a name="learn-best-practices-for-shaping-json-to-meet-your-storage-needsbr"></a>Informatie over aanbevolen procedures voor het vormgeven van JSON om te voldoen aan uw opslagbehoeften.</br>

> [!VIDEO https://www.youtube.com/embed/b2BD5hwbg5I]

## <a name="best-practices"></a>Aanbevolen procedures

Het is belangrijk om na te denken over hoe u gebeurtenissen naar de Time Series Insights verzenden. Dat wil zeggen moet u altijd:

1. gegevens zo efficiënt mogelijk via het netwerk verzenden.
1. Zorg ervoor dat uw gegevens worden opgeslagen op een begrijpelijke manier kunt u aggregatiefuncties die geschikt is voor uw scenario.
1. Zorg ervoor dat u geen limieten voor Time Series Insights-eigenschap maximum van bereikt:
   - 600 eigenschappen (kolommen) voor S1-omgevingen.
   - 800 eigenschappen (kolommen) voor S2-omgevingen.

De volgende richtlijnen kunt de best mogelijke query-prestaties:

1. Gebruik geen dynamische eigenschappen, zoals een label-ID als de naam, zoals het draagt bij aan de eigenschappen van de maximale limiet hebt bereikt.
1. Geen overbodige eigenschappen niet verzenden. Als de eigenschap van een query is niet vereist, is het raadzaam om niet te verzenden en te voorkomen dat opslag beperkingen.
1. Gebruik [verwijzen naar gegevens](time-series-insights-add-reference-data-set.md)om te voorkomen dat statische gegevens verzenden via het netwerk.
1. Delen tussen meerdere gebeurtenissen, om gegevens te verzenden via het netwerk efficiënter dimensie-eigenschappen.
1. Gebruik geen grondige matrix nesten. Time Series Insights biedt ondersteuning voor maximaal twee niveaus van geneste matrices die objecten bevatten. Time Series Insights effent matrices in de berichten in meerdere gebeurtenissen met eigenschap-waardeparen.
1. Als er slechts een paar metingen bestaan voor alle of de meeste gebeurtenissen, is het beter om het verzenden van deze metingen als afzonderlijke eigenschappen binnen hetzelfde object. Afzonderlijk naar hen kan vermindert het aantal gebeurtenissen, en query's beter als minder gebeurtenissen moeten worden verwerkt. Wanneer er meerdere metingen, minimaliseert ze worden verzonden als waarden in één eigenschap, de mogelijkheid van de eigenschap maximum limiet hebt bereikt.

## <a name="example-overview"></a>Voorbeeld-overzicht

De volgende twee voorbeelden worden verzonden gebeurtenissen, markeert u de bovenstaande aanbevelingen. De volgende elk voorbeeld ziet u hoe de aanbevelingen zijn toegepast.

De voorbeelden zijn gebaseerd op een scenario waar meerdere apparaten metingen of signalen verzenden. Metingen of signalen mogelijk Flow snelheid, Engine oliedruk, temperatuur en vochtigheid. In het eerste voorbeeld vindt enkele metingen voor alle apparaten. Er zijn veel apparaten in het tweede voorbeeld, en elk veel unieke metingen verzendt.

## <a name="scenario-one-only-a-few-measurements-exist"></a>Scenario 1: slechts een paar metingen bestaat

> [!TIP]
> **Aanbeveling**: elke meting/signaal verzenden als een afzonderlijke eigenschap/kolom.

In het volgende voorbeeld wordt is er een enkel bericht van de IoT Hub waar de buitenste matrix een gedeelde sectie van de algemene dimensiewaarden bevat. De buitenste matrix wordt referentiegegevens worden gebruikt om de efficiëntie van het bericht. Referentiegegevens bevat metagegevens van apparaten, die niet met elke gebeurtenis wordt gewijzigd, maar biedt nuttige eigenschappen voor data-analyse. Zowel algemene dimensiewaarden batchverwerking en die gebruikmaakt van referentiegegevens, worden opgeslagen op bytes verzonden via de kabel, waardoor het bericht efficiënter.

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

* Tabel met referentiegegevens (sleuteleigenschap is **deviceId**):

   | deviceId | messageId | deviceLocation |
   | --- | --- | --- |
   | FXXX | REGEL\_GEGEVENS | EU |
   | BJJJ | REGEL\_GEGEVENS | VS |

* Time Series Insights Gebeurtenistabel (na afvlakken):

   | deviceId | messageId | deviceLocation | timestamp | reeks. Stroom tarief ft3/s | reeks. Engine oliedruk psi |
   | --- | --- | --- | --- | --- | --- |
   | FXXX | REGEL\_GEGEVENS | EU | 2018-01-17T01:17:00Z | 1.0172575712203979 | 34,7 |
   | FXXX | REGEL\_GEGEVENS | EU | 2018-01-17T01:17:00Z | 2.445906400680542 | 49,2 |
   | BJJJ | REGEL\_GEGEVENS | VS | 2018-01-17T01:18:00Z | 0.58015072345733643 | 22.2 |

Hierboven:

- De **deviceId** kolom fungeert als de kop van de kolom voor de verschillende apparaten in een hele vloot. Er wordt geprobeerd om de apparaat-id-waarde zou een eigen eigenschapsnaam hebben beperkte totaal aantal apparaten 595 (S1-omgevingen) of 795 (S2-omgevingen), met de vijf kolommen.

- Eigenschappen van onnodige worden vermeden, bijvoorbeeld merk en model van gegevens, enzovoort. Omdat deze wordt niet in de toekomst worden opgevraagd, kunt zodat ze betere netwerk- en efficiëntie van opslag.

- Referentiegegevens wordt gebruikt voor het verminderen van het aantal bytes dat wordt overgedragen via het netwerk. Twee kenmerken **messageId** en **deviceLocation**, zijn gekoppeld met behulp van de sleuteleigenschap **deviceId**. Deze gegevens is samengevoegd met de telemetrische gegevens gelijktijdig met inkomend verkeer, en vervolgens opgeslagen in Time Series Insights voor het uitvoeren van query's.

- Twee lagen van geneste worden gebruikt, is de maximale hoeveelheid nesten ondersteund door de Time Series Insights. Het is essentieel om te voorkomen dat diep geneste matrices.

- Metingen worden verzonden als afzonderlijke eigenschappen binnen hetzelfde object, omdat er enkele metingen. Hier **reeks. Flow tarief psi** en **reeks. Oliedruk ft3/s-engine** unieke kolommen zijn.

## <a name="scenario-two-several-measures-exist"></a>Scenario 2: meerdere metingen bestaat

> [!TIP]
> **Aanbeveling:** metingen als 'type', 'eenheid', '' waarde '' tuples verzenden.

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

* Verwijzen naar gegevens (sleuteleigenschappen zijn **deviceId** en **series.tagId**):

   | deviceId | series.tagId | messageId | deviceLocation | type | eenheid |
   | --- | --- | --- | --- | --- | --- |
   | FXXX | pumpRate | REGEL\_GEGEVENS | EU | Stroom | ft3/s |
   | FXXX | oilPressure | REGEL\_GEGEVENS | EU | Engine oliedruk | psi |
   | BJJJ | pumpRate | REGEL\_GEGEVENS | VS | Stroom | ft3/s |
   | BJJJ | oilPressure | REGEL\_GEGEVENS | VS | Engine oliedruk | psi |

* Time Series Insights Gebeurtenistabel (na afvlakken):

   | deviceId | series.tagId | messageId | deviceLocation | type | eenheid | timestamp | series.value |
   | --- | --- | --- | --- | --- | --- | --- | --- |
   | FXXX | pumpRate | REGEL\_GEGEVENS | EU | Stroom | ft3/s | 2018-01-17T01:17:00Z | 1.0172575712203979 | 
   | FXXX | oilPressure | REGEL\_GEGEVENS | EU | Engine oliedruk | psi | 2018-01-17T01:17:00Z | 34,7 |
   | FXXX | pumpRate | REGEL\_GEGEVENS | EU | Stroom | ft3/s | 2018-01-17T01:17:00Z | 2.445906400680542 | 
   | FXXX | oilPressure | REGEL\_GEGEVENS | EU | Engine oliedruk | Psi | 2018-01-17T01:17:00Z | 49,2 |
   | BJJJ | pumpRate | REGEL\_GEGEVENS | VS | Stroom | ft3/s | 2018-01-17T01:18:00Z | 0.58015072345733643 |
   | BJJJ | oilPressure | REGEL\_GEGEVENS | VS | Engine oliedruk | psi | 2018-01-17T01:18:00Z | 22.2 |

Hierboven:

- De kolommen **deviceId** en **series.tagId** fungeren als de kolomkoppen bevat voor de verschillende apparaten en labels in een hele vloot. Met behulp van elk als een eigen kenmerk zou hebben beperkte als de query voor het 594 (S1-omgevingen) of 794 (S2-omgevingen) totaal aantal apparaten met de zes kolommen.

- Eigenschappen van onnodige zijn vermeden, om de reden dat wordt vermeld in het eerste voorbeeld.

- referentiegegevens wordt gebruikt om te beperken van het aantal bytes dat via het netwerk wordt overgedragen door de introductie van **deviceId**, voor een unieke combinatie van een **messageId** en **deviceLocation**. Een samengestelde sleutel wordt gebruikt, **series.tagId**, voor de unieke combinatie van **type** en **unit.**. De samengestelde sleutel kan de **deviceId** en **series.tagId** paar moet worden gebruikt om te verwijzen naar vier waarden: **messageId, deviceLocation, typt,** en **eenheid** . Deze gegevens is samengevoegd met de telemetrische gegevens gelijktijdig met inkomend verkeer, en vervolgens opgeslagen in Time Series Insights voor het uitvoeren van query's.

- twee lagen van geneste worden gebruikt om de reden dat wordt vermeld in het eerste voorbeeld.

### <a name="for-both-scenarios"></a>Voor beide scenario 's

Als u een eigenschap met een groot aantal mogelijke waarden hebt, is het raadzaam te verzenden als afzonderlijke waarden binnen één kolom, in plaats van het maken van een nieuwe kolom voor elke waarde. Uit de vorige twee voorbeelden:

  - In het eerste voorbeeld zijn er enkele eigenschappen met verschillende waarden, zodat deze geschikt is voor elk een afzonderlijke eigenschap maken.
  - Echter, in het tweede voorbeeld ziet u dat de maatregelen die niet zijn opgegeven als afzonderlijke eigenschappen, maar in plaats daarvan een matrix met waarden/maatregelen op grond van een algemene reeks-eigenschap. Een nieuwe sleutel wordt verzonden, **tagId** , deze toepassing maakt een nieuwe kolom **series.tagId** in de platte tabel. Eigenschappen van nieuw zijn gemaakt, **type** en **eenheid**, met behulp van referentiegegevens, dus voorkomen dat de eigenschap limiet wordt bereikt.

## <a name="next-steps"></a>Volgende stappen

- Lezen [Azure Time Series Insights-querysyntaxis](/rest/api/time-series-insights/ga-query-syntax) voor meer informatie over de query-syntaxis voor de Time Series Insights data access-REST-API.

- Informatie over [hoe vorm gebeurtenissen](./time-series-insights-send-events.md).
