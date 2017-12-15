---
title: Uw eerste Linux-functie maken vanuit Azure CLI (preview) | Microsoft Docs
description: Lees hier hoe u met behulp van Azure CLI uw eerste Azure-functie maakt voor uitvoering met een standaardinstallatiekopie voor Linux.
services: functions
keywords: 
author: ggailey777
ms.author: glenga
ms.date: 11/15/2017
ms.topic: quickstart
ms.service: functions
ms.custom: mvc
ms.devlang: azure-cli
manager: cfowler
ms.openlocfilehash: d04e2000f2043e8bb11e15f6b9d7fd06ef5b9da3
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2017
---
# <a name="create-your-first-function-running-on-linux-using-the-azure-cli-preview"></a>Uw eerste Linux-functie maken vanuit Azure CLI (preview)

Met Azure Functions kunt u uw functies voor Linux hosten in een standaardcontainer van Azure App Service. Deze functionaliteit is momenteel in preview. U kunt ook [uw eigen aangepaste container](functions-create-function-linux-custom-image.md) gebruiken. 

In deze quickstart wordt stapsgewijs uitgelegd hoe u Azure Functions gebruikt met de Azure CLI voor het maken van uw eerste functie-app voor Linux, gehost in de standaardcontainer van App Service. De functiecode zelf wordt vanuit een opslagplaats met voorbeelden van GitHub geïmplementeerd naar de installatiekopie.    

De volgende stappen worden ondersteund op een Mac-, Windows- of Linux-computer. 

## <a name="prerequisites"></a>Vereisten 

U hebt het volgende nodig om deze quickstart te voltooien:

+ Een actief Azure-abonnement.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit onderwerp gebruikmaken van Azure CLI versie 2.0.21 of hoger. Voer `az --version` uit om te zien welke versie u hebt. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-linux-app-service-plan"></a>Een Linux App Service-abonnement maken

Linux-hosting voor Functions wordt momenteel alleen ondersteund via een App Service-abonnement. Hosting via een verbruiksabonnement wordt nog niet ondersteund. Zie [deze vergelijking van hostingabonnementen van Azure Functions](functions-scale.md) voor meer informatie over hosting. 

[!INCLUDE [app-service-plan-no-h](../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

## <a name="create-a-function-app-on-linux"></a>Een functie-app maken in Linux

U moet beschikken over een functie-app om de uitvoering van uw functies in Linux te hosten. De functie-app biedt een omgeving waarin uw functiecode kan worden uitgevoerd. U kunt er functies mee groeperen in een logische eenheid, zodat u resources eenvoudiger kunt beheren, implementeren en delen. Maak een functie-app door de opdracht [az functionapp create](/cli/azure/functionapp#create) te gebruiken met een Linux App Service-abonnement. 

Vervang in de volgende opdracht de plaatsaanduiding `<app_name>` door een unieke functie-appnaam en gebruik de naam van het opslagaccount voor `<storage_name>`. De `<app_name>` wordt gebruikt als het standaard DNS-domein voor de functie-app. Om die reden moet de naam uniek zijn in alle apps in Azure. De parameter _deployment-source-url_ verwijst naar een opslagplaats met voorbeeld in GitHub die een door HTTP getriggerde functie 'Hello World' bevat.

```azurecli-interactive
az functionapp create --name <app_name> --storage-account  <storage_name>  --resource-group myResourceGroup \
--plan myAppServicePlan --deployment-source-url https://github.com/Azure-Samples/functions-quickstart-linux
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

>[!NOTE]  
>De opslagplaats met voorbeelden bevat momenteel twee scriptbestanden: [deploy.sh](https://github.com/Azure-Samples/functions-quickstart-linux/blob/master/deploy.sh) en [.deployment](https://github.com/Azure-Samples/functions-quickstart-linux/blob/master/.deployment). Via het bestand .deployment wordt aan het implementatieproces doorgegeven dat deploy.sh moet worden gebruikt als het [aangepaste implementatiescript](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script). In de huidige preview-versie moeten scripts de functie-app implementeren naar een Linux-installatiekopie.  

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

[!INCLUDE [functions-quickstart-next-steps-cli](../../includes/functions-quickstart-next-steps-cli.md)]
