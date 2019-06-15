---
title: Gebeurtenissen met de Azure Time Series Insights Preview vorm | Microsoft Docs
description: Lees hoe u gebeurtenissen met de Azure Time Series Insights Preview vorm.
author: ashannon7
ms.author: dpalled
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 04/30/2019
ms.custom: seodec18
ms.openlocfilehash: f0e1a79073596dcabfacb7163e12b33bb582b7c3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66238918"
---
# <a name="shape-events-with-azure-time-series-insights-preview"></a>Shapegebeurtenissen met de Azure Time Series Insights Preview

Dit artikel helpt u uw JSON-bestand voor een maximale de efficiency van uw Azure Time Series Insights Preview query's vormen.

## <a name="best-practices"></a>Aanbevolen procedures

Denk na over hoe u gebeurtenissen naar de Time Series Insights Preview verzenden. Dat wil zeggen moet u altijd:

* gegevens zo efficiënt mogelijk via het netwerk verzenden.
* Uw gegevens op een manier waarmee u meer naar behoren aggregeren voor uw scenario Store.

Voor de best mogelijke query's, het volgende doen:

* Geen overbodige eigenschappen niet verzenden. Time Series Insights Preview worden er kosten op uw gebruik. Het is raadzaam opslaan en verwerken van de gegevens die u hebt een query.
* Exemplaarvelden gebruiken voor statische gegevens. Met deze procedure kunt u statische gegevens verzenden via het netwerk voorkomen. Exemplaarvelden, een onderdeel van de time series-model, werken net als verwijzen naar gegevens in de Time Series Insights algemeen beschikbare service. Zie voor meer informatie over instantievelden [Time Series modellen](./time-series-insights-update-tsm.md).
* Dimensie-eigenschappen tussen twee of meer gebeurtenissen delen. Met deze procedure kunt u gegevens efficiënter via het netwerk verzenden.
* Gebruik geen grondige matrix nesten. Time Series Insights Preview biedt ondersteuning voor maximaal twee niveaus van geneste matrices die objecten bevatten. Time Series Insights Preview effent matrices in berichten in meerdere gebeurtenissen met eigenschap-waardeparen.
* Als er slechts een paar metingen bestaan voor alle of de meeste gebeurtenissen, is het beter om het verzenden van deze metingen als afzonderlijke eigenschappen binnen hetzelfde object. Afzonderlijk naar hen vermindert het aantal gebeurtenissen en prestaties van query's kan verbeteren omdat minder gebeurtenissen moeten worden verwerkt.

## <a name="example"></a>Voorbeeld

Het volgende voorbeeld is gebaseerd op een scenario waarbij twee of meer apparaten metingen of signalen verzenden. De metingen of signalen kunnen worden *Flow tarief*, *Engine oliedruk*, *temperatuur*, en *vochtigheid*.

In het volgende voorbeeld is er een enkel Azure IoT Hub-bericht wanneer de buitenste matrix een gedeelde sectie van de algemene dimensiewaarden bevat. Time Series-Instantiegegevens door de buitenste matrix wordt gebruikt om de efficiëntie van het bericht. Time Series-exemplaar bevat metagegevens van apparaten, die met elke gebeurtenis niet wijzigen, maar biedt nuttige eigenschappen voor data-analyse. Als u wilt opslaan op via de kabel verzonden bytes en het bericht efficiënter te maken, moet u rekening houden met algemene dimensiewaarden batchverwerking en die gebruikmaakt van de metagegevens van het exemplaar van Time Series.

### <a name="example-json-payload"></a>Voorbeeld van JSON-nettolading

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
                "Flow Rate ft3/s": 0.58015072345733643,
                "Engine Oil Pressure psi ": 22.2
            }
        ]
    }
]
```

### <a name="time-series-instance"></a>Time Series-exemplaar 
> [!NOTE]
> De Time Series-ID is *deviceId*.

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

Time Series Insights Preview lid wordt van een tabel (na afvlakken) tijdens het uitvoeren van query's. De tabel bevat aanvullende kolommen, zoals **Type**. Het volgende voorbeeld laat zien hoe u [vorm](./time-series-insights-send-events.md#json) uw telemetriegegevens.

| deviceId  | Type | L1 | L2 | tijdstempel | reeks. Stroom tarief ft3/s | reeks. Engine oliedruk psi |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| `FXXX` | Default_Type | SIMULATOR | Accu System | 2018-01-17T01:17:00Z |   1.0172575712203979 |    34,7 |
| `FXXX` | Default_Type | SIMULATOR |   Accu System |    2018-01-17T01:17:00Z | 2.445906400680542 |  49,2 |
| `FYYY` | ALGEMENE LINE_DATA | SIMULATOR |    Accu System |    2018-01-17T01:18:00Z | 0.58015072345733643 |    22.2 |

In het voorgaande voorbeeld, houd rekening met de volgende punten:

* Statische eigenschappen worden opgeslagen in de Preview van tijd Series Insights om te worden verzonden via het netwerk optimaliseren.
* Time Series Insights-Preview-gegevens is gekoppeld op het moment dat de query door de Time Series-ID die gedefinieerd in het exemplaar.
* Twee lagen van geneste worden gebruikt, is het meest die wordt ondersteund door de Time Series Insights Preview. Het is essentieel om te voorkomen dat diep geneste matrices.
* Omdat er enkele metingen, wordt deze zijn verzonden als afzonderlijke eigenschappen binnen hetzelfde object. In het voorbeeld **reeks. Flow tarief psi**, **reeks. Engine oliedruk psi**, en **reeks. Flow tarief ft3/s** unieke kolommen zijn.

>[!IMPORTANT]
> Exemplaarvelden worden niet opgeslagen met telemetrie. Ze zijn opgeslagen met metagegevens in de **Tijdreeksmodel**.
> De voorgaande tabel vertegenwoordigt de queryweergave.

## <a name="next-steps"></a>Volgende stappen

- Om deze richtlijnen in de praktijk, Zie [Azure Time Series Insights Preview querysyntaxis](./time-series-insights-query-data-csharp.md). Leert u dat meer informatie over de query-syntaxis voor de gegevens van de Time Series Insights Preview REST-API toegang.
- Zie voor meer informatie over ondersteunde JSON-vormen, [ondersteund JSON-vormen](./time-series-insights-send-events.md#json).
