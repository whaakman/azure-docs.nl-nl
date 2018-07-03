---
title: Azure-beveiliging en naleving blauwdruk - PaaS-webtoepassing voor UK NHS
description: Azure-beveiliging en naleving blauwdruk - PaaS-webtoepassing voor UK NHS
services: security
author: jomolesk
ms.assetid: fe409add-6d09-4062-b3c8-d74574747739
ms.service: security
ms.topic: article
ms.date: 06/15/2018
ms.author: jomolesk
ms.openlocfilehash: 319c89969e98eafd792af078bf3094ca0aa9a7dd
ms.sourcegitcommit: 4597964eba08b7e0584d2b275cc33a370c25e027
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2018
ms.locfileid: "37342938"
---
# <a name="azure-security-and-compliance-blueprint-paas-web-application-for-uk-nhs"></a>Azure-beveiliging en naleving blauwdruk: PaaS-webtoepassing voor UK NHS

## <a name="overview"></a>Overzicht

Deze Azure-beveiliging en naleving blauwdruk biedt een referentiearchitectuur en richtlijnen voor een platform als een service (PaaS)-oplossing die geschikt is voor de verzameling, opslaan en ophalen van de gezondheidszorg gegevens. Deze oplossing toont manieren waarin klanten aan de richtlijnen van voldoen kunnen de [Cloud Security goede Practice Guide](https://digital.nhs.uk/data-and-information/looking-after-information/data-security-and-information-governance/nhs-and-social-care-data-off-shoring-and-the-use-of-public-cloud-services/health-and-social-care-cloud-security-good-practice-guide) gepubliceerd door [NHS digitale](https://digital.nhs.uk/), een partner van het van het Verenigd Koninkrijk (VK) Afdeling van de gezondheid en sociale Care (DHSC). In de Cloud Security goede Practice Guide is gebaseerd op de 14 [Cloud Security Principles](https://www.ncsc.gov.uk/guidance/implementing-cloud-security-principles) gepubliceerd door de UK nationale Cyber Security Center (NCSC).

Deze referentiearchitectuur, Implementatiehandleiding en risicomodel zijn bedoeld om te fungeren als een basis voor klanten om aan te passen op hun specifieke behoeften en mag niet worden gebruikt als-in een productieomgeving zonder aanvullende configuratie is. Klanten zijn verantwoordelijk voor het uitvoeren van de juiste beveiliging en naleving evaluaties van een oplossing die is gebouwd met behulp van deze architectuur als vereisten kunnen variëren op basis van de details van de uitvoering van elke klant.

## <a name="architecture-diagram-and-components"></a>Diagram van architectuur en onderdelen

Deze oplossing biedt een referentiearchitectuur voor een PaaS-webtoepassing met een Azure SQL Database-back-end. De web-App wordt gehost in een geïsoleerde Azure App Service Environment, dit een exclusieve privéomgeving in een Azure-datacenter is. De omgeving verdeelt verkeer voor de web-App voor virtuele machines die worden beheerd door Azure. Alle externe verbindingen vereist ondersteuning voor TLSv1.2. Deze architectuur bevat ook netwerkbeveiligingsgroepen, een Application Gateway, Azure DNS en Load Balancer.

De oplossing maakt gebruik van Azure Storage-accounts, die klanten configureren kunnen met Storage Service Encryption kunt onderhouden van de vertrouwelijkheid van gegevens in rust. Azure slaat drie kopieën van gegevens binnen de geselecteerde datacenter van een klant voor tolerantie. Geografisch redundante opslag zorgt ervoor dat gegevens worden gerepliceerd naar een secundair datacenter honderden van mijl opslaan en opnieuw als drie kopieën binnen die datacenter opgeslagen, zo wordt voorkomen dat een negatieve gebeurtenis in het primaire datacenter van de klant dit resulteert in een verlies van de gegevens.

Voor een betere beveiliging, worden alle resources in deze oplossing worden beheerd als een resourcegroep via Azure Resource Manager. Azure Active Directory-rollen gebaseerd toegangsbeheer wordt gebruikt voor het beheren van toegang geïmplementeerd resources en sleutels in Azure Key Vault. Status van het bestandssysteem wordt bewaakt door Azure Security Center en Azure Monitor. Klanten configureren beide bewakingsservices voor het vastleggen van Logboeken en status van het bestandssysteem in een enkele, eenvoudig navigeerbaar dashboard weer te geven. Azure Application Gateway is geconfigureerd als een firewall in preventiemodus en het verkeer dat is geen ondersteuning voor TLSv1.2 is niet toegestaan. De oplossing maakt gebruik van Azure-toepassing Service-omgeving v2 te isoleren van de weblaag in een omgeving met meerdere tenants.

**Microsoft raadt aan een VPN of ExpressRoute-verbinding voor het beheer en de gegevens importeren in de referentie-architectuur subnet configureren.**

![PaaS-webtoepassing voor UK NHS verwijzing Architectuurdiagram](images/uknhs-paaswa-architecture.png?raw=true "PaaS-webtoepassing voor UK NHS verwijzing Architectuurdiagram")

Deze oplossing maakt gebruik van de volgende Azure-services. Informatie van de implementatiearchitectuur vindt u in de [architectuur](#deployment-architecture) sectie.

- Application Gateway
    - Web Application Firewall
        - Firewallmodus: preventie
        - Regelset: OWASP
        - -Listener-poort: 443
- Azure Active Directory
- Azure-toepassing Service-omgeving v2
- Azure Automation
- Azure DNS
- Azure Key Vault
- Azure Load Balancer
- Azure Monitor
- Azure Resource Manager
- Azure Security Center
- Azure SQL Database
- Azure Storage
- Azure Virtual Network
    - Netwerkbeveiligingsgroepen
- Azure Web App

## <a name="deployment-architecture"></a>Implementatie-architectuur

De volgende sectie bevat de elementen van de implementatie en uitvoering.

**Azure Resource Manager**: [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) kunnen klanten voor het werken met de resources in de oplossing als een groep. Klanten kunnen implementeren, bijwerken of verwijderen van alle resources voor de oplossing in een enkele, gecoördineerde bewerking. Klanten een sjabloon voor implementatie gebruiken en deze sjabloon kunt werken voor verschillende omgevingen, zoals testen, fasering en productie. Resource Manager biedt beveiliging, controle en tagfuncties die helpen bij klanten die hun resources beheren na de implementatie.

**App Service Environment v2**: de Azure App Service Environment is een App Service-functie waarmee een volledig geïsoleerde en toegewezen omgeving voor App Service-toepassingen op een grote schaal veilig kunnen worden uitgevoerd.

De App Service-omgeving is geïsoleerd voor één toepassing alleen worden uitgevoerd en wordt altijd geïmplementeerd in een virtueel netwerk. Deze isolatie-functie kunt de referentiearchitectuur volledig tenantisolatie, wordt dit verwijderd uit Azure omgeving met meerdere tenants hebben. Deze functie isolatie is vereist om te voldoen aan de vereisten van groot-Brittannië NHS principe 3. Klanten hebben meer controle over zowel toepassing binnenkomend en uitgaand netwerkverkeer en toepassingen zeer snelle, veilige verbindingen kunnen maken via virtuele netwerken tot on-premises bedrijfsresources. Klanten kunnen "automatisch schalen" met App Service-omgeving op basis van metrische gegevens laden, beschikbare budget of een ingesteld schema.


Gebruik van App Service-omgeving voor deze architectuur biedt de volgende besturingselementen/configuraties:

- Hosten in een beveiligde Azure-netwerk en netwerkbeveiligingsregels
- Zelf-ondertekend interne load balancer-certificaat voor HTTPS-communicatie. Als een best practice, wordt aangeraden het gebruik van een vertrouwde certificeringsinstantie voor verbeterde beveiliging.
- [Interne load balancing-modus](https://docs.microsoft.com/azure/app-service-web/app-service-environment-with-internal-load-balancer)
- Uitschakelen [TLS 1.0](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-custom-settings)
- Wijziging [TLS-codering](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-custom-settings)
- Besturingselement [binnenkomende verkeer N/W poorten](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-control-inbound-traffic)
- [Web application firewall: gegevens beperken](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-web-application-firewall)
- Toestaan dat [verkeer van de Azure SQL Database](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-network-architecture-overview)

**Azure-Web-App**: [Azure Web Apps](https://docs.microsoft.com/azure/app-service/) kunnen klanten bouwen en hosten van webtoepassingen in de programmeertaal van hun keuze zonder het beheren van infrastructuur. Het biedt automatisch schalen en een hoge beschikbaarheid, ondersteuning voor zowel Windows als Linux en maakt automatische implementaties mogelijk vanuit GitHub, Visual Studio Team Services of een willekeurige Git-repo.

### <a name="virtual-network"></a>Virtual Network

De architectuur definieert een particulier virtueel netwerk met een adresruimte van 10.200.0.0/16.

**Netwerkbeveiligingsgroepen**: [Netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) bevatten toegangsbeheerlijsten die verkeer binnen een virtueel netwerk toestaan of weigeren. Netwerkbeveiligingsgroepen kunnen worden gebruikt om verkeer een subnet of afzonderlijke virtuele machine te beveiligen. De volgende netwerkbeveiligingsgroepen bestaan:

- 1 netwerkbeveiligingsgroep voor Application Gateway
- 1 netwerkbeveiligingsgroep voor App Service-omgeving
- 1 netwerkbeveiligingsgroep voor Azure SQL Database

Elk van de netwerkbeveiligingsgroepen specifieke poorten en protocollen openen, zodat de oplossing kunt veilig en goed werken. Bovendien zijn de volgende configuraties ingeschakeld voor elke netwerkbeveiligingsgroep:

- [Diagnostische logboeken en gebeurtenissen](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) zijn ingeschakeld en die zijn opgeslagen in een storage-account
- Log Analytics is verbonden met de [netwerkbeveiligingsgroep&#39;s diagnostische logboeken](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json)

**Subnetten**: elk subnet is gekoppeld aan de bijbehorende netwerkbeveiligingsgroep.

**Azure DNS**: de Domain Name System- of DNS, is verantwoordelijk voor het omzetten van (of het oplossen van) de naam van een website of -service naar het IP-adres. [Azure DNS](https://docs.microsoft.com/azure/dns/dns-overview) is een hostingservice voor DNS-domeinen die naamomzetting met behulp van Azure-infrastructuur biedt. Door domeinen te hosten in Azure, kunnen gebruikers de DNS-records met dezelfde referenties, API's, hulpprogramma's en facturering als andere Azure-services beheren. Azure DNS ondersteunt ook persoonlijke DNS-domeinen.

**Azure Load Balancer**: [Azure Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview) kunnen klanten hun toepassingen schalen en hoge beschikbaarheid voor services. Load Balancer ondersteunt zowel binnenkomende als uitgaande-scenario's, en lage latentie, hoge doorvoer, en kan worden geschaald tot miljoenen stromen voor alle TCP en UDP-toepassingen.

### <a name="data-in-transit"></a>Gegevens die onderweg zijn

Alle communicatie naar en van Azure-datacenters versleutelt Azure standaard. Alle transacties met Azure Storage via Azure portal plaatsvinden via HTTPS.

### <a name="data-at-rest"></a>Data-at-rest

De architectuur beveiligt gegevens in rust via versleuteling, controle-database en andere metingen.

**Azure Storage**: om versleutelde data-at-rest-vereisten te voldoen aan alle [Azure Storage](https://azure.microsoft.com/services/storage/) maakt gebruik van [Storage Service Encryption](https://docs.microsoft.com/azure/storage/storage-service-encryption). Dit helpt beschermen en beveiligen van gegevens ter ondersteuning van de organisatie beveiligingsverplichtingen en nalevingsvereisten door NHS digitale gedefinieerd.

**Azure Disk Encryption**: [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) maakt gebruik van de BitLocker-functie van Windows om volumeversleuteling voor gegevensschijven te bieden. De oplossing kan worden geïntegreerd met Azure Key Vault voor het beheren en beheren van de versleutelingssleutels op de schijf.

**Azure SQL Database**: de Azure SQL Database-exemplaar maakt gebruik van de volgende metingen van de database-beveiliging:

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

**Geheimen management**: de oplossing maakt gebruik van [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) voor het beheer van sleutels en geheimen. Met Azure Sleutelkluis kunt u de cryptografische sleutels en geheimen beveiligen die door cloudtoepassingen en -services worden gebruikt. De volgende mogelijkheden van Azure Key Vault helpen klanten beveiligen en toegang tot deze gegevens:

- Geavanceerde beleidsregels zijn geconfigureerd op basis van behoefte.
- Toegangsbeleid voor Key Vault zijn met de minimaal vereiste machtigingen voor sleutels en geheimen gedefinieerd.
- Verloopdatum hebben alle sleutels en geheimen in Key Vault.
- Alle sleutels in Key Vault zijn beveiligd door gespecialiseerde hardware security modules. Het sleuteltype is dat een hardware security module beveiligde 2048-bits RSA-sleutel.
- Minimaal vereiste machtigingen met behulp van op rollen gebaseerd toegangsbeheer worden verleend door alle gebruikers en identiteiten.
- Diagnostische logboeken voor Key Vault worden ingeschakeld met een bewaarperiode van ten minste 365 dagen.
- Toegestane cryptografiebewerkingen voor sleutels zijn beperkt tot die nodig is.

**Azure Security Center**: met [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro), deze oplossing kunt Centraal toepassen en beheren van beveiligingsbeleid voor workloads, blootstelling aan bedreigingen beperken en detecteren en reageren op aanvallen. Azure Security Center heeft bovendien toegang tot bestaande configuraties van Azure-services voor configuratie en de serviceaanbevelingen om u te helpen bij het beveiligingspostuur verbeteren en gegevens te beschermen.

Azure Security Center maakt gebruik van tal van mogelijkheden voor het detecteren om klanten van potentiële aanvallen die gericht is op hun omgeving. Deze waarschuwingen bevatten waardevolle informatie over de trigger van de waarschuwing, de betrokken resources en de bron van de aanval. Azure Security Center bevat een set [vooraf gedefinieerde beveiligingswaarschuwingen](https://docs.microsoft.com/azure/security-center/security-center-alerts-type), die worden geactiveerd wanneer een bedreiging of verdachte activiteit wordt gedetecteerd. [Aangepaste waarschuwingsregels](https://docs.microsoft.com/azure/security-center/security-center-custom-alert) in Azure Security Center kunnen klanten voor het definiëren van nieuwe beveiligingswaarschuwingen op basis van gegevens die al zijn verzameld voor hun omgeving.

Azure Security Center biedt beveiligingswaarschuwingen en incidenten, waardoor het eenvoudiger om te detecteren en mogelijke beveiligingsproblemen te verhelpen. Een [threat intelligence-rapport](https://docs.microsoft.com/azure/security-center/security-center-threat-report) wordt gegenereerd voor elke bedreigingen gedetecteerde voor het incident response-teams helpt bij het onderzoeken en bescherm bedreigingen.

**Azure Application Gateway**: de architectuur vermindert het risico van beveiligingsproblemen met behulp van een Azure Application Gateway met een web application firewall is geconfigureerd en de OWASP ruleset ingeschakeld. Aanvullende mogelijkheden zijn onder andere:

- [End-naar-end-SSL](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- Schakel [SSL-Offload](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-portal)
- Uitschakelen [TLS v1.0 en v1.1](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- [De Web application firewall](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) (preventiemodus)
- [Preventiemodus](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal) met OWASP 3.0 ruleset
- Schakel [logboekregistratie van diagnostische gegevens](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics)
- [Aangepaste statuscontroles](https://docs.microsoft.com/azure/application-gateway/application-gateway-create-gateway-portal)
- [Azure Security Center](https://azure.microsoft.com/services/security-center) en [Azure Advisor](https://docs.microsoft.com/azure/advisor/advisor-security-recommendations) bieden extra beveiliging en meldingen. Azure Security Center biedt ook een reputatie-systeem.

### <a name="logging-and-auditing"></a>Logboekregistratie en controle

Azure-services zich uitgebreid systeem en activiteit van gebruikers, evenals systeemstatus:
- **Activiteitenlogboeken**: [activiteitenlogboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bieden inzicht in bewerkingen die worden uitgevoerd op resources in een abonnement. Activiteitenlogboeken kunnen u eenvoudiger bepalen van een bewerking initiator, tijdstip van de gebeurtenis en status.
- **Diagnostische logboeken**: [diagnostische logboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) bevatten alle logboeken die zijn gegenereerd door elke resource. Deze logboeken bevatten de logboeken voor Windows-systeem, Azure Storage-Logboeken, Key Vault-auditlogboeken en Application Gateway toegangs- en firewall-Logboeken. Alle logboeken met diagnostische gegevens schrijven naar een gecentraliseerd en versleutelde Azure storage-account voor archivering. De bewaarperiode is om te voldoen aan vereisten voor de bewaarperiode van de organisatie-specifieke gebruiker-configureren, tot maximaal 730 dagen.

**Log Analytics**: deze logboeken worden geconsolideerd [Log Analytics](https://azure.microsoft.com/services/log-analytics/) voor verwerking, opslag en -dashboardrapporten. Zodra de gegevens zijn verzameld, worden ze georganiseerd in aparte tabellen voor elk gegevenstype, zodat alle gegevens samen kunnen worden geanalyseerd, ongeacht de oorspronkelijke bron. Daarnaast wordt Azure Security Center kan worden geïntegreerd met Log Analytics, waardoor klanten kunnen Log Analytics-query's gebruiken voor toegang tot hun beveiligingsgegevens voor de gebeurtenis en combineren met gegevens van andere services.

De volgende Log Analytics [beheeroplossingen](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions) zijn opgenomen als onderdeel van deze architectuur:
-   [Active Directory-evaluatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): het Active Directory statuscontrole oplossing beoordeelt het risico en de gezondheid van server-omgevingen op een vast interval en biedt een geprioriteerde lijst met aanbevelingen die specifiek zijn voor de geïmplementeerde serverinfrastructuur.
- [SQL-evaluatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): statuscontrole van de SQL-oplossing beoordeelt het risico en de gezondheid van server-omgevingen op een vast interval en biedt klanten een geprioriteerde lijst met aanbevelingen die specifiek zijn voor de geïmplementeerde serverinfrastructuur.
- [Status van agent](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): oplossing status van de Agent rapporteert het aantal agents zijn geïmplementeerd en hun geografische verdeling, evenals hoeveel agents die niet meer reageert en het aantal agents die zijn operationele gegevens kan verzenden.
-   [Activity Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): de Activity Log Analytics-oplossing biedt ondersteuning voor analyse van de Azure-activiteitenlogboeken voor alle Azure-abonnementen voor een klant.

**Azure Automation**: [Azure Automation](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker) worden opgeslagen, wordt uitgevoerd runbooks worden beheerd. In deze oplossing te runbooks verzamelen van Logboeken van Azure SQL Database. De automatisering [bijhouden](https://docs.microsoft.com/azure/automation/automation-change-tracking) oplossing kan klanten eenvoudig wijzigingen in de omgeving identificeren.

**Azure Monitor**: [Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) helpt gebruikers bij het bijhouden van prestaties, veiligheid en trends te identificeren doordat organisaties om te controleren, waarschuwingen maken en archiveren van gegevens, inclusief bijhouden API-aanroepen in hun Azure resources.

## <a name="threat-model"></a>Risicomodel

Het diagram van de gegevensstroom voor deze referentiearchitectuur is beschikbaar voor [downloaden](https://aka.ms/uknhs-paaswa-tm) of vindt u hieronder. Dit model kunt klanten inzicht krijgen in de punten van de mogelijke risico's in de infrastructuur van het systeem als u wijzigingen aanbrengt.

![PaaS-webtoepassing voor UK NHS risicomodel](images/uknhs-paaswa-threat-model.png?raw=true "PaaS-webtoepassing voor UK NHS risicomodel")

## <a name="compliance-documentation"></a>Naleving-documentatie

De [Azure-beveiliging en naleving blauwdruk – Verenigd Koninkrijk NHS klant verantwoordelijkheid Matrix](https://aka.ms/uknhs-crm) geeft een lijst van alle UK NHS vereisten. Deze matrix bevat of de implementatie van elk principe de verantwoordelijkheid van Microsoft, de klant is, of gedeeld tussen de twee.

De [Azure-beveiliging en naleving blauwdruk – Verenigd Koninkrijk NHS PaaS Web Application implementatie Matrix](https://aka.ms/uknhs-paaswa-cim) biedt informatie over welke NHS UK vereisten worden verholpen door de PaaS-architectuur voor web-toepassing, met inbegrip van gedetailleerde beschrijvingen van hoe de implementatie voldoet aan de vereisten van elk principe besproken.

## <a name="guidance-and-recommendations"></a>Richtlijnen en aanbevelingen

### <a name="vpn-and-expressroute"></a>VPN en ExpressRoute

Een beveiligde VPN-tunnel of [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) moet worden geconfigureerd voor een veilig verbinding met de resources die zijn geïmplementeerd als onderdeel van deze referentiearchitectuur van PaaS web toepassing. Door op de juiste wijze instellen van een VPN of ExpressRoute, toevoegen klanten een beveiligingslaag voor gegevens in transit.

Door het implementeren van een beveiligde VPN-tunnel met Azure, kan een virtuele particuliere verbinding tussen een on-premises netwerk en een Azure-netwerk worden gemaakt. Deze verbinding vindt plaats via Internet en kunnen klanten veilig &quot;tunnel&quot; informatie binnen een gecodeerde verbinding tussen de klant&#39;s netwerk en Azure. Site-naar-site VPN is een veilige, volwassen technologie die is geïmplementeerd door bedrijven van alle groottes decennia. De [IPsec-tunnelmodus](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) bij deze optie als een versleutelingsmechanisme voor wordt gebruikt.

Omdat het verkeer binnen de VPN-tunnel via Internet met een site-naar-site-VPN, biedt Microsoft een andere verbinding nog veiliger optie. Azure ExpressRoute is een speciale WAN koppeling tussen Azure en een on-premises locatie of een Exchange-hostingprovider. Als het ExpressRoute-verbindingen gaan niet via het Internet, bieden deze verbindingen een meer betrouwbaarheid, hogere snelheden, kortere wachttijden en hogere beveiliging dan gebruikelijke verbindingen via Internet. Bovendien, omdat dit een directe verbinding van de klant is&#39;s-provider voor telecommunicatie, de gegevens niet via Internet kan worden verzonden en daarom geen toegang heeft tot deze.

Aanbevolen procedures voor het implementeren van een beveiligd hybride netwerk dat een on-premises netwerk naar Azure uitbreidt zijn [beschikbaar](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

## <a name="disclaimer"></a>Vrijwaring

- Dit document is uitsluitend ter informatie bedoeld. MICROSOFT BIEDT GEEN GARANTIES, EXPLICIETE, IMPLICIETE OF WETTELIJKE GARANTIE VOOR DE INFORMATIE IN DIT DOCUMENT. Dit document wordt geleverd &quot;as-is.&quot; Informatie en meningen in dit document, inclusief URL's en andere websiteverwijzingen, kunnen zonder kennisgeving worden gewijzigd. Klanten die in dit document leest draagt het risico van het gebruik ervan.
- Dit document biedt klanten met een enkel wettelijk recht op enig intellectueel in andere Microsoft-producten of oplossingen.
- Klanten kunnen kopiëren en gebruiken van dit document voor interne referentiedoeleinden.
- Bepaalde aanbevelingen in dit document kan leiden tot grotere hoeveelheden gegevens, netwerk- of gebruik van de compute-bronnen in Azure en een klant kan verhogen&#39;s Azure-licentie of abonnementskosten.
- Deze architectuur is bedoeld om te fungeren als een basis voor klanten om aan te passen op hun specifieke behoeften en mag niet worden gebruikt als-is in een productieomgeving.
- Dit document is ontwikkeld als referentie en mag niet worden gebruikt voor het definiëren van alle middelen waarmee een klant kan voldoen aan specifieke nalevingsvereisten en voorschriften. Klanten moeten juridische ondersteuning van hun organisatie op goedgekeurde klantimplementaties gezocht.
