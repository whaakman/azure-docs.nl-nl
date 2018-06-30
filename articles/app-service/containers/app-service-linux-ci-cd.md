---
title: Continue implementatie van een Docker-container register met voor Containers - Azure-Web-App | Microsoft Docs
description: Het instellen van continue implementatie van een register Docker-container in Web-App voor Containers.
keywords: Azure app service, linux, docker, acr, oss
services: app-service
documentationcenter: ''
author: msangapu
manager: jeconnoc
editor: ''
ms.assetid: a47fb43a-bbbd-4751-bdc1-cd382eae49f8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2018
ms.author: msangapu
ms.openlocfilehash: 0f2d4626308eed376b71f1b3df2f9e43f1b2a4f7
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37130958"
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

Noteer de webhook-URL. U moet deze in de volgende sectie.
`https://<publishingusername>:<publishingpwd>@<sitename>.scm.azurewebsites.net/docker/hook`.

Kunt u uw `publishingusername` en `publishingpwd` downloaden van de web-app publiceren profiel met behulp van de Azure-portal.

![Schermopname van het toevoegen van de webhook 2](./media/app-service-webapp-service-linux-ci-cd/step3-3.png)

## <a name="add-a-webhook"></a>Toevoegen van een webhook

Als u wilt toevoegen van een webhook, volg de stappen in deze richtlijnen:

- [Azure Container register](../../container-registry/container-registry-webhook.md) met behulp van de webhook-URL
- [Webhooks voor Docker-Hub](https://docs.docker.com/docker-hub/webhooks/)

## <a name="next-steps"></a>Volgende stappen

* [Inleiding tot op Linux-Azure App Service](./app-service-linux-intro.md)
* [Azure-Container register](https://azure.microsoft.com/services/container-registry/)
* [Een .NET Core-web-app maken in App Service in Linux](quickstart-dotnetcore.md)
* [Een Ruby web-app maken in App-Service op Linux](quickstart-ruby.md)
* [Een Docker/Ga web-app in Web-App voor Containers implementeren](quickstart-docker-go.md)
* [Veelgestelde vragen over Azure App Service on Linux](./app-service-linux-faq.md)
* [Web-App voor Containers met Azure CLI beheren](./app-service-linux-cli.md)
