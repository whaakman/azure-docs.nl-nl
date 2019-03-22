---
title: Overzicht van Azure-database-beveiliging | Microsoft Docs
description: Dit artikel bevat een overzicht van de Azure-database-beveiligingsfuncties.
services: security
documentationcenter: na
author: UnifyCloud
manager: barbkess
editor: TomSh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/30/2018
ms.author: TomSh
ms.openlocfilehash: 7e0e93c82279ec1a4fbecbbf27c7a1866286b2f8
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57860513"
---
# <a name="azure-database-security-overview"></a>Overzicht van de beveiliging van Azure-database

De beveiliging is een belangrijkste aandachtspunt voor het beheren van databases en het is altijd een prioriteit voor Azure SQL Database. Azure SQL Database biedt ondersteuning voor beveiliging van de verbinding met firewallregels en verbindingsversleuteling. Het ondersteunt verificatie met gebruikersnaam en wachtwoord en verificatie van Azure Active Directory (Azure AD), dat gebruikmaakt van identiteiten die worden beheerd door Azure Active Directory. Autorisatie maakt gebruik van op rollen gebaseerd toegangsbeheer.

Azure SQL Database biedt ondersteuning voor versleuteling door het uitvoeren van real-time versleuteling en ontsleuteling van databases, gekoppelde back-ups en transactielogboekbestanden in rust zonder wijzigingen aan de toepassing.

Microsoft biedt extra manieren voor het enterprise-gegevens versleutelen:

-   Versleuteling op celniveau is beschikbaar voor het versleutelen van specifieke kolommen of zelfs cellen met gegevens met verschillende versleutelingssleutels.
-   Als u een hardware security module of Centraal beheer van uw versleutelingssleutelhiërarchie nodig hebt, overweeg het gebruik van Azure Key Vault met SQL Server in een Azure-machine (VM).
-   Altijd maakt versleuteld (momenteel in preview) versleuteling transparant voor toepassingen. Daarnaast kunt u clients voor het versleutelen van gevoelige gegevens binnen clienttoepassingen zonder het delen van de versleutelingssleutels met SQL Database.

Azure SQL Database Auditing kunnen ondernemingen record Meld u gebeurtenissen naar een auditlogboek in Azure Storage. SQL Database Auditing kan ook worden geïntegreerd met Microsoft Power BI om het inzoomen op rapporten en analyses mogelijk te maken.

Azure SQL-databases kunnen nauw worden beveiligd om te voldoen aan meest regelgevende of beveiligingsvereisten, zoals HIPAA, ISO 27001/27002 en PCI DSS Level 1. Een huidige lijst van nalevingscertificeringen voor beveiliging is beschikbaar op de [Microsoft Azure Trust Center site](https://azure.microsoft.com/support/trust-center/services/).

In dit artikel worden de basisbeginselen besproken van het beveiligen van Microsoft Azure SQL-databases voor gestructureerde, in tabelvorm en relationele gegevens. Dit artikel helpt u in het bijzonder om aan de slag te gaan met resources voor gegevensbeveiliging, toegangsbeheer en proactieve controle.

## <a name="protection-of-data"></a>Beveiliging van gegevens

SQL Database helpt uw gegevens beveiligd door te bieden van versleuteling:

- Voor gegevens in beweging via [Transport Layer Security (TLS)](https://support.microsoft.com/kb/3135244).
- Voor gegevens in rust via [transparante gegevensversleuteling](https://go.microsoft.com/fwlink/?LinkId=526242).
- Voor gegevens in gebruik door [Always Encrypted](https://msdn.microsoft.com/library/mt163865.aspx).

Andere manieren om uw gegevens te versleutelen die u kunt overwegen:

-   [Versleuteling op celniveau](https://msdn.microsoft.com/library/ms179331.aspx) om specifieke kolommen of zelfs cellen met gegevens met verschillende versleutelingssleutels te versleutelen.
-   [Azure Key Vault met SQL Server in een Azure-VM](https://blogs.technet.com/b/kv/archive/2015/01/12/using-the-key-vault-for-sql-server-encryption.aspx), als u een hardware security module of Centraal beheer van uw versleutelingssleutelhiërarchie nodig hebt.

### <a name="encryption-in-motion"></a>Versleuteling in beweging

Een veelvoorkomend probleem voor alle client/server-toepassingen is de noodzaak van privacy als gegevens worden verplaatst via openbare en particuliere netwerken. Als gegevens verplaatst via een netwerk is niet versleuteld, is er een kans dat kan worden vastgelegd en diefstal door onbevoegde gebruikers. Wanneer u bent om met een database-services, zorg ervoor dat gegevens worden versleuteld tussen de databaseclient en server. Zorg ook dat gegevens worden versleuteld tussen database-servers die communiceren met elkaar en met toepassingen van de middelste laag.

Een probleem bij het beheren van een netwerk is de beveiliging van gegevens die tussen toepassingen via een niet-vertrouwd netwerk worden verzonden. U kunt [TLS/SSL](https://docs.microsoft.com/windows-server/security/tls/transport-layer-security-protocol) om te verifiëren van servers en clients, en vervolgens worden gebruikt voor het versleutelen van berichten tussen de geverifieerde partijen.

Een TLS/SSL-client verzendt tijdens het verificatieproces een bericht naar een TLS/SSL-server. De server reageert met de informatie die de server nodig heeft om zichzelf te verifiëren. De client en server een extra uitwisseling van sessiesleutels uitvoeren en de verificatiedialoog wordt beëindigd. Als verificatie is voltooid, kan SSL beveiligde communicatie tussen de server en de client gestart via de symmetrische versleutelingssleutels die tijdens het verificatieproces zijn bepaald.

Alle verbindingen met Azure SQL Database vereisen versleuteling (TLS/SSL) bij alle tijden terwijl gegevens 'in transit' naar en van de database. SQL-Database maakt gebruik van TLS/SSL om te verifiëren van servers en clients en vervolgens worden gebruikt voor het versleutelen van berichten tussen de geverifieerde partijen. 

In de verbindingsreeks van uw toepassing, moet u parameters voor het versleutelen van de verbinding en niet voor het servercertificaat vertrouwen. (Dit doet u als u de verbindingsreeks buiten de Azure-portal kopiëren.) Anders wordt de verbinding niet controleert of de identiteit van de server en is vatbaar voor 'man-in-the-middle'-aanvallen. Voor het stuurprogramma ADO.NET bijvoorbeeld deze Verbindingsreeksparameters zijn `Encrypt=True` en `TrustServerCertificate=False`.

### <a name="encryption-at-rest"></a>Versleuteling 'at rest'

U kunt verschillende voorzorgsmaatregelen nemen om te beveiligen van de database. Bijvoorbeeld, een beveiligd systeem ontwerpen, vertrouwelijke assets coderen en bouwen van een firewall om de database-servers. Maar in een scenario waarin de fysieke media (zoals stations of back-uptapes) worden gestolen, een schadelijke partij kunt alleen herstellen of koppel de database en de gegevens bladeren.

Eén oplossing is het versleutelen van gevoelige gegevens in de database en de sleutels die worden gebruikt voor het versleutelen van de gegevens met een certificaat te beschermen. Deze oplossing voorkomt personen zonder de sleutels met behulp van de gegevens, maar dit type beveiliging moet worden gepland.

Om op te lossen dit probleem, SQL Server en SQL Database-ondersteuning [transparante gegevensversleuteling](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql?view=azuresqldb-current&viewFallbackFrom=sql-server-2017). Transparante gegevensversleuteling worden de SQL Server en SQL Database-gegevensbestanden, bekend als van Versleutelingsgegevens in rust versleuteld.

Transparante gegevensversleuteling biedt bescherming tegen de dreiging van schadelijke activiteiten. Het voert realtime versleuteling en ontsleuteling van de database, gekoppelde back-ups en transactielogboekbestanden in rust zonder wijzigingen aan de toepassing.  

Transparante gegevensversleuteling versleutelt de opslag van een volledige database met behulp van een symmetrische sleutel met de naam de databaseversleutelingssleutel. In SQL-Database, wordt de versleutelingssleutel van de database beveiligd door een ingebouwde servercertificaat. Het certificaat van de ingebouwde is uniek voor elke SQL-Database-server.

Als een database in een Geo-DR-relatie is, wordt het beveiligd door een andere productcode op elke server. Als de twee databases zijn verbonden met dezelfde server, delen ze hetzelfde ingebouwde certificaat. Microsoft draait automatisch deze certificaten ten minste elke 90 dagen. 

Zie voor meer informatie, [transparante gegevensversleuteling](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-tde).

### <a name="encryption-in-use-client"></a>Versleuteling wordt gebruikt (client)

De meeste gegevensschendingen hebben betrekking op diefstal van kritieke gegevens, zoals creditcardnummers of persoonlijk identificeerbare informatie. Databases kunnen waardevolle troves van gevoelige informatie zijn. Ze kunnen ook persoonsgegevens (zoals nationale identificatienummers), vertrouwelijke gegevens over de concurrentie en intellectueel eigendom van klanten bevatten. Verloren of gestolen gegevens, met name klantgegevens, kan resulteren in merk schade, concurrerende nadeel en ernstige boetes--zelfs rechtszaken.

![De functie altijd versleuteld als een vergrendeling en de sleutel wordt geïllustreerd](./media/azure-databse-security-overview/azure-database-fig1.png)

[Altijd versleuteld](https://msdn.microsoft.com/library/mt163865.aspx) is een functie die is ontworpen voor het beveiligen van gevoelige gegevens die zijn opgeslagen in Azure SQL Database of SQL Server-databases. Altijd kunt versleutelde clients voor het versleutelen van gevoelige gegevens binnen clienttoepassingen en nooit onthullen de versleutelingssleutels voor de database-engine (SQL-Database of SQL Server).

Altijd biedt versleutelde een scheiding tussen personen die eigenaar zijn van de gegevens (en het kunnen bekijken) en de mensen die de gegevens beheren (maar geen toegang mogen hebben). Dit zorgt ervoor dat on-premises database beheerders, operators voor cloud-database of andere verhoogde maar niet-geautoriseerde gebruikers geen toegang de versleutelde gegevens tot.

Bovendien maakt Always Encrypted versleuteling transparant voor toepassingen. Een stuurprogramma Always Encrypted-functionaliteit is geïnstalleerd op de clientcomputer zodat deze automatisch versleutelen en ontsleutelen van gevoelige gegevens in de clienttoepassing. Het stuurprogramma versleutelt de gegevens in gevoelige kolommen voordat de gegevens doorgegeven aan de database-engine. Het stuurprogramma herschrijft automatisch query's, zodat de semantiek voor de toepassing blijven behouden. Het stuurprogramma ontsleutelt op dezelfde manier transparant gegevens, opgeslagen in de versleutelde databasekolommen, die is opgenomen in de queryresultaten.

## <a name="access-control"></a>Toegangsbeheer

Voor de beveiliging regelt SQL Database toegang met behulp van:

- Firewall-regels die de connectiviteit door IP-adres beperken.
- Verificatiemechanismen die vereisen dat gebruikers hun identiteit kunnen aantonen.
- Autorisatiemechanismen die gebruikers tot bepaalde gegevens en acties beperken.

### <a name="database-access"></a>Toegang tot de database

De beveiliging van gegevens begint met het beheren van toegang tot uw gegevens. Het datacenter dat als host fungeert voor uw gegevens beheert de fysieke toegang. U kunt een firewall voor het beheren van beveiliging in de netwerklaag configureren. U kunt ook toegang beheren door aanmeldingen voor verificatie configureren en het definiëren van machtigingen voor de server en database.

#### <a name="firewall-and-firewall-rules"></a>Firewall en firewallregels

[Azure SQL Database](https://azure.microsoft.com/services/sql-database/) is een relationele databaseservice voor Azure en andere toepassingen op basis van het internet. Om uw gegevens te beschermen, verhinderen firewalls alle toegang tot de databaseserver totdat u opgeeft welke computers zijn gemachtigd. De firewall verleent toegang tot databases op basis van het IP-adres waar de aanvraag vandaan komt. Zie [Overzicht van de firewallregels voor SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) voor meer informatie.

De Azure SQL Database-service is alleen beschikbaar via TCP-poort 1433. Voor toegang tot een SQL-database van uw computer, moet u ervoor zorgen dat de firewall van uw clientcomputer uitgaande TCP-communicatie op TCP-poort 1433 toestaat. Als binnenkomende verbindingen niet nodig zijn voor andere toepassingen, kunt u ze op TCP-poort 1433 blokkeren.

#### <a name="authentication"></a>Authentication

Verificatie verwijst naar hoe u uw identiteit bewijst bij het maken van verbinding met de database. SQL Database ondersteunt twee typen verificatie:

-   **SQL Server-verificatie**: Bij het maken van een logische SQL-instantie wordt er een account voor enkelvoudige aanmelding gemaakt, 'account van de SQL Database-abonnee' genoemd. Dit account maakt verbinding met behulp van [SQL Server-verificatie](https://docs.microsoft.com/azure/sql-database/sql-database-security-overview) (gebruikersnaam en wachtwoord). Het account fungeert als beheerder voor de logische serverinstantie en voor alle gebruikersdatabases die hieraan zijn gekoppeld. De machtigingen van het abonnee-account kunnen niet worden beperkt. Er kan slechts één van deze accounts bestaan.
-   **Azure Active Directory-verificatie**: [Azure AD-verificatie](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) is een mechanisme van verbinding maken met Azure SQL Database en Azure SQL Data Warehouse met behulp van identiteiten in Azure AD. U kunt deze gebruiken om de identiteit van databasegebruikers centraal te beheren.

![Azure AD-verificatie met SQL Database](./media/azure-databse-security-overview/azure-database-fig2.png)

 Voordelen van Azure AD-verificatie zijn onder andere:
  - Het biedt een alternatief voor SQL Server-verificatie.
  - Het helpt bij het tegengaan van de verspreiding van gebruikers-id's in de database-servers en wisseling van het wachtwoord op één plek kunt.
  - U kunt machtigingen voor database beheren met behulp van de groepen extern (Azure AD).
  - Deze kunt wachtwoorden moet opslaan elimineren door in te schakelen van geïntegreerde Windows-verificatie en andere vormen van die ondersteuning biedt voor Azure AD-verificatie.

#### <a name="authorization"></a>Autorisatie

[Autorisatie](https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins) verwijst naar wat een gebruiker binnen een Azure SQL database doen kan. Dit wordt bepaald door de database van uw gebruikersaccount [rollidmaatschappen](https://msdn.microsoft.com/library/ms189121) en [objectmachtigingen](https://msdn.microsoft.com/library/ms191291.aspx). Autorisatie is het proces voor het bepalen van welke beveiligbare resources hebben toegang tot een principal en welke bewerkingen zijn toegestaan voor deze resources.

### <a name="application-access"></a>Toegang tot toepassingen

#### <a name="dynamic-data-masking"></a>Dynamische gegevensmaskering

Een medewerker van de op een callcenter mogelijk aanroepers identificeren door verschillende cijfers van hun sociaal-fiscaal nummer of creditcardnummer. Maar deze gegevensitems niet volledig worden blootgesteld aan de klantenservice.

U kunt een maskeringsregel die alles behalve de laatste vier cijfers van een sociaal-fiscaal nummer of creditcardnummer in de resultatenset van een query maskeert definiëren.

![Maskering op een aantal verzonden tussen een SQL database- en business-apps](./media/azure-databse-security-overview/azure-database-fig3.png)

Als een ander voorbeeld: een masker voor de juiste gegevens ter bescherming van persoonlijke gegevens worden gedefinieerd. Een ontwikkelaar kan vervolgens opvragen voor productie-omgevingen voor het oplossen van problemen zonder te schenden van de regelgeving.

Met [dynamische gegevensmaskering in SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) wordt de blootstelling van gevoelige gegevens beperkt door deze gegevens te maskeren voor niet-gemachtigde gebruikers. Dynamische gegevensmaskering wordt ondersteund voor de V12-versie van Azure SQL Database.

[Dynamische gegevensmaskering](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking) helpt voorkomen dat onbevoegde toegang tot gevoelige gegevens doordat u aangeven hoeveel van de gevoelige gegevens worden vrijgegeven, met minimale impact op de toepassingslaag. Dit is een beveiligingsfunctie op basis van beleid. De gevoelige gegevens in de resultatenset van een query die is uitgevoerd op toegewezen databasevelden worden verborgen, terwijl de gegevens in de database niet worden gewijzigd.

> [!Note]
> Dynamische gegevensmaskering kan worden geconfigureerd door de beheerder van de Azure-Database, -serverbeheerder of officer van beveiligingsrollen.

#### <a name="row-level-security"></a>Beveiliging op rijniveau

Een andere algemene beveiligingsvereiste voor meerdere tenants databases is [Row-Level Security](https://msdn.microsoft.com/library/dn765131.aspx). U kunt deze functie gebruiken voor het beheren van toegang tot rijen in een database-tabel op basis van de kenmerken van de gebruiker die een query wordt uitgevoerd. (Voorbeeld van de kenmerken zijn groep lidmaatschap en uitvoering van context).

![Beveiliging op rijniveau zodat een gebruiker toegang tot rijen in een tabel via een client-app](./media/azure-databse-security-overview/azure-database-fig4.png)

De logica van de beperking van toegang is gevonden in de databaselaag in plaats van opslag van de gegevens in een andere toepassingslaag. De database-systeem is van toepassing de toegangsbeperkingen telkens wanneer die toegang tot gegevens van elke laag wordt uitgevoerd. Dit maakt het beveiligingssysteem betrouwbaarder en robuuste door te verminderen van het gebied van uw beveiligingssysteem.

Beveiliging op rijniveau introduceert toegangsbeheer op basis van een predicaat. Biedt een flexibele, gecentraliseerde evaluatie kunt uitvoeren in overweging metagegevens of andere criteria die de beheerder bepaalt indien van toepassing. Het predicaat wordt gebruikt als een criterium om te bepalen of de gebruiker de juiste toegang heeft tot de gegevens op basis van gebruikerskenmerken heeft. U kunt toegangsbeheer op basis van een label kunt implementeren met behulp van toegangsbeheer op basis van een predicaat.

## <a name="proactive-monitoring"></a>Proactieve controle

SQL Database helpt uw gegevens worden beveiligd door te geven *controle* en *detectie van bedreigingen* mogelijkheden.

### <a name="auditing"></a>Controleren

[Met Azure SQL Database auditing](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) verhoogt u de mogelijkheid meer inzicht krijgen in gebeurtenissen en wijzigingen die binnen de database plaatsvinden. Voorbeelden zijn updates en query's op basis van de gegevens.

SQL Database auditing worden databasegebeurtenissen bijgehouden en geschreven naar een auditlogboek in uw Azure storage-account database. Controles, kunt u de naleving van regelgeving, begrijpen databaseactiviteiten en inzicht krijgen in discrepanties en afwijkingen die kunnen wijzen op problemen voor het bedrijf of vermoedelijke beveiligingsschendingen. Controle kunt en vergemakkelijkt de naleving van standaarden voor compliance, maar biedt geen garantie voor naleving.

U kunt SQL Database auditing om te gebruiken:

- **Behouden** een audittrail van de geselecteerde gebeurtenissen. Categorieën van de databaseacties moeten worden gecontroleerd, kunt u definiëren.
- **Rapport** op database-activiteit. U kunt vooraf geconfigureerde rapporten en een dashboard snel aan de slag met de activiteit en rapportage.
- **Analyseren** rapporten. U kunt verdachte gebeurtenissen, ongebruikelijke activiteiten en trends vinden.

Er zijn twee methoden voor controle:

-   **Controlefunctie voor blobs**: Logboeken worden geschreven naar Azure Blob Storage. Dit is een methode om nieuwere controle. Dit biedt betere prestaties, biedt ondersteuning voor hogere granulariteit op objectniveau en rendabeler.
-   **Tabelcontrole**: Logboeken worden geschreven naar Azure Table storage.

### <a name="threat-detection"></a>Detectie van bedreigingen

[Advanced Threat Protection voor Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-advanced-threat-protection) detecteert verdachte activiteiten die duiden op potentiële beveiligingsrisico's. Wanneer deze zich voordoen, kunt u detectie van bedreigingen te reageren op verdachte gebeurtenissen in de database, zoals SQL-injecties. Het biedt waarschuwingen en het gebruik van Azure SQL Database auditing als u wilt de verdachte gebeurtenissen onderzoeken toestaat.

![Detectie van bedreigingen voor SQL Database en een web-app met een externe aanvaller en een kwaadwillende insider](./media/azure-databse-security-overview/azure-database-fig5.jpg)

SQL geavanceerde Threat Protection (ATP) biedt een geavanceerde SQL-beveiligingsmogelijkheden, met inbegrip van gegevensdetectie en classificatie, evaluatie van beveiligingsproblemen en detectie van bedreigingen. 

- [Gegevensdetectie en classificatie](../sql-database/sql-database-data-discovery-and-classification.md)
- [Evaluatie van beveiligingsproblemen](../sql-database/sql-vulnerability-assessment.md)  
- [Detectie van bedreigingen](../sql-database/sql-database-threat-detection.md)

[Azure Database for PostgreSQL Advanced Threat Protection](../postgresql/concepts-data-access-and-security-threat-protection.md) biedt een nieuwe beveiligingslaag, waarmee u om te detecteren en op mogelijke bedreigingen reageert zodra ze zich voordoen, dankzij beveiligingswaarschuwingen over afwijkende activiteiten. Gebruikers ontvangen een waarschuwing bij verdachte databaseactiviteiten, en mogelijke beveiligingsproblemen, evenals afwijkende toegang en query's patronen. Advanced Threat Protection voor Azure Database for PostgreSQL integreert waarschuwingen met Azure Security Center. Het type waarschuwingen zijn onder andere:

- Toegang vanaf ongebruikelijke locatie
- Toegang vanaf ongebruikelijk Azure-datacentrum 
- Toegang vanaf ongebruikelijke 
- Toegang vanaf een mogelijk schadelijke toepassing 
- Brute force Azure database voor PostgreSQL-referenties 

[Azure Database for MySQL Advanced Threat Protection](../mysql/concepts-data-access-and-security-threat-protection.md) biedt beveiliging die vergelijkbaar is met geavanceerde beveiliging voor PostgreSQL.  

## <a name="centralized-security-management"></a>Gecentraliseerd beheer

[Azure Security Center](https://azure.microsoft.com/documentation/services/security-center/) helpt u bedreigingen te voorkomen, te detecteren en erop te reageren. Het biedt geïntegreerde beveiligingsbewaking en beleidsbeheer voor uw Azure-abonnementen. Het helpt bedreigingen detecteren die anders onopgemerkt en het werkt met een uitgebreid ecosysteem van beveiligingsoplossingen.

[Security Center](https://docs.microsoft.com/azure/security-center/security-center-sql-database) helpt u uw gegevens in SQL Database beveiligen door inzicht in de beveiliging van uw servers en databases te bieden. Met Security Center, kunt u het volgende doen:

- Hiermee definieert u beleid voor controle en versleuteling van de SQL-Database.
- Bewaak de beveiliging van SQL Database-resources in al uw abonnementen.
- Snel identificeren en oplossen van beveiligingsproblemen met zich mee.
- Integreer waarschuwingen van [Azure SQL Database threat detection](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection).

Security Center biedt ondersteuning voor toegang op basis van rollen.

## <a name="sql-information-protection"></a>SQL Information Protection

[SQL Information Protection](../sql-database/sql-database-data-discovery-and-classification.md) automatisch worden gedetecteerd en classificeert potentieel gevoelige gegevens, biedt een mechanisme labelen voor gevoelige gegevens met de classificatie-kenmerken permanent tagging en biedt een gedetailleerde dashboard wordt weergegeven de status van de classificatie van de database.  

Bovendien berekent het resultaat gevoeligheid van de SQL-query's, zo instellen dat query's die gevoelige gegevens extraheren expliciet kunnen worden gecontroleerd en de gegevens kunnen worden beveiligd. Zie voor meer informatie over SQL Information Protection, Azure SQL Database-gegevensdetectie en classificatie.

U kunt configureren [SQL Information Protection-beleid](../security-center/security-center-info-protection-policy.md) in Azure Security Center.

## <a name="azure-marketplace"></a>Azure Marketplace

Azure Marketplace is een onlinemarktplaats voor toepassingen en services waar beginnende en onafhankelijke softwareleveranciers hun oplossingen kunnen aanbieden aan Azure-klanten over de hele wereld.
De Azure Marketplace combineert Microsoft Azure-partnerecosystemen in een uniform platform voor betere dienen klanten en partners. U kunt [een zoekopdracht uitvoert](https://azuremarketplace.microsoft.com/marketplace/apps?search=Database%20Security&page=1) database security-producten beschikbaar in de Azure Marketplace weergeven.

## <a name="next-steps"></a>Volgende stappen

- [Uw Azure SQL-database beveiligen](https://docs.microsoft.com/azure/sql-database/sql-database-security-tutorial)
- [Azure Security Center en Azure SQL Database-service](https://docs.microsoft.com/azure/security-center/security-center-sql-database)
- [Detectie van bedreigingen voor SQL-Database](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection)
- [Prestaties van de SQL-database verbeteren](https://docs.microsoft.com/azure/sql-database/sql-database-performance-tutorial)
