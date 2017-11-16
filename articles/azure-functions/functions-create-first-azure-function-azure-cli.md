---
title: Uw eerste Azure-functie maken vanuit Azure CLI | Microsoft Docs
description: Informatie over het maken van uw eerste serverloze Azure-functie met behulp van Azure CLI.
services: functions
keywords: 
author: ggailey777
ms.author: glenga
ms.assetid: 674a01a7-fd34-4775-8b69-893182742ae0
ms.date: 11/08/2017
ms.topic: quickstart
ms.service: functions
ms.custom: mvc
ms.devlang: azure-cli
manager: cfowler
ms.openlocfilehash: 4356d00b2694224f52a9359cd4a57d3a70a34d18
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2017
---
# <a name="create-your-first-function-using-the-azure-cli"></a>Uw eerste functie maken met Azure CLI

In dit onderwerp Quick Start leert u hoe u uw eerste functie maken met Azure Functions. De Azure CLI maakt u een functie-app is de [zonder server](https://azure.microsoft.com/overview/serverless-computing/) infrastructuur die als host fungeert voor de functie. De functiecode zelf wordt geïmplementeerd vanuit een voorbeeldopslagplaats in GitHub.    

U kunt de onderstaande stappen volgen op een Mac-, Windows- of Linux-computer. 

## <a name="prerequisites"></a>Vereisten 

Voordat u dit voorbeeld kunt uitvoeren moet u ervoor zorgen dat u het volgende hebt:

+ Een actief [GitHub](https://github.com)-account. 
+ Een actief Azure-abonnement.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u wilt installeren en de CLI lokaal gebruiken, moet dit onderwerp de Azure CLI versie 2.0 of hoger. Voer `az --version` de versie die u hebt te vinden. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 


[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-function-app"></a>Een functie-app maken

U moet een functie-app hebben die als host fungeert voor de uitvoering van uw functies. De functie-app biedt een omgeving waarin uw functiecode zonder server kan worden uitgevoerd. U kunt er functies mee groeperen in een logische eenheid, zodat u resources eenvoudiger kunt beheren, implementeren en delen. Een functie-app maken met behulp van de opdracht [az functionapp create](/cli/azure/functionapp#create). 

Vervang de naam van een unieke functie-app waarin u ziet in de volgende opdracht de `<app_name>` tijdelijke aanduiding en het storage-account een naam voor `<storage_name>`. De `<app_name>` wordt gebruikt als het standaard DNS-domein voor de functie-app. Om die reden moet de naam uniek zijn in alle apps in Azure. De _implementatie-bron-url_ parameter is een voorbeeld-opslagplaats in GitHub met een functie 'Hallo wereld' HTTP geactiveerd.

```azurecli-interactive
az functionapp create --name <app_name> --storage-account  <storage_name>  --resource-group myResourceGroup \
--consumption-plan-location westeurope --deployment-source-url https://github.com/Azure-Samples/functions-quickstart
```
Instellen van de _plan-verbruik-locatie_ parameter betekent dat de functie-app wordt gehost in een verbruik hosting-indeling. In dit plan bronnen dynamisch zoals vereist door uw functies zijn toegevoegd en betaalt u alleen wanneer functies worden uitgevoerd. Zie [Het juiste hostingabonnement kiezen](functions-scale.md) voor meer informatie. 

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

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

[!INCLUDE [functions-quickstart-next-steps-cli](../../includes/functions-quickstart-next-steps-cli.md)]