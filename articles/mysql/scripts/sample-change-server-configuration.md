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
ms.date: 08/29/2017
ms.openlocfilehash: 558e7d4ee96a3424a777f9f57679da25f2306efd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="list-and-update-configurations-of-an-azure-database-for-mysql-server-using-azure-cli"></a>Weergeven en bijwerken van configuraties van een Azure-Database voor de MySQL-server met Azure CLI
Dit voorbeeldscript CLI bevat alle beschikbare configuratieparameters, evenals de toegestane waarden for Azure Database MySQL-server, en stelt de *innodb_lock_wait_timeout* op een waarde die is dan de standaard een.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit onderwerp gebruikmaken van Azure CLI versie 2.0 of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

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
