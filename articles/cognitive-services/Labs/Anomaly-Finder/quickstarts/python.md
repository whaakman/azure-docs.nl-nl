---
title: Het gebruik van de Anomaliedetectie Finder-API met Python - Microsoft Cognitive Services | Microsoft Docs
description: Get-informatie en codevoorbeelden om u te helpen snel aan de slag met behulp van Afwijkingsdetectie met Python in Cognitive Services.
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: chliang
ms.openlocfilehash: c9fd96bf4a9bdc9dfe5baf52aa742e19d5436708
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/03/2018
ms.locfileid: "48247348"
---
# <a name="use-the-anomaly-finder-api-with-python"></a>De API voor Afwijkingsdetectie gebruiken met Python

[!INCLUDE [PrivatePreviewNote](../../../../../includes/cognitive-services-anomaly-finder-private-preview-note.md)]

Dit artikel bevat informatie en voorbeelden van code om u te helpen snel aan de slag met behulp van de Anomaliedetectie Finder-API met Python om uit te voeren taak van het ophalen van de anomaliedetectie-resultaat voor time series-gegevens.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="getting-anomaly-points-with-anomaly-finder-api-using-python"></a>Ophalen van afwijkingen punten met afwijkingen Finder-API met behulp van Python 

[!INCLUDE [DataContract](../includes/datacontract.md)]

### <a name="example-of-time-series-data"></a>Voorbeeld van time series-gegevens

Het voorbeeld van de tijd gegevenspunten van reeks is als volgt.

[!INCLUDE [Request](../includes/request.md)]

### <a name="analyze-data-and-get-anomaly-points-python-example"></a>Gegevens analyseren en anomaliedetectie punten Python-voorbeeld ophalen

Zorg ervoor dat u python3 hebt geïnstalleerd en maak vervolgens een uitvoerbaar bestand met de naam detect.py van python. In detect.py, moet u de onderstaande code opnemen. Voordat de code wordt uitgevoerd, vergeet niet om te vervangen door de `[YOUR_SUBSCRIPTION_KEY]` waarde door de sleutel geldig abonnement.
Vervang de `[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]` met uw gegevenspunten.

```python
import requests
import json


def detect(url, subscription_key, request_data):
    headers = {'Content-Type': 'application/json', 'Ocp-Apim-Subscription-Key': subscription_key}
    response = requests.post(url, data=json.dumps(request_data), headers=headers)
    if response.status_code == 200:
        return json.loads(response.content.decode("utf-8"))
    else:
        print(response.status_code)
        raise Exception(response.text)


sample_data = "[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]"
endpont = "https://api.labs.cognitive.microsoft.com/anomalyfinder/v1.0/anomalydetection"
subscription_key = "[YOUR_SUBSCRIPTION_KEY]"

result = detect(endpont, subscription_key, sample_data)
print(result)

```

### <a name="example-response"></a>Voorbeeld van een antwoord

Een geslaagd antwoord wordt geretourneerd in JSON-indeling. Voorbeeldantwoord is als volgt.
[!INCLUDE [Response](../includes/response.md)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Python-app](../tutorials/python-tutorial.md)
