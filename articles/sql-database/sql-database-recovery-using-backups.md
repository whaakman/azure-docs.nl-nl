---
title: Een Azure SQL-database herstellen vanuit een back-up | Microsoft Docs
description: Meer informatie over Point-in-Time-Restore, waarmee u terug moet brengen een Azure SQL Database naar een eerder punt in tijd (maximaal 35 dagen).
services: sql-database
ms.service: sql-database
ms.subservice: backup-restore
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
manager: craigg
ms.date: 10/23/2018
ms.openlocfilehash: 8785ef50cd7bd27e866b1fa546317021eed5da01
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53599098"
---
# <a name="recover-an-azure-sql-database-using-automated-database-backups"></a>Een Azure SQL-database met behulp van geautomatiseerde databaseback-ups herstellen

Back-ups van SQL-Database worden standaard opgeslagen in geo-replicatie blob-opslag (RA-GRS). De volgende opties zijn beschikbaar voor het gebruik van de database recovery [automatische databaseback-ups](sql-database-automated-backups.md):

- Maak een nieuwe database op dezelfde logische server hersteld naar een opgegeven punt in tijd binnen de bewaarperiode liggen.
- Maak een database in dezelfde logische server op het tijdstip van verwijdering van een verwijderde database kunt herstellen.
- Maak een nieuwe database op een logische server in dezelfde regio hersteld tot het tijdstip waarop de meest recente back-ups.
- Maak een nieuwe database op een logische server in een andere regio op het punt van de meest recente gerepliceerde back-ups kunt herstellen.

Als u hebt geconfigureerd [back-up maken met een langetermijnbewaarperiode](sql-database-long-term-retention.md) u kunt ook een nieuwe database maken van alle LTR back-up op een logische server in elke regio.

> [!IMPORTANT]
> U kunt een bestaande database tijdens het terugzetten niet overschrijven.

Bij het gebruik van Standard of Premium-servicelaag, maakt een herstelde database een kosten extra opslagruimte in de volgende omstandigheden:

- Herstellen van P11-P15 S4-S12 of P1 – P6 als de maximale databasegrootte groter is dan 500 GB.
- Herstellen van P1 – P6 voor S4-S12 als de maximale grootte van de database groter dan 250 GB is.

De extra kosten zijn omdat de maximale grootte van de herstelde database groter dan de hoeveelheid opslag die is opgenomen voor de compute-grootte is en alle extra opslagruimte die boven de inbegrepen hoeveelheid in rekening extra gebracht wordt. Informatie over prijzen van extra opslagruimte, Zie de [pagina met prijzen van SQL-Database](https://azure.microsoft.com/pricing/details/sql-database/). Als de werkelijke hoeveelheid ruimte die lager dan de hoeveelheid opslag die is opgenomen is, kan klikt u vervolgens deze extra kosten worden vermeden door de maximale grootte van de database voor de hoeveelheid die inclusief te beperken.

> [!NOTE]
> [Automatische databaseback-ups](sql-database-automated-backups.md) worden gebruikt bij het maken van een [kopiëren van de database](sql-database-copy.md).

## <a name="recovery-time"></a>Hersteltijd

De hersteltijd om terug te zetten van een database met behulp van geautomatiseerde databaseback-ups wordt beïnvloed door diverse factoren:

- De grootte van de database
- De compute-grootte van de database
- Het aantal betrokken transactielogboeken
- De hoeveelheid activiteit die moet opnieuw worden afgespeeld om te herstellen naar het herstelpunt
- De bandbreedte van het netwerk als het herstellen naar een andere regio
- Het aantal gelijktijdige restore-aanvragen worden verwerkt in de doelregio

Voor een zeer grote en/of actieve database, kan het terugzetten van enkele uren duren. Als er langdurige storing in een regio, is het mogelijk dat er grote aantallen geo-restore-aanvragen worden verwerkt door een andere regio's zijn. Wanneer er veel aanvragen, verhogen de hersteltijd voor databases in deze regio. De meeste worden voltooid in minder dan 12 uur hersteld.

Voor één abonnement, er zijn enkele beperkingen met betrekking tot het aantal aanvragen van gelijktijdige terugzetten (met inbegrip van punt in tijd herstel, geo-restore en op lange termijn bewaren back-up) die worden verzonden en zijn:

| | **Maximum aantal gelijktijdige aanvragen worden verwerkt** | **Maximum aantal gelijktijdige aanvragen worden ingediend** |
| :--- | --: | --: |
|Individuele database (per abonnement)|10|60|
|Elastische pool (per groep)|4|200|
||||

Er is geen ingebouwde functionaliteit bulksgewijs terugzetten. De [Azure SQL Database: Volledige Serverherstelproces](https://gallery.technet.microsoft.com/Azure-SQL-Database-Full-82941666) script is een voorbeeld van een manier om deze taak uitvoeren.

> [!IMPORTANT]
> Als u wilt herstellen met behulp van geautomatiseerde back-ups, moet u lid zijn van de rol Inzender voor SQL Server in het abonnement of raadpleegt u de eigenaar van de abonnement - [RBAC: Ingebouwde rollen](../role-based-access-control/built-in-roles.md). U kunt herstellen met behulp van Azure Portal, PowerShell of de REST-API. U kunt Transact-SQL niet gebruiken.

## <a name="point-in-time-restore"></a>Terugzetten naar eerder tijdstip

U kunt een enkele, gegroepeerde of beheerde exemplaar in de database naar een eerder tijdstip herstellen in tijd als een nieuwe database op dezelfde server met behulp van Azure portal, [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.sql/restore-azurermsqldatabase), of de [REST-API](https://docs.microsoft.com/rest/api/sql/databases). Een database kan worden hersteld naar een servicelaag of compute-grootte. Zorg ervoor dat er voldoende bronnen op de server waarop u de database wilt herstellen. Als u klaar bent, is de herstelde database een normale, volledig toegankelijk zijn, online-database. De herstelde database gelden de normale tarieven op basis van de servicelaag en de rekencapaciteit. U doet geen kosten in rekening totdat het herstellen van de database is voltooid.

U kunt in het algemeen een database herstellen naar een eerder tijdstip voor herstel voor. Wanneer doet, kunt u de herstelde database behandelen als vervanging voor de oorspronkelijke database of deze gebruiken voor het ophalen van gegevens uit en werk vervolgens de oorspronkelijke database.

- **Vervanging van de database**

  Als de herstelde database is bedoeld als vervanging voor de oorspronkelijke database, moet u controleren of de grootte van de compute en/of servicelaag geschikt zijn en schalen van de database indien nodig. U kunt Wijzig de naam van de oorspronkelijke database en geef de herstelde database of de oorspronkelijke naam met behulp van de [ALTER DATABASE](/sql/t-sql/statements/alter-database-azure-sql-database) opdracht in T-SQL.

- **Herstel van gegevens**

  Als u van plan bent voor het ophalen van gegevens uit de herstelde database te herstellen van een gebruiker of toepassing fout, moet u om te schrijven en uitvoeren van de scripts voor het herstel van gegevens die nodig zijn om gegevens te extraheren uit de herstelde database naar de oorspronkelijke database. Hoewel de herstelbewerking lang duren kan om uit te voeren, is het herstellen van de database is zichtbaar in de lijst van de database tijdens het herstelproces. Als u de database tijdens de terugzetbewerking verwijdert, de herstelbewerking is geannuleerd en u betaalt geen voor de database die u hebt niet de terugzetbewerking niet voltooien.

Als u wilt herstellen van een enkele, gegroepeerde of beheerde exemplaar in de database naar een tijdstip met behulp van de Azure-portal, open de pagina voor uw database en klik op **herstellen** op de werkbalk.

![punt-in-time-restore](./media/sql-database-recovery-using-backups/point-in-time-recovery.png)

> [!IMPORTANT]
> Programmatisch database herstellen vanuit een back-up, Zie [via een programma uitvoeren recovery met behulp van geautomatiseerde back-ups](sql-database-recovery-using-backups.md#programmatically-performing-recovery-using-automated-backups)

## <a name="deleted-database-restore"></a>Verwijderde database herstellen

U kunt een verwijderde database herstellen naar de tijd van verwijdering van een verwijderde database op dezelfde logische server met behulp van Azure portal, [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.sql/restore-azurermsqldatabase), of de [REST (createMode = terugzetten)](https://docs.microsoft.com/rest/api/sql/databases/createorupdate). U kunt een verwijderde database herstellen naar een eerder tijdstip tijdens de bewaarperiode via [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.sql/restore-azurermsqldatabase).

> [!Note]
> Het herstellen van verwijderde database is niet beschikbaar in het beheerde exemplaar.
> [!TIP]
> Zie voor een voorbeeld van PowerShell-script waarin wordt getoond hoe een verwijderde database herstellen, [herstellen van een SQL-database met behulp van PowerShell](scripts/sql-database-restore-database-powershell.md).
> [!IMPORTANT]
> Als u een Azure SQL Database server-exemplaar verwijdert, worden alle bijbehorende databases worden ook verwijderd en kunnen niet worden hersteld. Er is momenteel geen ondersteuning voor het herstellen van een server verwijderd.

### <a name="deleted-database-restore-using-the-azure-portal"></a>Verwijderde database herstellen met behulp van de Azure portal

Om te herstellen van een verwijderde database met behulp van de Azure portal tijdens de [bewaarperiode op basis van DTU-model](sql-database-service-tiers-dtu.md) of [bewaarperiode op vCore gebaseerde model](sql-database-service-tiers-vcore.md) met behulp van Azure portal, open de pagina voor uw server en in de Operations-gebied, klikt u op **verwijderde databases**.

![deleted-database-restore-1](./media/sql-database-recovery-using-backups/deleted-database-restore-1.png)

![deleted-database-restore-2](./media/sql-database-recovery-using-backups/deleted-database-restore-2.png)

> [!IMPORTANT]
> Als u programmatisch een verwijderde database herstellen, Zie [via een programma uitvoeren recovery met behulp van geautomatiseerde back-ups](sql-database-recovery-using-backups.md#programmatically-performing-recovery-using-automated-backups)

## <a name="geo-restore"></a>Geo-herstel

U kunt een SQL-database op elke server in een Azure-regio van de meest recente geo-replicatie back-ups herstellen. Geo-herstel een geografisch redundante back-up gebruikt als de bron- en kan worden gebruikt om een database herstellen, zelfs als de database of het datacenter niet toegankelijk als gevolg van een storing is.

> [!Note]
> Geo-restore is niet beschikbaar in het beheerde exemplaar.

Geo-restore is de standaardoptie voor herstel wanneer de database is niet beschikbaar vanwege een incident in de regio waar de database wordt gehost. Als een grootschalige incident in een regio, resulteert in niet-beschikbaarheid van uw databasetoepassing, kunt u een database herstellen vanuit back-ups via geo-replicatie naar een server in een andere regio. Er is een vertraging tussen wanneer een back-up is gemaakt en wanneer het zich geo-replicatie naar een Azure blob in een andere regio. Deze vertraging mag maximaal een uur, dus als er zich een noodgeval voordoet, kan er van één uur gegevens verloren gaan. De volgende afbeelding ziet het herstellen van de database van de laatste beschikbare back-up in een andere regio.

![Geo-herstel](./media/sql-database-geo-restore/geo-restore-2.png)

> [!TIP]
> Zie voor een voorbeeld van PowerShell-script waarin wordt getoond hoe om uit te voeren van een geo-restore, [herstellen van een SQL-database met behulp van PowerShell](scripts/sql-database-restore-database-powershell.md).

Point-in-time terugzetten op een secundaire geo-server is momenteel niet ondersteund. Point-in-time restore kan alleen op een primaire database worden uitgevoerd. Zie voor gedetailleerde informatie over het gebruik van geo-herstel te herstellen na een storing [herstellen na een storing](sql-database-disaster-recovery.md).

> [!IMPORTANT]
> Herstel vanuit back-ups is de meest eenvoudige noodhersteloplossingen die beschikbaar zijn in SQL Database met de langste Recovery Point Objective (RPO) en een schatting Recovery tijd (ERT). Geo-restore is voor oplossingen met behulp van de databases klein (bijvoorbeeld basis-servicelaag of klein formaat tenant-databases in elastische pools) vaak een redelijke DR-oplossing met een ERT van maximaal 12 uur (doorgaans veel minder). Voor oplossingen met behulp van grote databases en kortere vereist, moet u overwegen [actieve geo-replicatie](sql-database-active-geo-replication.md) of [automatische failovergroepen](sql-database-auto-failover-group.md). Actieve geo-replicatie biedt een veel lager RPO en ERT als alleen u moet start een failover naar een secundaire continu gerepliceerd. Automatische failovergroepen inschakelen automatische failover voor een groep databases. Zie voor meer informatie over opties voor bedrijfscontinuïteit, [overzicht van bedrijfscontinuïteit](sql-database-business-continuity.md).

### <a name="geo-restore-using-the-azure-portal"></a>Geo-herstellen met behulp van de Azure-portal

Voor geo-restore a database tijdens de [bewaarperiode op basis van DTU-model](sql-database-service-tiers-dtu.md) of [bewaarperiode op vCore gebaseerde model](sql-database-service-tiers-vcore.md) met behulp van de Azure-portal, open de pagina SQL-Databases en klik vervolgens op **toevoegen** . In de **bron selecteren** in het tekstvak, selecteer **back-up**. Geef de back-up van waaruit het herstel wilt uitvoeren in de regio en op de server van uw keuze.

## <a name="programmatically-performing-recovery-using-automated-backups"></a>Herstel met behulp van geautomatiseerde back-ups uitvoeren via een programma

Zoals eerder is besproken, naast de Azure portal, kan databaseherstel worden uitgevoerd via een programma met behulp van Azure PowerShell of de REST-API. De volgende tabellen beschrijven de reeks opdrachten die beschikbaar zijn.

### <a name="powershell"></a>PowerShell

- Een enkele of gegroepeerde als database wilt herstellen, Zie [Restore-AzureRmSqlDatabase](https://docs.microsoft.com/powershell/module/azurerm.sql/restore-azurermsqldatabase)

  | Cmdlet | Description |
  | --- | --- |
  | [Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase) |Hiermee haalt u een of meer databases op. |
  | [Get-AzureRMSqlDeletedDatabaseBackup](/powershell/module/azurerm.sql/get-azurermsqldeleteddatabasebackup) | Hiermee haalt u een verwijderde database die u kunt herstellen op. |
  | [Get-AzureRmSqlDatabaseGeoBackup](/powershell/module/azurerm.sql/get-azurermsqldatabasegeobackup) |Hiermee haalt u een geografisch redundante back-up van een database op. |
  | [Restore-AzureRmSqlDatabase](/powershell/module/azurerm.sql/restore-azurermsqldatabase) |Hiermee herstelt u een SQL-database. |

  > [!TIP]
  > Zie voor een voorbeeld van PowerShell-script waarin wordt getoond hoe om uit te voeren van een point-in-time-terugzetten van een database, [herstellen van een SQL-database met behulp van PowerShell](scripts/sql-database-restore-database-powershell.md).

- Als u een beheerd exemplaar in de database herstellen, Zie [Point-in-time-terugzetten van een database in Azure SQL Managed Instance met behulp van AzureRm.Sql PowerShell-bibliotheek](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/06/28/point-in-time-restore-of-a-database-on-azure-sql-managed-instance-using-azurerm-sql-powershell-library/)

### <a name="rest-api"></a>REST-API

Een enkele of gegroepeerde om database te herstellen met behulp van de REST-API:

| API | Description |
| --- | --- |
| [REST (createMode = Recovery)](https://docs.microsoft.com/rest/api/sql/databases) |Hiermee herstelt u een database |
| [Get maken of bijwerken van de Status van Database](https://docs.microsoft.com/rest/api/sql/operations) |De status geretourneerd tijdens een herstelbewerking |

### <a name="azure-cli"></a>Azure-CLI

Zie voor het herstellen van een enkele of gegroepeerde database met behulp van Azure CLI, [az sql db restore](https://docs.microsoft.com/cli/azure/sql/db#az-sql-db-restore).

## <a name="summary"></a>Samenvatting

Automatische back-ups beveiligen uw databases van gebruiker en toepassingsfouten, per ongeluk database verwijderen en langdurige uitval. Deze ingebouwde mogelijkheid is beschikbaar voor alle service-lagen en compute-grootten.

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht voor zakelijke continuïteit en scenario's, [overzicht voor zakelijke continuïteit](sql-database-business-continuity.md).
- Voor meer informatie over Azure SQL Database geautomatiseerde back-ups, Zie [geautomatiseerde back-ups van SQL-Database](sql-database-automated-backups.md).
- Zie voor meer informatie over met een langetermijnbewaarperiode, [langetermijnretentie](sql-database-long-term-retention.md).
- Zie voor meer informatie over opties voor sneller herstel, [actieve geo-replicatie](sql-database-active-geo-replication.md) of [automatische failovergroepen](sql-database-auto-failover-group.md).
