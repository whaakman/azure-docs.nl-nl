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
ms.date: 10/14/2016
ms.author: Joe.Sack
ms.suite: sql
ms.prod_service: sql-database
ms.component: migration
ms.openlocfilehash: e562c33cabc7d39d1f6a911c21343f85da205c0b
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="how-should-i-manage-my-azure-sql-database-after-migration"></a>Hoe beheer ik mijn Azure SQL-database moet na de migratie?

*Veelgestelde vragen over het beheren van uw Azure SQL Database-investeringen*

Zodat u onlangs SQL Server-databases hebt verplaatst naar Azure SQL Database, of misschien u van plan bent voor het verplaatsen van zo snel mogelijk. Nadat u hebt verplaatst, hoe nu verder? Gezien het feit dat SQL-Database is een *Platform als een Service*, Microsoft omgaat met de verschillende gebieden namens jou. Maar hoe wijzigt dit procedures om belangrijke gebieden zoals beveiliging, bedrijfscontinuïteit, Databaseonderhoud, prestaties afstemmen, bewaking en meer van uw bedrijf? 

Het doel van dit artikel is te organiseren bondig resources en richtlijnen die u nodig hebt voor het maken van een verschuiving voor het beheren van uw investeringen in SQL-Database. De belangrijke gebieden in dit artikel uitgelegd bedrijfscontinuïteit, beveiliging, Databaseonderhoud en bewaking, prestaties en gegevensverplaatsing. Aan bod belangrijke gebieden die zijn de verschillen tussen de SQL Server en SQL-Database en roepen operationele procedures waarmee u de voordelen maximaliseren en het risico minimaliseren. 

## <a name="manage-business-continuity-after-migration"></a>Beheren van zakelijke continuïteit na migratie

### <a name="how-do-i-create-and-manage-backups-on-sql-database"></a>Hoe ik maken en beheren van back-ups op SQL-Database? 
SQL-Database wordt automatisch voor u back-ups van databases en biedt u de mogelijkheid om terug te zetten naar een willekeurig punt in tijd in de bewaarperiode. De bewaarperiode is 35 dagen voor Standard en Premium-databases en 7 dagen voor Basic-databases. Bovendien kunt de functie Long-Term bewaren u back-upbestanden houdt voor een langere periode (maximaal 10 jaar) en herstel van deze back-ups op elk gewenst moment. Bovendien zijn de databaseback-ups geogerepliceerde om te controleren of de mogelijkheid om te geo-herstel in elke regio in het geval van een ramp of regionale ramp. Zie [Business continuity overview](sql-database-business-continuity.md).

### <a name="how-do-i-ensure-business-continuity-in-the-event-of-a-datacenter-level-disaster-or-a-regional-catastrophe"></a>Hoe kan ik ervoor zorgen dat zakelijke continuïteit in geval van een noodgeval datacenter-niveau of een regionale ramp? 

Databaseback-ups zijn om te controleren of de mogelijkheid om te geo-herstel in elke regio in het geval van een ramp of regionale ramp geogerepliceerde. Zie [Business continuity overview](sql-database-business-continuity.md). SQL-Database biedt bovendien de mogelijkheid voor het onderhouden van actieve geo-replicatie secundaire databases in een andere regio. Ze configureren in de groep van een automatisch Failover zorgt ervoor dat de databases automatisch een failover naar de secundaire in een scenario noodgevallen. Als een automatisch failover-groep niet is geconfigureerd, klikt u vervolgens moet uw toepassing actief voor een noodgeval bewaken en start een failover naar de secundaire. 
### <a name="sql-server-provided-me-readable-secondary-replicas-can-i-access-the-secondaries-on-sql-database"></a>SQL Server opgegeven mij leesbare secundaire replica's, ik toegang tot de secundaire replica's op de SQL-Database? 

Ja, de functie "Actieve Geo-replicatie" gebruikt voor het maken van leesbare secundaire replica's. 

### <a name="how-does-my-disaster-recovery-plan-change-from-on-premise-to-sql-database"></a>Hoe wordt mijn noodherstelplan gewijzigd van on-premises SQL-database? 
Implementaties van SQL Server vereist dat u voor het beheren van back-ups met behulp van functies zoals Failover Clustering, databasespiegeling, replicatie, logboekverzending of gewoon vanilla Bacpac-back-ups actief. Echter op SQL-Database, back-ups volledig worden beheerd door Microsoft en kunt u alleen back-up en noodherstel herstelplannen geconfigureerd en werken met een paar klikken op de Azure-portal (of enkele opdrachten op PowerShell) hebben. ‌
### <a name="in-the-event-of-disaster-how-do-i-recover-my-databases"></a>In geval van een ramp, hoe herstel ik mijn databases? 
SQL-Database kunt automatisch u de databases herstellen naar een willekeurig punt in tijd in de afgelopen 35 dagen. Dit is een optie als u gegevens verloren gaan of een toepassingsgerelateerde noodherstel te maken. 

Als u een regionaal noodgeval geconfronteerd als de geo-replicatie secundaire databases zijn geconfigureerd, kunt u uw geo-secundaire databases in een andere regio herstellen. Voor realtime toegang tot uw toepassingen u een failover naar de geo-secundaire in andere regio handmatig. U kunt ook als u automatische failover groep geconfigureerd hebt, wordt deze failover naar een secundaire geo-database automatisch in een scenario noodgevallen. Als u geen geo-replicatie secundaire database geconfigureerd hebt, kunt u nog steeds uw databases herstellen via automatische gerepliceerde back-upbestanden (ingebouwde functionaliteit configuratie is niet nodig), met relatief langer hersteltijd (RTO met 12 uur) en maximaal één uur van gegevensverlies. 

### <a name="are-the-failovers-to-secondary-transparent-how-does-my-application-handle-database-failovers"></a>De failover naar de secundaire transparant zijn? Hoe wordt mijn toepassing database failovers verwerkt? 
Als u automatische failover-groepen die zijn geconfigureerd, zijn de failover naar de secundaire is transparant. Als u niet hebt, klikt u vervolgens moet uw toepassing echter de logica voor het controleren van de beschikbaarheid van de primaire en vervolgens handmatig via failover naar de secundaire opnemen. 
 
## <a name="manage-security-after-migration"></a>Beveiliging beheren na migratie

### <a name="how-can-i-restrict-access-to-my-sql-database"></a>Hoe kan ik toegang tot mijn SQL-Database beperken? 
 
Er zijn een aantal manieren vergrendelen connectiviteit toegang tot de SQL-databases. 
1. Limiet-verkeer via de Internet-Express Route geeft u specifiek fiber met de Azure-netwerk, zodat uw gegevens worden niet via internet worden verzonden. Het is ook mogelijk om te configureren tussen regio connectiviteit met behulp van Express route. De volgende koppelingen beschrijven Express Route in meer detail: 
 - [Inleiding op Express Route](../expressroute/expressroute-introduction.md)
 - [Vereisten](../expressroute/expressroute-prerequisites.md) 
 - [Werkstromen](../expressroute/expressroute-workflows.md) 
 
2. Selecteer welke resources verbinden met SQL Database: 

   De SQL-database is standaard geconfigureerd voor 'Alle Azure-services toestaan' – wat betekent dat alle virtuele machines in Azure kan proberen verbinding maken met de database.  Verificatie van alle aanmeldingen moet nog steeds plaatsvinden. Als u niet de database dat wilt alleen toegankelijk voor alle IP-adressen van Azure, kunt u 'Alle Azure-services toestaan' uitschakelt en gebruiken [VNET Service-eindpunten](sql-database-vnet-service-endpoint-rule-overview.md) binnenkomende toegang beperken tot de database van alleen Azure-resources die zich binnen een gegeven Azure VNET subnet. 

   ![VNET-service-eindpunten](./media/sql-database-manage-after-migration/vnet-service-endpoints.png) 

   Een alternatief is om in te richten [gereserveerd IP-adressen](../virtual-network/virtual-networks-reserved-public-ip.md) voor uw virtuele machines en de goedgekeurde IP-adressen die specifieke VM IP-in de server adressen firewall-instellingen. (Zie de schermafbeelding hieronder als voorbeeld in de Azure portal.) Gereserveerd IP-adressen toe te wijzen, bespaart u het niet meer hoeft bij het bijwerken van de firewall-regels met het wijzigen van IP-adressen. 

3. Poort 1433 buiten Azure blootstellen voorkomen

   SSMS uitgevoerd met de Azure [Azure RemoteApp](https://www.microsoft.com/cloud-platform/azure-remoteapp-client-apps). Dit vereist niet dat u uitgaande verbindingen om poort 1433 openen, het IP-adres is statisch, zodat de database kan worden geopend zodat u alleen de RemoteApp, het ondersteunt Multi Factor Authentication (MFA) en meerdere gebruikers. 

### <a name="what-authentication-methods-are-offered-in-sql-database"></a>Welke verificatiemethoden zijn beschikbaar in SQL-Database?

De belangrijkste verificatiemethoden die worden aangeboden in SQL-Database en SQL Data Warehouse zijn Azure Active Directory-verificatie en SQL-verificatie. Azure Active Directory (AD) is een gecentraliseerde service voor identiteits- en toegangsbeheer en SQL is slechts een van de vele Azure-services die zijn geïntegreerd met Azure AD. Het voordeel van een centraal beheerde service is dat de referenties van een gebruiker wordt gedeeld door alle Azure-services u gebruiken voor verificatie eenvoudiger. Hierdoor kunnen ook SQL-Database en SQL Data Warehouse te bieden multi-factor authentication en Gast gebruikersaccounts in Azure AD-domein. 

Als u een Active Directory on-premises al hebt, kunt u de map met Azure Active Directory om uw directory naar Azure uitbreiden federeren. 


|||
|---|---|
| Als je…|Azure SQL Database / Azure SQL Data Warehouse|
| De voorkeur geeft niet aan Azure Active Directory (AD) in Azure|Gebruik [SQL-verificatie](sql-database-security-overview.md)|
| Gebruikte AD op SQL Server on-premises|[AD met Azure AD federeren](../active-directory/connect/active-directory-aadconnect.md), en Azure AD-verificatie te gebruiken. Hiermee kunt u eenmalige aanmelding.|
| Moet multi-factor authentication (MFA) af te dwingen|MFA is vereist als een beleid via [voorwaardelijke toegang van Microsoft](sql-database-conditional-access.md), en gebruik [Universal van Azure AD authentication met ondersteuning voor MFA](sql-database-ssms-mfa-authentication.md).|
| Gastaccounts van Microsoft-accounts (live.com, outlook.com) of andere domeinen (gmail.com) hebben|Gebruik [Azure AD Universal verificatie](sql-database-ssms-mfa-authentication.md) in SQL Database/Data Warehouse, die gebruikmaakt van [Azure AD B2B-samenwerking](../active-directory/active-directory-b2b-what-is-azure-ad-b2b.md).|
| Zijn Windows met behulp van uw Azure AD-referenties van een federatieve domein aangemeld|Gebruik [Azure AD-geïntegreerde verificatie](sql-database-aad-authentication-configure.md).|
| Zijn aangemeld bij Windows met referenties van een domein niet gefedereerd met Azure|Gebruik [Azure AD-wachtwoordverificatie](sql-database-aad-authentication-configure.md).|
| Middelste laag-services die verbinding moeten maken met Azure SQL Database- of datawarehouse hebben|Gebruik [Azure AD-tokenverificatie](sql-database-aad-authentication-configure.md).
|||

### <a name="how-can-i-limit-access-to-sensitive-data-in-my-databases-from-the-application-side"></a>Hoe beperk ik toegang tot gevoelige gegevens in mijn databases van de kant van de toepassing? 

Om te voorkomen dat onbevoegde gebruikers kunnen gevoelige gegevens weergeven, zijn een aantal opties beschikbaar zijn in SQL-Database: 

- [Altijd versleuteld](/sql/relational-databases/security/encryption/always-encrypted-database-engine) is een vorm van versleuteling voor clientzijde waarmee gevoelige kolommen in de database versleuteld (zodat ze in gecodeerde tekst voor databasebeheerders en niet-geautoriseerde gebruikers). De sleutel voor altijd versleuteld wordt opgeslagen op de client, zodat alleen geautoriseerde clients om de gevoelige kolommen te ontsleutelen. Versleutelde ondersteunt altijd gelijkheid vergelijkingen vandaag, dus DBA's kunnen blijven versleutelde kolommen niet opvragen als onderdeel van de SQL-opdrachten. Altijd versleutelde kan worden gebruikt met een groot aantal sleutelarchief opties, zoals [Azure Key Vault](sql-database-always-encrypted-azure-key-vault.md), Windows-certificaatarchief en lokale hardware security modules.
- [Dynamische-Gegevensmaskering](sql-database-dynamic-data-masking-get-started.md) is een functie voor het maskeren van gegevens die de blootstelling gevoelige gegevens beperkt door het maskeren voor onbevoegde gebruikers op de toepassingslaag. U definieert een maskeringsregel dat maskering patroon kunt maken (bijvoorbeeld moet worden alleen weergegeven laatste 4 cijfers van een sofi-nummer en de rest als markeren x) en identificeren welke gebruikers kunnen worden uitgesloten van de maskeringsregel.
- [Beveiliging](/sql/relational-databases/security/row-level-security) kunt u de controle op rijen in een databasetabel toegang op basis van de gebruiker (groep lidmaatschap of -uitvoering context) van de query uit te voeren. De beperking van de toegang is uitgevoerd op de databaselaag in plaats van in een toepassingslaag voor het vereenvoudigen van de logische app. 

### <a name="what-encryption-options-do-i-have-in-sql-database-and-what-actors-does-the-encryption-protect-from"></a>Welke versleutelingsopties heb ik in SQL-Database en welke actoren de versleuteling te beschermen tegen?
Er zijn drie belangrijkste versleutelingstechnologieën die beschikbaar in SQL-Database zijn: 
- [Altijd versleuteld](/sql/relational-databases/security/encryption/always-encrypted-database-engine) (die wordt vermeld in de bovenstaande vraag): gevoelige kolommen in de tabel end-to-end van niet-geautoriseerde clients naar de fysieke schijf worden versleuteld. De server en Gegevensbeheerders kunnen niet zien de gevoelige gegevens, omdat de versleutelingssleutels worden opgeslagen op de client. 
- [Met transparante gegevensversleuteling](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) (TDE): versleuteling in rust die versleutelt op het databaseniveau van de en de gegevensbestanden, logboekbestanden en gekoppelde back-ups worden beschermd tegen diefstal van de fysieke media. TDE is standaard ingeschakeld voor alle nieuwe databases.
 
  Het volgende diagram toont een overzicht van de versleuteling technologie keuzes.

   ![Overzicht van versleuteling](./media/sql-database-manage-after-migration/overview-encryption.png)

### <a name="how-should-i-manage-encryption-keys-in-the-cloud"></a>Hoe moet ik de versleutelingssleutels in de cloud beheren? 
Er zijn opties voor sleutelbeheer voor altijd versleuteld (versleuteling aan clientzijde) en Transparent Data Encryption (versleuteling-at-rest). Het is raadzaam regelmatig versleutelingssleutels roteren en met een frequentie die wordt uitgelijnd met zowel interne voorschriften en naleving vereisten.

- **Altijd versleuteld**: Er is een [twee toetsen hiërarchie](/sql/relational-databases/security/encryption/overview-of-key-management-for-always-encrypted) in altijd versleuteld - een kolom met gevoelige gegevens gecodeerd met een AES 256 kolomversleutelingssleutel (CEK) die op zijn beurt is versleuteld met een hoofdsleutel voor kolom (CMK). De clientstuurprogramma's voor altijd versleuteld hebben geen beperkingen op de lengte van CMKs.

  De versleutelde waarde van de CEK is opgeslagen op de database en de CMK is opgeslagen in een vertrouwde sleutelarchief, zoals Windows-certificaatarchief, Azure Key Vault of een hardware security module. 
  
  Zowel de [CEK en CMK](/sql/relational-databases/security/encryption/rotate-always-encrypted-keys-using-powershell) kunnen worden gedraaid. CEK rotatie mag tijdrovende afhankelijk van de grootte van de tabellen met de versleutelde kolommen. Plan daarom zeer zorgvuldig CEK wisselen van tapes. CMK worden gedraaid aan de andere kant heeft geen invloed op prestaties van de database, en kunt u doen met gescheiden rollen.

  Het volgende diagram toont de sleutelarchief opties voor de kolom hoofdsleutels in altijd versleuteld. 

   ![Altijd versleuteld CMK providers opslaan](./media/sql-database-manage-after-migration/always-encrypted.png)

- **Transparent Data Encryption (TDE)**: Er is een hiërarchie met twee-sleutel in TDE: de gegevens in de gebruikersdatabase van elke is versleuteld met een symmetrische AES-256 database unieke databaseversleutelingssleutel (DEK) die op zijn beurt is versleuteld met een server unieke asymmetrische RSA 2048 hoofdsleutel. 

  Standaard wordt de hoofdsleutel voor Transparent Data Encryption beheerd door de service SQL Database voor het gemak. Als uw organisatie besturen de hoofdsleutel wilt, er is een optie te gebruiken [Azure Key Vault](/sql/relational-databases/security/encryption/transparent-data-encryption-byok-azure-sql) als de sleutel store. 

  Met behulp van Azure Sleutelkluis, uw organisatie wordt ervan uitgegaan dat controle over de belangrijkste inrichten, draaiing en machtiging besturingselementen. [Rotatie of overschakelen van het type van een hoofdsleutel TDE](/sql/relational-databases/security/encryption/transparent-data-encryption-byok-azure-sql-key-rotation) is snel, omdat deze de DEK alleen opnieuw versleutelt. 

  Voor organisaties met scheiding van functies tussen beveiliging en beheer, een beveiliging beheerder kan het sleutelmateriaal voor de hoofdsleutel TDE in Azure Key Vault inrichten, en een sleutel-id van Azure Sleutelkluis bieden aan de beheerder van de database moet worden gebruikt voor codering in rust op een server. 

## <a name="monitoring-and-compliance-after-migration"></a>Controle en naleving na migratie

### <a name="how-do-i-monitor-database-activities-in-sql-database"></a>Hoe bewaak ik activiteiten met een database in SQL-Database
Er zijn slechts enkele bewakingsmogelijkheden ingebouwd in SQL-Database, die bijhouden beveiligings- en andere gebeurtenissen op de database:
- [Controle van SQL](sql-database-auditing.md) kunt u voor het verzamelen van controlelogboeken van databasegebeurtenissen in uw eigen Azure Storage-account.
- [Detectie van dreigingen SQL](sql-database-threat-detection.md) kunt u voor het detecteren van verdachte activiteiten die wijzen op een mogelijke kwade bedoelingen te openen, inbreuk of misbruik van gegevens in de database. Detectie van dreigingen voor SQL-Database wordt uitgevoerd meerdere sets van algoritmen die mogelijke beveiligingsproblemen en SQL-injectieaanvallen, evenals de toegangspatronen afwijkende database (zoals de toegang van een ongebruikelijke locatie of door een onbekende-principal) te detecteren. -Afdelingen of andere aangewezen beheerders ontvangen een e-mailmeldingen als een bedreiging wordt gedetecteerd op de database. Elke melding vindt details van de verdachte activiteit en aanbevelingen voor het verder onderzoek en risico. 
- [Controle op beveiligingslekken SQL](sql-vulnerability-assessment.md) is een database scannen en reporting service waarmee u de beveiligingsstatus van uw databases op grote schaal, bewaken en beveiligingsrisico's identificeren en het nemen van de door u gedefinieerde basisbeveiliging. Na elke scan een aangepaste lijst met bruikbare stappen en herstel scripts vindt, evenals een evaluatie-rapport dat kan worden gebruikt voor het helpen om te voldoen aan nalevingsvereisten. 
- [SQL-OMS Sync beveiligingstoepassing](https://github.com/Microsoft/Azure-SQL-DB-auditing-OMS-integration) maakt gebruik van Operations Management Suite (OMS) openbare API's voor het pushen van controlelogboeken voor SQL in naar OMS voor logboekanalyse en de mogelijkheid voor het definiëren van aangepaste detectiewaarschuwingen, met inbegrip van: 
 - SQL Database Audit tegel & dashboard waarmee duidelijke en samenhangende rapport van de databaseactiviteiten van uw. 
 - SQL-logboekanalyse voor het analyseren van de activiteit van uw database en onderzoek discrepanties en afwijkingen die vermoedelijke beveiligingsschendingen duiden kunnen.
 - Geavanceerde specifieke regels waarschuwingen over waargenomen-gebeurtenissen die e-mail activeren, waarschuwingen Webhook en Azure automation-runbook (dat wil zeggen wachtwoord wijzigen buiten kantooruren, specifieke SQL-opdrachten).
- [Azure Security Center](../security-center/security-center-intro.md) biedt gecentraliseerd beheer over de werkbelastingen die worden uitgevoerd in Azure, on-premises en in andere clouds. U kunt weergeven of essentieel SQL Database-beveiliging, zoals controle en Transparent Data Encryption zijn geconfigureerd op alle bronnen, en beleid op basis van uw eigen vereisten maken. 

### <a name="is-sql-database-compliant-with-any-regulatory-requirements-and-how-does-that-help-with-my-own-organizations-compliance"></a>SQL-Database voldoen aan eventuele voorschriften is en hoe helpt die bij de naleving van mijn eigen organisatie? 
Azure SQL-Database is compatibel met een bereik van de regelgeving conformiteit. Als u wilt weergeven van de meest recente set conformiteit die wordt voldaan, gaat u naar de [Microsoft Trust Center](https://www.microsoft.com/trustcenter/compliance/complianceofferings) en inzoomen op de conformiteit die belangrijk voor uw organisatie om te zien zijn als Azure SQL Database is opgenomen onder de compatibele Azure Services. Het is belangrijk te weten dat hoewel Azure SQL Database, kan worden goedgekeurd als een compatibele service, het helpt de compatibiliteit van uw organisatie service, maar kan deze niet automatisch garanderen. 

## <a name="database-maintenance-and-monitoring-after-migration"></a>Databaseonderhoud en regelmatige controle na de migratie

### <a name="how-do-i-monitor-growth-in-data-size-and-resource-utilization"></a>Hoe bewaak ik groei in grootte en de resource-gebruik van gegevens?

- U kunt monitor metrische gegevens weergeven over uw gebruik van database grootte en de resource op de grafiek 'Monitoring' in de Azure-portal. 

  ![Bewaking van de grafiek](./media/sql-database-manage-after-migration/monitoring-chart.png)

- Om meer inzicht en inzoomen op de details van query's, kunt u 'Query Performance Insight' beschikbaar in de Azure portal. Dit vereist dat 'Query Store' actief voor uw database is.

  ![Inzicht in queryprestaties](./media/sql-database-manage-after-migration/query-performance-insight.png)

- U kunt ook de metrische gegevens te met behulp van dynamische beheerweergaven (DMV's): met behulp van weergeven [sys.dm_db_resource_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) en [sys.resource_stats](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database). 

### <a name="how-often-do-i-need-to-run-consistency-checks-like-dbcccheckdb"></a>Hoe vaak moet ik Voer consistentiecontroles uit zoals DBCC_CHECKDB?
DBCC_CHECKDB controleert de integriteit van de logische en fysieke van alle objecten in de database. U hoeft niet meer te doen deze controles omdat deze worden beheerd door Microsoft in Azure. Zie voor meer informatie [integriteit van gegevens in Azure SQL Database](https://azure.microsoft.com/blog/data-integrity-in-azure-sql-database/)

## <a name="monitor-performance-and-resource-utilization-after-migration"></a>Prestatie- en gebruik na de migratie controleren

### <a name="how-do-i-monitor-performance-and-resource-utilization-in-azure-sql-database"></a>Hoe bewaak ik prestaties en gebruik van bronnen in Azure SQL Database
U kunt de prestatie- en -gebruik in Azure SQL Database met de volgende manieren controleren:

- **Azure-portal**: ziet u een individuele database gebruik door de database selecteren en te klikken op de grafiek in het overzichtsvenster van de Azure portal. U kunt de grafiek om weer te geven van meerdere metrische gegevens, met inbegrip van CPU-percentage, DTU-percentage, gegevens-IO-percentage, sessies percentage en Database als percentage van asgrootte wijzigen. 
  ![Resourcegebruik](./media/sql-database-manage-after-migration/resource-utilization.png)

  U kunt ook waarschuwingen door de bron van deze grafiek configureren. Deze waarschuwingen kunnen reageren op de voorwaarden van de resource met een e-mailbericht, schrijven naar een eindpunt HTTPS/HTTP- of een actie uitvoert. Zie de [databaseprestaties bewaken in Azure SQL Database](sql-database-single-database-monitor.md) voor gedetailleerde instructies.

- **Weergaven**: U kunt een query de [sys.dm_db_resource_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) dynamische Beheerweergave resource verbruik statistieken geschiedenis van het afgelopen uur retourneren en de [sys.resource_stats](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database) catalogus systeemweergave geschiedenis retourneren voor de afgelopen 14 dagen. 

- **Query Performance Insight**: [Query Performance Insight](sql-database-query-performance.md) kunt u een historisch overzicht van de top-resource verbruikende query's en langlopende query's voor een specifieke database. Dit onderdeel vereist [Query Store](/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store) worden ingeschakeld en actief is voor de database.

- **Azure SQL-analyses (Preview) in logboekanalyse**: [Azure Log Analytics](../log-analytics/log-analytics-azure-sql.md) kunt u voor het verzamelen en visualiseren van belangrijke Azure SQL Azure maatstaven voor prestaties ondersteuning biedt voor maximaal 150.000 Azure SQL-Databases en 5000 SQL elastische Pools per werkruimte. U kunt deze gebruiken om te controleren en meldingen ontvangen. U kunt Azure SQL Database en de elastische groep metrische gegevens controleren op meerdere Azure-abonnementen en elastische pools en kan worden gebruikt voor het identificeren van problemen bij elke laag van een toepassing-stack. 

### <a name="how-do-i-ensure-i-am-using-the-appropriate-service-tier-and-performance-level"></a>Hoe voorkom ik dat ik gebruik het juiste prijscategorie en prestatieniveau serviceniveau
Monitor voor de [sys.dm_db_resource_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) en [sys.resource_stats](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database) dynamische beheerweergaven om te begrijpen CPU, i/o-en geheugen. U kunt ook de SQL-Database [Query Performance Insight](sql-database-query-performance.md) brongebruik te zien. Als u consistent op een hoog percentage van de beschikbare bronnen worden uitgevoerd, moet u overwegen verplaatsen naar een hoger prestatieniveau binnen de bestaande servicelaag of verplaatsen naar een hogere servicelaag. Als u daarentegen als u een laag percentage van de beschikbare resources consistent worden gebruikt, kunt u overwegen verplaatsen naar een lager prestatieniveau of servicelaag.

### <a name="i-am-seeing-performance-issues-how-does-my-azure-sql-database-troubleshooting-methodology-differ-from-sql-server"></a>Ik ben prestatieproblemen te zien. Hoe verschilt mijn Azure SQL Database voor probleemoplossing methodologie van SQL Server?
Veel aspecten van de prestaties voor probleemoplossing methodologie blijft hetzelfde als in Azure SQL Database, maar er zijn enkele verschillen. Bijvoorbeeld, als er een verslechtering van de algehele prestaties, controleren de [sys.dm_db_resource_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) en [sys.resource_stats](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database) dynamische beheerweergaven om te begrijpen CPU, i/o- en geheugen verbruik. Mogelijk moet u het prestatieniveau wijzigen en/of servicelaag op basis van de eisen van de werkbelasting.
Zie voor een uitgebreide set met aanbevelingen voor het afstemmen van prestatieproblemen [prestaties in Azure SQL Database afstemmen](sql-database-performance-guidance.md). 

### <a name="do-i-need-to-maintain-indexes-and-statistics"></a>Heb ik nodig voor het onderhouden van indexen en statistieken?
Azure SQL Database blijft niet behouden indexen en statistieken automatisch als onderdeel van de service. U bent zelf verantwoordelijk voor het plannen van het onderhoud van indexen en statistieken. Het volgende artikel, Azure Automation-methoden, details over verschillende opties voor het plannen van onderhoudstaken op Azure SQL-Database.

## <a name="data-movement-after-migration"></a>Verplaatsing van gegevens na de migratie

### <a name="how-do-i-export-and-import-data-as-bacpac-files-from-azure-sql-database"></a>Hoe ik exporteren en importeren van gegevens als Bacpac-bestanden van Azure SQL Database? 

- **Exporteren**: U kunt uw Azure SQL Database exporteren als een Bacpac-bestand van de Azure-portal.

  ![Als een BACPAC exporteren](./media/sql-database-export/database-export.png)

- **Importeren**: U kunt importeren als Bacpac-bestand met een database met de Azure portal.

  ![Een BACPAC importeren](./media/sql-database-import/import.png)

### <a name="how-do-i-synchronize-data-between-azure-sql-database-sql-server-2016--2012"></a>Hoe synchroniseren gegevens tussen Azure SQL Database SQL Server 2016 / 2012?
De [gegevenssynchronisatie](sql-database-sync-data.md) functie kunt u gegevens in twee richtingen tussen meerdere lokale SQL Server-databases en Azure SQL Database te synchroniseren. Echter, aangezien dit trigger basis, uiteindelijke consistentie wordt gegarandeerd (zonder verlies van gegevens), maar consistentie van de transactie kan niet worden gegarandeerd. 

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [Azure SQL Database](sql-database-technical-overview.md).
