---
title: Een functie-app in Linux maken in een Azure App Service-plan
description: Informatie over het maken van een functie-app die wordt uitgevoerd op Linux in een App Service-plan met de Azure CLI.
services: functions
keywords: ''
author: ggailey777
ms.author: glenga
ms.date: 11/28/2018
ms.topic: conceptual
ms.service: azure-functions
ms.custom: mvc
ms.devlang: azure-cli
manager: jeconnoc
ms.openlocfilehash: d9800ff3fc82636c5cae12167738667ec84326ee
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52856154"
---
# <a name="create-a-function-app-on-linux-in-an-azure-app-service-plan-preview"></a>Een functie-app in Linux maken in een Azure App Service-abonnement (preview)

Met Azure Functions kunt u uw functies voor Linux hosten in een standaardcontainer van Azure App Service. In dit artikel leert u hoe u de Azure CLI gebruiken om te maken van een Linux-host functie-app in Azure die wordt uitgevoerd in een [App Service-plan](functions-scale.md#app-service-plan). U kunt ook [uw eigen aangepaste container](functions-create-function-linux-custom-image.md) gebruiken. Linux-hosting is momenteel in preview.

In een App Service-plan bent u verantwoordelijk voor het schalen van uw functie-app. Als u wilt profiteren van de serverloze mogelijkheden van Azure Functions, kunt u ook uw functies voor Linux in host een [verbruiksabonnement](functions-scale.md#consumption-plan).

U kunt de onderstaande stappen volgen op een Mac-, Windows- of Linux-computer.

## <a name="prerequisites"></a>Vereisten

U hebt het volgende nodig om deze quickstart te voltooien:

+ Een actief Azure-abonnement.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit onderwerp gebruikmaken van Azure CLI versie 2.0.21 of hoger. Voer `az --version` uit om te zien welke versie u hebt. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-linux-app-service-plan"></a>Een Linux App Service-abonnement maken

[!INCLUDE [app-service-plan-no-h](../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

## <a name="create-a-function-app-on-linux"></a>Een functie-app maken in Linux

U moet beschikken over een functie-app om de uitvoering van uw functies in Linux te hosten. De functie-app biedt een omgeving waarin uw functiecode kan worden uitgevoerd. U kunt er functies mee groeperen in een logische eenheid, zodat u resources eenvoudiger kunt beheren, implementeren en delen. Maak een functie-app door de opdracht [az functionapp create](/cli/azure/functionapp#az-functionapp-create) te gebruiken met een Linux App Service-abonnement.

Vervang in de volgende opdracht de plaatsaanduiding `<app_name>` door een unieke functie-appnaam en gebruik de naam van het opslagaccount voor `<storage_name>`. De `<app_name>` wordt gebruikt als het standaard DNS-domein voor de functie-app. Om die reden moet de naam uniek zijn in alle apps in Azure. U moet ook ingesteld de `<language>` -runtime voor uw functie-app van `dotnet` (C#), `node` (JavaScript), of `python`.

```azurecli-interactive
az functionapp create --resource-group myResourceGroup --plan myAppServicePlan \
--name <app_name> --storage-account  <storage_name> --runtime <language>
```

Nadat de functie-app is gemaakt en geïmplementeerd, ziet u in Azure CLI soortgelijke informatie als in het volgende voorbeeld:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
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

Omdat `myAppServicePlan` een Linux-abonnement is, wordt de ingebouwde docker-installatiekopie gebruikt voor het maken van de container waarmee de functie-app in Linux wordt uitgevoerd.

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources-simple.md)]

## <a name="next-steps"></a>Volgende stappen

In dit artikel wordt beschreven hoe u een functie-app voor Linux die worden gehost in Azure maakt. U kunt nu [implementeren een functieproject](https://docs.microsoft.com/cli/azure/functionapp/deployment/source?view=azure-cli-latest) aan deze functie-app. U kunt de Azure Functions Core Tools te gebruiken [een Functions-project maken](functions-run-local.md#create-a-local-functions-project) op uw lokale computer en deze implementeren in uw nieuwe functie-app voor Linux.  

> [!div class="nextstepaction"] 
> [Azure-functies lokaal programmeren en testen](functions-run-local.md)
