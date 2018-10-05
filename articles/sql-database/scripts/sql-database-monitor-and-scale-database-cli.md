---
title: 'Azure CLI-voorbeeldscript: een Azure SQL-database controleren en schalen | Microsoft Docs'
description: Azure CLI-voorbeeldscript voor het controleren en schalen van één Azure SQL-database
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: azurecli
ms.topic: sample
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 09/28/2018
ms.openlocfilehash: 3484c402f85985a4de70ae2077404613539eaeb6
ms.sourcegitcommit: f31bfb398430ed7d66a85c7ca1f1cc9943656678
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/28/2018
ms.locfileid: "47451796"
---
# <a name="use-cli-to-monitor-and-scale-a-single-sql-database"></a>CLI gebruiken voor het controleren en schalen van één Azure SQL-database

In dit Azure CLI-voorbeeldscript wordt één Azure SQL-database naar een andere rekengrootte geschaald nadat er een query is gestuurd over de grootte van de database. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit artikel gebruikmaken van Azure CLI versie 2.0 of hoger. Voer `az --version` uit om de versie te bekijken. Als u uw CLI wilt installeren of upgraden, raadpleegt u [De Azure CLI installeren]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Voorbeeldscript

[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/monitor-and-scale-database/monitor-and-scale-database.sh "Monitor and scale single SQL Database")]

> [!TIP]
> Gebruik [az sql db op list](/cli/azure/sql/db/op?#az-sql-db-op-list) voor een lijst met bewerkingen die worden uitgevoerd op de database en gebruik [az sql db op cancel](/cli/azure/sql/db/op#az-sql-db-op-cancel) om een update van de database te annuleren.

## <a name="clean-up-deployment"></a>Opschonen van implementatie

Na het uitvoeren van het voorbeeldscript kan de volgende opdracht worden gebruikt om de resourcegroep en alle resources die er aan zijn gekoppeld te verwijderen.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az-group-create) | Hiermee wordt een resourcegroep gemaakt waarin alle resources worden opgeslagen. |
| [az sql server create](https://docs.microsoft.com/cli/azure/sql/server#az-sql-server-create) | Hiermee wordt een logische server gemaakt waar een database wordt gehost. |
| [az sql db show-usage](https://docs.microsoft.com/cli/azure/sql/db#az-sql-db-show-usage) | Toont de gebruikte grootte voor een database. |
| [az sql db update](https://docs.microsoft.com/cli/azure/sql/db#az-sql-db-update) | Hiermee worden database-eigenschappen (zoals de servicelaag of de rekengrootte) bijgewerkt of wordt een database verplaatst naar, uit of tussen elastische pools. |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension#az-vm-extension-set) | Hiermee verwijdert u een resourcegroep met inbegrip van alle geneste resources. |
|||

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure) voor meer informatie over de Azure CLI.

Aanvullende voorbeelden van SQL Database CLI-scripts vindt u in de [documentatie van Azure SQL Database](../sql-database-cli-samples.md).
