---
title: PowerShell-cmdlets voor Azure SQL Data Warehouse
description: De bovenste PowerShell-cmdlets voor Azure SQL Data Warehouse, inclusief het onderbreken en hervatten van een database niet vinden.
services: sql-data-warehouse
documentationcenter: NA
author: kevinvngo
manager: jhubbard
editor: 
ms.assetid: 6f0d5772-f05f-4cc8-9749-4adb153dfd50
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: reference
ms.date: 10/31/2016
ms.author: kevin;barbkess
ms.openlocfilehash: ce3e11587c2e0cb92923868a4f26d7f59c7ef4ca
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="powershell-cmdlets-and-rest-apis-for-sql-data-warehouse"></a>PowerShell-cmdlets en REST-API's voor SQL Data Warehouse
Veel SQL Data Warehouse-beheertaken kunnen worden beheerd met behulp van Azure PowerShell-cmdlets of REST-API's.  Hieronder volgen enkele voorbeelden van het PowerShell-opdrachten gebruiken voor het automatiseren van algemene taken in uw SQL Data Warehouse.  Zie het artikel voor een goede voorbeelden van de REST [beheren schaalbaarheid met REST][Manage scalability with REST].

> [!NOTE]
> Om Azure PowerShell gebruiken met SQL Data Warehouse, moet u Azure PowerShell-versie 1.0.3 of hoger.  U kunt uw versie controleren door te voeren **Get-Module - ListAvailable-Name Azure**.  De meest recente versie kan worden geïnstalleerd vanaf [Microsoft Web Platform Installer][Microsoft Web Platform Installer].  Zie [Azure PowerShell installeren en configureren][How to install and configure Azure PowerShell] voor meer informatie over het installeren van de meest recente versie.
> 
> 

## <a name="get-started-with-azure-powershell-cmdlets"></a>Aan de slag met Azure PowerShell-cmdlets
1. Open Windows PowerShell.
2. Voer deze opdrachten aanmelden in de Azure Resource Manager en selecteer uw abonnement op de PowerShell-prompt.
   
    ```PowerShell
    Login-AzureRmAccount
    Get-AzureRmSubscription
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

## <a name="pause-sql-data-warehouse-example"></a>Voorbeeld van onderbreken SQL datawarehouse
Onderbreken van een database met naam 'Database02' gehost op een server met de naam "Server01."  De server zich in een Azure-resourcegroep met de naam 'ResourceGroup1'.

```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
```
Een variatie in dit voorbeeld doorgesluisd het opgehaalde object [Suspend-AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase].  Als gevolg hiervan is de database is onderbroken. De laatste opdracht toont de resultaten.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```

## <a name="start-sql-data-warehouse-example"></a>Start SQL datawarehouse voorbeeld
Hervatten van een database met naam 'Database02' gehost op een server met de naam "Server01." De server deel uitmaakt van een resourcegroep met de naam 'ResourceGroup1'.

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" -DatabaseName "Database02"
```

Een variatie in dit voorbeeld haalt een database met naam 'Database02' van een server met de naam 'Server01' die is opgenomen in een resourcegroep met de naam 'ResourceGroup1'. Deze het opgehaalde object doorgesluisd [Resume-AzureRmSqlDatabase][Resume-AzureRmSqlDatabase].

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
```

> [!NOTE]
> Let op: als uw server foo.database.windows.net, is "foo" als - servernaam in de PowerShell-cmdlets gebruiken.
> 
> 

## <a name="other-supported-powershell-cmdlets"></a>Overige ondersteunde PowerShell-cmdlets
Deze PowerShell-cmdlets worden ondersteund met Azure SQL Data Warehouse.

* [Get-AzureRmSqlDatabase][Get-AzureRmSqlDatabase]
* [Get-AzureRmSqlDeletedDatabaseBackup][Get-AzureRmSqlDeletedDatabaseBackup]
* [Get-AzureRmSqlDatabaseRestorePoints][Get-AzureRmSqlDatabaseRestorePoints]
* [Nieuwe AzureRmSqlDatabase][New-AzureRmSqlDatabase]
* [Verwijder AzureRmSqlDatabase][Remove-AzureRmSqlDatabase]
* [Restore-AzureRmSqlDatabase][Restore-AzureRmSqlDatabase]
* [Resume-AzureRmSqlDatabase][Resume-AzureRmSqlDatabase]
* [SELECT-AzureRmSubscription][Select-AzureRmSubscription]
* [Set-AzureRmSqlDatabase][Set-AzureRmSqlDatabase]
* [Onderbreken AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase]

## <a name="next-steps"></a>Volgende stappen
Zie voor meer voorbeelden van PowerShell:

* [Maken van een SQL Data Warehouse met behulp van PowerShell][Create a SQL Data Warehouse using PowerShell]
* [Database terugzetten][Database restore]

Zie voor andere taken die kunnen worden geautomatiseerd met PowerShell [Azure SQL Database-Cmdlets][Azure SQL Database Cmdlets]. Houd er rekening mee dat niet alle Azure SQL Database-cmdlets voor Azure SQL Data Warehouse worden ondersteund.  Zie voor een lijst van taken die kan worden geautomatiseerd met REST [bewerkingen voor Azure SQL-Databases][Operations for Azure SQL Databases].

<!--Image references-->

<!--Article references-->
[How to install and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
[Create a SQL Data Warehouse using PowerShell]: ./sql-data-warehouse-get-started-provision-powershell.md
[Database restore]: ./sql-data-warehouse-restore-database-powershell.md
[Manage scalability with REST]: ./sql-data-warehouse-manage-compute-rest-api.md

<!--MSDN references-->
[Azure SQL Database Cmdlets]: https://msdn.microsoft.com/library/mt574084.aspx
[Operations for Azure SQL Databases]: https://msdn.microsoft.com/library/azure/dn505719.aspx
[Get-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt603648.aspx
[Get-AzureRmSqlDeletedDatabaseBackup]: https://msdn.microsoft.com/library/mt693387.aspx
[Get-AzureRmSqlDatabaseRestorePoints]: https://msdn.microsoft.com/library/mt603642.aspx
[New-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619339.aspx
[Remove-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619368.aspx
[Restore-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx
[Resume-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619347.aspx
<!-- It appears that Select-AzureRmSubscription isn't documented, so this points to Select-AzureSubscription -->
[Select-AzureRmSubscription]: https://msdn.microsoft.com/library/dn722499.aspx
[Set-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619433.aspx
[Suspend-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619337.aspx

<!--Other Web references-->
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
