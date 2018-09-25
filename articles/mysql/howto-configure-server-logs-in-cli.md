---
title: Access-server-Logboeken in Azure Database for MySQL met behulp van Azure CLI
description: In dit artikel wordt beschreven hoe u toegang krijgen tot de serverlogboeken in Azure Database voor MySQL met behulp van het opdrachtregelprogramma van Azure CLI.
services: mysql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.devlang: azure-cli
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: e3940f07ab97eb2ad03c347c39bf2412a8497f5e
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46978451"
---
# <a name="configure-and-access-server-logs-by-using-azure-cli"></a>Configureren en serverlogboeken openen met behulp van Azure CLI
U kunt de Azure Database for MySQL-serverlogboeken downloaden met behulp van Azure CLI, het Azure-opdrachtregelprogramma.

## <a name="prerequisites"></a>Vereisten
Als u wilt in deze gebruiksaanwijzing kunt doorlopen, hebt u het volgende nodig:
- [Azure Database voor MySQL-server](quickstart-create-mysql-server-database-using-azure-cli.md)
- De [Azure CLI](/cli/azure/install-azure-cli) of Azure Cloud Shell in de browser

## <a name="configure-logging-for-azure-database-for-mysql"></a>Logboekregistratie voor Azure Database voor MySQL configureren
U kunt de server voor toegang tot het MySQL-logboek voor langzame query's door de volgende stappen configureren:
1. Logboekregistratie inschakelen door in te stellen de **trage\_query\_log** parameter op ON.
2. Andere parameters, zoals aanpassen **lang\_query\_tijd** en **log\_trage\_admin\_instructies**.

Zie voor meer informatie over het instellen van de waarde van deze parameters via Azure CLI, [serverparameters configureren](howto-configure-server-parameters-using-cli.md). 

Bijvoorbeeld, de volgende CLI-opdracht Hiermee schakelt u het logboek voor langzame query's, wordt de tijd van lange query ingesteld op 10 seconden en schakelt vervolgens de registratie van de trage admin-instructie. Ten slotte geeft de configuratieopties voor uw beoordeling.
```azurecli-interactive
az mysql server configuration set --name slow_query_log --resource-group myresourcegroup --server mydemoserver --value ON
az mysql server configuration set --name long_query_time --resource-group myresourcegroup --server mydemoserver --value 10
az mysql server configuration set --name log_slow_admin_statements --resource-group myresourcegroup --server mydemoserver --value OFF
az mysql server configuration list --resource-group myresourcegroup --server mydemoserver
```

## <a name="list-logs-for-azure-database-for-mysql-server"></a>Lijst met Logboeken voor Azure Database voor MySQL-server
Uitvoeren als u de beschikbare logboekbestanden voor uw server, de [az mysql server-logs list](/cli/azure/mysql/server-logs#az-mysql-server-logs-list) opdracht.

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
- Meer informatie over [serverlogboeken in Azure Database for MySQL](concepts-server-logs.md).
