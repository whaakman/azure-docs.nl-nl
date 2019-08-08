---
title: Artikel over het oplossen van problemen/fouten die zijn gekoppeld aan een Azure Database Migration Service verbinden met brondatabases bekende | Microsoft Docs
description: Meer informatie over het oplossen van bekende problemen/fouten die zijn gekoppeld aan een Azure Database Migration Service verbinden met brondatabases.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 06/28/2019
ms.openlocfilehash: a4ebd1d4c85631cc6ebc1f5787e7545b78d62b5e
ms.sourcegitcommit: aa66898338a8f8c2eb7c952a8629e6d5c99d1468
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67462848"
---
# <a name="troubleshoot-dms-errors-when-connecting-to-source-databases"></a>DMS-fouten oplossen bij het verbinden met de brondatabases

Het volgende artikel vindt u gedetailleerde informatie over hoe u mogelijke problemen die optreden bij het verbinden van de Azure Database Migration Service (DMS) met de brondatabase op te lossen. Elke sectie hieronder is gekoppeld aan een specifiek type source-database, met de fout optreden, samen met details en koppelingen naar informatie over het oplossen van de connectiviteit.

## <a name="sql-server"></a>SQL Server

Potentiële problemen die zijn gekoppeld aan de verbinding te maken met een SQL Server-brondatabase en hoe u deze aanpakt vindt u in de volgende tabel.

| Fout         | Oorzaak en de details van probleemoplossing |
| ------------- | ------------- |
| SQL-verbinding is mislukt. Een netwerkgerelateerde of exemplaarspecifieke fout is opgetreden bij het maken van een verbinding met SQL Server. De server wordt niet gevonden of toegang tot de server is niet mogelijk. Controleer of de exemplaarnaam juist is, en SQL Server is geconfigureerd voor het toestaan van externe verbindingen.<br> | Deze fout treedt op als de service kan de bronserver niet vinden. Om het probleem op te lossen, raadpleegt u het artikel [fout bij het verbinding maken met de bron-SQL Server bij het gebruik van dynamische poort of benoemd exemplaar](https://docs.microsoft.com/azure/dms/known-issues-troubleshooting-dms#error-connecting-to-source-sql-server-when-using-dynamic-port-or-named-instance). |
| **Fout 53** -SQL-verbinding is mislukt. (Ook voor fout codes 1, 2, 5, 53, 233, 258, 1225, 11001)<br><br> | Deze fout treedt op als de service kan geen verbinding met de bronserver maken. Om het probleem op te lossen, raadpleegt u de volgende bronnen en probeer het opnieuw. <br><br>  [Interactieve gebruikershandleiding om op te lossen van het probleem met de netwerkverbinding](https://support.microsoft.com/help/4009936/solving-connectivity-errors-to-sql-server)<br><br> [Vereisten voor SQL-Server migreren naar Azure SQL Database](https://docs.microsoft.com/azure/dms/pre-reqs#prerequisites-for-migrating-sql-server-to-azure-sql-database) <br><br> [Vereisten voor SQL-Server migreren naar een beheerd exemplaar voor Azure SQL Database](https://docs.microsoft.com/azure/dms/pre-reqs#prerequisites-for-migrating-sql-server-to-an-azure-sql-database-managed-instance) |
| **Fout 18456** -aanmelding is mislukt.<br> | Deze fout treedt op als de service kan geen verbinding maken met de brondatabase met behulp van de opgegeven T-SQL-referenties. Om het probleem op te lossen, Controleer of de ingevoerde referenties. U kunt ook verwijzen naar [MSSQLSERVER_18456](https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-18456-database-engine-error?view=sql-server-2017) of voor het oplossen van problemen met documenten die worden vermeld in de opmerking onder deze tabel en probeer het opnieuw. |
| Onjuist gevormde AccountName-waarde '{0}is opgegeven. Verwachte notatie voor AccountName is Domeinnaam\gebruikersnaam<br> | Deze fout treedt op als de gebruiker Windows-verificatie selecteert, maar biedt de gebruikersnaam in een ongeldige indeling. Om het probleem op te lossen, Geef een gebruikersnaam in de juiste indeling voor Windows-verificatie of selecteer **SQL-verificatie**. |

## <a name="aws-rds-mysql"></a>AWS RDS MySQL

Potentiële problemen die zijn gekoppeld aan de verbinding te maken met een bron AWS RDS MySQL-database en hoe u deze aanpakt vindt u in de volgende tabel.

| Fout         | Oorzaak en de details van probleemoplossing |
| ------------- | ------------- |
| **Fout [2003]** [HY000] - verbinding is mislukt. Fout [HY000] [MySQL] [ODBC x.x(w) stuurprogramma] kan geen verbinding maken met MySQL-server op {server} (10060) | Deze fout treedt op als het MySQL ODBC-stuurprogramma geen verbinding met de bronserver maken. Om het probleem op te lossen, raadpleegt u het oplossen van problemen met documenten die worden vermeld in de opmerking onder deze tabel en probeer het opnieuw.<br> |
| **Fout [2005]** [HY000] - verbinding is mislukt. Fout [HY000] [MySQL] [ODBC x.x(w) stuurprogramma] onbekende MySQL server-host {server} | Deze fout treedt op als de service de bronhost niet op RDS. vinden kan Het probleem kan zijn omdat het vermelde bron niet bestaat of er een probleem met infrastructuur voor extern bureaublad-services is. Om het probleem op te lossen, raadpleegt u het oplossen van problemen met documenten die worden vermeld in de opmerking onder deze tabel en probeer het opnieuw.<br> |
| **Fout [1045]** [HY000] - verbinding is mislukt. Fout [HY000] [MySQL] [ODBC x.x(w) stuurprogramma] toegang is geweigerd voor gebruiker {user}“@“{server} (met wachtwoord: YES) | Deze fout treedt op als MySQL ODBC-stuurprogramma geen verbinding met de bronserver vanwege ongeldige referenties maken. Controleer of de referenties die u hebt ingevoerd. Als het probleem zich blijft voordoen, controleert u of dat deze broncomputer heeft de juiste referenties. Mogelijk moet u het wachtwoord in de console opnieuw instellen. Als u nog steeds het probleem ondervindt, raadpleegt u het oplossen van problemen met documenten die worden vermeld in de opmerking onder deze tabel en probeer het opnieuw.<br> |
| **Fout [9002]** [HY000] - verbinding is mislukt. Fout [HY000] [MySQL] [ODBC x.x(w) stuurprogramma] de verbindingsreeks kan niet worden rechts. Ga naar de portal voor verwijzingen.| Deze fout treedt op als de verbinding is mislukt vanwege een probleem met de verbindingsreeks. Controleer of de opgegeven verbindingsreeks is ongeldig. Om het probleem op te lossen, raadpleegt u het oplossen van problemen met documenten die worden vermeld in de opmerking onder deze tabel en probeer het opnieuw.<br> |
| **Fout in binaire registratie. Variabele binlog_format heeft de waarde {value}. Wijzig het 'rij'.** | Deze fout treedt op als er een fout in binaire registratie; de variabele binlog_format heeft onjuiste waarde. De binlog_format in parametergroep wijzigen in 'Rij' om het probleem op te lossen, en vervolgens opnieuw opstarten van het exemplaar. Zie voor meer informatie, [binaire opties voor logboekregistratie en variabelen](https://dev.mysql.com/doc/refman/5.7/en/replication-options-binary-log.html) of [AWS RDS MySQL-Database-logboekbestanden documentatie](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_LogAccess.Concepts.MySQL.html).<br> |

> [!NOTE]
> Zie voor meer informatie over het oplossen van problemen met betrekking tot het verbinding maken met een bron AWS RDS MySQL-database met de volgende bronnen:
> * [Probleemoplossing voor verbindingsproblemen van Amazon extern bureaublad-services](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_Troubleshooting.html#CHAP_Troubleshooting.Connecting)
> * [Hoe los ik problemen met verbinding maken met mijn database-instantie van Amazon extern bureaublad-services?](https://aws.amazon.com/premiumsupport/knowledge-center/rds-cannot-connect)

## <a name="aws-rds-postgresql"></a>AWS RDS PostgreSQL

Potentiële problemen die zijn gekoppeld aan de verbinding te maken met een bron AWS RDS PostgreSQL-database en hoe u deze aanpakt vindt u in de volgende tabel.

| Fout         | Oorzaak en de details van probleemoplossing |
| ------------- | ------------- |
| **Fout [101]** [08001] - verbinding is mislukt. Fout [08001] time-out is verstreken. | Deze fout treedt op als de Postgres-stuurprogramma geen verbinding met de bronserver maken. Om het probleem op te lossen, raadpleegt u het oplossen van problemen met documenten die worden vermeld in de opmerking onder deze tabel en probeer het opnieuw. |
| **Fout: Parameter wal_level heeft de waarde {value}. Wijzig deze in logische zodat de replicatie.** | Deze fout treedt op als de parameter-wal_level onjuiste waarde heeft. Wijzigen van de rds.logical_replication in parametergroep op 1 om het probleem op te lossen, en vervolgens opnieuw opstarten van het exemplaar. Zie voor meer informatie, [vereisten voor het migreren naar Azure PostgreSQL met behulp van DMS](https://docs.microsoft.com/azure/dms/tutorial-postgresql-azure-postgresql-online#prerequisites) of [PostgreSQL op Amazon RDS](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_PostgreSQL.html). |

> [!NOTE]
> Zie voor meer informatie over het oplossen van problemen met betrekking tot het verbinding maken met een bron AWS RDS PostgreSQL-database met de volgende bronnen:
> * [Probleemoplossing voor verbindingsproblemen van Amazon extern bureaublad-services](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_Troubleshooting.html#CHAP_Troubleshooting.Connecting)
> * [Hoe los ik problemen met verbinding maken met mijn database-instantie van Amazon extern bureaublad-services?](https://aws.amazon.com/premiumsupport/knowledge-center/rds-cannot-connect)

## <a name="aws-rds-sql-server"></a>AWS RDS SQL Server

Potentiële problemen die zijn gekoppeld aan de verbinding te maken met een bron AWS RDS SQL Server-database en hoe u deze aanpakt vindt u in de volgende tabel.

| Fout         | Oorzaak en de details van probleemoplossing |
| ------------- | ------------- |
| **Fout 53** -SQL-verbinding is mislukt. Een netwerkgerelateerde of exemplaarspecifieke fout is opgetreden bij het maken van een verbinding met SQL Server. De server is niet gevonden of is niet toegankelijk is. Controleer of de exemplaarnaam juist is, en SQL Server is geconfigureerd voor het toestaan van externe verbindingen. (provider: Named Pipes-Provider, fout: 40 - kan geen verbinding met SQL Server niet openen | Deze fout treedt op als de service kan geen verbinding met de bronserver maken. Om het probleem op te lossen, raadpleegt u het oplossen van problemen met documenten die worden vermeld in de opmerking onder deze tabel en probeer het opnieuw. |
| **Fout 18456** -aanmelding is mislukt. Aanmelding voor gebruiker {user} is mislukt | Deze fout treedt op als de service voor de brondatabase kan geen verbinding met de opgegeven T-SQL-referenties maken. Om het probleem op te lossen, Controleer of de ingevoerde referenties. U kunt ook verwijzen naar [MSSQLSERVER_18456](https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-18456-database-engine-error?view=sql-server-2017) of voor het oplossen van problemen met documenten die worden vermeld in de opmerking onder deze tabel en probeer het opnieuw. |
| **Fout 87** -verbindingsreeks is niet geldig. Een netwerkgerelateerde of exemplaarspecifieke fout is opgetreden bij het maken van een verbinding met SQL Server. De server wordt niet gevonden of toegang tot de server is niet mogelijk. Controleer of de exemplaarnaam juist is, en SQL Server is geconfigureerd voor het toestaan van externe verbindingen. (provider: SQL-netwerkinterfaces, fout: 25 - verbindingsreeks is niet geldig) | Deze fout treedt op als de service kan geen verbinding met de bronserver vanwege een ongeldige verbindingsreeks maken. Om het probleem op te lossen, controleert u of de verbindingsreeks. Als het probleem zich blijft voordoen, raadpleegt u het oplossen van problemen met documenten die worden vermeld in de opmerking onder deze tabel en probeer het opnieuw. |
| **Fout - servercertificaat niet wordt vertrouwd.** Een verbinding is tot stand gebracht met de server, maar vervolgens is een fout opgetreden tijdens het aanmelden. (provider: SSL-Provider, fout: 0 - de certificaatketen is uitgegeven door een instantie die wordt niet vertrouwd.) | Deze fout treedt op als het certificaat dat wordt gebruikt niet-vertrouwd. Om het probleem op te lossen, moet u een certificaat dat kan worden vertrouwd en daarna inschakelen op de server zoeken. U kunt ook kunt u de optie vertrouwd certificaat tijdens het verbinding maken. Deze maatregelen alleen als u bekend met het certificaat dat wordt gebruikt bent en u deze vertrouwt. <br> SSL-verbindingen die zijn versleuteld met behulp van een zelfondertekend certificaat geen sterke beveiliging bieden--ze zijn vatbaar voor man-in-the-middle-aanvallen. Vertrouw niet op SSL met behulp van zelfondertekende certificaten in een productieomgeving of op servers die zijn verbonden met internet. <br> Zie voor meer informatie, [met behulp van SSL met een Microsoft SQL Server-exemplaar met DB](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/SQLServer.Concepts.General.SSL.Using.html) of [zelfstudie: SQL-Server voor extern bureaublad-services migreren naar Azure met behulp van DMS](https://docs.microsoft.com/azure/dms/tutorial-rds-sql-server-azure-sql-and-managed-instance-online#prerequisites). |
| **Fout bij 300** -gebruiker beschikt niet over de vereiste machtigingen. VIEW SERVER STATE machtiging is geweigerd voor object {server}, database {database} | Deze fout treedt op als de gebruiker heeft geen toegangsmachtiging voor de migratie uit te voeren. Om het probleem op te lossen, raadpleegt u [machtigingen verlenen Server - Transact-SQL](https://docs.microsoft.com/sql/t-sql/statements/grant-server-permissions-transact-sql?view=sql-server-2017) of [zelfstudie: SQL-Server voor extern bureaublad-services migreren naar Azure met behulp van DMS](https://docs.microsoft.com/azure/dms/tutorial-rds-sql-server-azure-sql-and-managed-instance-online#prerequisites) voor meer informatie. |

> [!NOTE]
> Zie voor meer informatie over het oplossen van problemen met betrekking tot het maken van een verbinding met een AWS RDS SQL Server-bron met de volgende bronnen:
>
> * [Solving Connectivity errors to SQL Server](https://support.microsoft.com/help/4009936/solving-connectivity-errors-to-sql-server) (Verbindingsproblemen met SQL Server oplossen)
> * [Hoe los ik problemen met verbinding maken met mijn database-instantie van Amazon extern bureaublad-services?](https://aws.amazon.com/premiumsupport/knowledge-center/rds-cannot-connect)

## <a name="known-issues"></a>Bekende problemen

* [Bekende problemen/migratiebeperkingen met online migratie naar Azure SQL Database](https://docs.microsoft.com/azure/dms/known-issues-azure-sql-online)
* [Bekende problemen/migratiebeperkingen met online migratie naar Azure Database for MySQL](https://docs.microsoft.com/azure/dms/known-issues-azure-mysql-online)
* [Bekende problemen/migratiebeperkingen met online migratie naar Azure Database for PostgreSQL](https://docs.microsoft.com/azure/dms/known-issues-azure-postgresql-online)

## <a name="next-steps"></a>Volgende stappen

* Artikel [Azure Database Migration Service PowerShell](https://docs.microsoft.com/powershell/module/azurerm.datamigration/?view=azurermps-6.13.0#data_migration).
* Artikel [parameters van de server configureren in Azure Database voor MySQL met behulp van de Azure-portal](https://docs.microsoft.com/azure/mysql/howto-server-parameters).
* Artikel [overzicht van vereisten voor het gebruik van Azure Database Migration Service](https://docs.microsoft.com/azure/dms/pre-reqs).
* Zie de [Veelgestelde vragen over het gebruik van Azure Database Migration Service](https://docs.microsoft.com/azure/dms/faq).
