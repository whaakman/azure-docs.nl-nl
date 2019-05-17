---
title: 'PowerShell-voorbeeld: Een enkele database maken in Azure SQL Database | Microsoft Docs'
description: Azure PowerShell-voorbeeldscript voor het maken van een enkele database in Azure SQL Database
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: PowerShell
ms.topic: sample
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 03/12/2019
ms.openlocfilehash: 949e0569a0ee2095e0dc7044160ec4e16ce33c78
ms.sourcegitcommit: 3ced637c8f1f24256dd6ac8e180fff62a444b03c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2019
ms.locfileid: "65834258"
---
# <a name="use-powershell-to-create-a-single-azure-sql-database-and-configure-a-sql-database-server-firewall-rule"></a>Gebruik PowerShell om een enkele Azure SQL-database te maken en een firewallregel voor de SQL Database-server te configureren

Met dit PowerShell-script wordt één Azure SQL-database gemaakt en een regel voor een firewall op serverniveau geconfigureerd. Nadat het script is uitgevoerd, is de SQL Database toegankelijk via alle Azure-services en het geconfigureerde IP-adres.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]
[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Als u ervoor kiest om te installeren en lokaal gebruik van PowerShell, voor deze zelfstudie vereist AZ PowerShell 1.4.0 of hoger. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-az-ps). Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzAccount` uitvoeren om verbinding te kunnen maken met Azure.

## <a name="sample-script"></a>Voorbeeldscript

[!code-powershell-interactive[main](../../../powershell_scripts/sql-database/create-and-configure-database/create-and-configure-database.ps1?highlight=15-16 "Create SQL Database")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie

Gebruik de volgende opdracht om te verwijderen van de resourcegroep en alle resources die zijn gekoppeld.

```powershell
Remove-AzResourceGroup -ResourceGroupName $resourcegroupname
```

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Hiermee maakt u een resourcegroep waarin alle resources worden opgeslagen. |
| [New-AzSqlServer](/powershell/module/az.sql/new-azsqlserver) | Hiermee maakt u een SQL Database-server die één database of elastische pool host. |
| [New-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule) | Hiermee wordt een firewall-regel gemaakt om toegang te verlenen tot alle SQL Databases op de server vanuit het ingevoerde IP-adresbereik. |
| [New-AzSqlDatabase](/powershell/module/az.sql/new-azsqldatabase) | Maakt één database of elastische pool. |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Hiermee verwijdert u een resourcegroep met inbegrip van alle geneste resources. |
|||

## <a name="next-steps"></a>Volgende stappen

Zie [Documentatie over Azure PowerShell](/powershell/azure/overview) voor meer informatie over Azure PowerShell.

Aanvullende voorbeelden van SQL Database PowerShell-scripts vindt u in [Azure SQL Database PowerShell-scripts](../sql-database-powershell-samples.md).
