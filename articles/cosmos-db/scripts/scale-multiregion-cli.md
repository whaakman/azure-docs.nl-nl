---
title: Azure CLI-script - Multi-regioreplicatie voor Azure Cosmos DB | Microsoft Docs
description: Azure CLI-voorbeeldscript - Multi-regioreplicatie voor Azure Cosmos-DB
services: cosmos-db
documentationcenter: cosmosdb
author: SnehaGunda
manager: kfile
tags: azure-service-management
ms.service: cosmos-db
ms.custom: mvc
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: cosmosdb
ms.workload: database
ms.date: 06/02/2017
ms.author: sngun
ms.openlocfilehash: cefc33227a540b3500278316cbee0306a42acc04
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38597856"
---
# <a name="replicate-an-azure-cosmos-db-database-account-in-multiple-regions-and-configure-failover-priorities-using-the-azure-cli"></a>Een Azure Cosmos DB-databaseaccount in meerdere regio's repliceren en failover-prioriteiten configureren met behulp van de Azure CLI

Met dit voorbeeld kunt u elk soort Azure Cosmos DB-databaseaccount in meerdere regio's repliceren en failover-prioriteiten configureren met behulp van de Azure CLI.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit onderwerp gebruikmaken van Azure CLI versie 2.0 of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Voorbeeldscript

[!code-azurecli-interactive[main](../../../cli_scripts/cosmosdb/scale-cosmosdb-replicate-multiple-regions/scale-cosmosdb-replicate-multiple-regions.sh?highlight=21-31 "Scale Azure Cosmos DB into multiple regions")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie

Na het uitvoeren van het voorbeeldscript kan de volgende opdracht worden gebruikt om de resourcegroep en alle resources die er aan zijn gekoppeld te verwijderen.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Hiermee wordt een resourcegroep gemaakt waarin alle resources worden opgeslagen. |
| [az cosmosdb update](https://docs.microsoft.com/cli/azure/cosmosdb#az_cosmosdb_update) | Hiermee wordt een Azure Cosmos DB-account bijgewerkt. |
| [az group delete](https://docs.microsoft.com/cli/azure/group#az_group_delete) | Hiermee verwijdert u een resourcegroep met inbegrip van alle geneste resources. |

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure) voor meer informatie over de Azure CLI.

Meer Azure Cosmos DB CLI-voorbeeldscripts vindt u in de [documentatie van Azure Cosmos DB CLI](../cli-samples.md).
