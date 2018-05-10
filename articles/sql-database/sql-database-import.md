---
title: Een Bacpac-bestand importeren om te maken van een Azure SQL database | Microsoft Docs
description: Maak een newAzure SQL-database door een Bacpac-bestand te importeren.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: load & move data
ms.date: 04/10/2018
ms.author: carlrab
ms.topic: article
ms.openlocfilehash: bd9554a18775cf98f4415ebd5d4b0d52edc53718
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2018
---
# <a name="import-a-bacpac-file-to-a-new-azure-sql-database"></a>Een Bacpac-bestand importeren in een nieuwe Azure SQL Database

Wanneer moet u een database te importeren vanuit een archief of wanneer u migreert vanaf een ander platform, kunt u het databaseschema en de gegevens van importeren een [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) bestand. Een Bacpac-bestand is een ZIP-bestand met de extensie BACPAC die de metagegevens en gegevens uit een SQL Server-database. Een Bacpac-bestand kan worden geïmporteerd uit Azure blob storage (standard-opslag alleen) of van lokale opslag in een on-premises locatie. Als u wilt de import-snelheid maximaliseren, wordt aangeraden dat u een hogere prijscategorie en prestatieniveau serviceniveau, zoals een P6 opgeven en vervolgens te schalen om omlaag naar gelang van toepassing na het importeren geslaagd is. Ook het databasecompatibiliteitsniveau na het importeren is gebaseerd op het compatibiliteitsniveau van de brondatabase. 

> [!IMPORTANT] 
> Nadat u de database naar Azure SQL Database migreert, kunt u de werking van de database op het huidige compatibiliteitsniveau (level 100 voor de database AdventureWorks2008R2) of op een hoger niveau. Zie [ALTER DATABASE Compatibility Level](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level) (Compatibiliteitsniveau ALTER DATABASE) voor meer informatie over de implicaties en opties bij het hanteren van een database op een bepaald compatibiliteitsniveau. Zie ook [ALTER DATABASE SCOPED CONFIGURATION](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) voor informatie over aanvullende instellingen op databaseniveau in verband met compatibiliteitsniveaus.   >

## <a name="import-from-a-bacpac-file-using-azure-portal"></a>Importeren uit een Bacpac-bestand met Azure portal

Dit artikel bevat instructies voor het maken van een Azure SQL database van een BACPAC-bestand dat is opgeslagen in Azure blob storage met behulp van de [Azure-portal](https://portal.azure.com). Importeren met Azure portal ondersteunt een BACPAC-bestand te importeren uit Azure blob-opslag.

Open de pagina voor de server naar de database te koppelen en klik vervolgens op voor het importeren van een database met behulp van de Azure-portal **importeren** op de werkbalk. Geef het opslagaccount en container en selecteer het Bacpac-bestand dat u wilt importeren. Selecteer de grootte van de nieuwe database (meestal hetzelfde als de oorsprong) en SQL Server-referenties voor de bestemming opgeven.  

   ![De database wordt geïmporteerd](./media/sql-database-import/import.png)

Open de pagina voor de logische server met de database wordt geïmporteerd om de voortgang van de importbewerking. Schuif omlaag naar **Operations** en klik vervolgens op **voor importeren/exporteren** geschiedenis.

> [!NOTE]
> [Azure SQL Database beheerd-instantie](sql-database-managed-instance.md) importeren uit een Bacpac-bestand met de andere methoden in dit artikel wordt ondersteund, maar biedt geen ondersteuning voor migratie met behulp van de Azure-portal.

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

## <a name="import-using-other-methods"></a>Importeren van andere methoden

U kunt ook deze wizards gebruiken:

- [Data-tier Application-Wizard in SQL Server Management Studio importeren](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/import-a-bacpac-file-to-create-a-new-user-database#using-the-import-data-tier-application-wizard).
- [Wizard SQL Server importeren en exporteren](https://docs.microsoft.com/sql/integration-services/import-export-data/start-the-sql-server-import-and-export-wizard).

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over verbinding maken met en query uitvoeren op een geïmporteerde SQL-Database, [verbinding maken met SQL Database met SQL Server Management Studio en een voorbeeld T-SQL-query uit te voeren](sql-database-connect-query-ssms.md).
* Raadpleeg dit blogartikel van het SQL Server-klantadviesteam over migratie met behulp van BACPAC-bestanden: [Migrating from SQL Server to Azure SQL Database using BACPAC Files](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/) (Migreren van SQL Server naar Azure SQL Database met BACPAC-bestanden).
* Zie voor een beschrijving van het volledige SQL Server-databasemigratieproces, inclusief aanbevelingen voor prestaties [een SQL Server-database migreren naar Azure SQL Database](sql-database-cloud-migrate.md).
* Voor informatie over het beheren en delen opslagsleutels en gedeelde toegang signitures veilig, Zie [Azure Storage-beveiligingshandleiding](https://docs.microsoft.com/azure/storage/common/storage-security-guide). 


  

