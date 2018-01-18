---
title: Een Azure SQL database terugzetten vanaf een back-up | Microsoft Docs
description: Meer informatie over het herstellen van de punt in tijd, waarmee u kunt terugdraaien een Azure SQL Database naar een eerder punt in tijd (maximaal 35 dagen).
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: monicar
ms.assetid: fd1d334d-a035-4a55-9446-d1cf750d9cf7
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: Active
ms.date: 11/20/2017
ms.author: carlrab
ms.openlocfilehash: 06eb02e408b95176ca99ca391b437a3751876fac
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2018
---
# <a name="recover-an-azure-sql-database-using-automated-database-backups"></a>Een Azure SQL-database herstelt via automatische databaseback-ups
SQL Database biedt deze opties voor het gebruik van de database recovery [automatische databaseback-ups](sql-database-automated-backups.md) en [back-ups in lange bewaartermijn](sql-database-long-term-retention.md). U kunt herstellen met een databaseback-up naar:

* Een nieuwe database op dezelfde logische server hersteld naar een bepaald punt in tijd binnen de bewaarperiode. 
* Een database op dezelfde logische server op het tijdstip van de verwijdering voor een verwijderde database herstellen.
* Een nieuwe database op een logische server in elke regio hersteld op het punt van de meest recente dagelijkse back-ups in geogerepliceerde blob-opslag (RA-GRS).

> [!IMPORTANT]
> Een bestaande database kan niet worden overschreven tijdens het terugzetten.
>

Een herstelde database maakt de kosten van een extra opslagruimte in de volgende omstandigheden: 
- Terugzetten van P11 – P15 S4 S12 of P1 – P6 als de maximale databasegrootte groter dan 500 GB is.
- Herstellen van P1 – P6 of PRS1 – PRS6 naar S4 S12 als de maximale databasegrootte groter dan 250 GB is.

De extra overhead is omdat de maximale grootte van de herstelde database groter dan de hoeveelheid opslagruimte opgenomen voor het prestatieniveau is en eventuele extra opslagruimte ingericht boven het inbegrepen bedrag is extra kosten verbonden.  Zie voor prijsinformatie van extra opslagruimte, het [pagina met prijzen SQL-Database](https://azure.microsoft.com/pricing/details/sql-database/).  Als de werkelijke hoeveelheid schijfruimte minder dan de hoeveelheid opslagruimte die zijn opgenomen is, kan klikt u vervolgens dit extra kosten verbonden worden vermeden door de maximale grootte van de database voor het bedrag opgenomen te beperken. Zie voor meer informatie over opslaggrootte van de database en het wijzigen van de maximale grootte van de database [één database limieten](sql-database-resource-limits.md#single-database-storage-sizes-and-performance-levels).  

> [!NOTE]
> [Automatische databaseback-ups](sql-database-automated-backups.md) worden gebruikt bij het maken van een [kopiëren van de database](sql-database-copy.md). 
>


## <a name="recovery-time"></a>Hersteltijd
De hersteltijd herstellen van een database met behulp van automatische databaseback-ups is beïnvloed door diverse factoren: 

* De grootte van de database
* Het prestatieniveau van de database
* Het aantal betrokken transactielogboeken
* De hoeveelheid activiteit die moet worden cookies als u wilt herstellen naar het herstelpunt
* De netwerkbandbreedte als het herstellen naar een andere regio 
* Het aantal gelijktijdige restore-aanvragen worden verwerkt in de doelregio. 
  
  Voor een zeer groot en/of actieve database, kan het terugzetten van enkele uren duren. Als er langdurige storing in een regio, is het mogelijk dat er grote aantallen geo-restore-aanvragen worden verwerkt door een andere regio's zijn. Wanneer er veel aanvragen, kan de hersteltijd verhogen voor databases in deze regio. De meeste database herstelt voltooid binnen de 12 uur.

Voor een enkele abonnement, er zijn enkele beperkingen op het aantal gelijktijdige restore-aanvragen (inclusief punt in tijd terugzetten, geo-restore en lange termijn bewaren van back-up) wordt ingediend en soepel:
|  | **Maximum aantal gelijktijdige aanvragen worden verwerkt** | **Maximum aantal gelijktijdige aanvragen worden verzonden** |
| :--- | --: | --: |
|Individuele database (per abonnement)|10|60|
|Elastische pool (per groep)|4|200|
||||

Er is geen ingebouwde functionaliteit bulksgewijs terugzetten. De [Azure SQL Database: volledige Server Recovery](https://gallery.technet.microsoft.com/Azure-SQL-Database-Full-82941666) script is een voorbeeld van een manier van deze taak uitvoeren.

> [!IMPORTANT]
> Als u wilt herstellen met behulp van automatische back-ups, moet u lid zijn van de rol Inzender van SQL-Server in het abonnement of de eigenaar van het abonnement worden. U kunt herstellen met behulp van Azure Portal, PowerShell of de REST-API. U kunt Transact-SQL niet gebruiken. 
> 

## <a name="point-in-time-restore"></a>Terugzetten naar eerder tijdstip

U kunt een bestaande database naar een eerder tijdstip herstellen als een nieuwe database op dezelfde logische server met de Azure portal [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.sql/restore-azurermsqldatabase), of de [REST-API](https://msdn.microsoft.com/library/azure/mt163685.aspx). 

> [!TIP]
> Zie voor een PowerShell-voorbeeldscript waarin wordt getoond hoe u een punt in tijd herstel uitvoeren van een database, [herstellen van een SQL-database met behulp van PowerShell](scripts/sql-database-restore-database-powershell.md).
>

De database kan worden hersteld naar een service tier of de prestaties, en als een individuele database of in een elastische pool. Zorg ervoor dat er voldoende bronnen op de logische server of in de elastische groep waaraan u de database terugzet. Als u klaar is, is de teruggezette database een normale, volledig toegankelijk zijn, online database. De teruggezette database wordt in rekening gebracht volgens de normale tarieven op basis van het serviceniveau prijscategorie en prestatieniveau. U bent niet worden kosten in rekening totdat het terugzetten van de database is voltooid.

U meestal herstellen een database naar een eerder voor hersteldoeleinden. Wanneer doet, kunt u de herstelde database behandelen als vervanging voor de oorspronkelijke database of gebruikt om gegevens van ophalen en werk vervolgens de oorspronkelijke database. 

* ***Vervanging van de database:*** als de herstelde database is bedoeld als vervanging voor de oorspronkelijke database, moet u controleren of het prestatieniveau en/of servicelaag geschikt zijn en schalen van de database indien nodig. U kunt de oorspronkelijke database wijzigen en vervolgens geeft de teruggezette database van de oorspronkelijke naam met behulp de [ALTER DATABASE](/sql/t-sql/statements/alter-database-azure-sql-database) opdracht in T-SQL. 
* ***Herstel van gegevens:*** als u van plan bent om gegevens te halen uit de herstelde database van een gebruiker of toepassing fout wilt herstellen, moet u schrijven en uitvoeren van de scripts voor het herstel van gegevens die nodig zijn om gegevens te extraheren uit de herstelde database naar de oorspronkelijke database. Hoewel de herstelbewerking lang duren kan om te voltooien, zijn terug te zetten database is zichtbaar in de lijst van de database in het herstelproces. Als u de database verwijderd tijdens het herstellen, de restore-bewerking is geannuleerd en u niet weet in rekening gebracht voor de database die de herstelbewerking kan niet worden voltooid. 

### <a name="azure-portal"></a>Azure Portal

Als u wilt herstellen naar een punt in tijd met de Azure portal, open de pagina voor de database en klik op **herstellen** op de werkbalk.

![punt in tijd terugzetten](./media/sql-database-recovery-using-backups/point-in-time-recovery.png)

## <a name="deleted-database-restore"></a>Herstellen van verwijderde database
U kunt een verwijderde database herstellen naar de tijd voor het verwijderen van een verwijderde database op dezelfde logische server met de Azure portal [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.sql/restore-azurermsqldatabase), of de [REST (createMode = herstellen)](https://msdn.microsoft.com/library/azure/mt163685.aspx). U kunt een verwijderde database herstellen naar een eerder tijdens het gebruik van de retentie [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.sql/restore-azurermsqldatabase).

> [!TIP]
> Zie voor een PowerShell-voorbeeldscript waarin wordt beschreven hoe u een verwijderde database te herstellen, [herstellen van een SQL-database met behulp van PowerShell](scripts/sql-database-restore-database-powershell.md).
>

> [!IMPORTANT]
> Als u een exemplaar van Azure SQL Database-server verwijdert, worden alle bijbehorende databases worden ook verwijderd en kunnen niet worden hersteld. Er is momenteel geen ondersteuning voor het herstellen van een server verwijderd.
> 

### <a name="azure-portal"></a>Azure Portal

Een verwijderde database tijdens het herstellen ervan [bewaarperiode](sql-database-service-tiers.md) met de Azure portal, open de pagina voor uw server en in het gebied van bewerkingen, klikt u op **databases verwijderd**.

![deleted-database-restore-1](./media/sql-database-recovery-using-backups/deleted-database-restore-1.png)


![deleted-database-restore-2](./media/sql-database-recovery-using-backups/deleted-database-restore-2.png)

## <a name="geo-restore"></a>Geo-herstel
U kunt een SQL-database op elke server in een Azure-regio herstellen met de meest recente geogerepliceerde volledige en differentiële back-ups. Geo-herstel een geografisch redundante back-up gebruikt als de bron- en kan worden gebruikt voor het herstellen van een database, zelfs als de database of het datacenter niet toegankelijk wegens een storing is. 

Geo-restore is de standaardoptie voor herstel wanneer de database is niet beschikbaar vanwege een incident in de regio waar de database wordt gehost. Als een grootschalige incident in een regio, worden niet beschikbaar zijn van uw databasetoepassing, kunt u een database herstellen vanuit back-ups geogerepliceerde naar een server in een andere regio. Er is een vertraging tussen wanneer een differentiële back-up is gemaakt en wanneer het zich geografisch worden gerepliceerd naar een Azure-blob in een andere regio. Deze vertraging mag maximaal een uur, dus als er een ramp optreedt, kan er van één uur gegevens verloren gaan. De volgende afbeelding ziet het herstellen van de database van de laatste beschikbare back-up in een andere regio.

![geo-herstel](./media/sql-database-geo-restore/geo-restore-2.png)

> [!TIP]
> Zie voor een PowerShell-voorbeeldscript waarin wordt getoond hoe een geo-herstel uitvoeren, [herstellen van een SQL-database met behulp van PowerShell](scripts/sql-database-restore-database-powershell.md).
> 

Punt in tijd terugzetten op een secundaire geo-server is momenteel niet ondersteund. Punt in tijd terugzetten kan alleen op een primaire database worden uitgevoerd. Zie voor gedetailleerde informatie over het herstellen van een storing met geo-restore [herstellen van een storing](sql-database-disaster-recovery.md).

> [!IMPORTANT]
> Herstel van back-ups is de eenvoudigste noodhersteloplossingen die beschikbaar zijn in SQL-Database met de langste Recovery Point Objective (RPO) en de schatting herstel tijd (invoegen). Geo-restore is voor oplossingen met behulp van Basic databases, vaak een redelijke DR-oplossing met een invoegen van 12 uur. Voor oplossingen met behulp van grotere Standard of Premium-databases die korter hersteltijden nodig hebt, moet u overwegen [actieve geo-replicatie](sql-database-geo-replication-overview.md). Actieve geo-replicatie biedt een veel lager RPO en invoegen als u alleen hoeft initieer een failover naar een continu gerepliceerde secundaire. Zie voor meer informatie over zakelijke continuïteit keuzes [overzicht van zakelijke continuïteit](sql-database-business-continuity.md).
> 

### <a name="azure-portal"></a>Azure Portal

Op het geo-terugzetten van een database tijdens de [bewaarperiode](sql-database-service-tiers.md) met de Azure portal, open de pagina SQL-Databases en klik vervolgens op **toevoegen**. In de **Select bron** in het tekstvak, selecteer **back-up**. Geef de back-up van waaruit het herstel wilt uitvoeren in de regio en op de server van uw keuze. 

## <a name="programmatically-performing-recovery-using-automated-backups"></a>Herstel met behulp van automatische back-ups uitvoeren via een programma
Als eerder beschreven, naast de Azure portal, databaseherstel, worden uitgevoerd via een programma met Azure PowerShell of de REST-API. De volgende tabellen beschrijven de reeks opdrachten die beschikbaar zijn.

### <a name="powershell"></a>PowerShell
| Cmdlet | Beschrijving |
| --- | --- |
| [Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase) |Hiermee haalt u een of meer databases. |
| [Get-AzureRMSqlDeletedDatabaseBackup](/powershell/module/azurerm.sql/get-azurermsqldeleteddatabasebackup) | Hiermee haalt u een verwijderde database die u kunt herstellen. |
| [Get-AzureRmSqlDatabaseGeoBackup](/powershell/module/azurerm.sql/get-azurermsqldatabasegeobackup) |Hiermee haalt u een geografisch redundante back-up van een database. |
| [Restore-AzureRmSqlDatabase](/powershell/module/azurerm.sql/restore-azurermsqldatabase) |Een SQL-database herstelt. |
|  | |

### <a name="rest-api"></a>REST-API
| API | Beschrijving |
| --- | --- |
| [REST (createMode=Recovery)](https://msdn.microsoft.com/library/azure/mt163685.aspx) |Hiermee herstelt u een database |
| [Get maken of bijwerken van de databasestatus](https://msdn.microsoft.com/library/azure/mt643934.aspx) |Hiermee wordt de status geretourneerd tijdens een terugzetbewerking |
|  | |

## <a name="summary"></a>Samenvatting
Automatische back-ups beveiligen uw databases van gebruikers- en toepassingsfouten, verwijderen van een database per ongeluk en langdurige uitval. Deze ingebouwde functie is beschikbaar voor alle Servicelagen en prestatieniveaus. 

## <a name="next-steps"></a>Volgende stappen
* Zie voor een overzicht van zakelijke continuïteit en scenario's [Business continuity overview](sql-database-business-continuity.md).
* Voor meer informatie over Azure SQL Database geautomatiseerde back-ups, Zie [geautomatiseerde back-ups van SQL-Database](sql-database-automated-backups.md).
* Zie voor meer informatie over lange bewaartermijn van de back-up, [lange bewaartermijn van de back-](sql-database-long-term-retention.md).
* Configureren, beheren en herstellen van lange bewaartermijn van automatische back-ups in een Azure Recovery Services-kluis met de Azure portal, Zie [configureren en gebruiken op lange termijn back-up bewaren](sql-database-long-term-backup-retention-configure.md). 
* Zie voor meer informatie over opties voor sneller herstel, [Failover groepen en actieve geo-replicatie](sql-database-geo-replication-overview.md).  
