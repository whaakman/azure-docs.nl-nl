---
title: Exporteren van een enkele of gegroepeerde Azure SQL-database naar een BACPAC-bestand | Microsoft Docs
description: Een Azure SQL-database exporteren naar een BACPAC-bestand met de Azure portal
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: carlrab
manager: craigg
ms.date: 03/11/2019
ms.openlocfilehash: 27a65a871264fa13a42acfb5be2d4b5f99d31adc
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57758689"
---
# <a name="export-an-azure-sql-database-to-a-bacpac-file"></a>Een Azure SQL-database naar een BACPAC-bestand exporteren

Wanneer u nodig hebt voor het exporteren van een database voor het archiveren of voor het verplaatsen naar een ander platform, kunt u de database-schema en gegevens te exporteren een [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) bestand. Een BACPAC-bestand is een ZIP-bestand met de extensie van de metagegevens en gegevens uit een SQL Server-database met BACPAC. Een BACPAC-bestand kan worden opgeslagen in Azure Blob-opslag of lokale opslag in een on-premises locatie en later geïmporteerde terug in Azure SQL Database of in een SQL Server on-premises installatie.

## <a name="considerations-when-exporting-an-azure-sql-database"></a>Overwegingen bij het exporteren van een Azure SQL database

- Exporteren van een transactioneel consistent, moet u ervoor zorgen die geen schrijven activiteit plaatsvindt tijdens het exporteren, of dat u wilt exporteren uit een [transactioneel consistente kopie](sql-database-copy.md) van uw Azure SQL-database.
- Als u naar blob-opslag exporteert, is de maximale grootte van een BACPAC-bestand 200 GB. Als u wilt archiveren in een grotere BACPAC-bestand, exporteren naar de lokale opslag.
- Een BACPAC-bestand exporteren naar Azure premium storage met behulp van de methoden die worden beschreven in dit artikel wordt niet ondersteund.
- De opslag achter een firewall is momenteel niet ondersteund.
- Als de exportbewerking uit Azure SQL-Database groter is dan 20 uur, kan worden geannuleerd. Voor betere prestaties tijdens het exporteren, kunt u het volgende doen:

  - Tijdelijk de grootte van uw compute vergroten.
  - Niet meer alle lezen en schrijven van activiteit tijdens het exporteren.
  - Gebruik een [geclusterde index](https://msdn.microsoft.com/library/ms190457.aspx) met niet-null-waarden voor alle grote tabellen. Zonder geclusterde indexen, worden een export kan mislukken als het langer duurt dan 6 of 12 uur. Dit komt doordat de export-service een tabelscan moet om te exporteren van de hele tabel te voltooien. Een goede manier om te bepalen als uw tabellen zijn geoptimaliseerd voor exporteren is om uit te voeren **DBCC SHOW_STATISTICS** en zorg ervoor dat de *RANGE_HI_KEY* niet null is en de waarde ervan is goed distributie. Zie voor meer informatie, [DBCC SHOW_STATISTICS](https://msdn.microsoft.com/library/ms174384.aspx).

> [!NOTE]
> BACPACs zijn niet bedoeld om te worden gebruikt voor back-up en herstelbewerkingen. Azure SQL Database maakt automatisch back-ups voor elke gebruikersdatabase. Zie voor meer informatie, [overzicht voor zakelijke continuïteit](sql-database-business-continuity.md) en [back-ups van SQL-Database](sql-database-automated-backups.md).

## <a name="export-to-a-bacpac-file-using-the-azure-portal"></a>Exporteren naar een BACPAC-bestand met de Azure portal

> [!NOTE]
> [Een beheerd exemplaar](sql-database-managed-instance.md) biedt momenteel geen ondersteuning voor het exporteren van een database naar een BACPAC-bestand met de Azure-portal. Als u wilt een beheerd exemplaar naar een BACPAC-bestand exporteren, moet u SQL Server Management Studio of SQLPackage gebruiken.

1. Voor het exporteren van een database met de [Azure-portal](https://portal.azure.com), opent u de pagina voor uw database en klik op **exporteren** op de werkbalk.

   ![database exporteren](./media/sql-database-export/database-export1.png)

2. Geef de bestandsnaam van het Bacpac-, selecteer een bestaand Azure storage-account en een container voor het exporteren en vervolgens de juiste referenties opgeven voor toegang tot de brondatabase.

    ![database exporteren](./media/sql-database-export/database-export2.png)

3. Klik op **OK**.

4. Voor het controleren van de voortgang van de exportbewerking, open de pagina voor de SQL Database-server met de database die wordt geëxporteerd. Klik onder aan **instellingen** en klik vervolgens op **geschiedenis van importeren/exporteren**.

   ![Geschiedenis van exporteren](./media/sql-database-export/export-history.png)

## <a name="export-to-a-bacpac-file-using-the-sqlpackage-utility"></a>Exporteren naar een BACPAC-bestand met het hulpprogramma SQLPackage

Voor het exporteren van een SQL-database met de [SqlPackage](https://docs.microsoft.com/sql/tools/sqlpackage) opdrachtregel-hulpprogramma, Zie [exporteren parameters en eigenschappen](https://docs.microsoft.com/sql/tools/sqlpackage#export-parameters-and-properties). Het hulpprogramma SQLPackage wordt geleverd met de nieuwste versies van [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) en [SQL Server Data Tools voor Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx), maar u kunt de nieuwste versie van downloaden [SqlPackage ](https://www.microsoft.com/download/details.aspx?id=53876) rechtstreeks vanuit het Microsoft download center.

We raden het gebruik van het hulpprogramma SQLPackage voor schaalbaarheid en prestaties in de meeste productieomgevingen. Raadpleeg dit blogartikel van het SQL Server-klantadviesteam over migratie met behulp van BACPAC-bestanden: [Migrating from SQL Server to Azure SQL Database using BACPAC Files](https://blogs.msdn.microsoft.com/sqlcat/20../../migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/) (Migreren van SQL Server naar Azure SQL Database met BACPAC-bestanden).

In dit voorbeeld laat zien hoe een database met SqlPackage.exe met Active Directory universele authenticatie exporteren:

```cmd
SqlPackage.exe /a:Export /tf:testExport.bacpac /scs:"Data Source=apptestserver.database.windows.net;Initial Catalog=MyDB;" /ua:True /tid:"apptest.onmicrosoft.com"
```

## <a name="export-to-a-bacpac-file-using-sql-server-management-studio-ssms"></a>Exporteren naar een BACPAC-bestand met behulp van SQL Server Management Studio (SSMS)

De nieuwste versies van SQL Server Management Studio biedt een wizard voor het exporteren van een Azure SQL-database naar een BACPAC-bestand. Zie de [exporteren van een Data-tier-toepassing](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application).

## <a name="export-to-a-bacpac-file-using-powershell"></a>Exporteren naar een BACPAC-bestand met behulp van PowerShell

> [!NOTE]
> [Een beheerd exemplaar](sql-database-managed-instance.md) biedt momenteel geen ondersteuning voor het exporteren van een database naar een BACPAC-bestand met behulp van Azure PowerShell. Als u wilt een beheerd exemplaar naar een BACPAC-bestand exporteren, moet u SQL Server Management Studio of SQLPackage gebruiken.

Gebruik de [New-AzSqlDatabaseExport](/powershell/module/az.sql/new-azsqldatabaseexport) cmdlet voor het indienen van een aanvraag van de database exporteren naar de Azure SQL Database-service. De exportbewerking kan enige tijd duren, afhankelijk van de grootte van uw database.

```powershell
$exportRequest = New-AzSqlDatabaseExport -ResourceGroupName $ResourceGroupName -ServerName $ServerName `
  -DatabaseName $DatabaseName -StorageKeytype $StorageKeytype -StorageKey $StorageKey -StorageUri $BacpacUri `
  -AdministratorLogin $creds.UserName -AdministratorLoginPassword $creds.Password
```

Om te controleren of de status van de aanvraag voor exporteren, gebruikt u de [Get-AzSqlDatabaseImportExportStatus](/powershell/module/az.sql/get-azsqldatabaseimportexportstatus) cmdlet. Met dit onmiddellijk na de aanvraag meestal retourneert **Status: InProgress**. Wanneer de melding **Status: Geslaagd** het exporteren is voltooid.

```powershell
$exportStatus = Get-AzSqlDatabaseImportExportStatus -OperationStatusLink $exportRequest.OperationStatusLink
[Console]::Write("Exporting")
while ($exportStatus.Status -eq "InProgress")
{
    Start-Sleep -s 10
    $exportStatus = Get-AzSqlDatabaseImportExportStatus -OperationStatusLink $exportRequest.OperationStatusLink
    [Console]::Write(".")
}
[Console]::WriteLine("")
$exportStatus
```

## <a name="next-steps"></a>Volgende stappen

- Voor meer informatie over langetermijnretentie van back-up van een enkele databases en gepoolde databases als alternatief voor een database voor archief-toepassing wordt geëxporteerd Zie [langetermijnretentie](sql-database-long-term-retention.md). U kunt SQL Agent-taken gebruiken om te plannen [kopie-alleen back-ups](https://docs.microsoft.com/sql/relational-databases/backup-restore/copy-only-backups-sql-server) als alternatief voor langetermijnretentie van back-up.
- Raadpleeg dit blogartikel van het SQL Server-klantadviesteam over migratie met behulp van BACPAC-bestanden: [Migrating from SQL Server to Azure SQL Database using BACPAC Files](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/) (Migreren van SQL Server naar Azure SQL Database met BACPAC-bestanden).
- Zie voor meer informatie over het importeren van een BACPAC naar een SQL Server-database, [een BACPAC naar een SQL Server-database importeren](https://msdn.microsoft.com/library/hh710052.aspx).
- Zie voor meer informatie over het exporteren van een BACPAC van een SQL Server-database, [exporteren van een Data-tier-toepassing](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application)
- Zie voor meer informatie over het gebruik van de Data Migration Service voor het migreren van een database, [SQL-Server migreren naar Azure SQL Database offline met behulp van DMS](../dms/tutorial-sql-server-to-azure-sql.md).
- Als u van SQL Server als een voorproefje op voor de migratie naar Azure SQL Database exporteert, Zie [een SQL Server-database migreren naar Azure SQL Database](sql-database-single-database-migrate.md).
- Voor informatie over het beheren en te delen opslagsleutels en gedeelde toegang handtekeningen veilig, Zie [Azure Storage-beveiligingshandleiding](https://docs.microsoft.com/azure/storage/common/storage-security-guide).
