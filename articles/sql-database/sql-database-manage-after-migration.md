---
title: Na de migratie - Azure SQL Database beheren | Microsoft Docs
description: Meer informatie over het beheren van uw database na migratie naar Azure SQL Database.
services: sql-database
author: joesackmsft
manager: craigg
ms.service: sql-database
ms.custom: migrate
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: josack
ms.suite: sql
ms.prod_service: sql-database
ms.component: data-movement
ms.openlocfilehash: 133cba72a93d692851043f1c66d6a4a38e18b324
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44379455"
---
# <a name="new-dba-in-the-cloud--managing-your-database-in-azure-sql-database"></a>Nieuwe DBA in de cloud, beheer van uw database in Azure SQL Database

Verplaatsen van de traditionele zelf wordt beheerd, zelf-gecontroleerde omgeving naar een PaaS-omgeving kan overweldigend zijn iets in eerste instantie. Als een app-ontwikkelaar of een DBA, zou u wilt weten van de belangrijkste mogelijkheden van het platform waarmee u uw toepassing beschikbaar is, prestaties, veilige en flexibele - altijd. In dit artikel is erop gericht om precies dat te doen. Het artikel bondig organiseert bronnen en biedt u enkele richtlijnen over hoe u optimaal gebruik van de belangrijkste mogelijkheden van SQL Database beheren en uw toepassing efficiënt te houden en optimale resultaten in de cloud. Typische publiek voor dit artikel zou zijn die: 
- Migratie van hun toepassingen met Azure SQL DB: moderniseren van uw toepassingen evalueert.
- Wordt momenteel gemigreerd van hun toepassingen – continue migratiescenario.
- De migratie naar Azure SQL DB: nieuwe DBA in de cloud hebben onlangs zijn voltooid.

In dit artikel komen enkele van de belangrijkste kenmerken van Azure SQL-database als een platform waarmee u gemakkelijk kunt gebruiken. De volgende zijn:- 
- Zakelijke continuïteit en herstel na noodgevallen (BCDR)
- Veiligheid en naleving
- Intelligente database controleren en onderhoud
- Gegevensverplaatsing

## <a name="business-continuity-and-disaster-recovery-bcdr"></a>Zakelijke continuïteit en herstel na noodgevallen (BCDR)
Mogelijkheden voor zakelijke continuïteit en noodherstel recovery kunnen u uw bedrijf, zoals gewoonlijk blijven in geval van een ramp. De noodherstel mogelijk een database op gebeurtenis (bijvoorbeeld iemand deze per ongeluk komt een belangrijke tabel) of een datacenter op gebeurtenis (regionale ramp, bijvoorbeeld een tsunami). 

### <a name="how-do-i-create-and-manage-backups-on-sql-database"></a>Hoe ik maken en beheren van back-ups op SQL-Database?
Maak u geen back-ups op Azure SQL DB en dat komt omdat u niet hoeft te. SQL-Database automatisch back-ups van databases voor u, zodat u niet meer over het plannen, nemen en beheren van back-ups moet zorgen. Het platform wordt een volledige back-up elke week, differentiële dat back-up elke paar uur en een logboek back-up om de 5 minuten om te controleren of het herstel na noodgevallen is efficiënt, en de minimale verlies van gegevens. De eerste volledige back-up gebeurt er als u een database maken. Deze back-ups voor u beschikbaar zijn voor een bepaalde periode de "bewaarperiode' genoemd en is afhankelijk van de prestatielaag die u kiest.  SQL Database biedt u de mogelijkheid om te herstellen naar een bepaald tijdstip binnen deze bewaarperiode periode met [punt in tijd herstel (PITR)](sql-database-recovery-using-backups.md#point-in-time-restore).

|Prestatielaag|Bewaarperiode in dagen|
|---|:---:|
|Basic|7|
|Standard|35|
|Premium|35|
|||

Bovendien de [Long-Term Retention (LTR)](sql-database-long-term-retention.md) functie kunt u afhangen uw back-upbestanden voor een veel langere periode specifiek, voor maximaal tien jaar, en het herstellen van gegevens van deze back-ups op elk gewenst moment binnen die periode. Bovendien worden de databaseback-ups opgeslagen in een opslag met geo-replicatie om ervoor te zorgen flexibiliteit bij de regionale ramp. U kunt ook deze back-ups in een Azure-regio op elk moment binnen de bewaarperiode liggen herstellen. Zie [overzicht voor zakelijke continuïteit](sql-database-business-continuity.md).

### <a name="how-do-i-ensure-business-continuity-in-the-event-of-a-datacenter-level-disaster-or-regional-catastrophe"></a>Hoe kan ik ervoor zorgen dat zakelijke continuïteit in geval van een datacenter op serverniveau na noodgevallen of regionale ramp?
Omdat uw databaseback-ups worden opgeslagen op een subsysteem voor opslag met geo-replicatie om ervoor te zorgen dat in het geval van een regionaal noodgeval, kunt u de back-up herstellen naar een andere Azure-regio. Dit is de geo-herstel genoemd. De RPO (Recovery Point Objective) voor dit is doorgaans < 1 uur en de ERT (geschatte hersteltijd – enkele minuten tot uren).

Voor databases die essentiële biedt Azure SQL DB actieve geo-replicatie. Wat dit in wezen doet, is dat er een geo-replicatie secundaire kopie van de oorspronkelijke database wordt gemaakt in een andere regio. Bijvoorbeeld, als uw database in eerste instantie wordt gehost in Azure VS-West-regio en u wilt dat flexibiliteit regionale bij noodgevallen. U maakt een actieve geo-replica van de database in VS-West te zeggen hadden VS-Oost. Wanneer de calamity op VS-West voordoet, kunt u failover naar de regio VS-Oost. Ze configureren in een automatische-Failovergroep is nog beter, omdat dit zorgt ervoor dat de database automatisch-schakeling naar de secundaire server in VS-Oost in het geval van een noodgeval. Het RPO voor deze is < 5 seconden en de ERT < 30 seconden.

Als een automatische failover-groep niet is geconfigureerd, klikt u vervolgens moet uw toepassing actief controleren op een noodgeval en start een failover naar de secundaire server. U kunt maximaal 4 dergelijke actieve geo-replica's maken in verschillende Azure-regio's. Het wordt nog beter. U kunt ook toegang tot deze secundaire actieve geo-replica's voor alleen-lezen toegang. Dit is erg handig om te verminderen latentie voor een toepassingsscenario voor geografisch gedistribueerde. 

### <a name="how-does-my-disaster-recovery-plan-change-from-on-premises-to-sql-database"></a>Hoe wordt mijn noodherstelplan gewijzigd van on-premises SQL-database?
Kortom vereist de installatie van de traditionele on-premises SQL Server dat u actief de beschikbaarheid van uw met behulp van functies zoals Failoverclustering, databasespiegeling, transactiereplicatie, Log verzending enzovoort beheren en onderhouden en beheren van back-ups om ervoor te zorgen Zakelijke continuïteit. Met SQL Database beheert het platform deze voor u, zodat u kunt zich richten op het ontwikkelen en het optimaliseren van uw databasetoepassing en u geen over het beheer van noodherstel zo veel mogelijk zorgen. U kunt back-up en plannen voor herstel na noodgevallen geconfigureerd en het werken met slechts een paar klikken op de Azure-portal (of een aantal opdrachten met behulp van de PowerShell-APIs) hebben. 

Zie voor meer informatie over herstel na noodgevallen: [Azure SQL Db Disaster Recovery 101](https://azure.microsoft.com/blog/azure-sql-databases-disaster-recovery-101/)

## <a name="security-and-compliance"></a>Veiligheid en naleving
SQL-Database hecht veel waarde beveiliging en Privacy. Beveiliging in SQL-Database is beschikbaar op databaseniveau en op het niveau van het platform en is het best begrijpen wanneer onderverdeeld in meerdere lagen. Op elke laag krijgt u te bepalen en optimale beveiliging bieden voor uw toepassing. De lagen zijn:
- Identiteit en verificatie ([Windows/SQL-verificatie en verificatie van Azure Active Directory [AAD]](sql-database-control-access.md)).
- Controleactiviteit ([controle](sql-database-auditing.md) en [detectie van bedreigingen](sql-database-threat-detection.md)).
- Beveiligen van gegevens ([Transparent Data Encryption [TDE]](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) en [Always Encrypted [AE]](/sql/relational-databases/security/encryption/always-encrypted-database-engine)). 
- Beheren van toegang tot gevoelige en beschermde gegevens ([Row Level security](/sql/relational-databases/security/row-level-security) en [Dynamic Data Masking](/sql/relational-databases/security/dynamic-data-masking)).

[Azure Security Center](https://azure.microsoft.com/services/security-center/) biedt gecentraliseerd beheer over workloads die worden uitgevoerd in Azure, on-premises en in andere clouds. U kunt weergeven of essentieel SQL Database protection zoals [controle](sql-database-auditing.md) en [Transparent Data Encryption [TDE]](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) zijn geconfigureerd op alle resources en -beleid op basis van uw eigen vereisten maken.

### <a name="what-user-authentication-methods-are-offered-in-sql-database"></a>Welke gebruikersverificatiemethoden zijn beschikbaar in SQL-Database?
Er zijn [twee verificatiemethoden](sql-database-control-access.md#authentication) aangeboden in SQL Database: 
- [Azure Active Directory-verificatie](sql-database-aad-authentication.md)
- SQL-verificatie. 

De traditionele windows-verificatie wordt niet ondersteund. Azure Active Directory (AD) is een gecentraliseerde service voor identiteits- en toegangsbeheer. Met dit kunt u heel eenvoudig een eenmalige aanmelding toegang (SSO) bieden tot alle personeel in uw organisatie. Dit betekent dat de referenties worden verdeeld over alle Azure-services voor eenvoudigere verificatie. Biedt ondersteuning voor AAD [MFA (Multi-factor Authentication)](sql-database-ssms-mfa-authentication.md) en een met een [enkele muisklikken](../active-directory/connect/active-directory-aadconnect-get-started-express.md) AAD kan worden geïntegreerd met Windows Server Active Directory. SQL-verificatie werkt precies zoals u hebt gebruikt deze in het verleden. U een gebruikersnaam en wachtwoord opgeven en u kunt verificatie van gebruikers voor elke database op een bepaalde logische server. Zo kan SQL-Database en SQL Data Warehouse te bieden van multi-factor authentication en Gast gebruikersaccounts in een Azure AD-domein. Als u een Active Directory on-premises al hebt, kunt u de map met Azure Active Directory voor uw directory uitbreiden naar Azure federeren.

|**Als u...**|**SQL-Database / SQL Data Warehouse**|
|---|---|
|De voorkeur geeft niet aan Azure Active Directory (AD) in Azure|Gebruik [SQL-verificatie](sql-database-security-overview.md)|
|Gebruikte AD op SQL Server on-premises|[AD met Azure AD federeren](../active-directory/connect/active-directory-aadconnect.md), en Azure AD-verificatie gebruiken. Hiermee kunt u eenmalige aanmelding.|
|Moet multi-factor authentication (MFA) afdwingen|MFA vereist als een beleid via [voorwaardelijke toegang van Microsoft](sql-database-conditional-access.md), en gebruik [Azure AD universele authenticatie met ondersteuning voor MFA](sql-database-ssms-mfa-authentication.md).|
|Gast-account van Microsoft-accounts (live.com, outlook.com) of andere domeinen (gmail.com)|Gebruik [Azure AD universele authenticatie](sql-database-ssms-mfa-authentication.md) in SQL Database/Data Warehouse, die gebruikmaakt van [Azure AD B2B-samenwerking](../active-directory/active-directory-b2b-what-is-azure-ad-b2b.md).|
|Zijn aangemeld bij Windows met behulp van uw Azure AD-referenties van een federatief domein|Gebruik [Azure AD-geïntegreerde verificatie](sql-database-aad-authentication-configure.md).|
|Zijn aangemeld bij Windows met behulp van referenties van een domein dat niet is gefedereerd met Azure|Gebruik [Azure AD-geïntegreerde verificatie](sql-database-aad-authentication-configure.md).|
|Middelste laag-services die verbinding maken met SQL-Database of SQL Data Warehouse nodig hebt|Gebruik [Azure AD-geïntegreerde verificatie](sql-database-aad-authentication-configure.md).|
|||

### <a name="how-do-i-limit-or-control-connectivity-access-to-my-database"></a>Hoe beperken of besturingselement ik toegang tot mijn database verbinding?
Er zijn meerdere technieken beschikken die u gebruiken kunt voor het bereiken van optimale connectiviteit organisatie voor uw toepassing. 
- Firewallregels
- VNET-Service-eindpunten
- Gereserveerde IP-adressen

#### <a name="firewall"></a>Firewall
Een firewall voorkomt dat toegang tot uw server een externe entiteit door toe te staan alleen specifieke entiteiten toegang tot uw logische server. Standaard alle verbindingen en databases op de logische server zijn niet toegestaan, behalve de verbindingen die afkomstig is van andere Azure-Services in. U kunt de toegang met uw server alleen op entiteiten (bijvoorbeeld een developer-machine) die u goedkeuren, doordat het IP-adres van die computer via de firewall openen met een firewall-regel. U kunt er ook om op te geven van een bereik van IP-adressen die u wilt toegang krijgt tot de logische server. Bijvoorbeeld, kunnen ontwikkelaars machine IP-adressen in uw organisatie in één keer worden toegevoegd door een adresbereik in de pagina van de Firewall-instellingen op te geven. 

U kunt firewallregels op serverniveau of op het databaseniveau van de maken. Server-level firewall-regels kunnen gemaakt via de portal of via SSMS. Zie voor meer informatie over het instellen van een server en database-level firewall-regel,: [firewallregels in SQL-Database maken](sql-database-security-tutorial.md#create-a-server-level-firewall-rule-in-the-azure-portal).

#### <a name="service-endpoints"></a>Service-eindpunten
Uw SQL-database is standaard geconfigureerd voor alle Azure-services toestaan': wat betekent dat elke virtuele Machine in Azure probeert verbinding maken met uw database. Deze pogingen nog steeds hoeft te geverifieerd. Echter, als u niet toegankelijk zijn via een Azure-IP-adressen van uw database wilt, kunt u uitschakelen 'Alle Azure-services toestaan'. Bovendien kunt u configureren [VNET-Service-eindpunten](sql-database-vnet-service-endpoint-rule-overview.md).

Service-eindpunten (SE) kunnen u uw kritieke Azure-resources alleen voor uw eigen persoonlijke virtueel netwerk in Azure beschikbaar. Op deze manier elimineren u in feite openbare toegang tot uw resources. Het verkeer tussen uw virtuele netwerk naar Azure blijft in het Azure-backbone-netwerk. U krijgt de geforceerde tunnels routering uit te voeren zonder SE. Het virtuele netwerk zorgt ervoor dat het verkeer van internet voor uw organisatie en de Azure-Service in op dezelfde route. Service-eindpunten kunt u optimaliseren dit omdat de pakketten stroom rechtstreeks vanuit uw virtuele netwerk naar de service op Azure-backbone-netwerk.

![VNET-service-eindpunten](./media/sql-database-manage-after-migration/vnet-service-endpoints.png) 

#### <a name="reserved-ips"></a>Gereserveerde IP-adressen
Een andere optie is om in te richten [gereserveerd IP-adressen](../virtual-network/virtual-networks-reserved-public-ip.md) voor uw VM's en de lijst met toegestane adressen die specifieke VM-IP-in de server adressen firewall-instellingen. Door gereserveerde IP-adressen toe te wijzen, bespaart u de moeite van het bij te werken van de firewall-regels met het wijzigen van IP-adressen.

### <a name="what-port-do-i-connect-to-sql-database-on"></a>Welke poorten ik verbinding maken met SQL-Database op?

Poort 1433. SQL Database communiceert via deze poort. Voor verbinding uit vanuit een bedrijfsnetwerk, moet u een uitgaande regel toevoegen in de firewall-instellingen van uw organisatie. Als een richtlijn te voorkomen dat poort 1433 buiten de grens van Azure. U kunt SSMS uitvoeren in Azure met [Azure RemoteApp](https://www.microsoft.com/cloud-platform/azure-remoteapp-client-apps). Dit vereist niet dat u uitgaande verbindingen voor poort 1433 openen, het IP-adres wordt statisch, zodat de database geopend zodat u kunt alleen de RemoteApp worden kan en Multi Factor Authentication (MFA) ondersteunt.

### <a name="how-can-i-monitor-and-regulate-activity-on-my-server-and-database-in-sql-database"></a>Hoe kan ik controleren en regelen van de activiteit op de server en database in SQL-Database?
#### <a name="sql-database-auditing"></a>SQL Database Auditing
Met SQL Database, kunt u controleren voor het bijhouden van databasegebeurtenissen. [SQL Database Auditing](sql-database-auditing.md) registreert databasegebeurtenissen en schrijft deze naar een controlelogboekbestand in uw Azure Storage-Account. Controleren is heel handig als u van plan bent krijgt u inzicht in mogelijke beveiliging en beleid schendingen, naleving van regelgeving enzovoort. Hiermee kunt u definiëren en configureren van bepaalde categorieën van gebeurtenissen die u denkt moet controleren en op basis van dat u vooraf geconfigureerde rapporten en een dashboard voor een overzicht van gebeurtenissen in uw database kunt ophalen. U kunt deze controlebeleid op het databaseniveau van de of op het serverniveau van de kunt toepassen. Een handleiding voor het inschakelen van controle voor uw server /-database, Zie: [inschakelen SQL Database Auditing](sql-database-security-tutorial.md#enable-sql-database-auditing-if-necessary).

#### <a name="threat-detection"></a>Detectie van bedreigingen
Met [detectie van bedreigingen](sql-database-threat-detection.md), krijgt u de mogelijkheid om te reageren op beveiligings- of beleid schendingen die zijn gedetecteerd door te controleren heel eenvoudig. U hoeft niet te worden van een beveiligingsexpert om potentiële bedreigingen of schendingen in uw systeem. Detectie van bedreigingen heeft ook enkele ingebouwde mogelijkheden, zoals SQL-injectie detectie. SQL-injectie is een poging te wijzigen of de gegevens en een gebruikelijk manier om in het algemeen is een aanval op een databasetoepassing in gevaar brengen. SQL Database Threat Detection uitgevoerd meerdere sets met algoritmen om mogelijke beveiligingsproblemen en SQL-injectieaanvallen, evenals afwijkende patronen voor databasetoegang (zoals toegang vanaf een ongewone locatie of door een ongebruikelijke) te detecteren. Security officers of andere aangewezen beheerders ontvangen een e-mailbericht als een bedreiging wordt gedetecteerd op de database. Elke melding bevat informatie over de verdachte activiteiten en aanbevelingen voor het verder te onderzoeken en tegenhouden. Zie voor meer het inschakelen van detectie van bedreigingen: [inschakelen SQL Database Threat Detection](sql-database-security-tutorial.md#enable-sql-database-threat-detection). 
### <a name="how-do-i-protect-my-data-in-general-on-sql-database"></a>Hoe kan ik beveiligen mijn gegevens in het algemeen op SQL-Database?
Versleuteling biedt een krachtig mechanisme om te beschermen en uw gevoelige gegevens beveiligen tegen indringers. De versleutelde gegevens is van de hacker zonder de ontsleutelingssleutel niet gebruikt. Daarom wordt er een extra beveiligingslaag boven op de bestaande lagen van SQL Database ingebouwde beveiliging toegevoegd. Er zijn twee aspecten in de bescherming van uw gegevens in SQL Database: 
- Uw gegevens in rust in de gegevens en logboekbestanden bestanden 
- Uw gegevens die onderweg zijn. 

In SQL-Database, standaard, worden uw gegevens in rust in de gegevens en logboekbestanden bestanden op het opslagsubsysteem volledig en altijd versleuteld [Transparent Data Encryption [TDE]](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql). Uw back-ups zijn eveneens versleuteld. Met TDE er zijn geen wijzigingen aan de kant van de toepassing die toegang heeft tot deze gegevens vereist. De versleuteling en ontsleuteling gebeuren transparant; daarom de naam. Voor het beveiligen van uw gevoelige gegevens die onderweg zijn en at-rest, SQL Database biedt een functie, genaamd [altijd versleuteld (AE)](/sql/relational-databases/security/encryption/always-encrypted-database-engine). AE is een vorm van clientversleuteling waarmee gevoelige kolommen in uw database worden gecodeerd (zodat ze zich in de gecodeerde tekst voor databasebeheerders en niet-geautoriseerde gebruikers). De server ontvangt begint met de versleutelde gegevens. De sleutel voor Always Encrypted worden ook opgeslagen op de client, zodat alleen geautoriseerde clients kunnen de gevoelige kolommen ontsleutelen. De server en de gegevensbeheerders zien niet de gevoelige gegevens omdat de versleutelingssleutels worden opgeslagen op de client. AE versleutelt gevoelige kolommen in de tabel end-to-end van niet-geautoriseerde clients naar de fysieke schijf. AE ondersteunt gelijkheid vergelijkingen vandaag, zodat de DBA kunnen blijven opvragen versleutelde kolommen als onderdeel van de SQL-opdrachten. Altijd versleutelde kan worden gebruikt met een verscheidenheid aan sleutelarchief opties, zoals [Azure Key Vault](sql-database-always-encrypted-azure-key-vault.md), Windows-certificaatarchief en lokale hardware security modules.

|**Kenmerken**|**Altijd versleuteld**|**Transparent Data Encryption**|
|---|---|---|
|**Versleuteling span**|End-to-end|Inactieve gegevens|
|**Database-server kunt krijgen tot gevoelige gegevens**|Nee|Ja, omdat versleuteling voor de data-at-rest|
|**Toegestane T-SQL-bewerkingen**|Gelijkheidsvergelijking|Alle T-SQL-gebied is beschikbaar|
|**App-wijzigingen die zijn vereist voor het gebruik van de functie**|Minimaal|Minimale|
|**Versleuteling granulariteit**|Op kolomniveau|Databaseniveau|
||||

### <a name="how-can-i-limit-access-to-sensitive-data-in-my-database"></a>Hoe kan ik toegang tot gevoelige gegevens in mijn database beperken?
Elke toepassing heeft een bepaalde deel van de gevoelige gegevens in de database die moet worden beveiligd tegen het voor iedereen zichtbaar wordt. Bepaalde personeel binnen de organisatie moeten deze gegevens weergeven, maar anderen mag niet mogelijk om deze gegevens weer te geven. Een voorbeeld hiervan is salarissen. Een manager zou moeten toegang hebben tot de loongegevens van zijn/haar directe ondergeschikten echter, de afzonderlijke teamleden al dan niet mogen toegang hebben tot de salarisgegevens van hun collega's. Een ander scenario is ontwikkelaars van gegevens die mogelijk interactie met gevoelige gegevens tijdens de fasen van de ontwikkeling of tests, bijvoorbeeld burgerservicenummers van klanten. Deze gegevens niet opnieuw hoeft te worden blootgesteld aan de ontwikkelaar. In dergelijke gevallen kan moet de gevoelige gegevens ofwel worden gemaskeerd of niet helemaal worden blootgesteld. SQL Database biedt twee manieren om te voorkomen dat onbevoegde gebruikers de mogelijkheid om gevoelige gegevens weer te geven:

[Dynamic Data Masking](sql-database-dynamic-data-masking-get-started.md) is een functie voor het maskeren van gegevens waarmee u beperken van blootstelling van gevoelige gegevens door deze te maskeren voor niet-gemachtigde gebruikers op de toepassingslaag. U definieert een maskeringsregel dat een patroon maskering kunt maken (bijvoorbeeld alleen weergeven laatste vier cijfers van een nationale ID SSN: XXX-XX-0000 en markeert u de meeste van deze als Xs) en nagaan welke gebruikers moeten worden uitgesloten van de maskeringsregel. De maskering vindt plaats op het begeven en er zijn verschillende maskeringsfuncties beschikbaar voor verschillende gegevenscategorieën. Dynamische gegevensmaskering kunt u automatisch detecteren van gevoelige gegevens in uw database en maskering erop van toepassing.

[Op rijniveau](/sql/relational-databases/security/row-level-security) kunt u toegangsbeheer op het rijniveau van de. Wat betekent dat zijn bepaalde rijen in een database-tabel op basis van de gebruiker uitvoeren van de query (groep groepslidmaatschap of uitvoeringscontext context) verborgen. De beperking van de toegang is uitgevoerd op de databaselaag in plaats van in de toepassingslaag van een, voor het vereenvoudigen van uw logische app. U begint met het maken van een filterpredicaat, gefilterd op rijen die worden niet weergegeven en de beveiliging beleid volgende definiëren wie toegang tot deze rijen heeft. Ten slotte de eindgebruiker hun query wordt uitgevoerd en, afhankelijk van de bevoegdheden van de gebruiker, de beperkte rijen weergeven of kan niet worden weergegeven in alle.

### <a name="how-do-i-manage-encryption-keys-in-the-cloud"></a>Hoe beheer ik coderingssleutels opgeslagen in de cloud?

Er zijn opties voor sleutelbeheer voor altijd versleuteld (client-side encryption) en Transparent Data Encryption (versleuteling-at-rest). Het raadzaam dat u regelmatig versleutelingssleutels draaien. De frequentie van de rotatie moet worden uitgelijnd met uw interne organisatie voorschriften en nalevingsvereisten.

**Transparante gegevensversleuteling (TDE)**: Er is een hiërarchie met twee-sleutel in TDE: de gegevens in elke gebruikersdatabase is versleuteld met een symmetrische AES-256-database-unieke databaseversleutelingssleutel (DEK), die op zijn beurt is versleuteld met een server-unieke asymmetrische RSA de hoofdsleutel van de 2048. De hoofdsleutel kunnen worden beheerd op:
- Automatisch door het platform - SQL-Database.
- Of door u met behulp van [Azure Key Vault](sql-database-always-encrypted-azure-key-vault.md) als de sleutel store.

Standaard wordt de hoofdsleutel voor Transparent Data Encryption beheerd door de service SQL Database voor het gemak. Als uw organisatie wil de controle over de hoofdsleutel, is er een optie voor het gebruik van Azure Key Vault](sql-database-always-encrypted-azure-key-vault.md) als de sleutel store. Met behulp van Azure Key Vault, uw organisatie wordt ervan uitgegaan dat controle over sleutels inrichten, draaien en de machtiging besturingselementen. [Rotatie of het overschakelen van het type van een hoofdsleutel TDE](/sql/relational-databases/security/encryption/transparent-data-encryption-byok-azure-sql-key-rotation) is snel, omdat deze de DEK alleen opnieuw versleutelt. Voor organisaties met scheiding van functies tussen beveiliging en beheer van gegevens, kan een beveiligingsbeheerder inrichten van het sleutelmateriaal voor de TDE-hoofdsleutel in Azure Key Vault en bieden de databasebeheerder moet worden gebruikt voor een Azure Key Vault-sleutel-id versleuteling-at-rest op een server. De Key Vault is ontworpen dat Microsoft niet kan zien of versleutelingssleutels extraheren. U kunt ook een gecentraliseerd beheer van sleutels ophalen voor uw organisatie. 

**Altijd versleuteld**: Er is ook een [twee sleutel hiërarchie](/sql/relational-databases/security/encryption/overview-of-key-management-for-always-encrypted) in Always Encrypted - een kolom met gevoelige gegevens versleuteld met een AES 256 kolomversleutelingssleutel (CEK), dat op zijn beurt is versleuteld met een kolomhoofdsleutel (CMK). De clientstuurprogramma's voor altijd versleuteld hebben geen beperkingen op de lengte van CMKs. De versleutelde waarde van de CEK is opgeslagen op de database en de CMK zijn opgeslagen in een vertrouwde sleutel store, zoals Windows certificaat Store, Azure Key Vault of een hardware security module. 
- Zowel de [CEK en CMK](/sql/relational-databases/security/encryption/rotate-always-encrypted-keys-using-powershell) kunnen worden gedraaid. 
- CEK rotatie is een grootte van gegevens en kan worden tijdrovende afhankelijk van de grootte van de tabellen met daarin de versleutelde kolommen. Het is daarom verstandig om te CEK opnamen dienovereenkomstig te plannen. 
- CMK roteren, echter niet van invloed op prestaties van de database en kan worden gedaan met behulp van gescheiden.
Het volgende diagram ziet u de opties sleutelarchief voor de kolom hoofdsleutels in Always Encrypted

![Altijd versleuteld CMK providers opslaan](./media/sql-database-manage-after-migration/always-encrypted.png)

### <a name="how-can-i-optimize-and-secure-the-traffic-between-my-organization-and-sql-database"></a>Hoe kan ik Optimaliseer en Beveilig het verkeer tussen mijn organisatie en de SQL-Database?
Het netwerkverkeer tussen uw organisatie en SQL Database zou in het algemeen krijgen gerouteerd via het openbare netwerk. Echter, als u wilt dit pad te optimaliseren en u beter te beveiligen, kunt u zoeken in Express Route. Express route in wezen kunt u uw bedrijfsnetwerk in het Azure-platform uitbreiden via een persoonlijke verbinding. Op deze manier, gaan u niet via het openbare Internet. Ook krijgt u betere beveiliging, betrouwbaarheid en routering optimalisatie die wordt omgezet in het onderste netwerkvertragingen en veel hogere snelheden worden bereikt dan u normaal zou optreden gaan via het openbare internet. Als u van plan bent voor het overdragen van een aanzienlijke hoeveelheid gegevens tussen uw organisatie en Azure, kan met behulp van Express Route kostenbesparingen opleveren. U kunt kiezen uit drie verschillende verbindingsvereisten modellen voor de verbinding van uw organisatie naar Azure: 
- [Cloud Exchange CO-locatie](../expressroute/expressroute-connectivity-models.md#CloudExchange)
- [Any-to-any](../expressroute/expressroute-connectivity-models.md#IPVPN)
- [Point-to-Point](../expressroute/expressroute-connectivity-models.md#Ethernet)

Express Route kunt u ook om uit te breiden tot 2 x de Bandbreedtelimiet die u voor zonder extra kosten koopt. Het is ook mogelijk om te configureren tussen regio connectiviteit met expressroute. Zie voor een lijst met connectiviteitsproviders ER: [Express Route-Partners en Peeringlocaties](../expressroute/expressroute-locations.md). De volgende artikelen wordt Express Route in meer detail beschreven:
- [Inleiding in Express Route](../expressroute/expressroute-introduction.md)
- [Vereisten](../expressroute/expressroute-prerequisites.md)
- [Werkstromen](../expressroute/expressroute-workflows.md)

### <a name="is-sql-database-compliant-with-any-regulatory-requirements-and-how-does-that-help-with-my-own-organizations-compliance"></a>Van alle wettelijke eisen die compatibel is met SQL-Database, en hoe helpt die met de naleving van mijn eigen organisatie?
SQL-Database is compatibel met een scala aan om. Als u de meest recente set om dat wordt voldaan, gaat u naar de [Microsoft Trust Center](https://microsoft.com/en-us/trustcenter/compliance/complianceofferings) en inzoomen op de ingebouwde die belangrijk voor uw organisatie om te zien zijn als de SQL-Database is opgenomen onder de compatibele Azure-services. Het is belangrijk te weten dat Hoewel SQL-Database kunnen worden gecertificeerd als een compatibele service, dit de compatibiliteit van de service van uw organisatie helpt, maar kan deze niet automatisch garanderen.

## <a name="intelligent-database-monitoring-and-maintenance-after-migration"></a>Intelligente database controleren en onderhoud na de migratie

Wanneer u uw database naar SQL Database hebt gemigreerd, u wilt bewaken van uw database (voor bijvoorbeeld controleren hoe het gebruik van bronnen, zoals of DBCC controleert) en regelmatig onderhoud uit te voeren (bijvoorbeeld opnieuw samenstellen of reorganize indexen, statistieken enz.). Gelukkig is SQL Database Intelligent in de zin dat maakt gebruik van de historische trends en geregistreerde metrische gegevens en statistieken om proactief te bewaken en onderhouden van uw database, zodat uw toepassing optimaal altijd wordt uitgevoerd. In sommige gevallen kan Azure SQL DB onderhoudstaken uitvoeren, afhankelijk van uw configuratie-instellingen automatisch uitvoeren. Er zijn drie facetten te bewaken van uw database in SQL-Database:
- Prestaties bewaken en optimaliseren.
- Optimalisatie van de beveiliging.
- Optimalisatie van kosten.

**Prestaties bewaken en optimaliseren**: met Query prestatie-inzichten krijgt u op maat gemaakte aanbevelingen voor de werkbelasting van uw database zodat uw toepassingen blijven op het niveau van een optimale - altijd uitvoeren kunnen. U kunt ook instellen deze zodat deze aanbevelingen worden automatisch toegepast en u geen hebt zin presterende onderhoudstaken. Met Index Advisor, kunt u automatisch op basis van uw werkbelasting indexaanbevelingen implementeren - dit automatisch afstemmen wordt genoemd. De aanbevelingen ontwikkelen als uw toepassing workload wordt gewijzigd zodat u kunt met de meest relevante suggesties. U krijgt ook de mogelijkheid handmatig controleren van deze aanbevelingen en deze naar eigen goeddunken toepassen.  

**Beveiliging optimalisatie**: SQL-Database bevat bruikbare security aanbevelingen voor het beveiligen van uw gegevens en detectie van bedreigingen voor het identificeren en onderzoeken van verdachte databaseactiviteiten waaraan een mogelijke thread kunnen blootstellen de de database. [Evaluatie van beveiligingsproblemen SQL](sql-vulnerability-assessment.md) is een scannen en -service waarmee u de beveiligingsstatus van uw databases op schaal controleren en Identificeer veiligheidsrisico en drift uit door u gedefinieerde basisbeveiliging reporting-database. Na elke scan een aangepaste lijst met stappen en herstelscripts worden vermeld en er een rapport over de beoordeling die kan worden gebruikt voor het helpen om te voldoen aan nalevingsvereisten.

Met Azure Security Center, moet u de aanbevelingen voor beveiliging op het bord identificeren en ze toepassen met één klik. 

**Kosten van optimalisatie**: Azure SQL-platform analyseert de gebruiksgeschiedenis voor de databases in een server om te evalueren en aan te bevelen kostenoptimalisatie opties voor u. Deze analyse duurt normaal gesproken een rijtijd te analyseren en bruikbare aanbevelingen opbouwen. Elastische Pools zijn een van deze opties. De aanbeveling wordt op de portal weergegeven als een banner:

![elastische pool](./media/sql-database-manage-after-migration/elastic-pool-recommendations.png) 

U kunt ook deze analyse in de sectie 'Adviseur' bekijken:

![elastische pool aanbevelingen-advisor](./media/sql-database-manage-after-migration/advisor-section.png)

### <a name="how-do-i-monitor-the-performance-and-resource-utilization-in-sql-database"></a>Hoe controleer ik de prestaties en het gebruik van resources in SQL-Database?

U kunt gebruikmaken van de intelligente inzichten van het platform voor het bewaken van de prestaties en afstemmen dienovereenkomstig in SQL-Database. U kunt prestaties en gebruik van resources in SQL-Database met behulp van de volgende methoden volgen:
- **Azure-portal**: Azure portal ziet u een individuele database-gebruik door de database te selecteren en te klikken op de grafiek in het overzichtsvenster van. U kunt de grafiek om weer te geven meerdere metrische gegevens, met inbegrip van CPU-percentage, DTU-percentage, gegevens-IO-percentage, percentage van de sessies en databaseomvangpercentage wijzigen.

   ![Controle grafiek](./media/sql-database-manage-after-migration/monitoring-chart.png)

   ![Bewaking van grafiek2](./media/sql-database-manage-after-migration/chart.png)

U kunt ook waarschuwingen door middel van deze grafiek configureren. Deze waarschuwingen kunnen u reageren op de voorwaarden van de resource met een e-mailadres, het schrijven naar een HTTPS/HTTP-eindpunt of een actie uitvoert. Zie de [databaseprestaties bewaken in SQL-Database](sql-database-single-database-monitor.md) voor gedetailleerde instructies.

- **Dynamische beheerweergaven**: U kunt een query de [sys.dm_db_resource_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) dynamische Beheerweergave om terug te keren resource verbruik statistieken geschiedenis van het afgelopen uur en de [sys.resource_stats](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database) system catalogusweergave om terug te keren geschiedenis voor de afgelopen 14 dagen.
- **Query Performance Insight**: [Query Performance Insight](sql-database-query-performance.md) kunt u een historisch overzicht bekijken van de top-resource-intensieve query's en langlopende query's voor een specifieke database. TOP-query's door gebruik van resources, de duur en de frequentie van de uitvoering, kunt u snel identificeren. U kunt query's bijhouden en regressie detecteren. Deze functie moet [Query Store](/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store) zijn ingeschakeld en actief voor de database.

   ![Inzicht in queryprestaties](./media/sql-database-manage-after-migration/query-performance-insight.png)

- **Azure SQL Analytics (Preview) in Log Analytics**: [Azure Log Analytics](../log-analytics/log-analytics-azure-sql.md) kunt u voor het verzamelen en visualiseren van de belangrijkste prestatiegegevens voor Azure SQL Azure, ondersteuning biedt voor maximaal 150.000 SQL-Databases en 5000 elastische SQL-Pools per werkruimte. U kunt deze gebruiken om te controleren en meldingen ontvangen. U kunt controleren op SQL-Database en elastische pool metrische gegevens op meerdere Azure-abonnementen en elastische pools en kan worden gebruikt voor het identificeren van problemen in elke laag van een toepassingsstack.

### <a name="i-am-noticing-performance-issues-how-does-my-sql-database-troubleshooting-methodology-differ-from-sql-server"></a>Ik ben dat u prestatieproblemen: hoe mijn SQL-Database voor probleemoplossing methodologie verschilt van SQL Server?
Een belangrijk deel van de technieken voor het oplossen van problemen die u zou gebruiken voor het vaststellen van de query en prestatieproblemen met de database blijven hetzelfde. Nadat de dezelfde SQL Server engine wordt gebruikt door de cloud. Echter is de platform, Azure SQL DB gebouwd in 'intelligentie'. U kunt oplossen en diagnosticeren van prestatieproblemen nog eenvoudiger. Het kan enkele van deze corrigerende acties ook uitvoeren op uw naam en in sommige gevallen, ze - automatisch proactief verhelpen. 

De aanpak van het oplossen van problemen met de prestaties aanzienlijk kan profiteren met behulp van intelligente functies zoals [Query Performance Insight(QPI)](sql-database-query-performance.md) en [Database Advisor](sql-database-advisor.md) in combinatie en zodat het verschil in methodologie verschilt in dat opzicht – u niet meer wilt oplossen van de handmatige werkzaamheden van malen uit de essentiële informatie die u kunnen helpen bij het probleem bij de hand. Het platform al het harde werk voor u. Een voorbeeld van is QPI. Met QPI, kunt u inzoomen helemaal op het niveau van de query en Ga naar de historische trends en dat precies de query is teruggedraaid. De Database Advisor biedt u aanbevelingen op dingen die u in het algemeen verbeteren van de prestaties van uw algehele zoals - ontbrekende indexen kunnen helpen bij verwijderen van indexen, parametriseren van uw query's enzovoort. 

Bij het oplossen van prestaties is het belangrijk om te bepalen of het is gewoon de toepassing of het maken van back-database die wordt die invloed hebben op prestaties van uw toepassing. Vaak is het prestatieprobleem ligt in de toepassingslaag. De architectuur of de data access-patroon kan het zijn. Neem bijvoorbeeld dat een intensieve toepassing die is gevoelig voor netwerklatentie. In dit geval uw toepassing heeft omdat er veel korte aanvragen heen en weer ("intensieve') tussen de toepassing en de server en in een overbelaste netwerk, dat deze interactie snel toevoegen. In dit geval de prestaties verbeteren, kunt u [Batchquery's](sql-database-performance-guidance.md#batch-queries). Met behulp van batches kunt u dankzij omdat nu uw aanvragen in een batch verwerkt; dus, helpt u bij het beperken van de retour-latentie en verbeteren de toepassingsprestaties van uw. 

Bovendien, als u merkt dat een verslechtering van de in de algehele prestaties van uw database, kunt u bewaken de [sys.dm_db_resource_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) en [sys.resource_stats](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database) dynamic management views om informatie over verbruik voor CPU, IO en geheugen. De prestaties van uw wordt mogelijk beïnvloed omdat de database wordt pas over van resources. Kan het zijn dat u wilt wijzigen van het prestatieniveau en/of servicelaag op basis van de groeiende en werklastvraag verkleinen. 

Zie voor een uitgebreide set met aanbevelingen voor het afstemmen van prestaties,: [afstemmen van uw database](sql-database-performance-guidance.md#tune-your-database).

### <a name="how-do-i-ensure-i-am-using-the-appropriate-service-tier-and-performance-level"></a>Hoe voorkom ik dat ik de servicelaag en het prestatieniveau niveau van de betreffende service gebruik?
SQL Database biedt verschillende Servicelagen Basic, Standard en Premium. Elke servicelaag krijgt u een gegarandeerde voorspelbare prestaties die zijn gekoppeld aan dit serviceniveau. Afhankelijk van uw werkbelasting wellicht u pieken van activiteit waar uw gebruik van resources mogelijk bereikt van het maximum van het huidige prestatieniveau die u in. In dergelijke gevallen is het nuttig om eerst door het evalueren van of alle afstemmen kunt (bijvoorbeeld toe te voegen of te wijzigen van een index enz.). Als u nog steeds problemen beperken, kunt u overwegen om naar een hoger prestatieniveau of serviceniveau. 

|**Serviceniveau**|**Veelvoorkomende Use-Casescenario 's**|
|---|---|
|**Basic**|Toepassingen met een paar gebruikers en een database waarvoor geen hoge eisen voor gelijktijdigheid, schaal en prestaties. |
|**Standard**|Toepassingen met een aanzienlijke hoeveelheid vereisten voor gelijktijdigheid, schaal en prestaties in combinatie met een lage tot gemiddelde i/o-aanvragen. |
|**Premium**|Toepassingen met een groot aantal gelijktijdige gebruikers, hoge CPU/geheugen en hoge i/o-vereist. Hoge gelijktijdigheid, hoge doorvoer en latentie gevoelige apps kunnen gebruikmaken van de Premium-niveau. |
|||

Om te controleren of dat u op het juiste prestatieniveau bent, kunt u het gebruik van de resources query's en -database via een van de bovengenoemde manieren 'Hoe controleer ik het gebruik van de prestaties en resource in SQL-Database' controleren. U vindt dat uw query's / databases consistent worden uitgevoerd op CPU/geheugen enz. u kunt omhoog schalen naar een hoger prestatieniveau ' hot '. Op dezelfde manier als u Houd er rekening mee dat, zelfs tijdens de piekuren die hebt u gebruikt u de resources zo veel mogelijk; Houd rekening met van het huidige prestatieniveau omlaag te schalen. 

Als u een patroon van SaaS-app of een database consolidatie scenario hebt, kunt u overwegen een elastische Pool om kosten te optimaliseren. Elastische pool is een uitstekende manier om de consolidatie van de database en kosten optimaliseren te realiseren. Meer informatie over het beheren van meerdere databases met behulp van de elastische Pool, Zie: [pools en databases beheren](sql-database-elastic-pool-manage.md#azure-portal-manage-elastic-pools-and-pooled-databases). 

### <a name="how-often-do-i-need-to-run-database-integrity-checks-for-my-database"></a>Hoe vaak moet ik database integriteitscontroles voor mijn database uitvoeren?
SQL-Database maakt gebruik van sommige slimme technieken die toe te staan om bepaalde klassen van beschadiging van gegevens automatisch en zonder verlies van gegevens te verwerken. Deze technieken zijn ingebouwd in de service en worden gebruikt door de service wanneer moet zich voordoet. Uw databaseback-ups via de service zijn er regelmatig getest door deze terug te zetten en DBCC CHECKDB uitvoeren. Als er problemen zijn, lost SQL-Database proactief ze. [Automatisch herstel](/sql/sql-server/failover-clusters/automatic-page-repair-availability-groups-database-mirroring) wordt gebruikt voor het oplossen van pagina's die zijn beschadigd of problemen met de gegevensintegriteit. De database-pagina's worden altijd geverifieerd met de standaardinstelling van de CONTROLESOM die controleert of de integriteit van de pagina. SQL-Database proactief bewaakt en controleert de integriteit van gegevens van uw database en, als er zich problemen voordoen, lost ze met de hoogste prioriteit. Naast deze kunt u desgewenst uw eigen integriteitscontroles op wordt uitgevoerd.  Zie voor meer informatie, [integriteit van gegevens in SQL-Database](https://azure.microsoft.com/blog/data-integrity-in-azure-sql-database/)

## <a name="data-movement-after-migration"></a>Verplaatsing van gegevens na de migratie

### <a name="how-do-i-export-and-import-data-as-bacpac-files-from-sql-database"></a>Hoe ik gegevens exporteren en importeren als Bacpac-bestanden van SQL Database?

- **Exporteren**: U kunt uw Azure SQL-database exporteren als een BACPAC-bestand vanuit de Azure-portal

   ![database exporteren](./media/sql-database-export/database-export.png)

- **Importeren**: U kunt ook gegevens als een BACPAC-bestand importeren in de database met behulp van de Azure-portal.

   ![database importeren](./media/sql-database-import/import.png)

### <a name="how-do-i-synchronize-data-between-sql-database-and-sql-server"></a>Synchroniseren van gegevens tussen SQL Database en SQL Server?
Er zijn verschillende manieren om dit te bereiken: 
- **[Gegevenssynchronisatie](sql-database-sync-data.md)**  : deze functie kunt u gegevens synchroniseren richtingen tussen meerdere on-premises SQL Server-databases en SQL-Database. Als u wilt synchroniseren met on-premises SQL Server-databases, die u wilt installeren en configureren van sync-agent op een lokale computer en de uitgaande TCP-poort 1433 openen.
- **[Transactionele replicatie](https://azure.microsoft.com/blog/transactional-replication-to-azure-sql-database-is-now-generally-available/)**  – met transactionele replicatie kunt u uw gegevens van on-premises naar Azure SQL DB synchroniseren met de on-premises wordt de uitgever en de Azure SQL-database wordt de abonnee. Alleen voor deze installatie wordt nu ondersteund. Zie voor meer informatie over het migreren van uw gegevens van on-premises naar Azure SQL met minimale downtime: [gebruik transactionele replicatie](sql-database-cloud-migrate.md#method-2-use-transactional-replication)

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [SQL-Database](sql-database-technical-overview.md).

