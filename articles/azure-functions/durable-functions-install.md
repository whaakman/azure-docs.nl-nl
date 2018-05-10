---
title: Installeer de duurzame functies extensie en steekproeven - Azure
description: Informatie over het installeren van de extensie duurzame functies voor Azure Functions voor portal ontwikkeling of Visual Studio-ontwikkeling.
services: functions
author: cgillum
manager: cfowler
editor: ''
tags: ''
keywords: ''
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/19/2018
ms.author: azfuncdf
ms.openlocfilehash: 4dd4bbb9c382b772f8f60b259844e7e471ec73e3
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="install-the-durable-functions-extension-and-samples-azure-functions"></a>Installeer de duurzame functies extensie en voorbeelden (Azure-functies)

De [duurzame functies](durable-functions-overview.md) -extensie voor Azure Functions is opgegeven in het NuGet-pakket [Microsoft.Azure.WebJobs.Extensions.DurableTask](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DurableTask). Dit artikel laat zien hoe u het pakket en een reeks voorbeelden voor het volgende ontwikkelomgevingen installeert:

* Visual Studio 2017 (aanbevolen voor C#) 
* Visual Studio Code (aanbevolen voor JavaScript)
* Azure Portal

## <a name="visual-studio-2017"></a>Visual Studio 2017

Visual Studio biedt momenteel de beste ervaring voor het ontwikkelen van apps die gebruikmaken van duurzame functies.  Uw functies kunnen lokaal worden uitgevoerd en kunnen ook worden gepubliceerd naar Azure. U kunt beginnen met een leeg project of met een reeks Voorbeeldfuncties.

### <a name="prerequisites"></a>Vereisten

* Installeer de [meest recente versie van Visual Studio](https://www.visualstudio.com/downloads/) (versie 15.3 of hoger). Bevatten de **ontwikkelen van Azure** werkbelasting in uw installatieopties.

### <a name="start-with-sample-functions"></a>Beginnen met Voorbeeldfuncties 

1. Download de [Voorbeeldapp ZIP-bestand voor Visual Studio](https://azure.github.io/azure-functions-durable-extension/files/VSDFSampleApp.zip). U hoeft niet de NuGet-verwijzing toevoegen omdat het voorbeeldproject het al heeft.
2. Installeren en uitvoeren [Azure-Opslagemulator](https://docs.microsoft.com/azure/storage/storage-use-emulator) versie 5.2 of hoger. U kunt ook bijwerken de *local.appsettings.json* bestand met echte Azure Storage-verbindingsreeksen.
3. Open het project in Visual Studio 2017. 
4. Voor instructies over het uitvoeren van de steekproef beginnen met [werken chaining - reeks voorbeeld Hallo](durable-functions-sequence.md). Het voorbeeld worden lokaal uitvoeren of gepubliceerd naar Azure.

### <a name="start-with-an-empty-project"></a>Beginnen met een leeg project
 
Volg de aanwijzingen dezelfde als voor het beginnen met het voorbeeld, maar de volgende stappen uit te voeren in plaats van gedownload de *.zip* bestand:

1. Een functie-App-project maken.
2. Zoeken naar de volgende NuGet pakket verwijzing met behulp van *NuGet-pakketten beheren* en voeg deze toe aan het project: Microsoft.Azure.WebJobs.Extensions.DurableTask v1.4.0 (controleren *Include prerelease* naar Zoek op voor dit pakket)
   
## <a name="visual-studio-code"></a>Visual Studio Code

Visual Studio Code bevat een lokale ontwikkeling ervaring die betrekking hebben op alle primaire platforms - Windows-, Mac OS- en Linux.  Uw functies kunnen lokaal worden uitgevoerd en ook worden gepubliceerd naar Azure. U kunt beginnen met een leeg project of met een reeks Voorbeeldfuncties.

### <a name="prerequisites"></a>Vereisten

* Installeer de [meest recente versie van Visual Studio Code](https://code.visualstudio.com/Download) 

* Volg de instructies in 'Installeer de Azure Functions Core Tools' op [Code en test Azure Functions lokaal](https://docs.microsoft.com/azure/azure-functions/functions-run-local)

    >[!IMPORTANT]
    > Als u de Platform hulpprogramma's voor Azure Functions al hebt, kunt u ze naar de meest recente versie bijwerken.

    >[!IMPORTANT]
    >Duurzame functies in JavaScript vereist versie 2.x van de hulpprogramma's van Azure Functions Core.

*  Als u zich op een Windows-machine, installeren en uitvoeren [Azure-Opslagemulator](https://docs.microsoft.com/azure/storage/storage-use-emulator) versie 5.2 of hoger. U kunt ook bijwerken de *local.appsettings.json* bestand met echte Azure Storage-verbinding. 


### <a name="start-with-sample-functions"></a>Beginnen met Voorbeeldfuncties

#### <a name="c"></a>C#

1. Kloon de [duurzame functies opslagplaats](https://github.com/Azure/azure-functions-durable-extension.git).
2. Navigeer op het apparaat aan de [C# scriptmap met voorbeelden](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/csx). 
3. Azure Functions duurzame uitbreiding installeren door het volgende in een opdracht vragen / terminal-venster:

    ```bash
    func extensions install -p Microsoft.Azure.WebJobs.Extensions.DurableTask -v 1.4.0
    ```
4. Azure Functions Twilio-uitbreiding installeren door het volgende in een opdracht vragen / terminal-venster:

    ```bash
    func extensions install -p Microsoft.Azure.WebJobs.Extensions.Twilio -v 3.0.0-beta5
    ```
5. Azure-Opslagemulator of update uitvoeren de *local.appsettings.json* bestand met de werkelijke verbindingsreeks voor Azure Storage.
6. Open het project in Visual Studio Code. 
7. Voor instructies over het uitvoeren van de steekproef beginnen met [werken chaining - reeks voorbeeld Hallo](durable-functions-sequence.md). Het voorbeeld worden lokaal uitvoeren of gepubliceerd naar Azure.
8. Het project starten door in te voeren opdracht vragen / terminal de volgende opdracht:
    ```bash
    func host start
    ```

#### <a name="javascript-functions-v2-only"></a>JavaScript (alleen functies v2)

1. Kloon de [duurzame functies opslagplaats](https://github.com/Azure/azure-functions-durable-extension.git).
2. Navigeer op het apparaat aan de [map met voorbeelden JavaScript](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/javascript). 
3. Azure Functions duurzame uitbreiding installeren door het volgende in een opdracht vragen / terminal-venster:

    ```bash
    func extensions install -p Microsoft.Azure.WebJobs.Extensions.DurableTask -v 1.4.0
    ```
4. De npm-pakketten te herstellen door het volgende in een opdracht vragen / terminal-venster:
    
    ```bash
    npm install
    ``` 
5. Update de *local.appsettings.json* bestand met de werkelijke verbindingsreeks voor Azure Storage.
6. Open het project in Visual Studio Code. 
7. Voor instructies over het uitvoeren van de steekproef beginnen met [werken chaining - reeks voorbeeld Hallo](durable-functions-sequence.md). Het voorbeeld worden lokaal uitvoeren of gepubliceerd naar Azure.
8. Het project starten door in te voeren opdracht vragen / terminal de volgende opdracht:
    ```bash
    func host start
    ```

### <a name="start-with-an-empty-project"></a>Beginnen met een leeg project
 
1. Navigeer naar de map die als host voor de functie-app fungeert in de opdracht vragen / terminal.
2. Een duurzame uitbreiding van de Azure-functies installeren door het volgende in een opdracht vragen / terminal-venster:

    ```bash
    func extensions install -p Microsoft.Azure.WebJobs.Extensions.DurableTask -v 1.4.0
    ```
3. Een functie-App-project maken met de volgende opdracht:

    ```bash
    func init
    ``` 
4. Azure-Opslagemulator of update uitvoeren de *local.appsettings.json* bestand met de werkelijke verbindingsreeks voor Azure Storage.
5. Vervolgens maakt u een nieuwe functie met de volgende opdracht en volg de stappen van de wizard:

    ```bash
    func new
    ```
    >[!IMPORTANT]
    > Op dit moment de sjabloon duurzame functie is niet beschikbaar, maar u kunt beginnen met een van de ondersteunde opties en wijzig vervolgens de code. Gebruik voor de verwijzing naar de voorbeelden voor [Orchestration Client](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/csx/HttpStart), [Orchestration Trigger](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/csx/E1_HelloSequence), en [activiteit Trigger](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/csx/E1_HelloSequence).

6. Open de projectmap in Visual Studio Code en gaat u door het wijzigen van de sjablooncode. 
7. Het project starten door in te voeren opdracht vragen / terminal de volgende opdracht:
    ```bash
    func host start
    ```

## <a name="azure-portal"></a>Azure Portal

Als u liever, kunt u de Azure-portal voor het ontwikkelen van duurzame functies.

   > [!NOTE]
   > Duurzame functies in JavaScript zijn nog niet beschikbaar in de portal.

### <a name="create-an-orchestrator-function"></a>Een orchestrator-functie maken

1. Maak een nieuwe functie-app op [functions.azure.com](https://functions.azure.com/signin).

2. Configureren van de functie app [versie 2.0 runtime](set-runtime-version.md).

   De extensie duurzame functies werkt in zowel de 1.X-runtime en de 2.0 runtime, maar de Azure Portal-sjablonen zijn alleen beschikbaar wanneer de doelcomputer de 2.0-runtime.

3. Maak een nieuwe functie door te selecteren **'uw eigen aangepaste functie maken'.** .

4. Wijziging de **taal** naar **C#**, **Scenario** naar **duurzame functies** en selecteer de **duurzame functies Http Starter -C#** sjabloon.

5. Onder **extensies is niet geïnstalleerd**, klikt u op **installeren** downloaden van de uitbreiding van NuGet.org. 

6. Nadat de installatie voltooid is, gaat u verder met het maken van een functie van de orchestration-client – **'HttpStart'** die is gemaakt door het selecteren van **duurzame functies Http Starter - C#** sjabloon.

7. Maak nu een functie orchestration **'HelloSequence'** van **duurzame functies Orchestrator - C#** sjabloon.

8. En de laatste functie moet worden aangeroepen **'Hallo'** van **duurzame functies activiteit - C#** sjabloon.

9. Ga naar **'HttpStart'** werken en kopieer de URL.

10. Gebruik Postman of cURL de duurzame functie aanroept. Voordat u gaat testen, vervangt u in de URL **{functienaam}** met de naam van de orchestrator-functie - **HelloSequence**.  Er zijn geen gegevens is vereist, gebruiken de term POST. 

    ```bash
    curl -X POST https://{your function app name}.azurewebsites.net/api/orchestrators/HelloSequence
    ```

11. Roep vervolgens de **'statusQueryGetUri'** eindpunt en u ziet u de huidige status van de functie duurzame

    ```json
        {
            "runtimeStatus": "Running",
            "input": null,
            "output": null,
            "createdTime": "2017-12-01T05:37:33Z",
            "lastUpdatedTime": "2017-12-01T05:37:36Z"
        }
    ```

12. Aanroepen gaan de **'statusQueryGetUri'** eindpunt totdat de status gewijzigd in **"Voltooid"** 

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

Gefeliciteerd! Uw eerste functie duurzame is actief in Azure-Portal.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Voer de functie sample-koppeling](durable-functions-sequence.md)
