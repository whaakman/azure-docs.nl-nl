---
title: 'Quickstart: Digitale inkt met de REST-API van Ink herkenning herkennen enC#'
description: De Ink-herkenning-API gebruiken om te beginnen met het herkennen van digitale inkt lijnen.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: ink-recognizer
ms.topic: quickstart
ms.date: 05/02/2019
ms.author: aahi
ms.openlocfilehash: d661d6eca6e4916946944c48cc2e5411aeaf8f14
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67060989"
---
# <a name="quickstart-recognize-digital-ink-with-the-ink-recognizer-rest-api-and-c"></a>Quickstart: Digitale inkt met de REST-API van Ink herkenning herkennen enC#

Gebruik deze Quick Start om te beginnen met digitale inkt lijnen verzenden naar de API Ink-herkenning. Dit C# toepassing een API-aanvraag met JSON-indeling ink lijn gegevens verzendt en het antwoord wordt opgehaald.

Hoewel deze toepassing in C# is geschreven, is de API een RESTful-webservice die compatibel is met vrijwel elke programmeertaal.

Doorgaans wilt u de API aanroepen vanuit een digitale bij app. In deze snelstartgids verzendt ink lijn gegevens voor het volgende voorbeeld van handgeschreven vanuit een JSON-bestand.

![een afbeelding van handgeschreven tekst](../media/handwriting-sample.jpg)

De broncode voor deze Quick Start kunt u vinden op [GitHub](https://go.microsoft.com/fwlink/?linkid=2089502).

## <a name="prerequisites"></a>Vereisten

- Elke versie van [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/).
- [Newtonsoft.Json](https://www.newtonsoft.com/json)
    - Newtonsoft.Json installeren als een NuGet-pakket in Visual studio:
        1. Klik met de rechtermuisknop op de **oplossing Manager**
        2. Klik op **NuGet-pakketten beheren...**
        3. Zoeken naar `Newtonsoft.Json` en installeer het pakket
- Als u gebruikmaakt van Linux/MacOS, is deze toepassing kan worden uitgevoerd met behulp van [Mono](https://www.mono-project.com/).

- De voorbeeld ink lijn gegevens voor deze Quick Start kunt u vinden op [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/dotnet/InkRecognition/quickstart/example-ink-strokes.json).

[!INCLUDE [cognitive-services-ink-recognizer-signup-requirements](../../../../includes/cognitive-services-ink-recognizer-signup-requirements.md)]


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
    using Newtonsoft.Json;
    using Newtonsoft.Json.Linq;
    ```

2. Variabelen voor de abonnementssleutel van uw en uw eindpunt maken. Hieronder ziet u de URI die u voor inktherkenning van Windows gebruiken kunt. Deze zullen worden toegevoegd aan uw service-eindpunt later voor het maken van de API aanvraag-URl.

    ```csharp
    // Replace the subscriptionKey string with your valid subscription key.
    const string subscriptionKey = "YOUR_SUBSCRIPTION_KEY";

    // Replace the dataPath string with a path to the JSON formatted ink stroke data.
    const string dataPath = @"PATH-TO-INK-STROKE-DATA"; 

    // URI information for ink recognition:
    const string endpoint = "https://api.cognitive.microsoft.com";
    const string inkRecognitionUrl = "/inkrecognizer/v1.0-preview/recognize";
    ```

## <a name="create-a-function-to-send-requests"></a>Maak een functie voor het verzenden van aanvragen

1. Maak een nieuwe async-functie met de naam `Request` waarmee de variabelen die eerder is gemaakt.

2. Instellen van de client security-protocol en header-informatie met een `HttpClient` object. Zorg ervoor dat u uw abonnementssleutel toevoegen aan de `Ocp-Apim-Subscription-Key` header. Maak vervolgens een `StringContent` -object voor de aanvraag.
 
3. De aanvraag met verzenden `PutAsync()`. Als de aanvraag geslaagd is, wordt het antwoord retourneren.  
    
    ```csharp
    static async Task<string> Request(string apiAddress, string endpoint, string subscriptionKey, string requestData){
        
        using (HttpClient client = new HttpClient { BaseAddress = new Uri(apiAddress) }){
            System.Net.ServicePointManager.SecurityProtocol = SecurityProtocolType.Tls12 | SecurityProtocolType.Tls11 | SecurityProtocolType.Tls;
            client.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", subscriptionKey);

            var content = new StringContent(requestData, Encoding.UTF8, "application/json");
            var res = await client.PutAsync(endpoint, content);
            if (res.IsSuccessStatusCode){
                return await res.Content.ReadAsStringAsync();
            }
            else{
                return $"ErrorCode: {res.StatusCode}";
            }
        }
    }
    ```

## <a name="send-an-ink-recognition-request"></a>Een aanvraag van de spraakherkenning ink verzenden

1. Maak een nieuwe functie met de naam `recognizeInk()`. De aanvraag te maken en te verzenden door het aanroepen van de `Request()` functie aan uw eindpunt, abonnementssleutel, de URL voor de API en de gegevens van de lijn digitale ink.

2. Het JSON-object te deserialiseren en schrijven naar de console. 
    
    ```csharp
    static void recognizeInk(string requestData){

        //construct the request
        var result = Request(
            endpoint,
            inkRecognitionUrl,
            subscriptionKey,
            requestData).Result;

        dynamic jsonObj = Newtonsoft.Json.JsonConvert.DeserializeObject(result);
        System.Console.WriteLine(jsonObj);
    }
    ```

## <a name="load-your-digital-ink-data"></a>Uw digitale Inktgegevens laden

Maak een functie met de naam `LoadJson()` laden van het JSON-bestand van de ink-gegevens. Gebruik een `StreamReader` en `JsonTextReader` maken een `JObject` en retourneer het.
    
```csharp
public static JObject LoadJson(string fileLocation){

    var jsonObj = new JObject();

    using (StreamReader file = File.OpenText(fileLocation))
    using (JsonTextReader reader = new JsonTextReader(file)){
        jsonObj = (JObject)JToken.ReadFrom(reader);
    }
    return jsonObj;
}
```

## <a name="send-the-api-request"></a>De API-aanvraag verzenden

1. In de belangrijkste methode van uw toepassing, uw JSON-gegevens worden geladen met de functie die eerder is gemaakt. 

2. Roep de `recognizeInk()` functie hierboven hebt gemaakt. Gebruik `System.Console.ReadKey()` in het consolevenster om open te houden na het uitvoeren van de toepassing.
    
    ```csharp
    static void Main(string[] args){

        var requestData = LoadJson(dataPath);
        string requestString = requestData.ToString(Newtonsoft.Json.Formatting.None);
        recognizeInk(requestString);
        System.Console.WriteLine("\nPress any key to exit ");
        System.Console.ReadKey();
        }
    ```

## <a name="run-the-application-and-view-the-response"></a>Voer de toepassing en de reactie weergeven

Voer de toepassing uit. Een geslaagde respons wordt geretourneerd in JSON-indeling. U kunt ook het JSON-antwoord vinden op [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/dotnet/InkRecognition/quickstart/example-response.json).


## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Naslaginformatie over REST API](https://go.microsoft.com/fwlink/?linkid=2089907)


Als u wilt zien hoe de Ink-API in een digitale bij app werkt, bekijk de volgende voorbeeldtoepassingen op GitHub:
* [C# en Universal Windows Platform (UWP)](https://go.microsoft.com/fwlink/?linkid=2089803)  
* [C# en Windows Presentation Foundation (WPF)](https://go.microsoft.com/fwlink/?linkid=2089804)
* [Webbrowser-app (Javascript)](https://go.microsoft.com/fwlink/?linkid=2089908)       
* [Mobiele app (Java en Android)](https://go.microsoft.com/fwlink/?linkid=2089906)
* [Mobiele app (Swift en iOS)](https://go.microsoft.com/fwlink/?linkid=2089805)
