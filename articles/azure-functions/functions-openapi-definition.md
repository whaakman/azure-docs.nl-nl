---
title: De definitie van een OpenAPI voor een functie maken | Microsoft Docs
description: Maak een definitie van een OpenAPI waarmee andere apps en services aan te roepen van de functie in Azure.
services: functions
keywords: OpenAPI, Swagger, cloud-apps, cloud-services
documentationcenter: 
author: mgblythe
manager: cfowler
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/15/2017
ms.author: mblythe; glenga
ms.custom: mvc
ms.openlocfilehash: 2bf1a3e80e96d76b15340f87166b2b4762271cf3
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="create-an-openapi-definition-for-a-function"></a>De definitie van een OpenAPI voor een functie maken
REST-API's worden vaak beschreven met behulp van de definitie van een OpenAPI (voorheen bekend als een [Swagger](http://swagger.io/) bestand). Deze definitie bevat informatie over welke bewerkingen zijn beschikbaar in een API en hoe de gegevens van de aanvraag en -antwoord voor de API moet worden opgebouwd.

In deze zelfstudie maakt u een functie waarmee wordt bepaald of een noodherstelproces op een turbine o rendabele. Vervolgens maakt u een OpenAPI definitie voor de functie-app zodat de functie kan worden aangeroepen vanuit andere apps en services.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Maken van een functie in Azure
> * De definitie van een OpenAPI met OpenAPI's genereren
> * De definitie voor aanvullende metagegevens wijzigen
> * De definitie van de door het aanroepen van de functie testen

## <a name="create-a-function-app"></a>Een functie-app maken

U moet een functie-app hebben die als host fungeert voor de uitvoering van uw functies. Een functie-app kunt u functies groeperen als een logische eenheid voor eenvoudiger beheer, implementatie, schaalbaarheid en delen van bronnen. 

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]


## <a name="create-the-function"></a>De functie maken

Deze zelfstudie wordt gebruikgemaakt van een HTTP-geactiveerd functie waarvoor twee parameters zijn vereist: de geschatte tijd voor het maken van een turbine herstellen (in uren); en de capaciteit van de turbine (in kilowatt). De functie vervolgens berekend hoeveel reparatie kost en hoeveel omzet turbine kan in een periode van 24 uur.

1. Vouw de functie-app en selecteer de  **+**  naast **functies**. Als dit de eerste functie in de functie-app is, selecteert u **Aangepaste functie**. U ziet nu de volledige set het functiesjablonen. 

    ![De Quick Start-pagina van Functions in Azure Portal](media/functions-openapi-definition/add-first-function.png)

2. Typ in het zoekveld `http` en kies vervolgens **C#** voor de HTTP-trigger-sjabloon. 
 
    ![Kies de HTTP-trigger](./media/functions-openapi-definition/select-http-trigger-portal.png)

3. Type `TurbineRepair` voor de functie **naam**, kies `Function` voor  **[verificatieniveau](functions-bindings-http-webhook.md#http-auth)**, en selecteer vervolgens **maken**.  

    ![De functie HTTP geactiveerd maken](./media/functions-openapi-definition/select-http-trigger-portal-2.png)

1. De inhoud van het bestand run.csx vervangen door de volgende code en klik vervolgens op **opslaan**:

    ```c#
    using System.Net;

    const double revenuePerkW = 0.12; 
    const double technicianCost = 250; 
    const double turbineCost = 100;

    public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
    {   

        //Get request body
        dynamic data = await req.Content.ReadAsAsync<object>();
        int hours = data.hours;
        int capacity = data.capacity;

        //Formulas to calculate revenue and cost
        double revenueOpportunity = capacity * revenuePerkW * 24;  
        double costToFix = (hours * technicianCost) +  turbineCost;
        string repairTurbine;

        if (revenueOpportunity > costToFix){
            repairTurbine = "Yes";
        }
        else {
            repairTurbine = "No";
        }

        return req.CreateResponse(HttpStatusCode.OK, new{
            message = repairTurbine,
            revenueOpportunity = "$"+ revenueOpportunity,
            costToFix = "$"+ costToFix         
        }); 
    }
    ```
    Deze functiecode retourneert een bericht van het `Yes` of `No` om aan te geven of een noodherstelproces rendabele, evenals de Omzetkans dat de turbine vertegenwoordigt en de kosten om op te lossen turbine. 

1. U kunt de functie testen, klikt u op **testen** helemaal rechts op het tabblad testresultaten uitbreiden. Voer de volgende waarde voor de **aanvraagtekst**, en klik vervolgens op **uitvoeren**.

    ```json
    {
    "hours": "6",
    "capacity": "2500"
    }
    ```

    ![De functie testen in de Azure portal](media/functions-openapi-definition/test-function.png)

    De volgende waarde in de hoofdtekst van het antwoord geretourneerd.

    ```json
    {"message":"Yes","revenueOpportunity":"$7200","costToFix":"$1600"}
    ```

U hebt nu een functie die de kosteneffectiviteit van noodgevallen reparaties bepaalt. Vervolgens genereren en de definitie van een OpenAPI voor de functie-app wijzigen.

## <a name="generate-the-openapi-definition"></a>De definitie OpenAPI genereren

U nu kunt de definitie OpenAPI genereren. Deze definitie kan worden gebruikt door andere Microsoft-technologieën, zoals API-Apps [PowerApps](functions-powerapps-scenario.md) en [Microsoft Flow](../azure-functions/app-service-export-api-to-powerapps-and-flow.md), zoals goed als derde partij ontwikkelhulpprogramma's, zoals [Postman](https://www.getpostman.com/docs/importing_swagger) en [veel meer pakketten](http://swagger.io/tools/).

1. Selecteer alleen de *termen* dat uw API (in dit geval POST ondersteunt). Hierdoor kunt u de gegenereerde API-definitie schonere.

    1. Op de **integreren** tabblad van uw nieuwe functie in de HTTP-Trigger, wijziging **toegestaan HTTP-methoden** naar **methoden geselecteerd**

    1. In **geselecteerd HTTP-methoden**, schakelt u elke optie behalve **POST**, klikt u vervolgens op **opslaan**.

        ![Geselecteerde HTTP-methoden](media/functions-openapi-definition/selected-http-methods.png)
        
1. Klik op de functie app-naam (zoals **functie demo energie**) > **platformfuncties** > **API-definitie**.

    ![API-definitie](media/functions-openapi-definition/api-definition.png)

1. Op de **API-definitie** tabblad **functie**.

    ![Bron van API-definitie](media/functions-openapi-definition/api-definition-source.png)

    In deze stap kunt u een reeks OpenAPI opties voor functie-app, met inbegrip van een eindpunt voor het hosten van een bestand OpenAPI uit van de functie app-domein, een inline-kopie van de [OpenAPI Editor](http://editor.swagger.io), en een API-definitie sjabloon generator.

1. Klik op **genereren API-definitie sjabloon** > **opslaan**.

    ![Sjabloon voor API-definitie genereren](media/functions-openapi-definition/generate-template.png)

    Azure uw app in de functie voor het aantal HTTP-Trigger functies gescand en maakt gebruik van de gegevens in functions.json voor het genereren van de definitie van een OpenAPI. Hier volgt de definitie die wordt gegenereerd:

    ```yaml
    swagger: '2.0'
    info:
    title: function-demo-energy.azurewebsites.net
    version: 1.0.0
    host: function-demo-energy.azurewebsites.net
    basePath: /
    schemes:
    - https
    - http
    paths:
    /api/TurbineRepair:
        post:
        operationId: /api/TurbineRepair/post
        produces: []
        consumes: []
        parameters: []
        description: >-
            Replace with Operation Object
            #http://swagger.io/specification/#operationObject
        responses:
            '200':
            description: Success operation
        security:
            - apikeyQuery: []
    definitions: {}
    securityDefinitions:
    apikeyQuery:
        type: apiKey
        name: code
        in: query
    ```

    Deze definitie wordt beschreven als een _sjabloon_ omdat hiervoor meer metagegevens voor de definitie van een volledige OpenAPI. U kunt de definitie van de volgende stap gaat wijzigen.

## <a name="modify-the-openapi-definition"></a>De definitie van de OpenAPI wijzigen
Nu dat u de Sjabloondefinitie van een hebt, wijzigen om de aanvullende metagegevens over de bewerkingen van de API en de gegevensstructuren. In **API-definitie**, verwijderen van de gegenereerde definitie uit `post` plakken in de onderstaande inhoud naar de onderkant van de definitie en klikt u op **opslaan**.

```yaml
    post:
      operationId: CalculateCosts
      description: Determines if a technician should be sent for repair
      summary: Calculates costs
      x-ms-summary: Calculates costs
      x-ms-visibility: important
      produces:
        - application/json
      consumes:
        - application/json
      parameters:
        - name: body
          in: body
          description: Hours and capacity used to calculate costs
          x-ms-summary: Hours and capacity
          x-ms-visibility: important
          required: true
          schema:
            type: object
            properties:
              hours:
                description: The amount of effort in hours required to conduct repair
                type: number
                x-ms-summary: Hours
                x-ms-visibility: important
              capacity:
                description: The max output of a turbine in kilowatts
                type: number
                x-ms-summary: Capacity
                x-ms-visibility: important
      responses:
        200:
          description: Message with cost and revenue numbers
          x-ms-summary: Message
          schema:
           type: object
           properties:
            message:
              type: string
              description: Returns Yes or No depending on calculations
              x-ms-summary: Message 
            revenueOpportunity:
              type: string
              description: The revenue opportunity cost
              x-ms-summary: RevenueOpportunity 
            costToFix:
              type: string
              description: The cost in $ to fix the turbine
              x-ms-summary: CostToFix
      security:
        - apikeyQuery: []
definitions: {}
securityDefinitions:
  apikeyQuery:
    type: apiKey
    name: code
    in: query
```

In dit geval kan alleen plakken in bijgewerkte metagegevens, maar het is belangrijk te begrijpen van de soorten wijzigingen we in de standaardsjabloon:

+ Opgegeven dat de API produceert en maakt gebruik van gegevens in een JSON-indeling.

+ De vereiste parameters opgegeven voor hun namen en gegevenstypen.

+ De retourwaarden voor een geslaagde reactie opgegeven met hun namen en gegevenstypen.

+ Beschrijvende samenvattingen en beschrijvingen voor de API en de bewerkingen en parameters opgegeven. Dit is belangrijk voor mensen die deze functie gebruiken.

+ X-ms-overzicht en x-ms-zichtbaarheid, die worden gebruikt in de gebruikersinterface voor Microsoft-Flow en Logic Apps toegevoegd. Zie voor meer informatie [OpenAPI uitbreidingen voor aangepaste API's in Microsoft Flow](https://preview.flow.microsoft.com/documentation/customapi-how-to-swagger/).

> [!NOTE]
> We laten de definitie van de beveiliging met de standaardmethode voor verificatie van de API-sleutel. U zou deze sectie van de definitie wijzigen als u een ander type verificatie gebruikt.

Zie voor meer informatie over het definiëren van de API-bewerkingen de [Open API-specificatie](https://swagger.io/specification/#operationObject).

## <a name="test-the-openapi-definition"></a>De definitie OpenAPI testen
Voordat u de API-definitie gebruikt, is het een goed idee om deze te testen in de gebruikersinterface van de Azure-functies.

1. Op de **beheren** tabblad van de functie onder **Hostsleutels**, Kopieer de **standaard** sleutel.

    ![Kopieer de API-sleutel](media/functions-openapi-definition/copy-api-key.png)

    > [!NOTE]
    >U gebruikt deze sleutel voor het testen en u deze ook gebruiken wanneer u de API vanuit een app of service aanroept.

1. Ga terug naar de API-definitie: **functie demo energie** > **platformfuncties** > **API-definitie**.

1. Klik in het rechterdeelvenster **verifiëren**, voer de API-sleutel die u gekopieerd en klik op **verifiëren**.

    ![Verificatie met de API-sleutel](media/functions-openapi-definition/authenticate-api-key.png)

1. Schuif naar beneden en klik op **probeer deze bewerking**.

    ![Voer deze bewerking](media/functions-openapi-definition/try-operation.png)

1. Voer waarden in voor **uren** en **capaciteit**.

    ![Geef parameters](media/functions-openapi-definition/parameters.png)

    U ziet hoe de beschrijvingen van de API-definitie in de gebruikersinterface wordt gebruikt.

1. Klik op **aanvraag verzenden**, klikt u vervolgens op de **vrij** tabblad voor een overzicht van de uitvoer.

    ![Een aanvraag verzenden](media/functions-openapi-definition/send-request.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Maken van een functie in Azure
> * De definitie van een OpenAPI met OpenAPI's genereren
> * De definitie voor aanvullende metagegevens wijzigen
> * De definitie van de door het aanroepen van de functie testen

Ga naar het volgende onderwerp voor informatie over het maken van een PowerApps-app die gebruikmaakt van de definitie van de OpenAPI die u hebt gemaakt.
> [!div class="nextstepaction"]
> [Aanroepen van een functie van PowerApps](functions-powerapps-scenario.md)
