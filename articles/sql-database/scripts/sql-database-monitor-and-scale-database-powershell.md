---
title: 'PowerShell-voorbeeldscript: een enkele Azure SQL-database controleren en schalen | Microsoft Docs'
description: Microsoft Azure PowerShell-voorbeeldscript voor het controleren en schalen van één Azure SQL-database
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: PowerShell
ms.topic: sample
author: juliemsft
ms.author: jrasnick
ms.reviewer: carlrab
manager: craigg
ms.date: 03/12/2019
ms.openlocfilehash: 88ec8b93e2429fd394eb3a8d10460a2a80d9e56f
ms.sourcegitcommit: 87bd7bf35c469f84d6ca6599ac3f5ea5545159c9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/22/2019
ms.locfileid: "58349481"
---
# <a name="use-powershell-to-monitor-and-scale-a-single-sql-database"></a>PowerShell gebruiken voor het controleren en schalen van één Azure SQL-database

Met dit PowerShell-script worden de prestatiemetrieken gecontroleerd van een database, waarna deze naar een grotere rekengrootte wordt geschaald en er een waarschuwingsregel voor een van de prestatiemetrieken wordt gemaakt.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]
[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Als u ervoor kiest om te installeren en lokaal gebruik van PowerShell, voor deze zelfstudie vereist AZ PowerShell 1.4.0 of hoger. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-az-ps). Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzAccount` uitvoeren om verbinding te kunnen maken met Azure.

## <a name="sample-script"></a>Voorbeeldscript

[!code-powershell-interactive[main](../../../powershell_scripts/sql-database/monitor-and-scale-database/monitor-and-scale-database.ps1?highlight=15-16 "Monitor and scale single database")]

> [!TIP]
> Zie voor een volledige lijst met metrische gegevens, [ondersteunde metrische gegevens](../../azure-monitor/platform/metrics-supported.md#microsoftsqlserversdatabases).

> [!TIP]
> Gebruik [Get-AzSqlDatabaseActivity](/powershell/module/az.sql/get-azsqldatabaseactivity) naar de status van databasebewerkingen halen en gebruik [Stop-AzSqlDatabaseActivity](/powershell/module/az.sql/stop-azsqldatabaseactivity) om een database-update-bewerking te annuleren.

## <a name="clean-up-deployment"></a>Opschonen van implementatie

Na het uitvoeren van het voorbeeldscript kan de volgende opdracht worden gebruikt om de resourcegroep en alle resources die er aan zijn gekoppeld te verwijderen.

```powershell
Remove-AzResourceGroup -ResourceGroupName $resourcegroupname
```

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
 [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Hiermee maakt u een resourcegroep waarin alle resources worden opgeslagen. |
| [New-AzSqlServer](/powershell/module/az.sql/new-azsqlserver) | Hiermee maakt u een SQL Database-server die één database of elastische pool host. |
| [Get-AzMetric](/powershell/module/az.monitor/get-azmetric) | Toont de gebruikte grootte voor een database.|
| [Set-AzSqlDatabase](/powershell/module/az.sql/set-azsqldatabase) | Hiermee worden database-eigenschappen bijgewerkt of worden databasegegevens verplaatst naar, uit of tussen elastische pools. |
| [Add-AzMetricAlertRule](/powershell/module/az.monitor/add-azmetricalertrule) | Hiermee stelt u een waarschuwingsregel in om automatisch DTU's in de toekomst te bewaken. |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Hiermee verwijdert u een resourcegroep met inbegrip van alle geneste resources. |
|||

## <a name="next-steps"></a>Volgende stappen

Zie [Documentatie over Azure PowerShell](/powershell/azure/overview) voor meer informatie over Azure PowerShell.

Aanvullende voorbeelden van SQL Database PowerShell-scripts vindt u in [Azure SQL Database PowerShell-scripts](../sql-database-powershell-samples.md).
