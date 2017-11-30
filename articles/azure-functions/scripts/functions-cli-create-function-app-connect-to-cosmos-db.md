---
title: Maken van een Azure-functie die is verbonden met een Cosmos Azure DB | Microsoft Docs
description: 'Azure CLI-Script voorbeeld: een Azure-functie die is verbonden met een Cosmos Azure DB maken'
services: functions
documentationcenter: functions
author: ggailey777
manager: cfowler
editor: 
tags: functions
ms.assetid: 
ms.service: functions
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 04/20/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 7e9ccd26e7c066189576903c39be63a421d1da23
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2017
---
# <a name="create-an-azure-function-that-connects-to-an-azure-cosmos-db"></a>Een Azure-functie die is verbonden met een Cosmos Azure DB maken

Dit voorbeeldscript wordt een Azure-functie-App gemaakt en verbinding maakt met een Azure DB die Cosmos-database.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u de CLI lokaal gebruikt, zorg ervoor dat u de Azure CLI versie 2.0 of hoger worden uitgevoerd. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Voorbeeld van een script

Dit voorbeeld maakt u een Azure-functie-app en een Cosmos-DB-eindpunt en de toegangssleutel toegevoegd aan app-instellingen.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-connect-to-cosmos-db/create-function-app-connect-to-cosmos-db.sh "Create an Azure Function that connects to an Azure Cosmos DB")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie

Na het uitvoeren van het voorbeeldscript de volgende opdracht kan worden gebruikt voor het verwijderen van de resourcegroep en alle bijbehorende resources.

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdrachten: elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [AZ aanmelding](https://docs.microsoft.com/cli/azure/#login) | Aanmelden bij Azure. |
| [AZ groep maken](https://docs.microsoft.com/cli/azure/group#az_group_create) | Een resourcegroep maken met de locatie |
| [AZ storage-accounts maken](https://docs.microsoft.com/cli/azure/storage/account) | Een opslagaccount maken |
| [AZ functionapp maken](https://docs.microsoft.com/cli/azure/functionapp#az_functionapp_create) | Een nieuwe functie-app maken |
| [AZ cosmosdb maken](https://docs.microsoft.com/cli/azure/cosmosdb#az_cosmosdb_create) | Cosmosdb database maken |
| [AZ groep verwijderen](https://docs.microsoft.com/cli/azure/group#az_group_delete) | Opruimen |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure CLI [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure/overview).

Aanvullende voorbeelden van Azure Functions CLI-script kunnen worden gevonden in de [documentatie van Azure Functions](../functions-cli-samples.md).




