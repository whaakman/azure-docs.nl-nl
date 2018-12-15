---
title: Het configureren van Azure SQL Database - Singleton | Microsoft Docs
description: Informatie over het configureren en beheren van Azure SQL Database - één Database.
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: howto
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: carlr
manager: craigg
ms.date: 12/14/2018
ms.openlocfilehash: d34853220e423e73c6ca8cf7c76ba616b815b8bd
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2018
ms.locfileid: "53440651"
---
# <a name="how-to-use-single-database"></a>Het gebruik van één Database

In deze sectie vindt u diverse handleidingen, scripts en uitleg waarmee u kunt beheren en configureren uw Azure SQL Database - één Database.

## <a name="migrate"></a>Migreren

- [Migreren naar SQL Database](sql-database-cloud-migrate.md) : meer informatie over de aanbevolen migratieproces en hulpprogramma's voor migratie naar Managed Instance.
- Meer informatie over het [SQL-database na migratie beheren](sql-database-manage-after-migration.md).

## <a name="configure-features"></a>Functies configureren

- [Transactionele replicatie configureren](replication-to-sql-database.md) voor het repliceren van uw datum tussen databases.
- [Detectie van bedreigingen instellen](sql-database-threat-detection.md) om te laten identificeren van verdachte activiteiten, zoals SQL-injectie of toegang vanaf verdachte locaties van Azure SQL Database.
- [Dynamische gegevensmaskering instellen](sql-database-dynamic-data-masking-get-started-portal.md) om uw gevoelige gegevens te beveiligen.
- [Configureren van back-upretentie](sql-database-long-term-backup-retention-configure.md) voor een database om te blijven van uw back-ups van Azure Blob Storage. Als alternatief er [configureren van back-upretentie met behulp van Azure vault (afgeschaft)](sql-database-long-term-backup-retention-configure-vault.md) benadering.
- [Geo-replicatie configureren](sql-database-geo-replication-portal.md) te houden van een replica van de database in een andere regio.
- [Configureren van beveiliging voor geo-replica's](sql-database-geo-replication-security-config.md).

## <a name="monitor-and-tune-your-database"></a>Controleren en afstemmen van uw database

- [Automatisch instellen inschakelen](sql-database-automatic-tuning-enable.md) zodat Azure SQL Database optimaliseren van prestaties van uw workload.
- [E-mailmeldingen voor automatisch afstemmen](sql-database-automatic-tuning-email-notifications.md) voor informatie over aanbevelingen voor het afstemmen.
- [Prestatie-aanbevelingen toepassen](sql-database-advisor-portal.md) en optimaliseer uw database.
- [Waarschuwingen maken](sql-database-insights-alerts-portal.md) meldingen wilt ontvangen van Azure SQL Database.
- [Problemen met verbindingen oplossen](sql-database-troubleshoot-common-connection-issues.md) als u merkt dat sommige problemen met de netwerkverbinding tussen de toepassingen en de database. U kunt ook [resourcestatus voor problemen met de netwerkverbinding](sql-database-resource-health.md).
- [Beheren van bestandsruimte](sql-database-file-space-management.md) om het opslaggebruik in uw database bewaken.

## <a name="query-distributed-data"></a>Een query uitvoeren op gedistribueerde gegevens

- [Query uitvoeren op verticaal gepartitioneerde gegevens](sql-database-elastic-query-getting-started-vertical.md) voor meerdere databases.
- [Rapport over uitgeschaalde gegevenslaag](sql-database-elastic-query-horizontal-partitioning.md).
- [Query's op tabellen met verschillende schema's](sql-database-elastic-query-vertical-partitioning.md).

## <a name="elastic-database-jobs"></a>Taken voor Elastic Database

- [Maken en beheren van](elastic-jobs-powershell.md) taken voor Elastic Database met behulp van PowerShell.
- [Maken en beheren van](elastic-jobs-tsql.md) taken voor Elastic Database met behulp van Transact-SQL.
- [Migreren van oude elastische taak](elastic-jobs-migrate.md).

## <a name="database-sharding"></a>Database-sharding

- [Clientbibliotheek voor elastic database bijwerken](sql-database-elastic-scale-upgrade-client-library.md).
- [Shard-app maken](sql-database-elastic-scale-get-started.md).
- [Query uitvoeren op horizontaal gedeelde gegevens](sql-database-elastic-query-getting-started.md).
- Voer [multi-shard query's](sql-database-elastic-scale-multishard-querying.md).
- [Shard-gegevens verplaatsen](sql-database-elastic-scale-configure-deploy-split-and-merge.md).
- [Beveiliging configureren](sql-database-elastic-scale-split-merge-security-configuration.md) in database-shards.
- [Een shard toevoegen](sql-database-elastic-scale-add-a-shard.md) instellen naar de huidige od database shards.
- [Problemen van shardtoewijzingen](sql-database-elastic-database-recovery-manager.md).
- [Shard-database migreren](sql-database-elastic-convert-to-use-elastic-tools.md).
- [Tellers maken](sql-database-elastic-database-perf-counters.md).
- [Gebruik entity framework](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) shard om gegevens te doorzoeken.
- [Gebruik Dapper framework](sql-database-elastic-scale-working-with-dapper.md) shard om gegevens te doorzoeken.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [instructies leidt in het beheerde exemplaar](sql-database-howto-managed-instance.md)
