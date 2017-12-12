---
title: CLI-voorbeeld wordt een SQL elastische pool Azure SQL Database | Microsoft Docs
description: Azure CLI-voorbeeldscript voor het schalen van een elastische SQL-groep in Azure SQL Database
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
ms.date: 06/23/2017
ms.author: janeng
ms.openlocfilehash: 1bd1bb6005f463a8a317ec959661ac6c75d600d1
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2017
---
# <a name="use-cli-to-scale-a-sql-elastic-pool-in-azure-sql-database"></a>CLI gebruiken voor het schalen van een elastische SQL-groep in Azure SQL Database

In dit voorbeeld van Azure CLI script SQL elastische pools maakt, verplaatst gegroepeerde databases en elastische pool prestatieniveaus wordt gewijzigd. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit onderwerp gebruikmaken van Azure CLI versie 2.0 of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Voorbeeld van een script

[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/scale-pool/scale-pool.sh "Move database between pools")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie

Na het uitvoeren van het voorbeeldscript kan de volgende opdracht worden gebruikt om te verwijderen van de resourcegroep en alle resources die zijn gekoppeld.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdrachten voor het maken van een resourcegroep, logische server, SQL-Database en firewall-regels. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [AZ groep maken](https://docs.microsoft.com/cli/azure/group#az_group_create) | Maakt een resourcegroep waarin alle resources worden opgeslagen. |
| [AZ sql server maken](https://docs.microsoft.com/cli/azure/sql/server#az_sql_server_create) | Maakt een logische server die als host fungeert voor de SQL-Database. |
| [elastische sql van AZ-toepassingen maken](https://docs.microsoft.com/cli/azure/sql/elastic-pool#az_sql_elastic_pool_create) | Maakt een pool voor elastische database in de logische server. |
| [AZ sql-database maken](https://docs.microsoft.com/cli/azure/sql/db#az_sql_db_create) | De SQL-Database wordt gemaakt in de logische server. |
| [update van AZ sql elastische pools](https://docs.microsoft.com/cli/azure/sql/elastic-pool#az_sql_elastic_pool_update) | Een pool voor elastische database updates, het toegewezen aantal edtu's in dit voorbeeld wordt gewijzigd. |
| [AZ groep verwijderen](https://docs.microsoft.com/cli/azure/vm/extension#az_vm_extension_set) | Hiermee verwijdert u een resourcegroep met inbegrip van alle ingesloten resources. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure CLI [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure/overview).

Voorbeelden van aanvullende SQL Database CLI-script kunnen worden gevonden in de [documentatie van Azure SQL Database](../sql-database-cli-samples.md).
