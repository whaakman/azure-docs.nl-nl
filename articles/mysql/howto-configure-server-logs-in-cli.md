---
title: Logboeken Access-server in Azure-Database voor MySQL met Azure CLI | Microsoft Docs
description: Dit artikel wordt beschreven hoe u toegang tot de server-Logboeken in Azure-Database voor MySQL met het opdrachtregelprogramma Azure CLI.
services: mysql
author: rachel-msft
ms.author: raagyema
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: article
ms.date: 11/28/2017
ms.openlocfilehash: 908f28d8bd3d0dcbd03636e69cd47b5c47f3cfde
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2017
---
# <a name="configure-and-access-server-logs-using-azure-cli"></a>Configureren en toegang tot de server-logboeken met Azure CLI
U kunt de Azure-Database voor MySQL server-logboeken met de Azure CLI, Azure opdrachtregelprogramma downloaden.

## <a name="prerequisites"></a>Vereisten
Stap in deze handleiding instructies, wilt u het volgende nodig:
- [Azure-Database voor de MySQL-server](quickstart-create-mysql-server-database-using-azure-cli.md)
- De [Azure CLI 2.0](/cli/azure/install-azure-cli) of de Azure-Cloud-Shell gebruiken in de browser.

## <a name="configure-logging-for-azure-database-for-mysql"></a>Logboekregistratie voor Azure-Database voor MySQL configureren
U kunt de server voor toegang tot de trage querylogboek MySQL configureren.
1. Logboekregistratie inschakelen door in te stellen **trage\_query\_logboek** parameter op ON.
2. Andere parameters zoals aanpassen **lang\_query\_tijd** en **logboek\_trage\_admin\_instructies**.

Zie [serverparameters configureren hoe](howto-configure-server-parameters-using-cli.md) voor informatie over het instellen van de waarde van deze parameters via de Azure CLI.

Bijvoorbeeld de volgende opdracht in de CLI Hiermee schakelt u het trage querylogboek, de lang Querytijd ingesteld op 10 seconden en schakelt de logboekregistratie van de langzaam admin-instructie. Ten slotte geeft de configuratieopties voor uw beoordeling.
```azurecli-interactive
az mysql server configuration set --name slow_query_log --resource-group myresourcegroup --server myserver4demo --value ON
az mysql server configuration set --name long_query_time --resource-group myresourcegroup --server myserver4demo --value 10
az mysql server configuration set --name log_slow_admin_statements --resource-group myresourcegroup --server myserver4demo --value OFF
az mysql server configuration list --resource-group myresourcegroup --server myserver4demo
```

## <a name="list-logs-for-azure-database-for-mysql-server"></a>Logboeken van de lijst voor de Azure-Database voor de MySQL-server
Uitvoeren als de beschikbare logboekbestanden voor uw server wilt weergeven, de [az mysql serverlogboeken lijst](/cli/azure/mysql/server-logs#az_mysql_server_logs_list) opdracht.

U kunt geven lijsten van logboekbestanden voor server **myserver4demo.mysql.database.azure.com** onder de resourcegroep **myresourcegroup**, en het omleiden naar een tekstbestand aangeroepen **logboek\_bestanden \_lijst.txt.**
```azurecli-interactive
az mysql server-logs list --resource-group myresourcegroup --server myserver4demo > log_files_list.txt
```
## <a name="download-logs-from-the-server"></a>Logboeken van de server downloaden
De [az mysql serverlogboeken downloaden](/cli/azure/mysql/server-logs#az_mysql_server_logs_download) opdracht kunt u afzonderlijke logboekbestanden voor uw server downloaden. 

In dit voorbeeld downloadt de specifiek logboekbestand voor de server **myserver4demo.mysql.database.azure.com** onder de resourcegroep **myresourcegroup** op uw lokale omgeving.
```azurecli-interactive
az mysql server-logs download --name 20170414-myserver4demo-mysql.log --resource-group myresourcegroup --server myserver4demo
```

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [serverlogboekbestanden in Azure-Database voor MySQL](concepts-server-logs.md)
