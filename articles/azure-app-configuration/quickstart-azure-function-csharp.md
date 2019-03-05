---
title: Quickstart voor Azure-app-configuratie met Azure Functions | Microsoft Docs
description: Een quickstart voor het gebruik van Azure-app-configuratie met Azure Functions.
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.devlang: csharp
ms.topic: quickstart
ms.tgt_pltfrm: Azure Functions
ms.workload: tbd
ms.date: 02/24/2019
ms.author: yegu
ms.openlocfilehash: 5f28e213a5f824562df62a05b98f0f92f71bc591
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2019
ms.locfileid: "56957433"
---
# <a name="quickstart-create-an-azure-function-with-app-configuration"></a>Quickstart: Een Azure-functie maken met app-configuratie

Azure-app-configuratie is een beheerde configuratieservice in Azure. Hiermee kunt u eenvoudig alle toepassingsinstellingen opslaan en beheren op één plek die gescheiden is van uw code. Deze quickstart laat u zien hoe u de service kunt opnemen in een Azure-functie. 

U kunt elke code-editor gebruiken om de stappen in deze snelstart uit te voeren. [Visual Studio Code](https://code.visualstudio.com/) is een uitstekende optie die beschikbaar is op de Windows-, macOS- en Linux-platforms.

![Downloaden van voorbeeld voor snelstart voltooid](./media/quickstarts/dotnet-core-function-launch-local.png)

## <a name="prerequisites"></a>Vereisten

Om deze quickstart te kunnen volgen, installeert u [Visual Studio 2017](https://visualstudio.microsoft.com/vs) (en zorg ervoor dat de workload **Azure-ontwikkeling** ook is geïnstalleerd) en de [nieuwste hulpprogramma's voor Azure Functions](../azure-functions/functions-develop-vs.md#check-your-tools-version).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-an-app-configuration-store"></a>Een app-configuratiearchief maken

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]

## <a name="create-a-function-app"></a>Een functie-app maken

[!INCLUDE [Create a project using the Azure Functions template](../../includes/functions-vstools-create.md)]

## <a name="connect-to-app-configuration-store"></a>Verbinding maken met een app-configuratiearchief

1. Open *Function1.cs* en voeg een verwijzing naar de configuratieprovider van de app-configuratie .NET Core toe.

    ```csharp
    using Microsoft.Extensions.Configuration.AzureAppConfiguration;
    ```

2. Werk de methode `Run` bij voor het gebruik van app-configuratie door `builder.AddAzureAppConfiguration()` aan te roepen.

    ```csharp
    public static async Task<IActionResult> Run(
        [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)] HttpRequest req, ILogger log)
    {
        log.LogInformation("C# HTTP trigger function processed a request.");

        var builder = new ConfigurationBuilder();
        builder.AddAzureAppConfiguration(Environment.GetEnvironmentVariable("ConnectionString"));
        var config = builder.Build();
        string message = config["TestApp:Settings:Message"];
        message = message ?? req.Query["message"];

        string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
        dynamic data = JsonConvert.DeserializeObject(requestBody);
        message = message ?? data?.message;

        return message != null
            ? (ActionResult) new OkObjectResult(message)
            : new BadRequestObjectResult("Please pass a message from a configuration store, on the query string or in the request body");
    }
    ```

## <a name="test-the-function-locally"></a>De functie lokaal testen

1. Stel een omgevingsvariabele met de naam **ConnectionString** in en stel deze in op de toegangssleutel van uw app-configuratiearchief. Als u de Windows-opdrachtprompt gebruikt, voert u de volgende opdracht uit en start u de opdrachtprompt opnieuw op om de wijziging door te voeren:

        setx ConnectionString "connection-string-of-your-app-configuration-store"

    Als u Windows PowerShell gebruikt, moet u de volgende opdracht uitvoeren:

        $Env:ConnectionString = "connection-string-of-your-app-configuration-store"

    Als u macOS of Linux gebruikt, moet u de volgende opdracht uitvoeren:

        export ConnectionString='connection-string-of-your-app-configuration-store'

2. Druk op **F5** om de functie testen. Accepteer desgevraagd de aanvraag van Visual Studio om **Azure Functions Core-hulpprogramma's (CLI)** te downloaden en installeren. Mogelijk moet u ook een firewall-uitzondering inschakelen, zodat de hulpprogramma's HTTP-aanvragen kunnen afhandelen.

3. Kopieer de URL van uw functie vanuit de uitvoer van de Azure Functions-runtime.

    ![Quickstart over foutopsporing in functies in Visual Studio](./media/quickstarts/function-visual-studio-debugging.png)

4. Plak de URL van de HTTP-aanvraag in de adresbalk van uw browser. Hieronder ziet u de reactie op de lokale GET-aanvraag die door de functie wordt geretourneerd, weergegeven in de browser:

    ![Quickstart over lokaal opstarten functies](./media/quickstarts/dotnet-core-function-launch-local.png)

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Volgende stappen

In deze quickstart hebt u een nieuw app-configuratiearchief gemaakt en deze gebruikt met een Azure-functie. Als u meer wilt weten over het gebruik van app-configuratie, gaat u verder met de volgende zelfstudie, waarin verificatie wordt behandeld.

> [!div class="nextstepaction"]
> [Beheerde identiteiten voor integratie met Azure-resources](./integrate-azure-managed-service-identity.md)
