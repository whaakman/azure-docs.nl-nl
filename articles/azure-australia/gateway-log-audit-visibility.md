---
title: Logboek registratie, controle en zicht baarheid van de gateway in azure Australië
description: Logboek registratie, controle en zicht baarheid binnen de Australische regio's configureren om te voldoen aan de specifieke vereisten van het beleid voor de Australische overheid, de regelgeving en de wetgeving.
author: galey801
ms.service: azure-australia
ms.topic: conceptual
ms.date: 07/22/2019
ms.author: grgale
ms.openlocfilehash: 153b9d503dfece404455fbb7e8cb51c51686ec57
ms.sourcegitcommit: 3073581d81253558f89ef560ffdf71db7e0b592b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68824292"
---
# <a name="gateway-logging-auditing-and-visibility-in-azure-australia"></a>Logboek registratie, controle en zicht baarheid van de gateway in azure Australië

Het detecteren en reageren op Cyber beveiligings dreigingen is afhankelijk van het genereren, verzamelen en analyseren van gegevens met betrekking tot de werking van een systeem.

Micro soft heeft ingebouwde hulpprogram ma's in azure om u te helpen bij het implementeren van logboek registratie, controle en zicht baarheid voor het beheren van de beveiliging van uw systemen die in azure zijn geïmplementeerd. Er is ook een referentie architectuur die wordt afgestemd op de ACSC-richt lijnen van de Australische Cyber Security Center (de gebruikers handleiding) en het doel van de Information Security-hand leiding (ISM).

Gateways fungeren als informatie stroom beheer mechanismen op de netwerklaag en kunnen ook informatie beheren in de hogere lagen van het OSI-model (Open System Interconnect). Gateways zijn nodig voor het beheren van gegevens stromen tussen beveiligings domeinen en het voor komen van onbevoegde toegang vanaf externe netwerken. Gezien de ernst van de gateways in het beheer van de gegevens stroom tussen beveiligings domeinen, kunnen storingen, met name bij hogere classificaties, ernstige gevolgen hebben. Als zodanig zijn robuuste mechanismen voor het melden van mede werkers in situaties met name belang rijk voor gateways.

Het implementeren van de mogelijkheden voor logboek registratie en waarschuwingen voor gateways kan helpen bij het detecteren van Cyber-beveiligings incidenten, inbreuken op pogingen en ongebruikelijke gebruiks patronen. Daarnaast neemt het opslaan van gebeurtenis logboeken op een afzonderlijke beveiligde logboek server het probleem voor een kwaadwillende persoon toe om de logboek gegevens te verwijderen om het bewijs van een gerichte Cyber-indringing te vernietigen.

## <a name="australian-cyber-security-centre-acsc-requirements"></a>Vereisten voor het Australische Cyber Security Center (ACSC)

De algemene beveiligings vereisten voor de Gemenebests systemen worden gedefinieerd in de ACSC Information Security-hand leiding (ISM). Om de Gemenebests-entiteiten te helpen voldoen aan deze vereisten in azure, wordt de *ACSC-gebruikers handleiding Microsoft Azure in het beveiligde* en *ACSC-certificerings rapport, Microsoft Azure* de publicaties de volgende specifieke vereisten met betrekking tot Logboek registratie, controle en zicht baarheid:

1. Om de Risico's te beperken die voortvloeien uit het gebruik van gedeelde onderliggende cloud resources, moeten Gemenebests entiteiten zich aanmelden Microsoft Azure geboden mogelijkheden, waaronder Azure Security Centre, Azure Monitor, Azure Policy en Azure Advisor om te helpen bij het uitvoeren van entiteiten Real-time bewaking van hun Azure-workloads

2. De ACSC raadt ook aan dat Gemenebests organisaties alle vastgelegde beveiligings logboeken door sturen naar de ACSC voor een volledige controle van de Australische overheid

3. Om het risico te beperken, moeten Gemenebests afdelingen worden geconfigureerd binnen hun Azure-abonnementen:

    * Azure Security Center inschakelen
    * Upgraden naar de Standard-laag
    * Automatische inrichting van micro soft Monitoring Agent op ondersteunde Azure-Vm's inschakelen
    * De beveiligings aanbevelingen en waarschuwingen in het dash board van Security Center regel matig controleren, priori teren en beperken

4. Overheids entiteiten moeten het door sturen van Logboeken en gebeurtenissen van hun Azure-abonnement naar het ACSC mogelijk maken om de ACSC te bieden met zicht baarheid van de niet-naleving van deze richt lijnen. Azure Event Hubs biedt de mogelijkheid om externe logboek streaming uit te voeren naar de ACSC of on-premises systemen die eigendom zijn van de Gemenebests organisatie

5. Gemenebests entiteiten moeten de logboek registratie die ze in azure inschakelt uitlijnen op de vereisten die zijn opgegeven in de ISM

6. Micro soft behoudt logboeken binnen Azure gedurende 90 dagen. Klant entiteiten moeten een archiverings regime voor logboeken implementeren om ervoor te zorgen dat Logboeken kunnen worden bewaard gedurende zeven jaar dat is vereist in de NAA AFDA

7. Gemenebests entiteiten met on-premises of op Azure gebaseerde Security Information and Event Management-mogelijkheden (SIEM) kunnen ook logboeken naar die systemen door sturen

8. Gemenebests entiteiten moeten Network Watcher-stroom logboeken implementeren voor netwerk beveiligings groepen (Nsg's) en Virtual Machines. Deze logboeken moeten worden opgeslagen in een specifiek opslag account met alleen beveiligings logboeken en de toegang tot het opslag account moet worden beveiligd met op rollen gebaseerd toegangs beheer

9. Gemenebests van de ACSC-gebruikers richtlijnen moeten implementeren om ervoor te zorgen dat Azure-workloads voldoen aan de bedoeling van de ISM voor logboek registratie en controle. Gemenebests van het Australische overheids gebruik van Azure moeten ook deel nemen aan de mogelijkheden van Azure om de ACSC te helpen bij het ontvangen van real-time bewaking, waarschuwingen en Logboeken.

## <a name="architecture"></a>Architectuur

Om zeker te zijn van het netwerk verkeer dat uw Azure-omgeving binnenkomt en verlaat, moet de benodigde logboek registratie worden ingeschakeld op de juiste set componenten. Dit zorgt voor volledige zicht baarheid van de omgeving en biedt de benodigde gegevens om analyses uit te voeren.

![Bewakings architectuur van Azure](media/visibility.png)

## <a name="components"></a>Onderdelen

De architectuur die hierboven wordt weer gegeven, bestaat uit discrete onderdelen die de functie van beide logboek bronnen, logboek verzameling, retentie van logboek registratie, logboek analyse of reactie op incidenten bieden. Deze architectuur bevat afzonderlijke onderdelen die doorgaans betrokken zijn bij Azure-implementaties die toegankelijk zijn via internet.

|Functies|Onderdelen|
|---|---|
|Logboek bronnen|<ul><li>Application Gateway</li><li>VPN Gateway</li><li>Azure Firewall</li><li>Virtuele netwerk apparaten</li><li>Azure Load Balancer</li><li>Virtuele machines</li><li>DNS-servers (Domain Naming System)</li><li>Syslog-en/of logboek verzamelings servers</li><li>NSG's</li><li>Azure-activiteitenlogboek</li><li>Diagnostisch logboek van Azure</li><li>Azure-beleid</li></ul>|
|Logboekverzameling|<ul><li>Event Hubs</li><li>Network Watcher</li><li>Log Analytics</li></ul>|
|Bewaar periode logboek|<ul><li>Azure Storage</li></ul>|
|Analyse vastleggen|<ul><li>Azure Security Center (ASC)</li><li>Azure Advisor</li><li>Log Analytics oplossingen<ul><li>Verkeersanalyse</li><li>DNS Analytics (preview-versie)</li><li>Activity Log Analytics</li></ul></li><li>SIEM</li><li>ACSC</li></ul>|
|Reageren op incidenten|<ul><li>Azure-waarschuwingen</li><li>Azure Automation</li></ul>|
|

De architectuur werkt eerst logboeken te genereren op basis van de benodigde bronnen en vervolgens te verzamelen in gecentraliseerde opslag plaatsen. Wanneer u de logboeken hebt verzameld, kunnen ze het volgende zijn:

* wordt door Azure Analysis Services gebruikt om inzicht te krijgen,
* Ga door naar externe systemen of
* Archief naar opslag voor lange termijn retentie.

Als u wilt reageren op belang rijke gebeurtenissen of incidenten die zijn geïdentificeerd door analyse hulpprogramma's, kunnen waarschuwingen worden geconfigureerd en automatisering is ontwikkeld om de nodige acties te ondernemen voor proactieve beheer en respons.

## <a name="general-guidance"></a>Algemene richtlijnen

Bij het implementeren van de onderdelen die in dit artikel worden vermeld, zijn de volgende algemene richt lijnen van toepassing:

* Valideer de beschik baarheid van de regio van services, zodat alle gegevens binnen de toegestane locaties blijven en op AU-centraal of AU-centraal 2 worden geïmplementeerd als de eerste voor keur voor beveiligde werk belastingen

* Raadpleeg het *Azure-ACSC-certificerings rapport-beveiligde 2018-* publicatie voor de certificerings status van afzonderlijke services en voer zelf beoordelingen uit op alle relevante onderdelen die niet in het rapport zijn opgenomen volgens de *ACSC-hand leiding voor gebruikers. Microsoft Azure op beveiligd*

* Voor onderdelen waarnaar niet in dit artikel wordt verwezen, moeten Gemenebests entiteiten de beginselen volgen die zijn opgenomen in Logboeken voor het genereren, vastleggen, analyseren en bewaren

* De logboek registratie, controle en zicht baarheid van systemen met hoge waarde identificeren en priori teren, evenals alle netwerk ingangen en uitgangs punten voor systemen die worden gehost in azure

* Consolideer logboeken en Minimaliseer het aantal exemplaren van logboek registratie Programma's, zoals opslag accounts, Log Analytics werk ruimten en Event Hubs

* Beheer rechten beperken via op rollen gebaseerd toegangs beheer

* Multi-factor Authentication (MFA) gebruiken voor accounts die resources beheren of configureren in azure

* Bij het verzamelen van centralising-logboeken over meerdere abonnementen moet u ervoor zorgen dat beheerders over de benodigde bevoegdheden beschikken voor elk abonnement

* Zorg ervoor dat de netwerk verbinding en de vereiste proxy configuratie voor Virtual Machines, waaronder Nva's (Network Virtual Appliance), logboek verzamelings servers en DNS-servers, verbinding kunnen maken met de benodigde Azure-Services, zoals de Log Analytics-werk ruimten, Event Hubs en opslag

* Micro soft Monitoring Agent (MMA) configureren voor het gebruik van TLS-versie 1,2

* Azure Policy gebruiken om naleving van vereisten te controleren en af te dwingen

* Versleuteling afdwingen voor alle gegevens opslagplaatsen, zoals opslag en data bases

* Lokaal redundante opslag (LRS) en moment opnamen gebruiken voor de beschik baarheid van opslag accounts en de bijbehorende gegevens

* Overweeg geo-redundante opslag (GRS) of buiten-site opslag om uit te lijnen met strategieën voor herstel na nood gevallen

|Resource|URL|
|---|---|
|Documenten voor Australische regelgeving en beleids regels voor naleving|[https://aka.ms/au-irap](https://aka.ms/au-irap)|
|Azure-producten: Australische regio's en niet-regionaal|[https://azure.microsoft.com/global-infrastructure/services/?regions=non-regional, Australië-centraal, Australië-centraal-2, Australië-Oost, Australië-Zuidoost](https://azure.microsoft.com/global-infrastructure/services/?regions=non-regional,australia-central,australia-central-2,australia-east,australia-southeast)|
|White Paper Microsoft Azure Beveiliging en audit log Management|[https://download.microsoft.com/download/B/6/C/B6C0A98B-D34A-417C-826E-3EA28CDFC9DD/AzureSecurityandAuditLogManagement_11132014.pdf](http://download.microsoft.com/download/B/6/C/B6C0A98B-D34A-417C-826E-3EA28CDFC9DD/AzureSecurityandAuditLogManagement_11132014.pdf)|
|Configuratie van micro soft Monitoring Agent|[https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent)|
|

## <a name="component-guidance"></a>Onderdeel richtlijnen

Deze sectie bevat informatie over het doel van elk onderdeel en de bijbehorende rol in de algemene architectuur voor logboek registratie, controle en zicht baarheid. Aanvullende koppelingen worden gegeven voor toegang tot nuttige bronnen, zoals naslag documentatie, hand leidingen en zelf studies.

## <a name="log-sources"></a>Logboek bronnen

Voordat een analyse, waarschuwing of rapportage kan worden voltooid, moeten de benodigde logboeken worden gegenereerd. Azure-logboeken zijn ingedeeld in Logboeken voor beheer, logboeken, gegevens vlak en verwerkte gebeurtenissen.

|type|Description|
|---|---|
|Beheer-en beheer logboeken|Informatie geven over Azure Resource Manager bewerkingen|
|Gegevens vlak logboeken|Geef informatie op over gebeurtenissen die worden gegenereerd als onderdeel van het gebruik van Azure-resources, zoals Logboeken in een virtuele machine en de diagnostische logboeken die beschikbaar zijn via Azure Monitor|
|Verwerkte gebeurtenissen|Geef informatie op over geanalyseerde gebeurtenissen/waarschuwingen die zijn verwerkt door Azure, zoals de locatie van Azure Security Center heeft verwerkte en geanalyseerde abonnementen voor het bieden van beveiligings waarschuwingen|
|

### <a name="application-gateway"></a>Application Gateway

Azure-toepassing gateway is een van de mogelijke ingangs punten in een Azure-omgeving, zodat u gegevens met betrekking tot binnenkomende verbindingen die communiceren met webtoepassingen moet vastleggen. Application Gateway kunt essentiële informatie geven over het gebruik van webtoepassingen, maar ook bij het detecteren van Cyber-beveiligings incidenten. Application Gateway verzendt meta gegevens naar het activiteiten logboek en de diagnostische Logboeken in Azure Monitor waar deze kunnen worden gedistribueerd in Log Analytics of naar een event hub-of opslag account kan worden verzonden.

|Resources|Koppelen|
|---|---|
|Documentatie over Application Gateway|[https://docs.microsoft.com/azure/application-gateway/](https://docs.microsoft.com/azure/application-gateway/)|
|Gids Application Gateway Snelstartgids|[https://docs.microsoft.com/azure/application-gateway/quick-create-portal](https://docs.microsoft.com/azure/application-gateway/quick-create-portal)|
|

### <a name="vpn-gateway"></a>VPN Gateway

De VPN Gateway is een mogelijk ingangs punt voor een breed scala aan communicatie in de Azure-omgeving, zoals de verbinding met een on-premises omgeving en beheer verkeer. Registratie op VPN-gateways biedt inzicht en traceer baarheid voor de verbindingen die tot de Azure-omgeving worden aangebracht. Logboek registratie kan zowel controles als analyses leveren als hulp bij het detecteren of onderzoeken van schadelijke of afwijkende verbindingen. VPN Gateway-logboeken worden naar het Azure Monitor-activiteiten logboek verzonden, waar ze kunnen worden gedistribueerd in Log Analytics of naar een event hub-of opslag account kan worden verdeeld.

|Resources|Koppelen|
|---|---|
|VPN Gateway-documentatie|[https://docs.microsoft.com/azure/vpn-gateway/](https://docs.microsoft.com/azure/vpn-gateway)|
|Specifieke richt lijnen voor de Australische overheid VPN Gateway|[Configuratie van Azure VPN Gateway](vpn-gateway.md)|
|

### <a name="azure-firewall"></a>Azure Firewall

Azure Firewall biedt een bepaald uitgangs punt van een Azure-omgeving en de gegenereerde logboeken, die informatie bevatten over pogingen en geslaagde uitgaande verbindingen, zijn een belang rijk onderdeel van uw strategie voor logboek registratie. In deze logboeken kan worden gecontroleerd of systemen werken zoals ze zijn ontworpen, en wordt u geholpen bij het detecteren van schadelijke code of Actors die proberen verbinding te maken met niet-geautoriseerde externe systemen. Azure Firewall schrijft logboeken naar het activiteiten logboek en Diagnostische logboeken in Azure Monitor waar het kan worden gebruikt in Log Analytics of kunnen worden gedistribueerd naar een event hub of een opslag account.

|Resources|Koppelen|
|---|---|
|Documentatie voor Azure Firewall|[https://docs.microsoft.com/azure/firewall/](https://docs.microsoft.com/azure/firewall)|
|Zelfstudie: Azure Firewall-logboeken en metrische gegevens bewaken|[https://docs.microsoft.com/azure/firewall/tutorial-diagnostics](https://docs.microsoft.com/azure/firewall/tutorial-diagnostics)|
|

### <a name="network-virtual-appliances-nva"></a>Virtuele netwerk apparaten (NVA)

Nva's kan worden gebruikt om de beschik bare beveiligings mogelijkheden in azure aan te vullen. De logboeken die zijn gegenereerd op Nva's kunnen waardevolle bronnen zijn in het detecteren van Cyber-beveiligings incidenten en zijn een belang rijk onderdeel van een algemene strategie voor logboek registratie, controle en zicht baarheid. Als u logboeken van Nva's wilt vastleggen, maakt u gebruik van micro soft Monitoring Agent (MMA). Voor Nva's die geen ondersteuning bieden voor de installatie van de MMA, kunt u een syslog of andere logboek verzamelings server gebruiken om logboeken door te sturen.

|Resources|Koppelen|
|---|---|
|Overzicht van virtuele netwerk apparaten|[https://azure.microsoft.com/solutions/network-appliances](https://azure.microsoft.com/solutions/network-appliances)|
|Documentatie voor NVA|Raadpleeg de documentatie van de leverancier over de implementatie van de relevante NVA in azure|
|

### <a name="azure-load-balancer"></a>Azure Load Balancer

Azure Load Balancer-logboeken worden gebruikt om nuttige informatie te verkrijgen over de verbindingen en het gebruik van systemen die zijn geïmplementeerd in Azure. Dit kan worden gebruikt voor de bewaking van de status en de beschik baarheid, maar vormt ook een ander belang rijk onderdeel voor het verkrijgen van het communicatie verkeer en het detecteren van schadelijke of afwijkende verkeers patronen. Azure Load Balancer logboeken vastleggen in het activiteiten logboek en Diagnostische logboeken in Azure Monitor waar het kan worden gedistribueerd in Log Analytics of worden verdeeld naar een event hub of een opslag account.

|Resources|Koppelen|
|---|---|
|Documentatie over Azure Load Balancer|[https://docs.microsoft.com/azure/load-balancer](https://docs.microsoft.com/azure/load-balancer)|
|Metrische gegevens en status diagnostiek voor Standard Load Balancer|[https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-diagnostics](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-diagnostics)|
|

### <a name="virtual-machines"></a>Virtuele machines

Virtual Machines zijn eind punten voor het verzenden en ontvangen van netwerk communicatie, het verwerken van gegevens en het leveren van services. Als Virtual Machines kunnen gegevens of cruciale systeem services hosten, zodat ze goed functioneren en gast-beveiligings incidenten kunnen worden gedetecteerd. Virtual Machines verschillende gebeurtenis-en audit logboeken verzamelen die de werking van het systeem en de acties die op dat systeem worden uitgevoerd, kunnen volgen. Logboeken die op Virtual Machines zijn verzameld, kunnen worden doorgestuurd naar een Log Analytics-werk ruimte met behulp van micro soft Monitoring Agent, waar ze kunnen worden geanalyseerd door Azure Security Center en toepasselijke Log Analytics oplossingen. Virtual Machines kan ook rechtstreeks worden geïntegreerd met Azure Event Hubs of op een SIEM, hetzij rechtstreeks hetzij via een logboek verzamelings server.

|Resources|Koppelen|
|---|---|
|Virtuele machines|[https://docs.microsoft.com/azure/virtual-machines](https://docs.microsoft.com/azure/virtual-machines)|
|Gegevens verzamelen uit Virtual Machines|[https://docs.microsoft.com/azure/log-analytics/log-analytics-quick-collect-azurevm](https://docs.microsoft.com/azure/log-analytics/log-analytics-quick-collect-azurevm)|
|Virtuele-machine logboeken streamen naar Event Hubs|[https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics-streaming-event-hubs](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics-streaming-event-hubs)|
|

### <a name="domain-name-services-dns-servers"></a>DNS-servers (Domain Name Services)

DNS-server logboeken bevatten belang rijke informatie met betrekking tot de services die door systemen worden getracht, hetzij intern hetzij extern. Het vastleggen van DNS-logboeken kan helpen bij het identificeren van een Cyber Security-incident en inzicht te krijgen in het type incident en de systemen die kunnen worden beïnvloed. De micro soft Management Agent (MMA) kan worden gebruikt op DNS-servers om de logboeken door te sturen naar Log Analytics voor gebruik in DNS-analyse (preview).

|Resources|Koppelen|
|---|---|
|Azure-naam omzetting voor virtuele netwerken|[https://docs.microsoft.com/azure/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances](https://docs.microsoft.com/azure/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances)|
|

### <a name="syslog-and-log-collection-servers"></a>Syslog-en logboek verzamelings servers

Voor het ontvangen van logboeken van virtuele netwerk apparaten of aangepaste beveiligings logboeken van andere systemen die in een SIEM worden gebruikt, kunnen speciale servers worden geïmplementeerd in azure VNets. Syslog-logboeken kunnen worden verzameld op een syslog-server en worden door gegeven aan Log Analytics voor analyse. Een logboek verzamelings server is een algemene term voor eventuele aggregatie van Logboeken en distributie mogelijkheden die worden gebruikt door gecentraliseerde bewakings systemen of Siem's. Deze kunnen worden gebruikt om de netwerk architectuur en beveiliging te vereenvoudigen en om logboeken te filteren en samen te voegen voordat ze worden gedistribueerd naar de gecentraliseerde mogelijkheid.

|Resources|Koppelen|
|---|---|
|Syslog-gegevensbronnen in Log Analytics|[https://docs.microsoft.com/azure/azure-monitor/platform/data-sources-syslog](https://docs.microsoft.com/azure/azure-monitor/platform/data-sources-syslog)|
|Logboek verzamelings server|Raadpleeg de documentatie van de leverancier voor meer informatie over de architectuur bewaking en SIEM|
|

### <a name="network-security-groups-nsgs"></a>Netwerk beveiligings groepen (Nsg's)

Nsg's beheert het verkeer naar en van virtuele netwerken in Azure. Nsg's regels Toep assen voor de verkeers stromen die zijn toegestaan of geweigerd, waaronder verkeer binnen Azure en tussen Azure en externe netwerken, zoals on-premises of het internet. Nsg's worden toegepast op subnetten binnen een virtueel netwerk of op afzonderlijke netwerk interfaces. NSG-logboeken kunnen worden ingeschakeld met de functie Network Watcher NSG-stroom Logboeken om informatie vast te leggen over het verkeer dat systemen in azure binnenkomt en verlaat. Deze logboeken worden gebruikt voor het maken van een basis lijn voor de standaard werking van een systeem en zijn de gegevens bron voor Traffic Analytics, die gedetailleerde inzichten biedt in de verkeers patronen van systemen die worden gehost in Azure.

|Resources|Koppelen|
|---|---|
|Documentatie voor de netwerk beveiligings groep|[https://docs.microsoft.com/azure/virtual-network/security-overview](https://docs.microsoft.com/azure/virtual-network/security-overview)|
|Inleiding tot stroom logboek registratie voor netwerk beveiligings groepen|[https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview)|
|Zelfstudie: Netwerk verkeer van en naar een virtuele machine vastleggen met behulp van de Azure Portal|[https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal)|
|

### <a name="azure-activity-log"></a>Azure-activiteitenlogboek

Azure-activiteiten logboek, een deel van Azure Monitor, is een abonnements logboek dat inzicht biedt in gebeurtenissen op abonnements niveau die in azure hebben plaatsgevonden. Het activiteiten logboek kan helpen bij het bepalen van de ' wat, wie en wanneer ' voor schrijf bewerkingen (PUT, POST, DELETE) die zijn uitgevoerd ***op*** de resources in een abonnement. Het activiteiten logboek is van cruciaal belang voor het bijhouden van de configuratie wijzigingen die in de Azure-omgeving zijn aangebracht. Azure-activiteiten logboeken zijn automatisch beschikbaar voor gebruik in Log Analytics oplossingen en kunnen worden verzonden naar Event Hubs of Azure Storage voor verwerking of retentie.

|Resources|Koppelen|
|---|---|
|Documentatie voor Azure-activiteiten logboek|[https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)|
|Het Azure-activiteiten logboek streamen naar Event Hubs|[https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-stream-activity-logs-event-hubs](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-stream-activity-logs-event-hubs)|
|

### <a name="azure-diagnostic-log"></a>Diagnostisch logboek van Azure

Azure Monitor Diagnostische logboeken worden logboeken gegenereerd door een Azure-service die voorziet in uitgebreide, frequente gegevens over de werking van die service. Diagnostische logboeken bieden inzicht in de werking van een resource op een gedetailleerd niveau en kunnen worden gebruikt voor een groot aantal vereisten, zoals het controleren of oplossen van problemen. Diagnostische logboeken van Azure zijn automatisch beschikbaar voor gebruik in Log Analytics oplossingen en kunnen worden verzonden naar Event Hubs of Azure Storage om te worden verwerkt of bewaard.

|Resources|Koppelen|
|---|---|
|Documentatie voor Diagnostische logboeken van Azure|[https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)|
|Ondersteunings services voor Diagnostische logboeken|[https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-diagnostic-logs-schema](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-diagnostic-logs-schema)|
|

### <a name="azure-policy"></a>Azure-beleid

Azure Policy dwingt regels af hoe resources kunnen worden geïmplementeerd, zoals het type, de locatie en de configuratie. Azure Policy kan worden geconfigureerd om ervoor te zorgen dat resources alleen kunnen worden geïmplementeerd als ze voldoen aan de vereisten. Azure Policy is een kern onderdeel voor het onderhouden van de integriteit van een Azure-omgeving. Gebeurtenissen met betrekking tot Azure Policy worden geregistreerd in het Azure-activiteiten logboek en zijn automatisch beschikbaar voor gebruik in Log Analytics oplossingen of kunnen worden verzonden naar Event Hubs of Azure Storage voor verwerking of retentie.

|Resources|Koppelen|
|---|---|
|Documentatie voor Azure Policy|[https://docs.microsoft.com/azure/governance/policy](https://docs.microsoft.com/azure/governance/policy)|
|Azure Policy-en Resource Manager-sjablonen gebruiken met behulp van Azure-blauw drukken|[https://docs.microsoft.com/azure/governance/blueprints/overview](https://docs.microsoft.com/azure/governance/blueprints/overview)|
|

## <a name="log-collection"></a>Logboek verzameling

Nadat de logboeken zijn gegenereerd vanuit de verschillende logboeken, moeten logbestanden worden opgeslagen op een gecentraliseerde locatie voor voortdurende toegang en analyse. Azure biedt meerdere methoden en opties voor logboek verzameling die kan worden benut, afhankelijk van het type en de vereisten van het logboek.

### <a name="event-hubs"></a>Event Hubs

Het doel van een event hub is het samen voegen van de logboek gegevens voor de verschillende bronnen voor distributie. Vanuit de Event hub kunnen de logboek gegevens worden verzonden naar een SIEM, naar de ACSC voor naleving en voor opslag voor lange termijn retentie.

|Resources|Koppelen|
|---|---|
|Documentatie over Event Hubs|[https://docs.microsoft.com/azure/event-hubs](https://docs.microsoft.com/azure/event-hubs)|
|Richt lijnen voor Event Hubs en externe Hulpprogram Ma's|[https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs)|
|

### <a name="log-analytics"></a>Log Analytics

Log Analytics maakt deel uit van Azure Monitor en wordt gebruikt voor logboek analyse. Log Analytics gebruikt een werk ruimte als het opslag mechanisme waar logboek gegevens beschikbaar kunnen worden gemaakt voor diverse analyse hulpprogramma's en oplossingen die beschikbaar zijn in Azure. Log Analytics integreert rechtstreeks met een breed scala aan Azure-onderdelen, evenals Virtual Machines via de micro soft monitoring agent.

|Resources|Koppelen|
|---|---|
|Documentatie voor Log Analytics|[https://docs.microsoft.com/azure/azure-monitor](https://docs.microsoft.com/azure/azure-monitor)|
|Zelfstudie: Gegevens in Log Analytics analyseren|[https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-viewdata](https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-viewdata)|
|

### <a name="network-watcher"></a>Network Watcher

Het gebruik van Network Watcher wordt aanbevolen door de ACSC om het netwerk verkeer in een Azure-abonnement te onderkennen en vast te leggen. NSG-stroom logboeken bieden de invoer voor de Traffic Analytics oplossing in Log Analytics, waardoor de zicht baarheid, analyse en rapportage systeem eigen worden verbeterd via Azure. Network Watcher biedt ook rechtstreeks vanuit de Azure Portal een functie voor het vastleggen van pakketten, zonder dat u zich hoeft aan te melden bij de virtuele machine. Met pakket opname kunt u pakket opname sessies maken om het verkeer van en naar een virtuele machine bij te houden.

|Resources|Koppelen|
|---|---|
|Network Watcher|[https://docs.microsoft.com/azure/network-watcher](https://docs.microsoft.com/azure/network-watcher)|
|Overzicht van pakket opname|[https://docs.microsoft.com/azure/network-watcher/network-watcher-packet-capture-overview](https://docs.microsoft.com/azure/network-watcher/network-watcher-packet-capture-overview)|
|

## <a name="log-retention"></a>Bewaar periode logboek

Voor de Australische overheids organisaties moeten de logboeken die zijn vastgelegd in azure, worden bewaard in overeenstemming met de nationale archieven van de AFDA van de Australische [beheer functies](http://www.naa.gov.au/information-management/records-authorities/types-of-records-authorities/AFDA/index.aspx), waarmee logboeken tot wel zeven jaar worden bewaard.

|Logboek locatie|Bewaarperiode|
|---|---|
|Azure-activiteitenlogboek|Maxi maal 90 dagen|
|Log Analytics-werkruimte|Maxi maal twee jaar|
|Event Hub|Maxi maal zeven dagen|
|

Het is uw verantwoordelijkheid om ervoor te zorgen dat Logboeken correct worden gearchiveerd om te voldoen aan AFDA en andere wettelijke vereisten.

### <a name="azure-storage"></a>Azure Storage

Azure Storage is de opslag plaats voor logboeken voor lange termijn retentie in Azure. Azure Storage kunnen worden gebruikt voor het archiveren van logboeken van Azure, met inbegrip van Event Hubs, Azure-activiteiten logboek en Azure Diagnostics-Logboeken. De periode voor het bewaren van gegevens in de opslag kan worden ingesteld op nul of kan worden opgegeven als een aantal dagen. Een Bewaar periode van nul dagen betekent dat Logboeken voor altijd worden bewaard, anders kan de waarde een wille keurig aantal dagen tussen 1 en 2147483647.

|Resources|Koppelen|
|---|---|
|Documentatie bij Azure Storage|[https://docs.microsoft.com/azure/storage](https://docs.microsoft.com/azure/storage)|
|Vastleggen van gebeurtenissen tot en met Azure Event Hubs in Azure Blob Storage of Azure Data Lake Storage|[https://docs.microsoft.com/azure/event-hubs/event-hubs-capture-overview](https://docs.microsoft.com/azure/event-hubs/event-hubs-capture-overview)|
|Zelfstudie: Metrische gegevens en logboekgegevens van Azure archiveren met behulp van Azure Storage|[https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-tutorial-archive-monitoring-data](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-tutorial-archive-monitoring-data)|
|Replicatie Azure Storage|[https://docs.microsoft.com/azure/storage/common/storage-redundancy](https://docs.microsoft.com/azure/storage/common/storage-redundancy)|
|Een moment opname van een BLOB maken|[https://docs.microsoft.com/rest/api/storageservices/creating-a-snapshot-of-a-blob](https://docs.microsoft.com/rest/api/storageservices/creating-a-snapshot-of-a-blob)|
|

## <a name="log-analysis"></a>Analyse vastleggen

Zodra de logboeken zijn gegenereerd en opgeslagen op een gecentraliseerde locatie, moeten ze worden geanalyseerd om te helpen bij het detecteren van pogingen of geslaagde beveiligings incidenten. Wanneer er beveiligings incidenten worden gedetecteerd, moet een agentschap reageren op deze incidenten en eventuele bedreigingen bijhouden, bevatten en oplossen.

### <a name="azure-security-center-asc"></a>Azure Security Center (ASC)

Azure Security Center biedt uniforme beveiligings beheer en geavanceerde bedreigings beveiliging. Azure Security Center kunt beveiligings beleid Toep assen op werk belastingen, bloot stelling aan bedreigingen beperken en aanvallen detecteren en erop reageren. Azure Security Center biedt Dash boards en analyses over een breed scala aan Azure-onderdelen. Het gebruik van Azure Security Center is opgegeven als een vereiste in de richt lijnen voor ACSC-gebruikers.

|Resources|Koppelen|
|---|---|
|Documentatie voor Azure Security Center|[https://docs.microsoft.com/azure/security-center](https://docs.microsoft.com/azure/security-center)|
|Quickstart: Uw Azure-abonnement onboarden voor Security Center Standard|[https://docs.microsoft.com/azure/security-center/security-center-get-started](https://docs.microsoft.com/azure/security-center/security-center-get-started)|
|

### <a name="traffic-analytics"></a>Verkeersanalyse

Traffic Analytics is een Cloud oplossing die inzicht geeft in de activiteit van gebruikers en toepassingen in Azure. De analyse van Traffic Analytics Network Watcher NSG-stroom Logboeken om inzicht te krijgen in de verkeers stroom in Azure. Traffic Analytics wordt gebruikt om Dash boards, rapporten, analyses en reacties op gebeurtenissen te bieden die betrekking hebben op het netwerk verkeer dat wordt weer gegeven in virtuele netwerken. Traffic Analytics biedt aanzienlijk inzicht en helpt bij het identificeren en oplossen van Cyber-beveiligings incidenten.

|Resources|Koppelen|
|---|---|
|Documentatie over Traffic Analytics|[https://docs.microsoft.com/azure/network-watcher/traffic-analytics](https://docs.microsoft.com/azure/network-watcher/traffic-analytics)|
|

### <a name="azure-advisor"></a>Azure Advisor

Azure Advisor analyse van resource configuratie en andere gegevens voor het aanbevelen van oplossingen voor het verbeteren van de prestaties, beveiliging en hoge Beschik baarheid van resources terwijl u op zoek bent naar mogelijkheden om de totale Azure-uitgaven te verminderen. Azure Advisor wordt aanbevolen door de ACSC en biedt gemakkelijk toegankelijke en gedetailleerde adviezen over de configuratie van de Azure-omgeving.

|Resources|Koppelen|
|---|---|
|Documentatie voor Azure Advisor|[https://docs.microsoft.com/azure/advisor](https://docs.microsoft.com/azure/advisor)|
|Aan de slag met Azure Advisor|[https://docs.microsoft.com/azure/advisor/advisor-get-started](https://docs.microsoft.com/azure/advisor/advisor-get-started)|
|

### <a name="dns-analytics-preview"></a>DNS Analytics (preview-versie)

DNS-analyse is een Log Analytics oplossing voor het verzamelen, analyseren en correleren van Windows DNS analytic-en audit logboeken en andere gerelateerde gegevens. DNS-analyse identificeert clients die proberen om kwaad aardige domein namen, verlopen bron records, vaak opgevraagde domein namen en DNS-clients op te lossen. DNS-analyse biedt ook inzicht in het laden van aanvragen voor DNS-servers en dynamische DNS-registratie fouten. DNS-analyse wordt gebruikt om Dash boards, rapporten, analyses en reacties op gebeurtenissen te bieden die betrekking hebben op de DNS-query's die zijn gemaakt in een Azure-omgeving. DNS-analyse biedt aanzienlijk inzicht en helpt bij het identificeren en oplossen van Cyber-beveiligings incidenten.

|Resources|Koppelen|
|---|---|
|Documentatie voor DNS Analytics|[https://docs.microsoft.com/azure/azure-monitor/insights/dns-analytics](https://docs.microsoft.com/azure/azure-monitor/insights/dns-analytics)|
|

### <a name="activity-log-analytics"></a>Activity Log Analytics

Analyse van activiteitenlogboek is een Log Analytics oplossing waarmee u het Azure-activiteiten logboek kunt analyseren en doorzoeken op meerdere Azure-abonnementen. Analyse van activiteitenlogboek wordt gebruikt om gecentraliseerde Dash boards, rapporten, analyses en reacties op gebeurtenissen te bieden die betrekking hebben op de acties die worden uitgevoerd voor de hele Azure-omgeving. Analyse van activiteitenlogboek kunnen helpen bij het controleren en onderzoeken.

|Resources|Koppelen|
|---|---|
|Verzamelen en analyseren van Azure-activiteitenlogboeken in Log Analytics|[https://docs.microsoft.com/azure/azure-monitor/platform/collect-activity-logs](https://docs.microsoft.com/azure/azure-monitor/platform/collect-activity-logs)|
|

### <a name="security-information-and-event-management-siem"></a>Security Information and Event Management (SIEM)

Een SIEM is een systeem dat gecentraliseerde opslag, controle en analyse van beveiligings Logboeken biedt, met gedefinieerde mechanismen voor het opnemen van een breed scala aan logboek gegevens en intelligente hulpprogram ma's voor analyse, rapportage en incident detectie en-respons. U kunt SIEM-mogelijkheden gebruiken die informatie over Azure-logboek registratie bevatten om de beveiligings mogelijkheden van Azure in te vullen. Gemenebests entiteiten kunnen een SIEM die wordt gehost op Virtual Machines in azure, on-premises of als SaaS-functionaliteit (Software as a Service), afhankelijk van specifieke vereisten.

|Resources|Koppelen|
|---|---|
|Azure-Sentinel (preview-versie)|[https://azure.microsoft.com/services/azure-sentinel](https://azure.microsoft.com/services/azure-sentinel)|
|Documentatie voor SIEM|Raadpleeg de documentatie van de leverancier voor SIEM-architectuur en-richt lijnen|
|Azure Monitor gebruiken om te integreren met SIEM-hulpprogram ma's|[https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools)|
|

### <a name="australian-cyber-security-centre"></a>Australische Cyber Security Center

Het Australische Cyber Security Center (ACSC) is de potentiële klant van de Australische overheid op National Cyber-Security. Het biedt de mogelijkheid om op basis van de Australische overheid Cyber-beveiligings mogelijkheden te bieden om de Cyber-tolerantie van de Australische community te verbeteren en de economische en sociale welvaart van Australië in het digitale tijd perk te ondersteunen. De ACSC raadt aan dat Gemenebests entiteiten alle door het systeem gegenereerde logboek bestanden, gebeurtenissen en logboeken door sturen naar de ACSC voor een volledige controle van de Australische overheid.

|Resources|Koppelen|
|---|---|
|Website van het Australische Cyber Security Center|[https://www.acsc.gov.au](https://www.acsc.gov.au)|
|

## <a name="incident-response"></a>Reageren op incidenten

Het genereren van de juiste logboeken, het verzamelen van deze in gecentraliseerde opslag plaatsen en het uitvoeren van analyses verhogen inzicht in systemen en biedt mechanismen voor het detecteren van Cyber-beveiligings incidenten. Nadat er incidenten of gebeurtenissen zijn gedetecteerd, is de volgende stap om te reageren op deze gebeurtenissen en acties uit te voeren om de systeem status bij te houden en services en gegevens te beschermen tegen inbreuk. Azure biedt een combi natie van services die effectief reageren op gebeurtenissen die zich voordoen.

### <a name="azure-alerts"></a>Azure-waarschuwingen

Azure-waarschuwingen kunnen worden gebruikt om ondersteuning en beveiligings personeel op de hoogte te stellen als reactie op bepaalde gebeurtenissen. Hierdoor kan een Gemenebest van de organisatie de detectie van relevante gebeurtenissen die worden veroorzaakt door de Analysis Services die in dit artikel worden genoemd, proactief kunnen reageren.

|Resources|Koppelen|
|---|---|
|Overzicht van waarschuwingen in Microsoft Azure|[https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-alerts](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-alerts)|
|Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center|[https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts)|
|Reageren op gebeurtenissen met Azure Monitor-waarschuwingen|[https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-response](https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-response)|
|

### <a name="azure-automation"></a>Azure Automation

Azure Automation kunnen Gemenebests organisaties acties activeren als reactie op gebeurtenissen. Dit kan zijn om een pakket opname te starten op Virtual Machines, een werk stroom uit te voeren, Virtual Machines of services te stoppen of te starten, of een reeks andere taken. Automatisering maakt snelle reacties van waarschuwingen mogelijk zonder hand matige interventie, waardoor de reactie tijd en ernst van een incident of gebeurtenis worden verminderd.

|Resources|Koppelen|
|---|---|
|Documentatie over Azure Automation|[https://docs.microsoft.com/azure/automation](https://docs.microsoft.com/azure/automation)|
|Instructies: Een waarschuwing gebruiken om een Azure Automation runbook te activeren|[https://docs.microsoft.com/azure/automation/automation-create-alert-triggered-runbook](https://docs.microsoft.com/azure/automation/automation-create-alert-triggered-runbook)|
|

## <a name="next-steps"></a>Volgende stappen

Raadpleeg het artikel over het [beheer van beveiligde externe gateways](gateway-secure-remote-administration.md) voor meer informatie over het veilig beheren van uw gateway omgeving in Azure.
