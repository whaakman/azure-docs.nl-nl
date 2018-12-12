---
title: Geïmplementeerde webservices gebruiken
titleSuffix: Azure Machine Learning service
description: Leer hoe u een webservice die is gegenereerd wanneer een model is geïmplementeerd met Azure Machine Learning-model te gebruiken. De webservice die een REST-API beschikbaar stelt. Clients voor deze API met behulp van de programmeertaal van uw keuze maken.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: raymondl
author: raymondlaghaeian
ms.reviewer: larryfr
ms.date: 12/03/2018
ms.custom: seodec18
ms.openlocfilehash: 0cf585ec3eb95b71080436791fd47d96239dfa9f
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53100438"
---
# <a name="consume-an-azure-machine-learning-model-deployed-as-a-web-service"></a>Een Azure Machine Learning-model dat is geïmplementeerd als een webservice gebruiken

Een Azure Machine Learning-model als een webservice implementeren, maakt u een REST-API. U kunt gegevens verzenden naar deze API en ontvangen de voorspelling geretourneerd door het model. In dit document, informatie over het maken van clients voor de web service via C#, Go, Java en Python.

Een webservice wordt gemaakt wanneer u een installatiekopie op een Azure Container Instance, Azure Kubernetes Service of Project Brainwave (veld programmable gate arrays implementeert). Installatiekopieën worden van geregistreerde modellen en scoring-bestanden gemaakt. De URI die wordt gebruikt voor toegang tot een webservice kan worden opgehaald met behulp van de [SDK van Azure Machine Learning](https://aka.ms/aml-sdk). Als verificatie is ingeschakeld, kunt u de SDK ook gebruiken om op te halen van de verificatiesleutels.

De algemene werkstroom wanneer het maken van een client die gebruikmaakt van een ML-webservice is:

1. De SDK gebruiken om de verbindingsgegevens ophalen
1. Bepalen welk type gegevens van aanvragen die worden gebruikt door het model
1. Maak een toepassing die de webservice-aanroepen

## <a name="connection-information"></a>Verbindingsgegevens

> [!NOTE]
> De SDK van Azure Machine Learning wordt gebruikt om de web service-informatie ophalen. Dit is een Python-SDK. Terwijl deze wordt gebruikt om informatie over de web-services te halen, kunt u elke taal kunt gebruiken om een client voor de service te maken.

De verbindingsgegevens van de web service kan worden opgehaald met de SDK van Azure Machine Learning. De [azureml.core.Webservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py) klasse bevat de informatie die nodig zijn voor het maken van een client. De volgende `Webservice` eigenschappen die nuttig zijn bij het maken van een clienttoepassing:

* `auth_enabled` -Als verificatie is ingeschakeld, `True`; anders `False`.
* `scoring_uri` -De REST-API-adres.

Er zijn een drie manieren om op te halen van deze informatie voor de geïmplementeerde webservices:

* Wanneer u een model implementeert, een `Webservice` object wordt geretourneerd met informatie over de service:

    ```python
    service = Webservice.deploy_from_model(name='myservice',
                                           deployment_config=myconfig,
                                           models=[model],
                                           image_config=image_config,
                                           workspace=ws)
    print(service.scoring_uri)
    ```

* U kunt `Webservice.list` geïmplementeerd om op te halen een lijst van webservices voor modellen in uw werkruimte. U kunt filters toevoegen om te beperken van de lijst met geretourneerde informatie. Zie voor meer informatie over wat kan worden gefilterd op, de [Webservice.list](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice.webservice?view=azure-ml-py#list) referentiedocumentatie.

    ```python
    services = Webservice.list(ws)
    print(services[0].scoring_uri)
    ```

* Als u de naam van de geïmplementeerde service weet, kunt u een nieuw exemplaar van `Webservice` en geef de naam van de werkruimte en de service als parameters. Het nieuwe object bevat informatie over de geïmplementeerde service.

    ```python
    service = Webservice(workspace=ws, name='myservice')
    print(service.scoring_uri)
    ```

### <a name="authentication-key"></a>Verificatiesleutel

Verificatiesleutels worden automatisch gemaakt als verificatie is ingeschakeld voor een implementatie.

* Verificatie is __standaard ingeschakeld__ bij het implementeren op __Azure Kubernetes Service__.
* Verificatie is __standaard uitgeschakeld__ bij het implementeren op __Azure container Instances__.

Voor het beheren van verificatie, gebruikt u de `auth_enabled` parameter bij het maken of bijwerken van een implementatie.

Als verificatie is ingeschakeld, kunt u de `get_keys` methode voor het ophalen van een primaire en secundaire verificatiesleutel:

```python
primary, secondary = service.get_keys()
print(primary)
```

> [!IMPORTANT]
> Als u een sleutel opnieuw genereren wilt, gebruikt u [ `service.regen_key` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#regen-key).

## <a name="request-data"></a>Gegevens van aanvragen

De REST-API wordt verwacht dat de hoofdtekst van het verzoek om te worden van een JSON-document met de volgende structuur:

```json
{
    "data":
        [
            <model-specific-data-structure>
        ]
}
```

> [!IMPORTANT]
> De structuur van de gegevens moet overeenkomen met wat de scoring-script en het model in de verwachte service. Het scoring-script kan de gegevens wijzigen voordat deze wordt doorgegeven aan het model.

Bijvoorbeeld, het model in de [Train binnen notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-within-notebook/train-within-notebook.ipynb) voorbeeld wordt een matrix van 10 cijfers verwacht. Het scoring-script in dit voorbeeld maakt een matrix Numpy van de aanvraag en doorgegeven aan het model. Het volgende voorbeeld ziet u de gegevens die deze service wordt verwacht dat:

```json
{
    "data": 
        [
            [
                0.0199132141783263, 
                0.0506801187398187, 
                0.104808689473925, 
                0.0700725447072635, 
                -0.0359677812752396, 
                -0.0266789028311707, 
                -0.0249926566315915, 
                -0.00259226199818282, 
                0.00371173823343597, 
                0.0403433716478807
            ]
        ]
}
``` 

De webservice kan meerdere sets met gegevens in één aanvraag accepteren. Retourneert een JSON-document met een matrix van antwoorden.

### <a name="binary-data"></a>Binaire gegevens

Als uw model binaire gegevens, zoals een afbeelding accepteert, moet u wijzigen de `score.py` bestand dat wordt gebruikt voor uw implementatie om onbewerkte HTTP-aanvragen te accepteren. Hier volgt een voorbeeld van een `score.py` die binaire gegevens accepteert en retourneert de omgekeerde bytes voor POST-aanvragen. Voor GET-aanvragen retourneert deze de volledige URL in de hoofdtekst van antwoord:

```python 
from azureml.contrib.services.aml_request  import AMLRequest, rawhttp
from azureml.contrib.services.aml_response import AMLResponse

def init():
    print("This is init()")

@rawhttp
def run(request):
    print("This is run()")
    print("Request: [{0}]".format(request))
    if request.method == 'GET':
        respBody = str.encode(request.full_path)
        return AMLResponse(respBody, 200)
    elif request.method == 'POST':
        reqBody = request.get_data(False)
        respBody = bytearray(reqBody)
        respBody.reverse()
        respBody = bytes(respBody)
        return AMLResponse(respBody, 200)
    else:
        return AMLResponse("bad request", 500)
```

> [!IMPORTANT]
> Dingen die u in de `azureml.contrib` naamruimte regelmatig wordt gewijzigd terwijl we er als u wilt de service te verbeteren. In deze naamruimte moet als zodanig worden beschouwd als een Preview-versie en niet volledig ondersteund door Microsoft.
>
> Als u testen op uw lokale ontwikkelomgeving wilt, kunt u de onderdelen installeren op de contrib-naamruimte met de volgende opdracht:
> 
> ```shell
> pip install azureml-contrib-services
> ```

## <a name="call-the-service-c"></a>De service aanroepen (C#)

In dit voorbeeld ziet u hoe u C# voor het aanroepen van de webservice gemaakt op basis van de [Train binnen notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-within-notebook/train-within-notebook.ipynb) voorbeeld:

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Net.Http;
using System.Net.Http.Headers;
using Newtonsoft.Json;

namespace MLWebServiceClient
{
    // The data structure expected by the service
    internal class InputData
    {
        [JsonProperty("data")]
        // The service used by this example expects an array containing
        //   one or more arrays of doubles
        internal double[,] data;
    }
    class Program
    {
        static void Main(string[] args)
        {
            // Set the scoring URI and authentication key
            string scoringUri = "<your web service URI>";
            string authKey = "<your key>";

            // Set the data to be sent to the service.
            // In this case, we are sending two sets of data to be scored.
            InputData payload = new InputData();
            payload.data = new double[,] {
                {
                    0.0199132141783263,
                    0.0506801187398187,
                    0.104808689473925,
                    0.0700725447072635,
                    -0.0359677812752396,
                    -0.0266789028311707,
                    -0.0249926566315915,
                    -0.00259226199818282,
                    0.00371173823343597,
                    0.0403433716478807
                },
                {
                    -0.0127796318808497, 
                    -0.044641636506989, 
                    0.0606183944448076, 
                    0.0528581912385822, 
                    0.0479653430750293, 
                    0.0293746718291555, 
                    -0.0176293810234174, 
                    0.0343088588777263, 
                    0.0702112981933102, 
                    0.00720651632920303
                }
            };

            // Create the HTTP client
            HttpClient client = new HttpClient();
            // Set the auth header. Only needed if the web service requires authentication.
            client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", authKey);

            // Make the request
            try {
                var request = new HttpRequestMessage(HttpMethod.Post, new Uri(scoringUri));
                request.Content = new StringContent(JsonConvert.SerializeObject(payload));
                request.Content.Headers.ContentType = new MediaTypeHeaderValue("application/json");
                var response = client.SendAsync(request).Result;
                // Display the response from the web service
                Console.WriteLine(response.Content.ReadAsStringAsync().Result);
            }
            catch (Exception e)
            {
                Console.Out.WriteLine(e.Message);
            }
        }
    }
}
```

De geretourneerde resultaten zijn vergelijkbaar met de volgende JSON-document:

```json
[217.67978776218715, 224.78937091757172]
```

## <a name="call-the-service-go"></a>De service (Ga) aanroepen

In dit voorbeeld ziet u hoe u met behulp van Go voor het aanroepen van de webservice gemaakt op basis van de [Train binnen notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-within-notebook/train-within-notebook.ipynb) voorbeeld:

```go
package main

import (
    "bytes"
    "encoding/json"
    "fmt"
    "io/ioutil"
    "net/http"
)

// Features for this model are an array of decimal values
type Features []float64

// The web service input can accept multiple sets of values for scoring
type InputData struct {
    Data []Features `json:"data",omitempty`
}

// Define some example data
var exampleData = []Features{
    []float64{
        0.0199132141783263, 
        0.0506801187398187, 
        0.104808689473925, 
        0.0700725447072635, 
        -0.0359677812752396, 
        -0.0266789028311707, 
        -0.0249926566315915, 
        -0.00259226199818282, 
        0.00371173823343597, 
        0.0403433716478807,
    },
    []float64{
        -0.0127796318808497, 
        -0.044641636506989, 
        0.0606183944448076, 
        0.0528581912385822, 
        0.0479653430750293, 
        0.0293746718291555, 
        -0.0176293810234174, 
        0.0343088588777263, 
        0.0702112981933102, 
        0.00720651632920303,
    },
}

// Set to the URI for your service
var serviceUri string = "<your web service URI>"
// Set to the authentication key (if any) for your service
var authKey string = "<your key>"

func main() {
    // Create the input data from example data
    jsonData := InputData{
        Data: exampleData,
    }
    // Create JSON from it and create the body for the HTTP request
    jsonValue, _ := json.Marshal(jsonData)
    body := bytes.NewBuffer(jsonValue)

    // Create the HTTP request
    client := &http.Client{}
    request, err := http.NewRequest("POST", serviceUri, body)
    request.Header.Add("Content-Type", "application/json")

    // These next two are only needed if using an authentication key
    bearer := fmt.Sprintf("Bearer %v", authKey)
    request.Header.Add("Authorization", bearer)

    // Send the request to the web service
    resp, err := client.Do(request)
    if err != nil {
        fmt.Println("Failure: ", err)
    }

    // Display the response received
    respBody, _ := ioutil.ReadAll(resp.Body)
    fmt.Println(string(respBody))
}
```

De geretourneerde resultaten zijn vergelijkbaar met de volgende JSON-document:

```json
[217.67978776218715, 224.78937091757172]
```

## <a name="call-the-service-java"></a>Roept de service (Java)

In dit voorbeeld laat zien hoe u Java gebruikt voor het aanroepen van de webservice gemaakt op basis van de [Train binnen notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-within-notebook/train-within-notebook.ipynb) voorbeeld:

```java
import java.io.IOException;
import org.apache.http.client.fluent.*;
import org.apache.http.entity.ContentType;
import org.json.simple.JSONArray;
import org.json.simple.JSONObject;

public class App {
    // Handle making the request
    public static void sendRequest(String data) {
        // Replace with the scoring_uri of your service
        String uri = "<your web service URI>";
        // If using authentication, replace with the auth key
        String key = "<your key>";
        try {
            // Create the request
            Content content = Request.Post(uri)
            .addHeader("Content-Type", "application/json")
            // Only needed if using authentication
            .addHeader("Authorization", "Bearer " + key)
            // Set the JSON data as the body
            .bodyString(data, ContentType.APPLICATION_JSON)
            // Make the request and display the response.
            .execute().returnContent();
            System.out.println(content);
        }
        catch (IOException e) {
            System.out.println(e);
        }
    }
    public static void main(String[] args) {
        // Create the data to send to the service
        JSONObject obj = new JSONObject();
        // In this case, it's an array of arrays
        JSONArray dataItems = new JSONArray();
        // Inner array has 10 elements
        JSONArray item1 = new JSONArray();
        item1.add(0.0199132141783263);
        item1.add(0.0506801187398187);
        item1.add(0.104808689473925);
        item1.add(0.0700725447072635);
        item1.add(-0.0359677812752396);
        item1.add(-0.0266789028311707);
        item1.add(-0.0249926566315915);
        item1.add(-0.00259226199818282);
        item1.add(0.00371173823343597);
        item1.add(0.0403433716478807);
        // Add the first set of data to be scored
        dataItems.add(item1);
        // Create and add the second set
        JSONArray item2 = new JSONArray();
        item2.add(-0.0127796318808497);
        item2.add(-0.044641636506989);
        item2.add(0.0606183944448076);
        item2.add(0.0528581912385822);
        item2.add(0.0479653430750293);
        item2.add(0.0293746718291555);
        item2.add(-0.0176293810234174);
        item2.add(0.0343088588777263);
        item2.add(0.0702112981933102);
        item2.add(0.00720651632920303);
        dataItems.add(item2);
        obj.put("data", dataItems);

        // Make the request using the JSON document string
        sendRequest(obj.toJSONString());
    }
}
```

De geretourneerde resultaten zijn vergelijkbaar met de volgende JSON-document:

```json
[217.67978776218715, 224.78937091757172]
```

## <a name="call-the-service-python"></a>De service (Python) aanroepen

In dit voorbeeld laat zien hoe u Python gebruikt voor het aanroepen van de webservice gemaakt op basis van de [Train binnen notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-within-notebook/train-within-notebook.ipynb) voorbeeld:

```python
import requests
import requests
import json

# URL for the web service
scoring_uri = '<your web service URI>'
# If the service is authenticated, set the key
key = '<your key>'

# Two sets of data to score, so we get two results back
data = {"data": 
            [
                [
                    0.0199132141783263, 
                    0.0506801187398187, 
                    0.104808689473925, 
                    0.0700725447072635, 
                    -0.0359677812752396, 
                    -0.0266789028311707, 
                    -0.0249926566315915, 
                    -0.00259226199818282, 
                    0.00371173823343597, 
                    0.0403433716478807
                ],
                [
                    -0.0127796318808497, 
                    -0.044641636506989, 
                    0.0606183944448076, 
                    0.0528581912385822, 
                    0.0479653430750293, 
                    0.0293746718291555, 
                    -0.0176293810234174, 
                    0.0343088588777263, 
                    0.0702112981933102, 
                    0.00720651632920303]
            ]
        }
# Convert to JSON string
input_data = json.dumps(data)

# Set the content type
headers = { 'Content-Type':'application/json' }
# If authentication is enabled, set the authorization header
headers['Authorization']=f'Bearer {key}'

# Make the request and display the response
resp = requests.post(scoring_uri, input_data, headers = headers)
print(resp.text)

```

De geretourneerde resultaten zijn vergelijkbaar met de volgende JSON-document:

```JSON
[217.67978776218715, 224.78937091757172]
```
