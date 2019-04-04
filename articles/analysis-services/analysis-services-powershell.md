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
ms.openlocfilehash: 1f9c30f1c914f6c8d42967e014d967ba0d5b85cc
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58893840"
---
# <a name="manage-azure-analysis-services-with-powershell"></a>Azure analyseservices beheren met PowerShell

Dit artikel beschrijft de PowerShell-cmdlets gebruikt voor het uitvoeren van Azure Analysis Services-server en database-beheertaken. 

Server-beheertaken, zoals het maken of verwijderen van een server, onderbreken of hervatten van serverbewerkingen of wijzigen van het serviceniveau (laag) gebruikt u Azure Resource Manager (resource)-cmdlets en cmdlets voor Analysis Services (server). Andere taken voor het beheren van databases wilt toevoegen of verwijderen van leden van een rol, verwerkt of gebruikt u de cmdlets opgenomen in de dezelfde SQL Server-module als SQL Server Analysis Services partitioneren.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="permissions"></a>Machtigingen

De meeste PowerShell-taken moet dat u beheerdersbevoegdheden hebben op de Analysis Services-server die u beheert. PowerShell-taken zijn zonder toezicht bewerkingen. Het account of service-principal die de scheduler uitvoeren, moet beheerdersbevoegdheden hebben op de Analysis Services-server. 

Voor serverbewerkingen met behulp van Azure PowerShell-cmdlets, uw account of de scheduler-account moet ook behoren tot de rol van eigenaar voor de resource in [op rollen gebaseerd toegangsbeheer (RBAC)](../role-based-access-control/overview.md). 

## <a name="resource-management-operations"></a>Resource management-bewerkingen 

Module - [Az.AnalysisServices](/powershell/module/az.analysisservices)

|Cmdlet|Description| 
|------------|-----------------| 
|[Get-AzAnalysisServicesServer](/powershell/module/az.analysisservices/get-azanalysisservicesserver)|Hiermee haalt u details van een server-exemplaar.|  
|[New-AzAnalysisServicesServer](/powershell/module/az.analysisservices/new-azanalysisservicesserver)|Hiermee maakt u een server-exemplaar.|   
|[New-AzAnalysisServicesFirewallConfig](/powershell/module/az.analysisservices/new-azanalysisservicesfirewallconfig)|Hiermee maakt u een nieuwe configuratie van de Analysis Services-firewall.|   
|[New-AzAnalysisServicesFirewallRule](/powershell/module/az.analysisservices/new-azanalysisservicesfirewallrule)|Hiermee maakt u een nieuwe firewallregel van Analysis Services.|   
|[Remove-AzAnalysisServicesServer](/powershell/module/az.analysisservices/remove-azanalysisservicesserver)|Hiermee verwijdert u een server-exemplaar.|  
|[Resume-AzAnalysisServicesServer](/powershell/module/az.analysisservices/resume-azanalysisservicesserver)|Hervat een exemplaar van server.|  
|[Suspend-AzAnalysisServicesServer](/powershell/module/az.analysisservices/suspend-azanalysisservicesserver)|Een server-exemplaar onderbreekt.| 
|[Set-AzAnalysisServicesServer](/powershell/module/az.analysisservices/set-azanalysisservicesserver)|Hiermee wijzigt u een server-exemplaar.|   
|[Test-AzAnalysisServicesServer](/powershell/module/az.analysisservices/test-azanalysisservicesserver)|Test het bestaan van een server-exemplaar.| 

## <a name="server-management-operations"></a>Server management-bewerkingen

Module - [Azure.AnalysisServices](https://www.powershellgallery.com/packages/Azure.AnalysisServices)

|Cmdlet|Description| 
|------------|-----------------| 
|[Add-AzAnalysisServicesAccount](/powershell/module/az.analysisservices/add-AzAnalysisServicesaccount)|Een geverifieerde account moet worden gebruikt voor Azure Analysis Services-serveraanvragen cmdlet toegevoegd.| 
|[Export-AzAnalysisServicesInstance](/powershell/module/az.analysisservices/export-AzAnalysisServicesinstancelog)|Hiermee exporteert u een logboek van een exemplaar van Analysis Services-server in de momenteel aangemelde omgeving zoals opgegeven in de opdracht Add-AzAnalysisServicesAccount|  
|[Restart-AzAnalysisServicesInstance](/powershell/module/az.analysisservices/restart-AzAnalysisServicesinstance)|Een exemplaar van Analysis Services-server opnieuw wordt opgestart in de omgeving die momenteel is aangemeld. in de opdracht Add-AzAnalysisServicesAccount hebt opgegeven.|  
|[Sync-AzAnalysisServicesInstance](/powershell/module/az.analysisservices/restart-AzAnalysisServicesinstance)|Een opgegeven database op het opgegeven exemplaar van Analysis Services-server op alle query scale-out-exemplaren in de momenteel aangemelde omgeving zoals opgegeven in de opdracht Add-AzAnalysisServicesAccount synchroniseert|  

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
|[Invoke-ProcessPartition](https://docs.microsoft.com/powershell/module/sqlserver/invoke-processpartition)|Een partitie worden verwerkt.| 
|[Invoke-ProcessTable](https://docs.microsoft.com/powershell/module/sqlserver/invoke-processtable)|Het proces een tabel.|  
|[Partitie samenvoegen](https://docs.microsoft.com/powershell/module/sqlserver/merge-partition)|Een partitie samenvoegen.|  
|[Restore-ASDatabase](https://docs.microsoft.com/powershell/module/sqlserver/restore-asdatabase)|Herstellen van een Analysis Services-database.| 
  

## <a name="related-information"></a>Gerelateerde informatie

* [SQL Server PowerShell-Module downloaden](https://docs.microsoft.com/sql/ssms/download-sql-server-ps-module)   
* [SSMS downloaden](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)   
* [SQL Server-module in PowerShell Gallery](https://www.powershellgallery.com/packages/SqlServer)    
* [Tabellair Model wilt programmeren voor compatibiliteit niveau 1200 en hoger](/sql/analysis-services/tabular-model-programming-compatibility-level-1200/tabular-model-programming-for-compatibility-level-1200)
