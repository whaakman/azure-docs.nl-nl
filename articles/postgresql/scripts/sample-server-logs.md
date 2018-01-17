---
title: 'Azure CLI-script: Download serverlogboekbestanden in Azure-Database voor PostgreSQL'
description: Dit voorbeeldscript Azure CLI laat zien hoe inschakelen en de serverlogboeken van een Azure-Database voor PostgreSQL-server downloaden.
services: postgresql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql-database
ms.devlang: azure-cli
ms.topic: sample
ms.custom: mvc
ms.date: 01/12/2018
ms.openlocfilehash: 8bd6e193ca52401b16e141162a76d39bdef840f1
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2018
---
# <a name="enable-and-download-server-slow-query-logs-of-an-azure-database-for-postgresql-server-using-azure-cli"></a>Inschakelen en downloaden van de query wordt vertraagd serverlogboeken van een Azure-Database voor PostgreSQL-server met Azure CLI
Dit voorbeeldscript CLI kunt en downloadt de logboeken van de query wordt vertraagd van één Azure-Database voor PostgreSQL-server.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Als u wilt installeren en gebruiken van de CLI lokaal, wordt in dit voorbeeld vereist dat u de Azure CLI versie 2.0 of hoger worden uitgevoerd. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Voorbeeld van een script
Wijzig de gemarkeerde regels voor het aanpassen van de gebruikersnaam van de beheerder en het wachtwoord in dit voorbeeldscript. De < logbestandnaam > in de monitor az opdrachten vervangen door uw eigen servernaam van het logboek.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/server-logs/server-logs.sh?highlight=15-16 "Manipulate with server logs.")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie
Na het uitvoeren van het voorbeeldscript kan de volgende opdracht worden gebruikt om te verwijderen van de resourcegroep en alle resources die zijn gekoppeld.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/server-logs/delete-postgresql.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Script uitleg
Dit script maakt gebruik van de volgende opdrachten. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| **Opdracht** | **Opmerkingen bij de** |
|---|---|
| [AZ groep maken](/cli/azure/group#az_group_create) | Maakt een resourcegroep waarin alle resources worden opgeslagen. |
| [AZ postgres server maken](/cli/azure/postgres/server#az_msql_server_create) | Maakt een PostgreSQL-server die als host fungeert voor de databases. |
| [lijst van AZ postgres server-configuratie](/cli/azure/postgres/server/configuration#az_postgres_server_configuration_list) | Lijst van de configuratiewaarden voor een server. |
| [AZ postgres server-configuratie instellen](/cli/azure/postgres/server/configuration#az_postgres_server_configuration_set) | Werk de configuratie van een server. |
| [AZ postgres serverlogboeken lijst](/cli/azure/postgres/server-logs#az_postgres_server_logs_list) | Een lijst met logboekbestanden voor een server. |
| [AZ postgres serverlogboeken downloaden](/cli/azure/postgres/server-logs#az_postgres_server_logs_download) | Download de logboekbestanden. |
| [AZ groep verwijderen](/cli/azure/group#az_group_delete) | Hiermee verwijdert u een resourcegroep met inbegrip van alle ingesloten resources. |

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over de Azure CLI: [documentatie van Azure CLI](/cli/azure/overview).
- Probeer extra scripts: [voorbeelden van Azure CLI voor Azure-Database voor PostgreSQL](../sample-scripts-azure-cli.md)
- [Configureren en de logboeken van de toegangsserver in de Azure portal](../howto-configure-server-logs-in-portal.md)
