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
ms.date: 10/24/2018
ms.openlocfilehash: b1ef03b97f9fe95286d427effc40e69ae07b6b3c
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53601488"
---
# <a name="store-azure-sql-database-backups-for-up-to-10-years"></a>Azure SQL Database-back-ups voor maximaal 10 jaar Store

Veel toepassingen hebben regelgeving, naleving, of andere zakelijke doeleinden waarvoor u databaseback-ups langer dan de 7-35 dagen geleverd door Azure SQL Database behouden [automatische back-ups](sql-database-automated-backups.md). Met behulp van de functie voor lange bewaarperiode (LTR), kunt u opslaan opgegeven SQL-database volledige back-ups in [RA-GRS](../storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage) blob-opslag voor maximaal 10 jaar. Vervolgens kunt u een back-up herstellen als een nieuwe database.

> [!NOTE]
> LTR kan worden ingeschakeld op de databases die worden gehost in Azure SQL Database-logische Servers. Het is nog niet beschikbaar voor databases die worden gehost in beheerde instanties. U kunt SQL Agent-taken gebruiken om te plannen [kopie-alleen back-ups](https://docs.microsoft.com/sql/relational-databases/backup-restore/copy-only-backups-sql-server) als alternatief voor LTR langer dan 35 dagen.
> 

## <a name="how-sql-database-long-term-retention-works"></a>De werking van de SQL-Database met een langetermijnbewaarperiode

Langetermijnretentie (LTR) maakt gebruik van de volledige databaseback-ups die zijn [automatisch gemaakte](sql-database-automated-backups.md) moment terugzetten (PITR) inschakelen. Deze back-ups worden gekopieerd naar de andere opslag-blobs als LTR-beleid is geconfigureerd.
U kunt een LTR-beleid voor elke SQL-database configureren en opgeven hoe vaak u wilt kopiëren van de back-ups naar de lange termijn opslagblobs. Om in te schakelen dat flexibiliteit kunt u het beleid met behulp van een combinatie van vier parameters definiëren: wekelijkse back-upretentie (S), maandelijkse back-upretentie (M), de jaarlijkse back-upretentie (Y) en de week van jaar (WeekOfYear). Als u opgeeft W, één back-up elke week worden gekopieerd naar de opslag op lange termijn. Als u M opgeeft, wordt één back-up tijdens de eerste week van elke maand worden gekopieerd naar de opslag op lange termijn. Als u Y opgeeft, wordt een back-up van de week is opgegeven door WeekOfYear worden gekopieerd naar de opslag op lange termijn. Elke back-up worden voor de periode die door deze parameters worden opgegeven in de opslag op lange termijn behouden. 

Voorbeelden:

-  W = 0, M = 0, Y = 5, WeekOfYear = 3

   De 3e volledige back-up van elk jaar worden voor 5 jaar behouden.
- W=0, M=3, Y=0

   De eerste volledige back-up van elke maand worden, bewaard gedurende 3 maanden.

- W=12, M=0, Y=0

   Elke wekelijkse volledige back-up worden voor 12 weken behouden.

- W = 6, M = 12, Y = 10 WeekOfYear = 16

   Elke wekelijkse volledige back-up worden voor 6 weken behouden. Behalve de eerste volledige back-up van elke maand, wordt die worden bewaard gedurende 12 maanden. Met uitzondering van de volledige back-up die wordt gemaakt op 16 week van jaar, wordt die worden bewaard gedurende tien jaar. 

De volgende tabel ziet u de uitgebracht en de vervaldatum van de lange termijn back-ups voor het volgende beleid:

W = 12 weken (84 dagen), M = 12 maanden (365 dagen), Y = 10 jaar (3650 dagen), WeekOfYear = 15 (week na 15 April)

   ![ltr-voorbeeld](./media/sql-database-long-term-retention/ltr-example.png)


 
Als u zou het bovenstaande beleid wijzigen en set W = 0 (geen wekelijkse back-ups), de frequentie van back-ups gewijzigd zou worden als die wordt weergegeven in de bovenstaande tabel op de gemarkeerde datums. De hoeveelheid opslagruimte die nodig zijn voor deze back-ups behouden zou dienovereenkomstig verminderen. 

> [!NOTE]
1. De LTR-exemplaren worden gemaakt door Azure storage-service, zodat het kopieerproces geen invloed op de prestaties op de bestaande database heeft.
2. Het beleid is van toepassing op de toekomstige back-ups. Bijvoorbeeld Als de opgegeven WeekOfYear in het verleden wanneer het beleid is geconfigureerd is, wordt de eerste LTR back-up gemaakt van volgend jaar. 
3. Database herstellen vanuit de LTR-opslag, kunt u een specifieke back-up op basis van de tijdstempel.   De database kan worden hersteld op een bestaande server onder hetzelfde abonnement als de oorspronkelijke database. 
> 

## <a name="geo-replication-and-long-term-backup-retention"></a>Geo-replicatie en langetermijnretentie

Als u actieve geo-replicatie of failover-groepen als uw zakelijke continuïteit-oplossing u moet voorbereiden op het uiteindelijke failovers en de dezelfde LTR-beleid configureren op de geo-secundaire database gebruiken. Dit naarmate niet uw opslagkosten LTR-back-ups zijn niet gegenereerd op basis van de secundaire replica's. Alleen wanneer de secundaire primaire wordt wordt de back-ups worden gemaakt. Op deze manier garandeert u generatie van de LTR back-ups niet onderbroken wanneer de failover wordt geactiveerd en de primaire worden verplaatst naar de secundaire regio. 

> [!NOTE]
Wanneer de oorspronkelijke primaire database wordt hersteld na de storing waardoor er failover plaatsvindt, wordt er een nieuwe secundaire. Daarom het back-up maken wordt niet voortgezet en het bestaande LTR-beleid wordt pas van kracht totdat deze opnieuw de primaire verandert. 
> 

## <a name="configure-long-term-backup-retention"></a>Langetermijnretentie van back-ups configureren

Zie voor informatie over het configureren van met een langetermijnbewaarperiode met behulp van de Azure portal of met behulp van PowerShell, [langetermijnretentie van back-up configureren](sql-database-long-term-backup-retention-configure.md).

## <a name="next-steps"></a>Volgende stappen

Omdat gegevens databaseback-ups tegen per ongeluk beschadigd of verwijderd beveiligen, zijn in feite een essentieel onderdeel van een zakelijke continuïteit en noodherstelplan. Zie voor meer informatie over de andere SQL-Database-oplossingen voor bedrijfscontinuïteit, [overzicht voor zakelijke continuïteit](sql-database-business-continuity.md).
