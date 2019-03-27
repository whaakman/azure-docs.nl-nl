---
title: 'Quickstart: Detecteer afwijkingen als een batch met behulp van de REST-API voor Afwijkingsdetectie Detector en Python | Microsoft Docs'
description: Gebruik de API van de detectie van afwijkingen voor het detecteren van afwijkingen in de reeks als een batch of voor streaming-gegevens.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: article
ms.date: 03/26/2019
ms.author: aahi
ms.openlocfilehash: 60307d51439b4474c8be4f040792c03a6f83b0fd
ms.sourcegitcommit: fbfe56f6069cba027b749076926317b254df65e5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58473160"
---
# <a name="quickstart-detect-anomalies-in-your-time-series-data-using-the-anomaly-detector-rest-api-and-python"></a>Quickstart: Detecteer afwijkingen in uw time series-gegevens met behulp van de REST-API voor Afwijkingsdetectie Detector en Python

Gebruik deze Quick Start om te starten met behulp van twee modi voor detectie van afwijkingen Detector-API's voor het detecteren van afwijkingen in uw time series-gegevens. Deze Python-toepassing twee API-aanvragen met JSON-indeling time series-gegevens verzendt en ontvangt de antwoorden.

| API-aanvraag                                        | Toepassingsuitvoer                                                                                                                         |
|----------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| Detecteer afwijkingen als batch                        | De JSON-antwoord met de anomaliedetectie-status (en andere gegevens) voor elk gegevenspunt in de time series-gegevens en de posities van alle gedetecteerde afwijkingen. |
| De status van afwijkingen van de meest recente gegevenspunt detecteren | De JSON-antwoord met de anomaliedetectie-status (en andere gegevens) voor de meest recente gegevenspunt in de time series-gegevens.                                                                                                                                         |

 Hoewel deze toepassing in Python is geschreven, is de API een RESTful-webservice die compatibel is met vrijwel elke programmeertaal.

## <a name="prerequisites"></a>Vereisten

- [Python 2.x of 3.x](https://www.python.org/downloads/)

- De [aanvragen bibliotheek](http://docs.python-requests.org) voor python

- Een JSON-bestand met time series-gegevens verwijst. De voorbeeldgegevens voor deze Quick Start kunt u vinden op [GitHub](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/request-data.json).

[!INCLUDE [cognitive-services-anomaly-detector-data-requirements](../../../../includes/cognitive-services-anomaly-detector-data-requirements.md)]

[!INCLUDE [cognitive-services-anomaly-detector-signup-requirements](../../../../includes/cognitive-services-anomaly-detector-signup-requirements.md)]


## <a name="create-a-new-application"></a>Een nieuwe toepassing maken

1. Maak een nieuwe python-bestand in uw favoriete teksteditor of IDE. Voeg de volgende import toe.

    ```python
    import requests
    import json
    ```

2. Variabelen voor de abonnementssleutel van uw en uw eindpunt maken. Hieronder vindt u de URI's die u voor detectie van afwijkingen gebruiken kunt. Deze worden toegevoegd aan uw service-eindpunt later het maken van de API aanvraag-URL's.

    |Detectiemethode  |URI  |
    |---------|---------|
    |Detectie van batch    | `/anomalydetector/v1.0/timeseries/entire/detect`        |
    |Detectie op de meest recente gegevenspunt     | `/anomalydetector/v1.0/timeseries/last/detect`        |

    ```python
    batch_detection_url = "/anomalydetector/v1.0/timeseries/entire/detect"
    latest_point_detection_url = "/anomalydetector/v1.0/timeseries/last/detect"

    endpoint = "[YOUR_ENDPOINT_URL]"
    subscription_key = "[YOUR_SUBSCRIPTION_KEY]"
    data_location = "[PATH_TO_TIME_SERIES_DATA]"
    ```

3. In de JSON-gegevensbestand lezen door deze te openen en het gebruik van `json.load()`. 

    ```python
    file_handler = open(data_location)
    json_data = json.load(file_handler)
    ```

## <a name="create-a-function-to-send-requests"></a>Maak een functie voor het verzenden van aanvragen

1. Maak een nieuwe functie met de naam `send_request()` waarmee de variabelen die eerder is gemaakt. Voer vervolgens de volgende stappen uit.

2. Maak een woordenlijst voor de aanvraagheaders. Instellen de `Content-Type` naar `application/json`, en uw abonnementssleutel toevoegen aan de `Ocp-Apim-Subscription-Key` header.

3. Verzendt de aanvraag via `requests.post()`. Combineer uw eindpunt en anomaliedetectie-detectie-URL voor de volledige aanvraag-URL en de kop- en json-aanvraaggegevens bevatten. 

4. Als de aanvraag geslaagd is, wordt het antwoord retourneren.  
    
    ```python
    def send_request(endpoint, url, subscription_key, request_data):
        headers = {'Content-Type': 'application/json', 'Ocp-Apim-Subscription-Key': subscription_key}
        response = requests.post(endpoint+url, data=json.dumps(request_data), headers=headers)
        if response.status_code == 200:
            return json.loads(response.content.decode("utf-8"))
        else:
            print(response.status_code)
            raise Exception(response.text)
    ```

## <a name="detect-anomalies-as-a-batch"></a>Detecteer afwijkingen als batch

1. Maken van een methode met de naam `detect_batch()` voor het detecteren van afwijkingen in de gegevens op als een batch. Roep de `send_request()` methode hierboven hebt gemaakt met uw eindpunt, de url, de abonnementssleutel en de json-gegevens. 

2. Bel `json.dumps()` op het resultaat opmaken en afdrukken naar de console.

3. De posities van afwijkingen vinden in de gegevensset. Van het antwoord `isAnomaly` veld bevat een Booleaanse waarde die betrekking hebben op of een bepaald gegevenspunt een afwijking is. U doorloopt de lijst en de index van een afdrukken `True` waarden. Deze waarden overeenkomen met de index van afwijkende gegevenspunten, als deze zijn gevonden.

```python
def detect_batch(request_data):
    print("Detecting anomalies as a batch")
    result = send_request(endpoint, batch_detection_url, subscription_key, request_data)
    print(json.dumps(result, indent=4))

    # Find and display the positions of anomalies in the data set
    anomalies = result["isAnomaly"]
    print("Anomalies detected in the following data positions:")
    for x in range(len(anomalies)):
        if anomalies[x] == True:
            print (x)
```

## <a name="detect-the-anomaly-status-of-the-latest-data-point"></a>De status van afwijkingen van de meest recente gegevenspunt detecteren

1. Maken van een methode met de naam `detect_latest()` om te bepalen of het laatste herstelpunt van de gegevens in de tijdreeks een afwijking. Roep de `send_request()` methode hierboven met uw eindpunt, de url, de abonnementssleutel en de json-gegevens. 

2. Bel `json.dumps()` op het resultaat opmaken en afdrukken naar de console.

```python
def detect_latest(request_data):
    print("Determining if latest data point is an anomaly")
    # send the request, and print the JSON result
    result = send_request(endpoint, latest_point_detection_url, subscription_key, request_data)
    print(json.dumps(result, indent=4))
```

## <a name="load-your-time-series-data-and-send-the-request"></a>Uw time series-gegevens laden en de aanvraag verzenden

1. Laden van uw JSON time series-gegevens een bestandshandler openen en het gebruik van `json.load()` erop. Roep de anomalie vervolgens detectiemethoden die eerder is gemaakt.
    
    ```python
    file_handler = open (data_location)
    json_data = json.load(file_handler)
    
    detect_batch(json_data)
    detect_latest(json_data)
    ```

### <a name="example-response"></a>Voorbeeld van een antwoord

Een geslaagde respons wordt geretourneerd in JSON-indeling. Klik op de onderstaande koppelingen voor het weergeven van het JSON-antwoord op GitHub:
* [Voorbeeld van een antwoord voor de detectie van batch](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/batch-response.json)
* [Voorbeeld van de laatste punt detectie antwoord](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/latest-point-response.json)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Naslaginformatie over REST API](https://westus2.dev.cognitive.microsoft.com/docs/services/AnomalyDetector/operations/post-timeseries-entire-detect)