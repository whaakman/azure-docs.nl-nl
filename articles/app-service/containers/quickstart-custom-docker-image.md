---
title: Een aangepaste installatiekopie van Docker Hub in Azure Web App for Containers uitvoeren | Microsoft Docs
description: Een aangepaste Docker-installatiekopie uitvoeren voor Azure Web App for Containers
keywords: azure-app-service, web-app, linux, docker, container
services: app-service
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
ms.assetid: b97bd4e6-dff0-4976-ac20-d5c109a559a8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 11/02/2017
ms.author: cephalin;wesmc
ms.custom: mvc
ms.openlocfilehash: 7f6ed6d52bea1faec9dbed96a8d7aef020aea5d9
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2018
---
# <a name="run-a-custom-docker-hub-image-in-azure-web-app-for-containers"></a>Een aangepaste installatiekopie van Docker Hub in Azure Web App for Containers uitvoeren

App Service biedt vooraf gedefinieerde toepassingsstacks op Linux met ondersteuning voor specifieke versies, zoals PHP 7.0 en Node.js 4.5. U kunt een aangepaste Docker-installatiekopie ook gebruiken om uw web-app uit te voeren op een toepassingsstack die nog niet in Azure is gedefinieerd. Deze Quick Start laat zien hoe u een web-app kunt maken en hoe u de [officiële installatiekopie van Nginx Docker](https://hub.docker.com/r/_/nginx/) erop kunt implementeren. U maakt de web-app met behulp van [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).

![Voorbeeld-app die wordt uitgevoerd in Azure](media/quickstart-custom-docker-image/hello-world-in-browser.png)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user.md)]

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group.md)]

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux.md)]

## <a name="create-a-web-app-for-container"></a>Een web-app voor Containers maken

Maak in Cloud Shell een [web-app](../app-service-web-overview.md) in het `myAppServicePlan` App Service-plan met de opdracht [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create). Vergeet niet om `<app name>` te vervangen door een unieke app-naam.

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app name> --deployment-container-image-name nginx
```

In de voorgaande opdracht verwijst `--deployment-container-image-name` naar de installatiekopie van de openbare Docker Hub [https://hub.docker.com/r/_/nginx/](https://hub.docker.com/r/_/nginx/).

Wanneer de web-app is gemaakt, toont de Azure CLI soortgelijke uitvoer als in het volgende voorbeeld:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app name>.scm.azurewebsites.net/<app name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

## <a name="browse-to-the-app"></a>Bladeren naar de app

Blader in uw webbrowser naar de volgende URL.

```bash
http://<app_name>.azurewebsites.net
```

![Voorbeeld-app die wordt uitgevoerd in Azure](media/quickstart-custom-docker-image/hello-world-in-browser.png)

**Gefeliciteerd!** U hebt een aangepaste Docker-installatiekopie naar Web App for Containers geïmplementeerd.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een aangepaste Docker-installatiekopie gebruiken](tutorial-custom-docker-image.md)
