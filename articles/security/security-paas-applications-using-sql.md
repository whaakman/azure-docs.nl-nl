---
title: PaaS-Databases in Azure beveiligen | Microsoft Docs
description: " Meer informatie over de beveiliging van Azure SQL Database en SQL Data Warehouse aanbevolen procedures voor het beveiligen van uw PaaS-webtoepassingen en mobiele toepassingen. "
services: security
documentationcenter: na
author: techlake
manager: MBaldwin
editor: ''
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: terrylan
ms.openlocfilehash: ee606540bef47b11ad8fd9e820af2f5b51d47b0b
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39493018"
---
# <a name="securing-paas-databases-in-azure"></a>PaaS-databases in Azure beveiligen

In dit artikel bespreken we een verzameling van [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) en [SQL Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/) aanbevolen beveiligingsprocedures voor het beveiligen van uw PaaS-webtoepassingen en mobiele toepassingen. Deze aanbevolen procedures zijn afgeleid van onze ervaring met Azure en de ervaringen van klanten zoals zelf.

## <a name="azure-sql-database-and-sql-data-warehouse"></a>Azure SQL Database en SQL datawarehouse
[Azure SQL Database](../sql-database/sql-database-technical-overview.md) en [SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) een relationele databaseservice voor uw toepassingen op basis van Internet. Laten we kijken services om uw toepassingen en gegevens beschermen bij het gebruik van Azure SQL Database en SQL Data Warehouse in een PaaS-implementatie:

- Azure Active Directory-verificatie (in plaats van SQL Server-verificatie)
- Azure SQL-firewall
- Transparante gegevensversleuteling (TDE)

## <a name="best-practices"></a>Aanbevolen procedures

### <a name="use-a-centralized-identity-repository-for-authentication-and-authorization"></a>Een gecentraliseerde identiteitsopslagplaats gebruiken voor verificatie en autorisatie

Azure SQL-databases kunnen worden geconfigureerd voor het gebruik van een van twee typen verificatie:

- **SQL-verificatie** maakt gebruik van een gebruikersnaam en wachtwoord. Wanneer u de logische server voor uw database hebt gemaakt, hebt u een aanmelding 'serverbeheerder' opgegeven met een gebruikersnaam en wachtwoord. Met deze referenties, kunt u verifiëren met elke database op die server als de database-eigenaar.

- **Azure Active Directory-verificatie** maakt gebruik van identiteiten die worden beheerd door Azure Active Directory en wordt ondersteund voor beheerde en geïntegreerde domeinen. Voor het gebruik van Azure Active Directory-verificatie, moet u een andere serverbeheerder met de naam ' Azure AD-beheerder,' die is toegestaan voor het beheer van Azure AD-gebruikers en groepen maken. Deze beheerder kan ook alle bewerkingen uitvoeren die reguliere serverbeheerders kunnen uitvoeren.

[Azure Active Directory-verificatie](../active-directory/develop/authentication-scenarios.md) is een mechanisme van verbinding maken met Azure SQL Database en SQL Data Warehouse met behulp van identiteiten in Azure Active Directory (AD). Azure AD biedt een alternatief voor SQL Server-verificatie, zodat u ook de verspreiding van gebruikers-id's voor database-servers stoppen kunt. Azure AD-verificatie kunt u de identiteit van databasegebruikers en andere Microsoft-services op één centrale locatie centraal te beheren. Centrale ID-beheer biedt één plek voor het beheren van databasegebruikers en vereenvoudigt het beheer van machtigingen.  

Voordelen van het gebruik van Azure AD-verificatie in plaats van SQL-verificatie zijn:

- Kan de wisseling van het wachtwoord op één plek.
- Beheert de machtigingen van de database met behulp van de externe Azure AD-groepen.
- Voorkomen dat wachtwoorden moet opslaan te maken met geïntegreerde Windows-verificatie en andere vormen van authenticatie wordt ondersteund door Azure AD.
- Maakt gebruik van ingesloten databasegebruikers om identiteiten op databaseniveau te verifiëren.
- Biedt ondersteuning voor verificatie op basis van tokens voor toepassingen die verbinding maken met SQL-Database.
- Biedt ondersteuning voor AD FS (domein Federatie) of systeemeigen gebruikerswachtwoord verificatie voor een lokale Azure AD zonder synchronisatie van domein.
- Ondersteunt verbindingen van SQL Server Management Studio die gebruikmaken van Active Directory Universal-verificatie, waaronder [multi-factor Authentication (MFA)](../active-directory/authentication/multi-factor-authentication.md). MFA omvat een robuuste verificatie met een scala aan gebruiksvriendelijke verificatieopties, telefonische oproepen, SMS-bericht, smartcards en pincode of mobiele app-meldingen. Zie voor meer informatie, [SSMS-ondersteuning voor Azure AD MFA met SQL Database en SQL Data Warehouse](../sql-database/sql-database-ssms-mfa-authentication.md).

Zie voor meer informatie over Azure AD-verificatie:

- [Verbinding maken met SQL-Database of SQL datawarehouse met behulp van Azure Active Directory-verificatie](../sql-database/sql-database-aad-authentication.md)
- [Verificatie met Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-authentication.md)
- [Ondersteuning voor verificatie op basis van tokens voor Azure SQL DB met behulp van Azure AD-verificatie](https://blogs.msdn.microsoft.com/sqlsecurity/2016/02/09/token-based-authentication-support-for-azure-sql-db-using-azure-ad-auth/)

> [!NOTE]
> Om ervoor te zorgen dat Azure Active Directory geschikt is voor uw omgeving, Zie [Azure AD-functies en beperkingen](../sql-database/sql-database-aad-authentication.md#azure-ad-features-and-limitations), specifiek de aanvullende overwegingen.
>
>

### <a name="restrict-access-based-on-ip-address"></a>Beperken van toegang op basis van IP-adres
U kunt firewallregels die bereiken van acceptabele IP-adressen opgeven maken. Deze regels kunnen worden gericht op het niveau van de server en de database. We raden u aan met behulp van firewallregels op databaseniveau indien mogelijk om beveiliging te verbeteren en uw database draagbaarder te maken. Firewallregels op serverniveau worden best gebruikt voor beheerders en wanneer u veel databases met dezelfde toegangsvereisten hebt, maar u niet wilt dat elke database afzonderlijk configureren.

SQL-Database standaard bron IP-adresbeperkingen zodat toegang vanaf elk Azure-adres (met inbegrip van andere abonnementen en tenants). U kunt dit zodat alleen uw IP-adressen voor toegang tot de instantie te beperken. Zelfs met uw SQL-firewall en IP-adresbeperkingen is sterke verificatie nog steeds nodig. Zie de aanbevelingen die eerder in dit artikel.

Zie voor meer informatie over Azure SQL-Firewall en IP-beperkingen:

- [Access control van Azure SQL-Database](../sql-database/sql-database-control-access.md)
- [Configureer de firewallregels voor Azure SQL Database - overzicht](../sql-database/sql-database-firewall-configure.md)
- [Een Azure SQL Database-firewallregel op serverniveau met de Azure-portal configureren](../sql-database/sql-database-configure-firewall-settings.md)

### <a name="encryption-of-data-at-rest"></a>Versleuteling van data-at-rest
[Transparante gegevensversleuteling (TDE)](https://msdn.microsoft.com/library/azure/bb934049) is standaard ingeschakeld. TDE versleutelt transparant SQL Server, Azure SQL Database en Azure SQL Data Warehouse-gegevens en logboekbestanden bestanden. TDE beschermt tegen inbreuk op directe toegang tot de bestanden of hun back-up. Hiermee kunt u voor het versleutelen van gegevens in rust zonder te hoeven wijzigen van bestaande toepassingen. TDE moet altijd blijven ingeschakeld; Hiermee worden een aanvaller met behulp van de normale toegangspad echter niet gestopt. TDE biedt de mogelijkheid om te voldoen aan veel wetgeving, regelgeving en richtlijnen voor het tot stand gebracht in verschillende branches.

Azure SQL beheert belangrijke problemen voor TDE. Zoals met TDE, on-premises speciale aandacht moet worden genomen voor herstel en bij het verplaatsen van databases. In complexere scenario's, de sleutels kunnen worden expliciet beheerd in Azure Key Vault via extensible key management (Zie [TDE inschakelen op SQL Server met behulp van EKM](/security/encryption/enable-tde-on-sql-server-using-ekm)). Zo kan voor Bring Your Own Key (BYOK) via Azure Key Vaults BYOK-mogelijkheden.

Azure SQL biedt versleuteling voor kolommen via [Always Encrypted](/sql/relational-databases/security/encryption/always-encrypted-database-engine). Hiermee wordt alleen gemachtigde toepassingen toegang tot gevoelige kolommen. SQL-query's voor versleutelde kolommen om te vergelijken op basis van waarden met behulp van dit type versleuteling worden beperkt.

Versleuteling van toepassing op moet ook worden gebruikt voor de geselecteerde gegevens. Gegevens soevereiniteit problemen kunnen soms worden opgelost door het versleutelen van gegevens met een sleutel die wordt opgeslagen in de juiste land/regio. Dit voorkomt dat gegevens zelfs per ongeluk overdracht een probleem veroorzaken omdat het is niet mogelijk om de gegevens zonder de sleutel, ervan uitgaande dat het dat een sterke algoritme wordt gebruikt (zoals de AES-256) te ontsleutelen.

U kunt aanvullende maatregelen gebruiken om te helpen beveiligen van de database, zoals het ontwerpen van een beveiligde systeem, vertrouwelijke assets coderen en het bouwen van een firewall om de database-servers.

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u kennisgemaakt met een verzameling van SQL-Database en SQL Data Warehouse aanbevolen beveiligingsprocedures voor het beveiligen van uw PaaS-webtoepassingen en mobiele toepassingen. Zie voor meer informatie over het beveiligen van uw PaaS-implementaties:

- [PaaS-implementaties beveiligen](security-paas-deployments.md)
- [Beveiliging van de PaaS-webtoepassingen en mobiele toepassingen met behulp van Azure App Services](security-paas-applications-using-app-services.md)
