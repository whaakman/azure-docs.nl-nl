---
title: Azure SQL Database - voor algemeen gebruik en bedrijfskritiek | Microsoft Docs
description: Het artikel wordt beschreven voor de algemeen gebruik en kritieke-bedrijfslaag in vCore model kopen.
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: sashan, moslake
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: 61a413fc0b27db66091c8e4a401de654455ebe7c
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55470991"
---
# <a name="azure-sql-database-service-tiers"></a>Azure SQL Database-Servicelagen

Azure SQL Database is gebaseerd op SQL Server Database Engine-architectuur die wordt aangepast aan de cloudomgeving om ervoor te zorgen, zelfs in het geval van infrastructuuruitval voor 99,99% beschikbaarheid. Er zijn drie architectuur modellen die worden gebruikt in Azure SQL Database:
- [Algemeen gebruik](sql-database-service-tier-general-purpose.md) ontworpen voor het merendeel van de algemene werkbelastingen.
- [Bedrijfskritiek](sql-database-service-tier-business-critical.md) ontworpen voor workloads met lage latentie met een leesbare replica.
- [Zeer grootschalige](sql-database-service-tier-hyperscale.md) is ontworpen voor zeer grote databases (tot 100 TB) met meerdere leesbare replica's.

Dit artikel worden de overwegingen voor opslag en back-up voor de Servicelagen voor algemeen gebruik en bedrijfskritiek in het op vCore gebaseerde aankoopmodel.

> [!NOTE]
> Zie voor meer informatie over de servicelaag grootschalige in het op vCore gebaseerde aankoopmodel [grootschalige servicelaag](sql-database-service-tier-hyperscale.md). Zie voor een vergelijking van de vCore gebaseerde aankoopmodel met het op DTU gebaseerde aankoopmodel [Azure SQL Database-modellen en -bronnen aanschaffen](sql-database-service-tiers.md).

## <a name="data-and-log-storage"></a>Opslag van gegevens en logboekbestanden

Overweeg de volgende:

- De toegewezen opslag wordt gebruikt door de gegevensbestanden (MDF) en logboekbestanden bestanden (LDF).
- Elke individuele database compute-grootte ondersteunt een maximale databasegrootte, met een maximale standaardgrootte van 32 GB.
- Wanneer u de grootte van de vereiste één database (grootte van het MDF) configureert, wordt 30% van de extra opslag automatisch toegevoegd ter ondersteuning van LDF
- Maximale grootte in het beheerde exemplaar moet worden opgegeven in veelvouden van 32 GB.
- U kunt de grootte van een individuele database tussen 10 GB en de maximale ondersteunde selecteren
  - Voor Standard-opslag, vergroten of verkleinen in stappen van 10 GB
  - Voor Premium-opslag, vergroten of verkleinen in stappen van 250 GB
- In de categorie Algemeen gebruik-service `tempdb` maakt gebruik van een gekoppelde SSD en deze kosten voor opslag is opgenomen in het vCore-prijs.
- In de laag bedrijfskritiek service `tempdb` shares de gekoppelde SSD met de MDF en LDF-bestanden en opslagkosten van de tempDB is opgenomen in het vCore-prijs.

> [!IMPORTANT]
> De totale opslag toegewezen voor MDF en LDF in rekening worden gebracht.

Voor het controleren van de huidige totale grootte van het MDF en LDF gebruikt [sp_spaceused](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-spaceused-transact-sql). Voor het controleren van de huidige grootte van de afzonderlijke MDF en LDF-bestanden gebruikt [sys.database_files](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-database-files-transact-sql).

> [!IMPORTANT]
> In sommige gevallen is het wellicht voor het verkleinen van een database voor het vrijmaken van ongebruikte ruimte. Zie voor meer informatie, [bestandsruimte in Azure SQL Database beheren](sql-database-file-space-management.md).

## <a name="backups-and-storage"></a>Back-ups en opslag

Opslag voor back-ups ter ondersteuning van het punt in tijd herstellen (PITR) is toegewezen en [lange termijn Retention (LTR)](sql-database-long-term-retention.md) mogelijkheden van SQL Database. Deze opslag is toegewezen afzonderlijk voor elke database en wordt gefactureerd als twee afzonderlijke database kosten in rekening gebracht.

- **PITR**: Afzonderlijke databaseback-ups worden gekopieerd naar [RA-GRS-opslag](../storage/common/storage-designing-ha-apps-with-ragrs.md) worden automatisch. De grootte van de dynamisch wordt verhoogd wanneer de nieuwe back-ups worden gemaakt.  De opslag wordt gebruikt voor wekelijkse volledige back-ups, dagelijkse differentiële back-ups en back-ups van transactielogboeken die om de vijf minuten worden gekopieerd. Het opslagverbruik, is afhankelijk van de wijzigingssnelheid van de database en de bewaarperiode. U kunt een afzonderlijke bewaarperiode voor elke database tussen 7 en 35 dagen configureren. Een opslagruimte ter gelijk zijn aan 1 x de grootte is opgegeven zonder extra kosten. Voor de meeste databases is dit bedrag voldoende voor het opslaan van zeven dagen aan back-ups.
- **LTR**: SQL Database biedt de mogelijkheid langetermijnretentie van volledige back-ups configureren voor maximaal 10 jaar. Als LTR-beleid is ingeschakeld, wordt deze back-ups worden opgeslagen in de RA-GRS-opslag automatisch, maar u kunt bepalen hoe vaak de back-ups worden gekopieerd. Om te voldoen aan verschillende nalevingsvereiste, kunt u verschillende bewaartermijnen voor wekelijkse, maandelijkse en/of jaarlijkse back-ups. Deze configuratie wordt gedefinieerd hoeveel opslagruimte wordt gebruikt voor het LTR-back-ups. Om in te schatten van de kosten voor LTR-opslag kunt u de LTR-prijscalculator. Zie [Langetermijnretentie](sql-database-long-term-retention.md) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over specifieke-grootten en opties voor opslag beschikbaar voor individuele database in de lagen algemeen gebruik en kritieke bedrijfsservice Reken, [SQL Database vCore gebaseerde resourcelimieten voor individuele databases](sql-database-vcore-resource-limits-single-databases.md#general-purpose-service-tier-storage-sizes-and-compute-sizes)
- Zie voor meer informatie over specifieke-grootten en opties voor opslag beschikbaar voor elastische pools in de lagen algemeen gebruik en kritieke bedrijfsservice Reken, [SQL Database vCore gebaseerde resourcelimieten voor elastische pools](sql-database-vcore-resource-limits-elastic-pools.md#general-purpose-service-tier-storage-sizes-and-compute-sizes).
