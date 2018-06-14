---
title: Voorbeeld van Azure CLI-script - Een back-up maken van een web-app | Microsoft Docs
description: Voorbeeld van Azure CLI-script - Een back-up maken van een web-app
services: app-service\web
documentationcenter: ''
author: cephalin
manager: cfowler
editor: ''
tags: azure-service-management
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 12/07/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 91bf4c7fce40a8e8c94299919e0bdc25c3d3487c
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/30/2018
ms.locfileid: "30281447"
---
# <a name="back-up-a-web-app"></a>Een back-up maken van een web-app

Met dit voorbeeldscript wordt er een web-app gemaakt in App Service, inclusief de bijbehorende resources, en wordt er vervolgens een eenmalige back-up van de app gemaakt. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, hebt u Azure CLI versie 2.0 of hoger nodig. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli).

## <a name="sample-script"></a>Voorbeeldscript

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/backup-onetime/backup-onetime.sh?highlight=3-7 "Back up a web app")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [`az group create`](/cli/azure/group?view=azure-cli-latest#az-group-create) | Hiermee maakt u een resourcegroep waarin alle resources worden opgeslagen. |
| [`az storage account create`](/cli/azure/storage/account?view=azure-cli-latest#az-storage-account-create) | Hiermee maakt u een opslagaccount. |
| [`az storage container create`](/cli/azure/storage/container?view=azure-cli-latest#az-storage-container-create) | Hiermee wordt een Azure-opslagcontainer gemaakt. |
| [`az storage container generate-sas`](/cli/azure/storage/container?view=azure-cli-latest#az-storage-container-generate-sas) | Hiermee wordt een SAS-token gegenereerd voor een Azure-opslagcontainer.  |
| [`az appservice plan create`](/cli/azure/appservice/plan?view=azure-cli-latest#az-appservice-plan-create) | Hiermee maakt u een App Service-plan. |
| [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create) | Hiermee maakt u een Azure-web-app. |
| [`az webapp config backup create`](/cli/azure/webapp/config/backup?view=azure-cli-latest#az-webapp-config-backup-create) | Hiermee maakt u een back-up voor een web-app. |
| [`az webapp config backup list`](/cli/azure/webapp/config/backup?view=azure-cli-latest#az-webapp-config-backup-list) | Hiermee vraagt u een lijst met back-ups op voor een web-app. |

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure) voor meer informatie over de Azure CLI.

Meer voorbeelden van App Service CLI-scripts vindt u in de [documentatie van Azure App Service](../app-service-cli-samples.md).
