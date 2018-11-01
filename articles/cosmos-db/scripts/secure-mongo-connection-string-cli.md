---
title: 'Azure CLI-voorbeeldscript: Azure Cosmos DB-verbindingsreeks ophalen voor MongoDB-apps | Microsoft Docs'
description: 'Azure CLI-voorbeeldscript: Azure Cosmos DB-verbindingsreeks ophalen voor MongoDB-apps'
services: cosmos-db
documentationcenter: cosmosdb
author: SnehaGunda
manager: kfile
tags: azure-service-management
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.custom: mvc
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: cosmosdb
ms.workload: database
ms.date: 06/02/2017
ms.author: sngun
ms.openlocfilehash: 33758d55698a5a4925980f7ebac00130e4df42dd
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2018
ms.locfileid: "50412394"
---
# <a name="get-an-azure-cosmos-db-connection-string-for-mongodb-apps-using-the-azure-cli"></a>Een Azure Cosmos DB-verbindingsreeks ophalen voor MongoDB-apps met de Azure CLI

Met dit voorbeeldscript haalt u een Azure Cosmos DB-verbindingsreeks op voor MongoDB-apps met behulp van de Azure CLI. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit onderwerp gebruikmaken van Azure CLI versie 2.0 of hoger. Voer `az --version` uit om de versie te bekijken. Zie [Azure CLI installeren]( /cli/azure/install-azure-cli) als u de CLI wilt installeren of een upgrade wilt uitvoeren. 

## <a name="sample-script"></a>Voorbeeldscript

[!code-azurecli-interactive[main](../../../cli_scripts/cosmosdb/secure-cosmosdb-get-mongodb-connection-string/secure-cosmosdb-get-mongodb-connection-string.sh "Get Azure Cosmos DB connection string for MongoDB apps")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie

Na het uitvoeren van het voorbeeldscript kan de volgende opdracht worden gebruikt om de resourcegroep en alle resources die er aan zijn gekoppeld te verwijderen.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az-group-create) | Hiermee wordt een resourcegroep gemaakt waarin alle resources worden opgeslagen. |
| [az cosmosdb update](https://docs.microsoft.com/cli/azure/cosmosdb#az-cosmosdb-update) | Hiermee wordt een Azure Cosmos DB-account bijgewerkt. |
| [az cosmosdb list-connection-strings](https://docs.microsoft.com/cli/azure/cosmosdb#az-cosmosdb-list-connection-strings) | Hiermee wordt de verbindingsreeks voor het account opgehaald.|
| [az group delete](https://docs.microsoft.com/cli/azure/group#az-group-delete) | Hiermee verwijdert u een resourcegroep met inbegrip van alle geneste resources. |

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure) voor meer informatie over de Azure CLI.

Meer Azure Cosmos DB CLI-voorbeeldscripts vindt u in de [documentatie van Azure Cosmos DB CLI](../cli-samples.md).
