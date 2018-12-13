---
title: 'Problemen met het oplossen van de prestaties van het virtuele netwerk: Azure | Microsoft Docs'
description: Deze pagina bevat een gestandaardiseerde methode voor Azure-netwerk koppeling prestaties testen.
services: expressroute
author: tracsman
ms.service: expressroute
ms.topic: article
ms.date: 12/20/2017
ms.author: jonor
ms.custom: seodec18
ms.openlocfilehash: 2572ff3711fb86cda88a86744192980a5b2d5361
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53277621"
---
# <a name="troubleshooting-network-performance"></a>Oplossen van problemen met prestaties van het netwerk
## <a name="overview"></a>Overzicht
Azure biedt stabiel en snelle manieren verbinding maken tussen uw on-premises netwerk en Azure. Methoden, zoals Site-naar-Site VPN en ExpressRoute is door klanten grote als kleine volumes worden gebruikt voor het uitvoeren van hun bedrijfsactiviteiten in Azure. Maar wat er gebeurt als prestaties niet voldoet aan uw verwachting of eerdere ervaring? Dit document kunt standaardiseren van de manier waarop die u testen en basislijn uw specifieke omgeving.

Dit document wordt beschreven hoe u eenvoudig en consistent netwerklatentie en bandbreedte tussen twee hosts testen kunt. Dit document bevat ook enkele advies over manieren om te kijken naar het Azure-netwerk en te isoleren probleem punten. De PowerShell-script en de hulpprogramma's besproken moeten twee hosts in het netwerk (aan beide uiteinden van de koppeling wordt getest). Een host moet een Windows Server of het bureaublad, de andere kan Windows of Linux. 

>[!NOTE]
>De aanpak voor het oplossen, de hulpprogramma's en methoden die worden gebruikt zijn persoonlijke voorkeuren. Dit document beschrijft de procedures en hulpmiddelen die ik vaak nemen. Uw benadering wordt waarschijnlijk verschillen, er is niets mis met verschillende methoden voor het oplossen van problemen. Echter, als u geen een gevestigde benadering, in dit document krijgt u aan de slag op het pad voor het bouwen van uw eigen methoden, hulpprogramma's en voorkeuren voor het oplossen van netwerkproblemen.
>
>

## <a name="network-components"></a>Netwerkonderdelen
Voordat u in het oplossen van problemen spitten, bespreken we enkele algemene voorwaarden en -onderdelen. Deze discussie zorgt ervoor dat we elk onderdeel in de keten voor end-to-end waarmee connectiviteit in Azure overweegt.
[![1]][1]

Op het hoogste niveau beschrijven ik drie primaire netwerk Routeringsdomeinen;

- het Azure-netwerk (blauw cloud aan de rechterkant)
- Internet of een WAN (groen cloud in het midden)
- het bedrijfsnetwerk bevinden (zalmkleurig cloud aan de linkerkant)

Het diagram bekijkt van rechts naar links, laten we even elk onderdeel bespreken:
 - **Virtuele Machine** : de server mogelijk meerdere NIC's, zorg ervoor dat alle statische routes, de standaardroutes, en besturingssysteeminstellingen verzenden en ontvangen verkeer van de manier waarop u denkt dat het is. Elke VM-SKU heeft ook een beperking van de bandbreedte. Als u een kleinere VM-SKU, wordt het verkeer beperkt door de beschikbare bandbreedte voor de NIC. Kan ik een DS5v2 doorgaans gebruikt voor testen (en vervolgens verwijderen eenmaal is klaar met het testen om geld te besparen) om ervoor te zorgen voldoende bandbreedte op de virtuele machine.
 - **NIC** -Zorg ervoor dat u weet dat de persoonlijke IP-adres dat is toegewezen aan de NIC in kwestie.
 - **NIC NSG** : Er kan mogelijk worden specifieke nsg's die worden toegepast op de NIC-niveau, zorg ervoor dat de NSG-regel-set is geschikt voor het verkeer die u probeert om door te geven. Bijvoorbeeld: Controleer of poorten 5201 voor iPerf, 3389 voor RDP of 22 voor SSH zijn geopend voor het toestaan van testverkeer om door te geven.
 - **VNet-Subnet** -de NIC is toegewezen aan een specifiek subnet, zorg ervoor dat u weet welke één en de regels die zijn gekoppeld aan dat subnet.
 - **Subnet-NSG** : net zoals de NIC, nsg's kunnen worden toegepast op het subnet ook. Zorg ervoor dat de NSG-regel-set is geschikt voor het verkeer die u probeert om door te geven. (voor verkeer inkomend aan de NIC en de Netwerkbeveiligingsgroep is van toepassing eerst subnet vervolgens de NSG NIC, omgekeerd voor uitgaande van de virtuele machine verkeer de NIC NSG eerst past vervolgens de Subnet-NSG wordt geleverd aan de orde).
 - **Subnet UDR** -gebruiker gedefinieerde Routes kunt instellen dat verkeer naar een tussenliggende hop (zoals een firewall of load balancer). Zorg ervoor dat u weet dat als er een UDR in plaats voor uw verkeer en als waar het gaat dus en wat dat volgende hop op uw verkeer doet. (bijvoorbeeld een firewall kan sommige verkeer wordt doorgegeven en het andere verkeer tussen de dezelfde twee hosts weigeren).
 - **Gateway-subnet / NSG / UDR** -net als bij het VM-subnet, het gatewaysubnet met nsg's en udr's kan hebben. Zorg ervoor dat u weet als ze er zijn en welk effect op uw verkeer hebben.
 - **VNet-Gateway (ExpressRoute)** -zodra peering (ExpressRoute) of VPN is ingeschakeld, er niet veel instellingen die invloed kunnen zijn op hoe of wanneer het verkeer van routes. Als u meerdere ExpressRoute-circuits of VPN-tunnels die zijn verbonden met dezelfde VNet-Gateway hebt, kunt u moet rekening houden met het gewicht verbindingsinstellingen als deze instelling is van invloed op verbinding voorkeur en is van invloed op het pad naar dat uw verkeer gaat.
 - **Route-Filter** (niet weergegeven): een routefilter alleen van toepassing op Microsoft-Peering voor ExpressRoute, maar is van essentieel belang om te controleren als u de routes die u verwacht niet ziet op de Microsoft-Peering. 

U kunt op dit moment op het WAN-gedeelte van de koppeling. Deze routeringsdomein mag uw serviceprovider, uw zakelijke WAN of het Internet. Veel hops, technologieën en bedrijven die betrokken zijn bij deze koppelingen kunnen u enigszins moeilijk op te lossen. Vaak het geval is, werkt u regel uit zowel Azure als uw bedrijfsnetwerken eerst voordat u deze verzameling van bedrijven en hops leert.

In het voorgaande diagram is aan de linkerkant uw bedrijfsnetwerk. Afhankelijk van de grootte van uw bedrijf, kan dit routeringsdomein zijn een aantal netwerkapparaten te beheren tussen u en de WAN- of meerdere lagen van apparaten in een campus-/ bedrijfsnetwerk.

Gezien de complexiteit van deze drie verschillende op hoog niveau netwerkomgevingen, het is vaak optimale om te beginnen bij de randen en probeer om weer te geven waarbij prestaties goed is en waar deze vermindert. Deze aanpak kan helpen het probleem routering domein van de drie identificeren en vervolgens concentreren uw problemen oplossen in die specifieke omgeving.

## <a name="tools"></a>Hulpprogramma's
De meeste netwerkproblemen kunnen worden geanalyseerd en geïsoleerd met behulp van eenvoudige hulpprogramma's zoals ping en traceroute. Het is zeldzaam dat u moet uitgebreide een analyse van interfacepakketten zoals Wireshark gaan. Om u te helpen bij het oplossen van problemen, is Azure Connectivity Toolkit (AzureCT) ontwikkeld om enkele van deze hulpprogramma's in een eenvoudig-pakket. Voor het Prestatietesten van, zoals ik iPerf en PSPing gebruiken. iPerf is een veelgebruikte hulpmiddel en wordt uitgevoerd op de meeste besturingssystemen. iPerf is geschikt voor eenvoudige prestaties tests en is tamelijk eenvoudig te gebruiken. PSPing is een hulpprogramma ping die zijn ontwikkeld door SysInternals. PSPing is een eenvoudige manier om uit te voeren van ICMP- en TCP-pings in één opdracht voor ook eenvoudig te gebruiken. Beide van deze hulpprogramma's zijn lichtgewicht en worden 'geïnstalleerd' door te kopiëren van de bestanden naar een map op de host.

Ik heb al deze hulpprogramma's en methoden verpakt in een PowerShell-module (AzureCT) die u kunt installeren en gebruiken.

### <a name="azurect---the-azure-connectivity-toolkit"></a>AzureCT - de Azure Connectivity Toolkit
De AzureCT PowerShell-module bestaat uit twee onderdelen [beschikbaarheid testen] [ Availability Doc] en [prestatietests][Performance Doc]. Dit document is alleen betrokken zijn bij het testen van prestaties, dus deze richten op de twee opdrachten in de prestaties van de koppeling in deze PowerShell-module.

Er zijn drie eenvoudige stappen voor het gebruik van deze toolkit voor het testen van prestaties. (1) Installeer de PowerShell-module, 2) Installeer de ondersteunende toepassingen iPerf en PSPing 3) de prestatietest uitvoeren.

1. De PowerShell-Module installeren

    ```powershell
    (new-object Net.WebClient).DownloadString("https://aka.ms/AzureCT") | Invoke-Expression
    
    ```

    Met deze opdracht de PowerShell-module downloadt en installeert deze lokaal.

2. De ondersteunende toepassingen installeren
    ```powershell
    Install-LinkPerformance
    ```
    Met deze opdracht AzureCT iPerf en PSPing installeert in een nieuwe map 'C:\ACTTools', wordt ook de Windows Firewall-poorten voor het toestaan van ICMP en poort 5201 (iPerf)-verkeer.

3. De prestatietest uitvoeren

    U moet op de externe host eerst installeren en uitvoeren van iPerf in de servermodus. Zorg er ook voor de externe host luistert op een van beide 3389 (RDP voor Windows) of 22 (SSH voor Linux) en het toestaan van verkeer op poort 5201 voor iPerf. Als de externe host windows is, de AzureCT installeren en voer de opdracht Install-LinkPerformance iPerf en de firewall-regels nodig iPerf is gestart in de servermodus instellen. 
    
    Zodra de externe computer gereed is, opent u PowerShell op de lokale computer en de test te starten:
    ```powershell
    Get-LinkPerformance -RemoteHost 10.0.0.1 -TestSeconds 10
    ```

    Deze opdracht voert een reeks gelijktijdige laden en latentie tests om te schatten van de bandbreedtecapaciteit en latentie van uw netwerkverbinding.

4. Controleer de uitvoer van de tests

    De indeling van de PowerShell-uitvoer lijkt op:

    [![4]][4]

    De gedetailleerde resultaten van alle iPerf en PSPing tests worden in afzonderlijke tekstbestanden in de map van de hulpprogramma's AzureCT op "C:\ACTTools."

## <a name="troubleshooting"></a>Problemen oplossen
Als de prestatietest wordt niet aangeboden moet u verwachte resultaten, waarom realisatie een progressieve stapsgewijze proces zijn. Gezien het aantal onderdelen in het pad, biedt een systematische benadering in het algemeen een snellere pad resolutie dan springen rond en mogelijk onnodig doet de dezelfde tests meerdere keren.

>[!NOTE]
>Het scenario hier is een prestatieprobleem met, niet een probleem met de netwerkverbinding. De stappen wilt afwijken als verkeer helemaal niet is doorgegeven.
>
>

Eerst daag uw veronderstellingen. Is uw verwachting redelijke? Bijvoorbeeld, als u een 1-Gbps ExpressRoute-circuit en 100 ms latentie hebt is onredelijk te verwachten dat de volledige 1 Gbps van verkeer dat is opgegeven, de prestatiekenmerken van TCP via koppelingen met hoge latentie. Zie de [verwijst naar de sectie](#references) voor meer op prestaties veronderstellingen.

Vervolgens ik kunt het beste beginnen bij de randen tussen Routeringsdomeinen en probeer het probleem op een enkele belangrijke routeringsdomein; te isoleren het bedrijfsnetwerk bevinden, het WAN of het Azure-netwerk. Personen vaak schuld de "zwarte doos' in het pad, terwijl de zwarte doos blaming is heel gemakkelijk, mogelijk aanzienlijk vertraagd resolutie met name als het probleem zich in een gebied dat u hebt de mogelijkheid om te wijzigen. Zorg ervoor dat u uw vanwege de zorgvuldigheid doen voordat het afleveren bij uw serviceprovider of Internet-provider.

Nadat u de belangrijkste routeringsdomein die wordt weergegeven voor het probleem hebt geïdentificeerd, moet u een diagram van het gebied in kwestie maken. Op een whiteboard, Kladblok of Visio als een diagram biedt een concreet 'intensief kaart' om toe te staan een methodisch benadering voor verdere kan worden geïsoleerd van het probleem. U kunt testen punten van plan bent, en bijwerken van de kaart als u gebieden of duik dieper als de tests worden uitgevoerd wissen.

Nu dat u een diagram hebt, kunt u beginnen met het netwerk verdelen in segmenten en verfijnen van het probleem. Ontdek waar het werkt en waar dat niet het geval. Uw test punten om te isoleren omlaag naar de strijdige onderdeel verplaatsen.

Ook, vergeet dan niet om te kijken naar andere lagen van het OSI-model. Het is eenvoudig te concentreren op het netwerk en lagen 1-3 (fysieke, gegevens en netwerk-lagen) maar de problemen kunnen ook van op laag 7 in de toepassingslaag. Een geopend waarmee u rekening moet houden en controleer of veronderstellingen.

## <a name="advanced-expressroute-troubleshooting"></a>Geavanceerde ExpressRoute-probleemoplossing
Als u niet zeker weet waar de rand van de cloud daadwerkelijk is kan voor het isoleren van de Azure-onderdelen lastig zijn. Wanneer u ExpressRoute gebruikt, is de rand een netwerkonderdeel de Microsoft Enterprise Edge (MSEE) genoemd. **Bij het gebruik van ExpressRoute**, de MSEE is de eerste aanspreekpunt in het netwerk van Microsoft en de laatste hop verlaten van het Microsoft-netwerk. Wanneer u een verbindingsobject tussen uw VNet-gateway en het ExpressRoute-circuit maakt, bent u daadwerkelijk maken van een verbinding met de MSEE. De MSEE herkennen als de eerste of laatste-hop (afhankelijk van welke richting u gaat) is essentieel om te isoleren van problemen met het Azure-netwerk met een bewijzen dat het probleem is in Azure of downstream verder in de WAN- of het bedrijfsnetwerk bevinden. 

[![2]][2]

>[!NOTE]
> U ziet dat de MSEE zich niet in de Azure-cloud. ExpressRoute is in werkelijkheid aan de rand van het Microsoft-netwerk niet daadwerkelijk in Azure. Nadat u bent verbonden met ExpressRoute voor een MSEE, u bent verbonden met het netwerk van Microsoft, daar vervolgens gaat u naar de cloud Services, zoals Office 365 (met Microsoft-Peering) of Azure (met persoonlijke en/of Microsoft-Peering).
>
>

Als twee VNets (VNets A en B in het diagram) zijn verbonden met de **dezelfde** ExpressRoute-circuit, kunt u een reeks van tests om te isoleren van het probleem in Azure (of bewijzen dat deze zich niet in Azure) uitvoeren
 
### <a name="test-plan"></a>Testplan
1. Voer de test Get-LinkPerformance tussen VM1 en VM2. Deze test biedt inzicht in als het probleem zich lokaal of niet. Als deze test worden aanvaardbare latentie en bandbreedte resultaten geproduceerd, kunt u het lokale VNet netwerk markeren als goed.
2. Ervan uitgaande dat het lokale VNet is verkeer goed, voert u de test Get-LinkPerformance tussen VM1 en VM3. Deze test oefent de verbinding via het netwerk van Microsoft naar de MSEE en terug in Azure. Als deze test worden aanvaardbare latentie en bandbreedte resultaten geproduceerd, kunt u het Azure-netwerk markeren als goed.
3. Als Azure is uitgesloten, kunt u een vergelijkbare reeks tests uitvoeren op uw bedrijfsnetwerk. Als u die ook tests ook, is het tijd om te werken met uw serviceprovider of Internet-provider voor het vaststellen van uw WAN-verbinding. Voorbeeld: Deze test uitvoeren tussen twee filialen, of tussen de helpdesk en een data center-server. Afhankelijk van wat u test, eindpunten vinden (servers, pc's, enzovoort) die het opgegeven pad kunnen uitoefenen.

>[!IMPORTANT]
> Het is essentieel dat voor elke test u het tijdstip waarop die u de test uitvoert markeren en noteer de resultaten in een algemene locatie (ik zoals OneNote of Excel). Elke testuitvoering moet identiek zijn uitvoer hebben, zodat u kunt de resulterende gegevens van de test wordt uitgevoerd met elkaar vergelijken en geen 'gaten' in de gegevens. Consistentie binnen meerdere tests is de belangrijkste reden dat ik de AzureCT gebruiken voor het oplossen van problemen. De Magic-pakket is niet in de exacte load-scenario's kan ik, maar in plaats daarvan uitvoeren de *magic* is het feit dat er verschijnt een *consistente test- en uitvoer* van elke test. De tijd op te nemen en met consistente telkens één is met name handig als u later vinden dat het probleem sporadisch is. Worden met het verzamelen van gegevens een toegewijd en voorkomt u uren van de bewaartijd van dezelfde scenario's (ik geleerd harde zo lang geleden).
>
>

## <a name="the-problem-is-isolated-now-what"></a>Het probleem is geïsoleerd, wat nu?
Hoe kunt u isoleren het probleem des te makkelijker het is om op te lossen, maar vaak het bereiken van het punt waar u uitgebreidere of verdere kan niet met het oplossen van problemen gaat. Voorbeeld: ziet u de koppeling tussen uw serviceprovider hops via Europa nemen, maar het verwachte pad is in Azië. Dit punt is wanneer u opnemen voor hulp contact moet. Die u vragen is afhankelijk van het routeringsdomein dat u het probleem is geïsoleerd, of nog beter als u deze naar een specifiek onderdeel te beperken.

Voor problemen met het bedrijfsnetwerk bevinden, uw interne IT-afdeling of serviceprovider ondersteuning van uw netwerk (die mogelijk is de hardwarefabrikant) mogelijk om te helpen bij de apparaatconfiguratie of reparatie van de hardware.

Voor het WAN kan uw testresultaten delen met uw serviceprovider of Internet-provider help ze aan de slag en te voorkomen dat die betrekking hebben op sommige van de dezelfde basis als die u al hebt getest. Echter niet worden offended u als ze willen controleren of uw resultaten zelf. "Vertrouwen, maar moet u controleren" is een goede motto bij het oplossen van problemen op basis van de gerapporteerde resultaten van anderen.

Met Azure, nadat u het probleem in zoveel mogelijk details kunt, u isoleren is het tijd om te controleren de [documentatie voor Azure Network] [ Network Docs] en vervolgens indien nodig nog steeds [open een ondersteuningsticket][Ticket Link].

## <a name="references"></a>Verwijzingen
### <a name="latencybandwidth-expectations"></a>Latentie/bandbreedte verwachtingen
>[!TIP]
> Geografische latentie (mijl of kilometer) tussen de eindpunten die u test is verreweg de grootste onderdeel van latentie. Hoewel er apparatuur latentie (fysieke en virtuele-onderdelen, het aantal hops, enzovoort) die zijn betrokken, blijkt Geografie te zijn van het grootste onderdeel van de totale latentie tijdens het afhandelen van WAN-verbindingen. Het is ook belangrijk te weten dat de afstand wordt de afstand van de fiber uitvoeren niet de lineaire of roadmap afstand. Deze afstand is zeer moeilijk is om op te halen met een nauwkeurigheid. Als gevolg hiervan ik in het algemeen een stad afstand Rekenmachine gebruiken op het internet en weet dat deze methode een aangemerkt onnauwkeurige maateenheid is, maar is dit voldoende om in te stellen een algemeen verwachting.
>
>

Ik heb een ExpressRoute-configuratie in Seattle, Washington in de Verenigde Staten. De volgende tabel ziet u de latentie en bandbreedte die ik zag testen tot verschillende Azure-locaties. Ik hebt, schatten we de geografische afstand tussen elk einde van de test.

Instellingen testen:
 - Een fysieke server met Windows Server 2016 met een 10 Gbps NIC, verbonden met een ExpressRoute-circuit.
 - Een 10 Gbps ExpressRoute Premium-circuit in de locatie die is geïdentificeerd met de persoonlijke Peering is ingeschakeld.
 - Een Azure VNet met een UltraPerformance-gateway in de opgegeven regio.
 - Een DS5v2 virtuele machine met Windows Server 2016 op het VNet. De virtuele machine is niet van het domein is toegevoegd, samengesteld uit de standaard installatiekopieën van Azure (geen optimalisatie of aanpassing) met AzureCT geïnstalleerd.
 - Alle tests is met behulp van de opdracht Get-LinkPerformance van AzureCT met een belastingstest van 5 minuten voor elk van de zes test wordt uitgevoerd. Bijvoorbeeld:

    ```powershell
    Get-LinkPerformance -RemoteHost 10.0.0.1 -TestSeconds 300
    ```
 - De gegevensstroom voor elke test heeft de belasting die voortvloeien uit de on-premises fysieke server (iPerf client in Haarlem) tot de Azure-VM (iPerf server in de vermelde Azure-regio).
 - De gegevens van de kolom 'Latentie' is van de belastingtest Nee (een TCP-latentie test zonder iPerf uitgevoerd).
 - De gegevens van de kolom 'Max Bandwidth' is van de 16 TCP-stroom load-test met een venstergrootte van 1 Mb.

[![3]][3]

### <a name="latencybandwidth-results"></a>Resultaten van de latentie/bandbreedte
>[!IMPORTANT]
> Deze getallen gelden voor alleen algemene ter informatie. Veel factoren van invloed op latentie en deze waarden zijn in het algemeen consistente na verloop van tijd, voorwaarden in Azure of het netwerk serviceproviders verkeer via verschillende paden op elk gewenst moment kunnen verzenden, waardoor latentie en bandbreedte kunnen worden beïnvloed. De effecten van deze wijzigingen resulteren niet over het algemeen in belangrijke verschillen.
>
>

| | | | | | |
|-|-|-|-|-|-|
|ExpressRoute<br/>Locatie|Azure<br/>Regio|Schatting<br/>Afstand (km)|Latentie|1-sessie<br/>Bandbreedte|Maximum<br/>Bandbreedte|
| Seattle | US - west 2        |    191 km |   5 ms | 262.0 Mbit per seconde |  3,74 Gbit/sec | 21
| Seattle | US - west          |  1,094 km |  18 ms |  82.3 Mbit per seconde |  3.70 Gbit/sec | 20
| Seattle | US - centraal       |  2,357 km |  40 ms |  38.8 Mbit per seconde |  2,55 Gbit/sec | 17
| Seattle | US - zuid-centraal |  2,877 km |  51 ms |  30,6 mil Mbit per seconde |  2.49 Gbit/sec | 19
| Seattle | US - noord-centraal |  2,792 km |  55 ms |  27.7 Mbit per seconde |  2.19 Gbit/sec | 18
| Seattle | US - oost 2        |  3,769 km |  73 ms |  21.3 Mbit per seconde |  1.79 Gbit/sec | 16
| Seattle | US - oost          |  3,699 km |  74 ms |  21.1 Mbit per seconde |  1,78 Gbit/sec | 15
| Seattle | Japan - oost       |  7,705 km | 106 ms |  14,6 Mbit per seconde |  1.22 Gbit/sec | 28
| Seattle | Verenigd Koninkrijk Zuid         |  7,708 km | 146 ms |  10.6 Mbit per seconde |   896 Mbit per seconde | 24
| Seattle | Europa -west      |  7,834 km | 153 ms |  10.2 Mbit per seconde |   761 Mbit per seconde | 23
| Seattle | Australië - oost   | 12,484 km | 165 ms |   9.4 Mbit per seconde |   794 Mbit per seconde | 26
| Seattle | Azië - zuidoost   | 12,989 km | 170 ms |   9.2 Mbit per seconde |   756 Mbit per seconde | 25
| Seattle | Brazilië-Zuid *   | 10,930 km | 189 ms |   8.2 Mbit per seconde |   699 Mbit per seconde | 22
| Seattle | India - zuid      | 12,918 km | 202 ms |   7,7 Mbit per seconde |   634 Mbit per seconde | 27

\* De latentie voor Brazilië is een goed voorbeeld waarbij de lineaire afstand aanzienlijk verschilt van de fiber afstand worden uitgevoerd. Kan ik verwachten dat de latentie in de groep van 160 ms worden zou, maar daadwerkelijk 189 ms is. Dit verschil op basis van de verwachting dat kan duiden op een netwerkprobleem ergens, maar zeer waarschijnlijk dat de fiber uitgevoerd niet naar Brazilië in een rechte lijn en heeft een extra 1000 km of zo manier van reizen om op te halen voor Brazilië uit Seattle.

## <a name="next-steps"></a>Volgende stappen
1. De Azure Connectivity Toolkit downloaden vanuit GitHub op [http://aka.ms/AzCT][ACT]
2. Volg de instructies voor [koppeling Prestatietesten][Performance Doc]

<!--Image References-->
[1]: ./media/expressroute-troubleshooting-network-performance/network-components.png "azure netwerkonderdelen"
[2]: ./media/expressroute-troubleshooting-network-performance/expressroute-troubleshooting.png "ExpressRoute-probleemoplossing"
[3]: ./media/expressroute-troubleshooting-network-performance/test-diagram.png "Perf-testomgeving"
[4]: ./media/expressroute-troubleshooting-network-performance/powershell-output.png "PowerShell-uitvoer"

<!--Link References-->
[Performance Doc]: https://github.com/Azure/NetworkMonitoring/blob/master/AzureCT/PerformanceTesting.md
[Availability Doc]: https://github.com/Azure/NetworkMonitoring/blob/master/AzureCT/AvailabilityTesting.md
[Network Docs]: https://docs.microsoft.com/azure/index#pivot=services&panel=network
[Ticket Link]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview
[ACT]: http://aka.ms/AzCT











