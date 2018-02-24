---
title: Netwerk-Prestatiemeter-oplossing in Azure Log Analytics | Microsoft Docs
description: De ExpressRoute-Manager-functie in Prestatiemeter netwerk kunt u end-to-end-connectiviteit en de prestaties tussen uw filialen en Azure controleren via Azure ExpressRoute.
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
ms.openlocfilehash: 405bcd7d69e93f838d35b61be489464fcf55ee88
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2018
---
# <a name="expressroute-manager"></a>ExpressRoute Manager

De ExpressRoute-Manager-functie in [netwerk Prestatiemeter](log-analytics-network-performance-monitor.md) kunt u end-to-end-connectiviteit en prestaties tussen uw filialen en Azure bewaken via Azure ExpressRoute. Belangrijkste voordelen zijn: 

- Automatische detectie van ExpressRoute-circuits die zijn gekoppeld aan uw abonnement 
- Bijhouden van gebruik van de bandbreedte, verlies en latentieperiode circuit, peering en VNet-niveau voor uw ExpressRoute 
- Detectie van de netwerktopologie van uw ExpressRoute-circuits 

![ExpressRoute-Monitor](media/log-analytics-network-performance-monitor/expressroute-intro.png)

## <a name="configuration"></a>Configuratie 
Open de configuratie voor netwerk-Prestatiemeter te openen de [netwerk Prestatiemeter oplossing](log-analytics-network-performance-monitor.md) en klik op de **configureren** knop.

### <a name="configure-nsg-rules"></a>NSG-regels configureren 
Voor de servers in Azure die worden gebruikt voor de bewaking via NPM, moet u configureren (NSG) netwerkbeveiligingsgroepen voor TCP-verkeer op de poort die wordt gebruikt door NPM van de synthetische transacties. De standaardpoort is 8084. Hierdoor kan de OMS-agent geïnstalleerd op de virtuele machine van Azure om te communiceren met een on-premises monitoring agent. 

Zie voor meer informatie over het NSG [Netwerkbeveiligingsgroepen](../virtual-network/virtual-networks-create-nsg-arm-pportal.md). 

>[!NOTE]
> Zorg ervoor dat u de agents (zowel de lokale server-agent en de Azure-server-agent) hebt geïnstalleerd en het EnableRules.ps1 PowerShell-script voordat u doorgaat met deze stap hebt uitgevoerd. 

 
### <a name="discover-expressroute-peering-connections"></a>ExpressRoute-Peering verbindingen detecteren 
 
1. Klik op de **ExpressRoute Peerings** weergeven.  
2. Klik op de **nu detecteren** knop voor het detecteren van alle persoonlijke peerings die zijn verbonden met de VNETs die in de Azure-abonnement is gekoppeld aan deze werkruimte voor logboekanalyse ExpressRoute.  

>[!NOTE]  
> De oplossing wordt momenteel alleen persoonlijke ExpressRoute peerings detecteert. 

>[!NOTE]  
> Alleen deze persoonlijke peerings gedetecteerd die zijn verbonden met de VNETs die zijn gekoppeld aan het abonnement is gekoppeld aan deze werkruimte voor logboekanalyse. Als uw ExpressRoute is verbonden met VNETs buiten het abonnement is gekoppeld aan deze werkruimte, moet u voor het maken van een werkruimte voor logboekanalyse in deze abonnementen en NPM gebruiken voor het bewaken van deze peerings. 

 ![ExpressRoute-Monitor configureren](media/log-analytics-network-performance-monitor/expressroute-configure.png)
 
 Zodra de detectie voltooid is, wordt de gedetecteerde persoonlijke peering verbindingen worden weergegeven in een tabel. De bewaking voor deze peerings wordt in eerste instantie worden uitgeschakeld. 

### <a name="enable-monitoring-of-the-expressroute-peering-connections"></a>Schakel de bewaking van de peering ExpressRoute-verbindingen 

1. Klik op de persoonlijke peering waarmee u verbinding maakt bent geïnteresseerd in de bewaking.  
2. Klik in het deelvenster RHS op het selectievakje voor **bewaken van deze Peering**. 
3. Als u van plan bent de gezondheid van gebeurtenissen voor deze verbinding te maken, moet u controleren **statuscontrole inschakelen voor deze peering**. 
4. Kies de bewaking van de voorwaarden. U kunt aangepaste drempelwaarden instellen voor het genereren van gebeurtenis health door drempelwaarden in te voeren. Wanneer de waarde van de voorwaarde hoger dan de geselecteerde drempelwaarde voor de peering verbinding is, wordt een health-gebeurtenis gegenereerd. 
5. Klik op **Agents toevoegen** de bewaking agents die u gebruiken wilt voor het bewaken van deze peering verbinding kiezen. U moet ervoor zorgen dat u agents aan beide uiteinden van de verbinding, ten minste één agent in het Azure VNET dat is verbonden toevoegen met deze peering evenals ten minste één lokale agent verbonden met deze peering. 
6. Klik op **opslaan** aan de configuratie op te slaan. 

![ExpressRoute-Monitor configureren](media/log-analytics-network-performance-monitor/expressroute-configure-discovery.png)


Na het inschakelen van de regels en selecteren van de waarden en de agents die u wilt bewaken, is een wachttijd van ongeveer 30 tot 60 minuten voor de waarden om te beginnen in te vullen en de **ExpressRoute bewaking** tegels beschikbaar. Zodra u de bewaking tegels ziet, zijn uw ExpressRoute-circuits en verbindingsbronnen worden bewaakt door NPM. 

>[!NOTE]
> Deze mogelijkheid werkt betrouwbare op werkruimten die u hebt bijgewerkt naar de nieuwe querytaal.  

## <a name="walkthrough"></a>Walkthrough 

Het dashboard Netwerkbewaking voor prestaties geeft een overzicht van de status van het ExpressRoute-circuits en peering verbindingen. 

![NPM Dashboard ExpressRoute](media/log-analytics-network-performance-monitor/npm-dashboard-expressroute.png) 

### <a name="circuits-list"></a>Circuits lijst 

Een lijst van alle bewaakte ExpressRoute-circuits wilt bekijken, klikt u op de tegel van ExpressRoute-circuits. U kunt een circuit selecteren en weergeven van de status, trendgrafieken voor pakketverlies, bandbreedte en latentie. De grafieken, zijn interactief. U kunt een aangepaste tijdvenster voor het uitzetten van de grafieken selecteren. Sleep de muis over een gebied op de grafiek inzoomen en fijnmazig gegevenspunten bekijken. 

![ExpressRoute-Circuits lijst](media/log-analytics-network-performance-monitor/expressroute-circuits.png) 

### <a name="trend-of-loss-latency-and-throughput"></a>Trend van gegevensverlies, latentie en doorvoer 

De grafieken bandbreedte, latentie en gegevensverlies, zijn interactief. U kunt inzoomen in een willekeurig gedeelte van deze grafieken met behulp van besturingselementen van de muis. U ziet ook de bandbreedte, latentie en van verliesgegevens voor andere intervallen door te klikken op de datum/tijd, zich onder de knop acties in de linkerbovenhoek. 

![ExpressRoute-latentie](media/log-analytics-network-performance-monitor/expressroute-latency.png) 

### <a name="peerings-list"></a>Lijst van Peerings 

Klik op de **persoonlijke Peerings** tegel op het dashboard wordt een lijst weergegeven van alle verbindingen met virtuele netwerken via persoonlijke peering. Hier kunt u een virtuele netwerkverbinding en bekijk de status, trendgrafieken voor pakketverlies, bandbreedte en latentie. 

![ExpressRoute-Peerings](media/log-analytics-network-performance-monitor/expressroute-peerings.png) 

### <a name="circuit-topology"></a>Circuit-topologie 

Voor het circuit topologie weergeven, klikt u op de **topologie** tegel. Hiermee gaat u naar de Topologieweergave van de geselecteerde circuit of peering. Diagram van de topologie biedt de latentie voor elk segment in het netwerk en elke laag 3-hop wordt vertegenwoordigd door een knooppunt van het diagram. Op een hop te klikken, geeft meer informatie over de hop. U kunt het niveau van zichtbaarheid lokale hops opnemen door de onderstaande schuifknop te verplaatsen verhogen **Filters**. De schuifregelaar naar links of rechts verplaatsen, verhoogt/verlaagt het aantal hops in de grafiek topologie. De latentie tussen elk segment is zichtbaar, waardoor voor snellere isolatie van hoge latentie segmenten in uw netwerk. 

![ExpressRoute-topologie](media/log-analytics-network-performance-monitor/expressroute-topology.png)  

### <a name="detailed-topology-view-of-a-circuit"></a>Gedetailleerde weergave van de topologie van een circuit 

Deze weergave toont de VNet-verbindingen. 

![ExpressRoute-VNet](media/log-analytics-network-performance-monitor/expressroute-vnet.png)
 

### <a name="diagnostics"></a>Diagnostiek 

Prestatiemeter netwerk kunt u verschillende circuit Verbindingsproblemen opsporen. Enkele van de problemen worden hieronder weergegeven. 

**Circuit is niet beschikbaar.** NPM waarschuwt u als de connectiviteit tussen uw lokale bronnen en Azure VNETs verloren gegaan is. Dit helpt u proactieve maatregelen nemen voordat u kunt ontvangen escalaties van de gebruiker en de uitvaltijd te verminderen 

![ExpressRoute-Circuit omlaag](media/log-analytics-network-performance-monitor/expressroute-circuit-down.png)
 

**Verkeer niet via de beoogde circuit.** NPM kunt u een melding wanneer het verkeer is onverwacht niet door de beoogde ExpressRoute-circuit stroomt. Dit kan gebeuren als het circuit niet actief is en het verkeer stroomt via de back-route of als er een probleem met de routering. Deze informatie helpt u proactief eventuele configuratieproblemen in uw routeringsbeleid beheren en zorg dat de meest optimaal en veilig route wordt gebruikt. 

 

**Verkeer niet via de primaire circuit.** De mogelijkheid waarschuwt u als het verkeer via het secundaire ExpressRoute-circuit stroomt. Hoewel u zult eventuele problemen met de netwerkverbinding niet in dit geval ervaren, maar proactief voor probleemoplossing de problemen met het primaire circuit u beter voorbereid laat. 

 
![ExpressRoute-netwerkverkeer](media/log-analytics-network-performance-monitor/expressroute-traffic-flow.png)


**Afnemen als gevolg van piekgebruik neemt.** U kunt de trend in het gebruik bandbreedte met de latentie trend te bepalen of de afname van de Azure-workload vanwege een piek in de bandbreedte of niet is correleert en onderneem actie dienovereenkomstig.  

![ExpressRoute-Monitor](media/log-analytics-network-performance-monitor/expressroute-peak-utilization.png)

 

## <a name="next-steps"></a>Volgende stappen
* [Zoeken in een logboek](log-analytics-log-searches.md) om gedetailleerde netwerk prestaties gegevensrecords weer te geven.
