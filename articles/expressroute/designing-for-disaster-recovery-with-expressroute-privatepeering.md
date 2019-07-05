---
title: Ontwerpen voor herstel na noodgeval met Azure ExpressRoute | Microsoft Docs
description: Deze pagina bevat architectuur aanbevelingen voor herstel na noodgevallen tijdens het gebruik van Azure ExpressRoute.
documentationcenter: na
services: networking
author: rambk
manager: tracsman
ms.service: expressroute
ms.topic: article
ms.workload: infrastructure-services
ms.date: 05/25/2019
ms.author: rambala
ms.openlocfilehash: cf2b4e385de901254fde3c3d3e807feda98d5b41
ms.sourcegitcommit: c63e5031aed4992d5adf45639addcef07c166224
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67466076"
---
# <a name="designing-for-disaster-recovery-with-expressroute-private-peering"></a>Ontwerpen voor herstel na noodgeval met ExpressRoute-privépeering

ExpressRoute is ontworpen voor hoge beschikbaarheid voor de provider over geavanceerde privénetwerkverbinding met Microsoft-resources. Met andere woorden, is er geen single point of failure wordt in het pad ExpressRoute binnen Microsoft-netwerk. Zie voor overwegingen bij het ontwerp voor een maximale de beschikbaarheid van een ExpressRoute-circuit, [ontwerpen voor hoge beschikbaarheid met ExpressRoute][HA].

Echter, nemen de Murphy populaire eigen werk beoordeelt--*als iets fout gaat kunt, wordt deze*--in acht genomen, in dit artikel laten we ons bezighouden met oplossingen die verder gaan dan de fouten die kunnen worden aangepakt met behulp van een enkel ExpressRoute-circuit. Met andere woorden, in dit artikel laat het ons kijken aandachtspunten voor netwerken architectuur voor het bouwen van robuuste back-end-netwerkconnectiviteit voor herstel na noodgevallen met geo-redundante ExpressRoute-circuits.

## <a name="need-for-redundant-connectivity-solution"></a>Redundante connectiviteit oplossing nodig

Zijn er mogelijkheden en exemplaren waar een hele regionale service (die van Microsoft, network serviceproviders, klant of andere cloud-serviceproviders worden) wordt verminderd. De hoofdoorzaak van dergelijke invloed op een regionale breed service natuurlijke calamity bevatten. Zakelijke continuïteit en belangrijke kritieke toepassingen is daarom belangrijk om te plannen voor herstel na noodgevallen.   

Ongeacht of u uw essentiële toepassingen in een Azure-regio of on-premises of ergens anders uitvoert, kunt u een andere Azure-regio als uw failoversite. De volgende artikelen adressen herstel na noodgevallen van toepassingen en front-end toegang perspectieven:

- [De herstel na noodgeval op ondernemingsschaal][Enterprise DR]
- [SMB-noodherstel met Azure Site Recovery][SMB DR]

Als u ExpressRoute-connectiviteit tussen uw on-premises netwerk en Microsoft voor essentiële bewerkingen missie afhankelijk zijn, moet ook geografisch redundante netwerkverbinding opnemen in uw plan voor herstel na noodgevallen. 

## <a name="challenges-of-using-multiple-expressroute-circuits"></a>Uitdagingen van het gebruik van meerdere ExpressRoute-circuits

Wanneer u dezelfde set netwerken die gebruikmaken van meer dan één verbinding interconnect, introduceert u parallelle paden tussen de netwerken. Parallelle paden, als u niet goed ontworpen, kan leiden tot asymmetrische routering. Hebt u stateful entiteiten (bijvoorbeeld NAT, firewall) in het pad, kan asymmetrische routering netwerkverkeer blokkeren.  Normaal gesproken over de persoonlijke ExpressRoute-peeringpad wordt niet u tijdens het stateful entiteiten zoals NAT- of Firewalls. Daarom asymmetrische routering via ExpressRoute-privépeering niet per se geblokkeerd netwerkverkeer.
 
Als u saldo verkeer via geo-redundante parallelle paden laden, ongeacht of u stateful entiteiten of niet hebt, zou u inconsistente network prestaties ervaren. In dit artikel laten we bespreken hoe u deze problemen aan te pakken.

## <a name="small-to-medium-on-premises-network-considerations"></a>Kleine tot middelgrote on-premises netwerkoverwegingen

Laten we eens de voorbeeld-netwerk in het volgende diagram wordt geïllustreerd. In het voorbeeld, geografisch redundante ExpressRoute-verbinding tot stand is gebracht tussen een Contoso on-premises locatie en Contoso VNet in een Azure-regio. In het diagram, groene lijn geeft voorkeurspad (via ExpressRoute 1) en de stippellijn met een stand-by-pad (via ExpressRoute 2) vertegenwoordigt.

[![1]][1]

Wanneer u een ExpressRoute-connectiviteit voor herstel na noodgevallen ontwerpt, moet u rekening houden met:

- met behulp van geo-redundante ExpressRoute-circuits
- met behulp van diverse serviceprovider-netwerken voor andere ExpressRoute-circuit
- het ontwerpen van elk van de ExpressRoute-circuit voor [hoge beschikbaarheid][HA]
- beëindigen van de andere ExpressRoute-circuit in een andere locatie op het netwerk van de klant

Standaard, als u identieke routes adverteren via alle ExpressRoute-paden Azure zal verdelen on-premises gebonden verkeer over alle ExpressRoute-paden met gelijke kosten Multipath (ECMP) routering.

Met de geografisch redundante ExpressRoute-circuits moeten we echter in overweging ander netwerk prestaties met verschillende netwerkpaden (met name voor netwerklatentie) te nemen. Als u consistente prestaties tijdens normale werking van het netwerk, kunt u het ExpressRoute-circuit met de minimale latentie.

U kunt van invloed zijn op Azure liever één ExpressRoute-circuit via een andere naam met een van de volgende technieken (weergegeven in de volgorde van effectiviteit):

- meer specifieke route adverteren via de voorkeur ExpressRoute-circuit ten opzichte van andere ExpressRoute-circuit
- hoger gewicht van de verbinding configureren voor de verbinding die is gekoppeld aan het virtuele netwerk naar het gewenste ExpressRoute-circuit
- de routes te adverteren via minder voorkeur ExpressRoute-circuit met langer AS-pad (zoals pad toevoegen)

### <a name="more-specific-route"></a>Meer specifieke route

Het volgende diagram illustreert beïnvloeden ExpressRoute Padselectie met behulp van meer specifieke route-aankondiging. In het voorbeeld, Contoso on-premises/24 IP-adresbereik wordt aangekondigd als twee /25-adresbereiken via het voorkeurspad (ExpressRoute-1) en als /24 via het pad stand-by (ExpressRoute-2).

[![2]][2]

Omdat /25 specifiekere, zou vergeleken met /24, Azure verzendt het verkeer dat is bestemd voor 10.1.11.0/24 via ExpressRoute 1 in de normale status. Als zowel de verbindingen van ExpressRoute 1 omlaag gaan, klikt u vervolgens ziet het VNet u de aankondiging van de route 10.1.11.0/24 alleen via ExpressRoute 2; en daarom de stand-by-circuit wordt gebruikt in deze foutstatus.

### <a name="connection-weight"></a>Gewicht van de verbinding

De volgende schermafbeelding ziet u het gewicht van een ExpressRoute-verbinding via Azure portal configureren.

[![3]][3]

Het volgende diagram illustreert beïnvloeden ExpressRoute Padselectie met behulp van de verbinding gewicht. Het gewicht van de verbinding standaard is 0. In het volgende voorbeeld wordt is het gewicht van de verbinding voor ExpressRoute-1 geconfigureerd als 100. Wanneer een VNet ontvangt een route voorvoegsel worden verzonden via meer dan één ExpressRoute-circuit, wordt de verbinding met het hoogste gewicht liever in het VNet.

[![4]][4]

Als zowel de verbindingen van ExpressRoute 1 omlaag gaan, klikt u vervolgens ziet het VNet u de aankondiging van de route 10.1.11.0/24 alleen via ExpressRoute 2; en daarom de stand-by-circuit wordt gebruikt in deze foutstatus.

### <a name="as-path-prepend"></a>Als het pad toevoegen

Het volgende diagram illustreert beïnvloeden ExpressRoute Padselectie gebruiken als pad toevoegen. In het diagram geeft de aankondiging route via ExpressRoute 1 het standaardgedrag van eBGP. Op de aankondiging van de route via ExpressRoute 2, wordt de on-premises netwerk ASN voorafgegaan daarnaast op van de route als pad. Wanneer dezelfde route wordt ontvangen via meerdere ExpressRoute-circuits, per het selectieproces van eBGP route liever VNet de route met het kortste pad. 

[![5]][5]

Als zowel de ExpressRoute-1-verbindingen gaat u naar beneden, zou de aankondiging van de route 10.1.11.0/24 alleen via ExpressRoute 2 zien op het VNet. Consequentially, hoe langer pad zou worden niet van belang. Daarom zou de stand-by-circuit worden gebruikt in deze foutstatus.

Met behulp van een van de technieken, als u Azure liever een van uw ExpressRoute via anderen beïnvloeden, moet u ook om te controleren of de on-premises netwerk ook de voorkeur geeft aan hetzelfde ExpressRoute-pad voor Azure verkeer om te voorkomen dat asymmetrische stromen. Lokale voorkeurswaarde wordt meestal gebruikt om on-premises netwerk één ExpressRoute-circuit liever via anderen beïnvloeden. Lokale voorkeur is een interne BGP (iBGP) metrische gegevens. De BGP-route met de hoogste waarde van de lokale voorkeur verdient de voorkeur.

> [!IMPORTANT]
> Als u bepaalde ExpressRoute-circuits als stand-by, moet u actief te beheren en failover-bewerking periodiek te testen. 
> 

## <a name="large-distributed-enterprise-network"></a>Grote gedistribueerde bedrijfsnetwerk

Wanneer u een grote ondernemingen-netwerk hebt, bent u waarschijnlijk meerdere ExpressRoute-circuits. Kijken hoe u herstel na noodgevallen met behulp van de actief / actief ExpressRoute-circuits, zonder extra stand-by-circuits ontwerpt in deze sectie. 

Laten we eens in het voorbeeld in het volgende diagram wordt geïllustreerd. In het voorbeeld, Contoso heeft twee on-premises locaties die zijn verbonden met twee Contoso IaaS-implementatie in twee verschillende Azure-regio's via ExpressRoute-circuits in twee verschillende peeringlocaties. 

[![6]][6]

Hoe we het herstel na noodgevallen ontwerpen heeft een invloed op hoe locatie (Regio1/gebied2 naar location2/location1)-verkeer wordt doorgestuurd cross regionale overschrijden. Laten we eens twee verschillende na noodgevallen architecturen waarmee verkeer tussen regio-locatie verschillend.

### <a name="scenario-1"></a>Scenario 1

In het eerste scenario gaan we ontwerpen herstel na noodgevallen, zodat al het verkeer tussen een virtueel Azure-regio en on-premises netwerk door het lokale ExpressRoute-circuit in de actieve status stromen. Als het lokale ExpressRoute-circuit niet beschikbaar is, klikt u vervolgens het externe ExpressRoute-circuit wordt gebruikt voor alle verkeersstromen tussen Azure en on-premises netwerk.

Scenario 1 wordt weergegeven in het volgende diagram. In het diagram geven groen regels paden voor de verkeersstroom tussen VNet1 en on-premises netwerken. De blauwe regels geven paden voor de verkeersstroom tussen VNet2 en on-premises netwerken. Effen regels geven aan de gewenste pad in de rustige toestand en de stippellijnen geven aan verkeer het pad in het uitvallen van het bijbehorende ExpressRoute-circuit die onveranderlijk verkeersstroom uitvoert. 

[![7]][7]

U kunt het gewicht van de verbinding met VNets liever verbinding met lokale peeringlocatie ExpressRoute voor on-premises netwerk verkeer afhankelijk is van invloed zijn op scenario ontwerpen. Voor het voltooien van de oplossing, moet u ervoor zorgen symmetrisch terugkerend verkeersstroom. Kunt u lokale voorkeur van de sessie iBGP tussen uw BGP-routers (waarop ExpressRoute-circuits worden beëindigd aan on-premises) naar een ExpressRoute-circuit de voorkeur geeft aan. De oplossing wordt weergegeven in het volgende diagram. 

[![8]][8]

### <a name="scenario-2"></a>Scenario 2

De Scenario-2 wordt weergegeven in het volgende diagram. In het diagram geven groen regels paden voor de verkeersstroom tussen VNet1 en on-premises netwerken. De blauwe regels geven paden voor de verkeersstroom tussen VNet2 en on-premises netwerken. In de actieve-status (ononderbroken lijnen in het diagram), alle flow via Microsoft-backbone voor het overgrote deel het verkeer tussen VNets en on-premises locaties en via de onderlinge verbinding tussen on-premises locaties alleen in de foutstatus (stippellijnen in het diagram) van een ExpressRoute.

[![9]][9]

De oplossing wordt weergegeven in het volgende diagram. Zoals wordt geïllustreerd, kunt u het scenario ofwel ontwerpen met behulp van meer specifieke route (optie 1) of AS-pad (optie 2) Voeg VNet Padselectie beïnvloeden. On-premises netwerk route selecteren voor Azure-gebonden verkeer beïnvloeden, moet u de onderlinge verbinding tussen de on-premises locatie configureren als minder zijn beter. Manier waarop u de koppeling geheugengebieden configureren als beter, is afhankelijk van de routing protocol dat wordt gebruikt in de on-premises netwerk. U kunt de lokale voorkeur met iBGP of metrische waarde met IGP (OSPF of IS IS).

[![10]][10]


## <a name="next-steps"></a>Volgende stappen

In dit artikel wordt besproken hoe u kunt ontwerpen voor herstel na noodgevallen van de connectiviteit voor persoonlijke peering van een ExpressRoute-circuit. De volgende artikelen adressen herstel na noodgevallen van toepassingen en front-end toegang perspectieven:

- [De herstel na noodgeval op ondernemingsschaal][Enterprise DR]
- [SMB-noodherstel met Azure Site Recovery][SMB DR]

<!--Image References-->
[1]: ./media/designing-for-disaster-recovery-with-expressroute-pvt/one-region.png "kleine tot middelgrote on-premises netwerkoverwegingen"
[2]: ./media/designing-for-disaster-recovery-with-expressroute-pvt/specificroute.png "Padselectie met behulp van meer specifieke routes beïnvloeden"
[3]: ./media/designing-for-disaster-recovery-with-expressroute-pvt/configure-weight.png "gewicht verbinding via Azure portal configureren"
[4]: ./media/designing-for-disaster-recovery-with-expressroute-pvt/connectionweight.png "Padselectie met behulp van de verbinding gewicht beïnvloeden"
[5]: ./media/designing-for-disaster-recovery-with-expressroute-pvt/aspath.png "toegestaan Padselectie gebruiken als pad toevoegen"
[6]: ./media/designing-for-disaster-recovery-with-expressroute-pvt/multi-region.png "grote gedistribueerde on-premises netwerk overwegingen"
[7]: ./media/designing-for-disaster-recovery-with-expressroute-pvt/multi-region-arch1.png "scenario 1"
[8]: ./media/designing-for-disaster-recovery-with-expressroute-pvt/multi-region-sol1.png "actief / actief ExpressRoute circuits oplossing 1"
[9]: ./media/designing-for-disaster-recovery-with-expressroute-pvt/multi-region-arch2.png "scenario 2"
[10]: ./media/designing-for-disaster-recovery-with-expressroute-pvt/multi-region-sol2.png "actief / actief ExpressRoute circuits oplossing 2"

<!--Link References-->
[HA]: https://docs.microsoft.com/azure/expressroute/designing-for-high-availability-with-expressroute
[Enterprise DR]: https://azure.microsoft.com/solutions/architecture/disaster-recovery-enterprise-scale-dr/
[SMB DR]: https://azure.microsoft.com/solutions/architecture/disaster-recovery-smb-azure-site-recovery/
[con wgt]: https://docs.microsoft.com/azure/expressroute/expressroute-optimize-routing#solution-assign-a-high-weight-to-local-connection
[AS Path Pre]: https://docs.microsoft.com/azure/expressroute/expressroute-optimize-routing#solution-use-as-path-prepending





