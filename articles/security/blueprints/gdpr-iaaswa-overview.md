---
title: Azure-beveiliging en naleving blauwdruk - IaaS-webtoepassing Avg
description: Azure-beveiliging en naleving blauwdruk - IaaS-webtoepassing Avg
services: security
author: jomolesk
ms.assetid: 04d5239c-fff0-4c2d-9379-1fa79ddbec78
ms.service: security
ms.topic: article
ms.date: 05/14/2018
ms.author: jomolesk
ms.openlocfilehash: 509c33f2774e2d16bb8a96c50d1fb53962578dee
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/18/2018
ms.locfileid: "49409319"
---
# <a name="azure-security-and-compliance-blueprint---iaas-web-application-for-gdpr"></a>Azure-beveiliging en naleving blauwdruk - IaaS-webtoepassing Avg

## <a name="overview"></a>Overzicht
De General Data Protection Regulation (GDPR) bevat veel vereisten voor het verzamelen, opslaan en het gebruik van persoonlijke gegevens, met inbegrip van hoe organisaties identificeren en beveiligen van persoonlijke gegevens, aan transparantie te voldoen, detecteren en rapporteren schendingen van persoonlijke gegevens, en train privacy personeel en andere werknemers. De AVG personen biedt meer controle over hun persoonlijke gegevens en legt veel nieuwe verplichtingen voor organisaties die verzameld, verwerkt of persoonlijke gegevens te analyseren. De AVG legt een nieuwe regels voor organisaties die goederen en services naar mensen in de Europese Unie (EU) of die verzamelen en analyseren van gegevens van inwoners van de EU. De AVG is van toepassing, ongeacht waar een organisatie zich bevindt.

Microsoft heeft Azure ontworpen met toonaangevende beveiligingsmaatregelen en privacybeleidsregels ter bescherming van de gegevens in de cloud, waaronder de categorieën van persoonlijke gegevens die zijn geïdentificeerd door de GDPR. Microsofts [contractvoorwaarden](http://aka.ms/Online-Services-Terms) Microsoft aan de vereisten van processors die zijn doorgevoerd.

Deze Azure-beveiliging en naleving blauwdruk biedt richtlijnen voor het implementeren van een infrastructuur als een geschikt is voor een eenvoudige webtoepassing voor internetgerichte service (IaaS)-omgeving. Deze oplossing laat zien waar klanten kunnen voldoen aan specifieke vereisten voor beveiliging en naleving van de AVG manieren en fungeert als een basis voor klanten om te bouwen en configureren van hun eigen IaaS-oplossingen voor web-toepassing in Azure. Klanten kunnen gebruikmaken van deze referentiearchitectuur en gaat u als volgt Microsofts [vier stappen](https://aka.ms/gdprebook) in hun op weg naar de GDPR-naleving:
1. Detecteren: Identificeer welke persoonlijke gegevens bestaat en waar deze zich bevindt.
2. Beheren: Bepalen hoe persoonsgegevens wordt gebruikt en geopend.
3. Beveiligen: Tot stand brengen beveiligingsmaatregelen als u wilt voorkomen, detecteren en reageren op beveiligingsproblemen en inbreuken op de gegevens.
4. Rapport: Vereiste documentatie houden en beheren van aanvragen voor gegevens en in strijd is met meldingen.

Deze referentiearchitectuur, gekoppelde Implementatiehandleiding en risicomodel zijn bedoeld om te fungeren als een basis voor klanten aan te passen aan hun specifieke vereisten en mag niet worden gebruikt als-is in een productieomgeving. Houd rekening met het volgende:
- De architectuur biedt een basislijn om workloads naar Azure te implementeren op een manier die compatibel met GDPR klanten te helpen.
- Klanten zijn verantwoordelijk voor het uitvoeren van de juiste beveiliging en naleving evaluaties van een oplossing die is gebouwd met behulp van deze architectuur als vereisten kunnen variëren op basis van de details van de uitvoering van elke klant.

## <a name="architecture-diagram-and-components"></a>Diagram van architectuur en onderdelen
Deze oplossing wordt geïmplementeerd voor een referentiearchitectuur voor een IaaS-webtoepassing met een SQL Server back-end. De architectuur bevat een weblaag, gegevenslaag, Active Directory-infrastructuur, Application Gateway en Load Balancer. Virtuele machines die zijn geïmplementeerd voor de web- en gegevenslagen zijn geconfigureerd in een beschikbaarheidsset en SQL Server-exemplaren zijn geconfigureerd in een AlwaysOn-beschikbaarheidsgroep voor hoge beschikbaarheid. Virtuele machines zijn domein en Active Directory-groepsbeleid worden gebruikt om af te dwingen van beveiliging en naleving configuraties op het niveau van het besturingssysteem. Een bastionhost management biedt een beveiligde verbinding voor beheerders voor toegang tot geïmplementeerd resources. **Azure wordt aanbevolen voor het configureren van een VPN of ExpressRoute-verbinding voor beheer en de gegevens importeren in het subnet van referentie-architectuur.**

![IaaS Web Applicaiton voor AVG verwijzing Architectuurdiagram](images/gdpr-iaaswa-architecture.png?raw=true "IaaS Web Applicaiton voor Architectuurdiagram van GDPR-verwijzing")

Deze oplossing maakt gebruik van de volgende Azure-services. Details van de implementatiearchitectuur bevinden zich in de [architectuur](#deployment-architecture) sectie.

- Azure Virtual Machines
    - (1) management/bastionhost (Windows Server 2016 Datacenter)
    - (2) active Directory-domeincontroller (Windows Server 2016 Datacenter)
    - (2) SQL Server-clusterknooppunt (SQL Server 2017 voor Windows Server 2016)
    - (2) web/IIS (Windows Server 2016 Datacenter)
- Beschikbaarheidssets
    - (1) active Directory-domeincontrollers
    - (1) SQL-clusterknooppunten
    - (1) web/IIS
- Azure Virtual Network
    - (1) /16 virtuele netwerken
    - (5) /24 subnetten
    - DNS-instellingen zijn ingesteld op beide domeincontrollers
- Azure Load Balancer
- Azure Application Gateway
    - (1) de WAF-toepassingsgateway ingeschakeld
        - Firewallmodus: preventie
        - Regelset: OWASP 3.0
        - listener: poort 443
- Azure Storage
    - (7) Geo-redundant storage-accounts
- Cloud-Witness
- Recovery Services-kluis
- Azure Key Vault
- Azure Active Directory (AAD)
- Azure Resource Manager
- Log Analytics
- Azure Security Center

## <a name="deployment-architecture"></a>Implementatie-architectuur
De volgende sectie bevat de elementen van de implementatie en uitvoering.

**Bastionhost**: de bastionhost is de enkel ingangspunt waarmee gebruikers toegang krijgen tot de geïmplementeerde resources in deze omgeving. De bastionhost biedt een beveiligde verbinding met de geïmplementeerde resources door toe te staan alleen extern verkeer vanaf openbare IP-adressen op een veilige lijst. Als u wilt toestaan dat verkeer van extern bureaublad (RDP), moet de bron van het verkeer worden gedefinieerd in de netwerkbeveiligingsgroep (NSG).

Deze oplossing maakt u een virtuele machine als een domein bastionhost met de volgende configuraties:
-   [Antimalware-uitbreiding](https://docs.microsoft.com/azure/security/azure-security-antimalware)
-   [Log Analytics-extensie](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-oms)
-   [Azure Diagnostics-extensie](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template)
-   [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) met behulp van Azure Key Vault
-   Een [beleid voor automatisch afsluiten](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) verkleind van resources van de virtuele machine als deze niet in gebruik
-   [Windows Defender Credential Guard](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard) ingeschakeld die referenties en andere geheimen in een beveiligde omgeving die is geïsoleerd van het besturingssysteem wordt uitgevoerd

### <a name="virtual-network"></a>Virtueel netwerk
De architectuur definieert een particulier virtueel netwerk met een adresruimte van 10.200.0.0/16.

**Netwerkbeveiligingsgroepen**: deze oplossing wordt geïmplementeerd voor resources in een architectuur met een afzonderlijke subnet, database-subnet, Active Directory-subnet en beheersubnet binnen een virtueel netwerk. Subnetten worden logisch gescheiden door regels voor netwerkbeveiligingsgroepen toegepast op de afzonderlijke subnetten te beperken het verkeer tussen subnetten die alleen dat nodig is voor het systeem en de beheerfunctionaliteit.

Zie de configuratie voor [netwerkbeveiligingsgroepen](https://github.com/Azure/fedramp-iaas-webapp/blob/master/nestedtemplates/virtualNetworkNSG.json) met deze oplossing wordt geïmplementeerd. Organisaties kunnen netwerkbeveiligingsgroepen configureren door het bewerken van het bestand dan het gebruik van [deze documentatie](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) als richtlijn.

Elk van de subnetten heeft een speciale netwerkbeveiligingsgroep (NSG):
- 1 NSG voor Application Gateway (LBNSG)
- 1 NSG voor bastionhost (MGTNSG)
- 1 NSG voor primaire en back-domeincontrollers (ADNSG)
- 1 NSG voor SQL-Servers en Cloud-Witness (SQLNSG)
- 1 NSG voor weblaag (WEBNSG)

### <a name="data-in-transit"></a>Gegevens die onderweg zijn
Alle communicatie naar en van Azure-datacenters versleutelt Azure standaard. Bovendien worden alle transacties met Azure Storage via Azure portal via HTTPS optreden.

### <a name="data-at-rest"></a>Data-at-rest
De architectuur beveiligt gegevens in rust via meerdere metingen, versleuteling en Databasecontrole.

**Azure Storage**: om versleutelde data-at-rest-vereisten te voldoen aan alle [Azure Storage](https://azure.microsoft.com/services/storage/) maakt gebruik van [Storage Service Encryption](https://docs.microsoft.com/azure/storage/storage-service-encryption). Dit helpt te beschermen en beveiligen van persoonlijke gegevens ter ondersteuning van de organisatie beveiligingsverplichtingen en nalevingsvereisten zijn gedefinieerd door de GDPR.

**Azure Disk Encryption**: Azure Disk Encryption wordt gebruikt Windows IaaS VM-schijven worden versleuteld. [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) maakt gebruik van de BitLocker-functie van Windows om volumeversleuteling voor het besturingssysteem en gegevensschijven te bieden. De oplossing is geïntegreerd met Azure Key Vault voor het beheren en beheren van de versleutelingssleutels op de schijf.

**SQL Server**: de SQL Server-exemplaar maakt gebruik van de volgende metingen van de database-beveiliging:
-   [AD-verificatie en autorisatie](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) maakt identiteitsbeheer van databasegebruikers en andere Microsoft-services op één centrale locatie.
-   [SQL database auditing](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) databasegebeurtenissen bijgehouden en geschreven naar een auditlogboek in Azure storage-account.
-   SQL-Database is geconfigureerd voor het gebruik van [transparante gegevensversleuteling (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql), die voert realtime versleuteling en ontsleuteling van de database, gekoppelde back-ups en transactielogboekbestanden ter bescherming van gegevens op over de rest. TDE biedt de zekerheid dat persoonlijke gegevens die zijn opgeslagen is niet onderhevig aan onbevoegde toegang.
-   [Firewall-regels](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) voorkomen dat u alle toegang tot de database-servers, totdat de juiste machtigingen zijn toegekend. De firewall verleent toegang tot databases op basis van het IP-adres waar de aanvraag vandaan komt.
-   [SQL Threat Detection](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) kunt u detectie en reactie op mogelijke bedreigingen wanneer deze zich voordoen, dankzij beveiligingswaarschuwingen voor verdachte databaseactiviteiten, potentiële kwetsbaarheden, SQL-injectieaanvallen en afwijkende database-toegang patronen.
-   [Altijd versleuteld kolommen](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) ervoor te zorgen dat gevoelige, persoonlijke gegevens nooit wordt weergegeven als tekst zonder opmaak in de database-systeem. Na het inschakelen van versleuteling van gegevens, alleen clienttoepassingen of toepassingsservers met toegang tot de sleutels gegevens zijn toegankelijk als tekst zonder opmaak.
- De [uitgebreide eigenschappen](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql) functie kan worden gebruikt om te stoppen met de verwerking van de betrokkenen, omdat hiermee gebruikers aangepaste eigenschappen toevoegen aan database-objecten en gegevens markeren als 'Discontinued' voor de ondersteuning van de toepassingslogica om te voorkomen dat de verwerking van de bijbehorende gegevens.
- [Beveiliging op rijniveau](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) kunnen gebruikers voor het definiëren van beleid om toegang te beperken tot gegevens te verwerken.
- [SQL Database Dynamic Data Masking (DDM)](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) blootstelling van gevoelige, persoonlijke gegevens door het maskeren van de gegevens naar niet-gemachtigde gebruikers of toepassingen. DDM kan automatisch detecteren van potentieel gevoelige gegevens en stel de juiste maskers moet worden toegepast. Dit helpt bij de identificatie van persoonlijke gegevens die in aanmerking komen voor de beveiliging van de AVG en voor het beperken van toegang, zodat deze niet de database via niet-geautoriseerde toegang wordt afgesloten. **Opmerking: Klanten moet DDM instellingen aanpassen om te voldoen aan het schema van de database.**

### <a name="identity-management"></a>Identiteitsbeheer
De volgende technologieën bieden mogelijkheden voor het beheren van toegang tot persoonlijke gegevens in de Azure-omgeving:
-   [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) is van Microsoft met meerdere tenants cloudgebaseerde directory service- en identiteit. Alle gebruikers voor deze oplossing worden gemaakt in AAD, met inbegrip van gebruikers met toegang tot de SQL-Server.
-   Verificatie van de toepassing wordt uitgevoerd met behulp van AAD. Zie voor meer informatie, [toepassingen integreren met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Bovendien de versleuteling van de kolom database maakt gebruik van AAD om de toepassing met SQL Server te verifiëren. Zie voor meer informatie over het [bescherming van gevoelige gegevens in SQL-Database](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   [Azure Role-Based Access Control (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) kunnen beheerders om te voorzien in specifieke toegangsmachtigingen te verlenen alleen de mate van toegang dat gebruikers moeten om hun werk te definiëren. In plaats van geeft elke onbeperkte gebruikersmachtigingen voor Azure-resources, beheerders kunnen toestaan dat alleen bepaalde acties voor toegang tot persoonlijke gegevens. Abonnementstoegang is beperkt tot de beheerder van het abonnement.
- [AAD Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) kunnen klanten voor het minimaliseren van het aantal gebruikers die toegang tot bepaalde resources hebben.  Beheerders kunnen AAD Privileged Identity Management gebruiken om te detecteren, beperken en controleren van bevoegde identiteiten en hun toegang tot bronnen. Deze functionaliteit kan ook worden gebruikt om af te dwingen op aanvraag, just-in-time beheerderstoegang wanneer dat nodig is.
- [AAD Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) mogelijke beveiligingsproblemen die betrekking hebben op van een organisatie-id's detecteert, configureert u automatische antwoorden op gedetecteerde verdachte activiteit met betrekking tot een organisatie-id's en verdachte onderzoekt het probleem incidenten, onderneem gepaste actie op te lossen.
- Een geïmplementeerde IaaS Active Directory-exemplaar biedt identiteitsbeheer op besturingssysteemniveau voor geïmplementeerde virtuele machines van IaaS.

### <a name="security"></a>Beveiliging
**Geheimen management**: de oplossing maakt gebruik van [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) voor het beheer van sleutels en geheimen. Met Azure Sleutelkluis kunt u de cryptografische sleutels en geheimen beveiligen die door cloudtoepassingen en -services worden gebruikt. De volgende mogelijkheden van Azure Key Vault zodat klanten bescherming van persoonlijke gegevens en toegang tot deze gegevens:
- Geavanceerde beleidsregels zijn geconfigureerd op basis van behoefte.
- Toegangsbeleid voor Key Vault zijn met de minimaal vereiste machtigingen voor sleutels en geheimen gedefinieerd.
- Verloopdatum hebben alle sleutels en geheimen in Key Vault.
- Alle sleutels in Key Vault worden beschermd door gespecialiseerde hardware security modules (HSM's). Het sleuteltype is een HSM beveiligd 2048-bits RSA-sleutel.
- Minimaal vereiste machtigingen met RBAC worden verleend door alle gebruikers en identiteiten.
- Diagnostische logboeken voor Key Vault worden ingeschakeld met een bewaarperiode van ten minste 365 dagen.
- Toegestane cryptografiebewerkingen voor sleutels zijn beperkt tot die nodig is.
- De oplossing is geïntegreerd met Azure Key Vault voor het beheren van IaaS VM-schijfversleuteling sleutels en geheimen.

**Patchbeheer**: Windows virtuele machines geïmplementeerd als onderdeel van deze referentiearchitectuur zijn standaard geconfigureerd voor automatische updates ontvangen van Windows Update-Service. Deze oplossing bevat ook de [Azure Automation](https://docs.microsoft.com/azure/automation/automation-intro) service waarmee bijgewerkte implementaties kunnen worden gemaakt voor de patch voor virtuele machines wanneer dat nodig is.

**Bescherming tegen schadelijke software**: [Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) voor virtuele Machines biedt de mogelijkheid van de realtime-beveiliging die helpt bij identificeren en virussen, spyware en andere schadelijke software, met configureerbare meldingen verwijderen Wanneer u bekende schadelijke of ongewenste software probeert te installeren of uitvoeren op de beveiligde virtuele machines.

**Beveiligingswaarschuwingen**: [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) kunnen klanten om verkeer te controleren, logboeken te verzamelen en analyseren van gegevensbronnen voor bedreigingen. Azure Security Center heeft bovendien toegang tot bestaande configuratie van Azure-services voor configuratie en de serviceaanbevelingen om u te helpen bij het beveiligingspostuur verbeteren en persoonlijke gegevens beschermen. Azure Security Center bevat een [threat intelligence-rapport](https://docs.microsoft.com/azure/security-center/security-center-threat-report) voor elke bedreigingen gedetecteerde te helpen incident response teams onderzoeken en bedreigingen verhelpen.

**Application Gateway**: de architectuur vermindert het risico van beveiligingsproblemen met behulp van een toepassingsgateway met web application firewall (WAF) en de OWASP-ruleset ingeschakeld. Aanvullende mogelijkheden zijn onder andere:

- [End-to-End-SSL](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- Schakel [SSL-Offload](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-portal)
- Uitschakelen [TLS v1.0 en v1.1](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- [De Web application firewall](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) (WAF-modus)
- [Preventiemodus](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal) met OWASP 3.0 ruleset
- Schakel [logboekregistratie van diagnostische gegevens](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics)
- [Aangepaste statuscontroles](https://docs.microsoft.com/azure/application-gateway/application-gateway-create-gateway-portal)
- [Azure Security Center](https://azure.microsoft.com/services/security-center) en [Azure Advisor](https://docs.microsoft.com/azure/advisor/advisor-security-recommendations) bieden extra beveiliging en meldingen. Azure Security Center biedt ook een reputatie-systeem.

### <a name="business-continuity"></a>Bedrijfscontinuïteit

**Hoge beschikbaarheid**: de oplossing wordt geïmplementeerd voor alle virtuele machines in een [Beschikbaarheidsset](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets). Beschikbaarheidssets zorgen ervoor dat de virtuele machines worden verdeeld over meerdere geïsoleerde hardwareclusters beschikbaarheid verbeteren. Ten minste één virtuele machine beschikbaar is tijdens gepland of ongepland onderhoud, voldoen aan de 99,95% Azure SLA.

**Recovery Services-kluis**: de [Recovery Services-kluis](https://docs.microsoft.com/azure/backup/backup-azure-recovery-services-vault-overview) -instellingen van de back-upgegevens en alle configuraties van Azure Virtual Machines in deze architectuur beveiligt. Met een Recovery Services-kluis, klanten kunnen bestanden en mappen terugzetten vanuit een IaaS-VM zonder te herstellen van de hele virtuele machine, waardoor sneller worden hersteld.

**Cloud-Witness**: [Cloud-Witness](https://docs.microsoft.com/windows-server/failover-clustering/whats-new-in-failover-clustering#BKMK_CloudWitness) is een type failovercluster-quorumwitness in Windows Server 2016 die gebruikmaakt van Azure als arbitragepunt. De Cloud-Witness, net als elke andere quorumwitness een stem opgehaald en kunt deelnemen aan de quorumberekeningen, maar hierbij de standaard openbare Azure Blob Storage. Dit elimineert de noodzaak extra onderhoud van virtuele machines die worden gehost in een openbare cloud.

### <a name="logging-and-auditing"></a>Logboekregistratie en controle

Log Analytics biedt uitgebreide logboekregistratie van het systeem- en gebruikersactiviteit, evenals de status van het bestandssysteem. De [Log Analytics](https://azure.microsoft.com/services/log-analytics/) oplossing verzamelt en analyseert gegevens gegenereerd door resources in Azure en on-premises omgevingen.
- **Activiteitenlogboeken**: [activiteitenlogboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bieden inzicht in bewerkingen die worden uitgevoerd op resources in een abonnement. Activiteitenlogboeken kunnen u eenvoudiger bepalen van een bewerking initiator, tijdstip van de gebeurtenis en status.
- **Diagnostische logboeken**: [diagnostische logboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) bevatten alle logboeken die zijn gegenereerd door elke resource. Deze logboeken bevatten de logboeken voor Windows-systeem, Azure Storage-Logboeken, Key Vault-auditlogboeken en Application Gateway toegangs- en firewall-Logboeken.
- **Logboek archiveren**: alle logboeken met diagnostische gegevens schrijven naar een gecentraliseerd en versleutelde Azure storage-account voor archivering. De bewaarperiode is om te voldoen aan vereisten voor de bewaarperiode van de organisatie-specifieke gebruiker-configureren, tot maximaal 730 dagen. Deze logboeken verbinding maken met Azure Log Analytics voor verwerking, opslag en -dashboardrapporten.

Bovendien is de volgende bewakingsoplossingen, opgenomen als onderdeel van deze architectuur:
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

De gegevensstroom-diagram (GSD) voor deze referentiearchitectuur is beschikbaar voor [downloaden](https://aka.ms/gdprIaaSdfd) of vindt u hieronder. Dit model kunt klanten inzicht krijgen in de punten van de mogelijke risico's in de infrastructuur van het systeem als u wijzigingen aanbrengt.

![IaaS Web Applicaiton voor AVG risicomodel](images/gdpr-iaaswa-threat-model.png?raw=true "IaaS Web Applicaiton voor risicomodel Avg")

## <a name="compliance-documentation"></a>Naleving-documentatie

De [Azure-beveiliging en naleving blauwdruk - AVG klant verantwoordelijkheid Matrix](https://aka.ms/gdprCRM) controller en processor verantwoordelijkheden voor alle AVG artikelen bevat. Houd er rekening mee dat voor Azure-services, een klant meestal de controller is en Microsoft als de processor fungeert.

De [Azure-beveiliging en naleving blauwdruk - AVG IaaS Web Application implementatie Matrix](https://aka.ms/gdprIaaSweb) bevat informatie over welke AVG artikelen worden verholpen door de webtoepassing voor IaaS-architectuur, met inbegrip van gedetailleerde beschrijvingen van hoe is de implementatie voldoet aan de vereisten van elk artikel vallen.

## <a name="guidance-and-recommendations"></a>Richtlijnen en aanbevelingen
### <a name="vpn-and-expressroute"></a>VPN en ExpressRoute
Een beveiligde VPN-tunnel of [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) moet worden geconfigureerd voor een veilig verbinding met de resources die zijn geïmplementeerd als onderdeel van deze referentiearchitectuur IaaS-webtoepassing. Door op de juiste wijze instellen van een VPN of ExpressRoute, toevoegen klanten een beveiligingslaag voor gegevens in transit.

Door het implementeren van een beveiligde VPN-tunnel met Azure, kan een virtuele particuliere verbinding tussen een on-premises netwerk en een Azure-netwerk worden gemaakt. Deze verbinding vindt plaats via Internet en kan klanten veilig 'tunnel' gegevens in een gecodeerde verbinding tussen het netwerk en Azure van de klant. Site-naar-Site VPN is een veilige, volwassen technologie die is geïmplementeerd door bedrijven van alle groottes decennia. De [IPsec-tunnelmodus](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) bij deze optie als een versleutelingsmechanisme voor wordt gebruikt.

Omdat het verkeer binnen de VPN-tunnel via Internet met een site-naar-site-VPN, biedt Microsoft een andere verbinding nog veiliger optie. Azure ExpressRoute is een speciale WAN koppeling tussen Azure en een on-premises locatie of een Exchange-hostingprovider. Als het ExpressRoute-verbindingen gaan niet via het Internet, bieden deze verbindingen een meer betrouwbaarheid, hogere snelheden, kortere wachttijden en hogere beveiliging dan gebruikelijke verbindingen via Internet. Bovendien, omdat dit een rechtstreekse verbinding van de klant telecommunicatie-provider, de gegevens niet via Internet kan worden verzonden en daarom geen toegang heeft tot deze.

Aanbevolen procedures voor het implementeren van een beveiligd hybride netwerk dat een on-premises netwerk naar Azure uitbreidt zijn [beschikbaar](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

## <a name="disclaimer"></a>Vrijwaring

- Dit document is uitsluitend ter informatie bedoeld. MICROSOFT BIEDT GEEN GARANTIES, EXPLICIETE, IMPLICIETE OF WETTELIJKE GARANTIE VOOR DE INFORMATIE IN DIT DOCUMENT. Dit document wordt geleverd ' as-is. " Informatie en meningen in dit document, inclusief URL's en andere websiteverwijzingen, kunnen zonder kennisgeving worden gewijzigd. Klanten die in dit document leest draagt het risico van het gebruik ervan.  
- Dit document biedt klanten met een enkel wettelijk recht op enig intellectueel in andere Microsoft-producten of oplossingen.  
- Klanten kunnen kopiëren en gebruiken van dit document voor interne referentiedoeleinden.  
- Bepaalde aanbevelingen in dit document kunnen leiden tot grotere hoeveelheden gegevens, netwerk- of gebruik van de compute-bronnen in Azure en de Azure-licentie of abonnement kosten van een klant kunnen verhogen.  
- Deze architectuur is bedoeld om te fungeren als een basis voor klanten om aan te passen op hun specifieke behoeften en mag niet worden gebruikt als-is in een productieomgeving.
- Dit document is ontwikkeld als referentie en mag niet worden gebruikt voor het definiëren van alle middelen waarmee een klant kan voldoen aan specifieke nalevingsvereisten en voorschriften. Klanten moeten juridische ondersteuning van hun organisatie op goedgekeurde klantimplementaties gezocht.
