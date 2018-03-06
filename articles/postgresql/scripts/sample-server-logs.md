---
title: 'Azure CLI-script: serverlogboeken downloaden in Azure Database for PostgreSQL'
description: Dit voorbeeld Azure CLI script laat zien hoe u de serverlogboeken van een Azure Database voor PostgreSQL server kunt activeren en downloaden.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: sample
ms.custom: mvc
ms.date: 02/28/2018
ms.openlocfilehash: 195a9d1162798e916a9fc8fc6efce58a0af9f2eb
ms.sourcegitcommit: 83ea7c4e12fc47b83978a1e9391f8bb808b41f97
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2018
---
# <a name="enable-and-download-server-slow-query-logs-of-an-azure-database-for-postgresql-server-using-azure-cli"></a>Langzame-serverquerylogboeken van een Azure Database voor PostgreSQL-server activeren en downloaden met behulp van Azure CLI
Met dit CLI-voorbeeldscript worden de langzame-querylogboeken van één Azure Database for PostgreSQL server geactiveerd en gedownload.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal uit te voeren, moet u voor dit artikel gebruikmaken van Azure CLI-versie 2.0 of hoger. Controleer de versie door `az --version` uit te voeren. Zie [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli) voor het installeren of upgraden van uw versie van Azure CLI.

## <a name="sample-script"></a>Voorbeeldscript
Bewerk in dit voorbeeldscript de gemarkeerde regels om de gebruikersnaam en het wachtwoord van de beheerder naar uw eigen bij te werken. Vervang <log_file_name> in de `az monitor`-opdrachten door de naam van uw eigen serverlogboekbestand.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/server-logs/server-logs.sh?highlight=18-19 "Manipulate with server logs.")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie
Gebruik de volgende opdracht om de resourcegroep en alle resources die er aan zijn gekoppeld te verwijderen nadat het script is uitgevoerd. 
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/server-logs/delete-postgresql.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Uitleg van het script
Dit script maakt gebruik van de opdrachten die in de volgende tabel worden weergegeven:

| **Opdracht** | **Opmerkingen** |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Hiermee wordt een resourcegroep gemaakt waarin alle resources worden opgeslagen. |
| [az postgres server create](/cli/azure/postgres/server#az_msql_server_create) | Hiermee wordt een PostgreSQL-server gemaakt waar de SQL-database wordt gehost. |
| [az postgres server configuration list](/cli/azure/postgres/server/configuration#az_postgres_server_configuration_list) | Hiermee maakt u een lijst van de configuratiewaarden voor een server. |
| [az postgres server configuration set](/cli/azure/postgres/server/configuration#az_postgres_server_configuration_set) | Hiermee werkt u de configuratie van een server bij. |
| [az postgres server-logs list](/cli/azure/postgres/server-logs#az_postgres_server_logs_list) | Hiermee maakt u een lijst met de logboekbestanden van een server. |
| [az postgres server-logs download](/cli/azure/postgres/server-logs#az_postgres_server_logs_download) | Hiermee kunt u logboekgegevens downloaden. |
| [az group delete](/cli/azure/group#az_group_delete) | Hiermee verwijdert u een resourcegroep met inbegrip van alle geneste resources. |

## <a name="next-steps"></a>Volgende stappen
- Lees de [documentatie van Azure CLI](/cli/azure/overview) voor meer informatie over de Azure CLI.
- Meer scripts om te proberen: [Azure CLI-voorbeelden voor Azure Database for PostgreSQL](../sample-scripts-azure-cli.md)
- [Serverlogboeken in de Azure-portal configureren en benaderen](../howto-configure-server-logs-in-portal.md)
