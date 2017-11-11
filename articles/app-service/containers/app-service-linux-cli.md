---
title: Web-App voor Containers met Azure CLI 2.0 beheren | Microsoft Docs
description: Web-App voor Containers met Azure CLI beheren.
keywords: Azure app service, web-app, cli, linux, oss
services: app-service
documentationCenter: 
author: ahmedelnably
manager: cfowler
editor: 
ms.assetid: 
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: aelnably
ms.openlocfilehash: 54c979313a6ffa43008aa9870332b92d2b2f182a
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/10/2017
---
# <a name="manage-web-app-for-containers-using-azure-cli"></a>Web-App voor Containers met Azure CLI beheren

Met de opdrachten in dit artikel bent u maken en beheren van een Web-App voor Containers met Azure CLI 2.0.
U kunt starten met behulp van de nieuwe versie van de CLI op twee manieren:

* [Installatie van Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) op uw computer.
* Met behulp van [Azure-Cloud-Shell (Preview)](../../cloud-shell/overview.md)

## <a name="create-a-linux-app-service-plan"></a>Een Linux-App Service-abonnement maken

Voor het maken van een Linux-App Service-Plan, kunt u de volgende opdracht:

```azurecli-interactive
az appservice plan create -n appname -g rgname --is-linux -l "South Central US" --sku S1 --number-of-workers 1
```

## <a name="create-a-custom-docker-container-web-app"></a>Een aangepaste Docker-container Web-App maken

Voor het maken van een web-app en configureert voor het uitvoeren van een aangepaste Docker-container, kunt u de volgende opdracht:

```azurecli-interactive
az webapp create -n sname -g rgname -p pname -i elnably/dockerimagetest
```

## <a name="activate-the-docker-container-logging"></a>Activeer de logboekregistratie van Docker-container

De Docker-container om logboekregistratie te activeren, kunt u de volgende opdracht:

```azurecli-interactive
az webapp log config -n sname -g rgname --web-server-logging filesystem
```

## <a name="change-the-custom-docker-container-for-an-existing-web-app-for-containers-app"></a>De aangepaste Docker-container voor een bestaande Web-App voor Containers App wijzigen

Als u wilt een eerder gemaakte app van de huidige Docker-installatiekopie aan een nieuwe installatiekopie wijzigt, kunt u de volgende opdracht:

```azurecli-interactive
az webapp config container set -n sname -g rgname -c apurvajo/mariohtml5
```

## <a name="using-docker-images-from-a-private-registry"></a>Met behulp van Docker-installatiekopieën van een persoonlijke register

U kunt uw app voor het gebruik van installatiekopieën van een persoonlijke register configureren. U moet de url voor uw register, gebruikersnaam en wachtwoord opgeven. Dit kan worden gerealiseerd met de volgende opdracht:

```azurecli-interactive
az webapp config container set -n sname1 -g rgname -c <container name> -r <server url> -u <username> -p <password>
```

## <a name="enable-continuous-deployments-for-custom-docker-images"></a>Continue implementaties voor aangepaste Docker-images inschakelen

U kunt met de volgende opdracht inschakelen van de CD-functionaliteit en ophalen van de webhook-url. Deze url kan worden gebruikt voor het configureren van u DockerHub of Azure Container register repo's.

```azurecli-interactive
az webapp deployment container config -n sname -g rgname -e true
```

## <a name="create-a-web-app-for-containers-app-using-one-of-our-built-in-runtime-frameworks"></a>Maken van een Web-App voor de Containers-App met een van onze frameworks ingebouwde runtime

Voor het maken van een PHP 5.6 Web-App voor App-Containers die, kunt u de volgende opdracht.

```azurecli-interactive
az webapp create -n sname -g rgname -p pname -r "php|5.6"
```

## <a name="change-framework-version-for-an-existing-web-app-for-containers-app"></a>Framework-versie voor een bestaande Web-App voor Containers App wijzigen

Als u wilt een eerder gemaakte app van de huidige framework-versie voor Node.js 6.11 wijzigt, kunt u de volgende opdracht:

```azurecli-interactive
az webapp config set -n sname -g rgname --linux-fx-version "node|6.11"
```

## <a name="set-up-git-deployments-for-your-web-app"></a>Git-implementaties voor uw Web-App instellen

Als u Git-implementaties voor uw app instelt, kunt u de volgende opdracht:

```azurecli-interactive
az webapp deployment source config -n sname -g rgname --repo-url <gitrepo url> --branch <branch>
```

## <a name="next-steps"></a>Volgende stappen

* [Wat is Azure App Service op Linux?](app-service-linux-intro.md)
* [Installeer Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli)
* [Azure-Cloud-Shell (Preview)](../../cloud-shell/overview.md)
* [Faseringsomgevingen in Azure App Service instellen](../../app-service/web-sites-staged-publishing.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
* [Continue implementatie met Web-App voor Containers](app-service-linux-ci-cd.md)
