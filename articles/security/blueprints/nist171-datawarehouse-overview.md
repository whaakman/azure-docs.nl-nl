---
title: Azure-beveiliging en naleving blauwdruk - datawarehouse voor SP NIST 800-171
description: Azure-beveiliging en naleving blauwdruk - datawarehouse voor SP NIST 800-171
services: security
author: jomolesk
ms.assetid: dbc9cafe-115d-4965-b0d4-fcf235a064c8
ms.service: security
ms.topic: article
ms.date: 07/31/2018
ms.author: jomolesk
ms.openlocfilehash: a1850ecfbb21eb9495bb0e6de362dc8dee3026a2
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2019
ms.locfileid: "57242357"
---
# <a name="azure-security-and-compliance-blueprint---data-warehouse-for-nist-sp-800-171"></a>Azure-beveiliging en naleving blauwdruk - datawarehouse voor SP NIST 800-171

## <a name="overview"></a>Overzicht
[NIST Special Publication 800-171](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-171.pdf) bevat richtlijnen voor het beveiligen van de gecontroleerde niet-geclassificeerde informatie (CUI) die deel uitmaakt van nonfederal informatiesystemen en organisaties. SP NIST 800-171 stelt 14 families van de beveiligingsvereisten voor het beveiligen van de vertrouwelijkheid van CUI.

Deze Azure-beveiliging en naleving blauwdruk biedt hulp bij het implementeren van een datawarehouse-architectuur in Azure die een van NIST SP subset klanten 800-171-besturingselementen. Deze oplossing toont manieren waarin klanten kunnen voldoen aan bepaalde beveiligings- en nalevingsvereisten. Het fungeert ook als basis voor klanten om te bouwen en configureren van hun eigen datawarehouse-oplossingen in Azure.

Deze referentiearchitectuur, gekoppelde Implementatiehandleiding en risicomodel zijn bedoeld om te fungeren als een basis voor klanten aan te passen aan hun specifieke wensen. Ze al dan niet mogen worden gebruikt als-is in een productieomgeving. Klanten zijn verantwoordelijk voor het uitvoeren van de juiste beveiliging en naleving evaluaties van een oplossing die is gebouwd met behulp van deze architectuur. Vereisten kunnen verschillen op basis van de details van de uitvoering van elke klant.

## <a name="architecture-diagram-and-components"></a>Diagram van architectuur en onderdelen
Deze oplossing biedt een referentiearchitectuur waarmee een krachtige en veilige cloud datawarehouse. Er zijn twee afzonderlijke lagen in deze architectuur. Één laag is waar gegevens worden geïmporteerd, opgeslagen en klaargezet binnen een geclusterde SQL-omgeving. Een andere laag is voor de SQL datawarehouse. Met deze laag de gegevens zijn geladen met behulp van een extract-transform-load (ETL)-hulpprogramma (bijvoorbeeld [PolyBase](https://docs.microsoft.com/azure/sql-data-warehouse/load-data-from-azure-blob-storage-using-polybase) T-SQL-query's) voor verwerking. Nadat de gegevens worden opgeslagen in SQL Data Warehouse, kunt op een zeer grote schaal analyses uitvoeren.

Azure biedt tal van services voor rapportage en analyse voor de klant. Deze oplossing omvat SQL Server Reporting Services voor het snel maken van rapporten uit SQL datawarehouse. Alle SQL-verkeer wordt versleuteld met SSL door de toevoeging van zelfondertekende certificaten. Als een best practice aangeraden het gebruik van een vertrouwde certificeringsinstantie voor verbeterde beveiliging.

Azure SQL Data Warehouse worden gegevens opgeslagen in relationele tabellen met opslag in kolommen. Deze indeling beperkt aanzienlijk kosten voor gegevensopslag terwijl verbetert de prestaties van query's. Afhankelijk van gebruiksvereisten, kunnen SQL Data Warehouse-rekenresources worden omhoog of omlaag geschaald of volledig afgesloten als er geen actieve processen compute-resources nodig hebt.

Een SQL Server load balancer beheert SQL-verkeer om te controleren of hoge prestaties. Alle virtuele machines (VM's) in deze referentiearchitectuur implementeert in een beschikbaarheidsset met SQL Server-exemplaren in een AlwaysOn-beschikbaarheidsgroep is geconfigureerd. Deze configuratie biedt mogelijkheden voor hoge beschikbaarheid en herstel na noodgevallen.

Deze referentiearchitectuur voor datawarehouse bevat ook een Active Directory-laag voor het beheren van resources in de architectuur. De Active Directory-subnet kan eenvoudig acceptatie onder een groter geheel van de Active Directory-forest. Op deze manier kan de omgeving werken, zelfs wanneer de toegang tot de grotere forest is niet beschikbaar. Alle virtuele machines zijn van een domein is toegevoegd aan de Active Directory-laag. Ze Active Directory-groepsbeleid gebruiken om af te dwingen van beveiliging en naleving configuraties op het niveau van het besturingssysteem.

De oplossing maakt gebruik van Azure Storage-accounts, die klanten configureren kunnen met Storage Service Encryption kunt onderhouden van de vertrouwelijkheid van gegevens in rust. Azure slaat drie kopieën van gegevens binnen de geselecteerde datacenter van een klant voor tolerantie. Geografisch redundante opslag zorgt ervoor dat gegevens worden gerepliceerd naar een secundair Datacenter honderden van mijl opslaan en opnieuw opgeslagen als drie kopieën binnen die Datacenter. Deze benadering wordt voorkomen dat een negatieve gebeurtenis in het primaire datacenter van de klant leidt tot een verlies van gegevens.

Voor een betere beveiliging, worden alle resources in deze oplossing worden beheerd als een resourcegroep via Azure Resource Manager. Azure Active Directory (Azure AD)-op rollen gebaseerd toegangsbeheer (RBAC) wordt gebruikt voor het beheren van toegang tot geïmplementeerde resources. Deze bronnen omvatten klant sleutels in Azure Key Vault. Status van het bestandssysteem wordt bewaakt door Azure Security Center en Azure Monitor. Klanten configureren beide bewakingsservices voor het vastleggen van Logboeken. Status van het bestandssysteem wordt weergegeven in één dashboard dat is eenvoudig te gebruiken.

Een virtuele machine fungeert als een bastionhost management. Het biedt een beveiligde verbinding voor beheerders voor toegang tot geïmplementeerd resources. De gegevens zijn geladen in het faseringsgebied via deze bastionhost management. *Het is raadzaam dat u een VPN of Azure ExpressRoute-verbinding configureren voor beheer en de gegevens importeren in het subnet van referentie-architectuur.*

![Data Warehouse voor SP NIST 800-171 verwijzing Architectuurdiagram](images/nist171-dw-architecture.png "datawarehouse voor SP NIST 800-171 verwijzing Architectuurdiagram")

Deze oplossing maakt gebruik van de volgende Azure-services. Zie voor meer informatie de [architectuur](#deployment-architecture) sectie.

- Beschikbaarheidssets
    - Active Directory-domeincontrollers
    - SQL Server-clusterknooppunten en -witness
- Azure Active Directory
- Azure Data Catalog
- Azure Key Vault
- Azure Monitor (Logboeken)
- Azure Security Center
- Azure Load Balancer
- Azure Storage
- Azure Virtual Machines
    - (1) bastionhost
    - (2) active Directory-domeincontroller
    - (2) SQL Server-clusterknooppunt
    - (1) SQL Server-witness
- Azure Virtual Network
    - (1) /16 netwerk
    - (4) /24 netwerken
    - (4) de netwerkbeveiligingsgroepen
- Recovery Services-kluis
- SQL Data Warehouse
- SQL Server Reporting Services

## <a name="deployment-architecture"></a>Implementatie-architectuur
De volgende sectie bevat de elementen van de implementatie en uitvoering.

**Azure SQL Data Warehouse**: [SQL Data Warehouse](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-overview-what-is) is een zakelijke datawarehouse die gebruik maakt van krachtige parallelle verwerking voor het snel complexe query's uitvoeren voor petabytes aan gegevens. Gebruikers kunnen eenvoudige PolyBase T-SQL-query's gebruiken om big data importeren in SQL datawarehouse en de kracht van enorme parallelle verwerking gebruiken om uit te voeren analyses met hoge prestaties.

**SQL Server Reporting Services**: [SQL Server Reporting Services](https://docs.microsoft.com/sql/reporting-services/report-data/sql-azure-connection-type-ssrs) biedt snel maken van rapporten met tabellen, diagrammen, kaarten, meters, matrices en meer voor SQL Data Warehouse.

**Azure Data Catalog**: [Data Catalog](https://docs.microsoft.com/azure/data-catalog/data-catalog-what-is-data-catalog) vergemakkelijkt gegevensbronnen detecteren en te begrijpen door de gebruikers die de gegevens beheren. Algemene gegevensbronnen kunnen worden geregistreerd, gelabeld en gezocht naar gegevens. De gegevens blijven in de bestaande locatie, maar een kopie van de metagegevens wordt toegevoegd aan Data Catalog. Een verwijzing naar de locatie van de gegevens is opgenomen. De metagegevens worden geïndexeerd zodat elke gegevensbron gemakkelijk detecteren via zoekopdrachten. Indexering ook is het begrijpen om de gebruikers die de gegevensbron detecteren.

**Bastionhost**: De bastionhost is de enkel ingangspunt die gebruikers gebruiken kunnen voor toegang tot de geïmplementeerde resources in deze omgeving. De bastionhost biedt een beveiligde verbinding met de geïmplementeerde resources door alleen extern verkeer vanaf openbare IP-adressen op een veilige lijst. Als u wilt toestaan dat verkeer van extern bureaublad, moet de oorzaak van het verkeer worden gedefinieerd in de netwerkbeveiligingsgroep.

Deze oplossing maakt u een virtuele machine als een domein bastionhost met de volgende configuraties:
-   [Antimalware-uitbreiding](https://docs.microsoft.com/azure/security/azure-security-antimalware).
-   [Azure Diagnostics-extensie](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template).
-   [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) met Key Vault.
-   Een [beleid voor automatisch afsluiten](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) verkleind van VM-resources als deze niet in gebruik.
-   [Windows Defender Credential Guard](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard) zodat referenties en andere geheimen worden uitgevoerd in een beveiligde omgeving die is geïsoleerd van het besturingssysteem is ingeschakeld.

### <a name="virtual-network"></a>Virtueel netwerk
Deze referentiearchitectuur definieert een particulier virtueel netwerk met een adresruimte van 10.0.0.0/16.

**Netwerkbeveiligingsgroepen**: [Netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) (nsg's) bevatten toegangsbeheerlijsten die verkeer binnen een virtueel netwerk toestaan of weigeren. Nsg's kunnen worden gebruikt om verkeer een subnet of afzonderlijke virtuele machine te beveiligen. De volgende nsg's bestaan:
  - Een NSG voor de gegevenslaag (SQL Server-clusters, SQL Server-witness en SQL load balancer)
  - Een NSG voor het beheer van bastionhost
  - Een NSG voor Active Directory
  - Een NSG voor SQL Server Reporting Services

Elk van de nsg's heeft specifieke poorten en protocollen open zodat de oplossing kan veilig en goed werken. Bovendien zijn de volgende configuraties voor elke NSG ingeschakeld:
  - [Diagnostische logboeken en gebeurtenissen](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) zijn ingeschakeld en die zijn opgeslagen in een storage-account.
  - Logboeken in Azure Monitor is verbonden met de [diagnostische gegevens van de NSG](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json).

**Subnetten**: Elk subnet is gekoppeld aan de bijbehorende NSG.

### <a name="data-at-rest"></a>Data-at-rest
De architectuur beveiligt gegevens in rust via meerdere metingen. Deze metingen zijn versleuteling en Databasecontrole.

**Azure Storage**: Om te voldoen aan vereisten voor versleutelde gegevens in rust, alle [opslag](https://azure.microsoft.com/services/storage/) maakt gebruik van [Storage Service Encryption](https://docs.microsoft.com/azure/storage/storage-service-encryption). Deze functie helpt beschermen en beveiligen van gegevens ter ondersteuning van de beveiliging van de organisatie toezeggingen en nalevingsvereisten.

**Azure Disk Encryption**: [Schijfversleuteling](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) maakt gebruik van de BitLocker-functie van Windows voor volumeversleuteling voor het besturingssysteem en gegevensschijven. De oplossing kan worden geïntegreerd met Key Vault voor het beheren en beheren van de versleutelingssleutels op de schijf.

**Azure SQL Database**: De SQL Database-exemplaar maakt gebruik van de volgende metingen van de database-beveiliging:
-   [Active Directory-verificatie en autorisatie](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) maakt identiteitsbeheer van databasegebruikers en andere Microsoft-services op één centrale locatie.
-   [SQL Database auditing](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) databasegebeurtenissen bijgehouden en geschreven naar een auditlogboek in Azure storage-account.
-   SQL-Database is geconfigureerd voor het gebruik van [transparante gegevensversleuteling](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql). Het voert realtime versleuteling en ontsleuteling van de database, gekoppelde back-ups en transactielogboekbestanden om gegevens in rust te beveiligen. Transparante gegevensversleuteling biedt de zekerheid dat de opgeslagen gegevens niet is onderworpen aan onbevoegde toegang.
-   [Firewall-regels](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) voorkomen dat u alle toegang tot de database-servers, totdat de juiste machtigingen zijn toegekend. De firewall verleent toegang tot databases op basis van het IP-adres waar de aanvraag vandaan komt.
-   [SQL Threat Detection](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) kunt u detectie en reactie op mogelijke bedreigingen wanneer deze zich voordoen. Het biedt beveiligingswaarschuwingen voor verdachte databaseactiviteiten, potentiële kwetsbaarheden, SQL-injectieaanvallen en afwijkende patronen voor databasetoegang.
-   [Versleutelde kolommen](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) ervoor te zorgen dat gevoelige gegevens nooit wordt weergegeven als tekst zonder opmaak in de database-systeem. Nadat versleuteling van gegevens is ingeschakeld, alleen clienttoepassingen of appservers met toegang tot de sleutels toegang tot gegevens van de tekst zonder opmaak.
- [Uitgebreide eigenschappen](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql) kan worden gebruikt om te stoppen met de verwerking van de betrokkenen. Gebruikers kunnen aangepaste eigenschappen toevoegen aan de database-objecten. Ze kunnen ook gegevens labelen als 'Discontinued' voor de ondersteuning van de toepassingslogica om te voorkomen dat de verwerking van gerelateerde financiële gegevens.
- [Beveiliging op rijniveau](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) kunnen gebruikers voor het definiëren van beleid om toegang te beperken tot gegevens te verwerken.
- [SQL Database dynamische gegevensmaskering](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) blootstelling van gevoelige gegevens maskeren van de gegevens naar de replicagegevens gebruikers of toepassingen. Het kan automatisch detecteren van potentieel gevoelige gegevens en voorstellen van de juiste maskers moet worden toegepast. Dynamische gegevensmaskering helpt bij de toegang beperken zodat gevoelige gegevens niet van de database via niet-gemachtigde toegang afsluiten. *Klanten zijn verantwoordelijk voor het aanpassen van de instellingen om te voldoen aan het schema van de database.*

### <a name="identity-management"></a>Identiteitsbeheer
De volgende technologieën bieden mogelijkheden voor het beheren van toegang tot gegevens in de Azure-omgeving:
-   [Azure AD](https://azure.microsoft.com/services/active-directory/) is de Microsoft multitenant cloudgebaseerde directory en identity management-service. Alle gebruikers voor deze oplossing worden gemaakt in Azure AD en de gebruikers die toegang hebben tot de SQL-database.
-   Verificatie van de toepassing wordt uitgevoerd met behulp van Azure AD. Zie voor meer informatie over het [toepassingen integreren met Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Azure AD de versleuteling van de kolom database ook gebruikt voor verificatie van de toepassing met SQL-Database. Zie voor meer informatie over het [bescherming van gevoelige gegevens in SQL-Database](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   [Azure RBAC](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) door beheerders kan worden gebruikt om te voorzien in specifieke toegangsmachtigingen definiëren. Met, kunnen ze alleen de mate van toegang verlenen dat gebruikers moeten hun werk. In plaats van elke Onbeperkte gebruikerstoegang voor Azure-resources geven, beheerders kunnen toestaan dat alleen bepaalde acties voor toegang tot bronnen en gegevens. Abonnementstoegang is beperkt tot de beheerder van het abonnement.
- [Azure Active Directory Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) door klanten kan worden gebruikt om het minimaliseren van het aantal gebruikers die toegang tot bepaalde gegevens, zoals gegevens hebben. Beheerders kunnen Azure AD Privileged Identity Management gebruiken om te detecteren, beperken en controleren van bevoegde identiteiten en hun toegang tot bronnen. Deze functionaliteit kan ook worden gebruikt om af te dwingen op aanvraag, just-in-time beheerderstoegang wanneer dat nodig is.
- [Azure Active Directory Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) detecteert mogelijke beveiligingsproblemen die invloed hebben op de identiteiten in een organisatie. Automatische antwoorden op gedetecteerde verdachte activiteit met betrekking tot een organisatie-id's configureren Ook onderzoekt het probleem verdachte incidenten passende actie op te lossen moet ondernemen.

### <a name="security"></a>Beveiliging
**Geheimen management**: De oplossing maakt gebruik van [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) voor het beheer van sleutels en geheimen. Key Vault helpt Beveilig cryptografische sleutels en geheimen die worden gebruikt door cloudtoepassingen en -services. De volgende mogelijkheden voor Key Vault zodat klanten gegevensbeveiliging:
- Geavanceerde beleidsregels zijn geconfigureerd op basis van behoefte.
- Toegangsbeleid voor Key Vault zijn met de minimaal vereiste machtigingen voor sleutels en geheimen gedefinieerd.
- Verloopdatum hebben alle sleutels en geheimen in Key Vault.
- Alle sleutels in Key Vault zijn beveiligd door gespecialiseerde hardware security modules. Het sleuteltype is een hardware security module beveiligde 2048-bits RSA-sleutel.
- Alle gebruikers en identiteiten zijn minimaal vereiste machtigingen verleend met behulp van RBAC.
- Diagnostische logboeken voor Key Vault worden ingeschakeld met een bewaarperiode van ten minste 365 dagen.
- Toegestane cryptografiebewerkingen voor sleutels zijn beperkt tot die nodig is.

**Patchbeheer**: Windows VM's geïmplementeerd als onderdeel van deze referentiearchitectuur zijn standaard geconfigureerd voor automatische updates ontvangen van Windows Update-Service. Deze oplossing bevat ook de [Azure Automation](https://docs.microsoft.com/azure/automation/automation-intro) service waarmee bijgewerkte implementaties kunnen worden gemaakt voor het patchen van virtuele machines wanneer dat nodig is.

**Bescherming tegen schadelijke software**: [Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) voor virtuele machines biedt de mogelijkheid van de realtime-beveiliging die helpt bij identificeren en virussen, spyware en andere schadelijke software verwijderen. Klanten kunnen waarschuwingen genereren wanneer bekende schadelijke of ongewenste software probeert te installeren of uitvoeren op de beveiligde virtuele machines configureren.

**Azure Security Center**: Met [Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro), klanten kunnen centraal toepassen en beheren van beveiligingsbeleid voor workloads, blootstelling aan bedreigingen beperken en detecteren en reageren op aanvallen. Security Center heeft ook toegang tot bestaande configuraties van Azure-services voor configuratie en de serviceaanbevelingen om u te helpen bij het beveiligingspostuur verbeteren en gegevens te beschermen.

Security Center maakt gebruik van tal van mogelijkheden voor het detecteren om te waarschuwen klanten van potentiële aanvallen die zijn gericht hun omgevingen. Deze waarschuwingen bevatten waardevolle informatie over de trigger van de waarschuwing, de betrokken resources en de bron van de aanval. Security Center bevat een set [vooraf gedefinieerde beveiligingswaarschuwingen](https://docs.microsoft.com/azure/security-center/security-center-alerts-type) die worden geactiveerd wanneer een bedreiging of verdachte activiteit plaatsvindt. Klanten kunnen gebruikmaken van [aangepaste waarschuwingsregels](https://docs.microsoft.com/azure/security-center/security-center-custom-alert) voor het definiëren van nieuwe beveiligingswaarschuwingen op basis van gegevens die al zijn verzameld voor hun omgeving.

Security Center biedt beveiligingswaarschuwingen en incidenten. Security Center kunt u eenvoudiger voor klanten om te detecteren en mogelijke beveiligingsproblemen te verhelpen. Een [threat intelligence-rapport](https://docs.microsoft.com/azure/security-center/security-center-threat-report) wordt gegenereerd voor elke bedreigingen gedetecteerde. Incident response teams kunnen de rapporten gebruiken wanneer ze onderzoeken en bedreigingen verhelpen.

Deze referentiearchitectuur gebruikt ook de [evaluatie van beveiligingsproblemen](https://docs.microsoft.com/azure/security-center/security-center-vulnerability-assessment-recommendations) mogelijkheden in Security Center. Nadat deze geconfigureerd, rapporteert de agent van een partner (bijvoorbeeld Qualys) gegevens over beveiligingsproblemen naar het beheerplatform van de partner. Het beheerplatform van de partner rapporteert op zijn beurt weer controlegegevens over beveiligingsproblemen en status naar Security Center. Klanten kunnen deze informatie gebruiken om snel te identificeren kwetsbare VM's.

### <a name="business-continuity"></a>Bedrijfscontinuïteit
**Hoge beschikbaarheid**: Server-workloads worden gegroepeerd in een [beschikbaarheidsset](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-manage-availability?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) om te zorgen voor hoge beschikbaarheid van virtuele machines in Azure. Ten minste één virtuele machine beschikbaar is tijdens gepland of ongepland onderhoud, die voldoet aan de 99,95% Azure SLA.

**Recovery Services-kluis**: De [Recovery Services-kluis](https://docs.microsoft.com/azure/backup/backup-azure-recovery-services-vault-overview) -instellingen van de back-upgegevens en alle configuraties van virtuele machines in deze architectuur beveiligt. Met een Recovery Services-kluis klanten kunnen bestanden en mappen terugzetten vanuit een IaaS-VM zonder het herstellen van de volledige virtuele machine. Dit proces sneller hersteltijden.

### <a name="logging-and-auditing"></a>Logboekregistratie en controle

Azure-services zich uitgebreid systeem en activiteit van gebruikers, evenals systeemstatus:
- **Activiteitenlogboeken**: [Activiteitenlogboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bieden inzicht in bewerkingen die worden uitgevoerd op resources in een abonnement. Activiteitenlogboeken kunnen u eenvoudiger bepalen van een bewerking initiator, tijdstip van de gebeurtenis en status.
- **Diagnostische logboeken**: [Diagnostische logboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) bevatten alle logboeken die zijn gegenereerd door elke resource. Deze logboeken bevatten de logboeken voor Windows-systeem, opslaglogboeken, auditlogboeken voor Key Vault en Azure Application Gateway toegangs- en firewall-Logboeken. Alle logboeken met diagnostische gegevens schrijven naar een gecentraliseerd en versleutelde Azure storage-account voor archivering. Gebruikers kunnen de bewaartermijn maximaal configureren tot 730 dagen, om te voldoen aan hun specifieke wensen.

**Logboeken in Azure Monitor**: Deze logboeken worden geconsolideerd [logboeken van Azure Monitor](https://azure.microsoft.com/services/log-analytics/) voor verwerking, opslag en -dashboardrapporten. Nadat gegevens zijn verzameld, onderverdeeld in afzonderlijke tabellen voor elk gegevenstype in Log Analytics-werkruimten. Op deze manier kunnen alle gegevens samen kunnen worden geanalyseerd, ongeacht de oorspronkelijke bron. Security Center kan worden geïntegreerd met Azure Monitor-Logboeken. Klanten kunnen Kusto-query's gebruiken voor toegang tot hun beveiligingsgegevens voor de gebeurtenis en combineren met gegevens van andere services.

De volgende Azure [bewakingsoplossingen](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions) zijn opgenomen als onderdeel van deze architectuur:
-   [Active Directory-evaluatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): De oplossing statuscontrole van Active Directory beoordeelt het risico en de gezondheid van server-omgevingen op een vast interval. Het biedt een geprioriteerde lijst met aanbevelingen die specifiek zijn voor de geïmplementeerde serverinfrastructuur.
- [SQL-evaluatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): De oplossing SQL-statuscontrole beoordeelt het risico en de gezondheid van server-omgevingen op een vast interval. Daardoor kunnen klanten met een geprioriteerde lijst met aanbevelingen die specifiek zijn voor de geïmplementeerde serverinfrastructuur.
- [Status van agent](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): De oplossing status van Agent rapporteert het aantal agents zijn geïmplementeerd en hun geografische verdeling. Deze rapporten ook het aantal agents zijn niet meer reageert en het aantal agents dat operationele gegevens verzendt.
-   [Activity Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): De oplossing Activity Log Analytics biedt ondersteuning voor analyse van de Azure-activiteitenlogboeken voor alle Azure-abonnementen voor een klant.

**Azure Automation**: [Automation](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker) worden opgeslagen, wordt uitgevoerd runbooks worden beheerd. In deze oplossing te runbooks verzamelen van Logboeken van de SQL-Database. Klanten kunnen gebruikmaken van de automatisering [bijhouden](https://docs.microsoft.com/azure/automation/automation-change-tracking) oplossing voor het herkennen van wijzigingen in de omgeving.

**Azure Monitor**: [Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) helpt gebruikers bij het bijhouden van prestaties, veiligheid en trends te identificeren. Organisaties kunnen deze gebruiken om te controleren, waarschuwingen maken en archiveren van gegevens. Ze kunnen ook API-aanroepen in hun Azure-resources bijhouden.

## <a name="threat-model"></a>Risicomodel

Het diagram van de gegevensstroom voor deze referentiearchitectuur is beschikbaar voor [downloaden](https://aka.ms/nist171-dw-tm) of kunt u hier vinden. Dit model kunt klanten inzicht krijgen in de punten van de mogelijke risico's in de infrastructuur van system wanneer deze wijzigingen aanbrengt.

![Data Warehouse voor SP NIST 800-171 risicomodel](images/nist171-dw-threat-model.png "datawarehouse voor SP NIST 800-171 risicomodel")

## <a name="compliance-documentation"></a>Naleving-documentatie
De [Azure-beveiliging en naleving blauwdruk – SP NIST 800-171 klant verantwoordelijkheid Matrix](https://aka.ms/nist171-crm) geeft een lijst van alle beveiligingsmaatregelen die zijn vereist voor SP NIST 800-171. Deze matrix bevat of de implementatie van elk besturingselement de verantwoordelijkheid van Microsoft, de klant is, of gedeeld tussen de twee.

De [Azure-beveiliging en naleving blauwdruk - SP NIST 800-171 Data Warehouse besturingselement implementatie Matrix](https://aka.ms/nist171-dw-cim) bevat informatie over welke SP NIST 800-171-besturingselementen worden gedekt door de datawarehouse-architectuur. Dit omvat gedetailleerde beschrijvingen van de manier waarop de implementatie voldoet aan de vereisten van elk besturingselement vallen.

## <a name="guidance-and-recommendations"></a>Richtlijnen en aanbevelingen

### <a name="vpn-and-expressroute"></a>VPN en ExpressRoute
Een beveiligde VPN-tunnel of [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) moet worden geconfigureerd voor een veilig verbinding met de resources die zijn geïmplementeerd als onderdeel van deze referentiearchitectuur voor datawarehouse. Door op de juiste wijze instellen van een VPN of ExpressRoute, toevoegen klanten een beveiligingslaag voor gegevens in transit.

Door het implementeren van een beveiligde VPN-tunnel met Azure, kan een virtuele particuliere verbinding tussen een on-premises netwerk en een Azure-netwerk worden gemaakt. Deze verbinding vindt plaats via Internet. Klanten kunnen deze verbinding met een veilig 'tunnel' gegevens in een gecodeerde verbinding tussen de klant netwerk en Azure gebruiken. Site-naar-site VPN is een veilige, volwassen technologie die is geïmplementeerd door bedrijven van alle groottes decennia. De [IPsec-tunnelmodus](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) bij deze optie als een versleutelingsmechanisme voor wordt gebruikt.

Omdat het verkeer binnen de VPN-tunnel over Internet met een site-naar-site-VPN wordt verzonden, biedt Microsoft een andere optie voor nog veiliger verbinding. ExpressRoute is een speciale WAN koppeling tussen Azure en een on-premises locatie of een Exchange-hostingprovider. ExpressRoute-verbindingen rechtstreeks verbinding maken met de klant telecommunicatie-provider. Als gevolg hiervan de gegevens niet via Internet worden verzonden en wordt niet blootgesteld aan het. Deze verbindingen bieden meer betrouwbaarheid, hogere snelheden, kortere wachttijden en hogere beveiliging dan gewone verbindingen.

Aanbevolen procedures voor het implementeren van een beveiligd hybride netwerk dat een on-premises netwerk naar Azure uitbreidt zijn [beschikbaar](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

### <a name="extract-transform-load-process"></a>Extract-Transform-Load-proces
[PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) kunnen gegevens laden in SQL Data Warehouse zonder de noodzaak voor een afzonderlijke ETL of hulpprogramma voor importeren. PolyBase biedt toegang tot gegevens via T-SQL-query's. De Microsoft business intelligence en analyse-stack en hulpprogramma's van derden die compatibel met SQL Server zijn kunnen worden gebruikt met PolyBase.

### <a name="azure-ad-setup"></a>Installatie van de Azure AD
[Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) is essentieel voor het beheren van de implementatie en inrichting van toegang tot personeel interactie met de omgeving. On-premises Active Directory kunnen worden geïntegreerd met Azure AD in [vier klikken](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-express). Klanten kunnen ook de geïmplementeerde Active Directory-infrastructuur (domeincontrollers) naar Azure AD koppelen. Om dit te doen, moet u de geïmplementeerde Active Directory-infrastructuur een subdomein van een Azure AD-forest.

### <a name="optional-services"></a>Optionele services
Azure biedt tal van services die u helpen met de opslag en fasering van geformatteerd en niet-opgemaakte gegevens. De volgende services kunnen worden toegevoegd aan deze verwijzende architectuur op basis van de klantvereisten:
-   [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/introduction) is een beheerde cloudservice die gebouwd voor complexe hybride ETL, extract-load-transform en gegevensintegratieprojecten. Data Factory heeft mogelijkheden om gegevens zoeken en te traceren. Visualisatie- en controlehulpprogramma's identificeren wanneer gegevens ontvangen en waar ze vandaan. Klanten kunnen maken en plannen van gegevensgestuurde werkstromen, pijplijnen, die gegevens uit verschillende gegevensarchieven opnemen genoemd. De pijplijnen kan worden gebruikt voor opname van gegevens uit interne en externe bronnen. Klanten kunnen vervolgens worden verwerkt en transformeer de gegevens voor uitvoer in gegevensarchieven, zoals SQL Data Warehouse.
- Klanten kunnen niet-gestructureerde gegevens in fase [Azure Data Lake Store](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-overview) om vast te leggen van de gegevens van elke grootte, soort en opnamesnelheid op één locatie voor operationele en verkennende analyse. Azure Data Lake bevat mogelijkheden waarmee de extractie en de conversie van gegevens. Data Lake Store is compatibel met de meeste open source-onderdelen in het Hadoop-ecosysteem. Deze integreert ook goed met andere Azure-services, zoals SQL Data Warehouse.

## <a name="disclaimer"></a>Vrijwaring

 - Dit document is uitsluitend ter informatie bedoeld. MICROSOFT BIEDT GEEN GARANTIES, EXPLICIETE, IMPLICIETE OF WETTELIJKE GARANTIE VOOR DE INFORMATIE IN DIT DOCUMENT. Dit document wordt geleverd ' as-is. " Informatie en meningen in dit document, inclusief URL's en andere websiteverwijzingen, kunnen zonder kennisgeving worden gewijzigd. Klanten die in dit document leest draagt het risico van het gebruik ervan.
 - Dit document biedt klanten met een enkel wettelijk recht op enig intellectueel in andere Microsoft-producten of oplossingen.
 - Klanten kunnen kopiëren en gebruiken van dit document voor interne referentiedoeleinden.
 - Bepaalde aanbevelingen in dit document kunnen leiden tot grotere hoeveelheden gegevens, netwerk- of gebruik van de compute-bronnen in Azure en de Azure-licentie of abonnement kosten van een klant kunnen verhogen.
 - Deze architectuur is bedoeld om te fungeren als een basis voor klanten om aan te passen op hun specifieke behoeften en mag niet worden gebruikt als-is in een productieomgeving.
 - Dit document is ontwikkeld als referentie en mag niet worden gebruikt voor het definiëren van alle middelen waarmee een klant kan voldoen aan specifieke nalevingsvereisten en voorschriften. Klanten moeten juridische ondersteuning van hun organisatie op goedgekeurde klantimplementaties gezocht.
