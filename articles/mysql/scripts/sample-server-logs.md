---
title: 'Azure CLI: Server-Logboeken in Azure-Database voor MySQL downloaden'
description: Dit voorbeeldscript Azure CLI laat zien hoe inschakelen en de serverlogboeken van een Azure-Database voor de MySQL-server downloaden.
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: sample
ms.custom: mvc
ms.date: 01/11/2018
ms.openlocfilehash: b0d34009d189ab136dcb6f28fdccc49b6da9e108
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="enable-and-download-server-slow-query-logs-of-an-azure-database-for-mysql-server-using-azure-cli"></a>Inschakelen en downloaden van de query wordt vertraagd serverlogboeken van een Azure-Database voor de MySQL-server met Azure CLI
Dit voorbeeldscript CLI kunt en downloadt de logboeken van de query wordt vertraagd van één Azure-Database voor de MySQL-server.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Als u wilt installeren en gebruiken van de CLI lokaal, wordt in dit voorbeeld vereist dat u de Azure CLI versie 2.0 of hoger worden uitgevoerd. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Voorbeeld van een script
Wijzig de gemarkeerde regels voor het aanpassen van de gebruikersnaam van de beheerder en het wachtwoord in dit voorbeeldscript. De < logbestandnaam > in de monitor az opdrachten vervangen door uw eigen servernaam van het logboek.
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/server-logs/server-logs.sh?highlight=15-16 "Manipulate with server logs.")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie
Na het uitvoeren van het voorbeeldscript kan de volgende opdracht worden gebruikt om te verwijderen van de resourcegroep en alle resources die zijn gekoppeld.
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/server-logs/delete-mysql.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Script uitleg
Dit script maakt gebruik van de volgende opdrachten. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| **Opdracht** | **Opmerkingen bij de** |
|---|---|
| [AZ groep maken](/cli/azure/group#az_group_create) | Maakt een resourcegroep waarin alle resources worden opgeslagen. |
| [AZ mysql-server maken](/cli/azure/mysql/server#az_msql_server_create) | Hiermee maakt u een MySQL-server die als host fungeert voor de databases. |
| [lijst van AZ mysql server-configuratie](/cli/azure/mysql/server/configuration#az_mysql_server_configuration_list) | Lijst van de configuratiewaarden voor een server. |
| [AZ mysql server configuratie instellen](/cli/azure/mysql/server/configuration#az_mysql_server_configuration_set) | Werk de configuratie van een server. |
| [AZ mysql serverlogboeken lijst](/cli/azure/mysql/server-logs#az_mysql_server_logs_list) | Een lijst met logboekbestanden voor een server. |
| [mysql-serverlogboeken AZ downloaden](/cli/azure/mysql/server-logs#az_mysql_server_logs_download) | Download de logboekbestanden. |
| [AZ groep verwijderen](/cli/azure/group#az_group_delete) | Hiermee verwijdert u een resourcegroep met inbegrip van alle ingesloten resources. |

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over de Azure CLI: [documentatie van Azure CLI](/cli/azure/overview).
- Probeer extra scripts: [voorbeelden van Azure CLI voor Azure-Database voor MySQL](../sample-scripts-azure-cli.md)
