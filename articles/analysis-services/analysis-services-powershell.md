---
title: Azure analyseservices beheren met PowerShell | Microsoft Docs
description: Azure Analysis Services-beheer met PowerShell.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: reference
ms.date: 07/01/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: a958c33e173c881a3ad09a49fe9f71ddb0c9df56
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2019
ms.locfileid: "67508941"
---
# <a name="manage-azure-analysis-services-with-powershell"></a>Azure analyseservices beheren met PowerShell

Dit artikel beschrijft de PowerShell-cmdlets gebruikt voor het uitvoeren van Azure Analysis Services-server en database-beheertaken. 

Server-beheertaken, zoals het maken of verwijderen van een server, onderbreken of hervatten van serverbewerkingen of wijzigen van het serviceniveau (laag) resource Azure Analysis Services-cmdlets gebruiken. Andere taken voor het beheren van databases wilt toevoegen of verwijderen van leden van een rol, verwerkt of gebruikt u de cmdlets opgenomen in de dezelfde SQL Server-module als SQL Server Analysis Services partitioneren.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="permissions"></a>Machtigingen

De meeste PowerShell-taken moet dat u beheerdersbevoegdheden hebben op de Analysis Services-server die u beheert. PowerShell-taken zijn zonder toezicht bewerkingen. Het account of service-principal die de scheduler uitvoeren, moet beheerdersbevoegdheden hebben op de Analysis Services-server. 

Voor serverbewerkingen met behulp van Azure PowerShell-cmdlets, uw account of de scheduler-account moet ook behoren tot de rol van eigenaar voor de resource in [op rollen gebaseerd toegangsbeheer (RBAC)](../role-based-access-control/overview.md). 

## <a name="resource-and-server-operations"></a>Resource-en-server 

Module - installeren [Az.AnalysisServices](https://www.powershellgallery.com/packages/Az.AnalysisServices)   
Documentatie - [Az.AnalysisServices verwijzing](/powershell/module/az.analysisservices)

## <a name="database-operations"></a>Databasebewerkingen

Azure Analysis Services-database-bewerkingen gebruik dezelfde SQL Server-module als SQL Server Analysis Services. Niet alle cmdlets worden echter ondersteund voor Azure Analysis Services. 

De SQL Server-module biedt taakspecifieke database management-cmdlets, evenals de voor algemeen gebruik Invoke-ASCmd cmdlet die een TMSL Tabular Model Scripting Language ()-query of het script accepteert. De volgende cmdlets in de SQL Server-module worden voor Azure Analysis Services ondersteund.

Module - installeren [SqlServer](https://www.powershellgallery.com/packages/SqlServer)   
Documentatie - [SqlServer-verwijzing](/powershell/module/sqlserver)

### <a name="supported-cmdlets"></a>Ondersteunde cmdlets

|Cmdlet|Description|
|------------|-----------------| 
|[Add-RoleMember](https://docs.microsoft.com/powershell/module/sqlserver/Add-RoleMember)|Lid toevoegen aan een databaserol.| 
|[Backup-ASDatabase](https://docs.microsoft.com/powershell/module/sqlserver/backup-asdatabase)|Back-up van een Analysis Services-database.|  
|[Remove-RoleMember](https://docs.microsoft.com/powershell/module/sqlserver/remove-rolemember)|Een lid verwijderen uit een databaserol.|   
|[Invoke-ASCmd](https://docs.microsoft.com/powershell/module/sqlserver/invoke-ascmd)|Voer een TMSL-script.|
|[Invoke-ProcessASDatabase](https://docs.microsoft.com/powershell/module/sqlserver/invoke-processasdatabase)|Het proces een database.|  
|[Invoke-ProcessPartition](https://docs.microsoft.com/powershell/module/sqlserver/invoke-processpartition)|Een partitie worden verwerkt.| 
|[Invoke-ProcessTable](https://docs.microsoft.com/powershell/module/sqlserver/invoke-processtable)|Het proces een tabel.|  
|[Merge-Partition](https://docs.microsoft.com/powershell/module/sqlserver/merge-partition)|Een partitie samenvoegen.|  
|[Restore-ASDatabase](https://docs.microsoft.com/powershell/module/sqlserver/restore-asdatabase)|Herstellen van een Analysis Services-database.| 
  

## <a name="related-information"></a>Gerelateerde informatie

* [SQL Server PowerShell](https://docs.microsoft.com/sql/powershell/sql-server-powershell)      
* [SQL Server PowerShell-Module downloaden](https://docs.microsoft.com/sql/ssms/download-sql-server-ps-module)   
* [SSMS downloaden](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)   
* [SQL Server-module in PowerShell Gallery](https://www.powershellgallery.com/packages/SqlServer)    
* [Tabellair Model wilt programmeren voor compatibiliteit niveau 1200 en hoger](/sql/analysis-services/tabular-model-programming-compatibility-level-1200/tabular-model-programming-for-compatibility-level-1200)
