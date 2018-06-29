---
title: Overzicht van Azure-database beveiliging | Microsoft Docs
description: Dit artikel bevat een overzicht van de Azure-database beveiligingsfuncties.
services: security
documentationcenter: na
author: UnifyCloud
manager: mbaldwin
editor: TomSh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/01/2017
ms.author: TomSh
ms.openlocfilehash: 27f6ccadaa89042ebced759ac3040b5b79e1f8f8
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37051522"
---
# <a name="azure-database-security-overview"></a>Overzicht van Azure-database-beveiliging

Beveiliging zijn bovenste zorgen voor het beheren van databases en is altijd een prioriteit voor Azure SQL Database. Azure SQL Database ondersteunt de beveiliging van de verbinding met firewall-regels en versleutelde verbinding. Het ondersteunt verificatie met gebruikersnaam en wachtwoord en verificatie van Azure Active Directory (Azure AD), dat gebruikmaakt van identiteiten die worden beheerd door Azure Active Directory. Autorisatie wordt gebruikt voor toegangsbeheer op basis van rollen.

Azure SQL Database ondersteunt versleuteling door realtime versleuteling en ontsleuteling van databases, gekoppelde back-ups en transactielogboekbestanden in rust uitvoeren zonder dat wijzigingen in de toepassing.

Microsoft biedt extra manieren voor het versleutelen van ondernemingsgegevens:

-   -Celcodering is beschikbaar voor het versleutelen van specifieke kolommen of zelfs cellen van gegevens met verschillende versleutelingssleutels.
-   Als u een hardware security module of Centraal beheer van uw belangrijkste versleuteling-hiërarchie nodig hebt, overweeg het gebruik van Azure Sleutelkluis met SQL Server in Azure een virtuele machine (VM).
-   Altijd wordt versleutelde (momenteel in preview) versleuteling transparant voor toepassingen. Ook kunnen clients voor het versleutelen van gevoelige gegevens binnen clienttoepassingen zonder de versleutelingssleutels te delen met SQL-Database.

Azure SQL Database Auditing kunnen ondernemingen record gebeurtenissen naar een auditlogboek worden geregistreerd in Azure Storage. SQL Database Auditing kan ook worden geïntegreerd met Microsoft Power BI om het inzoomen op rapporten en analyses mogelijk te maken.

Azure SQL-databases nauw kunnen worden beveiligd om te voldoen aan meest regelgevende of beveiligingsvereisten, zoals HIPAA, ISO 27001/27002 en PCI DSS-niveau 1. Een huidige lijst van naleving veiligheidscertificaten is beschikbaar op de [Microsoft Azure Trust Center site](http://azure.microsoft.com/support/trust-center/services/).

Dit artikel helpt bij de basisprincipes van beveiliging van Microsoft Azure SQL-databases voor gestructureerde, in tabelvorm en relationele gegevens. Dit artikel helpt u in het bijzonder om aan de slag te gaan met resources voor gegevensbeveiliging, toegangsbeheer en proactieve controle.

## <a name="protection-of-data"></a>Beveiliging van gegevens

SQL-Database helpt de beveiliging van uw gegevens dankzij versleuteling:

- Voor gegevens in beweging via [Transport Layer Security (TLS)](https://support.microsoft.com/kb/3135244).
- Voor gegevens in rust via [transparante gegevensversleuteling](http://go.microsoft.com/fwlink/?LinkId=526242).
- Voor gegevens in gebruik via [altijd versleuteld](https://msdn.microsoft.com/library/mt163865.aspx).

Andere manieren om uw gegevens te versleutelen die u kunt overwegen:

-   [Versleuteling op celniveau](https://msdn.microsoft.com/library/ms179331.aspx) om specifieke kolommen of zelfs cellen met gegevens met verschillende versleutelingssleutels te versleutelen.
-   [Azure Sleutelkluis met SQL Server in een Azure VM](http://blogs.technet.com/b/kv/archive/2015/01/12/using-the-key-vault-for-sql-server-encryption.aspx), als u een hardware security module of Centraal beheer van uw belangrijkste hiërarchie versleuteling nodig.

### <a name="encryption-in-motion"></a>Codering in beweging

Een veelvoorkomend probleem voor alle client/server-toepassingen is de noodzaak van privacy als gegevens worden verplaatst via openbare en particuliere netwerken. Als gegevens verplaatst via een netwerk is niet versleuteld, is er een kans dat deze kan worden vastgelegd of worden gestolen door onbevoegde gebruikers. Wanneer u omgaan met database-services, zorg ervoor dat gegevens worden versleuteld tussen de databaseclient en server. Zorg ook dat er gegevens worden versleuteld tussen database-servers die communiceren met elkaar en met de middelste laag toepassingen.

Een probleem bij het beheren van een netwerk is de beveiliging van gegevens die tussen toepassingen via een niet-vertrouwd netwerk worden verzonden. U kunt [TLS/SSL](https://docs.microsoft.com/windows-server/security/tls/transport-layer-security-protocol) om te verifiëren van servers en clients en vervolgens worden gebruikt om berichten tussen de geverifieerde partijen te versleutelen.

Een TLS/SSL-client verzendt tijdens het verificatieproces een bericht naar een TLS/SSL-server. De server reageert met de informatie die de server moet worden geverifieerd. De client en server wisselen daarna sessiesleutels uit en de verificatiedialoog wordt beëindigd. Als verificatie is voltooid, kan SSL beveiligde communicatie tussen de server en de client starten via de symmetrische versleutelingssleutels die tijdens het verificatieproces zijn bepaald.

Alle verbindingen met Azure SQL Database vereisen versleuteling (TLS/SSL) op alle tijden wanneer gegevens ' verzonden' naar en van de database. SQL-Database maakt gebruik van TLS/SSL om te verifiëren van servers en clients en vervolgens worden gebruikt om berichten tussen de geverifieerde partijen te versleutelen. 

In de verbindingsreeks van uw toepassing, moet u de parameters voor het versleutelen van de verbinding en niet naar het servercertificaat te vertrouwen. (Dit wordt gedaan voor u als u de verbindingsreeks buiten de Azure-portal kopieert.) Anders wordt de verbinding niet controleert of de identiteit van de server en worden gevoeliger voor 'man-in-the-middle'-aanvallen. Voor het stuurprogramma ADO.NET bijvoorbeeld deze parameters voor de verbindingsreeks zijn `Encrypt=True` en `TrustServerCertificate=False`.

### <a name="encryption-at-rest"></a>Versleuteling 'at rest'
U kunt enkele voorzorgsmaatregelen nemen om de database beveiligen. Bijvoorbeeld, een beveiligd systeem ontwerpen, vertrouwelijk assets coderen en bouwen van een firewall rond de databaseservers. Maar in een scenario waarbij het fysieke medium (zoals stations of back-uptapes) worden gestolen, een schadelijke party kunt alleen herstellen of koppel de database en de gegevens bladeren.

Er is een oplossing voor het versleutelen van de gevoelige gegevens in de database en bescherming van de sleutels die worden gebruikt voor het versleutelen van de gegevens met een certificaat. Deze oplossing wordt voorkomen dat iemand anders zonder de sleutels met behulp van de gegevens, maar dit soort bescherming moet worden gepland.

Voor het oplossen van dit probleem, SQL Server en SQL-Database ondersteuning [transparante gegevensversleuteling](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql?view=azuresqldb-current&viewFallbackFrom=sql-server-2017). Transparent data encryption voor versleutelt SQL Server en SQL Database-gegevensbestanden, bekend als van Versleutelingsgegevens in rust.

Transparante gegevensversleuteling beschermt tegen de dreiging van schadelijke activiteiten. Hiermee realtime versleuteling en ontsleuteling van de database, gekoppelde back-ups en transactielogboekbestanden in rust worden uitgevoerd zonder dat wijzigingen in de toepassing.  

Transparante gegevensversleuteling versleutelt de opslag van een volledige database met behulp van een symmetrische sleutel, naam van de databaseversleutelingssleutel. In SQL-Database, wordt de databaseversleutelingssleutel beveiligd door een ingebouwde servercertificaat. Het ingebouwde servercertificaat is uniek voor elke SQL-Database-server.

Als een database in een relatie Geo-Noodherstel is, het beveiligd door een andere sleutel op elke server. Als de twee databases zijn verbonden met dezelfde server, delen ze hetzelfde ingebouwde certificaat. Microsoft draait automatisch deze certificaten ten minste om de 90 dagen. 

Zie voor meer informatie [transparante gegevensversleuteling](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-tde).

### <a name="encryption-in-use-client"></a>Versleuteling gebruikt (client)

De meeste gegevens schendingen hebben betrekking op diefstal van kritieke gegevens, zoals creditcardnummers of persoonsgegevens. Databases kunnen worden rijkdommen vergaren troves van gevoelige informatie. Ze kunnen persoonlijke gegevens (zoals nationale identificatienummers), vertrouwelijke gegevens over de concurrentie en intellectueel eigendom van klanten bevatten. Verloren of gestolen gegevens, vooral klantgegevens kan leiden tot beschadiging van het merk, concurrerende nadeel en ernstige boeten--zelfs rechtszaken.

![De functie altijd versleuteld als een vergrendeling en sleutel geïllustreerd](./media/azure-databse-security-overview/azure-database-fig1.png)

[Altijd versleuteld](https://msdn.microsoft.com/library/mt163865.aspx) is een functie ter bescherming van gevoelige gegevens die zijn opgeslagen in Azure SQL Database of SQL Server-databases. Altijd toegestaan versleutelde clients voor het versleutelen van gevoelige gegevens binnen clienttoepassingen en nooit onthullen de versleutelingssleutels voor de database-engine (SQL-Database of SQL Server).

Versleutelde geeft altijd een scheiding tussen de mensen die eigenaar zijn van de gegevens (en het kunnen bekijken) en personen die de gegevens te beheren (maar moet hebben geen toegang). Dit zorgt ervoor dat de lokale groep administrators, cloudoperators database of andere verhoogde database maar niet-geautoriseerde gebruikers geen toegang de versleutelde gegevens tot.

Bovendien wordt altijd versleuteld versleuteling transparant voor toepassingen. Een stuurprogramma altijd versleuteld ingeschakeld is geïnstalleerd op de clientcomputer zodat automatisch kan versleutelen en ontsleutelen van gevoelige gegevens in de clienttoepassing. Het stuurprogramma versleutelt de gegevens in gevoelige kolommen voordat de gegevens doorgegeven aan de database-engine. Het stuurprogramma herschrijft automatisch query's zodat de semantiek voor de toepassing worden behouden. Het stuurprogramma ontsleutelt op dezelfde manier transparant, opgeslagen gegevens in gecodeerde databasekolommen die zijn opgenomen in de queryresultaten.

## <a name="access-control"></a>Toegangsbeheer
Als u wilt beveiligen, controleert de SQL-Database toegang met behulp van:

- De firewallregels die connectiviteit door IP-adres beperken.
- Verificatiemechanismen waarvoor gebruikers om hun identiteit te bewijzen.
- Autorisatiemechanismen die gebruikers tot specifieke acties en gegevens beperken.

### <a name="database-access"></a>Databasetoegang

Gegevensbeveiliging begint met het beheren van toegang tot uw gegevens. Het datacenter dat als host fungeert voor uw gegevens beheert fysieke toegang. U kunt een firewall voor het beheren van beveiliging op de netwerklaag configureren. U kunt ook toegang beheren door aanmeldingen voor verificatie configureren en machtigingen voor server en database-rollen te definiëren.

#### <a name="firewall-and-firewall-rules"></a>Firewall en firewallregels

[Azure SQL Database](https://azure.microsoft.com/services/sql-database/) voorziet in een relationele database-service voor Azure en andere toepassingen op basis van het internet. Om uw gegevens te beschermen, verhinderen firewalls alle toegang tot de databaseserver totdat u opgeeft welke computers zijn gemachtigd. De firewall verleent toegang tot databases op basis van het IP-adres waar de aanvraag vandaan komt. Zie [Overzicht van de firewallregels voor SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) voor meer informatie.

De Azure SQL Database-service is alleen beschikbaar via TCP-poort 1433. Zorg ervoor dat de firewall van uw client-computer uitgaande TCP-communicatie op TCP-poort 1433 toestaat voor toegang tot een SQL-database van de computer moet. Als inkomende verbindingen niet nodig voor andere toepassingen zijn, kunt u ze op TCP-poort 1433 blokkeren.

#### <a name="authentication"></a>Verificatie

Verificatie verwijst naar hoe u uw identiteit bewijst bij het maken van verbinding met de database. SQL Database ondersteunt twee typen verificatie:

-   **SQL Server-verificatie**: een aanmeldingsaccount met eenmalige wordt gemaakt wanneer een logische SQL-exemplaar is gemaakt, de Account van de SQL Database-abonnee genoemd. Dit account maakt verbinding met behulp van [SQL Server-verificatie](https://docs.microsoft.com/azure/sql-database/sql-database-security-overview) (gebruikersnaam en wachtwoord). Het account fungeert als beheerder voor de logische serverinstantie en voor alle gebruikersdatabases die hieraan zijn gekoppeld. De machtigingen van de abonnee-account kunnen niet worden beperkt. Er kan slechts één van deze accounts bestaan.
-   **Azure Active Directory-verificatie**: [Azure AD authentication](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) is een mechanisme van verbinding maken met Azure SQL Database en Azure SQL Data Warehouse met behulp van identiteiten in Azure AD. U kunt deze centraal beheren van identiteiten van database-gebruikers.

![Azure AD-verificatie met SQL Database](./media/azure-databse-security-overview/azure-database-fig2.png)

 Voordelen van Azure AD-verificatie zijn:
  - Het biedt een alternatief voor SQL Server-verificatie.
  - Dit helpt te stoppen van de komst van gebruikersidentiteiten voor databaseservers en kan wachtwoord draaien op één plaats.
  - U kunt de databasemachtigingen beheren met behulp van groepen met externe (Azure AD).
  - Deze kunt wachtwoorden moet opslaan voorkomen door het inschakelen van geïntegreerde Windows-verificatie en andere vormen van verificatie die ondersteuning biedt voor Azure AD.

#### <a name="authorization"></a>Autorisatie
[Autorisatie](https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins) verwijst naar wat een gebruiker binnen een Azure SQL database kunt uitvoeren. Dit wordt bepaald door uw gebruikersaccount database [rollidmaatschappen](https://msdn.microsoft.com/library/ms189121) en [op objectniveau machtigingen](https://msdn.microsoft.com/library/ms191291.aspx). Autorisatie is het proces om te bepalen welke beveiligbare bronnen hebben toegang tot een principal en welke bewerkingen zijn toegestaan voor deze resources.

### <a name="application-access"></a>Toegang tot toepassingen

#### <a name="dynamic-data-masking"></a>Dynamische gegevensmaskering
Een medewerker van de in het midden van een aanroep kan aanroepfuncties identificeren door verschillende cijfers van hun sociaal-fiscaal nummer of creditcardnummer. Maar deze gegevensitems niet volledig worden blootgesteld aan de klantenservice.

U kunt een maskeringsregel die alles behalve de laatste vier cijfers van een sofi-nummer of creditcardnummer in de resultatenset van een query maskeert kunt definiëren.

![Maskering op een getal tussen een SQL-database en business-apps is verzonden](./media/azure-databse-security-overview/azure-database-fig3.png)

Een ander voorbeeld: een juiste gegevens masker ter bescherming van persoonlijke gegevens worden gedefinieerd. Een ontwikkelaar kan vervolgens query productieomgevingen voor het oplossen van problemen regelgeving overtreden.

Met [dynamische gegevensmaskering in SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) wordt de blootstelling van gevoelige gegevens beperkt door deze gegevens te maskeren voor niet-gemachtigde gebruikers. Dynamische gegevensmaskering wordt ondersteund voor de versie van Azure SQL Database V12.

[Dynamische gegevensmaskering](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking) helpt voorkomen dat onbevoegde toegang tot gevoelige gegevens doordat u toewijzen hoeveel van de gevoelige gegevens om weer te geven met minimale gevolgen voor de toepassingslaag. Dit is een beveiligingsfunctie op basis van beleid. De gevoelige gegevens in de resultatenset van een query die is uitgevoerd op toegewezen databasevelden worden verborgen, terwijl de gegevens in de database niet worden gewijzigd.


> [!Note]
> Dynamische-gegevensmaskering kan worden geconfigureerd door de beheerder van de Azure-Database, serverbeheerder of officer beveiligingsrollen.

#### <a name="row-level-security"></a>Beveiliging op rijniveau
Een andere algemene beveiligingsvereiste voor multitenant databases is [beveiliging](https://msdn.microsoft.com/library/dn765131.aspx). Deze functie kunt u de toegang tot de rijen in een databasetabel op basis van de kenmerken van de gebruiker die een query wordt uitgevoerd. (Voorbeeld kenmerken zijn groep lidmaatschap en uitvoering context).

![Rij beveiligingsniveau zodat een gebruiker toegang rijen in een tabel door middel van een client-app](./media/azure-databse-security-overview/azure-database-fig4.png)

De beperking toegang logica is gevonden in de databaselaag in plaats van opslag van de gegevens in een andere toepassingslaag. Elke keer dat de toegang tot die gegevens van elke categorie wordt uitgevoerd, past het databasesysteem de toegangsbeperkingen. Dit maakt het beveiligingssysteem betrouwbaarder en robuuste doordat het oppervlak van het beveiligingssysteem.

Beveiliging introduceert toegangsbeheer op basis van een predikaat. Biedt een flexibele, gecentraliseerde evaluatieversie die kan worden uitgevoerd in overweging metagegevens of andere criteria die de beheerder bepaalt waar nodig. Het predikaat wordt gebruikt als een criterium om te bepalen of de gebruiker de juiste toegang tot de gegevens op basis van gebruikerskenmerken heeft. U kunt toegangsbeheer op basis van een label kunt implementeren met behulp van toegangsbeheer op basis van een predikaat.

## <a name="proactive-monitoring"></a>Proactieve controle
SQL-Database helpt u bij het beveiligen van uw gegevens dankzij de *controle* en *bedreigingendetectie* mogelijkheden.

### <a name="auditing"></a>Controleren
[Azure SQL Database auditing](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) verhoogt u de mogelijkheid inzicht te verwerven in de gebeurtenissen en wijzigingen die binnen de database plaatsvinden. Voorbeelden zijn updates en query's op de gegevens.

SQL Database auditing houdt databasegebeurtenissen en schrijft die deze naar een auditlogboek Meld u bij uw Azure storage-account. Controle, kunt u de naleving van regelgeving onderhouden, de activiteit in een database begrijpen en meer inzicht krijgen in discrepanties en afwijkingen die kunnen duiden op problemen voor het bedrijf of vermoedelijke beveiligingsschendingen. Controle kunt en voldoen aan de nalevingsstandaards vereenvoudigt maar niet garanderen dat.

Controle-instellingen voor SQL-Database, kunt u gebruiken:

-   **Behouden** een audittrail van de geselecteerde gebeurtenissen. Categorieën van databaseacties moeten worden gecontroleerd, kunt u definiëren.
-   **Rapport** op database-activiteit. U kunt vooraf geconfigureerde rapporten en een dashboard snel aan de slag met de activiteit en rapportage.
-   **Analyseren** rapporten. U kunt verdachte gebeurtenissen, ongebruikelijke activiteiten en trends vinden.

Er zijn twee methoden voor controle:

-   **Auditingfunctie voor blobs**: Logboeken worden geschreven naar Azure Blob-opslag. Dit is een nieuwere controle methode. Dit biedt betere prestaties, biedt ondersteuning voor hogere granulatie controlefunctionaliteit op objectniveau en rendabeler.
-   **Controle van de tabel**: Logboeken worden geschreven naar Azure Table storage.

### <a name="threat-detection"></a>Detectie van bedreigingen
[Azure SQL Database voor de detectie van dreigingen](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection) verdachte activiteiten worden gedetecteerd die duiden op beveiligingsdreigingen. Wanneer deze zich voordoen, kunt u de detectie van dreigingen te reageren op verdachte gebeurtenissen in de database, zoals SQL-injectie. Het biedt waarschuwingen en staat het gebruik van Azure SQL Database auditing om te bekijken van de verdachte gebeurtenissen.

![Detectie van dreigingen voor SQL-Database en een web-app met een externe aanvaller en een kwaadwillende insider](./media/azure-databse-security-overview/azure-database-fig5.jpg)

SQL-injectie is bijvoorbeeld een van de algemene beveiligingsproblemen voor webtoepassingen. Wordt gebruikt voor aanvallen gegevensgestuurde toepassingen. Aanvallers te profiteren van de toepassing zwakke plekken in het injecteren schadelijke SQL-instructies in de toepassing invoervelden, schendingen veroorzaken of wijzigen van gegevens in de database.

-Afdelingen of andere aangewezen beheerders kunnen een onmiddellijke melding over verdachte databaseactiviteiten krijgen wanneer deze zich voordoen. Elke melding vindt u informatie over de verdachte activiteit en raadt aan om verder onderzoek en risico.        

## <a name="centralized-security-management"></a>Gecentraliseerd beheer

[Azure Security Center](https://azure.microsoft.com/documentation/services/security-center/) helpt u bedreigingen te voorkomen, te detecteren en erop te reageren. Het biedt geïntegreerde beveiligingsbewaking en beleidsbeheer voor uw Azure-abonnementen. Het helpt bedreigingen die anders onopgemerkt detecteren en dit proces werkt met een uitgebreid ecosysteem van beveiligingsoplossingen.

[Security Center](https://docs.microsoft.com/azure/security-center/security-center-sql-database) helpt u gegevens in SQL-Database beveiligt door te geven inzicht in de beveiliging van uw servers en databases. U kunt met Security Center:

-   Beleid voor versleuteling van de SQL-Database en controle definiëren.
-   Het bewaken van de beveiliging van SQL Database-resources voor al uw abonnementen.
-   Snel identificeren en te verhelpen beveiligingsproblemen.
-   Waarschuwingen van integreren [Azure SQL Database met detectie van dreigingen](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection).

Security Center biedt ondersteuning voor toegang op basis van rollen.

## <a name="azure-marketplace"></a>Azure Marketplace

Azure Marketplace is een onlinemarktplaats voor toepassingen en services waar beginnende en onafhankelijke softwareleveranciers hun oplossingen kunnen aanbieden aan Azure-klanten over de hele wereld.
Microsoft Azure-partnerecosystemen combineert Azure Marketplace in een uniform platform voor betere dienst klanten en partners. U kunt [een zoekopdracht uitvoeren](https://azuremarketplace.microsoft.com/marketplace/apps?search=Database%20Security&page=1) beveiligingsproducten database beschikbaar weergeven in Azure Marketplace.

## <a name="next-steps"></a>Volgende stappen

- [Beveiligen van uw Azure SQL database](https://docs.microsoft.com/azure/sql-database/sql-database-security-tutorial)
- [Azure Security Center en Azure SQL Database-service](https://docs.microsoft.com/azure/security-center/security-center-sql-database)
- [Detectie van dreigingen SQL-Database](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection)
- [Verbeteren de prestaties van de SQL-database](https://docs.microsoft.com/azure/sql-database/sql-database-performance-tutorial)
