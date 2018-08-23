---
title: 'PowerShell-voorbeeld: back-up herstellen van een Azure SQL-database | Microsoft Docs'
description: Azure PowerShell-voorbeeldscript om een Azure SQL-database te herstellen vanuit geografisch redundante back-ups
services: sql-database
documentationcenter: sql-database
author: CarlRabeler
manager: craigg
editor: carlrab
tags: azure-service-management
ms.assetid: ''
ms.service: sql-database
ms.custom: business continuity, mvc
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 04/01/2018
ms.author: carlrab
ms.openlocfilehash: e93ef3f837d29204c6710684d4dfc6f359f572cd
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/10/2018
ms.locfileid: "41920973"
---
# <a name="use-powershell-to-restore-an-azure-sql-database-from-backups"></a>PowerShell gebruiken voor het herstellen van een Azure SQL-database vanuit back-ups

Met dit PowerShell-script wordt een Azure SQL-database teruggezet vanuit een geografisch redundante back-up, wordt een verwijderde Azure SQL-database naar de laatste back-up hersteld en wordt een Azure SQL-database hersteld naar een specifiek tijdstip.  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Voorbeeldscript

[!code-powershell[main](../../../powershell_scripts/sql-database/restore-database/restore-database.ps1?highlight=17-18 "Create SQL Database")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie

Na het uitvoeren van het voorbeeldscript kan de volgende opdracht worden gebruikt om de resourcegroep en alle resources die er aan zijn gekoppeld te verwijderen.

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourcegroupname
```

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup) | Hiermee wordt een resourcegroep gemaakt waarin alle resources worden opgeslagen. | [New-AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver) | Hiermee wordt een logische server gemaakt die als host fungeert voor een database of elastische pool. | 
| [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) | Hiermee maakt u een database op een logische server als een afzonderlijke of een gegroepeerde database. |
[Get-AzureRmSqlDatabaseGeoBackup](/powershell/module/azurerm.sql/get-azurermsqldatabasegeobackup) | Hiermee haalt u een geografisch redundante back-up van een database op. |
| [Restore-AzureRmSqlDatabase](/powershell/module/azurerm.sql/restore-azurermsqldatabase) | Hiermee herstelt u een SQL-database. |
|[Remove-AzureRmSqlDatabase](/powershell/module/azurerm.sql/remove-azurermsqldatabase) | Hiermee selecteert u een Azure SQL-database. |
| [Get-AzureRmSqlDeletedDatabaseBackup](/powershell/module/azurerm.sql/get-azurermsqldeleteddatabasebackup) | Hiermee haalt u een verwijderde database die u kunt herstellen op. |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Hiermee verwijdert u een resourcegroep met inbegrip van alle geneste resources. |

## <a name="next-steps"></a>Volgende stappen

Zie [Documentatie over Azure PowerShell](/powershell/azure/overview) voor meer informatie over Azure PowerShell.

Aanvullende voorbeelden van SQL Database PowerShell-scripts vindt u in [Azure SQL Database PowerShell-scripts](../sql-database-powershell-samples.md).
