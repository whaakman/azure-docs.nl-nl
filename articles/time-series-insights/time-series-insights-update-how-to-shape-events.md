---
title: Hoe vorm gebeurtenissen met de Azure Time Series Insights (preview) | Microsoft Docs
description: Informatie over hoe u kunt vormgeven van gebeurtenissen met de Azure Time Series Insights (preview)
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 11/30/2018
ms.openlocfilehash: edc1dac05a8ab4281eee3ee0eb4c5e6b7571b404
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52856092"
---
# <a name="shaping-events-with-azure-time-series-insights-preview"></a>Shaping gebeurtenissen met de Azure Time Series Insights (preview)

In dit artikel bevat richtlijnen voor het vormgeven van JSON, als u wilt maximaliseren de efficiëntie van u bent de Azure Time Series Insights (preview)-query's.

## <a name="best-practices"></a>Aanbevolen procedures

> [!NOTE]
> Voor de Azure TSI (preview), de grenzen van de eigenschap 600-800 voor S1/S2 niet van toepassing.

Het is belangrijk om na te denken over hoe u gebeurtenissen naar Azure TSI verzenden. Dat wil zeggen moet u altijd:

1. gegevens zo efficiënt mogelijk via het netwerk verzenden.
1. Zorg ervoor dat uw gegevens worden opgeslagen op een begrijpelijke manier kunt u aggregatiefuncties die geschikt is voor uw scenario.

De volgende richtlijnen kunt de best mogelijke query-prestaties:

1. Geen overbodige eigenschappen niet verzenden. TSI (preview) wordt u gefactureerd op uw gebruik en het is een aanbevolen procedure om op te slaan en de gegevens verwerken die u een query zullen uitvoeren.
1. Instantievelden voor statische gegevens gebruiken om te voorkomen dat statische gegevens verzenden via het netwerk. Exemplaarvelden, een onderdeel van de time series-model, werken zoals verwijzen naar gegevens in de algemeen beschikbare TSI-service. Lees voor meer informatie over het exemplaarveld [Time Series modellen](./time-series-insights-update-tsm.md).
1. Delen tussen meerdere gebeurtenissen, om gegevens te verzenden via het netwerk efficiënter dimensie-eigenschappen.
1. Gebruik geen grondige matrix nesten. TSI biedt ondersteuning voor maximaal twee niveaus van geneste matrices die objecten bevatten. TSI effent matrices in de berichten in meerdere gebeurtenissen met eigenschap-waardeparen.
1. Als er slechts een paar metingen bestaan voor alle of de meeste gebeurtenissen, is het beter om het verzenden van deze metingen als afzonderlijke eigenschappen binnen hetzelfde object. Afzonderlijk naar hen kan vermindert het aantal gebeurtenissen, en query's beter als minder gebeurtenissen moeten worden verwerkt.

## <a name="example"></a>Voorbeeld

Het voorbeeld is gebaseerd op een scenario waar meerdere apparaten metingen of signalen verzenden. Metingen of signalen konden worden **Flow tarief**, **Engine oliedruk**, **temperatuur**, en **vochtigheid**.

In het volgende voorbeeld wordt is er een enkel bericht van de IoT Hub waar de buitenste matrix een gedeelde sectie van de algemene dimensiewaarden bevat. Time Series-Instantiegegevens door de buitenste matrix wordt gebruikt om de efficiëntie van het bericht. Time Series-exemplaar bevat metagegevens van apparaten, die niet met elke gebeurtenis wordt gewijzigd, maar biedt nuttige eigenschappen voor data-analyse. Algemene dimensiewaarden batchverwerking, zowel die gebruikmaakt van de metagegevens van de Time Series-exemplaar, worden opgeslagen op bytes verzonden via de kabel, waardoor het bericht efficiënter.

Voorbeeld van JSON-nettolading:

```JSON
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

Time Series-exemplaar (Opmerking: de **Time Series-ID** is *deviceId*):

```JSON
{
    "timeSeriesId": [
      "FXXX"
    ],
    "typeId": "17150182-daf3-449d-adaf-69c5a7517546",
    "hierarchyIds": [
      "b888bb7f-06f0-4bfd-95c3-fac6032fa4da"
    ],
    "description": null,
    "instanceFields": {
      "L1": "REVOLT SIMULATOR",
      "L2": "Battery System",
    }
  },
  {
    "timeSeriesId": [
      "FYYY"
    ],
    "typeId": "17150182-daf3-449d-adaf-69c5a7517546",
    "hierarchyIds": [
      "b888bb7f-06f0-4bfd-95c3-fac6032fa4da"
    ],
    "description": null,
    "instanceFields": {
      "L1": "COMMON SIMULATOR",
      "L2": "Battery System",
    }
  },
```

TSI lid zijn van tabel (na afvlakken) tijdens het uitvoeren van query's. De tabel bevat meer kolommen zoals Type. In dit voorbeeld ziet u hoe u uw telemetriegegevens kunt vormgeven:

| deviceId  | Type | L1 | L2 | tijdstempel | reeks. Stroom tarief ft3/s | reeks. Engine oliedruk psi |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| `FXXX` | Default_Type | REVOLT SIMULATOR | Accu System | 2018-01-17T01:17:00Z |    1.0172575712203979 |    34,7 |
| `FXXX` | LINE_DATA REVOLT | SIMULATOR |    Accu System |    2018-01-17T01:17:00Z | 2.445906400680542 |  49,2 |
| `FYYY` | ALGEMENE LINE_DATA | SIMULATOR |    Accu System |    2018-01-17T01:18:00Z | 0.58015072345733643 |    22.2 |

Let op het volgende in het vorige voorbeeld:

* Statische eigenschappen worden opgeslagen als TSI om te worden verzonden via het netwerk optimaliseren.
* TSI-gegevens op het moment dat de query is gekoppeld met behulp van de *timeSeriesId* gedefinieerd in het exemplaar.
* Twee lagen van geneste worden gebruikt, is de maximale hoeveelheid nesten ondersteund door TSI. Het is essentieel om te voorkomen dat diep geneste matrices.
* Metingen worden verzonden als afzonderlijke eigenschappen binnen hetzelfde object, omdat er enkele metingen. Hier **reeks. Flow tarief psi**, reeks **Engine oliedruk**, en **ft3/s** unieke kolommen zijn.

>[!IMPORTANT]
> Exemplaarvelden worden niet opgeslagen met telemetrie, ze worden opgeslagen met metagegevens in de **Tijdreeksmodel**.
> De bovenstaande tabel vertegenwoordigt de queryweergave.

## <a name="next-steps"></a>Volgende stappen

Om deze richtlijnen in de praktijk, Zie [Azure TSI-querysyntaxis](./time-series-insights-query-data-csharp.md) voor meer informatie over de query-syntaxis voor de TSI-data access-REST-API.
