---
title: Azure SQL Database configureren | Microsoft Docs
description: Informatie over het configureren en beheren van Azure SQL Database.
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
ms.openlocfilehash: b4dd21324591075d7625a82fbbb661c4a8e84b1d
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2018
ms.locfileid: "53440504"
---
# <a name="how-to-use-azure-sql-database"></a>Het gebruik van Azure SQL Database

In deze sectie vindt u diverse handleidingen, scripts en uitleg waarmee u kunt beheren en configureren uw Azure SQL Database. U vindt hier ook specifieke handleidingen voor [individuele Database](sql-database-howto-single-database.md) en [Managed Instance](sql-database-howto-managed-instance.md).

## <a name="load-data"></a>Gegevens laden

- [Kopiëren van een individuele database in Azure](https://docs.microsoft.com/azure/sql-database/sql-database-copy)
- [Een database vanuit een BACPAC te importeren](https://docs.microsoft.com/azure/sql-database/sql-database-import)
- [Een database exporteren naar BACPAC](https://docs.microsoft.com/azure/sql-database/sql-database-export)
- [Gegevens laden met BCP](https://docs.microsoft.com/azure/sql-database/sql-database-load-from-csv-with-bcp)
- [Gegevens laden met ADF](https://docs.microsoft.com/azure/data-factory/connector-azure-sql-database?toc=/azure/sql-database/toc.json)

### <a name="data-sync"></a>Gegevenssynchronisatie

- [SQL Data Sync](https://docs.microsoft.com/azure/sql-database/sql-database-sync-data)
- [Data Sync-Agent](https://docs.microsoft.com/azure/sql-database/sql-database-data-sync-agent)
- [Schemawijzigingen repliceren](https://docs.microsoft.com/azure/sql-database/sql-database-update-sync-schema)
- [Bewaken met OMS](https://docs.microsoft.com/azure/sql-database/sql-database-sync-monitor-oms)
- [Aanbevolen procedures voor het synchroniseren van gegevens](https://docs.microsoft.com/azure/sql-database/sql-database-best-practices-data-sync)
- [Problemen met Data Sync oplossen](https://docs.microsoft.com/azure/sql-database/sql-database-troubleshoot-data-sync)

## <a name="monitoring-and-tuning"></a>Bewaking en afstemming

-  [Handmatig instellen](https://docs.microsoft.com/azure/sql-database/sql-database-performance-guidance)
- [DMV's gebruiken om prestaties te bewaken](https://docs.microsoft.com/azure/sql-database/sql-database-monitoring-with-dmvs)
- [Query store gebruiken om prestaties te bewaken](https://docs.microsoft.com/azure/sql-database/sql-database-operate-query-store)
- [Oplossen van prestatieproblemen met intelligente inzichten](https://docs.microsoft.com/azure/sql-database/sql-database-intelligent-insights-troubleshoot-performance)
- [Intelligent Insights-diagnoselogboek gebruiken](https://docs.microsoft.com/azure/sql-database/sql-database-intelligent-insights-use-diagnostics-log)
- [In-memory OLTP schijfruimte bewaken](https://docs.microsoft.com/azure/sql-database/sql-database-in-memory-oltp-monitoring)

### <a name="extended-events"></a>Uitgebreide gebeurtenissen

- [Uitgebreide gebeurtenissen](https://docs.microsoft.com/azure/sql-database/sql-database-xevent-db-diff-from-svr)
- [Extended events Store in gebeurtenisbestand](https://docs.microsoft.com/azure/sql-database/sql-database-xevent-code-event-file)
- [Extended events Store in ringbuffer](https://docs.microsoft.com/azure/sql-database/sql-database-xevent-code-ring-buffer)

## <a name="configure-features"></a>Functies configureren

- [Azure AD-verificatie configureren](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure)
- [Voorwaardelijke toegang configureren](https://docs.microsoft.com/azure/sql-database/sql-database-conditional-access)
- [Basis van meerdere factoren AAD-verificatie](https://docs.microsoft.com/azure/sql-database/sql-database-ssms-mfa-authentication)
- [Multi-factor Authentication instellen](https://docs.microsoft.com/azure/sql-database/sql-database-ssms-mfa-authentication-configure)
- [Beleid voor tijdelijke Gegevensretentie configureren](https://docs.microsoft.com/azure/sql-database/sql-database-temporal-tables-retention-policy)
- [TDE met BYOK configureren](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-byok-azure-sql-configure)
- [TDE BYOK sleutels draaien](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-byok-azure-sql-key-rotation)
- [TDE-beveiliging verwijderen](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-byok-azure-sql-remove-tde-protector)
- [OLTP in het geheugen configureren](https://docs.microsoft.com/azure/sql-database/sql-database-in-memory-oltp-migration)
- [Azure Automation configureren](https://docs.microsoft.com/azure/sql-database/sql-database-manage-automation)

## <a name="develop-applications"></a>Toepassingen ontwikkelen

- [Connectiviteit](https://docs.microsoft.com/azure/sql-database/sql-database-libraries)
- [Spark-Connector gebruiken](https://docs.microsoft.com/azure/sql-database/sql-database-spark-connector)
- [App verifiëren](https://docs.microsoft.com/azure/sql-database/sql-database-client-id-keys)
- [Foutberichten](https://docs.microsoft.com/azure/sql-database/sql-database-develop-error-messages)
- [Batchverwerking voor betere prestaties gebruiken](https://docs.microsoft.com/azure/sql-database/sql-database-use-batching-to-improve-performance)
- [Connectiviteitsrichtlijnen](https://docs.microsoft.com/azure/sql-database/sql-database-connectivity-issues)
- [DNS-aliassen](https://docs.microsoft.com/azure/sql-database/dns-alias-overview)
- [DNS-alias PowerShell instellen](https://docs.microsoft.com/azure/sql-database/dns-alias-powershell)
- [Poorten - ADO.NET](https://docs.microsoft.com/azure/sql-database/sql-database-develop-direct-route-ports-adonet-v12)
- [C en C ++](https://docs.microsoft.com/azure/sql-database/sql-database-develop-cplusplus-simple)
- [Excel](https://docs.microsoft.com/azure/sql-database/sql-database-connect-excel)

## <a name="design-applications"></a>Toepassingen ontwerpen

- [Ontwerpen voor herstel na noodgevallen](https://docs.microsoft.com/azure/sql-database/sql-database-designing-cloud-solutions-for-disaster-recovery)
- [Ontwerp voor elastische pools](https://docs.microsoft.com/azure/sql-database/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool)
- [Ontwerp voor app-upgrades](https://docs.microsoft.com/azure/sql-database/sql-database-manage-application-rolling-upgrade)

### <a name="design-multi-tenant-saas-applications"></a>Multitenant SaaS-toepassingen ontwerpen

- [Ontwerppatronen voor SaaS](https://docs.microsoft.com/azure/sql-database/saas-tenancy-app-design-patterns)
- [SaaS-video indexer](https://docs.microsoft.com/azure/sql-database/saas-tenancy-video-index-wingtip-brk3120-20171011)
- [SaaS-app-beveiliging](https://docs.microsoft.com/azure/sql-database/saas-tenancy-elastic-tools-multi-tenant-row-level-security)

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [instructies in het beheerde exemplaar begeleidt](sql-database-howto-managed-instance.md).
- Meer informatie over [instructies leidt in één Database](sql-database-howto-single-database.md).
