---
title: Een Azure SQL database naar een Bacpac-bestand exporteren | Microsoft Docs
description: Een Azure SQL database exporteren naar een Bacpac-bestand met de Azure Portal
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 41d63a97-37db-4e40-b652-77c2fd1c09b7
ms.service: sql-database
ms.custom: load & move data
ms.devlang: NA
ms.date: 06/15/2017
ms.author: carlrab
ms.workload: data-management
ms.topic: article
ms.tgt_pltfrm: NA
ms.openlocfilehash: faa567ec615a07da8633629fc98e3454c84a8f5f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="export-an-azure-sql-database-to-a-bacpac-file"></a>Een Azure SQL database naar een Bacpac-bestand exporteren

Als u exporteren van een database wilt voor het archiveren of voor het verplaatsen naar een ander platform, kunt u het databaseschema en de gegevens naar exporteren een [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) bestand. Een Bacpac-bestand is een ZIP-bestand met de extensie BACPAC die de metagegevens en gegevens uit een SQL Server-database. Een Bacpac-bestand worden opgeslagen in Azure blob-opslag of in de lokale opslag in een on-premises locatie en later geïmporteerd terug in Azure SQL Database of in een lokale installatie van SQL Server. 

> [!IMPORTANT] 
> Azure SQL Database geautomatiseerde exporteren buiten gebruik werd gesteld op 1 maart 2017. U kunt [lange bewaartermijn van de back-](sql-database-long-term-retention.md
) of [Azure Automation](https://github.com/Microsoft/azure-docs-pr/blob/2461f706f8fc1150e69312098640c0676206a531/articles/automation/automation-intro.md) periodiek archiveren SQL-databases met behulp van PowerShell volgens een planning van uw keuze. Voor een steekproef, downloadt u de [PowerShell-voorbeeldscript](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-automation-automated-export) vanuit Github.
>

## <a name="considerations-when-exporting-an-azure-sql-database"></a>Overwegingen bij het exporteren van een Azure SQL database

* Exporteren van een transactioneel consistent, moet u ervoor zorgen die geen schrijven activiteit optreedt tijdens het exporteren, of dat u gaat exporteren vanuit een [transactioneel consistent kopiëren](sql-database-copy.md) van uw Azure SQL database.
* Als u naar blob storage exporteert, is de maximale grootte van een Bacpac-bestand 200 GB. Als u wilt archiveren een groter Bacpac-bestand, exporteren naar de lokale opslag.
* Een Bacpac-bestand exporteren naar Azure premium-opslag met behulp van de methoden die worden beschreven in dit artikel wordt niet ondersteund.
* Als de exportbewerking uit Azure SQL Database groter is dan 20 uur, kan worden geannuleerd. U kunt voor het verhogen van de prestaties tijdens het exporteren:
  * Tijdelijk verhogen uw serviceniveau.
  * Niet langer alle lees- en schrijfbewerkingen tijdens het exporteren.
  * Gebruik een [geclusterde index](https://msdn.microsoft.com/library/ms190457.aspx) met niet-null-waarden voor alle grote tabellen. Exporteren van een kan zonder geclusterde indexen mislukken als het duurt langer dan 6 tot 12 uur. Dit komt doordat de export-service een tabelscan moet om te exporteren van de hele tabel te voltooien. Een goede manier om te bepalen als uw tabellen zijn geoptimaliseerd voor het exporteren is het uitvoeren van **DBCC SHOW_STATISTICS** en zorg ervoor dat de *RANGE_HI_KEY* niet null is en goed verdeling van de waarde heeft. Zie voor meer informatie [DBCC SHOW_STATISTICS](https://msdn.microsoft.com/library/ms174384.aspx).

> [!NOTE]
> BACPACs zijn niet bedoeld om te worden gebruikt voor back-up en herstelbewerkingen. Azure SQL Database maakt automatisch een back-ups voor de database van elke gebruiker. Zie voor meer informatie [Business Continuity Overview](sql-database-business-continuity.md) en [back-ups van SQL-Database](sql-database-automated-backups.md).  
> 

## <a name="export-to-a-bacpac-file-using-the-azure-portal"></a>Exporteren naar een Bacpac-bestand met de Azure portal

Exporteren van een database met de [Azure-portal](https://portal.azure.com), open de pagina voor de database en klik op **exporteren** op de werkbalk. De Bacpac-bestandsnaam opgeven, geeft u de Azure-opslagaccount en container voor het exporteren en geef de referenties op om verbinding maken met de brondatabase.  

![Database wordt geëxporteerd](./media/sql-database-export/database-export.png)

Open de pagina voor de logische server met de database wordt geëxporteerd als u wilt de voortgang van de exportbewerking. Schuif omlaag naar **Operations** en klik vervolgens op **voor importeren/exporteren** geschiedenis.

![Geschiedenis exporteren](./media/sql-database-export/export-history.png)
![geschiedenis status exporteren](./media/sql-database-export/export-history2.png)

## <a name="export-to-a-bacpac-file-using-the-sqlpackage-utility"></a>Exporteren naar een Bacpac-bestand met het hulpprogramma SQLPackage

Exporteren van een SQL-database met de [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) opdrachtregelhulpprogramma Zie [parameters en eigenschappen exporteren](https://msdn.microsoft.com/library/hh550080.aspx#Export Parameters and Properties). Het hulpprogramma SQLPackage wordt geleverd met de nieuwste versies van [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) en [SQL Server Data Tools voor Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx), of u kunt de nieuwste versie van downloaden [SqlPackage ](https://www.microsoft.com/download/details.aspx?id=53876) rechtstreeks vanuit het Microsoft download center.

U wordt aangeraden het gebruik van het hulpprogramma SQLPackage voor schaal en prestaties in de meeste productieomgevingen. Raadpleeg dit blogartikel van het SQL Server-klantadviesteam over migratie met behulp van BACPAC-bestanden: [Migrating from SQL Server to Azure SQL Database using BACPAC Files](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/) (Migreren van SQL Server naar Azure SQL Database met BACPAC-bestanden).

In dit voorbeeld ziet u hoe een database met behulp van SqlPackage.exe met verificatie van Active Directory-universele exporteren:

```cmd
SqlPackage.exe /a:Export /tf:testExport.bacpac /scs:"Data Source=apptestserver.database.windows.net;Initial Catalog=MyDB;" /ua:True /tid:"apptest.onmicrosoft.com"
```

## <a name="export-to-a-bacpac-file-using-sql-server-management-studio-ssms"></a>Exporteren naar een Bacpac-bestand met behulp van SQL Server Management Studio (SSMS)

De nieuwste versies van SQL Server Management Studio bieden ook een wizard voor het exporteren van een Azure SQL Database naar een Bacpac-bestand. Zie de [exporteren van een Gegevenslaagtoepassing](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application).

## <a name="export-to-a-bacpac-file-using-powershell"></a>Exporteren naar een Bacpac-bestand met behulp van PowerShell

Gebruik de [nieuw AzureRmSqlDatabaseExport](/powershell/module/azurerm.sql/new-azurermsqldatabaseexport) cmdlet een aanvraag van de database exporteren voor de Azure SQL Database-service. De exportbewerking kan enige tijd duren, afhankelijk van de grootte van uw database.

 ```powershell
 $exportRequest = New-AzureRmSqlDatabaseExport -ResourceGroupName $ResourceGroupName -ServerName $ServerName `
   -DatabaseName $DatabaseName -StorageKeytype $StorageKeytype -StorageKey $StorageKey -StorageUri $BacpacUri `
   -AdministratorLogin $creds.UserName -AdministratorLoginPassword $creds.Password
 ```

Gebruiken om te controleren van de status van de aanvraag exporteren, de [Get-AzureRmSqlDatabaseImportExportStatus](/powershell/module/azurerm.sql/get-azurermsqldatabaseimportexportstatus) cmdlet. Met dit onmiddellijk na de aanvraag meestal retourneert **Status: InProgress**. Wanneer er **Status: geslaagd** het exporteren is voltooid.

```powershell
$exportStatus = Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $exportRequest.OperationStatusLink
[Console]::Write("Exporting")
while ($exportStatus.Status -eq "InProgress")
{
    $exportStatus = Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $exportRequest.OperationStatusLink
    [Console]::Write(".")
    Start-Sleep -s 10
}
[Console]::WriteLine("")
$exportStatus
```

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over langdurig bewaren van back-up van een back-up van Azure SQL database als een alternatief voor een database voor archief doeleinden geëxporteerd, [lange bewaartermijn van de back-](sql-database-long-term-retention.md).
- Raadpleeg dit blogartikel van het SQL Server-klantadviesteam over migratie met behulp van BACPAC-bestanden: [Migrating from SQL Server to Azure SQL Database using BACPAC Files](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/) (Migreren van SQL Server naar Azure SQL Database met BACPAC-bestanden).
* Zie voor meer informatie over het importeren van een BACPAC met een SQL Server-database, [een BACPCAC importeren in een SQL Server-database](https://msdn.microsoft.com/library/hh710052.aspx).
* Zie voor meer informatie over het exporteren van een BACPAC van een SQL Server-database, [exporteren van een Gegevenslaagtoepassing](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application) en [migreren van uw eerste database](sql-database-migrate-your-sql-server-database.md).
* Als u vanuit SQL Server als een prelude voor de migratie naar Azure SQL Database exporteert, raadpleegt u [een SQL Server-database migreren naar Azure SQL Database](sql-database-cloud-migrate.md).
