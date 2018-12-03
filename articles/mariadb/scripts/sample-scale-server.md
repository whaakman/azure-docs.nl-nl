---
title: 'Azure CLI-script: de schaal van een Azure Database for MariaDB-server aanpassen'
description: Met dit CLI-voorbeeldscript wordt de schaal van een Azure Database for MariaDB-server aangepast naar een ander prestatieniveau na het doorzoeken van de metrische gegevens.
services: mariadb
author: ajlam
ms.author: andrela
editor: jasonwhowell
ms.service: mariadb
ms.devlang: azure-cli
ms.topic: sample
ms.custom: mvc
ms.date: 11/28/2018
ms.openlocfilehash: e5f6364992987e5aeb6cb1ccd2055823efae607e
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2018
ms.locfileid: "52585157"
---
# <a name="monitor-and-scale-an-azure-database-for-mariadb-server-using-azure-cli"></a>Een Azure Database for MariaDB-server bewaken en de schaal ervan aanpassen met Azure CLI
Met dit CLI-voorbeeldscript wordt de schaal van een enkele Azure Database for MariaDB-server aangepast naar een ander prestatieniveau na het doorzoeken van de metrische gegevens.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal uit te voeren, moet u voor dit artikel gebruikmaken van Azure CLI-versie 2.0 of hoger. Controleer de versie door `az --version` uit te voeren. Zie [Azure CLI installeren]( /cli/azure/install-azure-cli) voor het installeren of upgraden van uw versie van Azure CLI. 

## <a name="sample-script"></a>Voorbeeldscript
Bewerk in dit voorbeeldscript de gemarkeerde regels om de gebruikersnaam en het wachtwoord van de beheerder naar uw eigen bij te werken. Vervang de abonnement-ID die wordt gebruikt in de `az monitor`-opdrachten door uw eigen abonnement-ID. [!code-azurecli-interactive[main](../../../cli_scripts/mariadb/scale-mariadb-server/scale-mariadb-server.sh?highlight=15-16 "Create and scale Azure Database for MariaDB.")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie
Gebruik de volgende opdracht om de resourcegroep en alle resources die er aan zijn gekoppeld te verwijderen nadat het script is uitgevoerd. 
[!code-azurecli-interactive[main](../../../cli_scripts/mariadb/scale-mariadb-server/delete-mariadb.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Uitleg van het script
Dit script maakt gebruik van de opdrachten die in de volgende tabel worden weergegeven:

| **Opdracht** | **Opmerkingen** |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Hiermee maakt u een resourcegroep waarin alle resources worden opgeslagen. |
| [az mariadb server create](/cli/azure/mariadb/server#az-mariadb-server-create) | Hiermee wordt een MariaDB-server gemaakt waarop de databases worden gehost. |
| [az monitor metrics list](/cli/azure/monitor/metrics#az-monitor-metrics-list) | Geeft de metrische waarde weer voor de resources. |
| [az group delete](/cli/azure/group#az-group-delete) | Hiermee verwijdert u een resourcegroep met inbegrip van alle geneste resources. |

## <a name="next-steps"></a>Volgende stappen
- Lees de [documentatie van Azure CLI](/cli/azure) voor meer informatie over de Azure CLI.
- Meer scripts om te proberen: [Azure CLI-voorbeelden voor Azure Database for MariaDB](../sample-scripts-azure-cli.md)
- Zie [Pricing Tiers](../concepts-pricing-tiers.md voor meer informatie over schalen.
