---
title: 'Azure CLI-Script voorbeeld: een WebApp verbinden met een redis-cache | Microsoft Docs'
description: 'Azure CLI-Script voorbeeld: een WebApp verbinden met een redis-cache'
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: bc8345b2-8487-40c6-a91f-77414e8688e6
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 08/30/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: b8ae453ca73ae69c34ff785dc619433035257a6a
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="connect-a-web-app-to-a-redis-cache"></a>Een WebApp verbinden met een redis-cache

In dit scenario leert u hoe u een Azure redis-cache en een Azure-web-app maakt. Vervolgens koppelt u de redis-cache aan de web-app met behulp van appinstellingen.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeld van een script

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/connect-to-redis/connect-to-redis.sh "Azure Redis Cache")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdrachten voor het maken van een resourcegroep of web-app, redis-cache en alle bijbehorende resources. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [AZ groep maken](https://docs.microsoft.com/cli/azure/group#az_group_create) | Maakt een resourcegroep waarin alle resources worden opgeslagen. |
| [AZ appservice-abonnement maken](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) | Hiermee maakt u een App Service-abonnement. Dit is vergelijkbaar met een serverfarm voor uw Azure-web-app. |
| [AZ webapp maken](https://docs.microsoft.com/cli/azure/webapp#az_webapp_create) | Hiermee maakt u een Azure-web-app. |
| [AZ redis maken](https://docs.microsoft.com/cli/azure/redis#az_redis_create) | Nieuwe Redis-Cache-exemplaar maken. Dit is waar de gegevens worden opgeslagen. |
| [AZ redis lijst-sleutels](https://docs.microsoft.com/cli/azure/redis#az_redis_list_keys) | Geeft een lijst van de toegangssleutels voor het redis-cache-exemplaar. |
| [AZ webapp config appsettings instellen](https://docs.microsoft.com/cli/azure/webapp/config/appsettings#az_webapp_config_appsettings_set) | Maken of bijwerken van een app-instelling voor een Azure-web-app. App-instellingen worden weergegeven als omgevingsvariabelen voor uw app. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure CLI [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure/overview).

Extra-App Service CLI scriptvoorbeelden vindt u in de [Azure App Service-documentatie](../app-service-cli-samples.md).
