---
title: Maken van een functie in Linux met een aangepaste installatiekopie (preview) | Microsoft Docs
description: Informatie over het maken van Azure Functions uitgevoerd op een aangepaste installatiekopie van Linux.
services: functions
keywords: 
author: ggailey777
ms.author: glenga
ms.date: 11/15/2017
ms.topic: tutorial
ms.service: functions
ms.custom: mvc
ms.devlang: azure-cli
manager: cfowler
ms.openlocfilehash: 9ba5f45034561f8d897676e8cc4b1a59945403b8
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
# <a name="create-a-function-on-linux-using-a-custom-image-preview"></a>Maken van een functie in Linux met een aangepaste installatiekopie (preview)

Azure Functions, kunt u uw functies op Linux in uw eigen aangepaste container hosten. Deze functionaliteit is momenteel in preview. U kunt ook [host op een standaard-Azure App Service-container](functions-create-first-azure-function-azure-cli-linux.md).  

In deze zelfstudie leert u het implementeren van een functie-app als een aangepaste Docker-installatiekopie. Dit patroon is handig als u wilt aanpassen van de installatiekopie van het ingebouwde App Service-container. U wilt gebruiken een aangepaste installatiekopie wanneer uw functies de versie van een specifieke taal moeten of een specifieke afhankelijkheid of de configuratie die nog niet binnen de installatiekopie van het ingebouwde vereisen.

Deze zelfstudie leert u hoe u met Azure Functions maken en toepassen van een aangepaste installatiekopie met Docker-Hub. Vervolgens gebruikt u deze installatiekopie als de implementatiebron voor een functie-app die wordt uitgevoerd op Linux. U kunt Docker bouwen en de installatiekopie van het push. Met de Azure CLI kunt u een functie-app maken en implementeren van een installatiekopie van Docker-Hub. 

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een aangepaste installatiekopie met behulp van Docker bouwen.
> * Een aangepaste installatiekopie naar een container register publiceren. 
> * Maak een Azure Storage-account. 
> * Maak een Linux-App Service-abonnement. 
> * Implementeer een functie-app van Docker-Hub.
> * Toepassingsinstellingen toevoegen aan de functie-app. 

De volgende stappen uit worden op een Mac-, Windows- of Linux-computer ondersteund.  

## <a name="prerequisites"></a>Vereisten

Voor deze zelfstudie hebt u het volgende nodig:

* [Git](https://git-scm.com/downloads)
* Een actieve [Azure-abonnement](https://azure.microsoft.com/pricing/free-trial/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [Docker](https://docs.docker.com/get-started/#setup)
* Een [Docker-Hub-account](https://docs.docker.com/docker-id/)

[!INCLUDE [Free trial note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Het voorbeeld downloaden

Voer de volgende opdracht om een kloon de opslagplaats van de voorbeeld-app op uw lokale computer en wijzigt u vervolgens naar de map waarin zich de voorbeeldcode in een terminalvenster.

```bash
git clone https://github.com/Azure-Samples/functions-linux-custom-image.git --config core.autocrlf=input
cd functions-linux-custom-image
```

## <a name="build-the-image-from-the-docker-file"></a>De installatiekopie maken van de Docker-bestand

In deze Git-opslagplaats te kijken hoe de _Dockerfile_. Dit bestand bevat een beschrijving van de omgeving die nodig is voor het uitvoeren van de functie-app op Linux. 

```docker
# Base the image on the built-in Azure Functions Linux image.
FROM microsoft/azure-functions-runtime:2.0.0-jessie
ENV AzureWebJobsScriptRoot=/home/site/wwwroot

# Add files from this repo to the root site folder.
COPY . /home/site/wwwroot 
```
>[!NOTE]
> Bij het hosten van een installatiekopie in een register privé-container, moet u de verbindingsinstellingen toevoegen aan de functie-app met behulp van **ENV** variabelen in de Dockerfile. Omdat deze zelfstudie kan niet worden gegarandeerd dat u een persoonlijke register, de verbindingsinstellingen zijn [toegevoegd na de implementatie met behulp van de Azure CLI](#configure-the-function-app) als een best practice voor beveiliging.   

### <a name="run-the-build-command"></a>Voer de opdracht Build
De Docker-installatiekopie bouwen, voer de `docker build` opdracht in en geef een naam `mydockerimage`, en de tag `v1.0.0`. Vervang `<docker-id>` rekening met uw Hub Docker ID.

```bash
docker build --tag <docker-id>/mydockerimage:v1.0.0 .
```

De opdracht geeft de uitvoer ziet er als volgt:

```bash
Sending build context to Docker daemon  169.5kB
Step 1/3 : FROM microsoft/azure-functions-runtime:v2.0.0-jessie
v2.0.0-jessie: Pulling from microsoft/azure-functions-runtime
b178b12f7913: Pull complete
2d9ce077a781: Pull complete
4775d4ba55c8: Pull complete
Digest: sha256:073f45fc167b3b5c6642ef4b3c99064430d6b17507095...
Status: Downloaded newer image for microsoft/azure-functions-runtime:v2.0.0-jessie
 ---> 217799efa500
Step 2/3 : ENV AzureWebJobsScriptRoot /home/site/wwwroot
 ---> Running in 528fa2077d17
 ---> 7cc6323b8ae0
Removing intermediate container 528fa2077d17
Step 3/3 : COPY . /home/site/wwwroot
 ---> 5bdac9878423
Successfully built 5bdac9878423
Successfully tagged ggailey777/mydockerimage:v1.0.0
```

### <a name="test-the-image-locally"></a>De installatiekopie van het lokaal testen
Controleer of de installatiekopie van het ingebouwde werkt door de Docker-installatiekopie in een lokale container. Probleem de [docker uitvoeren](https://docs.docker.com/engine/reference/commandline/run/) opdracht en de naam en het label van de afbeelding doorgeven. Zorg ervoor dat u de poort met behulp van de `-p` argument.

```bash
docker run -p 8080:80 -it <docker-ID>/mydockerimage:v1.0.0
```

Met de aangepaste installatiekopie uitgevoerd in een lokale Docker-container, Controleer of de functie-app en de container goed werken door te bladeren naar <http://localhost: 8080>.

![De functie-app lokaal testen.](./media/functions-create-function-linux-custom-image/run-image-local-success.png)

Nadat u hebt geverifieerd hav de functie-app in de container, stopt u de uitvoering. U kunt nu de aangepaste installatiekopie pushen naar uw account Docker-Hub.

## <a name="push-the-custom-image-to-docker-hub"></a>De aangepaste installatiekopie push naar Docker-Hub

Een register is een toepassing die als host fungeert voor installatiekopieën en biedt services voor services en de container. Om uw installatiekopie delen, moet u dit doorgeven aan een register. Docker-Hub is een register voor de Docker-installatiekopieën waarmee u uw eigen opslagplaatsen, openbare of particuliere hosten. 

Voordat u een installatiekopie van een pushen kunt, moet u zich aanmeldt bij Docker-Hub met behulp van de [docker aanmelding](https://docs.docker.com/engine/reference/commandline/login/) opdracht. Vervang `<docker-id>` met uw accountnaam en typt u uw wachtwoord in de console bij de opdrachtprompt. Zie voor andere wachtwoordopties Docker-Hub het [docker aanmelding opdracht documentatie](https://docs.docker.com/engine/reference/commandline/login/).

```bash
docker login --username <docker-id> 
```

Een bericht 'aanmelding geslaagd' wordt bevestigd dat u bent aangemeld. Nadat u zich hebt aangemeld, u de installatiekopie van het Docker-hub pushen met behulp van de [docker push](https://docs.docker.com/engine/reference/commandline/push/) opdracht.

```bash
docker push <docker-id>/mydockerimage:v1.0.0 .
```

Controleer of dat de push-bewerking is voltooid door in de opdracht-uitvoer.

```bash
The push refers to a repository [docker.io/<docker-id>/mydockerimage:v1.0.0]
24d81eb139bf: Pushed
fd9e998161c9: Mounted from microsoft/azure-functions-runtime
e7796c35add2: Mounted from microsoft/azure-functions-runtime
ae9a05b85848: Mounted from microsoft/azure-functions-runtime
45c86e20670d: Mounted from microsoft/azure-functions-runtime
v1.0.0: digest: sha256:be080d80770df71234eb893fbe4d... size: 2422
```
Nu kunt u deze installatiekopie als de implementatiebron voor een nieuwe functie-app in Azure. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u wilt installeren en gebruiken van de CLI lokaal, in dit onderwerp is vereist voor de Azure CLI versie 2.0.21 of hoger. Voer `az --version` de versie die u hebt te vinden. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-linux-app-service-plan"></a>Een Linux-App Service-abonnement maken

Linux die als host fungeert voor functies wordt momenteel niet ondersteund in verbruik plannen. U moet uitvoeren op een Linux-App Service-abonnement. Zie voor meer informatie over het hosten van [vergelijking van Azure Functions hosting plannen](functions-scale.md). 

[!INCLUDE [app-service-plan-no-h](../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]


## <a name="create-and-deploy-the-custom-image"></a>Maken en implementeren van de aangepaste installatiekopie

De functie-app als host fungeert voor de uitvoering van uw functies. Een functie-app van een installatiekopie van een Docker-Hub te maken met behulp van de [az functionapp maken](/cli/azure/functionapp#create) opdracht. 

Vervang de naam van een unieke functie-app waarin u ziet in de volgende opdracht de `<app_name>` tijdelijke aanduiding en het storage-account een naam voor `<storage_name>`. De `<app_name>` wordt gebruikt als het standaard DNS-domein voor de functie-app. Om die reden moet de naam uniek zijn in alle apps in Azure. Als voorheen `<docker-id>` is de naam van uw Docker-account.

```azurecli-interactive
az functionapp create --name <app_name> --storage-account  <storage_name>  --resource-group myResourceGroup \
--plan myAppServicePlan --deployment-container-image-name <docker-id>/mydockerimage:v1.0.0 
```
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

De _implementatie-container-installatiekopie-name_ parameter geeft aan dat de afbeelding die wordt gehost op Docker-Hub gebruiken om te maken van de functie-app. 


## <a name="configure-the-function-app"></a>De functie-app configureren

De functie moet de verbindingsreeks verbinding maken met het standaardopslagaccount. Als u uw aangepaste installatiekopie naar een privé-container-account publiceert, moet u de instellingen voor deze toepassing in plaats daarvan instellen als omgevingsvariabelen in het Dockerfile met de [ENV instructie](https://docs.docker.com/engine/reference/builder/#env), of gelijkwaardig. 

In dit geval `<storage_account>` is de naam van het opslagaccount dat u hebt gemaakt. Ophalen van de verbindingsreeks met het [az storage-account weergeven verbindingsreeks](/cli/azure/storage/account#show-connection-string) opdracht. Deze toepassingsinstellingen toevoegen in de functie-app met de [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings#set) opdracht.

```azurecli-interactive
storageConnectionString=$(az storage account show-connection-string \
--resource-group myResourceGroup --name <storage_account> \
--query connectionString --output tsv)

az functionapp config appsettings set --name <function_app> \
--resource-group myResourceGroup \
--settings AzureWebJobsDashboard=$storageConnectionString \
AzureWebJobsStorage=$storageConnectionString
```

U kunt nu uw functies met op Linux in Azure testen.

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een aangepaste installatiekopie met behulp van Docker bouwen.
> * Een aangepaste installatiekopie naar een container register publiceren. 
> * Maak een Azure Storage-account. 
> * Maak een Linux-App Service-abonnement. 
> * Implementeer een functie-app van Docker-Hub.
> * Toepassingsinstellingen toevoegen aan de functie-app.

Meer informatie over het ontwikkelen van Azure Functions lokaal via de hulpprogramma's van Azure Functions Core.

> [!div class="nextstepaction"] 
> [Code en Azure Functions lokaal testen](functions-run-local.md)
