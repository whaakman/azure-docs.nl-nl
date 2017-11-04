---
title: Een Bacpac-bestand importeren om te maken van een Azure SQL database | Microsoft Docs
description: Maak een newAzure SQL-database door een Bacpac-bestand te importeren.
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: cf9a9631-56aa-4985-a565-1cacc297871d
ms.service: sql-database
ms.custom: load & move data
ms.devlang: NA
ms.date: 06/26/2017
ms.author: carlrab
ms.workload: Active
ms.topic: article
ms.tgt_pltfrm: NA
ms.openlocfilehash: 34dee9511822acec46ba4854729939b84f3c06c6
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="import-a-bacpac-file-to-a-new-azure-sql-database"></a>Een Bacpac-bestand importeren in een nieuwe Azure SQL Database

Wanneer moet u een database te importeren vanuit een archief of wanneer u migreert vanaf een ander platform, kunt u het databaseschema en de gegevens van importeren een [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) bestand. Een Bacpac-bestand is een ZIP-bestand met de extensie BACPAC die de metagegevens en gegevens uit een SQL Server-database. Een Bacpac-bestand kan worden geïmporteerd uit Azure blob storage (standard-opslag alleen) of van lokale opslag in een on-premises locatie. Als u wilt de import-snelheid maximaliseren, wordt aangeraden dat u een hogere prijscategorie en prestatieniveau serviceniveau, zoals een P6 opgeven en vervolgens te schalen om omlaag naar gelang van toepassing na het importeren geslaagd is. Ook het databasecompatibiliteitsniveau na het importeren is gebaseerd op het compatibiliteitsniveau van de brondatabase. 

> [!IMPORTANT] 
> Nadat u de database naar Azure SQL Database migreert, kunt u de werking van de database op het huidige compatibiliteitsniveau (level 100 voor de database AdventureWorks2008R2) of op een hoger niveau. Zie voor meer informatie over de gevolgen en opties voor de werking van een database op een specifieke compatibiliteitsniveau [databasecompatibiliteitsniveau ALTER](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level). Zie ook [ALTER DATABASE SCOPED CONFIGURATION](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) voor meer informatie over aanvullende databaseniveau instellingen met betrekking tot compatibiliteitsniveaus.   >

> [!NOTE]
> Als u wilt een BACPAC importeren naar een nieuwe database, moet u eerst een logische Azure SQL Database-server maken. Zie voor een zelfstudie waarin u een SQL Server-database migreren naar Azure SQL Database met SQLPackage [migreren van een SQL Server-Database](sql-database-migrate-your-sql-server-database.md)
>

## <a name="import-from-a-bacpac-file-using-azure-portal"></a>Importeren uit een Bacpac-bestand met Azure portal

Dit artikel bevat instructies voor het maken van een Azure SQL database van een BACPAC-bestand dat is opgeslagen in Azure blob storage met behulp van de [Azure-portal](https://portal.azure.com). Importeren met Azure portal ondersteunt een BACPAC-bestand te importeren uit Azure blob-opslag.

Open de pagina voor de server naar de database te koppelen en klik vervolgens op voor het importeren van een database met behulp van de Azure-portal **importeren** op de werkbalk. Geef het opslagaccount en container en selecteer het Bacpac-bestand dat u wilt importeren. Selecteer de grootte van de nieuwe database (meestal hetzelfde als de oorsprong) en SQL Server-referenties voor de bestemming opgeven.  

   ![De database wordt geïmporteerd](./media/sql-database-import/import.png)

Open de pagina voor de logische server met de database wordt geïmporteerd om de voortgang van de importbewerking. Schuif omlaag naar **Operations** en klik vervolgens op **voor importeren/exporteren** geschiedenis.

### <a name="monitor-the-progress-of-an-import-operation"></a>De voortgang van een importbewerking

Open de pagina voor de logische server waarin de database wordt geïmporteerd geïmporteerd om de voortgang van de importbewerking. Schuif omlaag naar **Operations** en klik vervolgens op **voor importeren/exporteren** geschiedenis.
   
   ![importeren](./media/sql-database-import/import-history.png) ![status importeren](./media/sql-database-import/import-status.png)

Om te controleren of de database op de server is, klikt u op **SQL-databases** en controleer of de nieuwe database **Online**.

## <a name="import-from-a-bacpac-file-using-sqlpackage"></a>Importeren uit een Bacpac-bestand met SQLPackage

Voor het importeren van een SQL-database met de [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) opdrachtregelhulpprogramma Zie [importeren parameters en eigenschappen](https://msdn.microsoft.com/library/hh550080.aspx#Import Parameters and Properties). Het hulpprogramma SQLPackage wordt geleverd met de nieuwste versies van [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) en [SQL Server Data Tools voor Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx), of u kunt de nieuwste versie van downloaden [SqlPackage ](https://www.microsoft.com/download/details.aspx?id=53876) rechtstreeks vanuit het Microsoft download center.

U wordt aangeraden het gebruik van het hulpprogramma SQLPackage voor schaal en prestaties in de meeste productieomgevingen. Raadpleeg dit blogartikel van het SQL Server-klantadviesteam over migratie met behulp van BACPAC-bestanden: [Migrating from SQL Server to Azure SQL Database using BACPAC Files](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/) (Migreren van SQL Server naar Azure SQL Database met BACPAC-bestanden).

Zie de volgende opdracht van SQLPackage voor een scriptvoorbeeld van een voor het importeren van de **AdventureWorks2008R2** -database van lokale opslag met een logische server van Azure SQL Database, aangeroepen **mynewserver20170403** in dit voorbeeld. Dit script toont het maken van een nieuwe database aangeroepen **myMigratedDatabase**, met een servicelaag van **Premium**, en een Servicedoelstelling van **P6**. Deze waarden naargelang uw omgeving te wijzigen.

```cmd
SqlPackage.exe /a:import /tcs:"Data Source=mynewserver20170403.database.windows.net;Initial Catalog=myMigratedDatabase;User Id=ServerAdmin;Password=<change_to_your_password>" /sf:AdventureWorks2008R2.bacpac /p:DatabaseEdition=Premium /p:DatabaseServiceObjective=P6
```

> [!IMPORTANT]
> Een logische Azure SQL Database-server luistert naar poort 1433. Als u vanachter een bedrijfsfirewall probeert verbinding te maken met een logische Azure SQL Database-server, moet deze poort in de bedrijfsfirewall zijn geopend om verbinding te kunnen maken.
>

In dit voorbeeld ziet u hoe een database met behulp van SqlPackage.exe met Universal verificatie van Active Directory importeren:

```cmd
SqlPackage.exe /a:Import /sf:testExport.bacpac /tdn:NewDacFX /tsn:apptestserver.database.windows.net /ua:True /tid:"apptest.onmicrosoft.com"
```

## <a name="import-from-a-bacpac-file-using-powershell"></a>Importeren uit een Bacpac-bestand met behulp van PowerShell

Gebruik de [nieuw AzureRmSqlDatabaseImport](/powershell/module/azurerm.sql/new-azurermsqldatabaseimport) cmdlet een aanvraag van de database importeren voor de Azure SQL Database-service. De importbewerking kan enige tijd duren, afhankelijk van de grootte van uw database.

 ```powershell
 $importRequest = New-AzureRmSqlDatabaseImport -ResourceGroupName "myResourceGroup" `
    -ServerName $servername `
    -DatabaseName "MyImportSample" `
    -DatabaseMaxSizeBytes "262144000" `
    -StorageKeyType "StorageAccessKey" `
    -StorageKey $(Get-AzureRmStorageAccountKey -ResourceGroupName "myResourceGroup" -StorageAccountName $storageaccountname).Value[0] `
    -StorageUri "http://$storageaccountname.blob.core.windows.net/importsample/sample.bacpac" `
    -Edition "Standard" `
    -ServiceObjectiveName "P6" `
    -AdministratorLogin "ServerAdmin" `
    -AdministratorLoginPassword $(ConvertTo-SecureString -String "ASecureP@assw0rd" -AsPlainText -Force)

 ```

U controleert de status van de aanvraag importeren gebruiken de [Get-AzureRmSqlDatabaseImportExportStatus](/powershell/module/azurerm.sql/get-azurermsqldatabaseimportexportstatus) cmdlet. Met dit onmiddellijk na de aanvraag meestal retourneert **Status: InProgress**. Wanneer er **Status: geslaagd** de importbewerking is voltooid.

```powershell
$importStatus = Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink
[Console]::Write("Importing")
while ($importStatus.Status -eq "InProgress")
{
    $importStatus = Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink
    [Console]::Write(".")
    Start-Sleep -s 10
}
[Console]::WriteLine("")
$importStatus
```

> [!TIP]
Zie voor een ander scriptvoorbeeld [een database uit een Bacpac-bestand importeren](scripts/sql-database-import-from-bacpac-powershell.md).

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over verbinding maken met en query uitvoeren op een geïmporteerde SQL-Database, [verbinding maken met SQL Database met SQL Server Management Studio en een voorbeeld T-SQL-query uit te voeren](sql-database-connect-query-ssms.md).
* Raadpleeg dit blogartikel van het SQL Server-klantadviesteam over migratie met behulp van BACPAC-bestanden: [Migrating from SQL Server to Azure SQL Database using BACPAC Files](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/) (Migreren van SQL Server naar Azure SQL Database met BACPAC-bestanden).
* Zie voor een beschrijving van het volledige SQL Server-databasemigratieproces, inclusief aanbevelingen voor prestaties [een SQL Server-database migreren naar Azure SQL Database](sql-database-cloud-migrate.md).



