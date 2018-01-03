---
title: Herstellen van een Azure SQL datawarehouse (PowerShell) | Microsoft Docs
description: PowerShell-taken voor het herstellen van een Azure SQL Data Warehouse.
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jenniehubbard
editor: 
ms.assetid: ac62f154-c8b0-4c33-9c42-f480808aa1d2
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: backup-restore
ms.date: 12/06/2017
ms.author: barbkess
ms.openlocfilehash: 3b39e87946f3787c71872c7b98db419f1f239e7a
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="restore-an-azure-sql-data-warehouse-powershell"></a>Herstellen van een Azure SQL datawarehouse (PowerShell)
> [!div class="op_single_selector"]
> * [Overzicht][Overview]
> * [Portal][Portal]
> * [PowerShell][PowerShell]
> * [REST][REST]
> 
> 

In dit artikel leert u hoe u een Azure SQL Data Warehouse met behulp van PowerShell.

## <a name="before-you-begin"></a>Voordat u begint
**Controleer of de capaciteit van de DTU.** Elke SQL Data Warehouse wordt gehost door een SQL-server (bijvoorbeeld myserver.database.windows.net) met een standaard DTU-quotum.  Voordat u een SQL Data Warehouse herstellen kunt, controleren of de SQL server heeft onvoldoende resterende DTU-quotum voor de database wordt hersteld. Zie voor meer informatie over het DTU nodig berekenen of dat er meer DTU, [een wijziging DTU-quotum aanvragen][Request a DTU quota change].

### <a name="install-powershell"></a>PowerShell installeren
U moet Azure PowerShell 1.0 of hoger installeren om Azure PowerShell gebruiken met SQL Data Warehouse.  U kunt uw versie controleren door te voeren **Get-Module - ListAvailable-Name AzureRM**.  De meest recente versie kan worden geïnstalleerd vanaf [Microsoft Web Platform Installer][Microsoft Web Platform Installer].  Zie [Azure PowerShell installeren en configureren][How to install and configure Azure PowerShell] voor meer informatie over het installeren van de meest recente versie.

## <a name="restore-an-active-or-paused-database"></a>Herstel de database van een actieve of onderbroken
Een database te herstellen van het gebruik van een momentopname van de [Restore-AzureRmSqlDatabase] [ Restore-AzureRmSqlDatabase] PowerShell-cmdlet.

1. Open Windows PowerShell.
2. Verbinding maken met uw Azure-account en de lijst van alle abonnementen die zijn gekoppeld aan uw account.
3. Selecteer het abonnement met de database te herstellen.
4. Lijst van de herstelpunten voor de database.
5. Kies het gewenste herstelpunt met behulp van de RestorePointCreationDate.
6. De database herstellen naar het gewenste herstelpunt.
7. Controleer of de teruggezette database online is.

```Powershell

$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# List the last 10 database restore points
((Get-AzureRMSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName ($DatabaseName)).RestorePointCreationDate)[-10 .. -1]

# Or list all restore points
Get-AzureRmSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Get the specific database to restore
$Database = Get-AzureRmSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Pick desired restore point using RestorePointCreationDate
$PointInTime="<RestorePointCreationDate>"  

# Restore database from a restore point
$RestoredDatabase = Restore-AzureRmSqlDatabase –FromPointInTimeBackup –PointInTime $PointInTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.$ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $Database.ResourceID

# Verify the status of restored database
$RestoredDatabase.status

```

> [!NOTE]
> Nadat het herstel is voltooid, kunt u de herstelde database configureren door [uw database na herstel configureren][Configure your database after recovery].
> 
> 

## <a name="restore-a-deleted-database"></a>Een verwijderde database herstellen
Als u een verwijderde database herstellen, gebruikt u de [Restore-AzureRmSqlDatabase] [ Restore-AzureRmSqlDatabase] cmdlet.

1. Open Windows PowerShell.
2. Verbinding maken met uw Azure-account en de lijst van alle abonnementen die zijn gekoppeld aan uw account.
3. Selecteer het abonnement met de verwijderde database moet worden hersteld.
4. De specifieke verwijderde database ophalen.
5. De verwijderde database herstellen.
6. Controleer of de teruggezette database online is.

```Powershell
$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# Get the deleted database to restore
$DeletedDatabase = Get-AzureRmSqlDeletedDatabaseBackup -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Restore deleted database
$RestoredDatabase = Restore-AzureRmSqlDatabase –FromDeletedDatabaseBackup –DeletionDate $DeletedDatabase.DeletionDate -ResourceGroupName $DeletedDatabase.ResourceGroupName -ServerName $DeletedDatabase.ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $DeletedDatabase.ResourceID

# Verify the status of restored database
$RestoredDatabase.status
```

> [!NOTE]
> Nadat het herstel is voltooid, kunt u de herstelde database configureren door [uw database na herstel configureren][Configure your database after recovery].
> 
> 

## <a name="restore-from-an-azure-geographical-region"></a>Herstellen van een Azure-geografische regio
Als u wilt een database herstelt, gebruiken de [Restore-AzureRmSqlDatabase] [ Restore-AzureRmSqlDatabase] cmdlet.

1. Open Windows PowerShell.
2. Verbinding maken met uw Azure-account en de lijst van alle abonnementen die zijn gekoppeld aan uw account.
3. Selecteer het abonnement met de database te herstellen.
4. Haal de database die u wilt herstellen.
5. De aanvraag voor het herstel voor de database maken.
6. Controleer de status van de database geo hersteld.

```Powershell
Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName "<Subscription_name>"

# Get the database you want to recover
$GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourServerName>" -DatabaseName "<YourDatabaseName>"

# Recover database
$GeoRestoredDatabase = Restore-AzureRmSqlDatabase –FromGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourTargetServer>" -TargetDatabaseName "<NewDatabaseName>" –ResourceId $GeoBackup.ResourceID

# Verify that the geo-restored database is online
$GeoRestoredDatabase.status
```

> [!NOTE]
> Voor het configureren van uw database nadat het herstel is voltooid. Zie [uw database na herstel configureren][Configure your database after recovery].
> 
> 

De herstelde database worden TDE ingeschakeld als de brondatabase TDE ingeschakeld is.

## <a name="next-steps"></a>Volgende stappen
Lees voor meer informatie over de zakelijke continuïteit functies van Azure SQL Database-versies, de [Azure SQL Database business continuity overview][Azure SQL Database business continuity overview].

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
[Restore-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx

<!--Other Web references-->
[Azure Portal]: https://portal.azure.com/
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
