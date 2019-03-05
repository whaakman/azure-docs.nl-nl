---
title: Herstellen van een Azure SQL datawarehouse (PowerShell) | Microsoft Docs
description: PowerShell-taken voor het herstellen van een Azure SQL Data Warehouse.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 6ad65e5e5d72d32442107a8f645c86f6f89157fd
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/04/2019
ms.locfileid: "57312287"
---
# <a name="restore-an-azure-sql-data-warehouse-powershell"></a>Herstellen van een Azure SQL datawarehouse (PowerShell)
> [!div class="op_single_selector"]
> * [Overzicht][Overview]
> * [Portal][Portal]
> * [PowerShell][PowerShell]
> * [REST][REST]
> 
> 

In dit artikel leert u hoe u kunt herstellen van een Azure SQL Data Warehouse met behulp van PowerShell.

## <a name="before-you-begin"></a>Voordat u begint

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

**Controleer of de capaciteit van uw DTU.** Elke SQL Data Warehouse wordt gehost door een SQL-server (bijvoorbeeld myserver.database.windows.net) met een standaard DTU-quotum.  Voordat u een SQL Data Warehouse herstelt kunt, controleren of de SQL server heeft onvoldoende resterende DTU-quotum voor de database wordt hersteld. Zie voor meer informatie over het berekenen van DTU nodig of om aan te vragen van meer DTU, [een wijziging van de DTU-quotum aanvragen][Request a DTU quota change].

### <a name="install-powershell"></a>PowerShell installeren
Om Azure PowerShell gebruiken met SQL Data Warehouse, moet u Azure PowerShell installeren.  U kunt uw versie controleren door te voeren **Get-Module - ListAvailable-Name Az**.  Zie [Azure PowerShell installeren en configureren][How to install and configure Azure PowerShell] voor meer informatie over het installeren van de meest recente versie.

## <a name="restore-an-active-or-paused-database"></a>Herstellen van een database actief of onderbroken
Een database te herstellen van een momentopname van het gebruik de [terugzetten AzSqlDatabase] [ Restore-AzSqlDatabase] PowerShell-cmdlet.

1. Open Windows PowerShell.
2. Verbinding maken met uw Azure-account en alle abonnementen die zijn gekoppeld aan uw account vermelden.
3. Selecteer het abonnement met de database te herstellen.
4. Een lijst van de herstelpunten voor de database.
5. Kies het gewenste herstelpunt met behulp van de RestorePointCreationDate.
6. De database herstellen naar het gewenste herstelpunt.
7. Controleer of de herstelde database online is.

```Powershell

$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Connect-AzAccount
Get-AzSubscription
Select-AzSubscription -SubscriptionName $SubscriptionName

# List the last 10 database restore points
((Get-AzSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName ($DatabaseName)).RestorePointCreationDate)[-10 .. -1]

# Or list all restore points
Get-AzSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Get the specific database to restore
$Database = Get-AzSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Pick desired restore point using RestorePointCreationDate
$PointInTime="<RestorePointCreationDate>"  

# Restore database from a restore point
$RestoredDatabase = Restore-AzSqlDatabase –FromPointInTimeBackup –PointInTime $PointInTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $Database.ResourceID

# Verify the status of restored database
$RestoredDatabase.status

```

> [!NOTE]
> Nadat het herstel is voltooid, kunt u de herstelde database configureren door [configureren van uw database na het herstel][Configure your database after recovery].
> 
> 

## <a name="restore-a-deleted-database"></a>Een verwijderde database herstellen
Als u een verwijderde database herstellen, gebruikt u de [terugzetten AzSqlDatabase] [ Restore-AzSqlDatabase] cmdlet.

1. Open Windows PowerShell.
2. Verbinding maken met uw Azure-account en alle abonnementen die zijn gekoppeld aan uw account vermelden.
3. Selecteer het abonnement met de verwijderde database kunnen worden hersteld.
4. De specifieke verwijderde database ophalen.
5. De verwijderde database herstellen.
6. Controleer of de herstelde database online is.

```Powershell
$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Connect-AzAccount
Get-AzSubscription
Select-AzSubscription -SubscriptionName $SubscriptionName

# Get the deleted database to restore
$DeletedDatabase = Get-AzSqlDeletedDatabaseBackup -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Restore deleted database
$RestoredDatabase = Restore-AzSqlDatabase –FromDeletedDatabaseBackup –DeletionDate $DeletedDatabase.DeletionDate -ResourceGroupName $DeletedDatabase.ResourceGroupName -ServerName $DeletedDatabase.ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $DeletedDatabase.ResourceID

# Verify the status of restored database
$RestoredDatabase.status
```

> [!NOTE]
> Nadat het herstel is voltooid, kunt u de herstelde database configureren door [configureren van uw database na het herstel][Configure your database after recovery].
> 
> 

## <a name="restore-from-an-azure-geographical-region"></a>Herstellen van een Azure-geografische regio
Als u wilt een database herstelt, gebruikt u de [terugzetten AzSqlDatabase] [ Restore-AzSqlDatabase] cmdlet.

> [!NOTE]
> U kunt een geo-herstel om de geoptimaliseerd uitvoeren voor de Compute-prestatielaag! Om dit te doen, geeft u een geoptimaliseerd voor berekenen ServiceObjectiveName als een optionele parameter. 
>
> 

1. Open Windows PowerShell.
2. Verbinding maken met uw Azure-account en alle abonnementen die zijn gekoppeld aan uw account vermelden.
3. Selecteer het abonnement met de database te herstellen.
4. Ophalen van de database die u wilt herstellen.
5. De aanvraag voor het herstel voor de database maken.
6. Controleer of de status van de database geo hersteld.

```Powershell
Connect-AzAccount
Get-AzSubscription
Select-AzSubscription -SubscriptionName "<Subscription_name>"

# Get the database you want to recover
$GeoBackup = Get-AzSqlDatabaseGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourServerName>" -DatabaseName "<YourDatabaseName>"

# Recover database
$GeoRestoredDatabase = Restore-AzSqlDatabase –FromGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourTargetServer>" -TargetDatabaseName "<NewDatabaseName>" –ResourceId $GeoBackup.ResourceID -ServiceObjectiveName "<YourTargetServiceLevel>"

# Verify that the geo-restored database is online
$GeoRestoredDatabase.status
```

> [!NOTE]
> Zie configureren van uw database nadat het herstel is voltooid, [configureren van uw database na het herstel][Configure your database after recovery].
> 
> 

De herstelde database worden TDE is ingeschakeld als de brondatabase TDE is ingeschakeld is.

## <a name="next-steps"></a>Volgende stappen
Lees voor meer informatie over de functies voor bedrijfscontinuïteit van Azure SQL Database-edities, de [Azure SQL Database-overzicht voor zakelijke continuïteit][Azure SQL Database business continuity overview].

<!--Image references-->

<!--Article references-->
[Azure SQL Database business continuity overview]: ../sql-database/sql-database-business-continuity.md
[Request a DTU quota change]: ./sql-data-warehouse-get-started-create-support-ticket.md
[Configure your database after recovery]: ../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery
[How to install and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
[Overview]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md
[Configure your database after recovery]: ../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery

<!--MSDN references-->
[Restore-AzSqlDatabase]: https://docs.microsoft.com/powershell/module/azurerm.sql/restore-Azsqldatabase

<!--Other Web references-->
[Azure Portal]: https://portal.azure.com/
