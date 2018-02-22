---
title: Beheren van Azure analyseservices met PowerShell | Microsoft Docs
description: Azure Analysis Services-beheer met PowerShell.
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
ms.assetid: 
ms.service: analysis-services
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/14/2018
ms.author: owend
ms.openlocfilehash: 0e154c7789b38843cd3784daa8cf60d078bdc546
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
---
# <a name="manage-azure-analysis-services-with-powershell"></a>Azure analyseservices met PowerShell beheren

In dit artikel beschrijft de PowerShell-cmdlets gebruikt voor het Azure Analysis Services-server en database-beheertaken uitvoeren. 

Server-beheertaken zoals maken of verwijderen van een server, onderbreken of hervatten van serverbewerkingen of wijzigen van het serviceniveau (laag) cmdlets van Azure Resource Manager (AzureRM) gebruiken. Andere taken voor het beheren van databases zoals het toevoegen of verwijderen van leden van een rol, verwerkt of partitioneren gebruiken cmdlets die zijn opgenomen in dezelfde module SqlServer als SQL Server Analysis Services.

## <a name="permissions"></a>Machtigingen
De meeste PowerShell taken moet dat u beheerdersbevoegdheden hebben op de Analysis Services-server die u beheert. Geplande taken van PowerShell zijn zonder toezicht bewerkingen. Het account dat de planner moet beheerdersbevoegdheden hebben op de Analysis Services-server. 

Serverbewerkingen met behulp van cmdlets AzureRm, uw account of het scheduler-account moet ook deel uitmaken van de rol van eigenaar voor de resource in [gebaseerd toegangsbeheer (RBAC)](../active-directory/role-based-access-control-what-is.md). 

## <a name="server-operations"></a>Bewerkingen van server 
Azure Analysis Services-cmdlets zijn opgenomen in de [AzureRM.AnalysisServices](https://www.powershellgallery.com/packages/AzureRM.AnalysisServices) onderdeel module. Zie het installeren van de cmdlet-modules AzureRM [Azure Resource Manager-cmdlets](/powershell/azure/overview) in de PowerShell-galerie.

|Cmdlet|Beschrijving| 
|------------|-----------------| 
|[Add-AzureAnalysisServicesAccount](/powershell/module/azurerm.analysisservices/add-azureanalysisservicesaccount)|Voegt een geverifieerde account moet worden gebruikt voor de cmdlet serveraanvragen Azure Analysis Services.| 
|[Get-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/get-azurermanalysisservicesserver)|Details van een server-exemplaar wordt opgehaald.|  
|[New-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver)|Hiermee maakt u een server-exemplaar.|   
|[Remove-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/remove-azurermanalysisservicesserver)|Hiermee verwijdert u een server-exemplaar.|  
|[Restart-AzureAnalysisServicesInstance](/powershell/module/azurerm.analysisservices/restart-azureanalysisservicesinstance)|Een exemplaar van Analysis Services-server opnieuw wordt opgestart in de omgeving die momenteel is aangemeld. in de opdracht Add-AzureAnalysisServicesAccount opgegeven.|  
|[Resume-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/resume-azurermanalysisservicesserver)|Hervatten van een server-exemplaar.|  
|[Suspend-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/suspend-azurermanalysisservicesserver)|Een server-exemplaar onderbreekt.| 
|[Set-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/set-azurermanalysisservicesserver)|Hiermee wijzigt u een server-exemplaar.|   
|[Test-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/test-azurermanalysisservicesserver)|Het bestaan van een server-exemplaar wordt gecontroleerd.| 

## <a name="database-operations"></a>Databasebewerkingen

Azure Analysis Services-database-bewerkingen gebruik van dezelfde [SqlServer](https://www.powershellgallery.com/packages/SqlServer) module als SQL Server Analysis Services. Niet alle cmdlets worden echter ondersteund voor Azure Analysis Services. 

De SQL Server-module bevat cmdlets voor het beheer van taak-specifieke database, evenals de algemene Invoke ASCmd cmdlet die een Tabellair Model Scripting Language (TMSL) query of script accepteert. De volgende cmdlets in de SQL Server-module worden ondersteund voor Azure Analysis Services.

  
|Cmdlet|Beschrijving|
|------------|-----------------| 
|[Add-RoleMember](https://msdn.microsoft.com/library/hh510167.aspx)|Een lid toevoegen aan een databaserol.| 
|[Backup-ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/backup-asdatabase-cmdlet)|Back-up van een Analysis Services-database.|  
|[Remove-RoleMember](https://msdn.microsoft.com/library/hh510173.aspx)|Een lid verwijderen uit een databaserol.|   
|[Invoke-ASCmd](https://msdn.microsoft.com/library/hh479579.aspx)|Een script TMSL uitvoeren.|
|[Invoke-ProcessASDatabase](https://msdn.microsoft.com/library/mt651773.aspx)|Verwerken van een database.|  
|[Invoke-ProcessPartition](https://msdn.microsoft.com/library/hh510164.aspx)|Verwerken van een partitie.| 
|[Invoke-ProcessTable](https://msdn.microsoft.com/library/mt651774.aspx)|Verwerken van een tabel.|  
|[Merge-Partition](https://msdn.microsoft.com/library/hh479576.aspx)|Samenvoegen van een partitie.|  
|[Restore-ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/restore-asdatabase-cmdlet)|Een Analysis Services-database herstellen.| 
  

## <a name="related-information"></a>Gerelateerde informatie

* [SQL Server PowerShell-Module downloaden](https://docs.microsoft.com/sql/ssms/download-sql-server-ps-module)   
* [Downloaden van SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)   
* [SQL Server-module in PowerShell Gallery](https://www.powershellgallery.com/packages/SqlServer)    
* [Tabellaire Model wilt programmeren voor compatibiliteit niveau 1200 of hoger](https://msdn.microsoft.com/library/mt712541.aspx)
