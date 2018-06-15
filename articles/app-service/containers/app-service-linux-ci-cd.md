---
title: Continue implementatie van een Docker-container register met voor Containers - Azure-Web-App | Microsoft Docs
description: Het instellen van continue implementatie van een register Docker-container in Web-App voor Containers.
keywords: Azure app service, linux, docker, acr, oss
services: app-service
documentationcenter: ''
author: ahmedelnably
manager: cfowler
editor: ''
ms.assetid: a47fb43a-bbbd-4751-bdc1-cd382eae49f8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: aelnably;msangapu
ms.openlocfilehash: ac35dbd041de50ab8aae1a0fb4c00fe3917a7297
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
ms.locfileid: "30168323"
---
# <a name="continuous-deployment-with-web-app-for-containers"></a>Continue implementatie met Web-App voor Containers

In deze zelfstudie configureert u continue implementatie voor de installatiekopie van een aangepaste container van beheerde [Azure Container register](https://azure.microsoft.com/services/container-registry/) opslagplaatsen of [Docker Hub](https://hub.docker.com).

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij [Azure Portal](https://portal.azure.com).

## <a name="enable-the-continuous-deployment-feature"></a>De functie continue implementatie inschakelen

De functie continue implementatie inschakelen met behulp van [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) en uitvoeren van de volgende opdracht:

```azurecli-interactive
az webapp deployment container config --name name --resource-group myResourceGroup --enable-cd true
```

In de [Azure-portal](https://portal.azure.com/), selecteer de **App Service** optie aan de linkerkant van de pagina.

Selecteer de naam van de app waarvan u wilt configureren Docker Hub continue implementatie.

Op de **Docker-Container** pagina **op**, en selecteer vervolgens **opslaan** continue implementatie moet worden ingeschakeld.

![Schermopname van app-instelling](./media/app-service-webapp-service-linux-ci-cd/step2.png)

## <a name="prepare-the-webhook-url"></a>Voorbereiden van de webhook-URL

De webhook-URL verkrijgen via [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) en uitvoeren van de volgende opdracht:

```azurecli-interactive
az webapp deployment container show-cd-url --name sname1 --resource-group rgname
```

De webhook-URL, moet u het volgende eindpunt: `https://<publishingusername>:<publishingpwd>@<sitename>.scm.azurewebsites.net/docker/hook`.

Kunt u uw `publishingusername` en `publishingpwd` downloaden van de web-app publiceren profiel met behulp van de Azure-portal.

![Schermopname van het toevoegen van de webhook 2](./media/app-service-webapp-service-linux-ci-cd/step3-3.png)

## <a name="add-a-webhook"></a>Toevoegen van een webhook

### <a name="azure-container-registry"></a>Azure Container Registry

1. Selecteer op de portal pagina register **Webhooks**.
2. Selecteer voor het maken van een nieuwe webhook **toevoegen**. 
3. In de **webhook maken** deelvenster uw webhook een naam geven. Geef de URL die is verkregen in de voorgaande sectie voor de webhook-URI.

Zorg ervoor dat u het bereik definiëren als de opslagplaats met de installatiekopie van de container.

![Schermafbeelding van de webhook](./media/app-service-webapp-service-linux-ci-cd/step3ACRWebhook-1.png)

Wanneer u de installatiekopie bijwerkt, wordt de web-app automatisch bijgewerkt met de nieuwe installatiekopie.

### <a name="docker-hub"></a>Docker Hub

Selecteer op de pagina Docker Hub **Webhooks**, en vervolgens **maken van een WEBHOOK**.

![Schermopname van het toevoegen van de webhook 1](./media/app-service-webapp-service-linux-ci-cd/step3-1.png)

Geef voor de webhook-URL de URL die u eerder hebt verkregen.

![Schermopname van het toevoegen van de webhook 2](./media/app-service-webapp-service-linux-ci-cd/step3-2.png)

Wanneer u de installatiekopie bijwerkt, wordt de web-app automatisch bijgewerkt met de nieuwe installatiekopie.

## <a name="next-steps"></a>Volgende stappen

* [Inleiding tot op Linux-Azure App Service](./app-service-linux-intro.md)
* [Azure Container Registry](https://azure.microsoft.com/services/container-registry/)
* [Een .NET Core-web-app maken in App Service in Linux](quickstart-dotnetcore.md)
* [Een Ruby web-app maken in App-Service op Linux](quickstart-ruby.md)
* [Een Docker/Ga web-app in Web-App voor Containers implementeren](quickstart-docker-go.md)
* [Veelgestelde vragen over Azure App Service on Linux](./app-service-linux-faq.md)
* [Web-App voor Containers met Azure CLI beheren](./app-service-linux-cli.md)
