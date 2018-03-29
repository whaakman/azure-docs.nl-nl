---
title: Prestatiemeter-oplossing in Azure-netwerk | Microsoft Docs
description: De Prestatiemeter netwerk in Azure kunt u de prestaties van uw netwerken, in bijna realtime, om te detecteren en zoek knelpunten in de prestaties controleren.
services: log-analytics
documentationcenter: ''
author: abshamsft
manager: carmonm
editor: ''
ms.assetid: 5b9c9c83-3435-488c-b4f6-7653003ae18a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2018
ms.author: abshamsft
ms.openlocfilehash: 847213a5b8aab908d412ecc9aecec984aec1a9ba
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2018
---
# <a name="network-performance-monitor-solution-in-azure"></a>Prestatiemeter-oplossing in Azure-netwerk

![Netwerk Prestatiemeter symbool](./media/log-analytics-network-performance-monitor/npm-symbol.png)


Netwerk-Prestatiemeter is een hybride cloud-gebaseerde netwerkbewakingsgroep oplossing waarmee u netwerkprestaties tussen verschillende punten in de infrastructuur van uw netwerk te bewaken. Hiermee kunt u het bewaken van de netwerkverbinding met de service en de eindpunten van de toepassingen en de monitor ook de prestaties van Azure ExpressRoute. 

Prestatiemeter netwerk detecteert netwerkproblemen zoals verkeer blackholing, routering fouten en problemen die controlemethoden conventionele netwerk niet detecteren. De oplossing genereert waarschuwingen en waarschuwt u als een drempelwaarde voor een netwerkverbinding is geschonden. Ook zorgt ervoor dat tijdige detectie van problemen met de netwerkprestaties en de bron van het probleem met een bepaald netwerksegment of het apparaat is vertaald. 

Prestaties Netwerkcontrole biedt drie algemene mogelijkheden: 

* [Prestatiemeter](log-analytics-network-performance-monitor-performance-monitor.md): via cloudimplementaties en lokale locaties, kunt u verbinding met het netwerk controleren meerdere datacenters en filialen en bedrijfskritieke met meerdere lagen toepassingen of microservices. Met Prestatiemeter, kunt u netwerkproblemen detecteren voordat gebruikers klagen.

* [Monitor voor service-eindpunt](log-analytics-network-performance-monitor-service-endpoint.md): U kunt de connectiviteit controleren van uw gebruikers tot de services die u interesseren bepalen welke infrastructuur in het pad en identificeren waar netwerkknelpunten worden uitgevoerd. U kunt uitval weten voordat u uw gebruikers en de exacte locatie van de problemen zien op uw netwerkpad. 

    Deze mogelijkheid kunt u voert tests uit op basis van HTTP, HTTPS, TCP- en ICMP bewaken bijna realtime of in het verleden de beschikbaarheid en reactietijd van uw service. U kunt ook de bijdrage van het netwerk in pakketverlies en latentie bewaken. U kunt met een topologiekaart netwerk netwerk vertragingen isoleren. U kunt identificeren probleem plaatsen die langs het netwerkpad van het knooppunt aan de service, met latentiegegevens op elke hop optreden. U kunt met ingebouwde tests netwerkverbinding met Office 365 en Dynamics CRM zonder eventuele meteen controleren. Met deze functie kunt u verbinding met het netwerk aan een TCP-compatibele eindpunt, zoals websites, SaaS-toepassingen, PaaS-toepassingen en SQL-databases bewaken.

* [ExpressRoute-Monitor](log-analytics-network-performance-monitor-expressroute.md): end-to-end-connectiviteit en de prestaties tussen uw filialen en Azure controleren via Azure ExpressRoute.  

Meer informatie over de verschillende mogelijkheden ondersteund door [netwerk Prestatiemeter](https://docs.microsoft.com/azure/networking/network-monitoring-overview) is online beschikbaar.
 
## <a name="supported-regions"></a>Ondersteunde regio 's
NPM kunt connectiviteit tussen de netwerken en toepassingen in een deel van de hele wereld, uit een andere werkruimte die wordt gehost in een van de volgende gebieden controleren:
* West-Europa
* West-centraal VS
* VS - oost
* Zuidoost-Azië
* Zuid-Oost-Australië
* Amerikaanse overheid Virginia

De lijst met ondersteunde regio's voor ExpressRoute-Monitor is beschikbaar in de [documentatie](https://docs.microsoft.com/azure/expressroute/how-to-npm?utm_swu=8117#regions).


## <a name="set-up-and-configure"></a>Installeren en configureren

### <a name="install-and-configure-agents"></a>Installeren en configureren van agents 

De fundamentele processen gebruiken voor het installeren van agents op [verbinding maken met Windows-computers met Azure Log Analytics](log-analytics-windows-agents.md) en [Operations Manager verbinden met Log Analytics](log-analytics-om-agents.md).

### <a name="where-to-install-the-agents"></a>Waar u de agents installeert 

* **Prestatiemeter**: Installeer Operations Management Suite-agents op ten minste één knooppunt verbonden met elk subnetwerk van waaruit u wilt bewaken van de netwerkverbinding met andere subnetwerken.

    Voor het bewaken van een netwerkverbinding, agents te installeren op beide eindpunten van die koppeling. Als u niet zeker weet over de topologie van uw netwerk, moet u de agents installeren op servers met kritieke werkbelastingen tussen wie u wilt bewaken van de prestaties van het netwerk. Bijvoorbeeld, als u bewaken van de netwerkverbinding tussen een webserver en een SQL-server wilt, een agent installeren op beide servers. Agents bewaken netwerkverbinding (koppelingen) tussen de hosts, niet de hosts zelf. 

* **Monitor voor service-eindpunt**: een Operations Management Suite-agent installeren op elk knooppunt van waaruit u wilt bewaken van de netwerkverbinding met het service-eindpunt. Een voorbeeld is als u wilt bewaken van verbinding met het netwerk op Office 365 vanaf uw office-sites met het label O1, O2 en O3. Installeer de Operations Management Suite-agent op ten minste één knooppunt in O1, O2 en O3. 

* **ExpressRoute-Monitor**: ten minste één Operations Management Suite-agent installeren in uw Azure virtual network. Ten minste één agent ook installeren in uw lokale subnetwerk, die is verbonden via ExpressRoute persoonlijke peering.  

### <a name="configure-operations-management-suite-agents-for-monitoring"></a>Operations Management Suite-agents voor bewaking configureren 

Prestatiemeter netwerk gebruikt synthetische transacties voor het bewaken van de prestaties van het netwerk tussen bron- en doelserver agents. U kunt kiezen tussen TCP- en ICMP-protocol voor het bewaken in Prestatiemeter en Service-eindpunt Monitor mogelijkheden. TCP wordt gebruikt voor de ExpressRoute-Monitor. Zorg ervoor dat de firewall toestaat communicatie tussen de Operations Management Suite-agents gebruikt voor het bewaken van het protocol dat u kiest. 

* **TCP-protocol**: als u TCP als protocol voor bewaking, opent u de firewallpoort voor de agents gebruikt voor de Prestatiemeter voor netwerk- en ExpressRoute-Monitor om ervoor te zorgen dat de agents met elkaar in verbinding kunnen maken. Om te openen in de poort, het EnableRules.ps1 PowerShell-script zonder parameters worden uitgevoerd in een PowerShell-venster met beheerdersbevoegdheden.

    Het script maakt registersleutels die nodig is door de oplossing. Ook wordt gemaakt van Windows Firewall-regels zodat agents TCP-verbindingen met elkaar maken. De registersleutels gemaakt door het script opgeven of de logboeken voor foutopsporing en het pad voor het bestand Logboeken in logboek registreren. Het script wordt ook gebruikt voor communicatie van agent-TCP-poort. De waarden voor deze sleutels worden automatisch ingesteld door het script. Deze sleutels niet handmatig te wijzigen. De poort die standaard geopend is 8084. U kunt een aangepaste poort gebruiken door de parameter-poortnummer aan het script. Gebruik dezelfde poort op alle computers waarop het script wordt uitgevoerd. 

    >[!NOTE]
    > Het script configureert alleen Windows Firewall lokaal. Als u een netwerkfirewall hebt, ervoor zorgen dat verkeer dat is bestemd voor de TCP-poort die wordt gebruikt door Netwerkcontrole prestaties kunnen.

    >[!NOTE]
    > U hoeft niet de EnableRules.ps1 PowerShell-script uitvoeren voor Service-eindpunt Monitor.

    

* **ICMP-protocol**: u kunt eventueel ICMP als protocol voor het bewaken van de volgende firewallregels op betrouwbare wijze gebruikmaken van ICMP inschakelen:
    
   ```
   netsh advfirewall firewall add rule name="NPMDICMPV4Echo" protocol="icmpv4:8,any" dir=in action=allow 
   netsh advfirewall firewall add rule name="NPMDICMPV6Echo" protocol="icmpv6:128,any" dir=in action=allow 
   netsh advfirewall firewall add rule name="NPMDICMPV4DestinationUnreachable" protocol="icmpv4:3,any" dir=in action=allow 
   netsh advfirewall firewall add rule name="NPMDICMPV6DestinationUnreachable" protocol="icmpv6:1,any" dir=in action=allow 
   netsh advfirewall firewall add rule name="NPMDICMPV4TimeExceeded" protocol="icmpv4:11,any" dir=in action=allow 
   netsh advfirewall firewall add rule name="NPMDICMPV6TimeExceeded" protocol="icmpv6:3,any" dir=in action=allow 
   ```
 

### <a name="configure-the-solution"></a>De oplossing configureren 

1. De netwerk-Prestatiemeter-oplossing toevoegen aan uw werkruimte van de [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.NetworkMonitoringOMS?tab=Overview). Ook kunt u de procedure beschreven in [toevoegen Log Analytics-oplossingen van de galerie met oplossingen](log-analytics-add-solutions.md). 
2. Open de werkruimte voor logboekanalyse en selecteer de **overzicht** tegel. 
3. Selecteer de **netwerk Prestatiemeter** tegel met het bericht *oplossing is aanvullende configuratie vereist*.

   ![Netwerk Prestatiemeter tegel](media/log-analytics-network-performance-monitor/npm-config.png)

4. Op de **Setup** pagina ziet u de optie Operations Management Suite-agents installeren en configureren van de agents voor bewaking in de **algemene instellingen** weergeven. Eerder uitgelegd, als u geïnstalleerd en geconfigureerd Operations Management Suite-agents, selecteert u de **Setup** weergave voor het configureren van de mogelijkheid die u wilt gebruiken. 

   **Prestatiemeter**: Kies het protocol moet worden gebruikt voor synthetische transacties in de **standaard** Prestatiemeter regel en selecteer **opslaan en doorgaan**. Deze selectie bevat alleen voor de standaardregel voor het systeem gegenereerd. U moet telkens wanneer die u een regel Prestatiemeter expliciet maken voor het protocol kiezen. U kunt altijd verplaatsen naar de **standaard** regel instellingen op de **Prestatiemeter** tabblad (deze wordt weergegeven na het voltooien van de configuratie van de dag-0) en wijzig het protocol later. Als u de rPerfomance Monitor mogelijkheid niet wilt, kunt u de standaardregel van uitschakelen de **standaard** regel instellingen op de **Prestatiemeter** tabblad.

   ![Monitor Prestatieweergave](media/log-analytics-network-performance-monitor/npm-synthetic-transactions.png)
    
   **Monitor voor service-eindpunt**: de mogelijkheid biedt ingebouwde vooraf geconfigureerde tests uit om de netwerkverbinding met Office 365 en Dynamics 365 van uw agents bewaken. Kies de Office 365 en Dynamics 365-services die u bewaken wilt door de selectievakjes naast deze in te schakelen. Als u de agents die u wilt bewaken, selecteer **Agents toevoegen**. Als u niet wilt gebruiken van deze mogelijkheid of later instellen, niet iets kiest en selecteert **opslaan en doorgaan**.

   ![Monitor voor service-eindpunt weergeven](media/log-analytics-network-performance-monitor/npm-service-endpoint-monitor.png)

   **ExpressRoute-Monitor**: Selecteer **nu detecteren** voor het detecteren van alle persoonlijke peerings die zijn verbonden met de virtuele netwerken in de Azure-abonnement is gekoppeld aan deze werkruimte voor logboekanalyse ExpressRoute. 

   >[!NOTE] 
   > De oplossing wordt momenteel alleen persoonlijke ExpressRoute peerings detecteert. 

   >[!NOTE] 
   > Alleen worden persoonlijke peerings die zijn verbonden met de virtuele netwerken die zijn gekoppeld aan het abonnement is gekoppeld aan deze werkruimte voor logboekanalyse gedetecteerd. Als ExpressRoute is verbonden met virtuele netwerken buiten het abonnement is gekoppeld aan deze werkruimte, maakt u een werkruimte voor logboekanalyse in deze abonnementen. Netwerk-Prestatiemeter gebruiken voor het bewaken van deze peerings.

   ![ExpressRoute-Monitor weergeven](media/log-analytics-network-performance-monitor/npm-express-route.png)

   Nadat de detectie is voltooid, worden de gedetecteerde persoonlijke peerings in een tabel weergegeven. 

   ![De pagina configuratie van de Monitor prestaties netwerk](media/log-analytics-network-performance-monitor/npm-private-peerings.png)
    
De bewaking voor deze peerings is in eerste instantie een uitgeschakeld. Selecteer elke peering die u wilt bewaken, en ze uit de detailweergave aan de rechterkant controle configureren. Selecteer **opslaan** aan de configuratie op te slaan. Zie het artikel 'Configureren ExpressRoute monitoring' voor meer informatie. 

Nadat de installatie is voltooid, duurt het 30 minuten tot een uur om de gegevens te vullen. Terwijl de oplossing maakt een aggregatie van gegevens van uw netwerk, ziet u het bericht *oplossing is aanvullende configuratie vereist* op de prestaties van Network Monitor **overzicht** tegel. Nadat de gegevens worden verzameld en geïndexeerd, de **overzicht** tegel verandert en geeft een melding van de status van uw netwerk in een samenvatting. Vervolgens kunt u de bewaking van de knooppunten op welke Operations Management Suite agents zijn geïnstalleerd, evenals de subnetten van uw omgeving zijn gedetecteerd.

#### <a name="edit-monitoring-settings-for-subnets-and-nodes"></a>Controle-instellingen voor subnetten en knooppunten bewerken 

Alle subnetten met ten minste één agent is geïnstalleerd, worden vermeld op de **subnetwerken** tabblad op de configuratiepagina. 


-Of uitschakelen van de bewaking van bepaalde subnetwerken:

1. Selecteer of schakel het selectievakje naast de **subnetwerk ID**. Zorg ervoor dat **gebruiken voor bewaking** is ingeschakeld of uitgeschakeld, indien nodig. U kunt selecteren of wissen van meerdere subnetten. Uitgeschakelde subnetwerken worden niet bewaakt en de agents worden bijgewerkt om te stoppen met het pingen van andere agents. 
2. Kies de knooppunten die u wilt controleren in een bepaalde subnetwerk. Selecteer het subnetwerk in de lijst en de vereiste knooppunten verplaatsen tussen de lijsten met niet-beheerd en gecontroleerd knooppunten bevatten. U kunt een aangepaste beschrijving toevoegen aan het subnetwerk.
3. Selecteer **opslaan** aan de configuratie op te slaan. 

#### <a name="choose-nodes-to-monitor"></a>Kies knooppunten om te controleren

Alle knooppunten die een agent is geïnstalleerd op deze worden vermeld op de **knooppunten** tabblad. 

1. Selecteer of schakel de knooppunten die u wilt controleren of de bewaking stopt. 
2. Selecteer **gebruiken voor bewaking**, in- of uitschakelen, naar gelang van toepassing. 
3. Selecteer **Opslaan**. 


Configureer de gewenste mogelijkheden:

- [Prestatiemeter](log-analytics-network-performance-monitor-performance-monitor.md#configuration)
- [Monitor voor service-eindpunt](log-analytics-network-performance-monitor-performance-monitor.md#configuration)
- [ExpressRoute Monitor](log-analytics-network-performance-monitor-expressroute.md#configuration)

 

## <a name="data-collection-details"></a>Details van de verzameling gegevens
Prestatiemeter netwerk gebruikt TCP SYN-SYNACK-ACK-handshake-pakketten en netwerklatentie om informatie te verzamelen, wanneer u ervoor TCP als protocol kiest. Prestatiemeter netwerk maakt gebruik van ICMP ECHO ICMP-ECHOANTWOORD wanneer u ervoor ICMP als protocol kiest. Opdracht Traceroute wordt ook gebruikt voor het ophalen van gegevens over de topologie.

De volgende tabel bevat gegevens verzameling methoden en andere informatie over hoe gegevens worden verzameld voor netwerk-Prestatiemeter.

| Platform | Directe agent | System Center Operations Manager-agent | Azure Storage | Operations Manager is vereist? | Operations Manager-agent gegevens verzonden via de beheergroep | Verzamelingsfrequentie |
| --- | --- | --- | --- | --- | --- | --- |
| Windows | &#8226; | &#8226; |  |  |  |TCP-handshakes/ICMP-echoberichten om de vijf seconden gegevens verzonden om de 3 minuten |
 

 
De oplossing maakt gebruik van synthetische transacties om de status van het netwerk te beoordelen. Operations Management Suite-agenten zijn geïnstalleerd op verschillende momenten in de TCP-netwerkpakketten exchange of ICMP Echo met elkaar. Hiermee wordt aangegeven of de agents gebruikt TCP-pakketten of ICMP Echo, hangt af van het protocol dat u hebt geselecteerd voor bewaking. In het proces agents meer informatie over de round trip time en pakket verloren zijn gegaan, indien van toepassing. Elke agent voert regelmatig ook een opdracht Traceroute naar andere agents vinden van de verschillende routes in het netwerk dat moet worden getest. Met deze gegevens kunnen de agents deduceren de netwerklatentie en pakket verliescijfers. De tests worden herhaald elke vijf seconden. Gegevens worden samengevoegd voor ongeveer drie minuten door de agents voordat deze wordt geüpload naar de Log Analytics-service.



>[!NOTE]
> Hoewel vaak agents met elkaar communiceren, geen ze significant netwerkverkeer genereren tijdens de uitvoering van de test. Agents afhankelijk zijn van alleen TCP SYN-SYNACK-ACK handshake pakketten om te bepalen en de netwerklatentie. Er is geen gegevenspakketten worden uitgewisseld. Tijdens dit proces communiceren agents met elkaar alleen wanneer deze nodig is. De agent communicatie-topologie wordt geoptimaliseerd netwerkverkeer te verminderen.

## <a name="use-the-solution"></a>De oplossing gebruiken 

### <a name="network-performance-monitor-overview-tile"></a>Tegel voor netwerk-Prestatiemeter-overzicht 

Nadat u de netwerk-Prestatiemeter-oplossing hebt ingeschakeld, wordt de oplossing tegel op de **overzicht** pagina bevat een overzicht van de status van het netwerk. 

 ![Tegel voor netwerk-Prestatiemeter-overzicht](media/log-analytics-network-performance-monitor/npm-overview-tile.png)

### <a name="network-performance-monitor-dashboard"></a>Netwerk Prestatiemeter-dashboard 

* **Top netwerk Health gebeurtenissen**: deze pagina bevat een lijst van de meest recente status gebeurtenissen en waarschuwingen in het systeem en de tijd sinds de gebeurtenissen actief zijn geweest. Een statusgebeurtenis of een waarschuwing wordt gegenereerd wanneer de waarde van de gekozen metriek (latentie verlies reactietijd of gebruik van de bandbreedte) voor de bewaking regel de drempelwaarde overschrijdt. 

* **ExpressRoute-Monitor**: deze pagina biedt samenvattingen van de status voor de verschillende peering ExpressRoute-verbindingen de oplossing monitors. De **topologie** tegel toont het aantal netwerkpaden via de ExpressRoute-circuits die worden bewaakt in uw netwerk. Selecteer deze tegel naar de **topologie** weergeven.

* **Monitor voor service-eindpunt**: deze pagina biedt samenvattingen van de status voor de verschillende tests die u hebt gemaakt. De **topologie** tegel toont het aantal eindpunten die worden bewaakt. Selecteer deze tegel naar de **topologie** weergeven.

* **Prestatiemeter**: deze pagina vindt u statusoverzichten voor de **netwerk** koppelingen en **subnetwerk** koppelingen die de oplossing wordt bewaakt. De **topologie** tegel toont het aantal netwerkpaden die worden bewaakt in uw netwerk. Selecteer deze tegel naar de **topologie** weergeven. 

* **Algemene query's**: deze pagina bevat een reeks zoekquery's ophalen van onbewerkte netwerk rechtstreeks met het bewaken van gegevens. U kunt deze query's als uitgangspunt gebruiken voor het maken van uw eigen query's voor aangepaste rapportage. 

   ![Netwerk Prestatiemeter-dashboard](media/log-analytics-network-performance-monitor/npm-dashboard.png)

 

### <a name="drill-down-for-depth"></a>Inzoomen voor diepte 

U kunt verschillende koppelingen op het dashboard van de oplossing om in te zoomen selecteren dieper omlaag in de gedeelten van belang. Bijvoorbeeld, wanneer er een waarschuwing of een onjuiste netwerkkoppeling worden weergegeven op het dashboard, selecteert u deze verder onderzoeken. Een pagina worden alle subnetwerkkoppelingen voor de koppeling bepaald netwerk. U ziet het gegevensverlies, latentie en status van elke koppeling subnetwerk. U vindt snel welke koppeling subnetwerk problemen veroorzaakt. Selecteer **weergeven knooppuntkoppelingen** voor een overzicht van alle knooppuntkoppelingen voor de koppeling slecht subnet. Vervolgens kunt u afzonderlijke knooppunt naar koppelingen weergeven en de slechte knooppuntkoppelingen vinden. 

Selecteer **weergave topologie** om weer te geven van de topologie hop door hop van de routes tussen de bron- en doelserver knooppunten. De slechte routes weergegeven in rood. De latentie die is bijgedragen door elke hop zodat u snel het probleem naar een bepaald deel van het netwerk kan identificeren, kunt u weergeven.

 

### <a name="network-state-recorder-control"></a>Controle van de Vluchtrecorder netwerk staat

Elke weergave toont een momentopname van de status van uw netwerk op een bepaald punt in tijd. Standaard zijn de meest recente status wordt weergegeven. De balk aan de bovenkant van de pagina geeft het punt in tijd waarvoor de status wordt weergegeven. Als u wilt weergeven van een momentopname van de status van uw netwerk op een eerder tijdstip, selecteer **acties**. U kunt ook inschakelen of uitschakelen automatisch vernieuwen voor alle pagina's, terwijl u de meest recente status weergeven. 

 ![Netwerk status doos](media/log-analytics-network-performance-monitor/network-state-recorder.png)

 

### <a name="trend-charts"></a>Van trendgrafieken 

Op elk niveau die u kunt inzoomen, ziet u de trend van de toepasselijke metrische gegevens. Het kan zijn gegevensverlies, latentie, reactietijd of gebruik van de bandbreedte. Wilt wijzigen van het interval voor de trend, gebruikt u het besturingselement tijd aan het begin van de grafiek. 

Van trendgrafieken tonen een historisch perspectief van de prestaties van een metriek prestaties. Sommige netwerkproblemen tijdelijke aard zijn en zijn moeilijk te vangen door te kijken alleen de huidige status van het netwerk. Problemen kunnen snel surface en verdwijnen voordat iedereen meldingen, alleen op een later tijdstip in de tijd weer. Dergelijke problemen van voorbijgaande aard ook zijn lastig voor toepassingsbeheerders. Problemen met de vaak worden weergegeven als Onverklaarbaar toename in de reactietijd van toepassing, zelfs als alle onderdelen van de toepassing worden weergegeven om te functioneren. 

Eenvoudig kunt u dit soort problemen detecteren door te kijken op een grafiek trend. Het probleem wordt weergegeven als een plotselinge piek in de netwerklatentie of pakketverlies. Voor het onderzoeken van het probleem door het netwerk staat doos-besturingselement te gebruiken om weer te geven van de momentopname van de netwerk- en voor dat punt in de tijd waarop het probleem is opgetreden.

 
![Van trendgrafieken](media/log-analytics-network-performance-monitor/trend-charts.png)
 

### <a name="topology-map"></a>Topologiekaart 

Netwerk-Prestatiemeter toont de topologie hop door hop van routes tussen de bron- en eindpunt op een interactieve topologiekaart. Als u wilt weergeven van de topologiekaart, selecteer de **topologie** tegel op het dashboard van oplossing. U kunt ook selecteren de **weergave topologie** koppeling op de pagina inzoomen's. 

De topologiekaart toont het aantal routes tussen de bron- en doelserver en wat zijn de gegevenspakketten nemen paden. De latentie die is bijgedragen door elke hop netwerk wordt ook weergegeven. Alle paden die waarvoor de totale pad latentie hoger dan de drempelwaarde is (ingesteld in de desbetreffende regel bewaking) zijn in rood weergegeven. 

Als u een knooppunt of aanwijzen eroverheen op de topologiekaart selecteert, ziet u de eigenschappen van het knooppunt, zoals de FQDN-naam en IP-adres. Selecteer een hop om te zien van het IP-adres. U kunt de hop lastige netwerk identificeren door de latentie die het bijdraagt daar iets van merkt. Als u wilt filteren op bepaalde routes, de filters te gebruiken in het deelvenster samenvouwbare actie. Verbergen om te vereenvoudigen de netwerktopologieën, de tussenliggende hops met behulp van de schuifregelaar in het actiedeelvenster. U kunt inzoomen of uitzoomen buiten de topologiekaart met behulp van het muiswiel. 

De topologie die wordt weergegeven in de kaart is laag 3-topologie en bevat geen laag 2-apparaten en verbindingen. 

 
![Topologiekaart](media/log-analytics-network-performance-monitor/topology-map.png)
 

## <a name="log-analytics-search"></a>Log Analytics zoeken 

Alle gegevens die pagina's grafisch weergegeven via het netwerk-Prestatiemeter-dashboard en inzoomen is ook beschikbaar in ingebouwd [logboekanalyse zoeken](log-analytics-log-search-new.md). U kunt interactieve analyses van gegevens in de opslagplaats en correleren van gegevens uit verschillende bronnen. U kunt ook aangepaste waarschuwingen en weergaven maken en de gegevens exporteren naar Excel, Power BI of een deelbaar koppeling. De **algemene query's** gebied in het dashboard heeft enkele nuttig query's die u als startpunt gebruiken kunt voor het maken van uw eigen query's en rapporten. 

 

## <a name="provide-feedback"></a>Feedback geven 

* **UserVoice:** kunt u uw ideeën voor netwerk-Prestatiemeter-functies die u laten werken wilt op boeken. Ga naar de [UserVoice pagina](https://feedback.azure.com/forums/267889-log-analytics/category/188146-network-monitoring). 

* **Deelnemen aan onze cohort:** we altijd geïnteresseerd in nieuwe klanten ons cohort join hebben. Als onderdeel van het krijgt u vroege toegang tot de nieuwe functies en een kans om u te helpen bij het verbeteren van de Prestatiemeter netwerk. Als u geïnteresseerd in toevoegen bent, vult u dit [Snelle enquête](https://aka.ms/npmcohort). 

## <a name="next-steps"></a>Volgende stappen 
Meer informatie over [Prestatiemeter](log-analytics-network-performance-monitor-performance-monitor.md), [Monitor voor Service-eindpunt](log-analytics-network-performance-monitor-performance-monitor.md), en [ExpressRoute-Monitor](log-analytics-network-performance-monitor-expressroute.md). 
