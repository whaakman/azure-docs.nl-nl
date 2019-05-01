---
title: Azure SQL Database-back-ups voor maximaal 10 jaar Store | Microsoft Docs
description: Informatie over hoe opslag volledige databaseback-ups in Azure SQL Database biedt ondersteuning voor maximaal 10 jaar.
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
ms.date: 04/23/2019
ms.openlocfilehash: 0f764ebbad53185f46c7166011e05493ed261d6a
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2019
ms.locfileid: "64696646"
---
# <a name="store-azure-sql-database-backups-for-up-to-10-years"></a>Azure SQL Database-back-ups voor maximaal 10 jaar Store

Veel toepassingen hebben regelgeving, naleving, of andere zakelijke doeleinden waarvoor u databaseback-ups langer dan de 7-35 dagen geleverd door Azure SQL Database behouden [automatische back-ups](sql-database-automated-backups.md). Met behulp van de functie voor lange bewaarperiode (LTR), kunt u opslaan opgegeven SQL-database volledige back-ups in [RA-GRS](../storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage) blob-opslag voor maximaal 10 jaar. U kunt elke back-up vervolgens herstellen als een nieuwe database.

> [!NOTE]
> LTR kan worden ingeschakeld voor één en gepoolde databases. Het is nog niet beschikbaar voor databases in beheerde instanties. U kunt SQL Agent-taken gebruiken om te plannen [kopie-alleen back-ups](https://docs.microsoft.com/sql/relational-databases/backup-restore/copy-only-backups-sql-server) als alternatief voor LTR langer dan 35 dagen.
> 

## <a name="how-sql-database-long-term-retention-works"></a>De werking van de SQL-Database met een langetermijnbewaarperiode

Langetermijnretentie (LTR) maakt gebruik van de volledige databaseback-ups die zijn [automatisch gemaakte](sql-database-automated-backups.md) moment terugzetten (PITR) inschakelen. Als een LTR-beleid is geconfigureerd, worden deze back-ups worden gekopieerd naar verschillende blobs voor langdurige opslag. De kopieerbewerking is een achtergrondtaak die geen invloed op de prestaties op de werkbelasting van de database heeft. De LTR back-ups worden bewaard gedurende een bepaalde periode ingesteld door het LTR-beleid. Het LTR-beleid voor elke SQL-database kunt ook opgeven hoe vaak de LTR back-ups worden gemaakt. Om in te schakelen dat flexibiliteit kunt u het beleid met behulp van een combinatie van vier parameters definiëren: wekelijkse back-upretentie (S), maandelijkse back-upretentie (M), de jaarlijkse back-upretentie (Y) en de week van jaar (WeekOfYear). Als u opgeeft W, één back-up elke week worden gekopieerd naar de opslag op lange termijn. Als u M opgeeft, wordt één back-up tijdens de eerste week van elke maand worden gekopieerd naar de opslag op lange termijn. Als u Y opgeeft, wordt een back-up van de week is opgegeven door WeekOfYear worden gekopieerd naar de opslag op lange termijn. Elke back-up worden voor de periode die door deze parameters worden opgegeven in de opslag op lange termijn behouden. Elke wijziging van de LTR-beleid is van toepassing op de toekomstige back-ups. Bijvoorbeeld als de opgegeven WeekOfYear in het verleden wanneer het beleid is geconfigureerd is, wordt de eerste LTR back-up gemaakt volgend jaar. 

Voorbeelden van de LTR-beleid:

-  W=0, M=0, Y=5, WeekOfYear=3

   De derde volledige back-up van elk jaar wordt vijf jaar worden bewaard.
   
- W=0, M=3, Y=0

   De eerste volledige back-up van elke maand worden, bewaard gedurende drie maanden.

- W=12, M=0, Y=0

   Elke wekelijkse volledige back-up worden voor 12 weken behouden.

- W=6, M=12, Y=10, WeekOfYear=16

   Elke wekelijkse volledige back-up worden voor zes weken behouden. Behalve de eerste volledige back-up van elke maand, wordt die worden bewaard gedurende 12 maanden. Met uitzondering van de volledige back-up die wordt gemaakt op 16 week van jaar, wordt die worden bewaard gedurende tien jaar. 

De volgende tabel ziet u de uitgebracht en de vervaldatum van de lange termijn back-ups voor het volgende beleid:

W = 12 weken (84 dagen), M = 12 maanden (365 dagen), Y = 10 jaar (3650 dagen), WeekOfYear = 15 (week na 15 April)

   ![ltr-voorbeeld](./media/sql-database-long-term-retention/ltr-example.png)



Als u het bovenstaande beleid wijzigen en set W = 0 (geen wekelijkse back-ups), de frequentie van back-ups worden gewijzigd, zoals weergegeven in de bovenstaande tabel op de gemarkeerde datums. De hoeveelheid opslagruimte die nodig zijn voor deze back-ups behouden zou dienovereenkomstig verminderen. 

> [!NOTE]
> De timing van de afzonderlijke LTR back-ups wordt bepaald door de Azure SQL Database. U kan handmatig maken van een back-up van links naar rechts of de timing van het maken van back-up wilt bepalen.
> 

## <a name="geo-replication-and-long-term-backup-retention"></a>Geo-replicatie en langetermijnretentie

Als u actieve geo-replicatie of failover-groepen als zakelijke continuïteit-oplossing gebruikt, moet u voorbereiden op het uiteindelijke failovers en de dezelfde LTR-beleid configureren op de geo-secundaire database. Uw opslagkosten LTR naarmate niet er back-ups zijn niet gegenereerd op basis van de secundaire replica's. Alleen wanneer de secundaire primaire wordt wordt de back-ups worden gemaakt. Daardoor wordt niet onderbroken generatie van de LTR back-ups wanneer de failover wordt geactiveerd en de primaire naar de secundaire regio verplaatst. 

> [!NOTE]
> Als de oorspronkelijke primaire database wordt hersteld na een storing waardoor de failover, wordt er een nieuwe secundaire. Daarom het back-up maken wordt niet voortgezet en het bestaande LTR-beleid wordt pas van kracht totdat deze opnieuw de primaire verandert. 

## <a name="configure-long-term-backup-retention"></a>Langetermijnretentie van back-ups configureren

Zie voor informatie over het configureren van de Azure portal of PowerShell met een langetermijnbewaarperiode, [langetermijnretentie voor Azure SQL Database beheren](sql-database-long-term-backup-retention-configure.md).

## <a name="restore-database-from-ltr-backup"></a>Database herstellen vanuit back-up van links naar rechts

Database herstellen vanuit de LTR-opslag, kunt u een specifieke back-up op basis van de tijdstempel. De database kan worden hersteld op een bestaande server onder hetzelfde abonnement als de oorspronkelijke database. Zie voor meer informatie over de database te herstellen vanaf een back-LTR up met behulp van de Azure portal of PowerShell, [langetermijnretentie voor Azure SQL Database beheren](sql-database-long-term-backup-retention-configure.md).

## <a name="next-steps"></a>Volgende stappen

Omdat gegevens databaseback-ups tegen per ongeluk beschadigd of verwijderd beveiligen, zijn in feite een essentieel onderdeel van een zakelijke continuïteit en noodherstelplan. Zie voor meer informatie over de andere SQL-Database-oplossingen voor bedrijfscontinuïteit, [overzicht voor zakelijke continuïteit](sql-database-business-continuity.md).
