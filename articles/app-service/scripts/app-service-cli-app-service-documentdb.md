---
title: Voorbeeld van Azure CLI-script - Een web-app verbinden met MongoDB (Cosmos DB) | Microsoft Docs
description: Voorbeeld van Azure CLI-script - Een web-app verbinden met MongoDB (Cosmos DB)
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: ''
tags: azure-service-management
ms.assetid: bbbdbc42-efb5-4b4f-8ba6-c03c9d16a7ea
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 12/11/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: f2b486e32cef87e3f06727f46c9741b4db546d18
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/30/2018
---
# <a name="connect-a-web-app-to-cosmos-db"></a>Een web-app verbinden met Cosmos DB

Met dit voorbeeldscript wordt een Azure Cosmos DB-account gemaakt met behulp van de MongoDB-API en een Azure-web-app. Vervolgens wordt de MongoDB-verbindingsreeks gekoppeld aan de web-app met behulp van app-instellingen.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, hebt u Azure CLI versie 2.0 of hoger nodig. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli).

## <a name="sample-script"></a>Voorbeeldscript

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/connect-to-documentdb/connect-to-documentdb.sh "Azure Cosmos DB")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt voor het maken van een resourcegroep, web-app, Cosmos-database en alle gerelateerde resources. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [`az group create`](/cli/azure/group?view=azure-cli-latest#az-group-create) | Hiermee maakt u een resourcegroep waarin alle resources worden opgeslagen. |
| [`az appservice plan create`](/cli/azure/appservice/plan?view=azure-cli-latest#az-appservice-plan-create) | Hiermee maakt u een App Service-plan. |
| [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create) | Hiermee maakt u een Azure-web-app. |
| [`az cosmosdb create`](/cli/azure/cosmosdb?view=azure-cli-latest#az-cosmosdb-create) | Hiermee maakt u een Cosmos DB-account. |
| [`az cosmosdb list-connection-strings`](/cli/azure/cosmosdb?view=azure-cli-latest#az-cosmosdb-list-connection-strings) | Hiermee geeft u de verbindingsreeksen weer voor het opgegeven Cosmos-DB-account. |
| [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) | Hiermee kunt u een app-instelling voor een Azure-web-app maken of bijwerken. App-instellingen worden weergegeven als omgevingsvariabelen voor uw app. |

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure) voor meer informatie over de Azure CLI.

Meer voorbeelden van App Service CLI-scripts vindt u in de [documentatie van Azure App Service](../app-service-cli-samples.md).
