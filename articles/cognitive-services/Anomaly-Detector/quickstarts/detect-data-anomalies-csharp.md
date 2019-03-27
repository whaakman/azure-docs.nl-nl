---
title: 'Quickstart: Detecteer afwijkingen in uw time series-gegevens met de REST API voor Afwijkingsdetectie Detector en C# | Microsoft Docs'
description: Gebruik de API van de detectie van afwijkingen voor het detecteren van afwijkingen in de reeks als een batch of voor streaming-gegevens.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: article
ms.date: 03/26/2019
ms.author: aahi
ms.openlocfilehash: 7aa171a49ea03769c3ecbb5d35ae31ac6fae052e
ms.sourcegitcommit: fbfe56f6069cba027b749076926317b254df65e5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58473156"
---
# <a name="quickstart-detect-anomalies-in-your-time-series-data-using-the-anomaly-detector-rest-api-and-c"></a>Quickstart: Detecteer afwijkingen in uw time series-gegevens met de REST API voor Afwijkingsdetectie Detector enC# 

Gebruik deze Quick Start om te starten met behulp van twee modi voor detectie van afwijkingen Detector-API's voor het detecteren van afwijkingen in uw time series-gegevens. Dit C# toepassing twee API-aanvragen met JSON-indeling time series-gegevens verzendt en de antwoorden opgehaald.

| API-aanvraag                                        | Toepassingsuitvoer                                                                                                                         |
|----------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| Detecteer afwijkingen als batch                        | De JSON-antwoord met de anomaliedetectie-status (en andere gegevens) voor elk gegevenspunt in de time series-gegevens en de posities van alle gedetecteerde afwijkingen. |
| De status van afwijkingen van de meest recente gegevenspunt detecteren | De JSON-antwoord met de anomaliedetectie-status (en andere gegevens) voor de meest recente gegevenspunt in de time series-gegevens.                                                                                                                                         |

 Hoewel deze toepassing in C# is geschreven, is de API een RESTful-webservice die compatibel is met vrijwel elke programmeertaal.

## <a name="prerequisites"></a>Vereisten

- Elke versie van [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/).
- [Newtonsoft.Json](https://www.newtonsoft.com/json)
    - Newtonsoft.Json installeren als een NuGet-pakket in Visual studio:
        1. Klik met de rechtermuisknop op de **oplossing Manager**
        2. Klik op **NuGet-pakketten beheren...**
        3. Zoeken naar `Newtonsoft.Json` en installeer het pakket
- Als u gebruikmaakt van Linux/MacOS, is deze toepassing kan worden uitgevoerd met behulp van [Mono](http://www.mono-project.com/).

- Een JSON-bestand met time series-gegevens verwijst. De voorbeeldgegevens voor deze Quick Start kunt u vinden op [GitHub](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/request-data.json).

[!INCLUDE [cognitive-services-anomaly-detector-data-requirements](../../../../includes/cognitive-services-anomaly-detector-data-requirements.md)]

[!INCLUDE [cognitive-services-anomaly-detector-signup-requirements](../../../../includes/cognitive-services-anomaly-detector-signup-requirements.md)]

## <a name="create-a-new-application"></a>Een nieuwe toepassing maken

1. In Visual Studio, een nieuwe console-oplossing maken en voeg de volgende pakketten. 

    ```csharp
    using System;
    using System.IO;
    using System.Net;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Text;
    using System.Threading.Tasks;
    ```

2. Variabelen voor de abonnementssleutel van uw en uw eindpunt maken. Hieronder vindt u de URI's die u voor detectie van afwijkingen gebruiken kunt. Deze worden toegevoegd aan uw service-eindpunt later het maken van de API aanvraag-URL's.

    |Detectiemethode  |URI  |
    |---------|---------|
    |Detectie van batch    | `/anomalydetector/v1.0/timeseries/entire/detect`        |
    |Detectie op de meest recente gegevenspunt     | `/anomalydetector/v1.0/timeseries/last/detect`        |
    
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

## <a name="create-a-function-to-send-requests"></a>Maak een functie voor het verzenden van aanvragen

1. Maak een nieuwe async-functie met de naam `Request` waarmee de variabelen die eerder is gemaakt.

2. Instellen van de client security-protocol en header-informatie met een `HttpClient` object. Zorg ervoor dat u uw abonnementssleutel toevoegen aan de `Ocp-Apim-Subscription-Key` header. Maak vervolgens een `StringContent` -object voor de aanvraag.
 
3. De aanvraag met verzenden `PostAsync()`. Als de aanvraag geslaagd is, wordt het antwoord retourneren.  

```csharp
static async Task<string> Request(string baseAddress, string endpoint, string subscriptionKey, string requestData){
    using (HttpClient client = new HttpClient { BaseAddress = new Uri(baseAddress) }){
        System.Net.ServicePointManager.SecurityProtocol = SecurityProtocolType.Tls12 | SecurityProtocolType.Tls11 | SecurityProtocolType.Tls;
        client.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
        client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", subscriptionKey);

        var content = new StringContent(requestData, Encoding.UTF8, "application/json");
        var res = await client.PostAsync(endpoint, content);
        if (res.IsSuccessStatusCode){
            return await res.Content.ReadAsStringAsync();
        }
        else{
            return $"ErrorCode: {res.StatusCode}";
        }
    }
}
```

## <a name="detect-anomalies-as-a-batch"></a>Detecteer afwijkingen als batch

1. Maak een nieuwe functie met de naam `detectAnomaliesBatch()`. De aanvraag te maken en te verzenden door het aanroepen van de `Request()` functie met uw eindpunt, abonnementssleutel, de URL voor batch-anomaliedetectie en de time series-gegevens.

2. Het JSON-object te deserialiseren en schrijven naar de console.

3. De posities van afwijkingen vinden in de gegevensset. Van het antwoord `isAnomaly` veld bevat een matrix van Booleaanse waarden, die elk geeft aan of een gegevenspunt een afwijking. Dit account converteren naar een string-matrix met de antwoordobject `ToObject<bool[]>()` functie.

4. Doorlopen van de matrix en afdrukken van de index van een `true` waarden. Deze waarden overeenkomen met de index van afwijkende gegevenspunten, als deze zijn gevonden.

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

    bool[] anomalies = jsonObj["isAnomaly"].ToObject<bool[]>();
    System.Console.WriteLine("\n Anomalies detected in the following data positions:");
    for (var i = 0; i < anomalies.Length; i++) {
        if (anomalies[i]) {
            System.Console.Write(i + ", ");
        }
    }
}
```

## <a name="detect-the-anomaly-status-of-the-latest-data-point"></a>De status van afwijkingen van de meest recente gegevenspunt detecteren

1. Maak een nieuwe functie met de naam `detectAnomaliesLatest()`. De aanvraag te maken en te verzenden door het aanroepen van de `Request()` functie met uw eindpunt, abonnementssleutel, de URL voor de meest recente herstelpunt voor de detectie van afwijkingen en de time series-gegevens.

2. Het JSON-object te deserialiseren en schrijven naar de console. 

```csharp
static void detectAnomaliesLatest(string requestData){
    System.Console.WriteLine("\n\n Determining if latest data point is an anomaly");
    var result = Request(
        endpoint,
        latestPointDetectionUrl,
        subscriptionKey,
        requestData).Result;

    dynamic jsonObj = Newtonsoft.Json.JsonConvert.DeserializeObject(result);
    System.Console.WriteLine(jsonObj);
}
```

## <a name="load-your-time-series-data-and-send-the-request"></a>Uw time series-gegevens laden en de aanvraag verzenden

1. In de belangrijkste methode van uw toepassing laden uw JSON time series-gegevens met `File.ReadAllText()`. 

2. Roep de anomaliedetectie-detectie-functies die eerder is gemaakt. Gebruik `System.Console.ReadKey()` in het consolevenster om open te houden na het uitvoeren van de toepassing.

```csharp
static void Main(string[] args){

    var requestData = File.ReadAllText(dataPath);

    detectAnomaliesBatch(requestData);
    detectAnomaliesLatest(requestData);

    System.Console.ReadKey();
}
```

### <a name="example-response"></a>Voorbeeld van een antwoord

Een geslaagde respons wordt geretourneerd in JSON-indeling. Klik op de onderstaande koppelingen voor het weergeven van het JSON-antwoord op GitHub:
* [Voorbeeld van een antwoord voor de detectie van batch](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/batch-response.json)
* [Voorbeeld van de laatste punt detectie antwoord](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/latest-point-response.json)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Naslaginformatie over REST API](https://westus2.dev.cognitive.microsoft.com/docs/services/AnomalyDetector/operations/post-timeseries-entire-detect)