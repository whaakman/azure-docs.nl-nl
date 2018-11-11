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
ms.date: 10/05/2018
ms.openlocfilehash: 86e60f339af3d6d467b68d5d3b27d77a9861add1
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51244068"
---
# <a name="use-azure-active-directory-authentication-for-authentication-with-sql"></a>Azure Active Directory-verificatie gebruiken voor verificatie met behulp van SQL

Azure Active Directory-verificatie is een mechanisme van verbinding maken met Azure [SQL-Database](sql-database-technical-overview.md) en [SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) met behulp van identiteiten in Azure Active Directory (Azure AD). 

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

> [!NOTE]  
> Verbinding maken met SQL Server die wordt uitgevoerd op een Azure-VM wordt niet ondersteund met behulp van Azure Active Directory-account. Gebruik in plaats daarvan een domein Active Directory-account.  

De configuratiestappen omvat de volgende procedures om te configureren en gebruiken van Azure Active Directory-verificatie.

1. Maken en vullen van Azure AD.
2. Optioneel: Koppelen of wijzigt u de active directory die is gekoppeld aan uw Azure-abonnement.
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

De volgende leden van Azure AD kunnen worden ingericht in Azure SQL-server of SQL Data Warehouse:

- Systeemeigen leden: een lid in Azure AD hebt gemaakt in het beheerde domein of in het domein van een klant. Zie voor meer informatie, [uw eigen domeinnaam toevoegen aan Azure AD](../active-directory/active-directory-domains-add-azure-portal.md).
- Leden van een domein federatieve: lid gemaakt in Azure AD met een federatief domein. Zie voor meer informatie, [Microsoft Azure ondersteunt nu Federatie met Windows Server Active Directory](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/).
- Geïmporteerde leden van andere Azure AD die leden van een systeemeigen of federatieve domein zijn.
- Active Directory-groepen gemaakt als beveiligingsgroepen.

Azure AD-beperkingen met betrekking tot het beheerde exemplaar:

- Alleen Azure AD-beheerder van databases maken kunt, Azure AD-gebruikers zijn gericht op een enkele database en bent niet gemachtigd deze
- Database-eigenaar:
  - Azure AD-principal kan geen eigendom van de database (AUTORISATIE van DATABASE wijzigen) en kan niet worden ingesteld als eigenaar.
  - Voor databases die zijn gemaakt door Azure AD-beheerder geen eigenaar is ingesteld (0x1 owner_sid veld in sys.sysdatabases is).
- SQL Agent kan niet worden beheerd wanneer u bent aangemeld bij het gebruik van Azure AD-principals.
- Azure AD-beheerder kan niet worden geïmiteerd met behulp van EXECUTE AS
- DAC-verbinding wordt niet ondersteund met Azure AD-principals.

Deze systeemfuncties retourneren NULL-waarden bij uitvoering onder Azure AD-principals:

- `SUSER_ID()`
- `SUSER_NAME(<admin ID>)`
- `SUSER_SNAME(<admin SID>)`
- `SUSER_ID(<admin name>)`
- `SUSER_SID(<admin name>)`

## <a name="connecting-using-azure-ad-identities"></a>Verbinding maken met behulp van Azure AD-identiteiten

Azure Active Directory-verificatie ondersteunt de volgende methoden voor het verbinden met een database met behulp van Azure AD-identiteiten:

- Met behulp van geïntegreerde Windows-verificatie
- Met behulp van een Azure AD-principal-naam en een wachtwoord
- Met behulp van de toepassing tokenverificatie

### <a name="additional-considerations"></a>Aanvullende overwegingen

- Ter verbetering van de beheerbaarheid, wordt aangeraden inrichten van een specifieke Azure AD als een beheerder een groep.   
- Slechts één Azure AD-beheerder (een gebruiker of groep) kan worden geconfigureerd voor een Azure SQL Database-server, een beheerd exemplaar of een Azure SQL Data Warehouse op elk gewenst moment.
- Alleen een Azure AD-beheerder voor SQL Server kan in eerste instantie verbinding maken met de Azure SQL Database-server, een beheerd exemplaar of een Azure SQL Data Warehouse met behulp van Azure Active Directory-account. De Active Directory-beheerder kunt de volgende Azure AD configureren gebruikers van de database.   
- U wordt aangeraden de verbindingstime-out ingesteld op 30 seconden.   
- Azure Active Directory-verificatie wordt ondersteund door SQL Server 2016 Management Studio en SQL Server Data Tools voor Visual Studio 2015 (versie 14.0.60311.1April 2016 of hoger). (Azure AD-verificatie wordt ondersteund door de **.NET Framework Data Provider Pro SqlServer**; ten minste versie .NET Framework 4.6). Daarom de nieuwste versies van deze hulpprogramma's en -gegevenslaagtoepassingen (DAC en. BACPAC) kunt Azure AD-verificatie gebruiken.   
- [ODBC versie 13.1](https://www.microsoft.com/download/details.aspx?id=53339) biedt echter ondersteuning voor Azure Active Directory-verificatie `bcp.exe` kan geen verbinding maken met behulp van Azure Active Directory-verificatie, omdat deze een oudere ODBC-provider gebruikt.   
- `sqlcmd` biedt ondersteuning voor Azure Active Directory-verificatie die begint met versie 13.1 beschikbaar is via de [Downloadcentrum](https://go.microsoft.com/fwlink/?LinkID=825643).
- SQL Server Data Tools voor Visual Studio 2015 vereist ten minste de April 2016-versie van de Data Tools (versie 14.0.60311.1). Azure AD-gebruikers worden momenteel niet weergegeven in SSDT-Objectverkenner. Als tijdelijke oplossing, bekijk de gebruikers in [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx).   
- [Microsoft JDBC-stuurprogramma 6.0 voor SQL Server](https://www.microsoft.com/download/details.aspx?id=11774) ondersteunt Azure AD-verificatie. Zie ook [instellen van de verbindingseigenschappen](https://msdn.microsoft.com/library/ms378988.aspx).   
- PolyBase kan niet verifiëren met behulp van Azure AD-verificatie.   
- Azure AD-verificatie wordt ondersteund voor SQL-Database door de Azure-portal **Database importeren** en **Database exporteren** blades. Importeren en exporteren met behulp van Azure AD-verificatie wordt ook ondersteund vanuit de PowerShell-opdracht.   
- Azure AD-verificatie wordt ondersteund voor SQL Database Managed Instance en SQL Data Warehouse met behulp van CLI. Zie voor meer informatie, [configureren en beheren van Azure Active Directory-verificatie met SQL-Database of SQL Data Warehouse](sql-database-aad-authentication-configure.md) en [SQL Server - az sql server](https://docs.microsoft.com/cli/azure/sql/server).

## <a name="next-steps"></a>Volgende stappen

- Zie voor informatie over het maken en vullen van Azure AD en Azure AD configureren met Azure SQL Database of Azure SQL Data Warehouse, [configureren en beheren van Azure Active Directory-verificatie met SQL Database Managed Instance of SQL Data Warehouse ](sql-database-aad-authentication-configure.md).
- Zie [SQL Database-toegang en -beheer](sql-database-control-access.md) voor een overzicht van toegang en beheer in SQL Database.
- Zie [Aanmeldingen, gebruikers en databaserollen](sql-database-manage-logins.md) voor een overzicht van aanmeldingen, gebruikers en databaserollen in SQL Database.
- Zie [Principals](https://msdn.microsoft.com/library/ms181127.aspx) voor meer informatie over database-principals.
- Zie [Databaserollen](https://msdn.microsoft.com/library/ms189121.aspx) voor meer informatie over databaserollen.
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
