---
title: Maken van een Azure-functie die is verbonden met een Cosmos Azure DB | Microsoft Docs
description: 'Azure CLI-Script voorbeeld: een Azure-functie die is verbonden met een Cosmos Azure DB maken'
services: functions
documentationcenter: functions
author: rachelappel
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
ms.author: rachelap
ms.custom: mvc
ms.openlocfilehash: c2c3530df62a1f291be51739a7918f7b8ab08487
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-azure-function-that-connects-to-an-azure-cosmos-db"></a>Een Azure-functie die is verbonden met een Cosmos Azure DB maken

Dit voorbeeldscript wordt een Azure-functie-App gemaakt en verbinding maakt met een Azure DB die Cosmos-database.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit onderwerp gebruikmaken van Azure CLI versie 2.0 of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Voorbeeld van een script

Dit voorbeeld maakt u een Azure-functie-app en een Cosmos-DB-eindpunt en de toegangssleutel toegevoegd aan app-instellingen.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-connect-to-cosmos-db/create-function-app-connect-to-cosmos-db.sh "Create an Azure Function that connects to an Azure Cosmos DB")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie

Na het uitvoeren van het voorbeeldscript de volgende opdracht kan worden gebruikt voor het verwijderen van de resourcegroep en alle bijbehorende resources.

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdrachten. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [AZ aanmelding](https://docs.microsoft.com/cli/azure/#login) | Aanmelden bij Azure. |
| [AZ groep maken](https://docs.microsoft.com/cli/azure/group#az_group_create) | Een resourcegroep maken met de locatie |
| [AZ storage-account maken](https://docs.microsoft.com/cli/azure/storage/account) | Een opslagaccount maken |
| [AZ functionapp maken](https://docs.microsoft.com/cli/azure/functionapp#az_functionapp_create) | Een nieuwe functie-app maken |
| [AZ documentdb maken](https://docs.microsoft.com/cli/azure/documentdb#az_documentdb_create) | Documentdb-database maken |
| [AZ groep verwijderen](https://docs.microsoft.com/cli/azure/group#az_group_delete) | Opruimen |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure CLI [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure/overview).

Aanvullende voorbeelden van Azure Functions CLI-script kunnen worden gevonden in de [documentatie van Azure Functions](../functions-cli-samples.md).




