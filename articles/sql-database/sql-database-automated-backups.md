---
title: Azure SQL Database automatische, geografisch redundante back-ups | Microsoft Docs
description: SQL-Database maakt een back-up van de lokale database om de paar minuten en maakt gebruik van Azure geografisch redundante opslag met leestoegang voor geografische redundantie automatisch.
services: sql-database
author: anosov1960
manager: craigg
ms.service: sql-database
ms.custom: business continuity
ms.topic: article
ms.workload: Active
ms.date: 04/04/2018
ms.author: sashan
ms.reviewer: carlrab
ms.openlocfilehash: 37bbbf8ea5a5d8439b300d0740e4f1a048e98e91
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="learn-about-automatic-sql-database-backups"></a>Meer informatie over automatische back-ups van SQL-Database

SQL-Database maakt databaseback-ups en geografisch redundante opslag met Azure-leestoegang (RA-GRS) gebruikt om te voorzien van geografische redundantie automatisch. Deze back-ups worden gemaakt, automatisch en zonder extra kosten. U hoeft niet te ondernemen om ze gebeuren te maken. Databaseback-ups zijn een essentieel onderdeel van een zakelijke continuïteit en noodherstel herstelstrategie omdat ze uw gegevens tegen per ongeluk beschadigd of verwijderd beschermen. Als u wilt back-ups behouden in uw eigen storage-container kunt u een lange termijn back-up bewaarbeleid configureren. Zie [Langetermijnretentie](sql-database-long-term-retention.md) voor meer informatie.

## <a name="what-is-a-sql-database-backup"></a>Wat is er een back-up van de SQL-Database?

SQL-Database maakt met SQL Server-technologie [volledige](https://msdn.microsoft.com/library/ms186289.aspx), [differentiële](https://msdn.microsoft.com/library/ms175526.aspx), en [transactielogboek](https://msdn.microsoft.com/library/ms191429.aspx) back-ups voor de doeleinden van punt in tijd herstellen (PITR). Elke 5-10 minuten de transactielogboekback-ups in het algemeen gebeuren met de frequentie op basis van het prestatieniveau en de hoeveelheid activiteit in een database. Transactielogboekback-ups, volledige en differentiële back-ups maakt, kunnen u een database herstellen naar een specifieke point-in-time naar dezelfde server die als host fungeert voor de database. Wanneer u een database herstelt, de service weten te achterhalen welke volledige en differentiële transactie logboek back-ups moeten worden teruggezet.


U kunt deze back-ups te gebruiken:

* Een database herstellen naar een punt in tijd binnen de bewaarperiode. Deze bewerking wordt een nieuwe database maken op dezelfde server als de oorspronkelijke database.
* Een verwijderde database herstellen naar de tijd die is verwijderd of binnen de bewaarperiode. De verwijderde database kan alleen worden teruggezet op dezelfde server waarop de oorspronkelijke database is gemaakt.
* Een database herstellen naar een andere geografische regio. Hiermee kunt u herstellen van een geografische noodgeval wanneer u geen toegang uw server en database tot. Deze maakt een nieuwe database in een bestaande server overal ter wereld. 
* Een database van een specifieke langetermijnback-up herstellen als de database is geconfigureerd met een lange termijn bewaarbeleid. Hiermee kunt u een oude versie van de database om te voldoen aan een aanvraag voor naleving of uit te voeren een oude versie van de toepassing te herstellen. Zie [lange bewaartermijn](sql-database-long-term-retention.md).
* Als u wilt een herstel uitvoeren, Zie [database terugzetten vanaf back-ups](sql-database-recovery-using-backups.md).

> [!NOTE]
> In Azure-opslag, de term *replicatie* verwijst naar het kopiëren van bestanden van de ene locatie naar een andere. De SQL *databasereplicatie* verwijst naar en meerdere secundaire databases worden gesynchroniseerd met een primaire database behouden blijft. 
> 

## <a name="how-often-do-backups-happen"></a>Hoe vaak gebeuren back-ups?
Volledige databaseback-ups gebeuren wekelijks, differentiële back-ups in het algemeen gebeuren elk paar uur en transactielogboek back-ups in het algemeen gebeuren elke 5-10 minuten. De eerste volledige back-up is gepland onmiddellijk nadat een database wordt gemaakt. Meestal is voltooid binnen 30 minuten, maar het kan langer duren wanneer de database van een aanzienlijke grootte. De eerste back-up kan onder andere op een herstelde database of een databasekopie langer duren. Na de eerste volledige back-up, worden alle verdere back-ups automatisch gepland en achtergrond beheerd op de achtergrond. De precieze timing van alle databaseback-ups wordt bepaald door de service SQL Database als het een compromis tussen de werkbelasting van het algehele systeem. 

De back-upopslag geo-replicatie vindt plaats op basis van de planning van Azure Storage-replicatie.

## <a name="how-long-do-you-keep-my-backups"></a>Hoe lang houdt u de back-ups?
De back-up van elke SQL-Database heeft een bewaarperiode die is gebaseerd op de servicelaag van de database en verschilt voor de [aankoopmodel DTU gebaseerde](sql-database-service-tiers-dtu.md) en de [vCore gebaseerde aankoopmodel (preview)](sql-database-service-tiers-vcore.md). 


### <a name="database-retention-for-dtu-based-purchasing-model"></a>Database bewaren voor op basis van DTU aankoopmodel
De bewaarperiode voor een database in de DTU-aankoopmodel is afhankelijk van de servicetier. De bewaarperiode voor een database voor de:

* Basic-servicelaag is 7 dagen.
* Standaard-servicelaag is 35 dagen.
* Premium servicecategorie is 35 dagen.
* Voor algemene doeleinden laag kan worden geconfigureerd met een maximum van 35 dagen (7 dagen standaard) *
* Kritieke bedrijfslaag (preview) kan worden geconfigureerd met een maximum van 35 dagen (7 dagen standaard) *

\* Tijdens de preview, worden de back-ups bewaarperiode kan niet worden geconfigureerd en vast is ingesteld op 7 dagen.

Als u een database met langer bewaren van back-ups naar een database met een kortere bewaren converteert, zijn alle bestaande back-ups ouder is dan de bewaarperiode voor doel laag niet meer beschikbaar.

Als u een database met een kortere bewaartermijn naar een database met een langere periode bijwerkt, houdt u SQL-Database bestaande back-ups totdat de bewaartermijn langer is bereikt. 

Als u een database verwijdert, blijven SQL-Database de back-ups op dezelfde manier zou voor een online-database. Stel bijvoorbeeld dat u een Basic-database met een bewaarperiode van zeven dagen verwijderd. Een back-up die is vier dagen oud is opgeslagen voor drie dagen.

> [!IMPORTANT]
> Als u de Azure SQL-server die als host fungeert voor SQL-Databases verwijdert, worden alle databases die deel uitmaken van de server worden ook verwijderd en kunnen niet worden hersteld. U kunt een verwijderde server niet herstellen.

### <a name="database-retention-for-the-vcore-based-purchasing-model-preview"></a>Bewaartijd voor de vCore gebaseerde aankoopmodel (preview)

Opslag voor databaseback-ups is toegewezen voor ondersteuning van het punt in tijd herstellen (PITR) en lange termijn bewaren (LTR) mogelijkheden van SQL-Database. Deze opslag is toegewezen afzonderlijk voor elke database en als twee aparte database kosten in rekening gebracht. 

- **PITR**: worden automatisch één database back-ups naar RA-GRS opslag worden gekopieerd. Maximale grootte van het duurt dynamisch omdat de nieuwe back-ups worden gemaakt.  De opslag wordt gebruikt voor wekelijkse volledige back-ups, dagelijkse differentiële back-ups en back-ups van transactielogboeken die om de vijf minuten worden gekopieerd. Het opslagverbruik is afhankelijk van de snelheid van de wijziging van de database en de bewaarperiode. U kunt een afzonderlijke bewaarperiode voor elke database tussen 7 en 35 dagen configureren. Een minimale opslaghoeveelheid gelijk zijn aan 1 x van gegevensgrootte wordt geleverd zonder extra kosten. Dit bedrag is voor de meeste databases voldoende is voor het opslaan van 7 dagen van back-ups. Zie voor meer informatie [punt in tijd terugzetten](sql-database-recovery-using-backups.md#point-in-time-restore)
- **LTR**: SQL-Database biedt de mogelijkheid om lange bewaartermijn van volledige back-ups configureren voor maximaal tien jaar. Als LTR beleid is ingeschakeld, wordt bij deze back-ups worden opgeslagen in de opslag van de RA-GRS automatisch, maar u kunt bepalen hoe vaak de back-ups worden gekopieerd. Om te voldoen aan verschillende nalevingsvereiste, kunt u verschillende bewaartermijnen voor wekelijkse, maandelijkse en/of jaarlijkse back-ups. Deze configuratie definieert hoeveel opslagruimte wordt gebruikt voor de back-ups van links naar rechts. De prijscategorie Rekenmachine LTR kunt u schat de kosten voor opslag van links naar rechts. Zie [Langetermijnretentie](sql-database-long-term-retention.md) voor meer informatie.

## <a name="how-to-extend-the-backup-retention-period"></a>Het uitbreiden van de back-up bewaarperiode?

Als uw toepassing vereist dat de back-ups gedurende langere tijd dan de maximale bewaarperiode voor back-up voor PITR beschikbaar zijn, kunt u een lange termijn back-up bewaarbeleid voor afzonderlijke databases (LTR beleid) configureren. Hiermee kunt u de ingebouwde it bewaartermijn van maximaal 35 dagen uitbreiden naar maximaal tien jaar. Zie [Langetermijnretentie](sql-database-long-term-retention.md) voor meer informatie.

Als u het beleid LTR aan een database met behulp van Azure portal of API toevoegt, wordt de wekelijkse volledige back-ups automatisch gekopieerd naar een afzonderlijke RA-GRS storage-container voor lange bewaartermijn (LTR opslag). Als uw database is versleuteld met TDE wordt de back-ups worden automatisch versleuteld in rust. SQL-Database worden de verlopen back-ups op basis van hun tijdstempel en het beleid LTR automatisch verwijderd. Nadat het beleid is ingesteld, hoeft u niet te beheren van de back-upschema of zorgen over het opruimen van de oude bestanden. U kunt de Azure-portal of PowerShell gebruiken om te bekijken, herstellen of verwijderen van deze back-ups.

## <a name="are-backups-encrypted"></a>Back-ups versleuteld?

Als TDE is ingeschakeld voor een Azure SQL database, worden back-ups eveneens versleuteld. Alle nieuwe Azure SQL-databases zijn geconfigureerd met TDE standaard ingeschakeld. Zie voor meer informatie over TDE [Transparent Data Encryption met Azure SQL Database](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql).

## <a name="are-the-automatic-backups-compliant-with-gdpr"></a>De automatische back-ups compatibel zijn met GDPR?
Als de back-up bevat persoonlijke gegevens, die is onderworpen aan algemene gegevens beveiliging regelgeving (GDPR), moet u Verbeterde beveiligingsmaatregelen de om gegevens te beveiligen tegen onbevoegde toegang toe te passen. Om te voldoen aan de GDPR, moet u een manier voor het beheren van de gegevensaanvragen eigenaars van gegevens zonder toegang tot de back-ups.  Voor back-ups op korte termijn, een oplossing kan zijn om te verkorten van de back-up venster onder 30 dagen, is de tijd mag de data access-aanvragen te voltooien.  Als langere termijn back-ups vereist zijn, wordt het aanbevolen alleen 'pseudonymized' gegevens in de back-ups opslaan. Bijvoorbeeld als gegevens over een persoon moet worden verwijderd of bijgewerkt, worden niet moet verwijderen of bijwerken van de bestaande back-ups. U vindt meer informatie over de aanbevolen procedures GDPR in [Data Governance voor naleving van GDPR](https://info.microsoft.com/DataGovernanceforGDPRCompliancePrinciplesProcessesandPractices-Registration.html).

## <a name="next-steps"></a>Volgende stappen

- Databaseback-ups zijn een essentieel onderdeel van een zakelijke continuïteit en noodherstel herstelstrategie omdat ze uw gegevens tegen per ongeluk beschadigd of verwijderd beschermen. Zie voor meer informatie over de andere Azure SQL Database business continuity oplossingen [Business continuity overview](sql-database-business-continuity.md).
- Als u wilt herstellen naar een punt in tijd met de Azure portal, Zie [database herstellen naar een tijdstip met de Azure portal](sql-database-recovery-using-backups.md).
- Als u wilt herstellen naar een punt in tijd met behulp van PowerShell, Zie [-database herstellen naar een punt in tijd met behulp van PowerShell](scripts/sql-database-restore-database-powershell.md).
- Configureren, beheren en herstellen van lange bewaartermijn van automatische back-ups in een Azure Recovery Services-kluis met de Azure portal, Zie [lange Backup-bewaartermijn met de Azure portal beheren](sql-database-long-term-backup-retention-configure.md).
- Configureren, beheren en herstellen van lange bewaartermijn van automatische back-ups in een Azure Recovery Services-kluis met behulp van PowerShell, raadpleegt u [lange Backup-bewaartermijn met behulp van PowerShell beheren](sql-database-long-term-backup-retention-configure.md).
