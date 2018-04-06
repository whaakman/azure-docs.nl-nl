---
title: Azure SQL Database back-ups opslaan voor maximaal tien jaar | Microsoft Docs
description: Meer informatie over hoe Azure SQL Database opslaan volledige databaseback-ups ondersteunt maximaal tien jaar.
services: sql-database
author: anosov1960
manager: craigg
ms.service: sql-database
ms.custom: business continuity
ms.topic: article
ms.date: 04/04/2018
ms.author: sashan
ms.reviewer: carlrab
ms.openlocfilehash: 51f00984a8f0d750bdb478ae4bc8093adad8108e
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
---
# <a name="store-azure-sql-database-backups-for-up-to-10-years"></a>Azure SQL Database back-ups voor maximaal tien jaar opslaan

Veel toepassingen hebben regelgeving, compatibiliteit of andere zakelijke doeleinden die vereisen dat u voor het bewaren van back-ups buiten de 7 35 dagen geleverd door de Azure SQL Database [automatische back-ups](sql-database-automated-backups.md). Met behulp van de lange termijn bewaren (LTR)-functie kunt u opgegeven SQL-database volledige back-ups in opslaan [RA-GRS](../storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage) blobopslag voor maximaal tien jaar. Vervolgens kunt u back-up herstellen als een nieuwe database.

> [!IMPORTANT]
> Lange bewaartermijn is momenteel in preview. Bestaande back-ups die zijn opgeslagen in de kluis Recovery-Service van Azure-Services als onderdeel van het vorige voorbeeld van deze functie worden gemigreerd naar een SQL Azure-opslag.<!-- and available in the following regions: Australia East, Australia Southeast, Brazil South, Central US, East Asia, East US, East US 2, India Central, India South, Japan East, Japan West, North Central US, North Europe, South Central US, Southeast Asia, West Europe, and West US.-->
>

## <a name="how-sql-database-long-term-retention-works"></a>De werking van de SQL-Database lange bewaartermijn

Lange bewaartermijn van de back-up maakt gebruik van de [automatische back-ups van SQL-Database](sql-database-automated-backups.md) vergt voor punt-tijd terugzetten (PITR) gemaakt. U kunt een bewaarbeleid voor de lange termijn voor elke SQL-database configureren en opgeven hoe vaak moet u de back-ups kopiëren naar de langetermijnopslag. U kunt het beleid een combinatie van vier parameters definiëren om in te schakelen die flexibiliteit: wekelijkse back-up bewaren (B) maandelijkse back-up bewaren (M), de jaarlijkse back-up bewaren (Y) en de week van jaar (WeekOfYear). Als u opgeeft W, één back-up elke week worden gekopieerd naar de langetermijnopslag. Als u M opgeeft, wordt één back-up tijdens de eerste week van elke maand worden gekopieerd naar de langetermijnopslag. Als u Y opgeeft, wordt een back-up van de week is opgegeven door WeekOfYear gekopieerd naar de langetermijnopslag. Elke back-up worden voor de periode die door deze parameters worden opgegeven in de opslag op lange termijn behouden. 

Voorbeelden:

-  W=0, M=0, Y=5, WeekOfYear=3

   De 3e volledige back-up van elk jaar worden voor 5 jaar behouden.

- W=0, M=3, Y=0

   De eerste volledige back-up van elke maand worden behouden voor 3 maanden.

- W=12, M=0, Y=0

   Elke wekelijkse volledige back-up worden voor 12 weken behouden.

- W=6, M=12, Y=10, WeekOfYear=16

   Elke wekelijkse volledige back-up worden voor 6 weken behouden. Behalve de eerste volledige back-up van elke maand, die worden bewaard gedurende 12 maanden. Met uitzondering van de volledige back-up op 16de week van jaar, die gehouden tien jaar. 

De volgende tabel ziet u de uitgebracht en verlooptijd van de lange termijn back-ups voor het volgende beleid:

W = 12 weken (84 dagen), M = 12 maanden (365 dagen), Y = 10 jaar (3650 dagen), WeekOfYear = 15 (week na 15 April)

   ![Voorbeeld van links naar rechts](./media/sql-database-long-term-retention/ltr-example.png)


 
Als u zou de bovenstaande beleid wijzigen en set W = 0 (geen wekelijkse back-ups), de frequentie van back-ups als veranderen zou weergegeven in de bovenstaande tabel op de gemarkeerde Data. De opslagruimte die nodig zijn voor deze back-ups behouden zou dienovereenkomstig verminderen. Opmerking: De LTR exemplaren worden gemaakt door Azure storage-service, zodat het kopieerproces geen invloed op de prestaties op de bestaande database heeft.
Als u een database herstellen uit de opslag LTR, kunt u een specifieke back-up op basis van de timestamp.   De database kan worden hersteld met een bestaande server onder hetzelfde abonnement als de oorspronkelijke database. 

## <a name="configure-long-term-backup-retention"></a>Langetermijnretentie van back-ups configureren

Zie voor meer informatie over het configureren van lange bewaartermijn met behulp van de Azure-portal of met behulp van PowerShell, [lange bewaartermijn van de back-up configureren](sql-database-long-term-backup-retention-configure.md).

## <a name="next-steps"></a>Volgende stappen

Omdat de databaseback-ups voorkomen dat gegevens per ongeluk beschadigd of verwijderd, zijn ze een essentieel onderdeel van een zakelijke continuïteit en noodherstelplan. Zie voor meer informatie over de andere SQL-Database-oplossingen voor bedrijfscontinuïteit, [Business continuity overview](sql-database-business-continuity.md).
