---
title: Verificatie met Azure Active Directory - Azure SQL | Microsoft Docs
description: Meer informatie over het gebruik van Azure Active Directory voor verificatie met SQL Database Managed Instance en SQL Data Warehouse
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: data warehouse
ms.devlang: ''
ms.topic: conceptual
author: GithubMirek
ms.author: mireks
ms.reviewer: vanto, carlrab
manager: craigg
ms.date: 02/20/2019
ms.openlocfilehash: 1318cd3d1c0c51889cc70b6836d06d6d6ee70c24
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/04/2019
ms.locfileid: "57308377"
---
# <a name="use-azure-active-directory-authentication-for-authentication-with-sql"></a>Azure Active Directory-verificatie gebruiken voor verificatie met behulp van SQL

Azure Active Directory-verificatie is een mechanisme van verbinding maken met Azure [SQL-Database](sql-database-technical-overview.md), [Managed Instance](sql-database-managed-instance.md), en [SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) met behulp van identiteiten in Azure Active Directory (Azure AD). 

> [!NOTE]
> Dit onderwerp is van toepassing op Azure SQL-servers en op SQL Database- en SQL Data Warehouse-databases die op deze Azure SQL-servers worden gemaakt. Voor het gemak wordt de term 'SQL Database' gebruikt wanneer er wordt verwezen naar zowel SQL Database als SQL Data Warehouse.

Met Azure AD-verificatie, kunt u de identiteit van databasegebruikers en andere Microsoft-services op één centrale locatie centraal beheren. Centrale ID-beheer biedt één plek voor het beheren van databasegebruikers en vereenvoudigt het beheer van machtigingen. Voordelen zijn:

- Het biedt een alternatief voor SQL Server-verificatie.
- Houdt de verspreiding van gebruikers-id's in de database-servers.
- Kan de wisseling van het wachtwoord op één plek.
- Klanten kunnen de machtigingen van de database met behulp van externe (Azure AD)-groepen beheren.
- Deze kunt wachtwoorden moet opslaan elimineren door in te schakelen van geïntegreerde Windows-verificatie en andere vormen van authenticatie wordt ondersteund door Azure Active Directory.
- Azure AD-verificatie gebruikt ingesloten databasegebruikers om identiteiten op databaseniveau te verifiëren.
- Azure AD biedt ondersteuning voor verificatie op basis van tokens voor toepassingen die verbinding maken met SQL-Database.
- Azure AD-verificatie ondersteunt AD FS (domein Federatie) of systeemeigen gebruikerswachtwoord verificatie voor een lokale Azure Active Directory zonder synchronisatie van domein.
- Azure AD ondersteunt verbindingen van SQL Server Management Studio die gebruikmaken van Active Directory Universal-verificatie, waaronder multi-factor Authentication (MFA).  MFA omvat een robuuste verificatie met een scala aan gebruiksvriendelijke verificatieopties, telefonische oproepen, SMS-bericht, smartcards en pincode of mobiele app-meldingen. Zie voor meer informatie, [SSMS-ondersteuning voor Azure AD MFA met SQL Database en SQL Data Warehouse](sql-database-ssms-mfa-authentication.md).
- Azure AD biedt ondersteuning voor vergelijkbare verbindingen van SQL Server Data Tools (SSDT) die gebruikmaken van Active Directory-interactieve verificatie. Zie voor meer informatie, [ondersteuning van Azure Active Directory in SQL Server Data Tools (SSDT)](/sql/ssdt/azure-active-directory).

> [!NOTE]  
> Verbinding maken met SQL Server die wordt uitgevoerd op een Azure-VM wordt niet ondersteund met behulp van Azure Active Directory-account. Gebruik in plaats daarvan een domein Active Directory-account.  

De configuratiestappen omvat de volgende procedures om te configureren en gebruiken van Azure Active Directory-verificatie.

1. Maken en vullen van Azure AD.
2. Optioneel: Koppelen aan of wijzigt u de active directory die is gekoppeld aan uw Azure-abonnement.
3. Een Azure Active Directory-beheerder voor de Azure SQL Database-server, het beheerde exemplaar maken of de [Azure SQL Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/).
4. Uw clientcomputers configureren.
5. Maak ingesloten databasegebruikers in de database die is toegewezen aan Azure AD-identiteiten.
6. Verbinding maken met uw database met behulp van Azure AD-identiteiten.

> [!NOTE]
> Zie voor informatie over het maken en vullen van Azure AD en Azure AD configureren met Azure SQL Database Managed Instance en SQL Data Warehouse, [configureren Azure AD met Azure SQL Database](sql-database-aad-authentication-configure.md).

## <a name="trust-architecture"></a>Architectuur van vertrouwensrelatie

Het volgende diagram op hoog niveau bevat een overzicht van de oplossingsarchitectuur van het gebruik van Azure AD-verificatie met Azure SQL Database. Dezelfde concepten worden toegepast met SQL Data Warehouse. Ter ondersteuning van systeemeigen Azure AD-gebruikerswachtwoord, alleen het deel van de Cloud en de Azure AD/Azure SQL Database wordt beschouwd als. Ter ondersteuning van federatieve verificatie (of een gebruiker en wachtwoord voor Windows-referenties), de communicatie met AD FS-blok is vereist. De pijlen geven communicatiepaden.

![diagram van AAD-verificatie][1]

Het volgende diagram geeft aan dat de Federatie, vertrouwen en hosting-relaties waarmee een client verbinding maakt met een database door het indienen van een token. Het token is geverifieerd door een Azure AD en wordt vertrouwd door de database. Klant 1 kan bestaan uit een Azure Active Directory met systeemeigen gebruikers of een Azure AD bij federatieve gebruikers. Klant 2 vertegenwoordigt een mogelijke oplossing, met inbegrip van geïmporteerde gebruikers. in dit voorbeeld die afkomstig zijn van een federatieve Azure Active Directory met AD FS worden gesynchroniseerd met Azure Active Directory. Het is belangrijk om te begrijpen dat toegang tot een database met behulp van Azure AD-verificatie is vereist dat de hosting-abonnement gekoppeld aan de Azure AD is. Hetzelfde abonnement moet worden gebruikt voor het maken van de SQL-Server die als host fungeert voor de Azure SQL Database of SQL Data Warehouse.

![abonnement-relatie][2]

## <a name="administrator-structure"></a>Structuur van de beheerder

Wanneer u Azure AD-verificatie, zijn er twee beheerdersaccounts voor de SQL Database-server en de Managed Instance; de oorspronkelijke SQL Server-beheerder en de Azure AD-beheerder. Dezelfde concepten worden toegepast met SQL Data Warehouse. Alleen de beheerder op basis van een Azure AD-account kunt maken van de eerste databasegebruiker van Azure AD die zijn opgenomen in een gebruikersdatabase. De aanmeldingsnaam van de Azure AD-beheerder kan een Azure AD-gebruiker of een Azure AD-groep zijn. Als de beheerder een groepsaccount is, kan deze worden gebruikt door een groepslid, waardoor beheerders van meerdere Azure AD voor de SQL Server-exemplaar. Met behulp van groepsaccount als beheerder beheerbaarheid verbetert doordat u centraal toevoegen en verwijderen van leden van een beveiligingsgroep in Azure AD zonder de gebruikers of de machtigingen in SQL-Database te wijzigen. Slechts één Azure AD-beheerder (een gebruiker of groep) kan op elk gewenst moment worden geconfigureerd.

![structuur van de beheerder][3]

## <a name="permissions"></a>Machtigingen

Voor het maken van nieuwe gebruikers, moet u de `ALTER ANY USER` machtiging in de database. De `ALTER ANY USER` machtiging kan worden verleend aan een databasegebruiker. De `ALTER ANY USER` machtiging ook wordt gehouden door de server administrator-accounts en gebruikers van de database met de `CONTROL ON DATABASE` of `ALTER ON DATABASE` machtiging voor die database, en door leden van de `db_owner` databaserol.

Als u wilt een ingesloten databasegebruiker maken in Azure SQL Database Managed Instance of SQL Data Warehouse, moet u verbinding maken met de database of het exemplaar met behulp van een Azure AD-identiteit. Als u wilt de eerste ingesloten databasegebruiker maken, moet u verbinding met de database met behulp van een Azure AD-beheerder (die de eigenaar van de database). Dit wordt geïllustreerd in [configureren en beheren van Azure Active Directory-verificatie met SQL-Database of SQL Data Warehouse](sql-database-aad-authentication-configure.md). Een Azure AD-verificatie is alleen mogelijk als de Azure AD-beheerder voor Azure SQL Database of SQL Data Warehouse-server is gemaakt. Als de Azure Active Directory-beheerder is verwijderd van de server, bestaande Active Directory-gebruikers die zijn gemaakt eerder in SQL Server kunnen niet meer verbinding maken met de database met behulp van hun Azure Active Directory-referenties.

## <a name="azure-ad-features-and-limitations"></a>Azure AD-functies en beperkingen

- De volgende leden van Azure AD kunnen worden ingericht in Azure SQL-server of SQL Data Warehouse:

  - Systeemeigen leden: Een lid in Azure AD hebt gemaakt in het beheerde domein of in het domein van een klant. Zie voor meer informatie, [uw eigen domeinnaam toevoegen aan Azure AD](../active-directory/active-directory-domains-add-azure-portal.md).
  - Leden van een federatief domein: Een lid gemaakt in Azure AD met een federatief domein. Zie voor meer informatie, [Microsoft Azure ondersteunt nu Federatie met Windows Server Active Directory](https://azure.microsoft.com/blog/20../../windows-azure-now-supports-federation-with-windows-server-active-directory/).
  - Geïmporteerde leden van andere Azure AD die leden van een systeemeigen of federatieve domein zijn.
  - Active Directory-groepen gemaakt als beveiligingsgroepen.

- Azure AD-gebruikers die deel uitmaken van een groep met `db_owner` serverfunctie kan niet worden gebruikt de **[CREATE DATABASE SCOPED CREDENTIAL](/sql/t-sql/statements/create-database-scoped-credential-transact-sql)** syntaxis op basis van Azure SQL Database en Azure SQL Data Warehouse. Hier ziet u de volgende fout:

    `SQL Error [2760] [S0001]: The specified schema name 'user@mydomain.com' either does not exist or you do not have permission to use it.`

    Verleen de `db_owner` rol rechtstreeks naar de afzonderlijke Azure AD gebruiker te beperken door de **CREATE DATABASE SCOPED CREDENTIAL** probleem.

- Deze systeemfuncties retourneren NULL-waarden bij uitvoering onder Azure AD-principals:

  - `SUSER_ID()`
  - `SUSER_NAME(<admin ID>)`
  - `SUSER_SNAME(<admin SID>)`
  - `SUSER_ID(<admin name>)`
  - `SUSER_SID(<admin name>)`

### <a name="manage-instances"></a>-Instanties beheren

- Azure AD server-principals (aanmeldingen) en gebruikers worden ondersteund als preview-functie voor [beheerde instanties](sql-database-managed-instance.md).
- Instellen van Azure AD server-principals (aanmeldingen) toegewezen aan een Azure AD-groep als de eigenaar van database wordt niet ondersteund in [beheerde instanties](sql-database-managed-instance.md).
    - Een extensie hiervan is dat wanneer een groep wordt toegevoegd als onderdeel van de `dbcreator` serverfunctie, gebruikers van deze groep kunnen verbinding maken met het beheerde exemplaar en nieuwe databases maken, maar pas weer toegang tot de database. Dit komt doordat de nieuwe database-eigenaar SA, en niet de Azure AD-gebruiker is. Dit probleem heeft geen manifest als afzonderlijke gebruiker is toegevoegd aan de `dbcreator` serverfunctie.
- Uitvoering van de beheer- en taken van de SQL Agent wordt ondersteund voor Azure AD server-principals (aanmeldingen).
- Database-back-up en herstelbewerkingen kunnen worden uitgevoerd door Azure AD-server-principals (aanmeldingen).
- Controle van alle instructies met betrekking tot Azure AD server-principals (aanmeldingen) en verificatiegebeurtenissen wordt ondersteund.
- Exclusieve beheerdersverbinding voor Azure AD server-principals (aanmeldingen) die lid van de serverrol sysadmin zijn wordt ondersteund.
    - Ondersteund via het hulpprogramma SQLCMD en SQL Server Management Studio.
- Aanmeldingstriggers worden ondersteund voor aanmeldingsgebeurtenissen die afkomstig zijn van Azure AD-server-principal s(aanmeldingen).
- Service Broker en DB e-mail kan de installatie met behulp van een Azure AD-server-principal (aanmelden) zijn.


## <a name="connecting-using-azure-ad-identities"></a>Verbinding maken met behulp van Azure AD-identiteiten

Azure Active Directory-verificatie ondersteunt de volgende methoden voor het verbinden met een database met behulp van Azure AD-identiteiten:

- Met behulp van geïntegreerde Windows-verificatie
- Met behulp van een Azure AD-principal-naam en een wachtwoord
- Met behulp van de toepassing tokenverificatie

De volgende verificatiemethoden voor Azure AD server-principals (aanmeldingen) worden ondersteund (**preview-versie**):

- Azure Active Directory Password
- Geïntegreerd met Azure Active Directory
- Azure Active Directory-Universal met MFA
- Azure Active Directory Interactive


### <a name="additional-considerations"></a>Aanvullende overwegingen

- Ter verbetering van de beheerbaarheid, wordt aangeraden inrichten van een specifieke Azure AD als een beheerder een groep.   
- Slechts één Azure AD-beheerder (een gebruiker of groep) kan worden geconfigureerd voor een Azure SQL Database-server of Azure SQL Data Warehouse op elk gewenst moment.
  - Het toevoegen van Azure AD server-principals (aanmeldingen) voor beheerde instanties (**openbare preview**) kunt u de mogelijkheid voor het maken van meerdere Azure AD server-principals (aanmeldingen) die kunnen worden toegevoegd aan de `sysadmin` rol.
- Alleen een Azure AD-beheerder voor SQL Server kan in eerste instantie verbinding maken met de Azure SQL Database-server, een beheerd exemplaar of een Azure SQL Data Warehouse met behulp van Azure Active Directory-account. De Active Directory-beheerder kunt de volgende Azure AD configureren gebruikers van de database.   
- U wordt aangeraden de verbindingstime-out ingesteld op 30 seconden.   
- Azure Active Directory-verificatie wordt ondersteund door SQL Server 2016 Management Studio en SQL Server Data Tools voor Visual Studio 2015 (versie 14.0.60311.1April 2016 of hoger). (Azure AD-verificatie wordt ondersteund door de **.NET Framework Data Provider Pro SqlServer**; ten minste versie .NET Framework 4.6). Daarom de nieuwste versies van deze hulpprogramma's en -gegevenslaagtoepassingen (DAC en. BACPAC) kunt Azure AD-verificatie gebruiken.   
- Vanaf versie 15.0.1, [sqlcmd-hulpprogramma](/sql/tools/sqlcmd-utility) en [hulpprogramma bcp](/sql/tools/bcp-utility) ondersteuning bieden voor Active Directory-interactieve verificatie met MFA.
- SQL Server Data Tools voor Visual Studio 2015 vereist ten minste de April 2016-versie van de Data Tools (versie 14.0.60311.1). Azure AD-gebruikers worden momenteel niet weergegeven in SSDT-Objectverkenner. Als tijdelijke oplossing, bekijk de gebruikers in [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx).   
- [Microsoft JDBC-stuurprogramma 6.0 voor SQL Server](https://www.microsoft.com/download/details.aspx?id=11774) ondersteunt Azure AD-verificatie. Zie ook [instellen van de verbindingseigenschappen](https://msdn.microsoft.com/library/ms378988.aspx).   
- PolyBase kan niet verifiëren met behulp van Azure AD-verificatie.   
- Azure AD-verificatie wordt ondersteund voor SQL-Database door de Azure-portal **Database importeren** en **Database exporteren** blades. Importeren en exporteren met behulp van Azure AD-verificatie wordt ook ondersteund vanuit de PowerShell-opdracht.   
- Azure AD-verificatie wordt ondersteund voor SQL Database Managed Instance en SQL Data Warehouse met behulp van CLI. Zie voor meer informatie, [configureren en beheren van Azure Active Directory-verificatie met SQL-Database of SQL Data Warehouse](sql-database-aad-authentication-configure.md) en [SQL Server - az sql server](https://docs.microsoft.com/cli/azure/sql/server).

## <a name="next-steps"></a>Volgende stappen

- Zie voor informatie over het maken en vullen van Azure AD en Azure AD configureren met Azure SQL Database of Azure SQL Data Warehouse, [configureren en beheren van Azure Active Directory-verificatie met SQL Database Managed Instance of SQL Data Warehouse ](sql-database-aad-authentication-configure.md).
- Zie voor een zelfstudie van het gebruik van Azure AD server-principals (aanmeldingen) met beheerde instanties [Azure AD server-principals (aanmeldingen) met beheerde exemplaren](sql-database-managed-instance-aad-security-tutorial.md)
- Zie [SQL Database-toegang en -beheer](sql-database-control-access.md) voor een overzicht van toegang en beheer in SQL Database.
- Zie [Aanmeldingen, gebruikers en databaserollen](sql-database-manage-logins.md) voor een overzicht van aanmeldingen, gebruikers en databaserollen in SQL Database.
- Zie [Principals](https://msdn.microsoft.com/library/ms181127.aspx) voor meer informatie over database-principals.
- Zie [Databaserollen](https://msdn.microsoft.com/library/ms189121.aspx) voor meer informatie over databaserollen.
- Zie voor de syntaxis voor het maken van Azure AD server-principals (aanmeldingen) voor beheerde instanties, [CREATE LOGIN](/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current).
- Zie [SQL Database-firewallregels](sql-database-firewall-configure.md) voor meer informatie over de firewallregels in SQL Database.

<!--Image references-->

[1]: ./media/sql-database-aad-authentication/1aad-auth-diagram.png
[2]: ./media/sql-database-aad-authentication/2subscription-relationship.png
[3]: ./media/sql-database-aad-authentication/3admin-structure.png
[4]: ./media/sql-database-aad-authentication/4select-subscription.png
[5]: ./media/sql-database-aad-authentication/5ad-settings-portal.png
[6]: ./media/sql-database-aad-authentication/6edit-directory-select.png
[7]: ./media/sql-database-aad-authentication/7edit-directory-confirm.png
[8]: ./media/sql-database-aad-authentication/8choose-ad.png
[9]: ./media/sql-database-aad-authentication/9ad-settings.png
[10]: ./media/sql-database-aad-authentication/10choose-admin.png
[11]: ./media/sql-database-aad-authentication/11connect-using-int-auth.png
[12]: ./media/sql-database-aad-authentication/12connect-using-pw-auth.png
[13]: ./media/sql-database-aad-authentication/13connect-to-db.png
