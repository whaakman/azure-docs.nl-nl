---
title: Azure analyseservices beheren met PowerShell | Microsoft Docs
description: Azure Analysis Services-beheer met PowerShell.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: reference
ms.date: 12/19/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: b4aec7ff560b0695175a18c9c7e429a8ab733345
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53633705"
---
# <a name="manage-azure-analysis-services-with-powershell"></a>Azure analyseservices beheren met PowerShell

Dit artikel beschrijft de PowerShell-cmdlets gebruikt voor het uitvoeren van Azure Analysis Services-server en database-beheertaken. 

Server-beheertaken, zoals het maken of verwijderen van een server, onderbreken of hervatten van serverbewerkingen of wijzigen van het serviceniveau (laag) gebruikt u Azure Resource Manager (resource)-cmdlets en cmdlets voor Analysis Services (server). Andere taken voor het beheren van databases wilt toevoegen of verwijderen van leden van een rol, verwerkt of gebruikt u de cmdlets opgenomen in de dezelfde SQL Server-module als SQL Server Analysis Services partitioneren.

## <a name="permissions"></a>Machtigingen
De meeste PowerShell-taken moet dat u beheerdersbevoegdheden hebben op de Analysis Services-server die u beheert. PowerShell-taken zijn zonder toezicht bewerkingen. Het account of service-principe uitvoering van de planner moet beheerdersbevoegdheden hebben op de Analysis Services-server. 

Voor serverbewerkingen met behulp van de AzureRm-cmdlets, uw account of de scheduler-account moet ook behoren tot de rol van eigenaar voor de resource in [op rollen gebaseerd toegangsbeheer (RBAC)](../role-based-access-control/overview.md). 

## <a name="resource-management-operations"></a>Resource management-bewerkingen 
Module - [AzureRM.AnalysisServices](https://www.powershellgallery.com/packages/AzureRM.AnalysisServices)

|Cmdlet|Description| 
|------------|-----------------| 
|[Get-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/get-azurermanalysisservicesserver)|Hiermee haalt u details van een server-exemplaar.|  
|[New-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver)|Hiermee maakt u een server-exemplaar.|   
|[Nieuwe AzureRmAnalysisServicesFirewallConfig](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesfirewallconfig)|Hiermee maakt u een nieuwe configuratie van de Analysis Services-firewall.|   
|[Nieuwe AzureRmAnalysisServicesFirewallRule](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesfirewallrule)|Hiermee maakt u een nieuwe firewallregel van Analysis Services.|   
|[Remove-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/remove-azurermanalysisservicesserver)|Hiermee verwijdert u een server-exemplaar.|  
|[Resume-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/resume-azurermanalysisservicesserver)|Hervat een exemplaar van server.|  
|[Suspend-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/suspend-azurermanalysisservicesserver)|Een server-exemplaar onderbreekt.| 
|[Set-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/set-azurermanalysisservicesserver)|Hiermee wijzigt u een server-exemplaar.|   
|[Test-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/test-azurermanalysisservicesserver)|Test het bestaan van een server-exemplaar.| 

## <a name="server-management-operations"></a>Server management-bewerkingen

Module - [Azure.AnalysisServices](https://www.powershellgallery.com/packages/Azure.AnalysisServices)

|Cmdlet|Description| 
|------------|-----------------| 
|[Add-AzureAnalysisServicesAccount](/powershell/module/azure.analysisservices/add-azureanalysisservicesaccount)|Een geverifieerde account moet worden gebruikt voor Azure Analysis Services-serveraanvragen cmdlet toegevoegd.| 
|[Exporteren-AzureAnalysisServicesInstance](/powershell/module/azurerm.analysisservices/export-azureanalysisservicesinstancelog)|Hiermee exporteert u een logboek van een exemplaar van Analysis Services-server in de momenteel aangemelde omgeving zoals opgegeven in de opdracht Add-azureanalysisservicesaccount biedt nu|  
|[Restart-AzureAnalysisServicesInstance](/powershell/module/azurerm.analysisservices/restart-azureanalysisservicesinstance)|Een exemplaar van Analysis Services-server opnieuw wordt opgestart in de omgeving die momenteel is aangemeld. in de opdracht Add-azureanalysisservicesaccount biedt nu hebt opgegeven.|  
|[Synchronisatie-AzureAnalysisServicesInstance](/powershell/module/azurerm.analysisservices/restart-azureanalysisservicesinstance)|Een opgegeven database op het opgegeven exemplaar van Analysis Services-server op alle query scale-out-exemplaren in de momenteel aangemelde omgeving zoals opgegeven in de opdracht Add-azureanalysisservicesaccount biedt nu synchroniseren|  

## <a name="database-operations"></a>Databasebewerkingen

Azure Analysis Services-databasebewerkingen gebruiken hetzelfde [SQL Server-module](https://www.powershellgallery.com/packages/SqlServer) als SQL Server Analysis Services. Niet alle cmdlets worden echter ondersteund voor Azure Analysis Services. Zie voor meer informatie, [SQL Server PowerShell](https://docs.microsoft.com/sql/powershell/sql-server-powershell).

De SQL Server-module biedt taakspecifieke database management-cmdlets, evenals de voor algemeen gebruik Invoke-ASCmd cmdlet die een TMSL Tabular Model Scripting Language ()-query of het script accepteert. De volgende cmdlets in de SQL Server-module worden voor Azure Analysis Services ondersteund.

  
|Cmdlet|Description|
|------------|-----------------| 
|[Voeg RoleMember](https://docs.microsoft.com/powershell/module/sqlserver/Add-RoleMember)|Lid toevoegen aan een databaserol.| 
|[Backup-ASDatabase](https://docs.microsoft.com/powershell/module/sqlserver/backup-asdatabase)|Back-up van een Analysis Services-database.|  
|[Remove-RoleMember](https://docs.microsoft.com/powershell/module/sqlserver/remove-rolemember)|Een lid verwijderen uit een databaserol.|   
|[Invoke-ASCmd](https://docs.microsoft.com/powershell/module/sqlserver/invoke-ascmd)|Voer een TMSL-script.|
|[Invoke-ProcessASDatabase](https://docs.microsoft.com/powershell/module/sqlserver/invoke-processasdatabase)|Het proces een database.|  
|[Aanroepen ProcessPartition](https://docs.microsoft.com/powershell/module/sqlserver/invoke-processpartition)|Een partitie worden verwerkt.| 
|[Aanroepen ProcessTable](https://docs.microsoft.com/powershell/module/sqlserver/invoke-processtable)|Het proces een tabel.|  
|[Partitie samenvoegen](https://docs.microsoft.com/powershell/module/sqlserver/merge-partition)|Een partitie samenvoegen.|  
|[Restore-ASDatabase](https://docs.microsoft.com/powershell/module/sqlserver/restore-asdatabase)|Herstellen van een Analysis Services-database.| 
  

## <a name="related-information"></a>Gerelateerde informatie

* [SQL Server PowerShell-Module downloaden](https://docs.microsoft.com/sql/ssms/download-sql-server-ps-module)   
* [SSMS downloaden](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)   
* [SQL Server-module in PowerShell Gallery](https://www.powershellgallery.com/packages/SqlServer)    
* [Tabellair Model wilt programmeren voor compatibiliteit niveau 1200 en hoger](https://msdn.microsoft.com/library/mt712541.aspx)
