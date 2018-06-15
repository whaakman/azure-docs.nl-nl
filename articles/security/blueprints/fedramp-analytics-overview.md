---
title: Azure-beveiliging en naleving blauwdruk - analyse voor FedRAMP
description: Azure-beveiliging en naleving blauwdruk - analyse voor FedRAMP
services: security
author: jomolesk
ms.assetid: b4675715-668e-4557-9c1c-698aabf62ceb
ms.service: security
ms.topic: article
ms.date: 05/02/2018
ms.author: jomolesk
ms.openlocfilehash: db9e49cc4dc02b6864bee2dc4b73ff3c085f5380
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/04/2018
ms.locfileid: "33206885"
---
# <a name="azure-security-and-compliance-blueprint-analytics-for-fedramp"></a>Azure-beveiliging en naleving blauwdruk: Analytics voor FedRAMP

## <a name="overview"></a>Overzicht

De [Federal risico en autorisatie Management-programma (FedRAMP)](https://www.fedramp.gov/) is een Verenigde Staten government wide programma waarmee een gestandaardiseerde aanpak voor beoordeling van de veiligheid, autorisatie en doorlopende bewaking voor cloud producten en services. Deze Azure-beveiliging en naleving blauwdruk biedt richtlijnen voor het leveren van een Microsoft Azure analytics-architectuur waarmee een subset van hoge FedRAMP besturingselementen implementeren. Deze oplossing biedt richtlijnen voor de implementatie en configuratie van Azure-resources voor een algemene referentiearchitectuur manieren waarin klanten kunnen voldoen aan specifieke vereisten voor beveiliging en naleving te demonstreren en fungeert als basis voor klanten bouw en hun eigen analyseoplossingen configureren in Azure.

Deze referentiearchitectuur, gekoppelde besturingselement implementatiehandleidingen en threat modellen zijn bedoeld om te fungeren als basis voor klanten om aan te passen aan hun specifieke vereisten en mag niet worden gebruikt als-is in een productieomgeving. Een toepassing in deze omgeving zonder aanpassingen implementeren is onvoldoende om volledig te voldoen aan de vereisten van de basislijn FedRAMP hoog. Houd rekening met het volgende:
- De architectuur biedt een basis zodat klanten werkbelastingen in een FedRAMP-compatibele manier implementeren in Azure.
- Klanten zijn verantwoordelijk voor het uitvoeren van de juiste beveiligingsrechten en beoordeling van naleving van een oplossing die zijn gebouwd met behulp van deze architectuur als vereisten kunnen variëren op basis van de details van de implementatie van de klant.

## <a name="architecture-diagram-and-components"></a>Architectuurdiagram en onderdelen

Deze oplossing biedt een analytics platform waarop klanten hun eigen analytics-hulpprogramma's kunnen maken. De referentiearchitectuur bevat een overzicht van een algemene gebruiksvoorbeeld waar klanten via bulkimport gegevens door de beheerder van SQL-gegevens of door middel van operationele Gegevensupdates via een gebruiker met operationele gegevens invoeren. Beide werken streams opnemen [Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-overview) voor het importeren van gegevens in de SQL-Database. Azure Functions moeten worden geconfigureerd door de klant via de Azure-Portal voor het afhandelen van de taken importeren uniek voor elke klant van eigen analytics vereisten.

Microsoft Azure biedt tal van services voor rapportage en analyse voor de klant. Deze oplossing bevat echter Azure Analysis Services in combinatie met Azure SQL Database snel bladeren door de gegevens en snellere resultaten via slimmer modellering van klantgegevens leveren. Azure Analytics-Services is een vorm van machine learning bedoeld query snelheden verbeteren door het detecteren van nieuwe relaties tussen gegevenssets. Nadat de gegevens is getraind via verschillende statistische functies, tot 7 extra query kunnen pools (totale met inbegrip van de klantserver 8) worden gesynchroniseerd met de dezelfde modellen in tabelvorm om verspreiden workload van query's en responstijden te verminderen.

Voor geavanceerde analyse en rapportage, kan de SQL-Databases worden geconfigureerd met columnstore-indexen. Zowel Azure Analytics-Services en SQL-Databases kunnen worden geschaald omhoog of omlaag of volledig afgesloten in reactie op gebruik van de klant. Alle SQL-verkeer is versleuteld met SSL door de toevoeging van zelfondertekende certificaten. Als een best practice adviseert Azure het gebruik van een vertrouwde certificeringsinstantie voor een betere beveiliging.

Wanneer gegevens worden geüpload naar de Azure SQL Database en getraind door Azure Analysis Services, is het verwerkte door de operationele gebruiker en de beheerder van de SQL-gegevens met Power BI. Power BI intuïtief gegevens worden weergegeven en informatie verzamelt over meerdere gegevenssets beter inzicht wordt getekend. De hoge mate van aanpassingsvermogen en eenvoudige integratie met Azure SQL Database zorgt ervoor dat klanten configureren kunnen voor het afhandelen van een breed scala aan scenario's zoals vereist door hun zakelijke behoeften.

De volledige oplossing is gebaseerd op een Azure Storage dat klanten account configureert vanuit de Azure Portal. Azure Storage versleutelt alle gegevens met Storage-Service Encryption vertrouwelijkheid van gegevens in rust te waarborgen.  Geografisch redundante opslag (GRS) zorgt ervoor dat een nadelige gebeurtenis in het primaire datacenter van de klant niet in een verlies van gegevens resulteren zal als een tweede kopie wordt opgeslagen in een andere locatie honderden van mijl verwijderd.

Deze architectuur beheert voor een betere beveiliging resources met Azure Active Directory en Azure Sleutelkluis. Systeemstatus wordt bewaakt door Operations Management Suite (OMS) en Azure-Monitor. Klanten configureren beide bewaking services logboeken vastleggen en systeemstatus wordt weergegeven in een enkele, gemakkelijk navigeerbaar dashboard.

Azure SQL-Database wordt meestal beheerd via SQL Server Management Studio (SSMS), die wordt uitgevoerd van een lokale machine is geconfigureerd voor toegang tot de Azure SQL Database via een veilige VPN- of ExpressRoute-verbinding. **Azure raadt het configureren van een VPN of Azure ExpressRoute-verbinding voor beheer en de gegevens importeren in de resourcegroep van de referentie-architectuur.**

![Analytics voor FedRAMP verwijzing Architectuurdiagram](images/fedramp-analytics-reference-architecture.png?raw=true "Analytics voor FedRAMP reference architecture-diagram")

### <a name="roles"></a>Rollen
De blauwdruk analytics bevat een overzicht van een scenario met drie algemene gebruikerstypen: de operationele gebruiker, de beheerder van de SQL-gegevens en de Engineer systemen. Azure op rollen gebaseerde toegangsbeheer (RBAC) kunt de implementatie van nauwkeurige access management via ingebouwde aangepaste rollen. Bronnen beschikbaar zijn voor het configureren van [toegangsbeheer op basis van rollen](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) en een overzicht maken en implementeren van [voorgedefinieerde rollen](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles).

#### <a name="systems-engineer"></a>Systemen engineering
De Engineer systemen is eigenaar van de klantabonnement voor Azure en configureert de implementatie van de oplossing via de Azure-Portal.

#### <a name="sqldata-administrator"></a>De beheerder van de SQL-gegevens
De beheerder van de SQL-gegevens worden ook de functie bulksgewijs gegevens importeren en de functie voor het bijwerken van operationele gegevens voor het uploaden naar de Azure SQL database. De beheerder van de SQL-gegevens is niet verantwoordelijk voor eventuele updates van operationele gegevens in de database, maar kunnen de gegevens via Power BI weergeven.

#### <a name="operational-user"></a>Operationele gebruiker
De gegevens worden regelmatig updates en het de operationele gebruiker eigenaar is van het genereren van de dagelijkse gegevens. De operationele gebruiker zullen ook resultaten via Power BI interpreteren.

### <a name="azure-services"></a>Azure-services

Deze oplossing gebruikt de volgende Azure-services. Details van de architectuur voor implementatie zijn de [architectuur voor implementatie](#deployment-architecture) sectie.
- Azure Functions
- Azure SQL Database
- Azure Analysis Services
- Azure Active Directory
- Azure Key Vault
- OMS
- Azure Monitor
- Azure Storage
- ExpressRoute/VPN-Gateway
- Power BI Dashboard

## <a name="deployment-architecture"></a>Architectuur voor implementatie
De volgende sectie wordt de ontwikkeling en implementatie-elementen.

![alternatieve tekst](images/fedramp-analytics-components.png?raw=true "Analytics voor FedRAMP onderdelen diagram")

**Azure Functions**: [Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-overview) oplossingen voor het uitvoeren van kleine stukjes code in de cloud via de meeste programmeertalen. Functies in deze oplossing integreren met Azure Storage voor het automatisch ophalen van gegevens van de klant in de cloud om integratie met andere Azure-services te vergemakkelijken. Functies zijn eenvoudig schaalbare en alleen kosten in rekening gebracht een wanneer ze worden uitgevoerd.

**Azure Analysis Service**: [Azure Analysis Services](https://docs.microsoft.com/azure/analysis-services/analysis-services-overview) enterprise gegevens modelleren en integratie met Azure data platformservices biedt. Azure Analysis Service sneller bladeren door de enorme hoeveelheden gegevens door het combineren van gegevens uit meerdere bronnen in een één-gegevensmodel.

**Power BI**: [Power BI](https://docs.microsoft.com/power-bi/service-azure-and-power-bi) voorziet in analyse en rapportage van de functionaliteit voor klanten probeert om beter inzicht buiten hun inspanningen gegevensverwerking binnen te halen.

### <a name="networking"></a>Netwerken
**Netwerkbeveiligingsgroepen**: [nsg's](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) zijn ingesteld om verkeer die gericht zijn op de geïmplementeerde resources en services te beheren. Netwerkbeveiligingsgroepen worden ingesteld op weigeren door standaardschema en alleen dat verkeer dat is opgenomen in de vooraf geconfigureerde lijst ACL (Access Control).

Elk van de nsg's een bepaalde poorten en protocollen open zodat de oplossing kunt veilig en correct werken. Bovendien zijn de volgende configuraties voor elke NSG ingeschakeld:
  - [Logboeken met diagnostische gegevens en gebeurtenissen](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) zijn ingeschakeld en opgeslagen in een opslagaccount
  - OMS [logboekanalyse](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-networking-analytics) is verbonden met het NSG diagnostische logboeken.

### <a name="data-at-rest"></a>Data-at-rest
De architectuur beveiligt de gegevens in rust via versleuteling, database, controle en andere maatregelen.

**Gegevensreplicatie** Azure Government heeft twee opties voor [gegevensreplicatie](https://docs.microsoft.com/azure/storage/common/storage-redundancy):
 - De standaardinstelling voor gegevensreplicatie is **geografisch redundante opslag (GRS)**, waarin gegevens van de klant asynchroon wordt opgeslagen in een afzonderlijke Datacenter buiten de primaire regio. Dit zorgt ervoor dat het herstellen van gegevens in een totale verlies-gebeurtenis voor het primaire Datacenter.
 - **Lokaal redundante opslag (LRS)** kunnen ook worden geconfigureerd via de Azure Storage-Account. LRS repliceert gegevens binnen een schaaleenheid opslag die wordt gehost in dezelfde regio waarin door de klant hun account wordt gemaakt. Alle gegevens worden gerepliceerd gelijktijdig, waarbij u ervoor zorgt dat er geen back-upgegevens gaat verloren in een primaire scale unit optreedt.

**Azure Storage** om te voldoen aan de versleutelde gegevens op de rest-vereisten, alle services die zijn geïmplementeerd in deze architectuur verwijzing hefboomwerking [Azure Storage](https://azure.microsoft.com/services/storage/), waarin wordt opgeslagen gegevens met [Service Opslagversleuteling](https://docs.microsoft.com/azure/storage/storage-service-encryption).

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

### <a name="logging-and-audit"></a>Logboekregistratie en controle
[Monitor voor Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-get-started) genereert een all-up-weergave van bewakingsgegevens inclusief activiteitenlogboeken, meetgegevens en diagnostische gegevens, waardoor klanten een volledig overzicht van de systeemstatus te maken.  
[Operations Management Suite (OMS)](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) biedt uitgebreide logboekregistratie van het systeem- en gebruikersactiviteit, evenals de systeemstatus. De OMS [logboekanalyse](https://azure.microsoft.com/services/log-analytics/) oplossing worden verzameld en analyseert gegevens die zijn gegenereerd voor resources in Azure en on-premises omgevingen.
- **Activiteitenlogboeken**: [activiteitenlogboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bieden inzicht in de bewerkingen die worden uitgevoerd op resources in een abonnement.
- **Diagnostische logboeken**: [diagnostische logboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) omvatten alle logboeken die door elke resource. Deze logboeken zijn Windows-gebeurtenislogboeken system en Azure Blob storage, tabellen en logboeken van de wachtrij.
- **Firewall-logboeken**: de toepassingsgateway biedt volledige diagnostische en toegang tot logboeken. Firewall-logboeken zijn beschikbaar voor Application Gateway WAF ingeschakeld resources.
- **Meld u archiveren**: alle logboeken met diagnostische gegevens schrijven naar een gecentraliseerd en versleutelde Azure storage-account voor archivering met een gedefinieerde bewaarperiode van 2 dagen. Deze logboeken verbinding maken met Azure Log Analytics voor verwerking, opslaan en dashboard reporting.

Bovendien zijn de volgende OMS-oplossingen opgenomen als onderdeel van deze architectuur:
-   [Azure Automation](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker): de Azure Automation-oplossing opgeslagen, wordt uitgevoerd en runbooks beheert.
-   [Beveiliging en Audit](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started): de beveiligings- en Audit dashboard biedt een op hoog niveau inzicht in de beveiligingsstatus van bronnen door te geven van metrische gegevens over beveiligingsdomeinen, problemen die aandacht vereisen, detecties, dreigingen en algemene beveiliging query's.
-   [SQL-evaluatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): de SQL-Serverstatus controleren oplossing evalueert de risico's en de gezondheid van server-omgevingen met regelmatige tussenpozen en biedt klanten een geprioriteerde lijst met aanbevelingen die specifiek zijn voor de geïmplementeerde serverinfrastructuur.
-   [Azure activiteitenlogboeken](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): de activiteit Log Analytics-oplossing biedt ondersteuning voor analyse van de Azure activiteitenlogboeken over alle Azure-abonnementen voor een klant.

### <a name="identity-management"></a>Identiteitsbeheer
-   Verificatie van de toepassing wordt uitgevoerd met behulp van Azure AD. Zie voor meer informatie [toepassingen integreren met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Bovendien de versleuteling van de kolom database maakt gebruik van Azure AD om te verifiëren van de toepassing met Azure SQL Database. Zie voor meer informatie hoe [gevoelige gegevens in SQL-Database beveiligen](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   [Azure Active Directory: Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) mogelijke beveiligingsproblemen die invloed hebben op een organisatie-id's detecteert, configureert u automatische antwoorden op gedetecteerde verdachte acties met betrekking tot een organisatie-id's en onderzoekt verdachte incidenten, onderneem gepaste actie deze op te lossen.
-   [Azure op rollen gebaseerde toegangsbeheer (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) maakt gericht toegangsbeheer voor Azure. Abonnement toegang is beperkt tot abonnementsbeheerder.

Zie voor meer informatie over het gebruik van de beveiligingsfuncties van Azure SQL Database, de [Contoso kliniek Demo-toepassing](https://github.com/Microsoft/azure-sql-security-sample) voorbeeld.

### <a name="security"></a>Beveiliging
**Geheimen management**: de oplossing maakt gebruik van [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) voor het beheer van sleutels en geheimen. Met Azure Sleutelkluis kunt u de cryptografische sleutels en geheimen beveiligen die door cloudtoepassingen en -services worden gebruikt.

## <a name="guidance-and-recommendations"></a>Richtlijnen en aanbevelingen

### <a name="expressroute-and-vpn"></a>ExpressRoute- en VPN
[ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) of een beveiligde VPN-tunnel moet worden geconfigureerd voor het veilig verbinding maken met de resources die zijn geïmplementeerd als onderdeel van deze gegevens analytics verwijzende architectuur. Als het ExpressRoute-verbindingen gaan niet via het Internet, bieden deze verbindingen meer betrouwbaarheid, sneller en hebben ze lagere latenties en betere beveiliging dan gewone verbindingen via Internet. Door op de juiste wijze instellen ExpressRoute of een VPN, kunnen klanten een laag van beveiliging voor gegevens onderweg toevoegen.

### <a name="azure-active-directory-setup"></a>Installatie van de Azure Active Directory
[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) is essentieel voor het beheren van de implementatie en inrichting van toegang tot personeel interactie met de omgeving. Een bestaande Windows Server Active Directory kan worden geïntegreerd met AAD in [vier klikken](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-express). Klanten kunnen de geïmplementeerde Active Directory-infrastructuur (domeincontrollers) ook koppelen aan een bestaand AAD door het maken van de geïmplementeerde Active Directory-infrastructuur een subdomein van een AAD-forest.

### <a name="additional-services"></a>Extra services
#### <a name="iaas---vm-vonsiderations"></a>IaaS - VM vonsiderations
Een Azure IaaS VM's zelf niet worden opgenomen in deze PaaS-oplossing. Een klant kan een virtuele machine van Azure om uit te voeren veel van deze PaaS-services maken. In dit geval kunnen specifieke functies en services voor bedrijfscontinuïteit en OMS worden gebruikt:

##### <a name="business-continuity"></a>Bedrijfscontinuïteit
- **Hoge beschikbaarheid**: Server-werkbelastingen worden gegroepeerd in een [Beschikbaarheidsset](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-manage-availability?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) om te zorgen voor hoge beschikbaarheid van virtuele machines in Azure. Ten minste één virtuele machine is beschikbaar tijdens gepland of ongepland onderhoud, voldoen aan de 99,95% Azure SLA.

- **Recovery Services-kluis**: de [Recovery Services-kluis](https://docs.microsoft.com/azure/backup/backup-azure-recovery-services-vault-overview) -instellingen van de back-upgegevens en alle configuraties van Azure Virtual Machines in deze architectuur beveiligt. Met een Recovery Services-kluis klanten kunnen bestanden en mappen herstellen van een IaaS-VM zonder te herstellen van de hele virtuele machine, het inschakelen van sneller worden hersteld.

##### <a name="oms"></a>OMS
-   [AD-evaluatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): het Active Directory Health Check oplossing evalueert de risico's en de gezondheid van server-omgevingen met regelmatige tussenpozen en biedt een geprioriteerde lijst met aanbevelingen die specifiek zijn voor de geïmplementeerde serverinfrastructuur.
-   [Antimalware Assessment](https://docs.microsoft.com/azure/log-analytics/log-analytics-malware): de anti-malwareoplossing rapporten over de status van schadelijke software, bedreigingen en beveiliging.
-   [Updatebeheer](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-update-management): oplossing voor het beheer van de Update kunt u beveiligingsupdates besturingssysteem, met inbegrip van de status van de beschikbare updates en het proces van het installeren van vereiste updates klant beheren.
-   [Status van agent](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): oplossing de status van de Agent rapporteert hoeveel agents zijn geïnstalleerd en hun geografische verdeling, evenals hoeveel agents die niet reageert en het aantal agents dat operationele gegevens wordt verzonden.
-   [Het bijhouden van](https://docs.microsoft.com/azure/automation/automation-change-tracking): de bijhouden oplossing kan klanten eenvoudig om wijzigingen te bepalen in de omgeving.

##### <a name="security"></a>Beveiliging
- **Bescherming tegen schadelijke software**: [Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) voor virtuele Machines biedt de mogelijkheid dat helpt bij het identificeren en te verwijderen van virussen, spyware en andere schadelijke software, met configureerbare waarschuwingen van de realtime-beveiliging Wanneer bekende schadelijke of ongewenste software probeert te installeren of uitvoeren op de beveiligde virtuele machines.
- **Beheer van patches**: Windows virtuele machines zijn geïmplementeerd als onderdeel van deze verwijzende architectuur zijn standaard geconfigureerd voor automatische updates ontvangen van Windows Update-Service. Deze oplossing bevat ook de OMS [Azure Automation](https://docs.microsoft.com/azure/automation/automation-intro) service waarmee bijgewerkte implementaties kunnen worden gemaakt met patch virtuele machines wanneer deze nodig is.

#### <a name="azure-commercial"></a>Azure commercieel
Hoewel deze gegevens analytics architectuur is niet bedoeld voor implementatie naar de [Azure commerciële](https://azure.microsoft.com/overview/what-is-azure/) omgeving, hetzelfde doel kunnen worden bereikt via de services die worden beschreven in deze verwijzende architectuur, evenals extra services is alleen beschikbaar in de Azure commerciële omgeving. Houd er rekening mee dat Azure commerciële een FedRAMP JAB P-ATO op het niveau van matige Impact onderhoudt, overheidsinstanties en partners matig gevoelige informatie naar de cloud gebruik van de Azure commerciële omgeving implementeren.

Azure commercieel biedt een groot aantal analytics-services voor het ophalen van insights buiten grote hoeveelheden gegevens:
-   [Azure Machine Learning Studio](https://docs.microsoft.com/azure/machine-learning/studio/what-is-ml-studio), een onderdeel van de [Cortana Intelligence Suite](https://azure.microsoft.com/overview/ai-platform/), ontwikkelt u een predictive Analytics-model uit een of meer gegevensbronnen. Statistische functies worden gebruikt via verschillende iteraties voor het genereren van een effectieve model dat toepassingen zoals Power BI vervolgens kunnen gebruiken.
-   [Azure Data Lake Store](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-overview) kunt u het vastleggen van gegevens van elke grootte, type en opnamesnelheid snelheid in een enkele locatie voor operationele en experimentele analyses. Azure Data Lake Store is compatibel met de meeste open source-onderdelen in het Hadoop-ecosysteem en mooi is geïntegreerd met andere Azure-services.

## <a name="threat-model"></a>Risicomodel

De gegevensstroom-diagram (GSD) voor deze verwijzende architectuur is beschikbaar voor [downloaden](https://aka.ms/blueprintanalyticsthreatmodel) of vindt u hieronder:

## <a name="compliance-documentation"></a>Documentatie voor naleving
De [Azure-beveiliging en naleving blauwdruk – FedRAMP hoge klant verantwoordelijkheid Matrix](https://aka.ms/blueprinthighcrm) geeft een lijst van alle beveiligingsmechanismen zijn vereist voor de basislijn FedRAMP hoog. Op dezelfde manier de [Azure-beveiliging en naleving blauwdruk – FedRAMP gemiddeld klant verantwoordelijkheid Matrix](https://aka.ms/blueprintanalyticscimmod) geeft een lijst van alle beveiligingsmechanismen zijn vereist voor de basislijn FedRAMP matig. Beide documenten beschreven of de implementatie van elk besturingselement de verantwoordelijkheid van Microsoft, de klant is of gedeeld tussen de twee.

De [Azure-beveiliging en naleving blauwdruk - FedRAMP hoge besturingselement implementatie Matrix](https://aka.ms/blueprintanalyticscimhigh) en de [Azure-beveiliging en naleving blauwdruk - FedRAMP gemiddeld besturingselement implementatie Matrix](https://aka.ms/blueprintanalyticscimmod) bieden informatie waarop besturingselementen worden gedekt door de analytics-architectuur voor elke basislijn FedRAMP, met inbegrip van gedetailleerde beschrijvingen van de manier waarop de implementatie voldoet aan de vereisten van elk gedekte besturingselement.

## <a name="disclaimer"></a>Vrijwaring

 - Dit document is alleen ter informatie. MICROSOFT GEEFT GEEN GARANTIES, SNELLE, IMPLICIETE OF WETTELIJKE GARANTIE VOOR DE INFORMATIE IN DIT DOCUMENT. Dit document wordt geleverd ' as-is. " Informatie en inzichten die in dit document, inclusief URL's en andere websiteverwijzingen, kunnen zonder kennisgeving worden gewijzigd. Dit document lezen klanten draagt het risico voor het gebruik ervan.
 - Dit document biedt geen enkel wettelijk recht op enig intellectueel eigendom van alle Microsoft-product of oplossingen klanten.
 - Klanten kunnen kopiëren en gebruiken van dit document voor interne referentiedoeleinden.
 - Bepaalde aanbevelingen in dit document kunnen leiden tot hogere-, netwerk- of compute-Resourcegebruik in Azure en een klant Azure licentie of abonnement kosten kunnen verhogen.
 - Deze architectuur is bedoeld om te fungeren als basis voor klanten om aan te passen aan hun specifieke vereisten en mag niet worden gebruikt als-is in een productieomgeving.
 - Dit document is ontwikkeld als een verwijzing en mag niet worden gebruikt voor het definiëren van alle middelen waarmee een klant kan voldoen aan specifieke nalevingsvereisten en -voorschriften. Klanten moeten juridische ondersteuning van hun organisatie met goedgekeurde klant implementaties zoeken.
