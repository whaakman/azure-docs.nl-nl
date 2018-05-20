---
title: Azure-beveiliging en naleving blauwdruk - datawarehouse voor GDPR
description: Azure-beveiliging en naleving blauwdruk - datawarehouse voor GDPR
services: security
author: jomolesk
ms.assetid: 7a33fb3b-cfb6-49dd-ab4d-c53cf0d5da41
ms.service: security
ms.topic: article
ms.date: 05/14/2018
ms.author: jomolesk
ms.openlocfilehash: a091f4c941b1f4a338ad23956dbba3a7b89b87e8
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/14/2018
---
# <a name="azure-security-and-compliance-blueprint-data-warehouse-for-gdpr"></a>Azure-beveiliging en naleving blauwdruk: datawarehouse voor GDPR

## <a name="overview"></a>Overzicht
De algemene gegevens beveiliging regelgeving (GDPR) bevat veel vereisten over het verzamelen, opslaan en gebruiken van persoonlijke gegevens, inclusief hoe organisaties identificeren en beveiligen van persoonlijke gegevens, aangepast aan de vereisten voor transparantie, detecteren en rapporteren persoonlijke gegevens inbreuk en train privacy personeel en andere werknemers. De GDPR personen biedt meer controle over hun persoonlijke gegevens en legt veel nieuwe verplichtingen in organisaties die verzamelen, verwerkt of persoonlijke gegevens te analyseren. De GDPR legt nieuwe regels voor organisaties die goederen bieden en services naar mensen in de Europese Unie, of dat verzamelen en analyseren van gegevens die zijn gekoppeld aan de EU inwoners. De GDPR is van toepassing ongeacht waar een organisatie zich bevinden.

Microsoft heeft Azure met toonaangevende beveiligingsmaatregelen en het privacybeleid ter bescherming van gegevens in de cloud, met inbegrip van de categorieën van persoonlijke gegevens die zijn geïdentificeerd door de GDPR ontworpen. Microsoft [contractvoorwaarden](http://aka.ms/Online-Services-Terms) Microsoft door te voeren op de vereisten van de processors.

Deze Azure-beveiliging en naleving blauwdruk biedt richtlijnen voor het implementeren van een datawarehouse-architectuur van de gegevens in Azure die bij de vereisten van de GDPR helpt. Deze oplossing laat zien waarin klanten kunnen voldoen aan specifieke vereisten voor beveiliging en naleving manieren en fungeert als basis voor klanten om te bouwen en oplossingen voor hun eigen datawarehouse configureren in Azure. Klanten kunnen gebruikmaken van deze verwijzende architectuur en voer de Microsoft [vier stappen proces](https://aka.ms/gdprebook) in hun reis GDPR naleving:
1. Detecteren: Identificeer welke persoonsgegevens bestaat en waar deze zich bevindt.
2. Beheren: Bepalen hoe persoonlijke gegevens wordt gebruikt en gebruikt.
3. Beveiligen: Tot stand brengen beveiligingsmaatregelen als u wilt detecteren, voorkomen van en reageren op beveiligingsproblemen en inbreuken op de gegevens.
4. Rapport: Vereiste documentatie houden en beheren van aanvragen voor gegevens en meldingen schenden.

Deze referentiearchitectuur, gekoppelde Implementatiehandleiding en risicomodel zijn bedoeld om te fungeren als basis voor klanten aan te passen aan hun specifieke vereisten en mag niet worden gebruikt als-is in een productieomgeving. Houd rekening met het volgende:
- De architectuur biedt een basis zodat klanten werkbelastingen in een GDPR-compatibele manier implementeren in Azure.
- Klanten zijn verantwoordelijk voor het uitvoeren van de juiste beveiligingsrechten en beoordeling van naleving van een oplossing die zijn gebouwd met behulp van deze architectuur als vereisten kunnen variëren op basis van de details van de implementatie van de klant.

## <a name="architecture-diagram-and-components"></a>Architectuurdiagram en onderdelen
Deze oplossing biedt een referentiearchitectuur waarmee een krachtige en veilige cloud datawarehouse wordt geïmplementeerd. Er zijn twee afzonderlijke gegevenslagen in deze architectuur: een waarbij gegevens worden geïmporteerd, opgeslagen en tijdelijk worden opgeslagen in een geclusterde SQL-omgeving, en een andere voor Azure SQL Data Warehouse waarin de gegevens worden geladen met een hulpprogramma ETL (bijvoorbeeld [PolyBase](https://docs.microsoft.com/azure/sql-data-warehouse/load-data-from-azure-blob-storage-using-polybase)T-SQL-query's) voor de verwerking. Wanneer gegevens worden opgeslagen in Azure SQL Data Warehouse, kunt een grootschalige analyses uitvoeren.

Azure biedt tal van services voor rapportage en analyse voor de klant. Deze oplossing bevat SQL Server Reporting Services (SSRS) voor het snel maken van rapporten van de Azure SQL Data Warehouse. Alle SQL-verkeer is versleuteld met SSL door de toevoeging van zelfondertekende certificaten. Als een best practice adviseert Azure het gebruik van een vertrouwde certificeringsinstantie voor een betere beveiliging.

Gegevens in Azure SQL Data Warehouse wordt opgeslagen in de relationele tabellen met kolommen opslag, een indeling die aanzienlijk minder de opslagkosten voor gegevens en verbetert de prestaties van query's.  Afhankelijk van de usage-vereisten voldoet, worden Azure SQL Data Warehouse rekenresources omhoog of omlaag geschaald of volledig afgesloten als er geen actieve processen rekenresources vereisen.

Een SQL-Load Balancer beheert SQL-verkeer garanderen van hoge prestaties. Alle virtuele machines in deze verwijzende architectuur implementeren in een beschikbaarheidsset met SQL Server-exemplaren die zijn geconfigureerd in een AlwaysOn-beschikbaarheidsgroep voor hoge beschikbaarheid en herstel na noodgevallen mogelijkheden.

Deze verwijzende architectuur van de datawarehouse bevat ook een Active Directory (AD)-laag voor het beheren van resources in de architectuur. De Active Directory-subnet kunt gemakkelijk vaststelling volgens een groter geheel voor AD-forest, ononderbroken werking van de omgeving waardoor zelfs wanneer toegang tot de grotere forest niet beschikbaar is. Alle virtuele machines zijn met het domein is gekoppeld aan de laag Active Directory en Active Directory-groepsbeleid gebruiken om af te dwingen, beveiliging en naleving configuraties op het niveau van het besturingssysteem.

Een virtuele machine fungeert als een management bastion host, een beveiligde verbinding voor beheerders geïmplementeerd toegang tot bronnen te bieden. De gegevens laadt in het faseringsgebied via deze host van de bastionomgeving management. **Azure raadt aan om een VPN of Azure ExpressRoute-verbinding voor beheer en de gegevens importeren in het subnet van de architectuur verwijzing configureren.**

![Datawarehouse voor GDPR verwijzing Architectuurdiagram](images/gdpr-datawarehouse-architecture.png?raw=true "datawarehouse voor GDPR verwijzing Architectuurdiagram")

Deze oplossing gebruikt de volgende Azure-services. Details van de architectuur voor implementatie zijn de [architectuur voor implementatie](#deployment-architecture) sectie.

-   Azure Virtual Machines
    - (1) bastion-Host
    -   (2) active Directory-domeincontroller
    -   (2) de SQL Server-clusterknooppunt
    -   (1) de SQL Server-Witness
-   Beschikbaarheidssets
    - Active Directory-domeincontrollers
    - SQL-clusterknooppunten en -Witness
-   Virtual Network
    - (4) subnetten
    -   (4) de Netwerkbeveiligingsgroepen
- SQL Data Warehouse
- SQL Server Reporting Services
- Azure SQL-taakverdeler
- Azure Active Directory
- Recovery Services-kluis
- Azure Key Vault
- Operations Management Suite (OMS)
- Azure Data Catalog
- Azure Security Center

## <a name="deployment-architecture"></a>Architectuur voor implementatie
De volgende sectie wordt de implementatie en de implementatie-elementen.

**SQL Data Warehouse**: [SQL Data Warehouse](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-overview-what-is) is een Enterprise Data Warehouse (EDW) die gebruikmaakt van Massively parallelle verwerking MPP-snel uitvoeren van complexe query's voor petabytes aan gegevens, zodat gebruikers kunnen persoonlijke gegevens efficiënt te identificeren. Gebruikers kunnen eenvoudige PolyBase T-SQL-query's gebruiken om grote gegevens importeren in de SQL Data Warehouse en gebruikmaken van de kracht van MPP krachtige analyses uitvoeren.

**SQL Server Reporting Services (SSRS)**: [SQL Server Reporting Services](https://docs.microsoft.com/en-us/sql/reporting-services/report-data/sql-azure-connection-type-ssrs) biedt snel maken van rapporten met tabellen, grafieken, kaarten, meters, matrices en meer voor Azure SQL Data Warehouse.

**Data Catalog**: [Data Catalog](https://docs.microsoft.com/azure/data-catalog/data-catalog-what-is-data-catalog) gegevensbronnen gemakkelijk kunnen worden gedetecteerd en begrijpelijk maakt voor de gebruikers die de gegevens te beheren. Algemene gegevensbronnen kunnen worden geregistreerd, label en gezocht naar persoonlijke gegevens. De gegevens blijven in de bestaande locatie, maar een kopie van de metagegevens wordt toegevoegd aan Data Catalog, samen met een verwijzing naar de locatie van de gegevensbron. Deze metagegevens worden ook geïndexeerd zodat elke gegevensbron gemakkelijk kan worden gedetecteerd via zoekopdrachten, en begrijpelijk is voor gebruikers die de gegevensbron detecteren.

**Host van de bastionomgeving**: de host van de bastionomgeving is de aanspreekpunt waarmee gebruikers toegang krijgen tot de geïmplementeerde resources in deze omgeving. De host van de bastionomgeving biedt een beveiligde verbinding met de geïmplementeerde resources door alleen het externe verkeer van openbare IP-adressen toe te staan op een veilige lijst. Extern bureaublad (RDP)-verkeer toestaan, moet de bron van het verkeer in de Netwerkbeveiligingsgroep (NSG) worden gedefinieerd.

Deze oplossing maakt een virtuele machine als een host bastion domein met de volgende configuraties:
-   [Antimalware-uitbreiding](https://docs.microsoft.com/azure/security/azure-security-antimalware)
-   [OMS-uitbreiding](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-oms)
-   [Azure extensie voor diagnostische gegevens](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template)
-   [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) met Azure Key Vault
-   Een [automatisch afsluiten beleid](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) gebruik kan worden verkleind van bronnen voor virtuele machines wanneer deze niet in gebruik
-   [Windows Defender referentie Guard](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard) ingeschakeld zodat referenties en andere geheimen in een beveiligde omgeving die geïsoleerd van het besturingssysteem wordt uitgevoerd

### <a name="virtual-network"></a>Virtueel netwerk
Deze verwijzende architectuur definieert een persoonlijke VNet met een adresruimte van 10.0.0.0/16.

**Netwerkbeveiligingsgroepen**: [nsg's](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) bevatten toegangsbeheerlijsten (ACL's) die het verkeer binnen een VNet toestaan of weigeren. Nsg's kunnen worden gebruikt om verkeer op een subnet of afzonderlijke VM-niveau te beveiligen. De volgende nsg's bestaan:
  - Een NSG voor de gegevenslaag (SQL Server-Clusters, Witness van SQL Server en SQL Load Balancer)
  - Een NSG voor de host van de bastionomgeving management
  - Een NSG voor Active Directory
  - Een NSG voor SQL Server Reporting Services

Elk van de nsg's een bepaalde poorten en protocollen open zodat de oplossing kunt veilig en correct werken. Bovendien zijn de volgende configuraties voor elke NSG ingeschakeld:
  - [Logboeken met diagnostische gegevens en gebeurtenissen](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) zijn ingeschakeld en opgeslagen in een opslagaccount
  - OMS Log Analytics is verbonden met de [NSG van diagnostische gegevens](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json)

**Subnetten**: elk subnet is gekoppeld aan de bijbehorende NSG.

### <a name="data-at-rest"></a>Data-at-rest
De architectuur beveiligt de gegevens in rust via meerdere metingen, versleuteling en Databasecontrole.

**Azure Storage**: om te voldoen aan de versleutelde gegevens op de rest-vereisten, alle [Azure Storage](https://azure.microsoft.com/services/storage/) gebruikt [Opslagversleuteling Service](https://docs.microsoft.com/azure/storage/storage-service-encryption). Hierdoor kunnen beveiligen en bescherming van persoonlijke gegevens ter ondersteuning van de beveiliging van de organisatie verbintenissen en nalevingsvereisten gedefinieerd door de GDPR.

**Azure Disk Encryption**: [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) maakt gebruik van de BitLocker-functie van Windows voor volumeversleuteling voor het besturingssysteem en gegevensschijven. De oplossing integreert met Azure Key Vault voor het beheren en beheren van de versleutelingssleutels op de schijf.

**Azure SQL Database**: de Azure SQL Database-instantie gebruikt de volgende veiligheidsmaatregelen voor database:
-   [AD-verificatie en autorisatie](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) maakt identiteitsbeheer van databasegebruikers en andere Microsoft-services op één centrale locatie.
-   [SQL database auditing](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) houdt database gebeurtenissen en schrijft deze naar een auditlogboek Meld u bij een Azure storage-account.
-   SQL-Database is geconfigureerd voor gebruik [Transparent Data Encryption (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql), uitvoert, wordt er realtime versleuteling en ontsleuteling van de database, gekoppelde back-ups en transactielogboekbestanden om informatie op te beschermen rest. TDE biedt de zekerheid dat persoonlijke gegevens opgeslagen niet is onderworpen aan onbevoegde toegang.
-   [Firewall-regels](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) voorkomen dat u alle toegang tot de database-servers, totdat de juiste machtigingen worden toegekend. De firewall verleent toegang tot databases op basis van het IP-adres waar de aanvraag vandaan komt.
-   [Detectie van dreigingen SQL](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) kunt u de detectie en het antwoord op mogelijke bedreigingen wanneer deze zich voordoen doordat beveiligingswaarschuwingen voor verdachte databaseactiviteiten, mogelijke beveiligingsproblemen, SQL-injectieaanvallen en toegang tot de afwijkende database patronen.
-   [Always-gecodeerd kolommen](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) ervoor te zorgen dat gevoelige, persoonlijke gegevens nooit wordt weergegeven als leesbare tekst binnen het databasesysteem. Na het inschakelen van gegevensversleuteling alleen clienttoepassingen of appservers met toegang tot de sleutels hebben toegang tot gecodeerde gegevens.
- De [Extended Properties](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql) functie kan worden gebruikt om te stoppen met het verwerken van de betrokkenen, zoals gebruikers worden aangepaste eigenschappen toevoegen aan de database-objecten en gegevens labelen als 'uit 'assortiment ter ondersteuning van de toepassingslogica om te voorkomen dat de verwerking van de bijbehorende gegevens.
- [Beveiliging](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) kunnen gebruikers beleid om toegang te beperken voor het stoppen met het verwerken van gegevens definiëren.
- [SQL Database dynamische gegevens maskeren (DDM)](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) blootstelling gevoelige, persoonlijke gegevens wordt beperkt door het maskeren van de gegevens voor onbevoegde gebruikers of toepassingen. DDM kan automatisch detecteren van mogelijk gevoelige gegevens en voorstellen van de juiste maskers moet worden toegepast. Dit helpt bij de identificatie van persoonlijke gegevens die in aanmerking komen voor GDPR beveiliging, en voor het beperken van toegang zodat deze niet de database via toegang door onbevoegden wordt afgesloten. **Opmerking: Klanten moet DDM instellingen aanpassen om te voldoen aan het schema van de database.**

### <a name="identity-management"></a>Identiteitsbeheer
De volgende technologieën bieden mogelijkheden voor het beheer van toegang tot persoonlijke gegevens in de Azure-omgeving:
-   [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) van Microsoft multitenant cloud-gebaseerde directory en identity management-service is. Alle gebruikers voor deze oplossing worden gemaakt in AAD, met inbegrip van gebruikers die toegang tot de SQL-Database.
-   Verificatie van de toepassing wordt uitgevoerd met behulp van AAD. Zie voor meer informatie [toepassingen integreren met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Bovendien de versleuteling van de kolom database AAD voor verificatie van de toepassing met Azure SQL Database gebruikt. Zie voor meer informatie hoe [gevoelige gegevens in SQL-Database beveiligen](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   [Azure op rollen gebaseerde toegangsbeheer (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) kunnen beheerders voor het definiëren van verfijnde toegangsmachtigingen voor de hoeveelheid toegang verlenen dat gebruikers moeten uitvoeren van hun taken. In plaats van geeft elke onbeperkte gebruikersmachtigingen voor Azure-resources, kunnen beheerders toestaan om alleen bepaalde acties voor toegang tot persoonlijke gegevens. Abonnement toegang is beperkt tot abonnementsbeheerder.
- [AAD Privileged Identity Management (PIM)](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) kunnen klanten Beperk het aantal gebruikers die toegang tot bepaalde gegevens zoals persoonlijke gegevens hebben.  Beheerders kunnen AAD Privileged Identity Management gebruiken om te detecteren, Beperk en bewaak bevoegde identiteiten en de toegang tot bronnen. Deze functionaliteit kan ook worden gebruikt om af te dwingen op aanvraag, just in time beheerderstoegang wanneer deze nodig is.
- [AAD Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) mogelijke beveiligingsproblemen die invloed hebben op een organisatie-id's detecteert, configureert u automatische antwoorden op gedetecteerde verdachte acties met betrekking tot een organisatie-id's en onderzoekt verdachte incidenten, onderneem gepaste actie deze op te lossen.

### <a name="security"></a>Beveiliging
**Geheimen management**: de oplossing maakt gebruik van [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) voor het beheer van sleutels en geheimen. Met Azure Sleutelkluis kunt u de cryptografische sleutels en geheimen beveiligen die door cloudtoepassingen en -services worden gebruikt. De volgende mogelijkheden van Azure Sleutelkluis waarmee klanten kunnen persoonlijke gegevens en toegang tot deze gegevens te beveiligen:
- Geavanceerde beleidsregels zijn geconfigureerd op basis van de nodig.
- Sleutelkluis-beleid worden met de minimaal vereiste machtigingen aan sleutels en geheimen gedefinieerd.
- Alle sleutels en geheimen in de Sleutelkluis hebben verloopdatum.
- Alle sleutels in de Sleutelkluis worden beschermd door speciale hardware security modules (HSM's). Het sleuteltype is een HSM beveiligde 2048-bits RSA-sleutel.
- Alle gebruikers en identiteiten krijgen minimaal vereiste machtigingen met RBAC.
- Diagnostische logboeken voor Sleutelkluis worden ingeschakeld met een bewaarperiode van ten minste 365 dagen.
- Toegestane cryptografische bewerkingen voor sleutels zijn beperkt tot de referenties die nodig is.

**Beheer van patches**: Windows virtuele machines zijn geïmplementeerd als onderdeel van deze verwijzende architectuur zijn standaard geconfigureerd voor automatische updates ontvangen van Windows Update-Service. Deze oplossing bevat ook de OMS [Azure Automation](https://docs.microsoft.com/azure/automation/automation-intro) service waarmee bijgewerkte implementaties kunnen worden gemaakt met patch virtuele machines wanneer deze nodig is.

**Bescherming tegen schadelijke software**: [Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) voor virtuele Machines biedt de mogelijkheid dat helpt bij het identificeren en te verwijderen van virussen, spyware en andere schadelijke software, met configureerbare waarschuwingen van de realtime-beveiliging Wanneer bekende schadelijke of ongewenste software probeert te installeren of uitvoeren op de beveiligde virtuele machines.

**Beveiligingswaarschuwingen**: [Azure Security Center](https://docs.microsoft.com/en-us/azure/security-center/security-center-intro) kunnen klanten verkeer bewaken, verzamelen van Logboeken en gegevensbronnen voor bedreigingen analyseren. Azure Security Center heeft bovendien toegang tot bestaande configuratie van Azure services voor configuratie en serviceaanbevelingen om te helpen verbeteren beveiligingspostuur en persoonlijke gegevens te beschermen. Azure Security Center bevat een [dreiging intelligence-rapport](https://docs.microsoft.com/en-us/azure/security-center/security-center-threat-report) voor elk gedetecteerd threat om te helpen respons op incidenten teams onderzoeken en bedreigingen te herstellen.

### <a name="business-continuity"></a>Bedrijfscontinuïteit
**Hoge beschikbaarheid**: Server-werkbelastingen worden gegroepeerd in een [Beschikbaarheidsset](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-manage-availability?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) om te zorgen voor hoge beschikbaarheid van virtuele machines in Azure. Ten minste één virtuele machine is beschikbaar tijdens gepland of ongepland onderhoud, voldoen aan de 99,95% Azure SLA.

**Recovery Services-kluis**: de [Recovery Services-kluis](https://docs.microsoft.com/azure/backup/backup-azure-recovery-services-vault-overview) -instellingen van de back-upgegevens en alle configuraties van Azure Virtual Machines in deze architectuur beveiligt. Met een Recovery Services-kluis klanten kunnen bestanden en mappen herstellen van een IaaS-VM zonder te herstellen van de hele virtuele machine, het inschakelen van sneller worden hersteld.

### <a name="logging-and-auditing"></a>Logboekregistratie en controle
[Operations Management Suite (OMS)](https://docs.microsoft.com/en-us/azure/operations-management-suite/operations-management-suite-overview) biedt uitgebreide logboekregistratie van het systeem- en gebruikersactiviteit, evenals de systeemstatus. De OMS [logboekanalyse](https://azure.microsoft.com/services/log-analytics/) oplossing worden verzameld en analyseert gegevens die zijn gegenereerd voor resources in Azure en on-premises omgevingen.
- **Activiteitenlogboeken**: [activiteitenlogboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bieden inzicht in de bewerkingen die worden uitgevoerd op resources in een abonnement. Activiteitenlogboeken kunt u bepalen van de initiator een bewerking, tijd van het exemplaar en status.
- **Diagnostische logboeken**: [diagnostische logboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) omvatten alle logboeken die door elke resource. Deze logboeken zijn Windows-gebeurtenislogboeken system en Azure Blob storage, tabellen en logboeken van de wachtrij.
- **Meld u archiveren**: alle logboeken met diagnostische gegevens schrijven naar een gecentraliseerd en versleutelde Azure storage-account voor archivering. De bewaartermijn is gebruiker configureerbare, up, 730 dagen om te voldoen aan vereisten voor de organisatie-specifieke bewaarperiode. Deze logboeken verbinding maken met Azure Log Analytics voor verwerking, opslaan en dashboard reporting.

Bovendien zijn de volgende OMS-oplossingen opgenomen als onderdeel van deze architectuur:
-   [AD-evaluatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): het Active Directory Health Check oplossing evalueert de risico's en de gezondheid van server-omgevingen met regelmatige tussenpozen en biedt een geprioriteerde lijst met aanbevelingen die specifiek zijn voor de geïmplementeerde serverinfrastructuur.
-   [Antimalware Assessment](https://docs.microsoft.com/azure/log-analytics/log-analytics-malware): de anti-malwareoplossing rapporten over de status van schadelijke software, bedreigingen en beveiliging.
-   [Azure Automation](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker): de Azure Automation-oplossing opgeslagen, wordt uitgevoerd en runbooks beheert.
-   [Beveiliging en Audit](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started): de beveiligings- en Audit dashboard biedt een op hoog niveau inzicht in de beveiligingsstatus van bronnen door te geven van metrische gegevens over beveiligingsdomeinen, problemen die aandacht vereisen, detecties, dreigingen en algemene beveiliging query's.
-   [SQL-evaluatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): de SQL-Serverstatus controleren oplossing evalueert de risico's en de gezondheid van server-omgevingen met regelmatige tussenpozen en biedt klanten een geprioriteerde lijst met aanbevelingen die specifiek zijn voor de geïmplementeerde serverinfrastructuur.
-   [Updatebeheer](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-update-management): oplossing voor het beheer van de Update kunt u beveiligingsupdates besturingssysteem, met inbegrip van de status van de beschikbare updates en het proces van het installeren van vereiste updates klant beheren.
-   [Status van agent](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): oplossing de status van de Agent rapporteert hoeveel agents zijn geïnstalleerd en hun geografische verdeling, evenals hoeveel agents die niet reageert en het aantal agents dat operationele gegevens wordt verzonden.
-   [Azure activiteitenlogboeken](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): de activiteit Log Analytics-oplossing biedt ondersteuning voor analyse van de Azure activiteitenlogboeken over alle Azure-abonnementen voor een klant.
-   [Het bijhouden van](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): de bijhouden oplossing kan klanten eenvoudig om wijzigingen te bepalen in de omgeving.

## <a name="threat-model"></a>Risicomodel

De gegevensstroom-diagram voor deze verwijzende architectuur is beschikbaar voor [downloaden](https://aka.ms/gdprDWdfd) of vindt u hieronder. Dit model kan zodat klanten inzicht krijgen van de punten van de mogelijke risico's in de infrastructuur van het systeem als u wijzigingen aanbrengt.

![Datawarehouse voor risicomodel GDPR](images/gdpr-datawarehouse-threat-model.png?raw=true "datawarehouse voor GDPR risicomodel")

## <a name="compliance-documentation"></a>Documentatie voor naleving
De [Azure-beveiliging en naleving blauwdruk – GDPR klant verantwoordelijkheid Matrix](https://aka.ms/gdprCRM) controller en de processortijd verantwoordelijkheden voor alle GDPR artikelen bevat. Houd er rekening mee dat voor de Azure-services, een klant meestal de controller is en Microsoft als de processor fungeert.

De [Azure-beveiliging en naleving blauwdruk - GDPR Data Warehouse implementatie Matrix](https://aka.ms/gdprDW) bevat informatie over welke GDPR artikelen worden verholpen door de architectuur voor datawarehouse, met inbegrip van gedetailleerde beschrijvingen van hoe de implementatie voldoet aan de vereisten van elk gedekte artikel.

## <a name="guidance-and-recommendations"></a>Richtlijnen en aanbevelingen
### <a name="vpn-and-expressroute"></a>VPN- en ExpressRoute
Een beveiligde VPN-tunnel of [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) moet worden geconfigureerd om veilig verbinding maken met de resources die zijn geïmplementeerd als onderdeel van deze verwijzende architectuur van de datawarehouse. Door op de juiste wijze instellen van een VPN of ExpressRoute, toevoegen klanten een laag van beveiliging voor gegevens onderweg.

Door het implementeren van een beveiligde VPN-tunnel met Azure kan een virtuele particuliere verbinding tussen een on-premises netwerk en een Azure-netwerk worden gemaakt. Deze verbinding vindt plaats via het Internet en kan klanten veilig 'tunnel' gegevens in een versleutelde koppeling tussen het netwerk en Azure van de klant. Site-naar-Site VPN is een beveiligde, volwassen technologie die is geïmplementeerd door bedrijven van elke grootte jarenlang. De [IPsec-tunnelmodus](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) wordt gebruikt bij deze optie als een mechanisme voor versleuteling.

Omdat het verkeer binnen de VPN-tunnel Internet met een site-naar-site VPN passeren, biedt Microsoft met een andere, zelfs veiliger verbindingsoptie. Azure ExpressRoute is een speciale WAN koppeling tussen Azure en een on-premises locatie of een Exchange-hostingprovider. Als het ExpressRoute-verbindingen gaan niet via het Internet, bieden deze verbindingen meer betrouwbaarheid, sneller en hebben ze lagere latenties en betere beveiliging dan gewone verbindingen via Internet. Bovendien, omdat dit een directe verbinding van de klant telecommunicatie provider, de gegevens niet via Internet kan worden verzonden en daarom geen toegang heeft tot het.

Aanbevolen procedures voor het implementeren van een beveiligde hybride netwerk die uitgebreider is dan een on-premises netwerk naar Azure [beschikbaar](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

### <a name="extract-transform-load-etl-process"></a>Proces extract-transformatie-Load (ETL)
[PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) kunnen gegevens laden in Azure SQL Data Warehouse zonder de noodzaak van een afzonderlijke ETL of hulpprogramma voor importeren. PolyBase biedt toegang tot gegevens via een T-SQL-query's. Microsoft business intelligence en analyse stack, evenals hulpprogramma's van derden die compatibel is met SQL Server kunnen worden gebruikt met PolyBase.

### <a name="azure-active-directory-setup"></a>Installatie van de Azure Active Directory
[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) is essentieel voor het beheren van de implementatie en inrichting van toegang tot personeel interactie met de omgeving. Een bestaande Windows Server Active Directory kan worden geïntegreerd met AAD in [vier klikken](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-express). Klanten kunnen de geïmplementeerde Active Directory-infrastructuur (domeincontrollers) ook koppelen aan een bestaand AAD door het maken van de geïmplementeerde Active Directory-infrastructuur een subdomein van een AAD-forest.

### <a name="optional-services"></a>Optionele services
Azure biedt tal van services om te helpen bij de opslag en fasering van geformatteerd en niet-opgemaakte gegevens. De volgende services kunnen worden toegevoegd aan deze verwijzende architectuur afhankelijk van de klantvereisten:
-   [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/introduction) is een beheerde cloudservice die is gebouwd voor complexe hybride extract-transformatie-load (ETL) extract load transformatie (ELT) en gegevens integratie-projecten. Azure Data Factory heeft mogelijkheden om persoonlijke gegevens zoeken en te traceren, inclusief visualisatie en hulpprogramma's om u te identificeren wanneer gegevens ontvangen en waar ze vandaan bewaken. Met behulp van Azure Data Factory, kunnen klanten maken en plannen van gegevensgestuurde werkstromen aangeroepen pijplijnen opnemen van gegevens uit verschillende gegevensarchieven. Deze pijplijnen kunnen klanten voor het opnemen van gegevens uit zowel interne als externe bronnen. Klanten kunnen vervolgens verwerken en de gegevens voor uitvoer transformeren naar gegevensarchieven zoals Azure SQL Data Warehouse.
- Klanten kunnen ongestructureerde gegevens in fase [Azure Data Lake Store](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-overview), waardoor het vastleggen van gegevens van elke grootte, type en opname in een enkele locatie voor operationele en experimentele analyses versnellen.  Azure Data Lake heeft mogelijkheden waarmee de extractie en de conversie van gegevens. Azure Data Lake Store is compatibel met de meeste open source-onderdelen in het Hadoop-ecosysteem en mooi is geïntegreerd met andere Azure-services zoals Azure SQL Data Warehouse.

## <a name="disclaimer"></a>Vrijwaring

 - Dit document is alleen ter informatie. MICROSOFT GEEFT GEEN GARANTIES, SNELLE, IMPLICIETE OF WETTELIJKE GARANTIE VOOR DE INFORMATIE IN DIT DOCUMENT. Dit document wordt geleverd ' as-is. " Informatie en inzichten die in dit document, inclusief URL's en andere websiteverwijzingen, kunnen zonder kennisgeving worden gewijzigd. Dit document lezen klanten draagt het risico voor het gebruik ervan.
 - Dit document biedt geen enkel wettelijk recht op enig intellectueel eigendom van alle Microsoft-product of oplossingen klanten.
 - Klanten kunnen kopiëren en gebruiken van dit document voor interne referentiedoeleinden.
 - Bepaalde aanbevelingen in dit document kunnen leiden tot hogere-, netwerk- of compute-Resourcegebruik in Azure en een klant Azure licentie of abonnement kosten kunnen verhogen.
 - Deze architectuur is bedoeld om te fungeren als basis voor klanten om aan te passen aan hun specifieke vereisten en mag niet worden gebruikt als-is in een productieomgeving.
 - Dit document is ontwikkeld als een verwijzing en mag niet worden gebruikt voor het definiëren van alle middelen waarmee een klant kan voldoen aan specifieke nalevingsvereisten en -voorschriften. Klanten moeten juridische ondersteuning van hun organisatie met goedgekeurde klant implementaties zoeken.
