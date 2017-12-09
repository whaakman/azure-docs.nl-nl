---
title: 'Azure CLI-Script voorbeeld: een WebApp verbinden met Cosmos DB | Microsoft Docs'
description: 'Azure CLI-Script voorbeeld: een WebApp verbinden met Cosmos-DB'
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: bbbdbc42-efb5-4b4f-8ba6-c03c9d16a7ea
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 06/19/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: a59e0512308a85248a10a24c5951984040bedbcc
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="connect-a-web-app-to-cosmos-db"></a>Een WebApp verbinden met Cosmos-DB

In dit scenario leert u hoe u een Azure DB die Cosmos-account en een Azure-web-app maakt. Vervolgens koppelt u de Cosmos-database aan de web-app met behulp van appinstellingen.


[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit onderwerp gebruikmaken van Azure CLI versie 2.0 of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Voorbeeld van een script

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/connect-to-documentdb/connect-to-documentdb.sh "Azure Cosmos DB")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Script uitleg

Dit script gebruikt de volgende opdrachten voor het maken van een resourcegroep of web-app Cosmos DB en alle gerelateerde resources. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [AZ groep maken](https://docs.microsoft.com/cli/azure/group#az_group_create) | Maakt een resourcegroep waarin alle resources worden opgeslagen. |
| [AZ appservice-abonnement maken](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) | Hiermee maakt u een App Service-abonnement. Dit is vergelijkbaar met een serverfarm voor uw Azure-web-app. |
| [AZ webapp maken](https://docs.microsoft.com/cli/azure/webapp#az_webapp_create) | Hiermee maakt u een Azure-web-app. |
| [AZ cosmosdb maken](https://docs.microsoft.com/cli/azure/cosmosdb#az_cosmosdb_create) | Maakt een Cosmos-DB-account. Dit is waar de gegevens worden opgeslagen. |
| [AZ cosmosdb lijst-sleutels](https://docs.microsoft.com/cli/azure/cosmosdb#az_cosmosdb_list_keys) | Geeft een lijst van de toegangssleutels voor het opgegeven Cosmos-DB-account. |
| [AZ webapp config appsettings instellen](https://docs.microsoft.com/cli/azure/webapp/config/appsettings#az_webapp_config_appsettings_set) | Maken of bijwerken van een app-instelling voor een Azure-web-app. App-instellingen worden weergegeven als omgevingsvariabelen voor uw app. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure CLI [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure/overview).

Extra-App Service CLI scriptvoorbeelden vindt u in de [Azure App Service-documentatie](../app-service-cli-samples.md).
