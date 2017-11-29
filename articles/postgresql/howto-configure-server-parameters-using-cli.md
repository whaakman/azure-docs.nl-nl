---
title: Configureer de parameters van de service in Azure-Database voor PostgreSQL | Microsoft Docs
description: In dit artikel wordt beschreven hoe de parameters van de service in Azure-Database configureren voor PostgreSQL via de opdrachtregel van Azure CLI.
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 11/27/2017
ms.openlocfilehash: a2a3395fad35599591e093385305f28bbb161014
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="customize-server-configuration-parameters-using-azure-cli"></a>Server-configuratieparameters met Azure CLI aanpassen
U kunt weergeven, weergeven en bijwerken van parameters voor de configuratie voor een Azure-PostgreSQL-server met de opdrachtregelinterface (Azure CLI). Een subset van engine-configuraties op niveau van de server wordt weergegeven en kan worden gewijzigd. 

## <a name="prerequisites"></a>Vereisten
Stap in deze handleiding instructies, wilt u het volgende nodig:
- Maken van een Azure-Database voor PostgreSQL-server en database door de volgende [PostgreSQL een Azure-Database gemaakt](quickstart-create-server-database-azure-cli.md)
- Installeer [Azure CLI 2.0](/cli/azure/install-azure-cli) opdrachtregelinterface op uw computer of gebruik de [Azure Cloud Shell](../cloud-shell/overview.md) in de Azure portal met behulp van uw browser.

## <a name="list-server-configuration-parameters-for-azure-database-for-postgresql-server"></a>Lijst met server configuratieparameters voor Azure-Database voor PostgreSQL-server
Uitvoeren als u alle bewerkbaar parameters in een server en hun waarden weergeven, de [az postgres serverlijst configuration](/cli/azure/postgres/server/configuration#az_postgres_server_configuration_list) opdracht.

U kunt de configuratieparameters van de server voor de server weergeven **mypgserver 20170401.postgres.database.azure.com** onder de resourcegroep **myresourcegroup**.
```azurecli-interactive
az postgres server configuration list --resource-group myresourcegroup --server mypgserver-20170401
```
## <a name="show-server-configuration-parameter-details"></a>Serverconfiguratie parameterdetails weergeven
Uitvoeren als details wilt weergeven over een specifieke configuratie-parameter voor een server, de [az postgres server configuratie weergeven](/cli/azure/postgres/server/configuration#az_postgres_server_configuration_show) opdracht.

In dit voorbeeld worden details weergegeven van de **logboek\_min\_berichten** configuratieparameter server voor server **mypgserver 20170401.postgres.database.azure.com** onder resourcegroep **myresourcegroup.**
```azurecli-interactive
az postgres server configuration show --name log_min_messages --resource-group myresourcegroup --server mypgserver-20170401
```
## <a name="modify-server-configuration-parameter-value"></a>Parameterwaarde voor server-configuratie wijzigen
U kunt ook de waarde van een bepaalde server configuratieparameter, die de onderliggende configuratiewaarde voor de server PostgreSQL engine-updates wijzigen. Voor het bijwerken van de configuratie, gebruiken de [az postgres server configuratieset](/cli/azure/postgres/server/configuration#az_postgres_server_configuration_set) opdracht. 

Bijwerken van de **logboek\_min\_berichten** server configuratieparameter van server **mypgserver 20170401.postgres.database.azure.com** onder de resourcegroep **myresourcegroup.**
```azurecli-interactive
az postgres server configuration set --name log_min_messages --resource-group myresourcegroup --server mypgserver-20170401 --value INFO
```
Als u wilt opnieuw instellen van de waarde van een configuratieparameter, kies u gewoon de optionele weglaten `--value` parameter en de service van toepassing is de standaardwaarde. In bovenstaande voorbeeld eruit deze als:
```azurecli-interactive
az postgres server configuration set --name log_min_messages --resource-group myresourcegroup --server mypgserver-20170401
```
Met deze opdracht herstelt de **logboek\_min\_berichten** configuratie op de standaardwaarde **waarschuwing**. Zie voor meer informatie over de serverconfiguratie en de toegestane waarden PostgreSQL-documentatie op [serverconfiguratie](https://www.postgresql.org/docs/9.6/static/runtime-config.html).

## <a name="next-steps"></a>Volgende stappen
- Als u wilt configureren en toegang tot de server-Logboeken, Zie [Server-logboeken in Azure-Database voor PostgreSQL](concepts-server-logs.md)
