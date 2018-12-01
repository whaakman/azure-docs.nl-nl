---
title: App-afhankelijkheden met Azure Monitor weergeven voor virtuele machines (Preview) | Microsoft Docs
description: Kaart is een functie van de Azure-Monitor voor virtuele machines die automatisch toepassingsonderdelen op Windows en Linux-systemen detecteert en de communicatie tussen services toewijst. Dit artikel bevat informatie over het gebruik ervan in een verscheidenheid aan scenario's.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/25/2018
ms.author: magoedte
ms.openlocfilehash: 0226e5dc03c3f10b951a4e4cc1d97a999c952f3b
ms.sourcegitcommit: cd0a1514bb5300d69c626ef9984049e9d62c7237
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/30/2018
ms.locfileid: "52678869"
---
# <a name="using-azure-monitor-for-vms-preview-map-to-understand-application-components"></a>Met behulp van Azure Monitor voor de toewijzing van virtuele machines (Preview) om te begrijpen van toepassingsonderdelen
De gedetecteerde toepassingsonderdelen weergeven op Windows en Linux-machines uitvoeren in uw omgeving kan worden waargenomen op twee manieren met Azure Monitor voor virtuele machines van een virtuele machine rechtstreeks of via groepen virtuele machines van Azure Monitor Azure. 

Dit artikel krijgt u inzicht in de ervaring tussen de twee perspectieven en hoe u de functie van de kaart. Zie voor meer informatie over het configureren van Azure Monitor voor virtuele machines [Azure Monitor inschakelen voor virtuele machines](vminsights-onboard.md).

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure
Meld u aan bij de Azure Portal op [https://portal.azure.com](https://portal.azure.com).

## <a name="introduction-to-map-experience"></a>Inleiding tot de ervaring van de kaart
Voordat u kaart weergeven voor een enkele virtuele machine of een groep virtuele machines, is het belangrijk dat we bieden een korte inleiding op de functie zodat u begrijpt hoe de informatie wordt weergegeven en wat de visualisaties vertegenwoordigen.  

Of u de functie voor toewijzing rechtstreeks vanuit een virtuele machine of van Azure Monitor selecteert, geeft deze een consistente ervaring.  Het enige verschil is van Azure Monitor dat kunt u alle leden van een toepassing met meerdere lagen of het cluster in een kaart bekijken.

Maps visualiseert de afhankelijkheden van de virtuele machines door detectie van actieve processen met actieve netwerkverbindingen tussen servers, binnenkomend en uitgaand verbinding latentie en poorten in alle via TCP verbonden architectuur gedurende een opgegeven tijdperiode.  Details van proces voor het uitbreiden van een virtuele machine bevat en processen die met de virtuele machine communiceren worden weergegeven. Het aantal front-clients die verbinding bij de virtuele machine maken wordt aangegeven met de clientgroep. Het aantal back-endservers die de virtuele machine maakt verbinding met het worden aangegeven op de Server-poort-groepen. Vouw een servergroep poort om te zien van de gedetailleerde lijst met servers die zijn verbonden via deze poort.  

Wanneer u op de virtuele machine klikt, de **eigenschappen** deelvenster aan de rechterkant om weer te geven van de eigenschappen van het item is geselecteerd, zoals gemeld door het besturingssysteem, de eigenschappen van de Azure-VM en een ringdiagram systeemgegevens is uitgevouwen Samenvatting van de gedetecteerde verbindingen. 

![Eigenschappen van de computer](./media/vminsights-maps/properties-pane-01.png)

Aan de rechterkant-van het deelvenster, klikt u op de **logboekgebeurtenissen** pictogram focus verplaatsen van het deelvenster om een lijst met tabellen die gegevens verzameld van de virtuele machine weer te geven is verzonden naar Log Analytics en is beschikbaar voor het uitvoeren van query's.  Te klikken op een van de typen bronrecords vermeld wordt geopend de **logboeken** pagina om de resultaten voor dat type met een vooraf geconfigureerde query weer te geven die zijn gefilterd op basis van de specifieke virtuele machine.  

![Lijst met zoeken in het deelvenster met eigenschappen](./media/vminsights-maps/properties-pane-logs-01.png)

Sluiten *logboeken** en Ga terug naar de **eigenschappen** deelvenster en selecteer **waarschuwingen** op waarschuwingen weergeven die een waarschuwing gegenereerd voor de VM op basis van criteria voor servicestatus. Kaart kan worden geïntegreerd met Azure-waarschuwingen om weer te geven van geactiveerde waarschuwingen voor de geselecteerde server in het geselecteerde tijdsbereik. De server, wordt er een pictogram weergegeven als er huidige waarschuwingen, en in het deelvenster waarschuwingen van de Machine de waarschuwingen worden. 

![Machine-waarschuwingen in het deelvenster met eigenschappen](./media/vminsights-maps/properties-pane-alerts-01.png)

De kaart als functie wilt inschakelen om relevante waarschuwingen weer te geven, maakt u een waarschuwingsregel die wordt geactiveerd voor een specifieke computer. Juiste om waarschuwingen te maken:
- Een component aan groep opnemen door computer (bijvoorbeeld **door Computer interval van 1 minuut**).
- Kies een waarschuwing moet op basis van de meting van metrische gegevens.

Zie voor meer informatie over Azure-waarschuwingen en het maken van het waarschuwingsregels [Unified waarschuwingen in Azure Monitor](../../monitoring-and-diagnostics/monitoring-overview-alerts.md)

De **legenda** optie in de rechterbovenhoek beschrijft de rollen van de symbolen en op een kaart.  Voor nog eens kijken uw kaart inzoomen en verplaatsen van de it rond de zoomknoppen aan de onderkant rechts van de pagina stelt het zoomniveau en op de pagina aan de grootte van de huidige pagina past.  

## <a name="connection-metrics"></a>Metrische verbindingsgegevens
De **verbindingen** deelvenster wordt het standaard connectiviteit metrische gegevens voor de geselecteerde verbinding van de virtuele machine voor de TCP-poort weergegeven. De metrische gegevens bevatten reactietijd, aantal aanvragen per minuut, verkeer doorvoer en koppelingen.  

![Network connectivity grafieken deelvenster Voorbeeld](./media/vminsights-maps/map-group-network-conn-pane-01.png)  

### <a name="failed-connections"></a>Mislukte verbindingen
Mislukte verbindingen worden weergegeven in de kaart voor processen en computers, met een rode stippellijn die aangeeft dat een clientsysteem te bereiken een proces of de poort is mislukt. Mislukte verbindingen worden gerapporteerd door een systeem met de agent voor afhankelijkheden als dit systeem is geprobeerd de mislukte verbinding. Dit proces maatregelen kaart met inachtneming van TCP-sockets die niet voldoen aan een verbinding tot stand brengen. Deze fout kan worden veroorzaakt door een firewall, een onjuiste configuratie van de client of server of een externe service niet beschikbaar.

![Voorbeeld van een mislukte verbinding op de kaart](./media/vminsights-maps/map-group-failed-connection-01.png)

Informatie over mislukte verbindingen kunnen helpen bij het oplossen van problemen, validatie van de migratie, beveiligingsanalyse en informatie over de algehele architectuur van de service. Mislukte verbindingen soms onschadelijk zijn, maar ze vaak rechtstreeks verwijzen naar een probleem is, zoals een failover-omgeving plotseling is onbereikbaar of twee toepassingslagen niet kan communiceren met elkaar worden verbonden na de cloudmigratie van een.

### <a name="client-groups"></a>Clientgroepen
Clientgroepen op de kaart vertegenwoordigen clientcomputers waarvoor verbindingen in de toegewezen machine. Een enkele Client groep vertegenwoordigt de clients voor een afzonderlijk proces of de machine.

![Voorbeeld van de client-groepen op de kaart](./media/vminsights-maps/map-group-client-groups-01.png)

Als u wilt zien van de bewaakte clients en het IP-adressen van de systemen in de groep van een Client, de groep te selecteren. De inhoud van de groep worden weergegeven onder de groep.  

![Client groep IP-adres lijst voorbeeld op de kaart](./media/vminsights-maps/map-group-client-group-iplist-01.png)

Als de groep bewaakte en niet-bewaakte clients omvat, kunt u de juiste sectie van het ringdiagram in de groep voor het filteren van de clients selecteren.

### <a name="server-port-groups"></a>Groepen van de server-poort
Server Poortgroepen vertegenwoordigen server-poorten op servers die binnenkomende verbindingen van de toegewezen machine. De groep bevat de server-poort en een telling van het aantal servers met verbinding naar die poort. Selecteer de groep om te zien van de afzonderlijke servers en de verbindingen die worden vermeld. 

![Voorbeeld van de server-poort-groep op de kaart](./media/vminsights-maps/map-group-server-port-groups-01.png)  

Als de groep bewaakte en niet-bewaakte servers bevat, kunt u de juiste sectie van het ringdiagram in de groep voor het filteren van de servers selecteren.

## <a name="view-map-directly-from-a-virtual-machine"></a>Kaart weergeven rechtstreeks vanuit een virtuele machine 

Voor toegang tot Azure Monitor voor virtuele machines rechtstreeks vanaf een virtuele machine, het volgende te doen.

1. Selecteer in de Azure portal, **virtuele Machines**. 
2. Kies in de lijst, een virtuele machine en klik in de **bewaking** sectie Kies **inzichten (preview)**.  
3. Selecteer de **kaart** tabblad.

Kaart visualiseert de afhankelijkheden VM's, die wordt uitgevoerd procesgroepen en processen met actieve netwerkverbindingen via een opgegeven tijdperiode.  Standaard ziet u de kaart de laatste 30 minuten.  Met behulp van de **TimeRange** selector in de linkerbovenhoek kunt u zoeken naar historische tijdsbereik van maximaal één uur om weer te geven hoe afhankelijkheden in het verleden hebt bekeken (bijvoorbeeld tijdens een incident, of voordat er een wijziging is opgetreden).  

![Direct overzicht van de toewijzing van VM](./media/vminsights-maps/map-direct-vm-01.png)

## <a name="view-map-from-azure-monitor"></a>Kaart van Azure Monitor weergeven
Van Azure Monitor biedt de functie van de kaart een algemeen overzicht van uw virtuele machines en hun afhankelijkheden.  Voor toegang tot de Map-functie van Azure Monitor, het volgende te doen. 

1. Selecteer in de Azure portal, **Monitor**. 
2. Kies **virtuele Machines (preview)** in de **Insights** sectie.
3. Selecteer de **kaart** tabblad.

![Overzicht van Azure Monitor multi-VM-kaart](./media/vminsights-maps/map-multivm-azure-monitor-01.png)

Uit de **werkruimte** kiezer boven aan de pagina, hebt u meer dan één Log Analytics-werkruimte, kies de werkruimte die met de oplossing is ingeschakeld en virtuele machines die rapporteren aan. De **groep** selector abonnementen, resourcegroepen, retourneert [computergroepen](../../azure-monitor/platform/computer-groups.md), en VM scale sets van computers met betrekking tot de geselecteerde werkruimte. Uw selectie wordt alleen toegepast op de kaart-functie en wordt niet meegenomen naar de prestaties of de kaart.

Standaard ziet u de kaart de laatste 30 minuten. Met behulp van de **TimeRange** selector, kunt u zoeken naar historische tijdsbereik van maximaal één uur om weer te geven hoe afhankelijkheden in het verleden hebt bekeken (bijvoorbeeld tijdens een incident, of voordat er een wijziging is opgetreden).   

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het gebruik van de health-functie, [weergave Azure VM-status](vminsights-health.md), of voor het identificeren van knelpunten en het algehele gebruik met de prestaties van uw VM's, Zie [weergave Azure Monitor voor de prestaties van virtuele machines](vminsights-performance.md). 
