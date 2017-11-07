---
title: Azure CLI-script - serverconfiguraties wijzigen | Microsoft Docs
description: Dit voorbeeldscript CLI geeft een lijst van alle beschikbare server-configuraties en de waarde van innodb_lock_wait_timeout bijgewerkt.
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: sample
ms.custom: mvc
ms.date: 11/03/2017
ms.openlocfilehash: 286b16148d28509714862714dcc5986496d9af44
ms.sourcegitcommit: 38c9176c0c967dd641d3a87d1f9ae53636cf8260
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2017
---
# <a name="list-and-update-configurations-of-an-azure-database-for-mysql-server-using-azure-cli"></a>Weergeven en bijwerken van configuraties van een Azure-Database voor de MySQL-server met Azure CLI
Dit voorbeeldscript CLI bevat alle beschikbare configuratieparameters, evenals de toegestane waarden for Azure Database MySQL-server, en stelt de *innodb_lock_wait_timeout* op een waarde die is dan de standaard een.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Als u wilt installeren en gebruiken van de CLI lokaal, in dit artikel is vereist dat u de Azure CLI versie 2.0 of hoger worden uitgevoerd. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Voorbeeld van een script
Wijzig de gemarkeerde regels voor het aanpassen van de gebruikersnaam van de beheerder en het wachtwoord in dit voorbeeldscript.
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/change-server-configurations/change-server-configurations.sh?highlight=15-16 "List and update configurations of Azure Database for MySQL.")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie
Na het uitvoeren van het voorbeeldscript kan de volgende opdracht worden gebruikt om te verwijderen van de resourcegroep en alle resources die zijn gekoppeld.
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/change-server-configurations/delete-mysql.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Script uitleg
Dit script maakt gebruik van de volgende opdrachten. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| **Opdracht** | **Opmerkingen bij de** |
|---|---|
| [AZ groep maken](/cli/azure/group#create) | Maakt een resourcegroep waarin alle resources worden opgeslagen. |
| [AZ mysql-server maken](/cli/azure/mysql/server#create) | Hiermee maakt u een MySQL-server die als host fungeert voor de databases. |
| [lijst van AZ mysql server-configuratie](/cli/azure/mysql/server/configuration#list) | Lijst van de configuraties van een Azure-Database voor de MySQL-server. |
| [AZ mysql server configuratie instellen](/cli/azure/mysql/server/configuration#set) | Werk de configuratie van een Azure-Database voor de MySQL-server. |
| [AZ mysql server configuratie weergeven](/cli/azure/mysql/server/configuration#show) | Tonen de configuratie van een Azure-Database voor de MySQL-server. |
| [AZ groep verwijderen](/cli/azure/group#delete) | Hiermee verwijdert u een resourcegroep met inbegrip van alle ingesloten resources. |

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over de Azure CLI: [documentatie van Azure CLI](/cli/azure/overview).
- Probeer extra scripts: [voorbeelden van Azure CLI voor Azure-Database voor MySQL](../sample-scripts-azure-cli.md)
- Zie voor meer informatie over parameters van de server [hoe te configureren Parameters van de Server in Azure-Database voor MySQL](../howto-server-parameters.md).
