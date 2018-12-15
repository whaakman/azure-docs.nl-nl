---
title: Uw eerste functie in Linux maken in Azure
description: Informatie over hoe u met de Azure Functions Core Tools en de Azure CLI uw eerste Linux-functie maakt in Azure.
services: functions
keywords: ''
author: ggailey777
ms.author: glenga
ms.date: 11/28/2018
ms.topic: quickstart
ms.service: azure-functions
ms.custom: mvc
ms.devlang: javascript
manager: jeconnoc
ms.openlocfilehash: b6df653f89f05a9b253ecea102ed8310ff2a53b7
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2018
ms.locfileid: "53438271"
---
# <a name="create-your-first-function-hosted-on-linux-using-core-tools-and-the-azure-cli-preview"></a>Uw eerste Linux-functie maken met Azure Functions Core Tools en de Azure CLI (preview)

Met Azure Functions kunt u uw code in een [serverloze](https://azure.com/serverless) Linux-omgeving uitvoeren zonder dat u eerst een virtuele machine moet maken of een webtoepassing publiceren. Hosting in Linux is momenteel alleen in preview en vereist [de Functions 2.0-runtime](functions-versions.md). Zie [dit Werkt op Linux-artikel](https://aka.ms/funclinux) voor meer informatie over preview-overwegingen voor het uitvoeren van uw functie-apps op Linux.

In dit snelstartartikel leert u hoe u de Azure CLI gebruikt om uw eerste functie-app te maken die in Linux wordt uitgevoerd. De functiecode wordt lokaal gemaakt en vervolgens naar Azure geïmplementeerd met behulp van de [Azure Functions Core Tools](functions-run-local.md).

De volgende stappen worden ondersteund op een Mac-, Windows- of Linux-computer. In dit artikel leest u hoe u functies maakt in JavaScript of C#. Zie [Uw eerste Python-functie maken met behulp van Core Tools en de Azure CLI (preview) maken](functions-create-first-function-python.md) voor meer informatie over het maken van Python-functies.

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
+ `python`: hiermee maakt u een Python-project. Zie de [Python-quickstart](functions-create-first-function-python.md) voor Python-functies.

Wanneer de opdracht wordt uitgevoerd, ziet u ongeveer de volgende uitvoer:

```output
Writing .gitignore
Writing host.json
Writing local.settings.json
Initialized empty Git repository in C:/functions/MyFunctionProj/.git/
```

[!INCLUDE [functions-create-function-core-tools](../../includes/functions-create-function-core-tools.md)]

[!INCLUDE [functions-update-function-code](../../includes/functions-update-function-code.md)]

[!INCLUDE [functions-run-function-test-local](../../includes/functions-run-function-test-local.md)]

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-linux-app-service-plan"></a>Een Linux App Service-abonnement maken

[!INCLUDE [app-service-plan-no-h](../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

## <a name="create-a-linux-function-app-in-azure"></a>Een Linux-functie-app maken in Azure

U moet beschikken over een functie-app om de uitvoering van uw functies in Linux te hosten. De functie-app biedt een serverloze omgeving voor de uitvoering van uw functiecode. U kunt er functies mee groeperen in een logische eenheid, zodat u resources eenvoudiger kunt beheren, implementeren en delen. U maakt een functie-app die in Linux wordt uitgevoerd met behulp van de opdracht [az functionapp create](/cli/azure/functionapp#az-functionapp-create).

Gebruik in de volgende opdracht een unieke functie-appnaam in plaats van de tijdelijke plaatsaanduiding `<app_name>` en gebruik de naam van het opslagaccount in plaats van `<storage_name>`. De `<app_name>` is ook het standaard DNS-domein voor de functie-app. Deze naam moet uniek zijn in alle apps in Azure. U moet ook de `<language>`-runtime voor uw functie-app instellen, vanuit `dotnet` (C#), `node` (JavaScript) of `python`.

```azurecli-interactive
az functionapp create --resource-group myResourceGroup --consumption-plan-location westus --os-type Linux \
--name <app_name> --storage-account  <storage_name> --runtime <language>
```

> [!NOTE]
> Als u al een resourcegroep met de naam `myResourceGroup` hebt die niet-Linux App Service-apps bevat, moet u een andere resourcegroep gebruiken. Het is niet mogelijk om zowel Windows- als Linux-apps in dezelfde resourcegroep uit te voeren.  

Nadat de functie-app is gemaakt, ziet u het volgende bericht:

```output
Your serverless Linux function app 'myfunctionapp' has been successfully created.
To active this function app, publish your app content using Azure Functions Core Tools or the Azure portal.
```

Nu kunt u uw project publiceren naar de nieuwe functie-app in Azure.

[!INCLUDE [functions-publish-project](../../includes/functions-publish-project.md)]

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

## <a name="next-steps"></a>Volgende stappen

In dit artikel ziet u hoe u uw functie-app uitvoert in een standaard Azure App Service-container. U kunt uw functies voor Linux ook uitvoeren in uw eigen aangepaste container.

> [!div class="nextstepaction"]
> [Een functie in Linux maken met een aangepaste installatiekopie](functions-create-function-linux-custom-image.md)
