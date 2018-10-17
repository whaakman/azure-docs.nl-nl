---
title: 'Voorbeeld: Maak een aangepaste vaardigheid in pijplijn cognitief zoeken (Azure Search) | Microsoft Docs'
description: Ziet u met behulp van de API voor het vertalen van tekst in aangepaste vaardigheden toegewezen aan een cognitief zoeken pijplijn in Azure Search indexeren.
manager: pablocas
author: luiscabrer
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 06/29/2018
ms.author: luisca
ms.openlocfilehash: 4dfc4d250cae80e7837db5cfbcc3af15390b2bb4
ms.sourcegitcommit: 8e06d67ea248340a83341f920881092fd2a4163c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49352333"
---
# <a name="example-create-a-custom-skill-using-the-text-translate-api"></a>Voorbeeld: Maak een aangepaste vaardigheden met behulp van de API tekst vertalen

In dit voorbeeld wordt informatie over het maken van een web API aangepaste vaardigheden die tekst in een andere taal wordt geaccepteerd en wordt omgezet in het Engels. Het voorbeeld wordt een [Azure Function](https://azure.microsoft.com/services/functions/) het verpakken van de [vertalen tekst-API](https://azure.microsoft.com/services/cognitive-services/translator-text-api/) zodat deze de aangepaste vaardigheden-interface implementeert.

## <a name="prerequisites"></a>Vereisten

+ Meer informatie over [aangepaste vaardigheden interface](cognitive-search-custom-skill-interface.md) artikel als u niet bekend bent met de i/o-interface die een aangepaste vaardigheden te implementeren.

+ [Aanmelden voor de Translator Text-API](../cognitive-services/translator/translator-text-how-to-signup.md), en ontvang een API-sleutel gebruikt.

+ Installeer [Visual Studio 2017 versie 15.5](https://www.visualstudio.com/vs/) of hoger, waaronder de Azure-ontwikkelworkload.

## <a name="create-an-azure-function"></a>Een Azure-functie maken

Hoewel dit voorbeeld wordt een Azure-functie voor het hosten van een web-API gebruikt, is het niet vereist.  Zolang u voldoet aan de [interface-vereisten voor een cognitieve vaardigheden](cognitive-search-custom-skill-interface.md), benadering u van belang is. Azure Functions, echter gemakkelijk te maken van een aangepaste vaardigheden.

### <a name="create-a-function-app"></a>Een functie-app maken

1. Selecteer in Visual Studio, **nieuw** > **Project** in het menu bestand.

1. Selecteer in het dialoogvenster Nieuw Project **geïnstalleerde**, vouw **Visual C#** > **Cloud**, selecteer **Azure Functions**, typ een Naam voor uw project en selecteer **OK**. De functie-appnaam moet geldig zijn als een C#-naamruimte. Gebruik dus geen onderstrepingstekens, afbreekstreepjes of andere niet-alfanumerieke tekens.

1. Selecteer **Azure Functions-v2 (.Net Core)**. U kunt het ook doen met versie 1, maar de code hieronder is gebaseerd op de v2-sjabloon.

1. Selecteer het type moet **HTTP-Trigger**

1. U kunt selecteren voor Storage-Account **geen**, zoals u niet alle opslag voor deze functie hoeft.

1. Selecteer **OK** om de functie te maken project- en HTTP-geactiveerde functie.

### <a name="modify-the-code-to-call-the-translate-cognitive-service"></a>De code voor het aanroepen van de vertalen Cognitive Service wijzigen

Visual Studio maakt een project met daarin een klasse die standaardcode voor het gekozen functietype bevat. Met het kenmerk *FunctionName* in de methode wordt de naam van de functie ingesteld. Met het kenmerk *HttpTrigger* wordt aangegeven dat de functie wordt geactiveerd door een HTTP-aanvraag.

Vervang de inhoud van het bestand nu, *Function1.cs* door de volgende code:

```csharp
using System;
using System.Net.Http;
using System.Collections.Generic;
using System.Threading.Tasks;
using System.IO;
using System.Text;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.AspNetCore.Http;
using Microsoft.Azure.WebJobs.Host;
using Newtonsoft.Json;

namespace TranslateFunction
{
    // This function will simply translate messages sent to it.
    public static class Function1
    {
        static string path = "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0";

        // NOTE: Replace this example key with a valid subscription key.
        static string key = "<enter your api key here>";

        #region classes used to serialize the response
        private class WebApiResponseError
        {
            public string message { get; set; }
        }

        private class WebApiResponseWarning
        {
            public string message { get; set; }
        }

        private class WebApiResponseRecord
        {
            public string recordId { get; set; }
            public Dictionary<string, object> data { get; set; }
            public List<WebApiResponseError> errors { get; set; }
            public List<WebApiResponseWarning> warnings { get; set; }
        }

        private class WebApiEnricherResponse
        {
            public List<WebApiResponseRecord> values { get; set; }
        }
        #endregion

        [FunctionName("Translate")]
        public static IActionResult Run(
            [HttpTrigger(AuthorizationLevel.Function, "post", Route = null)]HttpRequest req,
            TraceWriter log)
        {
            log.Info("C# HTTP trigger function processed a request.");

            string recordId = null;
            string originalText = null;
            string toLanguage = null;
            string translatedText = null;

            string requestBody = new StreamReader(req.Body).ReadToEnd();
            dynamic data = JsonConvert.DeserializeObject(requestBody);

            // Validation
            if (data?.values == null)
            {
                return new BadRequestObjectResult(" Could not find values array");
            }
            if (data?.values.HasValues == false || data?.values.First.HasValues == false)
            {
                // It could not find a record, then return empty values array.
                return new BadRequestObjectResult(" Could not find valid records in values array");
            }

            recordId = data?.values?.First?.recordId?.Value as string;
            originalText = data?.values?.First?.data?.text?.Value as string;
            toLanguage = data?.values?.First?.data?.language?.Value as string;

            if (recordId == null)
            {
                return new BadRequestObjectResult("recordId cannot be null");
            }

            translatedText = TranslateText(originalText, toLanguage).Result;
        
            // Put together response.
            WebApiResponseRecord responseRecord = new WebApiResponseRecord();
            responseRecord.data = new Dictionary<string, object>();
            responseRecord.recordId = recordId;
            responseRecord.data.Add("text", translatedText);

            WebApiEnricherResponse response = new WebApiEnricherResponse();
            response.values = new List<WebApiResponseRecord>();
            response.values.Add(responseRecord);

            return (ActionResult)new OkObjectResult(response);
        }


        /// <summary>
        /// Use Cognitive Service to translate text from one language to antoher.
        /// </summary>
        /// <param name="originalText">The text to translate.</param>
        /// <param name="toLanguage">The language you want to translate to.</param>
        /// <returns>Asynchronous task that returns the translated text. </returns>
        async static Task<string> TranslateText(string originalText, string toLanguage)
        {
            System.Object[] body = new System.Object[] { new { Text = originalText } };
            var requestBody = JsonConvert.SerializeObject(body);

            var uri = $"{path}&to={toLanguage}";

            string result = "";

            using (var client = new HttpClient())
            using (var request = new HttpRequestMessage())
            {
                request.Method = HttpMethod.Post;
                request.RequestUri = new Uri(uri);
                request.Content = new StringContent(requestBody, Encoding.UTF8, "application/json");
                request.Headers.Add("Ocp-Apim-Subscription-Key", key);

                var response = await client.SendAsync(request);
                var responseBody = await response.Content.ReadAsStringAsync();

                dynamic data = JsonConvert.DeserializeObject(responseBody);
                result = data?.First?.translations?.First?.text?.Value as string;

            }
            return result;
        }
    }
}
```

Zorg ervoor dat u Voer uw eigen *sleutel* waarde in de *TranslateText* methode op basis van de sleutel die u hebt verkregen bij het aanmelden voor de tekst vertalen API.

In dit voorbeeld is een eenvoudige enricher die alleen geschikt is voor één record op een tijdstip. Dit wordt later belangrijk bij het instellen van de batchgrootte van de voor de vaardigheden.

## <a name="test-the-function-from-visual-studio"></a>Test de functie vanuit Visual Studio

Druk op **F5** om uit te voeren van het gedrag van de functie programma en test. In dit geval gebruiken we de functie hieronder om te vertalen tekst in het Spaans naar het Engels. Postman of Fiddler gebruiken om uit te geven van een aanroep, zoals hieronder wordt weergegeven:

```http
POST https://localhost:7071/api/Translate
```
### <a name="request-body"></a>Aanvraagtekst
```json
{
   "values": [
        {
            "recordId": "a1",
            "data":
            {
               "text":  "Este es un contrato en Inglés",
               "language": "en"
            }
        }
   ]
}
```
### <a name="response"></a>Antwoord
Hier ziet u een reactie vergelijkbaar met het volgende voorbeeld:

```json
{
    "values": [
        {
            "recordId": "a1",
            "data": {
                "text": "This is a contract in English"
            },
            "errors": null,
            "warnings": null
        }
    ]
}
```

## <a name="publish-the-function-to-azure"></a>De functie publiceren naar Azure

Wanneer u tevreden met het gedrag van de functie bent, kunt u deze kunt publiceren.

1. Klik in **Solution Explorer** met de rechtermuisknop op het project en selecteer **Publiceren**. Kies **Maak een nieuwe** > **publiceren**.

1. Als u dit nog niet hebt Visual Studio al met uw Azure-account verbonden, selecteert u **een account toevoegen...**

1. Volg de aanwijzingen op. U wordt gevraagd om op te geven van het Azure-account, de resourcegroep, het hostingabonnement en het opslagaccount dat u wilt gebruiken. U kunt een nieuwe resourcegroep, een nieuw hosting plan en een storage-account maken als u dit nog niet hebt. Wanneer u klaar bent, selecteert u **maken**

1. Als de implementatie voltooid is, noteert u de Site-URL. Dit is het adres van uw functie-app in Azure. 

1. In de [Azure-portal](https://portal.azure.com), gaat u naar de resourcegroep en zoek naar de functie vertalen u gepubliceerd. Onder de **beheren** sectie ziet u Hostsleutels. Selecteer de **kopie** pictogram voor de *standaard* host-sleutel.  

## <a name="update-ssl-settings"></a>SSL-instellingen bijwerken

Alle Azure-functies die zijn gemaakt na 30 juni 2018 uitgeschakeld TLS 1.0, die is momenteel niet compatibel met aangepaste vaardigheden.

1. In de [Azure-portal](https://portal.azure.com), gaat u naar de resourcegroep en zoek naar de functie vertalen u gepubliceerd. Onder de **platformfuncties** sectie ziet u SSL.

1. Na het selecteren van SSL, moet u de **minimale TLS-versie** 1.0. TLS 1.2-functies zijn nog niet ondersteund als aangepaste vaardigheden.

## <a name="test-the-function-in-azure"></a>De functie testen in Azure

Nu dat u de standaard-hostsleutel hebt, test u de functie als volgt:

```http
POST https://translatecogsrch.azurewebsites.net/api/Translate?code=[enter default host key here]
```
### <a name="request-body"></a>Aanvraagtekst
```json
{
   "values": [
        {
            "recordId": "a1",
            "data":
            {
               "text":  "Este es un contrato en Inglés",
               "language": "en"
            }
        }
   ]
}
```

Dit moet een soortgelijk resultaat op die u eerder hebt gezien bij het uitvoeren van de functie in de lokale omgeving produceren.

## <a name="connect-to-your-pipeline"></a>Verbinding maken met uw pijplijn
Nu dat u een nieuwe aangepaste vaardigheden hebt, kunt u deze kunt toevoegen aan uw vaardigheden uit. Het volgende voorbeeld ziet u over het aanroepen van de kwalificatie. Omdat de kwalificatie geen batches verwerkt, toevoegen van een instructie voor het maximale batchgrootte moet alleen ```1``` documenten voor het verzenden van een op een tijdstip.

```json
{
    "skills": [
      ...,  
      {
        "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
        "description": "Our new translator custom skill",
        "uri": "http://translatecogsrch.azurewebsites.net/api/Translate?code=[enter default host key here]",
        "batchSize":1,
        "context": "/document",
        "inputs": [
          {
            "name": "text",
            "source": "/document/content"
          },
          {
            "name": "language",
            "source": "/document/destinationLanguage"
          }
        ],
        "outputs": [
          {
            "name": "text",
            "targetName": "translatedText"
          }
        ]
      }
  ]
}
```

## <a name="next-steps"></a>Volgende stappen
Gefeliciteerd! U hebt uw eerste aangepaste enricher gemaakt. U kunt nu hetzelfde patroon om toe te voegen van de functionaliteit van uw eigen aangepaste volgen. 

+ [Een aangepaste vaardigheid toevoegen aan een pijplijn cognitief zoeken](cognitive-search-custom-skill-interface.md)
+ [Hoe u een set vaardigheden definiëren](cognitive-search-defining-skillset.md)
+ [Vaardighedenset (REST) maken](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
+ [Verrijkt velden toewijzen](cognitive-search-output-field-mapping.md)
