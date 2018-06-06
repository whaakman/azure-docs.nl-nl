---
title: Azure-beveiliging en naleving blauwdruk - IaaS-webtoepassing voor FedRAMP
description: Azure-beveiliging en naleving blauwdruk - IaaS-webtoepassing voor FedRAMP
services: security
documentationcenter: na
author: jomolesk
manager: mbaldwin
editor: tomsh
ms.assetid: 8fe47cff-9c24-49e0-aa11-06ff9892a468
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/08/2018
ms.author: jomolesk
ms.openlocfilehash: 27346d8e2133ea61056817fb48050a4e5c8c3c97
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34726479"
---
# <a name="azure-security-and-compliance-blueprint-iaas-web-application-for-fedramp"></a>Azure-beveiliging en naleving blauwdruk: IaaS-webtoepassing voor FedRAMP

## <a name="overview"></a>Overzicht

De [Federal risico en autorisatie Management-programma (FedRAMP)](https://www.fedramp.gov) is een US government hele-programma dat biedt een gestandaardiseerde aanpak voor beoordeling van de veiligheid, autorisatie en doorlopende bewaking voor cloud-producten en Services. Deze Azure-beveiliging en naleving blauwdruk Automation biedt richtlijnen voor de implementatie van een FedRAMP-compatibele infrastructuur als een geschikt is voor een eenvoudige webtoepassing voor internetgerichte service (IaaS)-omgeving. Deze oplossing automatiseert implementatie en configuratie van Azure-resources voor een algemene referentiearchitectuur demonstreren van de manieren waarop klanten kunnen voldoen aan specifieke vereisten voor beveiliging en naleving en fungeert als basis voor klanten om op te bouwen en oplossingen voor hun eigen configureren in Azure. De oplossing implementeert een subset van besturingselementen van de basislijn FedRAMP hoge, op basis van NIST SP 800 53. Zie voor meer informatie over deze oplossing en FedRAMP de [naleving documentatie](#compliance-documentation).
> [!NOTE]
> Deze oplossing implementeert in Azure Government.

Deze Azure-beveiliging en naleving blauwdruk Automation automatisch wordt geïmplementeerd met een referentiearchitectuur IaaS web application met vooraf geconfigureerde beveiligingsmechanismen klanten helpen de naleving van voorschriften FedRAMP. De oplossing bestaat uit Azure Resource Manager-sjablonen en PowerShell-scripts die resource implementatie en configuratie leiden.

Deze architectuur is bedoeld om te fungeren als basis voor klanten om aan te passen aan hun specifieke vereisten en mag niet worden gebruikt als-is in een productieomgeving. Implementeren van een toepassing in deze omgeving zonder aanpassing is niet voldoende volledig voldoen aan de vereisten van de basislijn FedRAMP hoog. Houd rekening met het volgende:
- Deze architectuur biedt een basis zodat klanten Azure in een FedRAMP-compatibele manier gebruiken.
- Klanten zijn verantwoordelijk voor het uitvoeren van de juiste beveiligingsrechten en beoordeling van compatibiliteit van een oplossing die zijn gebouwd met behulp van deze architectuur als vereisten kunnen variëren op basis van de details van de implementatie van de klant.

Voor een snel overzicht van de werking van deze oplossing, bekijkt u deze [video](https://aka.ms/fedrampblueprintvideo) waarin wordt uitgelegd en demonstreren van de implementatie ervan.

Klik op [hier](https://aka.ms/fedrampblueprintrepo) voor implementatie-instructies.

## <a name="architecture-diagram-and-components"></a>Architectuurdiagram en onderdelen
Deze oplossing implementeert een referentiearchitectuur voor een IaaS-webtoepassing met een SQL Server-back-end. De architectuur bevat een weblaag, gegevenslaag, Active Directory-infrastructuur, Application Gateway en Load Balancer. Virtuele machines geïmplementeerd op de web- en lagen zijn geconfigureerd in een Beschikbaarheidsset en SQL Server-exemplaren zijn geconfigureerd in een AlwaysOn-beschikbaarheidsgroep voor hoge beschikbaarheid. Virtuele machines zijn lid van een domein en Active Directory-groepsbeleid worden gebruikt voor het afdwingen van beveiliging en naleving configuraties op het niveau van het besturingssysteem. Een host voor de bastionomgeving biedt een beveiligde verbinding voor beheerders voor geïmplementeerd toegang tot bronnen. **Azure raadt aan om een VPN of Azure ExpressRoute-verbinding voor beheer en de gegevens importeren in het subnet van de architectuur verwijzing configureren.**

![IaaS-webtoepassing voor FedRAMP verwijzing Architectuurdiagram](images/fedramp-iaaswa-architecture.png?raw=true "IaaS-webtoepassing voor FedRAMP reference architecture-diagram")

Deze oplossing gebruikt de volgende Azure-services. Details van de architectuur voor implementatie bevinden zich in de [architectuur voor implementatie](#deployment-architecture) sectie.

- Azure Virtual Machines
    - (1) bastion-host (Windows Server 2016 Datacenter)
    - (2) active Directory-domeincontroller (Windows Server 2016 Datacenter)
    - (2) de SQL Server-clusterknooppunt (SQL Server-2017 op Windows Server 2016)
    - (2) web/IIS (Windows Server 2016 Datacenter)
- Beschikbaarheidssets
    - (1) active Directory-domeincontrollers
    - (1) de SQL-clusterknooppunten
    - (1) web/IIS
- Azure Virtual Network
    - (((1) /16 virtuele netwerken
    - (5) /24 subnetten
    - DNS-instellingen zijn ingesteld op beide domeincontrollers
- Azure Load Balancer
- Azure Application Gateway
    - (1) WAF Application Gateway is ingeschakeld
        - firewallmodus: voorkomen
        - Regelset: OWASP 3.0
        - listener: poort 443
- Azure Storage
    - (7) Geo-redundant storage-accounts
- Azure-Cloud-Witness
- Recovery Services-kluis
- Azure Key Vault
- Azure Active Directory (Azure AD)
- Azure Resource Manager
- Operations Management Suite (OMS)
- Azure Monitor

## <a name="deployment-architecture"></a>Architectuur voor implementatie

De volgende sectie wordt de ontwikkeling en implementatie-elementen.

**Host van de bastionomgeving**: de host van de bastionomgeving is de aanspreekpunt waarmee een beveiligde verbinding voor beheerders voor geïmplementeerd toegang tot bronnen. De bastionomgeving van host NSG staat verbindingen alleen op TCP-poort 3389 voor RDP. Klanten kunnen de bastionomgeving host om te voldoen aan de organisatie system beperkingsvereisten verder configureren.

### <a name="virtual-network"></a>Virtueel netwerk
De architectuur definieert een persoonlijke virtueel netwerk met een adresruimte van 10.200.0.0/16.

**Netwerkbeveiligingsgroepen**: deze oplossing implementeert bronnen in een architectuur met een afzonderlijke web subnet, database-subnet, Active Directory-subnet en management subnet binnen een virtueel netwerk. Subnetten worden logisch gescheiden door netwerkbeveiligingsgroepen toegepast op de afzonderlijke subnetten voor verkeer tussen subnetten alleen noodzakelijke systeem en de beheerfunctionaliteit te beperken.

Zie de configuratie voor [netwerkbeveiligingsgroepen](https://github.com/Azure/fedramp-iaas-webapp/blob/master/nestedtemplates/virtualNetworkNSG.json) geïmplementeerd met deze oplossing. Klanten netwerkbeveiligingsgroepen kunnen configureren door het bewerken van het bestand dan het gebruik van [deze documentatie](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) als richtlijn.

Elk van de subnetten heeft een speciale netwerkbeveiligingsgroep (NSG):
- 1 NSG voor Application Gateway (LBNSG)
- 1 NSG voor de bastionomgeving host (MGTNSG)
- 1 NSG voor primaire en back-domeincontrollers (ADNSG)
- 1 NSG voor SQL-Servers (SQLNSG)
- 1 NSG voor weblaag (WEBNSG)

**Subnetten**: elk subnet is gekoppeld aan de bijbehorende NSG.

### <a name="data-at-rest"></a>Data-at-rest

De architectuur beveiligt de gegevens in rust met behulp van verschillende versleuteling metingen.

**Azure Storage**: om te voldoen aan versleutelingsvereisten voor de van gegevens in rust, het gebruik van alle opslagaccounts [Opslagversleuteling Service](https://docs.microsoft.com/azure/storage/common/storage-service-encryption).

**SQL Server**: SQL Server is geconfigureerd voor gebruik [Transparent Data Encryption (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption), uitvoert, wordt er realtime versleuteling en ontsleuteling van gegevens en logboekbestanden om gegevens in rust te beveiligen. TDE biedt de zekerheid dat de opgeslagen gegevens niet is onderworpen aan onbevoegde toegang.

Klanten kunnen ook de volgende SQL Server-veiligheidsmaatregelen configureren:
-   [AD-verificatie en autorisatie](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-aad-authentication) maakt identiteitsbeheer van databasegebruikers en andere Microsoft-services op één centrale locatie.
-   [SQL database auditing](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-auditing-get-started) houdt database gebeurtenissen en schrijft deze naar een auditlogboek Meld u bij een Azure storage-account.
-   [Firewall-regels](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-firewall-configure) voorkomen dat u alle toegang tot de database-servers, totdat de juiste machtigingen worden toegekend. De firewall verleent toegang tot databases op basis van het IP-adres waar de aanvraag vandaan komt.
-   [Detectie van dreigingen SQL](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-threat-detection-get-started) kunt u de detectie en het antwoord op mogelijke bedreigingen wanneer deze zich voordoen doordat beveiligingswaarschuwingen voor verdachte databaseactiviteiten, mogelijke beveiligingsproblemen, SQL-injectieaanvallen en toegang tot de afwijkende database patronen.
-   [Altijd versleutelde kolommen](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-always-encrypted-azure-key-vault) ervoor te zorgen dat gevoelige gegevens nooit wordt weergegeven als leesbare tekst binnen het databasesysteem. Na het inschakelen van gegevensversleuteling alleen clienttoepassingen of appservers met toegang tot de sleutels hebben toegang tot gecodeerde gegevens.
-   [SQL-Database dynamische-gegevensmaskering](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-dynamic-data-masking-get-started) kunnen worden uitgevoerd nadat de referentiearchitectuur implementeert. Klanten moeten dynamische gegevensmaskering instellingen om te voldoen aan hun databaseschema aanpassen.

**Azure Disk Encryption**: Azure Disk Encryption versleuteld Windows IaaS-schijven voor virtuele machine wordt gebruikt. [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) maakt gebruik van de BitLocker-functie van Windows voor volumeversleuteling voor het besturingssysteem en gegevensschijven. De oplossing is geïntegreerd met Azure Key Vault voor het beheren en beheren van de versleutelingssleutels op de schijf.

### <a name="identity-management"></a>Identiteitsbeheer

De volgende technologieën bieden identiteit beheermogelijkheden in de Azure-omgeving:
- [Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/) van Microsoft multitenant cloud-gebaseerde directory en identity management-service is.
- Verificatie voor een webtoepassing die door de klant geïmplementeerd kan worden uitgevoerd met behulp van Azure AD. Zie voor meer informatie [toepassingen integreren met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications).  
- [Azure op rollen gebaseerde toegangsbeheer (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) kunt nauwkeurig gerichte toegangsbeheer voor Azure. Abonnement toegang is beperkt tot abonnementsbeheerder en toegang tot bronnen worden beperkt op basis van gebruikersrol.
- Een geïmplementeerd exemplaar van Active Directory IaaS biedt identiteitsbeheer op de OS-niveau voor geïmplementeerde IaaS virtuele machines.

### <a name="security"></a>Beveiliging
**Geheimen management**: de oplossing maakt gebruik van [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) voor het beheer van sleutels en geheimen. Met Azure Sleutelkluis kunt u de cryptografische sleutels en geheimen beveiligen die door cloudtoepassingen en -services worden gebruikt. Azure Sleutelkluis kunt beheren IaaS virtuele machine schijfversleuteling sleutels en geheimen voor deze verwijzende architectuur.

**Beheer van patches**: Windows virtuele machines zijn geïmplementeerd door deze Azure-beveiliging en naleving blauwdruk automatisering zijn standaard geconfigureerd voor automatische updates ontvangen van Windows Update-Service. Deze oplossing implementeert ook de Azure Automation-oplossing waarmee Update-implementaties kunnen worden gemaakt voor het implementeren van patches op Windows-servers wanneer deze nodig is.

**Bescherming tegen schadelijke software**: [Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) voor virtuele Machines biedt de mogelijkheid dat helpt bij het identificeren en te verwijderen van virussen, spyware en andere schadelijke software, met configureerbare waarschuwingen van de realtime-beveiliging Wanneer bekende schadelijke of ongewenste software probeert te installeren of uitvoeren op de beveiligde virtuele machines.

**Toepassingsgateway**: de architectuur vermindert het risico van beveiligingsproblemen met behulp van een toepassingsgateway met web application firewall (WAF) en de ruleset OWASP ingeschakeld. Aanvullende mogelijkheden zijn:

- [End-to-End-SSL](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- Schakel [SSL-Offload](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-portal)
- Schakel [TLS v1.0 en v1.1](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- [Web application firewall](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) (WAF modus)
- [Preventie modus](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal) met OWASP 3.0 ruleset

### <a name="business-continuity"></a>Bedrijfscontinuïteit

**Hoge beschikbaarheid**: ten minste één virtuele machine is beschikbaar tijdens gepland of ongepland onderhoud, voldoen aan de 99,95% Azure SLA. De oplossing implementeert alle weblaag en gegevens virtuele machines in servicetier een [Beschikbaarheidsset](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets). Beschikbaarheidssets Zorg ervoor dat de virtuele machines zijn verdeeld over meerdere geïsoleerde hardware clusters voor het verbeteren van beschikbaarheid. Bovendien deze oplossing wordt geïmplementeerd met de SQL Server-virtuele machines in een Beschikbaarheidsset als een [AlwaysOn-beschikbaarheidsgroep](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-overview). De altijd op beschikbaarheid van groep functie biedt voor hoge beschikbaarheid en herstel na noodgevallen mogelijkheden.

**Recovery Services-kluis**: de [Recovery Services-kluis](https://docs.microsoft.com/azure/backup/backup-azure-recovery-services-vault-overview) -instellingen van de back-upgegevens en alle configuraties van Azure Virtual Machines in deze architectuur beveiligt. Met een Recovery Services-kluis klanten kunnen bestanden en mappen herstellen van een IaaS-VM zonder te herstellen van de hele virtuele machine, het inschakelen van sneller worden hersteld.

**Cloud Witness**: [Cloud Witness](https://docs.microsoft.com/en-us/windows-server/failover-clustering/whats-new-in-failover-clustering#BKMK_CloudWitness) is een soort quorumwitness Failover-Cluster in Windows Server 2016 die gebruikmaakt van Azure als het punt arbitrage. De Witness Cloud zoals elke andere quorumwitness een stem opgehaald en kunnen deelnemen aan de quorumberekeningen, maar het standaard openbaar Azure Blob Storage gebruikt. Hierdoor is de extra onderhoud overhead van virtuele machines die worden gehost in een openbare cloud.

### <a name="logging-and-auditing"></a>Logboekregistratie en controle

OMS biedt uitgebreide logboekregistratie van het systeem- en gebruikersactiviteit, evenals de systeemstatus. De OMS [logboekanalyse](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) oplossing worden verzameld en analyseert gegevens die zijn gegenereerd voor resources in Azure en on-premises omgevingen.

- **Activiteitenlogboeken:**[activiteitenlogboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bieden inzicht in de bewerkingen die worden uitgevoerd op resources in een abonnement. Activiteitenlogboeken kunt u bepalen van de initiator een bewerking, tijd van het exemplaar en status.
- **Diagnostische logboeken:**[diagnostische logboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) zijn alle logboeken die door elke resource. Deze logboeken bevatten Windows-gebeurtenislogboeken system, Azure-opslag logboeken Sleutelkluis controlelogboeken en Application Gateway toegang en firewall-Logboeken.
- **Logboek archiveren:** alle logboeken met diagnostische gegevens schrijven naar een gecentraliseerd en versleutelde Azure storage-account voor archivering. De bewaartermijn is gebruiker configureerbare, up, 730 dagen om te voldoen aan vereisten voor de organisatie-specifieke bewaarperiode. Deze logboeken verbinding maken met Azure Log Analytics voor verwerking, opslaan en dashboard reporting.

Bovendien worden de volgende OMS-oplossingen geïnstalleerd als onderdeel van deze architectuur. Houd er rekening mee dat het is de verantwoordelijkheid van de klant voor het configureren van deze oplossingen uitgelijnd met de beveiligingsmechanismen FedRAMP:
-   [AD-evaluatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): het Active Directory Health Check oplossing evalueert de risico's en de gezondheid van server-omgevingen met regelmatige tussenpozen en biedt een geprioriteerde lijst met aanbevelingen die specifiek zijn voor de geïmplementeerde serverinfrastructuur.
-   [Antimalware Assessment](https://docs.microsoft.com/azure/log-analytics/log-analytics-malware): de anti-malwareoplossing rapporten over de status van schadelijke software, bedreigingen en beveiliging.
-   [Azure Automation](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker): de Azure Automation-oplossing opgeslagen, wordt uitgevoerd en runbooks beheert.
-   [Beveiliging en Audit](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started): de beveiligings- en Audit dashboard biedt een op hoog niveau inzicht in de beveiligingsstatus van bronnen door te geven van metrische gegevens over beveiligingsdomeinen, problemen die aandacht vereisen, detecties, dreigingen en algemene beveiliging query's.
-   [SQL-evaluatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): de SQL-Serverstatus controleren oplossing evalueert de risico's en de gezondheid van server-omgevingen met regelmatige tussenpozen en biedt klanten een geprioriteerde lijst met aanbevelingen die specifiek zijn voor de geïmplementeerde serverinfrastructuur.
-   [Updatebeheer](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-update-management): oplossing voor het beheer van de Update kunt u beveiligingsupdates besturingssysteem, met inbegrip van de status van de beschikbare updates en het proces van het installeren van vereiste updates klant beheren.
-   [Status van agent](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): oplossing de status van de Agent rapporteert hoeveel agents zijn geïnstalleerd en hun geografische verdeling, evenals hoeveel agents die niet reageert en het aantal agents dat operationele gegevens wordt verzonden.
-   [Azure activiteitenlogboeken](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): de activiteit Log Analytics-oplossing biedt ondersteuning voor analyse van de Azure activiteitenlogboeken over alle Azure-abonnementen voor een klant.
-   [Het bijhouden van](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): de bijhouden oplossing kan klanten eenvoudig om wijzigingen te bepalen in de omgeving.

**Monitor voor Azure**
[Azure Monitor](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/) helpt gebruikers prestaties, beveiliging en trends voor doordat organisaties om te controleren, waarschuwingen maken en gegevens, zoals het bijhouden van de API-aanroepen te archiveren in klanten Azure-resources.

## <a name="threat-model"></a>Risicomodel
De gegevensstroom-diagram voor deze verwijzende architectuur is beschikbaar voor [downloaden](https://aka.ms/fedrampWAdfd) of vindt u hieronder. Dit model kan zodat klanten inzicht krijgen van de punten van de mogelijke risico's in de infrastructuur van het systeem als u wijzigingen aanbrengt.

![IaaS-webtoepassing voor risicomodel FedRAMP](images/fedramp-iaaswa-threat-model.png?raw=true "IaaS Web Applicaiton voor FedRAMP risicomodel")

## <a name="compliance-documentation"></a>Documentatie voor naleving

De [Azure-beveiliging en naleving blauwdruk - FedRAMP hoge klant verantwoordelijkheid Matrix](https://aka.ms/blueprinthighcrm) geeft een lijst van alle beveiligingsmechanismen zijn vereist voor de basislijn FedRAMP hoog. De matrix geeft aan of de implementatie van elk besturingselement de verantwoordelijkheid van Microsoft, de klant is of gedeeld tussen de twee.

De [Azure-beveiliging en naleving blauwdruk - FedRAMP IaaS Web Application hoge besturingselement implementatie Matrix](https://aka.ms/blueprintwacim) geeft een lijst van alle beveiligingsmechanismen zijn vereist voor de basislijn FedRAMP hoog. De matrix bevat informatie waarop besturingselementen worden gedekt door de IaaS web application architectuur, met inbegrip van gedetailleerde beschrijvingen van de manier waarop de implementatie voldoet aan de vereisten van elk gedekte besturingselement.

## <a name="deploy-the-solution"></a>De oplossing implementeren

Deze Azure-beveiliging en naleving blauwdruk Automation bestaat uit JSON-configuratiebestanden en PowerShell-scripts die worden verwerkt door Azure Resource Manager-API-service voor het implementeren van resources in Azure. Gedetailleerde implementatie-instructies zijn beschikbaar [hier](https://aka.ms/fedrampblueprintrepo).
> [!NOTE]
> Deze oplossing implementeert in Azure Government.

#### <a name="quickstart"></a>Snelstartgids
1. Klonen of downloaden [dit](https://aka.ms/fedrampblueprintrepo) GitHub-opslagplaats naar uw lokale werkstation.

2. Voer de PowerShell-script vóór implementatie: azure-blueprint/predeploy/Orchestration_InitialSetup.ps1.

3. Klik op de knop hieronder, meld u aan bij de Azure-portal, voer de vereiste parameters voor ARM-sjabloon en klikt u op **aankoop**.

    [![Implementeren in Azure](http://azuredeploy.net/AzureGov.png)](https://portal.azure.us/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Ffedramp-iaas-webapp%2Fmaster%2Fazuredeploy.json)

## <a name="guidance-and-recommendations"></a>Richtlijnen en aanbevelingen
### <a name="vpn-and-expressroute"></a>VPN- en ExpressRoute
Een beveiligde VPN-tunnel of [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) moet worden geconfigureerd om veilig verbinding maken met de resources die zijn geïmplementeerd als onderdeel van deze webtoepassing IaaS verwijzende architectuur. Door op de juiste wijze instellen van een VPN of ExpressRoute, toevoegen klanten een laag van beveiliging voor gegevens onderweg.

Door het implementeren van een beveiligde VPN-tunnel met Azure kan een virtuele particuliere verbinding tussen een on-premises netwerk en een Azure-netwerk worden gemaakt. Deze verbinding vindt plaats via het Internet en kan klanten veilig 'tunnel' gegevens in een versleutelde koppeling tussen het netwerk en Azure van de klant. Site-naar-Site VPN is een beveiligde, volwassen technologie die is geïmplementeerd door bedrijven van elke grootte jarenlang. De [IPsec-tunnelmodus](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) wordt gebruikt bij deze optie als een mechanisme voor versleuteling.

Omdat het verkeer binnen de VPN-tunnel Internet met een site-naar-site VPN passeren, biedt Microsoft met een andere, zelfs veiliger verbindingsoptie. Azure ExpressRoute is een speciale WAN koppeling tussen Azure en een on-premises locatie of een Exchange-hostingprovider. Als het ExpressRoute-verbindingen gaan niet via het Internet, bieden deze verbindingen meer betrouwbaarheid, sneller en hebben ze lagere latenties en betere beveiliging dan gewone verbindingen via Internet. Bovendien, omdat dit een directe verbinding van de klant telecommunicatie provider, de gegevens niet via Internet kan worden verzonden en daarom geen toegang heeft tot het.

Aanbevolen procedures voor het implementeren van een beveiligde hybride netwerk die uitgebreider is dan een on-premises netwerk naar Azure [beschikbaar](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

## <a name="disclaimer"></a>Vrijwaring

- Dit document is alleen ter informatie. MICROSOFT GEEFT GEEN GARANTIES, SNELLE, IMPLICIETE OF WETTELIJKE GARANTIE VOOR DE INFORMATIE IN DIT DOCUMENT. Dit document wordt geleverd ' as-is. " Informatie en inzichten die in dit document, inclusief URL's en andere websiteverwijzingen, kunnen zonder kennisgeving worden gewijzigd. Dit document lezen klanten draagt het risico voor het gebruik ervan.  
- Dit document biedt geen enkel wettelijk recht op enig intellectueel eigendom van alle Microsoft-product of oplossingen klanten.  
- Klanten kunnen kopiëren en gebruiken van dit document voor interne referentiedoeleinden.  
- Bepaalde aanbevelingen in dit document kunnen leiden tot hogere-, netwerk- of compute-Resourcegebruik in Azure en een klant Azure licentie of abonnement kosten kunnen verhogen.  
- Deze architectuur is bedoeld om te fungeren als basis voor klanten om aan te passen aan hun specifieke vereisten en mag niet worden gebruikt als-is in een productieomgeving.
- Dit document is ontwikkeld als een verwijzing en mag niet worden gebruikt voor het definiëren van alle middelen waarmee een klant kan voldoen aan specifieke nalevingsvereisten en -voorschriften. Klanten moeten juridische ondersteuning van hun organisatie met goedgekeurde klant implementaties zoeken.
