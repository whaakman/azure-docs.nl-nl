---
title: Azure Active Directory-auth - Azure SQL (overzicht) | Microsoft Docs
description: Meer informatie over het gebruik van Azure Active Directory voor verificatie bij SQL-Database en SQL Data Warehouse
services: sql-database
documentationcenter: 
author: GithubMirek
manager: johammer
editor: 
tags: 
ms.assetid: 7e2508a1-347e-4f15-b060-d46602c5ce7e
ms.service: sql-database
ms.custom: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Active
ms.date: 09/12/2017
ms.author: mireks
ms.openlocfilehash: ff6e4405819d1e7134eb5cdfd7d8ad712dc2a654
ms.sourcegitcommit: 71fa59e97b01b65f25bcae318d834358fea5224a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2018
---
# <a name="use-azure-active-directory-authentication-for-authentication-with-sql-database-or-sql-data-warehouse"></a>Azure Active Directory-verificatie gebruiken voor verificatie met SQL-Database of SQL Data Warehouse
Azure Active Directory-verificatie is een mechanisme van verbinding maken met Microsoft Azure SQL Database en [SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) met behulp van identiteiten in Azure Active Directory (Azure AD). Azure AD-verificatie, kunt u de identiteit van gebruikers en andere Microsoft-services op één centrale locatie centraal beheren. Centrale ID-beheer biedt er één plaats is voor het beheren van databasegebruikers en vereenvoudigt het beheer van machtigingen. Voordelen zijn onder andere:

* Het biedt een alternatief voor SQL Server-verificatie.
* Houdt de komst van gebruikersidentiteiten voor databaseservers.
* Hiermee kunt wachtwoord draaien op één plaats
* Klanten kunnen met behulp van externe (AAD) groepen databasemachtigingen beheren.
* Deze kunt wachtwoorden moet opslaan voorkomen door het inschakelen van geïntegreerde Windows-verificatie en andere soorten authenticatie wordt ondersteund door Azure Active Directory.
* Ingesloten databasegebruikers Azure AD-verificatie gebruikt voor het verifiëren van identiteiten op databaseniveau.
* Azure AD biedt ondersteuning voor verificatie op basis van tokens voor toepassingen die verbinding maken met SQL-Database.
* Azure AD-verificatie ondersteunt AD FS (domeinfederatie) of systeemeigen gebruikerswachtwoord verificatie voor een lokale Azure Active Directory zonder Domeinsynchronisatie.  
* Azure AD ondersteunt verbindingen van SQL Server Management Studio die gebruikmaken van Universal verificatie van Active Directory, waaronder de multi-factor Authentication (MFA).  MFA sterke verificatie met een bereik van eenvoudige verificatie-opties bevat: telefoonoproep, tekstbericht, smartcards en pincode of mobiele app-melding. Zie voor meer informatie [SSMS ondersteuning voor Azure AD MFA met SQL-Database en SQL Data Warehouse](sql-database-ssms-mfa-authentication.md).  

>  [!NOTE]  
>  Verbinding maken met SQL Server wordt uitgevoerd op een Azure VM wordt niet ondersteund met een Azure Active Directory-account. Gebruik in plaats daarvan de Active Directory-account van een domein.  

De configuratiestappen omvat de volgende procedures om te configureren en gebruiken van Azure Active Directory-verificatie.

1. Maken en Azure AD te vullen.
2. Optioneel: Koppelen of wijzig de active directory die momenteel is gekoppeld aan uw Azure-abonnement.
3. Een Azure Active Directory-beheerder voor Azure SQL-server maken of [Azure SQL Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/).
4. De clientcomputers configureren.
5. Ingesloten databasegebruikers maken in de database die is toegewezen aan Azure AD-identiteiten.
6. Verbinding maken met de database met behulp van Azure AD-identiteiten.

> [!NOTE]
> Zie voor informatie over het maken en Azure AD te vullen en vervolgens Azure AD configureren met Azure SQL Database en SQL Data Warehouse, [configureren Azure AD met Azure SQL Database](sql-database-aad-authentication-configure.md).
>

## <a name="trust-architecture"></a>Architectuur van vertrouwensrelatie
Het volgende diagram op hoog niveau bevat een overzicht van de oplossingsarchitectuur van het gebruik van Azure AD-verificatie met Azure SQL Database. Dezelfde concepten die van toepassing op de SQL Data Warehouse. Ter ondersteuning van Azure AD systeemeigen gebruikerswachtwoord slechts het deel van de Cloud en de Azure AD/Azure SQL Database wordt beschouwd als. Ter ondersteuning van federatieve verificatie (of gebruikerswachtwoord voor Windows-referenties), de communicatie met AD FS-blok is vereist. De pijlen communicatiepaden aangeven.

![AAD auth-diagram][1]

Het volgende diagram geeft de Federatie, de vertrouwensrelatie en de hosting-relaties waarmee een client verbinding maakt met een database door het indienen van een token. Het token is geverifieerd door een Azure AD en wordt vertrouwd door de database. Klant 1 kan een Azure Active Directory met systeemeigen gebruikers of een Azure AD bij federatieve gebruikers vertegenwoordigen. Klant 2 vertegenwoordigt een mogelijke oplossing, inclusief geïmporteerde gebruikers. in dit voorbeeld afkomstig is van een federatieve Azure Active Directory met AD FS worden gesynchroniseerd met Azure Active Directory. Het is belangrijk te weten dat toegang tot een database met Azure AD-verificatie vereist dat de hosting-abonnement gekoppeld aan de Azure AD is. Hetzelfde abonnement moet worden gebruikt voor het maken van de SQL-Server die als host fungeert voor de Azure SQL Database of SQL Data Warehouse.

![abonnement-relatie][2]

## <a name="administrator-structure"></a>Structuur van de beheerder
Wanneer u Azure AD-verificatie, zijn er twee beheerdersaccounts voor de SQL-Database-server. de oorspronkelijke SQL Server-beheerder en de Azure AD-beheerder. Dezelfde concepten die van toepassing op de SQL Data Warehouse. Alleen de beheerder op basis van een Azure AD-account kunt maken van de eerste databasegebruiker van Azure AD zijn opgenomen in een gebruikersdatabase. De aanmelding van Azure AD-beheerder kan een Azure AD-gebruiker of een Azure AD-groep zijn. Wanneer de beheerder een groepsaccount is, kan deze worden gebruikt door een groepslid, waardoor meerdere Azure AD-beheerders voor het SQL Server-exemplaar. Als een beheerder met behulp van groepsaccount beheerbaarheid verbetert doordat u centraal toevoegen en verwijderen van leden van de beveiligingsgroep in Azure AD zonder de gebruikers of de machtigingen in SQL-Database te wijzigen. Slechts één Azure AD-beheerder (een gebruiker of groep) kan op elk gewenst moment worden geconfigureerd.

![structuur van de beheerder][3]

## <a name="permissions"></a>Machtigingen
Voor het maken van nieuwe gebruikers, hebt u de `ALTER ANY USER` machtiging in de database. De `ALTER ANY USER` machtiging kan worden toegekend aan elke databasegebruiker. De `ALTER ANY USER` machtiging ook wordt vastgehouden door de server administrator-accounts en databasegebruikers met de `CONTROL ON DATABASE` of `ALTER ON DATABASE` machtigingen nodig voor deze database en door leden van de `db_owner` -databaserol.

Voor het maken van een ingesloten database-gebruiker in Azure SQL Database of SQL Data Warehouse, moet u verbinding maken met de database met een Azure AD-identiteit. Voor het maken van de eerste gebruiker van de ingesloten database, moet u verbinding met de database met behulp van een Azure AD-beheerder (die eigenaar is van de database). Dit wordt geïllustreerd in [configureren en beheren van Azure Active Directory-verificatie met SQL-Database of SQL Data Warehouse](sql-database-aad-authentication-configure.md). Elke Azure AD-verificatie is alleen mogelijk als de Azure AD-beheerder voor Azure SQL Database of SQL Data Warehouse-server is gemaakt. Als de beheerder van Azure Active Directory is verwijderd van de server, kunnen bestaande Azure Active Directory-gebruikers die zijn gemaakt eerder in SQL Server niet langer verbinding met de database met behulp van de Azure Active Directory-referenties.

## <a name="azure-ad-features-and-limitations"></a>Azure AD-functies en beperkingen
De volgende leden van Azure AD kunnen worden ingericht in Azure SQL-server of SQL Data Warehouse:

* Systeemeigen leden: lid gemaakt in Azure AD in het beheerde domein of in een domein van de klant. Zie voor meer informatie [uw eigen domeinnaam toevoegen aan Azure AD](../active-directory/active-directory-domains-add-azure-portal.md).
* Federatieve domeinleden: lid gemaakt in Azure AD met een federatieve domein. Zie voor meer informatie [Microsoft Azure biedt nu ondersteuning voor federatie met Windows Server Active Directory](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/).
* Geïmporteerde leden van andere Azure AD die systeemeigen of federatieve domeinleden zijn.
* Active Directory-groepen als beveiligingsgroepen worden gemaakt.


## <a name="connecting-using-azure-ad-identities"></a>Verbinding maken met behulp van Azure AD-identiteiten

Azure Active Directory-verificatie ondersteunt de volgende methoden van verbinding maken met een database met behulp van Azure AD-identiteiten:

* Met behulp van geïntegreerde Windows-verificatie
* Met behulp van een Azure AD-principal-naam en een wachtwoord
* Met behulp van de toepassing tokenverificatie

### <a name="additional-considerations"></a>Aanvullende overwegingen

* Betere beheerbaarheid, raden we u een specifieke Azure AD inrichten als een beheerder een groep.   
* Slechts één Azure AD-beheerder (een gebruiker of groep) kan worden geconfigureerd voor een Azure SQL-server of Azure SQL Data Warehouse op elk gewenst moment.   
* Alleen een Azure AD-beheerder voor SQL Server kan in eerste instantie verbinding maken met de Azure SQL-server of Azure SQL Data Warehouse met een Azure Active Directory-account. De Active Directory-beheerder kan de volgende Azure AD configureren gebruikers van de database.   
* U wordt aangeraden de verbindingstime-out ingesteld op 30 seconden.   
* Azure Active Directory-verificatie wordt ondersteund door SQL Server 2016 Management Studio en SQL Server Data Tools voor Visual Studio 2015 (versie 14.0.60311.1April 2016 of hoger). (Azure AD-verificatie wordt ondersteund door de **.NET Framework-gegevensprovider voor SqlServer**; ten minste versie .NET Framework 4.6). De nieuwste versies van deze hulpprogramma's en -gegevenslaagtoepassingen (DAC en Bacpac) Gebruik daarom Azure AD-verificatie.   
* [ODBC versie 13,1](https://www.microsoft.com/download/details.aspx?id=53339) biedt echter ondersteuning voor Azure Active Directory-verificatie `bcp.exe` kan geen verbinding maken met Azure Active Directory-verificatie, omdat deze gebruikmaakt van een oudere ODBC-provider.   
* `sqlcmd`biedt ondersteuning voor Azure Active Directory-verificatie die begint met versie 13,1 beschikbaar is via de [Downloadcentrum](http://go.microsoft.com/fwlink/?LinkID=825643).   
* SQL Server Data Tools voor Visual Studio 2015 vereist ten minste de April 2016-versie van de Data Tools (versie 14.0.60311.1). Azure AD-gebruikers worden momenteel niet weergegeven in de Objectverkenner SSDT. Als tijdelijke oplossing de gebruikers in bekijken [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx).   
* [Microsoft JDBC-stuurprogramma 6.0 voor SQL Server](https://www.microsoft.com/download/details.aspx?id=11774) ondersteunt Azure AD-verificatie. Zie ook [instellen van de verbindingseigenschappen](https://msdn.microsoft.com/library/ms378988.aspx).   
* PolyBase kan niet verifiëren met behulp van Azure AD-verificatie.   
* Azure AD-verificatie voor SQL-Database wordt ondersteund door de Azure-portal **Database importeren** en **Database exporteren** blades. Importeren en exporteren met behulp van Azure AD-verificatie wordt ook ondersteund van de PowerShell-opdracht.   
* Azure AD-verificatie wordt ondersteund voor SQL-Database en SQL Data Warehouse door gebruik CLI. Zie voor meer informatie [configureren en beheren van Azure Active Directory-verificatie met SQL-Database of SQL Data Warehouse](sql-database-aad-authentication-configure.md) en [SQL Server - az sql server](https://docs.microsoft.com/cli/azure/sql/server).

## <a name="next-steps"></a>Volgende stappen
- Zie voor informatie over het maken en Azure AD te vullen en vervolgens Azure AD configureren met Azure SQL Database- of Azure SQL Data Warehouse, [configureren en beheren van Azure Active Directory-verificatie met SQL-Database of SQL Data Warehouse](sql-database-aad-authentication-configure.md).
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

