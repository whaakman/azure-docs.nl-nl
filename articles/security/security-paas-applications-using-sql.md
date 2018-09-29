---
title: PaaS-Databases in Azure beveiligen | Microsoft Docs
description: 'Meer informatie over de beveiliging van Azure SQL Database en SQL Data Warehouse aanbevolen procedures voor het beveiligen van uw PaaS-webtoepassingen en mobiele toepassingen. '
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
ms.date: 09/28/2018
ms.author: terrylan
ms.openlocfilehash: 72d5ec09becc1f1d9e23e284e18bcc037ccb3072
ms.sourcegitcommit: f31bfb398430ed7d66a85c7ca1f1cc9943656678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/28/2018
ms.locfileid: "47452476"
---
# <a name="best-practices-for-securing-paas-databases-in-azure"></a>Aanbevolen procedures voor het beveiligen van PaaS-databases in Azure

In dit artikel bespreken we een verzameling van [Azure SQL Database](../sql-database/sql-database-technical-overview.md) en [SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) aanbevolen beveiligingsprocedures voor het beveiligen van uw platform-as-a-service (PaaS)-webtoepassingen en mobiele toepassingen. Deze aanbevolen procedures zijn afgeleid van onze ervaring met Azure en de ervaringen van klanten zoals zelf.

Azure SQL Database en SQL Data Warehouse bieden een relationele databaseservice voor uw toepassingen op basis van het internet. Laten we kijken services om uw toepassingen en gegevens beschermen bij het gebruik van Azure SQL Database en SQL Data Warehouse in een PaaS-implementatie:

- Azure Active Directory-verificatie (in plaats van SQL Server-verificatie)
- Azure SQL-firewall
- Transparante gegevensversleuteling (TDE)

## <a name="use-a-centralized-identity-repository"></a>Gebruik een gecentraliseerde identiteitsopslagplaats
Azure SQL-databases kunnen worden geconfigureerd voor het gebruik van een van twee typen verificatie:

- **SQL-verificatie** maakt gebruik van een gebruikersnaam en wachtwoord. Wanneer u de logische server voor uw database hebt gemaakt, hebt u een aanmelding 'serverbeheerder' opgegeven met een gebruikersnaam en wachtwoord. Met deze referenties, kunt u verifiëren met elke database op die server als de database-eigenaar.

- **Azure Active Directory-verificatie** maakt gebruik van identiteiten die worden beheerd door Azure Active Directory en wordt ondersteund voor beheerde en geïntegreerde domeinen. Voor het gebruik van Azure Active Directory-verificatie, moet u een andere serverbeheerder met de naam ' Azure AD-beheerder,' die is toegestaan voor het beheer van Azure AD-gebruikers en groepen maken. Deze beheerder kan ook alle bewerkingen uitvoeren die reguliere serverbeheerders kunnen uitvoeren.

[Azure Active Directory-verificatie](../active-directory/develop/authentication-scenarios.md) is een mechanisme van verbinding maken met Azure SQL Database en SQL Data Warehouse met behulp van identiteiten in Azure Active Directory (AD). Azure AD biedt een alternatief voor SQL Server-verificatie, zodat u ook de verspreiding van gebruikers-id's voor database-servers stoppen kunt. Azure AD-verificatie kunt u de identiteit van databasegebruikers en andere Microsoft-services op één centrale locatie centraal te beheren. Centrale ID-beheer biedt één plek voor het beheren van databasegebruikers en vereenvoudigt het beheer van machtigingen.  

### <a name="benefits-of-using-azure-ad-instead-of-sql-authentication"></a>Voordelen van het gebruik van Azure AD in plaats van SQL-verificatie
- Kan de wisseling van het wachtwoord op één plek.
- Beheert de machtigingen van de database met behulp van de externe Azure AD-groepen.
- Voorkomen dat wachtwoorden moet opslaan te maken met geïntegreerde Windows-verificatie en andere vormen van authenticatie wordt ondersteund door Azure AD.
- Maakt gebruik van ingesloten databasegebruikers om identiteiten op databaseniveau te verifiëren.
- Biedt ondersteuning voor verificatie op basis van tokens voor toepassingen die verbinding maken met SQL-Database.
- Biedt ondersteuning voor federatieve domein met Active Directory Federation Services (ADFS) of systeemeigen gebruikerswachtwoord verificatie voor een lokale Azure AD-Tenant zonder synchronisatie van domein.
- Ondersteunt verbindingen van SQL Server Management Studio die gebruikmaken van Active Directory Universal-verificatie, waaronder [multi-factor Authentication (MFA)](../active-directory/authentication/multi-factor-authentication.md). MFA omvat een robuuste verificatie met een scala aan gebruiksvriendelijke verificatieopties, telefonische oproepen, SMS-bericht, smartcards en pincode of mobiele app-meldingen. Zie voor meer informatie, [universele authenticatie met SQL-Database en SQL Data Warehouse](../sql-database/sql-database-ssms-mfa-authentication.md).

Zie voor meer informatie over Azure AD-verificatie:

- [Azure Active Directory-verificatie gebruiken voor verificatie met SQL Database Managed Instance of SQL Data Warehouse](../sql-database/sql-database-aad-authentication.md)
- [Verificatie met Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-authentication.md)
- [Ondersteuning voor verificatie op basis van tokens voor Azure SQL DB met behulp van Azure AD-verificatie](../sql-database/sql-database-aad-authentication.md)

> [!NOTE]
> Om ervoor te zorgen dat Azure Active Directory geschikt is voor uw omgeving, Zie [Azure AD-functies en beperkingen](../sql-database/sql-database-aad-authentication.md#azure-ad-features-and-limitations).
>
>

## <a name="restrict-access-based-on-ip-address"></a>Beperken van toegang op basis van IP-adres
U kunt firewallregels die bereiken van acceptabele IP-adressen opgeven maken. Deze regels kunnen worden gericht op het niveau van de server en de database. We raden u aan met behulp van firewallregels op databaseniveau indien mogelijk om beveiliging te verbeteren en uw database draagbaarder te maken. Firewallregels op serverniveau worden best gebruikt voor beheerders en wanneer u veel databases met dezelfde toegangsvereisten hebt, maar u niet wilt dat elke database afzonderlijk configureren.

SQL Database standaard bron IP-adresbeperkingen zodat toegang vanaf elk Azure-adres, met inbegrip van andere abonnementen en tenants. U kunt dit zodat alleen uw IP-adressen voor toegang tot de instantie te beperken. Zelfs met uw SQL-firewall en IP-adresbeperkingen is sterke verificatie nog steeds nodig. Zie de aanbevelingen die eerder in dit artikel.

Zie voor meer informatie over Azure SQL-Firewall en IP-beperkingen:

- [Toegangsbeheer voor Azure SQL Database en SQL Data Warehouse](../sql-database/sql-database-control-access.md)
- [Azure SQL-Database en SQL Data Warehouse firewall-regels](../sql-database/sql-database-firewall-configure.md)


## <a name="encrypt-data-at-rest"></a>Gegevens in rust versleutelen
[Transparante gegevensversleuteling (TDE)](/sql/relational-databases/security/encryption/transparent-data-encryption) is standaard ingeschakeld. TDE versleutelt transparant SQL Server, Azure SQL Database en Azure SQL Data Warehouse-gegevens en logboekbestanden bestanden. TDE beschermt tegen inbreuk op directe toegang tot de bestanden of hun back-up. Hiermee kunt u voor het versleutelen van gegevens in rust zonder te hoeven wijzigen van bestaande toepassingen. TDE moet altijd blijven ingeschakeld; Hiermee worden een aanvaller met behulp van de normale toegangspad echter niet gestopt. TDE biedt de mogelijkheid om te voldoen aan veel wetgeving, regelgeving en richtlijnen voor het tot stand gebracht in verschillende branches.

Azure SQL beheert belangrijke problemen voor TDE. Zoals met TDE, on-premises speciale aandacht moet worden genomen voor herstel en bij het verplaatsen van databases. In complexere scenario's, kunnen de sleutels worden expliciet beheerd in Azure Key Vault via uitbreidbaar Sleutelbeheer. Zie [inschakelen TDE op SQL Server met behulp van EKM](/sql/relational-databases/security/encryption/enable-tde-on-sql-server-using-ekm). Zo kan voor Bring Your Own Key (BYOK) via Azure Key Vaults BYOK-mogelijkheden.

Azure SQL biedt versleuteling voor kolommen via [Always Encrypted](/sql/relational-databases/security/encryption/always-encrypted-database-engine). Hiermee wordt alleen gemachtigde toepassingen toegang tot gevoelige kolommen. SQL-query's voor versleutelde kolommen om te vergelijken op basis van waarden met behulp van dit type versleuteling worden beperkt.

Versleuteling van toepassing op moet ook worden gebruikt voor de geselecteerde gegevens. Gegevens soevereiniteit problemen kunnen soms worden opgelost door het versleutelen van gegevens met een sleutel die wordt opgeslagen in de juiste land/regio. Dit voorkomt dat gegevens zelfs per ongeluk overdracht een probleem veroorzaken omdat het is niet mogelijk om de gegevens zonder de sleutel, ervan uitgaande dat het dat een sterke algoritme wordt gebruikt (zoals de AES-256) te ontsleutelen.

U kunt aanvullende maatregelen gebruiken om te helpen beveiligen van de database, zoals het ontwerpen van een beveiligde systeem, vertrouwelijke assets coderen en het bouwen van een firewall om de database-servers.

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u kennisgemaakt met een verzameling van SQL-Database en SQL Data Warehouse aanbevolen beveiligingsprocedures voor het beveiligen van uw PaaS-webtoepassingen en mobiele toepassingen. Zie voor meer informatie over het beveiligen van uw PaaS-implementaties:

- [PaaS-implementaties beveiligen](security-paas-deployments.md)
- [Beveiliging van de PaaS-webtoepassingen en mobiele toepassingen met behulp van Azure App Services](security-paas-applications-using-app-services.md)
