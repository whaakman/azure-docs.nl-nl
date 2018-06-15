---
title: Pakketinspecties met Azure-netwerk-Watcher | Microsoft Docs
description: Dit artikel wordt beschreven hoe u met netwerk-Watcher grondige pakketinspecties verzameld van een virtuele machine uitvoeren
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
ms.openlocfilehash: 1ad6ca4abe73336ce9ce3539fdaf2a9d7dd23fa6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
ms.locfileid: "23863970"
---
# <a name="packet-inspection-with-azure-network-watcher"></a>Pakketinspecties met Azure-netwerk-Watcher

Met de functie voor het vastleggen van pakket van netwerk-Watcher, kunt u starten en beheren van sessies mogelijk op uw Azure VM's via de portal, PowerShell, CLI en programmatisch via de SDK en de REST-API. Pakketopname kunt u de adres-scenario's waarvoor niveau pakketgegevens doordat de gegevens in een indeling die gemakkelijk kan worden gebruikt. Mogelijkheden van gratis hulpprogramma's voor het controleren van de gegevens, kunt u onderzoeken communicatie naar en van uw virtuele machines en inzicht in het netwerkverkeer. Sommige toepassingen voorbeeld van Pakketgegevens vastleggen zijn onder andere: onderzoeken van problemen met netwerk of de toepassing, de pogingen misbruik en inbraakdetectie netwerk te detecteren of naleving van regelgeving onderhouden. In dit artikel, laten we zien hoe een pakket vastleggen bestand geleverd door de netwerk-Watcher te openen met behulp van een populaire open-source hulpprogramma. We bieden ook voorbeelden waarin wordt getoond hoe een latentie van de verbinding berekenen, abnormaal verkeer identificeren en netwerken statistieken onderzoeken.

## <a name="before-you-begin"></a>Voordat u begint

In dit artikel gaat via een aantal vooraf geconfigureerde scenario's op een pakketopname die eerder is uitgevoerd. Deze scenario's illustreren mogelijkheden die aan de hand van een pakketopname kunnen worden geopend. Dit scenario wordt gebruikgemaakt van [WireShark](https://www.wireshark.org/) de pakketopname controleren.

Dit scenario wordt ervan uitgegaan dat u al een pakketopname uitgevoerd op een virtuele machine. Voor informatie over het maken van een pakket vastleggen Bezoek [beheren pakket worden vastgelegd met de portal](network-watcher-packet-capture-manage-portal.md) of met REST in via [pakket vastgelegd beheren met de REST-API](network-watcher-packet-capture-manage-rest.md).

## <a name="scenario"></a>Scenario

In dit scenario u:

* Bekijk een pakketopname

## <a name="calculate-network-latency"></a>Netwerklatentie berekenen

In dit scenario, laten we zien hoe de initiële Round Trip Time (RTT) van een Transmission Control Protocol (TCP) conversatie plaatsvinden tussen twee eindpunten weergeven.

Wanneer een TCP-verbinding tot stand is gebracht, volgt u de eerste drie pakketten worden verzonden in de verbinding een patroon vaak aangeduid als de handshake drie richtingen. Door in de eerste twee pakketten worden verzonden in deze-handshake wordt een eerste verzoek van de client en een reactie van de server, kunnen we de latentie berekenen wanneer deze verbinding tot stand is gebracht. Deze latentie wordt aangeduid als de Round Trip Time (RTT). Raadpleeg de volgende bron voor meer informatie over het TCP-protocol en de handshake drie richtingen. https://support.Microsoft.com/en-US/Help/172983/Explanation-of-the-three-way-handshake-via-TCP-IP

### <a name="step-1"></a>Stap 1

WireShark starten

### <a name="step-2"></a>Stap 2

Load de **CAP** bestand van uw pakketopname. Dit bestand kan gevonden worden in de blob machine werd opgeslagen in onze lokaal op de virtuele machine, afhankelijk van hoe u geconfigureerd.

### <a name="step-3"></a>Stap 3

Om weer te geven de initiële Round Trip Time (RTT) in de TCP-gesprekken, we alleen voor het bekijken van de eerste twee pakketten die zijn betrokken bij de TCP-handshake. We gebruiken de eerste twee pakketten in de handshake drie richtingen die de [SYN], [SYN, ACK] pakketten. Ze zijn met de naam van de vlaggen ingesteld in de TCP-header. Het laatste pakket in de handshake, het pakket [ACK] wordt niet gebruikt in dit scenario. Het pakket [SYN] wordt verzonden door de client. Zodra dit is ontvangen verzendt de server [ACK]-pakket als een bevestiging van ontvangst van de SYN van de client. Mogelijkheden van het feit dat het antwoord van de server weinig overhead vereist, we berekent de RTT door af te trekken van de tijd de [SYN, ACK] pakket is ontvangen door de client door de tijd [SYN] pakket is verzonden door de client.

WireShark met is deze waarde berekend voor ons.

Om het gemakkelijker de eerste twee pakketten in de TCP-handshake drie richtingen bekijken, maken we filteren gebruiker krijgt de mogelijkheid door WireShark gebruik.

Als u wilt toepassen op het filter in WireShark, vouw het Segment 'Transmission Control Protocol' van een pakket [SYN] in uw vastleggen en bekijk de vlaggen ingesteld in de TCP-header.

Aangezien we willen filteren op alle [SYN] en [SYN ACK] pakketten onder vlaggen cofirm dat de Syn-bit is ingesteld op 1 en rechts Klik op de Syn-bit -> toepassen als Filter -> geselecteerd.

![Afbeelding 7][7]

### <a name="step-4"></a>Stap 4

Nu dat u hebt gefilterd om te zien alleen de pakketten met de [SYN] bit is ingesteld, kunt u eenvoudig conversaties die u geïnteresseerd bent in de eerste RTT weergeven. Een eenvoudige manier om weer te geven van de RTT in WireShark Klik op de vervolgkeuzelijst ' SEQ/ACK' analyse gemarkeerd. Vervolgens ziet u de RTT weergegeven. In dit geval is de RTT 0.0022114 seconden of 2.211 ms.

![afbeelding 8][8]

## <a name="unwanted-protocols"></a>Ongewenste protocollen

U kunt hebben veel toepassingen die worden uitgevoerd op de instantie van een virtuele machine die u hebt geïmplementeerd in Azure. Veel van deze toepassingen communiceren via het netwerk mogelijk zonder uw expliciete toestemming. Pakketopname gebruiken voor het opslaan van netwerkcommunicatie, kunt we hoe de toepassing wordt gesproken op het netwerk en zoek naar problemen onderzoeken.

In dit voorbeeld we bekijken een eerdere pakketopname voor ongewenste protocollen die mogelijk aangeven onbevoegde communicatie van een toepassing die wordt uitgevoerd op uw computer uitgevoerd.

### <a name="step-1"></a>Stap 1

Klik met de dezelfde opname in het voorgaande scenario **statistieken** > **Protocol hiërarchie**

![Protocol hiërarchie menu][2]

Het protocol hiërarchie-venster wordt weergegeven. Deze weergave bevat een lijst van alle protocollen die werden gebruikt tijdens de opnamesessie en het aantal pakketten verzonden en ontvangen van de te gebruiken. Deze weergave is handig voor het vinden van ongewenste netwerkverkeer op de virtuele machines of het netwerk.

![protocol configureren geopend][3]

Zoals u in de volgende schermopname ziet, moet u er verkeer dat gebruikmaakt van het protocol BitTorrent, dat wordt gebruikt voor het delen van bestanden van de peer-to-peer is. Als een beheerder u niet van plan bent om te zien BitTorrent verkeer op deze bepaalde virtuele machines. Nu u op de hoogte van dit verkeer, kunt u verwijderen de peer-to-peer-software die op deze virtuele machine is geïnstalleerd, of het verkeer met behulp van Netwerkbeveiligingsgroepen of Firewall blokkeren. U kunt bovendien ervoor kiezen pakket opnamen uitvoeren volgens een schema, zodat u kunt het gebruik van het protocol op uw virtuele machines regelmatig bekijken. Voor een voorbeeld over het automatiseren van netwerktaken in azure gaat u naar [netwerkbronnen met azure automation bewaken](network-watcher-monitor-with-azure-automation.md)

## <a name="finding-top-destinations-and-ports"></a>Meest bezochte bestemmingen en poorten vinden

Informatie over de typen verkeer, de eindpunten en de poorten die via gecommuniceerd is een belangrijk bij het bewaken of het oplossen van toepassingen en bronnen in uw netwerk. Met behulp van een bestand voor het vastleggen van pakket van boven, kunnen we snel leren de bovenste bestemmingen die onze VM communiceert en de poorten die worden gebruikt.

### <a name="step-1"></a>Stap 1

Klik met de dezelfde opname in het voorgaande scenario **statistieken** > **IPv4-statistieken** > **bestemmingen en poorten**

![venster van pakket vastleggen][4]

### <a name="step-2"></a>Stap 2

Als we door de resultaten die een regel opvalt bekijken, zijn er meerdere verbindingen via poort 111. De meest gebruikte poort 3389, is dit de extern bureaublad is en de resterende RPC dynamische poorten zijn.

Hoewel dit verkeer betekenen er niets dat kan, is een poort die is gebruikt voor het aantal verbindingen en is onbekend bij de beheerder.

![Afbeelding 5][5]

### <a name="step-3"></a>Stap 3

Nu dat we hebben vastgesteld dat een out-of plaats poort filteren we onze vastleggen op basis van de poort.

Het filter in dit scenario zou zijn:

```
tcp.port == 111
```

We de Filtertekst uit boven in het tekstvak filter invoeren en op ENTER drukken.

![Afbeelding 6][6]

In de resultaten ziet u dat alle verkeer afkomstig is van een lokale virtuele machine op hetzelfde subnet. Als we nog steeds niet waarom dit verkeer plaatsvindt begrijpen, kunnen we de pakketten om te bepalen waarom het aan te brengen deze aanroepen op poort 111 verdere inspecteren. Met deze informatie kunnen we de juiste actie ondernemen.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de andere diagnostische functies van netwerk-Watcher in via [bewakingsoverzicht Azure-netwerk](network-watcher-monitoring-overview.md)

[1]: ./media/network-watcher-deep-packet-inspection/figure1.png
[2]: ./media/network-watcher-deep-packet-inspection/figure2.png
[3]: ./media/network-watcher-deep-packet-inspection/figure3.png
[4]: ./media/network-watcher-deep-packet-inspection/figure4.png
[5]: ./media/network-watcher-deep-packet-inspection/figure5.png
[6]: ./media/network-watcher-deep-packet-inspection/figure6.png
[7]: ./media/network-watcher-deep-packet-inspection/figure7.png
[8]: ./media/network-watcher-deep-packet-inspection/figure8.png













