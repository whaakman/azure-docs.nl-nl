---
title: Prestatiemeter-oplossing in Azure-netwerk | Microsoft Docs
description: Network Performance Monitor in Azure kunt u de prestaties van uw netwerken, bijna in realtime, om te detecteren en zoek netwerkknelpunten controleren.
services: log-analytics
documentationcenter: ''
author: abshamsft
manager: carmonm
editor: ''
ms.assetid: 5b9c9c83-3435-488c-b4f6-7653003ae18a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 02/20/2018
ms.author: abshamsft
ms.openlocfilehash: 7f7fe545c10d80fc5a201923679b7d61623786fb
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53193787"
---
# <a name="network-performance-monitor-solution-in-azure"></a>Prestatiemeter-oplossing in Azure-netwerk

![Network Performance Monitor symbool](./media/network-performance-monitor/npm-symbol.png)


Network Performance Monitor is een hybride cloud-gebaseerde Netwerkcontrole-oplossing die helpt u bij het bewaken van prestaties van het netwerk tussen verschillende punten in uw netwerkinfrastructuur. Daarnaast kunt u hiermee de netwerkverbinding met de service en toepassingseindpunten bewaken, evenals de prestaties van Azure ExpressRoute. 

Network Performance Monitor detecteert netwerkproblemen, zoals verkeer blackholing, routering fouten en problemen die controlemethoden conventionele netwerk niet kan detecteren. De oplossing genereert waarschuwingen en waarschuwt u als een drempelwaarde voor een netwerkverbinding wordt overschreden. Bovendien worden problemen met de netwerkprestaties tijdig gedetecteerd en wordt de oorzaak van het probleem op een bepaald netwerksegment of apparaat opgespoord. 

Network Performance Monitor biedt drie brede mogelijkheden: 

* [Prestatiemeter](network-performance-monitor-performance-monitor.md): U kunt de netwerkconnectiviteit controleren voor cloudimplementaties en on-premises locaties, meerdere datacenters en filialen en met meerdere lagen, essentiële toepassingen of microservices. Met Prestatiemeter, kunt u netwerkproblemen detecteren voordat gebruikers klagen.

* [Monitor voor connectiviteit](network-performance-monitor-service-endpoint.md): U kunt de connectiviteit van uw gebruikers controleren met de services die u interesseren, bepalen welke infrastructuur is in het pad en identificeren waar netwerkknelpunten optreden. U kunt informatie over storingen voordat uw gebruikers weten en ziet de exacte locatie van de problemen binnen het netwerkpad. 

    Deze mogelijkheid kunt u op basis van HTTP, HTTPS, TCP- en ICMP om te controleren in het verleden of in realtime de beschikbaarheid en reactietijd van uw service uit te voeren. U kunt ook de bijdrage van het netwerk in pakketverlies en latentie controleren. Met een netwerk-topologie-kaart, kunt u network vertragingen isoleren. U kunt identificeren probleem punten die op het netwerkpad van het knooppunt naar de service, met latentiegegevens voor elke hop plaatsvinden. Met ingebouwde tests kunt u de netwerkverbinding met Office 365 en Dynamics CRM zonder eventuele meteen controleren. Met deze functie kunt u de netwerkverbinding met een TCP-compatibele eindpunt, zoals websites, SaaS-toepassingen, PaaS-toepassingen en SQL-databases bewaken.

* [ExpressRoute Monitor](network-performance-monitor-expressroute.md): End-to-end-connectiviteit en -prestaties controleren tussen uw filialen en Azure, via Azure ExpressRoute.  

Meer informatie over de verschillende mogelijkheden ondersteund door [Network Performance Monitor](https://docs.microsoft.com/azure/networking/network-monitoring-overview) is online beschikbaar.
 
## <a name="supported-regions"></a>Ondersteunde regio 's
NPM kunt Controleer de connectiviteit tussen netwerken en toepassingen in een deel van de hele wereld, uit een werkruimte die wordt gehost in een van de volgende regio's:
* Europa -west
* US - west-centraal
* US - oost
* Japan - oost
* Azië - zuidoost
* Zuid-Oost-Australië
* Zuid-groot-Brittannië
* US Government Virginia

De lijst met ondersteunde regio's voor ExpressRoute-bewaking is beschikbaar in de [documentatie](https://docs.microsoft.com/azure/expressroute/how-to-npm?utm_swu=8117#regions).


## <a name="set-up-and-configure"></a>Instellen en configureren

### <a name="install-and-configure-agents"></a>Agents installeren en configureren 

Gebruik de basic-processen voor het installeren van agents op [verbinding maken met Windows-computers met Azure Log Analytics](../../azure-monitor/platform/om-agents.md) en [Operations Manager verbinden met Log Analytics](../../azure-monitor/platform/om-agents.md).

### <a name="where-to-install-the-agents"></a>Waar u de agents te installeren 

* **Prestatiemeter**: Log Analytics-agents op ten minste één knooppunt dat is verbonden met elk subnet van waaruit u wilt bewaken van de netwerkverbinding met andere subnetwerken installeren.

    Voor het controleren van een netwerkverbinding, agents te installeren op beide eindpunten van de koppeling. Als u over de topologie van uw netwerk twijfelt, moet u de agents installeren op servers met essentiële workloads tussen wie u wilt bewaken van de prestaties van het netwerk. Bijvoorbeeld, als u controleren van de netwerkverbinding tussen een webserver en een server met SQL wilt, een agent installeren op beide servers. Agents bewaken netwerkverbinding (koppelingen) tussen de hosts, niet de hosts zelf. 

* **Monitor voor connectiviteit**: Een Log Analytics-agent installeren op elk knooppunt van waaruit u wilt bewaken van de netwerkverbinding met het service-eindpunt. Een voorbeeld is als u wilt bewaken van verbinding met het netwerk op Office 365 van uw office-sites met het label O1, O2 en O3. De Log Analytics-agent installeren op ten minste één knooppunt in O1, O2 en O3. 

* **ExpressRoute Monitor**: Ten minste één Log Analytics-agent installeren in uw Azure-netwerk. Ten minste één agent ook installeren in uw on-premises subnetwerk die is verbonden via ExpressRoute-privépeering.  

### <a name="configure-log-analytics-agents-for-monitoring"></a>Log Analytics-agents configureren voor bewaking 

Network Performance Monitor maakt gebruik van synthetische transacties voor het bewaken van prestaties van het netwerk tussen de bron- en agents. U kunt kiezen tussen TCP- en ICMP als protocol voor de bewaking in Prestatiemeter en Connectiviteitsmonitor voor Service-mogelijkheden. Alleen TCP is beschikbaar als het controle-protocol voor ExpressRoute-bewaking. Zorg ervoor dat de firewall communicatie tussen de Log Analytics-agents die worden gebruikt voor het bewaken van het protocol dat u toestaat. 

* **TCP-protocol**: Als u TCP als protocol voor bewaking, opent u de firewallpoort voor agents die worden gebruikt voor Network Performance Monitor en ExpressRoute-bewaking om ervoor te zorgen dat de agents met elkaar kunnen verbinden. Uitvoeren als u wilt de poort te openen, de [EnableRules.ps1](https://aka.ms/npmpowershellscript) PowerShell-script zonder parameters in een PowerShell-venster met beheerdersbevoegdheden.

    Het script maakt registersleutels die door de oplossing nodig. Het maakt ook Windows Firewall-regels om toe te staan van agents op TCP-verbindingen met elkaar maken. De registersleutels die zijn gemaakt door het script opgeven of de logboeken voor foutopsporing en het pad naar het bestand zich aanmeldt. Het script wordt ook gebruikt voor communicatie van agent-TCP-poort. De waarden voor deze sleutels worden automatisch ingesteld door het script. Deze sleutels niet handmatig te wijzigen. De poort die standaard geopend is 8084. U kunt een aangepaste poort gebruiken door op te geven van de parameter-poortnummer voor het script. Gebruik dezelfde poort op alle computers waarop het script wordt uitgevoerd. 

    >[!NOTE]
    > Het script configureert alleen Windows Firewall lokaal. Als u de firewall van een netwerk hebt, zorg ervoor dat het verkeer dat bestemd is voor de TCP-poort die wordt gebruikt door Network Performance Monitor geeft.

    >[!NOTE]
    > U hoeft te worden uitgevoerd de [EnableRules.ps1](https://aka.ms/npmpowershellscript ) PowerShell-script voor Service-Eindpuntbewaking.

    

* **ICMP-protocol**: Als u ICMP als het protocol voor het bewaken van kiest, schakelt u de volgende firewallregels op betrouwbare wijze gebruikmaken van ICMP:
    
   ```
   netsh advfirewall firewall add rule name="NPMDICMPV4Echo" protocol="icmpv4:8,any" dir=in action=allow 
   netsh advfirewall firewall add rule name="NPMDICMPV6Echo" protocol="icmpv6:128,any" dir=in action=allow 
   netsh advfirewall firewall add rule name="NPMDICMPV4DestinationUnreachable" protocol="icmpv4:3,any" dir=in action=allow 
   netsh advfirewall firewall add rule name="NPMDICMPV6DestinationUnreachable" protocol="icmpv6:1,any" dir=in action=allow 
   netsh advfirewall firewall add rule name="NPMDICMPV4TimeExceeded" protocol="icmpv4:11,any" dir=in action=allow 
   netsh advfirewall firewall add rule name="NPMDICMPV6TimeExceeded" protocol="icmpv6:3,any" dir=in action=allow 
   ```
 

### <a name="configure-the-solution"></a>De oplossing configureren 

1. De oplossing Netwerkprestatiemeter toevoegen aan uw werkruimte uit de [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.NetworkMonitoringOMS?tab=Overview). Ook kunt u de procedure beschreven in [toevoegen Log Analytics-oplossingen uit de galerie van oplossingen](../../azure-monitor/insights/solutions.md). 
2. Open uw Log Analytics-werkruimte en selecteer de **overzicht** tegel. 
3. Selecteer de **Network Performance Monitor** tegel met het bericht *oplossing is aanvullende configuratie vereist*.

   ![Network Performance Monitor tegel](media/network-performance-monitor/npm-config.png)

4. Op de **Setup** pagina, ziet u de optie voor Log Analytics-agents installeren en configureren van de agents voor bewaking in de **algemene instellingen die u** weergeven. Als eerder uiteengezet, als u geïnstalleerd en geconfigureerd Log Analytics-agents, selecteert u de **Setup** weergeven om te configureren, de mogelijkheid die u wilt gebruiken. 

   **Prestatiemeter**: Kies het protocol wilt gebruiken voor synthetische transacties in de **standaard** Prestatiemeter regel en selecteer **opslaan en doorgaan**. Deze Protocolselectie bevat alleen voor de standaardregel voor het systeem gegenereerde. U moet kiezen het protocol telkens wanneer die u een regel Prestatiemeter expliciet maken. U kunt altijd verplaatsen naar de **standaard** regel instellingen op de **Prestatiemeter** tabblad (dit wordt weergegeven na het voltooien van de configuratie van de dag 0) en wijzig het protocol later opnieuw. Als u de rPerfomance Monitor mogelijkheid niet wilt, kunt u de standaardregel van uitschakelen de **standaard** regel instellingen op de **Prestatiemeter** tabblad.

   ![Monitor voor weergave van agentprestaties](media/network-performance-monitor/npm-synthetic-transactions.png)
    
   **Monitor voor connectiviteit**: De mogelijkheid biedt ingebouwde vooraf geconfigureerde tests voor het controleren van de netwerkverbinding met Office 365 en Dynamics 365 van uw agents. Kies de Office 365 en Dynamics 365-services die u wenst te bewaken door de selectievakjes ernaast in te schakelen. Als u de agents van waaruit u wilt controleren, selecteert u **Agents toevoegen**. Als u niet wilt dat met deze mogelijkheid of later instellen, geen Kies iets en selecteer **opslaan en doorgaan**.

   ![Service-Eindpuntbewaking weergeven](media/network-performance-monitor/npm-service-endpoint-monitor.png)

   **ExpressRoute Monitor**: Selecteer **nu detecteren** voor het detecteren van alle ExpressRoute particuliere peerings die zijn verbonden met de virtuele netwerken in het Azure-abonnement is gekoppeld met deze Log Analytics-werkruimte. 

   >[!NOTE] 
   > De oplossing wordt momenteel alleen ExpressRoute privé-peerings. 

   >[!NOTE] 
   > Alleen worden persoonlijke peerings die zijn verbonden met de virtuele netwerken die zijn gekoppeld aan het abonnement is gekoppeld met deze Log Analytics-werkruimte gedetecteerd. Als ExpressRoute is verbonden met virtuele netwerken buiten het abonnement dat is gekoppeld aan deze werkruimte, moet u een Log Analytics-werkruimte maken in deze abonnementen. Network Performance Monitor gebruiken voor het bewaken van deze peerings.

   ![ExpressRoute Monitor weergeven](media/network-performance-monitor/npm-express-route.png)

   Nadat de detectie is voltooid, worden de gedetecteerde privé-peerings worden weergegeven in een tabel. 

   ![De pagina configuratie van netwerkprestaties netwerk](media/network-performance-monitor/npm-private-peerings.png)
    
De bewaking voor deze peerings is in eerste instantie een uitgeschakelde status. Selecteer elke peering die u wilt bewaken en controle voor deze uit de detailweergave aan de rechterkant configureren. Selecteer **opslaan** aan de configuratie op te slaan. Zie het artikel "Configureren ExpressRoute-controle" voor meer informatie. 

Nadat de installatie is voltooid, wordt het 30 minuten duurt een uur voor de gegevens om in te vullen. Terwijl de oplossing gegevens van uw netwerk verzamelt, ziet u het bericht *oplossing is aanvullende configuratie vereist* op de Network Performance Monitor **overzicht** tegel. Nadat de gegevens worden verzameld en in een index, de **overzicht** tegel wordt gewijzigd en informeert u over de netwerkstatus van uw in een samenvatting. Vervolgens kunt u de bewaking van de knooppunten op welke Log Analytics-agents zijn geïnstalleerd, evenals de subnetten van uw omgeving gedetecteerd.

#### <a name="edit-monitoring-settings-for-subnets-and-nodes"></a>Controle-instellingen voor subnetten en knooppunten bewerken 

Alle subnetten waarop ten minste één agent is geïnstalleerd, worden vermeld op de **subnetwerken** tabblad op de configuratiepagina. 


Inschakelen of uitschakelen van de bewaking van bepaalde subnetwerken:

1. Selecteer of schakel het selectievakje in naast de **subnetwerk-ID**. Zorg ervoor dat **gebruiken voor bewaking** is ingeschakeld of uitgeschakeld, indien van toepassing. U kunt selecteren of wissen van meerdere subnetten. Als uitgeschakeld, subnetwerken zijn niet bewaakt en de agents worden bijgewerkt om te stoppen met het pingen van andere agents. 
2. Kies de knooppunten die u wilt bewaken in een bepaald subnet. Selecteer het subnet in de lijst en de vereiste knooppunten verplaatsen tussen de lijsten die niet-beheerde en bewaakte knooppunten bevatten. U kunt een aangepaste beschrijving toevoegen aan het subnet.
3. Selecteer **opslaan** aan de configuratie op te slaan. 

#### <a name="choose-nodes-to-monitor"></a>Kies knooppunten om te controleren

Alle knooppunten waarop een agent geïnstalleerd op deze worden weergegeven op de **knooppunten** tabblad. 

1. Selecteer of wis de knooppunten die u wilt bewaken of stoppen van de bewaking. 
2. Selecteer **gebruiken voor bewaking**, in- of uitschakelen, waar nodig. 
3. Selecteer **Opslaan**. 


De mogelijkheden die u wilt configureren:

- [Prestatiemeter](network-performance-monitor-performance-monitor.md#configuration)
- [Service-Eindpuntbewaking](network-performance-monitor-performance-monitor.md#configuration)
- [ExpressRoute-bewaking](network-performance-monitor-expressroute.md#configuration)

 

## <a name="data-collection-details"></a>Details van de verzameling gegevens
Network Performance Monitor gebruikt TCP SYN-SYNACK-ACK handshake pakketten verlies en latentie om informatie te verzamelen, als u kiest voor TCP als protocol. Network Performance Monitor maakt gebruik van ICMP-ECHO ICMP-ECHOANTWOORD als u ICMP als protocol kiest. Route traceren wordt ook gebruikt voor het ophalen van informatie over de topologie.

De volgende tabel bevat de methoden voor het verzamelen van gegevens en andere informatie over hoe gegevens worden verzameld voor Network Performance Monitor.

| Platform | Agent toewijzen | System Center Operations Manager-agent | Azure Storage | Operations Manager vereist? | Operations Manager-agent gegevens verzonden via de beheergroep | Verzamelingsfrequentie |
| --- | --- | --- | --- | --- | --- | --- |
| Windows | &#8226; | &#8226; |  |  |  |TCP-aantal-handshakes/ICMP-echoberichten om de vijf seconden gegevens verzonden om de 3 minuten |
 

 
De oplossing maakt gebruik van synthetische transacties om de status van het netwerk te beoordelen. Log Analytics-agents geïnstalleerd op verschillende momenten in de netwerkpakketten exchange TCP of ICMP Echo met elkaar. Of de agents pakketten TCP of ICMP Echo gebruiken, is afhankelijk van het protocol dat u hebt geselecteerd voor bewaking. In het proces meer agents traject tijd en pakket verlies, indien van toepassing. Elke agent voert ook periodiek een route traceren naar andere agents op de verschillende routes vinden in het netwerk dat moet worden getest. Met deze gegevens, kunnen de agents deduceren de netwerklatentie en pakket verliescijfers. De tests worden herhaald om de vijf seconden. Gegevens worden samengevoegd voor ongeveer drie minuten door de agents vóór deze geüpload naar de Log Analytics-service.



>[!NOTE]
> Hoewel vaak agents met elkaar communiceren, geen ze significant netwerkverkeer genereren tijdens de tests uitvoeren. Agents Vertrouw alleen op TCP SYN-SYNACK-ACK handshake-pakketten om het verlies en latentie te bepalen. Er worden geen gegevenspakketten worden uitgewisseld. Tijdens dit proces communiceren agents met elkaar alleen wanneer dat nodig is. De topologie van de communicatie agent is geoptimaliseerd om netwerkverkeer te verminderen.

## <a name="use-the-solution"></a>De oplossing gebruiken 

### <a name="network-performance-monitor-overview-tile"></a>Network Performance Monitor overzichtstegel 

Nadat u de oplossing Netwerkprestatiemeter hebt ingeschakeld, wordt de oplossing tegel op de **overzicht** pagina geeft een kort overzicht van de netwerkstatus. 

 ![Network Performance Monitor overzichtstegel](media/network-performance-monitor/npm-overview-tile.png)

### <a name="network-performance-monitor-dashboard"></a>Network Performance Monitor-dashboard 

* **Belangrijkste Netwerkstatusgebeurtenissen**: Deze pagina bevat een lijst van de meest recente health-gebeurtenissen en waarschuwingen in het systeem en de tijd sinds de gebeurtenissen actief zijn geweest. Een statusgebeurtenis of een waarschuwing wordt gegenereerd telkens wanneer de waarde van de gekozen metriek (verlies, latentie, reactietijd of bandbreedtegebruik) voor de bewaking regel de drempelwaarde overschrijdt. 

* **ExpressRoute Monitor**: Deze pagina biedt samenvattingen van de status voor de verschillende ExpressRoute-peeringverbindingen de oplossing monitors. De **topologie** tegel toont het aantal netwerkpaden via de ExpressRoute-circuits die worden bewaakt in uw netwerk. Selecteer deze tegel naar de **topologie** weergeven.

* **Monitor voor connectiviteit**: Deze pagina biedt samenvattingen van de status voor de verschillende tests die u hebt gemaakt. De **topologie** tegel ziet u het aantal eindpunten die worden bewaakt. Selecteer deze tegel naar de **topologie** weergeven.

* **Prestatiemeter**: Deze pagina vindt u statusoverzichten voor de **netwerk** koppelingen en **subnetwerk** koppelingen waarmee de oplossing worden gecontroleerd. De **topologie** tegel toont het aantal paden die worden bewaakt in uw netwerk. Selecteer deze tegel naar de **topologie** weergeven. 

* **Algemene query's**: Deze pagina bevat een set van zoekquery's ophalen van onbewerkte netwerk door gegevens rechtstreeks te controleren. U kunt deze query's als uitgangspunt gebruiken om uw eigen query's voor aangepaste rapportage maken. 

   ![Network Performance Monitor-dashboard](media/network-performance-monitor/npm-dashboard.png)

 

### <a name="drill-down-for-depth"></a>Inzoomen op diepte 

U kunt verschillende koppelingen op het dashboard van de oplossing om in te zoomen naar beneden dieper in elk gebied van belang zijn. Bijvoorbeeld, wanneer u een waarschuwing of een slechte netwerkverbinding weergegeven op het dashboard ziet, selecteert u deze voor verder onderzoek. Een pagina geeft een lijst van alle subnetwerkkoppelingen voor de koppeling bepaald netwerk. U ziet de winst, latentie en status van elke subnetwerkkoppeling. U vindt snel informatie welke subnetwerkkoppeling problemen zorgt. Selecteer **knooppuntkoppelingen weergeven** om te zien van alle knooppuntkoppelingen voor de koppeling niet in orde subnet. Vervolgens kunt u afzonderlijke koppelingen van knooppunt-naar-knooppunt en de slechte knooppuntkoppelingen gevonden. 

Selecteer **topologie weergeven** om weer te geven van de hop-by-hop '-topologie van de routes tussen de bron- als doelknooppunten. De routes niet in orde weergegeven in rood. Hier vindt u de latentie bijgedragen door elke hop zodat u snel het probleem naar een bepaald deel van het netwerk kunt identificeren.

 

### <a name="network-state-recorder-control"></a>Controle van de staat van de netwerk-Recorder

Elke weergave bevat een momentopname van de netwerkstatus van uw op een bepaald punt in tijd. Standaard zijn de meest recente status wordt weergegeven. De balk boven aan de pagina ziet het punt in tijd waarvoor de status wordt weergegeven. Als u een momentopname van de netwerkstatus van uw op een eerder tijdstip, selecteer **acties**. U kunt ook inschakelen of uitschakelen van automatisch vernieuwen voor alle pagina's, terwijl u de meest recente status weergeven. 

 ![Status van de netwerk-Recorder](media/network-performance-monitor/network-state-recorder.png)

 

### <a name="trend-charts"></a>Trendgrafieken 

Op elk niveau die u kunt inzoomen, ziet u de trend van de toepasselijke metrische gegevens. Verlies, latentie, reactietijd of bandbreedtegebruik kan het zijn. Als u wilt wijzigen van het tijdsinterval voor de trend, gebruik het besturingselement tijd aan de bovenkant van de grafiek. 

Trendgrafieken ziet u een historisch perspectief van de prestaties van een metrische gegevens voor prestaties. Sommige netwerkproblemen tijdelijk aard zijn en moeilijk te vangen door te kijken alleen de huidige status van het netwerk. Problemen kunnen snel het oppervlak en verdwijnen voordat iemand meldingen, alleen op een later tijdstip weer in de tijd. Dergelijke tijdelijke problemen kunnen ook lastig zijn voor beheerders. De problemen zich vaak weergegeven als onverklaarbare verhogingen in de reactietijd van toepassing, zelfs als alle onderdelen van de toepassing worden weergegeven aan het probleemloos werkt. 

Eenvoudig kunt u dit soort problemen detecteren door te kijken op een trendgrafiek. Het probleem wordt weergegeven als een plotselinge piek in de netwerklatentie of pakketverlies. U kunt het probleem onderzoeken door door het besturingselement netwerk staat Recorder te gebruiken om weer te geven van de momentopname van het netwerk en de topologie voor dat punt in de tijd waarop het probleem is opgetreden.

 
![Trendgrafieken](media/network-performance-monitor/trend-charts.png)
 

### <a name="topology-map"></a>Topologie-kaart 

Network Performance Monitor ziet u de hop-by-hop '-topologie van routes tussen de bron- en eindpunt op een interactieve topologiekaart weer. Als u wilt weergeven van de topologie-kaart, selecteer de **topologie** tegel op het dashboard van de oplossing. U kunt ook selecteren de **topologie weergeven** koppeling op de Inzoomen op pagina's. 

De topologie-kaart wordt weergegeven hoeveel routes zijn tussen de bron en doel- en wat de gegevenspakketten nemen paden. De latentie bijgedragen door elke hop netwerk wordt ook weergegeven. Alle paden waarop de latentie van de totale pad hoger dan de drempelwaarde is (ingesteld in de bijbehorende bewakingsregel) worden weergegeven in het rood. 

Wanneer u een knooppunt of plaats de muisaanwijzer over het op de kaart topologie selecteert, ziet u de eigenschappen van de knooppunten, zoals de FQDN-naam en IP-adres. Selecteer een hop om te zien van het IP-adres. U kunt de lastige netwerk hop identificeren dat u de latentie het draagt bij. Bepaalde om routes te filteren, door de filters te gebruiken in het deelvenster samenvouwbare actie. Ter vereenvoudiging van de netwerktopologieën verbergen de tussenliggende hops met behulp van de schuifregelaar in het actiedeelvenster. U kunt inzoomen of uitzoomen buiten de topologie-kaart met het muiswiel. 

De topologie die wordt weergegeven in de kaart is laag-3-topologie en bevat geen apparaten op laag 2 en verbindingen. 

 
![Topologie-kaart](media/network-performance-monitor/topology-map.png)
 

## <a name="log-analytics-search"></a>Analytics voor zoeken in Logboeken 

Alle gegevens die wordt blootgesteld grafisch weergegeven via het dashboard Network Performance Monitor en inzoomen op pagina's zijn ook beschikbaar in [zoeken in Log Analytics](../../azure-monitor/log-query/log-query-overview.md). U kunt het uitvoeren van interactieve analyses van gegevens in de opslagplaats en correleren van gegevens uit verschillende bronnen. U kunt ook aangepaste waarschuwingen en weergaven maken en de gegevens exporteren naar Excel, Power BI, of een deelbaar koppeling. De **algemene query's** gebied in het dashboard heeft enkele handige query's die u als startpunt gebruiken kunt om te maken van uw eigen query's en rapporten. 

## <a name="alerts"></a>Waarschuwingen

Network Performance Monitor maakt gebruik van de waarschuwingen mogelijkheden van [Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts).

Dit betekent dat alle meldingen worden beheerd met behulp van [actiegroepen](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups#overview).  

Als u een NPM-gebruiker die het maken van een waarschuwing via Log Analytics: 
1. Hier ziet u een koppeling waarmee u wordt omgeleid naar Azure-Portal. Klik hierop om de toegang tot de portal.
2. Klik op de tegel Network Performance Monitor. 
3. Ga om te configureren.  
4. Selecteer de test die u wilt maken van een waarschuwing op en volg de hieronder vermelde stappen.

Als u een NPM-gebruiker die het maken van een waarschuwing via Azure Portal:  
1. U kunt uw e-mailbericht rechtstreeks invoeren of u kunt kiezen om waarschuwingen via actiegroepen te maken.
2. Als u ervoor kiest uw e-mailbericht rechtstreeks invoeren, een actie met de naam van de groep **NPM e ActionGroup** wordt gemaakt en de e-mail-id wordt toegevoegd aan deze actiegroep.
3. Als u ervoor actiegroepen gebruiken kiest, moet u een eerder gemaakte actiegroep selecteren. U kunt informatie over het maken van een actiegroep [hier.](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups#create-an-action-group-by-using-the-azure-portal) 
4. Als de waarschuwing is gemaakt, kunt u waarschuwingen beheren koppeling gebruiken om uw waarschuwingen te beheren. 

Telkens wanneer die u een waarschuwing maakt maakt NPM een query op basis van waarschuwingsregel in Azure Monitor. Deze query is triggerred om de standaard 5 minuten. Met Azure monitor niet in rekening voor de eerste 250 waarschuwingsregels die zijn gemaakt aanmelden en geen waarschuwingsregels bovenstaande beperken van de 250 logboekwaarschuwingsregels wordt gefactureerd volgens [waarschuwingen in Azure Monitor-pagina met prijzen prijzen](https://azure.microsoft.com/en-us/pricing/details/monitor/).
Meldingen worden afzonderlijk in rekening gebracht volgens [meldingen in Azure Monitor pricig pagina prijzen](https://azure.microsoft.com/en-us/pricing/details/monitor/).


## <a name="pricing"></a>Prijzen

Informatie over prijzen vindt u [online](network-performance-monitor-pricing-faq.md).

## <a name="provide-feedback"></a>Feedback geven 

* **UserVoice:** U kunt uw ideeën voor functies van Network Performance Monitor die u laten werken wilt op plaatsen. Ga naar de [UserVoice-pagina](https://feedback.azure.com/forums/267889-log-analytics/category/188146-network-monitoring). 

* **Deelnemen aan onze cohort:** We zijn altijd geïnteresseerd hoeven nieuwe klanten die deelnemen aan onze cohort. Als onderdeel van deze krijgt u vroege toegang tot nieuwe functies en de mogelijkheid om u te helpen bij het verbeteren van Network Performance Monitor. Als u toevoegen wilt, vult u dit [korte enquête](https://aka.ms/npmcohort). 

## <a name="next-steps"></a>Volgende stappen 
Meer informatie over [Prestatiemeter](network-performance-monitor-performance-monitor.md), [Service Connectiviteitsmonitor](network-performance-monitor-performance-monitor.md), en [ExpressRoute Monitor](network-performance-monitor-expressroute.md). 
