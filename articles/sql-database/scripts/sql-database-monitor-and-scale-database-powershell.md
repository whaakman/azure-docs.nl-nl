---
title: PowerShell-voorbeeld-monitor-scale-enkel Azure SQL database | Microsoft Docs
description: "Azure PowerShell-voorbeeldscript om te bewaken en schalen van één Azure SQL database"
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune, mvc
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 12/14/2017
ms.author: janeng
ms.openlocfilehash: 70dcac2449b18e7afef3f96817593128ca1ab315
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2017
---
# <a name="use-powershell-to-monitor-and-scale-a-single-sql-database"></a>PowerShell gebruiken om te bewaken en schalen van een enkele SQL-database

Voorbeeld van deze PowerShell-script bewaakt de maatstaven voor prestaties van een database naar een hoger prestatieniveau op schaal en een waarschuwingsregel maakt op een van de maatstaven voor prestaties. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Voorbeeld van een script

[!code-powershell[main](../../../powershell_scripts/sql-database/monitor-and-scale-database/monitor-and-scale-database.ps1?highlight=13-14 "Monitor and scale single SQL Database")]

> [!TIP]
> Gebruik [Get-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/get-azurermsqldatabaseactivity) de status van de bewerkingen ophalen en gebruiken [Stop AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/stop-azurermsqldatabaseactivity) annuleert voor update-bewerking op de database.

## <a name="clean-up-deployment"></a>Opschonen van implementatie

Na het uitvoeren van het voorbeeldscript kan de volgende opdracht worden gebruikt om te verwijderen van de resourcegroep en alle resources die zijn gekoppeld.

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourcegroupname
```

## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdrachten. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
 [Nieuwe AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Maakt een resourcegroep waarin alle resources worden opgeslagen. |
| [Nieuwe AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver) | Maakt een logische server die als host fungeert voor een database of elastische pool. |
| [Get-AzureRmMetric](/powershell/module/azurerm.insights/get-azurermmetric) | Toont de gebruiksgegevens van de grootte voor de database.|
| [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase) | Database-eigenschappen-updates of een database verplaatst naar, uit of tussen elastische pools. |
| [Voeg AzureRMMetricAlertRule](/powershell/module/azurerm.insights/add-azurermmetricalertrule) | Hiermee stelt u een waarschuwingsregel automatisch dtu's in de toekomst te bewaken. |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Hiermee verwijdert u een resourcegroep met inbegrip van alle ingesloten resources. |
|||

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Azure PowerShell [documentatie van Azure PowerShell](/powershell/azure/overview).

Voorbeelden van aanvullende SQL Database PowerShell-script kunnen worden gevonden in de [Azure SQL Database PowerShell-scripts](../sql-database-powershell-samples.md).
