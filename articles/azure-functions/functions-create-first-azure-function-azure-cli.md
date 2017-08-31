---
title: Uw eerste Azure-functie maken vanuit Azure CLI | Microsoft Docs
description: Informatie over het maken van uw eerste serverloze Azure-functie met behulp van Azure CLI.
services: functions
keywords: 
author: ggailey777
ms.author: glenga
ms.assetid: 674a01a7-fd34-4775-8b69-893182742ae0
ms.date: 08/22/2017
ms.topic: hero-article
ms.service: functions
ms.custom: mvc
ms.devlang: azure-cli
manager: cfowler
ms.translationtype: HT
ms.sourcegitcommit: cf381b43b174a104e5709ff7ce27d248a0dfdbea
ms.openlocfilehash: 8bd3e4bb7423db44c48b04f25edcf1074e6ea0bd
ms.contentlocale: nl-nl
ms.lasthandoff: 08/23/2017

---

# <a name="create-your-first-function-using-the-azure-cli"></a>Uw eerste functie maken met Azure CLI

In deze Quick Start-zelfstudie wordt stapsgewijs uitgelegd hoe u Azure Functions kunt gebruiken om uw eerste functie te maken. Azure CLI gebruikt u om een functie-app te maken. Het is de serverloze infrastructuur die als host fungeert voor uw functie. De functiecode zelf wordt geïmplementeerd vanuit een voorbeeldopslagplaats in GitHub.    

U kunt de onderstaande stappen volgen op een Mac-, Windows- of Linux-computer. 

## <a name="prerequisites"></a>Vereisten 

Voordat u dit voorbeeld kunt uitvoeren moet u ervoor zorgen dat u het volgende hebt:

+ Een actief [GitHub](https://github.com)-account. 
+ Een actief Azure-abonnement.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit onderwerp gebruikmaken van Azure CLI versie 2.0 of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 


## <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een resourcegroep met de opdracht [az group create](/cli/azure/group#create). Een Azure-resourcegroep is een logische container waarin Azure-resources, zoals functie-apps, databases en opslagaccounts worden geïmplementeerd en beheerd.

In het volgende voorbeeld wordt een resourcegroep met de naam `myResourceGroup` gemaakt.  
Als u Cloud Shell niet gebruikt, moet u eerst zich aanmelden met `az login`.

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```


## <a name="create-an-azure-storage-account"></a>Een Azure Storage-account maken

Functions gebruikt een Azure Storage-account om de status van uw functies en andere informatie erover te onderhouden. Maak een opslagaccount in de resourcegroep die u hebt gemaakt met behulp van de opdracht [az storage account create](/cli/azure/storage/account#create).

Vervang in de volgende opdracht de plaatsaanduiding `<storage_name>` met uw eigen wereldwijd unieke opslagaccountnaam. Namen van opslagaccounts moeten tussen 3 en 24 tekens lang zijn en mogen alleen cijfers en kleine letters bevatten.

```azurecli-interactive
az storage account create --name <storage_name> --location westeurope --resource-group myResourceGroup --sku Standard_LRS
```

Nadat het opslagaccount is gemaakt, toont Azure CLI soortgelijke informatie als in het volgende voorbeeld:

```json
{
  "creationTime": "2017-04-15T17:14:39.320307+00:00",
  "id": "/subscriptions/bbbef702-e769-477b-9f16-bc4d3aa97387/resourceGroups/myresourcegroup/...",
  "kind": "Storage",
  "location": "westeurope",
  "name": "myfunctionappstorage",
  "primaryEndpoints": {
    "blob": "https://myfunctionappstorage.blob.core.windows.net/",
    "file": "https://myfunctionappstorage.file.core.windows.net/",
    "queue": "https://myfunctionappstorage.queue.core.windows.net/",
    "table": "https://myfunctionappstorage.table.core.windows.net/"
  },
     ....
    // Remaining output has been truncated for readability.
}
```

## <a name="create-a-function-app"></a>Een functie-app maken

U moet een functie-app hebben die als host fungeert voor de uitvoering van uw functies. De functie-app biedt een omgeving waarin uw functiecode zonder server kan worden uitgevoerd. U kunt er functies mee groeperen in een logische eenheid, zodat u resources eenvoudiger kunt beheren, implementeren en delen. Een functie-app maken met behulp van de opdracht [az functionapp create](/cli/azure/functionapp#create). 

Vervang in de volgende opdracht de plaatsaanduiding `<app_name>` en de opslagaccountnaam voor `<storage_name>` met uw eigen unieke functie-appnaam. De `<app_name>` wordt gebruikt als het standaard DNS-domein voor de functie-app. Om die reden moet de naam uniek zijn in alle apps in Azure. 

```azurecli-interactive
az functionapp create --name <app_name> --storage-account  <storage_name>  --resource-group myResourceGroup \
--consumption-plan-location westeurope
```
Standaard wordt een functie-app gemaakt met het hostingabonnement Consumption, wat betekent dat resources dynamisch worden toegevoegd wanneer ze voor uw functies zijn vereist; en u betaalt alleen wanneer functies worden uitgevoerd. Zie [Het juiste hostingabonnement kiezen](functions-scale.md) voor meer informatie. 

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

Nu u een functie-app hebt, kunt u de werkelijke functiecode vanuit de voorbeeldopslagplaats in GitHub implementeren.

## <a name="deploy-your-function-code"></a>Uw functiecode implementeren  

Er zijn verschillende manieren om uw functiecode te maken in uw nieuwe functie app. In dit onderwerp wordt verbinding gemaakt met een voorbeeldopslagplaats in GitHub. Net als voorheen moet u in de volgende code de plaatsaanduiding `<app_name>` vervangen door de naam van de functie-app die u hebt gemaakt. 

```azurecli-interactive
az functionapp deployment source config --name <app_name> --resource-group myResourceGroup --branch master \
--repo-url https://github.com/Azure-Samples/functions-quickstart \
--manual-integration 
```
Nadat de implementatiebron is ingesteld, wordt door Azure CLI informatie weergegeven die overeenkomt met het volgende voorbeeld (null-waarden zijn verwijderd vanwege de leesbaarheid):

```json
{
  "branch": "master",
  "deploymentRollbackEnabled": false,
  "id": "/subscriptions/bbbef702-e769-477b-9f16-bc4d3aa97387/resourceGroups/myResourceGroup/...",
  "isManualIntegration": true,
  "isMercurial": false,
  "location": "West Europe",
  "name": "quickstart",
  "repoUrl": "https://github.com/Azure-Samples/functions-quickstart",
  "resourceGroup": "myResourceGroup",
  "type": "Microsoft.Web/sites/sourcecontrols"
}
```

## <a name="test-the-function"></a>De functie testen

Gebruik cURL om de geïmplementeerde functie te testen op een Mac- of Linux-computer of met Bash op Windows. Voer de volgende cURL-opdracht uit, waarbij u de plaatsaanduiding `<app_name>` vervangt door de naam van de functie-app die u hebt gemaakt. Voeg de queryreeks `&name=<yourname>` toe aan de URL.

```bash
curl http://<app_name>.azurewebsites.net/api/HttpTriggerJS1?name=<yourname>
```  

![Het antwoord van de functie weergegeven in een browser.](./media/functions-create-first-azure-function-azure-cli/functions-azure-cli-function-test-curl.png)  

Als cURL niet beschikbaar is op uw opdrachtregel, moet u gewoon dezelfde URL opgeven op de adresbalk van uw webbrowser. Vervang weer de plaatsaanduiding `<app_name>` door de naam van uw functie-app, voeg de queryreeks `&name=<yourname>` toe aan de URL en voer de aanvraag uit. 

    http://<app_name>.azurewebsites.net/api/HttpTriggerJS1?name=<yourname>
   
![Het antwoord van de functie weergegeven in een browser.](./media/functions-create-first-azure-function-azure-cli/functions-azure-cli-function-test-browser.png)  

## <a name="clean-up-resources"></a>Resources opschonen

Andere Quick Starts in deze verzameling zijn op deze Quick Start gebaseerd. Als u van plan bent om door te gaan met andere Quick Starts of met de zelfstudies, verwijdert u de resources die u in deze Quick Start hebt gemaakt niet. Als u niet wilt doorgaan, gebruikt u de volgende opdracht om alle resources die via deze Quick Start zijn gemaakt, te verwijderen:

```azurecli-interactive
az group delete --name myResourceGroup
```
Typ `y` wanneer u daarom wordt gevraagd.

## <a name="next-steps"></a>Volgende stappen

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

