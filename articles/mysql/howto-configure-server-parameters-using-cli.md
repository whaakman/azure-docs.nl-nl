---
title: Configureren van de parameters van de service in Azure Database for MySQL
description: In dit artikel wordt beschreven hoe u de parameters van de service configureren in Azure Database voor MySQL met behulp van het opdrachtregelprogramma van Azure CLI.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.devlang: azurecli
ms.topic: conceptual
ms.date: 07/18/2018
ms.openlocfilehash: b0d7bbdc3e1dcad6f6cecb57b15e2e5df6b3fd28
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/17/2018
ms.locfileid: "53534890"
---
# <a name="customize-server-configuration-parameters-by-using-azure-cli"></a>Parameters voor serverconfiguratie aanpassen met behulp van Azure CLI
U kunt lijst, weergeven en bijwerken van parameters voor de configuratie voor een Azure Database for MySQL-server met behulp van Azure CLI, het Azure-opdrachtregelprogramma. Een subset van de engine-configuratie wordt weergegeven op het niveau van de server en kan worden gewijzigd. 

## <a name="prerequisites"></a>Vereisten
Als u wilt in deze gebruiksaanwijzing kunt doorlopen, hebt u het volgende nodig:
- [Een Azure Database for MySQL-server](quickstart-create-mysql-server-database-using-azure-cli.md)
- [Azure CLI](/cli/azure/install-azure-cli) opdrachtregelprogramma of gebruik de Azure Cloud Shell in de browser.

## <a name="list-server-configuration-parameters-for-azure-database-for-mysql-server"></a>Lijst met parameters voor serverconfiguratie voor Azure Database voor MySQL-server
Als u alle parameters van kan worden gewijzigd in een server en de bijbehorende waarden, voer de [az mysql server configuration list](/cli/azure/mysql/server/configuration#az-mysql-server-configuration-list) opdracht.

U kunt de parameters voor serverconfiguratie voor de server een lijst **mydemoserver.mysql.database.azure.com** onder de resourcegroep **myresourcegroup**.
```azurecli-interactive
az mysql server configuration list --resource-group myresourcegroup --server mydemoserver
```
Zie de sectie van de MySQL-documentatie op voor de definitie van elk van de vermelde parameters [Server systeemvariabelen](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html).

## <a name="show-server-configuration-parameter-details"></a>Details van de parameter voor de serverconfiguratie van de weergeven
Uitvoeren als u wilt weergeven van details over een bepaalde configuratieparameter voor een server, de [az mysql server configuration show](/cli/azure/mysql/server/configuration#az-mysql-server-configuration-show) opdracht.

In dit voorbeeld worden details van de **trage\_query\_log** configuratieparameter van de server voor server **mydemoserver.mysql.database.azure.com** onder de resourcegroep **myresourcegroup.**
```azurecli-interactive
az mysql server configuration show --name slow_query_log --resource-group myresourcegroup --server mydemoserver
```
## <a name="modify-a-server-configuration-parameter-value"></a>De parameterwaarde van een server-configuratie wijzigen
U kunt ook de waarde van een bepaalde server configuratieparameter, die de onderliggende configuratiewaarde voor de MySQL-server-engine-updates wijzigen. Voor het bijwerken van de configuratie, gebruikt u de [az mysql server configuratieset](/cli/azure/mysql/server/configuration#az-mysql-server-configuration-set) opdracht. 

Om bij te werken de **trage\_query\_log** server configuratieparameter van server **mydemoserver.mysql.database.azure.com** onder de resourcegroep  **myresourcegroup.**
```azurecli-interactive
az mysql server configuration set --name slow_query_log --resource-group myresourcegroup --server mydemoserver --value ON
```
Als u instellen van de waarde van een configuratieparameter wilt, laat de optionele `--value` parameter, de service is van toepassing op de standaardwaarde. Voor het bovenstaande voorbeeld deze zou er als volgt uitzien:
```azurecli-interactive
az mysql server configuration set --name slow_query_log --resource-group myresourcegroup --server mydemoserver
```
Deze code stelt de **trage\_query\_log** configuratie op de standaardwaarde **OFF**. 

## <a name="working-with-the-time-zone-parameter"></a>Werken met de parameter tijdzone

### <a name="populating-the-time-zone-tables"></a>Invullen van de tijdzone-tabellen

De tijdzone-tabellen op de server kunnen worden gevuld door het aanroepen van de `az_load_timezone` opgeslagen procedure van een hulpprogramma zoals de MySQL-opdrachtregel of MySQL Workbench.

> [!NOTE]
> Als u werkt met de `az_load_timezone` opdracht via MySQL Workbench, moet u de veilige modus eerst uitschakelen met behulp van `SET SQL_SAFE_UPDATES=0;`.

```sql
CALL mysql.az_load_timezone();
```

Als u wilt weergeven van waarden van de beschikbare tijd zone, moet u de volgende opdracht uitvoeren:

```sql
SELECT name FROM mysql.time_zone_name;
```

### <a name="setting-the-global-level-time-zone"></a>De globale niveau tijdzone instellen

De globale niveau tijdzone kan worden ingesteld met behulp van de [az mysql server configuratieset](/cli/azure/mysql/server/configuration#az-mysql-server-configuration-set) opdracht.

De volgende opdracht updates de **tijd\_zone** server configuratieparameter van server **mydemoserver.mysql.database.azure.com** onder de resourcegroep  **myresourcegroup** naar **VS / Stille Oceaan**.

```azurecli-interactive
az mysql server configuration set --name time_zone --resource-group myresourcegroup --server mydemoserver --value "US/Pacific"
```

### <a name="setting-the-session-level-time-zone"></a>De sessie niveau tijdzone instellen

De sessie niveau time zone kan worden ingesteld door het uitvoeren van de `SET time_zone` opdracht uit vanaf een hulpprogramma zoals de MySQL-opdrachtregel of MySQL Workbench. In het volgende voorbeeld wordt de tijdzone ingesteld op de **VS / Stille Oceaan** tijdzone.  

```sql
SET time_zone = 'US/Pacific';
```

Raadpleeg de MySQL-documentatie voor [datum- en tijdfuncties](https://dev.mysql.com/doc/refman/5.7/en/date-and-time-functions.html#function_convert-tz).


## <a name="next-steps"></a>Volgende stappen

- Het configureren van [parameters van de server in Azure portal](howto-server-parameters.md)