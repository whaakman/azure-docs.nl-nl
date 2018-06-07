---
title: 'Voorbeeld: Een aangepaste kwalificatie maken in de pipeline cognitieve zoeken (Azure Search) | Microsoft Docs'
description: Laat zien met de API voor het omzetten van tekst in aangepaste kwalificatie toegewezen aan een cognitieve zoekopdracht in de pijplijn in Azure Search indexeren.
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 056cff192b25068fa2e895fd46d143a834b7af0b
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34641081"
---
# <a name="example-create-a-custom-skill-using-the-text-translate-api"></a>Voorbeeld: Een aangepaste kwalificatie met de tekst vertalen API maken

Informatie over het maken van een web API aangepaste kwalificatie die accepteert tekst in elke taal en zet deze om naar het Engels in dit voorbeeld. In het voorbeeld wordt een [Azure-functie](https://azure.microsoft.com/services/functions/) inpakken van de [vertalen tekst API](https://azure.microsoft.com/services/cognitive-services/translator-text-api/) zodat deze de aangepaste kwalificatie-interface implementeert.

## <a name="prerequisites"></a>Vereisten

+ Meer informatie over [interface voor aangepaste kwalificatie](cognitive-search-custom-skill-interface.md) artikel als u niet bekend bent met de i/o-interface die een aangepaste kwalificatie moet implementeren.

+ [Aanmelden voor de API van de tekst conversieprogramma](../cognitive-services/translator/translator-text-how-to-signup.md), en een API-sleutel gebruiken voor het ophalen.

+ Installeer [Visual Studio 2017 versie 15,5](https://www.visualstudio.com/vs/) of hoger, waaronder de werkbelasting ontwikkelen van Azure.

## <a name="create-an-azure-function"></a>Een Azure-functie maken

Hoewel dit voorbeeld wordt een Azure-functie voor het hosten van een web-API gebruikt, is het niet vereist.  Als u voldoet aan de [interface-vereisten voor een kwalificatie cognitieve](cognitive-search-custom-skill-interface.md), de methode die u van belang is. Azure Functions maakt het echter eenvoudig te maken van een aangepaste kwalificatie.

### <a name="create-a-function-app"></a>Een functie-app maken

1. Selecteer in Visual Studio **nieuw** > **Project** in het menu bestand.

1. Selecteer in het dialoogvenster New Project **geïnstalleerde**, vouw **Visual C#** > **Cloud**, selecteer **Azure Functions**, typ een Naam voor uw project en selecteer **OK**. De functie-appnaam moet geldig zijn als een C#-naamruimte. Gebruik dus geen onderstrepingstekens, afbreekstreepjes of andere niet-alfanumerieke tekens.

1. Selecteer het type moet **HTTP-Trigger**

1. U kunt selecteren voor Storage-Account **geen**, zoals u geen opslag voor deze functie hoeft.

1. Selecteer **OK** voor het maken van de functie-project en HTTP-functie geactiveerd.

### <a name="modify-the-code-to-call-the-translate-cognitive-service"></a>De code voor het aanroepen van de vertalen cognitieve Service wijzigen

Visual Studio maakt een project met daarin een klasse die standaardcode voor het gekozen functietype bevat. Met het kenmerk *FunctionName* in de methode wordt de naam van de functie ingesteld. Met het kenmerk *HttpTrigger* wordt aangegeven dat de functie wordt geactiveerd door een HTTP-aanvraag.

Vervang alle inhoud van het bestand nu, *Function1.cs* met de volgende code:

```csharp
using System.IO;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.AspNetCore.Http;
using Microsoft.Azure.WebJobs.Host;
using Newtonsoft.Json;
using System.Net.Http;
using System.Collections.Generic;
using System.Threading.Tasks;
using System.Text;

namespace TranslateFunction
{
    // This function will simply translate messages sent to it.
    public static class Function1
    {
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


        /// <summary>
        /// Note that this function can translate up to 1000 characters. If you expect to need to translate more characters, use 
        /// the paginator skill before calling this custom enricher
        /// </summary>
        [FunctionName("Translate")]
        public static IActionResult Run(
            [HttpTrigger(AuthorizationLevel.Function, "post", Route = null)]HttpRequest req, 
            TraceWriter log)
        {
            log.Info("C# HTTP trigger function processed a request.");

            string recordId = null;
            string originalText = null;
            string originalLanguage = null;
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
            originalLanguage = data?.values?.First?.data?.language?.Value as string;

            if (recordId == null)
            {
                return new BadRequestObjectResult("recordId cannot be null");
            }

            // Only translate records that actually need to be translated. 
            if (!originalLanguage.Contains("en"))
            {
                translatedText = TranslateText(originalText, "en-us").Result;
            }
            else
            {
                // text is already in English.
                translatedText = originalText;
            }

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
        /// <param name="myText">The text to translate</param>
        /// <param name="destinationLanguage">The language you want to translate to.</param>
        /// <returns>Asynchronous task that returns the translated text. </returns>
        async static Task<string> TranslateText(string myText, string destinationLanguage)
        {
            string host = "https://api.microsofttranslator.com";
            string path = "/V2/Http.svc/Translate";

            // NOTE: Replace this example key with a valid subscription key.
            string key = "064d8095730d4a99b49f4bcf16ac67f8";

            HttpClient client = new HttpClient();
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", key);

            List<KeyValuePair<string, string>> list = new List<KeyValuePair<string, string>>() {
                new KeyValuePair<string, string>(myText, "en-us")
            };

            StringBuilder totalResult = new StringBuilder();

            foreach (KeyValuePair<string, string> i in list)
            {
                string uri = host + path + "?to=" + i.Value + "&text=" + System.Net.WebUtility.UrlEncode(i.Key);

                HttpResponseMessage response = await client.GetAsync(uri);

                string result = await response.Content.ReadAsStringAsync();

                // Parse the response XML
                System.Xml.XmlDocument xmlResponse = new System.Xml.XmlDocument();
                xmlResponse.LoadXml(result);
                totalResult.Append(xmlResponse.InnerText); 
            }

            return totalResult.ToString();
        }
    }
}
```

Zorg ervoor dat u Voer uw eigen *sleutel* waarde in de *TranslateText* methode op basis van de sleutel die u hebt verkregen bij het aanmelden voor de tekst vertalen API.

In dit voorbeeld is een eenvoudige enricher die alleen op één record tegelijk werkt. Dit wordt later belangrijk bij het instellen van de batchgrootte van de voor de vaardigheden.

## <a name="test-the-function-from-visual-studio"></a>De functie vanuit Visual Studio testen

Druk op **F5** om uit te voeren van het gedrag van de functie programma's en testen. Postman of Fiddler gebruiken bij het verlenen van een aanroep van zoals hieronder weergegeven:

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
               "language": "es"
            }
        }
   ]
}
```
### <a name="response"></a>Antwoord
U ziet een reactie vergelijkbaar met het volgende voorbeeld:

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

Wanneer u tevreden met het gedrag van de functie bent, kunt u deze hebt gepubliceerd.

1. Klik in **Solution Explorer** met de rechtermuisknop op het project en selecteer **Publiceren**. Kies **maken van nieuwe** > **publiceren**.

1. Als u dit nog niet hebt Visual Studio al met uw Azure-account verbonden, selecteert u **account toevoegen...**

1. Volg de aanwijzingen op. U wordt gevraagd om op te geven van het Azure-account, de resourcegroep, het plan hosting en het opslagaccount dat u wilt gebruiken. U kunt een nieuwe resourcegroep, een nieuw plan hosting en een opslagaccount maken als u dit nog niet hebt. Wanneer u klaar bent, selecteert u **maken**

1. Nadat de implementatie voltooid is, Let op de Site-URL. Het is het adres van de functie-app in Azure. 

1. In de [Azure-portal](https://portal.azure.com), navigeer naar de resourcegroep en zoekt u naar de functie vertalen u gepubliceerd. Onder de **beheren** sectie ziet u sleutels Host. Selecteer de **kopie** pictogram voor de *standaard* host-sleutel.  


## <a name="test-the-function-in-azure"></a>De functie testen in Azure

Nu dat u de standaardsleutel voor de host hebt, test u de functie als volgt:

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
               "language": "es"
            }
        }
   ]
}
```

Dit moet een vergelijkbaar resultaat het nummer dat u eerder hebt gezien wanneer de functie wordt uitgevoerd in de lokale omgeving produceren.

## <a name="connect-to-your-pipeline"></a>Verbinding maken met uw pijplijn
Nu dat u een nieuwe aangepaste vaardigheden hebt, kunt u deze toevoegen aan uw vaardigheden. Het volgende voorbeeld ziet u hoe de kwalificatie niet aanroepen. Omdat de kwalificatie geen batches verwerkt, Voeg een instructie voor het maximale batchgrootte moet NET ```1``` documenten één tegelijk verzenden.

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
            "source": "/document/languageCode"
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
Gefeliciteerd! U hebt uw eerste aangepaste enricher gemaakt. Nu kunt u hetzelfde patroon om toe te voegen uw eigen aangepaste volgen. 

+ [Een aangepaste kwalificatie toevoegen aan een pijplijn cognitieve zoeken](cognitive-search-custom-skill-interface.md)
+ [Het definiëren van een vaardigheden](cognitive-search-defining-skillset.md)
+ [Vaardigheden (REST) maken](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
+ [Hoe verrijkt velden toe te wijzen](cognitive-search-output-field-mapping.md)