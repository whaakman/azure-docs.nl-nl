---
title: Een functie in Linux maken met een aangepaste installatiekopie (preview) | Microsoft Docs
description: Informatie over het maken van Azure Functions uitgevoerd op een aangepaste installatiekopie van Linux.
services: functions
keywords: ''
author: ggailey777
ms.author: glenga
ms.date: 11/15/2017
ms.topic: tutorial
ms.service: functions
ms.custom: mvc
ms.devlang: azure-cli
manager: cfowler
ms.openlocfilehash: 4b9cb7923b177c73f83a7ac17b1022a5455c7f30
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="create-a-function-on-linux-using-a-custom-image-preview"></a>Een functie in Linux maken met een aangepaste installatiekopie (preview)

Met Azure Functions kunt u uw functies voor Linux hosten in uw eigen aangepaste container. U kunt ze ook [hosten op een standaard-Azure App Service-container](functions-create-first-azure-function-azure-cli-linux.md). Deze functionaliteit is momenteel alleen in preview en vereist [de runtime van Functions 2.0](functions-versions.md), die ook in preview is.

In deze zelfstudie leert u een functie-app te implementeren als een aangepaste Docker-installatiekopie. Dit patroon is handig wanneer u de installatiekopie van de ingebouwde App Service-container moet aanpassen. U kunt een aangepaste installatiekopie gebruiken wanneer uw functies een specifieke taalversie nodig hebben of een specifieke afhankelijkheid of configuratie vereisen die niet binnen de ingebouwde installatiekopie aanwezig is.

In deze zelfstudie leert u hoe u met Azure Functions een aangepaste installatiekopie kunt maken en deze naar Docker Hub kunt pushen. Vervolgens gebruikt u deze installatiekopie als implementatiebron voor een functie-app in Linux. U gebruikt Docker voor het maken en pushen van de installatiekopie. Met de Azure CLI kunt u een functie-app maken en de installatiekopie implementeren vanuit Docker Hub. 

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een aangepaste installatiekopie met behulp van Docker bouwen.
> * Een aangepaste installatiekopie naar een containerregister publiceren. 
> * Een Azure Storage-account maken. 
> * Een Linux App Service-plan maken. 
> * Een functie-app vanuit Docker Hub implementeren.
> * Toepassingsinstellingen toevoegen aan de functie-app. 

De volgende stappen worden ondersteund op een Mac-, Windows- of Linux-computer.  

## <a name="prerequisites"></a>Vereisten

Voor deze zelfstudie hebt u het volgende nodig:

* [Git](https://git-scm.com/downloads)
* Een actief [Azure-abonnement](https://azure.microsoft.com/pricing/free-trial/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [Docker](https://docs.docker.com/get-started/#setup)
* Een [Docker Hub-account](https://docs.docker.com/docker-id/)

[!INCLUDE [Free trial note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Het voorbeeld downloaden

Voer de volgende opdracht uit in een terminalvenster. Hiermee wordt de voorbeeld-app-opslagplaats gekloond naar uw lokale computer en navigeert u naar de map met de voorbeeldcode.

```bash
git clone https://github.com/Azure-Samples/functions-linux-custom-image.git --config core.autocrlf=input
cd functions-linux-custom-image
```

## <a name="build-the-image-from-the-docker-file"></a>De installatiekopie maken op basis van het Docker-bestand

Bekijk in deze Git-opslagplaats het _Docker-bestand_. Dit bestand beschrijft de omgeving die vereist is om de functie-app uit te voeren in Linux. 

```docker
# Base the image on the built-in Azure Functions Linux image.
FROM microsoft/azure-functions-runtime:2.0.0-jessie
ENV AzureWebJobsScriptRoot=/home/site/wwwroot

# Add files from this repo to the root site folder.
COPY . /home/site/wwwroot 
```
>[!NOTE]
> Bij het hosten van een installatiekopie in een persoonlijk containerregister moet u de verbindingsinstellingen toevoegen aan de functie-app met behulp van **ENV**-variabelen in het Docker-bestand. Omdat deze zelfstudie niet kan garanderen dat u een persoonlijk register gebruikt, worden de verbindingsinstellingen [na de implementatie toegevoegd met behulp van de Azure CLI](#configure-the-function-app) als een aanbevolen procedure voor beveiliging.   

### <a name="run-the-build-command"></a>De Build-opdracht uitvoeren
U maakt de Docker-installatiekopie door de `docker build`-opdracht uit te voeren en vervolgens een naam, `mydockerimage`, en label, `v1.0.0`, op te geven. Vervang `<docker-id>` door de ID van uw Docker Hub-account.

```bash
docker build --tag <docker-id>/mydockerimage:v1.0.0 .
```

De opdracht geeft een uitvoer die er ongeveer als volgt uitziet:

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

### <a name="test-the-image-locally"></a>De installatiekopie lokaal testen
Controleer of de installatiekopie van de build werkt door de Docker-installatiekopie uit te voeren in een lokale container. Geef de opdracht [docker run](https://docs.docker.com/engine/reference/commandline/run/) en geef de naam en het label van de installatiekopie door. Zorg ervoor dat u de poort opgeeft met behulp van het argument `-p`.

```bash
docker run -p 8080:80 -it <docker-ID>/mydockerimage:v1.0.0
```

Met de aangepaste installatiekopie die wordt uitgevoerd in een lokale Docker-container controleert u of de functie-app en container goed werken door te bladeren naar <http://localhost:8080>.

![Test de functie-app lokaal.](./media/functions-create-function-linux-custom-image/run-image-local-success.png)

Nadat u de functie-app in de container hebt geverifieerd, stopt u de uitvoering. U kunt nu de aangepaste installatiekopie naar uw Docker Hub-account pushen.

## <a name="push-the-custom-image-to-docker-hub"></a>De aangepaste installatiekopie naar Docker Hub pushen

Een register is een toepassing die als host fungeert voor installatiekopieën en biedt installatiekopie- en containerservices. Om uw installatiekopie te delen, moet u deze naar een register pushen. Docker Hub is een register voor Docker-installatiekopieën waarmee u uw eigen openbare of particuliere opslagplaatsen kunt hosten. 

Voordat u een installatiekopie kunt pushen, moet u zich aanmelden bij Docker Hub met behulp van de opdracht [docker login](https://docs.docker.com/engine/reference/commandline/login/). Vervang `<docker-id>` door uw accountnaam en typ uw wachtwoord bij de opdrachtprompt in de console. Zie [documentatie bij de opdracht voor dockeraanmelding](https://docs.docker.com/engine/reference/commandline/login/) voor andere wachtwoordopties voor Docker Hub.

```bash
docker login --username <docker-id> 
```

Een bericht 'aanmelding geslaagd' bevestigt dat u bent aangemeld. Nadat u zich hebt aangemeld, pusht u de installatiekopie naar Docker Hub met behulp van de [docker push](https://docs.docker.com/engine/reference/commandline/push/)-opdracht.

```bash
docker push <docker-id>/mydockerimage:v1.0.0
```

Controleer of de push-bewerking is voltooid door de uitvoer van de opdracht te inspecteren.

```bash
The push refers to a repository [docker.io/<docker-id>/mydockerimage:v1.0.0]
24d81eb139bf: Pushed
fd9e998161c9: Mounted from microsoft/azure-functions-runtime
e7796c35add2: Mounted from microsoft/azure-functions-runtime
ae9a05b85848: Mounted from microsoft/azure-functions-runtime
45c86e20670d: Mounted from microsoft/azure-functions-runtime
v1.0.0: digest: sha256:be080d80770df71234eb893fbe4d... size: 2422
```
U kunt nu deze installatiekopie gebruiken als implementatiebron voor een nieuwe functie-app in Azure. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit onderwerp gebruikmaken van Azure CLI versie 2.0.21 of hoger. Voer `az --version` uit om te zien welke versie u hebt. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-linux-app-service-plan"></a>Een Linux App Service-abonnement maken

Linux-hosting voor Functions wordt momenteel niet ondersteund in verbruiksabonnementen. U moet een Linux App Service-plan uitvoeren. Zie [deze vergelijking van hostingabonnementen van Azure Functions](functions-scale.md) voor meer informatie over hosting. 

[!INCLUDE [app-service-plan-no-h](../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]


## <a name="create-and-deploy-the-custom-image"></a>De aangepaste installatiekopie maken en implementeren

Een functie-app fungeert als host voor de uitvoering van uw functies. Een functie-app maken op basis van een Docker Hub-installatiekopie met behulp van de opdracht [az functionapp create](/cli/azure/functionapp#az_functionapp_create). 

Vervang in de volgende opdracht de plaatsaanduiding `<app_name>` door een unieke functie-appnaam en gebruik de naam van het opslagaccount voor `<storage_name>`. De `<app_name>` wordt gebruikt als het standaard DNS-domein voor de functie-app. Om die reden moet de naam uniek zijn in alle apps in Azure. Net als voorheen is `<docker-id>` de naam van uw Docker-account.

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

De _deployment-container-image-name_-parameter geeft aan dat de afbeelding die wordt gehost op Docker Hub wordt gebruikt om de functie-app te maken. 


## <a name="configure-the-function-app"></a>De functie-app configureren

De functie heeft de verbindingsreeks nodig om verbinding te maken met het standaardopslagaccount. Als u uw aangepaste installatiekopie naar een persoonlijk containeraccount publiceert, moet u de instellingen voor deze toepassing instellen als omgevingsvariabelen in het Docker-bestand met de [ENV-instructie](https://docs.docker.com/engine/reference/builder/#env) of een equivalent daarvan. 

In dit geval is `<storage_account>` de naam van het opslagaccount dat u hebt gemaakt. Haal de verbindingsreeks op met de opdracht [az storage account show-connection-string](/cli/azure/storage/account#show-connection-string). Voeg deze toepassingsinstellingen toe aan de functie-app met de opdracht [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings#az_functionapp_config_appsettings_set).

```azurecli-interactive
storageConnectionString=$(az storage account show-connection-string \
--resource-group myResourceGroup --name <storage_account> \
--query connectionString --output tsv)

az functionapp config appsettings set --name <function_app> \
--resource-group myResourceGroup \
--settings AzureWebJobsDashboard=$storageConnectionString \
AzureWebJobsStorage=$storageConnectionString
```

U kunt nu uw functies op Linux in Azure testen.

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een aangepaste installatiekopie met behulp van Docker bouwen.
> * Een aangepaste installatiekopie naar een containerregister publiceren. 
> * Een Azure Storage-account maken. 
> * Een Linux App Service-plan maken. 
> * Een functie-app vanuit Docker Hub implementeren.
> * Toepassingsinstellingen toevoegen aan de functie-app.

Lees meer over het lokaal ontwikkelen van Azure-functies met behulp van Azure Functions Core Tools.

> [!div class="nextstepaction"] 
> [Azure-functies lokaal programmeren en testen](functions-run-local.md)
