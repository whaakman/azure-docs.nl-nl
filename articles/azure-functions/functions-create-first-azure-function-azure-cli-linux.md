---
title: Uw eerste functie maken in Linux met Azure CLI (preview) | Microsoft Docs
description: Informatie over het maken van uw eerste Azure-functie uitgevoerd op een standaard Linux-installatiekopie met de Azure CLI.
services: functions
keywords: 
author: ggailey777
ms.author: glenga
ms.date: 11/07/2017
ms.topic: quickstart
ms.service: functions
ms.custom: mvc
ms.devlang: azure-cli
manager: cfowler
ms.openlocfilehash: 4adc25dbca06271382dc76690c75d3198d59d4be
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2017
---
# <a name="create-your-first-function-running-on-linux-using-the-azure-cli-preview"></a>Uw eerste functie met op Linux met de Azure CLI (preview) maken

Azure Functions, kunt u uw functies op Linux in Azure App Service standaardcontainer hosten. Deze functionaliteit is momenteel in preview. U kunt ook [brengt uw eigen aangepaste container](functions-create-function-linux-custom-image.md). 

In dit onderwerp Quick Start begeleidt u bij het gebruik van Azure Functions met de Azure CLI voor het maken van uw eerste functie-app op Linux gehost op de standaard-App Service-container. De functiecode zelf wordt geïmplementeerd op de installatiekopie van een steekproef GitHub-opslagplaats.    

De volgende stappen uit worden op een Mac-, Windows- of Linux-computer ondersteund. 

## <a name="prerequisites"></a>Vereisten 

U hebt het volgende nodig om deze snelstartgids te voltooien:

+ Een actief [GitHub](https://github.com)-account. 
+ Een actief Azure-abonnement.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u wilt installeren en de CLI lokaal gebruiken, moet dit onderwerp de Azure CLI versie 2.0 of hoger. Voer `az --version` de versie die u hebt te vinden. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-linux-app-service-plan"></a>Een Linux-App Service-abonnement maken

Linux die als host fungeert voor functies is momenteel alleen ondersteund op een App Service-abonnement. Hosting-verbruik plan is nog niet ondersteund. Zie voor meer informatie over het hosten van [vergelijking van Azure Functions hosting plannen](functions-scale.md). 

[!INCLUDE [app-service-plan-no-h](../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

## <a name="create-a-function-app-on-linux"></a>Een functie-app maken in Linux

U moet een functie-app voor het hosten van de uitvoering van uw functies op Linux hebben. De functie-app biedt een omgeving voor uitvoering van de functiecode. U kunt er functies mee groeperen in een logische eenheid, zodat u resources eenvoudiger kunt beheren, implementeren en delen. Een functie-app maken met behulp van de [az functionapp maken](/cli/azure/functionapp#create) opdracht met een Linux-App Service-abonnement. 

Vervang de naam van een unieke functie-app waarin u ziet in de volgende opdracht de `<app_name>` tijdelijke aanduiding en het storage-account een naam voor `<storage_name>`. De `<app_name>` wordt gebruikt als het standaard DNS-domein voor de functie-app. Om die reden moet de naam uniek zijn in alle apps in Azure. De _implementatie-bron-url_ parameter is een voorbeeld-opslagplaats in GitHub met een functie 'Hallo wereld' HTTP geactiveerd.

```azurecli-interactive
az functionapp create --name <app_name> --storage-account  <storage_name>  --resource-group myResourceGroup \
--plan myAppServicePlan --deployment-source-url https://github.com/Azure-Samples/functions-quickstart-linux
```
Nadat de functie is app gemaakt en geïmplementeerd, de Azure CLI geeft informatie weer zoals in het volgende voorbeeld:

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

Omdat `myAppServicePlan` is een Linux-abonnement, de installatiekopie van het ingebouwde docker gebruikt voor het maken van de container die de functie-app op Linux wordt uitgevoerd. 

>[!NOTE]  
>De voorbeeld-opslagplaats bevat momenteel twee scriptbestanden [deploy.sh](https://github.com/Azure-Samples/functions-quickstart-linux/blob/master/deploy.sh) en [.deployment](https://github.com/Azure-Samples/functions-quickstart-linux/blob/master/.deployment). Het bestand .deployment geeft aan het implementatieproces te gebruiken deploy.sh als de [aangepaste implementatiescript](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script). Scripts zijn in de huidige preview-versie vereist voor het implementeren van de functie-app op een Linux-installatiekopie.  

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

[!INCLUDE [functions-quickstart-next-steps-cli](../../includes/functions-quickstart-next-steps-cli.md)]
