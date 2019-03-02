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
ms.openlocfilehash: e3bfc3d0f444bece0afe7b7f5bcdac343a693a13
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2019
ms.locfileid: "57243700"
---
# <a name="azure-security-and-compliance-blueprint-analytics-for-fedramp"></a>Azure-beveiliging en naleving blauwdruk: Analytics voor FedRAMP

## <a name="overview"></a>Overzicht

De [Federal Risk and Authorization Management Program (FedRAMP)](https://www.fedramp.gov/) Verenigde Staten government-is een programma is dat een gestandaardiseerde benadering voor de beveiligingsbeoordeling, de autorisatie en voortdurende bewaking biedt voor cloud producten en services. Deze Azure-beveiliging en naleving blauwdruk biedt richtlijnen voor het leveren van een Microsoft Azure analytics-architectuur waarmee een subset van FedRAMP High besturingselementen implementeren. Deze oplossing biedt richtlijnen voor de implementatie en configuratie van Azure-resources voor een algemene referentiearchitectuur manieren waarin klanten specifieke vereisten voor beveiliging en naleving kunnen voldoen aan te tonen en fungeert als een basis voor klanten bouw en hun eigen analytics-oplossingen in Azure te configureren.

Deze referentiearchitectuur, bijbehorende besturingselement implementatiehandleidingen en bedreigingsmodellen zijn bedoeld om te fungeren als een basis voor klanten om aan te passen op hun specifieke behoeften en mag niet worden gebruikt als-is in een productieomgeving. Er is onvoldoende om volledig te voldoen aan de vereisten van de basislijn FedRAMP High implementeren van een toepassing in deze omgeving zonder aanpassingen. Houd rekening met het volgende:
- De architectuur biedt een basislijn om workloads naar Azure te implementeren op een manier die compatibel is met FedRAMP klanten te helpen.
- Klanten zijn verantwoordelijk voor het uitvoeren van de juiste beveiliging en naleving evaluaties van een oplossing die is gebouwd met behulp van deze architectuur als vereisten kunnen variëren op basis van de details van de uitvoering van elke klant.

## <a name="architecture-diagram-and-components"></a>Diagram van architectuur en onderdelen

Deze oplossing biedt een platform voor streaminganalyse waarop klanten hun eigen analysehulpmiddelen kunnen bouwen. De referentiearchitectuur bevat een overzicht van een algemene use-case waar de gegevens via bulkimport van gegevens door de SQL-gegevens-beheerder of via updates van operationele gegevens via een operationele gebruiker voor het invoeren van klanten. Beide opgenomen streams werken [Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-overview) voor het importeren van gegevens in de SQL-Database. Azure Functions moeten worden geconfigureerd door de klant via de Azure-Portal voor het afhandelen van de importtaken uniek is voor elk van de klant eigen analytics vereisten.

Microsoft Azure biedt tal van services voor rapportage en analyse voor de klant. Deze oplossing bevat echter Azure Analysis Services in combinatie met Azure SQL Database om snel door gegevens bladeren en snellere resultaten via slimmer modelleren van gegevens van de klant. Azure Analytics-Services is een vorm van machine learning is bedoeld om te verhogen query snelheden worden bereikt door het detecteren van nieuwe relaties tussen gegevenssets. Zodra de gegevens is getraind via verschillende statistische functies, maximaal 7 extra query kunnen pools (8 totale met inbegrip van de klantserver) worden gesynchroniseerd met de dezelfde tabellaire modellen om werkbelasting van query's te verspreiden en responstijden te verminderen.

Voor geavanceerde analyse en rapportage, kan de SQL-Databases worden geconfigureerd met columnstore-indexen. Zowel analyseservices van Azure en SQL-Databases kan omhoog of omlaag geschaald of volledig afgesloten in reactie op klanten te worden gebruikt. Alle SQL-verkeer wordt versleuteld met SSL door de toevoeging van zelfondertekende certificaten. Als een best practice raadt Azure het gebruik van een vertrouwde certificeringsinstantie voor verbeterde beveiliging.

Wanneer gegevens worden geüpload naar de Azure SQL Database en getraind door Azure Analysis Services, is het ontsloten door zowel de operationele gebruiker en de beheerder van de SQL-gegevens met Power BI. Power BI gegevens intuïtief worden weergegeven en informatie verzamelt over meerdere gegevenssets te tekenen meer inzicht krijgen. De hoge mate van aanpasbaarheid en eenvoudige integratie met Azure SQL Database zorgt ervoor dat klanten configureren kunnen voor het afhandelen van een breed scala aan scenario's zoals vereist door de zakelijke behoeften.

De gehele oplossing is gebaseerd op een Azure-opslag waarmee klanten account configureren vanuit de Azure-Portal. Azure Storage worden alle gegevens met Storage Service Encryption voor het onderhouden van de vertrouwelijkheid van gegevens in rust versleuteld.  Geografisch redundante opslag (GRS) zorgt ervoor dat een negatieve gebeurtenis in het primaire datacenter van de klant niet in een verlies van gegevens resulteren zal als een tweede exemplaar worden opgeslagen op een afzonderlijke locatie honderden van mijl weg.

Deze architectuur beheert voor een betere beveiliging resources met Azure Active Directory en Azure Key Vault. Status van het bestandssysteem wordt bewaakt met Azure Monitor. Klanten configureren beide bewakingsservices voor het vastleggen van Logboeken en status van het bestandssysteem in een enkele, eenvoudig navigeerbaar dashboard weer te geven.

Azure SQL-Database wordt meestal beheerd via SQL Server Management Studio (SSMS), die wordt uitgevoerd op een lokale machine is geconfigureerd voor toegang tot de Azure SQL Database via een beveiligde VPN of ExpressRoute-verbinding. **Azure wordt aanbevolen voor het configureren van een VPN of Azure ExpressRoute-verbinding voor het beheer en de gegevens importeren in de resourcegroep van de referentie-architectuur.**

![Analytics voor FedRAMP verwijzing Architectuurdiagram](images/fedramp-analytics-reference-architecture.png?raw=true "Analytics voor Architectuurdiagram van FedRAMP-verwijzing")

### <a name="roles"></a>Rollen
De blauwdruk analytics bevat een overzicht van een scenario met drie algemene gebruikerstypen: de operationele gebruiker, de beheerder van de SQL-gegevens en de engineering-systemen. Azure Role-based Access Control (RBAC) kunt de implementatie van nauwkeurig toegangsbeheer via de ingebouwde aangepaste rollen. Resources zijn beschikbaar voor het configureren van [Role-based Access Control](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) en overzichten en implementeren van [vooraf gedefinieerde rollen](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles).

#### <a name="systems-engineer"></a>Systeemengineer
De Systeemengineer eigenaar is van de klantabonnement voor Azure en configureert u de implementatie van de oplossing via Azure Portal.

#### <a name="sqldata-administrator"></a>De beheerder van de SQL-gegevens
De functie van grote hoeveelheden gegevens importeren en de functie voor het bijwerken van operationele gegevens voor het uploaden naar de Azure SQL database worden gedeeld door de beheerder van de SQL-gegevens. De beheerder van de SQL-gegevens is niet verantwoordelijk voor alle updates van operationele gegevens in de database, maar is mogelijk om de gegevens via Power BI weer te geven.

#### <a name="operational-user"></a>Operationele gebruiker
De operationele gebruiker de gegevens regelmatig bijgewerkt en eigenaar is van de dagelijkse gegevens genereren. De operationele gebruiker zal ook resultaten via Power BI interpreteren.

### <a name="azure-services"></a>Azure-services

Deze oplossing maakt gebruik van de volgende Azure-services. Informatie van de implementatiearchitectuur vindt u in de [architectuur](#deployment-architecture) sectie.
- Azure Functions
- Azure SQL Database
- Azure Analysis Service
- Azure Active Directory
- Azure Key Vault
- Azure Monitor (Logboeken)
- Azure Storage
- ExpressRoute/VPN-Gateway
- Power BI Dashboard

## <a name="deployment-architecture"></a>Implementatie-architectuur
De volgende sectie bevat de elementen van de ontwikkeling en implementatie.

![alternatieve tekst](images/fedramp-analytics-components.png?raw=true "Analytics voor FedRAMP-onderdelen-diagram")

**Azure Functions**: [Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-overview) zijn oplossingen voor het uitvoeren van kleine stukjes code in de cloud via de meeste moderne programmeertalen. Functies in deze oplossing integreren met Azure Storage voor het automatisch ophalen van gegevens van de klant naar de cloud, om integratie met andere Azure-services te vergemakkelijken. Functies zijn gemakkelijk schaalbare en alleen kosten in rekening gebracht, wanneer ze worden uitgevoerd.

**Azure Analysis Service**: [Azure Analysis Services](https://docs.microsoft.com/azure/analysis-services/analysis-services-overview) biedt gegevensmodellering van enterprise- en integratie met Azure data platform-services. Azure Analysis Services sneller te bladeren door enorme hoeveelheden gegevens van het combineren van gegevens uit meerdere bronnen in een één-gegevensmodel.

**Power BI**: [Power BI](https://docs.microsoft.com/power-bi/service-azure-and-power-bi) levert analyse- en rapportagefuncties voor klanten die probeert om op te halen van meer inzicht krijgen buiten hun inspanningen gegevensverwerking.

### <a name="networking"></a>Netwerken
**Netwerkbeveiligingsgroepen**: [Nsg's](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) zijn ingesteld voor het beheren van verkeer dat bestemd is geïmplementeerde resources en services. Netwerkbeveiligingsgroepen worden ingesteld om te weigeren door standaardschema en alleen verkeer dat is opgenomen in de vooraf geconfigureerde lijst met ACL (Access Control) toestaan.

Elk van de nsg's zijn bepaalde poorten en protocollen openen, zodat de oplossing kunt veilig en goed werken. Bovendien zijn de volgende configuraties voor elke NSG ingeschakeld:
  - [Diagnostische logboeken en gebeurtenissen](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) zijn ingeschakeld en die zijn opgeslagen in een storage-account
  - [Logboeken in Azure Monitor](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-networking-analytics) is verbonden met de diagnostische logboeken van de NSG.

### <a name="data-at-rest"></a>Data-at-rest
De architectuur beveiligt gegevens in rust via versleuteling, controle-database en andere metingen.

**Replicatie van gegevens** Azure Government heeft twee opties voor [gegevensreplicatie](https://docs.microsoft.com/azure/storage/common/storage-redundancy):
 - De standaardinstelling voor replicatie van gegevens is **geografisch redundante opslag (GRS)**, waarin gegevens van de klant asynchroon wordt opgeslagen in een afzonderlijk Datacenter buiten de primaire regio. Dit zorgt ervoor dat het herstellen van gegevens in een gebeurtenis van het totale verlies van het primaire Datacenter.
 - **Lokaal redundante opslag (LRS)** kunnen ook worden geconfigureerd via de Azure Storage-Account. LRS repliceert gegevens in een opslagschaaleenheid, die wordt gehost in dezelfde regio bevinden waarin de klant wordt hun account gemaakt. Alle gegevens worden gerepliceerd gelijktijdig, ervoor te zorgen dat er geen back-upgegevens in een fout bij de primaire opslag scale unit gaat verloren.

**Azure Storage** om te voldoen aan de gecodeerde data-at-rest-vereisten, alle services die zijn geïmplementeerd in deze referentie-architectuur gebruik te maken van [Azure Storage](https://azure.microsoft.com/services/storage/), waarin wordt opgeslagen gegevens met [Storage Service Encryption](https://docs.microsoft.com/azure/storage/storage-service-encryption).

**Azure Disk Encryption**
[Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) maakt gebruik van de BitLocker-functie van Windows om volumeversleuteling voor het besturingssysteem en gegevensschijven te bieden. De oplossing kan worden geïntegreerd met Azure Key Vault voor het beheren en beheren van de versleutelingssleutels op de schijf.

**Azure SQL Database** de Azure SQL Database-exemplaar maakt gebruik van de volgende metingen van de database-beveiliging:
-   [AD-verificatie en autorisatie](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) maakt identiteitsbeheer van databasegebruikers en andere Microsoft-services op één centrale locatie.
-   [SQL database auditing](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) databasegebeurtenissen bijgehouden en geschreven naar een auditlogboek in Azure storage-account.
-   SQL-Database is geconfigureerd voor het gebruik van [transparante gegevensversleuteling (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql), die voert realtime versleuteling en ontsleuteling van gegevens en logboekbestanden om gegevens in rust te beveiligen. TDE biedt de zekerheid dat de opgeslagen gegevens niet is onderworpen aan onbevoegde toegang.
-   [Firewall-regels](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) voorkomen dat u alle toegang tot de database-servers, totdat de juiste machtigingen zijn toegekend. De firewall verleent toegang tot databases op basis van het IP-adres waar de aanvraag vandaan komt.
-   [SQL Threat Detection](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) kunt u detectie en reactie op mogelijke bedreigingen wanneer deze zich voordoen, dankzij beveiligingswaarschuwingen voor verdachte databaseactiviteiten, potentiële kwetsbaarheden, SQL-injectieaanvallen en afwijkende database-toegang patronen.
-   [Altijd versleuteld kolommen](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) ervoor te zorgen dat gevoelige gegevens nooit wordt weergegeven als tekst zonder opmaak in de database-systeem. Na het inschakelen van versleuteling van gegevens, alleen clienttoepassingen of appservers met toegang tot de sleutels gegevens zijn toegankelijk als tekst zonder opmaak.
-   [SQL Database dynamische gegevensmaskering](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) kan worden uitgevoerd nadat de referentiearchitectuur implementeert. Klanten moet aanpassen, dynamische gegevensmaskering instellingen om te voldoen aan het schema van de database.

### <a name="logging-and-audit"></a>Logboekregistratie en controle
[Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-get-started) genereert een volledige-weergave van bewakingsgegevens zoals activiteitenlogboeken, metrische gegevens en diagnostische gegevens, zodat klanten kunnen een compleet beeld van de systeemstatus maken.  
[Logboeken in Azure Monitor](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) biedt uitgebreide logboekregistratie van het systeem- en gebruikersactiviteit, evenals de status van het bestandssysteem. Deze verzamelt en analyseert gegevens gegenereerd door resources in Azure en on-premises omgevingen.
- **Activiteitenlogboeken**: [Activiteitenlogboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bieden inzicht in bewerkingen die worden uitgevoerd op resources in een abonnement.
- **Diagnostische logboeken**: [Diagnostische logboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) bevatten alle logboeken die zijn gegenereerd door elke resource. Deze logboeken bevatten de logboeken voor Windows-systeem en Azure Blob-opslag, tabellen en wachtrijlogboeken.
- **Firewall-logboeken**: De Application Gateway biedt volledige diagnostische en toegang tot logboeken. Firewall-logboeken zijn beschikbaar voor Application Gateway WAF-functionaliteit resources.
- **Logboek archiveren**: Alle logboeken met diagnostische gegevens schrijven naar een gecentraliseerd en versleutelde Azure storage-account voor archivering met een gedefinieerde bewaarperiode van twee dagen. Deze logboeken verbinden met Azure Monitor-logboeken voor verwerking, opslag en -dashboardrapporten.

Bovendien is de volgende bewakingsoplossingen, opgenomen als onderdeel van deze architectuur:
-   [Azure Automation](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker): De oplossing Azure Automation opgeslagen, wordt uitgevoerd en runbooks worden beheerd.
-   [Beveiliging en controle](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started): Het dashboard beveiliging en controle biedt een hoogwaardig inzicht in de beveiligingsstatus van resources, dankzij metrische gegevens over beveiligingsdomeinen, problemen die aandacht vereisen, detecties, bedreigingsinformatie en algemene Beveiligingsquery's.
-   [SQL-evaluatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): De oplossing SQL-statuscontrole beoordeelt het risico en de gezondheid van server-omgevingen op een vast interval en biedt klanten een geprioriteerde lijst met aanbevelingen die specifiek zijn voor de geïmplementeerde serverinfrastructuur.
-   [Azure-activiteitenlogboeken](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): De oplossing Activity Log Analytics biedt ondersteuning voor analyse van de Azure-activiteitenlogboeken voor alle Azure-abonnementen voor een klant.

### <a name="identity-management"></a>Identiteitsbeheer
-   Verificatie van de toepassing wordt uitgevoerd met behulp van Azure AD. Zie voor meer informatie, [toepassingen integreren met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Bovendien de versleuteling van de kolom database maakt gebruik van Azure AD om te verifiëren van de toepassing naar Azure SQL Database. Zie voor meer informatie over het [bescherming van gevoelige gegevens in SQL-Database](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   [Azure Active Directory Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) mogelijke beveiligingsproblemen die betrekking hebben op van een organisatie-id's detecteert, configureert u automatische antwoorden op gedetecteerde verdachte activiteit met betrekking tot een organisatie-id's en onderzoekt het probleem verdachte incidenten, onderneem gepaste actie op te lossen.
-   [Azure Role-based Access Control (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) maakt gericht beheer van toegang voor Azure. Abonnementstoegang is beperkt tot de beheerder van het abonnement.

Zie voor meer informatie over het gebruik van de beveiligingsfuncties van Azure SQL Database, de [Contoso Clinic-Demotoepassing](https://github.com/Microsoft/azure-sql-security-sample) voorbeeld.

### <a name="security"></a>Beveiliging
**Geheimen management**: De oplossing maakt gebruik van [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) voor het beheer van sleutels en geheimen. Met Azure Sleutelkluis kunt u de cryptografische sleutels en geheimen beveiligen die door cloudtoepassingen en -services worden gebruikt.

## <a name="guidance-and-recommendations"></a>Richtlijnen en aanbevelingen

### <a name="expressroute-and-vpn"></a>ExpressRoute- en VPN
[ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) of een beveiligde VPN-tunnel moet worden geconfigureerd voor een veilig verbinding met de resources die zijn geïmplementeerd als onderdeel van deze referentiearchitectuur voor analyse van gegevens. Als het ExpressRoute-verbindingen gaan niet via het Internet, bieden deze verbindingen een meer betrouwbaarheid, hogere snelheden, kortere wachttijden en hogere beveiliging dan gebruikelijke verbindingen via Internet. Door op de juiste wijze instellen van ExpressRoute of een VPN-verbinding, kunnen klanten een beveiligingslaag voor gegevens in transit toevoegen.

### <a name="azure-active-directory-setup"></a>Installatie van de Azure Active Directory
[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) is essentieel voor het beheren van de implementatie en inrichting van toegang tot personeel interactie met de omgeving. Een bestaande Windows Server Active Directory kunnen worden geïntegreerd met AAD in [vier klikken](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-express). Klanten kunnen ook de geïmplementeerde Active Directory-infrastructuur (domeincontrollers) koppelen aan een bestaande AAD doordat de geïmplementeerde Active Directory-infrastructuur een subdomein van een AAD-forest.

### <a name="additional-services"></a>Extra services
#### <a name="iaas---vm-vonsiderations"></a>IaaS - VM vonsiderations
Deze PaaS-oplossing niet is opgenomen met een Azure IaaS VM's. Een klant kan een Azure-VM om uit te voeren veel van deze PaaS-services maken. In dit geval kunnen specifieke functies en services voor zakelijke continuïteit en logboeken van Azure Monitor worden gebruikt:

##### <a name="business-continuity"></a>Bedrijfscontinuïteit
- **Hoge beschikbaarheid**: Server-workloads worden gegroepeerd in een [Beschikbaarheidsset](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-manage-availability?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) om te zorgen voor hoge beschikbaarheid van virtuele machines in Azure. Ten minste één virtuele machine beschikbaar is tijdens gepland of ongepland onderhoud, voldoen aan de 99,95% Azure SLA.

- **Recovery Services-kluis**: De [Recovery Services-kluis](https://docs.microsoft.com/azure/backup/backup-azure-recovery-services-vault-overview) -instellingen van de back-upgegevens en alle configuraties van Azure Virtual Machines in deze architectuur beveiligt. Met een Recovery Services-kluis, klanten kunnen bestanden en mappen terugzetten vanuit een IaaS-VM zonder te herstellen van de hele virtuele machine, waardoor sneller worden hersteld.

##### <a name="monitoring-solutions"></a>Bewakingsoplossingen
-   [AD-evaluatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): De oplossing statuscontrole van Active Directory beoordeelt het risico en de gezondheid van server-omgevingen op een vast interval en biedt een geprioriteerde lijst met aanbevelingen die specifiek zijn voor de geïmplementeerde serverinfrastructuur.
-   [Antimalware-evaluatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-malware): De Antimalware-oplossing rapporteert over de status van schadelijke software, bedreigingen en beveiliging.
-   [Updatebeheer](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-update-management): De oplossing Update Management kunt Klantenbeheer van updates voor besturingssysteem, met inbegrip van de status van de beschikbare updates en het installatieproces van vereiste updates.
-   [Status van agent](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): De oplossing status van Agent rapporteert het aantal agents zijn geïmplementeerd en hun geografische verdeling, evenals hoeveel agents die niet meer reageert en het aantal agents die zijn operationele gegevens kan verzenden.
-   [Wijzigingen bijhouden](https://docs.microsoft.com/azure/automation/automation-change-tracking): De oplossing wijzigingen bijhouden kan klanten eenvoudig wijzigingen in de omgeving identificeren.

##### <a name="security"></a>Beveiliging
- **Bescherming tegen schadelijke software**: [Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) voor virtuele Machines biedt realtime-beveiliging-functie die helpt te identificeren en verwijderen van virussen, spyware en andere schadelijke software, met configureerbare meldingen wanneer bekende schadelijke of ongewenste software wil installeren of uitvoeren op de beveiligde virtuele machines.
- **Patchbeheer**: Windows virtuele machines geïmplementeerd als onderdeel van deze referentiearchitectuur worden standaard automatisch updates ontvangen van Windows Update-Service geconfigureerd. Deze oplossing bevat ook de [Azure Automation](https://docs.microsoft.com/azure/automation/automation-intro) service waarmee bijgewerkte implementaties kunnen worden gemaakt voor de patch voor virtuele machines wanneer dat nodig is.

#### <a name="azure-commercial"></a>Azure commercieel
Hoewel deze data analytics-architectuur is niet bedoeld voor implementatie naar de [Azure Commercial](https://azure.microsoft.com/overview/what-is-azure/) omgeving, hetzelfde doel kunnen worden bereikt via de services die worden beschreven in deze referentiearchitectuur, evenals aanvullende services die alleen beschikbaar in de commerciële Azure-omgeving. Houd er rekening mee dat Azure Commercial een FedRAMP JAB P-ATO op het niveau van matige Impact onderhoudt, overheidsinstellingen worden gesteld en partners te implementeren dat gevoelige informatie naar de cloud gebruik te maken van de commerciële Azure-omgeving.

Azure commercieel biedt nog tal van analytics-services voor het binnenhalen van inzichten uit grote hoeveelheden gegevens:
-   [Azure Machine Learning Studio](https://docs.microsoft.com/azure/machine-learning/studio/what-is-ml-studio), een onderdeel van de [Cortana Intelligence Suite](https://azure.microsoft.com/overview/ai-platform/), ontwikkelt een predictive Analytics-model uit een of meer gegevensbronnen. Statistische functies worden gebruikt via verschillende iteraties groeit voor het genereren van een doeltreffende model dat toepassingen, zoals Power BI vervolgens kunnen gebruiken.
-   [Azure Data Lake Store](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-overview) kunt u het vastleggen van gegevens van elke grootte, soort en opnamesnelheid op één plek voor operationele en verkennende analyse. Azure Data Lake Store is compatibel met de meeste open source-onderdelen in het Hadoop-ecosysteem en mooi integreert met andere Azure-services.

## <a name="threat-model"></a>Risicomodel

De gegevensstroom-diagram (GSD) voor deze referentiearchitectuur is beschikbaar voor [downloaden](https://aka.ms/blueprintanalyticsthreatmodel) of vindt u hieronder:

## <a name="compliance-documentation"></a>Naleving-documentatie
De [Azure-beveiliging en naleving blauwdruk – FedRAMP hoog klant verantwoordelijkheid Matrix](https://aka.ms/blueprinthighcrm) geeft een lijst van alle beveiligingsmaatregelen die zijn vereist voor de basislijn FedRAMP High. Op dezelfde manier de [Azure-beveiliging en naleving blauwdruk – FedRAMP gemiddeld klant verantwoordelijkheid Matrix](https://aka.ms/blueprintanalyticscimmod) geeft een lijst van alle beveiligingsmaatregelen die zijn vereist voor de basislijn FedRAMP gemiddeld. Beide documenten beschreven of de implementatie van elk besturingselement de verantwoordelijkheid van Microsoft, de klant is, of gedeeld tussen de twee.

De [Azure-beveiliging en blauwdruk voor naleving - FedRAMP hoog besturingselement implementatie Matrix](https://aka.ms/blueprintanalyticscimhigh) en de [Azure-beveiliging en blauwdruk voor naleving - FedRAMP gemiddeld besturingselement implementatie Matrix](https://aka.ms/blueprintanalyticscimmod) bieden informatie over waarop besturingselementen worden gedekt door de analytics-architectuur voor elke basislijn FedRAMP, met inbegrip van gedetailleerde beschrijvingen van de manier waarop de implementatie voldoet aan de vereisten van elk besturingselement vallen.

## <a name="disclaimer"></a>Vrijwaring

 - Dit document is uitsluitend ter informatie bedoeld. MICROSOFT BIEDT GEEN GARANTIES, EXPLICIETE, IMPLICIETE OF WETTELIJKE GARANTIE VOOR DE INFORMATIE IN DIT DOCUMENT. Dit document wordt geleverd ' as-is. " Informatie en meningen in dit document, inclusief URL's en andere websiteverwijzingen, kunnen zonder kennisgeving worden gewijzigd. Klanten die in dit document leest draagt het risico van het gebruik ervan.
 - Dit document biedt klanten met een enkel wettelijk recht op enig intellectueel in andere Microsoft-producten of oplossingen.
 - Klanten kunnen kopiëren en gebruiken van dit document voor interne referentiedoeleinden.
 - Bepaalde aanbevelingen in dit document kunnen leiden tot grotere hoeveelheden gegevens, netwerk- of gebruik van de compute-bronnen in Azure en de Azure-licentie of abonnement kosten van een klant kunnen verhogen.
 - Deze architectuur is bedoeld om te fungeren als een basis voor klanten om aan te passen op hun specifieke behoeften en mag niet worden gebruikt als-is in een productieomgeving.
 - Dit document is ontwikkeld als referentie en mag niet worden gebruikt voor het definiëren van alle middelen waarmee een klant kan voldoen aan specifieke nalevingsvereisten en voorschriften. Klanten moeten juridische ondersteuning van hun organisatie op goedgekeurde klantimplementaties gezocht.
