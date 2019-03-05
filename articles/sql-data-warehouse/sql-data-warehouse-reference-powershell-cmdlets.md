---
title: PowerShell-cmdlets voor Azure SQL Data Warehouse
description: De bovenste PowerShell-cmdlets voor Azure SQL Data Warehouse met inbegrip van hoe u een database pauzeren en hervatten niet vinden.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: ec1f9a8a10f346c4b8e9e8dac364b2687637c204
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/04/2019
ms.locfileid: "57317897"
---
# <a name="powershell-cmdlets-and-rest-apis-for-sql-data-warehouse"></a>PowerShell-cmdlets en REST-API's voor SQL Data Warehouse
Veel SQL Data Warehouse-beheertaken kunnen worden beheerd met behulp van Azure PowerShell-cmdlets of REST-API's.  Hieronder volgen enkele voorbeelden van het gebruik van PowerShell-opdrachten voor het automatiseren van algemene taken in uw SQL Data Warehouse.  Zie het artikel voor enkele goede voorbeelden van de REST [beheren schaalbaarheid met REST][Manage scalability with REST].

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="get-started-with-azure-powershell-cmdlets"></a>Aan de slag met Azure PowerShell-cmdlets
1. Open Windows PowerShell.
2. Voer deze opdrachten om te melden met de Azure Resource Manager en selecteer uw abonnement op de PowerShell-prompt.
   
    ```PowerShell
    Connect-AzAccount
    Get-AzSubscription
    Select-AzSubscription -SubscriptionName "MySubscription"
    ```

## <a name="pause-sql-data-warehouse-example"></a>Voorbeeld van onderbreken SQL datawarehouse
Onderbreken van een database met de naam 'Database02' die wordt gehost op een server met de naam "Server01."  De server zich in een Azure-resourcegroep met de naam "ResourceGroup1."

```Powershell
Suspend-AzSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
```
Een Variant in dit voorbeeld geeft de opgehaalde object [stand-by-AzSqlDatabase][Suspend-AzSqlDatabase].  Als gevolg hiervan is de database is onderbroken. De laatste opdracht toont de resultaten.

```Powershell
$database = Get-AzSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzSqlDatabase
$resultDatabase
```

## <a name="start-sql-data-warehouse-example"></a>Start SQL datawarehouse-voorbeeld
Hervatten van een database met de naam 'Database02' die wordt gehost op een server met de naam "Server01." De server is opgenomen in een resourcegroep met de naam "ResourceGroup1."

```Powershell
Resume-AzSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" -DatabaseName "Database02"
```

Een Variant dit voorbeeld wordt een database met de naam 'Database02' van een server met de naam 'Server01' die is opgenomen in een resourcegroep met de naam "ResourceGroup1." Deze geeft het opgehaalde object [hervatten AzSqlDatabase][Resume-AzSqlDatabase].

```Powershell
$database = Get-AzSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzSqlDatabase
```

> [!NOTE]
> Houd er rekening mee dat als uw server foo.database.Windows.NET "foo" Als de - servernaam in de PowerShell-cmdlets gebruiken.
> 
> 

## <a name="other-supported-powershell-cmdlets"></a>Andere ondersteunde PowerShell-cmdlets
Deze PowerShell-cmdlets worden ondersteund met Azure SQL Data Warehouse.

* [Get-AzSqlDatabase][Get-AzSqlDatabase]
* [Get-AzSqlDeletedDatabaseBackup][Get-AzSqlDeletedDatabaseBackup]
* [Get-AzSqlDatabaseRestorePoints][Get-AzSqlDatabaseRestorePoints]
* [New-AzSqlDatabase][New-AzSqlDatabase]
* [Remove-AzSqlDatabase][Remove-AzSqlDatabase]
* [Restore-AzSqlDatabase][Restore-AzSqlDatabase]
* [Resume-AzSqlDatabase][Resume-AzSqlDatabase]
* [Select-AzSubscription][Select-AzSubscription]
* [Set-AzSqlDatabase][Set-AzSqlDatabase]
* [Suspend-AzSqlDatabase][Suspend-AzSqlDatabase]

## <a name="next-steps"></a>Volgende stappen
Zie voor meer voorbeelden van PowerShell:

* [Maken van een SQL Data Warehouse met behulp van PowerShell][Create a SQL Data Warehouse using PowerShell]
* [Database terugzetten][Database restore]

Zie voor andere taken die kunnen worden geautomatiseerd met PowerShell, [Azure SQL Database-Cmdlets][Azure SQL Database Cmdlets]. Houd er rekening mee dat niet alle Azure SQL Database-cmdlets voor Azure SQL Data Warehouse worden ondersteund.  Zie voor een lijst met taken die kan worden geautomatiseerd met REST [bewerkingen voor Azure SQL Database][Operations for Azure SQL Database].

<!--Image references-->

<!--Article references-->
[How to install and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
[Create a SQL Data Warehouse using PowerShell]: ./create-data-warehouse-powershell.md
[Database restore]: ./sql-data-warehouse-restore-database-powershell.md
[Manage scalability with REST]: ./sql-data-warehouse-manage-compute-rest-api.md

<!--MSDN references-->
[Azure SQL Database Cmdlets]: https://docs.microsoft.com/powershell/module/azurerm.sql
[Operations for Azure SQL Database]: https://msdn.microsoft.com/library/azure/dn505719.aspx
[Get-AzSqlDatabase]: https://docs.microsoft.com/powershell/module/azurerm.sql/get-Azsqldatabase
[Get-AzSqlDeletedDatabaseBackup]: https://docs.microsoft.com/powershell/module/azurerm.sql/get-AzSqlDeletedDatabaseBackup
[Get-AzSqlDatabaseRestorePoints]: https://docs.microsoft.com/powershell/module/azurerm.sql/get-AzSqlDatabaseRestorePoints
[New-AzSqlDatabase]: https://docs.microsoft.com/powershell/module/azurerm.sql/New-AzSqlDatabase
[Remove-AzSqlDatabase]: https://docs.microsoft.com/powershell/module/azurerm.sql/Remove-AzSqlDatabase
[Restore-AzSqlDatabase]: https://docs.microsoft.com/powershell/module/azurerm.sql/Restore-AzSqlDatabase
[Resume-AzSqlDatabase]: https://docs.microsoft.com/powershell/module/azurerm.sql/Resume-AzSqlDatabase
<!-- It appears that Select-AzSubscription isn't documented, so this points to Select-AzureSubscription -->
[Select-AzSubscription]: https://msdn.microsoft.com/library/dn722499.aspx
[Set-AzSqlDatabase]: https://docs.microsoft.com/powershell/module/azurerm.sql/Set-AzSqlDatabase
[Suspend-AzSqlDatabase]: https://docs.microsoft.com/powershell/module/azurerm.sql/Suspend-AzSqlDatabase

<!--Other Web references-->
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
