---
title: Een BACPAC-bestand voor het maken van een Azure SQL-database importeren | Microsoft Docs
description: Maak een newAzure SQL-database door een BACPAC-bestand te importeren.
services: sql-database
ms.service: sql-database
ms.subservice: migration
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 02/18/2019
ms.openlocfilehash: 06c74aa85bda13ccd6849056ccc031ae6f1c12c2
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/04/2019
ms.locfileid: "57315551"
---
# <a name="quickstart-import-a-bacpac-file-to-a-database-in-azure-sql-database"></a>Quickstart: Een BACPAC-bestand importeren in een database in Azure SQL Database

U kunt een SQL Server-database importeren in een database in Azure SQL Database met behulp van een [BACPAC](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications#bacpac) bestand. U kunt de gegevens importeren uit een `BACPAC` bestand wordt opgeslagen in Azure Blob-opslag (alleen standard storage) of van de lokale opslag in een on-premises locatie. Voor maximale snelheid importeren door te geven van meer en snellere resources, schaal uw database naar een hogere servicelaag en grootte-rekenknooppunt tijdens het importproces. U kunt vervolgens omlaag schalen wanneer het importeren geslaagd is.

> [!NOTE]
> Compatibiliteitsniveau van de geïmporteerde database is gebaseerd op het compatibiliteitsniveau van de brondatabase.
> [!IMPORTANT]
> Na het importeren van uw database, kunt u de database op het huidige compatibiliteitsniveau (niveau 100 voor de database AdventureWorks2008R2) of op een hoger niveau. Zie [ALTER DATABASE Compatibility Level](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level) (Compatibiliteitsniveau ALTER DATABASE) voor meer informatie over de implicaties en opties bij het hanteren van een database op een bepaald compatibiliteitsniveau. Zie ook [ALTER DATABASE SCOPED CONFIGURATION](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) voor informatie over aanvullende instellingen op databaseniveau in verband met compatibiliteitsniveaus.

## <a name="import-from-a-bacpac-file-in-the-azure-portal"></a>Importeren uit een BACPAC-bestand in Azure portal

De [Azure-portal](https://portal.azure.com) *alleen* biedt ondersteuning voor het maken van een individuele database in Azure SQL Database en *alleen* vanuit een BACPAC-bestand opgeslagen in Azure Blob-opslag.

> [!NOTE]
> [Een beheerd exemplaar](sql-database-managed-instance.md) biedt momenteel geen ondersteuning voor het migreren van een database in een exemplaar in de database vanuit een BACPAC-bestand met de Azure-portal. Als u wilt importeren in een beheerd exemplaar, SQL Server Management Studio of SQLPackage te gebruiken.

1. Als u wilt importeren vanuit een BACPAC-bestand in een nieuwe database met behulp van de Azure portal, de juiste database-serverpagina openen en selecteer vervolgens op de werkbalk **database importeren**.  

   ![Database import1](./media/sql-database-import/import1.png)

2. Selecteer het opslagaccount en de container voor het BACPAC-bestand en selecteer vervolgens het BACPAC-bestand waaruit moet worden geïmporteerd.
3. Geef de nieuwe grootte van de database (meestal hetzelfde als oorsprong) en de bestemming van SQL Server-referenties opgeven. Zie voor een lijst van mogelijke waarden voor een nieuwe Azure SQL-database, [Create Database](https://docs.microsoft.com/sql/t-sql/statements/create-database-transact-sql?view=azuresqldb-current).

   ![Database import2](./media/sql-database-import/import2.png)

4. Klik op **OK**.

5. Voortgang van een import, open de pagina van de database-server en klikt u onder **instellingen**, selecteer **geschiedenis van importeren/exporteren**. Als dat lukt, wordt de invoer heeft een **voltooid** status.

   ![Status van de database importeren](./media/sql-database-import/import-status.png)

6. Selecteer om te controleren of de database online is op de databaseserver, **SQL-databases** en controleer of de nieuwe database **Online**.

## <a name="import-from-a-bacpac-file-using-sqlpackage"></a>Importeren uit een BACPAC-bestand met behulp van SqlPackage

Voor het importeren van een SQL Server-database met de [SqlPackage](https://docs.microsoft.com/sql/tools/sqlpackage) opdrachtregel-hulpprogramma, Zie [importeren parameters en eigenschappen](https://docs.microsoft.com/sql/tools/sqlpackage#import-parameters-and-properties). SqlPackage is de meest recente [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) en [SQL Server Data Tools voor Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx). U kunt ook de nieuwste versie downloaden [SqlPackage](https://www.microsoft.com/download/details.aspx?id=53876) van het Microsoft download center.

Voor de schaal en prestaties raden wij met behulp van SqlPackage in de meeste productieomgevingen in plaats van met behulp van de Azure portal. Voor een SQL Server Customer Advisory Team blog over het migreren van met `BACPAC` bestanden, Zie [migreren van SQL Server naar Azure SQL Database met BACPAC-bestanden](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).

Voor de schaal en prestaties, het beste met behulp van SqlPackage in de meeste productieomgevingen. Raadpleeg dit blogartikel van het SQL Server-klantadviesteam over migratie met behulp van BACPAC-bestanden: [Migrating from SQL Server to Azure SQL Database using BACPAC Files](https://blogs.msdn.microsoft.com/sqlcat/20../../migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/) (Migreren van SQL Server naar Azure SQL Database met BACPAC-bestanden).

De volgende SqlPackage opdracht importeert de **AdventureWorks2008R2** database uit de lokale opslag met een Azure SQL Database-server met de naam **mynewserver20170403**. Deze maakt u een nieuwe database met de naam **myMigratedDatabase** met een **Premium** servicelaag en een **P6** Servicedoelstelling. Deze waarden als geschikt is voor uw omgeving te wijzigen.

```cmd
SqlPackage.exe /a:import /tcs:"Data Source=mynewserver20170403.database.windows.net;Initial Catalog=myMigratedDatabase;User Id=<your_server_admin_account_user_id>;Password=<your_server_admin_account_password>" /sf:AdventureWorks2008R2.bacpac /p:DatabaseEdition=Premium /p:DatabaseServiceObjective=P6
```

> [!IMPORTANT]
> Voor verbinding met een SQL Database-server een individuele database achter een firewall van het bedrijf te beheren, moet de firewall poort 1433 openen. Voor verbinding met een beheerd exemplaar, hebt u een [verbindingstype punt-naar-site](sql-database-managed-instance-configure-p2s.md) of een express route-verbinding.
>

In dit voorbeeld laat zien hoe een database met behulp van SqlPackage met Universal verificatie van Active Directory importeren.

```cmd
SqlPackage.exe /a:Import /sf:testExport.bacpac /tdn:NewDacFX /tsn:apptestserver.database.windows.net /ua:True /tid:"apptest.onmicrosoft.com"
```

## <a name="import-into-a-single-database-from-a-bacpac-file-using-powershell"></a>Importeren in een individuele database vanuit een BACPAC-bestand met behulp van PowerShell

> [!NOTE]
> [Een beheerd exemplaar](sql-database-managed-instance.md) biedt momenteel geen ondersteuning voor het migreren van een database in een exemplaar in de database vanuit een BACPAC-bestand met behulp van Azure PowerShell]. Als u wilt importeren in een beheerd exemplaar, SQL Server Management Studio of SQLPackage te gebruiken.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Gebruik de [New-AzSqlDatabaseImport](/powershell/module/az.sql/new-azsqldatabaseimport) cmdlet voor het indienen van een aanvraag van de database importeren naar de Azure SQL Database-service. Het importeren kan enige tijd duren, afhankelijk van de grootte van de database.

 ```powershell
 $importRequest = New-AzSqlDatabaseImport 
    -ResourceGroupName "<your_resource_group>" `
    -ServerName "<your_server>" `
    -DatabaseName "<your_database>" `
    -DatabaseMaxSizeBytes "<database_size_in_bytes>" `
    -StorageKeyType "StorageAccessKey" `
    -StorageKey $(Get-AzStorageAccountKey -ResourceGroupName "<your_resource_group>" -StorageAccountName "<your_storage_account").Value[0] `
    -StorageUri "https://myStorageAccount.blob.core.windows.net/importsample/sample.bacpac" `
    -Edition "Standard" `
    -ServiceObjectiveName "P6" `
    -AdministratorLogin "<your_server_admin_account_user_id>" `
    -AdministratorLoginPassword $(ConvertTo-SecureString -String "<your_server_admin_account_password>" -AsPlainText -Force)

 ```

 U kunt de [Get-AzSqlDatabaseImportExportStatus](/powershell/module/az.sql/get-azsqldatabaseimportexportstatus) cmdlet om het importeren van de voortgang te controleren. De cmdlet uitvoert voor de retourneert onmiddellijk na de aanvraag meestal **Status: InProgress**. Het importeren is voltooid wanneer er **Status: Succeeded**.

```powershell
$importStatus = Get-AzSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink
[Console]::Write("Importing")
while ($importStatus.Status -eq "InProgress")
{
    $importStatus = Get-AzSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink
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
- Raadpleeg dit blogartikel van het SQL Server-klantadviesteam over migratie met behulp van BACPAC-bestanden: [Migrating from SQL Server to Azure SQL Database using BACPAC Files](https://techcommunity.microsoft.com/t5/DataCAT/Migrating-from-SQL-Server-to-Azure-SQL-Database-using-Bacpac/ba-p/305407) (Migreren van SQL Server naar Azure SQL Database met BACPAC-bestanden).
- Zie voor een discussie over de hele SQL Server-database migreren proces, inclusief aanbevelingen voor prestaties, [SQL Server-databasemigratie naar Azure SQL Database](sql-database-single-database-migrate.md).
- Voor informatie over het beheren en te delen opslagsleutels en gedeelde toegang handtekeningen veilig, Zie [Azure Storage-beveiligingshandleiding](https://docs.microsoft.com/azure/storage/common/storage-security-guide).
