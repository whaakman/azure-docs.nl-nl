---
title: Netwerk-Prestatiemeter-oplossing in Azure Log Analytics | Microsoft Docs
description: Gebruiken de ExpressRoute-Manager-functie in Prestatiemeter netwerk voor het bewaken van end-to-end-connectiviteit en prestaties tussen uw filialen en Azure via Azure ExpressRoute.
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
ms.openlocfilehash: 9610a8b37ead976cfdfa2fed81d4d3932055ddcc
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2018
ms.locfileid: "30237780"
---
# <a name="expressroute-manager"></a>ExpressRoute Manager

Kunt u de mogelijkheid Azure ExpressRoute Manager in [netwerk Prestatiemeter](log-analytics-network-performance-monitor.md) voor het bewaken van end-to-end-connectiviteit en prestaties tussen uw filialen en Azure via Azure ExpressRoute. Belangrijkste voordelen zijn: 

- Automatische detectie van ExpressRoute-circuits die zijn gekoppeld aan uw abonnement.
- Het bijhouden van gebruik van de bandbreedte, verlies en latentieperiode circuit, peering en Azure Virtual Network-niveau voor ExpressRoute.
- Detectie van netwerktopologie van uw ExpressRoute-circuits.

![ExpressRoute-Monitor](media/log-analytics-network-performance-monitor/expressroute-intro.png)

## <a name="configuration"></a>Configuratie 
Open de configuratie voor netwerk-Prestatiemeter te openen de [netwerk Prestatiemeter oplossing](log-analytics-network-performance-monitor.md) en selecteer **configureren**.

### <a name="configure-network-security-group-rules"></a>Netwerkbeveiligingsgroepen configureren 
Voor de servers in Azure die worden gebruikt voor het bewaken van via het netwerk Prestatiemeter configureren (NSG) netwerkbeveiligingsgroepen voor TCP-verkeer op de poort die wordt gebruikt door Netwerkcontrole van de prestaties van de synthetische transacties. De standaardpoort is 8084. Deze configuratie kan de Operations Management Suite-agent geïnstalleerd op Azure Virtual machines om te communiceren met een on-premises monitoring agent. 

Zie voor meer informatie over het NSG [Netwerkbeveiligingsgroepen](../virtual-network/virtual-networks-create-nsg-arm-pportal.md). 

>[!NOTE]
> Voordat u met deze stap doorgaat, de lokale server-agent en de Azure-server-agent installeren en de EnableRules.ps1 PowerShell-script uitvoeren. 

 
### <a name="discover-expressroute-peering-connections"></a>ExpressRoute-peering verbindingen detecteren 
 
1. Selecteer de **ExpressRoute Peerings** weergeven.
2. Selecteer **nu detecteren** voor het detecteren van alle persoonlijke peerings die zijn verbonden met de virtuele netwerken in de Azure-abonnement is gekoppeld aan deze werkruimte Azure Log Analytics ExpressRoute.

    >[!NOTE]
    > De oplossing wordt momenteel alleen persoonlijke ExpressRoute peerings detecteert. 

    >[!NOTE]
    > Alleen worden persoonlijke peerings verbonden met de virtuele netwerken die zijn gekoppeld aan het abonnement is gekoppeld aan deze werkruimte voor logboekanalyse gedetecteerd. Als ExpressRoute is verbonden met virtuele netwerken buiten het abonnement is gekoppeld aan deze werkruimte, maakt u een werkruimte voor logboekanalyse in deze abonnementen. Netwerk-Prestatiemeter vervolgens gebruiken voor het bewaken van deze peerings. 

    ![Configuratie van ExpressRoute-Monitor](media/log-analytics-network-performance-monitor/expressroute-configure.png)
 
 Nadat de detectie voltooid is, wordt de gedetecteerde persoonlijke peering verbindingen worden weergegeven in een tabel. De bewaking voor deze peerings is in eerste instantie een uitgeschakeld. 

### <a name="enable-monitoring-of-the-expressroute-peering-connections"></a>Schakel de bewaking van de peering ExpressRoute-verbindingen 

1. Selecteer de persoonlijke peering verbinding die u wilt bewaken.
2. Selecteer in het deelvenster aan de rechterkant de **bewaken van deze Peering** selectievakje. 
3. Als u van plan bent de gezondheid van gebeurtenissen voor deze verbinding te maken, selecteert u **statuscontrole inschakelen voor deze peering**. 
4. Kies de bewaking van de voorwaarden. U kunt aangepaste drempelwaarden voor het genereren van gebeurtenis health instellen door te voeren drempelwaarden. Wanneer de waarde van de voorwaarde hoger dan de geselecteerde drempelwaarde voor de peering verbinding is, wordt een health-gebeurtenis gegenereerd. 
5. Selecteer **Agents toevoegen** de bewaking agents die u gebruiken wilt voor het bewaken van deze peering verbinding kiezen. Zorg ervoor dat u agents toevoegen aan beide uiteinden van de verbinding. U moet ten minste één agent in het virtuele netwerk is verbonden met deze peering. U moet ook ten minste één lokale agent verbonden met deze peering. 
6. Selecteer **opslaan** aan de configuratie op te slaan. 

   ![Configuratie van de ExpressRoute-bewaking](media/log-analytics-network-performance-monitor/expressroute-configure-discovery.png)


Nadat u de regels en select-waarden en agents hebt ingeschakeld, wacht u 30 tot 60 minuten voor de waarden te vullen en de **ExpressRoute bewaking** tegels worden weergegeven. Wanneer u de bewaking tegels ziet, worden uw ExpressRoute-circuits en -verbindingsbronnen nu bewaakt door Netwerkcontrole prestaties. 

>[!NOTE]
> Deze mogelijkheid werkt op betrouwbare wijze op werkruimten die u hebt bijgewerkt naar de nieuwe querytaal.

## <a name="walkthrough"></a>Walkthrough 

De netwerk-Prestatiemeter-dashboard geeft een overzicht van de status van het ExpressRoute-circuits en peering verbindingen. 

![Netwerk Prestatiemeter-dashboard](media/log-analytics-network-performance-monitor/npm-dashboard-expressroute.png) 

### <a name="circuits-list"></a>Circuits lijst 

Selecteer de tegel van ExpressRoute-circuits een overzicht van alle bewaakte ExpressRoute-circuits. U kunt een circuit selecteren en weergeven van de status, trendgrafieken voor pakketverlies, bandbreedte en latentie. De grafieken, zijn interactief. U kunt een aangepaste tijdvenster voor het uitzetten van de grafieken selecteren. Sleep de muisaanwijzer over een gebied op de grafiek inzoomen en fijnmazig gegevenspunten bekijken. 

![ExpressRoute-circuits lijst](media/log-analytics-network-performance-monitor/expressroute-circuits.png) 

### <a name="trends-of-loss-latency-and-throughput"></a>Trends van gegevensverlies, latentie en doorvoer 

De bandbreedte-gebruik, latentie en gegevensverlies grafieken, zijn interactief. U kunt inzoomen op een willekeurig gedeelte van deze grafieken met de muis besturingselementen. Ook ziet u de bandbreedte, latentie en van verliesgegevens voor andere intervallen. In de linkerbovenhoek onder de **acties** knop, selecteer **datum/tijd**. 

![ExpressRoute-latentie](media/log-analytics-network-performance-monitor/expressroute-latency.png) 

### <a name="peerings-list"></a>Lijst van Peerings 

Voor het maken van een lijst van alle verbindingen met virtuele netwerken via persoonlijke peering, selecteer de **persoonlijke Peerings** tegel op het dashboard. Hier kunt u een virtuele netwerkverbinding en bekijk de status, trendgrafieken voor pakketverlies, bandbreedte en latentie. 

![ExpressRoute-peerings](media/log-analytics-network-performance-monitor/expressroute-peerings.png) 

### <a name="circuit-topology"></a>Circuit-topologie 

Als u wilt weergeven circuit topologie, selecteer de **topologie** tegel. Deze actie gaat u naar de Topologieweergave van de geselecteerde circuit of peering. Diagram van de topologie biedt de latentie voor elk segment in het netwerk en elke laag 3-hop wordt vertegenwoordigd door een knooppunt van het diagram. Als u een hop geeft meer informatie over de hop. Verplaats de schuifregelaar onder voor een verhoging van het niveau van de zichtbaarheid wilt opnemen lokale hops **FILTERS**. De schuifregelaar verplaatsen naar de linker- of toeneemt of verkleint u het aantal hops in de grafiek topologie. De latentie tussen elk segment is zichtbaar, waardoor voor snellere isolatie van hoge latentie segmenten in uw netwerk. 

![ExpressRoute-topologie](media/log-analytics-network-performance-monitor/expressroute-topology.png)

### <a name="detailed-topology-view-of-a-circuit"></a>Gedetailleerde Topologieweergave van een circuit 

Deze weergave toont virtuele netwerkverbindingen. 

![ExpressRoute-verbindingen voor virtueel netwerk](media/log-analytics-network-performance-monitor/expressroute-vnet.png)
 

### <a name="diagnostics"></a>Diagnostiek 

Prestatiemeter netwerk kunt u verschillende circuit Verbindingsproblemen opsporen. Enkele van de problemen worden hier weergegeven. 

**Circuit is niet beschikbaar.** Prestatiemeter-netwerk ontvangt u een melding zodra de connectiviteit tussen uw lokale bronnen en virtuele netwerken in Azure verbroken wordt. Deze melding kunt u proactief actie te ondernemen voordat u escalaties van de gebruiker ontvangt en de uitvaltijd beperken.

![ExpressRoute-circuit is niet beschikbaar](media/log-analytics-network-performance-monitor/expressroute-circuit-down.png)
 

**Verkeer niet via de beoogde circuit.** Netwerk Prestatiemeter waarschuwt u wanneer verkeer wordt niet door de beoogde ExpressRoute-circuit stroomt. Dit probleem kan zich voordoen als het circuit niet actief is en verkeer via de back-route stroomt. Dit kan ook gebeuren als er een probleem met de routering. Deze informatie helpt u proactief eventuele configuratieproblemen in uw routeringsbeleid beheren en zorg ervoor dat de meest optimaal en veilig route wordt gebruikt. 

 

**Verkeer niet via de primaire circuit.** Netwerk-Prestatiemeter waarschuwt u als verkeer via het secundaire ExpressRoute-circuit stroomt. Hoewel u connectiviteit problemen in dit geval, proactief optreden Won't kunt problemen met het primaire circuit u beter voorbereid. 

 
![ExpressRoute-netwerkverkeer](media/log-analytics-network-performance-monitor/expressroute-traffic-flow.png)


**Afnemen als gevolg van piekgebruik neemt.** U kunt de trend in het gebruik bandbreedte met de latentie trend te bepalen of de afname van de Azure-workload vanwege een piek in de bandbreedte of niet is correleren. U kunt vervolgens actie dienovereenkomstig nemen.

![ExpressRoute-bandbreedtegebruik](media/log-analytics-network-performance-monitor/expressroute-peak-utilization.png)

 

## <a name="next-steps"></a>Volgende stappen
[Zoeken in een logboek](log-analytics-log-searches.md) om gedetailleerde netwerk prestaties gegevensrecords weer te geven.
