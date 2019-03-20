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
ms.openlocfilehash: 22ec05660682f000d8bc3b9780732d5adf9b5c24
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58226705"
---
# <a name="quickstart-create-an-azure-function-with-app-configuration"></a>Quickstart: Een Azure-functie maken met app-configuratie

Azure-appconfiguratie is een beheerde configuratieservice in Azure. U kunt deze eenvoudig opslaan en beheren van alle instellingen van de toepassing op één plek dat gescheiden van uw code. In deze Quick Start laat zien hoe de service opnemen in een Azure-functie. 

Een code-editor kunt u de stappen in deze Quick Start. [Visual Studio Code](https://code.visualstudio.com/) is een uitstekende optie beschikbaar is op Windows, macOS en Linux-platforms.

![Snelstartgids voltooid lokale](./media/quickstarts/dotnet-core-function-launch-local.png)

## <a name="prerequisites"></a>Vereisten

Als u wilt doen in deze Quick Start, installeert u [Visual Studio 2017](https://visualstudio.microsoft.com/vs). Zorg ervoor dat de werkbelasting **Azure development** ook is geïnstalleerd. Installeer ook de [nieuwste hulpprogramma's voor Azure Functions](../azure-functions/functions-develop-vs.md#check-your-tools-version).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-an-app-configuration-store"></a>Een app-configuratiearchief maken

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]

## <a name="create-a-function-app"></a>Een functie-app maken

[!INCLUDE [Create a project using the Azure Functions template](../../includes/functions-vstools-create.md)]

## <a name="connect-to-an-app-configuration-store"></a>Verbinding maken met een app-configuratiearchief

1. Open *Function1.cs*, en voeg een verwijzing naar een provider van de configuratie van App-configuratie .NET Core.

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

1. Stel een omgevingsvariabele met de naam **ConnectionString**, en stel deze in op de toegangssleutel voor het opslaan van de app-configuratie. Als u de Windows-opdrachtprompt, voer de volgende opdracht uit en start opnieuw op de opdrachtprompt om toe te staan van de wijziging door te voeren:

        setx ConnectionString "connection-string-of-your-app-configuration-store"

    Als u Windows PowerShell gebruikt, voert u de volgende opdracht uit:

        $Env:ConnectionString = "connection-string-of-your-app-configuration-store"

    Als u Mac OS of Linux gebruikt, voert u de volgende opdracht uit:

        export ConnectionString='connection-string-of-your-app-configuration-store'

2. Druk op F5 om de functie testen. Accepteer desgevraagd de aanvraag van Visual Studio om **Azure Functions Core-hulpprogramma's (CLI)** te downloaden en installeren. Mogelijk moet u ook een firewall-uitzondering inschakelen zodat de hulpprogramma's voor HTTP-aanvragen kunnen verwerken.

3. Kopieer de URL van uw functie vanuit de uitvoer van de Azure Functions-runtime.

    ![Quickstart over foutopsporing in functies in Visual Studio](./media/quickstarts/function-visual-studio-debugging.png)

4. Plak de URL van de HTTP-aanvraag in de adresbalk van uw browser. De volgende afbeelding ziet het antwoord in de browser om de lokale GET-aanvraag geretourneerd door de functie.

    ![Quickstart over lokaal opstarten functies](./media/quickstarts/dotnet-core-function-launch-local.png)

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt gemaakt van een nieuwe app-configuratiearchief en deze gebruikt met een Azure-functie. Doorgaan naar de volgende zelfstudie waarin wordt gedemonstreerd verificatie voor meer informatie over het gebruik van App-configuratie.

> [!div class="nextstepaction"]
> [Beheerde identiteiten voor de integratie van Azure-resources](./integrate-azure-managed-service-identity.md)
