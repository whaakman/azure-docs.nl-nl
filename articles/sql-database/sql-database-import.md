---
title: Een BACPAC-bestand voor het maken van een Azure SQL-database importeren | Microsoft Docs
description: Maak een newAzure SQL-database door een BACPAC-bestand te importeren.
services: sql-database
ms.service: sql-database
ms.subservice: migration
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: douglaslMS
ms.author: douglasl
ms.reviewer: carlrab
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: 5b717e26e61a6633ea51817bcb21ebeb15a20969
ms.sourcegitcommit: e51e940e1a0d4f6c3439ebe6674a7d0e92cdc152
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2019
ms.locfileid: "55892000"
---
# <a name="quickstart-import-a-bacpac-file-to-a-new-azure-sql-database"></a>Quickstart: Een BACPAC-bestand importeren naar een nieuwe Azure SQL Database

U kunt een SQL Server-database migreren naar een Azure SQL database met behulp van een [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) bestand (een zip-bestand met een `.bacpac` extensie met de metagegevens en gegevens van een database). U kunt een BACPAC-bestand importeren uit Azure Blob-opslag (alleen standard storage) of van de lokale opslag in een on-premises locatie. Voor maximale snelheid importeren, kunt u opgeven van een hogere servicelaag en compute (zoals P6). U kunt vervolgens omlaag schalen wanneer het importeren geslaagd is. Compatibiliteitsniveau van de geïmporteerde database is gebaseerd op het compatibiliteitsniveau van de brondatabase.

> [!IMPORTANT]
> Na het importeren van uw database, kunt u de database op het huidige compatibiliteitsniveau (niveau 100 voor de database AdventureWorks2008R2) of op een hoger niveau. Zie [ALTER DATABASE Compatibility Level](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level) (Compatibiliteitsniveau ALTER DATABASE) voor meer informatie over de implicaties en opties bij het hanteren van een database op een bepaald compatibiliteitsniveau. Zie ook [ALTER DATABASE SCOPED CONFIGURATION](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) voor informatie over aanvullende instellingen op databaseniveau in verband met compatibiliteitsniveaus.

## <a name="import-from-a-bacpac-file-in-the-azure-portal"></a>Importeren uit een BACPAC-bestand in Azure portal

Deze sectie wordt beschreven hoe in de [Azure-portal](https://portal.azure.com)om te maken van een Azure SQL database vanuit een BACPAC-bestand opgeslagen in Azure Blob-opslag. De portal *alleen* ondersteunt een BACPAC-bestand importeren uit Azure Blob-opslag.

> [!NOTE]
> [Azure SQL Database Managed Instance](sql-database-managed-instance.md) biedt ondersteuning voor importeren vanuit een BACPAC-bestand met behulp van de andere methoden in dit artikel, maar ondersteunt momenteel geen migreren in Azure portal.

Voor het importeren van een database in Azure portal, open de pagina voor de SQL-Database-server die wordt host voor het importeren en selecteer op de werkbalk **database importeren**.  

   ![database importeren](./media/sql-database-import/import.png)

Selecteer de storage-account, container en Bacpac-bestand dat u wilt importeren. Geef de nieuwe grootte van de database (meestal hetzelfde als oorsprong) en de bestemming van SQL Server-referenties opgeven. 

### <a name="monitor-imports-progress"></a>Voortgang van importeren

Aan de voortgang van importeren, opent u de geïmporteerde database-serverpagina, en klikt u onder **instellingen**, selecteer **geschiedenis van importeren/exporteren**. Als dat lukt, wordt de invoer heeft een **voltooid** status.

Als u wilt controleren of de database op de server is, selecteert u **SQL-databases** en controleer of de nieuwe database **Online**.

## <a name="import-from-a-bacpac-file-using-sqlpackage"></a>Importeren uit een BACPAC-bestand met behulp van SqlPackage

Voor het importeren van een SQL-database met de [SqlPackage](https://docs.microsoft.com/sql/tools/sqlpackage) opdrachtregel-hulpprogramma, Zie [importeren parameters en eigenschappen](https://docs.microsoft.com/sql/tools/sqlpackage#import-parameters-and-properties). SqlPackage is de meest recente [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) en [SQL Server Data Tools voor Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx). U kunt ook de nieuwste versie downloaden [SqlPackage](https://www.microsoft.com/download/details.aspx?id=53876) van het Microsoft download center.

Voor de schaal en prestaties, het beste met behulp van SqlPackage in de meeste productieomgevingen. Raadpleeg dit blogartikel van het SQL Server-klantadviesteam over migratie met behulp van BACPAC-bestanden: [Migrating from SQL Server to Azure SQL Database using BACPAC Files](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/) (Migreren van SQL Server naar Azure SQL Database met BACPAC-bestanden).

De volgende SqlPackage opdracht importeert de **AdventureWorks2008R2** database uit de lokale opslag met een Azure SQL Database-server met de naam **mynewserver20170403**. Deze maakt u een nieuwe database met de naam **myMigratedDatabase** met een **Premium** servicelaag en een **P6** Servicedoelstelling. Deze waarden als geschikt is voor uw omgeving te wijzigen.

```cmd
SqlPackage.exe /a:import /tcs:"Data Source=mynewserver20170403.database.windows.net;Initial Catalog=myMigratedDatabase;User Id=<your_server_admin_account_user_id>;Password=<your_server_admin_account_password>" /sf:AdventureWorks2008R2.bacpac /p:DatabaseEdition=Premium /p:DatabaseServiceObjective=P6
```

> [!IMPORTANT]
> Een SQL Database-server luistert op poort 1433. Om verbinding te maken met een SQL Database-server achter een firewall van het bedrijf, moet de firewall voor deze poort zijn geopend.
>

In dit voorbeeld laat zien hoe een database met behulp van SqlPackage met Universal verificatie van Active Directory importeren.

```cmd
SqlPackage.exe /a:Import /sf:testExport.bacpac /tdn:NewDacFX /tsn:apptestserver.database.windows.net /ua:True /tid:"apptest.onmicrosoft.com"
```

## <a name="import-from-a-bacpac-file-using-powershell"></a>Importeren uit een BACPAC-bestand met behulp van PowerShell

Gebruik de [New-AzureRmSqlDatabaseImport](/powershell/module/azurerm.sql/new-azurermsqldatabaseimport) cmdlet voor het indienen van een aanvraag van de database importeren naar de Azure SQL Database-service. Het importeren kan enige tijd duren, afhankelijk van de grootte van de database.

 ```powershell
 $importRequest = New-AzureRmSqlDatabaseImport 
    -ResourceGroupName "<your_resource_group>" `
    -ServerName "<your_server>" `
    -DatabaseName "<your_database>" `
    -DatabaseMaxSizeBytes "<database_size_in_bytes>" `
    -StorageKeyType "StorageAccessKey" `
    -StorageKey $(Get-AzureRmStorageAccountKey -ResourceGroupName "<your_resource_group>" -StorageAccountName "<your_storage_account").Value[0] `
    -StorageUri "https://myStorageAccount.blob.core.windows.net/importsample/sample.bacpac" `
    -Edition "Standard" `
    -ServiceObjectiveName "P6" `
    -AdministratorLogin "<your_server_admin_account_user_id>" `
    -AdministratorLoginPassword $(ConvertTo-SecureString -String "<your_server_admin_account_password>" -AsPlainText -Force)

 ```

 U kunt de [Get-AzureRmSqlDatabaseImportExportStatus](/powershell/module/azurerm.sql/get-azurermsqldatabaseimportexportstatus) cmdlet om het importeren van de voortgang te controleren. De cmdlet uitvoert voor de retourneert onmiddellijk na de aanvraag meestal **Status: InProgress**. Het importeren is voltooid wanneer er **Status: Succeeded**.

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

Importeren naar een database in een elastische pool wordt niet ondersteund. U kunt gegevens importeren in een individuele database en vervolgens de database te verplaatsen naar een elastische pool.

## <a name="import-using-wizards"></a>Importeren met behulp van de wizards

Ook kunt u deze wizards.

- [Importeren van de Wizard voor Data-tier-toepassing in SQL Server Management Studio](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/import-a-bacpac-file-to-create-a-new-user-database#using-the-import-data-tier-application-wizard).
- [Wizard SQL Server importeren en exporteren](https://docs.microsoft.com/sql/integration-services/import-export-data/start-the-sql-server-import-and-export-wizard).

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over verbinding maken met en query uitvoeren op een geïmporteerde SQL-Database, [Quick Start: Azure SQL Database: SQL Server Management Studio gebruiken om te verbinden en gegevens op te vragen](sql-database-connect-query-ssms.md).
- Raadpleeg dit blogartikel van het SQL Server-klantadviesteam over migratie met behulp van BACPAC-bestanden: [Migrating from SQL Server to Azure SQL Database using BACPAC Files](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/) (Migreren van SQL Server naar Azure SQL Database met BACPAC-bestanden).
- Zie voor een discussie over de hele SQL Server-database migreren proces, inclusief aanbevelingen voor prestaties, [SQL Server-databasemigratie naar Azure SQL Database](sql-database-cloud-migrate.md).
- Voor informatie over het beheren en te delen opslagsleutels en gedeelde toegang handtekeningen veilig, Zie [Azure Storage-beveiligingshandleiding](https://docs.microsoft.com/azure/storage/common/storage-security-guide).
