---
title: Azure CLI Script - Maak een Azure-Database voor PostgreSQL | Microsoft Docs
description: Azure CLI-voorbeeldscript - maakt een Azure-Database voor PostgreSQL-server en configureert u een firewallregel op serverniveau.
services: postgresql
author: salonisonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.devlang: azure-cli
ms.topic: sample
ms.date: 11/27/2017
ms.openlocfilehash: f92739181a2011be7ce609b65bf7c862ac705129
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="create-an-azure-database-for-postgresql-server-and-configure-a-firewall-rule-using-the-azure-cli"></a>Een Azure-Database voor PostgreSQL-server maken en configureren van een firewallregel op met de Azure CLI
Dit voorbeeldscript CLI maakt een Azure-Database voor PostgreSQL-server en configureert u een firewallregel op serverniveau. Nadat het script is uitgevoerd, zijn de PostgreSQL-server toegankelijk vanuit alle Azure-services en het geconfigureerde IP-adres.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit artikel gebruikmaken van Azure CLI versie 2.0 of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Voorbeeld van een script
Bewerk de gemarkeerde regels voor het aanpassen van de gebruikersnaam van de beheerder en het wachtwoord in dit voorbeeldscript.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/create-postgresql-server-and-firewall-rule/create-postgresql-server-and-firewall-rule.sh?highlight=15-16 "Create an Azure Database for PostgreSQL, and server-level firewall rule.")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie
Na het uitvoeren van het voorbeeldscript kan de volgende opdracht worden gebruikt om te verwijderen van de resourcegroep en alle resources die zijn gekoppeld.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/create-postgresql-server-and-firewall-rule/delete-postgresql.sh "Delete the resource group.")]

## <a name="script-explanation"></a>Script uitleg
Dit script maakt gebruik van de volgende opdrachten. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| **Opdracht** | **Opmerkingen bij de** |
|---|---|
| [AZ groep maken](/cli/azure/group#az_group_create) | Maakt een resourcegroep waarin alle resources worden opgeslagen. |
| [AZ postgres server maken](/cli/azure/postgres/server#az_postgres_server_create) | Maakt een PostgreSQL-server die als host fungeert voor de databases. |
| [AZ postgres serverfirewall maken](/cli/azure/postgres/server/firewall-rule#az_postgres_server_firewall_rule_create) | Maakt een firewallregel voor toegang tot de server en de databases in deze uit het opgegeven IP-adresbereik. |
| [AZ groep verwijderen](/cli/azure/group#az_group_delete) | Hiermee verwijdert u een resourcegroep met inbegrip van alle ingesloten resources. |

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over de Azure CLI: [Azure CLI-documentatie](/cli/azure/overview)
- Probeer extra scripts: [voorbeelden van Azure CLI voor Azure-Database voor PostgreSQL](../sample-scripts-azure-cli.md)
