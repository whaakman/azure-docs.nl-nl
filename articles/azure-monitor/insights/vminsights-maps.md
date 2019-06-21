---
title: App-afhankelijkheden met Azure Monitor weergeven voor virtuele machines (preview) | Microsoft Docs
description: Kaart is een functie van Azure Monitor voor virtuele machines. Automatisch toepassingsonderdelen op Windows en Linux-systemen detecteert en de communicatie tussen services toewijst. Dit artikel bevat informatie over het gebruik van de kaart-functie in verschillende scenario's.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/09/2019
ms.author: magoedte
ms.openlocfilehash: f6273e9b6c7ed0c4685479976343497f01201b0b
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67206762"
---
# <a name="use-the-map-feature-of-azure-monitor-for-vms-preview-to-understand-application-components"></a>De functie voor toewijzing van Azure Monitor gebruiken voor virtuele machines (preview) om te begrijpen van toepassingsonderdelen
In Azure Monitor voor virtuele machines, kunt u de onderdelen van de gedetecteerde toepassing op Windows en Linux-machines (VM's) die worden uitgevoerd in Azure of uw omgeving bekijken. U kunt de virtuele machines op twee manieren zien. Een kaart rechtstreeks vanuit een virtuele machine weergeven of een overzicht van Azure Monitor om te zien van de onderdelen voor groepen van virtuele machines weergeven. Dit artikel krijgt u inzicht in deze twee methoden voor de weergave en het gebruik van de functie voor toewijzing. 

Zie voor meer informatie over het configureren van Azure Monitor voor virtuele machines [Azure Monitor inschakelen voor virtuele machines](vminsights-enable-overview.md).

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure
Meld u aan bij [Azure Portal](https://portal.azure.com).

## <a name="introduction-to-the-map-experience"></a>Inleiding tot de ervaring van de kaart
Voordat u de kaart-ervaring, moet u begrijpen hoe deze geeft en gegevens worden gevisualiseerd. Of u de functie voor toewijzing rechtstreeks vanuit een virtuele machine of van Azure Monitor selecteert, geeft de functie van de kaart een consistente ervaring. Het enige verschil is dat van Azure Monitor, één kaart ziet u alle leden van een toepassing met meerdere lagen of het cluster.

De functie voor toewijzing visualiseert de afhankelijkheden van de virtuele machine door actieve processen uit waarvoor detecteren: 

- Actieve netwerkverbindingen tussen servers.
- Binnenkomende en uitgaande verbinding latentie.
- De poorten in alle via TCP verbonden architectuur gedurende een opgegeven tijdperiode.  
 
Vouw een virtuele machine om details van proces en processen die met de virtuele machine communiceren weer te geven. De clientgroep bevat het aantal van de front-end-clients die verbinding met de virtuele machine maken. De groepen van de server-poort weergegeven de telling van de virtuele machine verbinding met back-endservers. Een server-poort uitvouwen om te zien van de gedetailleerde lijst met servers die verbinding via deze poort maken.  

Wanneer u de virtuele machine, selecteert de **eigenschappen** deelvenster aan de rechterkant ziet u de eigenschappen van de virtuele machine. Eigenschappen van zijn systeeminformatie, zoals gemeld door het besturingssysteem, de eigenschappen van de Azure-VM en een ringdiagram met een overzicht van de gedetecteerde verbindingen. 

![Het deelvenster met eigenschappen](./media/vminsights-maps/properties-pane-01.png)

Selecteer aan de rechterkant van het deelvenster **logboekgebeurtenissen** om een lijst met gegevens die de virtuele machine is verzonden naar Azure Monitor weer te geven. Deze gegevens zijn beschikbaar voor het uitvoeren van query's.  Selecteer een recordtype open de **logboeken** pagina, waar u de resultaten voor dat recordtype zien. U ziet ook een vooraf geconfigureerde query die gefilterd op basis van de virtuele machine.  

![Het deelvenster gebeurtenissen vastleggen in logboek](./media/vminsights-maps/properties-pane-logs-01.png)

Sluit de **logboeken** pagina en Ga terug naar de **eigenschappen** deelvenster. Selecteer hier **waarschuwingen** naar waarschuwingen voor VM-criteria voor servicestatus weergeven. De functie van de kaart kan worden geïntegreerd met Azure-waarschuwingen om weer te geven van waarschuwingen voor de geselecteerde server in het geselecteerde tijdsbereik. Een pictogram voor de huidige waarschuwingen op de server weergegeven en de **Machine waarschuwingen** deelvenster geeft een lijst van de waarschuwingen. 

![Het deelvenster waarschuwingen van](./media/vminsights-maps/properties-pane-alerts-01.png)

Als u de kaartfunctie relevante waarschuwingen worden weergegeven, maakt u een waarschuwingsregel die van toepassing op een specifieke computer:

- Een component naar waarschuwingen voor groepen opnemen door computer (bijvoorbeeld **door Computer interval van 1 minuut**).
- De waarschuwing baseren op een metrische waarde.

Zie voor meer informatie over Azure-waarschuwingen en het maken van het waarschuwingsregels [Unified waarschuwingen in Azure Monitor](../../azure-monitor/platform/alerts-overview.md).

In de rechterbovenhoek de **legenda** optie beschrijft de rollen van de symbolen en op de kaart. Voor een nader bekijken op uw kaart en om deze oplossing te verplaatsen, gebruikt u de zoomknoppen in de rechterbenedenhoek. U kunt het zoomniveau instellen en aanpassen van de kaart naar de grootte van de pagina.  

## <a name="connection-metrics"></a>Metrische verbindingsgegevens
De **verbindingen** deelvenster wordt het standaard metrische gegevens voor de geselecteerde verbinding van de virtuele machine voor de TCP-poort weergegeven. De metrische gegevens bevatten reactietijd, aantal aanvragen per minuut, verkeer doorvoer en koppelingen.  

![Network connectivity grafieken in het deelvenster verbindingen](./media/vminsights-maps/map-group-network-conn-pane-01.png)  

### <a name="failed-connections"></a>Mislukte verbindingen
De kaart bevat mislukte verbindingen voor processen en computers. Een rode stippellijn geeft aan dat een clientsysteem te bereiken een proces of de poort is mislukt. Voor systemen die gebruikmaken van de agent voor afhankelijkheden, wordt de agent rapporteert over mislukte verbindingspogingen. De functie voor toewijzing controleert een proces met inachtneming van TCP-sockets die niet voldoen aan een verbinding tot stand brengen. Deze fout kan worden veroorzaakt door een firewall, een onjuiste configuratie van de client of server of een externe service die niet beschikbaar.

![Een mislukte verbinding op de kaart](./media/vminsights-maps/map-group-failed-connection-01.png)

Informatie over mislukte verbindingen kunnen u helpen oplossen, migratie valideren, beveiliging analyseren en inzicht in de algehele architectuur van de service. Mislukte verbindingen soms onschadelijk zijn, maar ze vaak wijzen op een probleem. Verbindingen mislukken, bijvoorbeeld wanneer een failover-omgeving plotseling niet meer bereikbaar is of wanneer twee toepassingslagen niet kunnen met elkaar worden verbonden na de cloudmigratie van een communiceren.

### <a name="client-groups"></a>Clientgroepen
Op de kaart vertegenwoordigen clientgroepen clientcomputers die verbinding met de toegewezen machine maken. Één clientgroep vertegenwoordigt de clients voor een afzonderlijk proces of de machine.

![Een clientgroep op de kaart](./media/vminsights-maps/map-group-client-groups-01.png)

Als u wilt zien van de bewaakte clients en het IP-adressen van de systemen in de groep van een client, de groep te selecteren. De inhoud van de groep worden hieronder weergegeven.  

![Lijst van de clientgroep van een met IP-adressen op de kaart](./media/vminsights-maps/map-group-client-group-iplist-01.png)

Als de groep bewaakte en niet-bewaakte clients omvat, kunt u de juiste sectie van het ringdiagram van de groep voor het filteren van de clients selecteren.

### <a name="server-port-groups"></a>Serverpoort groepen
Server poortgroepen vertegenwoordigen poorten op servers waarop binnenkomende verbindingen van de toegewezen machine. De groep bevat de server-poort en een telling van het aantal servers dat verbindingen naar die poort hebben. Selecteer de groep om de afzonderlijke servers en verbindingen te bekijken. 

![Een groep server-poort op de kaart](./media/vminsights-maps/map-group-server-port-groups-01.png)  

Als de groep bewaakte en niet-bewaakte servers bevat, kunt u de juiste sectie van het ringdiagram van de groep voor het filteren van de servers selecteren.

## <a name="view-a-map-from-a-vm"></a>Een toewijzing van een virtuele machine weergeven 

Voor toegang tot Azure Monitor voor virtuele machines rechtstreeks vanaf een virtuele machine:

1. Selecteer in de Azure portal, **virtuele Machines**. 
2. Kies in de lijst, een virtuele machine. In de **bewaking** sectie **inzichten (preview)** .  
3. Selecteer de **kaart** tabblad.

De kaart worden gevisualiseerd met afhankelijkheden van de virtuele machine door te detecteren met verwerken van groepen en processen met actieve netwerkverbindingen via een opgegeven tijdperiode.  

Standaard ziet u de kaart de laatste 30 minuten. Als u zien hoe de afhankelijkheden in het verleden hebt bekeken wilt, kunt u een query voor historische tijdsbereik van maximaal één uur. De query wilt uitvoeren, gebruiken de **TimeRange** selector in de linkerbovenhoek. U kunt een query kunt uitvoeren, bijvoorbeeld tijdens een incident of de status voor een wijziging wilt weergeven.  

![Direct overzicht van de toewijzing van VM](./media/vminsights-maps/map-direct-vm-01.png)

## <a name="view-a-map-from-a-virtual-machine-scale-set"></a>Een toewijzing van een virtuele-machineschaalset weergeven

Toegang krijgen tot Azure Monitor voor virtuele machines rechtstreeks vanaf een virtuele-machineschaalset:

1. Selecteer in de Azure portal, **virtuele-machineschaalsets**.
2. Kies in de lijst, een virtuele machine. Klik in de **bewaking** sectie **inzichten (preview)** .  
3. Selecteer de **kaart** tabblad.

De kaart worden gevisualiseerd met alle exemplaren in de schaalset als een groepsknooppunt samen met de afhankelijkheden van de groep. De uitgevouwen knooppunt geeft de exemplaren in de schaalset. U kunt deze instanties 10 schuiven tegelijk. 

Voor het laden van een kaart voor een specifiek exemplaar, selecteert u eerst dat exemplaar op de kaart. Selecteer vervolgens de **weglatingsteken** knop (...) aan de rechterkant en kies **Servertoewijzing**. In de kaart die wordt weergegeven, ziet u de procesgroepen en processen met actieve netwerkverbindingen via een opgegeven tijdperiode. 

Standaard ziet u de kaart de laatste 30 minuten. Als u zien hoe de afhankelijkheden in het verleden hebt bekeken wilt, kunt u een query voor historische tijdsbereik van maximaal één uur. De query wilt uitvoeren, gebruiken de **TimeRange** selector. U kunt een query kunt uitvoeren, bijvoorbeeld tijdens een incident of de status voor een wijziging wilt weergeven.

![Direct overzicht van de toewijzing van VM](./media/vminsights-maps/map-direct-vmss-01.png)

>[!NOTE]
>U kunt ook toegang tot een toewijzing voor een specifiek exemplaar van de **exemplaren** weergave voor uw virtuele-machineschaalset. In de **instellingen** sectie, Ga naar **exemplaren** > **inzichten (preview)** .

## <a name="view-a-map-from-azure-monitor"></a>Een overzicht van Azure Monitor weergeven
In Azure Monitor biedt de functie van de kaart een algemeen overzicht van uw virtuele machines en hun afhankelijkheden. Voor toegang tot de Map-functie in Azure Monitor:

1. Selecteer in de Azure portal, **Monitor**. 
2. In de **Insights** sectie **virtuele Machines (preview)** .
3. Selecteer de **kaart** tabblad.

   ![Azure Monitor-overzichtskaart van meerdere virtuele machines](./media/vminsights-maps/map-multivm-azure-monitor-01.png)

Kies een werkruimte met behulp van de **werkruimte** kiezer boven aan de pagina. Als u meer dan één Log Analytics-werkruimte hebt, kiest u de werkruimte die met de oplossing ingeschakeld en die rapporteren aan de virtuele machines heeft. 

De **groep** selector retourneert abonnementen, resourcegroepen, [computergroepen](../../azure-monitor/platform/computer-groups.md), en virtual machine scale sets van computers die zijn gerelateerd aan de geselecteerde werkruimte. Uw selectie geldt alleen voor de functie voor toewijzing en niet meegenomen naar de prestaties of de status.

Standaard ziet u de kaart de laatste 30 minuten. Als u zien hoe de afhankelijkheden in het verleden hebt bekeken wilt, kunt u een query voor historische tijdsbereik van maximaal één uur. De query wilt uitvoeren, gebruiken de **TimeRange** selector. U kunt een query kunt uitvoeren, bijvoorbeeld tijdens een incident of de status voor een wijziging wilt weergeven.  

## <a name="next-steps"></a>Volgende stappen
- Zie voor meer informatie over het gebruik van de Health-functie, [weergave Azure VM-status](vminsights-health.md). 
- Voor het identificeren van knelpunten, prestaties, controleren en begrijpen van de totale capaciteit van uw VM's, Zie [status van de prestaties van Azure Monitor weergeven voor virtuele machines](vminsights-performance.md). 
