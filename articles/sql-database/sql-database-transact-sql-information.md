---
title: Het omzetten van T-SQL-verschillen-migratie-Azure SQL Database | Microsoft Docs
description: Transact-SQL-instructies die minder dan volledig worden ondersteund in Azure SQL Database
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 12/03/2018
ms.openlocfilehash: ce539da92b9d58282ab44a3729f4bf4fb8eaedf5
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52840330"
---
# <a name="resolving-transact-sql-differences-during-migration-to-sql-database"></a>Transact-SQL-verschillen oplossen tijdens de migratie naar SQL Database

Wanneer [migreren van uw database](sql-database-cloud-migrate.md) van SQL Server naar Azure SQL-Server, ontdekt u mogelijk dat uw database is vereist voor sommige herstructurering vereist voordat de SQL-Server kan worden gemigreerd. Dit artikel bevat richtlijnen om u te helpen u bij het uitvoeren van deze herstructurering zowel inzicht in de onderliggende redenen waarom de herstructurering nodig is. Voor het detecteren van compatibiliteitsproblemen, gebruikt u de [Data Migration Assistant (DMA)](https://www.microsoft.com/download/details.aspx?id=53595).

## <a name="overview"></a>Overzicht

De meeste Transact-SQL-functies die gebruikmaken van toepassingen worden volledig ondersteund in zowel Microsoft SQL Server en Azure SQL Database. Bijvoorbeeld, werken de belangrijkste SQL-onderdelen, zoals gegevenstypen, operators, string, rekenkundige, logische en functies van de cursor, identiek in SQL Server en SQL-Database. Er zijn echter enkele T-SQL-verschillen in DDL (data definition language) en DML (gegevens manipuleren taal) elementen, wat resulteert in T-SQL-instructies en query's die worden slechts gedeeltelijk ondersteund (die wordt besproken hoe verderop in dit artikel).

Bovendien zijn sommige functies en -syntaxis die niet wordt ondersteund op alle omdat Azure SQL Database is ontworpen om functies van afhankelijkheden in de hoofddatabase en het besturingssysteem te isoleren. De meeste op serverniveau activiteiten zijn daarom niet geschikt zijn voor SQL-Database. T-SQL-instructies en opties zijn niet beschikbaar als het niveau van de server opties, besturingssysteemonderdelen, configureren of de bestandssysteemconfiguratie. Wanneer deze mogelijkheden vereist zijn, is vaak een geschikt alternatief beschikbaar zijn in een andere manier van SQL-Database of een andere Azure-functie of -service.

Bijvoorbeeld, hoge beschikbaarheid is ingebouwd in Azure SQL Database met behulp van technologie die vergelijkbaar is met [Always On Availability Groups](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server). T-SQL-instructies met betrekking tot de beschikbaarheidsgroepen worden niet ondersteund door SQL Database en de dynamische beheerweergaven voor Always On Availability Groups worden ook niet ondersteund.

Zie voor een lijst van de functies die worden ondersteund en wordt niet ondersteund door SQL Database, [vergelijking van Azure SQL Database](sql-database-features.md). De lijst op deze pagina is een aanvulling op dat artikel richtlijnen en -functies, en is gericht op Transact-SQL-instructies.

## <a name="transact-sql-syntax-statements-with-partial-differences"></a>Overzichten van de Transact-SQL-syntaxis met gedeeltelijke verschillen

De core (data definition language met) DDL-instructies zijn beschikbaar, maar sommige DDL-componenten beschikken over uitbreidingen die betrekking hebben op de plaatsing van de schijf en niet-ondersteunde functies.

- MAKEN en ALTER DATABASE instructies hebben meer dan drie dozijn opties. De instructies zijn plaatsing van bestanden, FILESTREAM en service broker-opties die alleen van toepassing op SQL Server. Dit kan niet van belang als u databases maken voordat u migreert, maar als u migreert van T-SQL-code die wordt gemaakt van databases Vergelijk [CREATE DATABASE (Azure SQL Database)](https://msdn.microsoft.com/library/dn268335.aspx) met de syntaxis van de SQL Server op [maken DATABASE (SQL Server Transact-SQL)](https://msdn.microsoft.com/library/ms176061.aspx) om ervoor te zorgen dat alle opties die u gebruiken worden ondersteund. DATABASE maken voor Azure SQL Database heeft ook servicedoelstelling en flexibele schaalmogelijkheden opties die alleen van toepassing op SQL-Database.
- Het maken en ALTER TABLE-instructies hebt bestandstabel opties die op de SQL-Database kunnen niet worden gebruikt omdat FILESTREAM wordt niet ondersteund.
- MAKEN en ALTER login-instructies worden ondersteund, maar SQL Database biedt niet alle opties. Als u uw database draagbaarder, raadt SQL-Database met behulp van ingesloten databasegebruikers in plaats van aanmeldingen indien mogelijk. Zie voor meer informatie, [CREATE/ALTER LOGIN](https://msdn.microsoft.com/library/ms189828.aspx) en [beheren en het verlenen van toegang tot de database](https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins).

## <a name="transact-sql-syntax-not-supported-in-azure-sql-database"></a>Niet ondersteund in Azure SQL Database Transact-SQL-syntaxis

Naast de Transact-SQL-instructies met betrekking tot de niet-ondersteunde functies die beschreven worden [vergelijking van Azure SQL Database](sql-database-features.md), de volgende instructies en groepen instructies worden niet ondersteund. Als zodanig, als uw database worden gemigreerd maakt gebruik van de volgende functies, opnieuw engineering toepassen op uw T-SQL om deze T-SQL-functies en instructies te elimineren.

-Sortering van systeem objecten - verbinding gerelateerd: eindpuntinstructies. SQL Database biedt geen ondersteuning voor Windows-verificatie, maar ondersteunt wel de vergelijkbare Azure Active Directory-verificatie. Voor sommige verificatietypen is de nieuwste versie van SSMS vereist. Zie voor meer informatie, [verbinding maken met SQL-Database of SQL Data Warehouse door met behulp van Azure Active Directory-verificatie](sql-database-aad-authentication.md).
-Databaseoverschrijdende query's met drie of vier onderdeelnamen. (Alleen-lezen query's die databaseoverschrijdend zijn worden ondersteund door middel van [elastische databasequery](sql-database-elastic-query-overview.md).) - Cross database chaining-eigendom, `TRUSTWORTHY` instelling - `EXECUTE AS LOGIN` 'Uitvoeren als gebruiker' in plaats daarvan gebruiken.
-Versleuteling wordt ondersteund, met uitzondering van extensible key management - Eventing: gebeurtenissen, meldingen van gebeurtenissen, querymeldingen - bestandsplaatsing: syntaxis die betrekking heeft op databasebestandsplaatsing, grootte en databasebestanden die automatisch worden beheerd door Microsoft Azure.
- Hoge beschikbaarheid: syntaxis die betrekking hebben op hoge beschikbaarheid, die wordt beheerd via uw Microsoft Azure-account. Dit omvat syntaxis voor back-ups, herstellen, Always On, databasespiegeling, de back-upfunctie voor logboekbestanden en herstelmodi.
-Meld u lezer: syntaxis die afhankelijk is van de logboekweergave die niet beschikbaar op de SQL-Database is: Push-replicatie, Change Data Capture. SQL Database kan abonnee zijn op een artikel over push-replicatie.
-Functies: `fn_get_sql`, `fn_virtualfilestats`, `fn_virtualservernodes` -Hardware: syntaxis die betrekking heeft op hardwaregerelateerde serverinstellingen:, zoals geheugen, werkthreads, CPU-affiniteit, trace worden gemarkeerd. Gebruik van service-lagen en compute-grootten in plaats daarvan.
- `KILL STATS JOB`
- `OPENQUERY`, `OPENROWSET`, `OPENDATASOURCE`, en vierdelige namen - .NET Framework: CLR-integratie met de referenties van de SQL Server - functionaliteit voor semantische zoeken - Server: Gebruik [referenties in het databasebereik](https://msdn.microsoft.com/library/mt270260.aspx) in plaats daarvan.
-Items serverniveau: serverrollen, `sys.login_token`. `GRANT`, `REVOKE`, en `DENY` van de machtigingen op serverniveau zijn niet beschikbaar, hoewel enkele hiervan worden vervangen door machtigingen op databaseniveau. Sommige handige DMV’s op serverniveau hebben vergelijkbare DMV’s op databaseniveau.
- `SET REMOTE_PROC_TRANSACTIONS`
- `SHUTDOWN`
- `sp_addmessage`
- `sp_configure` opties en `RECONFIGURE`. Sommige opties zijn beschikbaar met [ALTER DATABASE SCOPED CONFIGURATION](https://msdn.microsoft.com/library/mt629158.aspx).
- `sp_helpuser`
- `sp_migrate_user_to_contained`
- SQL ServerAgent: Syntaxis die afhankelijk van de SQL Server Agent of de MSDB-database is: waarschuwingen, operators, servers voor Centraal beheer. Gebruik in plaats daarvan opties voor scripts, zoals Azure PowerShell.
-SQL Server audit: Gebruik SQL Database auditing in plaats daarvan.
-SQL Server trace - Tracering vlaggen: sommige traceermarkeringsitems zijn verplaatst naar compatibiliteitsmodi.
-Foutopsporing in transact-SQL - Triggers: binnen het serverbereik of aanmeldingstriggers - `USE` instructie: als u de databasecontext wilt een andere database, moet u een nieuwe verbinding met de nieuwe database.

## <a name="full-transact-sql-reference"></a>Volledige naslaginformatie voor Transact-SQL

Zie voor meer informatie over Transact-SQL-grammatica, syntaxis en voorbeelden, [naslaginformatie voor Transact-SQL (Database-Engine)](https://msdn.microsoft.com/library/bb510741.aspx) in SQL Server Books Online.

### <a name="about-the-applies-to-tags"></a>Over het label 'Van toepassing op'

De Transact-SQL-naslaginformatie bevat artikelen die betrekking hebben op versies van SQL Server 2008 bij de huidige. Onder de titel van het artikel is een pictogram balk met pictogrammen de vier SQL Server-platforms en toepasselijkheid waarmee wordt aangegeven. Beschikbaarheidsgroepen zijn bijvoorbeeld geïntroduceerd in SQL Server 2012. De [CREATE AVAILABILITY GROUP](https://msdn.microsoft.com/library/ff878399.aspx) artikel geeft aan dat de instructie is van toepassing op **SQL-Server (vanaf 2012)**. De instructie is niet van toepassing op SQL Server 2008, SQL Server 2008 R2, Azure SQL Database, Azure SQL Data Warehouse en Parallel Data Warehouse.

In sommige gevallen kan het algemene onderwerp van een artikel kan worden gebruikt in een product, maar er zijn kleine verschillen tussen producten. De verschillen worden waar nodig in het artikel waar nodig. In sommige gevallen kan het algemene onderwerp van een artikel kan worden gebruikt in een product, maar er zijn kleine verschillen tussen producten. De verschillen worden waar nodig in het artikel waar nodig. Het artikel CREATE TRIGGER is bijvoorbeeld beschikbaar in SQL-Database. Maar de **alle SERVER** optie voor triggers op serverniveau, geeft aan dat de triggers op serverniveau kunnen niet worden gebruikt in SQL-Database. Gebruik in plaats daarvan op databaseniveau triggers.

## <a name="next-steps"></a>Volgende stappen

Zie voor een lijst van de functies die worden ondersteund en wordt niet ondersteund door SQL Database, [vergelijking van Azure SQL Database](sql-database-features.md). De lijst op deze pagina is een aanvulling op dat artikel richtlijnen en -functies, en is gericht op Transact-SQL-instructies.
