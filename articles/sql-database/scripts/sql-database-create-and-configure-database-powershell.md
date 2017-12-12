---
title: PowerShell voorbeeld-Maak een Azure SQL database | Microsoft Docs
description: Azure PowerShell-voorbeeldscript om een Azure SQL-database te maken
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: DBs & servers, mvc
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 06/23/2017
ms.author: janeng
ms.openlocfilehash: cf4fa741674efbf9e495dbc8765ee8359387cda8
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2017
---
# <a name="use-powershell-to-create-a-single-azure-sql-database-and-configure-a-firewall-rule"></a>PowerShell gebruiken voor het maken van één Azure SQL database en een firewallregel configureren

Voorbeeld van deze PowerShell-script maakt een Azure SQL database en configureert u een firewallregel op serverniveau. Nadat het script is uitgevoerd, zijn de SQL-Database toegankelijk vanuit alle Azure-services en het geconfigureerde IP-adres. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Voorbeeld van een script

[!code-powershell[main](../../../powershell_scripts/sql-database/create-and-configure-database/create-and-configure-database.ps1?highlight=13-14 "Create SQL Database")]

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
| [Nieuwe AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver) | Maakt een logische server die als host fungeert voor een database of elastische pool. |
| [New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule) | Maakt een firewallregel voor toegang tot alle SQL-Databases op de server van het opgegeven IP-adresbereik. |
| [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) | Maakt een database in een logische server als één of een gegroepeerde database. |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Hiermee verwijdert u een resourcegroep met inbegrip van alle ingesloten resources. |
|||

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Azure PowerShell [documentatie van Azure PowerShell](/powershell/azure/overview).

Voorbeelden van aanvullende SQL Database PowerShell-script kunnen worden gevonden in de [Azure SQL Database PowerShell-scripts](../sql-database-powershell-samples.md).



