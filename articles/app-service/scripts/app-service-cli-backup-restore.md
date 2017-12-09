---
title: Azure CLI-voorbeeldscript - terugzetten vanuit een back-up van een web-app | Microsoft Docs
description: Azure CLI-voorbeeldscript - een web-app vanaf een back-up terugzetten
services: app-service\web
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
tags: azure-service-management
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 12/07/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: bec915f3e321cf2422f7cc2cad3ce1dfb6b3317a
ms.sourcegitcommit: 094061b19b0a707eace42ae47f39d7a666364d58
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="restore-a-web-app-from-a-backup"></a>Een web-app vanaf een back-up herstellen

Dit voorbeeldscript wordt een web-app in App Service gemaakt met de bijbehorende resources en vervolgens maakt een eenmalige back-up voor. 

Als u wilt dit script uitvoert, moet u een bestaande back-up voor een web-app. Als u wilt maken, Zie [back-up van een web-app](app-service-cli-backup-onetime.md) of [maken van een geplande back-up voor een web-app](app-service-cli-backup-scheduled.md).

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u wilt installeren en de CLI lokaal gebruiken, moet u Azure CLI versie 2.0 of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Voorbeeld van een script

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/backup-restore/backup-restore.sh?highlight=3-4,9 "Restore a web app from a backup")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdrachten. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [`az webapp config backup list`](/cli/azure/webapp/config/backup?view=azure-cli-latest#az_webapp_config_backup_list) | Hiermee wordt een lijst met back-ups voor een web-app. |
| [`az webapp config backup restore`](/cli/azure/webapp/config/backup?view=azure-cli-latest#az_webapp_config_backup_restore) | Hiermee zet een web-app uit een back-up. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure CLI [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure/overview).

Extra-App Service CLI scriptvoorbeelden vindt u in de [Azure App Service-documentatie](../app-service-cli-samples.md).
