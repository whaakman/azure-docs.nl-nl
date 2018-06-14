---
title: Een Azure-functie maken die verbinding maakt met een Azure Cosmos DB | Microsoft Docs
description: Azure CLI-scriptvoorbeeld - Een Azure-functie maken die verbinding maakt met een Azure Cosmos DB
services: functions
documentationcenter: functions
author: ggailey777
manager: cfowler
editor: ''
tags: functions
ms.assetid: ''
ms.service: functions
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: ''
ms.date: 01/22/2018
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 092e0681b0491fc1c54c19e234aafdac6d428fd1
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/09/2018
ms.locfileid: "29843218"
---
# <a name="create-an-azure-function-that-connects-to-an-azure-cosmos-db"></a>Een Azure-functie maken die verbinding maakt met een Azure Cosmos DB

Met dit Azure Functions-voorbeeldscript wordt een functie-app gemaakt en wordt de functie verbonden met een Azure Cosmos DB-database. De gemaakte app-instelling die de verbinding bevat, kan worden gebruikt met een [Azure Cosmos DB-trigger of -binding](..\functions-bindings-cosmosdb.md).

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u de CLI lokaal gebruikt, moet u ervoor zorgen dat u Azure CLI versie 2.0 of hoger gebruikt. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Voorbeeldscript

In dit voorbeeld maakt u een Azure-functie-app en voegt u een Cosmos-DB-eindpunt en -toegangssleutel aan app-instellingen.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-connect-to-cosmos-db/create-function-app-connect-to-cosmos-db.sh "Create an Azure Function that connects to an Azure Cosmos DB")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt: Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [az login](https://docs.microsoft.com/cli/azure/reference-index#az_login) | Aanmelden bij Azure. |
| [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create) | Een resourcegroep maken met locatie |
| [az storage accounts create](https://docs.microsoft.com/cli/azure/storage/account) | Een opslagaccount maken |
| [az functionapp create](https://docs.microsoft.com/cli/azure/functionapp#az_functionapp_create) | Een nieuwe functie-app maken |
| [az cosmosdb create](https://docs.microsoft.com/cli/azure/cosmosdb#az_cosmosdb_create) | Een CosmosDB-database maken |
| [az group delete](https://docs.microsoft.com/cli/azure/group#az_group_delete) | Opruimen |

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure) voor meer informatie over de Azure CLI.

Aanvullende CLI-voorbeeldscripts voor Azure Functions vindt u in de [Azure Functions-documentatie](../functions-cli-samples.md).




