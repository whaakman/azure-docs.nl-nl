---
title: Installeer de extensie duurzame functies en -voorbeelden - Azure
description: Informatie over het installeren van de extensie duurzame functies voor Azure Functions, voor de ontwikkeling van de portal of Visual Studio-ontwikkeling.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 08/31/2018
ms.author: azfuncdf
ms.openlocfilehash: 3f9bdcb67628a6780e42ef16acea2b91ca9817d9
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44377017"
---
# <a name="install-the-durable-functions-extension-and-samples-azure-functions"></a>Installeer de extensie duurzame functies en voorbeelden (Azure Functions)

De [duurzame functies](durable-functions-overview.md) -extensie voor Azure Functions vindt u in het NuGet-pakket [Microsoft.Azure.WebJobs.Extensions.DurableTask](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DurableTask). In dit artikel laat zien hoe om het pakket en een reeks voorbeelden voor de volgende ontwikkelomgevingen te installeren:

* Visual Studio 2017 (aanbevolen voor C#) 
* Visual Studio Code (aanbevolen voor JavaScript)
* Azure Portal

## <a name="visual-studio-2017"></a>Visual Studio 2017

Visual Studio biedt momenteel de beste ervaring voor het ontwikkelen van apps die gebruikmaken van duurzame functies.  Uw functies kunnen lokaal worden uitgevoerd en kunnen ook worden gepubliceerd naar Azure. U kunt beginnen met een leeg project of met een set Voorbeeldfuncties.

### <a name="prerequisites"></a>Vereisten

* Installeer de [meest recente versie van Visual Studio](https://www.visualstudio.com/downloads/) (versie: 15,6 of hoger). Bevatten de **Azure-ontwikkeling** werkbelasting in uw installatieopties.

### <a name="start-with-sample-functions"></a>Beginnen met Voorbeeldfuncties 

1. Download de [ZIP-bestand voor voorbeeld-App voor Visual Studio](https://azure.github.io/azure-functions-durable-extension/files/VSDFSampleApp.zip). U hoeft niet te de NuGet-verwijzing toevoegen omdat het voorbeeldproject al deze.
2. Installeren en uitvoeren [Azure-Opslagemulator](https://docs.microsoft.com/azure/storage/storage-use-emulator) versie 5.6 of hoger. U kunt ook u kunt bijwerken de *local.appsettings.json* -bestand met echte Azure Storage-verbindingsreeksen.
3. Open het project in Visual Studio 2017. 
4. Voor instructies over het uitvoeren van het voorbeeld beginnen met [functie chaining - reeks voorbeeld Hello](durable-functions-sequence.md). Het voorbeeld kan lokaal worden uitgevoerd of gepubliceerd naar Azure.

### <a name="start-with-an-empty-project"></a>Beginnen met een leeg project
 
Volg de dezelfde richtlijnen als voor het beginnen met het voorbeeld, maar de volgende stappen uit in plaats van downloaden van de *.zip* bestand:

1. Maak een functie-App-project.
2. Zoeken naar de volgende NuGet pakket verwijzing met behulp van *NuGet-pakketten beheren* en voeg deze toe aan het project: Microsoft.Azure.WebJobs.Extensions.DurableTask v1.6.0
   
## <a name="visual-studio-code"></a>Visual Studio Code

Visual Studio Code biedt een lokale ontwikkeling die betrekking hebben op alle grote platformen - Windows, macOS en Linux.  Uw functies kunnen lokaal worden uitgevoerd en ook worden gepubliceerd naar Azure. U kunt beginnen met een leeg project of met een set Voorbeeldfuncties.

### <a name="prerequisites"></a>Vereisten

* Installeer de [meest recente versie van Visual Studio Code](https://code.visualstudio.com/Download) 

* Volg de instructies in 'Installeert de Azure Functions Core Tools' op [Code en test Azure Functions lokaal](https://docs.microsoft.com/azure/azure-functions/functions-run-local)

    >[!IMPORTANT]
    > Als u de Cross-Platform's van Azure Functions al hebt, kunt u deze naar de meest recente beschikbare versie bijwerken.

    >[!IMPORTANT]
    >Duurzame functies in JavaScript is vereist voor versie 2.x van Azure Functions Core Tools.

*  Als u van een Windows-machine gebruikmaakt, installeren en uitvoeren [Azure-Opslagemulator](https://docs.microsoft.com/azure/storage/storage-use-emulator) versie 5.6 of hoger. U kunt ook u kunt bijwerken de *local.appsettings.json* bestand met echte Azure Storage-verbinding. 


### <a name="start-with-sample-functions"></a>Beginnen met Voorbeeldfuncties

#### <a name="c"></a>C#

1. Kloon de [duurzame functies opslagplaats](https://github.com/Azure/azure-functions-durable-extension.git).
2. Navigeer op uw computer naar de [C#-script-voorbeelden map](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/csx). 
3. Azure Functions duurzame Extension installeren door te voeren van het volgende in een opdracht vragen / terminal-venster:

    ```bash
    func extensions install -p Microsoft.Azure.WebJobs.Extensions.DurableTask -v 1.6.0
    ```
4. Azure Functions Twilio Extension installeren door te voeren van het volgende in een opdracht vragen / terminal-venster:

    ```bash
    func extensions install -p Microsoft.Azure.WebJobs.Extensions.Twilio -v 3.0.0-beta8
    ```
5. Voer Azure-Opslagemulator of update de *local.appsettings.json* -bestand met echte Azure Storage-verbindingsreeks.
6. Open het project in Visual Studio Code. 
7. Voor instructies over het uitvoeren van het voorbeeld beginnen met [functie chaining - reeks voorbeeld Hello](durable-functions-sequence.md). Het voorbeeld kan lokaal worden uitgevoerd of gepubliceerd naar Azure.
8. Start het project door te voeren in de opdracht vragen / terminal de volgende opdracht uit:
    ```bash
    func host start
    ```

#### <a name="javascript-functions-v2-only"></a>JavaScript (alleen functies v2)

1. Kloon de [duurzame functies opslagplaats](https://github.com/Azure/azure-functions-durable-extension.git).
2. Navigeer op uw computer naar de [JavaScript map met voorbeelden](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/javascript). 
3. Azure Functions duurzame Extension installeren door te voeren van het volgende in een opdracht vragen / terminal-venster:

    ```bash
    func extensions install -p Microsoft.Azure.WebJobs.Extensions.DurableTask -v 1.6.0
    ```
4. Herstellen van de npm-pakketten door het uitvoeren van de volgende in een opdracht vragen / terminal-venster:
    
    ```bash
    npm install
    ``` 
5. Update de *local.appsettings.json* -bestand met de echte Azure Storage-verbindingsreeks.
6. Open het project in Visual Studio Code. 
7. Voor instructies over het uitvoeren van het voorbeeld beginnen met [functie chaining - reeks voorbeeld Hello](durable-functions-sequence.md). Het voorbeeld kan lokaal worden uitgevoerd of gepubliceerd naar Azure.
8. Start het project door te voeren in de opdracht vragen / terminal de volgende opdracht uit:
    ```bash
    func host start
    ```

### <a name="start-with-an-empty-project"></a>Beginnen met een leeg project
 
1. Navigeer naar de map die als voor uw functie-app host in de opdracht vragen / terminal.
2. Azure Functions duurzame Extension installeren door te voeren van het volgende in een opdracht vragen / terminal-venster:

    ```bash
    func extensions install -p Microsoft.Azure.WebJobs.Extensions.DurableTask -v 1.6.0
    ```
3. Maak een functie-App-project met de volgende opdracht:

    ```bash
    func init
    ``` 
4. Voer Azure-Opslagemulator of update de *local.appsettings.json* -bestand met echte Azure Storage-verbindingsreeks.
5. Vervolgens een nieuwe functie maken die door de volgende opdracht uit en volg de stappen van de wizard:

    ```bash
    func new
    ```
    >[!IMPORTANT]
    > Op dit moment de duurzame functiesjabloon is niet beschikbaar, maar u kunt beginnen met een van de ondersteunde opties en wijzig vervolgens de code. Gebruik ter referentie de voorbeelden voor [Orchestration-Client](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/csx/HttpStart), [Orchestration Trigger](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/csx/E1_HelloSequence), en [activiteit Trigger](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/csx/E1_HelloSequence).

6. Open de projectmap in Visual Studio Code en gaat u door het wijzigen van de sjablooncode. 
7. Start het project door te voeren in de opdracht vragen / terminal de volgende opdracht uit:
    ```bash
    func host start
    ```

## <a name="azure-portal"></a>Azure Portal

Als u liever, kunt u de [Azure-portal](https://portal.azure.com) voor de ontwikkeling van duurzame functies.

   > [!NOTE]
   > Duurzame functies in JavaScript zijn nog niet beschikbaar in de portal.

### <a name="create-an-orchestrator-function"></a>Een orchestrator-functie maken

1. Een nieuwe functie-app maken in de portal, zoals wordt weergegeven in de [functies snelstartartikel](functions-create-first-azure-function.md#create-a-function-app).

2. De functie-app configureren [gebruikt u de runtimeversie 2.0](set-runtime-version.md).

   De extensie duurzame functies werkt op zowel de 1.X-runtime en de 2.0-runtime, maar de Azure Portal-sjablonen zijn alleen beschikbaar wanneer die gericht is op de 2.0-runtime.

3. Maak een nieuwe functie door te selecteren **'uw eigen aangepaste functie maken'.** .

4. Wijziging de **taal** naar **C#**, **Scenario** naar **duurzame functies** en selecteer de **duurzame functies HTTP-Starter -C#** sjabloon.

5. Onder **extensies zijn niet geïnstalleerd**, klikt u op **installeren** downloaden van de extensie van NuGet.org. 

6. Nadat de installatie voltooid is, gaat u verder met het maken van een functie van de orchestration-client: **"HttpStart"** die is gemaakt door het selecteren van **duurzame functies Http-Starter - C#** sjabloon.

7. Maak nu een functie orchestration **"HelloSequence"** van **duurzame functies - C#** sjabloon.

8. En de laatste functie wordt aangeroepen **"Hallo"** van **activiteit duurzame functies - C#** sjabloon.

9. Ga naar **"HttpStart"** functioneren en kopieert u de URL.

10. Postman of cURL duurzame functie gebruiken. Vervang voordat u kunt testen, in de URL **{functionName}** met de naam van de orchestrator-functie - **HelloSequence**.  Er zijn geen gegevens is vereist, gebruikt u POST-bewerking. 

    ```bash
    curl -X POST https://{your function app name}.azurewebsites.net/api/orchestrators/HelloSequence
    ```

11. Roep vervolgens de **"statusQueryGetUri"** eindpunt en u ziet u de huidige status van de functie duurzame

    ```json
        {
            "runtimeStatus": "Running",
            "input": null,
            "output": null,
            "createdTime": "2017-12-01T05:37:33Z",
            "lastUpdatedTime": "2017-12-01T05:37:36Z"
        }
    ```

12. Aanroepen blijven de **"statusQueryGetUri"** eindpunt totdat de status gewijzigd in **"Voltooid"** 

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

Gefeliciteerd! Uw eerste functie duurzame is actief en werkend in Azure Portal.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [De functie chaining-voorbeeld uitvoeren](durable-functions-sequence.md)
