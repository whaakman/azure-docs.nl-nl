---
title: PowerShell-cmdlets voor Azure SQL Data Warehouse
description: De bovenste PowerShell-cmdlets voor Azure SQL Data Warehouse met inbegrip van hoe u een database pauzeren en hervatten niet vinden.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 222bc8ee15fdc8802dacd5a5b74cfd84961aa397
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/30/2018
ms.locfileid: "43300750"
---
# <a name="powershell-cmdlets-and-rest-apis-for-sql-data-warehouse"></a>PowerShell-cmdlets en REST-API's voor SQL Data Warehouse
Veel SQL Data Warehouse-beheertaken kunnen worden beheerd met behulp van Azure PowerShell-cmdlets of REST-API's.  Hieronder volgen enkele voorbeelden van het gebruik van PowerShell-opdrachten voor het automatiseren van algemene taken in uw SQL Data Warehouse.  Zie het artikel voor enkele goede voorbeelden van de REST [beheren schaalbaarheid met REST][Manage scalability with REST].

> [!NOTE]
> Als u wilt Azure PowerShell gebruiken met SQL Data Warehouse, moet u Azure PowerShell-versie 1.0.3 of hoger.  U kunt uw versie controleren door te voeren **Get-Module - ListAvailable-Name Azure**.  De meest recente versie kan worden geïnstalleerd vanaf [Microsoft Web Platform Installer][Microsoft Web Platform Installer].  Zie [Azure PowerShell installeren en configureren][How to install and configure Azure PowerShell] voor meer informatie over het installeren van de meest recente versie.
> 
> 

## <a name="get-started-with-azure-powershell-cmdlets"></a>Aan de slag met Azure PowerShell-cmdlets
1. Open Windows PowerShell.
2. Voer deze opdrachten om te melden met de Azure Resource Manager en selecteer uw abonnement op de PowerShell-prompt.
   
    ```PowerShell
    Connect-AzureRmAccount
    Get-AzureRmSubscription
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

## <a name="pause-sql-data-warehouse-example"></a>Voorbeeld van onderbreken SQL datawarehouse
Onderbreken van een database met de naam 'Database02' die wordt gehost op een server met de naam "Server01."  De server zich in een Azure-resourcegroep met de naam "ResourceGroup1."

```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
```
Een Variant in dit voorbeeld geeft de opgehaalde object [stand-by-AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase].  Als gevolg hiervan is de database is onderbroken. De laatste opdracht toont de resultaten.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```

## <a name="start-sql-data-warehouse-example"></a>Start SQL datawarehouse-voorbeeld
Hervatten van een database met de naam 'Database02' die wordt gehost op een server met de naam "Server01." De server is opgenomen in een resourcegroep met de naam "ResourceGroup1."

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" -DatabaseName "Database02"
```

Een Variant dit voorbeeld wordt een database met de naam 'Database02' van een server met de naam 'Server01' die is opgenomen in een resourcegroep met de naam "ResourceGroup1." Deze geeft het opgehaalde object [Resume-AzureRmSqlDatabase][Resume-AzureRmSqlDatabase].

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
```

> [!NOTE]
> Houd er rekening mee dat als uw server foo.database.Windows.NET "foo" Als de - servernaam in de PowerShell-cmdlets gebruiken.
> 
> 

## <a name="other-supported-powershell-cmdlets"></a>Andere ondersteunde PowerShell-cmdlets
Deze PowerShell-cmdlets worden ondersteund met Azure SQL Data Warehouse.

* [Get-AzureRmSqlDatabase][Get-AzureRmSqlDatabase]
* [Get-AzureRmSqlDeletedDatabaseBackup][Get-AzureRmSqlDeletedDatabaseBackup]
* [Get-AzureRmSqlDatabaseRestorePoints][Get-AzureRmSqlDatabaseRestorePoints]
* [Nieuwe-AzureRmSqlDatabase][New-AzureRmSqlDatabase]
* [Remove-AzureRmSqlDatabase][Remove-AzureRmSqlDatabase]
* [Restore-AzureRmSqlDatabase][Restore-AzureRmSqlDatabase]
* [Resume-AzureRmSqlDatabase][Resume-AzureRmSqlDatabase]
* [SELECT-AzureRmSubscription][Select-AzureRmSubscription]
* [Set-AzureRmSqlDatabase][Set-AzureRmSqlDatabase]
* [Suspend-AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase]

## <a name="next-steps"></a>Volgende stappen
Zie voor meer voorbeelden van PowerShell:

* [Maken van een SQL Data Warehouse met behulp van PowerShell][Create a SQL Data Warehouse using PowerShell]
* [Database terugzetten][Database restore]

Zie voor andere taken die kunnen worden geautomatiseerd met PowerShell, [Azure SQL Database-Cmdlets][Azure SQL Database Cmdlets]. Houd er rekening mee dat niet alle Azure SQL Database-cmdlets voor Azure SQL Data Warehouse worden ondersteund.  Zie voor een lijst met taken die kan worden geautomatiseerd met REST [bewerkingen voor Azure SQL-Databases][Operations for Azure SQL Databases].

<!--Image references-->

<!--Article references-->
[How to install and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
[Create a SQL Data Warehouse using PowerShell]: ./create-data-warehouse-powershell.md
[Database restore]: ./sql-data-warehouse-restore-database-powershell.md
[Manage scalability with REST]: ./sql-data-warehouse-manage-compute-rest-api.md

<!--MSDN references-->
[Azure SQL Database Cmdlets]: https://docs.microsoft.com/powershell/module/azurerm.sql
[Operations for Azure SQL Databases]: https://msdn.microsoft.com/library/azure/dn505719.aspx
[Get-AzureRmSqlDatabase]: https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqldatabase
[Get-AzureRmSqlDeletedDatabaseBackup]: https://docs.microsoft.com/powershell/module/azurerm.sql/get-AzureRmSqlDeletedDatabaseBackup
[Get-AzureRmSqlDatabaseRestorePoints]: https://docs.microsoft.com/powershell/module/azurerm.sql/get-AzureRmSqlDatabaseRestorePoints
[New-AzureRmSqlDatabase]: https://docs.microsoft.com/powershell/module/azurerm.sql/New-AzureRmSqlDatabase
[Remove-AzureRmSqlDatabase]: https://docs.microsoft.com/powershell/module/azurerm.sql/Remove-AzureRmSqlDatabase
[Restore-AzureRmSqlDatabase]: https://docs.microsoft.com/powershell/module/azurerm.sql/Restore-AzureRmSqlDatabase
[Resume-AzureRmSqlDatabase]: https://docs.microsoft.com/powershell/module/azurerm.sql/Resume-AzureRmSqlDatabase
<!-- It appears that Select-AzureRmSubscription isn't documented, so this points to Select-AzureSubscription -->
[Select-AzureRmSubscription]: https://msdn.microsoft.com/library/dn722499.aspx
[Set-AzureRmSqlDatabase]: https://docs.microsoft.com/powershell/module/azurerm.sql/Set-AzureRmSqlDatabase
[Suspend-AzureRmSqlDatabase]: https://docs.microsoft.com/powershell/module/azurerm.sql/Suspend-AzureRmSqlDatabase

<!--Other Web references-->
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
