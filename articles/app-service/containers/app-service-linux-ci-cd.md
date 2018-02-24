---
title: Continue implementatie van een register Docker-container met voor Containers - Azure-Web-App | Microsoft Docs
description: Het instellen van continue implementatie van een register Docker-container in Web-App voor Containers.
keywords: Azure app service, linux, docker, acr, oss
services: app-service
documentationcenter: 
author: ahmedelnably
manager: cfowler
editor: 
ms.assetid: a47fb43a-bbbd-4751-bdc1-cd382eae49f8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: aelnably;wesmc
ms.openlocfilehash: 84bc344438d5542cc6409fa50aa6e55eb547c6a8
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2018
---
# <a name="continuous-deployment-with-web-app-for-containers"></a>Continue implementatie met Web-App voor Containers

In deze zelfstudie configureert u continue implementatie voor de installatiekopie van een aangepaste container van beheerde [Azure Container register](https://azure.microsoft.com/services/container-registry/) opslagplaatsen of [Docker Hub](https://hub.docker.com).

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Aanmelden bij de [Azure-portal](https://portal.azure.com)

## <a name="enable-container-continuous-deployment-feature"></a>Container voor continue implementatie inschakelen

Kunt u het gebruik van continue implementatie functie [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) en de volgende opdracht wordt uitgevoerd

```azurecli-interactive
az webapp deployment container config -n sname -g rgname -e true
```

In de  **[Azure-portal](https://portal.azure.com/)**, klikt u op de **App Service** optie aan de linkerkant van de pagina.

Klik op de naam van uw app, die u wilt configureren, continue implementatie voor Docker-Hub.

In de **appinstellingen**, een instelling app toevoegen `DOCKER_ENABLE_CI` met de waarde `true`.

![afbeelding van app-instelling worden ingevoegd](./media/app-service-webapp-service-linux-ci-cd/step2.png)

## <a name="prepare-webhook-url"></a>Voorbereiden van de Webhook-URL

U kunt ook het gebruik van de Webhook-URL verkrijgen [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) en de volgende opdracht wordt uitgevoerd

```azurecli-interactive
az webapp deployment container show-cd-url -n sname1 -g rgname
```

Voor de Webhook-URL moet u het volgende eindpunt: `https://<publishingusername>:<publishingpwd>@<sitename>.scm.azurewebsites.net/docker/hook`.

Kunt u uw `publishingusername` en `publishingpwd` downloaden van de web-app publiceren profiel met behulp van de Azure-portal.

![afbeelding van het toevoegen van de webhook 2 invoegen](./media/app-service-webapp-service-linux-ci-cd/step3-3.png)

## <a name="add-a-web-hook"></a>Een haakje web toevoegen

### <a name="azure-container-registry"></a>Azure Container Registry

Klik op de portalblade van register **Webhooks**, maak een nieuwe webhook door te klikken **toevoegen**. In de **webhook maken** blade een naam voor uw webhook geven. Voor de Webhook-URI moet u de URL die is verkregen van **stap 3**

Zorg ervoor dat u het bereik definiëren als de opslagplaats met de installatiekopie van de container.

![afbeelding van de webhook invoegen](./media/app-service-webapp-service-linux-ci-cd/step3ACRWebhook-1.png)

Wanneer de installatiekopie wordt bijgewerkt, krijgen de web-app automatisch bijgewerkt met de nieuwe installatiekopie.

### <a name="docker-hub"></a>Docker Hub

Klik op de pagina Docker Hub **Webhooks**, vervolgens **maken van een WEBHOOK**.

![afbeelding van het toevoegen van de webhook 1 invoegen](./media/app-service-webapp-service-linux-ci-cd/step3-1.png)

Voor de Webhook-URL die u wilt en geef de URL die is verkregen van **stap 3**

![afbeelding van het toevoegen van de webhook 2 invoegen](./media/app-service-webapp-service-linux-ci-cd/step3-2.png)

Wanneer de installatiekopie wordt bijgewerkt, krijgen de web-app automatisch bijgewerkt met de nieuwe installatiekopie.

## <a name="next-steps"></a>Volgende stappen

* [Wat is Azure App Service op Linux?](./app-service-linux-intro.md)
* [Azure Container Registry](https://azure.microsoft.com/services/container-registry/)
* [.NET Core gebruiken in Azure App Service onder Linux](quickstart-dotnetcore.md)
* [Ruby gebruiken in Azure App Service onder Linux](quickstart-ruby.md)
* [Een aangepaste Docker-installatiekopie uitvoeren voor Web App for Containers](quickstart-docker-go.md)
* [Web App for Containers van Azure App Service: veelgestelde vragen](./app-service-linux-faq.md)
* [Web-App voor Containers met Azure CLI 2.0 beheren](./app-service-linux-cli.md)