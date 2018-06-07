---
title: Aanbevolen procedures voor het vormgeven van JSON in Azure Time Series Insights query's.
description: Leer uw query Time Series Insights efficiëntie te verbeteren.
services: time-series-insights
author: ashannon7
manager: timlt
ms.service: time-series-insights
ms.topic: article
ms.date: 05/24/2018
ms.author: bryanla
ms.openlocfilehash: 29919a3ce8c18982c88f7f0e6bbd774c612e9c44
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34660832"
---
# <a name="how-to-shape-json-to-maximize-query-performance"></a>Hoe vorm JSON queryprestaties maximaliseren 

In dit artikel bevat richtlijnen voor het vormgeven van JSON maximaliseren van de efficiëntie van uw Azure Time Series Insights (TSI)-query's.

## <a name="best-practices"></a>Aanbevolen procedures

Het is belangrijk bij het nadenken over hoe u kunt gebeurtenissen naar Time Series Insights verzenden. Dat wil zeggen moet u altijd:

1. zo efficiënt mogelijk gegevens verzenden via het netwerk.
2. Zorg ervoor dat uw gegevens worden opgeslagen op een manier die u kunt uitvoeren aggregaties geschikt is voor uw scenario.
3. Zorg ervoor dat u niet van TSI eigenschap maximum grenzen van bereikt
   - 600 eigenschappen (kolommen) voor S1 omgevingen.
   - 800 eigenschappen (kolommen) voor S2 omgevingen.

De volgende richtlijnen voor gezorgd de best mogelijke queryprestaties:

1. Gebruik geen dynamische eigenschappen, zoals een label-ID als de eigenschapsnaam van de, zoals dit draagt bij aan kunt u door de eigenschappen van de maximale limiet.
2. Geen verzenden onnodige eigenschappen. Als de eigenschap van een query is niet vereist, is het raadzaam niet te verzenden en te voorkomen dat opslag beperkingen.
3. Gebruik [referentiegegevens](time-series-insights-add-reference-data-set.md), om te voorkomen dat statische gegevens verzenden via het netwerk.
4. Dimensie-eigenschappen tussen meerdere gebeurtenissen, om gegevens te verzenden via het netwerk efficiënter delen.
5. Gebruik geen grondige matrix nesten. TSI ondersteunt maximaal twee niveaus van geneste matrices die objecten bevatten. TSI worden samengevoegd matrices in de berichten in meerdere gebeurtenissen met de eigenschap / waardeparen.
6. Als er slechts een paar maatregelen bestaan voor alle of de meeste gebeurtenissen, is het beter om het verzenden van deze maatregelen als afzonderlijke eigenschappen binnen hetzelfde object. Ze afzonderlijk sturen vermindert het aantal gebeurtenissen en kan resulteren in query's meer zodat als minder gebeurtenissen moeten worden verwerkt. Wanneer er verscheidene maatregelen, minimaliseert ze worden verzonden als de waarden in één eigenschap de kans bestaat dat kunt u door de eigenschap maximum limiet.

## <a name="examples"></a>Voorbeelden

De volgende twee voorbeelden laten zien verzenden gebeurtenissen, selecteer de bovenstaande aanbevelingen. Na elke voorbeeld ziet u hoe de aanbevelingen zijn toegepast.

De voorbeelden zijn gebaseerd op een scenario waarbij meerdere apparaten metingen of signalen verzenden. Metingen of signalen kunnen worden stromen snelheid, Engine oliedruk, temperatuur en vochtigheid. In het eerste voorbeeld, moet u er een paar metingen zijn op alle apparaten. Er zijn veel apparaten in het tweede voorbeeld en elk veel metingen voor unieke verzendt.

### <a name="scenario-only-a-few-measurementssignals-exist"></a>Scenario: alleen enkele metingen/signalen bestaan

**Aanbeveling:** elke meting als een afzonderlijke eigenschappenkolom verzenden.

In het volgende voorbeeld wordt een enkele IoT Hub bericht, waarbij de buitenste matrix een gedeelde sectie van algemene dimensie bevat. De buitenste matrix wordt referentiegegevens gebruikt om de efficiëntie van het bericht. Referentiegegevens bevat metagegevens van apparaten die niet wijzigen met elke gebeurtenis, maar biedt nuttige eigenschappen voor gegevensanalyse. Zowel algemene dimensiewaarden batchverwerking en die gebruikmaakt van referentiegegevens, slaat op via de kabel verzonden bytes waardoor het bericht efficiënter.

Voorbeeld van de JSON-nettolading:

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
                "Flow Rate psi": 0.58015072345733643,
                "Engine Oil Pressure psi ": 22.2
            }
        ]
    }
]
```

Tabel met referentiegegevens (deviceId sleuteleigenschap is):

| deviceId | MessageId | deviceLocation |
| --- | --- | --- |
| FXXX | REGEL\_GEGEVENS | EU |
| BJJJ | REGEL\_GEGEVENS | VS |

Time Series Insights Gebeurtenistabel (na plat):

| deviceId | MessageId | deviceLocation | tijdstempel | reeks. Stroom snelheid ft3/s | reeks. Engine oliedruk psi |
| --- | --- | --- | --- | --- | --- |
| FXXX | REGEL\_GEGEVENS | EU | 2018-01-17T01:17:00Z | 1.0172575712203979 | 34,7 |
| FXXX | REGEL\_GEGEVENS | EU | 2018-01-17T01:17:00Z | 2.445906400680542 | 49,2 |
| BJJJ | REGEL\_GEGEVENS | VS | 2018-01-17T01:18:00Z | 0.58015072345733643 | 22.2 |

Houd rekening met het volgende in het vorige voorbeeld:

- De **deviceId** kolom fungeert als de kolomkop voor de verschillende apparaten in een wagenpark. Apparaat-id-waarde wilt zou eigen eigenschapsnaam hebben beperkte totaal aantal apparaten 595 (S1 omgevingen) of 795 (S2 omgevingen), met de vijf kolommen.

- Overbodige eigenschappen worden vermeden, bijvoorbeeld het merk en model informatie, enzovoort. Aangezien ze won't in de toekomst worden opgevraagd, schakelt want ze niet meer betere netwerk en opslag efficiëntie.

- Referentiegegevens wordt gebruikt voor het aantal bytes dat is overgedragen via het netwerk te verminderen. Twee kenmerken **messageId** en **deviceLocation** , zijn gekoppeld met behulp van de sleuteleigenschap **deviceId**. Deze gegevens is samengevoegd met de telemetriegegevens op moment van toegangsroutes en vervolgens worden opgeslagen in TSI voor het uitvoeren van query's.

- Twee lagen geneste worden gebruikt, is de maximale hoeveelheid ondersteund door TSI nesten. Het is essentieel om te voorkomen dat diep geneste matrices.

- Metingen worden verzonden als afzonderlijke eigenschappen binnen hetzelfde object, aangezien er enkele metingen zijn. Hier **reeks. Frequentie psi stromen** en **reeks. Oliedruk ft3/s in de engine** unieke kolommen zijn.

### <a name="scenario-several-measures-exist"></a>Scenario: verscheidene maatregelen bestaan

**Aanbeveling:** metingen verzenden als 'type', 'eenheid', '' waarde '' tuples.

Voorbeeld van de JSON-nettolading:

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

Referentiegegevens (sleuteleigenschappen zijn apparaat-id en series.tagId):

| deviceId | series.tagId | MessageId | deviceLocation | type | eenheid |
| --- | --- | --- | --- | --- | --- |
| FXXX | pumpRate | REGEL\_GEGEVENS | EU | Stroom | ft3/s |
| FXXX | oilPressure | REGEL\_GEGEVENS | EU | Engine oliedruk | psi |
| BJJJ | pumpRate | REGEL\_GEGEVENS | VS | Stroom | ft3/s |
| BJJJ | oilPressure | REGEL\_GEGEVENS | VS | Engine oliedruk | psi |

Time Series Insights Gebeurtenistabel (na plat):

| deviceId | series.tagId | MessageId | deviceLocation | type | eenheid | tijdstempel | Series.Value |
| --- | --- | --- | --- | --- | --- | --- | --- |
| FXXX | pumpRate | REGEL\_GEGEVENS | EU | Stroom | ft3/s | 2018-01-17T01:17:00Z | 1.0172575712203979 |
| FXXX | oilPressure | REGEL\_GEGEVENS | EU | Engine oliedruk | psi | 2018-01-17T01:17:00Z | 34,7 |
| FXXX | pumpRate | REGEL\_GEGEVENS | EU | Stroom | ft3/s | 2018-01-17T01:17:00Z | 2.445906400680542 |
| FXXX | oilPressure | REGEL\_GEGEVENS | EU | Engine oliedruk | PSI | 2018-01-17T01:17:00Z | 49,2 |
| BJJJ | pumpRate | REGEL\_GEGEVENS | VS | Stroom | ft3/s | 2018-01-17T01:18:00Z | 0.58015072345733643 |
| BJJJ | oilPressure | REGEL\_GEGEVENS | VS | Engine oliedruk | psi | 2018-01-17T01:18:00Z | 22.2 |

Houd rekening met de volgende in het vorige voorbeeld en vergelijkbaar met het eerste voorbeeld:

- kolommen **deviceId** en **series.tagId** fungeren als de kolomkoppen voor de verschillende apparaten en labels in een wagenpark. Met elk als een eigen kenmerk zou beperkt de query in 594 (S1 omgevingen) of 794 (S2 omgevingen) totaal aantal apparaten met de zes kolommen.

- overbodige eigenschappen zijn om de reden dat het eerste voorbeeld hierboven vermeden.

- referentiegegevens worden gebruikt om te Verminder het aantal bytes dat is overgedragen via het netwerk door de introductie **deviceId**, naar een uniek paar **messageId** en **deviceLocation**. Er wordt een samengestelde sleutel gebruikt, **series.tagId**, voor de unieke paar **type** en **eenheid**. De samengestelde sleutel kan de **deviceId** en **series.tagId** paar moet worden gebruikt om te verwijzen naar vier waarden: **messageId, deviceLocation, typt,** en **eenheid** . Deze gegevens is samengevoegd met de telemetriegegevens op moment van toegangsroutes en vervolgens worden opgeslagen in TSI voor het uitvoeren van query's.

- twee lagen geneste worden gebruikt voor de reden genoemd in het eerste voorbeeld.

### <a name="for-both-scenarios"></a>Voor beide scenario 's

Als u een eigenschap met een groot aantal mogelijke waarden hebt, is het raadzaam te verzenden als afzonderlijke waarden binnen één kolom, in plaats van het maken van een nieuwe kolom voor elke waarde. Van de vorige twee voorbeelden:
  - Er zijn enkele eigenschappen met verschillende waarden, zodat hij geschikt is om elk een afzonderlijke eigenschap in het eerste voorbeeld. 
  - Echter, in het tweede voorbeeld ziet u de metingen zijn niet opgegeven als afzonderlijke eigenschappen, maar in plaats daarvan een matrix met waarden/maatregelen op grond van een algemene reeks-eigenschap. Een nieuwe sleutel wordt verzonden, **tagId** , waarbij automatisch een nieuwe kolom **series.tagId** in de platte tabel. Eigenschappen van nieuwe worden gemaakt, **type** en **eenheid**, met behulp van referentiegegevens, dus voorkomen dat de eigenschap limiet wordt bereikt.

## <a name="next-steps"></a>Volgende stappen

Om deze richtlijnen in de praktijk, Zie [Azure Time Series Insights querysyntaxis](/rest/api/time-series-insights/time-series-insights-reference-query-syntax) voor meer informatie over de syntaxis van de query voor de gegevenstoegang TSI REST-API.