---
title: Opmerkingen bij de Release van de Azure SQL Database | Microsoft Docs
description: Meer informatie over de nieuwe functies en verbeteringen in de Azure SQL Database-service en in de documentatie van Azure SQL Database
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.subservice: service
ms.devlang: ''
ms.topic: conceptual
ms.date: 04/03/2019
ms.author: carlrab
ms.openlocfilehash: c63ce51f442c8d7552c382f0e103be443afb9c75
ms.sourcegitcommit: 43b85f28abcacf30c59ae64725eecaa3b7eb561a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59360080"
---
# <a name="sql-database-release-notes"></a>Opmerkingen bij de release van SQL-Database

In dit artikel bevat de nieuwe functies en verbeteringen in de service SQL Database en in de documentatie van SQL-Database. Zie voor verbetering van SQL Database-service ook [updates van de service SQL Database](https://azure.microsoft.com/updates/?product=sql-database). Zie voor verbeteringen aan andere Azure-services, [Service-updates](https://azure.microsoft.com/updates).

## <a name="features-in-public-preview"></a>Functies in public preview

| Functie | Details |
| ---| --- |
| Taken voor Elastic Database | Zie voor meer informatie, [maken, configureren en beheren van elastische taken](elastic-jobs-overview.md) |
| Elastische transacties | [Over clouddatabases gedistribueerde transacties](sql-database-elastic-transactions-overview.md) |
| Elastische query’s | Zie voor meer informatie, [elastische query's-overzicht](sql-database-elastic-query-overview.md) |
| Replicatie met beheerde exemplaren |Zie voor meer informatie, [-replicatie in een Azure SQL Database beheerde Exemplaardatabase configureren](replication-with-sql-database-managed-instance.md)|
| De sortering van het exemplaar met beheerde exemplaren |Zie voor meer informatie, [PowerShell gebruiken met Azure Resource Manager-sjabloon maken van een beheerd exemplaar in Azure SQL Database](./scripts/sql-managed-instance-create-powershell-azure-resource-manager-template.md)|
| R services / machine learning met individuele databases en elastische pools |Zie voor meer informatie, [Machine Learning-Services in Azure SQL Database](https://docs.microsoft.com/sql/advanced-analytics/what-s-new-in-sql-server-machine-learning-services?view=sql-server-2017#machine-learning-services-in-azure-sql-database)|
| Versneld databaseherstel met individuele databases en elastische pools | Zie voor meer informatie, [versneld databaseherstel](sql-database-accelerated-database-recovery.md)|
| Gegevensdetectie en -classificatie  |Zie voor meer informatie, [Azure SQL Database en SQL Data Warehouse gegevensdetectie en classificatie](sql-database-data-discovery-and-classification.md)|
| Transparante gegevensversleuteling (TDE) met Bring Your Own Key (BYOK) met beheerde exemplaren |Zie voor meer informatie, [Azure SQL Transparent Data Encryption met de klant beheerde sleutels in Azure Key Vault: Bring Your Own Key-ondersteuning](transparent-data-encryption-byok-azure-sql.md)|
| Opnieuw maken verwijderde databases met beheerde exemplaren |Zie voor meer informatie, [databases in Azure SQL Managed Instance opnieuw maken verwijderd](https://medium.com/azure-sqldb-managed-instance/re-create-dropped-databases-in-azure-sql-managed-instance-dc369ed60266)|
| Detectie van bedreigingen met beheerde exemplaren |Zie voor meer informatie, [detectie van bedreigingen configureren in Azure SQL-Database beheerd exemplaar](sql-database-managed-instance-threat-detection.md)|
| Zeer grootschalige service-lagen met individuele databases |Zie voor meer informatie, [grootschalige servicelaag voor maximaal 100 TB](sql-database-service-tier-hyperscale.md)|
| Query-editor in Azure portal |Zie voor meer informatie, [van de Azure portal SQL query-editor gebruiken om te verbinden en gegevens op te vragen](sql-database-connect-query-portal.md)|
|Geschatte aantal afzonderlijke|Zie voor meer informatie, [geschatte Count Distinct](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing#approximate-query-processing)|
|Batch-servicemodus op Rowstore (onder compatibiliteitsniveau 150)|Zie voor meer informatie, [batchmodus op Rowstore](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing#batch-mode-on-rowstore)|
|Feedback op Geheugentoekenning (rij-modus) (met compatibiliteitsniveau 150)|Zie voor meer informatie, [Feedback op Geheugentoekenning in (rij-modus)](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing#row-mode-memory-grant-feedback)|
|Tabel variabele uitgesteld compilatie (onder compatibiliteitsniveau 150)|Zie voor meer informatie, [tabel variabele uitgesteld compilatie](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing#table-variable-deferred-compilation)|
|||

## <a name="march-2019"></a>Maart 2019

### <a name="service-improvements"></a>Verbeteringen van de services

| Verbeteringen van de services | Details |
| --- | --- |
| Algemene beschikbaarheid: Ondersteuning voor uitschalen voor leesbewerkingen in Azure SQL Database | Zie voor meer informatie, [Read scale-out](sql-database-read-scale-out.md)|
| &nbsp; |

### <a name="documentation-improvements"></a>Verbeteringen in de documentatie bij

| Verbeteringen in de documentatie bij | Details |
| --- | --- |
| Toegevoegde log-limieten voor individuele databases|Zie voor meer informatie, [Single database vCore-resourcebeperkingen](sql-database-vcore-resource-limits-single-databases.md).|
| Toegevoegde log-limieten voor elastische pools en gepoolde databases|Zie voor meer informatie, [vCore resourcelimieten elastische pools](sql-database-vcore-resource-limits-elastic-pools.md).|
| Toegevoegde Transaction log tarief governance| Nieuwe inhoud toegevoegd [Transaction log tarief governance](sql-database-resource-limits-database-server.md#transaction-log-rate-governance).|
| Bijgewerkte PowerShell-voorbeelden voor individuele databases en elastische pools az.sql modules gebruiken | Zie voor meer informatie, [PowerShell-voorbeelden voor individuele databases en elastische pools](sql-database-powershell-samples.md#single-database-and-elastic-pools).|
| &nbsp; |

## <a name="february-2019"></a>Februari 2019

### <a name="service-improvements"></a>Verbeteringen van de services

| Verbeteringen van de services | Details |
| --- | --- |
|Het maken van een hervatbare index online is nu algemeen beschikbaar| Zie voor meer informatie, [Create Index](https://docs.microsoft.com/sql/t-sql/statements/create-index-transact-sql).|
|Ondersteuning voor beheerd exemplaar voor routetabellen verbeterd| Zie voor meer informatie, [vereisten voor de](sql-database-managed-instance-connectivity-architecture.md#network-requirements).|
|Naam van database ondersteund in het beheerde exemplaar | Zie voor meer informatie de [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-mi-current) en [sp_rename](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-rename-transact-sql) syntaxis.|
|SQL-Database als een bron van referentiegegevens voor Stream Analytics. | Zie voor meer informatie, [Stream Analytics](https://azure.microsoft.com/services/stream-analytics/).|
|Data Migration Assistance voegt ondersteuning toe voor beheerd exemplaar. |Zie voor meer informatie, [wat is er nieuw in DMA](https://docs.microsoft.com/sql/dma/dma-whatsnew).|
|SQL Server Migration Assistant voegt ondersteuning toe voor evaluatie van gereedheid doel voor het beheerde exemplaar. | Zie voor meer informatie, [SQL Server Migration Assistant](https://docs.microsoft.com/sql/ssma/sql-server-migration-assistant).
|Data migratieservice ondersteunt migreren van Amazon extern bureaublad-services naar beheerd exemplaar | Zie [Zelfstudie: SQL-Server voor extern bureaublad-services migreren naar Azure SQL Database of een Azure SQL-Database beheerd exemplaar online met behulp van DMS](../dms/tutorial-rds-sql-server-azure-sql-and-managed-instance-online.md).|
| &nbsp; |

### <a name="documentation-improvements"></a>Verbeteringen in de documentatie bij

| Verbeteringen in de documentatie bij | Details |
| --- | --- |
|Toevoegen voor het beheerde exemplaar implementatie optie verduidelijkingen|Groot aantal artikelen voor het verduidelijken van toepasselijkheid op individuele databases, elastische pool en implementatieopties voor beheerd exemplaar bijgewerkt. |
|Bijgewerkte tempdb-grootten voor DTU gebaseerde aankoopmodel | Zie voor meer informatie, [Tempdb-database in SQL-Database](https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database#tempdb-database-in-sql-database).|
|Bijgewerkte importeren en exporteren met bacpac-bestand voor de ondersteuning van het beheerde exemplaar| Zie voor meer informatie, [importeren vanuit BACPAC](sql-database-import.md) en [exporteren naar BACPAC](sql-database-export.md). |
| &nbsp; |


## <a name="january-2019"></a>Januari 2019

### <a name="service-improvements"></a>Verbeteringen van de services

| Verbeteringen van de services | Details |
| --- | --- |
| Als u meer granulatie opties voor compute-resources | Servicelagen voor algemeen gebruik en bedrijfskritiek [enkelvoudige databases](sql-database-vcore-resource-limits-single-databases.md) en [elastische pools](sql-database-vcore-resource-limits-elastic-pools.md) beschikt nu over meer fijnmazig compute-opties.|
| Controlerecords bekijken voor het beheerde exemplaar in de Azure-portal | Weergeven van [controlerecords voor beheerde exemplaren](sql-database-managed-instance-auditing.md) in Azure portal nu wordt ondersteund.|
| Geavanceerde threat detectiefunctie gewijzigd in de geavanceerde beveiliging van gegevens | Geavanceerde threat detectiefunctie gewijzigd in [gegevensbeveiliging geavanceerde](sql-advanced-threat-protection.md) voor individuele databases, elastische pools en beheerde exemplaren. |
| &nbsp; |

### <a name="documentation-improvements"></a>Verbeteringen in de documentatie bij

| Verbeteringen in de documentatie bij | Details |
| --- | --- |
| Beheerde exemplaren en transactionele replicatie | Toegevoegde artikel over het gebruik van [transactionele replicatie met beheerde exemplaren](replication-with-sql-database-managed-instance.md) |
| Extra Azure AD met beheerd exemplaar zelfstudie | Dit [Azure AD met beheerd exemplaar](sql-database-managed-instance-aad-security-tutorial.md) zelfstudie ziet u moet configureren en testen beheerd exemplaar security met behulp van Azure AD-aanmeldingen. |
| Bijgewerkte inhoud voor het automatiseren van de taak met behulp van Transact-SQL-scripts | Bijgewerkt en er wordt duidelijk gemaakt inhoud voor het gebruik van [taak automatisering met behulp van Transact-SQL-scripts](sql-database-job-automation-overview.md) voor individuele databases, elastische pools en beheerde exemplaren |
| Inhoud van de beveiliging voor beheerde exemplaren bijgewerkt | Bijgewerkt en er wordt duidelijk gemaakt inhoud voor de [beveiligingsmodel voor beheerde exemplaren](sql-database-security-overview.md), en verschillen met het beveiligingsmodel voor individuele databases en elastische pools |
| Vernieuwd alle snelstartgidsen en zelfstudies | Alle snelstartgidsen en zelfstudies in de [documentatie](https://docs.microsoft.com/azure/sql-database) zijn bijgewerkt en vernieuwd zodat deze overeenkomen met wijzigingen in de Azure-portal |
| Overzicht van toegevoegde snelstartgidsen | Een overzicht van Snelstartgids voor toegevoegd [enkelvoudige databases](sql-database-quickstart-guide.md) en voor [instanties die worden beheerd](sql-database-managed-instance-quickstart-guide.md) |
| Toegevoegde SQL-Database verklarende woordenlijst | Dit [verklarende woordenlijst](sql-database-glossary-terms.md) artikel bevat een definitieve lijst met de bepalingen van de SQL-Database en koppelingen naar de primaire conceptuele pagina waarin wordt uitgelegd van de term in context. |
| &nbsp; |

## <a name="contribute-to-content-improvement"></a>Bijdragen aan inhoud verbetering

De Azure SQL-documentatieset is open-source. In het openbaar werken biedt verschillende voordelen:

- Opensourceopslagplaatsen planning in om op te halen van feedback over welke documenten het meest nodig zijn zijn.
- Opensourceopslagplaatsen in de open voor het publiceren van de inhoud van het nuttigst op onze eerste release.
- Open-source-opslagplaatsen bijwerken in de geopend zodat u gemakkelijk het continu verbeteren van de inhoud.

Als u wilt bijdragen aan inhoud in de documentatie bij Azure SQL Database, Zie de [Microsoft-Docs Inzender handleiding overzicht](https://docs.microsoft.com/contribute/). De gebruikerservaring op [docs.microsoft.com](https://docs.microsoft.com/) integreert [GitHub](https://github.com/) werkstromen rechtstreeks aan u nog gemakkelijker. Begin met [bewerken van het document dat u bekijkt](https://docs.microsoft.com/contribute/#quick-edits-to-existing-documents). Of u kunt helpen door [nieuwe onderwerpen te beoordelen](https://docs.microsoft.com/contribute/#review-open-prs), of [kwaliteitsproblemen maken](https://docs.microsoft.com/contribute/#create-quality-issues).
