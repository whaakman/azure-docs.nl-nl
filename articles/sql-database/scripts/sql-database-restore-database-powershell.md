---
title: PowerShell-voorbeeld, herstel, back-up, Azure SQL-database | Microsoft Docs
description: Azure PowerShell-voorbeeldscript om terug te zetten van een Azure SQL database vanuit geografisch redundante back-ups
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: business continuity, mvc
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 06/23/2017
ms.author: janeng
ms.openlocfilehash: e09c14a33f655d059ec4bce6b4aa952855d3ad2a
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2017
---
# <a name="use-powershell-to-restore-an-azure-sql-database-from-backups"></a>PowerShell gebruiken voor het herstellen van een Azure SQL database vanuit back-ups

In dit voorbeeld van de PowerShell-script een Azure SQL database herstelt vanuit een geografisch redundante back-up, een verwijderde Azure SQL-database herstelt naar de meest recente back-up en wordt een Azure SQL database hersteld naar een bepaald punt in tijd.  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Voorbeeld van een script

[!code-powershell[main](../../../powershell_scripts/sql-database/restore-database/restore-database.ps1?highlight=17-18 "Create SQL Database")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie

Na het uitvoeren van het voorbeeldscript kan de volgende opdracht worden gebruikt om te verwijderen van de resourcegroep en alle resources die zijn gekoppeld.

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourcegroupname
```

## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdrachten. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [Nieuwe AzureRmResourceGroup](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/new-azurermresourcegroup) | Maakt een resourcegroep waarin alle resources worden opgeslagen. | [Nieuwe AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver) | Maakt een logische server die als host fungeert voor een database of elastische pool. | 
| [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) | Maakt een database in een logische server als één of een gegroepeerde database. |
[Get-AzureRmSqlDatabaseGeoBackup](/powershell/module/azurerm.sql/get-azurermsqldatabasegeobackup) | Hiermee haalt u een geografisch redundante back-up van een database. |
| [Restore-AzureRmSqlDatabase](/powershell/module/azurerm.sql/restore-azurermsqldatabase) | Een SQL-database herstelt. |
|[Verwijder AzureRmSqlDatabase](/powershell/module/azurerm.sql/remove-azurermsqldatabase) | Hiermee verwijdert u een Azure SQL database. |
| [Get-AzureRmSqlDeletedDatabaseBackup](/powershell/module/azurerm.sql/get-azurermsqldeleteddatabasebackup) | Hiermee haalt u een verwijderde database die u kunt herstellen. |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Hiermee verwijdert u een resourcegroep met inbegrip van alle ingesloten resources. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Azure PowerShell [documentatie van Azure PowerShell](/powershell/azure/overview).

Voorbeelden van aanvullende SQL Database PowerShell-script kunnen worden gevonden in de [Azure SQL Database PowerShell-scripts](../sql-database-powershell-samples.md).
