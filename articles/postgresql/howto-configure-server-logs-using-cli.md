---
title: Configureren en serverlogboeken openen voor PostgreSQL met behulp van Azure CLI
description: Dit artikel wordt beschreven hoe u configureert en toegang krijgen tot de serverlogboeken in Azure Database voor PostgreSQL met behulp van de Azure CLI-opdrachtregel.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: af5a0843acced4611b219088b7662f9ea4b32beb
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46969577"
---
# <a name="configure-and-access-server-logs-by-using-azure-cli"></a>Configureren en serverlogboeken openen met behulp van Azure CLI
U kunt de foutenlogboeken van de PostgreSQL-server downloaden via de opdrachtregelinterface (Azure CLI). Echter, toegang tot de transactielogboeken wordt niet ondersteund. 

## <a name="prerequisites"></a>Vereisten
Als u wilt in deze gebruiksaanwijzing kunt doorlopen, hebt u het volgende nodig:
- [Azure Database for PostgreSQL-server](quickstart-create-server-database-azure-cli.md)
- De [Azure CLI](/cli/azure/install-azure-cli) opdrachtregelprogramma of Azure Cloud Shell in de browser

## <a name="configure-logging-for-azure-database-for-postgresql"></a>Logboekregistratie voor Azure Database voor PostgreSQL configureren
U kunt de server voor toegang tot logboeken voor query's en foutenlogboeken configureren. Foutenlogboeken kunnen automatisch onderdruk-, verbindings-en controlepunt hebben.
1. Logboekregistratie inschakelen.
2. U schakelt de logboekregistratie voor query bijwerken **log\_instructie** en **log\_min\_duur\_instructie**.
3. Bewaarperiode voor bijwerken.

Zie voor meer informatie, [parameters voor serverconfiguratie aanpassen](howto-configure-server-parameters-using-cli.md).

## <a name="list-logs-for-azure-database-for-postgresql-server"></a>Lijst met Logboeken voor Azure Database for PostgreSQL-server
Uitvoeren als u de beschikbare logboekbestanden voor uw server, de [az postgres server-logs list](/cli/azure/postgres/server-logs#az_postgres_server_logs_list) opdracht.

U kunt een lijst de logboekbestanden voor server **mydemoserver.postgres.database.azure.com** onder de resourcegroep **myresourcegroup**. De lijst met logboekbestanden naar een tekstbestand met de naam vervolgens direct **log\_bestanden\_lijst.txt**.
```azurecli-interactive
az postgres server-logs list --resource-group myresourcegroup --server mydemoserver > log_files_list.txt
```
## <a name="download-logs-locally-from-the-server"></a>Logboeken van de server lokaal downloaden
Met de [az postgres server-logs download](/cli/azure/postgres/server-logs#az_postgres_server_logs_download) opdracht, kunt u afzonderlijke logboekbestanden voor de server downloaden. 

Gebruik het volgende voorbeeld voor het downloaden van het specifieke logboekbestand voor de server **mydemoserver.postgres.database.azure.com** onder de resourcegroep **myresourcegroup** op uw lokale omgeving.
```azurecli-interactive
az postgres server-logs download --name 20170414-mydemoserver-postgresql.log --resource-group myresourcegroup --server mydemoserver
```
## <a name="next-steps"></a>Volgende stappen
- Zie voor meer informatie over de serverlogboeken [serverlogboeken in Azure Database for PostgreSQL](concepts-server-logs.md).
- Zie voor meer informatie over serverparameters [met behulp van Azure CLI parameters voor serverconfiguratie aanpassen](howto-configure-server-parameters-using-cli.md).
