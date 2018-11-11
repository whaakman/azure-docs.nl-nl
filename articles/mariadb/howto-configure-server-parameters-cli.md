---
title: Configureren van de parameters van de service in Azure Database voor MariaDB
description: In dit artikel wordt beschreven hoe u de parameters van de service configureren in Azure Database voor MariaDB met behulp van het opdrachtregelprogramma van Azure CLI.
services: mariadb
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mariadb
ms.devlang: azure-cli
ms.topic: article
ms.date: 11/09/2018
ms.openlocfilehash: 4e391ce56b31f35da67b3d975bbc1290c17cb139
ms.sourcegitcommit: 5a1d601f01444be7d9f405df18c57be0316a1c79
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/10/2018
ms.locfileid: "51516176"
---
# <a name="customize-server-configuration-parameters-by-using-azure-cli"></a>Parameters voor serverconfiguratie aanpassen met behulp van Azure CLI
U kunt weergeven, weergeven en parameters voor de configuratie voor een Azure Database voor MariaDB-server bijwerken met behulp van Azure CLI, het Azure-opdrachtregelprogramma. Een subset van de engine-configuratie wordt weergegeven op het niveau van de server en kan worden gewijzigd.

## <a name="prerequisites"></a>Vereisten
Als u wilt in deze gebruiksaanwijzing kunt doorlopen, hebt u het volgende nodig:
- [Een Azure Database voor MariaDB-server](quickstart-create-mariadb-server-database-using-azure-cli.md)
- [Azure CLI](/cli/azure/install-azure-cli) opdrachtregelprogramma of gebruik de Azure Cloud Shell in de browser.

## <a name="list-server-configuration-parameters-for-azure-database-for-mariadb-server"></a>Lijst met parameters voor serverconfiguratie voor de Azure Database voor MariaDB-server
Als u alle parameters van kan worden gewijzigd in een server en de bijbehorende waarden, voer de [az mariadb-serverlijst configuratie](/cli/azure/mariadb/server/configuration#az-mariadb-server-configuration-list) opdracht.

U kunt de parameters voor serverconfiguratie voor de server een lijst **mydemoserver.mariadb.database.azure.com** onder de resourcegroep **myresourcegroup**.
```azurecli-interactive
az mariadb server configuration list --resource-group myresourcegroup --server mydemoserver
```

Zie de sectie MariaDB-documentatie op voor de definitie van elk van de vermelde parameters [Server systeemvariabelen](https://mariadb.com/kb/en/library/server-system-variables/).

## <a name="show-server-configuration-parameter-details"></a>Details van de parameter voor de serverconfiguratie van de weergeven
Uitvoeren als u wilt weergeven van details over een bepaalde configuratieparameter voor een server, de [az mariadb server configuration show](/cli/azure/mariadb/server/configuration#az-mariadb-server-configuration-show) opdracht.

In dit voorbeeld worden details van de **trage\_query\_log** configuratieparameter van de server voor server **mydemoserver.mariadb.database.azure.com** onder de resourcegroep **myresourcegroup.**
```azurecli-interactive
az mariadb server configuration show --name slow_query_log --resource-group myresourcegroup --server mydemoserver
```

## <a name="modify-a-server-configuration-parameter-value"></a>De parameterwaarde van een server-configuratie wijzigen
U kunt ook de waarde van een bepaalde server configuratieparameter, die de onderliggende configuratiewaarde voor de MariaDB-server-engine-updates wijzigen. Voor het bijwerken van de configuratie, gebruikt u de [az mariadb server configuratieset](/cli/azure/mariadb/server/configuration#az-mariadb-server-configuration-set) opdracht. 

Om bij te werken de **trage\_query\_log** server configuratieparameter van server **mydemoserver.mariadb.database.azure.com** onder de resourcegroep  **myresourcegroup.**
```azurecli-interactive
az mariadb server configuration set --name slow_query_log --resource-group myresourcegroup --server mydemoserver --value ON
```

Als u instellen van de waarde van een configuratieparameter wilt, laat de optionele `--value` parameter, de service is van toepassing op de standaardwaarde. Voor het bovenstaande voorbeeld deze zou er als volgt uitzien:
```azurecli-interactive
az mariadb server configuration set --name slow_query_log --resource-group myresourcegroup --server mydemoserver
```

Deze code stelt de **trage\_query\_log** configuratie op de standaardwaarde **OFF**. 

## <a name="working-with-the-time-zone-parameter"></a>Werken met de parameter tijdzone

### <a name="populating-the-time-zone-tables"></a>Invullen van de tijdzone-tabellen

De tijdzone-tabellen op de server kunnen worden gevuld door het aanroepen van de `az_load_timezone` opgeslagen procedure van een hulpprogramma zoals de MariaDB vanaf de opdrachtregel of MariaDB-Workbench.

> [!NOTE]
> Als u werkt met de `az_load_timezone` opdracht uit vanaf MariaDB Workbench, moet u de veilige modus eerst uitschakelen met behulp van `SET SQL_SAFE_UPDATES=0;`.

```sql
CALL mysql.az_load_timezone();
```

Als u wilt weergeven van waarden van de beschikbare tijd zone, moet u de volgende opdracht uitvoeren:

```sql
SELECT name FROM mysql.time_zone_name;
```

### <a name="setting-the-global-level-time-zone"></a>De globale niveau tijdzone instellen

De globale niveau tijdzone kan worden ingesteld met behulp van de [az mariadb server configuratieset](/cli/azure/mariadb/server/configuration#az-mariadb-server-configuration-set) opdracht.

De volgende opdracht updates de **tijd\_zone** server configuratieparameter van server **mydemoserver.mariadb.database.azure.com** onder de resourcegroep  **myresourcegroup** naar **VS / Stille Oceaan**.

```azurecli-interactive
az mariadb server configuration set --name time_zone --resource-group myresourcegroup --server mydemoserver --value "US/Pacific"
```

### <a name="setting-the-session-level-time-zone"></a>De sessie niveau tijdzone instellen

De sessie niveau time zone kan worden ingesteld door het uitvoeren van de `SET time_zone` opdracht uit vanaf een hulpprogramma zoals de MariaDB vanaf de opdrachtregel of MariaDB-Workbench. In het volgende voorbeeld wordt de tijdzone ingesteld op de **VS / Stille Oceaan** tijdzone.  

```sql
SET time_zone = 'US/Pacific';
```

Raadpleeg de documentatie MariaDB voor [datum- en tijdfuncties](https://mariadb.com/kb/en/library/date-time-functions/).

## <a name="next-steps"></a>Volgende stappen

- Het configureren van [parameters van de server in Azure portal](howto-server-parameters.md)