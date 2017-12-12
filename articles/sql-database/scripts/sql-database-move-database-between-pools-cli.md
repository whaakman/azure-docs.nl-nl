---
title: CLI voorbeeld verplaatsen Azure SQL database-SQL elastische pool | Microsoft Docs
description: Azure CLI-voorbeeldscript te verplaatsen van een SQL-database in een elastische SQL-groep
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune, mvc
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 07/05/2017
ms.author: janeng
ms.openlocfilehash: 6da859530cd692d80f82270cb599483041612db7
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2017
---
# <a name="use-cli-to-move-an-azure-sql-database-in-a-sql-elastic-pool"></a>Gebruik CLI voor het verplaatsen van een Azure SQL database in een elastische SQL-groep

In dit voorbeeld van Azure CLI script maakt twee elastische pools en een Azure SQL database in een elastische SQL-groep is verplaatst naar een andere elastische SQL-groep en gaat vervolgens door de database uit de elastische groep naar een prestatieniveau van één Azure-database. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit onderwerp gebruikmaken van Azure CLI versie 2.0 of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Voorbeeld van een script

[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/move-database-between-pools/move-database-between-pools.sh "Move database between pools")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie

Na het uitvoeren van het voorbeeldscript kan de volgende opdracht worden gebruikt om te verwijderen van de resourcegroep en alle resources die zijn gekoppeld.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdrachten. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [AZ groep maken](https://docs.microsoft.com/cli/azure/group#az_group_create) | Maakt een resourcegroep waarin alle resources worden opgeslagen. |
| [AZ sql server maken](https://docs.microsoft.com/cli/azure/sql/server#az_sql_server_create) | Maakt een logische server die als host fungeert voor een database of elastische pool. |
| [elastische sql van AZ-toepassingen maken](https://docs.microsoft.com/cli/azure/sql/elastic-pool#az_sql_elastic_pool_create) | Maakt een elastische pool in de logische server. |
| [AZ sql-database maken](https://docs.microsoft.com/cli/azure/sql/db#az_sql_db_create) | Maakt een database in een logische server als één of een gegroepeerde database. |
| [AZ sql database-update](https://docs.microsoft.com/cli/azure/sql/db#az_sql_db_update) | Database-eigenschappen-updates of een database verplaatst naar, uit of tussen elastische pools. |
| [AZ groep verwijderen](https://docs.microsoft.com/cli/azure/vm/extension#az_vm_extension_set) | Hiermee verwijdert u een resourcegroep met inbegrip van alle ingesloten resources. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure CLI [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure/overview).

Voorbeelden van aanvullende SQL Database CLI-script kunnen worden gevonden in de [documentatie van Azure SQL Database](../sql-database-cli-samples.md).


