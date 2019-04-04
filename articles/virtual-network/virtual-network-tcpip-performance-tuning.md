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
ms.date: 3/30/2019
ms.author:
- rimayber
- dgoddard
- stegag
- steveesp
- minale
- btalb
- prachank
ms.openlocfilehash: 664c8b659152a370d7fb31907b6cdbcd414dce31
ms.sourcegitcommit: 9f4eb5a3758f8a1a6a58c33c2806fa2986f702cb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58905089"
---
# <a name="tcpip-performance-tuning-for-azure-vms"></a>TCP/IP-prestaties afstemmen voor virtuele Azure-machines

Het doel van dit artikel is om te bespreken algemene TCP/IP prestaties afstemmen technieken en hun overwegingen voor virtuele machines die worden uitgevoerd op Microsoft Azure. Het is belangrijk om een basiskennis hebben van de concepten en vervolgens bespreken hoe ze kunnen worden afgestemd.

## <a name="common-tcpip-tuning-techniques"></a>Veelgebruikte methoden voor TCP/IP-afstemmen

### <a name="mtu-fragmentation-and-large-send-offload-lso"></a>MTU fragmentatie en grote verzendings-Offload (LSO)

#### <a name="explanation-of-mtu"></a>Uitleg van de MTU

De maximale verzendeenheid (MTU) is het grootste grootte frame (pakket), opgegeven in bytes, die kunnen worden verzonden via een netwerkinterface. De MTU is een configureerbare instelling en de standaard MTU die wordt gebruikt op Azure Virtual machines en de standaardinstelling voor de meeste netwerkapparaten is wereldwijd 1500 bytes.

#### <a name="explanation-of-fragmentation"></a>Uitleg van fragmentatie

Fragmentatie treedt op wanneer een pakket wordt verzonden die groter is dan de MTU van een netwerkinterface. De TCP/IP-stack wordt het pakket opsplitsen in kleinere delen (fragmenten) die aan de MTU-interfaces voldoen. Fragmentatie wordt uitgevoerd op de IP-laag en is onafhankelijk van het onderliggende protocol (zoals TCP). Wanneer een 2000-byte-pakket wordt verzonden via een netwerkinterface met een MTU van 1500, zal klikt u vervolgens het worden opgesplitst in één 1500-byte pakket- en één 500-byte pakket.

Netwerkapparaten in het pad tussen een bron en bestemming hebben de optie voor pakketten die groter zijn dan de MTU of het pakket opsplitsen in kleinere delen.

#### <a name="the-dont-fragment-df-bit-in-an-ip-packet"></a>De bits ' geen Fragment (DF) ' in een IP-pakket

De bit Fragment niet is een markering in de koptekst van de IP-Protocol. Wanneer de DF-bit is ingesteld, betekent dit dat tussenliggende netwerkapparaten op het pad tussen de afzender en de ontvanger moeten het pakket fragment. Er zijn diverse redenen waarom dit deel kan worden ingesteld (Zie hieronder voor een voorbeeld Path Discovery). Wanneer een netwerkapparaat ontvangt u een pakket met de niet-Fragment bit is ingesteld, en dat pakket overschrijdt de interface-MTU van de apparaten, wordt het standaardgedrag is voor het apparaat om te verwijderen van het pakket en een pakket "ICMP fragmentatie nodig" verzenden terug naar de oorspronkelijke bron van de pakket.

#### <a name="performance-implications-of-fragmentation"></a>Gevolgen voor de prestaties van fragmentatie

Fragmentatie kan negatieve prestaties gevolgen hebben. Een van de belangrijkste redenen voor prestatie-impact is de impact van de CPU/geheugen van fragmentatie en samenvoegen van pakketten. Wanneer een netwerkapparaat moet het fragment van een pakket, is er resources om uit te voeren van fragmentatie van de CPU/geheugen worden toegewezen. Dezelfde moet gebeuren wanneer het pakket opnieuw is samengesteld. Het netwerkapparaat moet alle fragmenten worden opgeslagen totdat ze worden ontvangen, zodat deze ze in het oorspronkelijke pakket samenstellen kan. Dit proces van fragmentatie/re-assembly kan ook leiden tot latentie vanwege het proces fragmentatie/re-assembly.

De andere mogelijke negatieve prestaties implicatie van fragmentatie is dat gefragmenteerde pakketten niet de juiste volgorde ontvangen. Niet-geordende pakketten kunnen leiden tot bepaalde typen apparaten de niet-geordende pakketten - waardoor er vervolgens het volledige pakket dat moet worden doorgegeven. Typische scenario's voor het verwijderen van fragmenten zijn beveiligingsapparaten, zoals netwerkfirewalls, of wanneer het ontvangen van een netwerkapparaat buffers zijn uitgeput. Wanneer het ontvangen van een netwerkapparaat buffers zijn uitgeput, een Cisco-apparaat probeert te opnieuw samenstellen van een gefragmenteerde pakketten, maar beschikt niet over de resources voor het opslaan en reassume van het pakket.

Fragmentatie kan worden beschouwd als een negatieve bewerking, maar ondersteuning voor fragmentatie nodig is om verbinding te maken van verschillende netwerken via Internet.

#### <a name="benefits-and-consequences-of-modifying-the-mtu"></a>Voordelen en de gevolgen van het wijzigen van de MTU

Vergroten van de MTU maken een efficiëntere netwerk als een algemeen overzicht. Elk pakket dat wordt verzonden is extra header-informatie die wordt toegevoegd aan het oorspronkelijke pakket. Meer pakket betekent meer overhead-header en het netwerk als gevolg hiervan is minder efficiënt.

De Ethernet-header-grootte is bijvoorbeeld slechts 14 bytes plus een 4-bytes Frame controleren reeks (FCS) om te zorgen voor consistentie frame. Als één 2000-byte pakket wordt verzonden, wordt de 18 bytes van Ethernet-overhead op het netwerk toegevoegd. Als het pakket is gefragmenteerd in een pakket 1500 bytes en een 500-byte pakket, klikt u vervolgens hebt elk pakket 18 van Ethernet-header- of 36 bytes. Terwijl een enkel 2000-byte pakket alleen een Ethernet-header van 18 bytes hoeft.

Het is belangrijk te weten dat verhogen van de MTU op zichzelf niet per se u een efficiëntere netwerk maakt. Als een toepassing alleen pakketten die 500-byte verzendt, klikt u vervolgens bestaat de dezelfde overhead van de header of de MTU 1500 of 9000 bytes is. In de volgorde voor het netwerk worden meer efficiënt, klikt u vervolgens het moet ook gebruiken pakket grotere die ten opzichte van de MTU zijn.

#### <a name="azure-and-vm-mtu"></a>Azure en de MTU van de virtuele machine

De MTU voor Azure VM's van de standaardwaarde is 1500 bytes. De Azure Virtual Network-stack probeert het fragment van een pakket 1400 bytes. De Azure Virtual Network-stack kunt echter pakketten tot 2006 bytes als het 'Fragment niet'-bit is ingesteld in de IP-Header.

Het is belangrijk te weten dat deze fragmentatie niet betekent dat de Azure Virtual Network-stack is inherent inefficiënt dat omdat deze pakketten op 1400 bytes fragmenten terwijl de virtuele machines hebben een MTU van 1500. De realiteit is dat een groot percentage van netwerkpakketten veel kleiner is dan 1400 of 1500 bytes.

#### <a name="azure-and-fragmentation"></a>Azure en fragmentatie

Van Azure Virtual Network-stack is vandaag geconfigureerd voor het verwijderen 'Out van Order-fragmenten' - wat betekent dat gefragmenteerde pakketten die niet in de oorspronkelijke gefragmenteerde volgorde binnenkomen. Deze pakketten verwijderd voornamelijk als gevolg van een netwerk-beveiligingsprobleem aangekondigd in November 2018 FragmentStack genoemd.

FragmentSmack is een fout in de manier waarop de Linux-kernel opnieuw samenstellen van gefragmenteerde pakketten voor IPv4 en IPv6 verwerkt. Een externe aanvaller kan deze fout op trigger dure fragment opnieuw samenstellen bewerkingen, die tot hogere CPU- en een DoS-aanval op het doelsysteem leiden gebruiken.

#### <a name="tune-the-mtu"></a>De MTU afstemmen

Azure-VM's ondersteunen een configureerbare MTU net zoals een ander besturingssysteem. De fragmentatie dat plaatsvindt binnen Azure, en dat hierboven wordt beschreven moet echter worden overwogen bij het configureren van de MTU.

Azure biedt geen Moedig klanten aan hun VM MTU verhogen. Deze beschrijving is bedoeld om het in detail uitgelegd hoe Azure MTU implementeert en uitvoert fragmentatie vandaag nog.

> [!IMPORTANT]
>MTU heeft niet laten zien om prestaties te verbeteren en kan een negatief effect hebben op de prestaties van toepassingen.
>
>

#### <a name="large-send-offload-lso"></a>Grote verzendings-Offload (LSO)

Grote verzendings-Offload (LSO) kunt verbetering van netwerkprestaties door het offloaden van de segmentatie van pakketten naar de Ethernet-Adapter. Met LSO ingeschakeld, wordt de TCP/IP-stack maken van een grote TCP-pakket en ontvangt vervolgens aan de Ethernet-adapter voor de segmentering voordat deze worden doorgestuurd. Het voordeel van LSO is dat deze de CPU van pakketten segmenteren in pakketgrootte die voldoen aan de MTU en die verwerking die aan de Ethernet-interface waar deze wordt uitgevoerd in de hardware-offload kunt vrijmaken. Meer informatie over de voordelen van LSO vindt u [prestaties in de documentatie van Microsoft-netwerkadapter](https://docs.microsoft.com/windows-hardware/drivers/network/performance-in-network-adapters#supporting-large-send-offload-lso).

Wanneer LSO is ingeschakeld, zien Azure-klanten mogelijk grote frame-formaten wanneer presterende pakket worden vastgelegd. Met deze framegrootten zijn grote kunnen leiden tot sommige klanten te geloven dat fragmentatie of een jumbo die MTU wanneer deze niet wordt gebruikt. Met LSO, kan de ethernet-adapter reclame maken voor een grotere MSS aan de TCP/IP-stack om te kunnen maken van een grotere TCP-pakket. Dit gehele niet-gesegmenteerde frame wordt vervolgens doorgestuurd naar de Ethernet-adapter en zichtbaar waren in een pakketopname uitgevoerd op de virtuele machine. Echter, het pakket wordt worden opgesplitst in veel kleinere frames van de Ethernet-adapter op basis van de MTU van de Ethernet-adapter.

### <a name="tcpmss-window-scaling-and-pmtud"></a>TCP/MSS venster schalen en PMTUD

#### <a name="explanation-of-tcp-mss"></a>Uitleg van TCP MSS

TCP-Segment grootte MSS (Maximum) is een instelling die is bedoeld om in te stellen de maximale grootte van de TCP-segment dat kan worden voorkomen fragmentatie van TCP-pakketten. Besturingssystemen wordt meestal ingesteld MSS vastzetten = MTU - IP- & TCP-Header-grootte (20 bytes of 40 totaal aantal bytes). Een interface met een MTU van 1500 heeft dus een MSS van 1460. De MSS kan echter worden geconfigureerd.

Deze instelling wordt in de handshake van de drie richtingen TCP overeengekomen wanneer een TCP-sessie is ingesteld tussen een bron en bestemming. Beide zijden een waarde MSS verzenden en de laagste waarde van de twee wordt gebruikt voor de TCP-verbinding.

Tussenliggende netwerkapparaten, zoals VPN-Gateways, met inbegrip van Azure VPN-Gateway, hebben de mogelijkheid om aan te passen van de MTU onafhankelijk van de bron- en om ervoor te zorgen optimale netwerkprestaties. Daarom moet worden opgemerkt dat de MTU van de bron- en doelserver alleen niet de enige factoren bij de werkelijke MSS-waarde is.

#### <a name="explanation-of-path-mtu-discovery-pmtud"></a>Uitleg van Path MTU Discovery (PMTUD)

Terwijl MSS wordt onderhandeld over, kan dit niet de werkelijke MSS die kunnen worden gebruikt als andere netwerkapparaten in het pad tussen bron en bestemming mogelijk een lagere MTU-waarde dan de bron en bestemming betekenen. Het apparaat waarvan MTU kleiner dan het pakket is wordt in dit geval verwijderen van het pakket en een Internet Control Message Protocol (ICMP) fragmentatie nodig (Type 3, 4-Code)-bericht met de MTU terugsturen. Dit bericht ICMP kan de bronhost te verminderen van de MTU-pad op de juiste wijze. Het proces wordt Path MTU discovery genoemd.

Het proces van PMTUD is inherent inefficiënt en heeft gevolgen voor de prestaties van het netwerk. Wanneer pakketten worden verzonden die groter zijn dan een netwerkpaden MTU, moeten deze pakketten worden doorgegeven met een lagere MSS. Als de afzender het pakket ICMP fragmentatie die nodig zijn, mogelijk vanwege de firewall van een netwerk in het pad (vaak aangeduid als zwarte gat PMTUD), niet ontvangt en vervolgens de afzender niet weet wat de moet de MSS verlagen en continu wordt opnieuw het pakket. Om deze reden wordt niet aanbevolen voor het verhogen van de Azure VM-MTU.

#### <a name="vpn-considerations-with-mtu"></a>Overwegingen voor VPN met MTU

Klanten die gebruikmaken van virtuele machines die encapsulation (zoals IPSec VPN's uitvoeren) kunnen extra gevolgen pakketgrootte en MTU hebben. VPN-verbindingen toevoegen aan extra kopteksten die wordt toegevoegd aan het oorspronkelijke pakket dus pakketgrootte verhogen en vereisen een kleinere MSS.

De huidige aanbeveling voor Azure is het instellen TCP MSS MSS 1350 bytes en tunnelinterface MTU 1400. Meer informatie kunt vinden op de [VPN-apparaten en IPSec/IKE-parameters pagina](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices).

### <a name="latency-round-trip-time-and-tcp-window-scaling"></a>Latentie, retourtijd en schalen van TCP-venster

#### <a name="latency-and-round-trip-time"></a>Latentie en round-trip tijd

De netwerklatentie wordt geregeld door de snelheid van licht via een glasvezelkabels fiber-netwerk. De realiteit is dat de netwerkdoorvoer van TCP is ook effectief bestuurbaar (praktische maximumwaarden) vanwege de retour-tijd (RTT) tussen twee netwerkapparaten.

| | | | |
|-|-|-|-|
|Route|Afstand|Eenzijdige tijd|Retourtijd (RTT)|
|New York met San Francisco|4,148 km|21 ms|42 ms|
|New York naar Londen|5,585 km|28 ms|56 ms|
|New York met Sydney|15,993 km|80 ms|160 ms|

Deze tabel toont de lineaire afstand tussen twee locaties, in netwerken, de afstand is echter meestal langer zijn dan de lineaire afstand. Een eenvoudige formule voor het berekenen van minimale RTT zoals bepaald door de snelheid van licht is: minimale RTT = 2 * (kilometer afstand / snelheid van doorgifte).

Een standaard waarde van 200 kan worden gebruikt voor de snelheid van doorgifte - waarde is dat de afstand in meters licht wordt verzonden in 1 milliseconde.

In het voorbeeld New York op San Francisco is het lineaire afstand 4,148 km. Minimale RTT = 2 * (4,148 / 20). De uitvoer van de vergelijking is in milliseconden.

Als de fysieke afstand tussen twee locaties een vaste realiteit is als maximale netwerkprestaties vereist, is dan is de meest logische optie te selecteren van doelen met de kleinste afstand tussen beide. Bijkomend, kunnen ontwerpbeslissingen binnen het virtuele netwerk worden gemaakt op het pad van het verkeer te optimaliseren en verminder de latentie. Deze overwegingen virtueel netwerk worden beschreven in de onderstaande overwegingen bij het ontwerp van de netwerk-sectie.

#### <a name="latency-and-round-trip-time-effects-on-tcp"></a>Latentie en round-trip tijd effecten op TCP

Round trip heeft time (RTT) een directe invloed op maximale doorvoer van TCP. De TCP-protocol heeft een concept van venstergrootte. Grootte van het venster is de maximale hoeveelheid verkeer die kan worden verzonden via een TCP-verbinding voordat de afzender bevestiging van de ontvanger ontvangen moet. Als de TCP-MSS is ingesteld op 1460 en de TCP-venstergrootte is ingesteld op 65535 vervolgens de afzender kunt 45 pakketten verzenden voordat deze bevestiging van de ontvanger ontvangen moet. De afzender wordt opnieuw als bevestiging is ontvangen. In dit voorbeeld wordt de TCP-venstergrootte / TCP MSS = pakketten worden verzonden. Of 65535 / 1460 wordt afgerond tot 45.

Deze status 'wachten op bevestiging voor' is als een mechanisme voor het maken van een betrouwbare levering van gegevens, wat effectief zorgt ervoor dat de RTT invloed heeft op TCP-doorvoer. Hoe langer de afzender wacht op bevestiging, hoe langer het ook moet wachten voordat u meer gegevens verzenden.

De formule voor het berekenen van de maximale doorvoer van een TCP-verbinding is als volgt: Venstergrootte / (RTT latentie in milliseconden / 1000) = Maximum aantal bytes per seconde. De onderstaande tabel is ingedeeld in Megabytes voor de leesbaarheid en ziet u het maximum aantal MB per seconde doorvoer van een TCP-verbinding.

| | | | |
|-|-|-|-|
|TCP-venstergrootte in Bytes|RTT Latency<br/>in milliseconden|Maximum<br/>MB per seconde doorvoer|Maximum<br/> Megabits per seconde doorvoer|
|65535|1|65.54|524.29|
|65535|30|2.18|17.48|
|65535|60|1.09|8.74|
|65535|90|.73|5.83|
|65535|120|.55|4.37|

Als er pakketverlies, wordt deze de maximale doorvoer van een TCP-verbinding beperken terwijl de afzender stuurt gegevens die al is verzonden.

#### <a name="explanation-of-tcp-window-scaling"></a>Uitleg over het schalen van TCP-venster

Schalen van TCP-venster is een concept, dat dynamisch de TCP-venstergrootte toe te staan meer gegevens worden verzonden verhoogt voordat een bevestiging vereist is. In het vorige voorbeeld, zou 45 pakketten worden verzonden voordat een bevestiging vereist is. Als het nummer van de pakketten die worden verzonden voordat een bevestiging is uitgebreid, wordt de maximale doorvoer van TCP ook verhoogd door te verminderen van het aantal keren dat een afzender op bevestiging wacht.

TCP-doorvoer wordt geïllustreerd in een eenvoudige tabel hieronder:

| | | | |
|-|-|-|-|
|TCP-venstergrootte<br/>in Bytes|RTT latentie in milliseconden|Maximum<br/>MB per seconde doorvoer|Maximum<br/> Megabits per seconde doorvoer|
|65535|30|2.18|17.48|
|131,070|30|4.37|34.95|
|262,140|30|8.74|69.91|
|524,280|30|17.48|139.81|

De waarde van de TCP-header voor de TCP-venstergrootte is echter alleen 2 bytes lang, wat betekent dat de maximale waarde voor een receive venster 65535. Als u wilt vergroten de maximale venstergrootte, is de schaalfactor van een TCP-venster ingevoerd.

De schaalfactor is ook een instelling die kan worden geconfigureerd in een besturingssysteem. De formule voor het berekenen van de TCP-venstergrootte met behulp van de schaalfactoren is als volgt: TCP-venstergrootte TCP-venstergrootte in Bytes = \* (2 ^ schalen van meerdere factoren). Als de schaalfactor venster 3 en venstergrootte van 65535, is berekening als volgt uit: 65535 \* (2 ^ 3) = 262,140 bytes. Een schaalfactor van 14 resulteert in een TCP-venstergrootte van 14 (de maximale offset toegestaan) en vervolgens worden de TCP-venstergrootte 1,073,725,440 bytes (8,5 Gigabit).

#### <a name="support-for-tcp-window-scaling"></a>Ondersteuning voor het schalen van TCP-venster

Windows biedt de mogelijkheid om in te stellen van verschillende factoren voor vergroten/verkleinen op een per verbinding type basis - er zijn verschillende soorten verbindingen (datacenter, internet, enzovoort). U kunt het venster vergroten/verkleinen verbinding classificatie met de powershell-opdracht Get-NetTCPConnection zien.

```powershell
Get-NetTCPConnection
```

U kunt de waarden van elke klasse met de powershell-opdracht Get-NetTCPSetting zien.

```powershell
Get-NetTCPSetting
```

De eerste TCP-venstergrootte en de TCP-schalingsfactor kan worden ingesteld in Windows via de powershell-opdracht Set-NetTCPSetting. Meer informatie kunt vinden op de [Set NetTCPSetting pagina](https://docs.microsoft.com/powershell/module/nettcpip/set-nettcpsetting?view=win10-ps)

```powershell
Set-NetTCPSetting
```

De actieve TCP-instellingen voor AutoTuningLevel zijn als volgt.

| | | | |
|-|-|-|-|
|AutoTuningLevel|Schaalfactor|Vermenigvuldiger voor vergroten/verkleinen|Formule die moet worden<br/>maximale grootte berekenen|
|Uitgeschakeld|Geen|Geen|Venstergrootte|
|Beperkt|4|2^4|Window Size * (2^4)|
|Zeer beperkt|2|2^2|Window Size * (2^2)|
|Normaal|8|2^8|Window Size * (2^8)|
|Experimenteel|14|2^14|Venstergrootte * (2 ^ 14)|

Deze instellingen zijn het meest waarschijnlijk invloed op TCP-prestaties, moet worden opgemerkt dat veel andere factoren op het Internet, buiten het besturingselement van Azure, ook invloed hebben op TCP-prestaties.

#### <a name="increase-mtu-size"></a>MTU-grootte verhogen

Een logische vraag is 'kan de MTU vergroot, TCP-prestaties als een grotere MTU een grotere MSS betekent'? Het eenvoudige antwoord is: waarschijnlijk niet. Zoals wordt beschreven, zijn er voor- en nadelen te pakketgrootte die van toepassing is meer dan alleen TCP-verkeer. Zoals hierboven, de belangrijkste factoren die invloed hebben op is TCP-prestaties van de netwerkdoorvoer TCP-venstergrootte, pakketverlies RTT.

> [!IMPORTANT]
> Azure wordt niet aanbevolen dat Azure-klanten de standaardwaarde van de MTU op virtuele Machines wijzigen.
>
>

### <a name="accelerated-networking-and-receive-side-scaling"></a>Versnelde netwerken en aan de ontvangstzijde

#### <a name="accelerated-networking"></a>Versneld netwerken

Virtuele Machine-netwerkfuncties zijn in het verleden CPU intensieve op zowel de Gast-VM en de Hypervisorhost. Elk pakket dat passages via de host is in software door de host CPU - met inbegrip van alle het Virtueelnetwerk-inkapseling/na-capsulation verwerkt. Dus meer gaat het verkeer dat via de host en hoe hoger de CPU-belasting. En als de host CPU Bezig met andere bewerkingen is, klikt u vervolgens die ook van invloed op netwerkdoorvoer en latentie. Dit probleem is opgelost met versnelde netwerken.

Versneld netwerken biedt consistente ultrakorte netwerklatentie via de interne programmeerbare hardware en de technologieën zoals SR-IOV van Azure. Door het verplaatsen van veel van de Azure software gedefinieerde netwerkstack uitgeschakeld van de CPU's en in FPGA gebaseerde SmartNICs compute cycli worden vrijgemaakt door toepassingen voor eindgebruikers, minder belasting op de virtuele machine te plaatsen, jitter en inconsistentie in de latentie verlagen. Prestaties kan met andere woorden, zijn meer deterministische.

Versneld netwerken realiseert verbeterde prestaties doordat de Gast-VM omzeilen van de host en een gegevenspad rechtstreeks met een host SmartNIC tot stand brengen. Voordelen van versnelde netwerken zijn:

- **Lagere latentie / hoger pakketten per seconde (pps)**: De virtuele switch verwijderen uit het gegevenspad elimineert de tijd voor pakketten uitgaven in de host voor de beleidsverwerking van en het aantal pakketten dat kan worden verwerkt binnen de virtuele machine wordt verhoogd.

- **Minder jitter**: Verwerking van de virtuele switch, is afhankelijk van de hoeveelheid beleid die moet worden toegepast en de werkbelasting van de CPU dat de verwerking. Offloading van het afdwingen van beleid voor de hardware verwijdert die variabiliteit door het leveren van pakketten rechtstreeks naar de virtuele machine, het verwijderen van de host de communicatie van de virtuele machine en alle software-interrupts en context switches.

- **CPU-gebruik verlaagd**: Overslaan van de virtuele switch op de host leidt tot minder CPU-gebruik voor het verwerken van netwerkverkeer.

Versneld netwerken moet expliciet zijn ingeschakeld op basis van per virtuele machine. Instructies voor het inschakelen van versnelde netwerken op een virtuele machine zijn beschikbaar op de [maken van een virtuele Linux-machine met versnelde netwerken pagina](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli).

#### <a name="receive-side-scaling-rss"></a>Aan de ontvangstzijde (RSS)

Aan de ontvangstzijde is een technologie voor het stuurprogramma van network die het ontvangen van het netwerkverkeer efficiënter distribueert door te distribueren verwerking over meerdere CPU's ontvangen in een systeem met meerdere processors. RSS kan simpel gezegd is een systeem voor het verwerken van een groter aantal ontvangen verkeer omdat alle beschikbare CPU's wordt gebruikt in plaats van slechts één. Een meer technische discussie van RSS kan worden gevonden op de [Inleiding tot aan de ontvangstzijde pagina](https://docs.microsoft.com/windows-hardware/drivers/network/introduction-to-receive-side-scaling).

RSS is vereist voor maximale prestaties worden behaald als versnelde netwerken is ingeschakeld op een virtuele machine. Er kan ook worden voordelen bij het gebruik van RSS op VM's waarop niet versneld netwerkondersteuning ingeschakeld. Een overzicht van hoe u bepaalt als RSS is ingeschakeld en configuratie voor het inschakelen van deze kan worden gevonden op de [netwerkdoorvoer optimaliseren voor virtuele machines van Azure-pagina](http://aka.ms/FastVM).

### <a name="tcp-time-wait-and-time-wait-assassination"></a>Wachttijd voor TCP- en -tijd wachten Assassination

Een andere veelvoorkomend probleem dat betrekking heeft op de netwerk- en toepassingsprestaties is de instelling van de TCP-tijd wachten. Op bezette VM's die zijn geopend en sluiten van veel sockets, hetzij als een client of server (IP:Source bronpoort + IP:Destination doelpoort), tijdens de normale werking van TCP, kunt een socket die is opgegeven krijgen in een tijd wachten staat voor een aanzienlijke hoeveelheid tijd. Deze status "tijd wachten" is bedoeld om toe te staan van extra gegevens worden bezorgd op een socket voordat deze wordt gesloten. TCP/IP-stacks dat het hergebruik van een socket daarom in het algemeen door op de achtergrond verwijderen van de clients TCP SYN-pakket.

Deze hoeveelheid tijd die een socket is in-time wachtstatus kan worden geconfigureerd, maar kan variëren van 30 seconden tot 240 seconden. Sockets zijn een eindige resource en het aantal sockets die kunnen worden gebruikt op een bepaald moment kunnen worden geconfigureerd (het nummer in het algemeen ligt ongeveer 30.000 mogelijke sockets). Als dit nummer is uitgeput, of clients en servers instellingen voor niet-overeenkomende tijd wachten hebt en een virtuele machine probeert om opnieuw een socket wachtstatus tijd te gebruiken, klikt u vervolgens mislukt nieuwe verbindingen als TCP SYN pakketten op de achtergrond verwijderd.

De waarde voor het bereik van poorten voor uitgaande sockets, evenals instellingen TCP tijd wachten en socket hergebruik zijn meestal binnen de TCP/IP-Stack van een besturingssysteem kunnen worden geconfigureerd. Wijzigen van deze getallen kunt mogelijk de schaalbaarheid, verbeterde, maar afhankelijk van het scenario, als kan leiden tot interoperabiliteitsproblemen en wees voorzichtig moet worden gewijzigd.

Een functie voor tijd wachten Assassination geïntroduceerd om dit te schalen van beperking. Tijd wachten Assassination kan een socket worden gebruikt in bepaalde scenario's, zoals wanneer het volgnummer in het IP-pakket van de nieuwe verbinding het volgnummer van het laatste pakket uit de vorige verbinding is overschreden. In dit geval wordt het besturingssysteem kan de nieuwe verbinding tot stand is gebracht (de nieuwe SYN ACK accepteren) en geforceerd sluiten de vorige verbinding is in de tijd wachten staat. Deze mogelijkheid wordt ondersteund op Windows-virtuele machines in Azure vandaag en ondersteuning in andere VM's moet worden onderzocht door de respectieve leveranciers Azure-klanten.

Documentatie over het configureren van instellingen voor TCP-tijd wachten en poortbereik van bron is beschikbaar op de [instellingen die kunnen worden gewijzigd voor verbetering van netwerkprestaties pagina](https://docs.microsoft.com/biztalk/technical-guides/settings-that-can-be-modified-to-improve-network-performance).

## <a name="virtual-network-factors-that-can-affect-performance"></a>Virtueel netwerk factoren die invloed kunnen hebben op prestaties

### <a name="vm-maximum-outbound-throughput"></a>Maximum aantal uitgaande doorvoer van de virtuele machine

Azure biedt tal van VM-grootten en -typen, elk met een andere combinatie van prestaties levert. Een dergelijke prestaties is netwerk doorvoer (of bandbreedte), gemeten in megabits per seconde (Mbps). Omdat virtuele machines worden gehost op gedeelde hardware, moet de netwerkcapaciteit redelijk worden gedeeld met de virtuele machines delen dezelfde hardware. Grotere virtuele machines zijn relatief meer bandbreedte dan kleinere virtuele machines toegewezen.

De bandbreedte van het netwerk toegewezen aan elke virtuele machine wordt gemeten op de uitgaande gegevens (uitgaand) verkeer van de virtuele machine. Alle netwerkverkeer verlaten van de virtuele machine wordt meegeteld voor de limiet van de toegewezen, ongeacht de bestemming. Bijvoorbeeld, als een virtuele machine een limiet van 1000 Mbps heeft, geldt dat een limiet of het uitgaande verkeer bestemd voor een andere virtuele machine in hetzelfde virtuele netwerk en buiten Azure is.
Inkomend verkeer wordt niet gemeten of die beperkt zijn rechtstreeks. Er zijn echter andere factoren, zoals CPU- en opslaglimieten die kunnen van invloed op de mogelijkheid van een virtuele machine voor het verwerken van inkomende gegevens.

Versneld netwerken is een functie die is ontworpen voor betere prestaties van het netwerk, met inbegrip van latentie, doorvoer en CPU-gebruik. Terwijl versnelde netwerken kunt verbetering van de doorvoer van een virtuele machine, deze mogelijk alleen up wordt gemaakt op de virtuele machine bandbreedte toegewezen.

Virtuele machines van Azure, moet een, maar kan er meerdere, netwerkinterfaces die zijn gekoppeld aan deze hebben. Bandbreedte die is toegewezen aan een virtuele machine is de som van al het uitgaande verkeer voor alle netwerkinterfaces die zijn gekoppeld aan een virtuele machine. Met andere woorden, is de toegewezen bandbreedte per virtuele machine, ongeacht het aantal netwerkinterfaces zijn gekoppeld aan de virtuele machine.
 
Verwachte uitgaande doorvoer en het aantal netwerkinterfaces dat wordt ondersteund door elke VM-grootte wordt hier beschreven. Als u wilt zien van maximale doorvoer, selecteer een type, zoals algemeen gebruik, en selecteer vervolgens een grootte-serie op de resulterende pagina, zoals de Dv2-serie. Elke reeks bevat een tabel met netwerken specificaties in de laatste kolom met de titel, Max. aantal NIC's / verwachte netwerkprestaties (Mbps).

De maximale doorvoer is van toepassing op de virtuele machine. Doorvoer wordt niet beïnvloed door de volgende factoren:

- **Het aantal netwerkinterfaces**: De limiet voor bandbreedte is cumulatieve van al het uitgaande verkeer van de virtuele machine.

- **Versnelde netwerken**: Hoewel de functie handig is bij het bereiken van de gepubliceerde limiet, verandert de limiet niet meer.

- **Het doel van verkeer**: Alle bestemmingen tellen mee voor de limiet voor uitgaande.

- **Protocol**: Al het uitgaande verkeer via alle protocollen telt voor de limiet.

Een [tabel van de maximale bandbreedte per VM-type kunt u vinden door naar deze pagina te gaan](https://docs.microsoft.com/azure/virtual-machines/windows/sizes) en te klikken op de desbetreffende VM-type. Op elke pagina type wordt een tabel de maximum aantal NIC's en de maximale netwerkbandbreedte voor verwachte weergegeven.

Meer informatie over de bandbreedte van de VM-netwerk kan worden gevonden op [netwerkbandbreedte van de virtuele machine](http://aka.ms/AzureBandwidth).

### <a name="internet-performance-considerations"></a>Prestatieoverwegingen voor Internet

Zoals besproken in dit artikel, kunnen factoren op het Internet en buiten de controle van Azure invloed hebben op prestaties van het netwerk. Deze factoren zijn:

- **Latentie**: De retourtijd tussen twee doelen kan worden beïnvloed door problemen op tussenliggende netwerken, waarbij de "kortste" afstand pad mogelijk en suboptimale-peeringpad verkeer

- **Pakketverlies**: Pakketverlies kan worden veroorzaakt door opstoppingen in het netwerk, fysieke pad problemen en netwerkapparaten onder uitvoeren

- **MTU grootte/fragmentatie**: Fragmentatie langs het pad kan leiden tot vertragingen bij de ontvangst van gegevens of pakketten die niet de juiste volgorde, die invloed kunnen zijn op de levering van pakketten

Traceroute is een goed hulpmiddel voor het meten van netwerk prestatiekenmerken (zoals pakketverlies en latentie) aan elke netwerkpad tussen een bron en doel-apparaat.

### <a name="network-design-considerations"></a>Overwegingen bij het ontwerpen van netwerk

Samen met de bovenstaande aandachtspunten de topologie van een Virtueelnetwerk kan invloed hebben op prestaties van het Virtueelnetwerk. Bijvoorbeeld, een hub en spoke-ontwerp dat backhauls verkeer wereldwijd naar een virtueel netwerk met één hub wordt de netwerklatentie introduceren en dus algehele prestaties van het netwerk gevolgen. Op deze manier kan het aantal apparaten dat netwerkverkeer wordt doorgegeven via ook algemene latentie beïnvloeden. Bijvoorbeeld in een hub en spoke-ontwerp, kan als verkeer wordt doorgegeven via een knooppunt Network Virtual Appliance en een virtueel apparaat Hub voordat u met het Internet, die vervolgens latentie ontstaan door de virtuele netwerkapparaten.

### <a name="azure-regions-virtual-networks-and-latency"></a>Azure Regions, Virtual Networks, and Latency

Azure-regio's bestaan uit meerdere datacenters die zijn opgeslagen in een algemeen geografisch gebied. Deze datacenters mogelijk niet fysiek naast elkaar en in sommige gevallen kunnen worden gescheiden door minder dan 10 kilometer zijn verwijderd. Het Virtueelnetwerk is een logische overlay boven op fysieke datacenternetwerk van Azure en een Virtueelnetwerk betekent niet dat voor elke specifieke netwerktopologie binnen het datacenter. VM-A en B van de VM in hetzelfde Virtueelnetwerk en subnet zijn maar mogelijk zijn in verschillende rekken, rijen of zelfs datacenters. Ze kunnen worden gescheiden door meter fiber glasvezelkabels kabel of kilometer van glasvezelkabels fiber-kabel. Dit kan leiden tot variabele latentie (enkele milliseconden verschil) tussen verschillende virtuele machines.

Deze geografische locatie, en dus latentie tussen twee virtuele machines, kunnen worden beïnvloed door de configuratie van Beschikbaarheidssets en Beschikbaarheidszones, maar de afstand tussen datacenters in een regio is regiospecifieke en voornamelijk beïnvloed door Datacenter-topologie in de regio.

### <a name="source-nat-port-exhaustion"></a>Poortuitputting bron-NAT

Een implementatie in Azure kan communiceren met eindpunten buiten Azure in het openbare Internet en/of het openbare IP-adresruimte. Wanneer een exemplaar van deze uitgaande verbinding initieert, Azure dynamisch privé IP-adres toegewezen aan een openbaar IP-adres. Nadat u deze toewijzing is gemaakt, kan retourverkeer voor deze uitgaande afkomstige stroom ook bereiken het privé IP-adres waarvan de stroom afkomstig is.

Voor elke uitgaande verbinding, moet de Azure Load Balancer deze toewijzing voor een bepaalde tijd behouden. Met de aard van meerdere tenants van Azure, onderhoud van deze toewijzing voor elke uitgaande stroom voor elke virtuele machine kan zijn resource-intensieve. Er zijn daarom beperkingen die zijn ingesteld en op basis van de configuratie van het Virtueelnetwerk van Azure. Of vermeld preciezer - een Azure-VM kan alleen een bepaald aantal uitgaande verbindingen op een bepaald moment. Wanneer deze limieten zijn bereikt, wordt de virtuele machine van Azure kunnen geen verdere uitgaande verbindingen worden voorkomen.

Dit gedrag is echter worden geconfigureerd. Voor meer informatie over [SNAT en SNAT poort uitputting], Zie [in dit artikel](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections).

## <a name="measure-network-performance-on-azure"></a>De netwerkprestaties meting op Azure

Een aantal van de prestaties van maximumwaarden in dit artikel zijn gerelateerd aan netwerklatentie / traject keer (RTT) tussen twee virtuele machines. Deze sectie bevat enkele suggesties voor het testen van de latentie/RTT, evenals TCP-prestaties en de prestaties van de VM-netwerk. De TCP/IP- & netwerk waarden die hierboven wordt beschreven, kunnen worden afgestemd en prestaties te testen met behulp van de technieken die hieronder worden beschreven. De waarden van latentie, MTU MSS en venstergrootte kunnen worden gebruikt in de hierboven genoemde berekeningen en theoretische maximumwaarden kunnen worden vergeleken met de werkelijke waarden van tijdens het testen.

### <a name="measure-round-trip-time-and-packet-loss"></a>Meting retourtijd en pakketverlies

TCP-prestaties is sterk afhankelijk van de RTT en pakketverlies. De eenvoudigste manier om te meten RTT en pakketverlies maakt gebruik van het hulpprogramma ping beschikbaar in Windows en Linux. Min/max/Gem. latentie tussen een bron en bestemming, evenals pakketverlies ziet u de uitvoer van de ping. Ping het ICMP-protocol standaard gebruikt. Als u wilt testen TCP RTT, kan klikt u vervolgens PsPing worden gebruikt. Meer informatie over PsPing is beschikbaar op [deze koppeling](https://docs.microsoft.com/sysinternals/downloads/psping).

### <a name="measure-actual-throughput-of-a-tcp-connection"></a>Meting werkelijke doorvoer van een TCP-verbinding

NTttcp is een hulpprogramma dat wordt gebruikt voor het testen van de TCP-prestaties van een virtuele Linux- of Windows. Verschillende TCP-instellingen kunnen worden toegepast en de voordelen getest met NTttcp. Meer informatie over NTttcp kan worden gevonden op de onderstaande koppelingen.

- [Bandbreedte/doorvoer testen (NTttcp)](https://aka.ms/TestNetworkThroughput)

- [NTttcp Utility](https://gallery.technet.microsoft.com/NTttcp-Version-528-Now-f8b12769)

### <a name="measure-actual-bandwidth-of-a-virtual-machine"></a>Werkelijke bandbreedte van de meting van een virtuele machine

Prestatietesten van verschillende VM-typen, versnelde netwerken, enzovoort, kan worden getest met een hulpprogramma met de naam Iperf, ook beschikbaar in Linux en Windows. Iperf kunt TCP of UDP gebruiken voor het testen van de algemene netwerkdoorvoer. TCP-doorvoer testen met behulp van Iperf worden beïnvloed door factoren besproken in dit artikel (latentie, RTT, enzovoort). UDP kan dus betere resultaten voor het testen van maximale doorvoer te leveren.

Als u meer informatie vindt u hieronder:

- [Oplossen van problemen met prestaties van het netwerk Expressroute](https://docs.microsoft.com/azure/expressroute/expressroute-troubleshooting-network-performance)

- [VPN-doorvoer naar een virtueel netwerk valideren](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-validate-throughput-to-vnet)

### <a name="detect-inefficient-tcp-behaviors"></a>Detecteren van inefficiënt TCP-gedrag

Azure-klanten zien kunnen krijgen voor TCP-pakketten met TCP-vlaggen (zak, DUP ACK RETRANSMIT en snel opnieuw) in pakketopnamen die op problemen met de netwerkprestaties duiden kunnen. Deze pakketten geven speciaal netwerk inefficiëntie als gevolg van pakketverlies. Pakketverlies is echter niet per se vanwege problemen met de prestaties van Azure. Problemen met prestaties mogelijk het resultaat van de toepassing, besturingssysteem of andere problemen die mogelijk niet rechtstreeks met betrekking tot het Azure-platform. Het is ook belangrijk te weten dat sommige doorgifte of dubbele ACK in een netwerk is normaal: TCP-protocollen zijn ontworpen als een betrouwbare. En bewijs van de TCP-pakketten in een pakketopname een systematische netwerkprobleem niet te betekenen, tenzij ze overmatige zijn.

Maar moet deze worden vermeld duidelijk dat deze typen pakketten aanwijzingen zijn dat TCP-doorvoer niet de maximale prestaties, om redenen die worden beschreven in andere gedeelten te bereiken.
