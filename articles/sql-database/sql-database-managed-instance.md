---
title: Azure SQL-Database beheerd exemplaar overzicht | Microsoft Docs
description: In dit onderwerp worden beschreven van een Azure SQL Database beheerd-instantie en wordt uitgelegd hoe het werkt en wat is het verschil tussen een individuele database in Azure SQL Database.
services: sql-database
author: bonova
ms.reviewer: carlrab
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 04/10/2018
ms.author: bonova
ms.openlocfilehash: f07ce542c176f4038378d54497d7114109ac5bd3
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36215521"
---
# <a name="what-is-a-managed-instance-preview"></a>Wat is een exemplaar beheerd (preview)?

Azure SQL Database beheerd-instantie (preview) is een nieuwe functie van Azure SQL Database, die in de buurt van 100% compatibel met SQL Server on-premises (Enterprise Edition) bieden een systeemeigen [virtueel netwerk (VNet)](../virtual-network/virtual-networks-overview.md) implementatie die zijn gericht op algemene beveiligingskwesties weergegeven, en een [bedrijfsmodel](https://azure.microsoft.com/pricing/details/sql-database/) gunstig voor lokale SQL Server-klanten. Beheerde exemplaar kan bestaande SQL Server-klanten en hun on-premises toepassingen naar de cloud met minimale wijzigingen voor toepassings- en verschuiven. Beheerde exemplaar van bewaart op hetzelfde moment alle PaaS mogelijkheden (automatische patchen en versie-updates, back-up, hoge beschikbaarheid), die aanzienlijk minder beheeroverhead en totale Eigendomskosten.

> [!IMPORTANT]
> Zie [Uw databases migreren naar een volledig beheerde service met Azure SQL Database Managed Instance](https://azure.microsoft.com/blog/migrate-your-databases-to-a-fully-managed-service-with-azure-sql-database-managed-instance/) voor een lijst met regio's waarin MI momenteel beschikbaar is.
 
Het volgende diagram geeft een overzicht van de belangrijkste functies van het exemplaar worden beheerd.

![belangrijke functies](./media/sql-database-managed-instance/key-features.png)

Beheerde exemplaar wordt overwogen als voorkeur platform voor de volgende scenario's: 

- SQL Server on-premises, / IaaS-klanten die willen migreren van hun toepassingen naar een volledig beheerde service met minimale wijzigingen ontwerpen.
- ISV's vertrouwen op de SQL-databases die willen kunnen hun klanten migreren naar de cloud en dus bereiken aanzienlijke concurrentie of globale markt bereiken. 

Door de algemene beschikbaarheid ernaar beheerd exemplaar leveren bijna serveroppervlaktegebied van 100% compatibel is met de meest recente on-premises SQL Server-versie via een gefaseerde release-abonnement. 

De volgende overzichten van de tabel van belangrijke verschillen en ontwikkelaars van gebruiksscenario's tussen SQL IaaS, Azure SQL Database en SQL Database beheerd-instantie:

| | Gebruiksscenario | 
| --- | --- | 
|SQL Database Managed Instance |Voor klanten die willen migreren van een groot aantal apps vanaf on-premises of IaaS, zelf is gebouwd, of ISV hebt opgegeven, met voorstellen als lage migratie inspanning mogelijk exemplaar beheerd. Met behulp van de volledig geautomatiseerde [gegevens migratie Service (DMS)](../dms/tutorial-sql-server-to-managed-instance.md#create-an-azure-database-migration-service-instance) in Azure, klanten kunnen lift- en hun lokale SQL Server verplaatsen naar een beheerde-exemplaar dat compatibiliteit met SQL Server on-premises en volledige isolatie van biedt de exemplaren van de klant met systeemeigen ondersteuning voor VNET.  Met Software Assurance, kun je de bestaande licenties voor kortingen op een beheerde exemplaar van SQL Database met de [Azure hybride gebruik voordeel voor SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md).  Beheerde exemplaar van SQL Database is de beste Migratiebestemming in de cloud voor SQL Server-exemplaren waarvoor hoge beveiliging en een uitgebreide programmeren voor aanvallen. |
|Azure SQL Database (enkele of groep) |**Elastische pools**: voor klanten ontwikkelen van nieuwe multitenant SaaS-toepassingen of opzettelijk transformeert hun bestaande on-premises apps in een multitenant SaaS-app, voorstellen elastische pools. Voordelen van dit model zijn: <br><ul><li>Conversie van het bedrijfsmodel te verkopen, licenties te verkopen abonnementen (voor ISV's)</li></ul><ul><li>Isolatie van tenants eenvoudig en opsommingsteken bewijs</li></ul><ul><li>Een vereenvoudigde database gericht programmeermodel</li></ul><ul><li>De mogelijkheid uit te schalen zonder een vaste maximum op</li></ul>**Eén databases**: voor het ontwikkelen van nieuwe apps dan de multitenant SaaS, waarvan de werklast stabiel en voorspelbaar is, klanten voorstellen individuele databases. Voordelen van dit model zijn:<ul><li>Een vereenvoudigde database gericht programmeermodel</li></ul>  <ul><li>Voorspelbare prestaties voor elke database</li></ul>|
|SQL-IaaS virtuele machine|Voor klanten die behoefte hebben aan voor het aanpassen van het besturingssysteem of de database-server, evenals klanten die de specifieke vereisten in termen van apps van derden door kant met SQL Server uitgevoerd (op dezelfde virtuele machine), SQL-VM's voorstellen / IaaS als de optimale oplossing|
|||

## <a name="how-to-programmatically-identify-a-managed-instance"></a>Een instantie worden beheerd via een programma te identificeren

De volgende tabel ziet u enkele eigenschappen, toegankelijk via de Transact-SQL gebruiken voor het detecteren van uw toepassing werkt met exemplaar beheerd en belangrijke eigenschappen ophalen.

|Eigenschap|Waarde|Opmerking|
|---|---|---|
|`@@VERSION`|Microsoft SQL Azure (RTM) - 12.0.2000.8 2018-03-07 Copyright (C) Microsoft Corporation die 2018.|Deze waarde is gelijk aan die in SQL-Database.|
|`SERVERPROPERTY ('Edition')`|SQL Azure|Deze waarde is gelijk aan die in SQL-Database.|
|`SERVERPROPERTY('EngineEdition')`|8|Deze waarde is uniek exemplaar beheerd.|
|`@@SERVERNAME`, `SERVERPROPERTY ('ServerName')`|Exemplaar van de volledige DNS-naam in de volgende indeling:<instanceName>.<dnsPrefix>. database.Windows.NET, waarbij <instanceName> is die is geleverd door de klant, terwijl <dnsPrefix> automatisch gegenereerde deel uitmaakt van de naam van de globale DNS-naam uniekheid garanderen ('wcus17662feb9ce98', bijvoorbeeld)|Voorbeeld: Mijn-managed-instance.wcus17662feb9ce98.database.windows.net|

## <a name="key-features-and-capabilities-of-a-managed-instance"></a>Belangrijkste functies en mogelijkheden van een beheerd-exemplaar 

> [!IMPORTANT]
> Een beheerde-exemplaar wordt uitgevoerd met alle van de functies van de meest recente versie van SQL Server, inclusief online bewerkingen, automatische plan correcties en andere prestatieverbeteringen enterprise. 

| **PaaS voordelen** | **Bedrijfscontinuïteit** |
| --- | --- |
|Er is geen hardware aanschaffen en beheer <br>Er is geen management overhead voor het beheren van onderliggende infrastructuur <br>Snelle inrichting en schalen van de service <br>Automatische upgrade voor patchen en versie <br>Integratie met andere services PaaS-gegevens |99,99% beschikbaarheid SLA  <br>Ingebouwde hoge beschikbaarheid <br>Gegevens die worden beveiligd met automatische back-ups <br>Klant configureerbare back-up bewaarperiode (vaste tot 7 dagen in Public Preview) <br>Back-ups gebruiker gestart <br>Punt in tijd database terugzetten mogelijkheid |
|**Beveiliging en naleving** | **Management**|
|Geïsoleerde omgeving (VNet-integratie, één tenant-service, speciale berekenings- en <br>Versleuteling van de gegevens onderweg <br>Azure AD-verificatie, ondersteuning voor eenmalige aanmelding <br>Voldoet aan de nalevingsstandaards dezelfde als de Azure SQL database <br>Controleren voor SQL <br>Detectie van bedreigingen |Azure Resource Manager-API voor het automatiseren van de service-inrichting en schalen <br>Azure portal-functionaliteit voor handmatige service inrichten en schalen <br>Service voor migratie van gegevens 

![Eenmalige aanmelding](./media/sql-database-managed-instance/sso.png) 

## <a name="vcore-based-purchasing-model-preview"></a>op basis van vCore aankoopmodel (preview)

De vCore gebaseerde aankoopmodel (preview) biedt uw flexibiliteit, controle, transparantie en een eenvoudige manier om te vertalen naar on-premises werklastvereisten naar de cloud. Dit model kunt u scale compute, geheugen en opslag op basis van hun werkbelasting behoeften. Het model vCore is ook in aanmerking voor van 30 procent besparingen met het [Azure hybride gebruik voordeel voor SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md).

Een virtuele core vertegenwoordigt de logische CPU aangeboden met de optie te kiezen tussen generaties van hardware.
- Logische CPU's van de vierde generatie zijn gebaseerd op Intel E5 2673 v3 (Haswell)-processors van 2,4 GHz.
- Gen 5 logische CPU's zijn gebaseerd op Intel E5-2673 v4 (Broadwell) 2.3 GHz-processors.

De volgende tabel helpt u begrijpen hoe u selecteert de optimale configuratie van uw berekenings-, geheugen-, opslag- en i/o-resources.

||Gen 4|Gen 5|
|----|------|-----|
|Hardware|Intel E5-2673 v3 (Haswell) 2,4 GHz processors, gekoppeld SSD vCore = 1 PP (fysieke kernen)|Intel E5-2673 v4 (Broadwell) 2.3 GHz processors, snelle eNVM SSD, vCore = 1 LP (hyper-thread)|
|Prestatieniveaus|8, 16, 24 vCores|8, 16, 24, 32, 40 vCores|
|Geheugen|7GB per vCore|5.5GB per vCore|
||||

## <a name="managed-instance-service-tier"></a>Beheerde exemplaar servicelaag

Beheerde exemplaar is beschikbaar in een enkel servicelaag - algemeen - die is ontworpen voor toepassingen met typische beschikbaarheid en algemene vereisten voor i/o-latentie.

De volgende lijst beschrijft de belangrijkste kenmerken van de servicelaag voor algemene doeleinden: 

- Ontwerpen voor de meeste zakelijke toepassingen met normale prestaties en HA-vereisten 
- Hoge prestaties Azure Premium-opslag (8 TB) 
- 100 databases / exemplaar 

U kunt onafhankelijk opslag selecteren en de rekencapaciteit in deze laag. 

Het volgende diagram illustreert de actieve compute en de redundante knooppunten in deze servicelaag.
 
![Algemene doel servicelaag](./media/sql-database-managed-instance/general-purpose-service-tier.png) 

Hieronder worden de belangrijkste functies van de servicelaag voor algemene doeleinden:

|Functie | Beschrijving|
|---|---|
| Aantal vCores * | 8, 16, 24 (gen 4)<br>8, 16, 24, 32, 40 (Gen5)|
| SQL Server-versie / bouwen | SQL Server recente (beschikbaar) |
| Grootte van de opslagruimte min | 32 GB |
| Maximale opslaggrootte | 8 TB |
| Maximale opslag per database | 8 TB |
| Verwachte opslag IOP 's | 500-7500 IOP's per bestand (afhankelijk van gegevensbestand). Zie [Premium-opslag](../virtual-machines/windows/premium-storage-performance.md#premium-storage-disk-sizes) |
| Het aantal gegevensbestanden (rijen) per de database | Meerdere | 
| Aantal logboekbestanden (LOGBOEKREGISTRATIE) per database | 1 | 
| Automatische back-ups worden beheerd | Ja |
| HA | Op basis van de externe opslag en [Azure Service Fabric](../service-fabric/service-fabric-overview.md) |
| Ingebouwde exemplaar en bewaking van de database en metrische gegevens | Ja |
| Automatische software patchen | Ja |
| VNet - implementatie van Azure Resource Manager | Ja |
| VNet - klassieke implementatiemodel | Nee |
| Portal-ondersteuning | Ja|
|||

\* Een virtuele core vertegenwoordigt de logische CPU aangeboden met de optie te kiezen tussen generaties van hardware. Gen 4 logische CPU's zijn gebaseerd op Intel E5-2673 v3 (Haswell) 2,4 GHz processors en Gen 5 logische CPU's zijn gebaseerd op Intel E5-2673 v4 (Broadwell) 2.3 GHz-processors. 

## <a name="advanced-security-and-compliance"></a>Geavanceerde beveiliging en naleving van voorschriften 

### <a name="managed-instance-security-isolation"></a>Beheerde exemplaar isolatie 

Beheerde exemplaar bieden extra beveiliging, isolatie van andere tenants in de Azure-cloud. Isolatie omvat: 

- [Implementatie van systeemeigen virtueel netwerk](sql-database-managed-instance-vnet-configuration.md) en de verbinding met uw on-premises omgeving met behulp van Azure Express Route- of VPN-Gateway 
- SQL-eindpunt is zichtbaar alleen via een particuliere IP-adres, zodat u veilige connectiviteit van persoonlijke Azure- of hybride netwerken
- Single-tenant met speciale onderliggende infrastructuur (compute, storage)

Het volgende diagram geeft een overzicht van isolatie ontwerp: 

![Hoge beschikbaarheid](./media/sql-database-managed-instance/application-deployment-topologies.png)  

### <a name="auditing-for-compliance-and-security"></a>Controles voor naleving en beveiliging 

[Beheerde exemplaar controle](sql-database-managed-instance-auditing.md) houdt database gebeurtenissen en schrijft deze naar een auditlogboek Meld u bij uw Azure storage-account. Controle kunt regelgeving onderhouden, de activiteit in een database begrijpen en meer inzicht krijgen in discrepanties en afwijkingen die kunnen wijzen op problemen voor het bedrijf of vermoedelijke beveiligingsschendingen. 

### <a name="data-encryption-in-motion"></a>Versleuteling van gegevens in beweging 

Beheerde exemplaar beveiligt uw gegevens dankzij de codering voor gegevens in beweging met Transport Layer Security.

Naast transport layer security, beheerde exemplaar van SQL Database biedt bescherming van gevoelige gegevens in rust en tijdens de verwerking met query vlucht [altijd versleuteld](/sql/relational-databases/security/encryption/always-encrypted-database-engine). Het unieke Always Encrypted biedt ongeëvenaarde beveiliging tegen diefstal van kritieke gegevens. Bijvoorbeeld met altijd versleuteld, zijn creditcardnummers versleuteld opgeslagen in de database altijd, zelfs tijdens query verwerken, zodat ontsleuteling bij gebruik door geautoriseerde personeel of toepassingen die moeten worden verwerkt die gegevens. 

### <a name="dynamic-data-masking"></a>Dynamische gegevensmaskering 

SQL-Database [dynamische gegevensmaskering](/sql/relational-databases/security/dynamic-data-masking) blootstelling van gevoelige gegevens beperkt door het maskeren voor replicagegevens gebruikers. Dynamische gegevensmaskering helpt te voorkomen dat onbevoegde toegang tot gevoelige gegevens doordat u toewijzen hoeveel van de gevoelige gegevens om weer te geven met minimale gevolgen voor de toepassingslaag. Dit is een beveiligingsfunctie op basis van beleid. De gevoelige gegevens in de resultatenset van een query die is uitgevoerd op toegewezen databasevelden worden verborgen, terwijl de gegevens in de database niet worden gewijzigd. 

### <a name="row-level-security"></a>Beveiliging op rijniveau 

[Beveiliging op gebruikersniveau rij](/sql/relational-databases/security/row-level-security) kunt u de controle op rijen in een databasetabel toegang op basis van de kenmerken van de gebruiker een query uit te voeren (zoals door de groep lidmaatschap of -uitvoering context). Beveiliging op rijniveau (RLS) vereenvoudigt het ontwerp en de code van de beveiliging in uw toepassing. Met RLS kunt u beperkingen instellen voor de toegang tot gegevens in rijen. Bijvoorbeeld, ervoor zorgen dat werknemers toegang kunnen krijgen tot alleen de rijen met gegevens die betrekking hebben op de afdeling of een gegevenstoegang beperken tot alleen de relevante gegevens. 

### <a name="threat-detection"></a>Detectie van bedreigingen 

[Detectie van dreigingen exemplaar beheerd](sql-database-managed-instance-threat-detection.md) vormt een aanvulling op [beheerd exemplaar controle](sql-database-managed-instance-auditing.md) door te geven van een extra laag van beveiliging intelligence ingebouwd in de service die door ongebruikelijke en potentieel schadelijke aanvallen kunnen worden gedetecteerd toegang tot of misbruik van databases. U wordt gewaarschuwd over mogelijke beveiligingsproblemen met verdachte activiteiten en SQL-injectie aanvallen, evenals database afwijkende toegangspatronen. Dagelijks geconstateerde waarschuwingen kunnen bekeken worden vanuit [Azure Security Center](https://azure.microsoft.com/services/security-center/) en Geef details op van de verdachte activiteit en actie voor het onderzoeken en het risico dat het beste.  

### <a name="azure-active-directory-integration-and-multi-factor-authentication"></a>Azure Active Directory-integratie en meervoudige verificatie 

Dankzij [Azure Active Directory-integratie](sql-database-aad-authentication.md) kunt u in SQL Database de identiteit van databasegebruikers en andere Microsoft-services centraal beheren. Deze mogelijkheid vereenvoudigt het beheer van machtigingen en verbetert de beveiliging. Azure Active Directory ondersteunt [Multi-Factor Authentication](sql-database-ssms-mfa-authentication-configure.md) (MFA) voor betere beveiliging van gegevens en toepassingen, en ondersteunt ook een proces voor eenmalige aanmelding (SSO). 

### <a name="authentication"></a>Verificatie 
SQL database-verificatie verwijst naar hoe gebruikers hun identiteit bewijzen bij het verbinden met de database. SQL Database ondersteunt twee typen verificatie:  

- SQL-verificatie met een gebruikersnaam en wachtwoord.
- Azure Active Directory-verificatie, die gebruikmaakt van identiteiten die worden beheerd door Azure Active Directory en wordt ondersteund voor beheerde en geïntegreerde domeinen. 

### <a name="authorization"></a>Autorisatie

Autorisatie verwijst naar wat een gebruiker binnen een Azure SQL Database kunt uitvoeren en wordt beheerd door de database rollidmaatschappen en objectmachtigingen uw gebruikersaccount. Beheerde exemplaar heeft dezelfde autorisatiemogelijkheden als SQL Server 2017. 

## <a name="database-migration"></a>Databasemigratie 

Beheerd exemplaar doelen gebruikersscenario met massaopslag databasemigratie vanaf on-premises of IaaS database implementaties. Beheerd exemplaar ondersteunt verschillende opties voor de migratie van database: 

### <a name="data-migration-service"></a>Service voor migratie van gegevens

De Azure-Service voor het migreren van Database is een volledig beheerde service die is ontworpen om in te schakelen naadloze migraties uit meerdere databasebronnen naar Azure Data platforms met minimale downtime. Deze service stroomlijnt de vereiste taken voor het verplaatsen van bestaande van derden en SQL Server-databases naar Azure. Implementatie-opties bevatten Azure SQL Database, exemplaar beheerd en SQL Server in Azure VM op openbare Preview. Zie [uw lokale-database migreren naar beheerde exemplaar met behulp van DMS](https://aka.ms/migratetoMIusingDMS). 

### <a name="backup-and-restore"></a>Back-ups en herstellen  

De migratie maakt gebruik van SQL-back-ups naar Azure blob storage. Back-ups opgeslagen in Azure storage-blob kunnen rechtstreeks worden hersteld naar exemplaar dat wordt beheerd. Als u een bestaande SQL-database herstellen naar een beheerde-exemplaar, kunt u het volgende doen:

- Gebruik [Data migratie-Service (DMS)](../dms/dms-overview.md). Zie voor een zelfstudie [migreren naar een exemplaar beheerd met behulp van Azure Database migratie Service (DMS)](../dms/tutorial-sql-server-to-managed-instance.md) van een back-upbestand van de database herstellen
- Gebruik de [T-SQL-herstelopdracht](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-transact-sql). 
  - Zie voor een zelfstudie waarin wordt getoond hoe Wide World Importers - back-upbestand Standard database terugzetten [herstelt van een back-upbestand naar een exemplaar beheerd](sql-database-managed-instance-restore-from-backup-tutorial.md). Deze zelfstudie ziet dat u hebt voor het uploaden van een back-upbestand naar de Azure-blog opslag en te beveiligen met behulp van een Shared access signature (SAS)-sleutel.
  - Zie voor meer informatie over het terugzetten van URL [systeemeigen terugzetten vanaf URL](sql-database-managed-instance-migrate.md#native-restore-from-url).

## <a name="sql-features-supported"></a>SQL-functies die worden ondersteund 

Beheerde exemplaar doelstellingen leveren bijna serveroppervlaktegebied van 100% compatibel met lokale SQL Server afkomstig in fasen totdat de algemene beschikbaarheid van de service. Voor een functies en vergelijkingslijst Zie [algemene functies van SQL](sql-database-features.md).
 
Beheerde exemplaar ondersteunt achterwaartse compatibiliteit met SQL 2008-databases. Directe migratie van SQL 2005-database-servers wordt ondersteund, compatibiliteitsniveau voor de gemigreerde SQL 2005-databases worden bijgewerkt naar SQL 2008. 
 
Het volgende diagram geeft een overzicht van de compatibiliteit van het oppervlak in beheerde exemplaar:  

![Migratie](./media/sql-database-managed-instance/migration.png) 

### <a name="key-differences-between-sql-server-on-premises-and-managed-instance"></a>Belangrijke verschillen tussen de SQL Server on-premises en beheerd exemplaar 

Beheerde exemplaar voordelen van wordt altijd-up-to-date in de cloud, betekent dat voor sommige functies in on-premises SQL-Server kunnen een verouderd, buiten gebruik gesteld of alternatieven zijn. Er zijn specifieke gevallen wanneer extra wilt dat een bepaalde functie in een iets andere manier werkt of service wordt niet uitgevoerd in een omgeving die u wilt geen volledig controle herkend: 

- Hoge beschikbaarheid is ingebouwd in en vooraf is geconfigureerd. Altijd op functies voor hoge beschikbaarheid niet worden weergegeven in een dezelfde manier als bij SQL IaaS-implementaties 
- Automatische back-ups en punt in tijd terugzetten. Klanten kan initiëren `copy-only` back-ups die niet aan de automatische back-keten verstoren. 
- Beheerde exemplaar kan geen volledige fysieke paden opgeven zodat alle bijbehorende scenario's moeten anders worden ondersteund: RESTORE DB biedt geen ondersteuning voor het verplaatsen met, DB maken mag geen fysieke paden, BULK INSERT werkt met Azure Blobs alleen, enzovoort. 
- Beheerde exemplaar ondersteunt [Azure AD authentication](sql-database-aad-authentication.md) als cloud alternatief voor het Windows-verificatie. 
- Beheerde exemplaar beheert automatisch de XTP-bestandsgroep en bestanden voor databases met In-geheugen OLTP-objecten
 
### <a name="managed-instance-administration-features"></a>Beheerde exemplaar-beheerfuncties  

Beheerde exemplaar inschakelen systeembeheerder om zich te richten op wat belangrijk de meest voor bedrijven is. Veel systeembeheerder/DBA activiteiten zijn niet vereist of ze zijn eenvoudige. Bijvoorbeeld, OS / RDBMS-installatie en patchen, dynamische exemplaar vergroten of verkleinen en configuratie, back-ups van databasereplicatie (inclusief systeemdatabases), configuratie voor hoge beschikbaarheid en configuratie van de status en prestaties bewaken van gegevens stromen. 

> [!IMPORTANT]
> Zie voor een lijst van ondersteunde gedeeltelijk ondersteunde en niet-ondersteunde functies [SQL-Database functies](sql-database-features.md). Zie voor een lijst van T-SQL-verschillen in ten opzichte van de SQL Server-exemplaren beheerd [beheerd exemplaar T-SQL verschillen met SQL Server](sql-database-managed-instance-transact-sql-information.md)
 
## <a name="next-steps"></a>Volgende stappen

- Voor een functies en vergelijkingslijst Zie [algemene functies van SQL](sql-database-features.md).
- Lees [Managed Instance VNet Configuration](sql-database-managed-instance-vnet-configuration.md) (VNet-configuratie voor beheerd exemplaar) voor meer informatie over VNet-configuratie.
- Zie voor een zelfstudie maakt u een exemplaar beheerd en een database herstelt vanuit een back-upbestand [maken van een exemplaar beheerd](sql-database-managed-instance-create-tutorial-portal.md).
- Lees het artikel [Managed Instance migration using DMS](../dms/tutorial-sql-server-to-managed-instance.md) (Migratie van een beheerd exemplaar via DMS) voor een zelfstudie over gebruik van de Azure Database Migration Service (DMS).
