---
title: Azure-beveiliging en naleving blauwdruk - datawarehouse voor FedRAMP Automation
description: Azure-beveiliging en naleving blauwdruk - datawarehouse voor FedRAMP Automation
services: security
author: jomolesk
ms.assetid: 834d1ff6-8369-455f-b052-1ef301e3d7e6
ms.service: security
ms.topic: article
ms.date: 05/02/2018
ms.author: jomolesk
ms.openlocfilehash: 110ce131286f437e051dd859eb4d2baa29f106f6
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/04/2018
---
# <a name="azure-security-and-compliance-blueprint-data-warehouse-for-fedramp-automation"></a>Azure-beveiliging en naleving blauwdruk: datawarehouse voor FedRAMP Automation

## <a name="overview"></a>Overzicht

De [Federal risico en autorisatie Management-programma (FedRAMP)](https://www.fedramp.gov/) is een Verenigde Staten government wide programma waarmee een gestandaardiseerde aanpak voor beoordeling van de veiligheid, autorisatie en doorlopende bewaking voor cloud producten en services. Deze Azure-beveiliging en naleving blauwdruk biedt richtlijnen voor het leveren van een Microsoft Azure datawarehouse-architectuur waarmee een subset van hoge FedRAMP besturingselementen implementeren. Deze oplossing biedt richtlijnen voor de implementatie en configuratie van Azure-resources voor een algemene referentiearchitectuur manieren waarin klanten kunnen voldoen aan specifieke vereisten voor beveiliging en naleving te demonstreren en fungeert als basis voor klanten bouw en oplossingen voor hun eigen datawarehouse configureren in Azure.

Deze referentiearchitectuur, gekoppelde besturingselement implementatiehandleidingen en threat modellen zijn bedoeld om te fungeren als basis voor klanten om aan te passen aan hun specifieke vereisten en mag niet worden gebruikt als-is in een productieomgeving. Een toepassing in deze omgeving zonder aanpassingen implementeren is onvoldoende om volledig te voldoen aan de vereisten van de basislijn FedRAMP hoog. Houd rekening met het volgende:
- De architectuur biedt een basis zodat klanten werkbelastingen in een FedRAMP-compatibele manier implementeren in Azure.
- Klanten zijn verantwoordelijk voor het uitvoeren van de juiste beveiligingsrechten en beoordeling van naleving van een oplossing die zijn gebouwd met behulp van deze architectuur als vereisten kunnen variëren op basis van de details van de implementatie van de klant.

## <a name="architecture-diagram-and-components"></a>Architectuurdiagram en onderdelen

Deze oplossing biedt een datawarehouse referentiearchitectuur waarmee een krachtige en veilige cloud datawarehouse wordt geïmplementeerd. Er zijn twee afzonderlijke gegevenslagen in deze architectuur: een waarbij gegevens worden geïmporteerd, opgeslagen en tijdelijk worden opgeslagen in een geclusterde SQL-omgeving, en een andere voor Azure SQL Data Warehouse waarin de gegevens worden geladen met een hulpprogramma ETL (bijvoorbeeld [PolyBase](https://docs.microsoft.com/azure/sql-data-warehouse/load-data-from-azure-blob-storage-using-polybase)T-SQL-query's) voor de verwerking. Wanneer gegevens worden opgeslagen in Azure SQL Data Warehouse, kunt een grootschalige analyses uitvoeren.

Microsoft Azure biedt tal van services voor rapportage en analyse voor de klant. Deze oplossing bevat SQL Server Reporting Services (SSRS) voor het snel maken van rapporten van de Azure SQL Data Warehouse. Alle SQL-verkeer is versleuteld met SSL door de toevoeging van zelfondertekende certificaten. Als een best practice adviseert Azure het gebruik van een vertrouwde certificeringsinstantie voor een betere beveiliging.

Gegevens in Azure SQL Data Warehouse wordt opgeslagen in de relationele tabellen met kolommen opslag, een indeling die aanzienlijk minder de opslagkosten voor gegevens en verbetert de prestaties van query's.  Afhankelijk van de usage-vereisten voldoet, worden Azure SQL Data Warehouse rekenresources omhoog of omlaag geschaald of volledig afgesloten als er geen actieve processen rekenresources vereisen.

Een SQL-load balancer beheert SQL-verkeer, garanderen van hoge prestaties. Alle virtuele machines in deze verwijzende architectuur implementeren in een beschikbaarheidsset met SQL Server-exemplaren die zijn geconfigureerd in een AlwaysOn-beschikbaarheidsgroep voor hoge beschikbaarheid en herstel na noodgevallen mogelijkheden.

Deze verwijzende architectuur van de datawarehouse bevat ook een Active Directory (AD)-laag voor het beheren van resources in de architectuur. De Active Directory-subnet kunt gemakkelijk vaststelling volgens een groter geheel voor AD-forest, ononderbroken werking van de omgeving waardoor zelfs wanneer toegang tot de grotere forest niet beschikbaar is. Alle virtuele machines zijn met het domein is gekoppeld aan de laag Active Directory en Active Directory-groepsbeleid gebruiken om af te dwingen, beveiliging en naleving configuraties op het niveau van het besturingssysteem.

Een virtuele machine fungeert als een management bastion host, een beveiligde verbinding voor beheerders geïmplementeerd toegang tot bronnen te bieden. De gegevens laadt in het faseringsgebied via deze host van de bastionomgeving management. **Azure raadt aan om een VPN of Azure ExpressRoute-verbinding voor beheer en de gegevens importeren in het subnet van de architectuur verwijzing configureren.**

![Datawarehouse voor FedRAMP verwijzing Architectuurdiagram](images/fedramp-datawarehouse-architecture.png?raw=true "datawarehouse voor FedRAMP verwijzing Architectuurdiagram")

Deze oplossing gebruikt de volgende Azure-services. Details van de architectuur voor implementatie zijn de [architectuur voor implementatie](#deployment-architecture) sectie.

Azure Virtual Machines
-   (1) bastion-Host
-   (2) active Directory-domeincontroller
-   (2) de SQL Server-clusterknooppunt
-   (1) de SQL Server-Witness

Beschikbaarheidssets
-   (1) active Directory-domeincontrollers
-   (1) de SQL-clusterknooppunten en -witness

Virtual Network
-   (4) subnetten
-   (4) de Netwerkbeveiligingsgroepen

SQL Data Warehouse

SQL Server Reporting Services

Azure SQL-taakverdeler

Azure Active Directory

Recovery Services-kluis

Azure Key Vault

Operations Management Suite (OMS)

## <a name="deployment-architecture"></a>Architectuur voor implementatie

De volgende sectie wordt de ontwikkeling en implementatie-elementen.

**SQL Data Warehouse**: [SQL Data Warehouse](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-overview-what-is) is een Enterprise Data Warehouse (EDW) die gebruikmaakt van Massively parallelle verwerking MPP-snel uitvoeren van complexe query's voor petabytes aan gegevens. Big-gegevens importeren in SQL Data Warehouse met eenvoudige PolyBase T-SQL-query's en de kracht van MPP gebruiken voor het uitvoeren van analyses van hoge prestaties.

**SQL Server Reporting Services**: [SQL Server Reporting Services](https://docs.microsoft.com/sql/reporting-services/report-data/sql-azure-connection-type-ssrs) Hiermee kunt u snel maken van rapporten met tabellen, grafieken, kaarten, meters, matrices en meer voor Azure SQL Data Warehouse.

**Host van de bastionomgeving**: de host van de bastionomgeving is de aanspreekpunt waarmee gebruikers toegang krijgen tot de geïmplementeerde resources in deze omgeving. De host van de bastionomgeving biedt een beveiligde verbinding met de geïmplementeerde resources door alleen het externe verkeer van openbare IP-adressen toe te staan op een veilige lijst. Extern bureaublad (RDP)-verkeer toestaan, moet de bron van het verkeer in de Netwerkbeveiligingsgroep (NSG) worden gedefinieerd.

Een virtuele machine is gemaakt als een host bastion domein met de volgende configuraties:
-   [Antimalware-uitbreiding](https://docs.microsoft.com/azure/security/azure-security-antimalware)
-   [OMS-uitbreiding](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-oms)
-   [Azure extensie voor diagnostische gegevens](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template)
-   [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) met Azure Key Vault (respecteert Azure Government, PCI DSS, HIPAA en andere vereisten)
-   Een [automatisch afsluiten beleid](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) gebruik kan worden verkleind van bronnen voor virtuele machines wanneer deze niet in gebruik
-   [Windows Defender referentie Guard](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard) ingeschakeld zodat referenties en andere geheimen in een beveiligde omgeving die geïsoleerd van het besturingssysteem wordt uitgevoerd

### <a name="virtual-network"></a>Virtueel netwerk
Deze verwijzende architectuur definieert een persoonlijke virtueel netwerk met een adresruimte van 10.0.0.0/16.

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
De architectuur beveiligt de gegevens in rust via versleuteling, database, controle en andere maatregelen.

**Azure Storage** versleutelde data-at-rest-vereisten voldoen aan alle [Azure Storage](https://azure.microsoft.com/services/storage/) gebruikt [Opslagversleuteling Service](https://docs.microsoft.com/azure/storage/storage-service-encryption).

**Azure Disk Encryption**
[Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) maakt gebruik van de BitLocker-functie van Windows voor volumeversleuteling voor het besturingssysteem en gegevensschijven. De oplossing integreert met Azure Key Vault voor het beheren en beheren van de versleutelingssleutels op de schijf.

**Azure SQL Database** de Azure SQL Database-instantie gebruikt de volgende veiligheidsmaatregelen voor database:
-   [AD-verificatie en autorisatie](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) maakt identiteitsbeheer van databasegebruikers en andere Microsoft-services op één centrale locatie.
-   [SQL database auditing](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) houdt database gebeurtenissen en schrijft deze naar een auditlogboek Meld u bij een Azure storage-account.
-   SQL-Database is geconfigureerd voor gebruik [Transparent Data Encryption (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql), uitvoert, wordt er realtime versleuteling en ontsleuteling van gegevens en logboekbestanden om gegevens in rust te beveiligen. TDE biedt de zekerheid dat de opgeslagen gegevens niet is onderworpen aan onbevoegde toegang.
-   [Firewall-regels](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) voorkomen dat u alle toegang tot de database-servers, totdat de juiste machtigingen worden toegekend. De firewall verleent toegang tot databases op basis van het IP-adres waar de aanvraag vandaan komt.
-   [Detectie van dreigingen SQL](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) kunt u de detectie en het antwoord op mogelijke bedreigingen wanneer deze zich voordoen doordat beveiligingswaarschuwingen voor verdachte databaseactiviteiten, mogelijke beveiligingsproblemen, SQL-injectieaanvallen en toegang tot de afwijkende database patronen.
-   [Altijd versleutelde kolommen](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) ervoor te zorgen dat gevoelige gegevens nooit wordt weergegeven als leesbare tekst binnen het databasesysteem. Na het inschakelen van gegevensversleuteling alleen clienttoepassingen of appservers met toegang tot de sleutels hebben toegang tot gecodeerde gegevens.
-   [SQL-Database dynamische-gegevensmaskering](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) kunnen worden uitgevoerd nadat de referentiearchitectuur implementeert. Klanten moeten dynamische gegevensmaskering instellingen om te voldoen aan hun databaseschema aanpassen.

### <a name="business-continuity"></a>Bedrijfscontinuïteit
**Hoge beschikbaarheid**: Server-werkbelastingen worden gegroepeerd in een [Beschikbaarheidsset](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-manage-availability?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) om te zorgen voor hoge beschikbaarheid van virtuele machines in Azure. Ten minste één virtuele machine is beschikbaar tijdens gepland of ongepland onderhoud, voldoen aan de 99,95% Azure SLA.

**Recovery Services-kluis**: de [Recovery Services-kluis](https://docs.microsoft.com/azure/backup/backup-azure-recovery-services-vault-overview) -instellingen van de back-upgegevens en alle configuraties van Azure Virtual Machines in deze architectuur beveiligt. Met een Recovery Services-kluis klanten kunnen bestanden en mappen herstellen van een IaaS-VM zonder te herstellen van de hele virtuele machine, het inschakelen van sneller worden hersteld.

### <a name="logging-and-audit"></a>Logboekregistratie en controle
[Operations Management Suite (OMS)](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) biedt uitgebreide logboekregistratie van het systeem- en gebruikersactiviteit, evenals de systeemstatus. De OMS [logboekanalyse](https://azure.microsoft.com/services/log-analytics/) oplossing worden verzameld en analyseert gegevens die zijn gegenereerd voor resources in Azure en on-premises omgevingen.
- **Activiteitenlogboeken**: [activiteitenlogboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bieden inzicht in de bewerkingen die worden uitgevoerd op resources in een abonnement.
- **Diagnostische logboeken**: [diagnostische logboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) omvatten alle logboeken die door elke resource. Deze logboeken zijn Windows-gebeurtenislogboeken system en Azure Blob storage, tabellen en logboeken van de wachtrij.
- **Firewall-logboeken**: de toepassingsgateway biedt volledige diagnostische en toegang tot logboeken. Firewall-logboeken zijn beschikbaar voor Application Gateway WAF ingeschakeld resources.
- **Meld u archiveren**: alle logboeken met diagnostische gegevens schrijven naar een gecentraliseerd en versleutelde Azure storage-account voor archivering met een gedefinieerde bewaarperiode van 2 dagen. Deze logboeken verbinding maken met Azure Log Analytics voor verwerking, opslaan en dashboard reporting.

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

### <a name="identity-management"></a>Identiteitsbeheer
De volgende technologieën bieden identiteit beheermogelijkheden in de Azure-omgeving:
-   [Active Directory (AD)](https://azure.microsoft.com/services/active-directory/) van Microsoft multitenant cloud-gebaseerde directory en identity management-service kan zijn. Alle gebruikers voor de oplossing zijn gemaakt in Azure Active Directory, inclusief gebruikers met toegang tot de SQL-Database.
-   Verificatie van de toepassing wordt uitgevoerd met behulp van Azure AD. Zie voor meer informatie [toepassingen integreren met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Bovendien de versleuteling van de kolom database maakt gebruik van Azure AD om te verifiëren van de toepassing met Azure SQL Database. Zie voor meer informatie hoe [gevoelige gegevens in SQL-Database beveiligen](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   [Azure Active Directory: Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) mogelijke beveiligingsproblemen die invloed hebben op een organisatie-id's detecteert, configureert u automatische antwoorden op gedetecteerde verdachte acties met betrekking tot een organisatie-id's en onderzoekt verdachte incidenten, onderneem gepaste actie deze op te lossen.
-   [Azure op rollen gebaseerde toegangsbeheer (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) maakt gericht toegangsbeheer voor Azure. Abonnement toegang is beperkt tot abonnementsbeheerder.

Zie voor meer informatie over het gebruik van de beveiligingsfuncties van Azure SQL Database, de [Contoso kliniek Demo-toepassing](https://github.com/Microsoft/azure-sql-security-sample) voorbeeld.

### <a name="security"></a>Beveiliging
**Geheimen management**: de oplossing maakt gebruik van [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) voor het beheer van sleutels en geheimen. Met Azure Sleutelkluis kunt u de cryptografische sleutels en geheimen beveiligen die door cloudtoepassingen en -services worden gebruikt.

**Bescherming tegen schadelijke software**: [Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) voor virtuele Machines biedt de mogelijkheid dat helpt bij het identificeren en te verwijderen van virussen, spyware en andere schadelijke software, met configureerbare waarschuwingen van de realtime-beveiliging Wanneer bekende schadelijke of ongewenste software probeert te installeren of uitvoeren op de beveiligde virtuele machines.

**Beheer van patches**: Windows virtuele machines zijn geïmplementeerd als onderdeel van deze verwijzende architectuur zijn standaard geconfigureerd voor automatische updates ontvangen van Windows Update-Service. Deze oplossing bevat ook de OMS [Azure Automation](https://docs.microsoft.com/azure/automation/automation-intro) service waarmee bijgewerkte implementaties kunnen worden gemaakt met patch virtuele machines wanneer deze nodig is.


## <a name="guidance-and-recommendations"></a>Richtlijnen en aanbevelingen
### <a name="expressroute-and-vpn"></a>ExpressRoute- en VPN
[ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) of een beveiligde VPN-tunnel moet worden geconfigureerd voor het veilig verbinding maken met de resources die zijn geïmplementeerd als onderdeel van deze verwijzende architectuur van de datawarehouse. Als het ExpressRoute-verbindingen gaan niet via het Internet, bieden deze verbindingen meer betrouwbaarheid, sneller en hebben ze lagere latenties en betere beveiliging dan gewone verbindingen via Internet. Door op de juiste wijze instellen ExpressRoute of een VPN, kunnen klanten een laag van beveiliging voor gegevens onderweg toevoegen.

### <a name="extract-transform-load-etl-process"></a>Proces extract-transformatie-Load (ETL)
[PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) kunnen gegevens laden in Azure SQL Data Warehouse zonder de noodzaak van een afzonderlijke ETL of hulpprogramma voor importeren. PolyBase biedt toegang tot gegevens via een T-SQL-query's. Microsoft business intelligence en analyse stack, evenals hulpprogramma's van derden die compatibel is met SQL Server kunnen worden gebruikt met PolyBase.

### <a name="azure-active-directory-setup"></a>Installatie van de Azure Active Directory
[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) is essentieel voor het beheren van de implementatie en inrichting van toegang tot personeel interactie met de omgeving. Een bestaande Windows Server Active Directory kan worden geïntegreerd met AAD in [vier klikken](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-express). Klanten kunnen de geïmplementeerde Active Directory-infrastructuur (domeincontrollers) ook koppelen aan een bestaand AAD door het maken van de geïmplementeerde Active Directory-infrastructuur een subdomein van een AAD-forest.

### <a name="additional-services"></a>Extra services
Hoewel deze gegevens datawarehouse architectuur is niet bedoeld voor implementatie naar de [Azure commerciële](https://azure.microsoft.com/overview/what-is-azure/) omgeving, hetzelfde doel kunnen worden bereikt via de services die worden beschreven in deze verwijzende architectuur, evenals extra services is alleen beschikbaar in de Azure commerciële omgeving. Houd er rekening mee dat Azure commerciële een FedRAMP JAB P-ATO op het niveau van matige Impact onderhoudt, overheidsinstanties en partners matig gevoelige informatie naar de cloud gebruik van de Azure commerciële omgeving implementeren.

Azure commerciële biedt die een groot aantal verschillende services die ingang geformatteerd en opslag van niet-opgemaakte gegevens en fasering moet worden gebruikt in de gegevensopslag, met inbegrip van:
-   [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/introduction) is een beheerde cloudservice die is gebouwd voor complexe hybride extract-transformatie-load (ETL) extract load transformatie (ELT) en gegevens integratie-projecten. Met behulp van Azure Data Factory, kunnen klanten maken en plannen van gegevensgestuurde werkstromen aangeroepen pijplijnen opnemen van gegevens uit verschillende gegevensarchieven. Klanten kunnen vervolgens verwerken en de gegevens voor uitvoer transformeren naar gegevensarchieven zoals Azure SQL Data Warehouse.
-   [Azure Data Lake Store](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-overview) kunt u het vastleggen van gegevens van elke grootte, type en opnamesnelheid snelheid in een enkele locatie voor operationele en experimentele analyses. Azure Data Lake Store is compatibel met de meeste open source-onderdelen in het Hadoop-ecosysteem en mooi is geïntegreerd met andere Azure-services zoals Azure SQL Data Warehouse.

## <a name="threat-model"></a>Risicomodel

De gegevensstroom-diagram (GSD) voor deze verwijzende architectuur is beschikbaar voor [downloaden](https://aka.ms/blueprintdwthreatmodel) of vindt u hieronder:

![Datawarehouse voor risicomodel FedRAMP](images/fedramp-datawarehouse-threat-model.png?raw=true "datawarehouse voor FedRAMP risicomodel")

## <a name="compliance-documentation"></a>Documentatie voor naleving
De [Azure-beveiliging en naleving blauwdruk – FedRAMP hoge klant verantwoordelijkheid Matrix](https://aka.ms/blueprinthighcrm) geeft een lijst van alle beveiligingsmechanismen zijn vereist voor de basislijn FedRAMP hoog. Op dezelfde manier de [Azure-beveiliging en naleving blauwdruk – FedRAMP gemiddeld klant verantwoordelijkheid Matrix](https://aka.ms/blueprintcrmmod) geeft een lijst van alle beveiligingsmechanismen zijn vereist voor de basislijn FedRAMP matig. Beide documenten beschreven of de implementatie van elk besturingselement de verantwoordelijkheid van Microsoft, de klant is of gedeeld tussen de twee.

De [Azure-beveiliging en naleving blauwdruk - FedRAMP hoge besturingselement implementatie Matrix](https://aka.ms/blueprintdwcimhigh) en de [Azure-beveiliging en naleving blauwdruk - FedRAMP gemiddeld besturingselement implementatie Matrix](https://aka.ms/blueprintdwcimmod) bieden informatie waarop besturingselementen worden gedekt door de datawarehouse-architectuur van de gegevens voor elke basislijn FedRAMP, met inbegrip van gedetailleerde beschrijvingen van de manier waarop de implementatie voldoet aan de vereisten van elk gedekte besturingselement.

## <a name="disclaimer"></a>Vrijwaring

 - Dit document is alleen ter informatie. MICROSOFT GEEFT GEEN GARANTIES, SNELLE, IMPLICIETE OF WETTELIJKE GARANTIE VOOR DE INFORMATIE IN DIT DOCUMENT. Dit document wordt geleverd ' as-is. " Informatie en inzichten die in dit document, inclusief URL's en andere websiteverwijzingen, kunnen zonder kennisgeving worden gewijzigd. Dit document lezen klanten draagt het risico voor het gebruik ervan.
 - Dit document biedt geen enkel wettelijk recht op enig intellectueel eigendom van alle Microsoft-product of oplossingen klanten.
 - Klanten kunnen kopiëren en gebruiken van dit document voor interne referentiedoeleinden.
 - Bepaalde aanbevelingen in dit document kunnen leiden tot hogere-, netwerk- of compute-Resourcegebruik in Azure en een klant Azure licentie of abonnement kosten kunnen verhogen.
 - Deze architectuur is bedoeld om te fungeren als basis voor klanten om aan te passen aan hun specifieke vereisten en mag niet worden gebruikt als-is in een productieomgeving.
 - Dit document is ontwikkeld als een verwijzing en mag niet worden gebruikt voor het definiëren van alle middelen waarmee een klant kan voldoen aan specifieke nalevingsvereisten en -voorschriften. Klanten moeten juridische ondersteuning van hun organisatie met goedgekeurde klant implementaties zoeken.
