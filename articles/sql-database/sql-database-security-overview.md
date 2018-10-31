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
ms.date: 10/22/2018
ms.openlocfilehash: 2a1d8a993f805c6ef814088af6fc4e3051519e37
ms.sourcegitcommit: 1d3353b95e0de04d4aec2d0d6f84ec45deaaf6ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50248792"
---
# <a name="an-overview-of-azure-sql-database-security-capabilities"></a>Een overzicht van de mogelijkheden van Azure SQL Database-beveiliging

In dit artikel worden de basisbeginselen van het beveiligen van de gegevenslaag van een toepassing met behulp van Azure SQL Database besproken. In het bijzonder, helpt in dit artikel u op weg met resources voor gegevensbeveiliging, toegangsbeheer en proactieve controle.

Zie het [Beveiligingscentrum voor SQL Server Database Engine en Azure SQL Database](https://msdn.microsoft.com/library/bb510589) voor een volledig overzicht van beveiligingsfuncties die beschikbaar zijn in alle versies van SQL. In het [technische rapport over beveiliging en Azure SQL Database](https://download.microsoft.com/download/A/C/3/AC305059-2B3F-4B08-9952-34CDCA8115A9/Security_and_Azure_SQL_Database_White_paper.pdf) (PDF-Bestand) is ook aanvullende informatie beschikbaar.

## <a name="protect-data"></a>Gegevens beveiligen

### <a name="encryption"></a>Versleuteling

Uw gegevens worden beveiligd met SQL Database via gegevensversleuteling. Voor gegevens die in beweging zijn, wordt [Transport Layer Security](https://support.microsoft.com/kb/3135244) gebruikt, voor data-at-rest [Transparent Data Encryption](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) en voor gebruikte gegevens [Altijd versleuteld](https://msdn.microsoft.com/library/mt163865.aspx).

> [!IMPORTANT]
> Azure SQL-Database wordt afgedwongen versleuteling (SSL/TLS) op alle tijden voor alle verbindingen die garandeert dat alle gegevens worden versleuteld 'in transit' tussen de database en de client. Dit gebeurt, ongeacht de instelling van **versleutelen** of **TrustServerCertificate** in de verbindingsreeks.
>
> Zorg ervoor dat u opgeeft dat een versleutelde verbinding in de verbindingsreeks van uw toepassing, en *niet* het servercertificaat vertrouwen (dit is voor de ADO.NET-stuurprogramma **Encrypt = True** en  **TrustServerCertificate = False**). Dit helpt voorkomen dat uw toepassing een man in de middelste aanval door af te dwingen van de toepassing om te controleren of de server en de uitvoerende versleuteling. Als u de verbindingsreeks vanuit Azure portal hebt verkregen, is er de juiste instellingen.
>
> Zie voor meer informatie over de TLS- en -connectiviteit [TLS-overwegingen](sql-database-connect-query.md#tls-considerations-for-sql-database-connectivity)

Andere manieren om uw gegevens te versleutelen die u kunt overwegen:

- [Versleuteling op celniveau](https://msdn.microsoft.com/library/ms179331.aspx) om specifieke kolommen of zelfs cellen met gegevens met verschillende versleutelingssleutels te versleutelen.
-  Als u een Hardware Security Module of Bring Your Own Key (BYOK)-technologie voor Transparent Data Encryption nodig hebt, kunt u overwegen [Azure SQL Transparent Data Encryption: Bring Your Own Key support](transparent-data-encryption-byok-azure-sql.md).

### <a name="data-discovery--classification"></a>Gegevensdetectie en classificatie

Gegevensdetectie en -classificatie (momenteel in preview) biedt geavanceerde mogelijkheden van ingebouwd in Azure SQL Database voor het detecteren, classificeren, labelen en beveiligen van de gevoelige gegevens in uw databases. Detecteren en classificeren van uw zoveel mogelijk gevoelige gegevens (business/financiële, gezondheidszorg, persoonlijke gegevens, enzovoort) kan een cruciale rol spelen in uw organisatie Information protection waarmee kan worden voldaan. Deze kan dienen als infrastructuur voor:

- Verschillende security-scenario's, zoals bewaking (controle) en waarschuwingen voor afwijkende toegang tot gevoelige gegevens.
- Toegang tot beheren en beperken van de beveiliging van databases met uiterst gevoelige gegevens.
- Helpen voldoen aan de privacystandaarden en wettelijke vereisten.

Zie voor meer informatie, [aan de slag met SQL DB-gegevensdetectie en classificatie](sql-database-data-discovery-and-classification.md).

## <a name="control-access"></a>Toegang beheren

Uw gegevens worden beveiligd met SQL Database door de toegang tot uw database te beperken met behulp van firewallregels, verificatiemechanismen die vereisen dat gebruikers hun identiteit bewijzen, en autorisatie voor gegevens via lidmaatschappen en machtigingen op basis van rollen. Ook wordt gebruikgemaakt van beveiliging op rijniveau en dynamische gegevensmaskering. Zie [Toegang beheren](sql-database-control-access.md) voor een discussie over het gebruik van functies voor toegangsbeheer in SQL Database.

> [!IMPORTANT]
> Het beheer van databases en logische servers in Azure wordt bepaald door de roltoewijzingen voor uw Portal-gebruikersaccount. Zie voor meer informatie over dit artikel [rollen gebaseerd toegangsbeheer in Azure portal](../role-based-access-control/overview.md).

### <a name="firewall-and-firewall-rules"></a>Firewall en firewallregels

Met firewalls wordt met behulp van [firewallregels](sql-database-firewall-configure.md) alle toegang tot uw databaseserver beperkt om uw gegevens te helpen beveiligen, totdat u opgeeft welke computers zijn gemachtigd. De firewall verleent toegang tot databases op basis van het IP-adres waar de aanvraag vandaan komt.

### <a name="authentication"></a>Verificatie

SQL Database-verificatie verwijst naar hoe u uw identiteit bewijst bij het maken van verbinding met de database. SQL Database ondersteunt twee typen verificatie:

- **SQL-verificatie**

  Deze verificatiemethode maakt gebruik van een gebruikersnaam en wachtwoord. Wanneer u de logische server voor uw database hebt gemaakt, hebt u een aanmelding 'serverbeheerder' opgegeven met een gebruikersnaam en wachtwoord. Met deze aanmeldingsgegevens kunt u zich bij elke database op die server als de database-eigenaar of 'dbo' verifiëren.

- **Azure Active Directory-verificatie**

  Deze verificatiemethode maakt gebruik van identiteiten die worden beheerd door Azure Active Directory en wordt ondersteund voor beheerde en geïntegreerde domeinen. Gebruik [waar mogelijk](https://msdn.microsoft.com/library/ms144284.aspx) Active Directory-verificatie (geïntegreerde beveiliging). Als u Azure Active Directory-verificatie wilt gebruiken, moet u een andere serverbeheerder maken, de 'Azure AD-beheerder' genaamd, die Azure AD-gebruikers en -groepen kan beheren. Deze beheerder kan ook alle bewerkingen uitvoeren die reguliere serverbeheerders kunnen uitvoeren. Zie [Verbinding maken met SQL Database met behulp van Azure Active Directory-verificatie](sql-database-aad-authentication.md) voor een overzicht van het maken van een Azure AD-beheerder om Azure Active Directory-verificatie in te schakelen.

### <a name="authorization"></a>Autorisatie

Autorisatie verwijst naar wat een gebruiker kan doen binnen een Azure SQL Database. Dit wordt bepaald door de databaserollidmaatschappen en objectmachtigingen van uw gebruikersaccount. Het wordt aanbevolen om gebruikers de minimaal benodigde bevoegdheden te verlenen. Het serverbeheerdersaccount waarmee u verbinding maakt is lid van db_owner, die geautoriseerd is om alle bewerkingen binnen de database uit te voeren. Gebruik dit account voor het implementeren van schema-updates en andere beheerbewerkingen. Gebruik het 'ApplicationUser'-account met beperktere machtigingen om vanuit van uw toepassing verbinding te maken met de database met de minste bevoegdheden die nodig zijn voor uw toepassing.

### <a name="row-level-security"></a>Beveiliging op rijniveau

Beveiliging op rijniveau kunnen klanten voor het beheren van toegang tot rijen in een database-tabel op basis van de kenmerken van de gebruiker die een query uitvoert (bijvoorbeeld groep groepslidmaatschap of uitvoeringscontext context). Zie [Beveiliging op rijniveau](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) voor meer informatie.

### <a name="dynamic-data-masking"></a>Dynamische gegevensmaskering

SQL Database dynamische gegevensmaskering blootstelling van gevoelige gegevens door deze te maskeren voor niet-gemachtigde gebruikers. Dynamische gegevensmaskering automatisch detecteert mogelijk gevoelige gegevens in Azure SQL Database en worden bruikbare aanbevelingen gedaan om te maskeren van deze velden, met minimale gevolgen voor de toepassingslaag. Dit werkt als volgt: de gevoelige gegevens in de resultatenset van een query die is uitgevoerd op toegewezen databasevelden, worden bedekt, terwijl de gegevens in de database niet worden gewijzigd. Zie voor meer informatie, [aan de slag met SQL Database dynamische gegevensmaskering](sql-database-dynamic-data-masking-get-started.md).

## <a name="proactive-monitoring"></a>Proactieve controle

Uw gegevens worden beveiligd met SQL Database met behulp van mogelijkheden voor controle en detectie van dreigingen.

### <a name="auditing"></a>Controleren

SQL Database Auditing houdt databaseactiviteiten bij en helpt u bij de naleving van regelgeving door databasegebeurtenissen vast te leggen in een auditlogboek in uw Azure Storage-account. Door te controleren krijgt u informatie over actieve databaseactiviteiten. Het helpt u ook bij het analyseren en onderzoeken van activiteiten uit het verleden. Op deze manier kunt u mogelijke dreigingen, vermoedelijk misbruik of schendingen van de beveiliging identificeren. Zie [Aan de slag met SQL Database Auditing](sql-database-auditing.md) voor meer informatie.  

### <a name="threat-detection"></a>Detectie van bedreigingen

Controle, door te geven van een extra laag met beveiligingsinformatie die is ingebouwd in de Azure SQL Database-service die ongebruikelijke en potentieel schadelijke pogingen om toegang tot of misbruik te maken van databases detecteert wordt aangevuld door detectie van bedreigingen. U wordt gewaarschuwd bij verdachte activiteiten, potentiële kwetsbaarheden, en SQL-injectie aanvallen, en afwijkende patronen voor databasetoegang. Waarschuwingen van Threat Detection kunnen worden bekeken via [Azure Security Center](https://azure.microsoft.com/services/security-center/) en Geef details op van verdachte activiteiten en geven aanbevelingen voor het onderzoeken en tegenhouden. Detectie van bedreigingen kost $15/server/maand. Het is gratis voor de eerste 60 dagen. Zie [Aan de slag met SQL Database Threat Detection](sql-database-threat-detection.md) voor meer informatie.

## <a name="compliance"></a>Naleving

Naast de bovenstaande functies en functionaliteit waarmee u kunt uw toepassing ook te voldoen aan verschillende beveiligingsvereisten, Azure SQL Database wordt regelmatig gecontroleerd en is gecertificeerd volgens diverse nalevingsstandaarden. Zie het [Vertrouwenscentrum van Microsoft Azure](https://azure.microsoft.com/support/trust-center/), waar u de meest recente lijst met [SQL Database-nalevingscertificeringen](https://www.microsoft.com/trustcenter/compliance/complianceofferings) vindt, voor meer informatie.

## <a name="security-management"></a>Beveiligingsbeheer

SQL Database helpt u bij het beheren van de beveiliging van uw gegevens door te geven van de database-scans en een gecentraliseerde beveiliging dashboard met [evaluatie van beveiligingsproblemen SQL](sql-vulnerability-assessment.md).

**[Evaluatie van beveiligingsproblemen SQL](sql-vulnerability-assessment.md)**  is een eenvoudig te configureren dat is ingebouwd in Azure SQL Database waarmee u ontdekken, volgen en herstellen van potentiële databaseproblemen. De evaluatie wordt uitgevoerd een scan door een beveiligingslek in uw database en genereert een rapport dat u meer inzicht in uw beveiligingsstatus hebt, inclusief stappen voor het oplossen van beveiligingsproblemen met zich mee en verbeter de beveiliging van uw database. Het evaluatierapport kan worden aangepast voor uw omgeving door in te stellen van een acceptabele basislijn voor de machtiging configuraties, configuratie en database-instellingen. Dit kan u te helpen:

- Voldoen aan nalevingsvereisten die nodig Scanrapporten database hebben.
- Aan de privacystandaarden voor gegevens.
- Bewaken van een dynamische database-omgeving waarin wijzigingen zijn moeilijk zijn om bij te houden.

Zie voor meer informatie, [evaluatie van beveiligingsproblemen SQL](sql-vulnerability-assessment.md).

## <a name="next-steps"></a>Volgende stappen

- Zie [Toegang beheren](sql-database-control-access.md) voor een discussie over het gebruik van functies voor toegangsbeheer in SQL Database.
- Zie voor een bespreking van de Databasecontrole [SQL Database auditing](sql-database-auditing.md).
- Zie voor een bespreking van de detectie van bedreigingen, [SQL Database threat detection](sql-database-threat-detection.md).
