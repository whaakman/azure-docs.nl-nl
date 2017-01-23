---
title: Overzicht van Azure SQL Database-functies | Microsoft Docs
description: Deze pagina biedt een overzicht van de logische servers en databases van Azure SQL Database en omvat een functieondersteuningsmatrix met een koppeling naar elke ondersteunde functie.
services: sql-database
documentationcenter: na
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: d1a46fa4-53d2-4d25-a0a7-92e8f9d70828
ms.service: sql-database
ms.custom: overview
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 11/28/2016
ms.author: carlrab; jognanay
translationtype: Human Translation
ms.sourcegitcommit: e5b5751facb68ae4a62e3071fe4dfefc02434a9f
ms.openlocfilehash: c153f09741b9b063d67459bbb127f9c4e7942a5b


---
# <a name="azure-sql-database-features"></a>Azure SQL Database-functies
Dit onderwerp biedt een overzicht van de logische servers en databases van Azure SQL Database en omvat een functieondersteuningsmatrix met een koppeling naar elke ondersteunde functie. 

## <a name="what-is-an-azure-sql-database-logical-server"></a>Wat is een logische server met Azure SQL Database?
Een logische server met Azure SQL Database fungeert als een centraal beheerpunt voor meerdere databases. In SQL Database is een server een logische constructie die zich onderscheidt van een SQL Server-exemplaar dat u mogelijk kent vanuit de on-premises wereld. Zo geeft de service SQL Database geen garanties met betrekking tot de locatie van de databases ten opzichte van hun logische servers en zijn er geen toegang of functies op exemplaarniveau zichtbaar. Zie [Logische servers](sql-database-server-overview.md) voor meer informatie over logische Azure SQL-servers. 

## <a name="what-is-an-azure-sql-database"></a>Wat is een Azure SQL Database?
Elke database in Azure SQL Database is gekoppeld aan een logische server. De database kan bestaan uit:

- Een individuele database met een [eigen set resources](sql-database-what-is-a-dtu.md#what-are-database-transaction-units-dtus) (DTU's)
- Een onderdeel van een [groep databases](sql-database-elastic-pool.md) die [een set resources delen](sql-database-what-is-a-dtu.md#what-are-elastic-database-transaction-units-edtus) (eDTU's)
- Een onderdeel van een set [uitgeschaalde Shard-databases](sql-database-elastic-scale-introduction.md#horizontal-and-vertical-scaling), welke individuele databases of databases in een groep kunnen zijn
- Een onderdeel van een set databases die deel uitmaken van een [SaaS-ontwerppatroon met meerdere tenants](sql-database-design-patterns-multi-tenancy-saas-applications.md), waarvan de databases individuele databases of een databases in een groep (of beide) kunnen zijn 

Zie [SQL-databases](sql-database-overview.md) voor meer informatie over Azure SQL-databases.

## <a name="what-features-are-supported"></a>Welke functies worden ondersteund?

In de volgende tabellen staan de belangrijkste functies van Azure SQL Database en SQL Server, worden de ondersteuningsmogelijkheden gespecificeerd en staan koppelingen naar meer informatie over de functie op elk platform. Voor Transact-SQL-functies volgt u de koppeling in de tabel voor de categorie van de functie. Zie ook [Azure SQL Database Transact-SQL differences](sql-database-transact-sql-information.md) (Verschillen tussen Azure SQL-database en Transact-SQL) voor meer achtergrondinformatie over de redenen waarom bepaalde soorten functies niet worden ondersteund.

Er worden doorlopend extra functies toegevoegd aan V12. Het wordt daarom aangeraden om onze Azure-webpagina Service-updates regelmatig te bezoeken en filters te gebruiken:

* Gefilterd op de [SQL Database-service](https://azure.microsoft.com/updates/?service=sql-database).
* Gefilterd op [aankondigingen](http://azure.microsoft.com/updates/?service=sql-database&update-type=general-availability) over de algemene beschikbaarheid (GA) van SQL Database-functies.

> [!TIP]
> Zie [De Azure SQL Database-compatibiliteit valideren](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md) als u wilt testen of een bestaande database compatibel is met Azure SQL Database.
>

| **Functie** | **SQL Server** | **Azure SQL Database** | 
| --- | :---: | :---: | 
| Actieve Geo-replicatie | Niet ondersteund - Zie [AlwaysOn-beschikbaarheidsgroepen](https://msdn.microsoft.com/library/hh510230.aspx) | [Ondersteund](sql-database-geo-replication-overview.md)
| Altijd versleuteld | [Ondersteund](https://msdn.microsoft.com/library/mt163865.aspx) | [Ondersteund](sql-database-always-encrypted.md) |
| AlwaysOn-beschikbaarheidsgroepen | [Ondersteund](https://msdn.microsoft.com/library/hh510230.aspx) | Niet ondersteund - Zie [Actieve geo-replicatie](sql-database-geo-replication-overview.md) |
| Een database koppelen | [Ondersteund](https://msdn.microsoft.com/library/ms190209.aspx) | Niet ondersteund |
| Toepassingsrollen | [Ondersteund](https://msdn.microsoft.com/library/ms190998.aspx) | [Ondersteund](https://msdn.microsoft.com/library/ms190998.aspx) |
| Automatisch schalen | Niet ondersteund | [Ondersteund](sql-database-scale-up.md) |
| Azure Active Directory | Niet ondersteund | [Ondersteund](sql-database-aad-authentication.md) |
| Azure Data Factory | Niet ondersteund - Zie [SQL Server Integration Services (SSIS)](https://msdn.microsoft.com/library/ms141026.aspx) | [Ondersteund](https://azure.microsoft.com/services/data-factory/) |
| Controleren | [Ondersteund](https://msdn.microsoft.com/library/cc280386.aspx) | [Ondersteund](sql-database-auditing-get-started.md) |
| BACPAC-bestand (exporteren) | [Ondersteund](https://msdn.microsoft.com/library/hh213241.aspx) | [Ondersteund](sql-database-export.md) |
| BACPAC-bestand (importeren) | [Ondersteund](https://msdn.microsoft.com/library/hh710052.aspx) | [Ondersteund](sql-database-import.md) |
| BACKUP- en RESTORE-instructies | [Ondersteund](https://msdn.microsoft.com/library/ff848768.aspx) | Niet ondersteund |
| Ingebouwde functies | [Ondersteund](https://msdn.microsoft.com/library/ms174318.aspx) | [De meeste](https://msdn.microsoft.com/library/ms174318.aspx) |
| Gegevensregistratie wijzigen | [Ondersteund](https://msdn.microsoft.com/library/cc645937.aspx) | Niet ondersteund |
| Wijzigingen bijhouden | [Ondersteund](https://msdn.microsoft.com/library/bb933875.aspx) | [Ondersteund](https://msdn.microsoft.com/library/bb933875.aspx) |
| Sorteringsinstructies | [Ondersteund](https://msdn.microsoft.com/library/ff848763.aspx) | [Ondersteund](https://msdn.microsoft.com/library/ff848763.aspx) |
| Columnstore-indexen | [Ondersteund](https://msdn.microsoft.com/library/gg492088.aspx) | [Alleen de Premium Edition](https://msdn.microsoft.com/library/gg492088.aspx) |
| Common Language Runtime (CLR) | [Ondersteund](https://msdn.microsoft.com/library/ms131102.aspx) | Niet ondersteund |
| Ingesloten databases | [Ondersteund](https://msdn.microsoft.com/library/ff929071.aspx) | Ingebouwd |
| Ingesloten gebruikers | [Ondersteund](https://msdn.microsoft.com/library/ff929188.aspx) | [Ondersteund](sql-database-manage-logins.md#non-administrator-users) |
| Trefwoorden voor de taal voor het bepalen van de uitvoeringsvolgorde | [Ondersteund](https://msdn.microsoft.com/library/ms174290.aspx) | [Ondersteund](https://msdn.microsoft.com/library/ms174290.aspx) |
| Query's tussen databases | [Ondersteund](https://msdn.microsoft.com/library/dn584627.aspx) | [Elastische query’s](sql-database-elastic-query-overview.md) |
| Cursors | [Ondersteund](https://msdn.microsoft.com/library/ms181441.aspx) | [Ondersteund](https://msdn.microsoft.com/library/ms181441.aspx) | 
| Gegevenscompressie | [Ondersteund](https://msdn.microsoft.com/library/cc280449.aspx) | [Ondersteund](https://msdn.microsoft.com/library/cc280449.aspx) |
| Databaseback-ups | [Zichtbaar voor gebruikers](https://msdn.microsoft.com/library/ms187048.aspx) | [Ingebouwd](sql-database-automated-backups.md) |
| Database-e-mail | [Ondersteund](https://msdn.microsoft.com/library/ms189635.aspx) | Niet ondersteund |
| Databasespiegeling | [Ondersteund](https://msdn.microsoft.com/library/ms189852.aspx) | Niet ondersteund |
| Databaseconfiguratie-opties | [Ondersteund](https://msdn.microsoft.com/library/mt629158.aspx) | [Ondersteund](https://msdn.microsoft.com/library/mt629158.aspx) |
| Data Quality Services (DQS) | [Ondersteund](https://msdn.microsoft.com/library/ff877925.aspx) | Niet ondersteund |
| Momentopnamen van databases | [Ondersteund](https://msdn.microsoft.com/library/ms175158.aspx) | Niet ondersteund |
| Gegevenstypen | [Ondersteund](https://msdn.microsoft.com/library/ms187752.aspx) | [Ondersteund](https://msdn.microsoft.com/library/ms187752.aspx) |  
| DBCC-instructies | [Alle](https://msdn.microsoft.com/library/ms188796.aspx) | [Sommige](https://msdn.microsoft.com/library/ms188796.aspx) |
| DDL-instructies | [Ondersteund](https://msdn.microsoft.com/library/ff848799.aspx) | [De meeste](https://msdn.microsoft.com/library/ff848799.aspx)
| DDL-triggers | [Ondersteund](https://msdn.microsoft.com/library/ms175941.aspx) | [Alleen de database](https://msdn.microsoft.com/library/ms175941.aspx) |
| Gedistribueerde transacties | [MS DTC](https://msdn.microsoft.com/library/ms131665.aspx) | Alleen beperkte intra-SQL Database-scenario's |
| DML-instructies | [Ondersteund](https://msdn.microsoft.com/library/ff848766.aspx) | [De meeste](https://msdn.microsoft.com/library/ff848766.aspx) |
| DML-triggers | [Ondersteund](https://msdn.microsoft.com/library/ms178110.aspx) | [Ondersteund](https://msdn.microsoft.com/library/ms178110.aspx) |
| DMV’s | [Alle](https://msdn.microsoft.com/library/ms188754.aspx) | [Sommige](https://msdn.microsoft.com/library/ms188754.aspx) |
| elastische pools | Niet ondersteund | [Ondersteund](sql-database-elastic-pool.md) |
| Elastische taken | Niet ondersteund - Zie [SQL Server Agent](https://msdn.microsoft.com/library/ms189237.aspx) | [Ondersteund](sql-database-elastic-jobs-getting-started.md) | 
| Elastische query’s | Niet ondersteund - Zie [Query’s tussen databases](https://msdn.microsoft.com/library/dn584627.aspx) | [Ondersteund](sql-database-elastic-query-overview.md) |
| Gebeurtenismeldingen | [Ondersteund](https://msdn.microsoft.com/library/ms186376.aspx) | [Ondersteund](sql-database-insights-alerts-portal.md) |
| Expressies | [Ondersteund](https://msdn.microsoft.com/library/ms190286.aspx) | [Ondersteund](https://msdn.microsoft.com/library/ms190286.aspx) |
| Uitgebreide gebeurtenissen | [Ondersteund](https://msdn.microsoft.com/library/bb630282.aspx) | [Sommige](sql-database-xevent-db-diff-from-svr.md) |
| Uitgebreide opgeslagen procedures | [Ondersteund](https://msdn.microsoft.com/library/ms164627.aspx) | Niet ondersteund |
| Bestandsgroepen | [Ondersteund](https://msdn.microsoft.com/library/ms189563.aspx#Anchor_2) | [Alleen primair](https://msdn.microsoft.com/library/ms189563.aspx#Anchor_2) |
| Bestandsstroom | [Ondersteund](https://msdn.microsoft.com/library/gg471497.aspx) | Niet ondersteund |
| Zoekopdracht in volledige tekst | [Ondersteund](https://msdn.microsoft.com/library/ms142571.aspx) | [Niet-ondersteunde woordafbrekingen van derden](https://msdn.microsoft.com/library/ms142571.aspx) |
| Functies | [Ondersteund](https://msdn.microsoft.com/library/ms174318.aspx) | [De meeste](https://msdn.microsoft.com/library/ms174318.aspx) |
| Optimalisatie in het geheugen | [Ondersteund](https://msdn.microsoft.com/library/dn133186.aspx) | [Alleen de Premium Edition](https://msdn.microsoft.com/library/dn133186.aspx) |
| Taken | [SQL Server Agent](https://msdn.microsoft.com/library/ms189237.aspx) | [Ondersteund](sql-database-elastic-jobs-getting-started.md) |
| Ondersteuning voor JSON-gegevens | [Ondersteund](https://msdn.microsoft.com/library/dn921897.aspx) | [Ondersteund](sql-database-json-features.md) |
| Taalelementen | [Ondersteund](https://msdn.microsoft.com/library/ff848807.aspx) | [De meeste](https://msdn.microsoft.com/library/ff848807.aspx) |  
| Gekoppelde servers | [Ondersteund](https://msdn.microsoft.com/library/ms188279.aspx) | Niet ondersteund - Zie [Elastische query](sql-database-elastic-query-horizontal-partitioning.md) |
| Back-upfunctie voor logboekbestanden | [Ondersteund](https://msdn.microsoft.com/library/ms187103.aspx) | Niet ondersteund - Zie [Actieve geo-replicatie](sql-database-geo-replication-overview.md) |
| Opdrachten voor beheer | [Ondersteund](https://msdn.microsoft.com/library/ms190286.aspx)| [Niet ondersteund](https://msdn.microsoft.com/library/ms190286.aspx) |
| Master Data Services (MDS) | [Ondersteund](https://msdn.microsoft.com/library/ff487003.aspx) | Niet ondersteund |
| Minimale bulkimport voor aanmelden | [Ondersteund](https://msdn.microsoft.com/library/ms190422.aspx) | Niet ondersteund |
| Systeemgegevens wijzigen | [Ondersteund](https://msdn.microsoft.com/library/ms178028.aspx) | Niet ondersteund |
| Online-indexbewerkingen | [Ondersteund](https://msdn.microsoft.com/library/ms177442.aspx) | [De grootte van de transactie wordt beperkt door de servicelaag](https://msdn.microsoft.com/library/ms177442.aspx) |
| Operators | [Ondersteund](https://msdn.microsoft.com/library/ms174986.aspx) | [De meeste](https://msdn.microsoft.com/library/ms174986.aspx) |
| Een database herstellen naar een bepaald tijdstip | [Ondersteund](https://msdn.microsoft.com/library/ms179451.aspx) | [Ondersteund](sql-database-recovery-using-backups.md#point-in-time-restore) |
| PolyBase | [Ondersteund](https://msdn.microsoft.com/library/mt143171.aspx) | [Niet ondersteund]
| Op beleid gebaseerd beheer | [Ondersteund](https://msdn.microsoft.com/library/bb510667.aspx) | Niet ondersteund |
| Predicaten | [Ondersteund](https://msdn.microsoft.com/library/ms189523.aspx) | [De meeste](https://msdn.microsoft.com/library/ms189523.aspx)
| Resource Governor | [Ondersteund](https://msdn.microsoft.com/library/bb933866.aspx) | [Ingebouwd](sql-database-service-tiers.md) |
| Database terugzetten via back-up | [Ondersteund](https://msdn.microsoft.com/library/ms187048.aspx#anchor_6) | [Alleen via ingebouwde back-ups](sql-database-recovery-using-backups.md) |
| Beveiliging op rijniveau | [Ondersteund](https://msdn.microsoft.com/library/dn765131.aspx) | [Ondersteund](https://msdn.microsoft.com/library/dn765131.aspx) |
| Instructies voor beveiliging | [Ondersteund](https://msdn.microsoft.com/library/ff848791.aspx) | [Sommige](https://msdn.microsoft.com/library/ff848791.aspx) |
| Semantische zoekopdrachten | [Ondersteund](https://msdn.microsoft.com/library/gg492075.aspx) | Niet ondersteund |
| Volgnummers | [Ondersteund](https://msdn.microsoft.com/library/ff878058.aspx) | [Ondersteund](https://msdn.microsoft.com/library/ff878058.aspx) |
| Service Broker | [Ondersteund](https://msdn.microsoft.com/library/bb522893.aspx) | Niet ondersteund |
| Configuratie-opties voor servers | [Ondersteund](https://msdn.microsoft.com/library/ms189631.aspx) | Niet ondersteund - Zie [Databaseconfiguratie-opties](https://msdn.microsoft.com/library/mt629158.aspx) |
| Instructies instellen | [Ondersteund](https://msdn.microsoft.com/library/ms190356.aspx) | [De meeste](https://msdn.microsoft.com/library/ms190356.aspx) 
| Ruimtelijk | [Ondersteund](https://msdn.microsoft.com/library/bb933790.aspx) | [Ondersteund](https://msdn.microsoft.com/library/bb933790.aspx) |
| SQL Server Agent | [Ondersteund](https://msdn.microsoft.com/library/ms189237.aspx) | Niet ondersteund - Zie [Elastische taken](sql-database-elastic-jobs-getting-started.md) |
| SQL Server Analysis Services (SSAS) | [Ondersteund](https://msdn.microsoft.com/library/bb522607.aspx) | Niet ondersteund - Zie [Azure Analysis Services](https://azure.microsoft.com/services/analysis-services/) |
| SQL Server Integration Services (SSIS) | [Ondersteund](https://msdn.microsoft.com/library/ms141026.aspx) | Niet ondersteund - Zie [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) |
| SQL Server PowerShell | [Ondersteund](https://msdn.microsoft.com/library/hh245198.aspx) | [Ondersteund](https://msdn.microsoft.com/library/hh245198.aspx) |
| SQL Server Profiler | [Ondersteund](https://msdn.microsoft.com/library/ms181091.aspx) | Niet ondersteund - Zie [Uitgebreid gebeurtenissen](https://msdn.microsoft.com/library/ms181091.aspx) |
| SQL Server-replicatie | [Ondersteund](https://msdn.microsoft.com/library/ms151198.aspx) | [Alleen voor transactionele abonnees en abonnees met momentopnamereplicatie](sql-database-cloud-migrate-compatible-using-transactional-replication.md) |
| SQL Server Reporting Services (SSRS) | [Ondersteund](https://msdn.microsoft.com/library/ms159106.aspx) | Niet ondersteund |
| Opgeslagen procedures | [Ondersteund](https://msdn.microsoft.com/library/ms190782.aspx) | [Ondersteund](https://msdn.microsoft.com/library/ms190782.aspx) |
| In het systeem opgeslagen functies | [Ondersteund](https://msdn.microsoft.com/library/ff848780.aspx) | [Sommige](https://msdn.microsoft.com/library/ff848780.aspx) |
| In het systeem opgeslagen procedures | [Ondersteund](https://msdn.microsoft.com/library/ms187961.aspx) | [Sommige](https://msdn.microsoft.com/library/ms187961.aspx) |
| Systeemtabellen | [Ondersteund](https://msdn.microsoft.com/library/ms179932.aspx) | [Sommige](https://msdn.microsoft.com/library/ms179932.aspx) |
| Systeemweergaven | [Ondersteund](https://msdn.microsoft.com/library/ms177862.aspx) | [Sommige](https://msdn.microsoft.com/library/ms177862.aspx)
| Tabellen partitioneren | [Ondersteund](https://msdn.microsoft.com/library/ms190787.aspx) | [Alleen primaire bestandsgroep](https://msdn.microsoft.com/library/ms190787.aspx) |
| Tijdelijke tabellen | [Lokaal en wereldwijd](https://msdn.microsoft.com/library/ms174979.aspx#Anchor_4) | [Alleen lokaal](https://msdn.microsoft.com/library/ms174979.aspx#Anchor_4) |
| Tijdelijke tabellen | [Ondersteund](https://msdn.microsoft.com/library/dn935015.aspx) | [Ondersteund](sql-database-temporal-tables.md) |
| Transactie-instructies | [Ondersteund](https://msdn.microsoft.com/library/ms174377.aspx) | [Ondersteund](https://msdn.microsoft.com/library/ms174377.aspx) |
| Variabelen | [Ondersteund](https://msdn.microsoft.com/library/ff848809.aspx) | | [Ondersteund](https://msdn.microsoft.com/library/ff848809.aspx) | 
| Transparent Data Encryption (TDE)  | [Ondersteund](https://msdn.microsoft.com/library/bb934049.aspx) | [Ondersteund](https://msdn.microsoft.com/dn948096.aspx) |
| Windows Server-failoverclusters | [Ondersteund](https://msdn.microsoft.com/library/hh270278.aspx) | Niet ondersteund - Zie [Actieve geo-replicatie](sql-database-geo-replication-overview.md) |
| XML-indexen | [Ondersteund](http://msdn.microsoft.com/library/bb934097.aspx) | [Ondersteund](http://msdn.microsoft.com/library/bb934097.aspx) |
| XML-instructies | [Ondersteund](https://msdn.microsoft.com/library/ff848798.aspx) | [Ondersteund](https://msdn.microsoft.com/library/ff848798.aspx) |

## <a name="next-steps"></a>Volgende stappen

- Zie [Wat is SQL Database?](sql-database-technical-overview.md) voor informatie over de service Azure SQL Database
- Zie [Overzicht van logische SQL Database-servers](sql-database-server-overview.md) voor een overzicht van logische Azure SQL-servers
- Zie [Overzicht van SQL Database](sql-database-overview.md) voor een overzicht van Azure SQL-databases
- Zie [Azure SQL Database Transact-SQL differences](sql-database-transact-sql-information.md) (Verschillen tussen Azure SQL-database en Transact-SQL) voor meer informatie over Transact-SQL-ondersteuning en -verschillen.
- Voor informatie over specifieke resourcequota en beperkingen op basis van uw **servicelaag**. Zie [SQL Database-servicelagen](sql-database-service-tiers.md) voor een overzicht van servicelagen.
- Zie [Azure SQL Database Security Overview](sql-database-security-overview.md) (Overzicht van Azure SQL Database-beveiliging) voor een beveiligingsoverzicht.
- Voor informatie over de beschikbaarheid van stuurprogramma's en ondersteuning voor SQL Database raadpleegt u [Connection Libraries for SQL Database and SQL Server](sql-database-libraries.md) (Verbindingsbibliotheken voor SQL Database en SQL Server).



<!--HONumber=Dec16_HO4-->


