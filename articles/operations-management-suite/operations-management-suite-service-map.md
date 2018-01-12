---
title: De oplossing Serviceoverzicht gebruiken in Operations Management Suite | Microsoft Docs
description: Serviceoverzicht is een Operations Management Suite-oplossing die automatisch de onderdelen van toepassing op Windows- en Linux-systemen worden gedetecteerd en de communicatie tussen services wordt toegewezen. Dit artikel bevat informatie voor het Serviceoverzicht implementeren in uw omgeving en gebruiken in verschillende scenario's.
services: operations-management-suite
documentationcenter: 
author: daveirwin1
manager: jwhit
editor: tysonn
ms.assetid: 3ceb84cc-32d7-4a7a-a916-8858ef70c0bd
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/22/2016
ms.author: daseidma;bwren;dairwin
ms.openlocfilehash: 993dff7657a73803ca21677e19b08946fb89bfa2
ms.sourcegitcommit: c4cc4d76932b059f8c2657081577412e8f405478
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2018
---
# <a name="use-the-service-map-solution-in-operations-management-suite"></a>De oplossing Serviceoverzicht gebruiken in Operations Management Suite
Serviceoverzicht ontdekt automatisch toepassingsonderdelen op Windows- en Linux-systemen en wijst de communicatie tussen services toe. Met het Serviceoverzicht, kunt u uw servers weergeven in de manier waarop u denkt ze dat: als onderling verbonden systemen die essentiële services leveren. Serviceoverzicht ziet u de verbindingen tussen servers, processen en poorten voor elke architectuur TCP-verbinding waarvoor geen configuratie nodig andere dan de installatie van een agent.

Dit artikel worden de details van het gebruik van Serviceoverzicht. Zie voor meer informatie over het configureren van Serviceoverzicht en voorbereiding agents [oplossing Serviceoverzicht configureren in de Operations Management Suite](operations-management-suite-service-map-configure.md).


## <a name="use-cases-make-your-it-processes-dependency-aware"></a>Gebruiksvoorbeelden: maken van uw IT-processen op de hoogte van afhankelijkheid

### <a name="discovery"></a>Detectie
Een algemene referentie-kaart van afhankelijkheden maakt Serviceoverzicht automatisch in uw servers, processen en services van derden. Het detecteert en alle TCP-afhankelijkheden, identificeren verrassing verbindingen, externe van derden systemen die u afhankelijk en afhankelijkheden van traditionele donkere gebieden van uw netwerk, zoals Active Directory wordt toegewezen. Serviceoverzicht detecteert mislukte netwerkverbindingen die uw beheerde systemen probeert te maken, zodat u kunt identificeren mogelijke onjuiste configuratie van server, onderbreking van deze service en netwerkproblemen.

### <a name="incident-management"></a>Incidentbeheer
Serviceoverzicht helpt groot deel van het probleem isolatie elimineren door die laat zien hoe systemen zijn verbonden en elkaar beïnvloeden. Naast het identificeren van mislukte verbindingen, helpt het identificeren van onjuist geconfigureerde load balancers, verrassend of overmatige belasting van essentiële services en clients, zoals ontwikkelaars machines praten met productiesystemen rogue. Met behulp van geïntegreerde werkstromen met Operations Management Suite wijzigen bijhouden, kunt u ook zien of de wijzigingsgebeurtenis voor een op een back-end-machine of service de hoofdoorzaak van een incident wordt uitgelegd.

### <a name="migration-assurance"></a>Migratie zekerheid
Met behulp van Serviceoverzicht, kunt u effectief plannen, versnellen en Azure migraties valideren die zorgt ervoor dat er niets is achtergelaten en geen onverwachte storingen optreden. U kunt alle onderling afhankelijk systemen die samen worden gemigreerd, beoordelen systeemconfiguratie en de capaciteit en bepalen of een actieve gebruikers nog steeds bedient of geschikt is voor het buiten gebruik stellen in plaats van de migratie moeten detecteren. Nadat de verplaatsing voltooid is, kunt u controleren of u over clientbelasting en identiteit om te controleren of de testsystemen en klanten verbinden. Als uw subnetdefinities plannen en firewall problemen hebt, wijst mislukte verbindingen Serviceoverzicht Maps u de systemen die moeten zijn verbonden.

### <a name="business-continuity"></a>Bedrijfscontinuïteit
Als u van Azure Site Recovery gebruikmaakt en moet de herstel-reeks voor uw toepassingsomgeving, serviceoverzicht definiëren help automatisch wordt weergegeven hoe systemen zijn afhankelijk van elkaar om ervoor te zorgen dat uw herstelplan betrouwbaar. Door een kritieke server of een groep te kiezen en de clients weergeven, kunt u bepalen welke front-end systemen herstellen nadat de server teruggezet en beschikbaar is. Als u daarentegen door te kijken essentiële servers back-end afhankelijkheden, kunt u welke systemen te herstellen voordat u uw systemen focus zijn hersteld identificeren.

### <a name="patch-management"></a>Patch management
Uw gebruik van de Operations Management Suite System Update-evaluatie verbetert Serviceoverzicht door te tonen die andere teams en -servers hangen af van uw service, zodat u deze vooraf waarschuwen kunt voordat u uw systeem voor patch-doeleinden. Patchbeheer van de in Operations Management Suite verbetert Serviceoverzicht ook door u of uw services beschikbaar en goed verbonden nadat zijn ze worden hersteld en opnieuw opgestart.


## <a name="mapping-overview"></a>Overzicht van de toewijzing
Serviceoverzicht agents verzamelen van informatie over alle processen TCP-verbinding op de server waar ze worden geïnstalleerd en meer informatie over de binnenkomende en uitgaande verbindingen voor elk proces. U kunt computers of groepen met Serviceoverzicht agents voor het visualiseren van afhankelijkheden zijn gedurende een opgegeven tijdperiode selecteren in de lijst in het linkerdeelvenster. Afhankelijkheid van de machine wordt toegewezen focus op een specifieke computer en ze de machines die direct TCP-clients of servers van de desbetreffende computer zijn weergeven.  Groepstoewijzingen machine weergeven sets van servers en de bijbehorende afhankelijkheden.

![Overzicht van het Serviceoverzicht](media/oms-service-map/service-map-overview.png)

Machines kunnen worden uitgebreid in de kaart om weer te geven van de actieve groepen en -processen met actieve netwerkverbindingen verwerken tijdens de geselecteerde periode. Wanneer een externe computer met een Serviceoverzicht-agent wordt uitgevouwen Procesdetails wilt weergeven, worden alleen die processen die met de machine focus communiceren weergegeven. Het aantal zonder agent front-machines die verbinding met de focus machine wordt aan de linkerkant van de processen die ze verbinding met maken aangegeven. Als de focus-machine met het maken van een verbinding met een back-end-machine waarop geen agent is, wordt de back-endserver opgenomen in een servergroep poort, samen met andere verbindingen met hetzelfde poortnummer.

Standaard weergeven Serviceoverzicht maps de laatste 30 minuten van afhankelijkheidsinformatie. Met behulp van de besturingselementen voor tijd in de linkerbovenhoek, kunt u een query-toewijzingen voor historische bereiken van maximaal één uur om weer te geven hoe afhankelijkheden in het verleden hebt bekeken (bijvoorbeeld, tijdens een incident, of voordat er een wijziging is opgetreden). Serviceoverzicht gegevens worden opgeslagen voor 30 dagen in betaalde werkruimten en 7 dagen in gratis werkruimten.

## <a name="status-badges-and-border-coloring"></a>Status badges en de kleur van rand
Aan de onderkant van elke server in de kaart kan worden een lijst met status badges overbrengen statusinformatie over de server. De badges erop duiden dat er een relevante informatie voor de server uit een van de integratie van Operations Management Suite-oplossing. Een badge te klikken gaat u rechtstreeks naar de details van de status in het rechterdeelvenster. De status van de momenteel beschikbare badges bevatten waarschuwingen, servicedesk, beveiliging, wijzigingen en Updates.

Afhankelijk van de ernst van de status badges machine knooppunt randen worden gekleurde rood (Kritiek), geel (waarschuwing) of blauw (informatief). De kleur vertegenwoordigt de ernstigste status van elk van de status badges. Een grijze rand geeft aan dat een knooppunt dat geen statusindicatoren is.

![Status badges](media/oms-service-map/status-badges.png)

## <a name="process-groups"></a>Verwerken van groepen
Procesgroepen combineren processen die gekoppeld aan een algemene product of service in een groep processen zijn.  Zelfstandige processen samen met het verwerken van groepen wordt weergegeven wanneer een machine-knooppunt wordt uitgevouwen.  Als u alle binnenkomende en uitgaande verbindingen met een proces in een procesgroep vervolgens de verbinding is mislukt, wordt weergegeven als is mislukt voor de hele groep.

## <a name="machine-groups"></a>Computergroepen
Computergroepen kunnen u maps gecentreerd rond een reeks servers, niet alleen een zodat u alle leden van een cluster met meerdere lagen toepassings- of in een kaart kunt zien.

Gebruikers selecteren welke servers deel uitmaken van een groep samen en kies een naam voor de groep.  U kunt vervolgens de groep met alle processen en verbindingen te bekijken of met alleen de processen en verbindingen die rechtstreeks betrekking op de andere leden van de groep hebben bekijken.

![Computergroep](media/oms-service-map/machine-group.png)

### <a name="creating-a-machine-group"></a>Maken van een groep machines
Voor het maken van een groep, selecteert u de machine of de machines die u wilt dat op de Machines lijst en klikt u op **toevoegen aan groep**.

![Groep maken](media/oms-service-map/machine-groups-create.png)

Daar kunt u **nieuw** en de groep een naam geven.

![De naam van groep](media/oms-service-map/machine-groups-name.png)

>[!NOTE]
>Computergroepen zijn momenteel beperkt tot 10 servers, maar we willen snel deze limiet te verhogen.

### <a name="viewing-a-group"></a>Een groep weergeven
Als u een aantal groepen hebt gemaakt, kunt u ze kunt weergeven door het tabblad groepen te kiezen.

![Tabblad groepen](media/oms-service-map/machine-groups-tab.png)

Selecteer vervolgens de naam van de groep om de toewijzing voor de Machine groep weer te geven.
![Machinegroep](media/oms-service-map/machine-group.png) de machines die deel uitmaken van de groep worden beschreven in wit in de kaart.

De machines die gezamenlijk de Machinegroep wordt een lijst uitbreiden van de groep.

![Machine groep machines](media/oms-service-map/machine-groups-machines.png)

### <a name="filter-by-processes"></a>Filteren op processen
U kunt de overzichtsweergave tussen alle processen en verbindingen met in de groep en alleen de waarden die rechtstreeks betrekking op de Machine groep hebben schakelen.  De standaardweergave is voor het weergeven van alle processen.  U kunt de weergave wijzigen door te klikken op het filterpictogram boven de kaart.

![Filter-groep](media/oms-service-map/machine-groups-filter.png)

Wanneer **alle processen** is geselecteerd, de kaart bevat alle processen en verbindingen op elk van de machines in de groep.

![Alle Machinegroep verwerkt](media/oms-service-map/machine-groups-all.png)

Als u de weer wijzigt te geven alleen **processen groep verbonden**, wordt de kaart worden teruggebracht tot alleen de processen en verbindingen die rechtstreeks zijn verbonden met andere computers in de groep een vereenvoudigde weergave maken.

![Machinegroep gefilterd processen](media/oms-service-map/machine-groups-filtered.png)
 
### <a name="adding-machines-to-a-group"></a>Computers toevoegen aan een groep
Als u wilt machines toevoegt aan een bestaande groep, schakel de selectievakjes naast de machines die u wilt en klik vervolgens op **toevoegen aan groep**.  Kies vervolgens de groep die u wilt de machines toevoegen.
 
### <a name="removing-machines-from-a-group"></a>Machines verwijderen uit een groep
Vouw de naam van de groep voor een lijst met computers in de groep Machine in de lijst met groepen.  Klik vervolgens op het menu van de drie puntjes naast de computer die u wilt verwijderen en kies **verwijderen**.

![Machine uit groep verwijderen](media/oms-service-map/machine-groups-remove.png)

### <a name="removing-or-renaming-a-group"></a>Verwijderen of hernoemen van een groep
Klik op het menu van de drie puntjes naast de groepnaam van de in de lijst met groepen.

![Menu voor groep machines](media/oms-service-map/machine-groups-menu.png)


## <a name="role-icons"></a>Rolpictogrammen
Bepaalde processen dienen bepaalde functies op computers: web-servers, toepassingsservers en -database. Serviceoverzicht annotates verwerken en machines vakken met Rolpictogrammen om vast te stellen in een oogopslag de rol van een proces of de server wordt afgespeeld.

| Functiepictogram | Beschrijving |
|:--|:--|
| ![Webserver](media/oms-service-map/role-web-server.png) | Webserver |
| ![Appserver](media/oms-service-map/role-application-server.png) | Toepassingsserver |
| ![Database-server](media/oms-service-map/role-database.png) | Database-server |
| ![LDAP-server](media/oms-service-map/role-ldap.png) | LDAP-server |
| ![SMB-server](media/oms-service-map/role-smb.png) | SMB-server |

![Rolpictogrammen](media/oms-service-map/role-icons.png)


## <a name="failed-connections"></a>Mislukte verbindingen
Mislukte verbindingen worden weergegeven in het Serviceoverzicht maps processen en computers, met een rode stippellijn die aangeeft dat een clientsysteem kan niet worden bereikt van een proces of poort. Mislukte verbindingen worden gerapporteerd door een systeem met een geïmplementeerde Serviceoverzicht agent als dat systeem is een poging de mislukte verbinding. Serviceoverzicht meet dit proces met inachtneming van TCP-sockets die niet voldoen aan een verbinding tot stand brengen. Deze fout kan worden veroorzaakt door een firewall, een onjuiste configuratie van de client of server of een externe service is tijdelijk niet beschikbaar zijn.

![Mislukte verbindingen](media/oms-service-map/failed-connections.png)

Informatie over mislukte verbindingen kunnen helpen bij het oplossen van problemen validatie van de migratie, beveiligingsanalyse en algemene architectuur understanding. Mislukte verbindingen zijn soms onschadelijk, maar ze vaak wijst u rechtstreeks een probleem voordoet, zoals een failover-omgeving plotseling is onbereikbaar of twee toepassingslagen niet kan communiceren na de cloudmigratie van een.

## <a name="client-groups"></a>Clientgroepen
Clientgroepen zijn vakken op de kaart die clientcomputers waarvoor geen afhankelijkheid Agents vertegenwoordigen. Een enkele Client groep vertegenwoordigt de clients voor een afzonderlijk proces of de machine.

![Clientgroepen](media/oms-service-map/client-groups.png)

Overzicht van de IP-adressen van de servers in een clientgroep door de groep te selecteren. De inhoud van de groep wordt weergegeven in de **Client groepseigenschappen** deelvenster.

![Eigenschappen van client](media/oms-service-map/client-group-properties.png)

## <a name="server-port-groups"></a>Serverpoort groepen
Serverpoort groepen zijn vakken waarin de server-poorten op servers waarop geen afhankelijkheid Agents. Het vak bevat de server-poort en een telling van het aantal servers met verbindingen op deze poort. Vouw in het vak om de afzonderlijke servers en verbindingen te bekijken. Als er slechts één server in het vak, wordt de naam of IP-adres vermeld.

![Serverpoort groepen](media/oms-service-map/server-port-groups.png)

## <a name="context-menu"></a>Contextmenu
Te klikken op het weglatingsteken (...) boven rechts van elke server worden weergegeven voor het snelmenu voor die server.

![Mislukte verbindingen](media/oms-service-map/context-menu.png)

### <a name="load-server-map"></a>Load server-kaart
Te klikken op **Load Server kaart** gaat u naar een nieuw overzicht met de geselecteerde server als de nieuwe focus-machine.

### <a name="show-self-links"></a>Weergeven Self link-elementen
Te klikken op **Self-Links weergeven** opnieuw getekend het serverknooppunt, inclusief alle Self link-elementen, die TCP-verbindingen die beginnen en eindigen op processen binnen de server. Als self link-elementen worden weergegeven, de wijzigingen van de opdracht menu in **Self-Links verbergen**, zodat u kunt deze uitschakelen.

## <a name="computer-summary"></a>Samenvatting van computer
De **Machine samenvatting** deelvenster bevat een overzicht van een server-besturingssysteem, afhankelijkheid tellingen en gegevens van andere Operations Management Suite-oplossingen. Dergelijke gegevens omvatten maatstaven voor prestaties, helpdesk servicetickets, bijhouden, beveiliging en updates.

![Samenvattingsvenster machine](media/oms-service-map/machine-summary.png)

## <a name="computer-and-process-properties"></a>Eigenschappen van de computer en het proces
Als u een kaart Serviceoverzicht navigeert, selecteert u machines en processen te krijgen van aanvullende context over de eigenschappen. Machines bieden informatie over DNS-naam, IPv4-adressen, CPU en geheugen capaciteit, VM-type, besturingssysteem en versie, laatste opnieuw opstarten tijd en de id's van de Operations Management Suite en Serviceoverzicht agents.

![Deelvenster machine-eigenschappen](media/oms-service-map/machine-properties.png)

U kunt procesgegevens verzamelen van metagegevens over het uitvoeren van processen, waaronder procesnaam, Procesbeschrijving, gebruikersnaam en domein (op Windows), bedrijfsnaam, productnaam, versie van het product, werkmap, opdrachtregel en proces besturingssysteem de begintijd.

![Deelvenster Eigenschappen verwerken](media/oms-service-map/process-properties.png)

De **overzicht** deelvenster biedt aanvullende informatie over het proces connectiviteit, met inbegrip van de afhankelijke poorten, binnenkomende en uitgaande verbindingen en verbindingen is mislukt.

![Deelvenster Overzicht van het proces](media/oms-service-map/process-summary.png)

## <a name="operations-management-suite-alerts-integration"></a>Integratie van operations Management Suite-waarschuwingen
Serviceoverzicht integreert met Operations Management Suite waarschuwingen gestarte u waarschuwingen wilt weergeven voor de geselecteerde server in de geselecteerde periode. De server wordt een pictogram weergegeven als er huidige waarschuwingen en de **Machine waarschuwingen** deelvenster geeft een lijst van de waarschuwingen.

![Deelvenster waarschuwingen van de computer](media/oms-service-map/machine-alerts.png)

Schakel Serviceoverzicht relevante waarschuwingen worden weergegeven door een waarschuwingsregel die wordt geactiveerd voor een specifieke computer te maken. Juiste om waarschuwingen te maken:
- Een component aan groep opnemen door computer (bijvoorbeeld **met Computer interval 1 minuut**).
- Kies een waarschuwing op basis van metrische meting.

![Configuratie van waarschuwingen](media/oms-service-map/alert-configuration.png)


## <a name="operations-management-suite-log-events-integration"></a>Integratie van operations Management Suite logboek gebeurtenissen
Serviceoverzicht integreert met logboek zoeken om een telling van alle beschikbare logboekgebeurtenissen voor de geselecteerde server tijdens de geselecteerde periode weer te geven. U kunt klikken op elke rij in de lijst van gebeurtenis tellingen springen naar logboek Search en de afzonderlijke gebeurtenissen bekijken.

![Machine logboekgebeurtenissen deelvenster](media/oms-service-map/log-events.png)

## <a name="operations-management-suite-service-desk-integration"></a>Integratie van operations Management Suite-servicedesk
Serviceoverzicht integratie met de IT-Service Management-Connector is automatische wanneer beide oplossingen zijn ingeschakeld en geconfigureerd in de Operations Management Suite-werkruimte. De integratie in Service-kaart wordt aangeduid als "Servicedesk." Zie voor meer informatie [centraal beheren van werkitems ITSM IT Service Management-Connector met](https://docs.microsoft.com/azure/log-analytics/log-analytics-itsmc-overview).

De **Machine servicedesk** deelvenster toont alle gebeurtenissen voor IT-servicebeheer voor de geselecteerde server in de geselecteerde periode. De server, wordt er een pictogram weergegeven als er huidige items en in het deelvenster Machine servicedesk ze worden.

![Machine servicedesk deelvenster](media/oms-service-map/service-desk.png)

Het item in de verbonden ITSM-oplossing, klikt u op **weergave werkitem**.

Klik op om de details van het item in logboek zoekopdracht **weergeven in het logboek zoeken**.


## <a name="operations-management-suite-change-tracking-integration"></a>Integratie van operations Management Suite-bijhouden
Serviceoverzicht integratie met wijzigingen bijhouden is automatische wanneer beide oplossingen zijn ingeschakeld en geconfigureerd in de Operations Management Suite-werkruimte.

De **Machine bijhouden** deelvenster geeft een lijst van alle wijzigingen, met de meest recente eerste, samen met een koppeling naar details weergeven van logboek-zoeken naar aanvullende informatie.

![Machine bijhouden deelvenster](media/oms-service-map/change-tracking.png)

De volgende afbeelding bevat een gedetailleerde weergave van een configuratiewijziging gebeurtenis die u ziet nadat u hebt geselecteerd **weergeven in logboekanalyse**.

![Configuratiewijziging gebeurtenis](media/oms-service-map/configuration-change-event.png)


## <a name="operations-management-suite-performance-integration"></a>Integratie van operations Management Suite-prestaties
De **Machine prestaties** paneel geeft standaard prestatiemetrieken voor de geselecteerde server weer. De metrische gegevens omvatten CPU-gebruik, geheugengebruik verzonden en ontvangen netwerkbytes en een lijst van de belangrijkste processen door het verzonden en ontvangen netwerkbytes.

![Machine prestaties deelvenster](media/oms-service-map/machine-performance.png)

Om prestatiegegevens te bekijken, moet u mogelijk [inschakelen van de juiste Log Analytics-prestatiemeteritems](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-performance-counters).  De items die u wilt inschakelen:

Windows:
- Processor(*)\\percentage processortijd
- Geheugen\\% toegewezen Bytes In gebruik
- Netwerk Adapter(*)\\verzonden Bytes per seconde
- Netwerk Adapter(*)\\ontvangen Bytes per seconde

Linux:
- Processor(*)\\percentage processortijd
- Memory(*)\\percentage gebruikt geheugen
- Netwerk Adapter(*)\\verzonden Bytes per seconde
- Netwerk Adapter(*)\\ontvangen Bytes per seconde

Als u de prestatiegegevens van het netwerk, moet u ook zijn ingeschakeld de kabel gegevens 2.0-oplossing in de Operations Management Suite.
 
## <a name="operations-management-suite-security-integration"></a>Integratie van operations Management Suite-beveiliging
Serviceoverzicht integratie met beveiligings- en Audit is automatische wanneer beide oplossingen zijn ingeschakeld en geconfigureerd in de Operations Management Suite-werkruimte.

De **Machine beveiliging** deelvenster geeft gegevens uit de beveiliging van Operations Management Suite en Audit-oplossing voor de geselecteerde server. Het deelvenster geeft een samenvatting weer van alle uitstaande beveiligingsproblemen voor de server tijdens de geselecteerde periode. Het selecteren van de problemen met beveiliging zoomt omlaag in een zoekopdracht logboek voor meer informatie over deze.

![Machine beveiliging deelvenster](media/oms-service-map/machine-security.png)


## <a name="operations-management-suite-updates-integration"></a>Integratie van operations Management Suite-Updates
Serviceoverzicht integratie met beheer van updates is automatisch wanneer beide oplossingen zijn ingeschakeld en geconfigureerd in de Operations Management Suite-werkruimte.

De **Machine Updates** deelvenster ziet u gegevens uit het beheersysteem van Operations Management Suite Update voor de geselecteerde server. Het deelvenster geeft een samenvatting weer van eventuele ontbrekende updates voor de server tijdens de geselecteerde periode.

![Machine bijhouden deelvenster](media/oms-service-map/machine-updates.png)

## <a name="log-analytics-records"></a>Log Analytics-records
Serviceoverzicht computer- en inventarisgegevens is beschikbaar voor [search](../log-analytics/log-analytics-log-searches.md) in logboekanalyse. U kunt deze gegevens in scenario's die migratieplanning capaciteitsanalyse, detectie en het oplossen van de prestaties op aanvraag zijn toepassen.

Een record wordt per uur voor elke unieke computernaam en het proces, naast de records die worden gegenereerd wanneer een proces of de computer wordt gestart of geïntegreerde Service met de hoofdversietoewijzing is gegenereerd. Deze records hebben de eigenschappen in de volgende tabellen. De velden en waarden in de ServiceMapComputer_CL-gebeurtenissen toewijzen aan velden van de bron van de Machine in de ServiceMap Azure Resource Manager-API. De velden en waarden in de gebeurtenissen ServiceMapProcess_CL worden toegewezen aan de velden van de resource proces in de ServiceMap Azure Resource Manager-API. Het veld ResourceName_s komt overeen met het naamveld in de overeenkomende Resource Manager-resource. 

>[!NOTE]
>Naarmate Serviceoverzicht functies groeien, zijn deze velden worden gewijzigd.

Er zijn intern gegenereerde eigenschappen die u gebruiken kunt om unieke processen en computers te identificeren:

- Computer: Gebruik ResourceId of ResourceName_s als unieke identificatie van een computer binnen een Operations Management Suite-werkruimte.
- Proces: Gebruik ResourceId om een proces in een werkruimte van Operations Management Suite uniek te identificeren. ResourceName_s is uniek binnen de context van de computer waarop het proces (MachineResourceName_s) wordt uitgevoerd 

Omdat er meerdere records kunnen zijn voor een bepaald proces en de computer in een opgegeven tijdperiode, kunnen query's meer dan een record voor dezelfde computer of hetzelfde proces worden geretourneerd. Voeg zodanig dat alleen de meest recente record, ' | Ontdubbeling ResourceId' aan de query.

### <a name="servicemapcomputercl-records"></a>ServiceMapComputer_CL records
Records van het type *ServiceMapComputer_CL* inventarisgegevens voor servers met Serviceoverzicht agents hebben. Deze records hebben de eigenschappen in de volgende tabel:

| Eigenschap | Beschrijving |
|:--|:--|
| Type | *ServiceMapComputer_CL* |
| SourceSystem | *OpsManager* |
| ResourceId | De unieke id voor een computer in de werkruimte |
| ResourceName_s | De unieke id voor een computer in de werkruimte |
| ComputerName_s | De FQDN-naam van de computer |
| Ipv4Addresses_s | Een lijst met de server IPv4-adressen |
| Ipv6Addresses_s | Een lijst van de server IPv6-adressen |
| DnsNames_s | Een matrix van DNS-namen |
| OperatingSystemFamily_s | Windows- of Linux |
| OperatingSystemFullName_s | De volledige naam van het besturingssysteem  |
| Bitness_s | De bitness van de machine (32-bits of 64-bits)  |
| PhysicalMemory_d | Het fysieke geheugen in MB |
| Cpus_d | Het aantal CPU 's |
| CpuSpeed_d | De CPU-snelheid in MHz|
| VirtualizationState_s | *Onbekende*, *fysieke*, *virtuele*, *hypervisor* |
| VirtualMachineType_s | *Hyper-v*, *vmware*, enzovoort |
| VirtualMachineNativeMachineId_g | De ID van de VM die is toegewezen door de hypervisor |
| VirtualMachineName_s | De naam van de virtuele machine |
| BootTime_t | De starttijd |



### <a name="servicemapprocesscl-type-records"></a>Type ServiceMapProcess_CL records
Records van het type *ServiceMapProcess_CL* beschikken over inventarisgegevens voor TCP-gerelateerde processen op servers met Serviceoverzicht agents. Deze records hebben de eigenschappen in de volgende tabel:

| Eigenschap | Beschrijving |
|:--|:--|
| Type | *ServiceMapProcess_CL* |
| SourceSystem | *OpsManager* |
| ResourceId | De unieke id voor een proces in de werkruimte |
| ResourceName_s | De unieke id voor een proces binnen de computer waarop deze wordt uitgevoerd|
| MachineResourceName_s | De naam van de resource van de machine |
| ExecutableName_s | De naam van het uitvoerbare bestand van het proces |
| StartTime_t | De begintijd van de proces-groep |
| FirstPid_d | De eerste PID in de procesgroep |
| Description_s | De Procesbeschrijving |
| CompanyName_s | De naam van het bedrijf |
| InternalName_s | De interne naam |
| ProductName_s | De naam van het product |
| ProductVersion_s | De versie van het product |
| FileVersion_s | De bestandsversie |
| CommandLine_s | De opdrachtregel |
| ExecutablePath _K | Het pad naar het uitvoerbare bestand |
| WorkingDirectory_s | De werkmap |
| Gebruikersnaam | Het account waaronder het proces wordt uitgevoerd |
| UserDomain | Het domein waaronder het proces wordt uitgevoerd |


## <a name="sample-log-searches"></a>Voorbeeldzoekopdrachten in logboeken

### <a name="list-all-known-machines"></a>Lijst van alle bekende machines
ServiceMapComputer_CL | overzicht van arg_max(TimeGenerated, *) door ResourceId

### <a name="list-the-physical-memory-capacity-of-all-managed-computers"></a>Lijst van de capaciteit van het fysieke geheugen van alle beheerde computers.
ServiceMapComputer_CL | overzicht van arg_max(TimeGenerated, *) door ResourceId | Project PhysicalMemory_d, ComputerName_s

### <a name="list-computer-name-dns-ip-and-os"></a>Computernaam van de lijst, DNS, IP- en OS.
ServiceMapComputer_CL | overzicht van arg_max(TimeGenerated, *) door ResourceId | Project ComputerName_s, OperatingSystemFullName_s, DnsNames_s, Ipv4Addresses_s

### <a name="find-all-processes-with-sql-in-the-command-line"></a>Alle processen met 'sql' niet vinden in de opdrachtregel
ServiceMapProcess_CL | waar CommandLine_s contains_cs 'sql' | overzicht van arg_max(TimeGenerated, *) door ResourceId

### <a name="find-a-machine-most-recent-record-by-resource-name"></a>Een machine (meest recente record) vinden met de resourcenaam
zoeken in (ServiceMapComputer_CL) 'm-4b9c93f9-bc37-46df-b43c-899ba829e07b' | overzicht van arg_max(TimeGenerated, *) door ResourceId

### <a name="find-a-machine-most-recent-record-by-ip-address"></a>Een machine (meest recente record) vinden door IP-adres
zoeken in (ServiceMapComputer_CL) '10.229.243.232' | overzicht van arg_max(TimeGenerated, *) door ResourceId

### <a name="list-all-known-processes-on-a-specified-machine"></a>Lijst van alle bekende processen op een opgegeven computer
ServiceMapProcess_CL | waar MachineResourceName_s == "m-559dbcd8-3130-454d-8d1d-f624e57961bc" | overzicht van arg_max(TimeGenerated, *) door ResourceId

### <a name="list-all-computers-running-sql"></a>Lijst van alle computers met SQL
ServiceMapComputer_CL | waar ResourceName_s in ((zoeken in (ServiceMapProcess_CL) '\*sql\*' | distinct MachineResourceName_s)) | distinct ComputerName_s

### <a name="list-all-unique-product-versions-of-curl-in-my-datacenter"></a>Lijst van alle unieke productversies van curl in mijn datacenter
ServiceMapProcess_CL | waar ExecutableName_s == "curl" | afzonderlijke ProductVersion_s

### <a name="create-a-computer-group-of-all-computers-running-centos"></a>Maak een computergroep van alle computers met CentOS
ServiceMapComputer_CL | waar OperatingSystemFullName_s contains_cs 'CentOS' | afzonderlijke ComputerName_s


## <a name="rest-api"></a>REST-API
Alle door de server-, proces- en afhankelijkheid gegevens in Serviceoverzicht beschikbaar via is de [REST-API van kaart](https://docs.microsoft.com/rest/api/servicemap/).


## <a name="diagnostic-and-usage-data"></a>Diagnostische gegevens en gebruiksgegevens
Microsoft verzamelt automatisch gebruiks- en via uw gebruik van de service Serviceoverzicht. Microsoft gebruikt deze gegevens te bieden en de kwaliteit, beveiliging en integriteit van de service Serviceoverzicht te verbeteren. De gegevens omvatten informatie over de configuratie van uw software, zoals het besturingssysteem en versie, IP-adres, DNS-naam en Werkstationnaam zodat nauwkeurige en efficiënte mogelijkheden voor probleemoplossing. Microsoft verzamelt geen namen, adressen of andere contactgegevens.

Zie voor meer informatie over het verzamelen van gegevens en gebruiksgegevens het [privacyverklaring van Microsoft Online Services](https://go.microsoft.com/fwlink/?LinkId=512132).


## <a name="next-steps"></a>Volgende stappen
Meer informatie over [Meld zoekopdrachten](../log-analytics/log-analytics-log-searches.md) in logboekanalyse voor het ophalen van gegevens die worden verzameld door Serviceoverzicht.


## <a name="troubleshooting"></a>Problemen oplossen
Zie de [probleemoplossing sectie van het document Serviceoverzicht configureren](operations-management-suite-service-map-configure.md#troubleshooting).


## <a name="feedback"></a>Feedback
Hebt u feedback voor ons over Serviceoverzicht of deze documentatie?  Ga naar onze [User Voice pagina](https://feedback.azure.com/forums/267889-log-analytics/category/184492-service-map), kunt u functies aanbevelen of om bestaande suggesties stemmen.
