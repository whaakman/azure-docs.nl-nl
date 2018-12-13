---
title: Azure tijd Series Insights Preview vormgeven events - gebeurtenissen met Azure Time Series Insights Preview vorm | Microsoft Docs
description: Begrijpen hoe u kunt vormgeven gebeurtenissen Azure Time Series Insights Preview.
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/03/2018
ms.custom: seodec18
ms.openlocfilehash: b0ce3b5bbd58c594147f64de9f06f248f4a87fc3
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53269070"
---
# <a name="shape-events-with-azure-time-series-insights-preview"></a>Shapegebeurtenissen met de Azure Time Series Insights Preview

Dit artikel helpt u uw JSON-bestand voor een maximale de efficiency van uw Azure Time Series Insights Preview query's vormen.

## <a name="best-practices"></a>Aanbevolen procedures

Het is belangrijk om na te denken over hoe u gebeurtenissen naar de Time Series Insights Preview verzenden. Dat wil zeggen moet u altijd:

* gegevens zo efficiënt mogelijk via het netwerk verzenden.
* Uw gegevens op een manier waarmee u meer naar behoren aggregeren voor uw scenario Store.

Voor de best mogelijke query's, het volgende doen:

* Geen overbodige eigenschappen niet verzenden. Time Series Insights Preview worden er kosten op uw gebruik. Het is raadzaam opslaan en verwerken van de gegevens die u hebt een query.
* Exemplaarvelden gebruiken voor statische gegevens. Met deze procedure kunt u statische gegevens verzenden via het netwerk voorkomen. Exemplaarvelden, een onderdeel van de time series-model, werken zoals verwijzen naar gegevens in de Time Series Insights GA-service. Zie voor meer informatie over instantievelden [Time Series modellen](./time-series-insights-update-tsm.md).
* Dimensie-eigenschappen tussen twee of meer gebeurtenissen delen. Met deze procedure kunt u gegevens efficiënter via het netwerk verzenden.
* Gebruik geen grondige matrix nesten. Time Series Insights Preview biedt ondersteuning voor maximaal twee niveaus van geneste matrices met objecten. Time Series Insights Preview effent matrices in berichten in meerdere gebeurtenissen met eigenschap-waardeparen.
* Als er slechts een paar metingen bestaan voor alle of de meeste gebeurtenissen, is het beter om het verzenden van deze metingen als afzonderlijke eigenschappen binnen hetzelfde object. Afzonderlijk naar hen vermindert het aantal gebeurtenissen en, omdat minder gebeurtenissen worden verwerkt moeten, de procedure kan query's beter.

## <a name="example"></a>Voorbeeld

Het volgende voorbeeld is gebaseerd op een scenario waarbij twee of meer apparaten metingen of signalen verzenden. De metingen of signalen mogelijk *Flow tarief*, *Engine oliedruk*, *temperatuur*, en *vochtigheid*.

In het volgende voorbeeld is er een enkel bericht van IoT Hub, waarbij de buitenste matrix een gedeelde sectie van de algemene dimensiewaarden bevat. Time Series-Instantiegegevens door de buitenste matrix wordt gebruikt om de efficiëntie van het bericht. Time Series-exemplaar bevat metagegevens van apparaten, die met elke gebeurtenis niet wijzigen, maar biedt nuttige eigenschappen voor data-analyse. Als u wilt opslaan op via de kabel verzonden bytes en het bericht efficiënter te maken, moet u rekening houden met algemene dimensiewaarden batchverwerking en die gebruikmaakt van de metagegevens van het exemplaar van Time Series.

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
                "Flow Rate psi": 0.58015072345733643,
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

Time Series Insights Preview lid wordt van een tabel (na afvlakken) tijdens het uitvoeren van query's. De tabel bevat aanvullende kolommen, zoals **Type**. Het volgende voorbeeld laat zien hoe u [vorm](./time-series-insights-send-events.md#json) uw telemetriegegevens:

| deviceId  | Type | L1 | L2 | tijdstempel | reeks. Stroom tarief ft3/s | reeks. Engine oliedruk psi |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| `FXXX` | Default_Type | REVOLT SIMULATOR | Accu System | 2018-01-17T01:17:00Z |    1.0172575712203979 |    34,7 |
| `FXXX` | LINE_DATA REVOLT | SIMULATOR |    Accu System |    2018-01-17T01:17:00Z | 2.445906400680542 |  49,2 |
| `FYYY` | ALGEMENE LINE_DATA | SIMULATOR |    Accu System |    2018-01-17T01:18:00Z | 0.58015072345733643 |    22.2 |

In het voorgaande voorbeeld, houd rekening met de volgende punten:

* Statische eigenschappen worden opgeslagen in de Preview van tijd Series Insights om te worden verzonden via het netwerk optimaliseren.
* Time Series Insights-Preview-gegevens op het moment dat de query is gekoppeld met behulp van de *timeSeriesId* die gedefinieerd in het exemplaar.
* Twee lagen van geneste worden gebruikt, is het meest die wordt ondersteund door de Time Series Insights Preview. Het is essentieel om te voorkomen dat diep geneste matrices.
* Omdat er enkele metingen, wordt deze zijn verzonden als afzonderlijke eigenschappen binnen hetzelfde object. In het voorbeeld **reeks. Flow tarief psi**, **reeks. Engine oliedruk psi**, en **reeks. Flow tarief ft3/s** unieke kolommen zijn.

>[!IMPORTANT]
> Exemplaarvelden worden niet opgeslagen met telemetrie. Ze worden opgeslagen met metagegevens in de **Tijdreeksmodel**.
> De voorgaande tabel vertegenwoordigt de queryweergave.

## <a name="next-steps"></a>Volgende stappen

Om deze richtlijnen in de praktijk, Zie [Azure Time Series Insights Preview querysyntaxis](./time-series-insights-query-data-csharp.md). Leert u dat meer informatie over de query-syntaxis voor de gegevens van de Time Series Insights Preview REST-API toegang.

Zie voor meer informatie over ondersteunde JSON-vormen, [ondersteund JSON-vormen](./time-series-insights-send-events.md#json).
