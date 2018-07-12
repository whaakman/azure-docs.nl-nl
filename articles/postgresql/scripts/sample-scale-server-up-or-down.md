---
title: 'Azure CLI-script: schaal aanpassen voor Azure Database for PostgreSQL'
description: "Azure CLI-voorbeeldscript: de schaal van een Azure Database for PostgreSQL-server aanpassen naar een ander prestatieniveau na het uitvoeren van query's op de metrische gegevens."
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.custom: mvc
ms.topic: sample
ms.date: 04/05/2018
ms.openlocfilehash: ff740bf22b745e3f9e00ccbdb40c5e4d5948fb41
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38618807"
---
# <a name="monitor-and-scale-a-single-postgresql-server-using-azure-cli"></a>Eén PostgreSQL-server bewaken en de schaal ervan aanpassen met Azure CLI
Met dit CLI-voorbeeldscript wordt de schaal van één Azure Database for PostgreSQL-server aangepast naar een ander prestatieniveau na het uitvoeren van query's op de metrische gegevens. 

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal uit te voeren, moet u voor dit artikel gebruikmaken van Azure CLI-versie 2.0 of hoger. Controleer de versie door `az --version` uit te voeren. Zie [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli) voor het installeren of upgraden van uw versie van Azure CLI.

## <a name="sample-script"></a>Voorbeeldscript
Bewerk in dit voorbeeldscript de gemarkeerde regels om de gebruikersnaam en het wachtwoord van de beheerder naar uw eigen bij te werken. Vervang de abonnement-ID die wordt gebruikt in de `az monitor`-opdrachten door uw eigen abonnement-ID.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/scale-postgresql-server/scale-postgresql-server.sh?highlight=18-19 "Create and scale Azure Database for PostgreSQL.")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie
Gebruik de volgende opdracht om de resourcegroep en alle resources die er aan zijn gekoppeld te verwijderen nadat het script is uitgevoerd. 
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/scale-postgresql-server/delete-postgresql.sh "Delete the resource group.")]

## <a name="script-explanation"></a>Uitleg van het script
Dit script maakt gebruik van de opdrachten die in de volgende tabel worden weergegeven:

| **Opdracht** | **Opmerkingen** |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Hiermee wordt een resourcegroep gemaakt waarin alle resources worden opgeslagen. |
| [az postgres server create](/cli/azure/postgres/server#az_postgres_server_create) | Hiermee wordt een PostgreSQL-server gemaakt waar de SQL-database wordt gehost. |
| [az monitor metrics list](/cli/azure/monitor/metrics#az_monitor_metrics_list) | Geeft de metrische waarde weer voor de resources. |
| [az group delete](/cli/azure/group#az_group_delete) | Hiermee verwijdert u een resourcegroep met inbegrip van alle geneste resources. |

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over de Azure CLI: [documentatie van Azure CLI](/cli/azure)
- Meer scripts om te proberen: [Azure CLI-voorbeelden voor Azure Database for PostgreSQL](../sample-scripts-azure-cli.md)
- Meer informatie over de schaal aanpassen: [Servicelagen](../concepts-service-tiers.md) en [Rekeneenheden en opslageenheden](../concepts-compute-unit-and-storage.md)
