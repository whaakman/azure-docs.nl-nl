---
title: Voer een aangepaste installatiekopie van de Docker-Hub in Web-App voor Containers | Microsoft Docs
description: Klik hier voor meer informatie over het gebruik van een aangepaste Docker-afbeelding voor Web-App voor Containers.
keywords: Azure app service, web-app, linux, docker, container
services: app-service
documentationcenter: 
author: naziml
manager: cfowler
editor: 
ms.assetid: b97bd4e6-dff0-4976-ac20-d5c109a559a8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 09/05/2017
ms.author: wesmc
ms.custom: mvc
ms.openlocfilehash: c85f79cc14cdcecd2a05fc0ff91c4864b9fba277
ms.sourcegitcommit: 3e3a5e01a5629e017de2289a6abebbb798cec736
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/27/2017
---
# <a name="run-a-custom-docker-hub-image-in-web-app-for-containers"></a>Voer een aangepaste installatiekopie van de Docker-Hub in Web-App voor Containers

App Service biedt vooraf gedefinieerde toepassing stacks op Linux met ondersteuning voor specifieke versies, zoals PHP 7.0 en Node.js 4.5. U kunt ook een aangepaste Docker-installatiekopie gebruiken voor het implementeren van uw web-app naar de stack van een toepassing die niet al is gedefinieerd in Azure. Deze snelstartgids laat zien hoe een web-app maken en implementeren van een Python op basis van Docker-installatiekopie aan. U maakt de web-app via de [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user.md)]

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group.md)]

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux.md)]

## <a name="create-a-web-app-for-container"></a>Een Web-App voor de Container maken

Maak een [web-app](../app-service-web-overview.md) in het App Service-plan `myAppServicePlan` met de opdracht [az webapp create](/cli/azure/webapp#create). Vergeet niet om te vervangen `<app name>` met een unieke app-naam.

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app name> --deployment-container-image-name elnably/dockerimagetest
```

In de voorgaande opdracht `--deployment-container-image-name` verwijst naar de installatiekopie van het openbare Docker Hub [https://hub.docker.com/r/elnably/dockerimagetest/](https://hub.docker.com/r/elnably/dockerimagetest/). U kunt de inhoud op inspecteren [https://github.com/ahmedelnably/dockerimagetest](https://github.com/ahmedelnably/dockerimagetest).

Wanneer de web-app is gemaakt, ziet de Azure CLI u uitvoer lijkt op het volgende voorbeeld:

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

Blader naar de volgende URL in uw webbrowser.

```bash
http://<app_name>.azurewebsites.net
```

![Voorbeeld-app die wordt uitgevoerd in Azure](media/quickstart-custom-docker-image/hello-world-in-browser.png)

**Gefeliciteerd!** U hebt een aangepaste Docker-installatiekopie aan Web-App voor Containers geïmplementeerd.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een Docker Python en PostgreSQL web-app in Azure bouwen](tutorial-docker-python-postgresql-app.md)
