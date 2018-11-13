---
title: Pakketinspecties uitvoeren met Azure Network Watcher | Microsoft Docs
description: In dit artikel wordt beschreven hoe u gebruik van Network Watcher om uit te voeren grondige inspecties van pakketten die worden verzameld van een virtuele machine
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: 7b907d00-9c35-40f5-a61e-beb7b782276f
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 7af14143e8ce4924c17a41c6bb1ff33954f4b583
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/12/2018
ms.locfileid: "51568723"
---
# <a name="packet-inspection-with-azure-network-watcher"></a>Pakketinspecties uitvoeren met Azure Network Watcher

Met behulp van de functie voor het vastleggen van pakketten van Network Watcher, kunt u starten en beheren van opnamen sessies op uw Azure Virtual machines uit de portal, PowerShell of CLI, en via een programma via de SDK en REST-API. Pakketopname kunt u soort scenario's waarvoor niveau pakketgegevens door te geven van de informatie in een indeling die ongewijzigd worden gebruikt. Gebruik te maken van een gratis hulpprogramma's voor het controleren van de gegevens, kunt u onderzoeken communicatie verzonden naar en van uw VM's en krijg inzicht in uw netwerkverkeer. Sommige toepassingen voorbeeld van pakket vastleggen van gegevens zijn onder andere: netwerk of de toepassing problemen onderzoeken, netwerk pogingen tot misbruik en inbraakdetectie te detecteren of naleving van regelgeving te onderhouden. In dit artikel laten we zien hoe een packet capture-bestand geleverd door de Network Watcher te openen met behulp van een populaire open-source-hulpprogramma. We sturen ook voorbeelden die laten zien hoe u een latentie van de verbinding berekenen, abnormaal verkeer wordt geïdentificeerd en netwerken statistieken onderzoeken.

## <a name="before-you-begin"></a>Voordat u begint

In dit artikel gaat via bepaalde scenario's met vooraf geconfigureerde op een pakketopname die eerder is uitgevoerd. Deze scenario's worden mogelijkheden die aan de hand van een pakketopname kunnen worden geopend. In dit scenario wordt gebruikgemaakt van [WireShark](https://www.wireshark.org/) om te controleren van de pakketopname.

In dit scenario wordt ervan uitgegaan dat u al een pakketopname uitgevoerd op een virtuele machine. Voor meer informatie over het maken van een pakket vastleggen Bezoek [beheren pakket worden vastgelegd met de portal](network-watcher-packet-capture-manage-portal.md) of met REST recentst [pakket vastgelegd beheren met REST-API](network-watcher-packet-capture-manage-rest.md).

## <a name="scenario"></a>Scenario

In dit scenario u:

* Bekijk een pakketopname

## <a name="calculate-network-latency"></a>De netwerklatentie berekenen

In dit scenario, laten we zien hoe u om de initiële Round Trip Time (RTT) van een Transmission Control Protocol (TCP)-conversatie plaatsvinden tussen twee eindpunten weer te geven.

Wanneer een TCP-verbinding tot stand is gebracht, volgt u de eerste drie pakketten worden verzonden in de verbinding met een patroon dat vaak aangeduid als de handshake drie richtingen. Aan de hand van de eerste twee pakketten worden verzonden in deze-handshake wordt een eerste aanvraag van de client en een reactie van de server, kunnen we de latentie berekenen wanneer deze verbinding tot stand is gebracht. Deze latentie wordt aangeduid als de Round Trip Time (RTT). Raadpleeg de volgende bronnen voor meer informatie over het TCP-protocol en de handshake drie richtingen. https://support.microsoft.com/en-us/help/172983/explanation-of-the-three-way-handshake-via-tcp-ip

### <a name="step-1"></a>Stap 1

WireShark starten

### <a name="step-2"></a>Stap 2

Load de **.cap** -bestand van de pakketopname. Dit bestand kan worden gevonden in de blob is opgeslagen in onze lokaal op de virtuele machine, afhankelijk van hoe u deze hebt geconfigureerd.

### <a name="step-3"></a>Stap 3

Als u wilt weergeven eerste Round Trip Time (Retourtijd) in de TCP-gesprekken, worden er alleen bekijken op de eerste twee pakketten die betrokken zijn bij de TCP-handshake. We gebruiken de eerste twee pakketten in de handshake drie richtingen, die de [SYN], [SYN, ACK] pakketten. Ze heten voor vlaggen instellen in de TCP-header. Het laatste pakket in de handshake, het pakket [ACK] wordt niet gebruikt in dit scenario. Het pakket [SYN] wordt verzonden door de client. Zodra dit is ontvangen verzendt de server [ACK]-pakket als een bevestiging van de SYN ontvangen van de client. Gebruik te maken van het feit dat het antwoord van de server is vereist weinig overhead, we berekenen de RTT door af te trekken van de tijd de [SYN, ACK]-pakket is ontvangen door de client met de tijd [SYN] pakket is verzonden door de client.

Met behulp van WireShark wordt deze waarde berekend voor ons.

Als u wilt weergeven eenvoudiger de eerste twee pakketten in de TCP-handshake drie richtingen, gaan we de mogelijkheid de filteren door WireShark gebruiken.

Als u wilt toepassen op het filter in WireShark, vouw het Segment 'Transmission Control Protocol' van een [SYN]-pakket in uw vastleggen en bekijk de vlaggen instellen in de TCP-header.

Omdat we op zoek bent om te filteren op alle [SYN] en [SYN, ACK] pakketten onder vlaggen cofirm dat de Syn-bit is ingesteld op 1 en rechts klikt u op de bit Syn -> toepassen als Filter -> geselecteerd.

![afbeelding 7][7]

### <a name="step-4"></a>Stap 4

Nu dat u hebt gefilterd, het venster om te zien alleen de pakketten met de [SYN] bit is ingesteld, kunt u eenvoudig gesprekken die u geïnteresseerd bent in om de initiële RTT weer te geven. Een eenvoudige manier om weer te geven van de RTT in WireShark klikt gewoon op de vervolgkeuzelijst "SEQ/ACK" analysis gemarkeerd. Vervolgens ziet u de RTT weergegeven. In dit geval is de RTT 0.0022114 seconden of 2.211 ms.

![afbeelding 8][8]

## <a name="unwanted-protocols"></a>Ongewenste protocollen

U kunt veel toepassingen die worden uitgevoerd op een virtuele machine-instantie die u hebt geïmplementeerd in Azure hebben. Veel van deze toepassingen communiceren via het netwerk mogelijk zonder uw expliciete toestemming. Met behulp van pakketopname voor het opslaan van de netwerkcommunicatie, kunt we hoe de toepassing praten dan over op het netwerk en zoek naar eventuele problemen onderzoeken.

In dit voorbeeld bekijken we een vorige pakketopname voor ongewenste protocollen die kan duiden op niet-geautoriseerde communicatie van een toepassing die wordt uitgevoerd op uw computer uitgevoerd.

### <a name="step-1"></a>Stap 1

Klik met de dezelfde vastleggen in het voorgaande scenario **statistieken** > **Protocol hiërarchie**

![Protocol hiërarchie menu][2]

Het protocol hiërarchie-venster wordt weergegeven. In deze weergave bevat een lijst van alle protocollen die zijn gebruikt tijdens de sessie vastleggen en het aantal pakketten verzonden en ontvangen met behulp van de protocollen. In deze weergave is handig voor het vinden van ongewenste netwerkverkeer op de virtuele machines of het netwerk.

![Protocol hiërarchie geopend][3]

Zoals u in de volgende schermafbeelding ziet, is er verkeer dat gebruikmaakt van de BitTorrent-protocol, dat wordt gebruikt voor het delen van peer-to-peer-bestand. Als een beheerder u niet verwacht om te zien BitTorrent verkeer op deze specifieke virtuele machines. Nu u op de hoogte van dit verkeer, u kunt verwijderen van de peer-to-peer-software die op deze virtuele machine is geïnstalleerd, of het verkeer met behulp van Netwerkbeveiligingsgroepen of een Firewall blokkeren. Bovendien kan u ervoor kiezen om uit te voeren pakketopnamen volgens een schema, zodat u kunt het gebruik van het protocol regelmatig op uw virtuele machines bekijken. Ga voor een voorbeeld over het automatiseren van taken in azure network naar [bewaken van netwerkbronnen met azure automation](network-watcher-monitor-with-azure-automation.md)

## <a name="finding-top-destinations-and-ports"></a>Meest gebruikte doelen en poorten vinden

De typen verkeer, de eindpunten en de poorten die is doorgegeven via, is een belangrijk bij het controleren of het oplossen van toepassingen en bronnen in uw netwerk. Met behulp van een bestand voor het vastleggen van pakket van boven, we kunnen snel informatie over de meest gebruikte doelen die onze VM met communiceert en de poorten die worden gebruikt.

### <a name="step-1"></a>Stap 1

Klik met de dezelfde vastleggen in het voorgaande scenario **statistieken** > **IPv4-statistieken** > **bestemmingen en poorten**

![periode voor het vastleggen van pakket][4]

### <a name="step-2"></a>Stap 2

Als we door de resultaten die een regel opvalt bekijken, zijn er meerdere verbindingen op poort 111. De meest gebruikte poort 3389, is dit extern bureaublad is, en de resterende RPC dynamische poorten zijn.

Hoewel dit verkeer dat er niets betekenen kan, is het een poort die is gebruikt voor het aantal verbindingen en is onbekend bij de beheerder.

![Afbeelding 5][5]

### <a name="step-3"></a>Stap 3

Nu dat we hebben vastgesteld dat een out-of interne poort filteren we onze vastleggen op basis van de poort.

Het filter in dit scenario zou zijn:

```
tcp.port == 111
```

We de Filtertekst hierboven invoeren in het filtertekstvak en druk op enter.

![Afbeelding 6][6]

Vanuit de resultaten zien we dat al het verkeer afkomstig is van een lokale virtuele machine in hetzelfde subnet. Als we nog steeds niet waarom dit verkeer plaatsvindt begrijpen, kunnen we de pakketten om te bepalen waarom het ervoor zorgt dat deze aanroepen op poort 111 verder controleren. Met deze informatie kunnen we de juiste actie ondernemen.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de andere diagnostische functies van Network Watcher, recentst [Azure-netwerk bewakingsoverzicht](network-watcher-monitoring-overview.md)

[1]: ./media/network-watcher-deep-packet-inspection/figure1.png
[2]: ./media/network-watcher-deep-packet-inspection/figure2.png
[3]: ./media/network-watcher-deep-packet-inspection/figure3.png
[4]: ./media/network-watcher-deep-packet-inspection/figure4.png
[5]: ./media/network-watcher-deep-packet-inspection/figure5.png
[6]: ./media/network-watcher-deep-packet-inspection/figure6.png
[7]: ./media/network-watcher-deep-packet-inspection/figure7.png
[8]: ./media/network-watcher-deep-packet-inspection/figure8.png













