---
title: Een OpenAPI-definitie voor een functie maken met Azure API Management
description: Maak een definitie van een OpenAPI waarmee andere apps en services uw functie in Azure aanroepen.
services: functions
keywords: OpenAPI, Swagger, cloud-apps, cloud-services,
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: tutorial
ms.date: 05/08/2019
ms.author: glenga
ms.reviewer: sunayv
ms.custom: mvc, cc996988-fb4f-47
ms.openlocfilehash: 255a7c9d0b9da15176fca90c6934a84fa0f863ed
ms.sourcegitcommit: 1d257ad14ab837dd13145a6908bc0ed7af7f50a2
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/09/2019
ms.locfileid: "65501857"
---
# <a name="create-an-openapi-definition-for-a-function-with-azure-api-management"></a>Een OpenAPI-definitie voor een functie maken met Azure API Management

REST-API's worden vaak beschreven met behulp van een OpenAPI-definitie. Deze definitie bevat informatie over welke bewerkingen beschikbaar zijn in een API en hoe de gegevens van de aanvraag en respons voor de API moeten worden opgebouwd.

In deze zelfstudie maakt u een functie waarmee wordt bepaald of een noodherstelproces op een windturbine rendabel is. U maakt u een OpenAPI-definitie voor de functie-app via [Azure API Management](../api-management/api-management-key-concepts.md) zodat de functie kan worden aangeroepen vanuit andere apps en services.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een maken functie in Azure
> * Genereren van een OpenAPI-definitie met Azure API Management
> * De definitie testen door het aanroepen van de functie

## <a name="create-a-function-app"></a>Een functie-app maken

U moet een functie-app hebben die als host fungeert voor de uitvoering van uw functies. Een functie-app kunt u functies groeperen in een logische eenheid voor eenvoudiger beheer, implementatie, schalen en delen van resources.

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

## <a name="create-the-function"></a>De functie maken

In deze zelfstudie wordt een door HTTP geactiveerde functie waarvoor twee parameters nodig:

* De geschatte tijd voor het maken van een turbine herstellen, in uren.
* De capaciteit van de turbine, in kilowatt. 

De functie berekend vervolgens hoeveel een reparatie kost en hoeveel omzet de turbine in een periode van 24 uur zou kunnen maken. HET maken van de HTTP geactiveerde functie in de [Azure-portal](https://portal.azure.com).

1. Vouw de functie-app uit en klik op de knop **+** naast **Functies**. Selecteer **portal** > **blijven**.

1. Selecteer **meer sjablonen...** en selecteer vervolgens **voltooien en de weergave sjablonen**

1. Selecteer HTTP-trigger, type `TurbineRepair` voor de functie **naam**, kiest u `Function` voor  **[verificatieniveau](functions-bindings-http-webhook.md#http-auth)**, en selecteer vervolgens  **Maak**.  

    ![HTTP-functie maken voor OpenAPI](media/functions-openapi-definition/select-http-trigger-openapi.png)

1. Vervang de inhoud van de run.csx C# -scriptbestand met de volgende code, en kies vervolgens **opslaan**:

    ```csharp
    #r "Newtonsoft.Json"
    
    using System.Net;
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.Extensions.Primitives;
    using Newtonsoft.Json;
    
    const double revenuePerkW = 0.12;
    const double technicianCost = 250;
    const double turbineCost = 100;
    
    public static async Task<IActionResult> Run(HttpRequest req, ILogger log)
    {
        // Get query strings if they exist
        int tempVal;
        int? hours = Int32.TryParse(req.Query["hours"], out tempVal) ? tempVal : (int?)null;
        int? capacity = Int32.TryParse(req.Query["capacity"], out tempVal) ? tempVal : (int?)null;
    
        // Get request body
        string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
        dynamic data = JsonConvert.DeserializeObject(requestBody);
    
        // Use request body if a query was not sent
        capacity = capacity ?? data?.capacity;
        hours = hours ?? data?.hours;
    
        // Return bad request if capacity or hours are not passed in
        if (capacity == null || hours == null){
            return new BadRequestObjectResult("Please pass capacity and hours on the query string or in the request body");
        }
        // Formulas to calculate revenue and cost
        double? revenueOpportunity = capacity * revenuePerkW * 24;  
        double? costToFix = (hours * technicianCost) +  turbineCost;
        string repairTurbine;
    
        if (revenueOpportunity > costToFix){
            repairTurbine = "Yes";
        }
        else {
            repairTurbine = "No";
        };
    
        return (ActionResult)new OkObjectResult(new{
            message = repairTurbine,
            revenueOpportunity = "$"+ revenueOpportunity,
            costToFix = "$"+ costToFix
        });
    }
    ```

    Deze functiecode retourneert een bericht van `Yes` of `No` om aan te geven of een noodherstelproces rendabel is, evenals de kans op omzet die de turbine vertegenwoordigt en de kosten om de turbine te repareren.

1. Om de functie te testen, klikt u op **Testen** helemaal rechts op het tabblad om de testresultaten uit te breiden. Voer de volgende waarde in voor de **Aanvraagtekst**, en klik vervolgens op **Uitvoeren**.

    ```json
    {
    "hours": "6",
    "capacity": "2500"
    }
    ```

    ![Test de functie in de Azure Portal](media/functions-openapi-definition/test-function.png)

    De volgende waarde wordt in de hoofdtekst van het antwoord geretourneerd.

    ```json
    {"message":"Yes","revenueOpportunity":"$7200","costToFix":"$1600"}
    ```

U hebt nu een functie die de kosteneffectiviteit van noodreparaties bepaalt. Vervolgens genereert u een OpenAPI-definitie voor de functie-app.

## <a name="generate-the-openapi-definition"></a>De OpenAPI-definitie genereren

U nu kunt de OpenAPI-definitie genereren.

1. Vervolgens selecteert u de functie-app in **platformfuncties**, kiest u **API Management** en selecteer **nieuw** onder **API Management**.

    ![API Management kiezen in de functies van het Platform](media/functions-openapi-definition/select-all-settings-openapi.png)

1. Gebruik de API Management-instellingen zoals opgegeven in de tabel onder de afbeelding.

    ![Nieuwe API Management-service maken](media/functions-openapi-definition/new-apim-service-openapi.png)

    | Instelling      | Voorgestelde waarde  | Description                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Naam** | Wereldwijd unieke naam | Een naam is gegenereerd op basis van de naam van uw functie-app. |
    | **Abonnement** | Uw abonnement | Het abonnement waaronder deze nieuwe resource wordt gemaakt. |  
    | **[Resourcegroep](../azure-resource-manager/resource-group-overview.md)** |  myResourceGroup | Dezelfde resource als uw functie-app die u moet ophalen. |
    | **Locatie** | US - west | Kies de locatie VS-West. |
    | **Naam van de organisatie** | Contoso | De naam van de organisatie in de portal voor ontwikkelaars en voor e-mailmeldingen gebruikt. |
    | **E-mailadres van de beheerder** | uw e-mailadres | E-mailadres dat systeemmeldingen van API Management hebt ontvangen. |
    | **Prijscategorie** | Verbruik (preview-versie) | Gebruik laag is in preview en is niet beschikbaar in alle regio's. Zie voor volledige informatie over prijsinformatie de [API Management-pagina met prijzen](https://azure.microsoft.com/pricing/details/api-management/) |

1. Kies **maken** te maken van de API Management-instantie kan enkele minuten duren.

1. Selecteer **Application Insights inschakelen** om Logboeken te zenden naar dezelfde locatie als de functie-toepassing, accepteer de overige standaardwaarden en selecteer **koppeling naar de API**.

1. De **importeren van Azure Functions** wordt geopend met de **TurbineRepair** functie gemarkeerd. Kies **Selecteer** om door te gaan.

    ![Importeren van Azure Functions in API Management](media/functions-openapi-definition/import-function-openapi.png)

1. In de **maken van de functie-App** pagina, accepteer de standaardwaarden en selecteer **maken**

    ![Maken op basis van functie-app](media/functions-openapi-definition/create-function-openapi.png)

De API is nu gemaakt voor de functie.

## <a name="test-the-openapi-definition"></a>De OpenAPI-definitie testen

Voordat u de API-definitie gebruiken, moet u controleren of deze werkt.

1. Op de **Test** tabblad van de functie, selecteer **POST** bewerking

1. Voer waarden in voor **uur** en **capaciteit**

    ```json
    {
    "hours": "6",
    "capacity": "2500"
    }
    ```

1. Klik op **verzenden**, bekijk vervolgens de HTTP-antwoord.

    ![Test function-API](media/functions-openapi-definition/test-function-api-openapi.png)

[!INCLUDE [clean-up-section-portal](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over API Management](../api-management/api-management-key-concepts.md)
