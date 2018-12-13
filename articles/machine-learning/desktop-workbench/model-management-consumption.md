---
title: Azure Machine Learning Modelbeheer web service-verbruik | Microsoft Docs
description: Dit document beschrijft de stappen en concepten die betrokken zijn bij het gebruik van webservices die zijn geïmplementeerd met behulp van Modelbeheer in Azure Machine Learning.
services: machine-learning
author: aashishb
ms.author: aashishb
manager: hjerez
ms.reviewer: jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 09/06/2017
ROBOTS: NOINDEX
ms.openlocfilehash: bbd36e5389208b21704f1749beee016e30cc4ec5
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2018
ms.locfileid: "53253847"
---
# <a name="consuming-web-services"></a>Webservices gebruiken

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 


Wanneer u een model als een realtime webservice implementeert, kunt u gegevens verzenden en voorspellingen ophalen uit een verscheidenheid aan platforms en toepassingen. De realtime webservice wordt aangegeven dat een REST-API voor het ophalen van voorspellingen. U kunt gegevens verzenden naar de webservice in de indeling één of meerdere rijen aan een of meer voorspellingen op een tijdstip.

Met de [Azure Machine Learning-webservice](model-management-service-deploy.md), een externe toepassing communiceert synchroon met een Voorspellend model dat door HTTP POST-aanroep naar de service-URL. Als u een webservice aanroepen, moet de clienttoepassing om op te geven van de API-sleutel die wordt gemaakt wanneer u een voorspelling te implementeren en de gegevens van aanvragen in de hoofdtekst van de POST-aanvraag geplaatst.

> [!NOTE]
> Houd er rekening mee API-sleutels zijn alleen beschikbaar in de clustermodus voor implementatie. Lokale web-services hebben geen sleutels.

## <a name="service-deployment-options"></a>Service-implementatie-opties
Azure Machine Learning-webservices kunnen worden geïmplementeerd naar de cloud gebaseerde clusters voor productie- en Testscenario's en lokale werkstations met behulp van docker-engine. De functionaliteit van het voorspellende model in beide gevallen blijft hetzelfde. Implementatie op basis van een cluster biedt schaalbare en goed presterende oplossing op basis van Azure Container Services, terwijl de lokale implementatie kan worden gebruikt voor het opsporen van fouten. 

De Azure Machine Learning CLI en de API biedt handige opdrachten voor het maken en beheren van compute-omgevingen voor service-implementaties met behulp van de ```az ml env``` optie. 

## <a name="list-deployed-services-and-images"></a>Lijst met services en -afbeeldingen geïmplementeerd
U kunt een lijst de momenteel geïmplementeerde services en de Docker-installatiekopieën met behulp van CLI-opdracht ```az ml service list realtime -o table```. Houd er rekening mee dat met deze opdracht altijd in de context van de huidige compute-omgeving werkt. Deze weergegeven services die zijn geïmplementeerd in een omgeving die niet is ingesteld op de huidige worden niet. Het instellen van het gebruik van de omgeving ```az ml env set```. 

## <a name="get-service-information"></a>Service-informatie ophalen
Nadat de web-service is geïmplementeerd, gebruikt u de volgende opdracht uit om op te halen van de service-URL en andere details voor het aanroepen van het service-eindpunt. 

```
az ml service usage realtime -i <web service id>
```

Met deze opdracht wordt de service-URL, vereist aanvraagheaders, swagger-URL en voorbeeldgegevens voor het aanroepen van de service als de service-API-schema is opgegeven op het moment van implementatie afgedrukt.

U kunt de service rechtstreeks vanuit de CLI kunt testen zonder het samenstellen van een HTTP-aanvraag door te voeren van de voorbeeld-CLI-opdracht met de ingevoerde gegevens:

```
az ml service run realtime -i <web service id> -d "Your input data"
```

## <a name="get-the-service-api-key"></a>De service-API-sleutel ophalen
Als u de sleutel voor de webservice, gebruikt u de volgende opdracht uit:

```
az ml service keys realtime -i <web service id>
```
Bij het maken van HTTP-aanvraag, gebruikt u de sleutel in de autorisatie-header: 'Autorisatie': "Bearer <key>"

## <a name="get-the-service-swagger-description"></a>De beschrijving van de Swagger service ophalen
Als het schema van de service-API is opgegeven, het service-eindpunt beschikbaar zou maken van een Swagger-document op ```http://<ip>/api/v1/service/<service name>/swagger.json```. De Swagger-document kan worden gebruikt om automatisch genereren van de service-client en de verwachte invoer- en andere details over de service te verkennen.

## <a name="get-service-logs"></a>Servicelogboeken ophalen
Begrijp het servicegedrag van de en problemen diagnosticeren, zijn er verschillende manieren om op te halen van de servicelogboeken:
- CLI-opdracht ```az ml service logs realtime -i <service id>```. Met deze opdracht werkt in het cluster en de lokale modus.
- Als de service logboekregistratie is ingeschakeld tijdens de implementatie, is de servicelogboeken worden ook verzonden naar AppInsight. De CLI-opdracht ```az ml service usage realtime -i <service id>``` ziet u de AppInsight-URL. Houd er rekening mee dat de logboeken AppInsight kunnen worden vertraagd door 2-5 minuten.
- Clusterlogboeken kunnen worden weergegeven in het Kubernetes-console die verbonden is bij het instellen van de huidige clusteromgeving met ```az ml env set```
- Lokale docker-logboeken zijn beschikbaar via de logboeken van docker-engine als de service lokaal wordt uitgevoerd.

## <a name="call-the-service-using-c"></a>Roept de service met behulp van C#
De service-URL gebruiken voor het verzenden van een aanvraag van een C#-consoletoepassing. 

1. Maak een nieuwe Console-App in Visual Studio: 
    * In het menu, klikt u op, File -> Nieuw Project ->
    * In Visual Studio C#, klikt u op Windows-klasse bureaublad, en selecteer vervolgens Console-App.
2. Voer `MyFirstService` als de naam van het project en klik vervolgens op OK.
3. In de projectverwijzingen, stelt u verwijzingen naar `System.Net`, en `System.Net.Http`.
4. Klik op hulpprogramma's-NuGet Package Manager > Package Manager Console -> en vervolgens installeren de **Microsoft.AspNet.WebApi.Client** pakket.
5. Open **Program.cs** bestand en vervang de code door de volgende code:
6. Update de `SERVICE_URL` en `API_KEY` parameters met de gegevens van uw webservice.
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
Python gebruiken om te verzenden van een aanvraag naar de realtime webservice. 

1. Kopieer het volgende codevoorbeeld naar een nieuwe Python-bestand.
2. Werk de gegevens, de url en de api_key parameters. Voor lokale webservices, verwijdert u de autorisatieheader.
3. Voer de code uit. 

```python
import requests
import json

data = "{\"input_df\": [{\"feature1\": value1, \"feature2\": value2}]}"
body = str.encode(json.dumps(data))

url = 'http://<service ip address>:80/api/v1/service/<service name>/score'
api_key = 'your service key' 
headers = {'Content-Type':'application/json', 'Authorization':('Bearer '+ api_key)}

resp = requests.post(url, body, headers=headers)
resp.text
```
