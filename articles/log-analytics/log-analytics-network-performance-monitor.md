---
title: Prestatiemeter-oplossing in Azure-netwerk | Microsoft Docs
description: Prestatiemeter in Azure-netwerk helpt u bij het controleren van de prestaties van uw netwerken in bijna de real time te detecteren en knelpunten in de prestaties te vinden.
services: log-analytics
documentationcenter: 
author: abshamsft
manager: carmonm
editor: 
ms.assetid: 5b9c9c83-3435-488c-b4f6-7653003ae18a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2018
ms.author: abshamsft
ms.openlocfilehash: 399fe552d5c7d9a96cdabc2a1dfafe99635d4a61
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2018
---
# <a name="network-performance-monitor-solution-in-azure"></a>Prestatiemeter-oplossing in Azure-netwerk

![Netwerk Prestatiemeter symbool](./media/log-analytics-network-performance-monitor/npm-symbol.png)


Netwerk Performance Monitor (NPM) is een hybride cloud-gebaseerde netwerkbewakingsgroep oplossing waarmee u netwerkprestaties tussen verschillende punten in de netwerkinfrastructuur, monitor-netwerkverbinding met de eindpunten van de service-toepassing te bewaken en de prestaties van uw Azure ExpressRoute controleren.  

NPM detecteert netwerkproblemen zoals verkeer blackholing, routering fouten en problemen die controlemethoden conventionele netwerk niet detecteren. De oplossing waarschuwingen genereert, wordt gewaarschuwd wanneer een drempelwaarde voor een netwerkverbinding is geschonden, zorgt u ervoor tijdige detectie van problemen met de netwerkprestaties en de bron van het probleem met een bepaald netwerksegment of het apparaat is vertaald. 

NPM biedt drie algemene mogelijkheden: 

[Prestatiemeter](log-analytics-network-performance-monitor-performance-monitor.md): netwerkverbinding bewaken voor cloud-implementaties en on-premises locaties, meerdere datacenters en filialen, bedrijfskritieke kritieke meerdere lagen toepassingen/micro-services. Met Prestatiemeter, kunt u netwerkproblemen detecteren voordat uw gebruikers klagen.  

[Monitor voor service-eindpunt](log-analytics-network-performance-monitor-service-endpoint.md): U kunt de connectiviteit controleren van uw gebruikers naar de services die u interesseren bepalen welke infrastructuur in het pad en waarbij het netwerk knelpunten plaatsvinden. Weten over storingen voordat uw gebruikers en de exacte locatie van de problemen op uw netwerkpad zien. 

Deze mogelijkheid kunt u het uitvoeren van http, HTTPS, TCP- en ICMP gebaseerd tests uit om controleren in bijna realtime of in het verleden de beschikbaarheid en reactietijd van uw service en de bijdrage van het netwerk in pakketverlies en latentie. Met de netwerktoewijzing-topologie, kunt u netwerk vertragingen isoleren door te identificeren probleem plaatsen die langs het netwerkpad van het knooppunt aan de service, met latentiegegevens op elke hop optreden. Met ingebouwde tests controleren netwerkverbinding met Office365 en Dynamics CRM zonder vooraf configuratie. Met deze functie kunt u de netwerkverbinding controleren voor alle TCP-compatibele eindpunt zoals websites, SaaS, PaaS-toepassingen, SQL-databases, enzovoort.  

[ExpressRoute-Monitor](log-analytics-network-performance-monitor-expressroute.md): end-to-end-connectiviteit en de prestaties tussen uw filialen en Azure controleren via Azure ExpressRoute.  
 

## <a name="set-up-and-configure"></a>Installeren en configureren

### <a name="install-and-configure-agents"></a>Installeren en configureren van agents 

De fundamentele processen gebruiken voor het installeren van agents op [verbinding maken met Windows-computers met logboekanalyse](log-analytics-windows-agents.md) en [Operations Manager verbinden met Log Analytics](log-analytics-om-agents.md).

### <a name="where-to-install-the-agents"></a>Waar u de agents installeert 

**Prestatiemeter:** installeren OMS-agents op ten minste één knooppunt met elk subnetwerk van waaruit u wilt bewaken van de netwerkverbinding met andere subnetwerken verbonden.  

Voor het bewaken van een netwerkverbinding, moet u agents installeren op beide eindpunten van die koppeling.  Als u over de topologie van uw netwerk weet, moet u de agents installeren op servers met kritieke werkbelastingen tussen wie u wilt bewaken van de prestaties van het netwerk. Bijvoorbeeld, als u bewaken van de netwerkverbinding tussen een webserver en een SQL-server wilt, een agent installeren op beide servers. Agents bewaken netwerkverbinding (koppelingen) tussen hosts niet de hosts zelf. 

**Monitor voor service-eindpunt:** installeren OMS-agent op elk knooppunt van waaruit u wilt bewaken van de netwerkverbinding met het service-eindpunt. Bijvoorbeeld, als u bewaken netwerkverbinding naar Office365 van uw office-website O1 wilt, installeer O2 en O3, de OMS-agent op ten minste één knooppunt in O1, O2 en O3. 

**ExpressRoute-Monitor:** ten minste één OMS-agent in uw Azure VNET en ten minste één agent installeren in uw lokale subnetwerk, die is verbonden via de ExpressRoute persoonlijke Peering.  

### <a name="configure-oms-agents-for-monitoring"></a>OMS-agents voor bewaking configureren  

NPM gebruikt synthetische transacties voor het bewaken van de prestaties van het netwerk tussen bron- en doelserver agents. De oplossing biedt een keuze tussen de TCP- en ICMP-protocol voor het bewaken in Prestatiemeter en Service-eindpunt Monitor mogelijkheden, terwijl TCP wordt gebruikt voor ExpressRoute-Monitor. Zorg ervoor dat de firewall communicatie tussen de OMS-agents wordt gebruikt voor het bewaken van het protocol dat u hebt gekozen voor het bewaken van toestaat.  

**TCP-protocol:** als u TCP hebt gekozen als het protocol voor bewaking, opent u de firewallpoort voor de agents die wordt gebruikt voor de Prestatiemeter en een ExpressRoute-Monitor mogelijkheden, om ervoor te zorgen dat de agents met elkaar in verbinding kunnen maken. U doet dit door het EnableRules.ps1 PowerShell-script zonder parameters worden uitgevoerd in een power shell-venster met beheerdersbevoegdheden.  

Het script maakt registersleutels die nodig is door de oplossing en Windows firewall-regels zodat agents TCP-verbindingen maken met elkaar wordt gemaakt. De registersleutels gemaakt door het script ook opgeven of om de logboeken voor foutopsporing en het pad voor het bestand logboeken te leggen. Het definieert ook de agent TCP-poort gebruikt voor communicatie. De waarden voor deze sleutels worden automatisch ingesteld door het script, zodat deze sleutels niet handmatig te wijzigen. De poort die standaard geopend is 8084. U kunt een aangepaste poort gebruiken door de parameter-poortnummer aan het script. Echter moet dezelfde poort worden gebruikt op alle computers waarop het script wordt uitgevoerd. 

>[!NOTE]
> Het script configureert alleen windows firewall lokaal. Als u een netwerkfirewall hebt, moet u ervoor zorgen dat het verkeer dat is bestemd voor de TCP-poort wordt gebruikt door NPM toestaat 

>[!NOTE]
> U hoeft niet de EnableRules.ps1 PowerShell-script uitvoeren voor Service-eindpunt Monitor 

 

**ICMP-protocol** : als u ICMP hebt gekozen als het protocol voor bewaking aan de volgende firewallregels voor ICMP betrouwbare manier gebruik inschakelen: 

 
```
netsh advfirewall firewall add rule name="NPMDICMPV4Echo" protocol="icmpv4:8,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6Echo" protocol="icmpv6:128,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV4DestinationUnreachable" protocol="icmpv4:3,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6DestinationUnreachable" protocol="icmpv6:1,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV4TimeExceeded" protocol="icmpv4:11,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6TimeExceeded" protocol="icmpv6:3,any" dir=in action=allow 
```
 

### <a name="configure-the-solution"></a>De oplossing configureren 

1. De netwerk-Prestatiemeter-oplossing toevoegen aan uw werkruimte van [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.NetworkMonitoringOMS?tab=Overview) of met behulp van de procedure beschreven in [toevoegen Log Analytics-oplossingen van de galerie met oplossingen](log-analytics-add-solutions.md). 
2. Open de werkruimte voor logboekanalyse en klik op de **overzicht** tegel.  
3. Klik op de tegel met de titel **netwerk Prestatiemeter** met het bericht *oplossing is aanvullende configuratie vereist*.

    ![NPM tegel](media/log-analytics-network-performance-monitor/npm-config.png)

3. Op de **Setup** pagina ziet u de optie OMS-agents installeren en configureren van de agents voor bewaking in de **algemene instellingen** weergeven. Zoals hierboven, vermeld als u al hebt geïnstalleerd en geconfigureerd OMS-agents, klikt u op de **Setup** weergave voor het configureren van de mogelijkheid dat u geïnteresseerd bent in het gebruik.  

    **Monitor Prestatieweergave** -kiezen welk protocol moet worden gebruikt voor synthetische transacties in de standaard prestaties monitor regel en klik op te slaan en verder. Deze selectie bevat alleen voor het systeem gegenereerde standaardregel en moet u ervoor kiezen het protocol telkens wanneer die u een regel Prestatiemeter expliciet maken. U kunt altijd verplaatsen naar de standaardinstellingen van de regel op het tabblad Prestatiemeter is (dit wordt weergegeven na het voltooien van de configuratie van de dag-0) en het protocol later wijzigen. Als u niet geïnteresseerd in de rPerfomance Monitor mogelijkheid, kunt u de standaardregel van de standaardinstellingen van de regel op het tabblad Prestatiemeter uitschakelen. 

    ![NPM-configuratie](media/log-analytics-network-performance-monitor/npm-synthetic-transactions.png)
    
    **Monitor voor weergave van service-eindpunt** -de mogelijkheid biedt ingebouwde vooraf geconfigureerde tests uit om de netwerkverbinding met Office365 en Dynamcis365 van uw agents bewaken. Kies de Office365 en Dynamcis365 services die u geïnteresseerd bent in de bewaking in door te controleren of het selectievakje ernaast. Kies de agents van waaruit u bewaken wilt, door te klikken op de knop toevoegen Agents. Als u niet wilt dat deze mogelijkheid gebruikt of wilt deze later instellen, kunt u deze stap overslaan en direct op in **opslaan** en **doorgaan** zonder iets te kiezen.  

    ![NPM-configuratie](media/log-analytics-network-performance-monitor/npm-service-endpoint-monitor.png)

    **Weergave van ExpressRoute-Monitor** -Klik op de **nu detecteren** knop voor het detecteren van alle persoonlijke peerings die zijn verbonden met de VNETs die in de Azure-abonnement is gekoppeld aan deze werkruimte voor logboekanalyse ExpressRoute.  


    >[!NOTE] 
    > De oplossing wordt momenteel alleen persoonlijke ExpressRoute peerings detecteert. 

    >[!NOTE] 
    > Alleen deze persoonlijke peerings gedetecteerd die zijn verbonden met de VNETs die zijn gekoppeld aan het abonnement is gekoppeld aan deze werkruimte voor logboekanalyse. Als uw ExpressRoute is verbonden met VNETs buiten het abonnement is gekoppeld aan deze werkruimte, moet u voor het maken van een werkruimte voor logboekanalyse in deze abonnementen en NPM gebruiken voor het bewaken van deze peerings. 

    ![NPM-configuratie](media/log-analytics-network-performance-monitor/npm-express-route.png)

    Zodra de detectie voltooid is, worden de gedetecteerde persoonlijke peerings in een tabel weergegeven.  

    ![NPM-configuratie](media/log-analytics-network-performance-monitor/npm-private-peerings.png)
    
    De bewaking voor deze peerings zijn in eerste instantie uitgeschakeld. Klik op elke peering dat u geïnteresseerd in de bewaking bent en het bewaken configureren voor deze uit de weergave van de gegevens aan de rechterzijde (RHS).  Klik op Opslaan om de configuratie op te slaan. Zie [ExpressRoute configureren bewaking]() voor meer informatie.  

    Zodra de installatie voltooid is, duurt het 30 minuten tot een uur om de gegevens te vullen. Terwijl de oplossing is aggregeren van gegevens van uw netwerk, ziet u *oplossing is aanvullende configuratie vereist* op de tegel NPM-overzicht. Zodra de gegevens worden verzameld en worden geïndexeerd, wordt het overzicht van wijzigingen tegel en informeert u de samenvatting van de status van uw netwerk. U kunt vervolgens de bewaking van de knooppunten waarop OMS-agents zijn geïnstalleerd, evenals de subnetten van uw omgeving gedetecteerd bewerken 

#### <a name="edit-monitoring-settings-for-subnets-and-nodes"></a>Controle-instellingen voor subnetten en knooppunten bewerken 

Alle subnetten met ten minste één agent zijn geïnstalleerd worden op het tabblad subnetwerken in de configuratiepagina weergegeven. 


-Of uitschakelen van de bewaking van bepaalde subnetwerken 

1. Selecteren of schakel het selectievakje naast de **subnetwerk ID** en zorg ervoor dat **gebruiken voor bewaking** is ingeschakeld of uitgeschakeld, indien nodig. U kunt selecteren of wissen van meerdere subnetten. Wanneer deze is uitgeschakeld, worden de subnetwerken niet bewaakt de agents zijn bijgewerkt om te stoppen met het pingen van andere agents. 
2. Kies de knooppunten die u controleren in een bepaalde subnetwerk wilt door het subnetwerk selecteren in de lijst en de vereiste knooppunten verplaatsen tussen de lijsten met niet-beheerd en gecontroleerd knooppunten. U kunt toevoegen een **aangepaste beschrijving tot** het subnetwerk. 
3. Klik op **opslaan** aan de configuratie op te slaan. 

#### <a name="choose-nodes-to-monitor"></a>Kies knooppunten om te controleren

Alle knooppunten die een agent is geïnstalleerd op deze worden vermeld in de **knooppunten** tabblad. 

1. Selecteer of schakel de knooppunten die u wilt controleren of de bewaking stopt. 
2. Klik op **gebruiken voor bewaking**, in- of uitschakelen, naar gelang van toepassing. 
3. Klik op **Opslaan**. 


Configureer de capability(s) die u geïnteresseerd bent in: 
- Configureer [Prestatiemeter](log-analytics-network-performance-monitor-performance-monitor.md#configuration)
- Configureer [Service Monitor-eindpunt](log-analytics-network-performance-monitor-performance-monitor.md#configuration)
- Configureer [ExpressRoute-Monitor](log-analytics-network-performance-monitor-expressroute.md#configuration)

 

## <a name="data-collection-details"></a>Details van de verzameling gegevens
Prestaties Netwerkcontrole gebruikt TCP SYN-SYNACK-ACK handshake pakketten wanneer TCP wordt gekozen en ICMP ECHO ICMP-ECHOANTWOORD wanneer ICMP is gekozen als het protocol en netwerklatentie informatie te verzamelen. Traceroute wordt ook gebruikt voor het ophalen van gegevens over de topologie.

De volgende tabel bevat gegevens verzameling methoden en andere informatie over hoe gegevens worden verzameld voor netwerk-Prestatiemeter.

| platform | Directe Agent | SCOM-agents | Azure Storage | SCOM vereist? | SCOM-agent gegevens die worden verzonden via de beheergroep | Frequentie van de verzameling |
| --- | --- | --- | --- | --- | --- | --- |
| Windows | &#8226; | &#8226; |  |  |  |TCP-handshakes/ICMP-echoberichten om de vijf seconden gegevens verzonden om de 3 minuten |
 

 
De oplossing maakt gebruik van synthetische transacties om de status van het netwerk te beoordelen. OMS-agents die zijn geïnstalleerd op verschillende momenten in de TCP-netwerkpakketten exchange of ICMP Echo (afhankelijk van het protocol geselecteerd voor bewaking) met elkaar. In het proces agents meer informatie over de round trip time en pakket verloren zijn gegaan, indien van toepassing. Elke agent voert regelmatig ook een opdracht Traceroute naar andere agents vinden van de verschillende routes in het netwerk dat moet worden getest. Met deze gegevens kunnen de agents deduceren de netwerklatentie en pakket verliescijfers. De tests worden herhaald op elke vijf seconden en de gegevens worden samengevoegd voor een periode van drie minuten door de agents voordat u dit uploadt naar de Log Analytics-service. 



>[!NOTE]
> Hoewel vaak agents met elkaar communiceren, ze genereren geen aanzienlijke netwerkverkeer tijdens de uitvoering van de test. Agents afhankelijk zijn van alleen TCP SYN-SYNACK-ACK handshake pakketten om te bepalen en de netwerklatentie--er worden geen gegevens pakketten worden uitgewisseld. Tijdens dit proces agents met elkaar communiceren alleen wanneer deze nodig is en de agent communicatie-topologie wordt geoptimaliseerd om het netwerkverkeer te verminderen.

## <a name="using-the-solution"></a>De oplossing gebruiken 

### <a name="npm-overview-tile"></a>Overzicht van de NPM-tegel 

Nadat u de netwerk-Prestatiemeter-oplossing hebt ingeschakeld, biedt de tegel oplossing op de pagina overzicht van een snel overzicht van de status van het netwerk. 

 ![NPM Overzichttegel](media/log-analytics-network-performance-monitor/npm-overview-tile.png)

### <a name="network-performance-monitor-dashboard"></a>Netwerk Prestatiemeter-dashboard 

De **boven netwerk Health gebeurtenissen** pagina bevat een lijst van de meest recente status gebeurtenissen en waarschuwingen in het systeem en de tijd sinds de gebeurtenis actief is geweest. Een statusgebeurtenis of een waarschuwing wordt gegenereerd wanneer de waarde van de gekozen metriek (latentie verlies reactietijd of gebruik van de bandbreedte) voor de bewaking regel de drempelwaarde overschrijdt. 

De **Prestatiemeter** pagina geeft u een overzicht van de status van de netwerkkoppelingen en subnetwerk koppelingen worden bewaakt door de oplossing. De tegel topologie informeert het nummer van de netwerkpaden worden bewaakt in uw netwerk. Rechtstreeks op deze tegel te klikken gaat u naar de weergave van de topologie. 

De **Monitor voor Service-eindpunt** pagina geeft u een overzicht van de status van de verschillende tests die u hebt gemaakt. De tegel topologie informeert het aantal eindpunten worden bewaakt. Rechtstreeks op deze tegel te klikken gaat u naar de weergave van de topologie.

De **ExpressRoute-Monitor** pagina geeft u een overzicht van de status van de verschillende peering ExpressRoute-verbindingen met de door de oplossing wordt bewaakt. De tegel topologie informeert het nummer van de netwerkpaden door middel van de ExpressRoute-circuit(s) bewaakt in uw netwerk. Rechtstreeks op deze tegel te klikken gaat u naar de weergave van de topologie.

De **algemene query's** pagina bevat een reeks zoekquery's ophalen van onbewerkte netwerk rechtstreeks met het bewaken van gegevens. U kunt deze query's als uitgangspunt gebruiken voor het maken van uw eigen query's voor aangepaste rapportage. 

![NPM Dashboard](media/log-analytics-network-performance-monitor/npm-dashboard.png)

 

### <a name="drill-down-for-depth"></a>Inzoomen voor diepte 

U kunt op verschillende koppelingen klikken op het dashboard van de oplossing om in te zoomen dieper omlaag in de gedeelten van belang. Bijvoorbeeld, wanneer er een waarschuwing of een onjuiste netwerkkoppeling worden weergegeven op het dashboard, kunt u deze verder onderzoeken. Gaat u naar een pagina met een lijst met alle subnetwerkkoppelingen voor de koppeling bepaald netwerk. U bent kunnen de status van het gegevensverlies, latentie en de gezondheid van elke koppeling subnetwerk zien en snel uitzoeken wat subnetwerkkoppelingen het probleem veroorzaken. U kunt vervolgens klikken op **knooppuntkoppelingen weergeven** voor een overzicht van alle knooppuntkoppelingen voor de koppeling slecht subnet. Vervolgens kunt u afzonderlijke knooppunt naar koppelingen weergeven en de slechte knooppuntkoppelingen vinden. 

U kunt klikken op **weergave topologie** om weer te geven van de topologie hop door hop van de routes tussen de bron- en doelserver knooppunten. De slechte routes worden in rood weergegeven en kunt u de latentie die is bijgedragen door elke hop zodat u snel het probleem naar een bepaald deel van het netwerk kan identificeren weergeven. 

 

### <a name="network-state-recorder"></a>Netwerk status doos 

Elke weergave toont een momentopname van de status van uw netwerk op een bepaald punt in tijd. Standaard zijn de meest recente status wordt weergegeven. De balk aan de bovenkant van de pagina geeft het punt in tijd waarvoor de status wordt weergegeven. U kunt teruggaan in tijd en weergeven van de momentopname van de status van uw netwerk door te klikken op de balk op acties. U kunt ook of automatisch vernieuwen voor alle pagina's uitschakelen terwijl u de meest recente status weergeven. 

 ![Netwerk status doos](media/log-analytics-network-performance-monitor/network-state-recorder.png)

 

### <a name="trend-charts"></a>Van trendgrafieken 

Op elk niveau die u kunt inzoomen, ziet u de trend van de toepasselijke metriek – verlies, latentie, reactietijden, gebruik van de bandbreedte. U kunt het interval voor de trend wijzigen met behulp van het besturingselement tijd aan het begin van de grafiek. 

Van trendgrafieken tonen een historisch perspectief van de prestaties van een metriek prestaties. Sommige netwerkproblemen tijdelijke aard zijn en zijn moeilijk te vangen alleen door te kijken naar de huidige status van het netwerk. Dit is omdat problemen kunnen snel surface en verdwijnen voordat iedereen meldingen, alleen op een later tijdstip in de tijd weer. Dergelijke problemen van voorbijgaande aard kunnen ook lastig voor toepassingsbeheerders die vaak oppervlak als Onverklaarbaar toename in de reactietijd van toepassing, zelfs als alle toepassingsonderdelen weergegeven probleemloos uitgeeft. 

Eenvoudig kunt u dit soort problemen detecteren door te kijken op een trend grafiek waar het probleem wordt weergegeven als een plotselinge piek in de netwerklatentie of pakketverlies. Vervolgens kunt u het probleem onderzoeken via het netwerk staat doos om weer te geven van de momentopname van de netwerk- en voor dat punt in de tijd waarop het probleem heeft voorgedaan. 

 
![Van trendgrafieken](media/log-analytics-network-performance-monitor/trend-charts.png)
 

### <a name="topology-map"></a>Topologiekaart 

NPM toont de topologie hop door hop van routes tussen de bron- en doelserver-eindpunt op een interactieve topologiekaart. U kunt de topologiekaart weergeven door te klikken op de **topologie** tegel op het dashboard van oplossing of door te klikken op **weergave topologie** koppeling op de pagina inzoomen's.  

De topologiekaart toont het aantal routes tussen de bron- en doelserver en wat zijn de gegevenspakketten nemen paden. De latentie die is bijgedragen door elke hop netwerk wordt ook weergegeven. Alle paden die waarvoor de totale pad latentie hoger dan de drempelwaarde is (ingesteld in de desbetreffende regel bewaking) zijn in rood weergegeven.  

Als u een knooppunt of aanwijzen eroverheen op de topologiekaart klikt, ziet u de eigenschappen van het knooppunt als FQDN-naam en IP-adres. Klik op een hop om te zien van het IP-adres. U kunt de hop lastige netwerk identificeren door de latentie die is bijgedragen door deze wordt daar iets van merkt. U kunt bepaalde routes filteren met behulp van de filters in het deelvenster samenvouwbare actie. Ook kunt u netwerktopologieën vereenvoudigen door te verbergen van de tussenliggende hops met behulp van de schuifregelaar in het actiedeelvenster. U kunt inzoomen in of buiten de topologiekaart met behulp van het muiswiel. 

Houd er rekening mee dat de topologie die wordt weergegeven in de kaart laag 3-topologie is en bevat geen laag 2-apparaten en verbindingen. 

 
![Topologiekaart](media/log-analytics-network-performance-monitor/topology-map.png)
 

## <a name="log-analytics-search"></a>Log Analytics zoeken 

Alle gegevens die pagina's grafisch weergegeven via het dashboard NPM en inzoomen is ook beschikbaar in ingebouwd [logboekanalyse zoeken](log-analytics-log-search-new.md). U kunt uitvoeren interactieve analyses van gegevens in de opslagplaats, correleren van gegevens uit verschillende bronnen, aangepaste waarschuwingen maken, aangepaste weergaven maken en de gegevens exporteren naar Excel, Power BI of een deelbaar koppeling. Het gebied van de algemene query's in het dashboard heeft enkele nuttig query's die u als startpunt gebruiken kunt voor het maken van uw eigen query's en rapporten. 

 

## <a name="provide-feedback"></a>Feedback geven 

**UserVoice** -kunt u uw ideeën voor netwerk-Prestatiemeter-functies die u laten werken wilt op boeken. Ga naar onze [UserVoice pagina](https://feedback.azure.com/forums/267889-log-analytics/category/188146-network-monitoring). 

**Deelnemen aan onze cohort** -we altijd geïnteresseerd in nieuwe klanten ons cohort join hebben. Als onderdeel van het vroege toegang krijgen tot nieuwe functies en helpt ons Netwerkcontrole prestaties te verbeteren. Als u geïnteresseerd in lid te worden bent, invullen van dit [Snelle enquête](https://aka.ms/npmcohort). 

## <a name="next-steps"></a>Volgende stappen 
- Meer informatie over [Prestatiemeter](log-analytics-network-performance-monitor-performance-monitor.md), [Monitor voor Service-eindpunt](log-analytics-network-performance-monitor-performance-monitor.md), en [ExpressRoute-Monitor](log-analytics-network-performance-monitor-expressroute.md). 
