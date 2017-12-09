---
title: Azure Machine Learning-Model Management web service-verbruik | Microsoft Docs
description: "Dit document beschrijft de stappen en -concepten betrokken bij geïmplementeerd met behulp van de model-management in Azure Machine Learning-webservices verbruiken."
services: machine-learning
author: raymondlaghaeian
ms.author: raymondl
manager: neerajkh
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/06/2017
ms.openlocfilehash: 64141afe421ace44fe71c04f8a2fba48144633c9
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="consuming-web-services"></a>Webservices verbruiken
Wanneer u een model als een realtime-webservice implementeert, kunt u gegevens verzenden en voorspellingen ophalen vanaf verschillende platforms en toepassingen. De realtime-webservice wordt een REST-API voor het ophalen van voorspellingen. U kunt gegevens verzenden naar de webservice in de indeling van één of meerdere rij ophalen van een of meer voorspellingen op tegelijk.

Met de [Azure Machine Learning-webservice](https://docs.microsoft.com/azure/machine-learning/preview/model-management-service-deploy), een externe toepassing communiceert synchroon met een Voorspellend model door het maken van HTTP POST-aanroep naar de service-URL. Als u een webservice aanroepen, moet de clienttoepassing specificeren van de API-sleutel die wordt gemaakt wanneer u een voorspelling te implementeren en de gegevens van aanvragen in de aanvraagtekst POST geplaatst.

Houd er rekening mee API-sleutels zijn alleen beschikbaar in de cluster-implementatie-modus. Lokale webservices hoeft geen sleutels.

## <a name="service-deployment-options"></a>Opties voor service-implementatie
Azure Machine Learning-webservices kunnen worden geïmplementeerd in de cloud clusters voor zowel productie- en Testscenario's en lokale werkstations met behulp van docker-engine. De functionaliteit van het voorspellende model in beide gevallen blijft hetzelfde. Implementatie van het cluster op basis van schaalbare biedt en zodat oplossing op basis van Azure Container Services, terwijl de lokale implementatie kan worden gebruikt voor foutopsporing. 

De Azure Machine Learning CLI en API biedt een handige opdrachten voor het maken en beheren van compute omgevingen voor service-implementaties met behulp van de ```az ml env``` optie. 

## <a name="list-deployed-services-and-images"></a>Lijst met geïmplementeerde services en installatiekopieën
U kunt weergeven van de momenteel geïmplementeerde services en de Docker-installatiekopieën met CLI opdracht ```az ml service list realtime -o table```. Houd er rekening mee dat deze opdracht altijd in de context van de huidige compute-omgeving werkt en zou worden services die zijn geïmplementeerd in een omgeving die niet is ingesteld op de huidige worden niet weergegeven. Het gebruik van de omgeving instellen ```az ml env set```. 

## <a name="get-service-information"></a>Service-informatie ophalen
Nadat de web-service is geïmplementeerd, moet u de volgende opdracht gebruiken om op te halen van de service-URL en andere details voor het aanroepen van het service-eindpunt. 

```
az ml service usage realtime -i <service name>
```

Met deze opdracht wordt de service-URL, vereist aanvraagheaders, swagger-URL en voorbeeldgegevens om de service aan te roepen als de service API-schema is opgegeven op het moment van implementatie afgedrukt.

U kunt de service rechtstreeks vanuit de CLI testen zonder het samenstellen van een HTTP-verzoek met de opdracht van de CLI voorbeeld met de invoergegevens:

```
az ml service run realtime -i <service name> -d "Your input data"
```

## <a name="get-the-service-api-key"></a>De service API-sleutel ophalen
Als u de sleutel voor de webservice, gebruikt u de volgende opdracht:

```
az ml service keys realtime -i <web service id>
```
Gebruik de sleutel bij het maken van HTTP-aanvraag in de autorisatie-header: "Autorisatie": "Bearer <key>'

## <a name="get-the-service-swagger-description"></a>De beschrijving van de service Swagger ophalen
Als de service API-schema is opgegeven voor het service-eindpunt zou een Swagger-document op blootstellen ```http://<ip>/api/v1/service/<service name>/swagger.json```. Het Swagger-document kan worden gebruikt om automatisch te genereren van de client-service en Verken de verwachte invoergegevens en andere details over de service.

## <a name="get-service-logs"></a>Servicelogboeken ophalen
Als u wilt meer inzicht in het servicegedrag en analyseren van problemen, zijn er verschillende manieren voor het ophalen van de servicelogboeken:
- CLI-opdracht ```az ml service logs realtime -i <service id>```. Met deze opdracht werkt in zowel als lokale modi.
- Als de service logboekregistratie op de implementatie is ingeschakeld, wordt de service worden ook worden verzonden naar AppInsight. De opdracht CLI ```az ml service usage realtime -i <service id>``` ziet u de URL AppInsight. Houd er rekening mee dat de logboeken AppInsight mogelijk een vertraging door 2-5 minuten.
- Cluster-logboeken kunnen worden bekeken via Kubernetes-console die verbonden is bij het instellen van de huidige clusteromgeving met```az ml env set```
- Lokale docker-logboeken zijn beschikbaar via de docker-enginelogboeken wanneer de service lokaal wordt uitgevoerd.

## <a name="call-the-service-using-c"></a>De service met C# aanroepen
Gebruik de URL een aanvraag te verzenden vanuit een C#-consoletoepassing. 

1. Maak een nieuwe consoletoepassing in Visual Studio: 
    * Klik in het menu, File -> Nieuw Project ->
    * In Visual Studio C#, klikt u op Windows-bureaublad van klasse en selecteer vervolgens Console-App.
2. Voer _MyFirstService_ als de naam van het project, klik vervolgens op OK.
3. Stel in projectverwijzingen, verwijzingen naar _System.Net_, en _System.Net.Http_.
4. Klik op Extra -> NuGet Package Manager -> Package Manager-Console en vervolgens het pakket Microsoft.AspNet.WebApi.Client installeren.
5. Open het bestand Program.cs en vervang de code door de volgende code:
6. Update de _SERVICE_URL_ en _API_KEY_ parameters met de gegevens van uw web-service.
7. Voer het project.

```csharp
using System;
using System.Collections.Generic;
using System.Net.Http;
using System.Net.Http.Headers;
using Newtonsoft.Json;

namespace MyFirstService
{
    class Program
    {
        // Web Service URL (update it with your service url)
        private const string SERVICE_URL = "http://<service ip address>:80/api/v1/service/<service name>/score";
        private const string API_KEY = "your service key";

        static void Main(string[] args)
        {
            Program.PostRequest();
            Console.ReadLine();
        }

        private static void PostRequest()
        {
            HttpClient client = new HttpClient();
            client.BaseAddress = new Uri(SERVICE_URL);
            //For local web service, comment out this line.
            client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", API_KEY);

            var inputJson = new List<RequestPayload>();
            RequestPayload payload = new RequestPayload();
            List<InputDf> inputDf = new List<InputDf>();
            inputDf.Add(new InputDf()
            {
                feature1 = value1,
                feature2 = value2,
            });
            payload.Input_df_list = inputDf;
            inputJson.Add(payload);

            try
            {
                var request = new HttpRequestMessage(HttpMethod.Post, string.Empty);
                request.Content = new StringContent(JsonConvert.SerializeObject(payload));
                request.Content.Headers.ContentType = new MediaTypeHeaderValue("application/json");
                var response = client.SendAsync(request).Result;

                Console.Write(response.Content.ReadAsStringAsync().Result);
            }
            catch (Exception e)
            {
                Console.Out.WriteLine(e.Message);
            }
        }
        public class InputDf
        {
            public double feature1 { get; set; }
            public double feature2 { get; set; }
        }
        public class RequestPayload
        {
            [JsonProperty("input_df")]
            public List<InputDf> Input_df_list { get; set; }
        }
    }
}
```

## <a name="call-the-web-service-using-python"></a>Roep de webservice met behulp van Python
Python gebruiken een aanvraag te verzenden naar uw realtime-webservice. 

1. Kopieer het volgende codevoorbeeld naar een nieuwe Python-bestand.
2. Werk de gegevens, url en api_key parameters. Verwijder de verificatieheader voor lokale webservices.
3. Voer de code. 

```python
import requests
import json

data = "{\"input_df\": [{\"feature1\": value1, \"feature2\": value2}]}"
body = str.encode(json.dumps(data))

url = 'http://<service ip address>:80/api/v1/service/<service name>/score'
api_key = 'your service key' 
headers = {'Content-Type':'application/json', 'Authorization':('Bearer '+ api_key)}

resp = requests.post(url, data, headers=headers)
resp.text
```
