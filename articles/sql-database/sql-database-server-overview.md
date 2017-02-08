---
title: Overzicht van logische servers met Azure SQL Database | Microsoft Docs
description: Op deze pagina vindt u overwegingen en richtlijnen voor het werken met logische Azure SQL-servers.
services: sql-database
documentationcenter: na
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: servers
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 02/01/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: e5b5751facb68ae4a62e3071fe4dfefc02434a9f
ms.openlocfilehash: 17e2830dceeaa313dd0fd7d406bf68a75b6f900e


---
# <a name="azure-sql-database-logical-servers"></a>Logische servers met Azure SQL Database

In dit onderwerp vindt u overwegingen en richtlijnen voor het werken met logische Azure SQL-servers. Zie [SQL-databases](sql-database-overview.md) voor meer informatie over Azure SQL-databases.

## <a name="what-is-an-azure-sql-database-logical-server"></a>Wat is een logische server met Azure SQL Database?
Een logische server met Azure SQL Database fungeert als een centraal beheerpunt voor meerdere databases. In SQL Database is een server een logische constructie die zich onderscheidt van een SQL Server-exemplaar dat u mogelijk kent vanuit de on-premises wereld. Zo geeft de service SQL Database geen garanties met betrekking tot de locatie van de databases ten opzichte van hun logische servers en zijn er geen toegang of functies op exemplaarniveau zichtbaar. Zie [Logische servers](sql-database-server-overview.md) voor meer informatie over logische Azure SQL-servers. 

Een logische server met Azure Database:

- Wordt gemaakt binnen een Azure-abonnement, maar kan met ingesloten resources naar een ander abonnement worden verplaatst
- Is de bovenliggende resource voor databases, elastische groepen en datawarehouses
- Biedt een naamruimte voor databases, elastische groepen en datawarehouses
- Is een logische container met sterke levensduursemantiek; d.w.z. als een server wordt verwijderd, worden ook de ingesloten databases, elastische groepen en datawarehouses verwijderd
- Maakt deel uit van het toegangsbeheer op basis van rollen (RBAC) van Azure; databases, elastische groepen binnen een server nemen toegangsrechten over van de server
- Is met betrekking tot het Azure-resourcebeheer (zie het URL-schema voor databases en groepen) een hoger element van de identiteit van databases en elastische groepen
- Groepeert resources in een regio
- Biedt een verbindingseindpunt voor databasetoegang (<serverName>.database.windows.net)
- Biedt toegang tot metagegevens van ingesloten resources via DMV's door verbinding te maken met een hoofddatabase 
- Levert de beheerbeleidsregels die van toepassing zijn op de betreffende databases: aanmeldingen, firewall, controle, detectie van bedreigingen, enzovoort 
- Wordt beperkt door een quotum binnen het bovenliggende abonnement (zes servers per abonnement - [bekijk hier de limieten van abonnementen](../azure-subscription-service-limits.md))
- Levert het databasequotum en DTU-quotum voor de resources die deze bevat (zoals 45.000 DTU in V12)
- Is het versiebeheerbereik voor de mogelijkheden die zijn ingeschakeld op ingesloten resources (de meest recente versie is V12)
- Hoofdaanmeldingen op serverniveau kunnen alle databases op een server beheren
- Kunnen aanmeldingen bevatten die vergelijkbaar zijn met die in de exemplaren van SQL Server op uw locaties die toegang hebben tot een of meer databases op de server, en waaraan beperkte beheerrechten kunnen worden verleend. Zie [Aanmeldingen](sql-database-manage-logins.md) voor meer informatie.

## <a name="how-do-i-connect-and-authenticate-to-an-azure-sql-database-logical-server"></a>Hoe maak ik verbinding en verifieer ik me bij een logische server met Azure SQL Database?

- **Verificatie en autorisatie**: Azure SQL Database ondersteunt SQL-verificatie en Azure Active Directory-verificatie (met bepaalde beperkingen - zie [Connect to SQL Database with Azure Active Directory Authentication](sql-database-aad-authentication.md) (Verbinding maken met SQL Database met behulp van Azure Active Directory-verificatie)) voor verificatie. U kunt verbinding maken en uzelf verifiëren bij Azure SQL-databases via de hoofddatabase van de server of rechtstreeks bij een gebruikersdatabase. Zie [Managing Databases and Logins in Azure SQL Database](sql-database-manage-logins.md) (Databases en aanmeldingen beheren in Azure SQL Database) voor meer informatie. Windows-verificatie wordt niet ondersteund. 
- **TDS**: Microsoft Azure SQL Database ondersteunt het TDS-protocol (Tabular Data Stream) clientversie 7.3 of hoger.
- **TCP/IP**: alleen TCP/IP-verbindingen zijn toegestaan.
- **SQL Database-firewall**: ter bescherming van uw gegevens voorkomt een SQL Database-firewall alle toegang tot uw databaseserver of de databases daarvan totdat u opgeeft welke computers zijn gemachtigd. Zie [Firewalls](sql-database-firewall-configure.md)

## <a name="what-collations-are-supported"></a>Welke sorteringen worden ondersteund?

De standaarddatabasesortering die door Microsoft Azure SQL Database (inclusief de hoofddatabase) wordt gebruikt, is **SQL_LATIN1_GENERAL_CP1_CI_AS**. Hierbij is **LATIN1_GENERAL** Engels (Verenigde Staten). **CP1** is codetabel 1252, **CI** is niet hoofdlettergevoelig en **AS** is accentgevoelig. Het wordt niet aangeraden om de sortering voor V12-databases te wijzigen nadat ze zijn gemaakt. Zie [COLLATE (Transact-SQL)](https://msdn.microsoft.com/library/ms184391.aspx) voor meer informatie over sorteringen.

## <a name="what-are-the-naming-requirements-for-database-objects"></a>Wat zijn de naamgevingsvereisten voor databaseobjecten?

Namen van nieuwe objecten moeten voldoen aan de SQL Server-regels voor id's. Zie [Id's](https://msdn.microsoft.com/library/ms175874.aspx) voor meer informatie.

## <a name="what-features-are-supported"></a>Welke functies worden ondersteund?

Zie [Functies](sql-database-features.md) voor meer informatie over ondersteunde functies. Zie ook [Azure SQL Database Transact-SQL differences](sql-database-transact-sql-information.md) (Verschillen tussen Azure SQL-database en Transact-SQL) voor meer achtergrondinformatie over de redenen waarom bepaalde soorten functies niet worden ondersteund.

## <a name="how-do-i-manage-a-logical-server"></a>Hoe beheer ik een logische server?

U kunt logische Azure SQL Database-servers beheren met behulp van een aantal methoden:
- [Azure Portal](sql-database-manage-portal.md)
- [PowerShell](sql-database-manage-powershell.md)
- [REST](/rest/api/sql/)

## <a name="next-steps"></a>Volgende stappen

- Zie [Wat is SQL Database?](sql-database-technical-overview.md) voor informatie over de service Azure SQL Database
- Zie [Functies](sql-database-features.md) voor meer informatie over ondersteunde functies
- Zie [Overzicht van SQL Database](sql-database-overview.md) voor een overzicht van Azure SQL-databases
- Zie [Azure SQL Database Transact-SQL differences](sql-database-transact-sql-information.md) (Verschillen tussen Azure SQL-database en Transact-SQL) voor meer informatie over Transact-SQL-ondersteuning en -verschillen.
- Voor informatie over specifieke resourcequota en beperkingen op basis van uw **servicelaag**. Zie [SQL Database-servicelagen](sql-database-service-tiers.md) voor een overzicht van servicelagen.
- Zie [Azure SQL Database Security Overview](sql-database-security-overview.md) (Overzicht van Azure SQL Database-beveiliging) voor een beveiligingsoverzicht.
- Voor informatie over de beschikbaarheid van stuurprogramma's en ondersteuning voor SQL Database raadpleegt u [Connection Libraries for SQL Database and SQL Server](sql-database-libraries.md) (Verbindingsbibliotheken voor SQL Database en SQL Server).




<!--HONumber=Dec16_HO4-->


