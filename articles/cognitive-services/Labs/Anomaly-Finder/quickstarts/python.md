---
title: Het gebruik van de Afwijkingsdetectie zoeken-API met behulp van Python - cognitieve Microsoft-Services | Microsoft Docs
description: Get-informatie en codevoorbeelden kunt u snel aan de slag met Afwijkingsdetectie zoeken met behulp van Python in cognitieve Services.
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: chliang
ms.openlocfilehash: c14916b0644edab613b298d6e71f8bbb9a6bb804
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345050"
---
# <a name="use-the-anomaly-finder-api-with-python"></a>Gebruik de zoekfunctie Afwijkingsdetectie API met behulp van Python

In dit artikel bevat informatie en codevoorbeelden kunt u snel aan de slag met behulp van de Afwijkingsdetectie zoeken-API met behulp van Python taak van het ophalen van afwijkingsdetectie resultaat voor reeksgegevens tijd uit te voeren.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="getting-anomaly-points-with-anomaly-finder-api-using-python"></a>Afwijkingsdetectie punten ophalen met Afwijkingsdetectie zoeken-API met behulp van Python 

[!INCLUDE [DataContract](../includes/datacontract.md)]

### <a name="example-of-time-series-data"></a>Voorbeeld van een reeksgegevens

In het voorbeeld van de gegevenspunten van reeksen tijd is als volgt.

[!INCLUDE [Request](../includes/request.md)]

### <a name="analyze-data-and-get-anomaly-points-python-example"></a>Gegevens analyseren en afwijkingsdetectie punten Python voorbeeld ophalen

Zorg ervoor dat u python3 hebt geïnstalleerd en maak vervolgens een uitvoerbaar bestand met de naam detect.py van python. In detect.py, moet u de onderstaande code opnemen. Voordat de code wordt uitgevoerd, moet u vervangen de `[YOUR_SUBSCRIPTION_KEY]` waarde met de sleutel geldig abonnement.
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

Een geslaagde reactie wordt geretourneerd als JSON. Voorbeeldreactie is als volgt.
[!INCLUDE [Response](../includes/response.md)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Python-app](../tutorials/python-tutorial.md)
