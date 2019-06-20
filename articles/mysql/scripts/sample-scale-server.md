---
title: 'Azure CLI-script: schaal van een Azure Database for MySQL-server aanpassen'
description: Met dit CLI-voorbeeldscript wordt de schaal van een Azure Database for MySQL-server aangepast naar een ander prestatieniveau na het uitvoeren van query's op de metrische gegevens.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.devlang: azurecli
ms.topic: sample
ms.custom: mvc
ms.date: 04/05/2018
ms.openlocfilehash: 63bd92aeea0ad85872c1165eab374e90de078d44
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67275138"
---
# <a name="monitor-and-scale-an-azure-database-for-mysql-server-using-azure-cli"></a>Een Azure Database for MySQL-server bewaken en de schaal ervan aanpassen met Azure CLI
Met dit CLI-voorbeeldscript wordt de schaal van één Azure Database for MySQL-server aangepast naar een ander prestatieniveau na het uitvoeren van query's op de metrische gegevens.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal uit te voeren, moet u voor dit artikel gebruikmaken van Azure CLI-versie 2.0 of hoger. Controleer de versie door `az --version` uit te voeren. Zie [Azure CLI installeren]( /cli/azure/install-azure-cli) voor het installeren of upgraden van uw versie van Azure CLI. 

## <a name="sample-script"></a>Voorbeeldscript
Bewerk in dit voorbeeldscript de gemarkeerde regels om de gebruikersnaam en het wachtwoord van de beheerder naar uw eigen bij te werken. Vervang de abonnement-ID die wordt gebruikt in de `az monitor` opdrachten door uw eigen abonnement-id.  
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/scale-mysql-server/scale-mysql-server.sh?highlight=15-16 "Create and scale Azure Database for MySQL.")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie
Gebruik de volgende opdracht om de resourcegroep en alle resources die er aan zijn gekoppeld te verwijderen nadat het script is uitgevoerd. 
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/scale-mysql-server/delete-mysql.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Uitleg van het script
Dit script maakt gebruik van de opdrachten die in de volgende tabel worden weergegeven:

| **Opdracht** | **Opmerkingen** |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Hiermee wordt een resourcegroep gemaakt waarin alle resources worden opgeslagen. |
| [az mysql server create](/cli/azure/mysql/server#az-mysql-server-create) | Hiermee wordt een MySQL-server gemaakt waar de databases worden gehost. |
| [az monitor metrics list](/cli/azure/monitor/metrics#az-monitor-metrics-list) | Geeft de metrische waarde weer voor de resources. |
| [az group delete](/cli/azure/group#az-group-delete) | Hiermee verwijdert u een resourcegroep met inbegrip van alle geneste resources. |

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over Azure CLI: [Azure CLI-documentatie](/cli/azure).
- Aanvullende scripts proberen: [Azure CLI-voorbeelden voor Azure Database for MySQL](../sample-scripts-azure-cli.md)
- Zie [Servicelagen](../concepts-service-tiers.md) en [Rekeneenheden en opslageenheden](../concepts-compute-unit-and-storage.md) voor meer informatie over de schaal aanpassen.
