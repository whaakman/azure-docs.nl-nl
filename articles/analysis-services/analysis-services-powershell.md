---
title: Beheren van Azure analyseservices met PowerShell | Microsoft Docs
description: Azure Analysis Services-beheer met PowerShell.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: reference
ms.date: 06/25/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 5c347a024af385e04bfdf3631ddcbaec89df4f40
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36937361"
---
# <a name="manage-azure-analysis-services-with-powershell"></a>Azure analyseservices met PowerShell beheren

In dit artikel beschrijft de PowerShell-cmdlets gebruikt voor het Azure Analysis Services-server en database-beheertaken uitvoeren. 

Server-beheertaken zoals maken of verwijderen van een server, onderbreken of hervatten van serverbewerkingen of wijzigen van het serviceniveau (laag) Azure Resource Manager (resource)-cmdlets en cmdlets van Analysis Services (server) gebruiken. Andere taken voor het beheren van databases zoals het toevoegen of verwijderen van leden van een rol, verwerkt of partitioneren gebruiken cmdlets die zijn opgenomen in dezelfde module SqlServer als SQL Server Analysis Services.

## <a name="permissions"></a>Machtigingen
De meeste PowerShell taken moet dat u beheerdersbevoegdheden hebben op de Analysis Services-server die u beheert. Geplande taken van PowerShell zijn zonder toezicht bewerkingen. Het account of -service principe met de planner moet beheerdersbevoegdheden hebben op de Analysis Services-server. 

Serverbewerkingen met behulp van cmdlets AzureRm, uw account of het scheduler-account moet ook deel uitmaken van de rol van eigenaar voor de resource in [gebaseerd toegangsbeheer (RBAC)](../role-based-access-control/overview.md). 

## <a name="resource-management-operations"></a>Resource-beheerbewerkingen 
Module - [AzureRM.AnalysisServices](https://www.powershellgallery.com/packages/AzureRM.AnalysisServices)

|Cmdlet|Beschrijving| 
|------------|-----------------| 
|[Get-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/get-azurermanalysisservicesserver)|Details van een server-exemplaar wordt opgehaald.|  
|[New-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver)|Hiermee maakt u een server-exemplaar.|   
|[Nieuwe AzureRmAnalysisServicesFirewallConfig](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesfirewallconfig)|Maakt een nieuwe configuratie van de Analysis Services-firewall.|   
|[Nieuwe AzureRmAnalysisServicesFirewallRule](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesfirewallrule)|Maakt een nieuwe firewallregel van Analysis Services.|   
|[Remove-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/remove-azurermanalysisservicesserver)|Hiermee verwijdert u een server-exemplaar.|  
|[Resume-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/resume-azurermanalysisservicesserver)|Hervatten van een server-exemplaar.|  
|[Suspend-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/suspend-azurermanalysisservicesserver)|Een server-exemplaar onderbreekt.| 
|[Set-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/set-azurermanalysisservicesserver)|Hiermee wijzigt u een server-exemplaar.|   
|[Test-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/test-azurermanalysisservicesserver)|Het bestaan van een server-exemplaar wordt gecontroleerd.| 

## <a name="server-management-operations"></a>Server beheerbewerkingen

Module - [Azure.AnalysisServices](https://www.powershellgallery.com/packages/Azure.AnalysisServices)

|Cmdlet|Beschrijving| 
|------------|-----------------| 
|[Add-AzureAnalysisServicesAccount](/powershell/module/azure.analysisservices/add-azureanalysisservicesaccount)|Voegt een geverifieerde account moet worden gebruikt voor de cmdlet serveraanvragen Azure Analysis Services.| 
|[Export AzureAnalysisServicesInstance]()|Exporteert een logboek van een exemplaar van Analysis Services-server in de momenteel aangemelde omgeving zoals opgegeven in de opdracht Add-AzureAnalysisServicesAccount|  
|[Restart-AzureAnalysisServicesInstance](/powershell/module/azurerm.analysisservices/restart-azureanalysisservicesinstance)|Een exemplaar van Analysis Services-server opnieuw wordt opgestart in de omgeving die momenteel is aangemeld. in de opdracht Add-AzureAnalysisServicesAccount opgegeven.|  
|[Synchronisatie AzureAnalysisServicesInstance](/powershell/module/azurerm.analysisservices/restart-azureanalysisservicesinstance)|Een opgegeven database op het opgegeven exemplaar van Analysis Services-server op alle query scaleout exemplaren in de momenteel aangemelde omgeving zoals opgegeven in de opdracht Add-AzureAnalysisServicesAccount synchroniseert|  

## <a name="database-operations"></a>Databasebewerkingen

Azure Analysis Services-database-bewerkingen gebruik van dezelfde [SqlServer module](https://www.powershellgallery.com/packages/SqlServer) als SQL Server Analysis Services. Niet alle cmdlets worden echter ondersteund voor Azure Analysis Services. Voor meer informatie over meer Zie [SQL Server PowerShell](https://docs.microsoft.com/sql/powershell/sql-server-powershell).

De SQL Server-module bevat cmdlets voor het beheer van taak-specifieke database, evenals de algemene Invoke ASCmd cmdlet die een Tabellair Model Scripting Language (TMSL) query of script accepteert. De volgende cmdlets in de SQL Server-module worden ondersteund voor Azure Analysis Services.

  
|Cmdlet|Beschrijving|
|------------|-----------------| 
|[Voeg RoleMember](https://docs.microsoft.com/powershell/module/sqlserver/Add-RoleMember)|Een lid toevoegen aan een databaserol.| 
|[Backup-ASDatabase](https://docs.microsoft.com/powershell/module/sqlserver/backup-asdatabase)|Back-up van een Analysis Services-database.|  
|[Remove-RoleMember](https://docs.microsoft.com/powershell/module/sqlserver/remove-rolemember)|Een lid verwijderen uit een databaserol.|   
|[Invoke-ASCmd](https://docs.microsoft.com/powershell/module/sqlserver/invoke-ascmd)|Een script TMSL uitvoeren.|
|[Invoke-ProcessASDatabase](https://docs.microsoft.com/powershell/module/sqlserver/invoke-processasdatabase)|Verwerken van een database.|  
|[Aanroepen ProcessPartition](https://docs.microsoft.com/powershell/module/sqlserver/invoke-processpartition)|Verwerken van een partitie.| 
|[Aanroepen ProcessTable](https://docs.microsoft.com/powershell/module/sqlserver/invoke-processtable)|Verwerken van een tabel.|  
|[Samenvoegen partitie](https://docs.microsoft.com/powershell/module/sqlserver/merge-partition)|Samenvoegen van een partitie.|  
|[Restore-ASDatabase](https://docs.microsoft.com/powershell/module/sqlserver/restore-asdatabase)|Een Analysis Services-database herstellen.| 
  

## <a name="related-information"></a>Gerelateerde informatie

* [SQL Server PowerShell-Module downloaden](https://docs.microsoft.com/sql/ssms/download-sql-server-ps-module)   
* [Downloaden van SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)   
* [SQL Server-module in PowerShell Gallery](https://www.powershellgallery.com/packages/SqlServer)    
* [Tabellaire Model wilt programmeren voor compatibiliteit niveau 1200 of hoger](https://msdn.microsoft.com/library/mt712541.aspx)
