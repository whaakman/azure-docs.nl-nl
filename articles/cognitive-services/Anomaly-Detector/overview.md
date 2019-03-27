---
title: Wat is de detectie van afwijkingen API? | Microsoft Docs
titleSuffix: Azure Cognitive Services
description: Gebruik geavanceerde algoritmen afwijkingen Detector-API's om te identificeren van afwijkingen in uw time series-gegevens.
services: cognitive-services
author: aahill
ms.service: cognitive-services
ms.subservice: anomaly-detection
ms.topic: article
ms.date: 03/26/2019
ms.author: aahi
ms.openlocfilehash: 48dffb8853d3def590e434c1683ecf038e2a194e
ms.sourcegitcommit: fbfe56f6069cba027b749076926317b254df65e5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58473161"
---
# <a name="what-is-the-anomaly-detector-api"></a>Wat is de detectie van afwijkingen API?

De detectie van afwijkingen API kunt u om te controleren en detecteren van afwijkingen in uw time series-gegevens met machine learning. De detectie van afwijkingen API worden aangepast door het automatisch identificeren en de best passende modellen toepassen op uw gegevens, ongeacht de branche, scenario of gegevensvolume. Met behulp van uw time series-gegevens, de API bepaalt grenzen voor detectie van afwijkingen, verwachte waarden, en welke gegevenspunten zijn afwijkingen.

![Patroon wijzigingen detecteren in serviceaanvragen](./media/anomaly_detection2.png)

Met de detectie van afwijkingen hoeft niet alle eerdere ervaring in machine learning- en de RESTful-API kunt u de service eenvoudig te integreren in uw toepassingen en processen.

## <a name="features"></a>Functies

Met de detectie van afwijkingen, kunt u afwijkingen automatisch detecteren in uw time series-gegevens of wanneer deze zich in realtime voordoen. 

|Functie  |Description  |
|---------|---------|
|Afwijkingen detecteren zodra ze zich in realtime voordoen. | Detecteer afwijkingen in uw streaming-gegevens met behulp van de eerder weergegeven gegevenspunten om te bepalen of uw recentste is een afwijking. Met deze bewerking wordt een model met behulp van de gegevenspunten die u verzendt, en bepaalt of de doelpunt een anomalie wordt gegenereerd. Door het aanroepen van de API met elk nieuw gegevenspunt die u genereert, kunt u uw gegevens controleren wanneer deze gemaakt. |
|Detecteer afwijkingen in uw gegevensset als een batch. | De tijdreeksen gebruiken voor het detecteren van mogelijke afwijkingen die in uw gegevens voorkomen kunnen. Met deze bewerking wordt een model met behulp van uw hele time series-gegevens met elk punt geanalyseerd met behulp van hetzelfde model gegenereerd.         |
| Aanvullende informatie over uw gegevens opvragen. | Nuttige informatie over uw gegevens en eventuele waargenomen afwijkingen, met inbegrip van de verwachte waarden, afwijkingen grenzen en functies ophalen. |
| Pas de grenzen van de detectie van afwijkingen. | De detectie van afwijkingen API maakt automatisch grenzen voor detectie van afwijkingen. Pas deze grenzen als u wilt vergroten of verkleinen van de gevoeligheid van de API voor data afwijkingen en betere aansluiting op uw gegevens. |

## <a name="demo"></a>Demo

Als u wilt snel beginnen met de API van de detectie van afwijkingen, probeert een [onlinedemo](https://notebooks.azure.com/AzureAnomalyDetection/projects/anomalydetector) die kan worden uitgevoerd in uw browser. Wordt uitgevoerd in een web-hosted Jupyter-notebook voor deze demo's en ziet u hoe u een API-aanvraag verzenden en het resultaat te visualiseren.

Als u wilt uitvoeren van de demo, voert u de volgende stappen uit:

1. Een geldige sleutel voor Anomaliedetectie Detector API-abonnement en een API-eindpunt ophalen. De onderstaande sectie bevat instructies om zich te registreren. 
2. Meld u aan en klikt u op de kloon, in de rechterbovenhoek.
3. Klik op **worden uitgevoerd op de gratis compute**
4. Selecteer een van de notebooks voor dit voorbeeld.
5. Uw geldige Anomaliedetectie Detector API abonnementssleutel toevoegen aan de `subscription_key` variabele. Wijzig de `endpoint` variabele aan uw eindpunt. Bijvoorbeeld: `https://westus2.api.cognitive.microsoft.com`
1. Klik op de bovenste menubalk **cel**, klikt u vervolgens **alles uitvoeren**.

## <a name="workflow"></a>Werkstroom

De detectie van afwijkingen API is een RESTful-web-service, zodat u eenvoudig kunt aanroepen vanuit elke programmeertaal die HTTP-aanvragen te parseren van JSON.

[!INCLUDE [cognitive-services-anomaly-detector-data-requirements](../../../includes/cognitive-services-anomaly-detector-data-requirements.md)]

[!INCLUDE [cognitive-services-anomaly-detector-signup-requirements](../../../includes/cognitive-services-anomaly-detector-signup-requirements.md)]

Na het aanmelden:

1. Uw time series-gegevens nemen en deze converteren naar een geldig JSON-indeling. Gebruik [aanbevolen procedures](concepts/anomaly-detection-best-practices.md) tijdens het voorbereiden van uw gegevens om op te halen van de beste resultaten.
1. Een aanvraag verzenden naar de Anomaliedetectie Detector API met uw gegevens.
1. Verwerk de API-reactie door het geretourneerde JSON-bericht te parseren.

## <a name="next-steps"></a>Volgende stappen

* [Snelstart: Detecteer afwijkingen in uw time series-gegevens met de REST API voor Afwijkingsdetectie Detector](quickstarts/detect-data-anomalies-csharp.md)
* De detectie van afwijkingen API [onlinedemo](https://notebooks.azure.com/AzureAnomalyDetection/projects/anomalydetector)
* De detectie van afwijkingen [REST API-naslaginformatie](https://westus2.dev.cognitive.microsoft.com/docs/services/AnomalyDetector/operations/post-timeseries-entire-detect)