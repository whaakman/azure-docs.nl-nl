---
title: Duurzame functies met behulp van de Azure portal maken
description: Informatie over het installeren van de extensie duurzame functies voor Azure Functions voor het ontwikkelen van de portal.
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 10/23/2018
ms.author: azfuncdf, glenga
ms.openlocfilehash: 3381939e296009b0fd58366f7fff410ea01d1206
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52864023"
---
# <a name="create-durable-functions-using-the-azure-portal"></a>Duurzame functies met behulp van de Azure portal maken

De [duurzame functies](durable-functions-overview.md) -extensie voor Azure Functions vindt u in het NuGet-pakket [Microsoft.Azure.WebJobs.Extensions.DurableTask](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DurableTask). Deze uitbreiding moet worden geïnstalleerd in uw functie-app. In dit artikel laat zien hoe dit pakket installeren zodat u duurzame functies in Azure portal kunt ontwikkelen.

>[!NOTE]
>
>* Als u ontwikkelt duurzame functies in C#, kunt u overwegen in plaats daarvan [ontwikkeling in Visual Studio 2017](durable-functions-create-first-csharp.md).
* Als u duurzame functies in JavaScript ontwikkelt, moet u in plaats daarvan overwegen **Visual Studio Code ontwikkelen**.
>
>Het maken van duurzame functies wordt met behulp van JavaScript nog niet ondersteund in de portal. Visual Studio Code in plaats daarvan gebruikt.

## <a name="create-a-function-app"></a>Een functie-app maken

U moet een functie-app voor het hosten van de uitvoering van elke functie hebben. Een functie-app kunt u uw functies groeperen in een logische eenheid voor eenvoudiger beheer, implementeren en delen van resources. U moet maken een C# functie-app, omdat de JavaScript-sjablonen zijn nog niet ondersteund voor duurzame functies.  

[!INCLUDE [Create function app Azure portal](../../../includes/functions-create-function-app-portal.md)]

Gemaakte functie-app maakt standaard gebruik van versie 2.x van de Azure Functions-runtime. De extensie duurzame functies werkt voor beide versies 1.x en 2.x van de Azure Functions-runtime. Sjablonen zijn echter alleen beschikbaar wanneer die gericht is op versie 2.x van de runtime.

## <a name="create-an-orchestrator-function"></a>Een orchestrator-functie maken

1. Vouw de functie-app uit en klik op de knop **+** naast **Functies**. Als dit de eerste functie in de functie-app is, selecteert u **In de portal** en vervolgens **Doorgaan**. Anders gaat u verder met stap drie.

   ![De Quick Start-pagina van Functions in Azure Portal](./media/durable-functions-create-portal/function-app-quickstart-choose-portal.png)

1. Kies **Meer sjablonen** en vervolgens **Voltooien en sjablonen weergeven**.

    ![De Quick Start-pagina 'Meer sjablonen kiezen' van Functions](./media/durable-functions-create-portal/add-first-function.png)

1. Typ in het zoekveld `durable` en kies vervolgens de **starter duurzame functies HTTP** sjabloon.

1. Wanneer u hierom wordt gevraagd, selecteert u **installeren** voor het installeren van de extensie Azure DurableTask eventuele afhankelijkheden in de functie-app. U hoeft alleen de extensie voor een functie-app kunt installeren. Wanneer de installatie is voltooid, selecteert u **Doorgaan**.

    ![Binding-extensies installeren](./media/durable-functions-create-portal/install-durabletask-extension.png)

1. Nadat de installatie is voltooid, noem de nieuwe functie `HttpStart` en kies **maken**. De gemaakte functie wordt gebruikt om de indeling start.

1. Een functie maken in de functie-app, ditmaal met behulp van de **duurzame functies** sjabloon. Naam voor de nieuwe functie voor orchestration `HelloSequence`.

1. Maken van een derde functie met de naam `Hello` met behulp van de **activiteit duurzame functies** sjabloon.

## <a name="test-the-durable-function-orchestration"></a>De indeling duurzame functie testen

1. Ga terug naar de **HttpStart** functie, kies **<> / functie-URL ophalen** en **kopie** de URL. U gebruikt deze URL om te beginnen de **HelloSequence** functie.

1. Een HTTP-hulpprogramma, zoals Postman of cURL gebruiken voor het verzenden van een POST-aanvraag naar de URL die u hebt gekopieerd. Het volgende voorbeeld wordt een cURL-opdracht die een POST-aanvraag naar de duurzame functie verzendt:

    ```bash
    curl -X POST https://{your-function-app-name}.azurewebsites.net/api/orchestrators/HelloSequence
    ```

    In dit voorbeeld `{your-function-app-name}` is het domein dat is de naam van uw functie-app. Het antwoordbericht bevat een set met URI-eindpunten die u gebruiken kunt om te controleren en beheren van de uitvoering van deze domeinnaam ziet als in het volgende voorbeeld eruit:

    ```json
    {  
       "id":"10585834a930427195479de25e0b952d",
       "statusQueryGetUri":"https://...",
       "sendEventPostUri":"https://...",
       "terminatePostUri":"https://...",
       "rewindPostUri":"https://..."
    }
    ```

1. Roep de `statusQueryGetUri` URI van het eindpunt en u ziet u de huidige status van de duurzame functie, er zoals in dit voorbeeld uitzien kan:

    ```json
        {
            "runtimeStatus": "Running",
            "input": null,
            "output": null,
            "createdTime": "2017-12-01T05:37:33Z",
            "lastUpdatedTime": "2017-12-01T05:37:36Z"
        }
    ```

1. Aanroepen blijven de `statusQueryGetUri` eindpunt totdat de status gewijzigd in **voltooid**, en ziet u een antwoord weergegeven zoals in het volgende voorbeeld: 

    ```json
    {
            "runtimeStatus": "Completed",
            "input": null,
            "output": [
                "Hello Tokyo!",
                "Hello Seattle!",
                "Hello London!"
            ],
            "createdTime": "2017-12-01T05:38:22Z",
            "lastUpdatedTime": "2017-12-01T05:38:28Z"
        }
    ```

Uw eerste duurzame functie is nu actief en werkend in Azure.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over algemene functiepatronen voor duurzame](durable-functions-overview.md)
