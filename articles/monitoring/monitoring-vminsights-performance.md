---
title: Hoe u in de grafiek prestaties met Azure Monitor voor virtuele machines (Preview) | Microsoft Docs
description: Prestaties is een functie van de Azure-Monitor voor virtuele machines die automatisch toepassingsonderdelen op Windows en Linux-systemen detecteert en de communicatie tussen services toewijst. Dit artikel bevat informatie over het gebruik ervan in een verscheidenheid aan scenario's.
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
ms.openlocfilehash: 776a8901696bd69eeee6fd4b3622c8992bfc25a3
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50094309"
---
# <a name="how-to-chart-performance-with-azure-monitor-for-vms-preview"></a>Hoe u op de prestaties van de grafiek met Azure Monitor voor virtuele machines (Preview)
Azure Monitor voor virtuele machines bevat een set van prestatiegrafieken weergegeven die zijn gericht verschillende prestatie-indicatoren (KPI's) om te bepalen hoe goed een virtuele machine wordt uitgevoerd. De grafieken weergegeven Resourcegebruik gedurende een bepaalde periode, zodat u kunt identificeren van knelpunten, afwijkingen, of schakel over naar een perspectief aanbieding elke machine om gebruik van resources op basis van de geselecteerde meetwaarde weer te geven. Er zijn talrijke elementen bij het omgaan met prestaties, wordt Azure Monitor voor virtuele machines is gericht op het besturingssysteem als gemanifesteerd via de processor, geheugen, netwerkadapters en schijven. Prestaties is een aanvulling op de functie voor health monitoring en helpt u problemen die wijzen op een onderdeel systeemfouten, ondersteuning voor afstemmen en optimalisatie voor het bereiken van efficiëntie blootstellen of ondersteuning voor capaciteitsplanning.  

## <a name="multi-vm-perspective-from-azure-monitor"></a>Perspectief van de multi-VM van Azure Monitor
Azure monitor biedt de functie voor prestaties een weergave met meerdere virtuele machine van alle bewaakte VM's geïmplementeerd op verschillende resourcegroepen in uw abonnementen of in uw omgeving.  Als u wilt openen via Azure Monitor, moet u de volgende stappen uitvoeren. 

1. Selecteer in de Azure portal, **Monitor**. 
2. Kies **virtuele Machines (preview)** in de **oplossingen** sectie.
3. Selecteer de **prestaties** tabblad.

![VM insights prestaties Top N-lijstweergave](./media/monitoring-vminsights-performance/vminsights-performance-aggview-01.png)

Op de **Top N grafieken** tabblad, hebt u meer dan één Log Analytics-werkruimte, kiest u de werkruimte ingeschakeld met de oplossing uit de **werkruimte** kiezer boven aan de pagina. De **groep** selector abonnementen, resourcegroepen, retourneert [computergroepen](../log-analytics/log-analytics-computer-groups.md), en virtual machine scale sets van computers met betrekking tot de geselecteerde werkruimte die u gebruiken kunt voor het verder filteren de resultaten weergegeven in de grafieken op deze pagina en alle andere pagina's. Uw selectie wordt alleen toegepast op de functie voor prestaties en wordt niet meegenomen naar de status of de kaart.  

Standaard wordt de grafieken de afgelopen 24 uur weergegeven. Met behulp van de **TimeRange** selector, kunt u zoeken naar historische tijdsbereik van maximaal 30 dagen om weer te geven hoe de prestaties in het verleden hebt bekeken.   

De vijf capaciteit gebruik grafieken weergegeven op de pagina zijn:

* Percentage van CPU-gebruik - ziet u de top vijf machines met de hoogste gemiddelde CPU-gebruik 
* Beschikbaar geheugen - ziet u de top vijf machines met de laagste gemiddelde hoeveelheid beschikbaar geheugen 
* Logische schijfruimte gebruikt % - ziet u de top vijf machines met de hoogste gemiddelde schijfruimte gebruikt % voor alle volumes op schijven 
* Bytes verzonden Rate - ziet u de top vijf machines met hoogste gemiddelde van verzonden bytes 
* Bytes ontvangen Rate - ziet u de top vijf machines met hoogste gemiddelde van verzonden bytes 

Te klikken op in de rechterbovenhoek van een van de vijf grafieken wordt geopend **Top N lijst** weergeven.  Hier ziet u het Resourcegebruik voor deze metrische gegevens voor prestaties van afzonderlijke virtuele machine in een lijst weergeven en welke machine trend voor de hoogste.  

![Bovenste N lijstweergave voor de prestaties van de geselecteerde metrische gegevens](./media/monitoring-vminsights-performance/vminsights-performance-topnlist-01.png)

Wanneer u op de virtuele machine klikt, de **eigenschappen** uitgevouwen deelvenster aan de rechterkant om weer te geven van de eigenschappen van het item is geselecteerd, zoals systeeminformatie, zoals gemeld door het besturingssysteem, de eigenschappen van de virtuele machine van Azure, enzovoort. Te klikken op een van de opties onder de **snelkoppelingen** sectie wordt u omgeleid naar deze functie rechtstreeks vanuit de geselecteerde VM.  

![Het deelvenster met eigenschappen van de virtuele Machine](./media/monitoring-vminsights-performance/vminsights-properties-pane-01.png)

Schakel over naar de **samengevoegd grafieken** tabblad om de maatstaven voor prestaties gefilterd op gemiddelde of percentielen metingen.  

![Virtuele machine insights cumulatieve prestaties weergeven](./media/monitoring-vminsights-performance/vminsights-performance-aggview-02.png)

De volgende capaciteit gebruik grafieken zijn beschikbaar:

* Percentage van CPU-gebruik - standaard het 95e percentiel van gemiddelde en de bovenrand van 
* Beschikbaar geheugen - standaardinstellingen van het gemiddelde, top 5 en 10 percentiel 
* Logische schijfruimte gebruikt % - standaard met het gemiddelde en 95e percentiel 
* Bytes verzonden Rate - gemiddelde aantal bytes verzonden met standaardinstellingen 
* Bytes ontvangen Rate - gemiddelde aantal bytes dat is ontvangen met standaardinstellingen

U kunt ook de granulatie van de grafieken binnen het tijdsbereik wijzigen door te selecteren **Avg**, **Min**, **Max**, **50e**,  **90e**, en **95th** in de lijst met percentiel.   

Als u wilt het Resourcegebruik door afzonderlijke virtuele machine in een lijstweergave bekijken en ziet trend voor de computer met het hoogste gebruik, selecteer de **Top N lijst** tabblad.  De **Top N lijst** pagina toont de top 20 machines gesorteerd op basis van de meest gebruikte door 95e percentiel voor de metrische gegevens *% CPU-gebruik*.  U kunt meer machines zien door te selecteren **Load meer**, en vouwt u de resultaten om weer te geven van de top 500-machines. 

>[!NOTE]
>De lijst met weergeven niet meer dan 500 machines tegelijk.  
>

![Voorbeeld van de pagina bovenste N List](./media/monitoring-vminsights-performance/vminsights-performance-topnlist-01.png)

Als u wilt de resultaten op een specifieke virtuele machine in de lijst te filteren, voer de naam van de computer in de **zoeken op naam** tekstvak.  

Als u in plaats daarvan gebruik van een andere prestatiemeetwaarde van weergeven wilt de **Metric** vervolgkeuzelijst selecteren **beschikbaar geheugen**, **ruimte logische schijf gebruikt %**,  **Ontvangen bytes/s netwerk**, of **netwerk verzonden bytes/s** en de lijst met updates voor gebruik binnen het bereik van deze metrische gegevens weergeven.  

Hiermee opent u een virtuele machine selecteren in de lijst de **eigenschappen** van het deelvenster aan de rechterkant van de pagina en van hieruit kunt u selecteren **details van de prestatie**.  De **details van de virtuele Machine** pagina wordt geopend en is afgestemd op die VM, vergelijkbare ervaring bij het openen van de prestaties van de VM-inzichten rechtstreeks vanuit de Azure-VM.  

## <a name="view-performance-directly-from-an-azure-vm"></a>Prestaties weergeven rechtstreeks vanuit een Azure-VM
Voor toegang tot rechtstreeks vanuit een virtuele machine, het volgende te doen.

1. Selecteer in de Azure portal, **virtuele Machines**. 
2. Kies in de lijst, een virtuele machine en klik in de **bewaking** sectie Kies **inzichten (preview)**.  
3. Selecteer de **prestaties** tabblad. 

Deze pagina niet alleen bevat prestatiegrafieken voor gebruik, maar ook een tabel weergegeven voor elke logische schijf wordt gedetecteerd, de capaciteit, gebruik, en totale gemiddelde van elke meting.  

De volgende capaciteit gebruik grafieken zijn beschikbaar:

* Percentage van CPU-gebruik - standaard het 95e percentiel van gemiddelde en de bovenrand van 
* Beschikbaar geheugen - standaardinstellingen van het gemiddelde, top 5 en 10 percentiel 
* Logische schijfruimte gebruikt % - standaard met het gemiddelde en 95e percentiel 
* Logische schijf-IOPS - standaard met het gemiddelde en 95e percentiel
* Logische schijf MB/s - standaard met het gemiddelde en 95e percentiel
* Maximum aantal logische schijf gebruikt % - standaardinstellingen van het gemiddelde en 95e percentiel
* Bytes verzonden Rate - gemiddelde aantal bytes verzonden met standaardinstellingen 
* Bytes ontvangen Rate - gemiddelde aantal bytes dat is ontvangen met standaardinstellingen

![Rechtstreeks VM insights prestaties weergeven van virtuele machine](./media/monitoring-vminsights-performance/vminsights-performance-directvm-01.png)

## <a name="alerting-and-alert-management"></a>Meldingen en waarschuwingen beheren 
Metrische gegevens voor prestaties ingeschakeld als onderdeel van Azure Monitor voor virtuele machines bevatten geen vooraf geconfigureerde regels voor waarschuwingen. Er zijn [statusmeldingen](monitoring-vminsights-health.md#alerting-and-alert-management) overeenkomt met de prestatieproblemen gedetecteerd op de virtuele machine van Azure, zoals hoge CPU-gebruik, weinig beschikbaar geheugen, schijf-i/o, weinig schijfruimte, enz., deze health-waarschuwingen worden alleen toegepast op alle virtuele machines verbonden met de dezelfde werkruimte voor logboekanalyse voor Azure Monitor is ingeschakeld voor virtuele machines. 

We kunnen echter alleen verzamelen en opslaan van een subset van de prestatiegegevens die u nodig in de Log Analytics-werkruimte hebt. Als uw strategie voor controle is vereist voor analyse of waarschuwingen die andere metrische gegevens voor prestaties bevat om te evalueren effectief capaciteit of de status van de virtuele machine, of moet u de flexibiliteit om op te geven uw eigen criteria of logische waarschuwingen, kunt u Configureer [verzamelen van deze prestatiemeteritems](../log-analytics/log-analytics-data-sources-performance-counters.md?toc=/azure/azure-monitor/toc.json) in Log Analytics en definieer [waarschuwingen voor activiteitenlogboeken](../monitoring-and-diagnostics/alert-log.md?toc=/azure/azure-monitor/toc.json). Log Analytics kunt u het uitvoeren van complexe analyse met andere gegevenstypen en geef langere bewaartermijn ter ondersteuning van trendanalyse, metrische gegevens aan de andere kant zijn lichtgewicht en kan in de buurt van realtime scenario's ondersteunen. Ze worden verzameld door de [Azure diagnoseagent](../virtual-machines/windows/monitor.md) en opgeslagen in de Azure Monitor metrics store, zodat u kunt waarschuwingen maken met een lagere latentie en tegen lagere kosten.

Bekijk het overzicht van [verzamelen van metrische gegevens en logboeken met Azure Monitor](monitoring-data-collection.md?toc=/azure/azure-monitor/toc.json) om verder te zien de fundamentele verschillen en andere overwegingen voor verzameling van deze aanvullende metrische gegevens en de regels voor waarschuwingen configureren.  

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het gebruik van de health-functie, [weergave Azure Monitor voor de gezondheid van virtuele machines](monitoring-vminsights-health.md), of als u afhankelijkheden van gedetecteerde toepassingen, Zie [weergave Azure Monitor voor virtuele machines kaart](monitoring-vminsights-maps.md). 