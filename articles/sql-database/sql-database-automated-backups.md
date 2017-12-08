---
title: Azure SQL Database automatische, geografisch redundante back-ups | Microsoft Docs
description: SQL-Database maakt een back-up van de lokale database om de paar minuten en maakt gebruik van Azure geografisch redundante opslag met leestoegang voor geografische redundantie automatisch.
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 3ee3d49d-16fa-47cf-a3ab-7b22aa491a8d
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: Active
ms.date: 07/05/2017
ms.author: carlrab
ms.openlocfilehash: 25fa8d03a206452c17e5d4ad38211203aeb5dd65
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2017
---
# <a name="learn-about-automatic-sql-database-backups"></a>Meer informatie over automatische back-ups van SQL-Database

SQL-Database maakt databaseback-ups en geografisch redundante opslag met Azure-leestoegang (RA-GRS) gebruikt om te voorzien van geografische redundantie automatisch. Deze back-ups worden gemaakt, automatisch en zonder extra kosten. U hoeft niet te ondernemen om ze gebeuren te maken. Databaseback-ups zijn een essentieel onderdeel van een zakelijke continuïteit en noodherstel herstelstrategie omdat ze uw gegevens tegen per ongeluk beschadigd of verwijderd beschermen. Als u wilt back-ups behouden in uw eigen storage-container kunt u een lange termijn back-up bewaarbeleid configureren. Zie [Langetermijnretentie](sql-database-long-term-retention.md) voor meer informatie.

## <a name="what-is-a-sql-database-backup"></a>Wat is er een back-up van de SQL-Database?

SQL-Database maakt met SQL Server-technologie [volledige](https://msdn.microsoft.com/library/ms186289.aspx), [differentiële](https://msdn.microsoft.com/library/ms175526.aspx), en [transactielogboek](https://msdn.microsoft.com/library/ms191429.aspx) back-ups. Elke 5-10 minuten de transactielogboekback-ups in het algemeen gebeuren met de frequentie op basis van het prestatieniveau en de hoeveelheid activiteit in een database. Transactielogboekback-ups, volledige en differentiële back-ups maakt, kunnen u een database herstellen naar een specifieke point-in-time naar dezelfde server die als host fungeert voor de database. Wanneer u een database herstelt, de service weten te achterhalen welke volledige en differentiële transactie logboek back-ups moeten worden teruggezet.


U kunt deze back-ups te gebruiken:

* Een database herstellen naar een punt in tijd binnen de bewaarperiode. Deze bewerking wordt een nieuwe database maken op dezelfde server als de oorspronkelijke database.
* Een verwijderde database herstellen naar de tijd die is verwijderd of binnen de bewaarperiode. De verwijderde database kan alleen worden teruggezet op dezelfde server waarop de oorspronkelijke database is gemaakt.
* Een database herstellen naar een andere geografische regio. Hiermee kunt u herstellen van een geografische noodgeval wanneer u geen toegang uw server en database tot. Deze maakt een nieuwe database in een bestaande server overal ter wereld. 
* Een database terugzetten vanaf een specifieke back-up zijn opgeslagen in uw Azure Recovery Services-kluis. Hiermee kunt u een oude versie van de database om te voldoen aan een aanvraag voor naleving of uit te voeren een oude versie van de toepassing te herstellen. Zie [lange bewaartermijn](sql-database-long-term-retention.md).
* Als u wilt een herstel uitvoeren, Zie [database terugzetten vanaf back-ups](sql-database-recovery-using-backups.md).

> [!NOTE]
> In Azure-opslag, de term *replicatie* verwijst naar het kopiëren van bestanden van de ene locatie naar een andere. De SQL *databasereplicatie* verwijst naar en meerdere secundaire databases worden gesynchroniseerd met een primaire database behouden blijft. 
> 

## <a name="how-much-backup-storage-is-included-at-no-cost"></a>Hoeveel back-upopslag is opgenomen kosteloos?
SQL-Database biedt tot 200% van de maximale ingerichte databaseopslag als back-upopslag zonder extra kosten. Bijvoorbeeld, als er een Standard database-exemplaar met een ingerichte DB-grootte van 250 GB, hebt u 500 GB aan back-upopslag zonder extra kosten. 

## <a name="how-often-do-backups-happen"></a>Hoe vaak gebeuren back-ups?
Volledige databaseback-ups gebeuren wekelijks, differentiële back-ups in het algemeen gebeuren elk paar uur en transactielogboek back-ups in het algemeen gebeuren elke 5-10 minuten. De eerste volledige back-up is gepland onmiddellijk nadat een database wordt gemaakt. Meestal is voltooid binnen 30 minuten, maar het kan langer duren wanneer de database van een aanzienlijke grootte. De eerste back-up kan onder andere op een herstelde database of een databasekopie langer duren. Na de eerste volledige back-up, worden alle verdere back-ups automatisch gepland en achtergrond beheerd op de achtergrond. De precieze timing van alle databaseback-ups wordt bepaald door de service SQL Database als het een compromis tussen de werkbelasting van het algehele systeem. 

De back-upopslag geo-replicatie vindt plaats op basis van de planning van Azure Storage-replicatie.

## <a name="how-long-do-you-keep-my-backups"></a>Hoe lang houdt u de back-ups?
De back-up van elke SQL-Database heeft een bewaarperiode die is gebaseerd op de [servicelaag](sql-database-service-tiers.md) van de database. De bewaarperiode voor een database in de:


* Basic-servicelaag is 7 dagen.
* Standaard-servicelaag is 35 dagen.
* Premium servicecategorie is 35 dagen.

Als u uw database, uit de Standard of Premium-Servicelagen Basic gedowngraded, worden de back-ups opgeslagen voor de zeven dagen. Alle bestaande back-ups ouder zijn dan zeven dagen zijn niet meer beschikbaar. 

Als u de database van de laag Basic service naar Standard of Premium bijwerkt, houdt bij SQL-Database bestaande back-ups totdat deze 35 dagen oud zijn. Nieuwe back-ups blijven wanneer deze zich voor 35 dagen voordoen.

Als u een database verwijdert, blijven SQL-Database de back-ups op dezelfde manier zou voor een online-database. Stel bijvoorbeeld dat u een Basic-database met een bewaarperiode van zeven dagen verwijderd. Een back-up die is vier dagen oud is opgeslagen voor drie dagen.

> [!IMPORTANT]
> Als u de Azure SQL-server die als host fungeert voor SQL-Databases verwijdert, worden alle databases die deel uitmaken van de server worden ook verwijderd en kunnen niet worden hersteld. U kunt een verwijderde server niet herstellen.
> 

## <a name="how-to-extend-the-backup-retention-period"></a>Het uitbreiden van de back-up bewaarperiode?
Als uw toepassing vereist dat de back-ups beschikbaar voor langere tijd zijn kunt u de ingebouwde bewaarperiode uitbreiden door het configureren van de lange termijn back-up bewaarbeleid voor afzonderlijke databases (LTR beleid). Hiermee kunt u de ingebouwde it bewaarperiode van 35 dagen uitgebreid naar maximaal tien jaar. Zie [Langetermijnretentie](sql-database-long-term-retention.md) voor meer informatie.

Als u het beleid LTR aan een database met behulp van Azure portal of API toevoegt, wordt de wekelijkse volledige back-ups automatisch gekopieerd naar uw eigen Azure Backup-Service-kluis. Als uw database is versleuteld met TDE wordt de back-ups worden automatisch versleuteld in rust.  Uw verlopen back-ups op basis van hun tijdstempel en het beleid LTR worden automatisch verwijderd door de Services-kluis.  U hoeft dus niet te beheren van de back-upschema of zorgen over het opruimen van de oude bestanden. De restore-API ondersteunt back-ups opgeslagen in de kluis, zolang de kluis bevindt zich in hetzelfde abonnement als uw SQL-database. U kunt de Azure-portal of PowerShell gebruiken voor toegang tot deze back-ups.

> [!TIP]
> Zie voor een handleiding met procedures [en herstel van back-up op lange termijn bewaren van Azure SQL Database configureren](sql-database-long-term-backup-retention-configure.md)
>

## <a name="are-backups-encrypted"></a>Back-ups versleuteld?

Als TDE is ingeschakeld voor een Azure SQL database, worden back-ups eveneens versleuteld. Alle nieuwe Azure SQL-databases zijn geconfigureerd met TDE standaard ingeschakeld. Zie voor meer informatie over TDE [Transparent Data Encryption met Azure SQL Database](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql).

## <a name="next-steps"></a>Volgende stappen

- Databaseback-ups zijn een essentieel onderdeel van een zakelijke continuïteit en noodherstel herstelstrategie omdat ze uw gegevens tegen per ongeluk beschadigd of verwijderd beschermen. Zie voor meer informatie over de andere Azure SQL Database business continuity oplossingen [Business continuity overview](sql-database-business-continuity.md).
- Als u wilt herstellen naar een punt in tijd met de Azure portal, Zie [database herstellen naar een tijdstip met de Azure portal](sql-database-recovery-using-backups.md).
- Als u wilt herstellen naar een punt in tijd met behulp van PowerShell, Zie [-database herstellen naar een punt in tijd met behulp van PowerShell](scripts/sql-database-restore-database-powershell.md).
- Configureren, beheren en herstellen van lange bewaartermijn van automatische back-ups in een Azure Recovery Services-kluis met de Azure portal, Zie [lange Backup-bewaartermijn met de Azure portal beheren](sql-database-long-term-backup-retention-configure.md).
- Configureren, beheren en herstellen van lange bewaartermijn van automatische back-ups in een Azure Recovery Services-kluis met behulp van PowerShell, raadpleegt u [lange Backup-bewaartermijn met behulp van PowerShell beheren](sql-database-long-term-backup-retention-configure.md).
