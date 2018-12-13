---
title: Doorlopende implementatie met Web App for Containers - Azure App Service | Microsoft Docs
description: Over het instellen van continue implementatie in Web App for Containers.
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
ms.date: 11/08/2018
ms.author: yili
ms.custom: seodec18
ms.openlocfilehash: 4acadc4c08ef50e7d52303689c38c43f81187669
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53315522"
---
# <a name="continuous-deployment-with-web-app-for-containers"></a>Doorlopende implementatie met Web App for Containers

In deze zelfstudie configureert u continue implementatie voor een aangepaste containerinstallatiekopie van beheerde [Azure Container Registry](https://azure.microsoft.com/services/container-registry/) opslagplaatsen of [Docker Hub](https://hub.docker.com).

## <a name="enable-continuous-deployment-with-acr"></a>Doorlopende implementatie met ACR inschakelen

![Schermafbeelding van de ACR-webhook](./media/app-service-webapp-service-linux-ci-cd/ci-cd-acr-02.png)

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer de **App Service** optie aan de linkerkant van de pagina.
3. Selecteer de naam van de app waarvan u wilt om continue implementatie te configureren.
4. Op de **Containerinstellingen** weergeeft, schakelt **enkele Container**
5. Selecteer **Azure Container Registry**
6. Selecteer **continue implementatie > op**
7. Selecteer **opslaan** continue implementatie moet worden ingeschakeld.

## <a name="use-the-acr-webhook"></a>Gebruik de ACR-webhook

Als u continue implementatie is ingeschakeld, kunt u de zojuist gemaakte webhook bekijken op de pagina van uw Azure Container Registry-webhooks.

![Schermafbeelding van de ACR-webhook](./media/app-service-webapp-service-linux-ci-cd/ci-cd-acr-03.png)

Klik in het Containerregister op Webhooks voor het weergeven van de huidige webhooks.

## <a name="enable-continuous-deployment-with-docker-hub-optional"></a>Inschakelen van continue implementatie met Docker Hub (optioneel)

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer de **App Service** optie aan de linkerkant van de pagina.
3. Selecteer de naam van de app waarvan u wilt om continue implementatie te configureren.
4. Op de **Containerinstellingen** weergeeft, schakelt **enkele Container**
5. Selecteer **Docker Hub**
6. Selecteer **continue implementatie > op**
7. Selecteer **opslaan** continue implementatie moet worden ingeschakeld.

![Schermopname van app-instelling](./media/app-service-webapp-service-linux-ci-cd/ci-cd-docker-02.png)

Kopieer de Webhook-URL. Als u wilt toevoegen een webhook voor Docker Hub, gaat u als volgt <a href="https://docs.docker.com/docker-hub/webhooks/" target="_blank">webhooks voor Docker Hub</a>.

## <a name="next-steps"></a>Volgende stappen

* [Inleiding tot Azure App Service onder Linux](./app-service-linux-intro.md)
* [Azure Container Registry](https://azure.microsoft.com/services/container-registry/)
* [Een .NET Core-web-app maken in App Service in Linux](quickstart-dotnetcore.md)
* [Een Ruby-web-app maken in App Service on Linux](quickstart-ruby.md)
* [Een Docker/Go-web-app in Web App for Containers implementeren](quickstart-docker-go.md)
* [Veelgestelde vragen over Azure App Service on Linux](./app-service-linux-faq.md)
* [Web App for Containers met behulp van Azure CLI beheren](./app-service-linux-cli.md)
