---
title: Overzicht van Azure-database beveiliging | Microsoft Docs
description: Dit artikel bevat een overzicht van de Azure-database beveiligingsfuncties.
services: security
documentationcenter: na
author: UnifyCloud
manager: swadhwa
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/01/2017
ms.author: TomSh
ms.openlocfilehash: 3c83645f61cd42c8c2b46f787c9e7531726d3fbb
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2017
---
# <a name="azure-database-security-overview"></a>Overzicht van Azure-database-beveiliging

Beveiliging is een bovenste probleem bij het beheren van databases en is altijd een prioriteit voor Azure SQL Database. Azure SQL Database ondersteunt de beveiliging van de verbinding met firewall-regels en versleutelde verbinding. Het ondersteunt verificatie met gebruikersnaam en wachtwoord en Azure Active Directory-verificatie met identiteiten die worden beheerd door Azure Active Directory. Autorisatie wordt gebruikt voor toegangsbeheer op basis van rollen.

Azure SQL Database ondersteunt versleuteling door realtime versleuteling en ontsleuteling van databases, gekoppelde back-ups en transactielogboekbestanden in rust uitvoeren zonder dat wijzigingen in de toepassing.

Microsoft biedt extra manieren voor het versleutelen van ondernemingsgegevens:

-   -Celcodering voor het versleutelen van specifieke kolommen of zelfs cellen van gegevens met verschillende versleutelingssleutels.
-   Als u een Hardware Security Module of Centraal beheer van uw belangrijkste versleuteling-hiërarchie nodig hebt, kunt u overwegen Azure Key Vault met SQL Server in een Azure VM.
-   Altijd versleutelde (momenteel in preview) wordt versleuteling transparant voor toepassingen en kan clients voor het versleutelen van gevoelige gegevens binnen clienttoepassingen zonder de versleutelingssleutels te delen met SQL-Database.

Azure SQL Database Auditing kunnen ondernemingen record gebeurtenissen naar een aanmelding audit Azure Storage. SQL Database Auditing kan ook worden geïntegreerd met Microsoft Power BI om het inzoomen op rapporten en analyses mogelijk te maken.

 Azure SQL-databases nauw kunnen worden beveiligd om te voldoen aan meest regelgevende of beveiligingsvereisten, zoals HIPAA, ISO 27001/27002 en PCI DSS-niveau 1, onder andere. Een huidige lijst van naleving veiligheidscertificaten is beschikbaar op de [Microsoft Azure Trust Center site](http://azure.microsoft.com/support/trust-center/services/).

Dit artikel helpt bij de basisprincipes van beveiliging van Microsoft Azure SQL-Databases voor gestructureerde, in tabelvorm en relationele gegevens. Dit artikel helpt u in het bijzonder om aan de slag te gaan met resources voor gegevensbeveiliging, toegangsbeheer en proactieve controle.

In dit artikel beveiligingsoverzicht van Azure-Database is gericht op de volgende gebieden:

-   Gegevens beveiligen
-   Toegangsbeheer
-   Proactieve controle
-   Gecentraliseerd beheer
-   Azure Marketplace

## <a name="protect-data"></a>Gegevens beveiligen

SQL-Database beveiligt uw gegevens dankzij de versleuteling van gegevens in beweging met behulp van [Transport Layer Security](https://support.microsoft.com/kb/3135244)voor data-at-rest met [Transparent Data Encryption](http://go.microsoft.com/fwlink/?LinkId=526242), en voor gegevens in het gebruik van gebruik [ Altijd versleuteld](https://msdn.microsoft.com/library/mt163865.aspx).

We hebben in deze sectie over:

-   Codering in beweging
-   Versleuteling 'at rest'
-   Versleuteling gebruikt (Client)

Andere manieren om uw gegevens te versleutelen die u kunt overwegen:

-   [Versleuteling op celniveau](https://msdn.microsoft.com/library/ms179331.aspx) om specifieke kolommen of zelfs cellen met gegevens met verschillende versleutelingssleutels te versleutelen.
-   Als u een Hardware Security Module of centraal beheer van uw versleutelingssleutelhiërarchie nodig heeft, kunt u overwegen om [Azure Key Vault met SQL Server in een virtuele Azure-machine](http://blogs.technet.com/b/kv/archive/2015/01/12/using-the-key-vault-for-sql-server-encryption.aspx) te gebruiken.

### <a name="encryption-in-motion"></a>Codering in beweging

Een veelvoorkomend probleem voor alle client/server-toepassingen is de noodzaak van privacy als gegevens worden verplaatst via openbare en particuliere netwerken. Als u gegevens verplaatst via een netwerk is niet versleuteld, is de kans dat deze kan worden vastgelegd of worden gestolen door onbevoegde gebruikers. Wanneer het omgaan met database-services, moet u ervoor zorgen dat gegevens worden versleuteld tussen de databaseclient en server, evenals tussen database-servers die communiceren met elkaar en met de middelste laag toepassingen.

Een probleem bij het beheren van een netwerk is de beveiliging van gegevens die tussen toepassingen via een niet-vertrouwd netwerk worden verzonden. U kunt [TLS/SSL](https://docs.microsoft.com/windows-server/security/tls/transport-layer-security-protocol) bij het verifiëren van servers en clients en vervolgens worden gebruikt om berichten tussen de geverifieerde partijen te versleutelen.

Tijdens het verificatieproces een TLS/SSL-client verzendt een bericht naar een TLS/SSL-server en de server reageert met de informatie dat de server moet worden geverifieerd. De client en server wisselen daarna sessiesleutels uit en de verificatiedialoog wordt beëindigd. Als verificatie is voltooid, kunt SSL beveiligde communicatie tussen de server en de client met behulp van de symmetrische versleutelingssleutels die tijdens het verificatieproces zijn bepaald beginnen.

Alle verbindingen met Azure SQL Database vereisen te allen tijde versleuteling (SSL/TLS) wanneer gegevens naar en vanuit de database worden verzonden. SQL Azure maakt gebruik van TLS/SSL om te verifiëren van servers en clients en vervolgens worden gebruikt om berichten tussen de geverifieerde partijen te versleutelen. In de verbindingsreeks van uw toepassing, moet u parameters voor het versleutelen van de verbinding en niet naar het servercertificaat wordt vertrouwd (dit geldt voor u als u de verbindingsreeks buiten de Azure Portal kopieert), anders is de verbinding niet controleert of de identiteit van de server en is vatbaar voor 'man-in-the-middle'-aanvallen. Voor het stuurprogramma ADO.NET, zijn deze parameters voor de verbindingsreeks bijvoorbeeld coderen = True en TrustServerCertificate = False.

### <a name="encryption-at-rest"></a>Versleuteling 'at rest'
U kunt enkele voorzorgsmaatregelen nemen om de database zoals ontwerpen van een beveiligde systeem, vertrouwelijk assets coderen en bouwen van een firewall rond de databaseservers beveiligen. Echter in een scenario waarbij het fysieke medium (zoals stations of back-uptapes) worden gestolen, kunt een schadelijke party alleen herstellen of koppel de database en de gegevens bladeren.

Eén oplossing is voor het versleutelen van de gevoelige gegevens in de database en beveiligt de sleutels die worden gebruikt voor het versleutelen van de gegevens met een certificaat. Voorkomen dat iemand zonder de sleutels van het gebruik van de gegevens, maar dit soort bescherming moet worden gepland.

U lost dit probleem, SQL Server en Azure SQL ondersteunen [Transparent Data Encryption (TDE)](https://docs.microsoft.com/sql/relational-databases/securityrecryption/transparent-data-encryption-tde). TDE versleutelt SQL Server en Azure SQL Database-gegevensbestanden, van Versleutelingsgegevens in rust genoemd.

Azure SQL Database transparante gegevensversleuteling beschermt tegen de dreiging van schadelijke activiteiten door te voeren realtime versleuteling en ontsleuteling van de database, gekoppelde back-ups en transactielogbestanden in rust zonder wijzigingen in de de toepassing.  

TDE versleutelt de opslag van een volledige database met behulp van een symmetrische sleutel, naam van de databaseversleutelingssleutel. In SQL-Database, wordt de databaseversleutelingssleutel beveiligd door een ingebouwde servercertificaat. Het ingebouwde servercertificaat is uniek voor elke SQL-Database-server.

Als een database in een GeoDR-relatie is, is het beveiligd door een andere sleutel op elke server. Als de twee databases zijn verbonden met dezelfde server, delen ze hetzelfde ingebouwde certificaat. Microsoft draait automatisch deze certificaten ten minste om de 90 dagen. Zie voor een algemene beschrijving van TDE [Transparent Data Encryption (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-tde).

### <a name="encryption-in-use-client"></a>Versleuteling gebruikt (client)

De meeste gegevens schendingen hebben betrekking op diefstal van kritieke gegevens, zoals creditcardnummers of persoonsgegevens. Databases kunnen worden rijkdommen vergaren troves van gevoelige informatie. Ze kunnen persoonlijke gegevens, vertrouwelijk gevoelige gegevens en intellectueel eigendom van klanten bevatten. Verloren of gestolen gegevens, vooral klantgegevens kan leiden tot beschadiging van het merk en concurrerende nadeel ernstige boetes, zelfs als deze rechtszaken.

![Altijd versleuteld](./media/azure-databse-security-overview/azure-database-fig1.png)

[Altijd versleuteld](https://msdn.microsoft.com/library/mt163865.aspx) is een functie ter bescherming van gevoelige gegevens, zoals creditcardnummers of nationale ID-nummers (bijvoorbeeld Amerikaans sofi-nummer), opgeslagen in Azure SQL Database of SQL Server-databases. Altijd toegestaan versleutelde clients voor het versleutelen van gevoelige gegevens binnen clienttoepassingen en nooit onthullen de versleutelingssleutels voor de Database-Engine (SQL-Database of SQL Server).

Versleutelde biedt altijd een scheiding tussen degenen die eigenaar zijn van de gegevens (en het kunnen bekijken) en degenen die de gegevens te beheren (maar moet hebben geen toegang). Doordat de lokale databasebeheerders cloud database operators of andere verhoogde, maar niet-geautoriseerde gebruikers geen toegang tot de versleutelde gegevens.

Bovendien wordt altijd versleuteld versleuteling transparant voor toepassingen. Een stuurprogramma altijd versleuteld is ingeschakeld op de clientcomputer is geïnstalleerd zodat automatisch kunt versleutelen en ontsleutelen van gevoelige gegevens in de clienttoepassing. Het stuurprogramma versleutelt de gegevens in gevoelige kolommen voordat de gegevens doorgegeven aan de Database-Engine en query's automatisch herschrijft zodat de semantiek voor de toepassing worden behouden. Het stuurprogramma ontsleutelt op dezelfde manier transparant, opgeslagen gegevens in gecodeerde databasekolommen die zijn opgenomen in de queryresultaten.

## <a name="access-control"></a>Toegangsbeheer
Voor de beveiliging regelt SQL Database toegang met firewallregels die de connectiviteit beperken op basis van IP-adres. Met behulp van verificatiemechanismen moeten gebruikers hun identiteit kunnen aantonen en met autorisatiemechanismen worden gebruikers beperkt tot bepaalde gegevens en acties.

### <a name="database-access"></a>Toegang tot de database

Gegevensbeveiliging begint met het beheren van toegang tot uw gegevens. Het datacenter die als host fungeert voor uw gegevens beheert fysieke toegang terwijl u een firewall voor het beheren van beveiliging op de netwerklaag kunt configureren. U kunt ook toegang beheren door aanmeldingen voor verificatie configureren en machtigingen voor server en database-rollen te definiëren.

We hebben in deze sectie over:

-   Firewall en firewallregels
-   Authentication
-   Autorisatie

#### <a name="firewall-and-firewall-rules"></a>Firewall en firewallregels

Microsoft Azure SQL Database levert een relationele-databaseservice voor Azure en andere op internet gebaseerde toepassingen. Om uw gegevens te beschermen, verhinderen firewalls alle toegang tot de databaseserver totdat u opgeeft welke computers zijn gemachtigd. De firewall verleent toegang tot databases op basis van het IP-adres waar de aanvraag vandaan komt. Zie [Overzicht van de firewallregels voor SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) voor meer informatie.

De [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) service is alleen beschikbaar via TCP-poort 1433. Zorg voor toegang tot een SQL Database vanaf uw computer ervoor dat de firewall van uw clientcomputer uitgaande TCP-communicatie op TCP-poort 1433 toestaat. Blokkeer binnenkomende verbindingen op TCP-poort 1433 als u deze niet nodig hebt voor andere toepassingen.

#### <a name="authentication"></a>Authentication

SQL Database-verificatie verwijst naar hoe u uw identiteit bewijst bij het maken van verbinding met de database. SQL Database ondersteunt twee typen verificatie:

-   **SQL-verificatie:** een eenmalige aanmeldingsaccount wordt gemaakt wanneer een logische SQL-exemplaar is gemaakt, de Account van de SQL Database-abonnee genoemd. Dit account maakt verbinding met [SQL Server-verificatie](https://docs.microsoft.com/azure/sql-database/sql-database-security-overview) (gebruikersnaam en wachtwoord). Het account fungeert als beheerder voor de logische serverinstantie en voor alle gebruikersdatabases die hieraan zijn gekoppeld. De machtigingen van het abonnee-account kunnen niet worden beperkt. Er kan slechts één van deze accounts bestaan.
-   **Azure Active Directory-verificatie:** [Azure Active Directory-verificatie](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) is een mechanisme van verbinding maken met Microsoft Azure SQL Database en SQL Data Warehouse met behulp van identiteiten in Azure Active Directory ( Azure AD). Hiermee kunt u de identiteit van de databasegebruikers centraal te beheren.

![Authentication](./media/azure-databse-security-overview/azure-database-fig2.png)

 Voordelen van Azure Active Directory-verificatie zijn:
  - Het biedt een alternatief voor SQL Server-verificatie.
  - Ook helpt te stoppen van de komst van gebruikers-id's tussen de databaseservers & kunt wachtwoord draaien op één plaats.
  - U kunt met behulp van externe (Azure Active Directory) groepen databasemachtigingen beheren.
  - Deze kunt wachtwoorden moet opslaan voorkomen door het inschakelen van geïntegreerde Windows-verificatie en andere soorten authenticatie wordt ondersteund door Azure Active Directory.

#### <a name="authorization"></a>Autorisatie
[Autorisatie](https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins) verwijst naar wat een gebruiker binnen een Azure SQL Database kunt uitvoeren, en dit wordt bepaald door uw gebruikersaccount database [rollidmaatschappen](https://msdn.microsoft.com/library/ms189121) en [op objectniveau machtigingen](https://msdn.microsoft.com/library/ms191291.aspx). Autorisatie is het proces om te bepalen welke beveiligbare bronnen hebben toegang tot een principal en welke bewerkingen zijn toegestaan voor deze resources.

### <a name="application-access"></a>Toegang tot toepassingen

We hebben in deze sectie over:

-   Dynamische gegevensmaskering
-   Beveiliging op rijniveau

#### <a name="dynamic-data-masking"></a>Dynamische gegevensmaskering
Een medewerker van de in het midden van een aanroep van aanroepfuncties door verschillende cijfers van hun sociaal-fiscaal nummer of creditcardnummer kan worden geïdentificeerd, maar deze gegevensitems niet volledig worden blootgesteld aan de klantenservice.

Een maskeringsregel kan worden gedefinieerd dat alle maskers, maar de laatste vier cijfers van een sofi-nummer of creditcardnummer in het resultaat van een query moet worden ingesteld.

![Dynamische gegevensmaskering](./media/azure-databse-security-overview/azure-database-fig3.png)

Als een ander voorbeeld kan invoermasker van de juiste gegevens worden gedefinieerd als persoonsgegevens (PII)-gegevens beveiligen zodat een ontwikkelaar productieomgevingen opvragen kan voor het oplossen van problemen regelgeving overtreden.

[SQL Database dynamische-Gegevensmaskering](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) blootstelling van gevoelige gegevens beperkt door het maskeren voor onbevoegde gebruikers. Dynamische gegevensmaskering wordt ondersteund voor de versie van Azure SQL Database V12.

[Dynamische gegevensmaskering](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking) helpt voorkomen dat onbevoegde toegang tot gevoelige gegevens doordat u toewijzen hoeveel van de gevoelige gegevens om weer te geven met minimale gevolgen voor de toepassingslaag. Dit is een beveiligingsfunctie op basis van beleid. De gevoelige gegevens in de resultatenset van een query die is uitgevoerd op toegewezen databasevelden worden verborgen, terwijl de gegevens in de database niet worden gewijzigd.


> [!Note]
> Dynamische-gegevensmaskering kan worden geconfigureerd door de beheerder van de Azure-Database, serverbeheerder of officer beveiligingsrollen.

#### <a name="row-level-security"></a>Beveiliging op rijniveau
Een andere algemene beveiligingsvereiste voor multitenant databases is [beveiliging](https://msdn.microsoft.com/library/dn765131.aspx). Deze functie kunt u toegang tot de rijen in een databasetabel op basis van de kenmerken van de gebruiker (bijv, groep lidmaatschap of -uitvoering context) van een query uit te voeren.

![-Beveiliging op rijniveau](./media/azure-databse-security-overview/azure-database-fig4.png)

De beperking toegang logica is gevonden in de databaselaag in plaats van opslag van de gegevens in een andere toepassingslaag. Elke keer dat de toegang tot die gegevens van elke categorie wordt uitgevoerd, past het databasesysteem de toegangsbeperkingen. Dit maakt het beveiligingssysteem betrouwbaarder en robuuste doordat het oppervlak van het beveiligingssysteem.

Beveiliging op rijniveau introduceert predikaat gebaseerd toegangsbeheer. Biedt een flexibele, gecentraliseerde, op basis van het predikaat evaluatieversie die kan worden uitgevoerd in overweging metagegevens of andere criteria die de beheerder bepaalt waar nodig. Het predikaat wordt gebruikt als een criterium om te bepalen of de gebruiker de juiste toegang tot de gegevens op basis van gebruikerskenmerken heeft. Toegangsbeheer op basis van het label kan worden geïmplementeerd met behulp van toegangsbeheer op basis van een predikaat.

## <a name="proactive-monitoring"></a>Proactieve controle
SQL-Database beveiligt uw gegevens dankzij de **controle** en **bedreigingendetectie** mogelijkheden.

### <a name="auditing"></a>Controleren
SQL Database Auditing, verhoogt de mogelijkheid om inzicht in gebeurtenissen en wijzigingen die binnen de database plaatsvinden, inclusief updates en query's op de gegevens te krijgen.

[Azure SQL Database Auditing](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) houdt database gebeurtenissen en schrijft deze naar een auditlogboek Meld u bij uw Azure Storage-account. Dankzij controles kunt u zorgen voor naleving van wet- en regelgeving, krijgt u inzicht in de activiteit in uw database en in de afwijkingen en discrepanties die kunnen wijzen op problemen voor het bedrijf of vermoedelijke schendingen van de beveiliging. Controle kunt en voldoen aan de nalevingsstandaards vereenvoudigt maar niet garanderen dat.

SQL Database Auditing, kunt u:

-   **Behouden** een audittrail van de geselecteerde gebeurtenissen. Categorieën van databaseacties moeten worden gecontroleerd, kunt u definiëren.
-   **Rapport** op database-activiteit. U kunt vooraf geconfigureerde rapporten en een dashboard snel aan de slag met de activiteit en rapportage.
-   **Analyseren** rapporten. U kunt verdachte gebeurtenissen, ongebruikelijke activiteiten en trends vinden.

Er zijn twee methoden voor controle:

-   **Auditingfunctie voor blobs** -Logboeken geschreven naar Azure Blob Storage. Dit is een nieuwere controle methode, waarbij biedt betere prestaties en rendabeler biedt ondersteuning voor hogere granulatie controlefunctionaliteit op objectniveau.
-   **Controle van de tabel** -Logboeken geschreven naar Azure Table Storage.

### <a name="threat-detection"></a>Detectie van bedreigingen
[Azure SQL Database voor de detectie van dreigingen](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection) verdachte activiteiten worden gedetecteerd die duiden op beveiligingsdreigingen. Detectie van dreigingen kunt u reageren op verdachte gebeurtenissen in de database, zoals SQL-injectie wanneer deze zich voordoen. Het biedt waarschuwingen en staat het gebruik van Azure SQL Database Auditing om te verkennen van de verdachte gebeurtenissen.

![Detectie van dreigingen](./media/azure-databse-security-overview/azure-database-fig5.jpg)

SQL-injectie is bijvoorbeeld een van de algemene Web application beveiligingsproblemen op het Internet worden gebruikt voor aanvallen op gegevensgestuurde toepassingen. Aanvallers te profiteren van de toepassing zwakke plekken in het injecteren schadelijke SQL-instructies in de toepassing invoervelden, schendingen veroorzaken of wijzigen van gegevens in de database.

-Afdelingen of andere aangewezen beheerders kunnen een onmiddellijke melding over verdachte databaseactiviteiten krijgen wanneer deze zich voordoen. Elke melding vindt u informatie over de verdachte activiteit en raadt aan om verder onderzoek en risico.        

## <a name="centralized-security-management"></a>Gecentraliseerd beheer

[Azure Security Center](https://azure.microsoft.com/documentation/services/security-center/) helpt u bedreigingen te voorkomen, te detecteren en erop te reageren. Het biedt geïntegreerde beveiligingsbewaking en beleidsbeheer voor uw Azure-abonnementen, helpt bedreigingen te detecteren die anders onopgemerkt zouden blijven, en werkt met een uitgebreid ecosysteem van beveiligingsoplossingen.

[Security Center](https://docs.microsoft.com/azure/security-center/security-center-sql-database) helpt u gegevens in SQL-Database beveiligt door te geven inzicht in de beveiliging van uw servers en databases. U kunt met Security Center:

-   Beleid voor versleuteling van de SQL-Database en controle definiëren.
-   Het bewaken van de beveiliging van SQL Database-resources voor al uw abonnementen.
-   Snel identificeren en te verhelpen beveiligingsproblemen.
-   Waarschuwingen van integreren [Azure SQL Database met detectie van dreigingen](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection).
-   Security Center biedt ondersteuning voor toegang op basis van rollen.

## <a name="azure-marketplace"></a>Azure Marketplace

Azure Marketplace is een onlinemarktplaats voor toepassingen en services waar beginnende en onafhankelijke softwareleveranciers hun oplossingen kunnen aanbieden aan Azure-klanten over de hele wereld.
Azure Marketplace combineert Microsoft Azure-partnerecosystemen in één uniform platform om onze klanten en partners beter van dienst te zijn. Klik op [hier](https://azuremarketplace.microsoft.com/marketplace/apps?search=Database%20Security&page=1) oogopslag database beveiliging producten op Azure-marktplaats.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [beveiligen van uw Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-security-tutorial).
- Meer informatie over [Azure Security Center en Azure SQL Database-service](https://docs.microsoft.com/azure/security-center/security-center-sql-database).
- Zie voor meer informatie over detectie van dreigingen, [SQL Database met detectie van dreigingen](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection).
- Zie voor meer informatie, [prestaties verbeteren SQL-database](https://docs.microsoft.com/azure/sql-database/sql-database-performance-tutorial). 
