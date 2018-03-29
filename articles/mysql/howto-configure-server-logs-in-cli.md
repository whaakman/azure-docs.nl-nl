---
title: Toegang tot logboeken-server in Azure-Database voor MySQL met Azure CLI
description: Dit artikel wordt beschreven hoe u toegang tot de server-Logboeken in Azure-Database voor MySQL met het opdrachtregelprogramma Azure CLI.
services: mysql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 037f631b3b23e66f1ed54106cc5be14a809b2e22
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2018
---
# <a name="configure-and-access-server-logs-by-using-azure-cli"></a>Configureren en server-Logboeken openen met behulp van Azure CLI
U kunt de Azure-Database voor MySQL server-logboeken downloaden met behulp van Azure CLI, het Azure-opdrachtregelprogramma.

## <a name="prerequisites"></a>Vereisten
Stap in deze handleiding instructies, wilt u het volgende nodig:
- [Azure-Database voor de MySQL-server](quickstart-create-mysql-server-database-using-azure-cli.md)
- De [Azure CLI 2.0](/cli/azure/install-azure-cli) of Azure Cloud Shell in de browser

## <a name="configure-logging-for-azure-database-for-mysql"></a>Logboekregistratie voor Azure-Database voor MySQL configureren
U kunt de server om het logboek van de query wordt vertraagd MySQL door het volgende doen om toegang te configureren:
1. Logboekregistratie inschakelen door het instellen van de **trage\_query\_logboek** parameter op ON.
2. Andere parameters, zoals aanpassen **lang\_query\_tijd** en **logboek\_trage\_admin\_instructies**.

Zie voor meer informatie over het instellen van de waarde van deze parameters via Azure CLI, [parameters van de server configureren](howto-configure-server-parameters-using-cli.md). 

Bijvoorbeeld de volgende opdracht in de CLI Hiermee schakelt u het trage querylogboek, de lang Querytijd ingesteld op 10 seconden en vervolgens de registratie van de instructie trage beheerder uitgeschakeld. Ten slotte geeft de configuratieopties voor uw beoordeling.
```azurecli-interactive
az mysql server configuration set --name slow_query_log --resource-group myresourcegroup --server mydemoserver --value ON
az mysql server configuration set --name long_query_time --resource-group myresourcegroup --server mydemoserver --value 10
az mysql server configuration set --name log_slow_admin_statements --resource-group myresourcegroup --server mydemoserver --value OFF
az mysql server configuration list --resource-group myresourcegroup --server mydemoserver
```

## <a name="list-logs-for-azure-database-for-mysql-server"></a>Logboeken van de lijst voor de Azure-Database voor de MySQL-server
Uitvoeren als de beschikbare logboekbestanden voor uw server wilt weergeven, de [az mysql serverlogboeken lijst](/cli/azure/mysql/server-logs#az_mysql_server_logs_list) opdracht.

U kunt geven lijsten van logboekbestanden voor server **mydemoserver.mysql.database.azure.com** onder de resourcegroep **myresourcegroup**. De lijst met logboekbestanden naar een tekstbestand aangeroepen vervolgens omleiden **logboek\_bestanden\_lijst.txt**.
```azurecli-interactive
az mysql server-logs list --resource-group myresourcegroup --server mydemoserver > log_files_list.txt
```
## <a name="download-logs-from-the-server"></a>Logboeken van de server downloaden
Met de [az mysql serverlogboeken downloaden](/cli/azure/mysql/server-logs#az_mysql_server_logs_download) opdracht, kunt u afzonderlijke logboekbestanden voor de server downloaden. 

Gebruik het volgende voorbeeld voor het downloaden van het specifieke logboekbestand voor de server **mydemoserver.mysql.database.azure.com** onder de resourcegroep **myresourcegroup** op uw lokale omgeving.
```azurecli-interactive
az mysql server-logs download --name 20170414-mydemoserver-mysql.log --resource-group myresourcegroup --server mydemoserver
```

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [serverlogboekbestanden in Azure-Database voor MySQL](concepts-server-logs.md).
