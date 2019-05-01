---
title: TCP/IP-prestaties afstemmen voor virtuele Azure-machines | Microsoft Docs
description: Meer informatie over verschillende algemene TCP/IP prestaties afstemmen technieken en hun relatie tot de Azure-VM's.
services: virtual-network
documentationcenter: na
author:
- rimayber
- dgoddard
- stegag
- steveesp
- minale
- btalb
- prachank
manager: paragk
editor: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/02/2019
ms.author:
- rimayber
- dgoddard
- stegag
- steveesp
- minale
- btalb
- prachank
ms.openlocfilehash: 31ca0ee666ff37afa37fb9636860c557d92a52c7
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/30/2019
ms.locfileid: "64924794"
---
# <a name="tcpip-performance-tuning-for-azure-vms"></a>TCP/IP-prestaties afstemmen voor virtuele Azure-machines

Dit artikel worden algemene TCP/IP prestaties afstemmen technieken en enkele aandachtspunten voor wanneer u deze gebruiken voor virtuele machines die worden uitgevoerd op Azure. Het wordt bieden een eenvoudig overzicht van de technieken en ontdek hoe ze kunnen worden afgestemd.

## <a name="common-tcpip-tuning-techniques"></a>Veelgebruikte methoden voor TCP/IP-afstemmen

### <a name="mtu-fragmentation-and-large-send-offload"></a>MTU fragmentatie en grote verzendings-offload

#### <a name="mtu"></a>MTU

De maximale verzendeenheid (MTU) is het grootste grootte frame (pakket), opgegeven in bytes, die kunnen worden verzonden via een netwerkinterface. De MTU is een configureerbare instelling. De standaard-MTU gebruikt op Azure Virtual machines en de standaardinstelling voor de meeste netwerkapparaten is wereldwijd 1500 bytes.

#### <a name="fragmentation"></a>Fragmentatie

Fragmentatie treedt op wanneer een pakket wordt verzonden die groter is dan de MTU van een netwerkinterface. De TCP/IP-stack wordt het pakket opsplitsen in kleinere delen (fragmenten) die aan de MTU van de interface voldoen. Fragmentatie wordt uitgevoerd op de IP-laag en is onafhankelijk van het onderliggende protocol (zoals TCP). Wanneer een 2000-byte-pakket wordt verzonden via een netwerkinterface met een MTU van 1500, wordt het pakket worden opgesplitst in één 1500-byte pakket- en één 500-byte-pakket.

Netwerkapparaten in het pad tussen een bron en bestemming kunnen een van beide drop-pakketten die groter zijn dan de MTU of het pakket opsplitsen in kleinere delen.

#### <a name="the-dont-fragment-bit-in-an-ip-packet"></a>Het Fragment niet bit in een IP-pakket

De bits niet Fragment (DF) is een markering in de koptekst van de IP-protocol. De DF-bit geeft aan dat netwerkapparaten op het pad tussen de afzender en de ontvanger moeten het pakket fragment. Dit deel kan worden ingesteld om verschillende redenen. (Zie de sectie 'Path MTU Discovery' van dit artikel voor een voorbeeld.) Wanneer u een netwerkapparaat ontvangt een pakket met de niet-Fragment bit is ingesteld en dat pakket overschrijdt de apparaatinterface MTU, is het standaardgedrag voor het apparaat verwijderen van het pakket. Het apparaat verzendt een bericht ICMP fragmentatie nodig terug naar de oorspronkelijke bron van het pakket.

#### <a name="performance-implications-of-fragmentation"></a>Gevolgen voor de prestaties van fragmentatie

Fragmentatie kan negatieve prestaties gevolgen hebben. Een van de belangrijkste redenen voor het effect op de prestaties is de impact van de CPU/geheugen van de fragmentatie en samenvoegen van pakketten. Wanneer een netwerkapparaat moet het fragment van een pakket, is er resources om uit te voeren van fragmentatie van de CPU/geheugen worden toegewezen.

Hetzelfde gebeurt wanneer het pakket opnieuw is samengesteld. Het netwerkapparaat heeft voor het opslaan van alle fragmenten totdat ze worden ontvangen, zodat deze ze in het oorspronkelijke pakket samenstellen kan. Dit proces van fragmentatie en samenvoegen kan ook leiden tot latentie.

De andere mogelijke negatieve prestaties implicatie van fragmentatie is dat gefragmenteerde pakketten, niet de juiste volgorde aankomen kunnen. Wanneer pakketten worden ontvangen niet de juiste volgorde, kunnen bepaalde typen netwerkapparaten te verwijderen. Wanneer dit gebeurt, is het volledige pakket opnieuw worden verzonden.

Fragmenten worden doorgaans verwijderd door beveiligingsapparaten, zoals netwerkfirewalls, of wanneer het ontvangen van een netwerkapparaat buffers zijn uitgeput. Wanneer het ontvangen van een netwerkapparaat buffers zijn uitgeput, een Cisco-apparaat probeert te opnieuw samenstellen van een gefragmenteerde pakketten, maar beschikt niet over de resources voor het opslaan en reassume van het pakket.

Fragmentatie kan worden gezien als een negatieve bewerking, maar ondersteuning voor fragmentatie nodig is wanneer u verschillende netwerken verbinding via internet maakt.

#### <a name="benefits-and-consequences-of-modifying-the-mtu"></a>Voordelen en de gevolgen van het wijzigen van de MTU

In het algemeen kunt u een efficiëntere netwerk door het verhogen van de MTU maken. Elk pakket dat wordt verzonden is header-informatie die wordt toegevoegd aan het oorspronkelijke pakket. Wanneer fragmentatie meer pakketten maakt, er is meer header overhead en daardoor is het netwerk minder efficiënt.

Hier volgt een voorbeeld. De Ethernet-header-grootte is bevatte slechts 14 bytes plus een reeks 4-bytes frame check frame van de consistentie. Als één 2000-byte pakket wordt verzonden, wordt 18 bytes van Ethernet-overhead toegevoegd op het netwerk. Als het pakket in een pakket 1500 bytes en een 500-byte pakket gefragmenteerd is, wordt elk pakket 18 bytes van Ethernet-header, een totaal van 36 bytes hebben.

Houd er rekening mee dat de MTU verhogen niet per se een efficiëntere netwerk gemaakt. Als een toepassing alleen pakketten die 500-byte verzendt, zullen de dezelfde overhead van de header of de MTU 1500 of 9000 bytes is bestaan. Het netwerk wordt efficiënter werken alleen als het pakket grotere die worden beïnvloed door de MTU gebruikt.

#### <a name="azure-and-vm-mtu"></a>Azure en de MTU van de virtuele machine

De MTU voor Azure VM's van de standaardwaarde is 1500 bytes. De Azure Virtual Network-stack probeert het fragment van een pakket 1.400 bytes. Maar de virtuele-netwerkstack kunnen pakketten tot 2,006 bytes als het Fragment niet-bit is ingesteld in de IP-header.

Houd er rekening mee dat de virtuele netwerkstack niet inherent inefficiënt omdat deze pakketten op 1.400 bytes fragmenten, zelfs als de virtuele machines hebben een MTU van 1500. Een groot percentage van netwerkpakketten veel kleiner is dan 1.400 of 1500 bytes zijn.

#### <a name="azure-and-fragmentation"></a>Azure en fragmentatie

Virtueel netwerkstack is ingesteld om te verwijderen 'niet-geordende fragmenten,' dat wil zeggen, gefragmenteerde pakketten die niet in de oorspronkelijke gefragmenteerde volgorde binnenkomen. Deze pakketten zijn verwijderd voornamelijk vanwege een beveiligingsprobleem voor netwerk aangekondigd in November 2018 FragmentSmack genoemd.

FragmentSmack is een fout in de manier waarop de Linux-kernel opnieuw samenstellen van gefragmenteerde pakketten voor IPv4 en IPv6 verwerkt. Een externe aanvaller kan deze fout trigger dure fragment opnieuw samenstellen bewerkingen, die tot hogere CPU- en een DoS-aanval op het doelsysteem leiden kunnen gebruiken.

#### <a name="tune-the-mtu"></a>De MTU afstemmen

U kunt een Azure VM-MTU configureren als in een ander besturingssysteem. Maar u moet rekening houden met de fragmentatie die wordt uitgevoerd in Azure, hierboven beschreven, wanneer u een MTU configureert.

We Moedig geen klanten aan het verhogen van de MTU van de virtuele machine. Deze beschrijving is bedoeld om uit te leggen van de details van hoe Azure MTU implementeert en uitvoert fragmentatie.

> [!IMPORTANT]
>MTU is niet bekend is dat de prestaties verbeteren en kan een negatief effect hebben op de prestaties van toepassingen.
>
>

#### <a name="large-send-offload"></a>Grote verzendings-offload

Grote verzendings-offload (LSO) kunt verbetering van netwerkprestaties door het offloaden van de segmentatie van pakketten naar de Ethernet-adapter. Wanneer LSO is ingeschakeld, wordt de TCP/IP-stack maakt u een grote TCP-pakket en verzendt dit naar de Ethernet-adapter voor de segmentering voordat deze worden doorgestuurd. Het voordeel van LSO is dat deze de CPU van pakketten segmenteren in grootten die voldoen aan de MTU en die verwerking die aan de Ethernet-interface waar deze wordt uitgevoerd in de hardware-offload kunt vrijmaken. Zie voor meer informatie over de voordelen van LSO [ondersteuning van grote verzendings-offload](https://docs.microsoft.com/windows-hardware/drivers/network/performance-in-network-adapters#supporting-large-send-offload-lso).

Wanneer LSO is ingeschakeld, Azure klanten zien mogelijk grote frame-formaten wanneer ze pakketopnamen uitvoeren. Met deze framegrootten zijn grote kunnen leiden sommige klanten om na te denken fragmentatie optreedt of dat een grote MTU wanneer deze niet wordt gebruikt. Met LSO, kan de Ethernet-adapter een grotere maximale segmentgrootte (MSS) adverteren naar de TCP/IP-stack maken van een grotere TCP-pakket. Dit gehele niet-gesegmenteerde frame wordt vervolgens doorgestuurd naar de Ethernet-adapter en zichtbaar waren in een pakketopname uitgevoerd op de virtuele machine. Maar het pakket wordt worden opgesplitst in veel kleinere frames van de Ethernet-adapter, op basis van de MTU van de Ethernet-adapter.

### <a name="tcp-mss-window-scaling-and-pmtud"></a>Schalen van MSS TCP-venster en PMTUD

#### <a name="tcp-maximum-segment-size"></a>De maximale segmentgrootte TCP

Maximale segmentgrootte van TCP (MSS) is een instelling waardoor de grootte van de TCP-segmenten, waarmee wordt voorkomen fragmentatie van TCP-pakketten. Deze formule wordt doorgaans gebruikt om in te stellen MSS operating systems:

`MSS = MTU - (IP header size + TCP header size)`

De IP-header en de TCP-header zijn 20 bytes en totaal aantal 40 bytes. Een interface met een MTU van 1500 heeft dus een MSS van 1,460. Maar de MSS kan worden geconfigureerd.

Deze instelling wordt geaccepteerd in de TCP-handshake drie richtingen wanneer een TCP-sessie is ingesteld tussen een bron en bestemming. Beide zijden een waarde MSS verzenden en de laagste waarde van de twee wordt gebruikt voor de TCP-verbinding.

Houd er rekening mee dat de MTU van de bron en bestemming zijn niet de enige factoren die de waarde MSS bepalen. Tussenliggende netwerkapparaten, zoals VPN-gateways, met inbegrip van Azure VPN-Gateway, de MTU onafhankelijk van de bron- en om ervoor te zorgen optimale netwerkprestaties kunnen aanpassen.

#### <a name="path-mtu-discovery"></a>Path MTU Discovery

MSS wordt onderhandeld over, maar deze mogelijk niet de werkelijke MSS die kunnen worden gebruikt. Dit komt doordat andere netwerkapparaten in het pad tussen de bron en bestemming mogelijk een lagere MTU-waarde dan de bron- en doelserver. Het apparaat waarvan MTU kleiner dan het pakket is wordt in dit geval wordt het pakket te verwijderen. Het apparaat wordt verzenden terug een ICMP-fragmentatie nodig (Type 3, 4-Code) die de MTU bevat. Dit bericht ICMP kan de bronhost te verminderen van de MTU-pad op de juiste wijze. Het proces wordt Path MTU Discovery (PMTUD) genoemd.

Het proces PMTUD is inefficiënt en is van invloed op prestaties van het netwerk. Wanneer pakketten worden verzonden die groter zijn dan een netwerkpad MTU, moeten de pakketten met een lagere MSS worden doorgegeven. Als de afzender bericht heeft ontvangen de ICMP-fragmentatie die nodig zijn, mogelijk vanwege de firewall van een netwerk in het pad (vaak aangeduid als een *PMTUD Zwarte gat*), de afzender niet weten moet het verlagen van de MSS en continu wordt het pakket opnieuw. Dit is de reden waarom wordt niet aanbevolen voor het verhogen van de Azure VM-MTU.

#### <a name="vpn-and-mtu"></a>VPN- en MTU

Als u virtuele machines die encapsulation (zoals IPsec VPN's uitvoeren) gebruikt, zijn er enkele aanvullende overwegingen met betrekking tot pakketgrootte en MTU. VPN-verbindingen toevoegen meer kopteksten voor pakketten, die neemt de omvang van het pakket en een kleinere MSS vereist.

Voor Azure, wordt aangeraden dat TCP MSS MSS 1.350 bytes ingesteld en tunnel interface MTU 1400. Zie voor meer informatie de [VPN-apparaten en IPSec/IKE-parameters pagina](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices).

### <a name="latency-round-trip-time-and-tcp-window-scaling"></a>Latentie, retourtijd en schalen van TCP-venster

#### <a name="latency-and-round-trip-time"></a>Latentie en round-trip tijd

De netwerklatentie wordt geregeld door de snelheid van licht via een glasvezelkabels fiber-netwerk. Netwerkdoorvoer van TCP-valt ook effectief de retourtijd (RTT) tussen twee netwerkapparaten.

| | | | |
|-|-|-|-|
|**Route**|**afstand**|**Eenzijdige tijd**|**RTT**|
|New York met San Francisco|4,148 km|21 ms|42 ms|
|New York naar Londen|5,585 km|28 ms|56 ms|
|New York met Sydney|15,993 km|80 ms|160 ms|

Deze tabel bevat de lineaire afstand tussen twee locaties. De afstand wordt doorgaans meer is dan de lineaire afstand in netwerken. Dit is een eenvoudige formule voor het berekenen van minimale RTT zoals bepaald door de snelheid van licht:

`minimum RTT = 2 * (Distance in kilometers / Speed of propagation)`

U kunt 200 gebruiken voor de snelheid van doorgeven. Dit is de afstand in meters, dat licht wordt verzonden in 1 milliseconde.

We gaan New York op San Francisco als voorbeeld. De lineaire afstand is 4,148 km. Aan te sluiten die waarde in de vergelijking, krijgen we het volgende:

`Minimum RTT = 2 * (4,148 / 200)`

De uitvoer van de vergelijking is in milliseconden.

Als u ophalen van de beste prestaties van het netwerk wilt, wordt de logische optie is om te selecteren van doelen met de kortste afstand tussen beide. Ook moet u het virtuele netwerk voor het pad van het verkeer te optimaliseren en verminder de latentie ontwerpen. Zie de sectie 'Ontwerpoverwegingen netwerk' in dit artikel voor meer informatie.

#### <a name="latency-and-round-trip-time-effects-on-tcp"></a>Latentie en round-trip tijd effecten op TCP

Retourtijd heeft een directe invloed op maximale doorvoer van TCP. In de TCP-protocol, *venstergrootte* is de maximale hoeveelheid verkeer die kan worden verzonden via een TCP-verbinding voordat de afzender moet bevestiging ontvangen van de ontvanger. Als de TCP-MSS is ingesteld op 1,460 en de TCP-venstergrootte tot 65.535 is ingesteld, kunt de afzender 45 pakketten verzenden voordat er bevestiging ontvangen van de ontvanger. Als de afzender geen bevestiging, wordt deze de gegevens opnieuw. Dit is de formule:

`TCP window size / TCP MSS = packets sent`

In dit voorbeeld 65.535 / 1,460 wordt afgerond tot 45.

Deze status 'wachten op bevestiging voor', een mechanisme om ervoor te zorgen betrouwbare levering van gegevens, is wat ervoor zorgt dat RTT invloed heeft op TCP-doorvoer. Hoe langer de afzender wacht op bevestiging, hoe langer moet wachten voordat meer gegevens worden verzonden.

Dit is de formule voor het berekenen van de maximale doorvoer van een TCP-verbinding:

`Window size / (RTT latency in milliseconds / 1,000) = maximum bytes/second`

Deze tabel ziet u het maximum aantal MB per seconde doorvoer van een TCP-verbinding. (Voor de leesbaarheid, in megabytes wordt gebruikt voor de maateenheid.)

| | | | |
|-|-|-|-|
|**TCP-venstergrootte (bytes)**|**RTT latentie (ms)**|**Maximum aantal MB/seconde doorvoer**|**Maximale megabit per seconde doorvoer**|
|65,535|1|65.54|524.29|
|65,535|30|2.18|17.48|
|65,535|60|1.09|8.74|
|65,535|90|.73|5.83|
|65,535|120|.55|4.37|

Als pakketten verloren gaan, wordt de maximale doorvoer van een TCP-verbinding wordt verlaagd wanneer de afzender stuurt gegevens die al is verzonden.

#### <a name="tcp-window-scaling"></a>TCP-venster schalen

Schalen van TCP-venster is een techniek die dynamisch de TCP-venstergrootte verhoogt zodat meer gegevens worden verzonden voordat een bevestiging vereist is. In het vorige voorbeeld, zou 45 pakketten worden verzonden voordat een bevestiging vereist is. Als u het aantal pakketten dat kan worden verzonden verhoogt voordat een bevestiging nodig is, bent u het aantal keren dat die een afzender wacht op bevestiging, waardoor de maximale doorvoer van TCP verlagen.

Deze tabel ziet u deze relaties:

| | | | |
|-|-|-|-|
|**TCP-venstergrootte (bytes)**|**RTT latentie (ms)**|**Maximum aantal MB/seconde doorvoer**|**Maximale megabit per seconde doorvoer**|
|65,535|30|2.18|17.48|
|131,070|30|4.37|34.95|
|262,140|30|8.74|69.91|
|524,280|30|17.48|139.81|

Maar de waarde van de TCP-header voor de TCP-venstergrootte is alleen 2 bytes lang, wat betekent dat de maximale waarde voor een receive venster is 65.535. Als u wilt vergroten de maximale venstergrootte, is de schaalfactor van een TCP-venster ingevoerd.

De schaalfactor is ook een instelling die u in een besturingssysteem configureren kunt. Dit is de formule voor het berekenen van de TCP-venstergrootte met behulp van de schaalfactoren:

`TCP window size = TCP window size in bytes \* (2^scale factor)`

Dit is de berekening voor een schaalfactor venster van de 3 en de venstergrootte van een van 65.535:

`65,535 \* (2^3) = 262,140 bytes`

Een schaalfactor van 14 resulteert in een TCP-venstergrootte van 14 (de maximale offset toegestaan). De TCP-venstergrootte worden 1,073,725,440 bytes (8,5 Gigabit).

#### <a name="support-for-tcp-window-scaling"></a>Ondersteuning voor het schalen van TCP-venster

Windows kan verschillende vergroten/verkleinen factoren ingesteld voor verschillende verbindingstypen. (Klassen van verbindingen omvatten datacenter, internet, enzovoort.) U gebruikt de `Get-NetTCPConnection` PowerShell-opdracht uit om het venster verbindingstype schalen weer te geven:

```powershell
Get-NetTCPConnection
```

U kunt de `Get-NetTCPSetting` PowerShell-opdracht uit om de waarden van elke objectklasse weer te geven:

```powershell
Get-NetTCPSetting
```

U kunt de initiële TCP-venstergrootte en de TCP-schaalfactor in Windows instellen met behulp van de `Set-NetTCPSetting` PowerShell-opdracht. Zie voor meer informatie, [Set NetTCPSetting](https://docs.microsoft.com/powershell/module/nettcpip/set-nettcpsetting?view=win10-ps).

```powershell
Set-NetTCPSetting
```

Dit zijn de actieve TCP-instellingen voor `AutoTuningLevel`:

| | | | |
|-|-|-|-|
|**AutoTuningLevel**|**Schaalfactor**|**Vermenigvuldiger voor vergroten/verkleinen**|**Formule aan<br/>maximale window-grootte berekenen**|
|Uitgeschakeld|Geen|Geen|Venstergrootte|
|Beperkt|4|2^4|Venstergrootte * (2 ^ 4)|
|Zeer beperkt|2|2^2|Venstergrootte * (2 ^ 2)|
|Normaal|8|2^8|Window size * (2^8)|
|Experimenteel|14|2^14|Venstergrootte * (2 ^ 14)|

Deze instellingen zijn het meest waarschijnlijk invloed op TCP-prestaties, maar houd er rekening mee dat veel andere factoren op het internet, buiten het besturingselement van Azure, ook invloed hebben op TCP-prestaties.

#### <a name="increase-mtu-size"></a>MTU-grootte verhogen

Omdat een grotere MTU een grotere MSS betekent, kunt u zich afvragen of het verhogen van de MTU TCP-prestaties kan verhogen. Waarschijnlijk niet. Er zijn voor- en nadelen te pakketgrootte meer dan alleen TCP-verkeer. Zoals eerder besproken, zijn de belangrijkste factoren die invloed hebben op prestaties van de netwerkdoorvoer TCP-TCP-venstergrootte pakketverlies en RTT.

> [!IMPORTANT]
> We raden niet met het Azure-klanten de standaardwaarde van de MTU op virtuele machines te wijzigen.
>
>

### <a name="accelerated-networking-and-receive-side-scaling"></a>Versnelde netwerken en aan de ontvangstzijde

#### <a name="accelerated-networking"></a>Versneld netwerken

Virtuele machine-netwerkfuncties zijn in het verleden CPU intensieve op zowel de Gast-VM en de hypervisorhost. Elk pakket dat passages via de host wordt verwerkt in software door de host CPU, met inbegrip van alle virtuele netwerk inkapselen en uitpakken. Dus meer gaat het verkeer dat via de host, hoe hoger CPU-belasting. En als de host CPU Bezig met andere bewerkingen is, die ook van invloed op netwerkdoorvoer en latentie. Azure lost dit probleem met versneld netwerken.

Versneld netwerken biedt consistente ultralow netwerklatentie via de interne programmeerbare hardware van Azure en technologieën zoals SR-IOV. Versneld netwerken worden veel van de Azure-software gedefinieerde netwerkstack uit de CPU's en in SmartNICs FPGA gebaseerde verplaatst. Deze wijziging kunt toepassingen voor eindgebruikers het vrijmaken van compute-cycli, plaatst u minder belasting op de virtuele machine, jitter en inconsistentie in de latentie verlagen. Prestaties kan met andere woorden, zijn meer deterministische.

Versneld netwerken verbetert de prestaties doordat de Gast-VM omzeilen van de host en een gegevenspad rechtstreeks met een host SmartNIC tot stand brengen. Hier volgen enkele voordelen van versneld netwerken:

- **Lagere latentie / hoger pakketten per seconde (pps)**: De virtuele switch verwijderen uit het gegevenspad elimineert de tijd die pakketten in de host voor de beleidsverwerking van besteden en het aantal pakketten dat kan worden verwerkt in de virtuele machine wordt verhoogd.

- **Minder jitter**: Verwerking van de virtuele switch, is afhankelijk van de hoeveelheid beleid die moet worden toegepast en de werkbelasting van de CPU dat de verwerking. Offloading van het afdwingen van beleid voor de hardware verwijdert die variabiliteit door het leveren van pakketten rechtstreeks naar de virtuele machine, zodat de communicatie van de host-VM en alle software-interrupts en context switches.

- **CPU-gebruik verlaagd**: Overslaan van de virtuele switch op de host leidt tot minder CPU-gebruik voor het verwerken van netwerkverkeer.

Voor het gebruik van versneld netwerken, moet u expliciet inschakelen op elke toepasselijke VM. Zie [maken van een virtuele Linux-machine met versnelde netwerken](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli) voor instructies.

#### <a name="receive-side-scaling"></a>Aan de ontvangstzijde

Ontvangen aan clientzijde die schalen (RSS) is een technologie voor het stuurprogramma van network die het ontvangen van het netwerkverkeer efficiënter distribueert door te distribueren verwerking over meerdere CPU's ontvangen in een systeem met meerdere processors. RSS kan simpel gezegd is een systeem meer ontvangen verkeer verwerken, omdat alle beschikbare CPU's wordt gebruikt in plaats van slechts één. Zie voor meer technische informatie van RSS [Inleiding tot aan de ontvangstzijde](https://docs.microsoft.com/windows-hardware/drivers/network/introduction-to-receive-side-scaling).

Voor de beste prestaties wanneer versneld netwerken is ingeschakeld op een virtuele machine, moet u RSS inschakelen. RSS biedt ook voordelen op virtuele machines die geen gebruik van versnelde netwerken. Zie voor een overzicht van hoe u om te bepalen of RSS is ingeschakeld en het inschakelen ervan [netwerkdoorvoer optimaliseren voor Azure virtual machines](http://aka.ms/FastVM).

### <a name="tcp-timewait-and-timewait-assassination"></a>TCP TIME_WAIT en TIME_WAIT assassination

TCP TIME_WAIT is een andere algemene instelling die van invloed is op het netwerk- en toepassingsprestaties. Op actieve virtuele machines die zijn openen en sluiten van veel sockets, als clients of servers (IP:Source bronpoort + IP:Destination doelpoort), tijdens de normale werking van TCP, kunt een bepaalde socket krijgen in een status TIME_WAIT gedurende een lange periode. De TIME_WAIT-status is bedoeld om toe te staan van extra gegevens worden bezorgd op een socket voordat deze wordt gesloten. TCP/IP-stacks dat het hergebruik van een socket dus in het algemeen door op de achtergrond verwijderen van de client TCP SYN-pakket.

De hoeveelheid tijd die een socket in TIME_WAIT is worden geconfigureerd. Dit kan variëren van 30 seconden tot 240 seconden. Sockets zijn een eindige resource en het aantal sockets die kunnen worden gebruikt op een bepaald moment kan worden geconfigureerd. (Het aantal beschikbare sockets is meestal ongeveer 30.000.) Als de beschikbare sockets verbruikt, of als clients en servers hebt niet-overeenkomende TIME_WAIT-instellingen en een virtuele machine probeert om een socket in een TIME_WAIT-status opnieuw te gebruiken, wordt nieuwe verbindingen mislukken als TCP SYN op de achtergrond worden pakketten verwijderd.

De waarde voor het bereik van poorten voor uitgaande sockets is gewoonlijk worden geconfigureerd in de TCP/IP-stack van een besturingssysteem. Hetzelfde geldt voor TCP TIME_WAIT-instellingen en socket hergebruik. Als u deze getallen wijzigt, kan de schaalbaarheid mogelijk verbeteren. Echter, afhankelijk van de situatie, deze wijzigingen interoperabiliteit privacyproblemen kunnen veroorzaken. Wees voorzichtig als u deze waarden te wijzigen.

U kunt TIME_WAIT assassination gebruiken om deze beperking vergroten/verkleinen. TIME_WAIT assassination kan een socket opnieuw kan worden gebruikt in bepaalde gevallen, zoals wanneer het volgnummer in het IP-pakket van de nieuwe verbinding het volgnummer van het laatste pakket uit de vorige verbinding is overschreden. In dit geval wordt het besturingssysteem kan de nieuwe verbinding tot stand is gebracht (deze wordt de nieuwe SYN/ACK accepteren) en geforceerd sluiten de vorige verbinding die is een TIME_WAIT-status. Deze mogelijkheid wordt ondersteund op Windows-VM's in Azure. Voor meer informatie over ondersteuning in andere VM's, contact op met de leverancier van het besturingssysteem.

Zie voor meer informatie over het configureren van instellingen voor de TIME_WAIT TCP en poortbereik van bron, [instellingen die kunnen worden aangepast voor verbetering van netwerkprestaties](https://docs.microsoft.com/biztalk/technical-guides/settings-that-can-be-modified-to-improve-network-performance).

## <a name="virtual-network-factors-that-can-affect-performance"></a>Virtueel netwerk factoren die invloed kunnen hebben op prestaties

### <a name="vm-maximum-outbound-throughput"></a>Maximum aantal uitgaande doorvoer van de virtuele machine

Azure biedt tal van VM-grootten en -typen, elk met een andere combinatie van prestaties levert. Er is een van deze mogelijkheden netwerk doorvoer (of bandbreedte), die wordt gemeten in megabits per seconde (Mbps). Omdat virtuele machines worden gehost op gedeelde hardware, moet de netwerkcapaciteit redelijk worden gedeeld met de virtuele machines met dezelfde hardware. Grotere virtuele machines worden toegewezen aan meer bandbreedte dan kleinere virtuele machines.

De bandbreedte van het netwerk toegewezen aan elke virtuele machine wordt gemeten op de uitgaande gegevens (uitgaand) verkeer van de virtuele machine. Alle netwerkverkeer verlaten van de virtuele machine wordt meegeteld voor de limiet van de toegewezen, ongeacht de bestemming. Bijvoorbeeld, als een virtuele machine een limiet van 1000 Mbps heeft, geldt dat een limiet of het uitgaande verkeer bestemd voor een andere virtuele machine in hetzelfde virtuele netwerk of een buiten Azure is.

Inkomend verkeer wordt niet gemeten of die beperkt zijn rechtstreeks. Maar er zijn andere factoren, zoals CPU- en opslaglimieten die kunnen invloed hebben op de mogelijkheid voor het verwerken van binnenkomende gegevens van een virtuele machine.

Versneld netwerken is ontworpen voor het verbeteren van de prestaties van het netwerk, met inbegrip van latentie, doorvoer en CPU-gebruik. Versneld netwerken kan de doorvoer van een virtuele machine verbeteren, maar dit wordt mogelijk tot de virtuele machine toegewezen bandbreedte.

Azure virtuele machines hebben ten minste één netwerkinterface die is gekoppeld aan deze. Hebben ze mogelijk meerdere. De bandbreedte die is toegewezen aan een virtuele machine is de som van al het uitgaande verkeer voor alle netwerkinterfaces die zijn gekoppeld aan de machine. Met andere woorden, is de bandbreedte die toegewezen op basis van per virtuele machine, ongeacht het aantal netwerkinterfaces zijn gekoppeld aan de machine.

Verwachte uitgaande doorvoer en het aantal netwerkinterfaces dat wordt ondersteund door elke VM-grootte worden beschreven in [grootten voor Windows virtuele machines in Azure](https://docs.microsoft.com/azure/virtual-machines/windows/sizes?toc=%2fazure%2fvirtual-network%2ftoc.json). Als u wilt zien van maximale doorvoer, selecteer een type, zoals **algemeen**, en gaat u naar de sectie over de reeks grootte op de resulterende pagina (bijvoorbeeld ' Dv2-serie'). Voor elke reeks, wordt een tabel die voorziet in netwerken specificaties in de laatste kolom heet ' max. aantal NIC's / netwerkbandbreedte (Mbps) verwacht. "

De maximale doorvoer is van toepassing op de virtuele machine. Doorvoer wordt niet beïnvloed door deze factoren:

- **Het aantal netwerkinterfaces**: De limiet voor bandbreedte is van toepassing op de som van al het uitgaande verkeer van de virtuele machine.

- **Versnelde netwerken**: Hoewel deze functie het handig zijn bij het bereiken van de gepubliceerde limiet zijn kan, verandert deze niet de limiet.

- **Het doel van verkeer**: Alle bestemmingen tellen mee voor de limiet voor uitgaande.

- **Protocol**: Al het uitgaande verkeer via alle protocollen telt voor de limiet.

Zie voor meer informatie, [netwerkbandbreedte van de virtuele machine](http://aka.ms/AzureBandwidth).

### <a name="internet-performance-considerations"></a>Prestatieoverwegingen voor Internet

Zoals besproken in dit artikel, kunnen factoren op het internet en buiten de controle van Azure invloed hebben op prestaties van het netwerk. Hier volgen enkele van deze factoren:

- **Latentie**: De retourtijd tussen twee doelen kan worden beïnvloed door problemen op tussenliggende netwerken, door het verkeer die geen het "kortste" afstand pad, en door suboptimale-peeringpad.

- **Pakketverlies**: Pakketverlies kan worden veroorzaakt door opstoppingen in het netwerk, fysieke pad problemen en onder de maat presterende netwerkapparaten.

- **MTU grootte/fragmentatie**: Fragmentatie langs het pad kan leiden tot vertragingen in de ontvangst van gegevens of in pakketten die niet de juiste volgorde, die invloed kunnen zijn op de levering van pakketten.

Traceroute is een goed hulpprogramma voor het meten van netwerk prestatiekenmerken (zoals pakketverlies en latentie) aan elke netwerkpad tussen een Bronapparaat en een doelapparaat.

### <a name="network-design-considerations"></a>Overwegingen bij het ontwerpen van netwerk

Samen met de overwegingen die eerder in dit artikel wordt besproken, de topologie van een virtueel netwerk kan invloed hebben op prestaties van het netwerk. Bijvoorbeeld, een hub en spoke-ontwerp backhauls wereldwijd verkeer aan een virtueel netwerk met één hub introduceert versies netwerklatentie, die wordt invloed hebben op de algehele prestaties van het netwerk.

Het aantal apparaten dat netwerkverkeer wordt doorgegeven via kan ook van invloed op de totale latentie. Bijvoorbeeld in een hub en spoke-ontwerp, als het verkeer wordt doorgegeven via een virtueel netwerkapparaat van knooppunt en een virtueel apparaat hub voordat u met het internet, die de virtuele netwerkapparaten kunnen leiden tot latentie.

### <a name="azure-regions-virtual-networks-and-latency"></a>Azure-regio's, virtuele netwerken en latentie

Azure-regio's bestaan uit meerdere datacenters die zijn opgeslagen in een algemeen geografisch gebied. Deze datacenters mogelijk niet fysiek naast elkaar. In sommige gevallen bent ze van elkaar gescheiden door minder dan 10 kilometer zijn verwijderd. Het virtuele netwerk is een logische overlay boven op het netwerk van Azure-fysieke datacenter. Een virtueel netwerk, houdt dat niet elke specifieke netwerktopologie binnen het datacenter.

Twee virtuele machines die zich in hetzelfde virtuele netwerk en subnet kunnen bijvoorbeeld zijn in verschillende rekken, rijen of zelfs datacenters. Ze kunnen worden gescheiden door meter fiber glasvezelkabels kabel of kilometer van glasvezelkabels fiber-kabel. Deze afwijking kan leiden tot variabele latentie (enkele milliseconden verschil) tussen verschillende virtuele machines.

De geografische plaatsing van virtuele machines en de mogelijke resulterende latentie tussen twee virtuele machines, kunnen worden beïnvloed door de configuratie van beschikbaarheidssets en Beschikbaarheidszones. Maar de afstand tussen datacenters in een regio is regiospecifieke en voornamelijk beïnvloed door de datacenter-topologie in de regio.

### <a name="source-nat-port-exhaustion"></a>Poortuitputting bron-NAT

Een implementatie in Azure kan communiceren met eindpunten buiten Azure op het openbare internet en/of in de openbare IP-adresruimte. Wanneer een exemplaar van een uitgaande verbinding initieert, Azure dynamisch privé IP-adres toegewezen aan een openbaar IP-adres. Nadat Azure deze toewijzing maakt, kan retourverkeer voor de uitgaande stroom afkomstige ook bereiken het privé IP-adres waarvan de stroom afkomstig is.

Voor elke uitgaande verbinding moet de Azure Load Balancer onderhouden van deze toewijzing voor een bepaalde tijd. Met de multitenant aard van Azure, onderhoud van deze toewijzing voor elke uitgaande stroom voor elke virtuele machine kan zijn resource-intensief. Er zijn dus beperkingen die zijn ingesteld en op basis van de configuratie van het Virtueelnetwerk van Azure. Of, om te melden dat preciezer, een Azure-VM kan alleen een bepaald aantal uitgaande verbindingen op een bepaald moment. Wanneer deze limieten zijn bereikt, zich de virtuele machine niet meer uitgaande verbindingen te maken.

Maar dit gedrag kan worden geconfigureerd. Voor meer informatie over SNAT en SNAT poort uitputting, Zie [in dit artikel](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections).

## <a name="measure-network-performance-on-azure"></a>De netwerkprestaties meting op Azure

Een aantal van de prestaties van maximumwaarden in dit artikel zijn met betrekking tot de netwerklatentie / traject keer (RTT) tussen twee virtuele machines. Deze sectie bevat enkele suggesties voor het testen van de latentie/RTT en het testen van TCP-prestaties en de prestaties van de VM-netwerk. U kunt afstemmen en prestaties van de TCP/IP- en netwerk-waarden die eerder zijn besproken met behulp van de technieken beschreven in deze sectie testen. U kunt latentie, MTU MSS en venster waarden voor schijfgroottes aansluiten op de berekeningen die eerder is verkregen en vergelijken theoretische maximumwaarden daadwerkelijke waarden die u ziet dat tijdens het testen.

### <a name="measure-round-trip-time-and-packet-loss"></a>Meting retourtijd en pakketverlies

TCP-prestaties is sterk afhankelijk van de RTT en pakket verloren gaan. Het hulpprogramma PING beschikbaar in Windows en Linux biedt de eenvoudigste manier om te meten RTT en pakket verloren gaan. De uitvoer van PING ziet u de latentie minimaal/maximaal/gemiddeld tussen een bron en bestemming. U ziet ook pakketverlies. PING het ICMP-protocol standaard gebruikt. U kunt PsPing gebruiken om te testen TCP RTT. Zie voor meer informatie, [PsPing](https://docs.microsoft.com/sysinternals/downloads/psping).

### <a name="measure-actual-throughput-of-a-tcp-connection"></a>Meting werkelijke doorvoer van een TCP-verbinding

NTttcp is een hulpprogramma voor het testen van de TCP-prestaties van een virtuele Linux- of Windows. U kunt verschillende TCP-instellingen wijzigen en vervolgens de prestaties testen met behulp van NTttcp. Zie de volgende bronnen voor meer informatie:

- [Bandbreedte/doorvoer testen (NTttcp)](https://aka.ms/TestNetworkThroughput)

- [Hulpprogramma voor NTttcp](https://gallery.technet.microsoft.com/NTttcp-Version-528-Now-f8b12769)

### <a name="measure-actual-bandwidth-of-a-virtual-machine"></a>Werkelijke bandbreedte van de meting van een virtuele machine

U kunt de prestaties van verschillende VM-typen, accelerated networking testen, enzovoort, met behulp van een hulpprogramma iPerf genoemd. iPerf is ook beschikbaar op Linux- en Windows. iPerf kunt TCP of UDP gebruiken voor het testen van de algemene netwerkdoorvoer. iPerf TCP doorvoer tests worden beïnvloed door de factoren die in dit artikel (zoals latentie en RTT) besproken. Dus mogelijk UDP betere resultaten opleveren als u alleen wilt testen van maximale doorvoer.

Raadpleeg voor meer informatie de volgende artikelen:

- [Oplossen van problemen met prestaties van het netwerk Expressroute](https://docs.microsoft.com/azure/expressroute/expressroute-troubleshooting-network-performance)

- [VPN-doorvoer naar een virtueel netwerk valideren](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-validate-throughput-to-vnet)

### <a name="detect-inefficient-tcp-behaviors"></a>Detecteren van inefficiënt TCP-gedrag

Azure-klanten ziet in pakketopnamen, TCP-pakketten met TCP-vlaggen (zak, DUP ACK RETRANSMIT en snel opnieuw te verzenden) die op problemen met prestaties van het netwerk wijzen kunnen. Deze pakketten geven speciaal netwerk inefficiënties waardoor het resultaat zijn van pakketverlies. Maar pakketverlies is niet noodzakelijkerwijs veroorzaakt door prestatieproblemen met van Azure. Problemen met de prestaties mogelijk het gevolg van problemen, problemen met besturingssysteem of andere problemen die mogelijk niet rechtstreeks met betrekking tot het Azure-platform.

Houd er ook rekening mee dat sommige opnieuw verzenden en dubbele ACK normale in een netwerk zijn. TCP-protocollen zijn ontworpen als een betrouwbare. Bewijs van de TCP-pakketten in een pakketopname noodzakelijkerwijs niet een systematische netwerkprobleem, tenzij ze overmatige.

Deze typen pakketten zijn nog steeds, aanwijzingen dat TCP-doorvoer wordt niet worden gebruikt voor het bereiken van de maximale prestaties, om redenen die in andere gedeelten van dit artikel worden besproken.

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd over TCP/IP-prestaties afstemmen voor Azure VM's, wilt u mogelijk meer informatie over andere overwegingen voor [virtuele netwerken plannen](https://docs.microsoft.com/azure/virtual-network/virtual-network-vnet-plan-design-arm) of [meer informatie over de verbinding te maken en configureren van virtuele netwerken ](https://docs.microsoft.com/azure/virtual-network/).
