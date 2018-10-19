---
title: Azure-beveiliging en naleving blauwdruk - datawarehouse Avg
description: Azure-beveiliging en naleving blauwdruk - datawarehouse Avg
services: security
author: jomolesk
ms.assetid: 7a33fb3b-cfb6-49dd-ab4d-c53cf0d5da41
ms.service: security
ms.topic: article
ms.date: 05/14/2018
ms.author: jomolesk
ms.openlocfilehash: 84e26212b1102d693b84e5b66fbd606da2673934
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/18/2018
ms.locfileid: "49405929"
---
# <a name="azure-security-and-compliance-blueprint-data-warehouse-for-gdpr"></a>Azure-beveiliging en naleving blauwdruk: datawarehouse Avg

## <a name="overview"></a>Overzicht
De General Data Protection Regulation (GDPR) bevat veel vereisten voor het verzamelen, opslaan en het gebruik van persoonlijke gegevens, met inbegrip van hoe organisaties identificeren en beveiligen van persoonlijke gegevens, aan transparantie te voldoen, detecteren en rapporteren schendingen van persoonlijke gegevens, en train privacy personeel en andere werknemers. De AVG personen biedt meer controle over hun persoonlijke gegevens en legt veel nieuwe verplichtingen voor organisaties die verzameld, verwerkt of persoonlijke gegevens te analyseren. De AVG legt een nieuwe regels voor organisaties die goederen en services naar mensen in de Europese Unie (EU) of die verzamelen en analyseren van gegevens van inwoners van de EU. De AVG is van toepassing, ongeacht waar een organisatie zich bevindt.

Microsoft heeft Azure ontworpen met toonaangevende beveiligingsmaatregelen en privacybeleidsregels ter bescherming van de gegevens in de cloud, waaronder de categorieën van persoonlijke gegevens die zijn geïdentificeerd door de GDPR. Microsofts [contractvoorwaarden](http://aka.ms/Online-Services-Terms) Microsoft aan de vereisten van processors die zijn doorgevoerd.

Deze Azure-beveiliging en naleving blauwdruk biedt hulp bij de implementatie van een datawarehouse-architectuur in Azure die ondersteuning aan de vereisten van de AVG biedt. Deze oplossing laat zien manieren waarin klanten specifieke vereisten voor beveiliging en naleving kunnen voldoen en fungeert als een basis voor klanten om te bouwen en configureren van hun eigen datawarehouse-oplossingen in Azure. Klanten kunnen gebruikmaken van deze referentiearchitectuur en gaat u als volgt Microsofts [vier stappen](https://aka.ms/gdprebook) in hun op weg naar de GDPR-naleving:
1. Detecteren: Identificeer welke persoonlijke gegevens bestaat en waar deze zich bevindt.
2. Beheren: Bepalen hoe persoonsgegevens wordt gebruikt en geopend.
3. Beveiligen: Tot stand brengen beveiligingsmaatregelen als u wilt voorkomen, detecteren en reageren op beveiligingsproblemen en inbreuken op de gegevens.
4. Rapport: Vereiste documentatie houden en beheren van aanvragen voor gegevens en in strijd is met meldingen.

Deze referentiearchitectuur, gekoppelde Implementatiehandleiding en risicomodel zijn bedoeld om te fungeren als een basis voor klanten aan te passen aan hun specifieke vereisten en mag niet worden gebruikt als-is in een productieomgeving. Houd rekening met het volgende:
- De architectuur biedt een basislijn om workloads naar Azure te implementeren op een manier die compatibel met GDPR klanten te helpen.
- Klanten zijn verantwoordelijk voor het uitvoeren van de juiste beveiliging en naleving evaluaties van een oplossing die is gebouwd met behulp van deze architectuur als vereisten kunnen variëren op basis van de details van de uitvoering van elke klant.

## <a name="architecture-diagram-and-components"></a>Diagram van architectuur en onderdelen
Deze oplossing biedt een referentiearchitectuur waarmee een krachtige en veilige cloud datawarehouse wordt geïmplementeerd. Er zijn twee afzonderlijke lagen in deze architectuur: één waar gegevens worden geïmporteerd, die zijn opgeslagen, en tijdelijk worden opgeslagen in een geclusterde SQL-omgeving, en een andere voor de Azure SQL Data Warehouse waar de gegevens zijn geladen met behulp van een ETL-hulpprogramma (bijvoorbeeld [PolyBase](https://docs.microsoft.com/azure/sql-data-warehouse/load-data-from-azure-blob-storage-using-polybase)T-SQL-query's) voor verwerking. Wanneer gegevens worden opgeslagen in Azure SQL Data Warehouse, kunt analytics uitvoeren op grote schaal.

Azure biedt tal van services voor rapportage en analyse voor de klant. Deze oplossing omvat SQL Server Reporting Services (SSRS) voor het snel maken van rapporten uit de Azure SQL Data Warehouse. Alle SQL-verkeer wordt versleuteld met SSL door de toevoeging van zelfondertekende certificaten. Als een best practice raadt Azure het gebruik van een vertrouwde certificeringsinstantie voor verbeterde beveiliging.

Gegevens in de Azure SQL Data Warehouse is opgeslagen in relationele tabellen met opslag in kolommen, een indeling die de kosten voor gegevensopslag aanzienlijk verlaagt en verbetert de prestaties van query's.  Afhankelijk van gebruiksvereisten, kunnen Azure SQL Data Warehouse-rekenresources worden omhoog of omlaag geschaald of volledig afgesloten als er geen actieve processen die rekenresources.

Een SQL-Load Balancer beheert SQL-verkeer, ervoor te zorgen dat de hoge prestaties. Alle virtuele machines in deze referentiearchitectuur implementeert in een beschikbaarheidsset met SQL Server-exemplaren die zijn geconfigureerd in een AlwaysOn-beschikbaarheidsgroep voor mogelijkheden voor hoge beschikbaarheid en herstel na noodgevallen.

Deze referentiearchitectuur voor datawarehouse bevat ook een laag Active Directory (AD) voor het beheren van resources in de architectuur. De Active Directory-subnet kan eenvoudig acceptatie onder een groter geheel voor AD-forest, zodat voor doorlopende werking van de omgeving, zelfs wanneer de toegang tot de grotere forest niet beschikbaar is. Alle virtuele machines worden toegevoegd aan de Active Directory-laag- en Active Directory-groepsbeleid gebruiken om af te dwingen van beveiliging en naleving configuraties op het niveau van het besturingssysteem.

Een virtuele machine fungeert als een bastionhost management biedt een beveiligde verbinding voor beheerders voor toegang tot geïmplementeerd resources. De gegevens zijn geladen in het faseringsgebied via deze bastionhost management. **Azure wordt aanbevolen voor het configureren van een VPN of Azure ExpressRoute-verbinding voor beheer en de gegevens importeren in het subnet van referentie-architectuur.**

![Datawarehouse voor AVG verwijzing Architectuurdiagram](images/gdpr-datawarehouse-architecture.png?raw=true "Data Warehouse voor Architectuurdiagram van GDPR-verwijzing")

Deze oplossing maakt gebruik van de volgende Azure-services. Informatie van de implementatiearchitectuur vindt u in de [architectuur](#deployment-architecture) sectie.

-   Azure Virtual Machines
    - (1) bastionhost
    -   (2) active Directory-domeincontroller
    -   (2) SQL Server-clusterknooppunt
    -   (1) SQL Server-Witness
-   Beschikbaarheidssets
    - Active Directory-domeincontrollers
    - SQL-clusterknooppunten en -Witness
-   Virtual Network
    - (4) subnetten
    -   (4) de Netwerkbeveiligingsgroepen
- SQL Data Warehouse
- SQL Server Reporting Services
- SQL Azure Load Balancer
- Azure Active Directory
- Recovery Services-kluis
- Azure Key Vault
- Log Analytics
- Azure Data Catalog
- Azure Security Center

## <a name="deployment-architecture"></a>Implementatie-architectuur
De volgende sectie bevat de elementen van de implementatie en uitvoering.

**SQL Data Warehouse**: [SQL Data Warehouse](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-overview-what-is) is een Enterprise Data Warehouse (EDW) die gebruikmaakt van uiterst (Massively Parallel Processing) als u wilt snel complexe query's uitvoeren voor petabytes aan gegevens, zodat gebruikers een voor persoonlijke gegevens efficiënt te identificeren. Gebruikers kunnen eenvoudige PolyBase T-SQL-query's gebruiken om big data importeren in SQL Data Warehouse en gebruikmaken van de kracht van MPP te krachtige analyses uitvoeren.

**SQL Server Reporting Services (SSRS)**: [SQL Server Reporting Services](https://docs.microsoft.com/sql/reporting-services/report-data/sql-azure-connection-type-ssrs) biedt snel maken van rapporten met tabellen, diagrammen, kaarten, meters, matrices en meer voor Azure SQL Data Warehouse.

**Data Catalog**: [Data Catalog](https://docs.microsoft.com/azure/data-catalog/data-catalog-what-is-data-catalog) gegevensbronnen gemakkelijk kunnen worden gedetecteerd en begrijpelijk is door de gebruikers die de gegevens beheren. Algemene gegevensbronnen kunnen worden geregistreerd, gelabeld en gezocht naar persoonlijke gegevens. De gegevens blijven in de bestaande locatie, maar een kopie van de metagegevens wordt toegevoegd aan Data Catalog, samen met een verwijzing naar de locatie van de gegevens. Deze metagegevens worden ook geïndexeerd zodat elke gegevensbron gemakkelijk kan worden gedetecteerd via zoekopdrachten, en begrijpelijk is voor gebruikers die de gegevensbron detecteren.

**Bastionhost**: de bastionhost is de enkel ingangspunt waarmee gebruikers toegang krijgen tot de geïmplementeerde resources in deze omgeving. De bastionhost biedt een beveiligde verbinding met de geïmplementeerde resources door toe te staan alleen extern verkeer vanaf openbare IP-adressen op een veilige lijst. Als u wilt toestaan dat verkeer van extern bureaublad (RDP), moet de oorzaak van het verkeer in de Netwerkbeveiligingsgroep (NSG) worden gedefinieerd.

Deze oplossing maakt u een virtuele machine als een domein bastionhost met de volgende configuraties:
-   [Antimalware-uitbreiding](https://docs.microsoft.com/azure/security/azure-security-antimalware)
-   [Log Analytics-extensie](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-oms)
-   [Azure Diagnostics-extensie](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template)
-   [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) met behulp van Azure Key Vault
-   Een [beleid voor automatisch afsluiten](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) verkleind van resources van de virtuele machine als deze niet in gebruik
-   [Windows Defender Credential Guard](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard) ingeschakeld die referenties en andere geheimen in een beveiligde omgeving die is geïsoleerd van het besturingssysteem wordt uitgevoerd

### <a name="virtual-network"></a>Virtueel netwerk
Deze referentiearchitectuur definieert een persoonlijke VNet met een adresruimte van 10.0.0.0/16.

**Netwerkbeveiligingsgroepen**: [nsg's](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) bevatten toegangsbeheerlijsten (ACL's) die verkeer binnen een VNet toestaan of weigeren. Nsg's kunnen worden gebruikt om verkeer een subnet of afzonderlijke virtuele machine te beveiligen. De volgende nsg's bestaan:
  - Een NSG voor de gegevenslaag (SQL Server-Clusters, SQL Server-Witness en SQL Load Balancer)
  - Een NSG voor het beheer van bastionhost
  - Een NSG voor Active Directory
  - Een NSG voor SQL Server Reporting Services

Elk van de nsg's zijn bepaalde poorten en protocollen openen, zodat de oplossing kunt veilig en goed werken. Bovendien zijn de volgende configuraties voor elke NSG ingeschakeld:
  - [Diagnostische logboeken en gebeurtenissen](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) zijn ingeschakeld en die zijn opgeslagen in een storage-account
  - Log Analytics is verbonden met de [NSG van diagnostische gegevens](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json)

**Subnetten**: elk subnet is gekoppeld aan de bijbehorende NSG.

### <a name="data-at-rest"></a>Data-at-rest
De architectuur beveiligt gegevens in rust via meerdere metingen, versleuteling en Databasecontrole.

**Azure Storage**: om versleutelde data-at-rest-vereisten te voldoen aan alle [Azure Storage](https://azure.microsoft.com/services/storage/) maakt gebruik van [Storage Service Encryption](https://docs.microsoft.com/azure/storage/storage-service-encryption). Dit helpt te beschermen en beveiligen van persoonlijke gegevens ter ondersteuning van de organisatie beveiligingsverplichtingen en nalevingsvereisten zijn gedefinieerd door de GDPR.

**Azure Disk Encryption**: [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) maakt gebruik van de BitLocker-functie van Windows om volumeversleuteling voor het besturingssysteem en gegevensschijven te bieden. De oplossing kan worden geïntegreerd met Azure Key Vault voor het beheren en beheren van de versleutelingssleutels op de schijf.

**Azure SQL Database**: de Azure SQL Database-exemplaar maakt gebruik van de volgende metingen van de database-beveiliging:
-   [AD-verificatie en autorisatie](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) maakt identiteitsbeheer van databasegebruikers en andere Microsoft-services op één centrale locatie.
-   [SQL database auditing](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) databasegebeurtenissen bijgehouden en geschreven naar een auditlogboek in Azure storage-account.
-   SQL-Database is geconfigureerd voor het gebruik van [transparante gegevensversleuteling (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql), die voert realtime versleuteling en ontsleuteling van de database, gekoppelde back-ups en transactielogboekbestanden ter bescherming van gegevens op over de rest. TDE biedt de zekerheid dat persoonlijke gegevens die zijn opgeslagen is niet onderhevig aan onbevoegde toegang.
-   [Firewall-regels](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) voorkomen dat u alle toegang tot de database-servers, totdat de juiste machtigingen zijn toegekend. De firewall verleent toegang tot databases op basis van het IP-adres waar de aanvraag vandaan komt.
-   [SQL Threat Detection](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) kunt u detectie en reactie op mogelijke bedreigingen wanneer deze zich voordoen, dankzij beveiligingswaarschuwingen voor verdachte databaseactiviteiten, potentiële kwetsbaarheden, SQL-injectieaanvallen en afwijkende database-toegang patronen.
-   [Altijd versleuteld kolommen](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) ervoor te zorgen dat gevoelige, persoonlijke gegevens nooit wordt weergegeven als tekst zonder opmaak in de database-systeem. Na het inschakelen van versleuteling van gegevens, alleen clienttoepassingen of appservers met toegang tot de sleutels gegevens zijn toegankelijk als tekst zonder opmaak.
- De [uitgebreide eigenschappen](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql) functie kan worden gebruikt om te stoppen met de verwerking van de betrokkenen, omdat hiermee gebruikers aangepaste eigenschappen toevoegen aan database-objecten en gegevens markeren als 'Discontinued' voor de ondersteuning van de toepassingslogica om te voorkomen dat de verwerking van de bijbehorende gegevens.
- [Beveiliging op rijniveau](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) kunnen gebruikers voor het definiëren van beleid om toegang te beperken tot gegevens te verwerken.
- [SQL Database Dynamic Data Masking (DDM)](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) blootstelling van gevoelige, persoonlijke gegevens door het maskeren van de gegevens naar niet-gemachtigde gebruikers of toepassingen. DDM kan automatisch detecteren van potentieel gevoelige gegevens en stel de juiste maskers moet worden toegepast. Dit helpt bij de identificatie van persoonlijke gegevens die in aanmerking komen voor de beveiliging van de AVG en voor het beperken van toegang, zodat deze niet de database via niet-geautoriseerde toegang wordt afgesloten. **Opmerking: Klanten moet DDM instellingen aanpassen om te voldoen aan het schema van de database.**

### <a name="identity-management"></a>Identiteitsbeheer
De volgende technologieën bieden mogelijkheden voor het beheren van toegang tot persoonlijke gegevens in de Azure-omgeving:
-   [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) is van Microsoft met meerdere tenants cloudgebaseerde directory service- en identiteit. Alle gebruikers voor deze oplossing worden gemaakt in AAD, met inbegrip van gebruikers met toegang tot de SQL-Database.
-   Verificatie van de toepassing wordt uitgevoerd met behulp van AAD. Zie voor meer informatie, [toepassingen integreren met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Bovendien de versleuteling van de kolom database maakt gebruik van AAD om te verifiëren van de toepassing naar Azure SQL Database. Zie voor meer informatie over het [bescherming van gevoelige gegevens in SQL-Database](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   [Azure Role-based Access Control (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) kunnen beheerders om te voorzien in specifieke toegangsmachtigingen te verlenen alleen de mate van toegang dat gebruikers moeten om hun werk te definiëren. In plaats van geeft elke onbeperkte gebruikersmachtigingen voor Azure-resources, beheerders kunnen toestaan dat alleen bepaalde acties voor toegang tot persoonlijke gegevens. Abonnementstoegang is beperkt tot de beheerder van het abonnement.
- [AAD Privileged Identity Management (PIM)](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) kunnen klanten voor het minimaliseren van het aantal gebruikers die toegang tot bepaalde gegevens, zoals persoonsgegevens hebben.  Beheerders kunnen AAD Privileged Identity Management gebruiken om te detecteren, beperken en controleren van bevoegde identiteiten en hun toegang tot bronnen. Deze functionaliteit kan ook worden gebruikt om af te dwingen op aanvraag, just-in-time beheerderstoegang wanneer dat nodig is.
- [AAD Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) mogelijke beveiligingsproblemen die betrekking hebben op van een organisatie-id's detecteert, configureert u automatische antwoorden op gedetecteerde verdachte activiteit met betrekking tot een organisatie-id's en verdachte onderzoekt het probleem incidenten, onderneem gepaste actie op te lossen.

### <a name="security"></a>Beveiliging
**Geheimen management**: de oplossing maakt gebruik van [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) voor het beheer van sleutels en geheimen. Met Azure Sleutelkluis kunt u de cryptografische sleutels en geheimen beveiligen die door cloudtoepassingen en -services worden gebruikt. De volgende mogelijkheden van Azure Key Vault zodat klanten bescherming van persoonlijke gegevens en toegang tot deze gegevens:
- Geavanceerde beleidsregels zijn geconfigureerd op basis van behoefte.
- Toegangsbeleid voor Key Vault zijn met de minimaal vereiste machtigingen voor sleutels en geheimen gedefinieerd.
- Verloopdatum hebben alle sleutels en geheimen in Key Vault.
- Alle sleutels in Key Vault worden beschermd door gespecialiseerde hardware security modules (HSM's). Het sleuteltype is een HSM beveiligd 2048-bits RSA-sleutel.
- Minimaal vereiste machtigingen met RBAC worden verleend door alle gebruikers en identiteiten.
- Diagnostische logboeken voor Key Vault worden ingeschakeld met een bewaarperiode van ten minste 365 dagen.
- Toegestane cryptografiebewerkingen voor sleutels zijn beperkt tot die nodig is.

**Patchbeheer**: Windows virtuele machines geïmplementeerd als onderdeel van deze referentiearchitectuur zijn standaard geconfigureerd voor automatische updates ontvangen van Windows Update-Service. Deze oplossing bevat ook de [Azure Automation](https://docs.microsoft.com/azure/automation/automation-intro) service waarmee bijgewerkte implementaties kunnen worden gemaakt voor de patch voor virtuele machines wanneer dat nodig is.

**Bescherming tegen schadelijke software**: [Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) voor virtuele Machines biedt de mogelijkheid van de realtime-beveiliging die helpt bij identificeren en virussen, spyware en andere schadelijke software, met configureerbare meldingen verwijderen Wanneer u bekende schadelijke of ongewenste software probeert te installeren of uitvoeren op de beveiligde virtuele machines.

**Beveiligingswaarschuwingen**: [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) kunnen klanten om verkeer te controleren, logboeken te verzamelen en analyseren van gegevensbronnen voor bedreigingen. Azure Security Center heeft bovendien toegang tot bestaande configuratie van Azure-services voor configuratie en de serviceaanbevelingen om u te helpen bij het beveiligingspostuur verbeteren en persoonlijke gegevens beschermen. Azure Security Center bevat een [threat intelligence-rapport](https://docs.microsoft.com/azure/security-center/security-center-threat-report) voor elke bedreigingen gedetecteerde te helpen incident response teams onderzoeken en bedreigingen verhelpen.

### <a name="business-continuity"></a>Bedrijfscontinuïteit
**Hoge beschikbaarheid**: Server-workloads worden gegroepeerd in een [Beschikbaarheidsset](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-manage-availability?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) om te zorgen voor hoge beschikbaarheid van virtuele machines in Azure. Ten minste één virtuele machine beschikbaar is tijdens gepland of ongepland onderhoud, voldoen aan de 99,95% Azure SLA.

**Recovery Services-kluis**: de [Recovery Services-kluis](https://docs.microsoft.com/azure/backup/backup-azure-recovery-services-vault-overview) -instellingen van de back-upgegevens en alle configuraties van Azure Virtual Machines in deze architectuur beveiligt. Met een Recovery Services-kluis, klanten kunnen bestanden en mappen terugzetten vanuit een IaaS-VM zonder te herstellen van de hele virtuele machine, waardoor sneller worden hersteld.

### <a name="logging-and-auditing"></a>Logboekregistratie en controle
[Log Analytics](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview) biedt uitgebreide logboekregistratie van het systeem- en gebruikersactiviteit, evenals de status van het bestandssysteem. De [Log Analytics](https://azure.microsoft.com/services/log-analytics/) oplossing verzamelt en analyseert gegevens gegenereerd door resources in Azure en on-premises omgevingen.
- **Activiteitenlogboeken**: [activiteitenlogboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bieden inzicht in bewerkingen die worden uitgevoerd op resources in een abonnement. Activiteitenlogboeken kunnen u eenvoudiger bepalen van een bewerking initiator, tijdstip van de gebeurtenis en status.
- **Diagnostische logboeken**: [diagnostische logboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) bevatten alle logboeken die zijn gegenereerd door elke resource. Deze logboeken bevatten de logboeken voor Windows-systeem en Azure Blob-opslag, tabellen en wachtrijlogboeken.
- **Meld u archiveren**: alle logboeken met diagnostische gegevens schrijven naar een gecentraliseerd en versleutelde Azure storage-account voor archivering. De bewaarperiode is om te voldoen aan vereisten voor de bewaarperiode van de organisatie-specifieke gebruiker-configureren, tot maximaal 730 dagen. Deze logboeken verbinding maken met Azure Log Analytics voor verwerking, opslag en -dashboardrapporten.

Bovendien is de volgende Log Analytics-oplossingen, opgenomen als onderdeel van deze architectuur:
-   [AD-evaluatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): het Active Directory statuscontrole oplossing beoordeelt het risico en de gezondheid van server-omgevingen op een vast interval en biedt een geprioriteerde lijst met aanbevelingen die specifiek zijn voor de geïmplementeerde serverinfrastructuur.
-   [Antimalware-evaluatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-malware): de antimalwareoplossing rapporten over malware, bedreigingen en bescherming van de status.
-   [Azure Automation](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker): de Azure Automation-oplossing worden opgeslagen, wordt uitgevoerd runbooks worden beheerd.
-   [Beveiliging en controle](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started): de beveiliging en controle dashboard biedt een hoogwaardig inzicht in de beveiligingsstatus van resources, dankzij metrische gegevens over beveiligingsdomeinen, problemen die aandacht vereisen, detecties, bedreigingsinformatie en algemene Beveiligingsquery's.
-   [SQL-evaluatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): statuscontrole van de SQL-oplossing beoordeelt het risico en de gezondheid van server-omgevingen op een vast interval en biedt klanten een geprioriteerde lijst met aanbevelingen die specifiek zijn voor de geïmplementeerde serverinfrastructuur.
-   [Updatebeheer](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-update-management): de updatebeheer-oplossing maakt Klantenbeheer van de beveiligingsupdates besturingssysteem, met inbegrip van de status van de beschikbare updates en het installatieproces van vereiste updates.
-   [Status van agent](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): oplossing status van de Agent rapporteert het aantal agents zijn geïmplementeerd en hun geografische verdeling, evenals hoeveel agents die niet meer reageert en het aantal agents die zijn operationele gegevens kan verzenden.
-   [Azure-activiteitenlogboeken](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): de Activity Log Analytics-oplossing biedt ondersteuning voor analyse van de Azure-activiteitenlogboeken voor alle Azure-abonnementen voor een klant.
-   [Wijzigingen bijhouden](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): de functie Wijzigingen bijhouden kan klanten eenvoudig wijzigingen in de omgeving identificeren.

## <a name="threat-model"></a>Risicomodel

Het diagram van de gegevensstroom voor deze referentiearchitectuur is beschikbaar voor [downloaden](https://aka.ms/gdprDWdfd) of vindt u hieronder. Dit model kunt klanten inzicht krijgen in de punten van de mogelijke risico's in de infrastructuur van het systeem als u wijzigingen aanbrengt.

![Datawarehouse voor AVG risicomodel](images/gdpr-datawarehouse-threat-model.png?raw=true "risicomodel AVG Data Warehouse")

## <a name="compliance-documentation"></a>Naleving-documentatie
De [Azure-beveiliging en naleving blauwdruk – AVG klant verantwoordelijkheid Matrix](https://aka.ms/gdprCRM) controller en processor verantwoordelijkheden voor alle AVG artikelen bevat. Houd er rekening mee dat voor Azure-services, een klant meestal de controller is en Microsoft als de processor fungeert.

De [Azure-beveiliging en naleving blauwdruk - AVG Data Warehouse-implementatie Matrix](https://aka.ms/gdprDW) bevat informatie over welke AVG artikelen worden verholpen door de datawarehouse-architectuur, met inbegrip van gedetailleerde beschrijvingen van hoe de implementatie voldoet aan de vereisten van elk artikel vallen.

## <a name="guidance-and-recommendations"></a>Richtlijnen en aanbevelingen
### <a name="vpn-and-expressroute"></a>VPN en ExpressRoute
Een beveiligde VPN-tunnel of [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) moet worden geconfigureerd voor een veilig verbinding met de resources die zijn geïmplementeerd als onderdeel van deze referentiearchitectuur voor datawarehouse. Door op de juiste wijze instellen van een VPN of ExpressRoute, toevoegen klanten een beveiligingslaag voor gegevens in transit.

Door het implementeren van een beveiligde VPN-tunnel met Azure, kan een virtuele particuliere verbinding tussen een on-premises netwerk en een Azure-netwerk worden gemaakt. Deze verbinding vindt plaats via Internet en kan klanten veilig 'tunnel' gegevens in een gecodeerde verbinding tussen het netwerk en Azure van de klant. Site-naar-Site VPN is een veilige, volwassen technologie die is geïmplementeerd door bedrijven van alle groottes decennia. De [IPsec-tunnelmodus](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) bij deze optie als een versleutelingsmechanisme voor wordt gebruikt.

Omdat het verkeer binnen de VPN-tunnel via Internet met een site-naar-site-VPN, biedt Microsoft een andere verbinding nog veiliger optie. Azure ExpressRoute is een speciale WAN koppeling tussen Azure en een on-premises locatie of een Exchange-hostingprovider. Als het ExpressRoute-verbindingen gaan niet via het Internet, bieden deze verbindingen een meer betrouwbaarheid, hogere snelheden, kortere wachttijden en hogere beveiliging dan gebruikelijke verbindingen via Internet. Bovendien, omdat dit een rechtstreekse verbinding van de klant telecommunicatie-provider, de gegevens niet via Internet kan worden verzonden en daarom geen toegang heeft tot deze.

Aanbevolen procedures voor het implementeren van een beveiligd hybride netwerk dat een on-premises netwerk naar Azure uitbreidt zijn [beschikbaar](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

### <a name="extract-transform-load-etl-process"></a>Proces extract-Transform-Load (ETL)
[PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) kunnen gegevens laden in Azure SQL Data Warehouse zonder de noodzaak voor een afzonderlijke ETL of hulpprogramma voor importeren. PolyBase biedt toegang tot gegevens via T-SQL-query's. Microsoft business intelligence en analyse-stack, evenals hulpprogramma's van derden die compatibel is met SQL Server, kunnen worden gebruikt met PolyBase.

### <a name="azure-active-directory-setup"></a>Installatie van de Azure Active Directory
[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) is essentieel voor het beheren van de implementatie en inrichting van toegang tot personeel interactie met de omgeving. Een bestaande Windows Server Active Directory kunnen worden geïntegreerd met AAD in [vier klikken](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-express). Klanten kunnen ook de geïmplementeerde Active Directory-infrastructuur (domeincontrollers) koppelen aan een bestaande AAD doordat de geïmplementeerde Active Directory-infrastructuur een subdomein van een AAD-forest.

### <a name="optional-services"></a>Optionele services
Azure biedt tal van services die u helpen met de opslag en fasering van geformatteerd en niet-opgemaakte gegevens. De volgende services kunnen worden toegevoegd aan deze referentiearchitectuur, afhankelijk van de eisen van klanten:
-   [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/introduction) is een beheerde cloudservice die is gebouwd voor complexe hybride extract-transform-load (ETL), extract-load-transform (ELT), en gegevensintegratieprojecten. Azure Data Factory heeft mogelijkheden om te helpen trace en persoonlijke gegevens, waaronder visualisatie- en controlehulpprogramma's om te identificeren wanneer gegevens ontvangen en waar ze vandaan. Met Azure Data Factory, kunnen klanten maken en plannen van gegevensgestuurde werkstromen pijplijnen die gegevens uit verschillende gegevensarchieven opnemen genoemd. Deze pijplijnen kunnen klanten voor opname van gegevens uit zowel interne als externe bronnen. Klanten kunnen vervolgens verwerken en transformeren van de gegevens voor uitvoer naar gegevensarchieven zoals Azure SQL Data Warehouse.
- Klanten kunnen niet-gestructureerde gegevens in fase [Azure Data Lake Store](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-overview), waarmee het vastleggen van gegevens van elke grootte, soort en gegevensopname versnellen op één plek voor operationele en verkennende analyse.  Azure Data Lake bevat mogelijkheden waarmee de extractie en de conversie van gegevens. Azure Data Lake Store is compatibel met de meeste open source-onderdelen in het Hadoop-ecosysteem en mooi integreert met andere Azure-services zoals Azure SQL Data Warehouse.

## <a name="disclaimer"></a>Vrijwaring

 - Dit document is uitsluitend ter informatie bedoeld. MICROSOFT BIEDT GEEN GARANTIES, EXPLICIETE, IMPLICIETE OF WETTELIJKE GARANTIE VOOR DE INFORMATIE IN DIT DOCUMENT. Dit document wordt geleverd ' as-is. " Informatie en meningen in dit document, inclusief URL's en andere websiteverwijzingen, kunnen zonder kennisgeving worden gewijzigd. Klanten die in dit document leest draagt het risico van het gebruik ervan.
 - Dit document biedt klanten met een enkel wettelijk recht op enig intellectueel in andere Microsoft-producten of oplossingen.
 - Klanten kunnen kopiëren en gebruiken van dit document voor interne referentiedoeleinden.
 - Bepaalde aanbevelingen in dit document kunnen leiden tot grotere hoeveelheden gegevens, netwerk- of gebruik van de compute-bronnen in Azure en de Azure-licentie of abonnement kosten van een klant kunnen verhogen.
 - Deze architectuur is bedoeld om te fungeren als een basis voor klanten om aan te passen op hun specifieke behoeften en mag niet worden gebruikt als-is in een productieomgeving.
 - Dit document is ontwikkeld als referentie en mag niet worden gebruikt voor het definiëren van alle middelen waarmee een klant kan voldoen aan specifieke nalevingsvereisten en voorschriften. Klanten moeten juridische ondersteuning van hun organisatie op goedgekeurde klantimplementaties gezocht.
