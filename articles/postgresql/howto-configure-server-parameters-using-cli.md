---
title: Configureren van de parameters van de service in Azure Database for PostgreSQL
description: In dit artikel wordt beschreven hoe u de parameters van de service configureren in Azure Database for PostgreSQL via de opdrachtregel van Azure CLI.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 5520c08d2bf5dba85ece1de0bca7329286625911
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46968047"
---
# <a name="customize-server-configuration-parameters-using-azure-cli"></a>Met behulp van Azure CLI parameters voor serverconfiguratie aanpassen
U kunt weergeven, weergeven en bijwerken van parameters voor de configuratie voor een Azure PostgreSQL-server met behulp van de opdrachtregelinterface (Azure CLI). Een subset van de engine configuraties op niveau van de server wordt weergegeven en kan worden gewijzigd. 

## <a name="prerequisites"></a>Vereisten
Als u wilt in deze gebruiksaanwijzing kunt doorlopen, hebt u het volgende nodig:
- Een Azure Database for PostgreSQL-server en database maken door de volgende [een Azure Database for PostgreSQL maken](quickstart-create-server-database-azure-cli.md)
- Installeer [Azure CLI](/cli/azure/install-azure-cli) opdrachtregelinterface op uw computer of gebruik de [Azure Cloud Shell](../cloud-shell/overview.md) in Azure portal met uw browser.

## <a name="list-server-configuration-parameters-for-azure-database-for-postgresql-server"></a>Lijst met parameters voor serverconfiguratie voor Azure Database for PostgreSQL-server
Als u alle parameters van kan worden gewijzigd in een server en de bijbehorende waarden, voer de [az postgres server configuration list](/cli/azure/postgres/server/configuration#az_postgres_server_configuration_list) opdracht.

U kunt de parameters voor serverconfiguratie voor de server een lijst **mydemoserver.postgres.database.azure.com** onder de resourcegroep **myresourcegroup**.
```azurecli-interactive
az postgres server configuration list --resource-group myresourcegroup --server mydemoserver
```
## <a name="show-server-configuration-parameter-details"></a>Details van de parameter voor de serverconfiguratie van de weergeven
Uitvoeren als u wilt weergeven van details over een bepaalde configuratieparameter voor een server, de [az postgres server configuration show](/cli/azure/postgres/server/configuration#az_postgres_server_configuration_show) opdracht.

In dit voorbeeld worden details van de **log\_min\_berichten** configuratieparameter van de server voor server **mydemoserver.postgres.database.azure.com** onder de resourcegroep **myresourcegroup.**
```azurecli-interactive
az postgres server configuration show --name log_min_messages --resource-group myresourcegroup --server mydemoserver
```
## <a name="modify-server-configuration-parameter-value"></a>Parameterwaarde voor server-configuratie wijzigen
U kunt ook de waarde van een bepaalde server configuratieparameter, die de onderliggende configuratiewaarde voor de PostgreSQL-server-engine-updates wijzigen. Voor het bijwerken van de configuratie, gebruikt u de [az postgres server configuratieset](/cli/azure/postgres/server/configuration#az_postgres_server_configuration_set) opdracht. 

Om bij te werken de **log\_min\_berichten** server configuratieparameter van server **mydemoserver.postgres.database.azure.com** onder de resourcegroep  **myresourcegroup.**
```azurecli-interactive
az postgres server configuration set --name log_min_messages --resource-group myresourcegroup --server mydemoserver --value INFO
```
Als u instellen van de waarde van een configuratieparameter wilt, kiest u gewoon laat u uit de optionele `--value` parameter, de service is van toepassing op de standaardwaarde. In bovenstaande voorbeeld wordt eruitziet het, zoals:
```azurecli-interactive
az postgres server configuration set --name log_min_messages --resource-group myresourcegroup --server mydemoserver
```
Hiermee stelt u deze opdracht de **log\_min\_berichten** configuratie op de standaardwaarde **waarschuwing**. Zie voor meer informatie over de serverconfiguratie en de toegestane waarden voor PostgreSQL-documentatie op [serverconfiguratie](https://www.postgresql.org/docs/9.6/static/runtime-config.html).

## <a name="next-steps"></a>Volgende stappen
- Als u wilt configureren en om toegang tot serverlogboeken, Zie [Server-logboeken in Azure Database for PostgreSQL](concepts-server-logs.md)
