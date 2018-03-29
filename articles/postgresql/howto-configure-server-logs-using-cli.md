---
title: Configureren en server-logboeken voor PostgreSQL openen met behulp van Azure CLI
description: Dit artikel wordt beschreven hoe u kunt configureren en toegang krijgen tot de server-Logboeken in Azure-Database voor PostgreSQL via de opdrachtregel van Azure CLI.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 951dcca562c08698b4ce4528d005fc91152ea337
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2018
---
# <a name="configure-and-access-server-logs-by-using-azure-cli"></a>Configureren en server-Logboeken openen met behulp van Azure CLI
U kunt de foutenlogboeken voor PostgreSQL-server downloaden via de opdrachtregelinterface (Azure CLI). Echter, de toegang tot de transactielogboeken wordt niet ondersteund. 

## <a name="prerequisites"></a>Vereisten
Stap in deze handleiding instructies, wilt u het volgende nodig:
- [Azure-Database voor PostgreSQL-server](quickstart-create-server-database-azure-cli.md)
- De [Azure CLI 2.0](/cli/azure/install-azure-cli) opdrachtregelprogramma of Azure Cloud Shell in de browser

## <a name="configure-logging-for-azure-database-for-postgresql"></a>Logboekregistratie voor Azure-Database voor PostgreSQL configureren
U kunt de server voor toegang tot de querylogboeken van de en foutenlogboeken configureren. Foutenlogboeken kunnen automatische onderdruk-, verbindings-en controlepunt hebben.
1. Logboekregistratie inschakelen.
2. Bijwerken zodat queryregistratie **logboek\_instructie** en **logboek\_min\_duur\_instructie**.
3. Bewaarperiode bijwerken.

Zie voor meer informatie [configuratieparameters server aanpassen](howto-configure-server-parameters-using-cli.md).

## <a name="list-logs-for-azure-database-for-postgresql-server"></a>Logboeken van de lijst voor de Azure-Database voor PostgreSQL-server
Uitvoeren als de beschikbare logboekbestanden voor uw server wilt weergeven, de [az postgres serverlogboeken lijst](/cli/azure/postgres/server-logs#az_postgres_server_logs_list) opdracht.

U kunt geven lijsten van logboekbestanden voor server **mydemoserver.postgres.database.azure.com** onder de resourcegroep **myresourcegroup**. De lijst met logboekbestanden naar een tekstbestand aangeroepen vervolgens omleiden **logboek\_bestanden\_lijst.txt**.
```azurecli-interactive
az postgres server-logs list --resource-group myresourcegroup --server mydemoserver > log_files_list.txt
```
## <a name="download-logs-locally-from-the-server"></a>Lokaal logboeken downloaden van de server
Met de [az postgres serverlogboeken downloaden](/cli/azure/postgres/server-logs#az_postgres_server_logs_download) opdracht, kunt u afzonderlijke logboekbestanden voor de server downloaden. 

Gebruik het volgende voorbeeld voor het downloaden van het specifieke logboekbestand voor de server **mydemoserver.postgres.database.azure.com** onder de resourcegroep **myresourcegroup** op uw lokale omgeving.
```azurecli-interactive
az postgres server-logs download --name 20170414-mydemoserver-postgresql.log --resource-group myresourcegroup --server mydemoserver
```
## <a name="next-steps"></a>Volgende stappen
- Zie voor meer informatie over de serverlogboeken, [serverlogboekbestanden in Azure-Database voor PostgreSQL](concepts-server-logs.md).
- Zie voor meer informatie over parameters van de server [aanpassen configuratieparameters server met Azure CLI](howto-configure-server-parameters-using-cli.md).
