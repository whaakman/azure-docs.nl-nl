---
title: PowerShell voorbeeld importeren Bacpac-bestand Azure SQL database | Microsoft Docs
description: Azure PowerShell-voorbeeldscript een Bacpac-bestand importeren in een SQL-database
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: load & move data, mvc
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 06/23/2017
ms.author: janeng
ms.openlocfilehash: c24a3b7551058a69cbfdddf8b859b635e0829402
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2017
---
# <a name="use-powershell-to-import-a-bacpac-file-into-an-azure-sql-database"></a>PowerShell gebruiken voor een Bacpac-bestand importeren in een Azure SQL database

Voorbeeld van deze PowerShell-script kunt een database uit een Bacpac-bestand importeren in een Azure SQL database.  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Voorbeeld van een script

[!code-powershell[main](../../../powershell_scripts/sql-database/import-from-bacpac/import-from-bacpac.ps1?highlight=18-19 "Create SQL Database")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie

Na het uitvoeren van het voorbeeldscript kan de volgende opdracht worden gebruikt om te verwijderen van de resourcegroep en alle resources die zijn gekoppeld.

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourcegroupname
```

## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdrachten. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [Nieuwe AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Maakt een resourcegroep waarin alle resources worden opgeslagen. |
| [Nieuwe AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver) | Maakt een logische server die als host fungeert voor de SQL-Database. |
| [New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule) | Maakt een firewallregel voor toegang tot alle SQL-Databases op de server van het opgegeven IP-adresbereik. |
| [Nieuwe AzureRmSqlDatabaseImport](/powershell/module/azurerm.sql/new-azurermsqldatabaseimport) | De invoer een BACPAC-bestand en een nieuwe database maken op de server. |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Hiermee verwijdert u een resourcegroep met inbegrip van alle ingesloten resources. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Azure PowerShell [documentatie van Azure PowerShell](/powershell/azure/overview).

Voorbeelden van aanvullende SQL Database PowerShell-script kunnen worden gevonden in de [Azure SQL Database PowerShell-scripts](../sql-database-powershell-samples.md).
