---
title: Configureer de parameters van de service in Azure-Database voor MySQL | Microsoft Docs
description: In dit artikel wordt beschreven hoe de parameters van de service in Azure-Database configureren voor MySQL met het opdrachtregelprogramma Azure CLI.
services: mysql
author: rachel-msft
ms.author: raagyema
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: article
ms.date: 11/29/2017
ms.openlocfilehash: 5983bbf6fac9c3cddda19f6a11d2fe2b18177160
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2017
---
# <a name="customize-server-configuration-parameters-by-using-azure-cli"></a>De parameters van de server-configuratie aanpassen met behulp van Azure CLI
U kunt weergeven, weergeven en configuratieparameters voor een Azure-Database voor de MySQL-server bijwerken met behulp van Azure CLI, het Azure-opdrachtregelprogramma. Een subset van engine-configuraties wordt weergegeven op het niveau van de server en kan worden gewijzigd. 

## <a name="prerequisites"></a>Vereisten
Stap in deze handleiding instructies, wilt u het volgende nodig:
- [Een Azure-Database voor de MySQL-server](quickstart-create-mysql-server-database-using-azure-cli.md)
- [Azure CLI 2.0](/cli/azure/install-azure-cli) opdrachtregelprogramma of gebruik de Azure-Cloud-Shell in de browser.

## <a name="list-server-configuration-parameters-for-azure-database-for-mysql-server"></a>Lijst met server configuratieparameters voor Azure-Database voor MySQL-server
Uitvoeren als u alle bewerkbaar parameters in een server en hun waarden weergeven, de [az mysql serverlijst configuration](/cli/azure/mysql/server/configuration#az_mysql_server_configuration_list) opdracht.

U kunt de configuratieparameters van de server voor de server weergeven **myserver4demo.mysql.database.azure.com** onder de resourcegroep **myresourcegroup**.
```azurecli-interactive
az mysql server configuration list --resource-group myresourcegroup --server myserver4demo
```
Zie de sectie MySQL-documentatie op voor de definitie van elk van de vermelde parameters [Server systeemvariabelen](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html).

## <a name="show-server-configuration-parameter-details"></a>Serverconfiguratie parameterdetails weergeven
Uitvoeren als details wilt weergeven over een specifieke configuratie-parameter voor een server, de [az mysql server configuratie weergeven](/cli/azure/mysql/server/configuration#az_mysql_server_configuration_show) opdracht.

In dit voorbeeld worden details weergegeven van de **trage\_query\_logboek** configuratieparameter server voor server **myserver4demo.mysql.database.azure.com** onder de resourcegroep **myresourcegroup.**
```azurecli-interactive
az mysql server configuration show --name slow_query_log --resource-group myresourcegroup --server myserver4demo
```
## <a name="modify-a-server-configuration-parameter-value"></a>De parameterwaarde van een server-configuratie wijzigen
U kunt ook de waarde van een bepaalde server configuratieparameter, die de onderliggende configuratiewaarde voor de MySQL-server-engine-updates wijzigen. Voor het bijwerken van de configuratie, gebruiken de [az mysql server configuratieset](/cli/azure/mysql/server/configuration#az_mysql_server_configuration_set) opdracht. 

Bijwerken van de **trage\_query\_logboek** server configuratieparameter van server **myserver4demo.mysql.database.azure.com** onder de resourcegroep  **myresourcegroup.**
```azurecli-interactive
az mysql server configuration set --name slow_query_log --resource-group myresourcegroup --server myserver4demo --value ON
```
Als u de waarde van een configuratieparameter instellen wilt, laat u de optionele `--value` parameter en de service van toepassing is de standaardwaarde. Het bovenstaande voorbeeld eruit deze als:
```azurecli-interactive
az mysql server configuration set --name slow_query_log --resource-group myresourcegroup --server myserver4demo
```
Hiermee stelt u deze code de **trage\_query\_logboek** configuratie op de standaardwaarde **OFF**. 

## <a name="next-steps"></a>Volgende stappen
- Het configureren van [parameters van de server in Azure-portal](howto-server-parameters.md)
