---
title: Aanbevolen beveiligingsprocedures voor Azure-database | Microsoft Docs
description: Dit artikel bevat een set van aanbevolen procedures voor beveiliging van de Azure-database.
services: security
documentationcenter: na
author: unifycloud
manager: mbaldwin
editor: tomsh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/20/2018
ms.author: tomsh
ms.openlocfilehash: 50cfc2e8420d9f427b02c739f497d8546d880d7c
ms.sourcegitcommit: 6678e16c4b273acd3eaf45af310de77090137fa1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2018
ms.locfileid: "50747759"
---
# <a name="azure-database-security-best-practices"></a>Aanbevolen beveiligingsprocedures voor Azure-database
De beveiliging is een belangrijkste aandachtspunt voor het beheren van databases en het is altijd een prioriteit voor [Azure SQL Database](https://docs.microsoft.com/azure/sql-database/). Uw databases kunnen nauw worden beveiligd om te voldoen aan meest regelgevingsbehoeften of beveiligingsvereisten, zoals HIPAA, ISO 27001/27002 en PCI DSS Level 1. Een huidige lijst van nalevingscertificeringen voor beveiliging is beschikbaar op de [Microsoft Trust Center site](http://azure.microsoft.com/support/trust-center/services/). U kunt ook uw databases in specifieke Azure-datacenters op basis van wettelijke eisen die plaatsen.

In dit artikel wordt besproken hoe een verzameling van aanbevolen beveiligingsprocedures voor Azure-database. Deze aanbevolen procedures zijn afgeleid van onze ervaring met beveiliging van de Azure database en de ervaringen van klanten zoals zelf.

Voor elke best practice wordt we het volgende uitgelegd:

-   Wat de beste manier is
-   Waarom u deze wilt inschakelen, die best practices
-   Wat kan het resultaat zijn als u een failover naar de aanbevolen procedure inschakelen
-   Hoe u meer informatie kunt krijgen om in te schakelen van de aanbevolen procedure

In dit artikel aanbevolen procedures voor beveiliging op Azure-Database is gebaseerd op een consensus advies en mogelijkheden van Azure-platform en functie wordt ingesteld als deze bestaan op het moment dat dit artikel is geschreven. Adviezen en -technologieën na verloop van tijd worden gewijzigd en regelmatig op basis van deze wijzigingen in dit artikel wordt bijgewerkt.

## <a name="use-firewall-rules-to-restrict-database-access"></a>Firewall-regels gebruiken om te beperken van toegang tot de database
Microsoft Azure SQL Database is een relationele databaseservice voor Azure en andere toepassingen op basis van internet. Om toegang te beveiligen, beheert SQL Database de toegang met:

- Firewall-regels die de connectiviteit door IP-adres beperken.
- Verificatiemechanismen die vereisen dat gebruikers hun identiteit kunnen aantonen.
- Autorisatiemechanismen die gebruikers tot bepaalde gegevens en acties beperken.

Totdat u opgeeft welke computers zijn gemachtigd, verhinderen firewalls alle toegang tot uw database-server. De firewall verleent toegang tot databases op basis van het IP-adres waar de aanvraag vandaan komt.

De volgende afbeelding ziet u waar u de firewall van een server in SQL-Database instellen:

![Firewall-regels](./media/azure-database-security-best-practices/azure-database-security-best-practices-Fig1.png)

De Azure SQL Database-service is alleen beschikbaar via TCP-poort 1433. Voor toegang tot een SQL-database van uw computer, moet u ervoor zorgen dat de firewall van uw clientcomputer uitgaande TCP-communicatie op TCP-poort 1433 toestaat. Blokkeer binnenkomende verbindingen op TCP-poort 1433 met behulp van firewallregels, als u geen deze verbindingen nodig voor andere toepassingen hebt.

Als onderdeel van het verbindingsproces worden verbindingen van virtuele Azure-machines omgeleid naar een IP-adres en poort op die uniek zijn voor elke werkrol. Het poortnummer ligt in het bereik van 11000 tot 11999. Zie voor meer informatie over TCP-poorten, [poorten boven 1433 voor ADO.NET 4.5](../sql-database/sql-database-develop-direct-route-ports-adonet-v12.md).

Zie [SQL Database-firewallregels](../sql-database/sql-database-firewall-configure.md) voor meer informatie over de firewallregels in SQL Database.

> [!Note]
> Naast de IP-regels beheert de firewall virtual network-regels. Virtual network-regels zijn gebaseerd op virtual network-service-eindpunten. Regels voor virtueel netwerk is mogelijk beter aan IP-regels in sommige gevallen. Zie voor meer informatie, [service-eindpunten voor virtuele netwerken en regels voor Azure SQL Database](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md).

## <a name="enable-database-authentication"></a>Database-verificatie inschakelen
SQL Database ondersteunt twee typen verificatie, SQL Server-verificatie en Azure AD-verificatie.

### <a name="sql-server-authentication"></a>*SQL Server-verificatie*

Voordelen zijn:

- Hierdoor kan SQL-Database voor de ondersteuning van omgevingen met verschillende besturingssystemen, waarbij alle gebruikers niet worden geverifieerd door een Windows-domein.
- Hiermee kunt u SQL-Database om oudere toepassingen en partner opgegeven toepassingen waarvoor SQL Server-verificatie te ondersteunen.
- Hiermee kunnen gebruikers verbinding maken vanaf onbekende of niet-vertrouwde domeinen. Een voorbeeld is een toepassing waar klanten tot stand gebrachte verbinding maken met SQL Server-aanmeldingen toegewezen voor het ontvangen van de status van hun orders.
- Kan SQL-Database voor de ondersteuning van webtoepassingen, waar gebruikers hun eigen identiteiten maken.
- Hiermee kunt u software-ontwikkelaars hun toepassingen distribueren met behulp van een complexe machtiging hiërarchie op basis van bekende, vooraf gedefinieerde SQL Server-aanmelding.

> [!NOTE]
> SQL Server-verificatie niet het Kerberos-beveiliging-protocol gebruiken.

Als u SQL Server-verificatie gebruikt, moet u:

- De sterke referenties beheren zelf.
- De referenties in de connection string worden beveiligd.
- De referenties die zijn doorgegeven via het netwerk van de webserver naar de database (potentieel) beveiligen. Zie voor meer informatie, [hoe: verbinding maken met SQL Server met behulp van SQL-verificatie in ASP.NET 2.0](/previous-versions/msp-n-p/ff648340(v=pandp.10)).

### <a name="azure-active-directory-ad-authentication"></a>*Verificatie met Azure Active Directory (AD)*
Azure AD-verificatie is een mechanisme van verbinding maken met Azure SQL Database en [SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) met behulp van identiteiten in Azure AD. Met Azure AD-verificatie, kunt u de identiteit van databasegebruikers en andere Microsoft-services op één centrale locatie kunt beheren. Centrale ID-beheer biedt één plek voor het beheren van databasegebruikers en vereenvoudigt het beheer van machtigingen.

> [!NOTE]
> We raden het gebruik van Azure AD-verificatie via het gebruik van SQL Server-verificatie.

Voordelen zijn:

- Het biedt een alternatief voor SQL Server-verificatie.
- Dit helpt bij het tegengaan van de verspreiding van gebruikers-id's in de database-servers.
- Hiermee kunt de wisseling van het wachtwoord op één plek.
- Klanten kunnen machtigingen voor database beheren met behulp van de groepen extern (Azure AD).
- Deze kunt wachtwoorden moet opslaan elimineren door in te schakelen van geïntegreerde Windows-verificatie en andere vormen van authenticatie wordt ondersteund door Azure Active Directory.
- Ingesloten databasegebruikers wordt gebruikt om identiteiten op databaseniveau te verifiëren.
- De service ondersteunt verificatie op basis van tokens voor toepassingen die verbinding met SQL-Database maken.
- Het ondersteunt AD FS (domein Federatie) of systeemeigen gebruikerswachtwoord verificatie voor een lokaal exemplaar van Azure Active Directory zonder synchronisatie van domein.
- Azure AD ondersteunt verbindingen van SQL Server Management Studio die gebruikmaken van Active Directory Universal-verificatie, waaronder multi-factor Authentication. Multi-factor Authentication biedt sterke verificatie met een scala aan opties voor verificatie: telefoonoproep, SMS-bericht, smartcards en PINCODE of mobiele app-meldingen. Zie voor meer informatie, [SSMS-ondersteuning voor Azure AD multi-factor Authentication met SQL Database en SQL Data Warehouse](../sql-database/sql-database-ssms-mfa-authentication.md).

De configuratiestappen zijn onder andere de volgende procedures om te configureren en gebruiken van Azure AD-verificatie:

- Maken en vullen van Azure AD.
- Optioneel: Koppelen of wijzigt u de Active Directory-exemplaar dat momenteel is gekoppeld aan uw Azure-abonnement.
- Een Azure Active Directory-beheerder voor Azure SQL Database maken of [Azure SQL Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/).
- Uw clientcomputers configureren.
- Maak ingesloten databasegebruikers in de database die is toegewezen aan Azure AD-identiteiten.
- Verbinding maken met uw database met behulp van Azure AD-identiteiten.

U kunt gedetailleerde informatie vinden in [gebruik Azure Active Directory-verificatie voor verificatie met SQL Database Managed Instance of SQL Data Warehouse](../sql-database/sql-database-aad-authentication.md).

## <a name="protect-your-data-by-using-encryption-and-row-level-security"></a>Uw gegevens beschermen met behulp van versleuteling en beveiliging op rijniveau
[Transparent data encryption voor Azure SQL-Database](../sql-database/transparent-data-encryption-azure-sql.md) beschermt gegevens op schijf en worden beschermd tegen onbevoegde toegang tot de hardware. Het voert realtime versleuteling en ontsleuteling van de database, gekoppelde back-ups en transactielogboekbestanden in rust zonder wijzigingen aan de toepassing. Transparante gegevensversleuteling versleutelt de opslag van een volledige database met behulp van een symmetrische sleutel met de naam de databaseversleutelingssleutel.

Zelfs als de volledige opslag wordt gecodeerd, is het belangrijk dat u ook het versleutelen van de database zelf. Dit is een implementatie van de verdediging in de diepte aanpak voor gegevensbeveiliging. Als u van Azure SQL Database gebruikmaakt en gevoelige gegevens (zoals een creditcard of sociaal-fiscale nummers bevatten) wilt beveiligen, kunt u databases met FIPS 140-2 gevalideerde 256-bits AES-versleuteling versleutelen. Deze versleuteling voldoet aan de vereisten van veel industrienormen (bijvoorbeeld, HIPAA en PCI).

Bestanden met betrekking tot [extensie (BPE) van de buffer](https://docs.microsoft.com/sql/database-engine/configure-windows/buffer-pool-extension) worden niet versleuteld wanneer u een database met behulp van transparante gegevensversleuteling versleutelen. U moet system-versleuteling op bestandsniveau hulpprogramma's zoals [BitLocker](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732774(v=ws.11)) of de [Encrypting File System (EFS)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc749610(v%3dws.10)) voor BPE-gerelateerde bestanden.

Omdat een geautoriseerde gebruiker, zoals een beveiligingsbeheerder of databasebeheerder van een toegang hebben tot de gegevens, zelfs als de database is versleuteld met transparante gegevensversleuteling, moet u ook deze aanbevelingen volgen:

- SQL Server-verificatie op databaseniveau inschakelen.
- Gebruik Azure AD-verificatie met behulp van [RBAC-rollen](../role-based-access-control/overview.md).
- Zorg ervoor dat gebruikers en toepassingen afzonderlijke accounts gebruiken om te verifiëren. Op deze manier kunt u de machtigingen verleend aan gebruikers en toepassingen beperken en het risico op schadelijke activiteiten worden verminderd.
- Op databaseniveau beveiliging implementeren met behulp van de vaste databaserollen (zoals db_datareader of db_datawriter). Of u kunt aangepaste functies voor uw toepassing naar expliciete machtigingen verlenen voor objecten in de geselecteerde database maken.

Andere manieren om uw gegevens te versleutelen die u kunt overwegen:

- [Versleuteling op celniveau](/sql/relational-databases/security/encryption/encrypt-a-column-of-data) om specifieke kolommen of zelfs cellen met gegevens met verschillende versleutelingssleutels te versleutelen.
- [Altijd versleuteld](/sql/relational-databases/security/encryption/always-encrypted-database-engine), waardoor clients voor het versleutelen van gevoelige gegevens binnen clienttoepassingen en nooit onthullen de versleutelingssleutels voor de Database-Engine (SQL-Database of SQL Server). Als gevolg hiervan Always Encrypted biedt een scheiding tussen personen die eigenaar zijn van de gegevens (en het kunnen bekijken) en degenen die de gegevens beheren (maar geen toegang mogen hebben).
- [Beveiliging op rijniveau](/sql/relational-databases/security/row-level-security), waardoor klanten voor het beheren van toegang tot rijen in een database-tabel op basis van de kenmerken van de gebruiker die een query wordt uitgevoerd. (Voorbeeld van de kenmerken zijn groep lidmaatschap en uitvoering van context).

Organisaties die van versleuteling op databaseniveau niet gebruikmaken mogelijk gevoeliger voor aanvallen die de gegevens in SQL-databases.

U kunt meer informatie over transparent data encryption voor SQL-Database lezen van het artikel [Transparent Data Encryption met Azure SQL Database](https://msdn.microsoft.com/library/0bf7e8ff-1416-4923-9c4c-49341e208c62.aspx).

## <a name="enable-database-auditing"></a>Databasecontrole inschakelen
Controle van een exemplaar van SQL Server Database Engine of een afzonderlijke database omvat het bijhouden van en het vastleggen van gebeurtenissen. Voor SQL Server, kunt u controles die de specificaties voor gebeurtenissen op serverniveau en specificaties voor gebeurtenissen op databaseniveau bevat maken. Gecontroleerde gebeurtenissen kunnen worden geschreven naar de logboeken van de gebeurtenis of om te controleren van bestanden.

Er zijn verschillende niveaus van controle-instellingen voor SQL Server, afhankelijk van de overheid of standaarden vereisten voor uw installatie. Controle van SQL Server biedt hulpprogramma's en processen voor het inschakelen, opslaan en weergeven van controles op verschillende-server en database-objecten.

[Met Azure SQL Database auditing](../sql-database/sql-database-auditing.md) databasegebeurtenissen bijgehouden en geschreven naar een auditlogboek in uw Azure storage-account.

Controles, kunt u de naleving van regelgeving, inzicht in de databaseactiviteiten en vinden discrepanties en afwijkingen die op zakelijke belangen of beveiligingsovertredingen wijzen kunnen. Controle vergemakkelijkt de naleving van standaarden voor compliance, maar biedt geen garantie voor naleving.

Zie voor meer informatie over het controleren van de database en het inschakelen ervan, [aan de slag met SQL database auditing](../sql-database/sql-database-auditing.md).

## <a name="enable-database-threat-detection"></a>Detectie van bedreigingen van database inschakelen
Beveiliging tegen bedreigingen gaat na detectie. Database threat protection bevat:

- Detecteren en classificeren van uw meest gevoelige gegevens, zodat u uw gegevens kunt beveiligen.
- Implementeren van veilige configuraties in uw database, zodat u uw database kunt beveiligen.
- Detecteren en reageren op potentiële dreigingen wanneer deze zich voordoen, zodat u snel kunt reageren en herstellen.

**Beste**: detecteren, classificeren en labelen van de gevoelige gegevens in uw databases.   
**Details**: classificeren van de gegevens in uw SQL-database door in te schakelen [gegevensdetectie en classificatie](../sql-database/sql-database-data-discovery-and-classification.md) in Azure SQL Database. U kunt toegang tot uw gevoelige gegevens in het Azure-dashboard te controleren of rapporten te downloaden.

**Beste**: databaseproblemen bijhouden, zodat u proactief de beveiliging van uw database kunt verbeteren.   
**Details**: Gebruik de Azure SQL Database [evaluatie van beveiligingsproblemen](../sql-database/sql-vulnerability-assessment.md) -service, die naar potentiële databaseproblemen zoekt. De service maakt gebruik van een kennisdatabase met regels die een vlag beveiligingsproblemen en afwijkingen van aanbevolen procedures, zoals onjuiste configuraties, overmatige machtigingen en niet-beveiligde gevoelige gegevens weergeven.

De regels worden op basis van best practices van Microsoft en u richten op de beveiligingsproblemen die de grootste risico's voor uw database en de bijbehorende waardevolle gegevens. Deze omvatten zowel problemen op databaseniveau en serverniveau beveiligingsproblemen, zoals server firewall-instellingen en machtigingen op serverniveau. Deze regels vertegenwoordigen veel van de vereisten van regelgevende instanties om te voldoen aan de standaarden voor compliance.

**Beste**: detectie van bedreigingen inschakelen.  
**Details**: Azure SQL-Database inschakelen [detectie van bedreigingen](../sql-database/sql-database-threat-detection.md) om op te halen van beveiligingswaarschuwingen en aanbevelingen voor het onderzoeken en bedreigingen te verhelpen. U ontvang waarschuwingen over verdachte databaseactiviteiten, potentiële kwetsbaarheden, SQL-injectieaanvallen en afwijkende patronen voor toegang en query's uitvoeren.

[Advanced Threat Protection](../sql-database/sql-advanced-threat-protection.md) is een geïntegreerde pakket voor geavanceerde mogelijkheden voor de beveiliging van SQL. Het bevat de eerder genoemde services: gegevensdetectie en classificatie, evaluatie van beveiligingsproblemen en detectie van bedreigingen. Het biedt één locatie voor het inschakelen en beheren van deze mogelijkheden.

Deze mogelijkheden inschakelen, kunt u:

- Aan de privacystandaarden en wettelijke vereisten.
- Toegang tot uw databases beheren en beperken van de beveiliging.
- Bewaken van een dynamische database-omgeving waarin wijzigingen moeilijk zijn te volgen.
- Detecteren en reageren op mogelijke bedreigingen.

Threat Detection integreert ook waarschuwingen met Azure Security Center voor een centraal overzicht van de beveiligingsstatus van al uw Azure-resources.

## <a name="next-steps"></a>Volgende stappen
Zie [Azure-beveiliging aanbevolen procedures en patronen](security-best-practices-and-patterns.md) voor meer best practices voor beveiliging moeten worden gebruikt wanneer bent u het ontwerpen, implementeren en beheren van uw cloudoplossingen met behulp van Azure.

De volgende resources zijn beschikbaar, voor meer algemene informatie over Azure-beveiliging en gerelateerde Microsoft-services:
* [Azure-Team Beveiligingsblog](https://blogs.msdn.microsoft.com/azuresecurity/) - voor up-to-date informatie over het laatste nieuws over Azure-beveiliging
* [Microsoft Security Response Center](https://technet.microsoft.com/library/dn440717.aspx) - hier Microsoft beveiligingsproblemen, met inbegrip van problemen met Azure, kunnen u ook via e-mail secure@microsoft.com
