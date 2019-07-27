---
title: 'Quickstart: Detecteer afwijkingen in uw time series-gegevens met behulp van de anomalie detectie REST API enC#'
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
ms.openlocfilehash: 97efa5cd91646809178d685ca51e29ef2fda7c0d
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68564734"
---
# <a name="quickstart-detect-anomalies-in-your-time-series-data-using-the-anomaly-detector-rest-api-and-c"></a>Quickstart: Detecteer afwijkingen in uw time series-gegevens met behulp van de anomalie detectie REST API enC# 

Gebruik deze Quick Start om de twee detectie modi van de anomalie detectie-API te gebruiken voor het detecteren van afwijkingen in uw time series-gegevens. Met C# deze toepassing worden twee API-aanvragen verzonden met tijdreeks gegevens in JSON-indeling en worden de antwoorden opgehaald.

| API-aanvraag                                        | Toepassingsuitvoer                                                                                                                         |
|----------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| Afwijkingen als een batch detecteren                        | Het JSON-antwoord met de afwijkings status (en andere gegevens) voor elk gegevens punt in de tijdreeks gegevens en de posities van gedetecteerde afwijkingen. |
| De afwijkings status van het laatste gegevens punt detecteren | Het JSON-antwoord met de afwijkings status (en andere gegevens) voor het laatste gegevens punt in de time series-gegevens.                                                                                                                                         |

 Hoewel deze toepassing in C# is geschreven, is de API een RESTful-webservice die compatibel is met vrijwel elke programmeertaal.

## <a name="prerequisites"></a>Vereisten

- Elke versie van [Visual Studio 2017 of hoger](https://visualstudio.microsoft.com/downloads/),

- Het [Json.NET](https://www.newtonsoft.com/json)-framework, beschikbaar als NuGet-pakket. Newton soft. json installeren als een NuGet-pakket in Visual Studio:
    
    1. Klik met de rechter muisknop op uw project in **Solution Explorer**.
    2. Selecteer **NuGet-pakketten beheren**.
    3. Zoek naar *Newton soft. json* en installeer het pakket.

- Als u Linux/MacOS gebruikt, kan deze toepassing worden uitgevoerd met behulp van [mono](https://www.mono-project.com/).

- Een JSON-bestand met gegevens punten van de tijd reeks. De voorbeeld gegevens voor deze Quick Start vindt u op [github](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/request-data.json).

[!INCLUDE [cognitive-services-anomaly-detector-data-requirements](../../../../includes/cognitive-services-anomaly-detector-data-requirements.md)]

[!INCLUDE [cognitive-services-anomaly-detector-signup-requirements](../../../../includes/cognitive-services-anomaly-detector-signup-requirements.md)]

## <a name="create-a-new-application"></a>Een nieuwe toepassing maken

1. Maak in Visual Studio een nieuwe console oplossing en voeg de volgende pakketten toe. 

    ```csharp
    using System;
    using System.IO;
    using System.Net;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Text;
    using System.Threading.Tasks;
    ```

2. Maak variabelen voor uw abonnements sleutel en uw eind punt. Hieronder vindt u de Uri's die u voor anomalie detectie kunt gebruiken. Deze worden later toegevoegd aan uw service-eind punt om de API-aanvraag-Url's te maken.

    |Detectie methode  |URI  |
    |---------|---------|
    |Batch detectie    | `/anomalydetector/v1.0/timeseries/entire/detect`        |
    |Detectie op het laatste gegevens punt     | `/anomalydetector/v1.0/timeseries/last/detect`        |
    
    ```csharp
    // Replace the subscriptionKey string value with your valid subscription key.
    const string subscriptionKey = "[YOUR_SUBSCRIPTION_KEY]";
    // Replace the endpoint URL with the correct one for your subscription. 
    // Your endpoint can be found in the Azure portal. For example: https://westus2.api.cognitive.microsoft.com
    const string endpoint = "[YOUR_ENDPOINT_URL]";
    // Replace the dataPath string with a path to the JSON formatted time series data.
    const string dataPath = "[PATH_TO_TIME_SERIES_DATA]";
    const string latestPointDetectionUrl = "/anomalydetector/v1.0/timeseries/last/detect";
    const string batchDetectionUrl = "/anomalydetector/v1.0/timeseries/entire/detect";
    ```

## <a name="create-a-function-to-send-requests"></a>Een functie maken om aanvragen te verzenden

1. Maak een nieuwe async-functie `Request` met de naam die de hierboven gemaakte variabelen accepteert.

2. Het beveiligings protocol en de header gegevens van de client instellen `HttpClient` met behulp van een-object. Zorg ervoor dat u uw abonnements sleutel aan de `Ocp-Apim-Subscription-Key` koptekst toevoegt. Maak vervolgens een `StringContent` object voor de aanvraag.

3. Verzend de aanvraag met `PostAsync()`en retour neer het antwoord.

```csharp
static async Task<string> Request(string apiAddress, string endpoint, string subscriptionKey, string requestData){
    using (HttpClient client = new HttpClient { BaseAddress = new Uri(apiAddress) }){
        System.Net.ServicePointManager.SecurityProtocol = SecurityProtocolType.Tls12 | SecurityProtocolType.Tls11 | SecurityProtocolType.Tls;
        client.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
        client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", subscriptionKey);

        var content = new StringContent(requestData, Encoding.UTF8, "application/json");
        var res = await client.PostAsync(endpoint, content);
        return await res.Content.ReadAsStringAsync();
    }
}
```

## <a name="detect-anomalies-as-a-batch"></a>Afwijkingen als een batch detecteren

1. Maak een nieuwe functie met `detectAnomaliesBatch()`de naam. Maak de aanvraag en verzend deze door de `Request()` functie aan te roepen met het eind punt, de abonnements sleutel, de URL voor de batch afwijkings detectie en de tijdreeks gegevens.

2. Deserialiseren van het JSON-object en schrijf het naar de-console.

3. Als het antwoord veld `code` bevat, drukt u de fout code en het fout bericht af. 

4. Als dat niet het geval is, kunt u de positie van afwijkingen vinden in de gegevensset. Het `isAnomaly` veld van de reactie bevat een matrix met Boole-waarden, die elk aangeeft of een gegevens punt een afwijkend is. Converteer deze naar een teken reeks matrix met de functie van `ToObject<bool[]>()` het Response-object. Herhaal de matrix en druk de index van alle `true` waarden af. Deze waarden komen overeen met de index van afwijkende gegevens punten, als deze zijn gevonden.

```csharp
static void detectAnomaliesBatch(string requestData){
    System.Console.WriteLine("Detecting anomalies as a batch");

    var result = Request(
        endpoint,
        batchDetectionUrl,
        subscriptionKey,
        requestData).Result;

    dynamic jsonObj = Newtonsoft.Json.JsonConvert.DeserializeObject(result);
    System.Console.WriteLine(jsonObj);

    if (jsonObj["code"] != null){
        System.Console.WriteLine($"Detection failed. ErrorCode:{jsonObj["code"]}, ErrorMessage:{jsonObj["message"]}");
    }
    else{
        bool[] anomalies = jsonObj["isAnomaly"].ToObject<bool[]>();
        System.Console.WriteLine("\nAnomalies detected in the following data positions:");
        for (var i = 0; i < anomalies.Length; i++){
            if (anomalies[i])
            {
                System.Console.Write(i + ", ");
            }
        }
    }
}
```

## <a name="detect-the-anomaly-status-of-the-latest-data-point"></a>De afwijkings status van het laatste gegevens punt detecteren

1. Maak een nieuwe functie met `detectAnomaliesLatest()`de naam. Maak de aanvraag en verzend deze door de `Request()` functie aan te roepen met uw eind punt, abonnements sleutel, de URL voor de laatste detectie van punt afwijkingen en de tijdreeks gegevens.

2. Deserialiseren van het JSON-object en schrijf het naar de-console.

```csharp
static void detectAnomaliesLatest(string requestData){
    System.Console.WriteLine("\n\nDetermining if latest data point is an anomaly");
    var result = Request(
        endpoint,
        latestPointDetectionUrl,
        subscriptionKey,
        requestData).Result;

    dynamic jsonObj = Newtonsoft.Json.JsonConvert.DeserializeObject(result);
    System.Console.WriteLine(jsonObj);
}
```

## <a name="load-your-time-series-data-and-send-the-request"></a>Laad uw time series-gegevens en verzend de aanvraag

1. In de hoofd methode van uw toepassing laadt u uw JSON Time Series- `File.ReadAllText()`gegevens met. 

2. Roep de hierboven gemaakte anomalie detectie functies aan. Gebruiken `System.Console.ReadKey()` om te voor komen dat het console venster wordt geopend nadat de toepassing is uitgevoerd.

```csharp
static void Main(string[] args){

    var requestData = File.ReadAllText(dataPath);

    detectAnomaliesBatch(requestData);
    detectAnomaliesLatest(requestData);

    System.Console.ReadKey();
}
```

### <a name="example-response"></a>Voorbeeld van een antwoord

Een geslaagde reactie wordt geretourneerd in JSON-indeling. Klik op de onderstaande koppelingen om het JSON-antwoord op GitHub weer te geven:
* [Voor beeld van een antwoord op een batch detectie](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/batch-response.json)
* [Voor beeld van laatste punt detectie respons](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/latest-point-response.json)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Naslaginformatie over REST API](https://westus2.dev.cognitive.microsoft.com/docs/services/AnomalyDetector/operations/post-timeseries-entire-detect)
