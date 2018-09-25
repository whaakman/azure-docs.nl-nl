---
title: Web App for Containers met behulp van Azure CLI beheren | Microsoft Docs
description: Web App for Containers met behulp van Azure CLI beheren.
keywords: Azure appservice, web-app, cli, linux, oss
services: app-service
documentationCenter: ''
author: ahmedelnably
manager: cfowler
editor: ''
ms.assetid: ''
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: aelnably
ms.openlocfilehash: 024ca5bcf9c1f82f07656691d6ef5358ca3d5111
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46998488"
---
# <a name="manage-web-app-for-containers-using-azure-cli"></a>Web App for Containers met behulp van Azure CLI beheren

Met behulp van de opdrachten in dit artikel kunt maken en beheren van een Web-App voor Containers met de Azure CLI.
U kunt starten met de nieuwe versie van de CLI op twee manieren:

* [Azure CLI installeren](https://docs.microsoft.com/cli/azure/install-azure-cli) op uw computer.
* Met behulp van [Azure Cloudshell (Preview)](../../cloud-shell/overview.md)

## <a name="create-a-linux-app-service-plan"></a>Een Linux App Service-Plan maken

Voor het maken van een Linux App Service-Plan, kunt u de volgende opdracht uit:

```azurecli-interactive
az appservice plan create -n appname -g rgname --is-linux -l "South Central US" --sku S1 --number-of-workers 1
```

## <a name="create-a-custom-docker-container-web-app"></a>Een aangepaste Docker-container Web-App maken

Voor het maken van een web-app en te configureren voor het uitvoeren van een aangepaste Docker-container, kunt u de volgende opdracht uit:

```azurecli-interactive
az webapp create -n sname -g rgname -p pname -i elnably/dockerimagetest
```

## <a name="activate-the-docker-container-logging"></a>Activeer de logboekregistratie voor Docker-container

Activeer de logboekregistratie voor Docker-container, kunt u de volgende opdracht uit:

```azurecli-interactive
az webapp log config -n sname -g rgname --web-server-logging filesystem
```

## <a name="change-the-custom-docker-container-for-an-existing-web-app-for-containers-app"></a>De aangepaste Docker-container voor een bestaande Web-App voor Containers App wijzigen

Als u wilt een eerder gemaakte app uit de huidige Docker-installatiekopie naar een nieuwe afbeelding wijzigen, kunt u de volgende opdracht uit:

```azurecli-interactive
az webapp config container set -n sname -g rgname -c apurvajo/mariohtml5
```

## <a name="using-docker-images-from-a-private-registry"></a>Met behulp van Docker-installatiekopieën uit een persoonlijk register

U kunt uw app voor het gebruik van afbeeldingen uit een persoonlijk register configureren. U moet de url voor uw register, gebruikersnaam en wachtwoord opgeven. Dit kan worden bereikt met behulp van de volgende opdracht uit:

```azurecli-interactive
az webapp config container set -n sname1 -g rgname -c <container name> -r <server url> -u <username> -p <password>
```

## <a name="enable-continuous-deployments-for-custom-docker-images"></a>Inschakelen van continue implementaties voor aangepaste Docker-installatiekopieën

U kunt met de volgende opdracht inschakelen van de CD-functionaliteit en ophalen van de webhook-url. Deze url kan worden gebruikt om te configureren dat u DockerHub of Azure Container Registry-opslagplaatsen.

```azurecli-interactive
az webapp deployment container config -n sname -g rgname -e true
```

## <a name="create-a-web-app-for-containers-app-using-one-of-our-built-in-runtime-frameworks"></a>Maak een Web-App voor Containers-App met behulp van een van onze ingebouwde runtime-frameworks

Voor het maken van een PHP-5.6-Web-App voor Containers-App die, kunt u de volgende opdracht uit.

```azurecli-interactive
az webapp create -n sname -g rgname -p pname -r "php|5.6"
```

## <a name="change-framework-version-for-an-existing-web-app-for-containers-app"></a>Framework-versie voor een bestaande Web-App voor Containers App wijzigen

Als u wilt een eerder gemaakte app wijzigen van de huidige framework-versie naar Node.js 6.11, kunt u de volgende opdracht uit:

```azurecli-interactive
az webapp config set -n sname -g rgname --linux-fx-version "node|6.11"
```

## <a name="set-up-git-deployments-for-your-web-app"></a>Git-implementaties voor uw Web-App instellen

Als u Git-implementaties voor uw app instelt, kunt u de volgende opdracht uit:

```azurecli-interactive
az webapp deployment source config -n sname -g rgname --repo-url <gitrepo url> --branch <branch>
```

## <a name="next-steps"></a>Volgende stappen

* [Wat is Azure App Service on Linux?](app-service-linux-intro.md)
* [Azure CLI installeren](https://docs.microsoft.com/cli/azure/install-azure-cli)
* [Azure Cloudshell (Preview)](../../cloud-shell/overview.md)
* [Faseringsomgevingen in Azure App Service instellen](../../app-service/web-sites-staged-publishing.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
* [Doorlopende implementatie met WebApp for Containers](app-service-linux-ci-cd.md)
