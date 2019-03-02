---
title: Azure-beveiliging en naleving blauwdruk - IaaS-webtoepassing voor SP NIST 800-171
description: Azure-beveiliging en naleving blauwdruk - IaaS Web Application NIST SP 800-171
services: security
author: jomolesk
ms.assetid: 1f1ad27f-32c3-4e76-abb9-ea768d01747f
ms.service: security
ms.topic: article
ms.date: 07/31/2018
ms.author: jomolesk
ms.openlocfilehash: b30094e264086f018acbf84144300df46c60ac4e
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2019
ms.locfileid: "57241320"
---
# <a name="azure-security-and-compliance-blueprint---iaas-web-application-for-nist-sp-800-171"></a>Azure-beveiliging en naleving blauwdruk - IaaS-webtoepassing voor SP NIST 800-171

## <a name="overview"></a>Overzicht
[NIST Special Publication 800-171](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-171.pdf) bevat richtlijnen voor het beveiligen van de gecontroleerde niet-geclassificeerde informatie (CUI) die deel uitmaakt van nonfederal informatiesystemen en organisaties. SP NIST 800-171 stelt 14 families van de beveiligingsvereisten voor het beveiligen van de vertrouwelijkheid van CUI.

Deze Azure-beveiliging en naleving blauwdruk biedt hulp bij het implementeren van een architectuur voor webtoepassingen in Azure waarmee een subset van SP NIST 800-171-besturingselementen klanten. Deze oplossing toont manieren waarin klanten kunnen voldoen aan bepaalde beveiligings- en nalevingsvereisten. Het fungeert ook als basis voor klanten om te bouwen en hun eigen web-apps configureren in Azure.

Deze referentiearchitectuur, gekoppelde Implementatiehandleiding en risicomodel zijn bedoeld om te fungeren als een basis voor klanten aan te passen aan hun specifieke wensen. Ze al dan niet mogen worden gebruikt als-is in een productieomgeving. Er is onvoldoende om volledig te voldoen aan de vereisten van SP NIST 800-171 implementeren van deze architectuur zonder aanpassingen. Klanten zijn verantwoordelijk voor het uitvoeren van de juiste beveiliging en naleving evaluaties van een oplossing die is gebouwd met behulp van deze architectuur. Vereisten kunnen verschillen op basis van de details van de uitvoering van elke klant.

## <a name="architecture-diagram-and-components"></a>Diagram van architectuur en onderdelen
Deze Azure-beveiliging en naleving blauwdruk implementeert een referentiearchitectuur voor een IaaS-webtoepassing met een SQL Server-back-end. De architectuur bevat een weblaag, gegevenslaag, Active Directory-infrastructuur, Azure Application Gateway en Azure Load Balancer. Virtuele machines (VM) geïmplementeerd op het web- en gegevenslagen zijn geconfigureerd in een beschikbaarheidsset. SQL Server-exemplaren zijn geconfigureerd in de groep van een Always On-beschikbaarheid voor hoge beschikbaarheid. Virtuele machines worden toegevoegd aan een domein. Active Directory-groepsbeleid afdwingen voor beveiliging en naleving configuraties op het niveau van het besturingssysteem.

De volledige oplossing is gebouwd op Azure-opslag, waarmee klanten vanuit de Azure portal configureren. Opslag worden alle gegevens met Storage Service Encryption voor het onderhouden van de vertrouwelijkheid van gegevens in rust versleuteld. Geografisch redundante opslag zorgt ervoor dat een negatieve gebeurtenis in het primaire datacenter van de klant niet tot verlies van gegevens leiden. Een tweede kopie wordt opgeslagen op een afzonderlijke locatie honderden van mijl weg.

Voor een betere beveiliging, worden alle resources in deze oplossing worden beheerd als een resourcegroep via Azure Resource Manager. Azure Active Directory (Azure AD) op rollen gebaseerd toegangsbeheer (RBAC) wordt gebruikt voor het beheren van toegang tot de geïmplementeerde resources en sleutels in Azure Key Vault. Status van het bestandssysteem wordt bewaakt met Azure Monitor. Klanten configureren beide bewakingsservices voor het vastleggen van Logboeken. Status van het bestandssysteem wordt weergegeven in één dashboard dat is eenvoudig te gebruiken.

Een bastionhost management biedt een beveiligde verbinding voor beheerders voor toegang tot geïmplementeerd resources. *Microsoft raadt aan dat u een VPN of Azure ExpressRoute-verbinding configureren voor beheer en de gegevens importeren in het subnet van referentie-architectuur.*


![IaaS-webtoepassing voor SP NIST 800-171 verwijzing Architectuurdiagram](images/nist171-iaaswa-architecture.png "IaaS-webtoepassing voor Architectuurdiagram van SP NIST 800-171-verwijzing")

Deze oplossing maakt gebruik van de volgende Azure-services. Zie voor meer informatie de [architectuur](#deployment-architecture) sectie.

- Azure Virtual Machines
    - (1) management/bastionhost (Windows Server 2016 Datacenter)
    - (2) active Directory-domeincontroller (Windows Server 2016 Datacenter)
    - (2) SQL Server-clusterknooppunt (SQL Server 2017 voor Windows Server 2016)
    - (2) web/IIS (Windows Server 2016 Datacenter)
- Azure Virtual Network
    - (1) /16 netwerk
    - (5) /24 netwerken
    - (5) netwerkbeveiligingsgroepen
- Beschikbaarheidssets
    - (1) active Directory-domeincontrollers
    - (1) SQL-clusterknooppunten
    - (1) web/IIS
- Azure Application Gateway
    - (1) de web application firewall
        - Firewallmodus: preventie
        - regel is ingesteld: OWASP 3.0
        - -Listener-poort: 443
- Azure Active Directory
- Azure Key Vault
- Azure Load Balancer
- Azure Monitor (Logboeken)
- Azure Resource Manager
- Azure Security Center
- Azure Storage
- Azure Automation
- Cloud-Witness
- Recovery Services-kluis

## <a name="deployment-architecture"></a>Implementatie-architectuur
De volgende sectie bevat de elementen van de implementatie en uitvoering.

**Bastionhost**: De bastionhost is de enkel ingangspunt die gebruikers gebruiken kunnen voor toegang tot de geïmplementeerde resources in deze omgeving. De bastionhost biedt een beveiligde verbinding met de geïmplementeerde resources door alleen extern verkeer vanaf openbare IP-adressen op een veilige lijst. Als u wilt toestaan dat verkeer van extern bureaublad, moet de oorzaak van het verkeer worden gedefinieerd in de netwerkbeveiligingsgroep (NSG).

Deze oplossing maakt u een virtuele machine als een domein bastionhost met de volgende configuraties:
-   [Antimalware-uitbreiding](https://docs.microsoft.com/azure/security/azure-security-antimalware).
-   [Azure Diagnostics-extensie](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template).
-   [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) met Key Vault.
-   Een [beleid voor automatisch afsluiten](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) verkleind van VM-resources als deze niet in gebruik.
-   [Windows Defender Credential Guard](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard) zodat referenties en andere geheimen worden uitgevoerd in een beveiligde omgeving die is geïsoleerd van het besturingssysteem is ingeschakeld.

### <a name="virtual-network"></a>Virtueel netwerk
De architectuur definieert een particulier virtueel netwerk met een adresruimte van 10.200.0.0/16.

**Netwerkbeveiligingsgroepen**: Deze oplossing wordt geïmplementeerd resources in een architectuur met afzonderlijke subnetten voor web, database, Active Directory en beheer binnen een virtueel netwerk. Subnetten worden logisch van elkaar gescheiden door NSG-regels toegepast op de afzonderlijke subnetten. De regels beperken het verkeer tussen subnetten die alleen dat nodig is voor het systeem en de beheerfunctionaliteit.

Zie de configuratie voor [nsg's](https://github.com/Azure/fedramp-iaas-webapp/blob/master/nestedtemplates/virtualNetworkNSG.json) met deze oplossing wordt geïmplementeerd. Organisaties kunnen nsg's configureren door het vorige bestand bewerken met behulp van [deze documentatie](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) als richtlijn.

Elk van de subnetten heeft een NSG toegewezen:
- Een NSG voor Application Gateway (LBNSG)
- Een NSG voor bastionhost (MGTNSG)
- Een NSG voor primaire en back-domeincontrollers (ADNSG)
- Een NSG voor SQL-Servers en Cloud-Witness (SQLNSG)
- Een NSG voor weblaag (WEBNSG)

### <a name="data-in-transit"></a>Gegevens die onderweg zijn
Alle communicatie naar en vanuit Azure-datacenters versleutelt Azure standaard. Bovendien plaatsvinden alle transacties naar de opslag via de Azure-portal via HTTPS.

### <a name="data-at-rest"></a>Data-at-rest
De architectuur beveiligt gegevens in rust via meerdere metingen. Deze metingen zijn versleuteling en Databasecontrole.

**Azure Storage**: Om te voldoen aan vereisten voor versleutelde gegevens in rust, alle [opslag](https://azure.microsoft.com/services/storage/) maakt gebruik van [Storage Service Encryption](https://docs.microsoft.com/azure/storage/storage-service-encryption). Deze functie helpt beschermen en beveiligen van gegevens ter ondersteuning van de organisatie beveiligingsverplichtingen en nalevingsvereisten zijn gedefinieerd door de SP NIST 800-171.

**Azure Disk Encryption**: Schijfversleuteling wordt gebruikt om versleutelde Windows IaaS VM-schijven. [Schijfversleuteling](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) maakt gebruik van de BitLocker-functie van Windows voor volumeversleuteling voor het besturingssysteem en gegevensschijven. De oplossing is geïntegreerd met Key Vault voor het beheren en beheren van de versleutelingssleutels op de schijf.

**SQL Server**: De SQL Server-exemplaar maakt gebruik van de volgende metingen van de database-beveiliging:
-   [Controle van SQL Server](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine?view=sql-server-2017) databasegebeurtenissen en schrijft ze auditlogboeken.
-   [Transparante gegevensversleuteling](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017) voert realtime versleuteling en ontsleuteling van de database, gekoppelde back-ups en transactielogboekbestanden om gegevens in rust te beveiligen. Transparante gegevensversleuteling biedt de zekerheid dat de opgeslagen gegevens niet is onderworpen aan onbevoegde toegang.
-   [Firewall-regels](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) voorkomen dat u alle toegang tot de database-servers, totdat de juiste machtigingen zijn toegekend. De firewall verleent toegang tot databases op basis van het IP-adres waar de aanvraag vandaan komt.
-   [Versleutelde kolommen](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-wizard?view=sql-server-2017) ervoor te zorgen dat gevoelige gegevens nooit wordt weergegeven als tekst zonder opmaak in de database-systeem. Nadat versleuteling van gegevens is ingeschakeld, alleen clienttoepassingen of toepassingsservers met toegang tot de sleutels kunnen toegang tot gegevens van de tekst zonder opmaak.
- [Dynamische gegevensmaskering](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking?view=sql-server-2017) blootstelling van gevoelige gegevens maskeren van de gegevens naar de replicagegevens gebruikers of toepassingen. Het kan automatisch detecteren van potentieel gevoelige gegevens en voorstellen van de juiste maskers moet worden toegepast. Dynamische gegevensmaskering helpt bij de toegang beperken zodat gevoelige gegevens niet van de database via niet-gemachtigde toegang afsluiten. *Klanten zijn verantwoordelijk voor het aanpassen van de instellingen om te voldoen aan het schema van de database.*

### <a name="identity-management"></a>Identiteitsbeheer
De volgende technologieën bieden mogelijkheden voor het beheren van toegang tot gegevens in de Azure-omgeving:
-   [Azure AD](https://azure.microsoft.com/services/active-directory/) Microsofts multitenant cloud-gebaseerde directory en identity management-service is. Alle gebruikers voor deze oplossing worden gemaakt in Azure AD en de gebruikers die toegang hebben tot de SQL Server-exemplaar.
-   Verificatie van de toepassing wordt uitgevoerd met behulp van Azure AD. Zie voor meer informatie over het [toepassingen integreren met Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications).
-   [Azure RBAC](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) door beheerders kan worden gebruikt om te voorzien in specifieke toegangsmachtigingen te verlenen alleen de mate van toegang dat gebruikers moeten om hun werk te definiëren. In plaats van geeft elke onbeperkte gebruikersmachtigingen voor Azure-resources, beheerders kunnen toestaan dat alleen bepaalde acties voor toegang tot gegevens. Abonnementstoegang is beperkt tot de beheerder van het abonnement.
- [Azure Active Directory Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) door klanten kan worden gebruikt om het minimaliseren van het aantal gebruikers die toegang tot bepaalde resources hebben. Beheerders kunnen Azure AD Privileged Identity Management gebruiken om te detecteren, beperken en controleren van bevoegde identiteiten en hun toegang tot bronnen. Deze functionaliteit kan ook worden gebruikt om af te dwingen op aanvraag, just-in-time beheerderstoegang wanneer dat nodig is.
- [Azure Active Directory Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) detecteert mogelijke beveiligingsproblemen die invloed hebben op de identiteiten in een organisatie. Automatische antwoorden op gedetecteerde verdachte activiteit met betrekking tot een organisatie-id's configureren Ook onderzoekt het probleem verdachte incidenten passende actie op te lossen moet ondernemen.

### <a name="security"></a>Beveiliging
**Geheimen management**: De oplossing maakt gebruik van [Key Vault](https://azure.microsoft.com/services/key-vault/) voor het beheer van sleutels en geheimen. Key Vault helpt Beveilig cryptografische sleutels en geheimen die worden gebruikt door cloudtoepassingen en -services. De volgende mogelijkheden voor Key Vault zodat klanten gegevensbeveiliging:
- Geavanceerde beleidsregels zijn geconfigureerd op basis van behoefte.
- Toegangsbeleid voor Key Vault zijn met de minimaal vereiste machtigingen voor sleutels en geheimen gedefinieerd.
- Verloopdatum hebben alle sleutels en geheimen in Key Vault.
- Alle sleutels in Key Vault zijn beveiligd door gespecialiseerde hardware security modules. Het sleuteltype is een hardware security module beveiligde 2048-bits RSA-sleutel.
- Alle gebruikers en identiteiten zijn minimaal vereiste machtigingen verleend met behulp van RBAC.
- Diagnostische logboeken voor Key Vault worden ingeschakeld met een bewaarperiode van ten minste 365 dagen.
- Toegestane cryptografiebewerkingen voor sleutels zijn beperkt tot die nodig is.
- De oplossing is geïntegreerd met Key Vault voor het beheren van IaaS-VM-schijfversleuteling sleutels en geheimen.

**Patchbeheer**: Windows VM's geïmplementeerd als onderdeel van deze referentiearchitectuur zijn standaard geconfigureerd voor automatische updates ontvangen van Windows Update-Service. Deze oplossing bevat ook de [Azure Automation](https://docs.microsoft.com/azure/automation/automation-intro) service waarmee bijgewerkte implementaties kunnen worden gemaakt voor het patchen van virtuele machines wanneer dat nodig is.

**Bescherming tegen schadelijke software**: [Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) voor virtuele machines biedt de mogelijkheid van de realtime-beveiliging die helpt bij identificeren en virussen, spyware en andere schadelijke software verwijderen. Klanten kunnen waarschuwingen genereren wanneer bekende schadelijke of ongewenste software probeert te installeren of uitvoeren op de beveiligde virtuele machines configureren.

**Azure Security Center**: Met [Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro), klanten kunnen centraal toepassen en beheren van beveiligingsbeleid voor workloads, blootstelling aan bedreigingen beperken en detecteren en reageren op aanvallen. Security Center heeft ook toegang tot bestaande configuraties van Azure-services voor configuratie en de serviceaanbevelingen om u te helpen bij het beveiligingspostuur verbeteren en gegevens te beschermen.

Security Center maakt gebruik van tal van mogelijkheden voor het detecteren om te waarschuwen klanten van potentiële aanvallen die zijn gericht hun omgevingen. Deze waarschuwingen bevatten waardevolle informatie over de trigger van de waarschuwing, de betrokken resources en de bron van de aanval. Security Center bevat een set [vooraf gedefinieerde beveiligingswaarschuwingen](https://docs.microsoft.com/azure/security-center/security-center-alerts-type) die worden geactiveerd wanneer een bedreiging of verdachte activiteit wordt gedetecteerd. Klanten kunnen gebruikmaken van [aangepaste waarschuwingsregels](https://docs.microsoft.com/azure/security-center/security-center-custom-alert) voor het definiëren van nieuwe beveiligingswaarschuwingen op basis van gegevens die al zijn verzameld voor hun omgeving.

Security Center biedt beveiligingswaarschuwingen en incidenten. Security Center kunt u eenvoudiger voor klanten om te detecteren en mogelijke beveiligingsproblemen te verhelpen. Een [threat intelligence-rapport](https://docs.microsoft.com/azure/security-center/security-center-threat-report) wordt gegenereerd voor elke bedreigingen gedetecteerde. Incident response teams kunnen de rapporten gebruiken wanneer ze onderzoeken en bedreigingen verhelpen.

Deze referentiearchitectuur gebruikt de [evaluatie van beveiligingsproblemen](https://docs.microsoft.com/azure/security-center/security-center-vulnerability-assessment-recommendations) mogelijkheden in Security Center. Nadat deze geconfigureerd, rapporteert de agent van een partner (bijvoorbeeld Qualys) gegevens over beveiligingsproblemen naar het beheerplatform van de partner. Het beheerplatform van de partner rapporteert op zijn beurt weer controlegegevens over beveiligingsproblemen en status naar Security Center. Klanten kunnen deze informatie gebruiken om snel te identificeren kwetsbare VM's.

**Azure Application Gateway**: De architectuur vermindert het risico van beveiligingsproblemen met behulp van een toepassingsgateway met een web application firewall is geconfigureerd en de regelset OWASP is ingeschakeld. Aanvullende mogelijkheden zijn onder andere:

- [End-to-end-SSL](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell).
- Schakel [SSL-offload](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-portal).
- Uitschakelen [TLS v1.0 en v1.1](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell).
- [De Web application firewall](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) (preventiemodus).
- [Preventiemodus](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal) met OWASP 3.0 regelset.
- Schakel [Diagnostische logboekregistratie](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics).
- [Aangepaste statuscontroles](https://docs.microsoft.com/azure/application-gateway/application-gateway-create-gateway-portal).
- [Security Center](https://azure.microsoft.com/services/security-center) en [Azure Advisor](https://docs.microsoft.com/azure/advisor/advisor-security-recommendations) bieden extra beveiliging en meldingen. Security Center biedt ook een reputatie-systeem.

### <a name="business-continuity"></a>Bedrijfscontinuïteit

**Hoge beschikbaarheid**: De oplossing wordt geïmplementeerd voor alle virtuele machines in een [beschikbaarheidsset](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets). Beschikbaarheidssets zorgen ervoor dat de virtuele machines worden verdeeld over meerdere geïsoleerde hardwareclusters beschikbaarheid verbeteren. Ten minste één virtuele machine beschikbaar is tijdens gepland of ongepland onderhoud, die voldoet aan de 99,95% Azure SLA.

**Recovery Services-kluis**: De [Recovery Services-kluis](https://docs.microsoft.com/azure/backup/backup-azure-recovery-services-vault-overview) -instellingen van de back-upgegevens en alle configuraties van Azure Virtual Machines in deze architectuur beveiligt. Met een Recovery Services-kluis klanten kunnen bestanden en mappen terugzetten vanuit een IaaS-VM zonder het herstellen van de volledige virtuele machine. Dit proces sneller hersteltijden.

**Cloud-Witness**: [Cloud-Witness](https://docs.microsoft.com/windows-server/failover-clustering/whats-new-in-failover-clustering#BKMK_CloudWitness) is een failover-clusterquorum-witness in Windows Server 2016 die Azure als arbitragepunt gebruikt. Cloud-Witness, net als elke andere quorumwitness een stem opgehaald en kan deelnemen aan de quorumberekeningen. De standaard, openbaar beschikbare Azure Blob-opslag wordt gebruikt. Deze benadering wordt voorkomen dat de overhead extra onderhoud van virtuele machines die worden gehost in een openbare cloud.

### <a name="logging-and-auditing"></a>Logboekregistratie en controle

Azure-services zich uitgebreid systeem en activiteit van gebruikers, evenals systeemstatus:
- **Activiteitenlogboeken**: [Activiteitenlogboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bieden inzicht in bewerkingen die worden uitgevoerd op resources in een abonnement. Activiteitenlogboeken kunnen u eenvoudiger bepalen van een bewerking initiator, tijdstip van de gebeurtenis en status.
- **Diagnostische logboeken**: [Diagnostische logboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) bevatten alle logboeken die zijn gegenereerd door elke resource. Deze logboeken bevatten de logboeken voor Windows-systeem, opslaglogboeken, Key Vault-auditlogboeken en Application Gateway toegangs- en firewall-Logboeken. Alle logboeken met diagnostische gegevens schrijven naar een gecentraliseerd en versleutelde Azure storage-account voor archivering. Gebruikers kunnen de bewaartermijn maximaal configureren tot 730 dagen, om te voldoen aan hun specifieke wensen.

**Logboeken in Azure Monitor**: Deze logboeken worden geconsolideerd [logboeken van Azure Monitor](https://azure.microsoft.com/services/log-analytics/) voor verwerking, opslag en -dashboardrapporten. Nadat gegevens zijn verzameld, onderverdeeld in afzonderlijke tabellen voor elk gegevenstype in Log Analytics-werkruimten. Op deze manier kunnen alle gegevens samen kunnen worden geanalyseerd, ongeacht de oorspronkelijke bron. Security Center kan worden geïntegreerd met Azure Monitor-Logboeken. Klanten kunnen Kusto-query's gebruiken voor toegang tot hun beveiligingsgegevens voor de gebeurtenis en combineren met gegevens van andere services.

De volgende Azure [bewakingsoplossingen](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions) zijn opgenomen als onderdeel van deze architectuur:
-   [Active Directory-evaluatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): De oplossing statuscontrole van Active Directory beoordeelt het risico en de gezondheid van server-omgevingen op een vast interval. Het biedt een geprioriteerde lijst met aanbevelingen die specifiek zijn voor de geïmplementeerde serverinfrastructuur.
- [SQL-evaluatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): De oplossing SQL-statuscontrole beoordeelt het risico en de gezondheid van server-omgevingen op een vast interval. Daardoor kunnen klanten met een geprioriteerde lijst met aanbevelingen die specifiek zijn voor de geïmplementeerde serverinfrastructuur.
- [Status van agent](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): De oplossing status van Agent rapporteert het aantal agents zijn geïmplementeerd en hun geografische verdeling. Deze rapporten ook het aantal agents zijn niet meer reageert en het aantal agents dat operationele gegevens verzendt.
-   [Activity Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): De oplossing Activity Log Analytics biedt ondersteuning voor analyse van de Azure-activiteitenlogboeken voor alle Azure-abonnementen voor een klant.

**Azure Automation**: [Automation](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker) worden opgeslagen, wordt uitgevoerd runbooks worden beheerd. In deze oplossing te runbooks verzamelen van Logboeken van SQL Server. Klanten kunnen gebruikmaken van de automatisering [bijhouden](https://docs.microsoft.com/azure/automation/automation-change-tracking) oplossing voor het herkennen van wijzigingen in de omgeving.

**Azure Monitor**: [Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) helpt gebruikers bij het bijhouden van prestaties, veiligheid en trends te identificeren. Organisaties kunnen deze gebruiken om te controleren, waarschuwingen maken en archiveren van gegevens. Ze kunnen ook API-aanroepen in hun Azure-resources bijhouden.

## <a name="threat-model"></a>Risicomodel

Het diagram van de gegevensstroom voor deze referentiearchitectuur is beschikbaar voor [downloaden](https://aka.ms/nist171-iaaswa-tm) of kunt u hier vinden. Dit model kunt klanten inzicht krijgen in de punten van de mogelijke risico's in de infrastructuur van system wanneer deze wijzigingen aanbrengt.

![IaaS-webtoepassing voor SP NIST 800-171 risicomodel](images/nist171-iaaswa-threat-model.png "IaaS-webtoepassing voor SP NIST 800-171 risicomodel")

## <a name="compliance-documentation"></a>Naleving-documentatie

De [Azure-beveiliging en naleving blauwdruk - SP NIST 800-171 klant verantwoordelijkheid Matrix](https://aka.ms/nist171-crm) geeft een lijst van alle beveiligingsmaatregelen die zijn vereist voor SP NIST 800-171. Deze matrix bevat of de implementatie van elk besturingselement de verantwoordelijkheid van Microsoft, de klant is, of gedeeld tussen de twee.

De [Azure-beveiliging en naleving blauwdruk - SP NIST 800-171 IaaS Web Application Control-implementatie Matrix](https://aka.ms/nist171-iaaswa-cim) bevat informatie over welke SP NIST 800-171-besturingselementen worden verholpen door de architectuur voor webtoepassingen IaaS. Dit omvat gedetailleerde beschrijvingen van de manier waarop de implementatie voldoet aan de vereisten van elk besturingselement vallen.

## <a name="guidance-and-recommendations"></a>Richtlijnen en aanbevelingen
### <a name="vpn-and-expressroute"></a>VPN en ExpressRoute
Een beveiligde VPN-tunnel of [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) moet worden geconfigureerd voor een veilig verbinding met de resources die zijn geïmplementeerd als onderdeel van deze referentiearchitectuur web IaaS. Door op de juiste wijze instellen van een VPN of ExpressRoute, toevoegen klanten een beveiligingslaag voor gegevens in transit.

Door het implementeren van een beveiligde VPN-tunnel met Azure, kan een virtuele particuliere verbinding tussen een on-premises netwerk en een Azure-netwerk worden gemaakt. Deze verbinding vindt plaats via Internet. Klanten kunnen deze verbinding met een veilig 'tunnel' gegevens in een gecodeerde verbinding tussen de klant netwerk en Azure gebruiken. Site-naar-site VPN is een veilige, volwassen technologie die is geïmplementeerd door bedrijven van alle groottes decennia. De [IPsec-tunnelmodus](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) bij deze optie als een versleutelingsmechanisme voor wordt gebruikt.

Omdat het verkeer binnen de VPN-tunnel over Internet met een site-naar-site-VPN wordt verzonden, biedt Microsoft een andere optie voor nog veiliger verbinding. ExpressRoute is een speciale WAN koppeling tussen Azure en een on-premises locatie of een Exchange-hostingprovider. ExpressRoute-verbindingen rechtstreeks verbinding maken met de klant telecommunicatie-provider. Als gevolg hiervan de gegevens niet via Internet worden verzonden en wordt niet blootgesteld aan het. Deze verbindingen bieden meer betrouwbaarheid, hogere snelheden, kortere wachttijden en hogere beveiliging dan gewone verbindingen. 

Aanbevolen procedures voor het implementeren van een beveiligd hybride netwerk dat een on-premises netwerk naar Azure uitbreidt zijn [beschikbaar](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

## <a name="disclaimer"></a>Vrijwaring

- Dit document is uitsluitend ter informatie bedoeld. MICROSOFT BIEDT GEEN GARANTIES, EXPLICIETE, IMPLICIETE OF WETTELIJKE GARANTIE VOOR DE INFORMATIE IN DIT DOCUMENT. Dit document wordt geleverd ' as-is. " Informatie en meningen in dit document, inclusief URL's en andere websiteverwijzingen, kunnen zonder kennisgeving worden gewijzigd. Klanten die in dit document leest draagt het risico van het gebruik ervan. 
- Dit document biedt klanten met een enkel wettelijk recht op enig intellectueel in andere Microsoft-producten of oplossingen. 
- Klanten kunnen kopiëren en gebruiken van dit document voor interne referentiedoeleinden. 
- Bepaalde aanbevelingen in dit document kunnen leiden tot grotere hoeveelheden gegevens, netwerk- of gebruik van de compute-bronnen in Azure en de Azure-licentie of abonnement kosten van een klant kunnen verhogen. 
- Deze architectuur is bedoeld om te fungeren als een basis voor klanten om aan te passen op hun specifieke behoeften en mag niet worden gebruikt als-is in een productieomgeving.
- Dit document is ontwikkeld als referentie en mag niet worden gebruikt voor het definiëren van alle middelen waarmee een klant kan voldoen aan specifieke nalevingsvereisten en voorschriften. Klanten moeten juridische ondersteuning van hun organisatie op goedgekeurde klantimplementaties gezocht.
