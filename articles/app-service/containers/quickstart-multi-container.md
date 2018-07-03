---
title: Een app met meerdere containers (preview) maken in Azure-Web App for Containers met behulp van een configuratie van Docker Compose
description: Uw eerste app met meerdere containers binnen enkele minuten implementeren in Azure Web-App for Containers
keywords: azure app service, web-app, linux, docker, compose, multicontainer, container, kubernetes
services: app-service\web
documentationcenter: ''
author: msangapu
manager: jeconnoc
editor: ''
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 06/22/2018
ms.author: msangapu
ms.custom: mvc
ms.openlocfilehash: ec5c92415668c925fe360c0c8887fd792a121842
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36753710"
---
# <a name="create-a-multicontainer-preview-app-using-web-app-for-containers"></a>Een app met meerdere containers (preview) maken in Web App for Containers

Met behulp van [Web App for Containers](app-service-linux-intro.md) kunt u op een flexibele manier Docker-installatiekopieën gebruiken. Deze snelstartgids laat zien hoe u een app met meerdere containers implementeert in Web App for Containers in [Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview) met behulp van een configuratie van Docker Compose. Volg voor Kubernetes de stappen in de [zelfstudie voor meerdere containers](tutorial-multi-container-app.md).

U gaat deze Quickstart in Cloud Shell doen, maar u kunt deze opdrachten ook lokaal uitvoeren met [Azure CLI](/cli/azure/install-azure-cli) (2.0.32 of hoger). In deze snelstartgids wordt een configuratiebestand voor Docker Compose gebruikt.

![Voorbeeld van app met meerdere containers in Web App for Containers][1]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="download-the-sample"></a>Het voorbeeld downloaden

Voor deze snelstartgids gebruikt u het samenstellingsbestand van [Docker](https://docs.docker.com/compose/wordpress/#define-the-project), maar bewerkt u dit zo dat hierin Azure Database for MySQL, permanente opslag en Redis worden opgenomen. U vindt het configuratiebestanden in [Azure-voorbeelden](https://github.com/Azure-Samples/multicontainerwordpress).

[!code-yml[Main](../../../azure-app-service-multi-container/docker-compose-wordpress.yml)]

Maak een map 'quickstart' in de Cloud Shell en ga er vervolgens naartoe.

```bash
mkdir quickstart

cd quickstart
```

Voer vervolgens de volgende opdracht uit om de voorbeeld-app-opslagplaats te klonen naar de map 'quickstart'.

```bash
git clone https://github.com/Azure-Samples/multicontainerwordpress
```

## <a name="create-a-resource-group"></a>Een resourcegroep maken

[!INCLUDE [resource group intro text](../../../includes/resource-group.md)]

Maak een resourcegroep in Cloud Shell met de opdracht [`az group create`](/cli/azure/group?view=azure-cli-latest#az_group_create). In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *US - zuid-centraal*. Als u alle ondersteunde locaties voor App Service op Linux in prijscategorie **Standard** wilt zien, voert u de opdracht [`az appservice list-locations --sku S1 --linux-workers-enabled`](/cli/azure/appservice?view=azure-cli-latest#az_appservice_list_locations) uit.

```azurecli-interactive
az group create --name myResourceGroup --location "South Central US"
```

In het algemeen maakt u een resourcegroep en resources in een regio bij u in de buurt.

Wanneer de opdracht is voltooid, laat een JSON-uitvoer u de eigenschappen van de resource-groep zien.

## <a name="create-an-azure-app-service-plan"></a>Een Azure App Service-plan maken

Maak in Cloud Shell een App Service-plan in de resourcegroep met de opdracht [`az appservice plan create`](/cli/azure/appservice/plan?view=azure-cli-latest#az_appservice_plan_create).

In het volgende voorbeeld wordt een App Service-plan gemaakt met de naam `myAppServicePlan` in de prijscategorie **Standard** (`--sku S1`) en in een Linux-container (`--is-linux`).

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku S1 --is-linux
```

Wanneer het App Service-plan is gemaakt, toont de Azure CLI soortgelijke informatie als in het volgende voorbeeld:

```json
{
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "South Central US",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/0000-0000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan",
  "kind": "linux",
  "location": "South Central US",
  "maximumNumberOfWorkers": 1,
  "name": "myAppServicePlan",
  < JSON data removed for brevity. >
  "targetWorkerSizeId": 0,
  "type": "Microsoft.Web/serverfarms",
  "workerTierName": null
}
```

## <a name="create-a-docker-compose-app"></a>Een Docker Compose-app maken

Ga in Cloud Shell naar de map `multicontainerwordpress`. Maak een [web-app](app-service-linux-intro.md) met meerdere containers in het App Service-plan `myAppServicePlan` met de opdracht [az webapp create](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create). Vergeet niet om de _\<app_naam>_ te vervangen door een unieke app-naam.

```bash
cd multicontainerwordpress

az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app_name> --multicontainer-config-type compose --multicontainer-config-file compose-wordpress.yml
```

Wanneer de web-app is gemaakt, toont de Azure CLI soortgelijke uitvoer als in het volgende voorbeeld:

```json
{
  "additionalProperties": {},
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

### <a name="browse-to-the-app"></a>Bladeren naar de app

Blader naar de geïmplementeerde app in (`http://<app_name>.azurewebsites.net`). Het laden van de app kan een paar minuten duren. Als u een foutbericht ontvangt, wacht u een paar minuten en vernieuwt u de browser.

![Voorbeeld van app met meerdere containers in Web App for Containers][1]

**Gefeliciteerd**, u hebt een app met meerdere containers gemaakt in Web App for Containers.

[!INCLUDE [Clean-up section](../../../includes/cli-script-clean-up.md)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een WordPress-app met meerdere containers maken in Web App for Containers](tutorial-multi-container-app.md)

<!--Image references-->
[1]: ./media/tutorial-multi-container-app/azure-multi-container-wordpress-install.png