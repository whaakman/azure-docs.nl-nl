---
title: Niet-ondersteund in Azure SQL Database T-SQL | Microsoft Docs
description: Transact-SQL-instructies die minder dan volledig worden ondersteund in Azure SQL Database
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: 
tags: 
ms.assetid: c05abd9e-28a7-4c97-9bdf-bc60d08fc92e
ms.service: sql-database
ms.custom: overview
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 11/28/2016
ms.author: rickbyh
translationtype: Human Translation
ms.sourcegitcommit: 7db5939c88045ab59d7d9fb4b58f07374d7bca01
ms.openlocfilehash: ebec6ca87cfd5e9d2c4ea726d5d31d20998ef33f


---
# <a name="azure-sql-database-transact-sql-differences"></a>Verschillen tussen Azure SQL-database en Transact-SQL   
De meeste Transact-SQL-functies waar toepassingen afhankelijk van zijn, worden ondersteund in zowel Microsoft SQL Server als Azure SQL Database. De belangrijkste SQL-onderdelen, zoals gegevenstypen, operators, tekenreeksen en rekenkundige, logische en cursorfuncties, etc., werken hetzelfde als bij SQL Server.

## <a name="why-some-transact-sql-is-not-supported"></a>Waarom bepaalde Transact-SQL niet wordt ondersteund
Azure SQL Database is ontworpen om functies te isoleren van afhankelijkheden in de hoofddatabase en het besturingssysteem. Als gevolg hiervan zijn veel activiteiten op serverniveau niet geschikt voor SQL Database. Transact-SQL-instructies zijn meestal niet beschikbaar als hiermee opties op serverniveau of besturingssysteemonderdelen worden geconfigureerd of als hiermee de bestandssysteemconfiguratie wordt gespecificeerd. Als er functies buiten de gebruikersdatabase nodig zijn, is er vaak een geschikt alternatief beschikbaar via SQL Database of een andere Azure-functie of -service. 

Always On wordt bijvoorbeeld vervangen door actieve geo-replicatie. Om die reden worden alle Transact-SQL-instructies die zijn gerelateerd aan beschikbaarheidsgroepen niet ondersteund door SQL Database en worden de dynamische beheerweergaven voor Always On ook niet ondersteund.  

Zie [Azure SQL Database-overwegingen, -richtlijnen en -functies](sql-database-features.md) voor een lijst met functies die wel en niet worden ondersteund door SQL Database.

De syntaxis die is afgeschaft in SQL Server, wordt over het algemeen ook niet meer ondersteund in SQL Database.

## <a name="transact-sql-syntax-partially-supported-in-sql-database"></a>Transact-SQL-syntaxis die gedeeltelijk wordt ondersteund in SQL Database
SQL Database biedt ondersteuning voor sommige, maar niet alle argumenten die bestaan in de bijbehorende SQL Server 2016 Transact-SQL-instructies. De instructie `CREATE PROCEDURE` is bijvoorbeeld beschikbaar, maar niet alle opties van `CREATE PROCEDURE` zijn beschikbaar. Het zou verwarrend en overbodig zijn om hier de volledige syntaxis te beschrijven. Zie de gerelateerde syntaxisonderwerpen voor meer informatie over de ondersteunde onderdelen van elke instructie.

- Databases: [CREATE](https://msdn.microsoft.com/library/dn268335.aspx)/[ALTER DATABASE](https://msdn.microsoft.com/library/ms174269.aspx)   
- Functies: [CREATE](https://msdn.microsoft.com/library/ms186755.aspx)/[ALTER FUNCTION](https://msdn.microsoft.com/library/ms186967.aspx)   
- Aanmeldingen: [CREATE](https://msdn.microsoft.com/library/ms189751.aspx)/[ALTER LOGIN](https://msdn.microsoft.com/library/ms189828.aspx)   
- Opgeslagen procedures: [CREATE](https://msdn.microsoft.com/library/ms187926.aspx)/[ALTER PROCEDURE](https://msdn.microsoft.com/library/ms189762.aspx)   
- Tabellen: [CREATE](https://msdn.microsoft.com/library/dn305849.aspx)/[ALTER TABLE](https://msdn.microsoft.com/library/ms190273.aspx)   
- Typen (aangepast): [CREATE TYPE](https://msdn.microsoft.com/library/ms175007.aspx)   
- Gebruikers: [CREATE](https://msdn.microsoft.com/library/ms173463.aspx)/[ALTER USER](https://msdn.microsoft.com/library/ms176060.aspx)   
- Weergaven: [CREATE](https://msdn.microsoft.com/library/ms187956.aspx)/[ALTER VIEW](https://msdn.microsoft.com/library/ms173846.aspx)   

## <a name="transact-sql-syntax-not-supported-in-sql-database"></a>De Transact-SQL-syntaxis wordt niet ondersteund in SQL Database   
Naast de Transact-SQL-instructies die betrekking hebben op de niet-ondersteunde functies die beschreven worden in [Azure SQL Database-overwegingen, -richtlijnen en -functies](sql-database-features.md), worden ook de volgende instructies en groepen instructies niet ondersteund.
- Systeemobjecten sorteren
- Aan verbindingen gerelateerd: eindpuntinstructies, `ORIGINAL_DB_NAME`. SQL Database biedt geen ondersteuning voor Windows-verificatie, maar ondersteunt wel de vergelijkbare Azure Active Directory-verificatie. Voor sommige verificatietypen is de nieuwste versie van SSMS vereist. Zie [Verbinding maken met SQL Database of SQL Data Warehouse met behulp van Azure Active Directory-verificatie](sql-database-aad-authentication.md) voor meer informatie.
- Databaseoverschrijdende query’s met drie of vier onderdeelnamen. (Alleen-lezen query’s die databaseoverschrijdend zijn worden ondersteund dankzij [elastische databasequery’s](sql-database-elastic-query-overview.md).)
- Databaseoverschrijdend het eigendom koppelen, instelling `TRUSTWORTHY`
- `DATABASEPROPERTY` Gebruik in plaats daarvan `DATABASEPROPERTYEX`.
- `EXECUTE AS LOGIN` Gebruik in plaats daarvan 'EXECUTE AS USER'.
- Versleuteling wordt ondersteund, behalve voor Extensible Key Management
- Eventing: gebeurtenissen, meldingen van gebeurtenissen, querymeldingen
- Syntaxis die betrekking heeft op databasebestandsplaatsing, grootte en databasebestanden die automatisch worden beheer door Microsoft Azure.
- Syntaxis die betrekking heeft op hoge beschikbaarheid die wordt beheerd via uw Microsoft Azure-account. Dit omvat syntaxis voor back-ups, herstellen, Always On, databasespiegeling, de back-upfunctie voor logboekbestanden en herstelmodi.
- Syntaxis die afhankelijk is van de logboekweergave die niet beschikbaar is in SQL Database: push-replicatie, gegevensregistratie wijzigen. SQL Database kan abonnee zijn op een artikel over push-replicatie.
- Syntaxis die afhankelijk is van de SQL Server Agent of de MSDB-database: waarschuwingen, operators, servers voor centraal beheer. Gebruik in plaats daarvan opties voor scripts, zoals Azure PowerShell.
- Functies: `fn_get_sql`, `fn_virtualfilestats`, `fn_virtualservernodes`
- Algemene tijdelijke tabellen
- Syntaxis die betrekking heeft op hardwaregerelateerde serverinstellingen: geheugen, werkthreads, CPU-affiniteit, traceermarkeringen, etc. Gebruik in plaats daarvan serviceniveaus.
- `HAS_DBACCESS`
- `KILL STATS JOB`
- `OPENQUERY`, `OPENROWSET`, `OPENDATASOURCE`, `BULK INSERT` en vierdelige namen
- .NET Framework [CLR-integratie met SQL Server](http://msdn.microsoft.com/library/ms254963.aspx)
- Semantische zoekopdrachten
- Serverreferenties. Gebruik in plaats daarvan referenties in het databasebereik.
- Items op serverniveau: serverrollen, `IS_SRVROLEMEMBER`, `sys.login_token`. `GRANT`, `REVOKE`, en `DENY` van de machtigingen op serverniveau zijn niet beschikbaar, hoewel enkele hiervan worden vervangen door machtigingen op databaseniveau. Sommige handige DMV’s op serverniveau hebben vergelijkbare DMV’s op databaseniveau.
- `SET REMOTE_PROC_TRANSACTIONS`
- `SHUTDOWN`
- `sp_addmessage`
- `sp_configure`-opties en `RECONFIGURE`. Sommige opties zijn beschikbaar met [ALTER DATABASE SCOPED CONFIGURATION](https://msdn.microsoft.com/library/mt629158.aspx).
- `sp_helpuser`
- `sp_migrate_user_to_contained`
- SQL Server-controle. Gebruik in plaats daarvan SQL Database-controles.
- SQL Server-tracering
- Traceermarkeringen. Sommige traceermarkeringsitems zijn verplaatst naar compatibiliteitsmodi.
- Transact-SQL-foutopsporing
- Triggers: triggers binnen het serverbereik of aanmeldingstriggers
- `USE`-instructie: als u de databasecontext wilt wijzigen naar een andere database, moet u een nieuwe verbinding maken met de nieuwe database.

## <a name="full-transact-sql-reference"></a>Volledige naslaginformatie voor Transact-SQL
Zie [Naslaginformatie voor Transact-SQL (database-engine)](https://msdn.microsoft.com/library/bb510741.aspx) in SQL Server Books Online voor meer informatie over Transact-SQL-grammatica, -gebruik en -voorbeelden. 

### <a name="about-the-applies-to-tags"></a>Over het label 'Van toepassing op'
De Transact-SQL-naslaginformatie bevat onderwerpen over SQL Server 2008 en hoger. Onder de titel van het onderwerp staat een balk met pictogrammen voor de vier SQL Server-platforms. Er staat ook welke platforms van toepassing zijn. Beschikbaarheidsgroepen zijn bijvoorbeeld geïntroduceerd in SQL Server 2012. In het onderwerp [BESCHIKBAARHEIDSGROEP MAKEN](https://msdn.microsoft.com/library/ff878399.aspx) staat dat de instructie van toepassing is op **SQL Server (vanaf 2012)**. De instructie is niet van toepassing op SQL Server 2008, SQL Server 2008 R2, Azure SQL Database, Azure SQL Data Warehouse en Parallel Data Warehouse.

In sommige gevallen kan het algemene onderwerp van een onderwerp in een product worden gebruikt, maar er zijn kleine verschillen tussen producten. De verschillen worden waar nodig in de onderwerpen beschreven.



<!--HONumber=Dec16_HO3-->


