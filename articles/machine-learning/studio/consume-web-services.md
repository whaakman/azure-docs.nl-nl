---
title: Een Azure Machine Learning-webservice gebruiken | Microsoft Docs
description: "Zodra een machine learning-service is geïmplementeerd, kan de RESTFul-Web-service die beschikbaar wordt gesteld worden gebruikt als realtime aanvragen en antwoorden service of als een batch-service kan worden uitgevoerd."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 804f8211-9437-4982-98e9-ca841b7edf56
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 06/02/2017
ms.author: garye
ms.openlocfilehash: ed41ec58dbdfd41931e8bab3a3cbe33caafe9a74
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2017
---
# <a name="how-to-consume-an-azure-machine-learning-web-service"></a>Een Azure Machine Learning-webservice gebruiken

Wanneer u een Azure Machine Learning Voorspellend model als een webservice implementeert, kunt u een REST-API als u wilt gegevens verzenden en voorspellingen. U kunt de gegevens verzenden in realtime of in de batchmodus.

U vindt meer informatie over het maken en implementeren van een Machine Learning-webservice met Machine Learning Studio hier:

* Zie voor een zelfstudie over het maken van een experiment in Machine Learning Studio [uw eerste experiment maken](create-experiment.md).
* Zie voor meer informatie over het implementeren van een webservice [een Machine Learning-webservice implementeren](publish-a-machine-learning-web-service.md).
* Voor meer informatie over Machine Learning in het algemeen, gaat u naar de [Machine Learning-documentatiecentrum](https://azure.microsoft.com/documentation/services/machine-learning/).

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="overview"></a>Overzicht
Met de webservice Azure Machine Learning communiceert een externe toepassing met een Machine Learning werkstroom score-model in realtime. Aanroep van een Machine Learning-webservice retourneert voorspelling resultaten naar een externe toepassing. Als u een Machine Learning-webservice aanroepen, moet u een API-sleutel die wordt gemaakt bij het implementeren van een voorspelling doorgeven. De Machine Learning-webservice is gebaseerd op REST, een populaire architectuur keuze voor web programming projecten.

Azure Machine Learning heeft twee soorten services:

* Aanvraag en antwoord-Service (RSS) – een lage latentie en zeer schaalbare service die voorziet in een interface voor de staatloze modellen gemaakt en geïmplementeerd vanuit de Machine Learning Studio.
* Batch uitvoering-Service (BES) – een asynchrone service die scores per batch voor records met gegevens.

Zie voor meer informatie over Machine Learning-webservices [een Machine Learning-webservice implementeren](publish-a-machine-learning-web-service.md).

## <a name="get-an-azure-machine-learning-authorization-key"></a>Een Azure Machine Learning-autorisatie-sleutel ophalen
Wanneer u uw experiment implementeert, wordt API-sleutels worden gegenereerd voor de webservice. U kunt de sleutels ophalen vanaf verschillende locaties.

### <a name="from-the-microsoft-azure-machine-learning-web-services-portal"></a>Vanuit de portal voor Microsoft Azure Machine Learning-webservices
Aanmelden bij de [Microsoft Azure Machine Learning-webservices](https://services.azureml.net) portal.

Voor het ophalen van de API-sleutel voor een nieuwe Machine-Learning-webservice:

1. Klik in de portal voor Azure Machine Learning-webservices op **webservices** het bovenste menu.
2. Klik op de webservice waarvan u wilt ophalen van de sleutel.
3. Klik op het bovenste menu **verbruiken**.
4. Kopieer en sla de **primaire sleutel**.

Voor het ophalen van de API-sleutel voor een klassieke Machine-Learning-webservice:

1. Klik in de portal voor Azure Machine Learning-webservices op **klassieke webservices** het bovenste menu.
2. Klik op de Web-service waarmee u werkt.
3. Klik op het eindpunt waarvoor u wilt ophalen van de sleutel.
4. Klik op het bovenste menu **verbruiken**.
5. Kopieer en sla de **primaire sleutel**.

### <a name="classic-web-service"></a>Klassieke-webservice
 U kunt ook een sleutel voor een klassieke webservice ophalen van Machine Learning Studio.

#### <a name="machine-learning-studio"></a>Machine Learning Studio
1. Klik in Machine Learning Studio **WEBSERVICES** aan de linkerkant.
2. Klik op een webservice. De **API-sleutel** is op de **DASHBOARD** tabblad.

## <a id="connect"></a>Verbinding maken met een Machine Learning-webservice
U kunt verbinding maken met een Machine Learning-webservice met behulp van elke programmeertaal die ondersteuning biedt voor HTTP-aanvraag en -antwoord. U kunt de voorbeelden in C#, Python en R weergeven van een help-pagina Machine Learning-webservice.

**Machine Learning API help** Machine Learning API help wordt gemaakt wanneer u een webservice implementeert. Zie [overzicht van Azure Machine Learning - webservice implementeren](walkthrough-5-publish-web-service.md).
De help van Machine Learning API bevat details over een voorspelling webservice.

1. Klik op de Web-service waarmee u werkt.
2. Klik op het eindpunt waarvoor u wilt weergeven van de API Help-pagina.
3. Klik op het bovenste menu **verbruiken**.
4. Klik op **API help-pagina** onder de reactie op aanvragen of een Batchuitvoering eindpunten.

**Help voor een nieuwe Web-service naar de weergave Machine Learning API**

In de [Azure Machine Learning Web Services-Portal](https://services.azureml.net/):

1. Klik op **WEBSERVICES** in het bovenste menu.
2. Klik op de webservice waarvan u wilt ophalen van de sleutel.

Klik op **gebruik webservice** ophalen van de URI's voor de aanvraag Reposonse en Batch uitvoering Services en voorbeeld-code in C#, R en Python.

Klik op **Swagger API** Swagger ophalen op basis van de documentatie voor de API's vanuit de opgegeven URI's aangeroepen.

### <a name="c-sample"></a>C#-voorbeeld
Gebruiken voor verbinding met een Machine Learning-webservice, een **HttpClient** ScoreData wordt doorgegeven. ScoreData bevat een FeatureVector, een n-dimensionale vector van numerieke functies die de ScoreData vertegenwoordigt. U kunt verifiëren met de Machine Learning-service met een API-sleutel.

Verbinding maken met een Machine Learning-webservice, de **Microsoft.AspNet.WebApi.Client** NuGet-pakket moet worden geïnstalleerd.

**Microsoft.AspNet.WebApi.Client NuGet in Visual Studio installeren**

1. De gegevensset downloaden van UCI publiceren: volwassenen 2 klasse gegevensset Web Service.
2. Klik op **Hulpprogramma's** > **NuGet Package Manager** > **Package Manager-console**.
3. Kies **Install-pakket Microsoft.AspNet.WebApi.Client**.

**De voorbeeldcode uitvoeren**

1. Publiceren ' voorbeeld 1: gegevensset downloaden van UCI: volwassene 2 klasse gegevensset ' experiment, deel van de Machine Learning-voorbeeld-verzameling.
2. Wijs apiKey met de sleutel van een webservice. Zie **ophalen van een Azure Machine Learning-autorisatiesleutel** hierboven.
3. Wijs serviceUri met de aanvraag-URI.

**Hier volgt een volledige aanvraag zoeken.**
```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Net.Http;
using System.Net.Http.Formatting;
using System.Net.Http.Headers;
using System.Text;
using System.Threading.Tasks;

namespace CallRequestResponseService
{
    class Program
    {
        static void Main(string[] args)
        {
            InvokeRequestResponseService().Wait();
        }

        static async Task InvokeRequestResponseService()
        {
            using (var client = new HttpClient())
            {
                var scoreRequest = new
                {
                    Inputs = new Dictionary<string, List<Dictionary<string, string>>> () {
                        {
                            "input1",
                            // Replace columns labels with those used in your dataset
                            new List<Dictionary<string, string>>(){new Dictionary<string, string>(){
                                    {
                                        "column1", "value1"
                                    },
                                    {
                                        "column2", "value2"
                                    },
                                    {
                                        "column3", "value3"
                                    }
                                }
                            }
                        },
                    },
                    GlobalParameters = new Dictionary<string, string>() {}
                };

                // Replace these values with your API key and URI found on https://services.azureml.net/
                const string apiKey = "<your-api-key>"; 
                const string apiUri = "<your-api-uri>";
                
                client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue( "Bearer", apiKey);
                client.BaseAddress = new Uri(apiUri);

                // WARNING: The 'await' statement below can result in a deadlock
                // if you are calling this code from the UI thread of an ASP.Net application.
                // One way to address this would be to call ConfigureAwait(false)
                // so that the execution does not attempt to resume on the original context.
                // For instance, replace code such as:
                //      result = await DoSomeTask()
                // with the following:
                //      result = await DoSomeTask().ConfigureAwait(false)

                HttpResponseMessage response = await client.PostAsJsonAsync("", scoreRequest);

                if (response.IsSuccessStatusCode)
                {
                    string result = await response.Content.ReadAsStringAsync();
                    Console.WriteLine("Result: {0}", result);
                }
                else
                {
                    Console.WriteLine(string.Format("The request failed with status code: {0}", response.StatusCode));

                    // Print the headers - they include the requert ID and the timestamp,
                    // which are useful for debugging the failure
                    Console.WriteLine(response.Headers.ToString());

                    string responseContent = await response.Content.ReadAsStringAsync();
                    Console.WriteLine(responseContent);
                }
            }
        }
    }
}
```

### <a name="python-sample"></a>Python-voorbeeld
Gebruiken voor verbinding met een Machine Learning-webservice, de **urllib2** -bibliotheek voor Python 2.X en **urllib.request** -bibliotheek voor Python 3.X. U geeft ScoreData waarin een FeatureVector, een n-dimensionale vector van numerieke functies die de ScoreData vertegenwoordigt. U kunt verifiëren met de Machine Learning-service met een API-sleutel.

**De voorbeeldcode uitvoeren**

1. Implementeren ' voorbeeld 1: gegevensset downloaden van UCI: volwassene 2 klasse gegevensset ' experiment, deel van de Machine Learning-voorbeeld-verzameling.
2. Wijs apiKey met de sleutel van een webservice. Zie de **ophalen van een Azure Machine Learning-autorisatiesleutel** sectie in het begin van dit artikel.
3. Wijs serviceUri met de aanvraag-URI.

**Hier volgt een volledige aanvraag zoeken.**
```python
import urllib2 # urllib.request for Python 3.X
import json

data = {
    "Inputs": {
        "input1":
        [
            {
                'column1': "value1",   
                'column2': "value2",   
                'column3': "value3"
            }
        ],
    },
    "GlobalParameters":  {}
}

body = str.encode(json.dumps(data))

# Replace this with the URI and API Key for your web service
url = '<your-api-uri>'
api_key = '<your-api-key>'
headers = {'Content-Type':'application/json', 'Authorization':('Bearer '+ api_key)}

# "urllib.request.Request(uri, body, headers)" for Python 3.X
req = urllib2.Request(url, body, headers)

try:
    # "urllib.request.urlopen(req)" for Python 3.X
    response = urllib2.urlopen(req)

    result = response.read()
    print(result)
# "urllib.error.HTTPError as error" for Python 3.X
except urllib2.HTTPError, error: 
    print("The request failed with status code: " + str(error.code))

    # Print the headers - they include the requert ID and the timestamp, which are useful for debugging the failure
    print(error.info())
    print(json.loads(error.read())) 
```

### <a name="r-sample"></a>R-voorbeeld

Gebruiken voor verbinding met een Machine Learning-webservice, de **RCurl** en **rjson** bibliotheken maken van de aanvraag en verwerken van het geretourneerde JSON-antwoord. U geeft ScoreData waarin een FeatureVector, een n-dimensionale vector van numerieke functies die de ScoreData vertegenwoordigt. U kunt verifiëren met de Machine Learning-service met een API-sleutel.

**Hier volgt een volledige aanvraag zoeken.**
```r
library("RCurl")
library("rjson")

# Accept SSL certificates issued by public Certificate Authorities
options(RCurlOptions = list(cainfo = system.file("CurlSSL", "cacert.pem", package = "RCurl")))

h = basicTextGatherer()
hdr = basicHeaderGatherer()

req = list(
    Inputs = list(
            "input1" = list(
                list(
                        'column1' = "value1",
                        'column2' = "value2",
                        'column3' = "value3"
                    )
            )
        ),
        GlobalParameters = setNames(fromJSON('{}'), character(0))
)

body = enc2utf8(toJSON(req))
api_key = "<your-api-key>" # Replace this with the API key for the web service
authz_hdr = paste('Bearer', api_key, sep=' ')

h$reset()
curlPerform(url = "<your-api-uri>",
httpheader=c('Content-Type' = "application/json", 'Authorization' = authz_hdr),
postfields=body,
writefunction = h$update,
headerfunction = hdr$update,
verbose = TRUE
)

headers = hdr$value()
httpStatus = headers["status"]
if (httpStatus >= 400)
{
print(paste("The request failed with status code:", httpStatus, sep=" "))

# Print the headers - they include the requert ID and the timestamp, which are useful for debugging the failure
print(headers)
}

print("Result:")
result = h$value()
print(fromJSON(result))
```

### <a name="javascript-sample"></a>JavaScript-voorbeeld

Gebruiken voor verbinding met een Machine Learning-webservice, de **aanvraag** npm-pakket in uw project. U zult ook gebruiken de `JSON` object notatie voor uw invoer en parseren van het resultaat. Installeren met behulp van `npm install request --save`, of Voeg `"request": "*"` naar uw package.json onder `dependencies` en voer `npm install`.

**Hier volgt een volledige aanvraag zoeken.**
```js
let req = require("request");

const uri = "<your-api-uri>";
const apiKey = "<your-api-key>";

let data = {
    "Inputs": {
        "input1":
        [
            {
                'column1': "value1",
                'column2': "value2",
                'column3': "value3"
            }
        ],
    },
    "GlobalParameters": {}
}

const options = {
    uri: uri,
    method: "POST",
    headers: {
        "Content-Type": "application/json",
        "Authorization": "Bearer " + apiKey,
    },
    body: JSON.stringify(data)
}

req(options, (err, res, body) => {
    if (!err && res.statusCode == 200) {
        console.log(body);
    } else {
        console.log("The request failed with status code: " + res.statusCode);
    }
});
```