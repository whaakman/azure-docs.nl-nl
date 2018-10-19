---
title: Azure-beveiliging en naleving blauwdruk - PaaS-webtoepassing voor SP NIST 800-171
description: Azure-beveiliging en naleving blauwdruk - PaaS Web Application NIST SP 800-171
services: security
author: jomolesk
ms.assetid: eea21a0a-5930-43e8-937f-5419c20744c9
ms.service: security
ms.topic: article
ms.date: 07/31/2018
ms.author: jomolesk
ms.openlocfilehash: e7851b39327e61f1676ae0cf1c3bff3de75b56bd
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/18/2018
ms.locfileid: "49409268"
---
# <a name="azure-security-and-compliance-blueprint---paas-web-application-for-nist-special-publication-800-171"></a>Azure-beveiliging en naleving blauwdruk - PaaS-webtoepassing voor NIST Special Publication 800-171

## <a name="overview"></a>Overzicht
[NIST Special Publication 800-171](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-171.pdf) bevat richtlijnen voor het beveiligen van de gecontroleerde niet-geclassificeerde informatie (CUI) die deel uitmaakt van nonfederal informatiesystemen en organisaties. SP NIST 800-171 stelt 14 families van de beveiligingsvereisten voor het beveiligen van de vertrouwelijkheid van CUI.

Deze Azure-beveiliging en naleving blauwdruk biedt hulp bij het implementeren van een platform als een service (PaaS)-webtoepassing in Azure die een van NIST SP subset klanten 800-171-besturingselementen. Deze oplossing toont manieren waarin klanten kunnen voldoen aan bepaalde beveiligings- en nalevingsvereisten. Het fungeert ook als basis voor klanten om te bouwen en configureren van hun eigen web-App in Azure.

Deze referentiearchitectuur, gekoppelde Implementatiehandleiding en risicomodel zijn bedoeld om te fungeren als een basis voor klanten aan te passen aan hun specifieke wensen. Ze al dan niet mogen worden gebruikt als-is in een productieomgeving. Er is onvoldoende om volledig te voldoen aan de vereisten van SP NIST 800-171 implementeren van deze architectuur zonder aanpassingen. Klanten zijn verantwoordelijk voor het uitvoeren van de juiste beveiliging en naleving evaluaties van een oplossing die is gebouwd met behulp van deze architectuur. Vereisten kunnen verschillen op basis van de details van de uitvoering van elke klant.

## <a name="architecture-diagram-and-components"></a>Diagram van architectuur en onderdelen

Deze Azure-beveiliging en naleving blauwdruk biedt een referentiearchitectuur voor een PaaS-webtoepassing met een Azure SQL Database-back-end. De web-App wordt gehost in een geïsoleerde App Service-omgeving, dit een exclusieve privéomgeving in een Azure-datacenter is. De omgeving verdeelt verkeer voor de web-App voor virtuele machines (VM's) worden beheerd door Azure. Deze architectuur bevat ook netwerkbeveiligingsgroepen (nsg's), een Azure-toepassingsgateway, Azure DNS en Azure Load Balancer.

Voor geavanceerde analyse en rapportage, kunnen Azure SQL-databases worden geconfigureerd met columnstore-indexen. Azure SQL-databases kunnen omhoog of omlaag geschaald of volledig afgesloten in reactie op klanten te worden gebruikt. Alle SQL-verkeer wordt versleuteld met SSL door de toevoeging van zelfondertekende certificaten. Als een best practice raadt Azure het gebruik van een vertrouwde certificeringsinstantie voor verbeterde beveiliging.

De oplossing maakt gebruik van Azure Storage-accounts, die klanten configureren kunnen met Storage Service Encryption kunt onderhouden van de vertrouwelijkheid van gegevens in rust. Azure slaat drie kopieën van gegevens binnen de geselecteerde datacenter van een klant voor tolerantie. Geografisch redundante opslag zorgt ervoor dat gegevens gerepliceerd naar een secundair Datacenter honderden van mijl opslaan en opnieuw als drie kopieën binnen die Datacenter opgeslagen. Deze benadering wordt voorkomen dat een negatieve gebeurtenis in het primaire datacenter van de klant leidt tot een verlies van gegevens.

Voor een betere beveiliging, worden alle resources in deze oplossing worden beheerd als een resourcegroep via Azure Resource Manager. Azure Active Directory (Azure AD)-op rollen gebaseerd toegangsbeheer (RBAC) wordt gebruikt voor het beheren van toegang tot geïmplementeerde resources. Deze bronnen omvatten klant sleutels in Azure Key Vault. Status van het bestandssysteem wordt bewaakt met Azure Monitor. Klanten configureren deze monitoring-service voor het vastleggen van Logboeken. Status van het bestandssysteem wordt weergegeven in één dashboard dat is eenvoudig te gebruiken.

SQL-Database is meestal beheerd via SQL Server Management Studio. Deze wordt uitgevoerd vanuit een lokale computer die geconfigureerd voor toegang tot de SQL-database via een beveiligde VPN- of Azure ExpressRoute-verbinding.

Application Insights biedt realtime prestatiebeheer en analytics gebruikt via Azure Log Analytics. *Microsoft raadt aan dat u een VPN of ExpressRoute-verbinding voor het beheer en de gegevens importeren in de referentie-architectuur subnet configureren.*

![PaaS-webtoepassing voor SP NIST 800-171 verwijzing Architectuurdiagram](images/nist171-paaswa-architecture.png "PaaS-webtoepassing voor Architectuurdiagram van SP NIST 800-171-verwijzing")

Deze oplossing maakt gebruik van de volgende Azure-services. Zie voor meer informatie de [architectuur](#deployment-architecture) sectie.

- Azure Virtual Machines
    - (1) management/bastionhost (Windows Server 2016 Datacenter)
- Azure Virtual Network
    - (1) /16 netwerk
    - (4) /24 netwerken
    - (4) de netwerkbeveiligingsgroepen
- Azure Application Gateway
    - Web Application Firewall
        - Firewallmodus: preventie
        - Regelset: OWASP
        - -Listener-poort: 443
- Application Insights
- Azure Active Directory
- App Service Environment v2
- Azure Automation
- Azure DNS
- Azure Key Vault
- Azure Load Balancer
- Azure Monitor
- Azure Resource Manager
- Azure Security Center
- Azure SQL Database
- Azure Storage
- Azure Log Analytics
- Azure Automation
- Azure Web Apps

## <a name="deployment-architecture"></a>Implementatie-architectuur
De volgende sectie bevat de elementen van de implementatie en uitvoering.

**Azure Resource Manager**: [Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) door klanten kan worden gebruikt om te werken met de resources in de oplossing als een groep. Klanten kunnen implementeren, bijwerken of verwijderen van alle resources voor de oplossing in een enkele, gecoördineerde bewerking. Klanten gebruiken een sjabloon voor implementatie. De sjabloon kunt voor verschillende omgevingen, zoals testen, Faseren en productie werken. Resource Manager biedt beveiliging, controle en tagfuncties die helpen bij klanten die hun resources beheren na de implementatie.

**Bastionhost**: de bastionhost is het één punt van de vermelding die gebruikers gebruiken kunnen voor toegang tot de geïmplementeerde resources in deze omgeving. De bastionhost biedt een beveiligde verbinding met de geïmplementeerde resources door alleen extern verkeer vanaf openbare IP-adressen op een veilige lijst. Als u wilt toestaan dat verkeer van extern bureaublad, moet de oorzaak van het verkeer worden gedefinieerd in de NSG.

Deze oplossing maakt u een virtuele machine als een domein bastionhost met de volgende configuraties:
-   [Antimalware-uitbreiding](https://docs.microsoft.com/azure/security/azure-security-antimalware).
-   [Azure Diagnostics-extensie](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template).
-   [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) met Key Vault.
-   Een [beleid voor automatisch afsluiten](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) verkleind van VM-resources als deze niet in gebruik.
-   [Windows Defender Credential Guard](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard) is ingeschakeld, zodat de referenties en andere geheimen in een beveiligde omgeving die is geïsoleerd van het besturingssysteem wordt uitgevoerd.

**Web-Apps**: [Web-Apps](https://docs.microsoft.com/azure/app-service/) is een functie van Azure App Service. Klanten kunnen deze gebruiken om te bouwen en hosten van webtoepassingen in de programmeertaal van hun keuze zonder het beheren van infrastructuur. Het biedt automatisch schalen en hoge beschikbaarheid. Het biedt ondersteuning voor Windows en Linux en maakt automatische implementaties vanuit GitHub, Azure DevOps of een willekeurige Git-repo mogelijk.

**App Service-omgeving**: [App Service-omgeving](https://docs.microsoft.com/azure/app-service/environment/intro) is een functie-App Service. Het biedt een volledig geïsoleerde en toegewezen omgeving voor App Service-toepassingen op een grote schaal veilig kunnen worden uitgevoerd.

De App Service-omgeving is geïsoleerd voor het uitvoeren van één toepassing. Deze altijd geïmplementeerd in een virtueel netwerk. De referentiearchitectuur is isolatie van tenants voltooid vanwege de isolatie-functie, en wordt deze verwijderd uit Azure multitenant-omgeving. Klanten hebben meer controle over zowel toepassing binnenkomend en uitgaand netwerkverkeer. Toepassingen kunnen maken van snelle, veilige verbindingen via virtuele netwerken tot on-premises bedrijfsresources. Klanten kunnen "automatisch schalen" met App Service-omgeving op basis van metrische gegevens laden, beschikbare budget of een ingesteld schema.

Gebruik van App Service-omgeving voor deze architectuur biedt de volgende besturingselementen en configuraties:

- Hosten in een beveiligde Azure-netwerk en de beveiligingsregels voor verbindingen.
- Zelf-ondertekend interne load balancer-certificaat voor HTTPS-communicatie. Als een best practice, wordt aangeraden het gebruik van een vertrouwde certificeringsinstantie voor verbeterde beveiliging.
- [Interne load balancing-modus](https://docs.microsoft.com/azure/app-service-web/app-service-environment-with-internal-load-balancer) (modus 3).
- Uitschakelen [TLS 1.0](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-custom-settings).
- Wijziging [TLS cipher](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-custom-settings).
- Besturingselement [binnenkomende verkeer N/W poorten](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-control-inbound-traffic).
- [Web application firewall: gegevens beperken](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-web-application-firewall).
- Toestaan dat [Azure SQL Database-verkeer](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-network-architecture-overview).

### <a name="virtual-network"></a>Virtueel netwerk
De architectuur definieert een particulier virtueel netwerk met een adresruimte van 10.200.0.0/16.

**Netwerkbeveiligingsgroepen**: [nsg's](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) bevatten toegangsbeheerlijsten die verkeer binnen een virtueel netwerk toestaan of weigeren. Nsg's kunnen worden gebruikt om verkeer een subnet of afzonderlijke virtuele machine te beveiligen. De volgende nsg's bestaan:
- Een NSG voor Application Gateway
- Een NSG voor App Service-omgeving
- Een NSG voor SQL-Database
- Een NSG voor bastionhost

Elk van de nsg's heeft specifieke poorten en protocollen open zodat de oplossing kan veilig en goed werken. Bovendien zijn de volgende configuraties voor elke NSG ingeschakeld:
  - [Diagnostische logboeken en gebeurtenissen](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) zijn ingeschakeld en die zijn opgeslagen in een storage-account.
  - Log Analytics is verbonden met de [diagnostische gegevens van de NSG](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json).

**Subnetten**: elk subnet is gekoppeld aan de bijbehorende NSG.

**Azure DNS**: de Domain Name System (DNS) is verantwoordelijk voor het omzetten van (of het oplossen van) de naam van een website of -service naar het IP-adres. [Azure DNS](https://docs.microsoft.com/azure/dns/dns-overview) is een hostingservice voor DNS-domeinen die naamomzetting biedt met behulp van Azure-infrastructuur. Door domeinen te hosten in Azure, kunnen gebruikers de DNS-records beheren met behulp van dezelfde referenties, API's, hulpprogramma's en facturering als andere Azure-services. Azure DNS ondersteunt ook persoonlijke DNS-domeinen.

**Azure Load Balancer**: [Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview) kan worden gebruikt door klanten hun toepassingen schalen en hoge beschikbaarheid voor services. Load Balancer biedt ondersteuning voor scenario's voor binnenkomend en uitgaand. Het lage latentie en hoge doorvoer en kan worden geschaald tot miljoenen stromen voor alle TCP en UDP-toepassingen.

### <a name="data-in-transit"></a>Gegevens die onderweg zijn
Alle communicatie naar en vanuit Azure-datacenters versleutelt Azure standaard. Alle transacties met Azure Storage via Azure portal plaatsvinden via HTTPS.

### <a name="data-at-rest"></a>Data-at-rest

De architectuur beveiligt gegevens in rust via versleuteling, controle-database en andere metingen.

**Azure Storage**: om te voldoen aan vereisten voor versleutelde gegevens in rust, alle [opslag](https://azure.microsoft.com/services/storage/) maakt gebruik van [Storage Service Encryption](https://docs.microsoft.com/azure/storage/storage-service-encryption). Deze functie helpt beschermen en beveiligen van gegevens ter ondersteuning van de organisatie beveiligingsverplichtingen en nalevingsvereisten door SP NIST 800-171 gedefinieerd.

**Azure Disk Encryption**: [schijfversleuteling](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) maakt gebruik van de BitLocker-functie van Windows om volumeversleuteling voor gegevensschijven te bieden. De oplossing kan worden geïntegreerd met Key Vault voor het beheren en beheren van de versleutelingssleutels op de schijf.

**Azure SQL Database**: de SQL-Database-exemplaar maakt gebruik van de volgende metingen van de database-beveiliging:
-   [Active Directory-verificatie en autorisatie](https://docs.microsoft.com/azure/sql-database/sql-database-AAD-authentication) maakt identiteitsbeheer van databasegebruikers en andere Microsoft-services op één centrale locatie.
-   [SQL database auditing](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) databasegebeurtenissen bijgehouden en geschreven naar een auditlogboek in Azure storage-account.
-   SQL-Database is geconfigureerd voor het gebruik van [transparante gegevensversleuteling](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql). Het voert realtime versleuteling en ontsleuteling van de database, gekoppelde back-ups en transactielogboekbestanden om gegevens in rust te beveiligen. Transparante gegevensversleuteling biedt de zekerheid dat de opgeslagen gegevens niet is onderworpen aan onbevoegde toegang.
-   [Firewall-regels](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) voorkomen dat u alle toegang tot de database-servers, totdat de juiste machtigingen zijn toegekend. De firewall verleent toegang tot databases op basis van het IP-adres waar de aanvraag vandaan komt.
-   [SQL Threat Detection](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) kunt u detectie en reactie op mogelijke bedreigingen wanneer deze zich voordoen. Het biedt beveiligingswaarschuwingen voor verdachte databaseactiviteiten, potentiële kwetsbaarheden, SQL-injectieaanvallen en afwijkende patronen voor databasetoegang.
-   [Versleutelde kolommen](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) ervoor te zorgen dat gevoelige gegevens nooit wordt weergegeven als tekst zonder opmaak in de database-systeem. Nadat versleuteling van gegevens is ingeschakeld, alleen clienttoepassingen of toepassingsservers met toegang tot de sleutels kunnen toegang tot gegevens van de tekst zonder opmaak.
- [Dynamische gegevensmaskering](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) blootstelling van gevoelige gegevens maskeren van de gegevens naar de replicagegevens gebruikers of toepassingen. Het kan automatisch detecteren van potentieel gevoelige gegevens en voorstellen van de juiste maskers moet worden toegepast. Dynamische gegevensmaskering helpt bij de toegang beperken zodat gevoelige gegevens niet van de database via niet-gemachtigde toegang afsluiten. *Klanten zijn verantwoordelijk voor het aanpassen van de instellingen om te voldoen aan het schema van de database.*

### <a name="identity-management"></a>Identiteitsbeheer
De volgende technologieën bieden mogelijkheden voor het beheren van toegang tot gegevens in de Azure-omgeving:
-   [Azure AD](https://azure.microsoft.com/services/active-directory/) Microsofts multitenant cloud-gebaseerde directory en identity management-service is. Alle gebruikers voor deze oplossing worden gemaakt in Azure AD en gebruikers die toegang hebben tot de SQL-database.
-   Verificatie van de toepassing wordt uitgevoerd met behulp van Azure AD. Zie voor meer informatie over het [toepassingen integreren met Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Azure AD de versleuteling van de kolom database ook gebruikt voor verificatie van de toepassing met SQL-Database. Zie voor meer informatie over het [bescherming van gevoelige gegevens in SQL-Database](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   [Azure RBAC](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) door beheerders kan worden gebruikt om te voorzien in specifieke toegangsmachtigingen definiëren. Met, kunnen ze alleen de mate van toegang verlenen dat gebruikers moeten hun werk. In plaats van elke Onbeperkte gebruikerstoegang voor Azure-resources geven, beheerders kunnen toestaan dat alleen bepaalde acties voor toegang tot bronnen en gegevens. Abonnementstoegang is beperkt tot de beheerder van het abonnement.
- [Azure Active Directory Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) door klanten kan worden gebruikt om het minimaliseren van het aantal gebruikers die toegang tot bepaalde gegevens hebben. Beheerders kunnen Azure AD Privileged Identity Management gebruiken om te detecteren, beperken en controleren van bevoegde identiteiten en hun toegang tot bronnen. Deze functionaliteit kan ook worden gebruikt om af te dwingen op aanvraag, just-in-time beheerderstoegang wanneer dat nodig is.
- [Azure Active Directory Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) detecteert mogelijke beveiligingsproblemen die invloed hebben op de identiteiten in een organisatie. Automatische antwoorden op gedetecteerde verdachte activiteit met betrekking tot een organisatie-id's configureren Ook onderzoekt het probleem verdachte incidenten passende actie op te lossen moet ondernemen.

### <a name="security"></a>Beveiliging
**Geheimen management**: de oplossing maakt gebruik van [Key Vault](https://azure.microsoft.com/services/key-vault/) voor het beheer van sleutels en geheimen. Key Vault helpt Beveilig cryptografische sleutels en geheimen die worden gebruikt door cloudtoepassingen en -services. De volgende mogelijkheden voor Key Vault zodat klanten gegevensbeveiliging:
- Geavanceerde beleidsregels zijn geconfigureerd op basis van behoefte.
- Toegangsbeleid voor Key Vault zijn met de minimaal vereiste machtigingen voor sleutels en geheimen gedefinieerd.
- Verloopdatum hebben alle sleutels en geheimen in Key Vault.
- Alle sleutels in Key Vault zijn beveiligd door gespecialiseerde hardware security modules. Het sleuteltype is een hardware security module beveiligde 2048-bits RSA-sleutel.
- Alle gebruikers en identiteiten zijn minimaal vereiste machtigingen verleend met behulp van RBAC.
- Diagnostische logboeken voor Key Vault worden ingeschakeld met een bewaarperiode van ten minste 365 dagen.
- Toegestane cryptografiebewerkingen voor sleutels zijn beperkt tot die nodig is.

**Azure Security Center**: met [Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro), klanten kunnen centraal toepassen en beheren van beveiligingsbeleid voor workloads, blootstelling aan bedreigingen beperken en detecteren en reageren op aanvallen. Security Center heeft ook toegang tot bestaande configuraties van Azure-services voor configuratie en de serviceaanbevelingen om u te helpen bij het beveiligingspostuur verbeteren en gegevens te beschermen.

Security Center maakt gebruik van tal van mogelijkheden voor het detecteren om te waarschuwen klanten van potentiële aanvallen die zijn gericht hun omgevingen. Deze waarschuwingen bevatten waardevolle informatie over de trigger van de waarschuwing, de betrokken resources en de bron van de aanval. Security Center bevat een set [vooraf gedefinieerde beveiligingswaarschuwingen](https://docs.microsoft.com/azure/security-center/security-center-alerts-type) die worden geactiveerd wanneer een bedreiging of verdachte activiteit plaatsvindt. Klanten kunnen gebruikmaken van [aangepaste waarschuwingsregels](https://docs.microsoft.com/azure/security-center/security-center-custom-alert) voor het definiëren van nieuwe beveiligingswaarschuwingen op basis van gegevens die al zijn verzameld voor hun omgeving.

Security Center biedt beveiligingswaarschuwingen en incidenten. Security Center kunt u eenvoudiger voor klanten om te detecteren en mogelijke beveiligingsproblemen te verhelpen. Een [threat intelligence-rapport](https://docs.microsoft.com/azure/security-center/security-center-threat-report) wordt gegenereerd voor elke bedreigingen gedetecteerde. Incident response teams kunnen de rapporten gebruiken wanneer ze onderzoeken en bedreigingen verhelpen.

**Azure Application Gateway**: de architectuur vermindert het risico van beveiligingsproblemen met behulp van een toepassingsgateway met een web application firewall is geconfigureerd en de regelset OWASP is ingeschakeld. Aanvullende mogelijkheden zijn onder andere:

- [End-end SSL-](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell).
- Schakel [SSL-offload](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-portal).
- Uitschakelen [TLS v1.0 en v1.1](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell).
- [De Web application firewall](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) (preventiemodus).
- [Preventiemodus](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal) met OWASP 3.0 regelset.
- Schakel [Diagnostische logboekregistratie](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics).
- [Aangepaste statuscontroles](https://docs.microsoft.com/azure/application-gateway/application-gateway-create-gateway-portal).
- [Security Center](https://azure.microsoft.com/services/security-center) en [Azure Advisor](https://docs.microsoft.com/azure/advisor/advisor-security-recommendations) bieden extra beveiliging en meldingen. Security Center biedt ook een reputatie-systeem.

### <a name="logging-and-auditing"></a>Logboekregistratie en controle

Azure-services zich uitgebreid systeem en activiteit van gebruikers, evenals systeemstatus:
- **Activiteitenlogboeken**: [activiteitenlogboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bieden inzicht in bewerkingen die worden uitgevoerd op resources in een abonnement. Activiteitenlogboeken kunnen u eenvoudiger bepalen van een bewerking initiator, tijdstip van de gebeurtenis en status.
- **Diagnostische logboeken**: [diagnostische logboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) bevatten alle logboeken die zijn gegenereerd door elke resource. Deze logboeken bevatten de logboeken voor Windows-systeem, opslaglogboeken, Key Vault-auditlogboeken en Application Gateway toegangs- en firewall-Logboeken. Alle logboeken met diagnostische gegevens schrijven naar een gecentraliseerd en versleutelde Azure storage-account voor archivering. Gebruikers kunnen de bewaartermijn maximaal configureren tot 730 dagen, om te voldoen aan hun specifieke wensen.

**Azure Log Analytics**: Logboeken worden geconsolideerd [Log Analytics](https://azure.microsoft.com/services/log-analytics/) voor verwerking, opslag en -dashboardrapporten. Nadat de gegevens zijn verzameld, onderverdeeld in afzonderlijke tabellen voor elk gegevenstype in Log Analytics-werkruimten. Op deze manier kunnen alle gegevens samen kunnen worden geanalyseerd, ongeacht de oorspronkelijke bron. Security Center kan worden geïntegreerd met Log Analytics. Klanten kunnen Log Analytics-query's gebruiken voor toegang tot hun beveiligingsgegevens voor de gebeurtenis en combineren met gegevens van andere services.

De volgende Log Analytics [beheeroplossingen](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions) zijn opgenomen als onderdeel van deze architectuur:
-   [Active Directory-evaluatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): het Active Directory statuscontrole oplossing beoordeelt het risico en de gezondheid van server-omgevingen op een vast interval. Het biedt een geprioriteerde lijst met aanbevelingen die specifiek zijn voor de geïmplementeerde serverinfrastructuur.
- [SQL-evaluatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): statuscontrole van de SQL-oplossing beoordeelt het risico en de gezondheid van server-omgevingen op een vast interval. Daardoor kunnen klanten met een geprioriteerde lijst met aanbevelingen die specifiek zijn voor de geïmplementeerde serverinfrastructuur.
- [Status van agent](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): oplossing status van de Agent rapporteert het aantal agents zijn geïmplementeerd en hun geografische verdeling. Deze rapporten ook het aantal agents zijn niet meer reageert en het aantal agents dat operationele gegevens verzendt.
-   [Activity Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): de Activity Log Analytics-oplossing biedt ondersteuning voor analyse van de Azure-activiteitenlogboeken voor alle Azure-abonnementen voor een klant.

**Azure Automation**: [Automation](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker) worden opgeslagen, wordt uitgevoerd runbooks worden beheerd. In deze oplossing te runbooks verzamelen van Logboeken van de SQL-Database. Klanten kunnen gebruikmaken van de automatisering [bijhouden](https://docs.microsoft.com/azure/automation/automation-change-tracking) oplossing voor het herkennen van wijzigingen in de omgeving.

**Azure Monitor**: [Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) helpt gebruikers bij het bijhouden van prestaties, veiligheid en trends te identificeren. Organisaties kunnen deze gebruiken om te controleren, waarschuwingen maken en archiveren van gegevens. Ze kunnen ook API-aanroepen in hun Azure-resources bijhouden.

**Application Insights**: [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) is een uitbreidbaar application performance management-service voor webontwikkelaars op meerdere platforms. Application Insights detecteert afwijkende prestaties. Klanten kunnen deze gebruiken voor het bewaken van de live web-App. Application Insights bevat krachtige analysehulpmiddelen om klanten problemen vaststellen en begrijpen wat gebruikers doen met hun app te helpen. Deze&#39;s bedoeld voor klanten, prestaties en bruikbaarheid continu te verbeteren.

## <a name="threat-model"></a>Risicomodel

Het diagram van de gegevensstroom voor deze referentiearchitectuur is beschikbaar voor [downloaden](https://aka.ms/nist171-paaswa-tm) of kunt u hier vinden. Dit model kunt klanten inzicht krijgen in de punten van de mogelijke risico's in de infrastructuur van system wanneer deze wijzigingen aanbrengt.

![PaaS-webtoepassing voor SP NIST 800-171 risicomodel](images/nist171-paaswa-threat-model.png "PaaS-webtoepassing voor SP NIST 800-171 risicomodel")

## <a name="compliance-documentation"></a>Naleving-documentatie
De [Azure-beveiliging en naleving blauwdruk - SP NIST 800-171 klant verantwoordelijkheid Matrix](https://aka.ms/nist171-crm) geeft een lijst van alle beveiligingsmaatregelen die zijn vereist voor SP NIST 800-171. Deze matrix bevat of de implementatie van elk besturingselement de verantwoordelijkheid van Microsoft, de klant is, of gedeeld tussen de twee.

De [Azure-beveiliging en naleving blauwdruk - SP NIST 800-171 PaaS Web Application Control-implementatie Matrix](https://aka.ms/nist171-paaswa-cim) bevat informatie over welke SP NIST 800-171-besturingselementen worden verholpen door de architectuur voor webtoepassingen PaaS. Dit omvat gedetailleerde beschrijvingen van de manier waarop de implementatie voldoet aan de vereisten van elk besturingselement vallen.

## <a name="guidance-and-recommendations"></a>Richtlijnen en aanbevelingen
### <a name="vpn-and-expressroute"></a>VPN en ExpressRoute
Een beveiligde VPN-tunnel of [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) moet worden geconfigureerd voor een veilig verbinding met de resources die zijn geïmplementeerd als onderdeel van deze referentiearchitectuur van PaaS web toepassing. Door op de juiste wijze instellen van een VPN of ExpressRoute, toevoegen klanten een beveiligingslaag voor gegevens in transit.

Door het implementeren van een beveiligde VPN-tunnel met Azure, kan een virtuele particuliere verbinding tussen een on-premises netwerk en een Azure-netwerk worden gemaakt. Deze verbinding vindt plaats via Internet en kan klanten veilig 'tunnel' gegevens in een gecodeerde verbinding tussen het netwerk en Azure van de klant. Site-naar-site VPN is een veilige, volwassen technologie die is geïmplementeerd door bedrijven van alle groottes decennia. De [IPsec-tunnelmodus](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) bij deze optie als een versleutelingsmechanisme voor wordt gebruikt.

Omdat het verkeer binnen de VPN-tunnel over Internet met een site-naar-site-VPN wordt verzonden, biedt Microsoft een andere optie voor nog veiliger verbinding. ExpressRoute is een speciale WAN koppeling tussen Azure en een on-premises locatie of een Exchange-hostingprovider. ExpressRoute-verbindingen rechtstreeks verbinding maken met een klant telecommunicatie-provider. Als gevolg hiervan de gegevens niet via Internet worden verzonden en wordt niet blootgesteld aan het. Deze verbindingen bieden meer betrouwbaarheid, hogere snelheden, kortere wachttijden en hogere beveiliging dan gewone verbindingen.

Aanbevolen procedures voor het implementeren van een beveiligd hybride netwerk dat een on-premises netwerk naar Azure uitbreidt zijn [beschikbaar](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

## <a name="disclaimer"></a>Vrijwaring

 - Dit document is uitsluitend ter informatie bedoeld. MICROSOFT BIEDT GEEN GARANTIES, EXPLICIETE, IMPLICIETE OF WETTELIJKE GARANTIE VOOR DE INFORMATIE IN DIT DOCUMENT. Dit document wordt geleverd ' as-is. " Informatie en meningen in dit document, inclusief URL's en andere websiteverwijzingen, kunnen zonder kennisgeving worden gewijzigd. Klanten die in dit document leest draagt het risico van het gebruik ervan.
 - Dit document biedt klanten met een enkel wettelijk recht op enig intellectueel in andere Microsoft-producten of oplossingen.
 - Klanten kunnen kopiëren en gebruiken van dit document voor interne referentiedoeleinden.
 - Bepaalde aanbevelingen in dit document kunnen leiden tot grotere hoeveelheden gegevens, netwerk- of gebruik van de compute-bronnen in Azure en de Azure-licentie of abonnement kosten van een klant kunnen verhogen.
 - Deze architectuur is bedoeld om te fungeren als een basis voor klanten om aan te passen op hun specifieke behoeften en mag niet worden gebruikt als-is in een productieomgeving.
 - Dit document is ontwikkeld als referentie en mag niet worden gebruikt voor het definiëren van alle middelen waarmee een klant kan voldoen aan specifieke nalevingsvereisten en voorschriften. Klanten moeten juridische ondersteuning van hun organisatie op goedgekeurde klantimplementaties gezocht.
