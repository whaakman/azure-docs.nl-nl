---
title: Uw eerste functie maken in Azure met behulp van Visual Studio Code
description: Maak een eenvoudige HTTP-geactiveerde functie en publiceer deze op Azure met behulp van de Azure Functions-extensie in Visual Studio Code.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
keywords: azure-functies, functies, gebeurtenisverwerking, berekenen, architectuur zonder server
ms.service: azure-functions
ms.devlang: multiple
ms.topic: quickstart
ms.date: 06/25/2019
ms.author: glenga
ms.custom: mvc, devcenter
ms.openlocfilehash: fcf9f1d6420dbbde359d386bc3b67a0866aca30d
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67444588"
---
# <a name="create-your-first-function-using-visual-studio-code"></a>Uw eerste functie maken met Visual Studio Code

Met Azure Functions kunt u uw code in een [serverloze](https://azure.microsoft.com/solutions/serverless/) omgeving uitvoeren zonder dat u eerst een virtuele machine moet maken of een webtoepassing publiceren.

In dit artikel leert u hoe u de [Azure Functions extension for Visual Studio Code] (Azure Functions-extensie voor Visual Studio Code) gebruikt om met Microsoft Visual Studio Code een 'hello world'-functie op uw lokale computer te maken en te testen. Vervolgens publiceert u de functiecode vanuit Visual Studio Code op Azure.

![Azure-functiecode in een Visual Studio-project](./media/functions-create-first-function-vs-code/functions-vscode-intro.png)

De extensie ondersteunt momenteel C#, JavaScript en Java werkt, met ondersteuning voor Python momenteel in Preview. De stappen in dit artikel en het artikel die volgt ondersteunen alleen JavaScript en C# functies. Zie voor meer informatie over het gebruik van Visual Studio Code maken en publiceren van Python-functies, [Python implementeren naar Azure Functions](https://code.visualstudio.com/docs/python/tutorial-azure-functions). Zie voor meer informatie over het gebruik van Visual Studio Code maken en publiceren van de PowerShell-functies, [uw eerste PowerShell-functie maken in Azure](functions-create-first-function-powershell.md). 

De extensie is momenteel beschikbaar als preview-product. Zie de uitbreidingspagina [Azure Functions extension for Visual Studio Code] (Azure Functions-extensie voor Visual Studio Code) voor meer informatie.

## <a name="prerequisites"></a>Vereisten

Dit zijn de vereisten voor het voltooien van deze snelstart:

* Installeer [Visual Studio Code](https://code.visualstudio.com/) op een van de [ondersteunde platforms](https://code.visualstudio.com/docs/supporting/requirements#_platforms).

* Installeer versie 2.x van de [Azure Functions Core Tools](functions-run-local.md#v2).

* Installeer de specifieke vereisten voor de taal van uw keuze:

    | Taal | Vereiste |
    | -------- | --------- |
    | **C#** | [C#-extensie](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)  |
    | **JavaScript** | [Node.js](https://nodejs.org/)<sup>*</sup> | 
 
    <sup>*</sup>Actieve LTS en onderhoud LTS versies (8.11.1 en 10.14.1 aanbevolen).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [functions-install-vs-code-extension](../../includes/functions-install-vs-code-extension.md)]

[!INCLUDE [functions-create-function-app-vs-code](../../includes/functions-create-function-app-vs-code.md)]

[!INCLUDE [functions-run-function-test-local-vs-code](../../includes/functions-run-function-test-local-vs-code.md)]

Nadat u hebt gecontroleerd of de functie correct wordt uitgevoerd op uw lokale computer, is het tijd om het project te publiceren in Azure.

[!INCLUDE [functions-sign-in-vs-code](../../includes/functions-sign-in-vs-code.md)]

[!INCLUDE [functions-publish-project-vscode](../../includes/functions-publish-project-vscode.md)]

## <a name="run-the-function-in-azure"></a>De functie uitvoeren in Azure

1. Kopieer de URL van de HTTP-trigger vanuit het deelvenster **Output** (Uitvoer). Zorg ervoor dat u net als eerder de queryreeks `?name=<yourname>` toevoegt aan het eind van deze URL en de aanvraag uitvoert.

    De URL die uw HTTP-geactiveerde functie aanroept, moet de volgende indeling hebben:

        http://<functionappname>.azurewebsites.net/api/<functionname>?name=<yourname> 

1. Plak deze nieuwe URL van de HTTP-aanvraag in de adresbalk van uw browser. Hieronder ziet u het antwoord op de externe GET-aanvraag dat door de functie wordt geretourneerd, weergegeven in de browser: 

    ![Het antwoord van de functie in de browser](./media/functions-create-first-function-vs-code/functions-test-remote-browser.png)

## <a name="next-steps"></a>Volgende stappen

U hebt een functie-app met een eenvoudige HTTP-geactiveerde functie gemaakt in Visual Studio Code. In het volgende artikel, kunt u deze functie uitbreiden door een Uitvoerbinding toe te voegen. Deze binding schrijft de tekenreeks van de HTTP-aanvraag naar een bericht in een Azure Queue Storage-wachtrij. Het volgende artikel leest u ook hoe voor het opschonen van deze nieuwe Azure-resources door het verwijderen van de resourcegroep die u hebt gemaakt.

> [!div class="nextstepaction"]
> [Een Azure Storage-wachtrij-binding toevoegen aan uw functie](functions-add-output-binding-storage-queue-vs-code.md)

[Azure Functions Core Tools]: functions-run-local.md
[Azure Functions extension for Visual Studio Code]: https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions (Azure Functions-extensie voor Visual Studio Code)
