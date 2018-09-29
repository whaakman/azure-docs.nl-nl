---
title: PowerShell-voorbeeld-controle-detectie van bedreigingen-Azure SQL Database | Microsoft Docs
description: Voorbeeldscript van Azure PowerShell voor het configureren van controle en detectie van bedreigingen in een Azure SQL-database
services: sql-database
ms.service: sql-database
ms.subservice: threat-detection
ms.custom: security
ms.devlang: PowerShell
ms.topic: sample
author: giladmit
ms.author: giladm
ms.reviewer: carlrab
manager: craigg
ms.date: 09/07/2018
ms.openlocfilehash: 6942577c2bf6d031ded9ba6af93a2ab592fe9976
ms.sourcegitcommit: 715813af8cde40407bd3332dd922a918de46a91a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47053806"
---
# <a name="use-powershell-to-configure-sql-database-auditing-and-threat-detection"></a>PowerShell gebruiken om controle en detectie van bedreigingen in SQL Database te configureren

Met dit PowerShell-voorbeeldscript wordt controle en detectie van bedreigingen in SQL Database geconfigureerd. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Als u ervoor kiest om PowerShell lokaal te installeren en te gebruiken, moet u moduleversie 5.7.0 of hoger van Azure PowerShell gebruiken voor deze zelfstudie. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.

## <a name="sample-script"></a>Voorbeeldscript

[!code-powershell-interactive[main](../../../powershell_scripts/sql-database/database-auditing-and-threat-detection/database-auditing-and-threat-detection.ps1?highlight=13-14 "Configure auditing and threat detection")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie

Na het uitvoeren van het voorbeeldscript kan de volgende opdracht worden gebruikt om de resourcegroep en alle resources die er aan zijn gekoppeld te verwijderen.

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourcegroupname
```

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Hiermee wordt een resourcegroep gemaakt waarin alle resources worden opgeslagen. |
| [New-AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver) | Hiermee wordt een logische server gemaakt die als host fungeert voor een database of elastische pool. |
| [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) | Hiermee maakt u een database op een logische server als een afzonderlijke of een gegroepeerde database. |
| [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) | Hiermee maakt u een opslagaccount. |
| [Set-AzureRmSqlDatabaseAuditingPolicy](/powershell/module/azurerm.sql/set-azurermsqldatabaseauditingpolicy) | Hiermee wordt het controlebeleid voor een database ingesteld. |
| [Set-AzureRmSqlDatabaseThreatDetectionPolicy](/powershell/module/azurerm.sql/set-azurermsqldatabasethreatdetectionpolicy) | Hiermee wordt een beleid voor detectie van bedreigingen in een database ingesteld. |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Hiermee verwijdert u een resourcegroep met inbegrip van alle geneste resources. |
|||

## <a name="next-steps"></a>Volgende stappen

Zie [Documentatie over Azure PowerShell](/powershell/azure/overview) voor meer informatie over Azure PowerShell.

Aanvullende voorbeelden van SQL Database PowerShell-scripts vindt u in [Azure SQL Database PowerShell-scripts](../sql-database-powershell-samples.md).
