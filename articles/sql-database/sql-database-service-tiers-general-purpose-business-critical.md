---
title: Azure SQL Database - voor algemeen gebruik en bedrijfskritiek | Microsoft Docs
description: Het artikel behandelt de zakelijke kritieke-lagen in het op vCore gebaseerde aankoopmodel en algemeen gebruik.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: sashan, moslake, carlrab
manager: craigg
ms.date: 02/23/2019
ms.openlocfilehash: e5426bb7c8eba9d58dbf0472360c6ce0b19c9bc4
ms.sourcegitcommit: 087ee51483b7180f9e897431e83f37b08ec890ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/31/2019
ms.locfileid: "66431341"
---
# <a name="azure-sql-database-service-tiers"></a>Azure SQL Database-Servicelagen

Azure SQL Database is gebaseerd op SQL Server database engine-architectuur die wordt aangepast voor de cloudomgeving om te controleren of de beschikbaarheid van 99,99 procent, zelfs als er een fout van de infrastructuur. Drie Servicelagen worden gebruikt in Azure SQL Database, elk met een andere architectuur. Deze service-lagen zijn:

- [Algemeen gebruik](sql-database-service-tier-general-purpose.md), die is ontworpen voor de meest algemene werkbelastingen.
- [Bedrijfskritiek](sql-database-service-tier-business-critical.md), die is ontworpen voor workloads met een leesbare replica's met lage latentie.
- [Zeer grootschalige](sql-database-service-tier-hyperscale.md), die is ontworpen voor zeer grote databases (tot 100 TB) met meerdere leesbare replica's.

Dit artikel worden de opslag en back-up overwegingen voor de zakelijke kritieke-lagen in het op vCore gebaseerde aankoopmodel en algemeen gebruik.

> [!NOTE]
> Zie voor meer informatie over de grootschalige service tier in het op vCore gebaseerde aankoopmodel [grootschalige servicelaag](sql-database-service-tier-hyperscale.md). Zie voor een vergelijking van de vCore gebaseerde aankoopmodel met het op DTU gebaseerde aankoopmodel [Azure SQL Database-modellen en -bronnen aanschaffen](sql-database-purchase-models.md).

## <a name="data-and-log-storage"></a>Opslag van gegevens en logboekbestanden

De volgende factoren van invloed zijn op de hoeveelheid opslag die wordt gebruikt voor gegevens en logboekbestanden:

- De toegewezen opslag wordt gebruikt door de gegevensbestanden (MDF) en logboekbestanden (LDF).
- Elke individuele database compute-grootte ondersteunt een maximale databasegrootte, met een maximale standaardgrootte van 32 GB.
- Wanneer u de grootte van de vereiste één database (de grootte van het MDF-bestand) configureert, wordt automatisch extra opslagruimte van 30 procent meer toegevoegd ter ondersteuning van LDF-bestanden.
- De maximale grootte voor een beheerd exemplaar van SQL-Database moet worden opgegeven in veelvouden van 32 GB.
- U kunt de grootte van een individuele database tussen 10 GB en de maximale ondersteunde selecteren.
  - Voor opslag in de standard-of algemene service-lagen, vergroten of verkleinen van de grootte in stappen van 10 GB.
  - Voor opslag in de premium of business kritieke service-lagen, vergroten of verkleinen in stappen van 250 GB.
- In de categorie Algemeen gebruik-service `tempdb` maakt gebruik van een gekoppelde SSD en deze kosten voor opslag is opgenomen in het vCore-prijs.
- In de bedrijfslaag kritieke- `tempdb` de gekoppelde SSD deelt met de MDF en LDF-bestanden, en de `tempdb` opslagkosten is opgenomen in het vCore-prijs.

> [!IMPORTANT]
> U betaalt voor de totale opslag toegewezen voor MDF en LDF-bestanden.

Voor het controleren van de huidige totale grootte van de MDF en LDF-bestanden gebruikt [sp_spaceused](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-spaceused-transact-sql). Voor het controleren van de huidige grootte van de afzonderlijke MDF en LDF-bestanden gebruikt [sys.database_files](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-database-files-transact-sql).

> [!IMPORTANT]
> In sommige gevallen is het wellicht voor het verkleinen van een database voor het vrijmaken van ongebruikte ruimte. Zie voor meer informatie, [bestandsruimte in Azure SQL Database beheren](sql-database-file-space-management.md).

## <a name="backups-and-storage"></a>Back-ups en opslag

Opslag voor back-ups ter ondersteuning van de punt-in-time-restore (PITR) is toegewezen en [(LTR) met een langetermijnbewaarperiode](sql-database-long-term-retention.md) mogelijkheden van SQL Database. Deze opslag is toegewezen afzonderlijk voor elke database en wordt gefactureerd als twee afzonderlijke database kosten in rekening gebracht.

- **PITR**: Afzonderlijke databaseback-ups worden gekopieerd naar [leestoegang geografisch redundante opslag met (RA-GRS)](../storage/common/storage-designing-ha-apps-with-ragrs.md) automatisch. De grootte van de neemt dynamisch toe naarmate nieuwe back-ups worden gemaakt. De opslag wordt gebruikt voor wekelijkse volledige back-ups, dagelijkse differentiële back-ups en transactielogboekback-ups, deze om de 5 minuten worden gekopieerd. Het opslagverbruik, is afhankelijk van de wijzigingssnelheid van de database en de bewaarperiode voor back-ups. U kunt een afzonderlijke bewaarperiode voor elke database tussen 7 en 35 dagen configureren. Een opslagruimte ter waarde van 100 procent (x 1) de grootte van de database wordt geleverd zonder extra kosten. Voor de meeste databases is dit bedrag voldoende voor het opslaan van zeven dagen aan back-ups.
- **LTR**: SQL Database biedt de mogelijkheid om langetermijnretentie voor volledige back-ups configureren voor maximaal 10 jaar. Als u een LTR-beleid hebt ingesteld, wordt deze back-ups worden opgeslagen in de RA-GRS-opslag automatisch, maar u kunt bepalen hoe vaak de back-ups worden gekopieerd. Om te voldoen aan verschillende nalevingsvereisten voldoet, kunt u verschillende bewaartermijnen voor wekelijkse, maandelijkse of jaarlijkse back-ups. De configuratie die u kiest, bepaalt hoeveel opslagruimte wordt gebruikt voor LTR-back-ups. Voor een schatting van de kosten voor LTR-opslag, kunt u de LTR-prijscalculator. Zie voor meer informatie, [SQL-Database met een langetermijnbewaarperiode](sql-database-long-term-retention.md).

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over de specifieke-grootten en opslag-grootten die beschikbaar zijn voor een individuele database in de zakelijke kritieke-lagen en algemeen gebruik Reken, [SQL Database vCore gebaseerde resourcelimieten voor individuele databases](sql-database-vcore-resource-limits-single-databases.md).
- Zie voor meer informatie over de specifieke-grootten en opslag-grootten die beschikbaar zijn voor elastische pools in de zakelijke kritieke-lagen en algemeen gebruik Reken, [SQL Database vCore gebaseerde resourcelimieten voor elastische pools](sql-database-vcore-resource-limits-elastic-pools.md).
