---
title: 'Azure CLI-script: server-configuraties wijzigen'
description: Dit voorbeeldscript CLI geeft een lijst van alle beschikbare server-configuratie-opties en de waarde van een van de opties bijgewerkt.
services: postgres
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql-database
ms.devlang: azure-cli
ms.topic: sample
ms.custom: mvc
ms.date: 01/12/2018
ms.openlocfilehash: 334322557c83d5576c9a11def6bb0dc0e3ad1fec
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2018
---
# <a name="list-and-update-configurations-of-an-azure-database-for-postgresql-server-using-azure-cli"></a>Weergeven en bijwerken van configuraties van een Azure-Database voor PostgreSQL-server met Azure CLI
Dit voorbeeldscript CLI geeft een lijst van alle beschikbare configuratieparameters, evenals de toegestane waarden voor de Azure-Database voor PostgreSQL-server, en stelt de *log_retention_days* op een waarde die is dan de standaard een.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit artikel gebruikmaken van Azure CLI versie 2.0 of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Voorbeeld van een script
Wijzig de gemarkeerde regels voor het aanpassen van de gebruikersnaam van de beheerder en het wachtwoord in dit voorbeeldscript.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/change-server-configurations/change-server-configurations.sh?highlight=15-16 "List and update configurations of Azure Database for PostgreSQL.")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie
Na het uitvoeren van het voorbeeldscript kan de volgende opdracht worden gebruikt om te verwijderen van de resourcegroep en alle resources die zijn gekoppeld.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/change-server-configurations/delete-postgresql.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Script uitleg
Dit script maakt gebruik van de volgende opdrachten. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| **Opdracht** | **Opmerkingen bij de** |
|---|---|
| [AZ groep maken](/cli/azure/group#az_group_create) | Maakt een resourcegroep waarin alle resources worden opgeslagen. |
| [AZ postgres server maken](/cli/azure/postgres/server#az_postgres_server_create) | Maakt een PostgreSQL-server die als host fungeert voor de databases. |
| [lijst van AZ postgres server-configuratie](/cli/azure/postgres/server/configuration#az_postgres_server_configuration_list) | Lijst van de configuraties van een Azure-Database voor PostgreSQL-server. |
| [AZ postgres server-configuratie instellen](/cli/azure/postgres/server/configuration#az_postgres_server_configuration_set) | Werk de configuratie van een Azure-Database voor PostgreSQL-server. |
| [AZ postgres server configuratie weergeven](/cli/azure/postgres/server/configuration#az_postgres_server_configuration_show) | Tonen de configuratie van een Azure-Database voor PostgreSQL-server. |
| [AZ groep verwijderen](/cli/azure/group#az_group_delete) | Hiermee verwijdert u een resourcegroep met inbegrip van alle ingesloten resources. |

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over de Azure CLI: [documentatie van Azure CLI](/cli/azure/overview).
- Probeer extra scripts: [voorbeelden van Azure CLI voor Azure-Database voor PostgreSQL](../sample-scripts-azure-cli.md)
- Zie voor meer informatie over parameters van de server [serverparameters hoe te configureren in Azure-portal](../howto-configure-server-parameters-using-portal.md).
