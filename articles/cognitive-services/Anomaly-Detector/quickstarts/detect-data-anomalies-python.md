---
title: 'Quickstart: Afwijkingen als een batch detecteren met behulp van de anomalie detectie REST API en python'
titleSuffix: Azure Cognitive Services
description: Gebruik de anomalie detectie-API om afwijkingen in uw gegevens reeksen op te sporen als een batch of gegevens stromen.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: quickstart
ms.date: 07/26/2019
ms.author: aahi
ms.openlocfilehash: f40f1b94b3e7c2732fd8bed0bc6e503277b533c3
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68565818"
---
# <a name="quickstart-detect-anomalies-in-your-time-series-data-using-the-anomaly-detector-rest-api-and-python"></a>Quickstart: Afwijkingen in uw time series-gegevens detecteren met behulp van de anomalie detectie REST API en python

Gebruik deze Quick Start om de twee detectie modi van de anomalie detectie-API te gebruiken voor het detecteren van afwijkingen in uw time series-gegevens. Deze python-toepassing verzendt twee API-aanvragen met tijdreeks gegevens in JSON-indeling en ontvangt de antwoorden.

| API-aanvraag                                        | Toepassingsuitvoer                                                                                                                         |
|----------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| Afwijkingen als een batch detecteren                        | Het JSON-antwoord met de afwijkings status (en andere gegevens) voor elk gegevens punt in de tijdreeks gegevens en de posities van gedetecteerde afwijkingen. |
| De afwijkings status van het laatste gegevens punt detecteren | Het JSON-antwoord met de afwijkings status (en andere gegevens) voor het laatste gegevens punt in de time series-gegevens.                                                                                                                                         |

 Hoewel deze toepassing in Python is geschreven, is de API een RESTful-webservice die compatibel is met vrijwel elke programmeertaal.

## <a name="prerequisites"></a>Vereisten

- [Python 2.x of 3.x](https://www.python.org/downloads/)

- De [bibliotheek aanvragen](http://docs.python-requests.org) voor python

- Een JSON-bestand met gegevens punten van de tijd reeks. De voorbeeld gegevens voor deze Quick Start vindt u op [github](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/request-data.json).

[!INCLUDE [cognitive-services-anomaly-detector-data-requirements](../../../../includes/cognitive-services-anomaly-detector-data-requirements.md)]

[!INCLUDE [cognitive-services-anomaly-detector-signup-requirements](../../../../includes/cognitive-services-anomaly-detector-signup-requirements.md)]


## <a name="create-a-new-application"></a>Een nieuwe toepassing maken

1. Maak een nieuw python-bestand in uw favoriete tekst editor of IDE. Voeg de volgende import bewerkingen toe.

    ```python
    import requests
    import json
    ```

2. Maak variabelen voor uw abonnements sleutel en uw eind punt. Hieronder vindt u de Uri's die u voor anomalie detectie kunt gebruiken. Deze worden later toegevoegd aan uw service-eind punt om de API-aanvraag-Url's te maken.

    |Detectie methode  |URI  |
    |---------|---------|
    |Batch detectie    | `/anomalydetector/v1.0/timeseries/entire/detect`        |
    |Detectie op het laatste gegevens punt     | `/anomalydetector/v1.0/timeseries/last/detect`        |

    ```python
    batch_detection_url = "/anomalydetector/v1.0/timeseries/entire/detect"
    latest_point_detection_url = "/anomalydetector/v1.0/timeseries/last/detect"

    endpoint = "[YOUR_ENDPOINT_URL]"
    subscription_key = "[YOUR_SUBSCRIPTION_KEY]"
    data_location = "[PATH_TO_TIME_SERIES_DATA]"
    ```

3. Lees in het JSON-gegevens bestand door het te openen en `json.load()`met.

    ```python
    file_handler = open(data_location)
    json_data = json.load(file_handler)
    ```

## <a name="create-a-function-to-send-requests"></a>Een functie maken om aanvragen te verzenden

1. Maak een nieuwe functie met `send_request()` de naam die de hierboven gemaakte variabelen accepteert. Voer vervolgens de volgende stappen uit.

2. Maak een woorden lijst voor de aanvraag headers. Stel de `Content-Type` in `application/json`op en voeg uw abonnements sleutel toe aan `Ocp-Apim-Subscription-Key` de koptekst.

3. Verzend de aanvraag via `requests.post()`. Combi neer uw eind punt en de detectie-URL voor afwijkingen voor de volledige aanvraag-URL en voeg uw kopteksten en JSON-aanvraag gegevens toe. En retourneert vervolgens het antwoord.

```python
def send_request(endpoint, url, subscription_key, request_data):
    headers = {'Content-Type': 'application/json',
               'Ocp-Apim-Subscription-Key': subscription_key}
    response = requests.post(
        endpoint+url, data=json.dumps(request_data), headers=headers)
    return json.loads(response.content.decode("utf-8"))
```

## <a name="detect-anomalies-as-a-batch"></a>Afwijkingen als een batch detecteren

1. Maak een methode met `detect_batch()` de naam om afwijkingen in de gegevens op te sporen als een batch. Roep de `send_request()` hierboven gemaakte methode aan met uw eind punt, URL, abonnements sleutel en JSON-gegevens.

2. Bel `json.dumps()` het resultaat om het te Format teren en druk het af op de-console.

3. Als het antwoord veld `code` bevat, drukt u de fout code en het fout bericht af.

4. Als dat niet het geval is, kunt u de positie van afwijkingen vinden in de gegevensset. Het veld van `isAnomaly` de respons bevat een Booleaanse waarde die aangeeft of een gegeven gegevens punt een afwijkend is. De lijst herhalen en de index van alle `True` waarden afdrukken. Deze waarden komen overeen met de index van afwijkende gegevens punten, als deze zijn gevonden.

```python
def detect_batch(request_data):
    print("Detecting anomalies as a batch")
    result = send_request(endpoint, batch_detection_url,
                          subscription_key, request_data)
    print(json.dumps(result, indent=4))

    if result.get('code') != None:
        print("Detection failed. ErrorCode:{}, ErrorMessage:{}".format(
            result['code'], result['message']))
    else:
        # Find and display the positions of anomalies in the data set
        anomalies = result["isAnomaly"]
        print("Anomalies detected in the following data positions:")
        for x in range(len(anomalies)):
            if anomalies[x] == True:
                print(x)
```

## <a name="detect-the-anomaly-status-of-the-latest-data-point"></a>De afwijkings status van het laatste gegevens punt detecteren

1. Maak een methode met `detect_latest()` de naam om te bepalen of het meest recente gegevens punt in de tijd reeks een afwijkend is. Roep de `send_request()` bovenstaande methode aan met uw eind punt, URL, abonnements sleutel en JSON-gegevens. 

2. Bel `json.dumps()` het resultaat om het te Format teren en druk het af op de-console.

```python
def detect_latest(request_data):
    print("Determining if latest data point is an anomaly")
    # send the request, and print the JSON result
    result = send_request(endpoint, latest_point_detection_url,
                          subscription_key, request_data)
    print(json.dumps(result, indent=4))
```

## <a name="load-your-time-series-data-and-send-the-request"></a>Laad uw time series-gegevens en verzend de aanvraag

1. Laad uw JSON time series-gegevens die een bestandshandler openen en gebruiken `json.load()` . Roep vervolgens de hierboven gemaakte anomalie detectie methoden aan.

```python
file_handler = open(data_location)
json_data = json.load(file_handler)

detect_batch(json_data)
detect_latest(json_data)
```

### <a name="example-response"></a>Voorbeeld van een antwoord

Een geslaagde reactie wordt geretourneerd in JSON-indeling. Klik op de onderstaande koppelingen om het JSON-antwoord op GitHub weer te geven:
* [Voor beeld van een antwoord op een batch detectie](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/batch-response.json)
* [Voor beeld van laatste punt detectie respons](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/latest-point-response.json)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Naslaginformatie over REST API](https://westus2.dev.cognitive.microsoft.com/docs/services/AnomalyDetector/operations/post-timeseries-entire-detect)
