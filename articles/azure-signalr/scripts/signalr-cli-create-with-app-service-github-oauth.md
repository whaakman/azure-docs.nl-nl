---
title: Azure CLI-voorbeeldscript - Een web-app maken die gebruikmaakt van de service SignalR en GitHub-verificatie | Microsoft Docs
description: Azure CLI-voorbeeldscript - Een web-app maken die gebruikmaakt van de service SignalR en GitHub-verificatie
services: signalr
documentationcenter: signalr
author: sffamily
manager: cfowler
editor: ''
tags: azure-service-management
ms.service: signalr
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: signalr
ms.date: 04/22/2018
ms.author: zhshang
ms.custom: mvc
ms.openlocfilehash: c2bef63673c333aa30151c50667edff4c62b3e17
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46958999"
---
# <a name="create-a-web-app-that-uses-signalr-service-and-github-authentication"></a>Een web-app maken die gebruikmaakt van de service SignalR en GitHub-verificatie

Met dit voorbeeldscript maakt u een nieuwe resource voor de Azure SignalR-service, die vervolgens wordt gebruikt om realtime updates van inhoud te pushen naar clients. Met dit script worden ook een nieuwe web-app en een App Service-plan toegevoegd voor het hosten van een web-app van ASP.NET Core die de SignalR-service gebruikt. De web-app is geconfigureerd met app-instellingen voor het maken van verbinding met de nieuwe SignalR-serviceresource en voor het verifiëren met [GitHub-verificatie](https://developer.github.com/v3/guides/basics-of-authentication/). De web-app wordt ook geconfigureerd voor het gebruik van een implementatiebron van een lokale Git-opslagplaats.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit artikel gebruikmaken van Azure CLI versie 2.0 of hoger. Voer `az --version` uit om de versie te bekijken. Als u uw CLI wilt installeren of upgraden, raadpleegt u [De Azure CLI installeren]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Voorbeeldscript

Dit script maakt gebruik van de extensie *signalr* voor Azure CLI. Voer de volgende opdracht uit om de extensie *signalr* voor Azure CLI te installeren voordat u dit voorbeeldscript gaat gebruiken:

```azurecli-interactive
az extension add -n signalr
```

[!code-azurecli-interactive[main](../../../cli_scripts/azure-signalr/create-signalr-with-app-service-github-oauth/create-signalr-with-app-service-github-oauth.sh "Create a new SignalR Service and Web App configured to use SignalR, GitHub OAuth, and local Git repository deployment source.")]

Noteer de naam die wordt gegenereerd voor de nieuwe resourcegroep. Deze naam wordt weergegeven in de uitvoer. U hebt deze naam nodig als u later alle groepsresources wilt verwijderen.

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Uitleg van het script

Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht. In dit script worden de volgende opdrachten gebruikt:

| Opdracht | Opmerkingen |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Hiermee maakt u een resourcegroep waarin alle resources worden opgeslagen. |
| [az signalr create](/cli/azure/ext/signalr/signalr#ext-signalr-az-signalr-create) | Hiermee maakt u een resource voor de Azure SignalR-service. |
| [az signalr key list](/cli/azure/ext/signalr/signalr/key#ext-signalr-az-signalr-key-list) | Hiermee vraagt u de sleutels op die door uw toepassing worden gebruikt om realtime updates van inhoud te pushen met SignalR. |
| [az appservice plan create](/cli/azure/appservice/plan#az-appservice-plan-create) | Hiermee maakt u een Azure App Service-plan voor het hosten van web-apps. |
| [az webapp create](/cli/azure/webapp#az-webapp-create) | Hiermee maakt u een Azure-web-app met behulp van het App Service-hostingplan. |
| [az webapp config appsettings set](/cli/azure/webapp/config/appsettings#az-webapp-config-appsettings-set) | Hiermee voegt u nieuwe app-instellingen toe voor de web-app. Deze app-instellingen worden gebruikt voor het opslaan van de SignalR-verbindingsreeks en geheimen van GitHub OAuth-app. |
| [az webapp deployment user set](/cli/azure/webapp/deployment/user#az-webapp-deployment-user-set) | Hiermee werkt u de implementatiereferenties bij. |
| [az webapp deployment source config-local-git](/cli/azure/webapp/deployment/source#az-webapp-deployment-source-config-local-git) | Hiermee haalt u een URL op voor een eindpunt van de git-opslagplaats voor het klonen en pushen voor de implementatie van de web-app. |

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie van Azure CLI](/cli/azure) voor meer informatie over de Azure CLI.

Extra CLI-voorbeeldscripts voor de Azure SignalR-service vindt u in de [documentatie van de Azure SignalR-service](../signalr-cli-samples.md).
