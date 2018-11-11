---
title: Access-server-Logboeken in Azure Database voor MariaDB met behulp van Azure CLI
description: In dit artikel wordt beschreven hoe u toegang krijgen tot de serverlogboeken in Azure Database voor MariaDB met behulp van het opdrachtregelprogramma van Azure CLI.
services: mariadb
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: mariadb
ms.devlang: azure-cli
ms.topic: article
ms.date: 11/10/2018
ms.openlocfilehash: b9ae07b88164a830598db791d61b77a6abb7b1df
ms.sourcegitcommit: 5a1d601f01444be7d9f405df18c57be0316a1c79
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/10/2018
ms.locfileid: "51516173"
---
# <a name="configure-and-access-server-logs-by-using-azure-cli"></a>Configureren en serverlogboeken openen met behulp van Azure CLI
U kunt de Azure Database voor MariaDB serverlogboeken downloaden met behulp van Azure CLI, het Azure-opdrachtregelprogramma.

## <a name="prerequisites"></a>Vereisten
Als u wilt in deze gebruiksaanwijzing kunt doorlopen, hebt u het volgende nodig:
- [Azure Database voor MariaDB-server](quickstart-create-mariadb-server-database-using-azure-cli.md)
- De [Azure CLI](/cli/azure/install-azure-cli) of Azure Cloud Shell in de browser

## <a name="configure-logging-for-azure-database-for-mariadb"></a>Configureer de logboekregistratie voor Azure Database voor MariaDB
U kunt de server voor toegang tot het MariaDB-logboek voor langzame query's door de volgende stappen configureren:
1. Logboekregistratie inschakelen door in te stellen de **trage\_query\_log** parameter op ON.
2. Andere parameters, zoals aanpassen **lang\_query\_tijd** en **log\_trage\_admin\_instructies**.

Zie voor meer informatie over het instellen van de waarde van deze parameters via Azure CLI, [serverparameters configureren](howto-configure-server-parameters-cli.md).

Bijvoorbeeld, de volgende CLI-opdracht Hiermee schakelt u het logboek voor langzame query's, wordt de tijd van lange query ingesteld op 10 seconden en schakelt vervolgens de registratie van de trage admin-instructie. Ten slotte geeft de configuratieopties voor uw beoordeling.
```azurecli-interactive
az mariadb server configuration set --name slow_query_log --resource-group myresourcegroup --server mydemoserver --value ON
az mariadb server configuration set --name long_query_time --resource-group myresourcegroup --server mydemoserver --value 10
az mariadb server configuration set --name log_slow_admin_statements --resource-group myresourcegroup --server mydemoserver --value OFF
az mariadb server configuration list --resource-group myresourcegroup --server mydemoserver
```

## <a name="list-logs-for-azure-database-for-mariadb-server"></a>Lijst met Logboeken voor Azure Database voor MariaDB-server
Uitvoeren als u de beschikbare logboekbestanden voor uw server, de [az mariadb-server-logs list](/cli/azure/mariadb/server-logs#az-mariadb-server-logs-list) opdracht.

U kunt een lijst de logboekbestanden voor server **mydemoserver.mariadb.database.azure.com** onder de resourcegroep **myresourcegroup**. De lijst met logboekbestanden naar een tekstbestand met de naam vervolgens direct **log\_bestanden\_lijst.txt**.
```azurecli-interactive
az mariadb server-logs list --resource-group myresourcegroup --server mydemoserver > log_files_list.txt
```
## <a name="download-logs-from-the-server"></a>Logboeken downloaden die van de server
Met de [az mariadb-server-logs download](/cli/azure/mariadb/server-logs#az-mariadb-server-logs-download) opdracht, kunt u afzonderlijke logboekbestanden voor de server downloaden.

Gebruik het volgende voorbeeld voor het downloaden van het specifieke logboekbestand voor de server **mydemoserver.mariadb.database.azure.com** onder de resourcegroep **myresourcegroup** op uw lokale omgeving.
```azurecli-interactive
az mariadb server-logs download --name mysql-slow-mydemoserver-2018110800.log --resource-group myresourcegroup --server mydemoserver
```

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [serverlogboeken in Azure Database voor MariaDB](concepts-server-logs.md).
