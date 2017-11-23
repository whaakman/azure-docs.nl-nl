---
title: PaaS-Databases in Azure beveiligen | Microsoft Docs
description: " Meer informatie over Azure SQL Database en SQL Data Warehouse security best practices voor het beveiligen van uw PaaS-web- en mobiele toepassingen. "
services: security
documentationcenter: na
author: techlake
manager: MBaldwin
editor: 
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: terrylan
ms.openlocfilehash: ed15ff750e73179f6979d13e45ab27aeee5c82ab
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/22/2017
---
# <a name="securing-paas-databases-in-azure"></a>PaaS-databases in Azure beveiligen

In dit artikel bespreken we een verzameling van [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) en [SQL Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/) aanbevolen beveiligingsprocedures voor het beveiligen van uw PaaS-webtoepassingen en mobiele toepassingen. Deze aanbevolen procedures zijn afgeleid van onze ervaring met Azure en de ervaringen van klanten zoals zelf.

## <a name="azure-sql-database-and-sql-data-warehouse"></a>Azure SQL Database en SQL datawarehouse
[Azure SQL Database](../sql-database/sql-database-technical-overview.md) en [SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) een relationele database-service voor uw internetgebaseerde toepassingen bieden. Bekijk de services die u uw toepassingen en gegevens beschermen helpen wanneer u Azure SQL Database en SQL Data Warehouse in een PaaS-implementatie:

- Azure Active Directory-verificatie (in plaats van SQL Server-verificatie)
- Azure SQL-firewall
- Transparante gegevensversleuteling (TDE)

## <a name="best-practices"></a>Aanbevolen procedures

### <a name="use-a-centralized-identity-repository-for-authentication-and-authorization"></a>Een gecentraliseerde identiteitsopslagplaats gebruiken voor verificatie en autorisatie

Azure SQL-databases kunnen worden geconfigureerd voor het gebruik van een van twee typen verificatie:

- **SQL-verificatie** maakt gebruik van een gebruikersnaam en wachtwoord. Wanneer u de logische server voor uw database hebt gemaakt, hebt u een aanmelding 'serverbeheerder' opgegeven met een gebruikersnaam en wachtwoord. Met deze referenties, kunt u verifiëren met een database op die server als de eigenaar van de database.

- **Azure Active Directory-verificatie** identiteiten die worden beheerd door Azure Active Directory gebruikt en wordt ondersteund voor beheerde en geïntegreerde domeinen. Voor het gebruik van Azure Active Directory-verificatie, moet u een andere server admin aangeroepen de 'Azure AD-beheerder,' die is toegestaan voor het beheer van Azure AD-gebruikers en groepen maken. Deze beheerder kan ook alle bewerkingen uitvoeren die reguliere serverbeheerders kunnen uitvoeren.

[Azure Active Directory-verificatie](../active-directory/develop/active-directory-authentication-scenarios.md) is een mechanisme van verbinding maken met Azure SQL Database en SQL Data Warehouse met behulp van identiteiten in Azure Active Directory (AD). Azure AD levert een alternatief voor SQL Server-verificatie, zodat u de komst van gebruikersidentiteiten over databaseservers stoppen kunt. Azure AD-verificatie kunt u de identiteit van gebruikers en andere Microsoft-services op één centrale locatie centraal te beheren. Centrale ID-beheer biedt er één plaats is voor het beheren van databasegebruikers en vereenvoudigt het beheer van machtigingen.  

Voordelen van het gebruik van Azure AD-verificatie in plaats van de SQL-verificatie zijn:

- Kan wachtwoord draaien op één plaats.
- Beheert de databasemachtigingen met externe Azure AD-groepen.
- Elimineert wachtwoorden moet opslaan door het inschakelen van geïntegreerde Windows-verificatie en andere soorten authenticatie wordt ondersteund door Azure AD.
- Maakt gebruik van opgenomen databasegebruikers om identiteiten op het databaseniveau van de te verifiëren.
- Biedt ondersteuning voor verificatie op basis van tokens voor toepassingen die verbinding maken met SQL-Database.
- Biedt ondersteuning voor AD FS (domeinfederatie) of systeemeigen gebruikerswachtwoord verificatie voor een lokale Azure AD zonder Domeinsynchronisatie.
- Ondersteunt verbindingen van SQL Server Management Studio die gebruikmaken van Universal verificatie van Active Directory, waaronder [multi-factor Authentication (MFA)](../multi-factor-authentication/multi-factor-authentication.md). MFA sterke verificatie met een bereik van eenvoudige verificatie-opties bevat: telefoonoproep, tekstbericht, smartcards en pincode of mobiele app-melding. Zie voor meer informatie [SSMS ondersteuning voor Azure AD MFA met SQL-Database en SQL Data Warehouse](../sql-database/sql-database-ssms-mfa-authentication.md).

Zie voor meer informatie over Azure AD-verificatie:

- [Verbinding maken met SQL-Database of SQL datawarehouse met behulp van Azure Active Directory-verificatie](../sql-database/sql-database-aad-authentication.md)
- [Verificatie met Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-authentication.md)
- [Ondersteuning voor verificatie op basis van tokens voor Azure SQL DB met Azure AD-verificatie](https://blogs.msdn.microsoft.com/sqlsecurity/2016/02/09/token-based-authentication-support-for-azure-sql-db-using-azure-ad-auth/)

> [!NOTE]
> Om ervoor te zorgen dat Azure Active Directory geschikt voor uw omgeving is, Zie [Azure AD-functies en beperkingen](../sql-database/sql-database-aad-authentication.md#azure-ad-features-and-limitations), specifiek voor de aanvullende overwegingen.
>
>

### <a name="restrict-access-based-on-ip-address"></a>Toegang beperken op basis van IP-adres
U kunt firewallregels die aanvaardbaar IP-adresbereiken opgeven. Deze regels kunnen worden gericht op de server en de database niveaus. U kunt het beste databaseniveau firewallregels zo veel mogelijk om beveiliging te verbeteren en kunt u de database meer draagbare gebruikt. Firewallregels op serverniveau worden beste gebruikt voor beheerders en wanneer er veel databases die dezelfde toegangsvereisten hebben, maar u niet wilt dat elke database afzonderlijk configureren tijd te besteden aan.

SQL-Database standaard bron IP-adresbeperkingen toestaan toegang van elk Azure-adres (inclusief andere abonnementen en tenants). U kunt deze optie als u wilt dat alleen uw IP-adressen voor toegang tot het exemplaar te beperken. Zelfs met uw SQL-firewall en IP-adresbeperkingen sterke authenticatie nog steeds vereist. Zie de aanbevelingen die eerder in dit artikel.

Zie voor meer informatie over Azure SQL-Firewall en IP-beperkingen:

- [Toegangsbeheer voor Azure SQL Database](../sql-database/sql-database-control-access.md)
- [Configureren van Azure SQL Database-firewallregels - overzicht](../sql-database/sql-database-firewall-configure.md)
- [Een Azure SQL Database serverniveau firewallregel met de Azure portal configureren](../sql-database/sql-database-configure-firewall-settings.md)

### <a name="encryption-of-data-at-rest"></a>Versleuteling van data-at-rest
[Transparent Data Encryption (TDE)](https://msdn.microsoft.com/library/azure/bb934049) is standaard ingeschakeld. TDE versleutelt transparant SQL Server, Azure SQL Database en Azure SQL Data Warehouse-gegevens en logboekbestanden bestanden. TDE beschermd tegen inbreuk directe toegang tot de bestanden of hun back-up. Hiermee kunt u gegevens in rust te versleutelen zonder te wijzigen van bestaande toepassingen. TDE moet altijd blijven ingeschakeld; Hierdoor wordt een kwaadwillende persoon via het pad van de normale toegang echter niet gestopt. TDE biedt de mogelijkheid om te voldoen aan veel wettelijke, en tot stand gebracht in verschillende branches richtlijnen.

Azure SQL beheert sleutel toegangsproblemen voor TDE. Zoals met TDE, on-premises speciale moet nauwkeurig worden gegarandeerd kunnen worden hersteld en bij het verplaatsen van databases. In meer geavanceerde scenario's, de sleutels kunnen worden expliciet beheerd in Azure Key Vault via extensible key management (Zie [TDE inschakelen op de SQL Server met behulp van EKM](/security/encryption/enable-tde-on-sql-server-using-ekm)). Hierdoor kunnen ook voor Bring Your Own Key (BYOK) via Azure sleutel kluizen BYOK-mogelijkheden.

Azure SQL biedt versleuteling voor de kolommen via [altijd versleuteld](/sql/relational-databases/security/encryption/always-encrypted-database-engine). Hiermee kunt alleen geautoriseerde toepassingen toegang tot gevoelige kolommen. SQL-query's voor versleutelde kolommen op gelijkheid gebaseerde waarden met behulp van deze versleuteling worden beperkt.

Versleuteling van toepassing op moet ook worden gebruikt voor de geselecteerde gegevens. Gegevens onafhankelijkheid problemen kunnen soms worden voorkomen door gegevens te coderen met een sleutel die wordt opgeslagen in het juiste land. Dit voorkomt dat zelfs onbedoeld gegevensoverdracht een probleem veroorzaken, omdat het is niet mogelijk om de gegevens zonder dat de sleutel, ervan uitgaande dat een sterke-algoritme wordt gebruikt (zoals AES 256) te ontsleutelen.

U kunt aanvullende voorzorgsmaatregelen gebruiken om te helpen beveiligen van de database zoals ontwerpen van een beveiligde systeem, vertrouwelijk assets coderen en bouwen van een firewall rond de databaseservers.

## <a name="next-steps"></a>Volgende stappen
In dit artikel is geïntroduceerd aan een verzameling van SQL-Database en SQL Data Warehouse aanbevolen beveiligingsprocedures voor het beveiligen van uw PaaS-webtoepassingen en mobiele toepassingen. Zie voor meer informatie over het beveiligen van uw PaaS-implementaties:

- [PaaS-implementaties beveiligen](security-paas-deployments.md)
- [PaaS-webtoepassingen en mobiele toepassingen met Azure App Services beveiligen](security-paas-applications-using-app-services.md)
