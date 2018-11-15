---
title: Uw eerste functie maken met Azure CLI
description: Informatie over het maken van uw eerste Azure-functie met behulp van Azure CLI en Azure Functions Core Tools.
services: functions
keywords: ''
author: ggailey777
ms.author: glenga
ms.assetid: 674a01a7-fd34-4775-8b69-893182742ae0
ms.date: 11/13/2018
ms.topic: quickstart
ms.service: azure-functions
ms.custom: mvc
ms.devlang: azure-cli
manager: jeconnoc
ms.openlocfilehash: 05b35ac182d70d6d7a7630a14c8a8aa3b7a6a9fd
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51634308"
---
# <a name="create-your-first-function-from-the-command-line"></a>Uw eerste functie maken vanaf de opdrachtregel

In dit onderwerp van de zelfstudie wordt stapsgewijs uitgelegd hoe u uw eerste functie maakt vanaf de opdrachtregel of in de terminal. Azure CLI gebruikt u om een functie-app te maken. Het is de [serverloze](https://azure.microsoft.com/solutions/serverless/) infrastructuur die als host fungeert voor uw functie. Het functiecodeproject wordt gegenereerd vanuit een sjabloon met behulp van de [Azure Functions Core Tools](functions-run-local.md), dat ook wordt gebruikt om het functie-app-project te implementeren naar Azure.

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

Wanneer u hierom wordt gevraagd, selecteert u een runtime voor de werkrol uit de volgende taalopties:

+ `dotnet`: hiermee maakt u een .NET-klassebibliotheekproject (.csproj).
+ `node`: hiermee maakt u een JavaScript-project.

Wanneer de opdracht wordt uitgevoerd, ziet u ongeveer de volgende uitvoer:

```output
Writing .gitignore
Writing host.json
Writing local.settings.json
Initialized empty Git repository in C:/functions/MyFunctionProj/.git/
```

Gebruik de volgende opdracht om naar de nieuwe projectmap `MyFunctionProj` te navigeren.

```bash
cd MyFunctionProj
```

[!INCLUDE [functions-create-function-core-tools](../../includes/functions-create-function-core-tools.md)]

[!INCLUDE [functions-update-function-code](../../includes/functions-update-function-code.md)]

[!INCLUDE [functions-run-function-test-local](../../includes/functions-run-function-test-local.md)]

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

### <a name="configure-the-function-app-nodejs"></a>De functie-app configureren (Node.js)

Als u een functie-app in JavaScript maakt, is het belangrijk dat u zich op de juiste Node.js-versie richt. Voor versie 2.x van de Functions-runtime is versie 8.x van Node.js vereist. Instelling `WEBSITE_NODE_DEFAULT_VERSION` van de toepassing regelt de versie van Node.js die wordt gebruikt door de functie-app in Azure. Gebruik de opdracht [az functionapp config appsettings set](https://docs.microsoft.com/cli/azure/functionapp/config/appsettings#set) om de Node.js-versie in te stellen op `8.11.1`.

In de volgende Azure CLI-opdracht is < app_name> de naam van uw functie-app.

```azurecli-interactive
az functionapp config appsettings set --resource-group myResourceGroup \
 --name <app_name> --settings WEBSITE_NODE_DEFAULT_VERSION=8.11.1
```

Controleer de nieuwe instelling in de uitvoer.

[!INCLUDE [functions-publish-project](../../includes/functions-publish-project.md)]

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

[!INCLUDE [functions-quickstart-next-steps-cli](../../includes/functions-quickstart-next-steps-cli.md)]

