---
title: Na de migratie - Azure SQL Database beheren | Microsoft Docs
description: Informatie over het beheren van uw database na de migratie naar Azure SQL Database.
services: sql-database
documentationcenter: 
author: joesackmsft
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: migrate
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Inactive
ms.date: 12/06/2016
ms.author: Joe.Sack
ms.suite: sql
ms.prod_service: sql-database
ms.component: migration
ms.openlocfilehash: b65236fb2d11473d626ee2602237ed4a49380702
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="new-dba-in-the-cloud--managing-your-database-in-azure-sql-database"></a>Nieuwe DBA in de cloud: uw database in Azure SQL Database beheren

Verplaatsen van de traditionele zelf wordt beheerd, automatisch gecontroleerde omgeving met een PaaS-omgeving kan enigszins overweldigend lijken op het eerste. Als een ontwikkelaar van de app of een DBA, zou u wilt weten van de belangrijkste mogelijkheden van het platform waarmee u uw toepassing beschikbaar is, zodat, veilige en robuuste - altijd. In dit artikel is erop gericht precies dat doet. Het artikel bondig organiseert bronnen en biedt u enkele richtlijnen over het optimaal gebruik van de belangrijkste mogelijkheden van SQL Database beheren en uw toepassing efficiënt houden en optimale resultaten in de cloud bereikt. Typische publiek voor dit artikel zou zijn de eigenschappen die:-
- Migratie van hun toepassingen met Azure SQL DB – modernisering van uw toepassingen beoordeelt.
- Zijn bezig met hun toepassing(en) – continu migratiescenario's migreren.
- De migratie naar Azure SQL DB – nieuwe DBA in de cloud onlangs voltooid.

Dit artikel worden enkele van de belangrijkste kenmerken van Azure SQL-database als een platform waarmee u gemakkelijk gebruikmaken van kunt. De volgende zijn:- 
- Zakelijke continuïteit en herstel na noodgevallen (BCDR)
- Veiligheid en naleving
- Intelligent database bewaken en onderhoud
- Gegevensverplaatsing


## <a name="business-continuity-and-disaster-recovery-bcdr"></a>Zakelijke continuïteit en herstel na noodgevallen (BCDR)
Mogelijkheden voor zakelijke continuïteit en noodherstel recovery kunnen u uw bedrijf, zoals gebruikelijk gaan als er een ramp optreedt. De noodherstel mogelijk een gebeurtenis voor het niveau van database (bijvoorbeeld iemand per ongeluk zakt een cruciaal tabel) of een datacenter niveau gebeurtenis (regionale ramp, bijvoorbeeld een tsunami). 

### <a name="how-do-i-create-and-manage-backups-on-sql-database"></a>Hoe ik maken en beheren van back-ups op SQL-Database?
U geen back-ups maken op Azure SQL DB en dat is omdat u niet hoeft te. SQL-Database automatisch back-ups van databases voor u, zodat u niet langer over planning, maken en beheren van back-ups moet zorgen. Het platform duurt een volledige back-up elke week differentiële dat back-up om de paar uur en een logboek back-up maken om de 5 minuten om te controleren of het herstel na noodgevallen is efficiënter en de minimale gegevens verloren gaan. De eerste volledige back-up gebeurt er als u een database maken. Deze back-ups voor u beschikbaar zijn voor een bepaalde periode de 'bewaarperiode' genoemd en is afhankelijk van de prestatielaag die u kiest.  SQL-Database biedt u de mogelijkheid om terug te zetten naar een willekeurig punt in tijd binnen deze bewaren periode met [punt in tijd herstel (PITR)](sql-database-recovery-using-backups.md#point-in-time-restore).

|Prestatielaag|Bewaartermijn in dagen|
|---|:---:|
|Basic|7|
|Standard|35|
|Premium|35|
|||

Bovendien de [Long-Term bewaren (LTR)](sql-database-long-term-retention.md) functie kunt u bevatten naar de back-upbestanden voor een veel langere periode in het bijzonder voor maximaal tien jaar en gegevens terugzetten vanuit deze back-ups op elk gewenst moment binnen die periode. Bovendien worden de databaseback-ups op een gegevensopslag geogerepliceerde om ervoor te zorgen herstelmogelijkheden van regionale ramp bewaard. U kunt ook deze back-ups in een Azure-regio op een tijdstip binnen de bewaarperiode herstellen. Zie [Business continuity overview](sql-database-business-continuity.md).

### <a name="how-do-i-ensure-business-continuity-in-the-event-of-a-datacenter-level-disaster-or-regional-catastrophe"></a>Hoe kan ik ervoor zorgen dat zakelijke continuïteit in geval van een datacenter-niveau na noodgevallen of regionale ramp?
Omdat de back-ups van uw database worden opgeslagen op een opslagsubsysteem ervoor te zorgen dat een regionale noodgevallen geo-replicatie, kunt u de back-up herstellen naar een andere Azure-regio. Dit wordt geo-herstel genoemd. De RPO (beoogd herstelpunt) voor dit wordt doorgaans < 1 uur en het invoegen (geschatte hersteltijd – enkele minuten tot uur).

Voor databases die bedrijfskritieke biedt Azure SQL DB, actieve geo-replicatie. Wat dit in feite heeft, is dat er een geo-replicatie secundaire kopie van de oorspronkelijke database in een andere regio gemaakt. Als bijvoorbeeld uw database in eerste instantie wordt gehost in Azure West ons regio en gewenste regionale herstelmogelijkheden. U maakt een actieve geo-replica van de database in VS-West om in te spreken VS-Oost. Als de calamity in VS-West juist, kunt u failover voor de regio VS-Oost. Ze configureren in de groep van een automatisch Failover is nog beter omdat dit zorgt ervoor dat de database automatisch wordt overgenomen door de secundaire in VS-Oost als er een ramp optreedt. De RPO voor deze is < 5 en het invoegen < 30 seconden.

Als een automatisch failover-groep niet is geconfigureerd, klikt u vervolgens moet uw toepassing actief voor een noodgeval bewaken en start een failover naar de secundaire. U kunt maximaal 4 dergelijke actieve geo-replica's maken in verschillende Azure-regio's. Het ophalen van nog beter. U kunt ook toegang tot deze secundaire actieve geo-replica's voor alleen-lezen toegang. Dit is erg handig om te beperken latentie voor een scenario geo gedistribueerde toepassing. 

### <a name="how-does-my-disaster-recovery-plan-change-from-on-premises-to-sql-database"></a>Hoe wordt mijn noodherstelplan gewijzigd van on-premises SQL-database?
De traditionele on-premises SQL Server-installatie vereist Kortom, u actief uw beschikbaarheid met behulp van functies zoals Failover Clustering, databasespiegeling transactiereplicatie logboek back-upfunctie enzovoort beheren en onderhouden en beheren van back-ups om ervoor te zorgen Zakelijke continuïteit. Met SQL-Database beheert het platform deze voor u, zodat u kunt zich richten op het ontwikkelen en optimaliseren van uw databasetoepassing en u geen over het beheer van noodherstel zo veel mogelijk zorgen. U kunt back-up en plannen voor herstel na noodgevallen geconfigureerd en werken met een paar klikken op de Azure-portal (of een aantal opdrachten met de PowerShell APIs) hebben. 

Zie voor meer informatie over herstel na noodgevallen: [Azure SQL Db Disaster Recovery 101](https://azure.microsoft.com/blog/azure-sql-databases-disaster-recovery-101/)

## <a name="security-and-compliance"></a>Veiligheid en naleving
SQL-Database hecht veel waarde beveiliging en Privacy. Beveiliging in SQL-Database is beschikbaar op het databaseniveau van de en op het niveau van het platform en het best begrijpen wanneer onderverdeeld in meerdere lagen. Bij elke laag krijgt u te beheren en optimale beveiliging bieden voor uw toepassing. De lagen zijn:
- Identiteit en verificatie ([Windows/SQL-verificatie en verificatie van Azure Active Directory [AAD]](sql-database-control-access.md)).
- Bewaking van activiteit ([controle](sql-database-auditing.md) en [bedreigingendetectie](sql-database-threat-detection.md)).
- Werkelijke gegevens beveiligen ([Transparent Data Encryption [TDE]](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) en [altijd versleuteld [AE]](/sql/relational-databases/security/encryption/always-encrypted-database-engine)). 
- Beheren van toegang tot gevoelige en bevoorrechte gegevens ([rij beveiligingsniveau](/sql/relational-databases/security/row-level-security) en [dynamische-Gegevensmaskering](/sql/relational-databases/security/dynamic-data-masking)).

[Azure Security Center](https://azure.microsoft.com/services/security-center/) biedt gecentraliseerd beheer over de werkbelastingen die worden uitgevoerd in Azure, on-premises en in andere clouds. U kunt weergeven of essentieel beveiliging van de SQL-Database zoals [controle](sql-database-auditing.md) en [Transparent Data Encryption [TDE]](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) zijn geconfigureerd op alle bronnen en beleid op basis van uw eigen vereisten maken.

### <a name="what-user-authentication-methods-are-offered-in-sql-database"></a>Welke gebruikersverificatiemethoden zijn beschikbaar in SQL-Database?
Er zijn [twee verificatiemethoden](sql-database-control-access.md#authentication) aangeboden in SQL-Database: 
- [Azure Active Directory-verificatie](sql-database-aad-authentication.md)
- SQL-verificatie. 

De traditionele windows-verificatie wordt niet ondersteund. Azure Active Directory (AD) is een gecentraliseerde service voor identiteits- en toegangsbeheer. Dit kunt u heel gemakkelijk een eenmalige aanmelding toegang (SSO) opgeven voor alle medewerkers in uw organisatie. Dit betekent dat de referenties worden gedeeld door alle Azure-services voor eenvoudigere verificatie. Biedt ondersteuning voor AAD [MFA (Multi-factor Authentication)](sql-database-ssms-mfa-authentication.md) en een met een [enkele muisklikken](../active-directory/connect/active-directory-aadconnect-get-started-express.md) AAD kan worden geïntegreerd met Windows Server Active Directory. SQL-verificatie werkt precies zoals u hebt gebruikt deze in het verleden. U geeft een gebruikersnaam en wachtwoord en u kunt gebruikers met een database op een bepaalde logische server verifiëren. Hierdoor kunnen ook SQL-Database en SQL Data Warehouse te bieden multi-factor authentication en Gast gebruikersaccounts in Azure AD-domein. Als u een Active Directory on-premises al hebt, kunt u de map met Azure Active Directory om uw directory naar Azure uitbreiden federeren.

|**Als u...**|**SQL-Database / SQL-datawarehouse**|
|---|---|
|De voorkeur geeft niet aan Azure Active Directory (AD) in Azure|Gebruik [SQL-verificatie](sql-database-security-overview.md)|
|Gebruikte AD op SQL Server on-premises|[AD met Azure AD federeren](../active-directory/connect/active-directory-aadconnect.md), en Azure AD-verificatie te gebruiken. Hiermee kunt u eenmalige aanmelding.|
|Moet multi-factor authentication (MFA) af te dwingen|MFA is vereist als een beleid via [voorwaardelijke toegang van Microsoft](sql-database-conditional-access.md), en gebruik [Universal van Azure AD authentication met ondersteuning voor MFA](sql-database-ssms-mfa-authentication.md).|
|Gastaccounts van Microsoft-accounts (live.com, outlook.com) of andere domeinen (gmail.com) hebben|Gebruik [Azure AD Universal verificatie](sql-database-ssms-mfa-authentication.md) in SQL Database/Data Warehouse, die gebruikmaakt van [Azure AD B2B-samenwerking](../active-directory/active-directory-b2b-what-is-azure-ad-b2b.md).|
|Zijn Windows met behulp van uw Azure AD-referenties van een federatieve domein aangemeld|Gebruik [Azure AD-geïntegreerde verificatie](sql-database-aad-authentication-configure.md).|
|Zijn aangemeld bij Windows met referenties van een domein niet gefedereerd met Azure|Gebruik [Azure AD-geïntegreerde verificatie](sql-database-aad-authentication-configure.md).|
|Hebben de middelste laag diensten die verbinding moeten maken met SQL-Database of SQL Data Warehouse|Gebruik [Azure AD-geïntegreerde verificatie](sql-database-aad-authentication-configure.md).|
|||

### <a name="how-do-i-limit-or-control-connectivity-access-to-my-database"></a>Hoe beperken of beheren ik toegang tot mijn database connectivity?
Er zijn meerdere technieken tot uw beschikking staan die u gebruiken kan om de organisatie van de optimale connectiviteit voor uw toepassing bereiken. 
- Firewallregels
- VNET-Service-eindpunten
- Gereserveerde IP-adressen

#### <a name="firewall"></a>Firewall
Een firewall voorkomt toegang tot uw server uit een externe entiteit doordat alleen specifieke entiteiten toegang tot uw logische server. Standaard alle verbindingen en databases binnen de logische server zijn niet toegestaan, behalve de verbindingen die afkomstig zijn van andere Azure-Services in. U kunt de toegang met de server alleen voor entiteiten (bijvoorbeeld een developer-machine) die u goedkeuren, doordat IP-adres van de computer via de firewall openen met een firewallregel. Ook kunt u een bereik van IP-adressen die u wilt toestaan toegang tot de logische server opgeven. Ontwikkelaars machine IP-adressen in uw organisatie kunnen bijvoorbeeld in één keer worden toegevoegd door te geven van een bereik op de pagina van de Firewall-instellingen. 

U kunt firewallregels op serverniveau of op het databaseniveau. Niveau firewallregels voor server kunnen via de portal of via SSMS gemaakt. Zie voor meer informatie over het instellen van een server en een firewallregel op database,: [firewallregels maken in SQL-Database](sql-database-security-tutorial.md#create-a-server-level-firewall-rule-in-the-azure-portal).

#### <a name="service-endpoints"></a>Service-eindpunten
De SQL-database is standaard geconfigureerd voor 'Alle Azure-services toestaan' – wat betekent dat elke virtuele Machine in Azure kan proberen verbinding maken met uw database. Deze pogingen nog steeds hoeft te worden geverifieerd. Echter, als u niet toegankelijk zijn voor een Azure-IP-adressen van uw database wilt, kunt u uitschakelen 'Alle Azure-services toestaan'. Bovendien kunt u configureren [VNET Service-eindpunten](sql-database-vnet-service-endpoint-rule-overview.md).

Service-eindpunten (SE) kunnen u uw essentiële Azure-resources alleen voor uw eigen persoonlijke virtuele netwerk in Azure beschikbaar. Door dit te doen, elimineren u in feite openbare toegang tot uw resources. Het verkeer tussen het virtuele netwerk naar Azure blijft van toepassing op het Azure-backbone-netwerk. Zonder SE krijgt u routering van pakketten geforceerde tunneling. Het virtuele netwerk zorgt ervoor dat de internet-verkeer voor uw organisatie en de Azure-Service om te gaan via dezelfde route. Met de Service-eindpunten, kunt u optimaliseren dit omdat de stroom van pakketten rechtstreeks vanuit uw virtuele netwerk met de service op Azure-backbone-netwerk.

![VNET-service-eindpunten](./media/sql-database-manage-after-migration/vnet-service-endpoints.png) 

#### <a name="reserved-ips"></a>Gereserveerde IP-adressen
Een andere optie is om in te richten [gereserveerd IP-adressen](../virtual-network/virtual-networks-reserved-public-ip.md) voor uw virtuele machines en de goedgekeurde IP-adressen die specifieke VM IP-in de server adressen firewall-instellingen. Gereserveerd IP-adressen toe te wijzen, bespaart u het niet meer hoeft bij het bijwerken van de firewall-regels met het wijzigen van IP-adressen.

### <a name="what-port-do-i-connect-to-sql-database-on"></a>Welke poort kan ik verbinden met SQL-Database op?

Poort 1433. SQL-Database communiceert via deze poort. Als u wilt verbinding maken vanaf een bedrijfsnetwerk, die u moet een uitgaande regel toevoegen in de firewall-instellingen van uw organisatie. Vermijd poort 1433 buiten de grens Azure blootstellen als uitgangspunt. U kunt SSMS uitvoeren in Azure worden verkregen met [Azure RemoteApp](https://www.microsoft.com/cloud-platform/azure-remoteapp-client-apps). Dit vereist niet dat u uitgaande verbindingen om poort 1433 openen, het IP-adres is statisch, zodat de database geopend zodat u alleen de RemoteApp worden kan- en Multi Factor Authentication (MFA ondersteunt).

### <a name="how-can-i-monitor-and-regulate-activity-on-my-server-and-database-in-sql-database"></a>Hoe kan ik controleren en regelen activiteit op mijn server en database in SQL-Database?
#### <a name="sql-database-auditing"></a>SQL Database Auditing
Met SQL-Database, kunt u de controle op databasegebeurtenissen bijhouden. [SQL Database Auditing](sql-database-auditing.md) databasegebeurtenissen vastgelegd en schrijft deze naar een controlelogboekbestand in uw Azure-Opslagaccount. Controle is vooral nuttig als u wilt meer inzicht krijgen in mogelijke overtredingen van beveiligings- en -beleid, onderhouden van naleving van regelgeving enzovoort. Hiermee kunt u voor het definiëren en configureren van bepaalde categorieën van gebeurtenissen die u denkt dat moet controle en op basis van dat u kunt vooraf geconfigureerde rapporten en een dashboard om een overzicht van gebeurtenissen die plaatsvinden op de database ophalen. U kunt deze controlebeleid op databaseniveau of op serverniveau kunt toepassen. Een handleiding voor het inschakelen van controle-instellingen voor uw server/database, Zie: [inschakelen SQL Database Auditing](sql-database-security-tutorial.md#enable-sql-database-auditing-if-necessary).

#### <a name="threat-detection"></a>Detectie van bedreigingen
Met [bedreigingendetectie](sql-database-threat-detection.md), krijgt u de mogelijkheid om te reageren op beveiligings- of schendingen worden gedetecteerd door te controleren heel eenvoudig. U hoeft niet te worden van een beveiligingsbeleid voor deskundige voor het oplossen van potentiële bedreigingen of schendingen in uw systeem. Detectie van dreigingen heeft ook een aantal ingebouwde mogelijkheden, zoals SQL-injectie detectie. SQL-injectie is een poging om te wijzigen of de gegevens en een gebruikelijk manier aanvallen van de databasetoepassing van een in het algemeen in gevaar brengen. Detectie van dreigingen voor SQL-Database wordt uitgevoerd meerdere sets van algoritmen die mogelijke beveiligingsproblemen en SQL-injectieaanvallen, evenals de toegangspatronen afwijkende database (zoals de toegang van een ongebruikelijke locatie of door een onbekende-principal) te detecteren. -Afdelingen of andere aangewezen beheerders ontvangen een e-mailmeldingen als een bedreiging wordt gedetecteerd op de database. Elke melding vindt details van de verdachte activiteit en aanbevelingen voor het verder onderzoek en risico. Zie voor meer informatie over het inschakelen van de detectie van dreigingen,: [Bedreigingsdetectie van de SQL-Database inschakelen](sql-database-security-tutorial.md#enable-sql-database-threat-detection). 
### <a name="how-do-i-protect-my-data-in-general-on-sql-database"></a>Hoe kan ik beschermen mijn gegevens in het algemeen op SQL-Database?
Versleuteling biedt een krachtig mechanisme om te beveiligen en uw gevoelige gegevens beveiligen tegen indringers. Versleutelde gegevens is de indringer zonder de ontsleutelingssleutel niet gebruikt. Daarom wordt er een extra beschermingslaag boven op de bestaande beveiligingslagen ingebouwd in SQL-Database toegevoegd. Er zijn twee aspecten bescherming van uw gegevens in SQL-Database: 
- Uw gegevens in rust in de bestanden voor gegevens en logboekbestanden 
- Uw gegevens die onderweg is. 

In SQL-Database, standaard uw gegevens in rust in de gegevens en logboekbestanden bestanden op het opslagsubsysteem volledig en altijd versleuteld [Transparent Data Encryption [TDE]](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql). Uw back-ups zijn eveneens versleuteld. Met TDE er zijn geen wijzigingen aan de kant van de toepassing die toegang heeft tot deze gegevens vereist. De versleuteling en ontsleuteling gebeuren transparant; daarom de naam. Voor uw gevoelige gegevens onderweg te beschermen en in rust, SQL-Database bevat een functie [altijd versleuteld (AE)](/sql/relational-databases/security/encryption/always-encrypted-database-engine). AE is een vorm van versleuteling voor clientzijde waarmee gevoelige kolommen in de database versleuteld (zodat ze in gecodeerde tekst voor databasebeheerders en niet-geautoriseerde gebruikers). De server ontvangt de versleutelde gegevens beginnen. De sleutel voor altijd versleuteld wordt ook opgeslagen op de client, zodat alleen geautoriseerde clients om de gevoelige kolommen te ontsleutelen. De server en Gegevensbeheerders kunnen niet zien de gevoelige gegevens omdat de versleutelingssleutels worden opgeslagen op de client. AE versleutelt gevoelige kolommen in de tabel end-to-end van niet-geautoriseerde clients naar de fysieke schijf. AE ondersteunt gelijkheid vergelijkingen vandaag de dag, zodat DBA's kunnen blijven opvragen versleutelde kolommen als onderdeel van de SQL-opdrachten. Altijd versleutelde kan worden gebruikt met een groot aantal sleutelarchief opties, zoals [Azure Key Vault](sql-database-always-encrypted-azure-key-vault.md), Windows-certificaatarchief en lokale hardware security modules.

|**Kenmerken**|**Altijd versleuteld.**|**Transparante gegevensversleuteling**|
|---|---|---|
|**Codering in beslag neemt**|End-to-end|Gegevens in rust|
|**Databaseserver heeft toegang tot gevoelige gegevens**|Nee|Ja, omdat versleuteling voor de gegevens in rust|
|**Toegestane T-SQL-bewerkingen**|Gelijkheidsvergelijking|Alle T-SQL-oppervlak is beschikbaar|
|**Appwijzigingen die zijn vereist om de functie te gebruiken**|minimale|Minimale|
|**Versleuteling granulatie**|Kolomniveau|Databaseniveau|
||||

### <a name="how-can-i-limit-access-to-sensitive-data-in-my-database"></a>Hoe beperk ik toegang tot gevoelige gegevens in mijn database
Elke toepassing heeft een bepaalde bits gevoelige gegevens in de database die worden beschermd moet tegen wordt voor iedereen zichtbaar. Bepaalde personeel binnen de organisatie, moet deze gegevens mogen bekijken, maar anderen mag niet kunnen deze gegevens mogen bekijken. Een voorbeeld hiervan is salarissen. Een manager moet toegang tot de informatie salaris van stelt zijn/haar directe ondergeschikten echter, de afzonderlijke teamleden mag geen toegang tot de informatie salaris van hun collega's. Een ander scenario is gegevens ontwikkelaars die mogelijk interactie met gevoelige gegevens tijdens de ontwikkelingsstadia of testen, bijvoorbeeld burgerservicenummers van klanten. Deze gegevens niet opnieuw hoeft te worden blootgesteld aan de ontwikkelaar. In dergelijke gevallen moet uw vertrouwelijke gegevens ofwel worden gemaskeerd of niet helemaal worden weergegeven. SQL-Database biedt twee manieren om te voorkomen dat onbevoegde gebruikers kunnen gevoelige gegevens weergeven:

[Dynamische-Gegevensmaskering](sql-database-dynamic-data-masking-get-started.md) is een functie voor het maskeren van gegevens waarmee u kunt het beperken van blootstelling van gevoelige gegevens door het maskeren voor onbevoegde gebruikers op de toepassingslaag. Definieert u een maskeringsregel dat maskering patroon kunt maken (bijvoorbeeld alleen weergeven laatste vier cijfers van een nationale ID SSN: XXX-XX-0000 en markeren als Xs de meeste) en nagaan welke gebruikers moeten worden uitgesloten van de maskeringsregel. Het maskeren gebeurt tijdens het en er zijn verschillende maskeerfuncties beschikbaar voor verschillende gegevenscategorieën. Maskering van dynamische gegevens kunt u automatisch detecteren van gevoelige gegevens in uw database en maskering erop van toepassing.

[Beveiliging op rijniveau rij](/sql/relational-databases/security/row-level-security) kunt u toegangsbeheer op het rijniveau van de. Wat betekent dat zijn bepaalde rijen in een databasetabel op basis van de gebruiker uitvoeren van de query (groep lidmaatschap of -uitvoering context) verborgen. De beperking van de toegang is uitgevoerd op de databaselaag in plaats van in een toepassingslaag voor het vereenvoudigen van de logische app. U begint met het maken van een filterpredicaat, worden uitgefilterd rijen die niet worden weergegeven en de beveiliging beleid volgende bepalen wie toegang tot deze rijen heeft. Ten slotte de eindgebruiker hun query wordt uitgevoerd en, afhankelijk van de bevoegdheden van de gebruiker, de beperkte rijen weergeven of Zie helemaal niet.

### <a name="how-do-i-manage-encryption-keys-in-the-cloud"></a>Hoe beheer ik versleutelingssleutels in de cloud?

Er zijn opties voor sleutelbeheer voor altijd versleuteld (versleuteling aan clientzijde) en Transparent Data Encryption (versleuteling-at-rest). Het raadzaam dat u regelmatig versleutelingssleutels roteren. De frequentie van de draaihoek moet worden uitgelijnd met uw interne organisatie voorschriften- en nalevingsvereisten.

**Transparent Data Encryption (TDE)**: Er is een hiërarchie met twee-sleutel in TDE: de gegevens in de gebruikersdatabase van elke is versleuteld met een symmetrische AES-256 database unieke databaseversleutelingssleutel (DEK) die op zijn beurt is versleuteld met een server unieke asymmetrische RSA 2048 hoofdsleutel. De hoofdsleutel kunnen worden beheerd op:
- Automatisch door het platform - SQL-Database.
- Of door u met behulp van [Azure Key Vault](sql-database-always-encrypted-azure-key-vault.md) als de sleutel store.

Standaard wordt de hoofdsleutel voor Transparent Data Encryption beheerd door de service SQL Database voor het gemak. Als uw organisatie besturen de hoofdsleutel wilt, is er een optie voor het gebruik van Azure sleutel Vault](sql-database-always-encrypted-azure-key-vault.md) als de sleutel store. Met behulp van Azure Sleutelkluis, uw organisatie wordt ervan uitgegaan dat controle over de belangrijkste inrichten, draaiing en machtiging besturingselementen. [Rotatie of overschakelen van het type van een hoofdsleutel TDE](/sql/relational-databases/security/encryption/transparent-data-encryption-byok-azure-sql-key-rotation) is snel, omdat deze de DEK alleen opnieuw versleutelt. Voor organisaties met scheiding van functies tussen beveiliging en beheer van kan een beheerder van de beveiliging inrichten van het sleutelmateriaal voor de hoofdsleutel TDE in Azure Sleutelkluis en een sleutel-id van Azure Sleutelkluis bieden aan de beheerder van de database moet worden gebruikt voor codering in rust op een server. De Sleutelkluis is zodanig ontworpen dat Microsoft niet kan zien of eventuele versleutelingssleutels extraheren. U kunt ook een gecentraliseerd beheer van sleutels ophalen voor uw organisatie. 

**Altijd versleuteld**: Er is ook een [twee toetsen hiërarchie](/sql/relational-databases/security/encryption/overview-of-key-management-for-always-encrypted) in altijd versleuteld - een kolom met gevoelige gegevens gecodeerd met een AES 256 kolomversleutelingssleutel (CEK) die op zijn beurt is versleuteld met een hoofdsleutel voor kolom (CMK). De clientstuurprogramma's voor altijd versleuteld hebben geen beperkingen op de lengte van CMKs. De versleutelde waarde van de CEK is opgeslagen op de database en de CMK is opgeslagen in een vertrouwde sleutelarchief, zoals Windows-certificaatarchief, Azure Key Vault of een hardware security module. 
- Zowel de [CEK en CMK](/sql/relational-databases/security/encryption/rotate-always-encrypted-keys-using-powershell) kunnen worden gedraaid. 
- CEK rotatie heeft een grootte van gegevens en kan worden tijdrovende afhankelijk van de grootte van de tabellen met daarin de versleutelde kolommen. Daarom is het verstandig om te plannen CEK draaiingen dienovereenkomstig. 
- CMK worden gedraaid echter niet van invloed op prestaties van de database, en kunt u doen met gescheiden rollen.
Het volgende diagram toont de sleutelarchief opties voor de kolom hoofdsleutels in altijd versleuteld.

![Altijd versleuteld CMK providers opslaan](./media/sql-database-manage-after-migration/always-encrypted.png)

### <a name="how-can-i-optimize-and-secure-the-traffic-between-my-organization-and-sql-database"></a>Hoe kan ik optimaliseren en het verkeer tussen mijn organisatie en de SQL-Database beveiligen?
Het netwerkverkeer tussen uw organisatie en de SQL-Database zou in het algemeen via het openbare netwerk ophalen doorgestuurd. Echter, als u wilt dit pad te optimaliseren en u beter kunt beveiligen, kunt u zoeken in Express Route. Express route in wezen kunt u uw bedrijfsnetwerk via een particuliere verbinding uitbreiden het Azure-platform. Door dit te doen, bent u niet verder via het openbare Internet. U betere beveiliging, betrouwbaarheid en routering optimalisatie dat wordt omgezet in lagere netwerklatenties ook ophalen en veel sneller en hebben ze dan u normaal zou optreden gaan via het openbare internet. Als u van plan bent voor het overdragen van een aanzienlijke hoeveelheid gegevens tussen uw organisatie en Azure, kan met behulp van Express Route levert kostenvoordelen op. U kunt kiezen uit drie verschillende connectiviteitsmodellen voor de verbinding van uw organisatie naar Azure: 
- [Cloud Exchange CO-locatie](../expressroute/expressroute-connectivity-models.md#CloudExchange)
- [Any-to-any](../expressroute/expressroute-connectivity-models.md#IPVPN)
- [Point-to-Point](../expressroute/expressroute-connectivity-models.md#Ethernet)

Express Route kunt u burst tot 2 x de Bandbreedtelimiet die u voor zonder extra kosten koopt. Het is ook mogelijk om te configureren tussen regio connectiviteit met behulp van Express route. Zie voor een lijst met ER connectiviteitsproviders: [Express Route-Partners en locaties Peering](../expressroute/expressroute-locations.md). De volgende artikelen wordt Express Route in meer detail beschreven:
- [Inleiding op Express Route](../expressroute/expressroute-introduction.md)
- [Vereisten](../expressroute/expressroute-prerequisites.md)
- [Werkstromen](../expressroute/expressroute-workflows.md)

### <a name="is-sql-database-compliant-with-any-regulatory-requirements-and-how-does-that-help-with-my-own-organizations-compliance"></a>SQL-Database voldoen aan eventuele voorschriften is en hoe helpt die bij de naleving van mijn eigen organisatie?
SQL-Database is compatibel met een bereik van de regelgeving conformiteit. Als u wilt weergeven van de meest recente set conformiteit die wordt voldaan, gaat u naar de [Microsoft Trust Center](https://www.microsoft.com/trustcenter/compliance/complianceofferings) en inzoomen op de conformiteit die belangrijk voor uw organisatie om te zien zijn als de SQL-Database is opgenomen onder de compatibele Azure-services. Het is belangrijk te weten dat Hoewel SQL-Database kan worden goedgekeurd als een compatibele service, het helpt de compatibiliteit van uw organisatie service, maar kan deze niet automatisch garanderen.

## <a name="intelligent-database-monitoring-and-maintenance-after-migration"></a>Intelligent database bewaking en het onderhoud na migratie

Zodra u hebt uw database in SQL-Database hebt gemigreerd, u wilt bewaken van uw database (voor bijvoorbeeld Controleer hoe het Resourcegebruik lijkt of DBCC controles) en uitvoeren van regelmatig onderhoud (bijvoorbeeld opnieuw samenstellen of reorganize indexen, statistieken enz.). SQL-Database is gelukkig intelligente in de zin dat deze gebruikmaakt van de historische trends en opgenomen metrische gegevens en statistieken om proactief te bewaken en onderhouden van uw database, zodat uw toepassing optimaal altijd wordt uitgevoerd. Azure SQL DB in sommige gevallen kunt automatisch onderhoudstaken uitvoeren, afhankelijk van uw configuratie-instellingen uitvoeren. Er zijn drie facetten te bewaken van uw database in SQL-Database:
- Prestaties bewaken en optimaliseren.
- Optimalisatie van de beveiliging.
- Kosten optimalisatie.

**Bewaking van toepassingsprestaties en optimalisatie van**: met Query Performance Insights, kunt u krijgen op maat gemaakte aanbevelingen voor de werkbelasting van uw database zodat uw toepassingen blijven op voor optimale prestaties - altijd uitvoeren kunnen. U kunt ook instellen deze zodat deze aanbevelingen automatisch toegepast en er geen zin presterende onderhoudstaken. Kunt u de indexaanbevelingen op basis van uw werkbelasting automatisch implementeren met Index Advisor - dit automatische afstemming wordt genoemd. De aanbevelingen ontwikkelen als uw toepassing werklast wijzigingen om te voorzien van de meest relevante suggesties. Ook krijgt u de mogelijkheid handmatig deze aanbevelingen bekijken en naar eigen inzicht toegepast.  

**Beveiliging optimalisatie**: SQL-Database bevat bruikbare beveiligingsaanbevelingen voor het beveiligen van uw gegevens en de detectie van dreigingen voor het identificeren en te onderzoeken verdachte databaseactiviteiten die een mogelijke thread kunnen inhouden de de database. [Controle op beveiligingslekken SQL](sql-vulnerability-assessment.md) is een database scannen en reporting service waarmee u de beveiligingsstatus van uw databases op grote schaal bewaken en beveiligingsrisico's identificeren en het nemen van de door u gedefinieerde basisbeveiliging. Na elke scan een aangepaste lijst met bruikbare stappen en herstel scripts vindt, evenals een evaluatie-rapport dat kan worden gebruikt voor het helpen om te voldoen aan nalevingsvereisten.

Met Azure Security Center aanbevelingen voor de beveiliging over de hele linie identificeren en deze met een enkele klik toepassen. 

**Optimalisatie kosten**: Azure SQL-platform analyseert de geschiedenis gebruik voor de databases in een server om te evalueren en kosten-optimalisatie-instellingen voor u het beste. Deze analyse duurt normaal gesproken een twee te analyseren en bouwen uitvoerbare aanbevelingen. Elastische groep is een van deze opties. De aanbeveling op de portal wordt weergegeven als een banner:

![aanbevelingen voor elastische Pools](./media/sql-database-manage-after-migration/elastic-pool-recommendations.png) 

U kunt ook deze analyse onder de sectie 'Advisor' bekijken:

![elastische pool aanbevelingen-advisor](./media/sql-database-manage-after-migration/advisor-section.png)

### <a name="how-do-i-monitor-the-performance-and-resource-utilization-in-sql-database"></a>Hoe bewaak ik de prestaties en het Resourcegebruik in SQL-Database

In SQL-Database kunt u gebruikmaken van de intelligent inzichten van het platform voor het bewaken van de prestaties en dienovereenkomstig afstemmen. U kunt bewaken prestaties en het brongebruik in SQL-Database met behulp van de volgende methoden:
- **Azure-portal**: ziet u een individuele database gebruik door de database selecteren en te klikken op de grafiek in het overzichtsvenster van de Azure portal. U kunt de grafiek om weer te geven van meerdere metrische gegevens, met inbegrip van CPU-percentage, DTU-percentage, gegevens-IO-percentage, sessies percentage en Database als percentage van asgrootte wijzigen.

   ![Bewaking van de grafiek](./media/sql-database-manage-after-migration/monitoring-chart.png)

   ![Bewaking van grafiek2](./media/sql-database-manage-after-migration/chart.png)

U kunt ook waarschuwingen door de bron van deze grafiek configureren. Deze waarschuwingen kunnen u reageren op de voorwaarden van de resource met een e-mailbericht, het schrijven naar een eindpunt HTTPS/HTTP- of een actie uitvoert. Zie de [databaseprestaties bewaken in SQL-Database](sql-database-single-database-monitor.md) voor gedetailleerde instructies.

- **Dynamische beheerweergaven**: U kunt een query de [sys.dm_db_resource_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) dynamische Beheerweergave resource verbruik statistieken geschiedenis van het afgelopen uur retourneren en de [sys.resource_stats ](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database) system catalogusweergave geschiedenis retourneren voor de afgelopen 14 dagen.
- **Query Performance Insight**: [Query Performance Insight](sql-database-query-performance.md) kunt u een historisch overzicht van de top-resource verbruikende query's en langlopende query's voor een specifieke database. U kunt snel TOP-query's door Resourcegebruik, duur en frequentie van de uitvoering van identificeren. U kunt query's volgen en regressie detecteren. Dit onderdeel vereist [Query Store](/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store) worden ingeschakeld en actief is voor de database.

   ![Inzicht in queryprestaties](./media/sql-database-manage-after-migration/query-performance-insight.png)

- **Azure SQL-analyses (Preview) in logboekanalyse**: [Azure Log Analytics](../log-analytics/log-analytics-azure-sql.md) kunt u voor het verzamelen en visualiseren van belangrijke Azure SQL Azure maatstaven voor prestaties ondersteuning biedt voor maximaal 150.000 SQL-Databases en 5000 SQL elastische Pools per werkruimte. U kunt deze gebruiken om te controleren en meldingen ontvangen. U kunt SQL-Database en de elastische groep metrische gegevens controleren op meerdere Azure-abonnementen en elastische pools en kan worden gebruikt voor het identificeren van problemen bij elke laag van een toepassing-stack.

### <a name="i-am-noticing-performance-issues-how-does-my-sql-database-troubleshooting-methodology-differ-from-sql-server"></a>Ik ben daar iets van merkt prestatieproblemen: hoe mijn SQL-Database voor probleemoplossing methodologie verschilt van SQL Server?
Een belangrijk deel van de technieken voor probleemoplossing die u zou gebruiken voor het oplossen van de query en prestatieproblemen met de database blijft hetzelfde. Nadat de dezelfde SQL Server drijft de engine voor de cloud. De platform - Azure SQL DB heeft echter 'intelligentie' ingebouwd. Dit kunt u prestatieproblemen zelfs eenvoudiger vaststellen en oplossen. Het kan enkele van deze corrigerende maatregelen ook uitvoeren namens jou en in sommige gevallen, deze - automatisch proactief te repareren. 

Een benadering voor het oplossen van prestatieproblemen aanzienlijk kan profiteren met behulp van intelligente functies zoals [Query Performance Insight(QPI)](sql-database-query-performance.md) en [Database Advisor](sql-database-advisor.md) samen en zodat het verschil in methodologie verschilt in dat opzicht – niet langer hoeft te doen is de handmatige werkzaamheden van de essentiële informatie die u kunnen helpen malen los problemen met het probleem op dat moment. Het platform biedt harde werk voor u. Een voorbeeld van die is QPI. Met QPI, kunt u inzoomen helemaal naar beneden op het niveau van de query en historische trends kijken en achterhalen exact de query is opgelost. De Database Advisor biedt u aanbevelingen op zaken waarmee u verbeteren de algehele prestaties in het algemeen zoals - ontbrekende indexen verwijderen van indexen, parameters voorzien van uw query's enz. 

Prestaties op te lossen is het belangrijk om te bepalen of het is alleen de toepassing of het maken van back-database die wordt die invloed hebben op de toepassingsprestaties van uw. Vaak is het prestatieprobleem veroorzaakt in de toepassingslaag. Het is mogelijk de architectuur of de data access-patroon. Neem bijvoorbeeld dat een chatty toepassing die is gevoelig voor netwerklatentie. In dit geval wordt uw toepassing te lijden heeft onder omdat er veel korte aanvragen heen en weer ('chatty') tussen de toepassing en de server en in een overbelaste netwerk, deze interactie optellen fast. In dit geval de prestaties verbeteren, kunt u [Batch query's](sql-database-performance-guidance.md#batch-queries). Met batches kunt u kostte omdat nu uw aanvragen in een batch verwerkt; dus helpt u bij het beperken van de retour-latentie en verbeteren de toepassingsprestaties van uw. 

Bovendien, als u merkt dat een verslechtering van de algehele prestaties van uw database, kunt u bewaken de [sys.dm_db_resource_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) en [sys.resource_stats](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database) dynamische Beheerweergave weergaven om begrijpen CPU, i/o-en geheugen. Prestaties van uw wordt mogelijk beïnvloed omdat de database tekort van resources komt is. Kan het zijn dat u wilt wijzigen van het prestatieniveau en/of servicelaag op basis van de groeiende en werkbelasting eisen verkleinen. 

Zie voor een uitgebreide set met aanbevelingen voor het afstemmen van prestatieproblemen: [optimaliseren van uw database](sql-database-performance-guidance.md#tune-your-database).

### <a name="how-do-i-ensure-i-am-using-the-appropriate-service-tier-and-performance-level"></a>Hoe voorkom ik dat ik gebruik het juiste prijscategorie en prestatieniveau serviceniveau
SQL-Database biedt verschillende servicecategorieën Basic, Standard en Premium. Elke servicelaag krijgt u een gegarandeerde voorspelbare prestaties gekoppeld aan dit serviceniveau. Afhankelijk van uw werkbelasting wellicht bursts van activiteit waar uw Resourcegebruik het maximum van het huidige prestatieniveau die u in mogelijk bereikt. In dergelijke gevallen is dit handig voor het opnieuw opstarten door te evalueren of eventuele afstemmen kunt (bijvoorbeeld toe te voegen of te wijzigen van een index enz.). Als u nog steeds limiet problemen ondervindt, kunt u verplaatsen naar een hoger prestatieniveau of serviceniveau. 

|**Serviceniveau**|**Veelvoorkomende gebruiksscenario 's**|
|---|---|
|**Basic**|Toepassingen met een enkele gebruikers en een database die geen hoge eisen voor gelijktijdigheid van taken, schaal en prestaties. |
|**Standard**|Toepassingen met een aanzienlijke gelijktijdigheid, schaal en prestaties vereisten, samen met een laag tot gemiddeld i/o-vereisten. |
|**Premium**|Toepassingen met een groot aantal gelijktijdige gebruikers, hoog CPU/geheugen en hoge i/o vereist. Hoge gelijktijdigheid, hoge doorvoer en latentie gevoelig apps kunnen gebruikmaken van het niveau van Premium. |
|||

Om te controleren of dat u bent op de juiste prestatieniveau, kunt u uw query's en database resourceverbruik via een van de hierboven vermelde manieren 'Hoe bewaak ik het prestatie- en -gebruik in SQL-Database' controleren. Ziet u dat uw query's / databases consistent worden uitgevoerd op CPU/geheugen enz. u overwegen kunt omhoog schalen naar een hoger prestatieniveau hot. Op dezelfde manier als u Houd er rekening mee dat zelfs tijdens de piekuren die u niet goed lijken te gebruiken van de resources zo veel mogelijk; Overweeg bij het verkleinen van het huidige prestatieniveau. 

Als u een patroon SaaS-app of een database consolidatie scenario hebt, kunt u overwegen een elastische Pool voor kostenoptimalisatie op. Elastische groep is een uitstekende manier om een database consolidatie en kosten-optimalisatie. Voor meer informatie over het beheren van meerdere databases met behulp van de elastische Pool, Zie: [beheren van toepassingen en databases](sql-database-elastic-pool.md#manage-elastic-pools-and-databases-using-the-azure-portal). 

### <a name="how-often-do-i-need-to-run-database-integrity-checks-for-my-database"></a>Hoe vaak moet ik database integriteitscontroles voor mijn database uitvoeren?
SQL-Database maakt gebruik van sommige smart technieken die toe te staan om bepaalde klassen van beschadigde gegevens automatisch en zonder verlies van gegevens te verwerken. Deze technieken zijn ingebouwd in de service en wordt gebruikt door de service wanneer moet zich voordoet. Uw back-ups over de service worden regelmatig een getest door deze terug te zetten en DBCC CHECKDB waarop. Als er problemen zijn, lost SQL-Database proactief ze. [Automatische pagina reparatie](/sql/sql-server/failover-clusters/automatic-page-repair-availability-groups-database-mirroring) wordt gebruikt voor het oplossen van pagina's die zijn beschadigd of hebben problemen met de gegevensintegriteit. De database-pagina's worden altijd geverifieerd met de standaardinstelling van de CONTROLESOM die controleert of de integriteit van de pagina. SQL-Database proactief bewaakt en controleert de integriteit van gegevens van uw database en als er zich problemen voordoen, lost ze met de hoogste prioriteit. Naast deze informatie kunt u desgewenst uw eigen integriteitscontroles op uw wordt uitgevoerd.  Zie voor meer informatie [integriteit van gegevens in SQL-Database](https://azure.microsoft.com/blog/data-integrity-in-azure-sql-database/)

## <a name="data-movement-after-migration"></a>Verplaatsing van gegevens na de migratie

### <a name="how-do-i-export-and-import-data-as-bacpac-files-from-sql-database"></a>Hoe ik exporteren en importeren van gegevens als Bacpac-bestanden uit SQL-Database?

- **Exporteren**: U kunt uw Azure SQL database exporteren als een Bacpac-bestand van de Azure-portal

   ![Database wordt geëxporteerd](./media/sql-database-export/database-export.png)

- **Importeren**: U kunt ook gegevens als een Bacpac-bestand importeren in de database met de Azure portal.

   ![De database wordt geïmporteerd](./media/sql-database-import/import.png)

### <a name="how-do-i-synchronize-data-between-sql-database-and-sql-server"></a>Hoe ik gegevens tussen de SQL-Database en SQL Server synchroniseren?
Er zijn verschillende manieren om dit te bereiken: 
- **[Synchroniseren van gegevens](sql-database-sync-data.md)**  – met deze functie kunt u gegevens in twee richtingen tussen meerdere lokale SQL Server-databases en SQL-Database te synchroniseren. Als u wilt synchroniseren met on-premises SQL Server-databases, die u wilt installeren en sync-agent configureren op een lokale computer en de uitgaande TCP-poort 1433 openen.
- **[Transactiereplicatie](https://azure.microsoft.com/en-us/blog/transactional-replication-to-azure-sql-database-is-now-generally-available/)**  – met transactiereplicatie kunt u uw gegevens van on-premises naar Azure SQL DB synchroniseren met de on-premises wordt van de uitgever en de Azure SQL DB, wordt de abonnee. Alleen voor deze installatie wordt nu ondersteund. Zie voor meer informatie over het migreren van uw gegevens van on-premises naar Azure SQL met minimale downtime: [transactiereplicatie gebruik](sql-database-cloud-migrate.md#method-2-use-transactional-replication)

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [SQL-Database](sql-database-technical-overview.md).

