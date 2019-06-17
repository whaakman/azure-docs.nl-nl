---
title: Best practices bij het gebruik van de Anomaly Detector-API
description: Meer informatie over aanbevolen procedures bij het detecteren van afwijkingen met de API van de detectie van afwijkingen.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: article
ms.date: 03/26/2019
ms.author: aahi
ms.openlocfilehash: 766d009be3cd664d928a3c12f5fea38c26bbbdde
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64692201"
---
# <a name="best-practices-for-using-the-anomaly-detector-api"></a>Aanbevolen procedures voor het gebruik van de Anomaliedetectie-API voor Detector

De detectie van afwijkingen API is een staatloze anomaly detection-service. De nauwkeurigheid en de prestaties van de resultaten kunnen worden beïnvloed door:

* Hoe uw time series-gegevens wordt voorbereid.
* De API voor Afwijkingsdetectie Detector-parameters die zijn gebruikt.
* Het aantal gegevenspunten in uw API-aanvraag. 

Gebruik dit artikel voor meer informatie over aanbevolen procedures voor het gebruik van de API ophalen van de beste resultaten voor uw gegevens. 

## <a name="when-to-use-batch-entire-or-latest-last-point-anomaly-detection"></a>Wanneer u batch (hele) of de meest recente (laatste) wijst u detectie van afwijkingen

De Anomaliedetectie Detector API van batch-detectie-eindpunt kunt u afwijkingen via uw volledige tijden series-gegevens. In deze detectiemodus één statistische model gemaakt en toegepast op elk punt in de gegevensset. Als de tijdreeks heeft de onderstaande kenmerken, wordt u aangeraden de detectie van batch om een voorbeeld van uw gegevens in één API-aanroep.

* Een seizoensgebonden tijdreeks met incidentele afwijkingen.
* Een platte trend tijdreeks, met af en toe pieken dips krijgen. 

We raden gebruik van batch-anomaliedetectie voor gegevens in realtime controleren, of gebruikt op time series-gegevens die geen hierboven kenmerken. 

* Detectie van batch maakt en geldt slechts één model, de detectie voor elk punt in de context van een hele serie wordt uitgevoerd. Als de tijd reeks gegevenstrends omhoog of omlaag zonder seizoensgebondenheid zien, enkele punten van wijzigt (Spanningsdips en pieken in de gegevens) worden gemist door het model. Op deze manier enkele punten van de wijziging die minder belangrijk dan de wijzigingsaanvragen later in de gegevensset zijn mogelijk niet worden geteld als aanzienlijk kunnen worden opgenomen in het model.

* Batch-detectie is langzamer dan het detecteren van de anomaliedetectie-status van het laatste herstelpunt bij het uitvoeren van realtime gegevens controleren, vanwege het aantal punten die wordt geanalyseerd.

Voor het bewaken van real-time gegevens, wordt u aangeraden de anomaliedetectie-status van alleen de meest recente gegevenspunt detecteren. Laatste punt detectie continu toepast, kan streaming gegevens bewaking worden gedaan efficiënt en nauwkeurig.

In het volgende voorbeeld wordt de impact die deze modi detectie op prestaties hebben kunnen beschreven. De eerste afbeelding toont het resultaat van het laatste herstelpunt van afwijkingen status langs 28 eerder weergegeven gegevenspunten continu te detecteren. De rode punten worden afwijkingen.

![Een afbeelding van de detectie van afwijkingen met behulp van het laatste herstelpunt](../media/last.png)

Hieronder ziet u de dezelfde gegevensset met behulp van batch-anomaliedetectie. Het model die is gebouwd voor de bewerking heeft verschillende afwijkingen, gemarkeerd met rechthoeken genegeerd.

![Een afbeelding van de detectie van afwijkingen met behulp van de batch-methode](../media/entire.png)

## <a name="data-preparation"></a>Gegevensvoorbereiding

De detectie van afwijkingen API accepteert tijdreeks gegevens opgemaakt in een JSON-request-object. Een tijdreeks kan worden geregistreerd na verloop van tijd in opeenvolgende volgorde, numerieke gegevens. U kunt windows van uw time series-gegevens verzenden naar de Anomaliedetectie Detector API-eindpunt van de API-prestaties te verbeteren. Het minimum aantal gegevenspunten die u kunt verzenden 12 is, en de maximumwaarde is 8640 punten. 

Gegevenspunten die worden verzonden naar de detectie van afwijkingen API moeten een geldige tijdstempel van Coordinated Universal Time (UTC), en een numerieke waarde hebben. 

```json
{
    "granularity": "daily",
    "series": [
      {
        "timestamp": "2018-03-01T00:00:00Z",
        "value": 32858923
      },
      {
        "timestamp": "2018-03-02T00:00:00Z",
        "value": 29615278
      },
    ]
}
```

### <a name="missing-data-points"></a>Ontbrekende gegevenspunten

Ontbrekende gegevenspunten worden gebruikt in gelijkmatig gedistribueerd time series gegevenssets, met name die met een granulariteit van fijn (een kleine steekproefinterval. For example, gegevens verzameld om de paar minuten). Minder dan 10% van het verwachte aantal punten in uw gegevens ontbreken, mag niet een negatieve invloed hebben op uw resultaten. Beschrijf de hiaten in uw gegevens op basis van de kenmerken, zoals het vervangen van gegevenspunten van een eerdere periode, lineaire interpolatie of een zwevend gemiddelde.

### <a name="aggregate-distributed-data"></a>Cumulatieve gedistribueerde gegevens

De detectie van afwijkingen API werkt het beste op een gelijkmatig gedistribueerd tijdreeksen. Als uw gegevens willekeurig is gedistribueerd, moet u aggregeren door een tijdseenheid, zoals Per minuut, uur of dagelijks bijvoorbeeld.

## <a name="anomaly-detection-on-data-with-seasonal-patterns"></a>Detectie van afwijkingen op gegevens met seizoensgebonden patronen

Als u weet dat uw time series-gegevens een seizoenspatroon (één die uitgevoerd op regelmatige intervallen wordt) heeft, kunt u de nauwkeurigheid en API-reactietijd kunt verbeteren. 

Opgeven van een `period` als u uw JSON-aanvraag anomaly detection latentie kunt verminderen met maximaal 50%. De `period` is een geheel getal dat Hiermee geeft u de tijdreeksen van ongeveer het aantal gegevenspunten nodig is voor het herhalen van een patroon. Bijvoorbeeld, een tijdreeks met één gegevenspunt per dag zou hebben een `period` als `7`, en een tijdreeks met één punt per uur (met de wekelijkse hetzelfde patroon) moet een `period` van `7*24`. Als u niet zeker weet van patronen van uw gegevens, hebt u niet aan deze parameter opgeeft.

Voor de beste resultaten bieden 4 `period`de waarde van gegevenspunt, plus een extra regel. Bijvoorbeeld, per uur gegevens met een wekelijkse patroon zoals hierboven wordt beschreven dient 673 gegevenspunten in de hoofdtekst van de aanvraag (`7 * 24 * 4 + 1`).

### <a name="sampling-data-for-real-time-monitoring"></a>Steekproef nemen voor gegevens voor realtime-controle

Als uw streaming-gegevens verzameld op basis van een kort interval (bijvoorbeeld seconden of minuten), misschien verzendt het aanbevolen aantal gegevenspunten overschrijdt de Anomaliedetectie Detector API maximumaantal toegestane (8640 gegevenspunten). Als u uw gegevens ziet een stabiele seizoenspatroon, houd rekening met het verzenden van een voorbeeld van uw time series-gegevens met een grotere tijdsinterval, zoals uur. Steekproeven van uw gegevens op deze manier, kunt u de API-reactietijd ook aanzienlijk kunt verbeteren. 

## <a name="next-steps"></a>Volgende stappen

* [Wat is de detectie van afwijkingen API?](../overview.md)
* [Snelstart: Detecteer afwijkingen in uw time series-gegevens met de REST API voor Afwijkingsdetectie Detector](../quickstarts/detect-data-anomalies-csharp.md)
