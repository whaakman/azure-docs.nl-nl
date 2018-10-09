---
title: Continue implementatie vanuit een Docker-containerregister met Web App for Containers - Azure | Microsoft Docs
description: Over het instellen van continue implementatie vanuit een Docker-containerregister in Web App for Containers.
keywords: Azure appservice, linux, docker, acr, oss
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
ms.openlocfilehash: 20ca63b7126a6800538129115ff339308c11d8c5
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/08/2018
ms.locfileid: "48867017"
---
# <a name="continuous-deployment-with-web-app-for-containers"></a>Doorlopende implementatie met Web App for Containers

In deze zelfstudie configureert u continue implementatie voor een aangepaste containerinstallatiekopie van beheerde [Azure Container Registry](https://azure.microsoft.com/services/container-registry/) opslagplaatsen of [Docker Hub](https://hub.docker.com).

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij [Azure Portal](https://portal.azure.com).

## <a name="enable-the-continuous-deployment-feature"></a>De functie continue implementatie inschakelen

De functie continue implementatie inschakelen met behulp van [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) en uitvoeren van de volgende opdracht:

```azurecli-interactive
az webapp deployment container config --name name --resource-group myResourceGroup --enable-cd true
```

In de [Azure-portal](https://portal.azure.com/), selecteer de **App Service** optie aan de linkerkant van de pagina.

Selecteer de naam van de app die u wilt configureren van continue implementatie van Docker Hub.

Op de **Containerinstellingen** weergeeft, schakelt **op**, en selecteer vervolgens **opslaan** continue implementatie moet worden ingeschakeld.

![Schermopname van app-instelling](./media/app-service-webapp-service-linux-ci-cd/step2.png)

## <a name="prepare-the-webhook-url"></a>Voorbereiden van de webhook-URL

De webhook-URL ophalen met behulp van [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) en uitvoeren van de volgende opdracht:

```azurecli-interactive
az webapp deployment container show-cd-url --name sname1 --resource-group rgname
```

Noteer de webhook-URL. U hebt deze nodig in de volgende sectie.
`https://<publishingusername>:<publishingpwd>@<sitename>.scm.azurewebsites.net/docker/hook`.

U vindt uw `publishingusername` en `publishingpwd` door te downloaden van de web-app publiceren profiel met behulp van de Azure-portal.

![Schermafbeelding van de webhook 2 toevoegen](./media/app-service-webapp-service-linux-ci-cd/step3-3.png)

## <a name="add-a-webhook"></a>Een webhook toevoegen

Als u wilt toevoegen van een webhook, volg de stappen in deze richtlijnen:

- [Azure Container Registry](../../container-registry/container-registry-webhook.md) met behulp van de webhook-URL
- [Webhooks voor Docker Hub](https://docs.docker.com/docker-hub/webhooks/)

## <a name="next-steps"></a>Volgende stappen

* [Inleiding tot Azure App Service onder Linux](./app-service-linux-intro.md)
* [Azure Container Registry](https://azure.microsoft.com/services/container-registry/)
* [Een .NET Core-web-app maken in App Service in Linux](quickstart-dotnetcore.md)
* [Een Ruby-web-app maken in App Service on Linux](quickstart-ruby.md)
* [Een Docker/Go-web-app in Web App for Containers implementeren](quickstart-docker-go.md)
* [Veelgestelde vragen over Azure App Service on Linux](./app-service-linux-faq.md)
* [Web App for Containers met behulp van Azure CLI beheren](./app-service-linux-cli.md)
