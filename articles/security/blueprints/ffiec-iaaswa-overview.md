---
title: Azure-beveiliging en naleving blauwdruk - IaaS-webtoepassing voor FFIEC
description: Azure-beveiliging en naleving blauwdruk - IaaS-webtoepassing voor FFIEC
services: security
author: meladie
ms.assetid: 8577e982-8bc0-4817-a16e-adf2ffefc6f5
ms.service: security
ms.topic: article
ms.date: 06/20/2018
ms.author: meladie
ms.openlocfilehash: 10d13e7dd145feff286b8dd58fa1bc657961e8c4
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2019
ms.locfileid: "57247559"
---
# <a name="azure-security-and-compliance-blueprint-iaas-web-application-for-ffiec-financial-services"></a>Azure-beveiliging en naleving blauwdruk: IaaS-webtoepassing voor de financiële dienstverlening FFIEC

## <a name="overview"></a>Overzicht

Deze Azure-beveiliging en naleving blauwdruk biedt richtlijnen voor de implementatie van een infrastructuur als een service (IaaS)-omgeving die geschikt is voor het verzamelen, opslaan en ophalen van de financiële gegevens die worden geregeld door de federale financiële instelling Onderzoek Raad (FFIEC).

Deze referentiearchitectuur, Implementatiehandleiding en risicomodel voorzien in een basis voor klanten om te voldoen aan de FFIEC. Deze oplossing biedt een basislijn waarmee klanten workloads in een FFIEC compatibel manier implementeren in Azure, maar deze oplossing moet niet worden gebruikt als-is in een productieomgeving omdat er aanvullende configuratie vereist is.

Bereiken FFIEC-naleving is vereist dat gekwalificeerde auditors, een oplossing voor productie-klant certificeren. De audits worden beheerd door onderzoekers van FFIEC van lid instanties, met inbegrip van de Raad van bestuur van de Federal Reserve System (FRB), het Federal aanbetaling Insurance Corporation (FDIC), de nationale Credit Union beheer (NCUA), het kantoor van de beheerscontrole de valuta (OCC) en de bescherming van financiële Bureau voor consumenten (CFPB). Deze onderzoekers verklaren dat controles worden uitgevoerd door de beoordelaars die onafhankelijk van de gecontroleerde instelling onderhouden. Klanten zijn verantwoordelijk voor het uitvoeren van de juiste beveiliging en naleving evaluaties van een oplossing die is gebouwd met behulp van deze architectuur als vereisten kunnen variëren op basis van de details van de uitvoering van elke klant.

## <a name="architecture-diagram-and-components"></a>Diagram van architectuur en onderdelen

Deze Azure-beveiliging en naleving blauwdruk implementeert een referentiearchitectuur voor een IaaS-webtoepassing met een SQL Server back-end. De architectuur bevat een weblaag, gegevenslaag, Active Directory-infrastructuur, Application Gateway en Load Balancer. Virtuele machines die zijn geïmplementeerd voor de web- en gegevenslagen zijn geconfigureerd in een beschikbaarheidsset en SQL Server-exemplaren zijn geconfigureerd in de groep van een Always On-beschikbaarheid voor hoge beschikbaarheid. Virtuele machines zijn domein en Active Directory-groepsbeleid worden gebruikt om af te dwingen van beveiliging en naleving configuraties op het niveau van het besturingssysteem.

De volledige oplossing is gebouwd op Azure-opslag, waarmee klanten vanuit de Azure portal configureren. Azure Storage worden alle gegevens met Storage Service Encryption voor het onderhouden van de vertrouwelijkheid van gegevens in rust versleuteld. Geografisch redundante opslag zorgt ervoor dat gegevens worden gerepliceerd naar een secundair datacenter honderden van mijl opslaan en opnieuw als drie kopieën binnen die datacenter opgeslagen, zo wordt voorkomen dat een negatieve gebeurtenis in het primaire datacenter van de klant dit resulteert in een verlies van de gegevens.

Voor een betere beveiliging, worden alle resources in deze oplossing worden beheerd als een resourcegroep via Azure Resource Manager. Azure Active Directory op rollen gebaseerd toegangsbeheer wordt gebruikt voor het beheren van toegang tot resources, met inbegrip van de sleutels in Azure Key Vault geïmplementeerd. Status van het bestandssysteem wordt bewaakt met Azure Monitor. Klanten configureren beide bewakingsservices voor het vastleggen van Logboeken en status van het bestandssysteem in een enkele, eenvoudig navigeerbaar dashboard weer te geven.

Een bastionhost management biedt een beveiligde verbinding voor beheerders voor toegang tot geïmplementeerd resources. **Microsoft raadt aan een VPN of ExpressRoute-verbinding voor het beheer en de gegevens importeren in de referentie-architectuur subnet configureren.**

![IaaS-webtoepassing voor FFIEC verwijzing Architectuurdiagram](images/ffiec-iaaswa-architecture.png "IaaS-webtoepassing voor FFIEC reference architecture-diagram")

Deze oplossing maakt gebruik van de volgende Azure-services. Details van de implementatiearchitectuur bevinden zich in de [architectuur](#deployment-architecture) sectie.

- Beschikbaarheidssets
    - (1) active Directory-domeincontrollers
    - (1) SQL-clusterknooppunten
    - (1) web/IIS
- Azure Active Directory
- Azure Application Gateway
    - (1) web Application Firewall
        - Firewallmodus: preventie
        - regel is ingesteld: OWASP 3.0
        - -Listener-poort: 443
- Azure Key Vault
- Azure Load Balancer
- Azure Monitor (Logboeken)
- Azure Resource Manager
- Azure Security Center
- Azure Storage
    - (7) Geo-redundant storage-accounts
- Azure Virtual Machines
    - (1) management/bastionhost (Windows Server 2016 Datacenter)
    - (2) active Directory-domeincontroller (Windows Server 2016 Datacenter)
    - (2) SQL Server-clusterknooppunt (SQL Server 2017 voor Windows Server 2016)
    - (2) web/IIS (Windows Server 2016 Datacenter)
- Azure Virtual Network
    - (1) /16 netwerk
    - (5) /24 netwerken
    - (5) Netwerkbeveiligingsgroepen
- Cloud-Witness
- Recovery Services-kluis

## <a name="deployment-architecture"></a>Implementatie-architectuur

De volgende sectie bevat de elementen van de implementatie en uitvoering.

**Bastionhost**: De bastionhost is de enkel ingangspunt waarmee gebruikers toegang krijgen tot de geïmplementeerde resources in deze omgeving. De bastionhost biedt een beveiligde verbinding met de geïmplementeerde resources door toe te staan alleen extern verkeer vanaf openbare IP-adressen op een veilige lijst. Als u wilt toestaan dat verkeer van extern bureaublad (RDP), moet de bron van het verkeer worden gedefinieerd in de netwerkbeveiligingsgroep.

Deze oplossing maakt u een virtuele machine als een domein bastionhost met de volgende configuraties:
-   [Antimalware-uitbreiding](https://docs.microsoft.com/azure/security/azure-security-antimalware)
-   [Azure Diagnostics-extensie](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template)
-   [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) met behulp van Azure Key Vault
-   Een [beleid voor automatisch afsluiten](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) verkleind van resources van de virtuele machine als deze niet in gebruik
-   [Windows Defender Credential Guard](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard) ingeschakeld die referenties en andere geheimen in een beveiligde omgeving die is geïsoleerd van het besturingssysteem wordt uitgevoerd

### <a name="virtual-network"></a>Virtueel netwerk

De architectuur definieert een particulier virtueel netwerk met een adresruimte van 10.200.0.0/16.

**Netwerkbeveiligingsgroepen**: Deze oplossing wordt geïmplementeerd resources in een architectuur met een afzonderlijke subnet, database-subnet, Active Directory-subnet en beheersubnet binnen een virtueel netwerk. Subnetten worden logisch gescheiden door regels voor netwerkbeveiligingsgroepen toegepast op de afzonderlijke subnetten te beperken het verkeer tussen subnetten die alleen dat nodig is voor het systeem en de beheerfunctionaliteit.

Zie de configuratie voor [netwerkbeveiligingsgroepen](https://github.com/Azure/fedramp-iaas-webapp/blob/master/nestedtemplates/virtualNetworkNSG.json) met deze oplossing wordt geïmplementeerd. Organisaties kunnen netwerkbeveiligingsgroepen configureren door het bewerken van het bestand hoger via [deze documentatie](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) als richtlijn.

Elk van de subnetten heeft een speciaal netwerk beveiligingsgroep:
- 1 network Security Group voor Application Gateway (LBNSG)
- 1 netwerk beveiligingsgroep voor bastionhost (MGTNSG)
- 1 netwerk beveiligingsgroep voor de primaire en back-domeincontrollers (ADNSG)
- 1 netwerk beveiligingsgroep voor SQL-Servers en Cloud-Witness (SQLNSG)
- 1 netwerk beveiligingsgroep voor de weblaag (WEBNSG)

### <a name="data-in-transit"></a>Gegevens die onderweg zijn
Alle communicatie naar en van Azure-datacenters versleutelt Azure standaard. Bovendien worden alle transacties met Azure Storage via Azure portal via HTTPS optreden.

### <a name="data-at-rest"></a>Data-at-rest

De architectuur beveiligt gegevens in rust via versleuteling, controle-database en andere metingen.

**Azure Storage**: Om versleutelde data-at-rest-vereisten te voldoen aan alle [Azure Storage](https://azure.microsoft.com/services/storage/) maakt gebruik van [Storage Service Encryption](https://docs.microsoft.com/azure/storage/storage-service-encryption). Dit helpt te beschermen en beveiligen van gegevens ter ondersteuning van de organisatie beveiligingsverplichtingen en nalevingsvereisten zijn gedefinieerd door FFIEC.

**Azure Disk Encryption**: [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) maakt gebruik van de BitLocker-functie van Windows om volumeversleuteling voor gegevensschijven te bieden. De oplossing kan worden geïntegreerd met Azure Key Vault voor het beheren en beheren van de versleutelingssleutels op de schijf.

**Azure SQL Database**: De Azure SQL Database-exemplaar maakt gebruik van de volgende metingen van de database-beveiliging:

- [Active Directory-verificatie en autorisatie](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) maakt identiteitsbeheer van databasegebruikers en andere Microsoft-services op één centrale locatie.
- [SQL database auditing](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) databasegebeurtenissen bijgehouden en geschreven naar een auditlogboek in Azure storage-account.
- Azure SQL Database is geconfigureerd voor het gebruik van [transparante gegevensversleuteling](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql), die voert realtime versleuteling en ontsleuteling van de database, gekoppelde back-ups en transactielogboekbestanden ter bescherming van gegevens op over de rest. Transparante gegevensversleuteling biedt de zekerheid dat de opgeslagen gegevens niet is onderworpen aan onbevoegde toegang.
- [Firewall-regels](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) voorkomen dat u alle toegang tot de database-servers, totdat de juiste machtigingen zijn toegekend. De firewall verleent toegang tot databases op basis van het IP-adres waar de aanvraag vandaan komt.
- [SQL Threat Detection](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) kunt u detectie en reactie op mogelijke bedreigingen wanneer deze zich voordoen, dankzij beveiligingswaarschuwingen voor verdachte databaseactiviteiten, potentiële kwetsbaarheden, SQL-injectieaanvallen en afwijkende database-toegang patronen.
- [Versleutelde kolommen](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) ervoor te zorgen dat gevoelige gegevens nooit wordt weergegeven als tekst zonder opmaak in de database-systeem. Na het inschakelen van versleuteling van gegevens, alleen clienttoepassingen of toepassingsservers met toegang tot de sleutels gegevens zijn toegankelijk als tekst zonder opmaak.
- [SQL Database dynamische gegevensmaskering](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) blootstelling van gevoelige gegevens maskeren van de gegevens naar niet-gemachtigde gebruikers of toepassingen. Dynamische gegevensmaskering automatisch detecteren van potentieel gevoelige gegevens en stel de juiste maskers moet worden toegepast. Dit helpt om te bepalen en toegang tot gegevens beperken zodat deze niet de database via niet-geautoriseerde toegang wordt afgesloten. Klanten zijn verantwoordelijk voor het aanpassen van dynamische gegevensmaskering instellingen om te voldoen aan het schema van de database.

### <a name="identity-management"></a>Identiteitsbeheer

De volgende technologieën bieden mogelijkheden voor het beheren van toegang tot gegevens in de Azure-omgeving:

- [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) is Microsoft&#39;s multitenant cloudgebaseerde directory en identity management-service. Alle gebruikers voor deze oplossing worden gemaakt in Azure Active Directory, met inbegrip van gebruikers met toegang tot de Azure SQL Database.
- Verificatie van de toepassing wordt uitgevoerd met behulp van Azure Active Directory. Zie voor meer informatie, [toepassingen integreren met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Bovendien de versleuteling van de kolom database maakt gebruik van Azure Active Directory om te verifiëren van de toepassing naar Azure SQL Database. Zie voor meer informatie over het [bescherming van gevoelige gegevens in Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
- [Op rollen gebaseerd toegangsbeheer in Azure](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) kunnen beheerders om te voorzien in specifieke toegangsmachtigingen te verlenen alleen de mate van toegang dat gebruikers moeten om hun werk te definiëren. In plaats van geeft elke onbeperkte gebruikersmachtigingen voor Azure-resources, beheerders kunnen toestaan dat alleen bepaalde acties voor toegang tot gegevens. Abonnementstoegang is beperkt tot de beheerder van het abonnement.
- [Azure Active Directory Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) kunnen klanten voor het minimaliseren van het aantal gebruikers die toegang tot bepaalde gegevens hebben. Beheerders kunnen Azure Active Directory Privileged Identity Management gebruiken om te detecteren, beperken en controleren van bevoegde identiteiten en hun toegang tot bronnen. Deze functionaliteit kan ook worden gebruikt om af te dwingen op aanvraag, just-in-time beheerderstoegang wanneer dat nodig is.
- [Azure Active Directory Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) detecteert mogelijke beveiligingsproblemen die betrekking hebben op een organisatie&#39;s identiteiten, configureert u automatische antwoorden op gedetecteerde verdachte activiteit met betrekking tot een organisatie&#39;s identiteiten , en verdachte incidenten passende actie op te lossen moet onderzoekt het probleem.


### <a name="security"></a>Beveiliging

**Geheimen management**: De oplossing maakt gebruik van [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) voor het beheer van sleutels en geheimen. Met Azure Sleutelkluis kunt u de cryptografische sleutels en geheimen beveiligen die door cloudtoepassingen en -services worden gebruikt. De volgende mogelijkheden van Azure Key Vault helpen klanten beveiligen en toegang tot deze gegevens:

- Geavanceerde beleidsregels zijn geconfigureerd op basis van behoefte.
- Toegangsbeleid voor Key Vault zijn met de minimaal vereiste machtigingen voor sleutels en geheimen gedefinieerd.
- Verloopdatum hebben alle sleutels en geheimen in Key Vault.
- Alle sleutels in Key Vault zijn beveiligd door gespecialiseerde hardware security modules. Het sleuteltype is een HSM beveiligd 2048-bits RSA-sleutel.
- Minimaal vereiste machtigingen met behulp van op rollen gebaseerd toegangsbeheer worden verleend door alle gebruikers en identiteiten.
- Diagnostische logboeken voor Key Vault worden ingeschakeld met een bewaarperiode van ten minste 365 dagen.
- Toegestane cryptografiebewerkingen voor sleutels zijn beperkt tot die nodig is.
- De oplossing is geïntegreerd met Azure Key Vault voor het beheren van IaaS VM-schijfversleuteling sleutels en geheimen.

**Patchbeheer**: Windows virtuele machines geïmplementeerd als onderdeel van deze referentiearchitectuur worden standaard automatisch updates ontvangen van Windows Update-Service geconfigureerd. Deze oplossing bevat ook de [Azure Automation](https://docs.microsoft.com/azure/automation/automation-intro) service waarmee bijgewerkte implementaties kunnen worden gemaakt voor de patch voor virtuele machines wanneer dat nodig is.

**Bescherming tegen schadelijke software**: [Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) voor virtuele Machines biedt realtime-beveiliging-functie die helpt te identificeren en verwijderen van virussen, spyware en andere schadelijke software, met configureerbare meldingen wanneer bekende schadelijke of ongewenste software wil installeren of uitvoeren op de beveiligde virtuele machines.

**Azure Security Center**: Met [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro), klanten kunnen centraal toepassen en beheren van beveiligingsbeleid voor workloads, blootstelling aan bedreigingen beperken en detecteren en reageren op aanvallen. Azure Security Center heeft bovendien toegang tot bestaande configuraties van Azure-services voor configuratie en de serviceaanbevelingen om u te helpen bij het beveiligingspostuur verbeteren en gegevens te beschermen.

Azure Security Center maakt gebruik van tal van mogelijkheden voor het detecteren om klanten van potentiële aanvallen die gericht is op hun omgeving. Deze waarschuwingen bevatten waardevolle informatie over de trigger van de waarschuwing, de betrokken resources en de bron van de aanval. Azure Security Center bevat een set [vooraf gedefinieerde beveiligingswaarschuwingen](https://docs.microsoft.com/azure/security-center/security-center-alerts-type), die worden geactiveerd wanneer een bedreiging of verdachte activiteit wordt gedetecteerd. [Aangepaste waarschuwingsregels](https://docs.microsoft.com/azure/security-center/security-center-custom-alert) in Azure Security Center kunnen klanten voor het definiëren van nieuwe beveiligingswaarschuwingen op basis van gegevens die al zijn verzameld voor hun omgeving.

Azure Security Center biedt beveiligingswaarschuwingen en incidenten, waardoor het eenvoudiger voor klanten om te detecteren en mogelijke beveiligingsproblemen te verhelpen. Een [threat intelligence-rapport](https://docs.microsoft.com/azure/security-center/security-center-threat-report) wordt gegenereerd voor elke bedreigingen gedetecteerde voor het incident response-teams helpt bij het onderzoeken en bescherm bedreigingen.

**Azure Application Gateway**: De architectuur vermindert het risico van beveiligingsproblemen met behulp van een Azure Application Gateway met een web application firewall is geconfigureerd en de OWASP ruleset ingeschakeld. Aanvullende mogelijkheden zijn onder andere:

- [End-to-end-SSL](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- Schakel [SSL-Offload](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-portal)
- Uitschakelen [TLS v1.0 en v1.1](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- [De Web application firewall](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) (preventiemodus)
- [Preventiemodus](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal) met OWASP 3.0 ruleset
- Schakel [logboekregistratie van diagnostische gegevens](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics)
- [Aangepaste statuscontroles](https://docs.microsoft.com/azure/application-gateway/application-gateway-create-gateway-portal)
- [Azure Security Center](https://azure.microsoft.com/services/security-center) en [Azure Advisor](https://docs.microsoft.com/azure/advisor/advisor-security-recommendations) bieden extra beveiliging en meldingen. Azure Security Center biedt ook een reputatie-systeem.

### <a name="business-continuity"></a>Bedrijfscontinuïteit

**Hoge beschikbaarheid**: De oplossing wordt geïmplementeerd voor alle virtuele machines in een [Beschikbaarheidsset](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets). Beschikbaarheidssets zorgen ervoor dat de virtuele machines worden verdeeld over meerdere geïsoleerde hardwareclusters beschikbaarheid verbeteren. Ten minste één virtuele machine beschikbaar is tijdens gepland of ongepland onderhoud, voldoen aan de 99,95% Azure SLA.

**Recovery Services-kluis**: De [Recovery Services-kluis](https://docs.microsoft.com/azure/backup/backup-azure-recovery-services-vault-overview) -instellingen van de back-upgegevens en alle configuraties van Azure Virtual Machines in deze architectuur beveiligt. Met een Recovery Services-kluis, klanten kunnen bestanden en mappen terugzetten van een IaaS-virtuele machine zonder te herstellen van de volledige virtuele machine, waardoor sneller worden hersteld.

**Cloud-Witness**: [Cloud-Witness](https://docs.microsoft.com/windows-server/failover-clustering/whats-new-in-failover-clustering#BKMK_CloudWitness) is een type failovercluster-quorumwitness in Windows Server 2016 die gebruikmaakt van Azure als arbitragepunt. De Cloud-Witness, net als elke andere quorumwitness een stem opgehaald en kunt deelnemen aan de quorumberekeningen, maar hierbij de standaard openbare Azure Blob Storage. Dit elimineert de noodzaak extra onderhoud van virtuele machines die worden gehost in een openbare cloud.

### <a name="logging-and-auditing"></a>Logboekregistratie en controle

Azure-services zich uitgebreid systeem en activiteit van gebruikers, evenals systeemstatus:
- **Activiteitenlogboeken**: [Activiteitenlogboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bieden inzicht in bewerkingen die worden uitgevoerd op resources in een abonnement. Activiteitenlogboeken kunnen u eenvoudiger bepalen van een bewerking initiator, tijdstip van de gebeurtenis en status.
- **Diagnostische logboeken**: [Diagnostische logboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) bevatten alle logboeken die zijn gegenereerd door elke resource. Deze logboeken bevatten de logboeken voor Windows-systeem, Azure Storage-Logboeken, Key Vault-auditlogboeken en Application Gateway toegangs- en firewall-Logboeken. Alle logboeken met diagnostische gegevens schrijven naar een gecentraliseerd en versleutelde Azure storage-account voor archivering. De bewaarperiode is om te voldoen aan vereisten voor de bewaarperiode van de organisatie-specifieke gebruiker-configureren, tot maximaal 730 dagen.

**Logboeken in Azure Monitor**: Deze logboeken worden geconsolideerd [logboeken van Azure Monitor](https://azure.microsoft.com/services/log-analytics/) voor verwerking, opslag en -dashboardrapporten. Zodra de verzameld, worden de gegevens zijn onderverdeeld in afzonderlijke tabellen voor elk gegevenstype in Log Analytics-werkruimten, zodat alle gegevens samen kunnen worden geanalyseerd, ongeacht de oorspronkelijke bron. Daarnaast wordt Azure Security Center kan worden geïntegreerd met Azure Monitor-Logboeken zodat klanten kunnen Kusto-query's gebruiken voor toegang tot hun beveiligingsgegevens voor de gebeurtenis en combineren met gegevens van andere services.

De volgende Azure [bewakingsoplossingen](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions) zijn opgenomen als onderdeel van deze architectuur:
-   [Active Directory-evaluatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): De oplossing statuscontrole van Active Directory beoordeelt het risico en de gezondheid van server-omgevingen op een vast interval en biedt een geprioriteerde lijst met aanbevelingen die specifiek zijn voor de geïmplementeerde serverinfrastructuur.
- [SQL-evaluatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): De oplossing SQL-statuscontrole beoordeelt het risico en de gezondheid van server-omgevingen op een vast interval en biedt klanten een geprioriteerde lijst met aanbevelingen die specifiek zijn voor de geïmplementeerde serverinfrastructuur.
- [Status van agent](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): De oplossing status van Agent rapporteert het aantal agents zijn geïmplementeerd en hun geografische verdeling, evenals hoeveel agents die niet meer reageert en het aantal agents die zijn operationele gegevens kan verzenden.
-   [Activity Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): De oplossing Activity Log Analytics biedt ondersteuning voor analyse van de Azure-activiteitenlogboeken voor alle Azure-abonnementen voor een klant.

**Azure Automation**: [Azure Automation](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker) worden opgeslagen, wordt uitgevoerd runbooks worden beheerd. In deze oplossing te runbooks verzamelen van Logboeken van Azure SQL Database. De automatisering [bijhouden](https://docs.microsoft.com/azure/automation/automation-change-tracking) oplossing kan klanten eenvoudig wijzigingen in de omgeving identificeren.

**Azure Monitor**: [Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) helpt gebruikers bij het bijhouden van prestaties, beveiliging en trends te identificeren doordat organisaties om te controleren, waarschuwingen maken en archiveren van gegevens, inclusief bijhouden API-aanroepen in hun Azure-resources.

## <a name="threat-model"></a>Risicomodel

Het diagram van de gegevensstroom voor deze referentiearchitectuur is beschikbaar voor [downloaden](https://aka.ms/ffiec-iaaswa-tm) of vindt u hieronder. Dit model kunt klanten inzicht krijgen in de punten van de mogelijke risico's in de infrastructuur van het systeem als u wijzigingen aanbrengt.

![IaaS-webtoepassing voor FFIEC risicomodel](images/ffiec-iaaswa-threat-model.png "IaaS-webtoepassing voor FFIEC risicomodel")

## <a name="compliance-documentation"></a>Naleving-documentatie

De [Azure-beveiliging en naleving blauwdruk – FFIEC klant verantwoordelijkheid Matrix](https://aka.ms/ffiec-crm) geeft een lijst van alle beveiligingsdoelstellingen FFIEC voorschrijft. Deze matrix bevat of de implementatie van elk doel de verantwoordelijkheid van Microsoft, de klant is, of gedeeld tussen de twee.

De [Azure-beveiliging en naleving blauwdruk – FFIEC IaaS Web Application implementatie Matrix](https://aka.ms/ffiec-iaaswa-cim) biedt informatie over welke FFIEC vereisten worden verholpen door de IaaS-architectuur voor web-toepassing, met inbegrip van gedetailleerde beschrijvingen van hoe de implementatie voldoet aan de vereisten van elke gedekte doelstelling.

## <a name="guidance-and-recommendations"></a>Richtlijnen en aanbevelingen

### <a name="vpn-and-expressroute"></a>VPN en ExpressRoute

Een beveiligde VPN-tunnel of [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) moet worden geconfigureerd voor een veilig verbinding met de resources die zijn geïmplementeerd als onderdeel van deze referentiearchitectuur web IaaS. Door op de juiste wijze instellen van een VPN of ExpressRoute, toevoegen klanten een beveiligingslaag voor gegevens in transit.

Door het implementeren van een beveiligde VPN-tunnel met Azure, kan een virtuele particuliere verbinding tussen een on-premises netwerk en een Azure-netwerk worden gemaakt. Deze verbinding vindt plaats via Internet en kunnen klanten veilig &quot;tunnel&quot; informatie binnen een gecodeerde verbinding tussen de klant&#39;s netwerk en Azure. Site-naar-site VPN is een veilige, volwassen technologie die is geïmplementeerd door bedrijven van alle groottes decennia. De [IPsec-tunnelmodus](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) bij deze optie als een versleutelingsmechanisme voor wordt gebruikt.

Omdat het verkeer binnen de VPN-tunnel via Internet met een site-naar-site-VPN, biedt Microsoft een andere verbinding nog veiliger optie. Azure ExpressRoute is een speciale WAN koppeling tussen Azure en een on-premises locatie of een Exchange-hostingprovider. Als het ExpressRoute-verbindingen gaan niet via het Internet, bieden deze verbindingen een meer betrouwbaarheid, hogere snelheden, kortere wachttijden en hogere beveiliging dan gebruikelijke verbindingen via Internet. Bovendien, omdat dit een directe verbinding van de klant is&#39;s-provider voor telecommunicatie, de gegevens niet via Internet kan worden verzonden en daarom geen toegang heeft tot deze.

Aanbevolen procedures voor het implementeren van een beveiligd hybride netwerk dat een on-premises netwerk naar Azure uitbreidt zijn [beschikbaar](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

## <a name="disclaimer"></a>Vrijwaring

- Dit document is uitsluitend ter informatie bedoeld. MICROSOFT BIEDT GEEN GARANTIES, EXPLICIETE, IMPLICIETE OF WETTELIJKE GARANTIE VOOR DE INFORMATIE IN DIT DOCUMENT. Dit document wordt geleverd ' as-is. " Informatie en meningen in dit document, inclusief URL's en andere websiteverwijzingen, kunnen zonder kennisgeving worden gewijzigd. Klanten die in dit document leest draagt het risico van het gebruik ervan.
- Dit document biedt klanten met een enkel wettelijk recht op enig intellectueel in andere Microsoft-producten of oplossingen.
- Klanten kunnen kopiëren en gebruiken van dit document voor interne referentiedoeleinden.
- Bepaalde aanbevelingen in dit document kunnen leiden tot grotere hoeveelheden gegevens, netwerk- of gebruik van de compute-bronnen in Azure en de Azure-licentie of abonnement kosten van een klant kunnen verhogen.
- Deze architectuur is bedoeld om te fungeren als een basis voor klanten om aan te passen op hun specifieke behoeften en mag niet worden gebruikt als-is in een productieomgeving.
- Dit document is ontwikkeld als referentie en mag niet worden gebruikt voor het definiëren van alle middelen waarmee een klant kan voldoen aan specifieke nalevingsvereisten en voorschriften. Klanten moeten juridische ondersteuning van hun organisatie op goedgekeurde klantimplementaties gezocht.
