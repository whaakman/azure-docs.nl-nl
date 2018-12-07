---
title: Data migration service en hulpprogramma's matrix - Azure | Microsoft Docs
description: Meer informatie over de services en hulpprogramma's die beschikbaar zijn voor het migreren van databases en voor de ondersteuning van verschillende fasen van het migratieproces.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: ''
ms.reviewer: ''
ms.service: database-migration
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 12/07/2018
ms.openlocfilehash: a3580c2939f03e6ede6341e7afb293e7f7c5f885
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2018
ms.locfileid: "53016126"
---
# <a name="service-and-tools-for-data-migration"></a>Service en -hulpprogramma's voor migratie van gegevens

In dit artikel bevat een matrix van de Microsoft en de services van derden en de hulpprogramma's om u te helpen met verschillende database en migratiescenario's waarbij gegevens en speciale taken beschikbaar.

De volgende tabellen identificeren de service en de hulpprogramma's die u gebruiken kunt om te plannen is voor migratie van gegevens en om te voltooien van de verschillende stadia.

> [!NOTE]
> In de volgende tabellen staan voor items die zijn gemarkeerd met een sterretje (*) hulpprogramma's van derden.

## <a name="business-justification-stage"></a>Zakelijke reden fase

| **Bron** | **Doel** | **Ontdek /**<br/>**Inventaris** | **Doel en SKU**<br/>**Aanbeveling** | **Totale Eigendomskosten/rendement op investering opleveren en**<br/>**Bedrijfsscenario** |
| --- | --- | --- | --- | --- |
| SQL Server | Azure SQL Database | [MAP Toolkit](https://msdn.microsoft.com/library/bb977556.aspx)<br/>[Azure Migrate](https://azure.microsoft.com/services/azure-migrate/)<br/>[Cloudamize *](https://www.cloudamize.com/) | [DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017)<br/>[Cloud-Atlas *](https://www.unifycloud.com/cloud-migration-tool/)<br/>[Cloudamize *](https://www.cloudamize.com/) | [TCO Calculator](https://azure.microsoft.com/pricing/tco/calculator/) |
| SQL Server | Azure SQL DB MI | [MAP Toolkit](https://msdn.microsoft.com/library/bb977556.aspx)<br/>[Azure Migrate](https://azure.microsoft.com/services/azure-migrate/)<br/>[Cloudamize *](https://www.cloudamize.com/) | [Cloud-Atlas *](https://www.unifycloud.com/cloud-migration-tool/)<br/>[Cloudamize *](https://www.cloudamize.com/) | [TCO Calculator](https://azure.microsoft.com/pricing/tco/calculator/) |
| SQL Server | Azure SQL-VM | [MAP Toolkit](https://msdn.microsoft.com/library/bb977556.aspx)<br/>[Azure Migrate](https://azure.microsoft.com/services/azure-migrate/)<br/>[Cloudamize *](https://www.cloudamize.com/) | [Cloud-Atlas *](https://www.unifycloud.com/cloud-migration-tool/)<br/>[Cloudamize *](https://www.cloudamize.com/) | [TCO Calculator](https://azure.microsoft.com/pricing/tco/calculator/) |
| SQL Server | SQL DW |  |  | [TCO Calculator](https://azure.microsoft.com/pricing/tco/calculator/) |
| EXTERN BUREAUBLAD-SERVICES SQL | Azure SQL DB, MI-VM |  | [DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017) | [TCO Calculator](https://azure.microsoft.com/pricing/tco/calculator/) |
| Oracle | Azure SQL DB, MI-VM | [MAP Toolkit](https://msdn.microsoft.com/library/bb977556.aspx)<br/>[Azure Migrate](https://azure.microsoft.com/services/azure-migrate/) | [MigVisor *](https://www.migvisor.com/) |  |
| Oracle | SQL DW | [MAP Toolkit](https://msdn.microsoft.com/library/bb977556.aspx)<br/>[Azure Migrate](https://azure.microsoft.com/services/azure-migrate/) |  |  |
| Oracle | Azure DB voor PostgreSQL |  |  |  |
| MongoDB | Cosmos DB | [Cloudamize *](https://www.cloudamize.com/) | [Cloudamize *](https://www.cloudamize.com/) |  |
| Cassandra | Cosmos DB |  |  |  |
| Extern bureaublad-services/on-premises MySQL | Azure DB voor MySQL |  |  | [TCO Calculator](https://azure.microsoft.com/pricing/tco/calculator/) |
| Extern bureaublad-services/on-premises PostgreSQL | Azure DB voor PostgreSQL |  |  | [TCO Calculator](https://azure.microsoft.com/pricing/tco/calculator/) |
| MySQL | Azure SQL DB, MI-VM | [Azure Migrate](https://azure.microsoft.com/services/azure-migrate/) | [Cloud-Atlas *](https://www.unifycloud.com/cloud-migration-tool/) | [TCO Calculator](https://azure.microsoft.com/pricing/tco/calculator/) |
| DB2 | Azure SQL DB, MI-VM |  |  |  |
| Access | Azure SQL DB, MI-VM |  |  |  |
| Sybase | Azure SQL DB, MI-VM |  |  |  |
| | | | | |

## <a name="pre-migration-stage"></a>De fase vóór de migratie

| **Bron** | **Doel** | **Toegang tot de App-gegevens**<br/>**Laag-evaluatie** | **Database**<br/>**Evaluatie** | **Prestaties**<br/>**Evaluatie** |
| --- | --- | --- | --- | --- |
| SQL Server | Azure SQL Database |  | [DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017)<br/>[Cloud-Atlas *](https://www.unifycloud.com/cloud-migration-tool/)<br/>[Cloudamize *](https://www.cloudamize.com/) | [DEA](https://www.microsoft.com/download/details.aspx?id=54090)<br/>[Cloudamize *](https://www.cloudamize.com/) |
| SQL Server | Azure SQL DB MI |  | [DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017)<br/>[Cloud-Atlas *](https://www.unifycloud.com/cloud-migration-tool/)<br/>[Cloudamize *](https://www.cloudamize.com/) | [DEA](https://www.microsoft.com/download/details.aspx?id=54090)<br/>[Cloudamize *](https://www.cloudamize.com/) |
| SQL Server | Azure SQL-VM |  | [DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017)<br/>[Cloud-Atlas *](https://www.unifycloud.com/cloud-migration-tool/)<br/>[Cloudamize *](https://www.cloudamize.com/) | [DEA](https://www.microsoft.com/download/details.aspx?id=54090)<br/>[Cloudamize *](https://www.cloudamize.com/) |
| SQL Server | SQL DW |  |  |  |
| EXTERN BUREAUBLAD-SERVICES SQL | Azure SQL DB, MI-VM |  | [DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017) | [DEA](https://www.microsoft.com/download/details.aspx?id=54090) |
| Oracle | Azure SQL DB, MI-VM |  | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) | [Simora *](http://www.simora.co.uk/) |
| Oracle | SQL DW |  | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) | [Simora *](http://www.simora.co.uk/) |
| Oracle | Azure DB voor PostgreSQL |  |  |  |
| MongoDB | Cosmos DB |  | [Cloudamize *](https://www.cloudamize.com/) | [Cloudamize *](https://www.cloudamize.com/) |
| Cassandra | Cosmos DB |  |  |  |
| Extern bureaublad-services/on-premises MySQL | Azure DB voor MySQL |  |  |  |
| Extern bureaublad-services/on-premises PostgreSQL | Azure DB voor PostgreSQL |  |  |  |
| MySQL | Azure SQL DB, MI-VM |  | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017)<br/>[Cloud-Atlas *](https://www.unifycloud.com/cloud-migration-tool/) |  |
| DB2 | Azure SQL DB, MI-VM |  | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) |  |
| Access | Azure SQL DB, MI-VM |  | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) |  |
| Sybase | Azure SQL DB, MI-VM |  | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) |  |
| | | | | |

## <a name="migration-stage"></a>Migratie-fase

| **Bron** | **Doel** | **schema** | **Gegevens**<br/>**(Offline)** | **Gegevens**<br/>**(Online)** |
| --- | --- | --- | --- | --- |
| SQL Server | Azure SQL Database | [DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017)<br/>[Cloudamize *](https://www.cloudamize.com/) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Cloudamize *](https://www.cloudamize.com/) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Cloudamize *](https://www.cloudamize.com/)<br/>[Attunity *](https://www.attunity.com/products/replicate/)<br/>[Realtimeplatform *](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| SQL Server | Azure SQL DB MI | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Cloudamize *](https://www.cloudamize.com/) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Cloudamize *](https://www.cloudamize.com/) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Cloudamize *](https://www.cloudamize.com/)<br/>[Attunity *](https://www.attunity.com/products/replicate/)<br/>[Realtimeplatform *](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| SQL Server | Azure SQL-VM | [DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017)<br/>[Cloudamize *](https://www.cloudamize.com/) | [DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017)<br/>[Cloudamize *](https://www.cloudamize.com/) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Cloudamize *](https://www.cloudamize.com/)<br/>[Attunity *](https://www.attunity.com/products/replicate/)<br/>[Realtimeplatform *](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| SQL Server | SQL DW |  |  |  |
| EXTERN BUREAUBLAD-SERVICES SQL | Azure SQL DB, MI-VM | [DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017) | [DMS](https://azure.microsoft.com/services/database-migration/) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Attunity *](https://www.attunity.com/products/replicate/)<br/>[Realtimeplatform *](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| Oracle | Azure SQL DB, MI-VM | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017)<br/>[SharePlex *](https://www.quest.com/products/shareplex/) | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017)<br/>[SharePlex *](https://www.quest.com/products/shareplex/) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[SharePlex *](https://www.quest.com/products/shareplex/)<br/>[Attunity *](https://www.attunity.com/products/replicate/)<br/>[Realtimeplatform *](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| Oracle | SQL DW | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) |
| Oracle | Azure DB voor PostgreSQL |  |  |  |
| MongoDB | Cosmos DB | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Cloudamize *](https://www.cloudamize.com/)<br/>[Imanis gegevens *](https://www.imanisdata.com/wp-content/uploads/2018/02/Imanis_DS_MongoDB_Azure_FINAL.pdf) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Cloudamize *](https://www.cloudamize.com/)<br/>[Imanis gegevens *](https://www.imanisdata.com/wp-content/uploads/2018/02/Imanis_DS_MongoDB_Azure_FINAL.pdf) | [Cloudamize *](https://www.cloudamize.com/)<br/>[Imanis gegevens *](https://www.imanisdata.com/wp-content/uploads/2018/02/Imanis_DS_MongoDB_Azure_FINAL.pdf)<br/>[Realtimeplatform *](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| Cassandra | Cosmos DB | [Imanis gegevens *](https://www.imanisdata.com/wp-content/uploads/2018/02/Imanis_DS_MongoDB_Azure_FINAL.pdf) | [Imanis gegevens *](https://www.imanisdata.com/wp-content/uploads/2018/02/Imanis_DS_MongoDB_Azure_FINAL.pdf) | [Imanis gegevens *](https://www.imanisdata.com/wp-content/uploads/2018/02/Imanis_DS_MongoDB_Azure_FINAL.pdf) |
| Extern bureaublad-services/on-premises MySQL | Azure DB voor MySQL | [MySQL-dump *](https://dev.mysql.com/doc/refman/5.7/en/mysqldump.html) | [DMS](https://azure.microsoft.com/services/database-migration/) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Attunity *](https://www.attunity.com/products/replicate/)<br/>[Realtimeplatform *](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| Extern bureaublad-services/on-premises PostgreSQL | Azure DB voor PostgreSQL | [PG dump *](https://www.postgresql.org/docs/11/static/app-pgdump.html) | [DMS](https://azure.microsoft.com/services/database-migration/) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Attunity *](https://www.attunity.com/products/replicate/)<br/>[Realtimeplatform *](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| MySQL | Azure SQL DB, MI-VM | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Attunity *](https://www.attunity.com/products/replicate/)<br/>[Realtimeplatform *](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| DB2 | Azure SQL DB, MI-VM | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Attunity *](https://www.attunity.com/products/replicate/)<br/>[Realtimeplatform *](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| Access | Azure SQL DB, MI-VM | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) |
| Sybase | Azure SQL DB, MI-VM | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) | [SSMA](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Attunity *](https://www.attunity.com/products/replicate/)<br/>[Realtimeplatform *](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| | | | | |

## <a name="post-migration-stage"></a>De fase na de migratie

| **Bron** | **Doel** | **Optimaliseren** |
| --- | --- | --- |
| SQL Server | Azure SQL Database | [Cloud-Atlas *](https://www.unifycloud.com/cloud-migration-tool/)<br/>[Cloudamize *](https://www.cloudamize.com/) |
| SQL Server | Azure SQL DB MI | [Cloud-Atlas *](https://www.unifycloud.com/cloud-migration-tool/)<br/>[Cloudamize *](https://www.cloudamize.com/) |
| SQL Server | Azure SQL-VM | [Cloud-Atlas *](https://www.unifycloud.com/cloud-migration-tool/)<br/>[Cloudamize *](https://www.cloudamize.com/) |
| SQL Server | SQL DW |  |
| EXTERN BUREAUBLAD-SERVICES SQL | Azure SQL DB, MI-VM |  |
| Oracle | Azure SQL DB, MI-VM |  |
| Oracle | SQL DW |  |
| Oracle | Azure DB voor PostgreSQL |  |
| MongoDB | Cosmos DB | [Cloudamize *](https://www.cloudamize.com/) |
| Cassandra | Cosmos DB |  |
| Extern bureaublad-services/on-premises MySQL | Azure DB voor MySQL |  |
| Extern bureaublad-services/on-premises PostgreSQL | Azure DB voor PostgreSQL |  |
| MySQL | Azure SQL DB, MI-VM |  |
| DB2 | Azure SQL DB, MI-VM |  |
| Access | Azure SQL DB, MI-VM |  |
| Sybase | Azure SQL DB, MI-VM |  |
| | | |

## <a name="next-steps"></a>Volgende stappen

Zie voor een overzicht van de Azure Database Migration Service, [wat is de Azure Database Migration Service Preview](dms-overview.md).
