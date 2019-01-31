---
title: Een Azure SQL-database exporteren naar een BACPAC-bestand | Microsoft Docs
description: Een Azure SQL-database exporteren naar een BACPAC-bestand met de Azure portal
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: douglaslMS
ms.author: douglasl
ms.reviewer: carlrab
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: e980ec81f66e1045a4d9df33b3392589f5c1a668
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55463749"
---
# <a name="export-an-azure-sql-database-to-a-bacpac-file"></a>Een Azure SQL-database naar een BACPAC-bestand exporteren

Wanneer u nodig hebt voor het exporteren van een database voor het archiveren of voor het verplaatsen naar een ander platform, kunt u de database-schema en gegevens te exporteren een [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) bestand. Een BACPAC-bestand is een ZIP-bestand met de extensie van de metagegevens en gegevens uit een SQL Server-database met BACPAC. Een BACPAC-bestand kan worden opgeslagen in Azure blob-opslag of in de lokale opslag in een on-premises locatie en later geïmporteerd terug naar Azure SQL Database of een installatie van de on-premises SQL Server.

> [!IMPORTANT]
> Azure SQL Database functie voor geautomatiseerde Export is op 1 maart 2017 stopgezet. U kunt [langetermijnretentie](sql-database-long-term-retention.md
) of [Azure Automation](https://github.com/Microsoft/azure-docs/blob/2461f706f8fc1150e69312098640c0676206a531/articles/automation/automation-intro.md) periodiek archiveren SQL-databases met behulp van PowerShell volgens een schema van uw keuze. Voor het voorbeeld downloaden de [PowerShell-script voorbeeld](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-automation-automated-export) vanuit GitHub.
>

## <a name="considerations-when-exporting-an-azure-sql-database"></a>Overwegingen bij het exporteren van een Azure SQL database

- Exporteren van een transactioneel consistent, moet u ervoor zorgen die geen schrijven activiteit plaatsvindt tijdens het exporteren, of dat u wilt exporteren uit een [transactioneel consistente kopie](sql-database-copy.md) van uw Azure SQL-database.
- Als u naar blob-opslag exporteert, is de maximale grootte van een BACPAC-bestand 200 GB. Als u wilt archiveren in een grotere BACPAC-bestand, exporteren naar de lokale opslag.
- Een BACPAC-bestand exporteren naar Azure premium storage met behulp van de methoden die worden beschreven in dit artikel wordt niet ondersteund.
- Als de exportbewerking uit Azure SQL-Database groter is dan 20 uur, kan worden geannuleerd. Voor betere prestaties tijdens het exporteren, kunt u het volgende doen:
  - Tijdelijk de grootte van uw compute vergroten.
  - Niet meer alle lezen en schrijven van activiteit tijdens het exporteren.
  - Gebruik een [geclusterde index](https://msdn.microsoft.com/library/ms190457.aspx) met niet-null-waarden voor alle grote tabellen. Zonder geclusterde indexen, worden een export kan mislukken als het langer duurt dan 6 of 12 uur. Dit komt doordat de export-service een tabelscan moet om te exporteren van de hele tabel te voltooien. Een goede manier om te bepalen als uw tabellen zijn geoptimaliseerd voor exporteren is om uit te voeren **DBCC SHOW_STATISTICS** en zorg ervoor dat de *RANGE_HI_KEY* niet null is en de waarde ervan is goed distributie. Zie voor meer informatie, [DBCC SHOW_STATISTICS](https://msdn.microsoft.com/library/ms174384.aspx).

> [!NOTE]
> BACPACs zijn niet bedoeld om te worden gebruikt voor back-up en herstelbewerkingen. Azure SQL Database maakt automatisch back-ups voor elke gebruikersdatabase. Zie voor meer informatie, [overzicht voor zakelijke continuïteit](sql-database-business-continuity.md) en [back-ups van SQL-Database](sql-database-automated-backups.md).

## <a name="export-to-a-bacpac-file-using-the-azure-portal"></a>Exporteren naar een BACPAC-bestand met de Azure portal

Voor het exporteren van een database met de [Azure-portal](https://portal.azure.com), opent u de pagina voor uw database en klik op **exporteren** op de werkbalk. Het BACPAC-bestandsnaam opgeven, geeft u de Azure-opslagaccount en container voor het exporteren en geef de referenties verbinding maken met de brondatabase.

![database exporteren](./media/sql-database-export/database-export.png)

Voor het controleren van de voortgang van de exportbewerking, open de pagina voor de SQL Database-server met de database die wordt geëxporteerd. Schuif omlaag naar **Operations** en klik vervolgens op **Import/Export** geschiedenis.

![Geschiedenis exporteren](./media/sql-database-export/export-history.png)
![geschiedenis status exporteren](./media/sql-database-export/export-history2.png)

## <a name="export-to-a-bacpac-file-using-the-sqlpackage-utility"></a>Exporteren naar een BACPAC-bestand met het hulpprogramma SQLPackage

Voor het exporteren van een SQL-database met de [SqlPackage](https://docs.microsoft.com/sql/tools/sqlpackage) opdrachtregel-hulpprogramma, Zie [exporteren parameters en eigenschappen](https://docs.microsoft.com/sql/tools/sqlpackage#export-parameters-and-properties). Het hulpprogramma SQLPackage wordt geleverd met de nieuwste versies van [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) en [SQL Server Data Tools voor Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx), maar u kunt de nieuwste versie van downloaden [SqlPackage ](https://www.microsoft.com/download/details.aspx?id=53876) rechtstreeks vanuit het Microsoft download center.

We raden het gebruik van het hulpprogramma SQLPackage voor schaalbaarheid en prestaties in de meeste productieomgevingen. Raadpleeg dit blogartikel van het SQL Server-klantadviesteam over migratie met behulp van BACPAC-bestanden: [Migrating from SQL Server to Azure SQL Database using BACPAC Files](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/) (Migreren van SQL Server naar Azure SQL Database met BACPAC-bestanden).

In dit voorbeeld laat zien hoe een database met SqlPackage.exe met Active Directory universele authenticatie exporteren:

```cmd
SqlPackage.exe /a:Export /tf:testExport.bacpac /scs:"Data Source=apptestserver.database.windows.net;Initial Catalog=MyDB;" /ua:True /tid:"apptest.onmicrosoft.com"
```

## <a name="export-to-a-bacpac-file-using-sql-server-management-studio-ssms"></a>Exporteren naar een BACPAC-bestand met behulp van SQL Server Management Studio (SSMS)

De nieuwste versies van SQL Server Management Studio bieden ook een wizard voor het exporteren van een Azure SQL Database naar een BACPAC-bestand. Zie de [exporteren van een Data-tier-toepassing](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application).

## <a name="export-to-a-bacpac-file-using-powershell"></a>Exporteren naar een BACPAC-bestand met behulp van PowerShell

Gebruik de [New-AzureRmSqlDatabaseExport](/powershell/module/azurerm.sql/new-azurermsqldatabaseexport) cmdlet voor het indienen van een aanvraag van de database exporteren naar de Azure SQL Database-service. De exportbewerking kan enige tijd duren, afhankelijk van de grootte van uw database.

```powershell
$exportRequest = New-AzureRmSqlDatabaseExport -ResourceGroupName $ResourceGroupName -ServerName $ServerName `
  -DatabaseName $DatabaseName -StorageKeytype $StorageKeytype -StorageKey $StorageKey -StorageUri $BacpacUri `
  -AdministratorLogin $creds.UserName -AdministratorLoginPassword $creds.Password
```

Om te controleren of de status van de aanvraag voor exporteren, gebruikt u de [Get-AzureRmSqlDatabaseImportExportStatus](/powershell/module/azurerm.sql/get-azurermsqldatabaseimportexportstatus) cmdlet. Met dit onmiddellijk na de aanvraag meestal retourneert **Status: InProgress**. Wanneer de melding **Status: Geslaagd** het exporteren is voltooid.

```powershell
$exportStatus = Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $exportRequest.OperationStatusLink
[Console]::Write("Exporting")
while ($exportStatus.Status -eq "InProgress")
{
    Start-Sleep -s 10
    $exportStatus = Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $exportRequest.OperationStatusLink
    [Console]::Write(".")
}
[Console]::WriteLine("")
$exportStatus
```

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over langetermijnretentie van back-up van een Azure SQL database-back-up als een alternatief voor een database voor archief doeleinden geëxporteerd, [langetermijnretentie](sql-database-long-term-retention.md).
- Raadpleeg dit blogartikel van het SQL Server-klantadviesteam over migratie met behulp van BACPAC-bestanden: [Migrating from SQL Server to Azure SQL Database using BACPAC Files](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/) (Migreren van SQL Server naar Azure SQL Database met BACPAC-bestanden).
- Zie voor meer informatie over het importeren van een BACPAC naar een SQL Server-database, [een BACPAC naar een SQL Server-database importeren](https://msdn.microsoft.com/library/hh710052.aspx).
- Zie voor meer informatie over het exporteren van een BACPAC van een SQL Server-database, [exporteren van een Data-tier-toepassing](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application)
- Zie voor meer informatie over het gebruik van de Data Migration Service voor het migreren van een database, [SQL-Server migreren naar Azure SQL Database offline met behulp van DMS](../dms/tutorial-sql-server-to-azure-sql.md).
- Als u van SQL Server als een voorproefje op voor de migratie naar Azure SQL Database exporteert, Zie [een SQL Server-database migreren naar Azure SQL Database](sql-database-cloud-migrate.md).
- Voor informatie over het beheren en te delen opslagsleutels en gedeelde toegang handtekeningen veilig, Zie [Azure Storage-beveiligingshandleiding](https://docs.microsoft.com/azure/storage/common/storage-security-guide).
