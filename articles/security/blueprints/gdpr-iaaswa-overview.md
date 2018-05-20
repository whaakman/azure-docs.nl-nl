---
title: Azure-beveiliging en naleving blauwdruk - IaaS-webtoepassing voor GDPR
description: Azure-beveiliging en naleving blauwdruk - IaaS-webtoepassing voor GDPR
services: security
author: jomolesk
ms.assetid: 04d5239c-fff0-4c2d-9379-1fa79ddbec78
ms.service: security
ms.topic: article
ms.date: 05/14/2018
ms.author: jomolesk
ms.openlocfilehash: c418664fe94ee2801a24df59b9ef3451f3985cdb
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/14/2018
---
# <a name="azure-security-and-compliance-blueprint---iaas-web-application-for-gdpr"></a>Azure-beveiliging en naleving blauwdruk - IaaS-webtoepassing voor GDPR

## <a name="overview"></a>Overzicht
De algemene gegevens beveiliging regelgeving (GDPR) bevat veel vereisten over het verzamelen, opslaan en gebruiken van persoonlijke gegevens, inclusief hoe organisaties identificeren en beveiligen van persoonlijke gegevens, aangepast aan de vereisten voor transparantie, detecteren en rapporteren persoonlijke gegevens inbreuk en train privacy personeel en andere werknemers. De GDPR personen biedt meer controle over hun persoonlijke gegevens en legt veel nieuwe verplichtingen in organisaties die verzamelen, verwerkt of persoonlijke gegevens te analyseren. De GDPR legt nieuwe regels voor organisaties die goederen bieden en services naar mensen in de Europese Unie, of dat verzamelen en analyseren van gegevens die zijn gekoppeld aan de EU inwoners. De GDPR is van toepassing ongeacht waar een organisatie zich bevinden.

Microsoft heeft Azure met toonaangevende beveiligingsmaatregelen en het privacybeleid ter bescherming van gegevens in de cloud, met inbegrip van de categorieën van persoonlijke gegevens die zijn geïdentificeerd door de GDPR ontworpen. Microsoft [contractvoorwaarden](http://aka.ms/Online-Services-Terms) Microsoft door te voeren op de vereisten van de processors.

Deze Azure-beveiliging en naleving blauwdruk biedt richtlijnen voor het implementeren van een infrastructuur als een geschikt is voor een eenvoudige webtoepassing voor internetgerichte service (IaaS)-omgeving. Deze oplossing laat zien waarin klanten kunnen voldoen aan specifieke vereisten voor de beveiliging en naleving van de GDPR manieren en fungeert als basis voor klanten om te bouwen en hun eigen oplossingen voor IaaS web toepassingen configureren in Azure. Klanten kunnen gebruikmaken van deze verwijzende architectuur en voer de Microsoft [vier stappen proces](https://aka.ms/gdprebook) in hun reis GDPR naleving:
1. Detecteren: Identificeer welke persoonsgegevens bestaat en waar deze zich bevindt.
2. Beheren: Bepalen hoe persoonlijke gegevens wordt gebruikt en gebruikt.
3. Beveiligen: Tot stand brengen beveiligingsmaatregelen als u wilt detecteren, voorkomen van en reageren op beveiligingsproblemen en inbreuken op de gegevens.
4. Rapport: Vereiste documentatie houden en beheren van aanvragen voor gegevens en meldingen schenden.

Deze referentiearchitectuur, gekoppelde Implementatiehandleiding en risicomodel zijn bedoeld om te fungeren als basis voor klanten aan te passen aan hun specifieke vereisten en mag niet worden gebruikt als-is in een productieomgeving. Houd rekening met het volgende:
- De architectuur biedt een basis zodat klanten werkbelastingen in een GDPR-compatibele manier implementeren in Azure.
- Klanten zijn verantwoordelijk voor het uitvoeren van de juiste beveiligingsrechten en beoordeling van naleving van een oplossing die zijn gebouwd met behulp van deze architectuur als vereisten kunnen variëren op basis van de details van de implementatie van de klant.

## <a name="architecture-diagram-and-components"></a>Architectuurdiagram en onderdelen
Deze oplossing implementeert een referentiearchitectuur voor een IaaS-webtoepassing met een SQL Server-back-end. De architectuur bevat een weblaag, gegevenslaag, Active Directory-infrastructuur, Application Gateway en Load Balancer. Virtuele machines die zijn geïmplementeerd op de web- en lagen zijn geconfigureerd in een beschikbaarheidsset en SQL Server-exemplaren zijn geconfigureerd in een AlwaysOn-beschikbaarheidsgroep voor hoge beschikbaarheid. Virtuele machines zijn lid van een domein en Active Directory-groepsbeleid worden gebruikt voor het afdwingen van beveiliging en naleving configuraties op het niveau van het besturingssysteem. Een host van de bastionomgeving management biedt een beveiligde verbinding voor beheerders voor geïmplementeerd toegang tot bronnen. **Azure raadt aan om een VPN- of ExpressRoute-verbinding voor beheer en de gegevens importeren in het subnet van de architectuur verwijzing configureren.**

![IaaS Web Applicaiton voor GDPR verwijzing Architectuurdiagram](images/gdpr-iaaswa-architecture.png?raw=true "IaaS Web Applicaiton voor GDPR reference architecture-diagram")

Deze oplossing gebruikt de volgende Azure-services. Details van de architectuur voor implementatie bevinden zich in de [architectuur voor implementatie](#deployment-architecture) sectie.

- Azure Virtual Machines
    - (1) management/bastion (Windows Server 2016 Datacenter)
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
- Cloud-Witness
- Recovery Services-kluis
- Azure Key Vault
- Azure Active Directory (AAD)
- Azure Resource Manager
- Operations Management Suite (OMS)
- Azure Security Center

## <a name="deployment-architecture"></a>Architectuur voor implementatie
De volgende sectie wordt de implementatie en de implementatie-elementen.

**Host van de bastionomgeving**: de host van de bastionomgeving is de aanspreekpunt waarmee gebruikers toegang krijgen tot de geïmplementeerde resources in deze omgeving. De host van de bastionomgeving biedt een beveiligde verbinding met de geïmplementeerde resources door alleen het externe verkeer van openbare IP-adressen toe te staan op een veilige lijst. Extern bureaublad (RDP)-verkeer toestaan, moet de bron van het verkeer worden gedefinieerd in de netwerkbeveiligingsgroep (NSG).

Deze oplossing maakt een virtuele machine als een host bastion domein met de volgende configuraties:
-   [Antimalware-uitbreiding](https://docs.microsoft.com/azure/security/azure-security-antimalware)
-   [OMS-uitbreiding](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-oms)
-   [Azure extensie voor diagnostische gegevens](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template)
-   [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) met Azure Key Vault
-   Een [automatisch afsluiten beleid](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) gebruik kan worden verkleind van bronnen voor virtuele machines wanneer deze niet in gebruik
-   [Windows Defender referentie Guard](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard) ingeschakeld zodat referenties en andere geheimen in een beveiligde omgeving die geïsoleerd van het besturingssysteem wordt uitgevoerd

### <a name="virtual-network"></a>Virtueel netwerk
De architectuur definieert een persoonlijke virtueel netwerk met een adresruimte van 10.200.0.0/16.

**Netwerkbeveiligingsgroepen**: deze oplossing implementeert bronnen in een architectuur met een afzonderlijke web subnet, database-subnet, Active Directory-subnet en management subnet binnen een virtueel netwerk. Subnetten worden logisch gescheiden door netwerkbeveiligingsgroepen toegepast op de afzonderlijke subnetten voor verkeer tussen subnetten alleen noodzakelijke systeem en de beheerfunctionaliteit te beperken.

Zie de configuratie voor [netwerkbeveiligingsgroepen](https://github.com/Azure/fedramp-iaas-webapp/blob/master/nestedtemplates/virtualNetworkNSG.json) geïmplementeerd met deze oplossing. Organisaties kunnen netwerk-beveiligingsgroepen configureren door het bewerken van het bestand dan het gebruik van [deze documentatie](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) als richtlijn.

Elk van de subnetten heeft een speciale netwerkbeveiligingsgroep (NSG):
- 1 NSG voor Application Gateway (LBNSG)
- 1 NSG voor de bastionomgeving host (MGTNSG)
- 1 NSG voor primaire en back-domeincontrollers (ADNSG)
- 1 NSG voor SQL-Servers en Cloud-Witness (SQLNSG)
- 1 NSG voor weblaag (WEBNSG)

### <a name="data-in-transit"></a>Gegevens die onderweg
Azure versleutelt alle communicatie naar en van Azure-datacenters standaard. Bovendien worden alle transacties naar Azure Storage via de Azure portal via HTTPS optreden.

### <a name="data-at-rest"></a>Data-at-rest
De architectuur beveiligt de gegevens in rust via meerdere metingen, versleuteling en Databasecontrole.

**Azure Storage**: om te voldoen aan de versleutelde gegevens op de rest-vereisten, alle [Azure Storage](https://azure.microsoft.com/services/storage/) gebruikt [Opslagversleuteling Service](https://docs.microsoft.com/azure/storage/storage-service-encryption). Hierdoor kunnen beveiligen en bescherming van persoonlijke gegevens ter ondersteuning van de beveiliging van de organisatie verbintenissen en nalevingsvereisten gedefinieerd door de GDPR.

**Azure Disk Encryption**: Azure Disk Encryption versleuteld Windows IaaS-schijven voor virtuele machine wordt gebruikt. [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) maakt gebruik van de BitLocker-functie van Windows voor volumeversleuteling voor het besturingssysteem en gegevensschijven. De oplossing is geïntegreerd met Azure Key Vault voor het beheren en beheren van de versleutelingssleutels op de schijf.

**SQL Server**: de volgende veiligheidsmaatregelen voor de database maakt gebruik van de SQL Server-exemplaar:
-   [AD-verificatie en autorisatie](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) maakt identiteitsbeheer van databasegebruikers en andere Microsoft-services op één centrale locatie.
-   [SQL database auditing](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) houdt database gebeurtenissen en schrijft deze naar een auditlogboek Meld u bij een Azure storage-account.
-   SQL-Database is geconfigureerd voor gebruik [Transparent Data Encryption (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql), uitvoert, wordt er realtime versleuteling en ontsleuteling van de database, gekoppelde back-ups en transactielogboekbestanden om informatie op te beschermen rest. TDE biedt de zekerheid dat persoonlijke gegevens opgeslagen niet is onderworpen aan onbevoegde toegang.
-   [Firewall-regels](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) voorkomen dat u alle toegang tot de database-servers, totdat de juiste machtigingen worden toegekend. De firewall verleent toegang tot databases op basis van het IP-adres waar de aanvraag vandaan komt.
-   [Detectie van dreigingen SQL](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) kunt u de detectie en het antwoord op mogelijke bedreigingen wanneer deze zich voordoen doordat beveiligingswaarschuwingen voor verdachte databaseactiviteiten, mogelijke beveiligingsproblemen, SQL-injectieaanvallen en toegang tot de afwijkende database patronen.
-   [Always-gecodeerd kolommen](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) ervoor te zorgen dat gevoelige, persoonlijke gegevens nooit wordt weergegeven als leesbare tekst binnen het databasesysteem. Na het inschakelen van gegevensversleuteling alleen clienttoepassingen of toepassingsservers met toegang tot de sleutels hebben toegang tot gecodeerde gegevens.
- De [Extended Properties](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql) functie kan worden gebruikt om te stoppen met het verwerken van de betrokkenen, zoals gebruikers worden aangepaste eigenschappen toevoegen aan de database-objecten en gegevens labelen als 'uit 'assortiment ter ondersteuning van de toepassingslogica om te voorkomen dat de verwerking van de bijbehorende gegevens.
- [Beveiliging](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) kunnen gebruikers beleid om toegang te beperken voor het stoppen met het verwerken van gegevens definiëren.
- [SQL Database dynamische gegevens maskeren (DDM)](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) blootstelling gevoelige, persoonlijke gegevens wordt beperkt door het maskeren van de gegevens voor onbevoegde gebruikers of toepassingen. DDM kan automatisch detecteren van mogelijk gevoelige gegevens en voorstellen van de juiste maskers moet worden toegepast. Dit helpt bij de identificatie van persoonlijke gegevens die in aanmerking komen voor GDPR beveiliging, en voor het beperken van toegang zodat deze niet de database via toegang door onbevoegden wordt afgesloten. **Opmerking: Klanten moet DDM instellingen aanpassen om te voldoen aan het schema van de database.**

### <a name="identity-management"></a>Identiteitsbeheer
De volgende technologieën bieden mogelijkheden voor het beheer van toegang tot persoonlijke gegevens in de Azure-omgeving:
-   [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) van Microsoft multitenant cloud-gebaseerde directory en identity management-service is. Alle gebruikers voor deze oplossing worden gemaakt in AAD, met inbegrip van gebruikers die toegang tot de SQL-Server.
-   Verificatie van de toepassing wordt uitgevoerd met behulp van AAD. Zie voor meer informatie [toepassingen integreren met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Bovendien de versleuteling van de kolom database AAD voor verificatie van de toepassing naar SQL Server gebruikt. Zie voor meer informatie hoe [gevoelige gegevens in SQL-Database beveiligen](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   [Azure op rollen gebaseerde toegangsbeheer (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) kunnen beheerders voor het definiëren van verfijnde toegangsmachtigingen voor de hoeveelheid toegang verlenen dat gebruikers moeten uitvoeren van hun taken. In plaats van geeft elke onbeperkte gebruikersmachtigingen voor Azure-resources, kunnen beheerders toestaan om alleen bepaalde acties voor toegang tot persoonlijke gegevens. Abonnement toegang is beperkt tot abonnementsbeheerder.
- [AAD Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) kunnen klanten Beperk het aantal gebruikers die toegang tot bepaalde bronnen hebben.  Beheerders kunnen AAD Privileged Identity Management gebruiken om te detecteren, Beperk en bewaak bevoegde identiteiten en de toegang tot bronnen. Deze functionaliteit kan ook worden gebruikt om af te dwingen op aanvraag, just in time beheerderstoegang wanneer deze nodig is.
- [AAD Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) mogelijke beveiligingsproblemen die invloed hebben op een organisatie-id's detecteert, configureert u automatische antwoorden op gedetecteerde verdachte acties met betrekking tot een organisatie-id's en onderzoekt verdachte incidenten, onderneem gepaste actie deze op te lossen.
- Een geïmplementeerd exemplaar van Active Directory IaaS biedt identiteitsbeheer op de OS-niveau voor geïmplementeerde IaaS virtuele machines.

### <a name="security"></a>Beveiliging
**Geheimen management**: de oplossing maakt gebruik van [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) voor het beheer van sleutels en geheimen. Met Azure Sleutelkluis kunt u de cryptografische sleutels en geheimen beveiligen die door cloudtoepassingen en -services worden gebruikt. De volgende mogelijkheden van Azure Sleutelkluis waarmee klanten kunnen persoonlijke gegevens en toegang tot deze gegevens te beveiligen:
- Geavanceerde beleidsregels zijn geconfigureerd op basis van de nodig.
- Sleutelkluis-beleid worden met de minimaal vereiste machtigingen aan sleutels en geheimen gedefinieerd.
- Alle sleutels en geheimen in de Sleutelkluis hebben verloopdatum.
- Alle sleutels in de Sleutelkluis worden beschermd door speciale hardware security modules (HSM's). Het sleuteltype is een HSM beveiligde 2048-bits RSA-sleutel.
- Alle gebruikers en identiteiten krijgen minimaal vereiste machtigingen met RBAC.
- Diagnostische logboeken voor Sleutelkluis worden ingeschakeld met een bewaarperiode van ten minste 365 dagen.
- Toegestane cryptografische bewerkingen voor sleutels zijn beperkt tot de referenties die nodig is.
- De oplossing is geïntegreerd met Azure Key Vault voor het beheren van IaaS virtuele machine schijfversleuteling sleutels en geheimen.

**Beheer van patches**: Windows virtuele machines zijn geïmplementeerd als onderdeel van deze verwijzende architectuur zijn standaard geconfigureerd voor automatische updates ontvangen van Windows Update-Service. Deze oplossing bevat ook de OMS [Azure Automation](https://docs.microsoft.com/azure/automation/automation-intro) service waarmee bijgewerkte implementaties kunnen worden gemaakt met patch virtuele machines wanneer deze nodig is.

**Bescherming tegen schadelijke software**: [Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) voor virtuele Machines biedt de mogelijkheid dat helpt bij het identificeren en te verwijderen van virussen, spyware en andere schadelijke software, met configureerbare waarschuwingen van de realtime-beveiliging Wanneer bekende schadelijke of ongewenste software probeert te installeren of uitvoeren op de beveiligde virtuele machines.

**Beveiligingswaarschuwingen**: [Azure Security Center](https://docs.microsoft.com/en-us/azure/security-center/security-center-intro) kunnen klanten verkeer bewaken, verzamelen van Logboeken en gegevensbronnen voor bedreigingen analyseren. Azure Security Center heeft bovendien toegang tot bestaande configuratie van Azure services voor configuratie en serviceaanbevelingen om te helpen verbeteren beveiligingspostuur en persoonlijke gegevens te beschermen. Azure Security Center bevat een [dreiging intelligence-rapport](https://docs.microsoft.com/en-us/azure/security-center/security-center-threat-report) voor elk gedetecteerd threat om te helpen respons op incidenten teams onderzoeken en bedreigingen te herstellen.

**Toepassingsgateway**: de architectuur vermindert het risico van beveiligingsproblemen met behulp van een toepassingsgateway met web application firewall (WAF) en de ruleset OWASP ingeschakeld. Aanvullende mogelijkheden zijn:

- [End-to-End-SSL](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- Schakel [SSL-Offload](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-portal)
- Schakel [TLS v1.0 en v1.1](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- [Web application firewall](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) (WAF modus)
- [Preventie modus](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal) met OWASP 3.0 ruleset
- Schakel [logboekregistratie van diagnostische gegevens](https://docs.microsoft.com/en-us/azure/application-gateway/application-gateway-diagnostics)
- [Aangepaste statuscontroles](https://docs.microsoft.com/en-us/azure/application-gateway/application-gateway-create-gateway-portal)
- [Azure Security Center](https://azure.microsoft.com/services/security-center) en [Azure Advisor](https://docs.microsoft.com/en-us/azure/advisor/advisor-security-recommendations) bieden extra beveiliging en meldingen. Azure Security Center biedt ook een reputatie-systeem.

### <a name="business-continuity"></a>Bedrijfscontinuïteit

**Hoge beschikbaarheid**: de oplossing implementeert alle virtuele machines in een [Beschikbaarheidsset](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets). Beschikbaarheidssets Zorg ervoor dat de virtuele machines zijn verdeeld over meerdere geïsoleerde hardware clusters voor het verbeteren van beschikbaarheid. Ten minste één virtuele machine is beschikbaar tijdens gepland of ongepland onderhoud, voldoen aan de 99,95% Azure SLA.

**Recovery Services-kluis**: de [Recovery Services-kluis](https://docs.microsoft.com/azure/backup/backup-azure-recovery-services-vault-overview) -instellingen van de back-upgegevens en alle configuraties van Azure Virtual Machines in deze architectuur beveiligt. Met een Recovery Services-kluis klanten kunnen bestanden en mappen herstellen van een IaaS-VM zonder te herstellen van de hele virtuele machine, het inschakelen van sneller worden hersteld.

**Cloud Witness**: [Cloud Witness](https://docs.microsoft.com/en-us/windows-server/failover-clustering/whats-new-in-failover-clustering#BKMK_CloudWitness) is een soort quorumwitness Failover-Cluster in Windows Server 2016 die gebruikmaakt van Azure als het punt arbitrage. De Witness Cloud zoals elke andere quorumwitness een stem opgehaald en kunnen deelnemen aan de quorumberekeningen, maar het standaard openbaar Azure Blob Storage gebruikt. Hierdoor is de extra onderhoud overhead van virtuele machines die worden gehost in een openbare cloud.

### <a name="logging-and-auditing"></a>Logboekregistratie en controle

OMS biedt uitgebreide logboekregistratie van het systeem- en gebruikersactiviteit, evenals de systeemstatus. De OMS [logboekanalyse](https://azure.microsoft.com/services/log-analytics/) oplossing worden verzameld en analyseert gegevens die zijn gegenereerd voor resources in Azure en on-premises omgevingen.
- **Activiteitenlogboeken**: [activiteitenlogboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bieden inzicht in de bewerkingen die worden uitgevoerd op resources in een abonnement. Activiteitenlogboeken kunt u bepalen van de initiator een bewerking, tijd van het exemplaar en status.
- **Diagnostische logboeken**: [diagnostische logboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) omvatten alle logboeken die door elke resource. Deze logboeken bevatten Windows-gebeurtenislogboeken system, Azure Storage-logboeken Sleutelkluis controlelogboeken en Application Gateway toegang en firewall-Logboeken.
- **Logboek archiveren**: alle logboeken met diagnostische gegevens schrijven naar een gecentraliseerd en versleutelde Azure storage-account voor archivering. De bewaartermijn is gebruiker configureerbare, up, 730 dagen om te voldoen aan vereisten voor de organisatie-specifieke bewaarperiode. Deze logboeken verbinding maken met Azure Log Analytics voor verwerking, opslaan en dashboard reporting.

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

De gegevensstroom-diagram (GSD) voor deze verwijzende architectuur is beschikbaar voor [downloaden](https://aka.ms/gdprIaaSdfd) of vindt u hieronder. Dit model kan zodat klanten inzicht krijgen van de punten van de mogelijke risico's in de infrastructuur van het systeem als u wijzigingen aanbrengt.

![IaaS Web Applicaiton voor GDPR risicomodel](images/gdpr-iaaswa-threat-model.png?raw=true "IaaS Web Applicaiton voor GDPR risicomodel")

## <a name="compliance-documentation"></a>Documentatie voor naleving

De [Azure-beveiliging en naleving blauwdruk - GDPR klant verantwoordelijkheid Matrix](https://aka.ms/gdprCRM) controller en de processortijd verantwoordelijkheden voor alle GDPR artikelen bevat. Houd er rekening mee dat voor de Azure-services, een klant meestal de controller is en Microsoft als de processor fungeert.

De [Azure-beveiliging en naleving blauwdruk - GDPR IaaS Web Application-implementatie Matrix](https://aka.ms/gdprIaaSweb) bevat informatie over welke GDPR artikelen worden verholpen door de webtoepassing IaaS-architectuur, met inbegrip van gedetailleerde beschrijvingen van hoe is de implementatie voldoet aan de vereisten van elk gedekte artikel.

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
