---
title: Een database in SQL Data Warehouse beveiligen | Microsoft Docs
description: Tips voor het beveiligen van een database in Azure SQL Data Warehouse om oplossingen te ontwikkelen.
services: sql-data-warehouse
author: KavithaJonnakuti
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: implement
ms.date: 04/17/2018
ms.author: kavithaj
ms.reviewer: igorstan
ms.openlocfilehash: c3844d378b44d292b9a7eb631fa896d5f6e61dbe
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55472181"
---
# <a name="secure-a-database-in-sql-data-warehouse"></a>Een database in SQL Data Warehouse beveiligen
> [!div class="op_single_selector"]
> * [Beveiligingsoverzicht](sql-data-warehouse-overview-manage-security.md)
> * [Verificatie](sql-data-warehouse-authentication.md)
> * [Versleuteling (Portal)](sql-data-warehouse-encryption-tde.md)
> * [Encryption (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

In dit artikel worden de basisbeginselen besproken van het beveiligen van uw Azure SQL Data Warehouse-database. In het bijzonder, in dit artikel krijgt u aan de slag met resources voor het beperken van toegang, beveiligen van gegevens en bewakingsactiviteiten in een database.

## <a name="connection-security"></a>Verbindingsbeveiliging
Verbindingsbeveiliging verwijst naar de manier waarop u verbindingen met uw database beperkt en beveiligt met behulp van firewallregels en verbindingsversleuteling.

Firewallregels worden zowel door de server als de database gebruikt om verbindingspogingen van IP-adressen die niet expliciet zijn goedgekeurd te weigeren. Als u wilt toestaan verbindingen van uw toepassing of het openbare IP-adres van client-computer, moet u eerst een firewallregel op serverniveau-regel met de Azure portal, REST-API of PowerShell maken. Als best practice moet u het IP-adresbereik dat is toegestaan door uw serverfirewall zoveel mogelijk beperken.  Voor toegang tot Azure SQL Data Warehouse vanaf uw lokale computer, zorg ervoor dat de firewall op uw netwerk en de lokale computer uitgaande communicatie op TCP-poort 1433 toestaat.  

SQL Data Warehouse maakt gebruik van firewallregels op serverniveau. Firewallregels op databaseniveau worden niet ondersteund. Zie voor meer informatie, [Azure SQL Database-firewall][Azure SQL Database firewall], [sp_set_firewall_rule][sp_set_firewall_rule].

Verbindingen met uw SQL Data Warehouse worden standaard versleuteld.  Verbindingsinstellingen aanpassen om versleuteling te schakelen worden genegeerd.

## <a name="authentication"></a>Verificatie
Verificatie verwijst naar hoe u uw identiteit bewijst bij het maken van verbinding met de database. SQL Data Warehouse biedt momenteel ondersteuning voor SQL Server-verificatie met een gebruikersnaam en wachtwoord, en met Azure Active Directory. 

Wanneer u de logische server voor uw database hebt gemaakt, hebt u een aanmelding 'serverbeheerder' opgegeven met een gebruikersnaam en wachtwoord. Met deze referenties, kunt u verifiëren met elke database op die server als de database-eigenaar of 'dbo' via SQL Server-verificatie.

Echter, als een best practice, gebruikers van uw organisatie moeten een ander account gebruiken om te verifiëren. Op deze manier kunt u de machtigingen verleend aan de toepassing beperken en het risico op schadelijke activiteiten verminderen in het geval uw toepassingscode kwetsbaar voor een SQL-injectieaanvallen is. 

Voor het maken van een SQL-Server geverifieerde gebruiker verbinding maken met de **master** op uw server met uw beheerdersaanmelding bij de server-database en maak een nieuwe server-aanmelding.  Daarnaast is het een goed idee om te maken van een gebruiker in de hoofddatabase voor gebruikers van Azure SQL Data Warehouse. Het maken van een gebruiker in master kan een gebruiker zich aanmelden met behulp van hulpprogramma's zoals SSMS zonder een databasenaam op te geven.  Ook kunnen ze de Objectverkenner om weer te geven van alle databases op een SQL server gebruiken.

```sql
-- Connect to master database and create a login
CREATE LOGIN ApplicationLogin WITH PASSWORD = 'Str0ng_password';
CREATE USER ApplicationUser FOR LOGIN ApplicationLogin;
```

Vervolgens verbinding maken met uw **SQL Data Warehouse-database** met uw beheerdersaanmelding bij de server en maak een databasegebruiker op basis van de aanmelding van de server die u hebt gemaakt.

```sql
-- Connect to SQL DW database and create a database user
CREATE USER ApplicationUser FOR LOGIN ApplicationLogin;
```

Om te geven een gebruikersmachtigingen voor aanvullende bewerkingen uitvoeren zoals het maken van aanmeldingen of het maken van nieuwe databases, wijst u de gebruiker de `Loginmanager` en `dbmanager` rollen in de database master. Zie voor meer informatie over deze extra rollen en zich verifiëren bij een SQL-Database, [databases en aanmeldingen beheren in Azure SQL Database][Managing databases and logins in Azure SQL Database].  Zie voor meer informatie, [verbinding te maken met SQL Data Warehouse door met behulp van Azure Active Directory-verificatie][Connecting to SQL Data Warehouse By Using Azure Active Directory Authentication].

## <a name="authorization"></a>Autorisatie
Autorisatie verwijst naar wat u binnen een Azure SQL Data Warehouse-database doen kunt. Autorisatie-bevoegdheden worden bepaald door de rollidmaatschappen en machtigingen. Het wordt aanbevolen om gebruikers de minimaal benodigde bevoegdheden te verlenen. Voor het beheren van rollen, kunt u de volgende opgeslagen procedures:

```sql
EXEC sp_addrolemember 'db_datareader', 'ApplicationUser'; -- allows ApplicationUser to read data
EXEC sp_addrolemember 'db_datawriter', 'ApplicationUser'; -- allows ApplicationUser to write data
```

Het serverbeheerdersaccount waarmee u verbinding maakt is lid van db_owner, die geautoriseerd is om alle bewerkingen binnen de database uit te voeren. Gebruik dit account voor het implementeren van schema-updates en andere beheerbewerkingen. Gebruik het 'ApplicationUser'-account met beperktere machtigingen om vanuit van uw toepassing verbinding te maken met de database met de minste bevoegdheden die nodig zijn voor uw toepassing.

Er zijn manieren om verder te beperken wat een gebruiker kan doen met Azure SQL Data Warehouse:

* Gedetailleerde [machtigingen] [ Permissions] kunt u beheren welke bewerkingen u kunt op afzonderlijke kolommen, tabellen, weergaven en schema's, procedures en andere objecten in de database. Gebruik gedetailleerde machtigingen voor de meeste controle hebt en de vereiste minimale machtigingen te verlenen. 
* [Databaserollen] [ Database roles] anders dan db_datareader en db_datawriter kunnen worden gebruikt om krachtigere toepassingsgebruikersaccounts of minder krachtige beheeraccounts te maken. De ingebouwde vaste databaserollen bieden een eenvoudige manier om machtigingen te verlenen, maar kunnen leiden tot toestemming geven meer dan nodig zijn.
* [Opgeslagen procedures] [ Stored procedures] kan worden gebruikt om de acties die kunnen worden uitgevoerd op de database te beperken.

Het volgende voorbeeld verleent leestoegang tot een door de gebruiker gedefinieerde schema.
```sql
--CREATE SCHEMA Test
GRANT SELECT ON SCHEMA::Test to ApplicationUser
```

Databases en logische servers beheren vanuit Azure portal of met behulp van de Azure Resource Manager-API wordt bepaald door de roltoewijzingen van de account van uw portal-gebruiker. Zie voor meer informatie, [rollen gebaseerd toegangsbeheer in Azure portal][Role-based access control in Azure portal].

## <a name="encryption"></a>Versleuteling
Azure SQL Data Warehouse transparante gegevensversleuteling (TDE) helpt te beschermen tegen de dreiging van schadelijke activiteiten door te coderen en decoderen van uw gegevens in rust.  Wanneer u uw database versleutelt, worden gekoppelde back-ups en transactielogboekbestanden versleuteld zonder wijzigingen aan uw toepassingen. De opslag van een volledige database versleutelt TDE met behulp van een symmetrische sleutel met de naam de databaseversleutelingssleutel. 

In SQL-Database, wordt de versleutelingssleutel van de database beveiligd door een ingebouwde servercertificaat. Het certificaat van de ingebouwde is uniek voor elke SQL-Database-server. Microsoft draait automatisch deze certificaten ten minste elke 90 dagen. De versleutelingsalgoritme die worden gebruikt door SQL Data Warehouse is AES-256. Zie voor een algemene beschrijving van TDE [Transparent Data Encryption][Transparent Data Encryption].

U kunt versleutelen met uw database met de [Azure-portal] [ Encryption with Portal] of [T-SQL][Encryption with TSQL].

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie en voorbeelden gegeven van de verbinding te maken met uw SQL Data Warehouse met verschillende protocollen, [verbinding maken met SQL Data Warehouse][Connect to SQL Data Warehouse].

<!--Image references-->

<!--Article references-->
[Connect to SQL Data Warehouse]: ./sql-data-warehouse-connect-overview.md
[Encryption with Portal]: ./sql-data-warehouse-encryption-tde.md
[Encryption with TSQL]: ./sql-data-warehouse-encryption-tde-tsql.md
[Connecting to SQL Data Warehouse By Using Azure Active Directory Authentication]: ./sql-data-warehouse-authentication.md

<!--MSDN references-->
[Azure SQL Database firewall]: https://msdn.microsoft.com/library/ee621782.aspx
[sp_set_firewall_rule]: https://msdn.microsoft.com/library/dn270017.aspx
[sp_set_database_firewall_rule]: https://msdn.microsoft.com/library/dn270010.aspx
[Database roles]: https://msdn.microsoft.com/library/ms189121.aspx
[Managing databases and logins in Azure SQL Database]: https://msdn.microsoft.com/library/ee336235.aspx
[Permissions]: https://msdn.microsoft.com/library/ms191291.aspx
[Stored procedures]: https://msdn.microsoft.com/library/ms190782.aspx
[Transparent Data Encryption]: https://msdn.microsoft.com/library/bb934049.aspx
[Azure portal]: https://portal.azure.com/

<!--Other Web references-->
[Role-based access control in Azure portal]: https://azure.microsoft.com/documentation/articles/role-based-access-control-configure
