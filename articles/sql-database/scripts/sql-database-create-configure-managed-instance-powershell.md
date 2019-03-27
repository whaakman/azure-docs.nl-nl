---
title: 'PowerShell - voorbeeld: een beheerd exemplaar maken in Azure SQL Database | Microsoft Docs'
description: Azure PowerShell-voorbeeldscript voor het maken van een beheerd exemplaar in Azure SQL Database
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: PowerShell
ms.topic: sample
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 03/25/2019
ms.openlocfilehash: 955f13376ac899f66b0ec4e1ed99166164508fbe
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58449866"
---
# <a name="use-powershell-to-create-an-azure-sql-database-managed-instance"></a>PowerShell gebruiken om u te maken van een Azure SQL-Database beheerd exemplaar

Met dit PowerShell-voorbeeldscript maakt een beheerd exemplaar voor Azure SQL Database in een toegewezen subnet in een nieuw virtueel netwerk. Het is ook configureert u een routetabel en een netwerkbeveiligingsgroep voor het virtuele netwerk. Nadat het script is uitgevoerd, kan het beheerde exemplaar van worden geopend binnen het virtuele netwerk of vanuit een on-premises omgeving. Zie voor snelstartgidsen waarin u wordt getoond hoe om dit te doen, [configureren van virtuele Azure-machine verbinding maken met een Azure SQL Database Managed Instance](../sql-database-managed-instance-configure-vm.md) en [configureren van een punt-naar-site-verbinding naar een Azure SQL Database Managed Instance van on-premises](../sql-database-managed-instance-configure-p2s.md).

> [!IMPORTANT]
> Zie voor beperkingen, [ondersteunde regio's](../sql-database-managed-instance-resource-limits.md#supported-regions) en [abonnementstypen ondersteund](../sql-database-managed-instance-resource-limits.md#supported-subscription-types).

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Als u ervoor kiest om te installeren en lokaal gebruik van PowerShell, voor deze zelfstudie vereist AZ PowerShell 1.4.0 of hoger. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-az-ps). Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzAccount` uitvoeren om verbinding te kunnen maken met Azure.

## <a name="sample-script"></a>Voorbeeldscript

[!code-powershell-interactive[main](../../../powershell_scripts/sql-database/managed-instance/create-and-configure-managed-instance.ps1 "Create managed instance")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie

Na het uitvoeren van het voorbeeldscript kan de volgende opdracht worden gebruikt om de resourcegroep en alle resources die er aan zijn gekoppeld te verwijderen.

```powershell
Remove-AzResourceGroup -ResourceGroupName $resourcegroupname
```

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Hiermee maakt u een resourcegroep waarin alle resources worden opgeslagen. 
| [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) | Hiermee maakt u een virtueel netwerk |
| [Add-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/Add-AzVirtualNetworkSubnetConfig) | Hiermee voegt u een subnetconfiguratie toe aan een virtueel netwerk |
| [Get-AzVirtualNetwork](/powershell/module/az.network/Get-AzVirtualNetwork) | Hiermee haalt u een virtueel netwerk in een resourcegroep |
| [Set-AzVirtualNetwork](/powershell/module/az.network/Set-AzVirtualNetwork) | Stelt de doelstatus voor een virtueel netwerk |
| [Get-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/Get-AzVirtualNetworkSubnetConfig) | Hiermee haalt u een subnet in een virtueel netwerk |
| [Set-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/Set-AzVirtualNetworkSubnetConfig) | Hiermee configureert u de doelstatus voor een subnetconfiguratie in een virtueel netwerk |
| [New-AzRouteTable](/powershell/module/az.network/New-AzRouteTable) | Hiermee maakt u een routetabel |
| [Get-AzRouteTable](/powershell/module/az.network/Get-AzRouteTable) | Routetabellen opgehaald |
| [Set-AzRouteTable](/powershell/module/az.network/Set-AzRouteTable) | Stelt de doelstatus voor een routetabel |
| [New-AzSqlInstance](/powershell/module/az.sql/New-AzSqlInstance) | Hiermee maakt u een beheerd exemplaar voor Azure SQL Database |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Hiermee verwijdert u een resourcegroep met inbegrip van alle geneste resources. |
|||

## <a name="next-steps"></a>Volgende stappen

Zie [Documentatie over Azure PowerShell](/powershell/azure/overview) voor meer informatie over Azure PowerShell.

Aanvullende voorbeelden van SQL Database PowerShell-scripts vindt u in [Azure SQL Database PowerShell-scripts](../sql-database-powershell-samples.md).
