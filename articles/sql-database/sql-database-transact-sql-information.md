---
title: Het omzetten van T-SQL verschillen-migratie-Azure SQL Database | Microsoft Docs
description: Transact-SQL-instructies die minder dan volledig worden ondersteund in Azure SQL Database
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
tags: 
ms.assetid: c05abd9e-28a7-4c97-9bdf-bc60d08fc92e
ms.service: sql-database
ms.custom: migrate
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: On Demand
ms.date: 10/23/2017
ms.author: carlrab
ms.openlocfilehash: f311c0d139d5ec35cbd85a34bd5a5e991bccba3a
ms.sourcegitcommit: 71fa59e97b01b65f25bcae318d834358fea5224a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2018
---
# <a name="resolving-transact-sql-differences-during-migration-to-sql-database"></a>Transact-SQL-verschillen tijdens de migratie met SQL Database oplossen   
Wanneer [migreren van uw database](sql-database-cloud-migrate.md) van SQL Server naar Azure SQL-Server, ontdekt u mogelijk de database moeten sommige herstructureren voordat de SQL-Server kan worden gemigreerd. In dit artikel bevat richtlijnen om u te helpen zowel uitvoeren van deze herstructureren en kennis van de onderliggende redenen waarom de herstructureren nodig is. Om te detecteren compatibiliteitsproblemen, gebruiken de [gegevens migratie-assistent (DMA)](https://www.microsoft.com/download/details.aspx?id=53595).

## <a name="overview"></a>Overzicht
De meeste Transact-SQL-functies die gebruikmaken van toepassingen worden volledig ondersteund in Microsoft SQL Server en Azure SQL Database. Bijvoorbeeld, werken de kernonderdelen van SQL zoals gegevenstypen, operators, string, rekenkundig logische, en functies van de cursor, identiek in SQL Server en SQL-Database. Er zijn echter enkele TSQL-verschillen in DDL (data definition language) en (gegevens manipulatie language) van de DML-elementen waardoor T-SQL-instructies en query's die worden slechts gedeeltelijk ondersteund (die bespreken we verderop in dit artikel).

Bovendien zijn sommige functies en de syntaxis die niet wordt ondersteund op alle omdat Azure SQL Database is ontworpen voor het isoleren van de functies van afhankelijkheden van de database master en het besturingssysteem. De meeste serverniveau activiteiten zijn als zodanig niet geschikt voor SQL-Database. Opties en T-SQL-instructies zijn niet beschikbaar als ze serverniveau opties, de onderdelen van het besturingssysteem configureren, of geef configuratie bestandssysteem. Als er dergelijke mogelijkheden zijn vereist, is vaak een geschikt alternatief beschikbaar zijn in een andere manier van SQL-Database of van een andere Azure-functie of service. 

Hoge beschikbaarheid is bijvoorbeeld ingebouwd in Azure, dus configureren AlwaysOn niet nodig is (Hoewel u configureren van actieve geo-replicatie voor sneller herstel in geval van een ramp wilt). Dus T-SQL-instructies die betrekking hebben op beschikbaarheidsgroepen worden niet ondersteund door SQL-Database en de dynamische Beheerweergave weergaven voor Always On worden ook niet ondersteund.

Zie voor een lijst van de functies die worden ondersteund en worden niet ondersteund door SQL-Database [vergelijking van functies van Azure SQL Database](sql-database-features.md). De lijst op deze pagina is een aanvulling op dat artikel richtlijnen en functies en legt de nadruk op Transact-SQL-instructies.

## <a name="transact-sql-syntax-statements-with-partial-differences"></a>Transact-SQL-syntaxis instructies met gedeeltelijke verschillen
De core DDL (data definition language)-instructies zijn beschikbaar, maar sommige DDL-componenten extensies zijn gerelateerd aan de plaatsing van de schijf en niet-ondersteunde functies. 

- MAKEN en ALTER DATABASE instructies hebben meer dan drie dozijn opties. De instructies zijn plaatsing van bestanden, FILESTREAM en opties van service broker voor die alleen van toepassing op SQL Server. Dit mogelijk niet van belang als u databases maakt voordat u migreert, maar als u migreert T-SQL-code die databases maakt moet u vergelijken [CREATE DATABASE (Azure SQL Database)](https://msdn.microsoft.com/library/dn268335.aspx) met de syntaxis van de SQL Server op [maken DATABASE (SQL Server Transact-SQL)](https://msdn.microsoft.com/library/ms176061.aspx) om ervoor te zorgen dat alle opties die u gebruikt, worden ondersteund. CREATE DATABASE voor Azure SQL Database beschikt ook over servicedoelstelling en elastisch schalingsopties die alleen van toepassing op SQL-Database.
- Het maken en ALTER TABLE-instructies hebben bestandstabel opties op de SQL-Database kunnen niet worden gebruikt omdat FILESTREAM wordt niet ondersteund.
- MAKEN en ALTER login-instructies worden ondersteund, maar SQL-Database biedt niet alle opties. Als u uw database meer draagbare, raadt SQL-Database met ingesloten databasegebruikers in plaats van aanmeldingen indien mogelijk. Zie voor meer informatie [CREATE/ALTER LOGIN](https://msdn.microsoft.com/library/ms189828.aspx) en [beheren en het verlenen van toegang tot de database](https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins).

## <a name="transact-sql-syntax-not-supported-in-azure-sql-database"></a>Niet ondersteund in Azure SQL Database Transact-SQL-syntaxis   
Naast de Transact-SQL-instructies die betrekking hebben op de niet-ondersteunde functies beschreven in [vergelijking van functies van Azure SQL Database](sql-database-features.md), de volgende instructies en groepen van-instructies worden niet ondersteund. Als zodanig als de database worden gemigreerd van de volgende functies, opnieuw engineering toepassen op uw T-SQL om te voorkomen van deze functies van T-SQL en instructies.

- Systeemobjecten sorteren
- Verbinding gerelateerde: Endpoint-instructies. SQL Database biedt geen ondersteuning voor Windows-verificatie, maar ondersteunt wel de vergelijkbare Azure Active Directory-verificatie. Voor sommige verificatietypen is de nieuwste versie van SSMS vereist. Zie [Verbinding maken met SQL Database of SQL Data Warehouse met behulp van Azure Active Directory-verificatie](sql-database-aad-authentication.md) voor meer informatie.
- Databaseoverschrijdende query’s met drie of vier onderdeelnamen. (Alleen-lezen query’s die databaseoverschrijdend zijn worden ondersteund dankzij [elastische databasequery’s](sql-database-elastic-query-overview.md).)
- Databaseoverschrijdend het eigendom koppelen, instelling `TRUSTWORTHY`
- `EXECUTE AS LOGIN` Gebruik in plaats daarvan 'EXECUTE AS USER'.
- Versleuteling wordt ondersteund, behalve voor Extensible Key Management
- Eventing: Gebeurtenissen, meldingen van gebeurtenissen, querymeldingen
- Plaatsing van bestanden: syntaxis betrekking hebben op de plaatsing van de database-bestanden, grootte en databasebestanden die automatisch worden beheerd door Microsoft Azure.
- Hoge beschikbaarheid: syntaxis verband houden met hoge beschikbaarheid, die wordt beheerd via uw Microsoft Azure-account. Dit omvat syntaxis voor back-ups, herstellen, Always On, databasespiegeling, de back-upfunctie voor logboekbestanden en herstelmodi.
- Meld u lezer: syntaxis die is afhankelijk van de logboekweergave die niet beschikbaar op SQL-Database: Push-replicatie, Change Data Capture. SQL Database kan abonnee zijn op een artikel over push-replicatie.
- Functies: `fn_get_sql`, `fn_virtualfilestats`, `fn_virtualservernodes`
- Hardware: Syntaxis betrekking hebben op hardware-gerelateerde serverinstellingen: zoals geheugen, werkthreads, CPU-affiniteit, trace vlaggen. Gebruik in plaats daarvan serviceniveaus.
- `KILL STATS JOB`
- `OPENQUERY`, `OPENROWSET`, `OPENDATASOURCE`, en vierdelige namen
- .NET framework: CLR-integratie met SQL Server
- Semantische zoekopdrachten
- Referenties voor server: Gebruik [database binnen het bereik van referenties](https://msdn.microsoft.com/library/mt270260.aspx) in plaats daarvan.
- Niveau van de server items: serverrollen, `sys.login_token`. `GRANT`, `REVOKE`, en `DENY` van de machtigingen op serverniveau zijn niet beschikbaar, hoewel enkele hiervan worden vervangen door machtigingen op databaseniveau. Sommige handige DMV’s op serverniveau hebben vergelijkbare DMV’s op databaseniveau.
- `SET REMOTE_PROC_TRANSACTIONS`
- `SHUTDOWN`
- `sp_addmessage`
- `sp_configure`-opties en `RECONFIGURE`. Sommige opties zijn beschikbaar met [ALTER DATABASE SCOPED CONFIGURATION](https://msdn.microsoft.com/library/mt629158.aspx).
- `sp_helpuser`
- `sp_migrate_user_to_contained`
- SQL ServerAgent: Syntaxis die is afhankelijk van de SQL Server Agent of de MSDB-database: waarschuwingen, operators, Centraal beheer-servers. Gebruik in plaats daarvan opties voor scripts, zoals Azure PowerShell.
- SQL Server audit: Gebruik SQL Database auditing in plaats daarvan.
- SQL Server-tracering
- Trace-vlaggen: sommige items trace-vlag zijn verplaatst naar de compatibiliteitsmodus.
- Transact-SQL-foutopsporing
- Triggers: triggers binnen het serverbereik of aanmeldingstriggers
- `USE`-instructie: als u de databasecontext wilt wijzigen naar een andere database, moet u een nieuwe verbinding maken met de nieuwe database.

## <a name="full-transact-sql-reference"></a>Volledige naslaginformatie voor Transact-SQL
Zie [Naslaginformatie voor Transact-SQL (database-engine)](https://msdn.microsoft.com/library/bb510741.aspx) in SQL Server Books Online voor meer informatie over Transact-SQL-grammatica, -gebruik en -voorbeelden. 

### <a name="about-the-applies-to-tags"></a>Over het label 'Van toepassing op'
De Transact-SQL-verwijzing bevat artikelen die betrekking hebben op versies van SQL Server 2008 bij de huidige. Onder de titel van het artikel is een pictogram lijst klikt, worden de vier SQL Server-platforms en toepasselijkheid waarmee wordt aangegeven. Beschikbaarheidsgroepen zijn bijvoorbeeld geïntroduceerd in SQL Server 2012. De [AVAILABILTY groep maken](https://msdn.microsoft.com/library/ff878399.aspx) artikel geeft aan dat de instructie is van toepassing op **SQL-Server (te beginnen met 2012)**. De instructie is niet van toepassing op SQL Server 2008, SQL Server 2008 R2, Azure SQL Database, Azure SQL Data Warehouse en Parallel Data Warehouse.

In sommige gevallen kan een algemeen onderwerp van een artikel kan worden gebruikt in een product, maar er zijn kleine verschillen tussen producten. De verschillen worden aangeduid op middelpunten in het artikel naar gelang van toepassing. In sommige gevallen kan een algemeen onderwerp van een artikel kan worden gebruikt in een product, maar er zijn kleine verschillen tussen producten. De verschillen worden aangeduid op middelpunten in het artikel naar gelang van toepassing. Het artikel CREATE TRIGGER is bijvoorbeeld beschikbaar in SQL-Database. Maar de **alle SERVER** optie voor het niveau van de server-triggers, geeft aan dat het niveau van de server-triggers kunnen niet worden gebruikt in SQL-Database. Gebruik in plaats daarvan databaseniveau triggers.

## <a name="next-steps"></a>Volgende stappen

Zie voor een lijst van de functies die worden ondersteund en worden niet ondersteund door SQL-Database [vergelijking van functies van Azure SQL Database](sql-database-features.md). De lijst op deze pagina is een aanvulling op dat artikel richtlijnen en functies en legt de nadruk op Transact-SQL-instructies.

