---
title: Overzicht van Azure SQL-databases | Microsoft Docs
description: Op deze pagina vindt u een overzicht van Azure SQL-databases.
services: sql-database
documentationcenter: na
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: single databases
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 11/28/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 145cdc5b686692b44d2c3593a128689a56812610
ms.openlocfilehash: 6d5f4640556f98f9601139c318ffc578e9df2539


---
# <a name="azure-sql-database-overview"></a>Overzicht van Azure SQL-databases
In dit onderwerp vindt u een overzicht van Azure SQL-databases. Zie [Logische servers](sql-database-server-overview.md) voor informatie over logische Azure SQL-servers.

## <a name="what-is-azure-sql-database"></a>Wat is een Azure SQL-database?
Elke database in Azure SQL Database is gekoppeld aan een logische server. De database kan bestaan uit:

- Een zelfstandige database met een [eigen set resources](sql-database-what-is-a-dtu.md#what-are-database-transaction-units-dtus) (DTU's)
- Een onderdeel van een [elastische pool](sql-database-elastic-pool.md) dat [een set resources deelt](sql-database-what-is-a-dtu.md#what-are-elastic-database-transaction-units-edtus) (eDTU's)
- Een onderdeel van een set [uitgeschaalde Shard-databases](sql-database-elastic-scale-introduction.md#horizontal-and-vertical-scaling), welke individuele databases of databases in een groep kunnen zijn
- Een onderdeel van een set databases die deel uitmaken van een [SaaS-ontwerppatroon met meerdere tenants](sql-database-design-patterns-multi-tenancy-saas-applications.md), waarvan de databases individuele databases of een databases in een groep (of beide) kunnen zijn 

## <a name="how-do-i-connect-and-authenticate-to-an-azure-sql-database"></a>Hoe maak ik verbinding en verifieer ik me bij een Azure SQL-database?

- **Verificatie en autorisatie**: Azure SQL Database ondersteunt SQL-verificatie en Azure Active Directory-verificatie (met bepaalde beperkingen - zie [Connect to SQL Database with Azure Active Directory Authentication](sql-database-aad-authentication.md) (Verbinding maken met SQL Database met behulp van Azure Active Directory-verificatie)) voor verificatie. U kunt verbinding maken en uzelf verifiëren bij Azure SQL-databases via de hoofddatabase van de server of rechtstreeks bij een gebruikersdatabase. Zie [Managing Databases and Logins in Azure SQL Database](sql-database-manage-logins.md) (Databases en aanmeldingen beheren in Azure SQL Database) voor meer informatie. Windows-verificatie wordt niet ondersteund. 
- **TDS**: Microsoft Azure SQL Database ondersteunt het TDS-protocol (Tabular Data Stream) clientversie 7.3 of hoger.
- **TCP/IP**: alleen TCP/IP-verbindingen zijn toegestaan.
- **SQL Database-firewall**: ter bescherming van uw gegevens voorkomt een SQL Database-firewall alle toegang tot uw databaseserver of de databases daarvan totdat u opgeeft welke computers zijn gemachtigd. Zie [Firewalls](sql-database-firewall-configure.md)

## <a name="what-collations-are-supported"></a>Welke sorteringen worden ondersteund?
De standaarddatabasesortering die door Microsoft Azure SQL Database wordt gebruikt, is **SQL_LATIN1_GENERAL_CP1_CI_AS**. Hierbij is **LATIN1_GENERAL** Engels (Verenigde Staten). **CP1** is codetabel 1252, **CI** is niet hoofdlettergevoelig en **AS** is accentgevoelig. U kunt de sortering voor V12-databases niet wijzigen. Zie [COLLATE (Transact-SQL)](https://msdn.microsoft.com/library/ms184391.aspx) voor meer informatie over het instellen van de sortering.

## <a name="what-are-the-naming-requirements-for-database-objects"></a>Wat zijn de naamgevingsvereisten voor databaseobjecten?

Namen van nieuwe objecten moeten voldoen aan de SQL Server-regels voor id's. Zie [Id's](https://msdn.microsoft.com/library/ms175874.aspx) voor meer informatie.

## <a name="what-features-are-supported-by-azure-sql-databases"></a>Welke functies worden ondersteund door Azure SQL-databases?

Zie [Functies](sql-database-features.md) voor meer informatie over ondersteunde functies. Zie ook [Azure SQL Database Transact-SQL differences](sql-database-transact-sql-information.md) (Verschillen tussen Azure SQL-database en Transact-SQL) voor meer achtergrondinformatie over de redenen waarom bepaalde soorten functies niet worden ondersteund.

## <a name="how-do-i-manage-an-azure-sql-database"></a>Hoe beheer ik een Azure SQL-database?

U kunt logische Azure SQL Database-servers beheren met behulp van een aantal methoden:
- [Azure Portal](sql-database-manage-portal.md)
- [PowerShell](sql-database-manage-powershell.md)
- [Transact-SQL](sql-database-manage-azure-ssms.md)
- [REST](/rest/api/sql/)

## <a name="next-steps"></a>Volgende stappen

- Zie [Wat is SQL Database?](sql-database-technical-overview.md) voor informatie over de service Azure SQL Database
- Zie [Functies](sql-database-features.md) voor meer informatie over ondersteunde functies
- Zie [Overzicht van logische SQL Database-servers](sql-database-server-overview.md) voor een overzicht van logische Azure SQL-servers
- Zie [Azure SQL Database Transact-SQL differences](sql-database-transact-sql-information.md) (Verschillen tussen Azure SQL-database en Transact-SQL) voor meer informatie over Transact-SQL-ondersteuning en -verschillen.
- Voor informatie over specifieke resourcequota en beperkingen op basis van uw **servicelaag**. Zie [SQL Database-servicelagen](sql-database-service-tiers.md) voor een overzicht van servicelagen.
- Voor beveiligingsrichtlijnen raadpleegt u [Azure SQL Database Security Guidelines and Limitations](sql-database-security-guidelines.md) (Beveiligingsrichtlijnen en -beperkingen voor Azure SQL Database).
- Voor informatie over de beschikbaarheid van stuurprogramma's en ondersteuning voor SQL Database raadpleegt u [Connection Libraries for SQL Database and SQL Server](sql-database-libraries.md) (Verbindingsbibliotheken voor SQL Database en SQL Server).




<!--HONumber=Dec16_HO2-->


