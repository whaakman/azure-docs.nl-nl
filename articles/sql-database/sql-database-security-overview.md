---
title: Overzicht van Azure SQL Database-beveiliging| Microsoft Docs
description: Meer informatie over de beveiliging van een Azure SQL Database en SQL Server, met inbegrip van de verschillen tussen de cloud en on-premises SQL Server.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: aliceku
ms.author: aliceku
ms.reviewer: vanto, carlrab, emlisa
manager: craigg
ms.date: 02/04/2019
ms.openlocfilehash: a4f1b26a20da3b22561538f7814105b356c4148a
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58519133"
---
# <a name="an-overview-of-azure-sql-database-security-capabilities"></a>Een overzicht van de mogelijkheden van Azure SQL Database-beveiliging

In dit artikel bevat een overzicht van de basisbeginselen van het beveiligen van de gegevenslaag van een toepassing met behulp van Azure SQL Database. De beveiligingsstrategie die worden beschreven de gelaagde defense-in-depth-aanpak volgt, zoals wordt weergegeven in de onderstaande afbeelding, en wordt verplaatst van buitenaf in:

![sql-security-layer.png](media/sql-database-security-overview/sql-security-layer.png)

## <a name="network-security"></a>Netwerkbeveiliging

Microsoft Azure SQL Database is een relationele databaseservice voor cloud- en zakelijke toepassingen. Ter bescherming van gegevens van de klant, verhinderen firewalls toegang tot het netwerk met de databaseserver totdat toegang worden nadrukkelijk de machtigingen op basis van IP-adres of Azure Virtual network-verkeer oorsprong.

### <a name="ip-firewall-rules"></a>IP-firewallregels

IP-firewallregels verlenen toegang tot databases op basis van het oorspronkelijke IP-adres van elke aanvraag. Zie voor meer informatie, [overzicht van Azure SQL Database en SQL Data Warehouse firewallregels](sql-database-firewall-configure.md).

### <a name="virtual-network-firewall-rules"></a>Virtual network-firewallregels

[Service-eindpunten](../virtual-network/virtual-network-service-endpoints-overview.md) uitbreiden van uw verbinding met het virtuele netwerk via de Azure-backbone en Azure SQL Database om te identificeren van het virtuele netwerksubnet dat verkeer van afkomstig is uit te schakelen. Als u wilt dat verkeer bereiken van Azure SQL Database, gebruikt u de SQL [servicetags](../virtual-network/security-overview.md) waarmee uitgaand verkeer via Netwerkbeveiligingsgroepen.

[Regels voor virtueel netwerk](sql-database-vnet-service-endpoint-rule-overview.md) inschakelen van Azure SQL Database om alleen communicatie die zijn verzonden vanaf de geselecteerde subnetten binnen een virtueel netwerk te accepteren.

> [!NOTE]
> Beheren van toegang met firewallregels heeft *niet* zijn van toepassing op **een beheerd exemplaar**. Zie voor meer informatie over de netwerkconfiguratie die nodig zijn, [verbinding te maken met een beheerd exemplaar](sql-database-managed-instance-connect-app.md)

## <a name="access-management"></a>Toegangsbeheer

> [!IMPORTANT]
> Beheer van databases en database-servers in Azure wordt bepaald door de roltoewijzingen van de account van uw portal-gebruiker. Zie voor meer informatie over dit artikel [rollen gebaseerd toegangsbeheer in Azure portal](../role-based-access-control/overview.md).

### <a name="authentication"></a>Authentication

Verificatie is het proces van de gebruiker aan te tonen die zij beweren te zijn. Azure SQL Database ondersteunt twee typen verificatie:

- **SQL-verificatie**:

    SQL database-verificatie verwijst naar de verificatie van een gebruiker bij het verbinden met [Azure SQL Database](sql-database-technical-overview.md) met gebruikersnaam en wachtwoord. Tijdens het maken van de server database voor de database moet een aanmelding 'Serverbeheerder' met een gebruikersnaam en wachtwoord worden opgegeven. Met deze referenties, een 'serverbeheerder' kan worden geverifieerd bij elke database op die databaseserver als de database-eigenaar. Hierna kunnen aanvullende SQL-aanmeldingen en gebruikers worden gemaakt door de serverbeheerder, waarmee gebruikers verbinding kunnen maken met gebruikersnaam en wachtwoord.

- **Azure Active Directory-verificatie**:

    Azure Active Directory-verificatie is een mechanisme van verbinding maken met [Azure SQL Database](sql-database-technical-overview.md) en [SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) met behulp van identiteiten in Azure Active Directory (Azure AD). Azure AD-verificatie kan beheerders de identiteiten en de machtigingen van gebruikers van de database, samen met andere Microsoft-services op één centrale locatie centraal beheren. Dit omvat de afhankelijk van de wachtwoorden worden opgeslagen en kan centrale rotatiehoek van het wachtwoordbeleid.

     De serverbeheerder van een met de naam de **Active Directory-beheerder** moet worden gemaakt voor het gebruik van Azure AD-verificatie met SQL Database. Zie voor meer informatie, [verbinding te maken met SQL Database met behulp van Azure Active Directory-verificatie](sql-database-aad-authentication.md). Azure AD-verificatie ondersteunt zowel beheerde als federatieve-accounts. De federatieve accounts ondersteuning voor Windows-gebruikers en groepen voor een klant-domein dat gefedereerd met Azure AD.

    Extra Azure AD-verificatie-opties die beschikbaar zijn [Universal verificatie van Active Directory voor SQL Server Management Studio](sql-database-ssms-mfa-authentication.md) verbindingen met inbegrip van [multi-Factor Authentication](../active-directory/authentication/concept-mfa-howitworks.md) en [ Voorwaardelijke toegang](sql-database-conditional-access.md).

> [!IMPORTANT]
> Beheer van databases en servers in Azure wordt bepaald door de roltoewijzingen van de account van uw portal-gebruiker. Zie voor meer informatie over dit artikel [rollen gebaseerd toegangsbeheer in Azure portal](../role-based-access-control/overview.md). Beheren van toegang met firewallregels heeft *niet* zijn van toepassing op **een beheerd exemplaar**. Raadpleeg het volgende artikel over [verbinding te maken met een beheerd exemplaar](sql-database-managed-instance-connect-app.md) voor meer informatie over welke netwerkconfiguratie is vereist.

Autorisatie verwijst naar de machtigingen worden toegewezen aan een gebruiker binnen een Azure SQL Database, en bepaalt wat de gebruiker mag doen. Machtigingen worden bepaald door het toevoegen van gebruikersaccounts aan [databaserollen](/sql/relational-databases/security/authentication-access/database-level-roles) waarmee u de machtigingen op databaseniveau of het verlenen van de gebruiker bepaalde [objectmachtigingen](/sql/relational-databases/security/permissions-database-engine). Zie voor meer informatie, [aanmeldingen en gebruikers](sql-database-manage-logins.md)

Als een best practice, moet u gebruikers toevoegen aan de rol met de minste bevoegdheden die nodig is voor hun functie. Het serverbeheerdersaccount is lid van de rol db_owner, die beschikt over uitgebreide machtigingen en moet worden verleend aan gebruikers voorzichtig. Wanneer u toepassingen met Azure SQL Database, gebruiken [toepassingsrollen](/sql/relational-databases/security/authentication-access/application-roles) met beperkte machtigingen. Dit zorgt ervoor dat de toepassing die verbinding maken met de database de minimaal benodigde bevoegdheden die nodig zijn voor de toepassing is.

### <a name="row-level-security"></a>Beveiliging op rijniveau

Beveiliging op rijniveau kunnen klanten voor het beheren van toegang tot rijen in een database-tabel op basis van de kenmerken van de gebruiker die een query uitvoert (bijvoorbeeld groep groepslidmaatschap of uitvoeringscontext context). Zie [Beveiliging op rijniveau](/sql/relational-databases/security/row-level-security) voor meer informatie.

![azure-database-rls.png](media/sql-database-security-overview/azure-database-rls.png)

  Deze verificatiemethode maakt gebruik van een gebruikersnaam en wachtwoord. 

Zie voor een overzicht van machtigingen in Azure SQL Database, [aanmeldingen en gebruikers](sql-database-manage-logins.md#permissions)

## <a name="threat-protection"></a>Bescherming tegen bedreigingen

SQL-Database voor beveiliging van gegevens van de klant door de mogelijkheden voor controle en bedreigingen detectie.

### <a name="sql-auditing-in-azure-monitor-logs-and-event-hubs"></a>SQL-controle in Azure Monitor-logboeken en Event Hubs

SQL Database auditing houdt databaseactiviteiten bij en zorgt voor naleving van beveiligingsstandaarden door databasegebeurtenissen naar een auditlogboek in een klanten van Azure storage-account. Controle, kunnen gebruikers actieve bewaken, evenals analyseren en onderzoeken van de historische activiteit voor het identificeren van mogelijke bedreigingen of vermoedelijke schendingen van misbruik en beveiliging. Voor meer informatie, Zie het Engelstalige artikel [SQL Database Auditing](sql-database-auditing.md).  

### <a name="threat-detection"></a>Detectie van bedreigingen

Threat detection verbetert de controle door een analyse van controlelogboeken voor het gedrag van ongebruikelijke en potentieel schadelijke probeert te openen of misbruik te maken van databases. Waarschuwingen worden voor verdachte activiteiten of afwijkende toegangspatronen, zoals SQL-injectieaanvallen, potentiële gegevens is binnengedrongen en wachtwoord beveiligingsaanvallen gemaakt. Meldingen voor geconstateerde bedreigingen worden weergegeven in de [Azure Security Center](https://azure.microsoft.com/services/security-center/), waarbij de details van de verdachte activiteiten zijn opgegeven en aanbevelingen voor nader onderzoek samen met de acties die aan het oplossen van de bedreiging. Detectie van bedreigingen kost $15/server/maand. Het is gratis voor de eerste 60 dagen. Zie voor meer informatie, [aan de slag met SQL Database Threat detection](sql-database-threat-detection.md).

![azure-database-td.jpg](media/sql-database-security-overview/azure-database-td.jpg)

## <a name="information-protection-and-encryption"></a>Gegevensbescherming en -versleuteling

### <a name="transport-layer-security-tls-encryption-in-transit"></a>Transport Layer Security TLS (versleuteling-in-transit)

SQL-Database beveiligt de gegevens van de klant door het versleutelen van gegevens in beweging met [Transport Layer Security](https://support.microsoft.com/help/3135244/tls-1-2-support-for-microsoft-sql-server).

SQL Server afgedwongen versleuteling (SSL/TLS) op alle tijden voor alle verbindingen. Dit zorgt ervoor dat alle gegevens worden versleuteld 'in transit' tussen de client en server, ongeacht de instelling van **versleutelen** of **TrustServerCertificate** in de verbindingsreeks.

Als een best practice aangeraden die in de verbinding van uw toepassing tekenreeks u een versleutelde verbinding opgeven en _**niet**_ het servercertificaat vertrouwen. Dit zorgt ervoor dat de uw toepassing om te controleren of het servercertificaat en zo wordt voorkomen dat uw toepassing worden vatbaar voor man in het middelste aanvallen.

Bijvoorbeeld bij het gebruik van het stuurprogramma ADO.NET dit wordt bereikt **Encrypt = True** en **TrustServerCertificate = False**. Als u de verbindingsreeks vanuit Azure portal hebt verkregen, is er de juiste instellingen.

> [!IMPORTANT]
> Houd er rekening mee dat bepaalde niet-Microsoft-stuurprogramma's mogelijk maken standaard gebruik van TLS of afhankelijk van een oudere versie van TLS zijn (< 2.0) om te kunnen werken. In dit geval SQL Server nog steeds kunt u verbinding maakt met uw database. We raden echter aan dat u het beveiligingsrisico van zodat deze stuurprogramma's en de toepassing verbinding maken met SQL-Database, met name als u gevoelige gegevens opslaat evalueren. 
>
> Zie voor meer informatie over de TLS- en -connectiviteit [TLS-overwegingen](sql-database-connect-query.md#tls-considerations-for-sql-database-connectivity)

### <a name="transparent-data-encryption-encryption-at-rest"></a>Transparent Data Encryption (versleuteling-at-rest)

[Transparante gegevensversleuteling (TDE) voor Azure SQL Database](transparent-data-encryption-azure-sql.md) voegt een beveiligingslaag om gegevens in rust tegen onbevoegde of offline toegang tot onbewerkte bestanden of back-ups te beveiligen. Algemene scenario's omvatten datacenter diefstal of niet-beveiligde verwijdering van hardware en media, zoals harde schijven en back-uptapes. TDE versleutelt de gehele database met behulp van een versleutelingsalgoritme AES, waarmee ontwikkelaars van toepassingen kunnen wijzigingen aanbrengen in bestaande toepassingen niet vereist.

Alle gemaakte SQL-databases worden standaard versleuteld in Azure, en de databaseversleutelingssleutel wordt beveiligd door een ingebouwde servercertificaat.  Certificaat onderhoud en rotatie worden beheerd door de service en vereist geen invoer van de gebruiker. Klanten die liever overnemen van de versleutelingssleutels kunnen de sleutels in beheren [Azure Key Vault](../key-vault/key-vault-secure-your-key-vault.md).

### <a name="key-management-with-azure-key-vault"></a>Sleutelbeheer met Azure Key Vault

[Bring Your Own Key](transparent-data-encryption-byok-azure-sql.md) (BYOK) ondersteuning voor [Transparent Data Encryption](/sql/relational-databases/security/encryption/transparent-data-encryption) (TDE) kan klanten eigenaar worden van Sleutelbeheer en het gebruik van de rotatie [Azure Key Vault](../key-vault/key-vault-secure-your-key-vault.md), Azure de cloudgebaseerde externe sleutelbeheersysteem. Als van de database-toegang tot de key vault is ingetrokken, kunnen een database kan niet worden ontsleuteld en lezen in het geheugen. Azure Key Vault biedt een platform voor Centraal beheer, maakt gebruik van nauw bewaakte hardware security modules (HSM's), en kunt scheiding van functies tussen het beheer van sleutels en gegevens om te voldoen aan nalevingsvereisten voor beveiliging.

### <a name="always-encrypted-encryption-in-use"></a>Altijd versleuteld (versleuteling in gebruik)

![azure-database-ae.png](media/sql-database-security-overview/azure-database-ae.png)

[Altijd versleuteld](/sql/relational-databases/security/encryption/always-encrypted-database-engine) is een functie die is ontworpen om gevoelige gegevens die zijn opgeslagen in de kolommen van toegang tot specifieke database (bijvoorbeeld, creditcardnummers, nationale identificatienummers of gegevens op een _moet weten_ basis). Dit omvat databasebeheerders of andere gebruikers met bevoegdheden die zijn gemachtigd voor toegang tot de database voor beheertaken uitvoeren, maar er zijn geen bedrijven nodig hebben tot de specifieke gegevens in de versleutelde kolommen hebben. De gegevens worden altijd versleuteld, wat betekent dat de versleutelde gegevens ontsleuteld alleen voor verwerking door clienttoepassingen met toegang tot de versleutelingssleutel.  De versleutelingssleutel wordt nooit blootgesteld aan SQL en kunnen worden opgeslagen in de [Windows certificaat Store](sql-database-always-encrypted.md) of in [Azure Key Vault](sql-database-always-encrypted-azure-key-vault.md).

### <a name="masking"></a>Maskeren

![azure-database-ddm.png](media/sql-database-security-overview/azure-database-ddm.png)

#### <a name="dynamic-data-masking"></a>Dynamische gegevensmaskering

SQL Database dynamische gegevensmaskering blootstelling van gevoelige gegevens door deze te maskeren voor niet-gemachtigde gebruikers. Dynamische gegevensmaskering automatisch detecteert mogelijk gevoelige gegevens in Azure SQL Database en worden bruikbare aanbevelingen gedaan om te maskeren van deze velden, met minimale gevolgen voor de toepassingslaag. Dit werkt als volgt: de gevoelige gegevens in de resultatenset van een query die is uitgevoerd op toegewezen databasevelden, worden bedekt, terwijl de gegevens in de database niet worden gewijzigd. Zie voor meer informatie, [aan de slag met SQL Database dynamische gegevensmaskering](sql-database-dynamic-data-masking-get-started.md).

#### <a name="static-data-masking"></a>Statische gegevens maskeren

[Statische gegevens maskeren](/sql/relational-databases/security/static-data-masking) is een hulpprogramma voor client-side beschikbaar in [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms) 18,0 preview 5 en hoger.  Statische gegevens maskeren Hiermee kunnen gebruikers een kopie maken van een database waar de gegevens in de geselecteerde kolommen definitief zijn gemaskeerd. De van beschikbare maskeringsfuncties zijn NULL maskeren, één waarde maskeren, shuffle en groep shuffle maskeren en samengestelde maskering tekenreeks. Organisaties zijn met de gemaskeerd kopie van de database, kunnen afzonderlijke productie- en testomgevingen door te delen de gemaskeerd kopie. De gevoelige gegevens voldoende is beveiligd en alle andere kenmerken uit de database is behouden. Databases maskeren, wordt u aangeraden waarbij externe toegang tot databases moeten worden bijgehouden.

## <a name="security-management"></a>Beveiligingsbeheer

### <a name="vulnerability-assessment"></a>Evaluatie van beveiligingsproblemen

[Evaluatie van beveiligingsproblemen](sql-vulnerability-assessment.md) is een eenvoudig te configureren van de service die u kunt detecteren, volgen en te herstellen van potentiële databaseproblemen met het doel proactief algemene om databasebeveiliging te verbeteren. Evaluatie van beveiligingsproblemen (VA) maakt deel uit van de geavanceerde beveiliging (AD)-aanbieding, dit een geïntegreerde-pakket voor geavanceerde mogelijkheden voor de beveiliging van SQL is. Evaluatie van beveiligingsproblemen kan worden geopend en worden beheerd via de centrale SQL AD-portal.

### <a name="data-discovery--classification"></a>Gegevensdetectie en -classificatie

Gegevensdetectie en -classificatie (momenteel in preview) biedt geavanceerde mogelijkheden van ingebouwd in Azure SQL Database voor het detecteren, classificeren, labelen en beveiligen van de gevoelige gegevens in uw databases. Detecteren en classificeren van uw zoveel mogelijk gevoelige gegevens (business/financiële, gezondheidszorg, persoonlijke gegevens, enzovoort) kunnen een cruciale rol spelen in uw organisatie Information protection waarmee kan worden voldaan. Deze kan dienen als infrastructuur voor:

- Verschillende security-scenario's, zoals bewaking (controle) en waarschuwingen voor afwijkende toegang tot gevoelige gegevens.
- Toegang tot beheren en beperken van de beveiliging van databases met uiterst gevoelige gegevens.
- Helpen voldoen aan de privacystandaarden en wettelijke vereisten.

Zie voor meer informatie, [aan de slag met gegevensdetectie en classificatie](sql-database-data-discovery-and-classification.md).

### <a name="compliance"></a>Naleving

Naast de bovenstaande functies en functionaliteit waarmee u kunt uw toepassing ook te voldoen aan verschillende beveiligingsvereisten, Azure SQL Database wordt regelmatig gecontroleerd en is gecertificeerd volgens diverse nalevingsstandaarden. Zie het [Vertrouwenscentrum van Microsoft Azure](https://azure.microsoft.com/support/trust-center/), waar u de meest recente lijst met [SQL Database-nalevingscertificeringen](https://www.microsoft.com/trustcenter/compliance/complianceofferings) vindt, voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

- Zie [Toegang beheren](sql-database-control-access.md) voor een discussie over het gebruik van functies voor toegangsbeheer in SQL Database.
- Zie voor een bespreking van de Databasecontrole [SQL Database auditing](sql-database-auditing.md).
- Zie voor een bespreking van de detectie van bedreigingen, [SQL Database threat detection](sql-database-threat-detection.md).
