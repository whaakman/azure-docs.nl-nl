---
title: De anomalie detectie-API gebruiken voor uw tijdreeks gegevens
titleSuffix: Azure Cognitive Services
description: Meer informatie over het detecteren van afwijkingen in uw gegevens, hetzij als een batch of bij het streamen van gegevens.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: article
ms.date: 03/26/2019
ms.author: aahi
ms.openlocfilehash: 4b97c52ec0ed076e1ab8aeada90f430b8ed87514
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2019
ms.locfileid: "68854798"
---
# <a name="how-to-use-the-anomaly-detector-api-on-your-time-series-data"></a>Procedure: De anomalie detectie-API gebruiken voor uw time series-gegevens  

De [anomalie](https://westus2.dev.cognitive.microsoft.com/docs/services/AnomalyDetector/operations/post-timeseries-entire-detect) detectie-API biedt twee methoden voor het afwijken van afwijkingen. U kunt afwijkingen als een batch in uw reeks tijden detecteren of wanneer uw gegevens worden gegenereerd door de afwijkings status van het meest recente gegevens punt te detecteren. Het detectie model retourneert afwijkende resultaten samen met de verwachte waarde van elk gegevens punt en de detectie grenzen van de boven-en ondergrens. u kunt deze waarden gebruiken om het bereik van normale waarden en afwijkingen in de gegevens te visualiseren.

## <a name="anomaly-detection-modes"></a>Afwijkings detectie modi 

De anomalie detector-API biedt detectie modi: batch en streaming.

> [!NOTE]
> De volgende aanvraag-Url's moeten worden gecombineerd met het juiste eind punt voor uw abonnement. Bijvoorbeeld: `https://westus2.api.cognitive.microsoft.com/anomalydetector/v1.0/timeseries/entire/detect`


### <a name="batch-detection"></a>Batch detectie

Als u afwijkingen wilt detecteren in een batch met gegevens punten gedurende een bepaald tijds bereik, gebruikt u de volgende aanvraag-URI met de tijdreeks gegevens: 

`/timeseries/entire/detect`. 

Wanneer u de tijdreeks gegevens in één keer verzendt, genereert de API een model met behulp van de hele serie en analyseert elk gegevens punt ermee.  

### <a name="streaming-detection"></a>Streaming-detectie

Om voortdurend afwijkingen op streaminggegevens te detecteren, gebruikt u de volgende aanvraag-URI met uw laatste gegevens punt: 

`/timeseries/last/detect'`. 

Door nieuwe gegevens punten te verzenden terwijl u ze genereert, kunt u uw gegevens in realtime bewaken. Er wordt een model gegenereerd met de gegevens punten die u verzendt, en de API bepaalt of het laatste punt in de tijd reeks een afwijkend is.

## <a name="adjusting-lower-and-upper-anomaly-detection-boundaries"></a>De grenzen voor de detectie van lagere en hoogste afwijkingen aanpassen

Standaard worden de boven-en ondergrenzen voor anomalie detectie berekend met `expectedValue`, `upperMargin`en `lowerMargin`. Als u andere grenzen nodig hebt, kunt u het `marginScale` beste `upperMargin` een `lowerMargin`to-of Toep assen. De grenzen worden als volgt berekend:

|Vormt  |Berekening  |
|---------|---------|
|`upperBoundary` | `expectedValue + (100 - marginScale) * upperMargin`        |
|`lowerBoundary` | `expectedValue - (100 - marginScale) * lowerMargin`        |

In de volgende voor beelden ziet u een anomalie detectie-API-resultaat op verschillende sensitivities.

### <a name="example-with-sensitivity-at-99"></a>Voor beeld met een gevoeligheid van 99

![Standaard gevoeligheid](../media/sensitivity_99.png)

### <a name="example-with-sensitivity-at-95"></a>Voor beeld met een gevoeligheid van 95

![99-gevoeligheid](../media/sensitivity_95.png)

### <a name="example-with-sensitivity-at-85"></a>Voor beeld met een gevoeligheid van 85

![85-gevoeligheid](../media/sensitivity_85.png)

## <a name="next-steps"></a>Volgende stappen

* [Wat is de anomalie detectie-API?](../overview.md)
* [Snelstart: Afwijkingen in uw time series-gegevens detecteren met behulp van de anomalie detectie REST API](../quickstarts/detect-data-anomalies-csharp.md)
