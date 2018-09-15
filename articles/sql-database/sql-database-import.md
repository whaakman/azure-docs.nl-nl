---
title: Een BACPAC-bestand voor het maken van een Azure SQL-database importeren | Microsoft Docs
description: Maak een newAzure SQL-database door een BACPAC-bestand te importeren.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: load & move data
ms.date: 09/04/2018
ms.author: carlrab
ms.topic: conceptual
ms.openlocfilehash: 6df71f50129ec6901d0b8688b0a6d3619260cf22
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/15/2018
ms.locfileid: "45634221"
---
# <a name="import-a-bacpac-file-to-a-new-azure-sql-database"></a>Een BACPAC-bestand importeren naar een nieuwe Azure SQL Database

Wanneer moet u een database importeren vanuit een archief of wanneer u migreert vanaf een ander platform, kunt u het databaseschema en de gegevens van importeren een [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) bestand. Een BACPAC-bestand is een ZIP-bestand met de extensie van de metagegevens en gegevens uit een SQL Server-database met BACPAC. Een BACPAC-bestand kan worden geïmporteerd uit Azure blob-opslag (alleen standard storage) of van de lokale opslag in een on-premises locatie. Voor maximale snelheid van de invoer, wordt aangeraden dat u een hogere servicelaag en het prestatieniveau serviceniveau, zoals een P6 opgeven en vervolgens te schalen om zo nodig omlaag na het importeren voltooid is. Het databasecompatibiliteitsniveau na het importeren is ook gebaseerd op het compatibiliteitsniveau van de brondatabase. 

> [!IMPORTANT] 
> Nadat u uw database naar Azure SQL Database migreren, kunt u de database op het huidige compatibiliteitsniveau (niveau 100 voor de database AdventureWorks2008R2) of op een hoger niveau. Zie [ALTER DATABASE Compatibility Level](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level) (Compatibiliteitsniveau ALTER DATABASE) voor meer informatie over de implicaties en opties bij het hanteren van een database op een bepaald compatibiliteitsniveau. Zie ook [ALTER DATABASE SCOPED CONFIGURATION](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) voor informatie over aanvullende instellingen op databaseniveau in verband met compatibiliteitsniveaus.   >

## <a name="import-from-a-bacpac-file-using-azure-portal"></a>Importeren uit een BACPAC-bestand met behulp van Azure portal

Dit artikel bevat instructies voor het maken van een Azure SQL database vanuit een BACPAC-bestand opgeslagen in Azure blob storage met behulp van de [Azure-portal](https://portal.azure.com). Importeren met behulp van Azure portal biedt ondersteuning voor een BACPAC-bestand importeren uit Azure blob-opslag.

Voor het importeren van een database met behulp van de Azure-portal, opent u de pagina voor de server (niet op de pagina voor de database) voor de database te koppelen en klik op **importeren** op de werkbalk. Geef het opslagaccount en container en selecteert u het BACPAC-bestand dat u wilt importeren. Selecteer de grootte van de nieuwe database (meestal hetzelfde als oorsprong) en de bestemming van SQL Server-referenties opgeven.  

   ![database importeren](./media/sql-database-import/import.png)

Open de pagina voor de logische server met de database wordt geïmporteerd voor het controleren van de voortgang van de importbewerking. Schuif omlaag naar **Operations** en klik vervolgens op **Import/Export** geschiedenis.

> [!NOTE]
> [Azure SQL Database Managed Instance](sql-database-managed-instance.md) importeren vanuit een BACPAC-bestand met behulp van de andere methoden in dit artikel wordt ondersteund, maar ondersteunt momenteel geen migreren met behulp van de Azure portal.

### <a name="monitor-the-progress-of-an-import-operation"></a>De voortgang van een importbewerking

Open de pagina voor de logische server waarin de database wordt geïmporteerd geïmporteerd voor het controleren van de voortgang van de importbewerking. Schuif omlaag naar **Operations** en klik vervolgens op **Import/Export** geschiedenis.
   
   ![importeren](./media/sql-database-import/import-history.png) ![Importstatus](./media/sql-database-import/import-status.png)

Als u wilt controleren of de database op de server is, klikt u op **SQL-databases** en controleer of de nieuwe database **Online**.

## <a name="import-from-a-bacpac-file-using-sqlpackage"></a>Importeren uit een BACPAC-bestand met behulp van SQLPackage

Voor het importeren van een SQL-database met de [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) opdrachtregel-hulpprogramma, Zie [importeren parameters en eigenschappen](https://msdn.microsoft.com/library/hh550080.aspx#Import Parameters and Properties). Het hulpprogramma SQLPackage wordt geleverd met de nieuwste versies van [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) en [SQL Server Data Tools voor Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx), maar u kunt de nieuwste versie van downloaden [SqlPackage ](https://www.microsoft.com/download/details.aspx?id=53876) rechtstreeks vanuit het Microsoft download center.

We raden het gebruik van het hulpprogramma SQLPackage voor schaalbaarheid en prestaties in de meeste productieomgevingen. Raadpleeg dit blogartikel van het SQL Server-klantadviesteam over migratie met behulp van BACPAC-bestanden: [Migrating from SQL Server to Azure SQL Database using BACPAC Files](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/) (Migreren van SQL Server naar Azure SQL Database met BACPAC-bestanden).

Zie de volgende opdracht van SQLPackage voor een voorbeeld van een script voor het importeren van de **AdventureWorks2008R2** database uit de lokale opslag naar een logische server van Azure SQL Database, met de naam **mynewserver20170403** in dit voorbeeld. Met dit script toont het maken van een nieuwe database met de naam **myMigratedDatabase**, met een servicelaag **Premium**, en een Servicedoelstelling van **P6**. Deze waarden als geschikt is voor uw omgeving te wijzigen.

```cmd
SqlPackage.exe /a:import /tcs:"Data Source=mynewserver20170403.database.windows.net;Initial Catalog=myMigratedDatabase;User Id=ServerAdmin;Password=<change_to_your_password>" /sf:AdventureWorks2008R2.bacpac /p:DatabaseEdition=Premium /p:DatabaseServiceObjective=P6
```

> [!IMPORTANT]
> Een logische Azure SQL Database-server luistert naar poort 1433. Als u vanachter een bedrijfsfirewall probeert verbinding te maken met een logische Azure SQL Database-server, moet deze poort in de bedrijfsfirewall zijn geopend om verbinding te kunnen maken.
>

In dit voorbeeld laat zien hoe een database met SqlPackage.exe met Universal verificatie van Active Directory importeren:

```cmd
SqlPackage.exe /a:Import /sf:testExport.bacpac /tdn:NewDacFX /tsn:apptestserver.database.windows.net /ua:True /tid:"apptest.onmicrosoft.com"
```

## <a name="import-from-a-bacpac-file-using-powershell"></a>Importeren uit een BACPAC-bestand met behulp van PowerShell

Gebruik de [New-AzureRmSqlDatabaseImport](/powershell/module/azurerm.sql/new-azurermsqldatabaseimport) cmdlet voor het indienen van een aanvraag van de database importeren naar de Azure SQL Database-service. De importbewerking kan enige tijd duren, afhankelijk van de grootte van uw database.

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

Om te controleren of de status van de import-aanvraag, gebruikt u de [Get-AzureRmSqlDatabaseImportExportStatus](/powershell/module/azurerm.sql/get-azurermsqldatabaseimportexportstatus) cmdlet. Met dit onmiddellijk na de aanvraag meestal retourneert **Status: InProgress**. Wanneer de melding **Status: geslaagd** het importeren is voltooid.

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
Zie voor een ander scriptvoorbeeld [een database uit een BACPAC-bestand importeren](scripts/sql-database-import-from-bacpac-powershell.md).

## <a name="limitations"></a>Beperkingen
- Importeren met een database in een elastische pool wordt niet ondersteund. U kunt gegevens importeren in een singleton-database en vervolgens de database te verplaatsen naar een pool.

## <a name="import-using-other-methods"></a>Importeren met behulp van andere methoden

U kunt ook deze wizards gebruiken:

- [Importeren van de Wizard voor Data-tier-toepassing in SQL Server Management Studio](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/import-a-bacpac-file-to-create-a-new-user-database#using-the-import-data-tier-application-wizard).
- [Wizard SQL Server importeren en exporteren](https://docs.microsoft.com/sql/integration-services/import-export-data/start-the-sql-server-import-and-export-wizard).

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over verbinding maken met en query uitvoeren op een geïmporteerde SQL-Database, [verbinding maken met SQL Database met SQL Server Management Studio en een voorbeeld T-SQL-query uitvoeren](sql-database-connect-query-ssms.md).
* Raadpleeg dit blogartikel van het SQL Server-klantadviesteam over migratie met behulp van BACPAC-bestanden: [Migrating from SQL Server to Azure SQL Database using BACPAC Files](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/) (Migreren van SQL Server naar Azure SQL Database met BACPAC-bestanden).
* Zie voor een discussie over de hele SQL Server-database migreren proces, inclusief aanbevelingen voor prestaties, [een SQL Server-database migreren naar Azure SQL Database](sql-database-cloud-migrate.md).
* Voor informatie over het beheren en te delen opslagsleutels en gedeelde toegang signitures veilig, Zie [Azure Storage-beveiligingshandleiding](https://docs.microsoft.com/azure/storage/common/storage-security-guide). 


  

