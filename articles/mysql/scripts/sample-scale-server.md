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
ms.date: 05/31/2017
ms.openlocfilehash: 33316ff3db382d25a444d55772c6ee4d7b7ac418
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="monitor-and-scale-an-azure-database-for-mysql-server-using-azure-cli"></a>Bewaken en schalen van een Azure-Database voor de MySQL-server met Azure CLI
Dit voorbeeldscript CLI schaalt één Azure-Database voor de MySQL-server naar een andere prestatieniveau na het uitvoeren van query's de metrische gegevens.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit onderwerp gebruikmaken van Azure CLI versie 2.0 of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Voorbeeld van een script
Wijzig de gemarkeerde regels voor het aanpassen van de gebruikersnaam van de beheerder en het wachtwoord in dit voorbeeldscript. Vervang de abonnements-id in de opdrachten az monitor met uw eigen abonnements-id wordt gebruikt.[!code-azurecli-interactive[main](../../../cli_scripts/mysql/scale-mysql-server/scale-mysql-server.sh?highlight=15-16 "Create and scale Azure Database for MySQL.")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie
Na het uitvoeren van het voorbeeldscript kan de volgende opdracht worden gebruikt om te verwijderen van de resourcegroep en alle resources die zijn gekoppeld.
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/scale-mysql-server/delete-mysql.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Script uitleg
Dit script maakt gebruik van de volgende opdrachten. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| **Opdracht** | **Opmerkingen bij de** |
|---|---|
| [AZ groep maken](/cli/azure/group#create) | Maakt een resourcegroep waarin alle resources worden opgeslagen. |
| [AZ mysql-server maken](/cli/azure/mysql/server#create) | Hiermee maakt u een MySQL-server die als host fungeert voor de databases. |
| [lijst met AZ monitor metrische gegevens](/cli/azure/monitor/metrics#list) | Lijst van de metrische waarde voor de bronnen. |
| [AZ groep verwijderen](/cli/azure/group#delete) | Hiermee verwijdert u een resourcegroep met inbegrip van alle ingesloten resources. |

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over de Azure CLI: [documentatie van Azure CLI](/cli/azure/overview).
- Probeer extra scripts: [voorbeelden van Azure CLI voor Azure-Database voor MySQL](../sample-scripts-azure-cli.md)
- Zie voor meer informatie over het schalen [Servicelagen](../concepts-service-tiers.md) en [Compute-eenheden en opslageenheden](../concepts-compute-unit-and-storage.md).
