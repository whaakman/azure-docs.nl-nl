---
title: PowerShell-voorbeeld-controle-detectie van bedreigingen-Azure SQL Database | Microsoft Docs
description: Voorbeeldscript van Azure PowerShell voor het configureren van controle en detectie van bedreigingen in een Azure SQL-database
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: security
ms.devlang: PowerShell
ms.topic: sample
author: ronitr
ms.author: ronitr
ms.reviewer: carlrab
manager: craigg
ms.date: 03/12/2019
ms.openlocfilehash: 5e838aa0bda0620958e679ab614cc238df607fdd
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58891528"
---
# <a name="use-powershell-to-configure-sql-database-auditing-and-threat-detection"></a>PowerShell gebruiken om controle en detectie van bedreigingen in SQL Database te configureren

Met dit PowerShell-voorbeeldscript wordt controle en detectie van bedreigingen in SQL Database geconfigureerd.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]
[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Als u ervoor kiest om te installeren en lokaal gebruik van PowerShell, voor deze zelfstudie vereist AZ PowerShell 1.4.0 of hoger. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-az-ps). Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzAccount` uitvoeren om verbinding te kunnen maken met Azure.

## <a name="sample-script"></a>Voorbeeldscript

[!code-powershell-interactive[main](../../../powershell_scripts/sql-database/database-auditing-and-threat-detection/database-auditing-and-threat-detection.ps1?highlight=15-16 "Configure auditing and threat detection")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie

Na het uitvoeren van het voorbeeldscript kan de volgende opdracht worden gebruikt om de resourcegroep en alle resources die er aan zijn gekoppeld te verwijderen.

```powershell
Remove-AzResourceGroup -ResourceGroupName $resourcegroupname
```

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Hiermee maakt u een resourcegroep waarin alle resources worden opgeslagen. |
| [New-AzSqlServer](/powershell/module/az.sql/new-azsqlserver) | Hiermee maakt u een SQL Database-server die één database of elastische pool host. |
| [New-AzSqlDatabase](/powershell/module/az.sql/new-azsqldatabase) | Hiermee maakt u een enkele database of elastische pool. |
| [New-AzStorageAccount](/powershell/module/az.storage/new-azstorageaccount) | Hiermee maakt u een opslagaccount. |
| [Set-AzSqlDatabaseAuditing](/powershell/module/az.sql/set-azsqldatabaseauditing) | Hiermee wordt het controlebeleid voor een database ingesteld. |
| [Set-AzSqlDatabaseThreatDetectionPolicy](/powershell/module/az.sql/set-azsqldatabasethreatdetectionpolicy) | Hiermee wordt een beleid voor detectie van bedreigingen in een database ingesteld. |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Hiermee verwijdert u een resourcegroep met inbegrip van alle geneste resources. |
|||

## <a name="next-steps"></a>Volgende stappen

Zie [Documentatie over Azure PowerShell](/powershell/azure/overview) voor meer informatie over Azure PowerShell.

Aanvullende voorbeelden van SQL Database PowerShell-scripts vindt u in [Azure SQL Database PowerShell-scripts](../sql-database-powershell-samples.md).
