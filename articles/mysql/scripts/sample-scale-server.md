---
title: Azure CLI-voorbeelden voor het schalen van een Azure-Database voor de MySQL-server | Microsoft Docs
description: Dit voorbeeldscript CLI schaalt Azure Database voor de MySQL-server naar een andere prestatieniveau na het uitvoeren van query's de metrische gegevens.
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: sample
ms.custom: mvc
ms.date: 11/02/2017
ms.openlocfilehash: 517208b76757655990f82f50677267320eb1393c
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2017
---
# <a name="monitor-and-scale-an-azure-database-for-mysql-server-using-azure-cli"></a>Bewaken en schalen van een Azure-Database voor de MySQL-server met Azure CLI
Dit voorbeeldscript CLI schaalt één Azure-Database voor de MySQL-server naar een andere prestatieniveau na het uitvoeren van query's de metrische gegevens.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit artikel gebruikmaken van Azure CLI versie 2.0 of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Voorbeeld van een script
Wijzig de gemarkeerde regels voor het aanpassen van de gebruikersnaam van de beheerder en het wachtwoord in dit voorbeeldscript. Vervang de abonnements-ID in de opdrachten az monitor met uw eigen abonnements-id wordt gebruikt.[!code-azurecli-interactive[main](../../../cli_scripts/mysql/scale-mysql-server/scale-mysql-server.sh?highlight=15-16 "Create and scale Azure Database for MySQL.")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie
Nadat het voorbeeldscript is uitgevoerd, kunt u de resourcegroep en alle resources die zijn gekoppeld met de volgende opdracht:[!code-azurecli-interactive[main](../../../cli_scripts/mysql/scale-mysql-server/delete-mysql.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Script uitleg
Dit script maakt gebruik van de volgende opdrachten. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| **Opdracht** | **Opmerkingen bij de** |
|---|---|
| [AZ groep maken](/cli/azure/group#az_group_create) | Maakt een resourcegroep waarin alle resources worden opgeslagen. |
| [AZ mysql-server maken](/cli/azure/mysql/server#az_mysql_server_create) | Hiermee maakt u een MySQL-server die als host fungeert voor de databases. |
| [lijst met AZ monitor metrische gegevens](/cli/azure/monitor/metrics#az_monitor_metrics_list) | Lijst van de metrische waarde voor de bronnen. |
| [AZ groep verwijderen](/cli/azure/group#az_group_delete) | Hiermee verwijdert u een resourcegroep met inbegrip van alle ingesloten resources. |

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over de Azure CLI: [documentatie van Azure CLI](/cli/azure/overview).
- Probeer extra scripts: [voorbeelden van Azure CLI voor Azure-Database voor MySQL](../sample-scripts-azure-cli.md)
- Zie voor meer informatie over het schalen [Servicelagen](../concepts-service-tiers.md) en [Compute-eenheden en opslageenheden](../concepts-compute-unit-and-storage.md).
