---
title: Uw eerste functie maken met Azure CLI
description: Informatie over het maken van uw eerste Azure-functie met behulp van Azure CLI en Azure Functions Core Tools.
services: functions
keywords: ''
author: ggailey777
ms.author: glenga
ms.assetid: 674a01a7-fd34-4775-8b69-893182742ae0
ms.date: 09/10/2018
ms.topic: quickstart
ms.service: azure-functions
ms.custom: mvc
ms.devlang: azure-cli
manager: jeconnoc
ms.openlocfilehash: f65ff3e38e3768dccf04e9eaa4345ffcea4daa9e
ms.sourcegitcommit: f31bfb398430ed7d66a85c7ca1f1cc9943656678
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/28/2018
ms.locfileid: "47451117"
---
# <a name="create-your-first-function-from-the-command-line"></a>Uw eerste functie maken vanaf de opdrachtregel

In dit onderwerp van de zelfstudie wordt stapsgewijs uitgelegd hoe u uw eerste functie maakt vanaf de opdrachtregel of in de terminal. Azure CLI gebruikt u om een functie-app te maken. Het is de [serverloze](https://azure.microsoft.com/overview/serverless-computing/) infrastructuur die als host fungeert voor uw functie. Het functiecodeproject wordt gegenereerd vanuit een sjabloon met behulp van de [Azure Functions Core Tools](functions-run-local.md), dat ook wordt gebruikt om het functie-app-project te implementeren naar Azure.

U kunt de onderstaande stappen volgen op een Mac-, Windows- of Linux-computer.

## <a name="prerequisites"></a>Vereisten

Voordat u dit voorbeeld kunt uitvoeren moet u ervoor zorgen dat u het volgende hebt:

+ Installeer [Azure Core Tools versie 2.x](functions-run-local.md#v2).

+ Installeer de [Azure CLI]( /cli/azure/install-azure-cli). In dit artikel is Azure CLI versie 2.0 of hoger vereist. Voer `az --version` uit om te zien welke versie u hebt. U kunt ook de [Azure Cloud Shell](https://shell.azure.com/bash) gebruiken.

+ Een actief Azure-abonnement.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-the-local-function-app-project"></a>Het lokale functie-app-project maken

Voer de volgende opdracht uit vanaf de opdrachtregel om een functie-app-project te maken in de map `MyFunctionProj` van de huidige lokale map. Er wordt ook een GitHub-opslagplaats gemaakt in `MyFunctionProj`.

```bash
func init MyFunctionProj
```

Wanneer u hierom wordt gevraagd, gebruikt u de pijltoetsen om een runtime voor de werkrol te selecteren uit de volgende taalopties:

+ `dotnet`: hiermee maakt u een .NET-klassebibliotheekproject (.csproj).
+ `node`: hiermee maakt u een JavaScript-project.

Wanneer de opdracht wordt uitgevoerd, ziet u ongeveer de volgende uitvoer:

```output
Writing .gitignore
Writing host.json
Writing local.settings.json
Initialized empty Git repository in C:/functions/MyFunctionProj/.git/
```

## <a name="create-a-function"></a>Een functie maken

Met de volgende opdracht navigeert u naar het nieuwe project en maakt u een HTTP-geactiveerde functie met de naam `MyHtpTrigger`.

```bash
cd MyFunctionProj
func new --name MyHttpTrigger --template "HttpTrigger"
```

Wanneer de opdracht wordt uitgevoerd, ziet u ongeveer de volgende uitvoer. Dit is een JavaScript-functie:

```output
Writing C:\functions\MyFunctionProj\MyHttpTrigger\index.js
Writing C:\functions\MyFunctionProj\MyHttpTrigger\sample.dat
Writing C:\functions\MyFunctionProj\MyHttpTrigger\function.json
```

## <a name="edit-the-function"></a>De functie bewerken

Standaard maakt de sjabloon een functie die een functie-sleutel vereist bij het doen van aanvragen. Om het testen van de functie in Azure te vergemakkelijken, moet u de functie bijwerken voor het toestaan van anonieme toegang. De manier waarop u deze wijziging aanbrengt, is afhankelijk van de taal van uw functieproject.

### <a name="c"></a>C\#

Open het code-bestand MyHttpTrigger.cs dat uw nieuwe functie is en wijzig het kenmerk **AuthorizationLevel** in de functiedefinitie in de waarde van `anonymous` en sla de wijzigingen op.

```csharp
[FunctionName("MyHttpTrigger")]
        public static IActionResult Run([HttpTrigger(AuthorizationLevel.Anonymous, 
            "get", "post", Route = null)]HttpRequest req, ILogger log)
```

### <a name="javascript"></a>Javascript

Open het bestand function.json voor de nieuwe functie in een teksteditor, wijzig de eigenschap **authLevel** in **bindings.httpTrigger** in `anonymous` en sla de wijzigingen op.

```json
  "bindings": [
    {
      "authLevel": "anonymous",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    }
  ]
```

Nu kunt u de functie aanroepen in Azure zonder dat u de functiesleutel hoeft op te geven. De functiesleutel is nooit vereist wanneer de functie lokaal wordt uitgevoerd.

## <a name="run-the-function-locally"></a>De functie lokaal uitvoeren

Met de volgende opdracht start u de functie-app. De app wordt uitgevoerd met dezelfde Azure Functions-runtime als die van Azure.

```bash
func host start --build
```

De optie `--build` is vereist voor het compileren van C#-projecten. U hebt deze optie niet nodig voor een JavaScript-project.

Wanneer de Functions-host start, wordt ongeveer de volgende uitvoer weergegeven. Deze is voor de leesbaarheid afgekapt:

```output

                  %%%%%%
                 %%%%%%
            @   %%%%%%    @
          @@   %%%%%%      @@
       @@@    %%%%%%%%%%%    @@@
     @@      %%%%%%%%%%        @@
       @@         %%%%       @@
         @@      %%%       @@
           @@    %%      @@
                %%
                %

...

Content root path: C:\functions\MyFunctionProj
Now listening on: http://0.0.0.0:7071
Application started. Press Ctrl+C to shut down.

...

Http Functions:

        HttpTrigger: http://localhost:7071/api/HttpTrigger

[8/27/2018 10:38:27 PM] Host started (29486ms)
[8/27/2018 10:38:27 PM] Job host started
```

Kopieer de URL van uw `HTTPTrigger`-functie uit de uitvoer van de runtime en plak deze in de adresbalk van uw browser. Voeg de queryreeks `?name=<yourname>` toe aan de URL en voer de aanvraag uit. Hieronder ziet u de reactie op de GET-aanvraag die door de lokale functie wordt geretourneerd, weergegeven in de browser:

![Lokaal testen in de browser](./media/functions-create-first-azure-function-azure-cli/functions-test-local-browser.png)

Nu u de functie lokaal hebt uitgevoerd, kunt u de functie-app en andere vereiste resources maken in Azure.

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-function-app"></a>Een functie-app maken

U moet een functie-app hebben die als host fungeert voor de uitvoering van uw functies. De functie-app biedt een omgeving waarin uw functiecode zonder server kan worden uitgevoerd. U kunt er functies mee groeperen in een logische eenheid, zodat u resources eenvoudiger kunt beheren, implementeren en delen. Een functie-app maken met behulp van de opdracht [az functionapp create](/cli/azure/functionapp#az-functionapp-create). 

Vervang in de volgende opdracht de plaatsaanduiding `<app_name>` door een unieke functie-appnaam en gebruik de naam van het opslagaccount voor `<storage_name>`. De `<app_name>` wordt gebruikt als het standaard DNS-domein voor de functie-app. Om die reden moet de naam uniek zijn in alle apps in Azure. De parameter _deployment-source-url_ verwijst naar een opslagplaats met voorbeeld in GitHub die een door HTTP getriggerde functie 'Hello World' bevat.

```azurecli-interactive
az functionapp create --resource-group myResourceGroup --consumption-plan-location westeurope \
--name <app_name> --storage-account  <storage_name>  
```

Als u de parameter _consumption-plan-location_ instelt, betekent dat dat de function-app wordt gehost in een hostingabonnement van het type Consumption. In dit serverloze abonnement worden resources naar behoefte dynamisch door uw functies toegevoegd, en betaalt u alleen wanneer functies worden uitgevoerd. Zie [Het juiste hostingabonnement kiezen](functions-scale.md) voor meer informatie.

Nadat de functie-app is gemaakt, toont Azure CLI soortgelijke informatie als in het volgende voorbeeld:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "containerSize": 1536,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "quickstart.azurewebsites.net",
  "enabled": true,
  "enabledHostNames": [
    "quickstart.azurewebsites.net",
    "quickstart.scm.azurewebsites.net"
  ],
   ....
    // Remaining output has been truncated for readability.
}
```

## <a name="configure-the-function-app"></a>De functie-app configureren

Met Core Tools versie 2.x worden projecten gemaakt met sjablonen voor de Azure Functions 2.x-runtime. Daarom moet u controleren of de runtime van versie 2.x in Azure wordt gebruikt. Door de toepassingsinstelling `FUNCTIONS_WORKER_RUNTIME` in te stellen op `~2`, dwingt u de functie-app de meest recente 2.x-versie te gebruiken. Stel toepassingsinstellingen in met de opdracht [az functionapp config appsettings set](https://docs.microsoft.com/cli/azure/functionapp/config/appsettings#set).

In de volgende Azure CLI-opdracht is < app_name> de naam van uw functie-app.

```azurecli-interactive
az functionapp config appsettings set --name <app_name> \
--resource-group myResourceGroup \
--settings FUNCTIONS_WORKER_RUNTIME=~2
```

[!INCLUDE [functions-publish-project](../../includes/functions-publish-project.md)]

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

[!INCLUDE [functions-quickstart-next-steps-cli](../../includes/functions-quickstart-next-steps-cli.md)]
