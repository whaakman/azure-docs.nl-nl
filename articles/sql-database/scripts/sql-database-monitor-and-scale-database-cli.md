---
title: 'Azure CLI-voorbeeldscript: een Azure SQL-database controleren en schalen | Microsoft Docs'
description: Azure CLI-voorbeeldscript voor het controleren en schalen van één Azure SQL-database
services: sql-database
documentationcenter: sql-database
author: CarlRabeler
manager: craigg
editor: carlrab
tags: azure-service-management
ms.assetid: ''
ms.service: sql-database
ms.custom: monitor & tune, mvc
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 04/01/2018
ms.author: carlrab
ms.openlocfilehash: 00464de212b7a2faa62eeac15bc4db1fd0f84b4f
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
ms.locfileid: "34364434"
---
# <a name="use-cli-to-monitor-and-scale-a-single-sql-database"></a>CLI gebruiken voor het controleren en schalen van één Azure SQL-database

In dit Azure CLI-voorbeeldscript wordt één Azure SQL-database naar een ander prestatieniveau geschaald nadat er een query is gestuurd over de grootte van de database. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit artikel gebruikmaken van Azure CLI versie 2.0 of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Voorbeeldscript

[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/monitor-and-scale-database/monitor-and-scale-database.sh "Monitor and scale single SQL Database")]

> [!TIP]
> Gebruik [az sql db op list](/cli/azure/sql/db/op?#az_sql_db_op_list) voor een lijst met bewerkingen die worden uitgevoerd op de database en gebruik [az sql db op cancel](/cli/azure/sql/db/op#az_sql_db_op_cancel) om een update van de database te annuleren.

## <a name="clean-up-deployment"></a>Opschonen van implementatie

Na het uitvoeren van het voorbeeldscript kan de volgende opdracht worden gebruikt om de resourcegroep en alle resources die er aan zijn gekoppeld te verwijderen.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create) | Hiermee wordt een resourcegroep gemaakt waarin alle resources worden opgeslagen. |
| [az sql server create](https://docs.microsoft.com/cli/azure/sql/server#az_sql_server_create) | Hiermee wordt een logische server gemaakt waar een database wordt gehost. |
| [az sql db show-usage](https://docs.microsoft.com/cli/azure/sql/db#az_sql_db_show_usage) | Toont de gebruikte grootte voor een database. |
| [az sql db update](https://docs.microsoft.com/cli/azure/sql/db#az_sql_db_update) | Hiermee worden database-eigenschappen (zoals de servicelaag of het prestatieniveau) bijgewerkt of wordt een database verplaatst naar, uit of tussen elastische pools. |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension#az_vm_extension_set) | Hiermee verwijdert u een resourcegroep met inbegrip van alle geneste resources. |
|||

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure) voor meer informatie over de Azure CLI.

Aanvullende voorbeelden van SQL Database CLI-scripts vindt u in de [documentatie van Azure SQL Database](../sql-database-cli-samples.md).
