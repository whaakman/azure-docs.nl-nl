---
title: Het oplossen van de prestaties van de virtuele Azure-netwerk | Microsoft Docs
description: Deze pagina bevat een gestandaardiseerde methode van de Azure-netwerk koppeling prestaties testen.
services: expressroute
documentationcenter: na
author: tracsman
manager: rossort
editor: 
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/20/2017
ms.author: jonor
ms.openlocfilehash: 56f011632a2aa3ef0632efd5ace472c0fc79a329
ms.sourcegitcommit: a648f9d7a502bfbab4cd89c9e25aa03d1a0c412b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/22/2017
---
# <a name="troubleshooting-network-performance"></a>Het oplossen van de prestaties van het netwerk
## <a name="overview"></a>Overzicht
Azure biedt een stabiele, snelle manieren voor het verbinden van uw on-premises netwerk naar Azure. Methoden zoals Site-naar-Site VPN en ExpressRoute is door klanten kleine en grote worden gebruikt voor het uitvoeren van hun bedrijf in Azure. Maar wat gebeurt er wanneer prestaties voldoet niet aan uw verwachting of ervaring? Dit document kunt u de manier waarop die u testen en basislijn standaardiseren uw specifieke omgeving.

Dit document ziet u hoe u eenvoudig en consistent netwerklatentie en bandbreedte tussen twee hosts testen kunt. Dit document bevat ook advies over manieren om te kijken naar het Azure-netwerk en helpen bij het probleem punten isoleren. De PowerShell-script en de hulpprogramma's beschreven vereist twee hosts in het netwerk (aan beide uiteinden van de koppeling wordt getest). Een host moet een Windows Server of het bureaublad, de andere kan Windows of Linux. 

>[!NOTE]
>De aanpak voor het oplossen van problemen, de hulpprogramma's en de methoden zijn persoonlijke voorkeuren. Dit document beschrijft de benadering en hulpprogramma's die ik vaak duren. Een benadering zullen waarschijnlijk verschillen, er is niets mis met verschillende methoden voor het oplossen van problemen. Echter als er geen een tot stand gebrachte benadering, kunt dit document kunt u het pad voor het ontwikkelen van uw eigen methoden, hulpprogramma's en voorkeuren voor problemen met het netwerk.
>
>

## <a name="network-components"></a>Netwerkonderdelen
Voordat spitten bij het oplossen van problemen bespreken we enkele algemene voorwaarden en onderdelen. Deze discussie zorgt ervoor dat we overweegt elk onderdeel in de end-to-end-keten waarmee connectiviteit in Azure.
[![1]][1]

Op het hoogste niveau beschrijven ik drie primaire netwerk Routeringsdomeinen;

- het Azure-netwerk (blauw cloud aan de rechterkant)
- de Internet- of WAN (groen cloud in het midden)
- het bedrijfsnetwerk (peach cloud aan de linkerkant)

Kijken naar het diagram van rechts naar links, gaan we kort elk onderdeel behandeld:
 - **Virtuele Machine** - de server mogelijk meerdere NIC's, zorg ervoor dat alle statische routes, standaardroutes en besturingssysteeminstellingen berichten te verzenden en ontvangen verkeer van de manier waarop u denkt dat het is. Elk VM-SKU heeft ook een beperking van de bandbreedte. Als u een kleinere VM SKU, wordt het verkeer beperkt door de bandbreedte die beschikbaar zijn naar de NIC. Ik een DS5v2 doorgaans gebruikt voor testdoeleinden (en vervolgens verwijderen eenmaal is klaar met het testen om geld besparen) om ervoor te zorgen voldoende bandbreedte op de virtuele machine.
 - **NIC** -Zorg ervoor dat u weet dat het persoonlijke IP-adres dat is toegewezen aan de NIC in kwestie.
 - **NIC NSG** : Er kan worden specifieke nsg's die worden toegepast op het niveau van de NIC, controleert u het NSG regelset geschikt is voor het verkeer dat u probeert om door te geven. Controleer bijvoorbeeld poorten 5201 voor iPerf, 3389 voor RDP of 22 voor SSH zijn geopend voor test-verkeer door te geven.
 - **VNet Subnet** -de NIC is toegewezen aan een specifiek subnet, zorg ervoor dat u weet welk één en de regels die zijn gekoppeld aan dat subnet.
 - **NSG voor subnet** - net als de NIC, het nsg's kunnen worden toegepast op het subnet ook. Zorg ervoor dat het NSG regelset geschikt is voor het verkeer dat u probeert om door te geven. (voor verkeer inkomend naar de NIC het NSG eerst toepast subnet en vervolgens het NSG NIC, als u daarentegen voor verkeer van de virtuele machine uitgaande het NSG NIC eerst toepast en het NSG voor Subnet binnenkomt in play).
 - **Subnet UDR** -gebruiker gedefinieerde Routes kan verkeer omleiden naar een tussenliggende hop (zoals een firewall of een load balancer). Zorg ervoor dat u weet of er een UDR in voor het verkeer en is als dus waar gaat en wat die volgende hop om verkeer te doen. (bijvoorbeeld een firewall kan doorgeven verkeer en het andere verkeer tussen de twee dezelfde hosts weigeren).
 - **Gateway-subnet / NSG / UDR** -net als bij het VM-subnet nsg's en udr's het gatewaysubnet hebben. Zorg ervoor dat u weet als ze er zijn en welke gevolgen op het verkeer hebben.
 - **VNet-Gateway (ExpressRoute)** -zodra peering (ExpressRoute) of VPN is ingeschakeld, er niet veel instellingen die invloed kunnen zijn op hoe of als het verkeer van routes. Als u meerdere ExpressRoute-circuits of VPN-tunnels die zijn verbonden met dezelfde VNet Gateway hebt, kunt u moet rekening houden met het gewicht verbindingsinstellingen als deze instelling is van invloed op verbinding voorkeur en is van invloed op het pad naar dat uw verkeer duurt.
 - **Filter routeren** (niet weergegeven) - een routefilter alleen van toepassing op Microsoft-Peering in ExpressRoute, maar is essentieel om te controleren als u niet ziet, de routes die u verwacht op Microsoft-Peering. 

U kunt op dit moment op het WAN-gedeelte van de koppeling. Dit routeringsdomein mag uw serviceprovider, uw zakelijke WAN of het Internet. Groot aantal hops, technologieën en bedrijven die betrokken zijn bij deze koppelingen kunnen lastig iets om op te lossen. Vaak het geval is, werkt u regel van Azure en uw bedrijfsnetwerken eerst voordat springen in deze verzameling van bedrijven en hops.

In het voorgaande diagram is aan de linkerkant uw bedrijfsnetwerk. Afhankelijk van de grootte van uw bedrijf zijn deze routeringsdomein enkele netwerkapparaten tussen u en het WAN of meerdere lagen van apparaten in een bereik/enterprise-netwerk.

Gezien de complexiteit van deze drie verschillende op hoog niveau netwerkomgevingen, het is vaak optimale om te beginnen bij de randen en probeert weer te geven waarbij prestaties goed is en waar deze vermindert. Deze aanpak kunt u het probleem routeringsdomein van de drie identificeren en richt u een oplossing die specifieke omgeving.

## <a name="tools"></a>Hulpprogramma's
De meeste netwerkproblemen kunnen worden geanalyseerd en geïsoleerd met eenvoudige hulpprogramma's zoals ping en traceroute. Het is trouwens zeldzaam dat u moet diep een analyse van pakketten via zoals Wireshark gaan. Om u te helpen bij het oplossen van problemen, worden de Azure-connectiviteit Toolkit (AzureCT) is ontwikkeld om sommige van deze hulpprogramma's in een eenvoudig pakket. Ik wil graag iPerf en psping om gebruiken voor het testen van de prestaties. iPerf is een veelgebruikte hulpprogramma en wordt uitgevoerd op de meeste besturingssystemen. iPerf geschikt is voor basic prestaties tests en redelijk eenvoudig te gebruiken. Psping om is een hulpprogramma ping die zijn ontwikkeld door SysInternals. Psping om is een eenvoudige manier om uit te voeren van ICMP- en TCP-pings in één opdracht voor ook eenvoudig te gebruiken. Beide van deze hulpprogramma's zijn lichtgewicht en zijn 'geïnstalleerd' door te kopiëren van de bestanden naar een map op de host.

Ik hebt alle van deze methoden en hulpprogramma's samengevoegd in een PowerShell-module (AzureCT) die u kunt installeren en gebruiken.

### <a name="azurect---the-azure-connectivity-toolkit"></a>AzureCT - de Toolkit Azure connectiviteit
De AzureCT PowerShell-module bestaat uit twee onderdelen [beschikbaarheid testen] [ Availability Doc] en [prestatietests][Performance Doc]. Dit document is alleen betrokken zijn bij de prestatietests, dus kunt richten op de twee opdrachten in de prestaties van de koppeling in deze PowerShell-module.

Er zijn drie eenvoudige stappen die met deze toolkit voor prestaties testen. 1) Installeer de PowerShell-module, 2) Installeer de ondersteunende toepassingen iPerf en psping om 3) de prestatietest uitvoeren.

1. De PowerShell-Module installeren

    ```powershell
    (new-object Net.WebClient).DownloadString("https://aka.ms/AzureCT") | Invoke-Expression
    
    ```

    Met deze opdracht de PowerShell-module downloadt en installeert deze lokaal.

2. De ondersteunende toepassingen installeren
    ```powershell
    Install-LinkPerformance
    ```
    Met deze opdracht AzureCT iPerf en psping om installeert in een nieuwe map 'C:\ACTTools', en deze ook de Windows Firewall-poorten voor het toestaan van ICMP en poort 5201 (iPerf)-verkeer wordt geopend.

3. De prestatietest uitvoeren

    U moet op de externe host eerst installeren en uitvoeren van iPerf in de servermodus. Zorg er ook voor de externe host luistert op beide 3389 (RDP voor Windows) of 22 (SSH voor Linux) en het toestaan van verkeer op poort 5201 voor iPerf. Als de externe host windows is, de AzureCT installeren en voer de opdracht Install-LinkPerformance iPerf en de firewall-regels nodig iPerf is gestart in de servermodus instellen. 
    
    Zodra de externe computer gereed is, opent u PowerShell op de lokale computer en de test te starten:
    ```powershell
    Get-LinkPerformance -RemoteHost 10.0.0.1 -TestSeconds 10
    ```

    Met deze opdracht wordt uitgevoerd een reeks gelijktijdige werklast en latentie tests uit om te schatten van de bandbreedtecapaciteit van en de latentie van de netwerkkoppeling van uw.

4. Controleer de uitvoer van de tests

    De indeling van de PowerShell-uitvoer er ongeveer als volgt:

    [![4]][4]

    De gedetailleerde resultaten van de iPerf en psping om tests worden in afzonderlijke tekstbestanden in de map van de hulpprogramma's AzureCT op 'C:\ACTTools'.

## <a name="troubleshooting"></a>Problemen oplossen
Als u de prestatietest niet geeft moet u verwachte resultaten, Uitzoeken waarom een progressieve stapsgewijs proces. Gezien het aantal onderdelen in het pad, biedt een systematische aanpak doorgaans een snellere pad oplossingstijd dan springen rond en mogelijk onnodig doen de dezelfde tests meerdere keren.

>[!NOTE]
>Het scenario hier is een prestatieprobleem, niet een verbindingsprobleem. De stappen zou afwijken als verkeer helemaal niet is doorgegeven.
>
>

Ten eerste uitdaging uw veronderstellingen. Is uw verwachting redelijke? Bijvoorbeeld als u een 1-Gbps ExpressRoute-circuit en 100 ms van latentie hebt is redelijkerwijs niet de volledige-1 Gbps van verkeer dat is opgegeven de prestatiekenmerken van TCP via verbindingen met hoge latentie. Zie de [verwijst naar de sectie](#references) voor meer op prestaties veronderstellingen.

Vervolgens moet ik het beste beginnen bij de randen tussen Routeringsdomeinen en probeer het isoleren van het probleem naar één primaire routeringsdomein; het bedrijfsnetwerk, het WAN of het Azure-netwerk. Personen die vaak schuldig de 'zwart vak' in het pad, terwijl de zwart vak blaming is eenvoudig doen, mogelijk aanzienlijk vertraagd resolutie met name als het probleem zich in een gebied dat u hebt de mogelijkheid om te wijzigen. Zorg ervoor dat u uw innen doen voordat het afleveren bij uw serviceprovider of Internet-provider.

Als u de belangrijkste routeringsdomein die wordt weergegeven om het probleem hebt geïdentificeerd, maakt u een diagram van het gebied in kwestie in. Op een whiteboard, Kladblok of Visio als een diagram biedt een concrete 'strijd kaart' om toe te staan een methodisch benadering van verdere isolaat het probleem. U kunt testen punten plannen en bijwerken van de kaart als u gebieden of dig diepere loop van het testen wist.

Nu dat u een diagram hebt, kunt u beginnen met het netwerk in segmenten opdelen en beperken van het probleem. Ontdek waar het werkt en waar dat niet het geval. Houd verplaatsen van uw test verwijst naar het naar beneden op het onderdeel strijdige isoleren.

Bovendien Vergeet niet om te kijken naar andere lagen van het OSI-model. Het is gemakkelijk te concentreren op het netwerk en lagen 1-3 (fysiek, gegevens en het netwerk lagen) maar de problemen kunnen ook op laag 7 in de toepassingslaag. Een open rekening te houden en controleer of veronderstellingen.

## <a name="advanced-expressroute-troubleshooting"></a>Geavanceerde probleemoplossing van ExpressRoute
Als u niet zeker weet waar de rand van de cloud daadwerkelijk is zijn isoleren van de Azure-onderdelen moeilijk. Wanneer u ExpressRoute gebruikt, is de rand een netwerkonderdeel de Microsoft Enterprise Edge (MSEE) genoemd. **Wanneer u ExpressRoute**, de MSEE is de eerste contactpunt in het netwerk van Microsoft en de laatste hop waardoor het Microsoft-netwerk. Wanneer u een verbindingsobject tussen de gateway van uw VNet en het ExpressRoute-circuit maakt, bent u daadwerkelijk maken van een verbinding met de MSEE. De MSEE herkennen als de eerste of laatste hop (afhankelijk van welke richting u gaat) is essentieel om te isoleren van problemen met het Azure-netwerk te bewijzen dat het probleem zich in Azure of downstream verder in het WAN of het bedrijfsnetwerk. 

[![2]][2]

>[!NOTE]
> U ziet dat de MSEE bevindt zich niet in de Azure-cloud. ExpressRoute is daadwerkelijk op de rand van het Microsoft-netwerk niet daadwerkelijk in Azure. Als u bent verbonden met ExpressRoute voor een MSEE, moet u verbonden bent met het netwerk van Microsoft, daar vervolgens gaat u naar een van de cloudservices, zoals Office 365 (met Microsoft-Peering) of Azure (met particuliere en/of Microsoft-Peering).
>
>

Als twee VNets (VNets A en B in het diagram) zijn verbonden met de **dezelfde** ExpressRoute-circuit, kunt u een reeks tests isoleren van het probleem in Azure (of aantonen dat u zich niet in Azure) uitvoeren
 
### <a name="test-plan"></a>Testplan
1. De test Get-LinkPerformance tussen VM1 en VM2 uitvoert. Deze test biedt inzicht in als het probleem zich lokaal of niet. Als deze test aanvaardbare latentie en bandbreedte resultaten produceert, kunt u het lokale netwerk van de VNet markeren als goed.
2. Ervan uitgaande dat de lokale VNet is verkeer goed, voert u de test Get-LinkPerformance tussen VM1 en VM3. Deze test oefent de verbinding via het Microsoft-netwerk naar de MSEE en terug in Azure. Als deze test aanvaardbare latentie en bandbreedte resultaten produceert, kunt u het Azure-netwerk markeren als goed.
3. Als Azure is uitgesloten, kunt u een vergelijkbare reeks tests uitvoeren op uw bedrijfsnetwerk. Als dat ook tests goed, is het tijd om te werken met uw serviceprovider of Internet-provider voor het vaststellen van de WAN-verbinding. Voorbeeld: Deze test worden uitgevoerd tussen twee filialen, of uw helpdesk en een data center-server. Afhankelijk van wat u test, vinden eindpunten (servers, pc's, enzovoort) die kunnen uitoefenen dat pad.

>[!IMPORTANT]
> Het is essentieel dat voor elke test u markeert het tijdstip waarop die u de test uitvoert en de resultaten in een veelgebruikte locatie opnemen (ik zoals OneNote of Excel). Elke test uitvoeren moet identiek uitvoer hebben, zodat u kunt de resulterende gegevens van de test wordt uitgevoerd met elkaar vergelijken en geen 'gaten' in de gegevens. Consistentie tussen meerdere tests is de primaire reden dat ik de AzureCT gebruiken voor het oplossen van problemen. De magie bevindt zich niet in de exacte load-scenario's ik, maar in plaats daarvan uitvoeren de *magic* is van het feit dat ik krijg een *consistente test- en uitvoer* van elke test. Opnemen van de tijd en ervoor te zorgen dat consistente gegevens elke één is vooral nuttig als u later vinden dat het probleem enkel geval is. Toegewijd aan de gegevensverzameling van uw vooraf zijn en voorkomt u uren aan bewaartijd de dezelfde scenario's (ik geleerd harde zo lang geleden).
>
>

## <a name="the-problem-is-isolated-now-what"></a>Het probleem is geïsoleerd, wat nu?
Hoe kunt u isoleren het probleem des te gemakkelijker is om op te lossen, maar vaak het bereiken van het punt waar u diepere of verdere kan niet uw op te lossen gaat. Voorbeeld: ziet u de koppeling tussen uw serviceprovider duurt hops via Europa, maar het verwachte pad is in Azië. Dit punt is wanneer u voor hulp bereiken moet. Wie u vragen is afhankelijk van het routeringsdomein geïsoleerd van het probleem op, of nog beter als u deze naar beneden op een bepaald onderdeel te beperken.

Voor problemen met het bedrijfsnetwerk, uw interne IT-afdeling of serviceprovider ondersteuning van uw netwerk (die mogelijk de hardwarefabrikant) mogelijk om te helpen bij het configureren van een apparaat of reparatie van de hardware.

Voor het WAN kunt uw testresultaten delen met uw serviceprovider of de Internetprovider ze gestart en te voorkomen dat die een deel van de dezelfde grond die u al hebt getest. Echter niet worden offended u als ze wilt controleren of uw resultaten zelf. "Vertrouwt, maar controleren" is een goede motto bij het oplossen van problemen op basis van andere gebruikers gemelde resultaten.

Met Azure, zodra het isoleren van het probleem zo gedetailleerd zoals u kunt, is het tijd om te controleren de [Azure Network-documentatie] [ Network Docs] en klik indien nodig nog steeds [opent u een ondersteuningsticket][Ticket Link].

## <a name="references"></a>Verwijzingen
### <a name="latencybandwidth-expectations"></a>Latentie/bandbreedte verwachtingen
>[!TIP]
> Geografische latentie (mijl of kilometer) tussen de eindpunten die u test is tot nu toe door de grootste onderdeel van de latentie. Hoewel er apparatuur latentie (fysieke en virtuele-onderdelen, aantal hops, enzovoort) die zijn betrokken, is Geografie gebleken het grootste onderdeel van de totale latentie WAN-verbindingen betreft. Het is ook belangrijk te weten dat de afstand wordt de afstand van de fiber niet de lineaire uitvoeren of de routekaart afstand. Deze afstand is zeer moeilijk om op te halen met alle nauwkeurigheid. Als gevolg hiervan ik in het algemeen gebruik van een stad afstand Rekenmachine op het internet en weten dat deze methode een meting aangemerkt onnauwkeurig is, maar is voldoende om in te stellen een algemeen verwachting.
>
>

Ik heb een ExpressRoute-installatie in Seattle, Washington, in de Verenigde Staten. De volgende tabel toont de latentie en bandbreedte die ik gezien testen naar verschillende locaties voor Azure. Ik heb de geografische afstand tussen beide uiteinden van de test geschat.

Instellingen testen:
 - Een fysieke server met Windows Server 2016 met een 10 Gbps NIC, verbonden met een ExpressRoute-circuit.
 - Een 10 Gbps Premium ExpressRoute-circuit in de locatie die wordt geïdentificeerd met persoonlijke Peering is ingeschakeld.
 - Een Azure VNet met een gateway UltraPerformance in de opgegeven regio.
 - Een DS5v2 virtuele machine met Windows Server 2016 op het VNet. De virtuele machine is niet van het domein is toegevoegd, gebouwd op basis van de standaard Azure afbeelding (geen optimalisatie of aanpassing) met AzureCT geïnstalleerd.
 - Alle tests is AzureCT Get-LinkPerformance opdracht gebruiken met een 5 minuten belastingtest uit voor elk van de zes test worden uitgevoerd. Bijvoorbeeld:

    ```powershell
    Get-LinkPerformance -RemoteHost 10.0.0.1 -TestSeconds 300
    ```
 - De gegevensstroom voor elke test heeft de belasting van de lokale fysieke server (iPerf client in Haarlem) tot de virtuele machine van Azure (iPerf server in de vermelde Azure-regio).
 - De gegevens van de kolom 'Latentie' is van de test niet laden (een TCP-latentie test zonder iPerf uitgevoerd).
 - De gegevens van de kolom 'Max Bandwidth' is van 16 TCP-stroom belastingtest uit met een venstergrootte 1 Mb.

[![3]][3]

### <a name="latencybandwidth-results"></a>Latentie/bandbreedte resultaten
>[!IMPORTANT]
> Deze getallen zijn algemeen alleen ter informatie. Veel factoren van invloed zijn op latentie en deze waarden zijn in het algemeen consistent gedurende een bepaalde periode, voorwaarden in Azure of het netwerk serviceproviders verkeer via verschillende paden op elk gewenst moment kunnen verzenden, waardoor latentie en bandbreedte kunnen worden beïnvloed. De effecten van deze wijzigingen resulteren niet in het algemeen in belangrijke verschillen.
>
>

| | | | | | |
|-|-|-|-|-|-|
|ExpressRoute<br/>Locatie|Azure<br/>Regio|Schatting<br/>Afstand (km)|Latentie|1-sessie<br/>Bandbreedte|Maximum<br/>Bandbreedte|
| Seattle | VS - west 2        |    191 km |   5 ms | 262.0 Mbit per seconde |  3,74 Gbit per seconde | 21
| Seattle | VS - west          |  1,094 km |  18 ms |  82.3 Mbit per seconde |  3.70 Gbit per seconde | 20
| Seattle | VS - midden       |  2,357 km |  40 ms |  38.8 Mbit per seconde |  2,55 Gbit per seconde | 17
| Seattle | Zuid-centraal VS |  2,877 km |  51 ms |  30,6 mil Mbit per seconde |  2.49 Gbit per seconde | 19
| Seattle | Noord-centraal VS |  2,792 km |  55 ms |  27.7 Mbit per seconde |  2.19 Gbit per seconde | 18
| Seattle | VS - oost 2        |  3,769 km |  73 ms |  21,3 Mbit per seconde |  1.79 Gbit per seconde | 16
| Seattle | VS - oost          |  3,699 km |  74 ms |  21.1 Mbit per seconde |  1.78 Gbit per seconde | 15
| Seattle | Japan - oost       |  7,705 km | 106 ms |  14,6 Mbit per seconde |  1.22 Gbit per seconde | 28
| Seattle | Verenigd Koninkrijk Zuid         |  7,708 km | 146 ms |  10.6 Mbit per seconde |   896 Mbit per seconde | 24
| Seattle | West-Europa      |  7,834 km | 153 ms |  10.2 Mbit per seconde |   761 Mbit per seconde | 23
| Seattle | Australië - oost   | 12,484 km | 165 ms |   9.4 Mbit per seconde |   794 Mbit per seconde | 26
| Seattle | Zuidoost-Azië   | 12,989 km | 170 ms |   9.2 Mbit per seconde |   756 Mbit per seconde | 25
| Seattle | Brazilië-Zuid *   | 10,930 km | 189 ms |   8.2 Mbit per seconde |   699 Mbit per seconde | 22
| Seattle | Zuid-India      | 12,918 km | 202 ms |   7,7 Mbit per seconde |   634 Mbit per seconde | 27

\*De latentie voor Brazilië is een goed voorbeeld waar de lineaire afstand aanzienlijk verschilt van de fiber afstand worden uitgevoerd. Ik verwachten dat de latentie in de groep van 160 ms worden zou, maar daadwerkelijk 189 ms is. Dit verschil tegen mijn verwachting kan duiden op een netwerkprobleem ergens echter waarschijnlijk dat de fiber uitgevoerd niet naar Brazilië op één lijn en heeft een extra 1000 km of dus reizen ophalen voor Brazilië van Seattle.

## <a name="next-steps"></a>Volgende stappen
1. De werkset Azure connectiviteit downloaden vanuit GitHub op [http://aka.ms/AzCT][ACT]
2. Volg de instructies voor [prestatietests koppelen][Performance Doc]

<!--Image References-->
[1]: ./media/expressroute-troubleshooting-network-performance/network-components.png "azure netwerkonderdelen"
[2]: ./media/expressroute-troubleshooting-network-performance/expressroute-troubleshooting.png "ExpressRoute probleemoplossing"
[3]: ./media/expressroute-troubleshooting-network-performance/test-diagram.png "Perf-testomgeving"
[4]: ./media/expressroute-troubleshooting-network-performance/powershell-output.png "PowerShell-uitvoer"

<!--Link References-->
[Performance Doc]: https://github.com/Azure/NetworkMonitoring/blob/master/AzureCT/PerformanceTesting.md
[Availability Doc]: https://github.com/Azure/NetworkMonitoring/blob/master/AzureCT/AvailabilityTesting.md
[Network Docs]: https://docs.microsoft.com/azure/index#pivot=services&panel=network
[Ticket Link]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview
[ACT]: http://aka.ms/AzCT











