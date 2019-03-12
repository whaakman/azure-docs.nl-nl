---
title: Azure SQL Database configureren | Microsoft Docs
description: Informatie over het configureren en beheren van Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: carlr
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: f7e31c9e153f25faae9224f04eabf5ca54bb06b4
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57759216"
---
# <a name="how-to-use-azure-sql-database"></a>Het gebruik van Azure SQL Database

In deze sectie vindt u diverse handleidingen, scripts en uitleg waarmee u kunt beheren en configureren uw Azure SQL Database. U vindt hier ook specifieke handleidingen voor [individuele database](sql-database-howto-single-database.md) en [Managed Instance](sql-database-howto-managed-instance.md).

## <a name="load-data"></a>Gegevens laden

- [Kopiëren van een individuele database of gegroepeerde database in Azure](sql-database-copy.md)
- [Een database vanuit een BACPAC te importeren](sql-database-import.md)
- [Een database exporteren naar BACPAC](sql-database-export.md)
- [Gegevens laden met BCP](sql-database-load-from-csv-with-bcp.md)
- [Gegevens laden met ADF](../data-factory/connector-azure-sql-database.md?toc=/azure/sql-database/toc.json)

### <a name="data-sync"></a>Gegevenssynchronisatie

- [SQL Data Sync](sql-database-sync-data.md)
- [Data Sync-Agent](sql-database-data-sync-agent.md)
- [Schemawijzigingen repliceren](sql-database-update-sync-schema.md)
- [Bewaken met OMS](sql-database-sync-monitor-oms.md)
- [Aanbevolen procedures voor het synchroniseren van gegevens](sql-database-best-practices-data-sync.md)
- [Problemen met Data Sync oplossen](sql-database-troubleshoot-data-sync.md)

## <a name="monitoring-and-tuning"></a>Bewaking en afstemming

- [Handmatig instellen](sql-database-performance-guidance.md)
- [DMV's gebruiken om prestaties te bewaken](sql-database-monitoring-with-dmvs.md)
- [Query store gebruiken om prestaties te bewaken](sql-database-operate-query-store.md)
- [Oplossen van prestatieproblemen met intelligente inzichten](sql-database-intelligent-insights-troubleshoot-performance.md)
- [Intelligent Insights-diagnoselogboek gebruiken](sql-database-intelligent-insights-use-diagnostics-log.md)
- [In-memory OLTP schijfruimte bewaken](sql-database-in-memory-oltp-monitoring.md)

### <a name="extended-events"></a>Uitgebreide gebeurtenissen

- [Uitgebreide gebeurtenissen](sql-database-xevent-db-diff-from-svr.md)
- [Extended events Store in gebeurtenisbestand](sql-database-xevent-code-event-file.md)
- [Extended events Store in ringbuffer](sql-database-xevent-code-ring-buffer.md)

## <a name="configure-features"></a>Functies configureren

- [Azure AD-verificatie configureren](sql-database-aad-authentication-configure.md)
- [Voorwaardelijke toegang configureren](sql-database-conditional-access.md)
- [Basis van meerdere factoren AAD-verificatie](sql-database-ssms-mfa-authentication.md)
- [Multi-factor Authentication instellen](sql-database-ssms-mfa-authentication-configure.md)
- [Beleid voor tijdelijke Gegevensretentie configureren](sql-database-temporal-tables-retention-policy.md)
- [TDE met BYOK configureren](transparent-data-encryption-byok-azure-sql-configure.md)
- [TDE BYOK sleutels draaien](transparent-data-encryption-byok-azure-sql-key-rotation.md)
- [TDE-beveiliging verwijderen](transparent-data-encryption-byok-azure-sql-remove-tde-protector.md)
- [OLTP in het geheugen configureren](sql-database-in-memory-oltp-migration.md)
- [Azure Automation configureren](sql-database-manage-automation.md)

## <a name="develop-applications"></a>Toepassingen ontwikkelen

- [Connectiviteit](sql-database-libraries.md)
- [Spark-Connector gebruiken](sql-database-spark-connector.md)
- [App verifiëren](sql-database-client-id-keys.md)
- [Foutberichten](sql-database-develop-error-messages.md)
- [Batchverwerking voor betere prestaties gebruiken](sql-database-use-batching-to-improve-performance.md)
- [Connectiviteitsrichtlijnen](sql-database-connectivity-issues.md)
- [DNS-aliassen](dns-alias-overview.md)
- [DNS-alias PowerShell instellen](dns-alias-powershell.md)
- [Poorten - ADO.NET](sql-database-develop-direct-route-ports-adonet-v12.md)
- [C en C ++](sql-database-develop-cplusplus-simple.md)
- [Excel](sql-database-connect-excel.md)

## <a name="design-applications"></a>Toepassingen ontwerpen

- [Ontwerpen voor herstel na noodgeval](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
- [Ontwerp voor elastische pools](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md)
- [Ontwerp voor app-upgrades](sql-database-manage-application-rolling-upgrade.md)

### <a name="design-multi-tenant-saas-applications"></a>Multitenant SaaS-toepassingen ontwerpen

- [Ontwerppatronen voor SaaS](saas-tenancy-app-design-patterns.md)
- [SaaS-video indexer](saas-tenancy-video-index-wingtip-brk3120-20171011.md)
- [SaaS-app-beveiliging](saas-tenancy-elastic-tools-multi-tenant-row-level-security.md)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [instructies gidsen voor beheerde exemplaren](sql-database-howto-managed-instance.md).
- Meer informatie over [instructies gidsen voor individuele databases](sql-database-howto-single-database.md).
