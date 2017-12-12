---
title: PowerShell voorbeeld-actieve geo-replicatie gegroepeerde Azure SQL database | Microsoft Docs
description: Azure PowerShell-voorbeeldscript actieve geo-replicatie voor een gegroepeerde Azure SQL-database instellen en failover.
services: sql-database
documentationcenter: sql-database
author: CarlRabeler
manager: jhubbard
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: business continuity, mvc
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 11/29/2017
ms.author: carlrab
ms.openlocfilehash: b45e829b9f168e28530bc6237433463d4bfc1bed
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2017
---
# <a name="use-powershell-to-configure-active-geo-replication-for-a-pooled-azure-sql-database"></a>PowerShell gebruiken voor het configureren van actieve geo-replicatie voor een gegroepeerde Azure SQL database

Voorbeeld van deze PowerShell-script actieve geo-replicatie voor een Azure SQL database in een elastische pool configureert en failover op de secundaire replica van de Azure SQL database.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-scripts"></a>Voorbeeldscripts

[!code-powershell[main](../../../powershell_scripts/sql-database/setup-geodr-and-failover-pool/setup-geodr-and-failover-pool.ps1?highlight=16-19 "Set up active geo-replication for elastic pool")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie

Na het uitvoeren van het voorbeeldscript kan de volgende opdracht worden gebruikt om te verwijderen van de resourcegroep en alle resources die zijn gekoppeld.

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $primaryresourcegroupname
Remove-AzureRmResourceGroup -ResourceGroupName $secondaryresourcegroupname
```

## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdrachten. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [Nieuwe AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Maakt een resourcegroep waarin alle resources worden opgeslagen. |
| [Nieuwe AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver) | Maakt een logische server die als host fungeert voor een database of elastische pool. |
| [Nieuwe AzureRmSqlElasticPool](/powershell/module/azurerm.sql/new-azurermsqlelasticpool) | Maakt een elastische pool in een logische server. |
| [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) | Maakt een database in een logische server als één of een gegroepeerde database. |
| [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase) | Database-eigenschappen-updates of een database verplaatst naar, uit of tussen elastische pools. |
| [Nieuwe AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/new-azurermsqldatabasesecondary)| Hiermee maakt u een secundaire database voor een bestaande database en start gegevensreplicatie. |
| [Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase)| Hiermee haalt u een of meer databases. |
| [Set-AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/set-azurermsqldatabasesecondary)| Een secundaire database als primaire om start failover verandert.|
| [Get-AzureRmSqlDatabaseReplicationLink](/powershell/module/azurerm.sql/get-azurermsqldatabasereplicationlink) | Hiermee haalt u de geo-replicatiekoppelingen tussen een Azure SQL Database en een resourcegroep of SQL Server. |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Hiermee verwijdert u een resourcegroep met inbegrip van alle ingesloten resources. |
|||

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Azure PowerShell [documentatie van Azure PowerShell](/powershell/azure/overview).

Voorbeelden van aanvullende SQL Database PowerShell-script kunnen worden gevonden in de [Azure SQL Database PowerShell-scripts](../sql-database-powershell-samples.md).
