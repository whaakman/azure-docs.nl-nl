---
title: Herstellen van een Azure SQL datawarehouse | Microsoft Docs
description: Stapsgewijze handleiding voor het herstellen van een Azure SQL Data Warehouse.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 08/29/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 5de9c674ceeab6d45211b46e2f03d91b943751ac
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55246208"
---
# <a name="restoring-azure-sql-data-warehouse"></a>Herstellen van Azure SQL datawarehouse 
In dit artikel leert u hoe u in Azure portal en PowerShell het volgende doen:

- Een herstelpunt maken
- Herstellen van een punt voor het automatisch herstellen of de gebruiker gedefinieerde herstelpunt
- Vanuit een verwijderde database herstellen
- Herstellen vanuit een geo-back-up
- Een kopie van uw datawarehouse maken vanuit een door de gebruiker gedefinieerde herstelpunt

## <a name="before-you-begin"></a>Voordat u begint
**Controleer of de capaciteit van uw DTU.** Elke SQL Data Warehouse wordt gehost door een SQL-server (bijvoorbeeld myserver.database.windows.net) met een standaard DTU-quotum.  Voordat u een SQL datawarehouse herstelt kunt, controleren of de SQL server heeft onvoldoende resterende DTU-quotum voor de database wordt hersteld. Zie voor meer informatie over het berekenen van DTU nodig of om aan te vragen van meer DTU, [een wijziging van de DTU-quotum aanvragen][Request a DTU quota change].

## <a name="restore-through-powershell"></a>Herstellen via PowerShell

## <a name="install-powershell"></a>PowerShell installeren
Om Azure PowerShell gebruiken met SQL Data Warehouse, moet u Azure PowerShell-versie 1.0 of hoger installeren.  U kunt uw versie controleren door te voeren **Get-Module - ListAvailable-Name AzureRM**.  De meest recente versie kan worden geïnstalleerd vanaf [Microsoft Web Platform Installer][Microsoft Web Platform Installer].  Zie [Azure PowerShell installeren en configureren][How to install and configure Azure PowerShell] voor meer informatie over het installeren van de meest recente versie.

## <a name="restore-an-active-or-paused-database-using-powershell"></a>Herstellen van een actieve of onderbroken database met behulp van PowerShell
Een database terugzetten vanuit een gebruik van de punt herstellen de [Restore-AzureRmSqlDatabase] [ Restore-AzureRmSqlDatabase] PowerShell-cmdlet.

1. Open Windows PowerShell.

2. Verbinding maken met uw Azure-account en alle abonnementen die zijn gekoppeld aan uw account vermelden.

3. Selecteer het abonnement met de database te herstellen.

4. Een lijst van de herstelpunten voor de database.

5. Kies het gewenste herstelpunt met behulp van de RestorePointCreationDate.

   > [!NOTE]
   > Bij het herstellen, kunt u een andere ServiceObjectiveName (DWU) of een andere server die zich bevinden in een andere regio opgeven.

6. De database herstellen naar het gewenste herstelpunt.

7. Controleer of de herstelde database online is.

```Powershell

$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Connect-AzureRmAccount
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
$RestoredDatabase = Restore-AzureRmSqlDatabase –FromPointInTimeBackup –PointInTime $PointInTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $Database.ResourceID

# Verify the status of restored database
$RestoredDatabase.status

```

> [!NOTE]
> Nadat het herstel is voltooid, kunt u de herstelde database configureren door [configureren van uw database na het herstel][Configure your database after recovery].
>

## <a name="copy-your-data-warehouse-with-user-defined-restore-points-using-powershell"></a>Kopieer uw datawarehouse met de gebruiker gedefinieerde herstelpunten met behulp van PowerShell
Een database te herstellen van een gebruiker gedefinieerde terugzetten punt gebruiken de [Restore-AzureRmSqlDatabase] [ Restore-AzureRmSqlDatabase] PowerShell-cmdlet.

1. Open Windows PowerShell.
2. Verbinding maken met uw Azure-account en alle abonnementen die zijn gekoppeld aan uw account vermelden.
3. Selecteer het abonnement met de database te herstellen.
4. Een herstelpunt voor een onmiddellijke kopie van uw database maken
5. Wijzig de naam van uw database naar een tijdelijke naam.
6. De meest recente herstelpunt door de opgegeven RestorePointLabel ophalen.
7. Ophalen van de resource-id van de database wilt terugzetten starten
8. De database herstellen naar het gewenste herstelpunt.
9. Controleer of de herstelde database online is.

```Powershell

$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$TempDatabaseName = "<YourTemporaryDatabaseName>"
$Label = "<YourRestorePointLabel"

Connect-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# Create a restore point of the original database
New-AzureRmSqlDatabaseRestorePoint -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName -RestorePointLabel $Label

# Rename the database to a temporary name
Set-AzureRmSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName -NewName $TempDatabaseName

# Get the most recent restore point with the specified label
$LabelledRestorePoint = Get-AzureRmSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $TempDatabaseName | where {$_.RestorePointLabel -eq $Label} | sort {$_.RestorePointCreationDate} | select -Last 1

# Get the resource id of the database
$ResourceId = (Get-AzureRmSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $TempDatabaseName).ResourceId

# Restore the database to its original name from the labelled restore point from the temporary database
$RestoredDatabase = Restore-AzureRmSqlDatabase -FromPointInTimeBackup -ResourceGroupName $ResourceGroupName -ServerName $ServerName -ResourceId $ResourceId -PointInTime $LabelledRestorePoint.RestorePointCreationDate -TargetDatabaseName $DatabaseName

# Verify the status of restored database
$RestoredDatabase.status

# The original temporary database can be deleted at this point

```

## <a name="restore-a-deleted-database-using-powershell"></a>Herstellen van een verwijderde database met behulp van PowerShell
Als u een verwijderde database herstellen, gebruikt u de [Restore-AzureRmSqlDatabase] [ Restore-AzureRmSqlDatabase] cmdlet.

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

Connect-AzureRmAccount
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
> Nadat het herstel is voltooid, kunt u de herstelde database configureren door [configureren van uw database na het herstel][Configure your database after recovery].
>

## <a name="restore-from-an-azure-geographical-region-using-powershell"></a>Herstellen van een Azure-geografische regio met behulp van PowerShell
Als u wilt een database herstelt, gebruikt u de [Restore-AzureRmSqlDatabase] [ Restore-AzureRmSqlDatabase] cmdlet.

> [!NOTE]
> U kunt een geo-herstel naar Gen2 uitvoeren! Om dit te doen, Geef een ServiceObjectiveName Gen2 (bijvoorbeeld DW1000**c**) als een optionele parameter.
>

1. Open Windows PowerShell.
2. Verbinding maken met uw Azure-account en alle abonnementen die zijn gekoppeld aan uw account vermelden.
3. Selecteer het abonnement met de database te herstellen.
4. Ophalen van de database die u wilt herstellen.
5. De aanvraag voor het herstel voor de database maken.
6. Controleer of de status van de database geo hersteld.

```Powershell
Connect-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName "<Subscription_name>"

# Get the database you want to recover
$GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourServerName>" -DatabaseName "<YourDatabaseName>"

# Recover database
$GeoRestoredDatabase = Restore-AzureRmSqlDatabase –FromGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourTargetServer>" -TargetDatabaseName "<NewDatabaseName>" –ResourceId $GeoBackup.ResourceID -ServiceObjectiveName "<YourTargetServiceLevel>"

# Verify that the geo-restored database is online
$GeoRestoredDatabase.status
```

> [!NOTE]
> Zie configureren van uw database nadat het herstel is voltooid, [configureren van uw database na het herstel][Configure your database after recovery].
>

De herstelde database worden TDE is ingeschakeld als de brondatabase TDE is ingeschakeld is.

## <a name="restore-through-the-azure-portal"></a>Herstellen via Azure portal

## <a name="create-a-user-defined-restore-point-using-the-azure-portal"></a>Een gebruiker gedefinieerde herstelpunt met behulp van de Azure portal maken
1. Meld u aan bij [Azure Portal][Azure portal].

2. Navigeer naar de SQL datawarehouse die u wilt maken van een herstelpunt voor.

3. Selecteer aan de bovenkant van de blade overzicht **+ nieuw herstelpunt**.

    ![Nieuw herstelpunt](./media/sql-data-warehouse-restore-database-portal/creating_restore_point_0.png)

4. Geef een naam voor het herstelpunt.

    ![Naam van het herstelpunt](./media/sql-data-warehouse-restore-database-portal/creating_restore_point_1.png)

## <a name="restore-an-active-or-paused-database-using-the-azure-portal"></a>Herstellen van een actieve of onderbroken database met behulp van de Azure portal
1. Meld u aan bij [Azure Portal][Azure portal].
2. Navigeer naar de SQL datawarehouse die u herstellen wilt uit.
3. Selecteer aan de bovenkant van de blade overzicht **herstellen**.

    ![ Overzicht van Herstellen](./media/sql-data-warehouse-restore-database-portal/restoring_0.png)

4. Selecteer een **automatisch herstelpunten** of **herstelpunten zelfgedefinieerde**.

    ![Automatische herstelpunten](./media/sql-data-warehouse-restore-database-portal/restoring_1.png)

5. Voor door de gebruiker gedefinieerde herstellen punten **Selecteer een herstelpunt** of **maken van een nieuw herstelpunt voor gebruiker gedefinieerde**.

    ![Herstelpunten gebruiker gedefinieerd](./media/sql-data-warehouse-restore-database-portal/restoring_2_udrp.png)

## <a name="restore-a-deleted-database-using-the-azure-portal"></a>Een verwijderde database herstellen via de Azure-portal
1. Meld u aan bij [Azure Portal][Azure portal].
2. Navigeer naar de SQL-server die de verwijderde database werd gehost op.
3. Selecteer het pictogram van de verwijderde databases in de inhoudsopgave.

    ![Verwijderde Databases](./media/sql-data-warehouse-restore-database-portal/restoring_deleted_0.png)

4. Selecteer de verwijderde database die u wilt herstellen.

    ![Verwijderde Databases selecteren](./media/sql-data-warehouse-restore-database-portal/restoring_deleted_1.png)

5. Geef de naam van een nieuwe database.

    ![Geef de naam van Database](./media/sql-data-warehouse-restore-database-portal/restoring_deleted_2.png)

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
[Restore-AzureRmSqlDatabase]: https://docs.microsoft.com/powershell/module/azurerm.sql/restore-azurermsqldatabase

<!--Other Web references-->
[Azure Portal]: https://portal.azure.com/
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
