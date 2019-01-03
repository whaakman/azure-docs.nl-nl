---
title: Azure-beveiliging en naleving blauwdruk - PaaS-webtoepassing voor Australië beveiligd
description: Azure-beveiliging en naleving blauwdruk - PaaS-webtoepassing voor Australië beveiligd
services: security
author: meladie
ms.assetid: 708aa129-b226-4e02-85c6-1f86e54564e4
ms.service: security
ms.topic: article
ms.date: 08/23/2018
ms.author: meladie
ms.openlocfilehash: 3505d65b55807010904494079532fe5741e6df77
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53601165"
---
# <a name="azure-security-and-compliance-blueprint---paas-web-application-for-australia-protected"></a>Azure-beveiliging en naleving blauwdruk - PaaS-webtoepassing voor Australië beveiligd

## <a name="overview"></a>Overzicht

Deze Azure-beveiliging en naleving blauwdruk biedt richtlijnen voor de implementatie van een platform als een service (PaaS)-omgeving die geschikt is voor het verzamelen, opslaan en ophalen van de overheid AU beschermde gegevens die voldoet aan de doelstellingen van de Australische overheid informatie beveiliging handmatig (ISM) die worden geproduceerd door de Australian Signals Directorate (ASD). Deze blauwdruk brengt een algemene referentiearchitectuur en helpt bij de juiste afhandeling van gevoelige government-gegevens in een omgeving beveiligd, voldoen aan het beleid, meerdere lagen laten zien.

Deze referentiearchitectuur, Implementatiehandleiding en risicomodel voorzien in een basis voor klanten om hun eigen accrediteringsprocessen plannings- en system helpen van klanten workloads in een ASD-compatibele manier implementeren in Azure. Klanten kunnen ervoor kiezen om een Azure VPN-Gateway of ExpressRoute federatieve services te gebruiken en integreren van on-premises bronnen met Azure-resources te implementeren. Klanten moeten rekening houden met de gevolgen voor de beveiliging van het gebruik van on-premises bronnen. Er is aanvullende configuratie vereist om te voldoen aan alle vereisten, als ze op basis van de details van de uitvoering van elke klant variëren kunnen.

ASD-naleving bereiken, vereist dat een Information Security Registered Assessor het systeem controleert. Klanten zijn verantwoordelijk voor het uitvoeren van de juiste beveiliging en naleving evaluaties van een oplossing die is gebouwd met behulp van deze architectuur als vereisten kunnen variëren op basis van de details van de uitvoering van elke klant.

## <a name="architecture-diagram-and-components"></a>Diagram van architectuur en onderdelen
Deze oplossing biedt een referentiearchitectuur voor een PaaS-webtoepassing met een Azure SQL Database-back-end. De web-App wordt gehost in een geïsoleerde Azure App Service Environment, dit een exclusieve privéomgeving in een Azure datacenter is. De omgeving verdeelt verkeer voor de web-App voor virtuele machines die worden beheerd door Azure. Alle web application verbindingen vereisen TLS met een minimale versie 1.2. Deze architectuur bevat ook netwerkbeveiligingsgroepen, een Application Gateway, Azure DNS en Load Balancer.

De architectuur biedt een beveiligde hybride omgeving die een on-premises netwerk naar Azure uitbreidt, zodat web gebaseerde workloads veilig worden geopend door zakelijke gebruikers van een organisatie particuliere LAN-netwerk of via internet. Voor on-premises oplossingen is de klant verantwoordelijk en die verantwoordelijk is voor alle aspecten van beveiliging, activiteiten en naleving.

De Azure-resources opgenomen in deze oplossing kunnen verbinding maken met een on-premises netwerk- of Datacenter CO-locatiefaciliteit (bijvoorbeeld CDC in Canberra) via een IPSec VPN tussen sites met een VPN-Gateway en ExpressRoute. De beslissing om het gebruik van een VPN-verbinding moet worden gedaan met de classificatie van de verzonden gegevens en het netwerkpad in gedachten. Klanten die grootschalige, bedrijfskritieke met big data-vereisten, moeten rekening houden met een hybride netwerkarchitectuur met behulp van ExpressRoute voor privénetwerkverbinding met Azure-services. Raadpleeg de [richtlijnen en aanbevelingen](#guidance-and-recommendations) sectie voor meer informatie over de verbindingsmechanismen naar Azure.

Federatie met Azure Active Directory moet worden gebruikt om gebruikers te verifiëren met on-premises referenties en toegang krijgen tot alle bronnen in de cloud met behulp van een on-premises Active Directory Federation Services-infrastructuur. Active Directory Federation Services kunt vereenvoudigde, beveiligde identiteit federation- en bieden web eenmalige aanmelding voor deze hybride omgeving. Raadpleeg de [richtlijnen en aanbevelingen](#guidance-and-recommendations) sectie voor meer details van Azure Active Directory-installatie.

De oplossing maakt gebruik van Azure Storage-accounts, die klanten configureren kunnen met Storage Service Encryption kunt onderhouden van de vertrouwelijkheid van gegevens in rust. Azure slaat drie kopieën van gegevens binnen de geselecteerde regio van de klant voor tolerantie. Azure-regio's worden geïmplementeerd in flexibele regioparen en geografisch redundante opslag zorgt ervoor dat gegevens worden gerepliceerd naar de tweede regio met drie kopieën ook. Dit voorkomt dat een negatieve gebeurtenis om de locatie van de primaire gegevens van de klant leidt tot een verlies van gegevens.

Voor een betere beveiliging, worden alle Azure-resources in deze oplossing worden beheerd als een resourcegroep via Azure Resource Manager. Azure Active Directory-rollen gebaseerd toegangsbeheer wordt gebruikt voor het beheren van toegang geïmplementeerd resources en sleutels in Azure Key Vault. Status van het bestandssysteem wordt bewaakt door Azure Security Center en Azure Monitor. Klanten configureren beide bewakingsservices voor het vastleggen van Logboeken en status van het bestandssysteem in een enkele, eenvoudig navigeerbaar dashboard weer te geven. Azure Application Gateway is geconfigureerd als een firewall in preventiemodus en is niet toegestaan door het verkeer dat is niet TLS 1.2 of hoger. De oplossing maakt gebruik van Azure-toepassing Service-omgeving v2 te isoleren van de weblaag in een omgeving met meerdere tenants.

![PaaS-webtoepassing voor Australië beschermde referentiearchitectuur](images/au-protected-paaswa-architecture.png?raw=true "PaaS-webtoepassing voor Architectuurdiagram AU-beveiligde referentie")

Deze oplossing maakt gebruik van de volgende Azure-services. Meer informatie vindt u in de [architectuur](#deployment-architecture) sectie.

- Application Gateway
    - Web Application Firewall
        - Firewallmodus: preventie
        - regel is ingesteld: OWASP
        - -Listener-poort: 443
- Application Insights
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
- Azure Log Analytics
- Azure Virtual Network
    - (1) /16 netwerk
    - (4) /24 netwerken
    - Netwerkbeveiligingsgroepen
- Netwerkbeveiligingsgroepen
- Recovery Services-kluis
- Azure Web App

Deze blauwdruk bevat Azure-Services die niet zijn gecertificeerd voor gebruik op de beveiligde-indeling door de Australische Cyber Security Center (ACSC). Microsoft raadt aan dat klanten de gepubliceerde beveiliging aangeraden en auditrapporten met betrekking tot deze Azure-Services en hun risk management framework gebruiken om te bepalen of de Azure-Service is geschikt voor hun interne erkenning en gebruik in de Beveiligde indeling.

## <a name="deployment-architecture"></a>Implementatie-architectuur
De volgende sectie bevat de elementen van de implementatie en uitvoering.

**Azure Resource Manager**: [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) kunnen klanten voor het werken met de resources in de oplossing als een groep. Klanten kunnen implementeren, bijwerken of verwijderen van alle resources voor de oplossing in een enkele, gecoördineerde bewerking. Klanten een sjabloon voor implementatie gebruiken en deze sjabloon kunt werken voor verschillende omgevingen, zoals testen, fasering en productie. Resource Manager biedt beveiliging, controle en tagfuncties die helpen bij klanten die hun resources beheren na de implementatie.

**Bastionhost**: De bastionhost is de enkel ingangspunt waarmee gebruikers toegang krijgen tot de geïmplementeerde resources in deze omgeving. De bastionhost biedt een beveiligde verbinding met de geïmplementeerde resources door toe te staan alleen extern verkeer vanaf openbare IP-adressen op een veilige lijst. Als u wilt toestaan dat verkeer van extern bureaublad (RDP), moet de bron van het verkeer worden gedefinieerd in de netwerkbeveiligingsgroep.

Deze oplossing maakt u een virtuele machine als een domein bastionhost met de volgende configuraties:
-   [Antimalware-uitbreiding](https://docs.microsoft.com/azure/security/azure-security-antimalware)
-   [Azure Diagnostics-extensie](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template)
-   [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) met behulp van Azure Key Vault
-   Een [beleid voor automatisch afsluiten](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) verkleind van resources van de virtuele machine als deze niet in gebruik

**App Service Environment v2**: De [Azure App Service Environment](https://docs.microsoft.com/azure/app-service/environment/intro) is een App Service-functie waarmee een volledig geïsoleerde en toegewezen omgeving voor App Service-toepassingen op een grote schaal veilig kunnen worden uitgevoerd.

App Service-omgevingen zijn geïsoleerd om één klant toepassingen alleen worden uitgevoerd en worden altijd geïmplementeerd in een virtueel netwerk. Klanten hebben meer controle over zowel toepassing binnenkomend en uitgaand netwerkverkeer en toepassingen zeer snelle, veilige verbindingen kunnen maken via virtuele netwerken tot on-premises bedrijfsresources.

Gebruik van App Service-omgevingen voor deze architectuur toestaan voor de volgende besturingselementen/configuraties:

- App Service-omgevingen moeten worden geconfigureerd voor het gebruik van het serviceplan geïsoleerd
    - Verschillende App Service-omgevingen voor toepassingen op verschillende classificaties configureren
- Hosten in een beveiligde Azure-netwerk en netwerkbeveiligingsregels
- App Service-omgevingen die zijn geconfigureerd met een zelf-ondertekend interne load balancer-certificaat voor HTTPS-communicatie. Als een best practice, wordt aangeraden het gebruik van een vertrouwde certificeringsinstantie voor verbeterde beveiliging.
- [Interne load balancing-modus](https://docs.microsoft.com/azure/app-service-web/app-service-environment-with-internal-load-balancer) (modus 3)
- Uitschakelen [TLS v1.0 en v1.1](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-custom-settings)
- Wijziging [TLS-codering](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-custom-settings)
- Besturingselement [binnenkomende verkeer N/W poorten](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-control-inbound-traffic)
- [Web application firewall: gegevens beperken](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-web-application-firewall)
- Toestaan dat [verkeer van de Azure SQL Database](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-network-architecture-overview)

**Azure-Web-App**: [Azure App Service](https://docs.microsoft.com/azure/app-service/) kunnen klanten bouwen en hosten van webtoepassingen in de programmeertaal van hun keuze zonder het beheren van infrastructuur. Het biedt automatisch schalen en een hoge beschikbaarheid, ondersteuning voor zowel Windows als Linux en maakt automatische implementaties mogelijk vanuit GitHub, Azure DevOps Services of een willekeurige Git-repo.

### <a name="virtual-network"></a>Virtual Network
De architectuur definieert een particulier virtueel netwerk met een adresruimte van 10.200.0.0/16.

**Netwerkbeveiligingsgroepen**: [Netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) bevatten toegangsbeheerlijsten die verkeer binnen een virtueel netwerk toestaan of weigeren. Netwerkbeveiligingsgroepen kunnen worden gebruikt om verkeer een subnet of afzonderlijke virtuele machine te beveiligen. De volgende netwerkbeveiligingsgroepen bestaan:
- 1 netwerkbeveiligingsgroep voor Application Gateway
- 1 netwerkbeveiligingsgroep voor App Service-omgeving
- 1 netwerkbeveiligingsgroep voor Azure SQL Database
- 1 netwerkbeveiligingsgroep voor bastionhost

Elk van de netwerkbeveiligingsgroepen specifieke poorten en protocollen openen, zodat de oplossing kunt veilig en goed werken. Bovendien zijn de volgende configuraties ingeschakeld voor elke netwerkbeveiligingsgroep:

  - [Diagnostische logboeken en gebeurtenissen](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) zijn ingeschakeld en die zijn opgeslagen in een storage-account
  - Azure Log Analytics is verbonden met de [beveiligingsgroep van diagnostische gegevens over het netwerk](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json)

**Subnetten**: Elk subnet is gekoppeld aan de bijbehorende netwerkbeveiligingsgroep.

**Azure DNS**: De Domain Name System of DNS is verantwoordelijk voor het omzetten van (of het oplossen van) de naam van een website of -service naar het IP-adres. [Azure DNS](https://docs.microsoft.com/azure/dns/dns-overview) is een hostingservice voor DNS-domeinen die naamomzetting met behulp van Azure-infrastructuur biedt. Door domeinen te hosten in Azure, kunnen gebruikers de DNS-records met dezelfde referenties, API's, hulpprogramma's en facturering als andere Azure-services beheren. Azure DNS ondersteunt ook persoonlijke DNS-domeinen.

**Azure Load Balancer**: [Azure Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview) kunnen klanten hun toepassingen schalen en hoge beschikbaarheid voor services. Load Balancer ondersteunt zowel binnenkomende als uitgaande-scenario's, en lage latentie, hoge doorvoer, en kan worden geschaald tot miljoenen stromen voor alle TCP en UDP-toepassingen.

### <a name="data-in-transit"></a>Gegevens die onderweg zijn
Alle communicatie naar en van Azure-datacenters versleutelt Azure standaard. 

Voor beveiligde gegevens tijdens de overdracht van netwerken van klanten, de architectuur maakt gebruik van Azure Internet of een ExpressRoute met een VPN-Gateway zijn geconfigureerd met IPSEC.

Bovendien worden alle transacties naar Azure via de Azure management portal via HTTPS met TLS 1.2 optreden.

### <a name="data-at-rest"></a>Data-at-rest
De architectuur beveiligt gegevens in rust via versleuteling, controle-database en andere metingen.

**Azure Storage**: Om versleutelde data-at-rest-vereisten te voldoen aan alle [Azure Storage](https://azure.microsoft.com/services/storage/) maakt gebruik van [Storage Service Encryption](https://docs.microsoft.com/azure/storage/storage-service-encryption). Dit helpt te beschermen en beveiligen van gegevens ter ondersteuning van de organisatie beveiligingsverplichtingen en nalevingsvereisten zijn gedefinieerd door de Australische overheid ISM.

**Azure Disk Encryption**: [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) maakt gebruik van de BitLocker-functie van Windows om volumeversleuteling voor gegevensschijven te bieden. De oplossing kan worden geïntegreerd met Azure Key Vault voor het beheren en beheren van de versleutelingssleutels op de schijf.

**Azure SQL Database**: De Azure SQL Database-exemplaar maakt gebruik van de volgende metingen van de database-beveiliging:
-   [Active Directory-verificatie en autorisatie](https://docs.microsoft.com/azure/sql-database/sql-database-AAD-authentication) maakt identiteitsbeheer van databasegebruikers en andere Microsoft-services op één centrale locatie.
-   [SQL database auditing](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) databasegebeurtenissen bijgehouden en geschreven naar een auditlogboek in Azure storage-account.
-   Azure SQL Database is geconfigureerd voor het gebruik van [transparante gegevensversleuteling](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql), die voert realtime versleuteling en ontsleuteling van de database, gekoppelde back-ups en transactielogboekbestanden ter bescherming van gegevens op over de rest. Transparante gegevensversleuteling biedt de zekerheid dat de opgeslagen gegevens niet is onderworpen aan onbevoegde toegang.
-   [Firewall-regels](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) voorkomen dat u alle toegang tot de database-servers, totdat de juiste machtigingen zijn toegekend. De firewall verleent toegang tot databases op basis van het IP-adres waar de aanvraag vandaan komt.
-   [SQL Threat Detection](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) kunt u detectie en reactie op mogelijke bedreigingen wanneer deze zich voordoen, dankzij beveiligingswaarschuwingen voor verdachte databaseactiviteiten, potentiële kwetsbaarheden, SQL-injectieaanvallen en afwijkende database-toegang patronen. SQL Threat Detection integreert waarschuwingen met [Azure Security Center](https://azure.microsoft.com/services/security-center/), die bevat details van verdachte activiteiten en aanbevolen actie voor het onderzoeken en tegenhouden.
-   [Altijd versleuteld kolommen](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) ervoor te zorgen dat gevoelige gegevens nooit wordt weergegeven als tekst zonder opmaak in de database-systeem. Na het inschakelen van versleuteling van gegevens, alleen clienttoepassingen of toepassingsservers met toegang tot de sleutels gegevens zijn toegankelijk als tekst zonder opmaak.
- [SQL Database dynamische gegevensmaskering](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) blootstelling van gevoelige gegevens maskeren van de gegevens naar niet-gemachtigde gebruikers of toepassingen. Dynamische gegevensmaskering automatisch detecteren van potentieel gevoelige gegevens en stel de juiste maskers moet worden toegepast. Dit helpt bij het beperken van toegang dat gevoelige gegevens via niet-geautoriseerde toegang tot de database niet bestaat. Klanten moet aanpassen, dynamische gegevensmaskering instellingen om te voldoen aan het schema van de database.

### <a name="identity-management"></a>Identiteitsbeheer
Klanten kunnen gebruikmaken van on-premises Active Directory Federated Services om te federeren met [Azure Active Directory](https://azure.microsoft.com/services/active-directory/), dit is van Microsoft cloud-gebaseerde directory- en identiteitsbeheer management service met meerdere tenants. [Azure Active Directory Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) on-premises directory's integreert met Azure Active Directory. Alle gebruikers in deze oplossing vereist Azure Active Directory-accounts, met inbegrip van gebruikers met toegang tot de Azure SQL Database. Met federation aanmelden, kunnen gebruikers zich aanmelden bij Azure Active Directory en worden geverifieerd bij Azure-resources met behulp van on-premises referenties.

Bovendien helpen de volgende mogelijkheden van Azure Active Directory toegang tot gegevens in de Azure-omgeving beheren:
- Verificatie van de toepassing wordt uitgevoerd met behulp van Azure Active Directory. Zie voor meer informatie, [toepassingen integreren met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Bovendien de versleuteling van de kolom database maakt gebruik van Azure Active Directory om te verifiëren van de toepassing naar Azure SQL Database. Zie voor meer informatie over het [bescherming van gevoelige gegevens in Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
- [Op rollen gebaseerd toegangsbeheer in Azure](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) kunnen beheerders om te voorzien in specifieke toegangsmachtigingen te verlenen alleen de mate van toegang dat gebruikers moeten om hun werk te definiëren. In plaats van geeft elke onbeperkte gebruikersmachtigingen voor Azure-resources, beheerders kunnen toestaan dat alleen bepaalde acties voor toegang tot gegevens. Abonnementstoegang is beperkt tot de beheerder van het abonnement.
- [Azure Active Directory Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) kunnen klanten voor het minimaliseren van het aantal gebruikers die toegang tot bepaalde gegevens hebben. Beheerders kunnen Azure Active Directory Privileged Identity Management gebruiken om te detecteren, beperken en controleren van bevoegde identiteiten en hun toegang tot bronnen. Deze functionaliteit kan ook worden gebruikt om af te dwingen op aanvraag, just-in-time beheerderstoegang wanneer dat nodig is.
- [Azure Active Directory Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) detecteert mogelijke beveiligingsproblemen die betrekking hebben op een organisaties identiteiten en onderzoekt het probleem Hiermee configureert u automatische antwoorden op gedetecteerde verdachte activiteit met betrekking tot een organisatie-id's verdachte incidenten, onderneem gepaste actie op te lossen.

**Azure multi-factor Authentication**: Ter bescherming van identiteiten, moet meervoudige verificatie worden geïmplementeerd. [Azure multi-factor Authentication](https://azure.microsoft.com/services/multi-factor-authentication/) is een gebruiksvriendelijke, schaalbare en betrouwbare oplossing waarmee u een tweede verificatiemethode om gebruikers te beschermen. Azure multi-factor Authentication gebruikt de kracht van de cloud en kan worden geïntegreerd met on-premises Active Directory en aangepaste toepassingen. Deze beveiliging wordt uitgebreid naar essentiële scenario's met hoog volume.

### <a name="security"></a>Beveiliging
**Geheimen management**: De oplossing maakt gebruik van [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) voor het beheer van sleutels en geheimen. Met Azure Sleutelkluis kunt u de cryptografische sleutels en geheimen beveiligen die door cloudtoepassingen en -services worden gebruikt. De volgende mogelijkheden van Azure Key Vault zodat klanten gegevensbeveiliging:
- Geavanceerde beleidsregels zijn geconfigureerd op basis van behoefte.
- Toegangsbeleid voor Key Vault zijn met de minimaal vereiste machtigingen voor sleutels en geheimen gedefinieerd.
- Verloopdatum hebben alle sleutels en geheimen in Key Vault.
- Alle sleutels in Key Vault zijn beveiligd door gespecialiseerde hardware security modules. Het sleuteltype is dat een hardware security module beveiligde 2048-bits RSA-sleutel.
- Minimaal vereiste machtigingen met behulp van op rollen gebaseerd toegangsbeheer worden verleend door alle gebruikers en identiteiten.
- Diagnostische logboeken voor Key Vault worden ingeschakeld met een bewaarperiode van ten minste 365 dagen.
- Toegestane cryptografiebewerkingen voor sleutels zijn beperkt tot die nodig is.

**Azure Security Center**: Met [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro), klanten kunnen centraal toepassen en beheren van beveiligingsbeleid voor workloads, blootstelling aan bedreigingen beperken en detecteren en reageren op aanvallen. Azure Security Center heeft bovendien toegang tot bestaande configuraties van Azure-services voor configuratie en de serviceaanbevelingen om u te helpen bij het beveiligingspostuur verbeteren en gegevens te beschermen.

Azure Security Center maakt gebruik van tal van mogelijkheden voor het detecteren om klanten van potentiële aanvallen die gericht is op hun omgeving. Deze waarschuwingen bevatten waardevolle informatie over de trigger van de waarschuwing, de betrokken resources en de bron van de aanval. Azure Security Center bevat een set [vooraf gedefinieerde beveiligingswaarschuwingen](https://docs.microsoft.com/azure/security-center/security-center-alerts-type), die worden geactiveerd wanneer een bedreiging of verdachte activiteit wordt gedetecteerd. [Aangepaste waarschuwingsregels](https://docs.microsoft.com/azure/security-center/security-center-custom-alert) in Azure Security Center kunnen klanten voor het definiëren van nieuwe beveiligingswaarschuwingen op basis van gegevens die al zijn verzameld voor hun omgeving.

Azure Security Center biedt beveiligingswaarschuwingen en incidenten, waardoor het eenvoudiger voor klanten om te detecteren en mogelijke beveiligingsproblemen te verhelpen. Een [threat intelligence-rapport](https://docs.microsoft.com/azure/security-center/security-center-threat-report) wordt gegenereerd voor elke bedreigingen gedetecteerde voor het incident response-teams helpt bij het onderzoeken en bescherm bedreigingen.

**Application Gateway**: De architectuur vermindert het risico van beveiligingsproblemen met behulp van een toepassingsgateway met een web application firewall en het ruleset OWASP is ingeschakeld. Aanvullende mogelijkheden zijn onder andere:

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
- **Activiteitenlogboeken**: [Activiteitenlogboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bieden inzicht in bewerkingen die worden uitgevoerd op resources in een abonnement. Activiteitenlogboeken kunnen u eenvoudiger bepalen van een bewerking initiator, tijdstip van de gebeurtenis en status.
- **Diagnostische logboeken**: [Diagnostische logboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) bevatten alle logboeken die zijn gegenereerd door elke resource. Deze logboeken bevatten de logboeken voor Windows-systeem, Azure Storage-Logboeken, Key Vault-auditlogboeken en Application Gateway toegangs- en firewall-Logboeken. Alle logboeken met diagnostische gegevens schrijven naar een gecentraliseerd en versleutelde Azure storage-account voor archivering. De bewaarperiode is om te voldoen aan vereisten voor de bewaarperiode van de organisatie-specifieke gebruiker-configureren, tot maximaal 730 dagen.

**Log Analytics**: Deze logboeken worden geconsolideerd [Log Analytics](https://azure.microsoft.com/services/log-analytics/) voor verwerking, opslag en -dashboardrapporten. Zodra de verzameld, worden de gegevens zijn onderverdeeld in afzonderlijke tabellen voor elk gegevenstype, waarmee alle gegevens worden geanalyseerd samen kunt, ongeacht de oorspronkelijke bron. Daarnaast wordt Azure Security Center kan worden geïntegreerd met Log Analytics, waardoor klanten kunnen Log Analytics-query's gebruiken voor toegang tot hun beveiligingsgegevens voor de gebeurtenis en combineren met gegevens van andere services.

De volgende Log Analytics [beheeroplossingen](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions) zijn opgenomen als onderdeel van deze architectuur:
-   [Active Directory-evaluatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): De oplossing statuscontrole van Active Directory beoordeelt het risico en de gezondheid van server-omgevingen op een vast interval en biedt een geprioriteerde lijst met aanbevelingen die specifiek zijn voor de geïmplementeerde serverinfrastructuur.
- [SQL-evaluatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): De oplossing SQL-statuscontrole beoordeelt het risico en de gezondheid van server-omgevingen op een vast interval en biedt klanten een geprioriteerde lijst met aanbevelingen die specifiek zijn voor de geïmplementeerde serverinfrastructuur.
- [Status van agent](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): De oplossing status van Agent rapporteert het aantal agents zijn geïmplementeerd en hun geografische verdeling, evenals hoeveel agents die niet meer reageert en het aantal agents die zijn operationele gegevens kan verzenden.
-   [Activity Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): De oplossing Activity Log Analytics biedt ondersteuning voor analyse van de Azure-activiteitenlogboeken voor alle Azure-abonnementen voor een klant.

**Azure Automation**: [Azure Automation](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker) worden opgeslagen, wordt uitgevoerd runbooks worden beheerd. In deze oplossing te runbooks verzamelen van Logboeken van Azure SQL Database. De automatisering [bijhouden](https://docs.microsoft.com/azure/automation/automation-change-tracking) oplossing kan klanten eenvoudig wijzigingen in de omgeving identificeren.

**Azure Monitor**: [Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) helpt gebruikers bij het bijhouden van prestaties, beveiliging en trends te identificeren doordat organisaties om te controleren, waarschuwingen maken en archiveren van gegevens, inclusief bijhouden API-aanroepen in hun Azure-resources.

Azure Network Watcher: [Azure Network Watcher] 9 https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) voorziet in hulpprogramma's als u wilt bewaken, diagnosticeren, bekijk metrische gegevens, en in- of uitschakelen Logboeken voor resources in een Azure-netwerk.  Noordelijke entiteiten moeten Network Watcher-stroomlogboeken voor nsg's en virtuele Machines implementeren. Deze logboeken moeten worden opgeslagen in een speciale storage-account dat alleen beveiligingslogboeken worden opgeslagen in en toegang tot het opslagaccount dat moet worden beveiligd met rollen gebaseerd toegangsbeheer.

## <a name="threat-model"></a>Risicomodel

Het diagram van de gegevensstroom voor deze referentiearchitectuur is beschikbaar voor [downloaden](https://aka.ms/au-protected-paaswa-tm) of vindt u hieronder. Dit model kunt klanten inzicht krijgen in de punten van de mogelijke risico's in de infrastructuur van het systeem als u wijzigingen aanbrengt.

![PaaS-webtoepassing voor AU-PROTECTED risicomodel](images/au-protected-paaswa-threat-model.png?raw=true "PaaS-webtoepassing voor AU-beveiliging tegen bedreigingen modeldiagram")

## <a name="compliance-documentation"></a>Naleving-documentatie
Deze documentatie naleving wordt geproduceerd door Microsoft op basis van platforms en services van Microsoft. Vanwege de grote verscheidenheid aan implementaties van klanten biedt deze documentatie een algemene aanpak voor een oplossing voor alleen die wordt gehost in de Azure-omgeving. Klanten kunnen identificeren en andere producten en services op basis van hun eigen omgevingen en de bedrijfsresultaten. Klanten die gebruik van on-premises bronnen moeten rekening houden met de beveiliging en bewerkingen voor de on-premises bronnen. De gedocumenteerde oplossing kan worden aangepast door klanten aan hun specifieke on-premises en de beveiligingsvereisten voor.

De [Azure-beveiliging en naleving blauwdruk - AU-PROTECTED klant verantwoordelijkheid Matrix](https://aka.ms/au-protected-crm) geeft een lijst van alle beveiligingsmaatregelen die zijn vereist voor AU-protocol Deze matrix bevat of de implementatie van elk besturingselement de verantwoordelijkheid van Microsoft, de klant is, of gedeeld tussen de twee.

De [Azure-beveiliging en naleving blauwdruk - AU-PROTECTED PaaS Web Application implementatie Matrix](https://aka.ms/au-protected-paaswa-cim) bevat informatie waarop AU beschermde besturingselementen worden verholpen door de PaaS-architectuur voor web-toepassing, met inbegrip van gedetailleerde beschrijvingen van de manier waarop de implementatie voldoet aan de vereisten van elk besturingselement vallen.

## <a name="guidance-and-recommendations"></a>Richtlijnen en aanbevelingen
### <a name="vpn-and-expressroute"></a>VPN en ExpressRoute

Een beveiligde tunnel met IPSec VPN moet worden geconfigureerd voor een veilig verbinding met de resources die zijn geïmplementeerd als onderdeel van deze referentiearchitectuur IaaS web application van geclassificeerde gegevens. Door in te stellen op de juiste wijze een IPSec VPN, kunnen klanten een beveiligingslaag voor gegevens toevoegen tijdens de overdracht.

Door het implementeren van een beveiligde tunnel met IPSec VPN met Azure, kan een virtuele particuliere verbinding tussen een on-premises netwerk en een Azure-netwerk worden gemaakt. Deze verbinding kan plaatsvinden via Internet en kan klanten veilig 'tunnel' gegevens in een gecodeerde verbinding tussen het netwerk en Azure van de klant. Site-naar-site VPN is een veilige, volwassen technologie die is geïmplementeerd door bedrijven van alle groottes decennia. 

Omdat het verkeer binnen de VPN-tunnel via Internet met een site-naar-site-VPN, biedt Microsoft een optie voor particuliere verbinding. Met Azure ExpressRoute is een speciale koppeling tussen Azure en een on-premises locatie of een Exchange-hostingprovider, en wordt beschouwd als een particulier netwerk. Als het ExpressRoute-verbindingen gaan niet via het Internet, bieden deze verbindingen een meer betrouwbaarheid, hogere snelheden en kortere wachttijden dan gebruikelijke verbindingen via Internet. Bovendien, omdat dit een rechtstreekse verbinding van de klant telecommunicatie-provider, de gegevens niet via Internet kan worden verzonden en daarom geen toegang heeft tot deze.

Aanbevolen procedures voor het implementeren van een beveiligd hybride netwerk dat een on-premises netwerk naar Azure uitbreidt zijn [beschikbaar](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid). 

Ter bescherming van geclassificeerde gegevens, ongeacht of de Internet- of Azure ExpressRoute, moeten klanten hun IPSec-VPN configureren door toe te passen van de volgende instellingen:

• De initiator van de VPN-klant moet worden geconfigureerd voor een SA-levensduur van niet meer dan 14400 seconden.
• De initiator van de VPN-klant moet IKEv1 agressieve modus uitschakelen.
• De initiator van de VPN-klant moet Perfect Forward Secrecy configureren.
• De VPN-Initiator van de klant moet het gebruik van de HMAC-SHA256 of hoger configureren.

Configuratie-opties voor VPN-apparaten en IPSec / IKE-parameters is [beschikbaar](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices) voor controle.

### <a name="azure-active-directory-setup"></a>Installatie van de Azure Active Directory
[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) is essentieel voor het beheren van de implementatie en inrichting van toegang tot personeel interactie met de omgeving. Een bestaande Windows Server Active Directory kunnen worden geïntegreerd met Azure Active Directory in [vier klikken](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-express).

Bovendien [Azure Active Directory Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) kunnen klanten om Federatie te configureren met on-premises [Active Directory Federation Services]( https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-azure-adfs) en Azure Active Directory. Met federatieve aanmelden, kunnen klanten gebruikers zich aanmelden bij Azure Active Directory gebaseerde services met hun on-premises wachtwoorden en zonder te hoeven hun wachtwoord opnieuw invoeren terwijl op het bedrijfsnetwerk inschakelen. U kunt een nieuwe installatie van Active Directory Federation Services implementeren met de optie Federatie met Active Directory Federation Services, of kunt u een bestaande installatie in een farm met Windows Server 2012 R2.

Om te voorkomen dat geclassificeerde gegevens synchroniseren naar Azure Active Directory, kunnen klanten de kenmerken die worden gerepliceerd naar Azure Active Directory door toe te passen van de volgende instellingen in Azure Active Directory Connect beperken:

- [Filteren inschakelen](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-configure-filtering)
- [Wachtwoord-hashsynchronisatie uitschakelen](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-implement-password-hash-synchronization)
-   [Wachtwoord terugschrijven uitschakelen](https://docs.microsoft.com/azure/active-directory/authentication/quickstart-sspr)
-   [Write-back van apparaat uitschakelen](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-feature-device-writeback)
-   Behoud de standaardinstellingen voor [onopzettelijke verwijderingen voorkomen](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-feature-prevent-accidental-deletes) en [Automatische upgrade](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-feature-automatic-upgrade)


## <a name="disclaimer"></a>Vrijwaring

 - Dit document is uitsluitend ter informatie bedoeld. MICROSOFT BIEDT GEEN GARANTIES, EXPLICIETE, IMPLICIETE OF WETTELIJKE GARANTIE VOOR DE INFORMATIE IN DIT DOCUMENT. Dit document wordt geleverd ' as-is. " Informatie en meningen in dit document, inclusief URL's en andere websiteverwijzingen, kunnen zonder kennisgeving worden gewijzigd. Klanten die in dit document leest draagt het risico van het gebruik ervan.
 - Dit document biedt klanten met een enkel wettelijk recht op enig intellectueel in andere Microsoft-producten of oplossingen.
 - Klanten kunnen kopiëren en gebruiken van dit document voor interne referentiedoeleinden.
 - Bepaalde aanbevelingen in dit document kunnen leiden tot grotere hoeveelheden gegevens, netwerk- of gebruik van de compute-bronnen in Azure en de Azure-licentie of abonnement kosten van een klant kunnen verhogen.
 - Deze architectuur is bedoeld om te fungeren als een basis voor klanten om aan te passen op hun specifieke behoeften en mag niet worden gebruikt als-is in een productieomgeving.
 - Dit document is ontwikkeld als referentie en mag niet worden gebruikt voor het definiëren van alle middelen waarmee een klant kan voldoen aan specifieke nalevingsvereisten en voorschriften. Klanten moeten juridische ondersteuning van hun organisatie op goedgekeurde klantimplementaties gezocht.
