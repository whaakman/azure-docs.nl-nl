---
title: Het gebruik van de detectie van afwijkingen API op uw time series-gegevens
description: Informatie over het detecteren van afwijkingen in uw gegevens als een batch of voor streaming-gegevens.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: article
ms.date: 03/26/2019
ms.author: aahi
ms.openlocfilehash: 63ede8fe90d5c19c2473ffb315bf6096599ffb9c
ms.sourcegitcommit: fbfe56f6069cba027b749076926317b254df65e5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58473234"
---
# <a name="how-to-use-the-anomaly-detector-api-on-your-time-series-data"></a>Procedure: Gebruik de API van de detectie van afwijkingen op uw time series-gegevens  

De [Anomaliedetectie Detector API](https://westus2.dev.cognitive.microsoft.com/docs/services/AnomalyDetector/operations/post-timeseries-entire-detect) biedt twee methoden voor het detecteren van afwijkingen. Kunt u ofwel afwijkingen detecteren als batch in de tijden voor uw gehele reeks, of als uw gegevens wordt gegenereerd door het detecteren van de status van afwijkingen van de meest recente gegevenspunt. Model voor de resultaten van afwijkingen samen met elk gegevenspunt verwachtingswaarde en de boven- en ondergrens van afwijkingen detectie grenzen. u kunt deze waarden gebruiken voor het visualiseren van het bereik van de normale waarden en afwijkingen in de gegevens.

## <a name="anomaly-detection-modes"></a>Anomaly detection modi 

De detectie van afwijkingen API biedt detectie-modi: batch- en streaming.

> [!NOTE]
> De volgende aanvraag die URL 's moeten worden gecombineerd met het juiste eindpunt voor uw abonnement. Bijvoorbeeld: `https://westus2.api.cognitive.microsoft.com/anomalydetector/v1.0/timeseries/entire/detect`


### <a name="batch-detection"></a>Detectie van batch

Voor het detecteren van afwijkingen in een batch van gegevenspunten gedurende een bepaalde periode, gebruikt u de volgende aanvraag-URI met uw time series-gegevens: 

`/timeseries/entire/detect`. 

Door uw time series-gegevens in één keer verzendt, wordt de API een model met behulp van de volledige reeks genereren en analyseer elk gegevenspunt ermee.  

### <a name="streaming-detection"></a>Streaming-detectie

Voor het continu detecteren van afwijkingen op streaming-gegevens, gebruikt u de volgende aanvraag-URI met de meest recente gegevenspunt: 

`/timeseries/last/detect'`. 

U kunt uw gegevens in realtime controleren door te sturen nieuwe gegevenspunten als u ze genereren. Een model wordt gegenereerd met de gegevenspunten die u verzendt, en de API kan bepalen of het laatste herstelpunt in de tijdreeks een anomalie is.

## <a name="adjusting-lower-and-upper-anomaly-detection-boundaries"></a>Onderste en bovenste anomaly detection grenzen aanpassen

Standaard de grenzen boven en ondergrenzen voor detectie van afwijkingen worden berekend met behulp van `expectedValue`, `upperMargin`, en `lowerMargin`. Als u verschillende grenzen nodig hebt, raden wij aan toepassen van een `marginScale` naar `upperMargin` of `lowerMargin`. De grenzen zou als volgt berekend:

|Grens  |Berekening  |
|---------|---------|
|`upperBoundary` | `expectedValue + (100 - marginScale) * upperMargin`        |
|`lowerBoundary` | `expectedValue - (100 - marginScale) * lowerMargin`        |

De volgende voorbeelden ziet een API voor Afwijkingsdetectie Detector resultaat op verschillende gevoeligheid.

### <a name="example-with-sensitivity-at-99"></a>Voorbeeld met gevoeligheid op 99

![Standaard gevoeligheid](../media/sensitivity_99.png)

### <a name="example-with-sensitivity-at-95"></a>Voorbeeld met gevoeligheid op 95

![99 gevoeligheid](../media/sensitivity_95.png)

### <a name="example-with-sensitivity-at-85"></a>Voorbeeld met gevoeligheid op 85

![85 gevoeligheid](../media/sensitivity_85.png)

## <a name="next-steps"></a>Volgende stappen

* [Wat is de detectie van afwijkingen API?](../overview.md)
* [Snelstart: Detecteer afwijkingen in uw time series-gegevens met de REST API voor Afwijkingsdetectie Detector](../quickstarts/detect-data-anomalies-csharp.md)