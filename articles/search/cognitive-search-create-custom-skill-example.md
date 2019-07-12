---
title: 'Voorbeeld: Het maken van een aangepaste cognitieve vaardigheden met de Bing Entity Search API - Azure Search'
description: Ziet u in een aangepaste vaardigheden toegewezen aan een indexering pijplijn cognitief zoeken in Azure Search met behulp van de Bing Entity Search-service.
manager: pablocas
author: luiscabrer
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: 7d90f46ada9b9453b4c1516a4a898456dc73b8e7
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67672151"
---
# <a name="example-create-a-custom-skill-using-the-bing-entity-search-api"></a>Voorbeeld: Een aangepaste vaardigheden met behulp van de Bing Entity Search API maken

In dit voorbeeld wordt informatie over het maken van een web API-aangepaste vaardigheden. Deze vaardigheid accepteert locaties, openbare afbeeldingen en organisaties, en beschrijvingen voor hen retourneren. Het voorbeeld wordt een [Azure Function](https://azure.microsoft.com/services/functions/) het verpakken van de [Bing Entity Search API](https://azure.microsoft.com/services/cognitive-services/bing-entity-search-api/) zodat deze de aangepaste vaardigheden-interface implementeert.

## <a name="prerequisites"></a>Vereisten

+ Meer informatie over [aangepaste vaardigheden interface](cognitive-search-custom-skill-interface.md) artikel als u niet bekend bent met de i/o-interface die een aangepaste vaardigheden te implementeren.

+ [!INCLUDE [cognitive-services-bing-entity-search-signup-requirements](../../includes/cognitive-services-bing-entity-search-signup-requirements.md)]

+ Installeer [Visual Studio 2019](https://www.visualstudio.com/vs/) of hoger, waaronder de Azure-ontwikkelworkload.

## <a name="create-an-azure-function"></a>Een Azure-functie maken

Hoewel dit voorbeeld wordt een Azure-functie voor het hosten van een web-API gebruikt, is het niet vereist.  Zolang u voldoet aan de [interface-vereisten voor een cognitieve vaardigheden](cognitive-search-custom-skill-interface.md), benadering u van belang is. Azure Functions, echter gemakkelijk te maken van een aangepaste vaardigheden.

### <a name="create-a-function-app"></a>Een functie-app maken

1. Selecteer in Visual Studio, **nieuw** > **Project** in het menu bestand.

1. Selecteer in het dialoogvenster Nieuw Project **geïnstalleerde**, vouw **Visual C#**  > **Cloud**, selecteer **Azure Functions**, typ een Naam voor uw project en selecteer **OK**. De functie-appnaam moet geldig zijn als een C# naamruimte, gebruik dus geen onderstrepingstekens, afbreekstreepjes of andere niet-alfanumerieke tekens.

1. Selecteer **Azure Functions-v2 (.NET Core)** . U kunt het ook doen met versie 1, maar de code hieronder is gebaseerd op de v2-sjabloon.

1. Selecteer het type moet **HTTP-Trigger**

1. U kunt selecteren voor Storage-Account **geen**, zoals u niet alle opslag voor deze functie hoeft.

1. Selecteer **OK** om de functie te maken project- en HTTP-geactiveerde functie.

### <a name="modify-the-code-to-call-the-bing-entity-search-service"></a>Wijzig de code voor het aanroepen van de Bing Entity Search-Service

Visual Studio maakt een project met daarin een klasse die standaardcode voor het gekozen functietype bevat. Met het kenmerk *FunctionName* in de methode wordt de naam van de functie ingesteld. Met het kenmerk *HttpTrigger* wordt aangegeven dat de functie wordt geactiveerd door een HTTP-aanvraag.

Vervang de inhoud van het bestand nu, *Function1.cs* door de volgende code:

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Net.Http;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.Logging;
using Newtonsoft.Json;

namespace SampleSkills
{
    /// <summary>
    /// Sample custom skill that wraps the Bing entity search API to connect it with a 
    /// cognitive search pipeline.
    /// </summary>
    public static class BingEntitySearch
    {
        #region Credentials
        // IMPORTANT: Make sure to enter your credential and to verify the API endpoint matches yours.
        static readonly string bingApiEndpoint = "https://api.cognitive.microsoft.com/bing/v7.0/entities/";
        static readonly string key = "<enter your api key here>";  
        #endregion

        #region Class used to deserialize the request
        private class InputRecord
        {
            public class InputRecordData
            {
                public string Name { get; set; }
            }

            public string RecordId { get; set; }
            public InputRecordData Data { get; set; }
        }

        private class WebApiRequest
        {
            public List<InputRecord> Values { get; set; }
        }
        #endregion

        #region Classes used to serialize the response

        private class OutputRecord
        {
            public class OutputRecordData
            {
                public string Name { get; set; } = "";
                public string Description { get; set; } = "";
                public string Source { get; set; } = "";
                public string SourceUrl { get; set; } = "";
                public string LicenseAttribution { get; set; } = "";
                public string LicenseUrl { get; set; } = "";
            }

            public class OutputRecordMessage
            {
                public string Message { get; set; }
            }

            public string RecordId { get; set; }
            public OutputRecordData Data { get; set; }
            public List<OutputRecordMessage> Errors { get; set; }
            public List<OutputRecordMessage> Warnings { get; set; }
        }

        private class WebApiResponse
        {
            public List<OutputRecord> Values { get; set; }
        }
        #endregion

        #region Classes used to interact with the Bing API
        private class BingResponse
        {
            public BingEntities Entities { get; set; }
        }
        private class BingEntities
        {
            public BingEntity[] Value { get; set; }
        }

        private class BingEntity
        {
            public class EntityPresentationinfo
            {
                public string[] EntityTypeHints { get; set; }
            }

            public class License
            {
                public string Url { get; set; }
            }

            public class ContractualRule
            {
                public string _type { get; set; }
                public License License { get; set; }
                public string LicenseNotice { get; set; }
                public string Text { get; set; }
                public string Url { get; set; }
            }

            public ContractualRule[] ContractualRules { get; set; }
            public string Description { get; set; }
            public string Name { get; set; }
            public EntityPresentationinfo EntityPresentationInfo { get; set; }
        }
        #endregion

        #region The Azure Function definition

        [FunctionName("EntitySearch")]
        public static async Task<IActionResult> Run(
            [HttpTrigger(AuthorizationLevel.Function, "post", Route = null)] HttpRequest req,
            ILogger log)
        {
            log.LogInformation("Entity Search function: C# HTTP trigger function processed a request.");

            var response = new WebApiResponse
            {
                Values = new List<OutputRecord>()
            };

            string requestBody = new StreamReader(req.Body).ReadToEnd();
            var data = JsonConvert.DeserializeObject<WebApiRequest>(requestBody);

            // Do some schema validation
            if (data == null)
            {
                return new BadRequestObjectResult("The request schema does not match expected schema.");
            }
            if (data.Values == null)
            {
                return new BadRequestObjectResult("The request schema does not match expected schema. Could not find values array.");
            }

            // Calculate the response for each value.
            foreach (var record in data.Values)
            {
                if (record == null || record.RecordId == null) continue;

                OutputRecord responseRecord = new OutputRecord
                {
                    RecordId = record.RecordId
                };

                try
                {
                    responseRecord.Data = GetEntityMetadata(record.Data.Name).Result;
                }
                catch (Exception e)
                {
                    // Something bad happened, log the issue.
                    var error = new OutputRecord.OutputRecordMessage
                    {
                        Message = e.Message
                    };

                    responseRecord.Errors = new List<OutputRecord.OutputRecordMessage>
                    {
                        error
                    };
                }
                finally
                {
                    response.Values.Add(responseRecord);
                }
            }

            return (ActionResult)new OkObjectResult(response);
        }

        #endregion

        #region Methods to call the Bing API
        /// <summary>
        /// Gets metadata for a particular entity based on its name using Bing Entity Search
        /// </summary>
        /// <param name="entityName">The name of the entity to extract data for.</param>
        /// <returns>Asynchronous task that returns entity data. </returns>
        private async static Task<OutputRecord.OutputRecordData> GetEntityMetadata(string entityName)
        {
            var uri = bingApiEndpoint + "?q=" + entityName + "&mkt=en-us&count=10&offset=0&safesearch=Moderate";
            var result = new OutputRecord.OutputRecordData();

            using (var client = new HttpClient())
            using (var request = new HttpRequestMessage {
                Method = HttpMethod.Get,
                RequestUri = new Uri(uri)
            })
            {
                request.Headers.Add("Ocp-Apim-Subscription-Key", key);

                HttpResponseMessage response = await client.SendAsync(request);
                string responseBody = await response?.Content?.ReadAsStringAsync();

                BingResponse bingResult = JsonConvert.DeserializeObject<BingResponse>(responseBody);
                if (bingResult != null)
                {
                    // In addition to the list of entities that could match the name, for simplicity let's return information
                    // for the top match as additional metadata at the root object.
                    return AddTopEntityMetadata(bingResult.Entities?.Value);
                }
            }

            return result;
        }

        private static OutputRecord.OutputRecordData AddTopEntityMetadata(BingEntity[] entities)
        {
            if (entities != null)
            {
                foreach (BingEntity entity in entities.Where(
                    entity => entity?.EntityPresentationInfo?.EntityTypeHints != null
                        && (entity.EntityPresentationInfo.EntityTypeHints[0] == "Person"
                            || entity.EntityPresentationInfo.EntityTypeHints[0] == "Organization"
                            || entity.EntityPresentationInfo.EntityTypeHints[0] == "Location")
                        && !String.IsNullOrEmpty(entity.Description)))
                {
                    var rootObject = new OutputRecord.OutputRecordData
                    {
                        Description = entity.Description,
                        Name = entity.Name
                    };

                    if (entity.ContractualRules != null)
                    {
                        foreach (var rule in entity.ContractualRules)
                        {
                            switch (rule._type)
                            {
                                case "ContractualRules/LicenseAttribution":
                                    rootObject.LicenseAttribution = rule.LicenseNotice;
                                    rootObject.LicenseUrl = rule.License.Url;
                                    break;
                                case "ContractualRules/LinkAttribution":
                                    rootObject.Source = rule.Text;
                                    rootObject.SourceUrl = rule.Url;
                                    break;
                            }
                        }
                    }

                    return rootObject;
                }
            }

            return new OutputRecord.OutputRecordData();
        }
        #endregion
    }
}
```

Zorg ervoor dat u Voer uw eigen *sleutel* waarde in de `key` constante op basis van de sleutel die u hebt verkregen bij het aanmelden voor de Bing entiteiten zoeken-API.

In dit voorbeeld bevat alle benodigde code in een enkel bestand voor het gemak. U vindt een iets meer gestructureerde versie van die dezelfde vaardigheid in [de opslagplaats van de vaardigheden power](https://github.com/Azure-Samples/azure-search-power-skills/tree/master/Text/BingEntitySearch).

U kunt natuurlijk, naam van het bestand uit `Function1.cs` naar `BingEntitySearch.cs`.

## <a name="test-the-function-from-visual-studio"></a>Test de functie vanuit Visual Studio

Druk op **F5** om uit te voeren van het gedrag van de functie programma en test. In dit geval gebruikt we de functie hieronder om te controleren of twee entiteiten. Postman of Fiddler gebruiken om uit te geven van een aanroep, zoals hieronder wordt weergegeven:

```http
POST https://localhost:7071/api/EntitySearch
```

### <a name="request-body"></a>Aanvraagbody
```json
{
    "values": [
        {
            "recordId": "e1",
            "data":
            {
                "name":  "Pablo Picasso"
            }
        },
        {
            "recordId": "e2",
            "data":
            {
                "name":  "Microsoft"
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
            "recordId": "e1",
            "data": {
                "name": "Pablo Picasso",
                "description": "Pablo Ruiz Picasso was a Spanish painter [...]",
                "source": "Wikipedia",
                "sourceUrl": "http://en.wikipedia.org/wiki/Pablo_Picasso",
                "licenseAttribution": "Text under CC-BY-SA license",
                "licenseUrl": "http://creativecommons.org/licenses/by-sa/3.0/"
            },
            "errors": null,
            "warnings": null
        },
        "..."
    ]
}
```

## <a name="publish-the-function-to-azure"></a>De functie publiceren naar Azure

Wanneer u tevreden met het gedrag van de functie bent, kunt u deze kunt publiceren.

1. Klik in **Solution Explorer** met de rechtermuisknop op het project en selecteer **Publiceren**. Kies **Maak een nieuwe** > **publiceren**.

1. Als u dit nog niet hebt Visual Studio al met uw Azure-account verbonden, selecteert u **een account toevoegen...**

1. Volg de aanwijzingen op. U wordt gevraagd om op te geven van een unieke naam voor uw appservice, het Azure-abonnement, de resourcegroep, het hostingabonnement en het opslagaccount dat u wilt gebruiken. U kunt een nieuwe resourcegroep, een nieuw hosting plan en een storage-account maken als u dit nog niet hebt. Wanneer u klaar bent, selecteert u **maken**

1. Nadat de implementatie voltooid is, ziet u de Site-URL. Dit is het adres van uw functie-app in Azure. 

1. In de [Azure-portal](https://portal.azure.com), gaat u naar de resourcegroep en zoek naar de `EntitySearch` functie die u hebt gepubliceerd. Onder de **beheren** sectie ziet u Hostsleutels. Selecteer de **kopie** pictogram voor de *standaard* host-sleutel.  

## <a name="test-the-function-in-azure"></a>De functie testen in Azure

Nu dat u de standaard-hostsleutel hebt, test u de functie als volgt:

```http
POST https://[your-entity-search-app-name].azurewebsites.net/api/EntitySearch?code=[enter default host key here]
```

### <a name="request-body"></a>Aanvraagtekst
```json
{
    "values": [
        {
            "recordId": "e1",
            "data":
            {
                "name":  "Pablo Picasso"
            }
        },
        {
            "recordId": "e2",
            "data":
            {
                "name":  "Microsoft"
            }
        }
    ]
}
```

In dit voorbeeld moet u eerder hebt gezien bij het uitvoeren van de functie in de lokale omgeving hetzelfde resultaat opleveren.

## <a name="connect-to-your-pipeline"></a>Verbinding maken met uw pijplijn
Nu dat u een nieuwe aangepaste vaardigheden hebt, kunt u deze kunt toevoegen aan uw vaardigheden uit. Het volgende voorbeeld ziet u over het aanroepen van de vaardigheid om toe te voegen beschrijvingen voor organisaties in het document (dit kan worden uitgebreid om te werken ook op locaties en mensen). Vervang `[your-entity-search-app-name]` met de naam van uw app.

```json
{
    "skills": [
      "[... your existing skills remain here]",  
      {
        "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
        "description": "Our new Bing entity search custom skill",
        "uri": "https://[your-entity-search-app-name].azurewebsites.net/api/EntitySearch?code=[enter default host key here]",
          "context": "/document/merged_content/organizations/*",
          "inputs": [
            {
              "name": "name",
              "source": "/document/merged_content/organizations/*"
            }
          ],
          "outputs": [
            {
              "name": "description",
              "targetName": "description"
            }
          ]
      }
  ]
}
```

Hier, we tellen op de ingebouwde [entiteit erkenning vaardigheid](cognitive-search-skill-entity-recognition.md) aanwezig zijn in de vaardigheden en het document met de lijst met organisaties zijn uitgebreid. Ter informatie is dit een entiteit extractie vaardigheid configuratie die volstaan worden kan bij het genereren van de gegevens die we nodig:

```json
{
    "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
    "name": "#1",
    "description": "Organization name extraction",
    "context": "/document/merged_content",
    "categories": [ "Organization" ],
    "defaultLanguageCode": "en",
    "inputs": [
        {
            "name": "text",
            "source": "/document/merged_content"
        },
        {
            "name": "languageCode",
            "source": "/document/language"
        }
    ],
    "outputs": [
        {
            "name": "organizations",
            "targetName": "organizations"
        }
    ]
},
```

## <a name="next-steps"></a>Volgende stappen
Gefeliciteerd! U hebt uw eerste aangepaste enricher gemaakt. U kunt nu hetzelfde patroon om toe te voegen van de functionaliteit van uw eigen aangepaste volgen. 

+ [Een aangepaste vaardigheid toevoegen aan een pijplijn cognitief zoeken](cognitive-search-custom-skill-interface.md)
+ [Hoe u een set vaardigheden definiëren](cognitive-search-defining-skillset.md)
+ [Vaardighedenset (REST) maken](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
+ [Verrijkt velden toewijzen](cognitive-search-output-field-mapping.md)
