---
title: Langzame query's voor toegang tot logboeken in Azure Database voor MySQL met behulp van Azure CLI
description: In dit artikel wordt beschreven hoe u toegang krijgen tot de logboeken voor langzame query's in Azure Database voor MySQL met behulp van de Azure CLI.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.devlang: azurecli
ms.topic: conceptual
ms.date: 06/12/2019
ms.openlocfilehash: e6d25a4d8b470580626cab4a84f9d912a3f79f75
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2019
ms.locfileid: "67612649"
---
# <a name="configure-and-access-slow-query-logs-by-using-azure-cli"></a>Configureren en toegang tot logboeken voor langzame query's met behulp van Azure CLI
U kunt de Azure Database for MySQL langzame-querylogboeken downloaden met behulp van Azure CLI, het Azure-opdrachtregelprogramma.

## <a name="prerequisites"></a>Vereisten
Als u wilt in deze gebruiksaanwijzing kunt doorlopen, hebt u het volgende nodig:
- [Azure Database voor MySQL-server](quickstart-create-mysql-server-database-using-azure-cli.md)
- De [Azure CLI](/cli/azure/install-azure-cli) of Azure Cloud Shell in de browser

## <a name="configure-logging"></a>Logboekregistratie configureren
U kunt de server voor toegang tot het MySQL-logboek voor langzame query's door de volgende stappen configureren:
1. Logboekregistratie voor langzame query's inschakelen door in te stellen de **trage\_query\_log** parameter op ON.
2. Andere parameters, zoals aanpassen **lang\_query\_tijd** en **log\_trage\_admin\_instructies**.

Zie voor meer informatie over het instellen van de waarde van deze parameters via Azure CLI, [serverparameters configureren](howto-configure-server-parameters-using-cli.md).

Bijvoorbeeld, de volgende CLI-opdracht Hiermee schakelt u het logboek voor langzame query's, wordt de tijd van lange query ingesteld op 10 seconden en schakelt vervolgens de registratie van de trage admin-instructie. Ten slotte geeft de configuratieopties voor uw beoordeling.
```azurecli-interactive
az mysql server configuration set --name slow_query_log --resource-group myresourcegroup --server mydemoserver --value ON
az mysql server configuration set --name long_query_time --resource-group myresourcegroup --server mydemoserver --value 10
az mysql server configuration set --name log_slow_admin_statements --resource-group myresourcegroup --server mydemoserver --value OFF
az mysql server configuration list --resource-group myresourcegroup --server mydemoserver
```

## <a name="list-logs-for-azure-database-for-mysql-server"></a>Lijst met Logboeken voor Azure Database voor MySQL-server
Uitvoeren als u de logboekbestanden van de beschikbare langzame query's voor uw server, de [az mysql server-logs list](/cli/azure/mysql/server-logs#az-mysql-server-logs-list) opdracht.

U kunt een lijst de logboekbestanden voor server **mydemoserver.mysql.database.azure.com** onder de resourcegroep **myresourcegroup**. De lijst met logboekbestanden naar een tekstbestand met de naam vervolgens direct **log\_bestanden\_lijst.txt**.
```azurecli-interactive
az mysql server-logs list --resource-group myresourcegroup --server mydemoserver > log_files_list.txt
```
## <a name="download-logs-from-the-server"></a>Logboeken downloaden die van de server
Met de [az mysql server-logs download](/cli/azure/mysql/server-logs#az-mysql-server-logs-download) opdracht, kunt u afzonderlijke logboekbestanden voor de server downloaden. 

Gebruik het volgende voorbeeld voor het downloaden van het specifieke logboekbestand voor de server **mydemoserver.mysql.database.azure.com** onder de resourcegroep **myresourcegroup** op uw lokale omgeving.
```azurecli-interactive
az mysql server-logs download --name 20170414-mydemoserver-mysql.log --resource-group myresourcegroup --server mydemoserver
```

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [logboeken van de langzame query's in Azure Database voor MySQL](concepts-server-logs.md).
