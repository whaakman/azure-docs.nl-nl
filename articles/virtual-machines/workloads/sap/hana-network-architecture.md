---
title: Het netwerk van de architectuur van SAP HANA op Azure (grote instanties) | Microsoft Docs
description: Architectuur van SAP HANA op Azure (grote instanties) implementeren op het netwerk.
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/25/2019
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8794a93cecb50774f30746c22931db31a9fa9194
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66239587"
---
# <a name="sap-hana-large-instances-network-architecture"></a>Netwerkarchitectuur van SAP HANA (grote instanties)

De architectuur van Azure network services is een belangrijk onderdeel van de succesvolle implementatie van SAP-toepassingen op HANA grote instantie. SAP HANA op Azure (grote instanties) implementaties hebben meestal een grotere SAP-landschap met diverse verschillende SAP-oplossingen met verschillende grootten van databases, resourceverbruik CPU en geheugengebruik. Is het waarschijnlijk dat niet alle IT-systemen bevinden zich in Azure al. Uw SAP-landschap dat is vaak hybride ook van een DBMS punt en de SAP-toepassing oogpunt met behulp van een combinatie van NetWeaver, en S/4HANA, SAP HANA en andere DBMS-systemen. Azure biedt verschillende services waarmee u kunt de verschillende DBMS-systemen, NetWeaver en S/4HANA-systemen worden uitgevoerd in Azure. Azure ook biedt die u technologie zodat er Azure-netwerk, zoals een virtueel Datacenter naar uw on-premises software-implementaties

Tenzij uw volledige IT-systemen worden gehost in Azure. Functionaliteit van Azure-netwerk wordt gebruikt om de wereld van de on-premises verbinden met uw Azure-assets te zoeken, zoals een virtueel datacenter van uw Azure. De functionaliteit van de Azure-netwerk gebruikt is: 

- Virtuele Azure-netwerken zijn verbonden met de [ExpressRoute](https://azure.microsoft.com/services/expressroute/) circuit dat is verbonden met uw lokale netwerk activa.
- Een ExpressRoute-circuit dat is verbonden on-premises naar Azure moet een minimale bandbreedte van [1 Gbps of hoger](https://azure.microsoft.com/pricing/details/expressroute/). Deze minimale bandbreedte kunt voldoende bandbreedte voor de overdracht van gegevens tussen on-premises systemen en systemen die worden uitgevoerd op virtuele machines. Daarnaast kunt u voldoende bandbreedte voor de verbinding met Azure-systemen van on-premises gebruikers.
- Alle SAP-systemen in Azure worden ingesteld in virtuele netwerken met elkaar communiceren.
- Active Directory en DNS-on-premises gehost worden uitgebreid naar Azure via ExpressRoute van on-premises zijn of voltooid in Azure worden uitgevoerd.

Voor de specifieke aanvraag van de integratie van HANA grote instanties in de Azure data center netwerk-fabric, wordt Azure ExpressRoute-technologie en gebruikt


> [!NOTE] 
> Slechts één Azure-abonnement kan worden gekoppeld aan slechts één tenant in een stempel HANA grote instantie in een specifieke Azure-regio. Daarentegen kan één HANA grote instantie stempel tenant worden gekoppeld aan slechts één Azure-abonnement. Deze vereiste is consistent met andere factureerbare objecten in Azure.

Als SAP HANA op Azure (grote instanties) is geïmplementeerd in meerdere verschillende Azure-regio's, wordt een afzonderlijke tenant wordt geïmplementeerd in de stempel HANA grote instantie. U kunt beide onder hetzelfde Azure-abonnement uitvoeren, zolang deze exemplaren onderdeel van de SAP-landschap dat dezelfde zijn. 

> [!IMPORTANT] 
> Alleen de implementatiemethode van Azure Resource Manager wordt ondersteund met SAP HANA op Azure (grote instanties).

 

## <a name="additional-virtual-network-information"></a>Informatie over aanvullende virtuele netwerken

Voor een virtueel netwerk verbinding met ExpressRoute, moet een Azure ExpressRoute-gateway worden gemaakt. Zie voor meer informatie, [over Expressroute-gateways voor ExpressRoute](../../../expressroute/expressroute-about-virtual-network-gateways.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

Een Azure ExpressRoute-gateway wordt gebruikt met ExpressRoute naar een infrastructuur buiten Azure of naar een Azure Large Instance-stempel. U kunt de Azure ExpressRoute-gateway met een maximum van vier andere ExpressRoute-circuits verbinden, zolang deze verbindingen afkomstig van andere Microsoft enterprise-randrouters zijn. Zie voor meer informatie, [SAP HANA (grote instanties)-infrastructuur en connectiviteit in Azure](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

> [!NOTE] 
> De maximale doorvoer die u met een ExpressRoute-gateway bereiken kunt is 10 Gbps met behulp van een ExpressRoute-verbinding. Kopiëren van bestanden tussen een virtuele machine die zich in een virtueel netwerk bevinden en een on-premises (als een stroom één exemplaar) de volledige doorvoer van de andere gateway-SKU's niet bereiken. Als u wilt gebruikmaken van de volledige bandbreedte van de ExpressRoute-gateway, gebruik van meerdere streams. Of u moet verschillende bestanden in parallelle stromen van één enkel bestand kopiëren.


## <a name="networking-architecture-for-hana-large-instance"></a>Architectuur van de netwerken voor HANA grote instantie
De VPN-architectuur voor HANA grote instantie kan worden onderverdeeld in vier verschillende onderdelen:

- On-premises netwerken en ExpressRoute-verbinding naar Azure. Dit onderdeel is van de klant domein en is verbonden met Azure via ExpressRoute. Dit Expressroute-circuit is volledig betaalde door u als een klant. De bandbreedte moet groot genoeg is voor het verwerken van netwerkverkeer tussen uw on-premises assets en de u verbinding maakt op basis van Azure-regio. Zie de rechtsonder in de volgende afbeelding.
- Netwerk van Azure-services, zoals eerder besproken, met virtuele netwerken, die opnieuw ExpressRoute-gateways toegevoegd moeten. Dit onderdeel is een gebied waar u nodig hebt om te vinden van de juiste ontwerpen voor uw toepassingsvereisten-, beveiligings- en nalevingsvereisten. Of u HANA grote instantie is een ander punt rekening houden met betrekking tot het aantal virtuele netwerken en Azure gateway-SKU's waaruit u kunt kiezen. Zie de rechtsboven in de afbeelding.
- Connectiviteit van HANA grote instantie via ExpressRoute-technologie in Azure. Dit onderdeel is geïmplementeerd en uitgevoerd door Microsoft. U hoeft alleen is bieden bepaalde IP-adresbereiken na de implementatie van uw activa in HANA grote instantie verbinding maken met het ExpressRoute-circuit met de virtuele netwerken. Zie voor meer informatie, [SAP HANA (grote instanties)-infrastructuur en connectiviteit in Azure](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Er zijn geen extra kosten voor u als een klant voor de connectiviteit tussen de Azure data center netwerkinfrastructuur en HANA grote instantie eenheden.
- Netwerken binnen de stempel HANA grote instantie, die voornamelijk transparant is voor u.

![Virtuele netwerken die zijn verbonden met SAP HANA op Azure (grote instanties) en on-premises](./media/hana-overview-architecture/image1-architecture.png)

De vereiste dat uw on-premises assets verbinding via ExpressRoute naar Azure maken moeten verandert niet omdat u gebruikmaakt van HANA grote instantie. De vereiste voor een of meerdere virtuele netwerken die worden uitgevoerd van de virtuele machines, waarop het niveau van de toepassing die verbinding met de HANA-instanties die worden gehost in eenheden van HANA grote instantie maakt wordt gehost, hebben ook wijzigen niet. 

De verschillen naar SAP-oplossingen in Azure zijn:

- De eenheden HANA grote instantie van de tenant van uw klant zijn via een andere ExpressRoute-circuit in uw virtuele netwerken verbonden. Naar een afzonderlijke load voorwaarden, delen niet de on-premises naar Azure-netwerk ExpressRoute-circuits en de circuits tussen virtuele netwerken van Azure en HANA grote instanties de dezelfde routers.
- De werkbelastingprofiel tussen het niveau van de SAP-toepassing en de HANA grote instantie is van een andere aard, met een groot aantal kleine aanvragen en pieken zoals (resultatensets) van SAP HANA in de toepassingslaag gegevensoverdracht.
- De architectuur van SAP-toepassing is gevoeliger voor netwerklatentie dan typische scenario's waarbij gegevens worden uitgewisseld tussen on-premises en Azure.
- De Azure ExpressRoute-gateway heeft ten minste twee ExpressRoute-verbindingen. Een circuit dat is verbonden van on-premises en één die is verbonden van HANA grote instanties. Dit laat alleen ruimte voor een andere twee extra circuits van verschillende msee's verbinding maken met op ExpressRoute-Gateway. Deze beperking is afhankelijk van het gebruik van ExpressRoute snelle pad. De gekoppelde circuits delen de maximale bandbreedte voor binnenkomende gegevens van de ExpressRoute-gateway.

De ervaren netwerklatentie tussen virtuele machines en HANA grote instantie eenheden kunnen hoger zijn dan een normaal traject latentie van de VM-VM-netwerk. Afhankelijk van de Azure-regio, de meetwaarden kunnen groter zijn dan de 0,7 ms traject latentie geclassificeerd als een waarde onder het gemiddelde in [SAP Opmerking #1100926 - Veelgestelde vragen over: Prestaties van het netwerk](https://launchpad.support.sap.com/#/notes/1100926/E). Afhankelijk van Azure-regio en hulpprogramma voor het meten van de traject netwerklatentie tussen een virtuele machine van Azure en HANA grote instantie eenheid, de gemeten latentie kan worden tot en rond 2 milliseconden. Niettemin implementeren klanten voor productie op basis van SAP HANA SAP-toepassingen is op SAP HANA grote instantie. Zorg ervoor dat u uw bedrijfsprocessen grondig testen in Azure HANA grote instantie. Een nieuwe functionaliteit, met de naam ExpressRoute snel Path, kunnen aanzienlijk reduceren van de netwerklatentie tussen HANA grote instanties en virtuele machines in Azure het niveau van toepassing is (Zie hieronder). 

SKU is essentieel voor deterministische netwerklatentie tussen virtuele machines en HANA grote instantie, de keuze van de ExpressRoute-gateway. In tegenstelling tot de patronen in het netwerkverkeer tussen on-premises en virtuele machines, kunt het patroon voor verkeer tussen virtuele machines en HANA grote instantie ontwikkelen met kleine maar hoge pieken van aanvragen en gegevens volumes moet worden verzonden. Voor het afhandelen van dergelijke bursts goed, raden we het gebruik van de gateway-SKU UltraPerformance. Voor het Type II-klasse van HANA grote instantie SKU's is het gebruik van de gateway-SKU UltraPerformance als een gateway ExpressRotue verplicht.

> [!IMPORTANT] 
> Gezien de algehele netwerkverkeer tussen de SAP-toepassing en de database-lagen, worden alleen de HighPerformance of UltraPerformance gateway-SKU's voor virtuele netwerken ondersteund voor het verbinden met SAP HANA op Azure (grote instanties). Voor HANA grote instantie SKU's Type II, wordt alleen de gateway-SKU UltraPerformance ondersteund als een ExpressRoute-gateway. Uitzonderingen zijn van toepassing bij het gebruik van ExpressRoute snel pad (Zie hieronder)

### <a name="expressroute-fast-path"></a>ExpressRoute snel-pad
Als u wilt de latentie verlagen, ExpressRoute snel pad hebt ingevoerd en de uitgebracht in mei 2019 voor de specifieke verbinding van HANA grote instanties met virtuele netwerken van Azure die als host fungeren voor de SAP toepassings-VM's. Het belangrijkste verschil op de oplossing voor bekendgemaakt die tot nu toe is dat de gegevensstromen tussen virtuele machines en HANA grote instanties, niet worden gerouteerd via de ExpressRoute-gateway niet meer. De virtuele machines die in de subnetten van het Azure-netwerk zijn toegewezen in plaats daarvan communiceren rechtstreeks met de speciale enterprise edge router. 

> [!IMPORTANT] 
> Het snelle pad van ExpressRoute-functionaliteit is vereist dat de subnetten met de SAP toepassings-VM's zich in hetzelfde Azure virtual network die is verbonden met de HANA grote instanties. Virtuele machines die zich in Azure virtuele netwerken die zijn gekoppeld met het Azure-netwerk rechtstreeks verbonden met de eenheden HANA grote instantie profiteren niet van ExpressRoute snelle pad. Als gevolg hiervan typische hub en spoke virtueel netwerk mogelijk te maken, waarbij de ExpressRoute-circuits zijn verbinding te maken op basis van een virtuele hub-netwerk en virtuele netwerken met de SAP-toepassingslaag (knooppunten) ophalen gekoppeld, de optimalisatie van ExpressRoute-Fast Pad werkt niet. Door een groepscertificaat ondersteunt ExpressRoute snel pad niet door de gebruiker gedefinieerde routeringsregels (UDR) vandaag nog. Zie voor meer informatie, [virtuele ExpressRoute-gateway en FastPath netwerk](https://docs.microsoft.com/azure/expressroute/expressroute-about-virtual-network-gateways). 


Voor meer informatie over het configureren van ExpressRoute snel pad, leest u het document [een virtueel netwerk verbinden met HANA grote instanties](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-connect-vnet-express-route).    

> [!NOTE]
> Een UltraPerformance ExpressRoute-gateway is vereist om ExpressRoute snel pad werken


## <a name="single-sap-system"></a>Één SAP-systeem

De on-premises infrastructuur die eerder is getoond is verbonden via ExpressRoute in Azure. Het ExpressRoute-circuit verbinding maakt in een Microsoft enterprise edge router (MSEE). Zie voor meer informatie, [technisch overzicht van ExpressRoute](../../../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Nadat de route is gemaakt, maakt deze verbinding in de Azure-backbone.

> [!NOTE] 
> Als u wilt uitvoeren van SAP-landschappen in Azure, verbinding maken met de die het dichtst bij de Azure-regio in de SAP-landschap enterprise edge router. HANA grote instantie stempels zijn verbonden via een speciale enterprise edge router-apparaten voor minimale netwerklatentie tussen virtuele machines in Azure IaaS- en HANA grote instantie stempels.

De ExpressRoute-gateway voor de virtuele machines die als exemplaren van SAP-toepassing host is verbonden met een ExpressRoute-circuit dat is verbonden on-premises. Hetzelfde virtuele netwerk is verbonden met een speciaal voor de verbinding te maken met een grote instantie stempels afzonderlijke enterprise edge router. Met behulp van ExpressRoute snel pad, de gegevensstroom van HANA grote instanties voor de SAP-toepassingslaag virtuele machines zijn niet doorgestuurd via de ExpressRoute-gateway niet meer en met die verkleint het netwerk round trip latentie.

Dit systeem is een eenvoudig voorbeeld van een enkele SAP-systeem. Het niveau van de SAP-toepassing wordt gehost in Azure. De SAP HANA-database wordt uitgevoerd op SAP HANA op Azure (grote instanties). De veronderstelling is dat een knelpunt niet staan voor de bandbreedte van het ExpressRoute-gateway van 2 Gbps of 10 Gbps-doorvoer.

## <a name="multiple-sap-systems-or-large-sap-systems"></a>Meerdere SAP-systemen of grote SAP-systemen

Als er meerdere SAP-systemen of grote SAP-systemen worden geïmplementeerd om te verbinden met SAP HANA op Azure (grote instanties), kan een knelpunt worden in de doorvoer van de ExpressRoute-gateway. Of u wilt isoleren van productie- en niet-productiesystemen in verschillende virtuele netwerken. In dat geval de toepassingslagen te splitsen in meerdere virtuele netwerken. U kunt ook een speciale virtuele netwerk dat verbinding met HANA grote instantie voor het geval, zoals maakt maken:

- Het uitvoeren van back-ups rechtstreeks vanuit de HANA-exemplaren in HANA grote instantie aan een virtuele machine in Azure die als host fungeert voor NFS-shares.
- Grote back-ups of andere bestanden kopiëren van HANA grote instantie eenheden schijfruimte beheerd in Azure.

Gebruik een afzonderlijke virtual network naar host-VM's die opslag voor massaopslag overdracht van gegevens tussen HANA grote instanties en Azure te beheren. In deze rangschikking vermijdt u de gevolgen van grote bestanden of gegevensoverdracht van HANA grote instantie naar Azure via de ExpressRoute-gateway die de VM's waarop de SAP-toepassingslaag fungeert. 

Voor een beter schaalbaar netwerkarchitectuur:

- Maak gebruik van meerdere virtuele netwerken voor een enkele, grotere SAP-toepassingslaag.
- Implementeer een afzonderlijke virtuele netwerk voor elk SAP-systeem dat is geïmplementeerd, vergeleken met het combineren van deze SAP-systemen in afzonderlijke subnetten in hetzelfde virtuele netwerk.

  Een beter schaalbaar netwerkarchitectuur voor SAP HANA op Azure (grote instanties):

![SAP-toepassingslaag via meerdere virtuele netwerken implementeren](./media/hana-overview-architecture/image4-networking-architecture.png)

Afhankelijk van de regels en beperkingen, dat u wilt toepassen tussen de verschillende virtuele netwerken die als host fungeert voor virtuele machines van verschillende SAP-systemen, moet u deze virtuele netwerken koppelen. Zie voor meer informatie over virtueel-netwerkpeering [peering van virtuele netwerken](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview).


## <a name="routing-in-azure"></a>Routering in Azure

Door de standaardimplementatie zijn de drie routering aandachtspunten voor netwerken belangrijk voor SAP HANA op Azure (grote instanties):

* SAP HANA op Azure (grote instanties) kan alleen worden verkregen via Azure VM's en het toegewezen ExpressRoute-verbinding wordt niet rechtstreeks van on-premises. Directe toegang vanaf on-premises naar de eenheden HANA grote instantie, die door Microsoft worden geleverd, kunt u niet mogelijk is onmiddellijk. De transitieve routering beperkingen zijn vanwege de huidige Azure-netwerk-architectuur die worden gebruikt voor SAP HANA grote instantie. Bepaalde beheerclients en alle toepassingen die toegang, zoals SAP oplossing Manager directe moeten on-premises uitgevoerd kunnen geen verbinding maken met de SAP HANA-database. Controleer de sectie 'Directe routering naar HANA grote instanties' voor uitzonderingen.

* Als u geïmplementeerd in twee verschillende Azure-regio's voor herstel na noodgevallen, dezelfde tijdelijke routering beperkingen toegepast in het verleden HANA grote instantie-eenheden hebt. Met andere woorden, zijn de IP-adressen van een eenheid HANA grote instantie in één regio (bijvoorbeeld VS West) niet doorgestuurd naar een eenheid HANA grote instantie is geïmplementeerd in een andere regio (bijvoorbeeld VS Oost). Deze beperking is onafhankelijk van het gebruik van Azure-netwerk peering met andere regio's of het ExpressRoute-circuits die verbinding maken met HANA grote instantie eenheden naar virtuele netwerken cross-verbinding. Zie voor een grafische weergave in de afbeelding in de sectie 'Gebruik HANA grote instantie eenheden in meerdere regio's '. Deze beperking, die is meegeleverd met de geïmplementeerde architectuur, is het direct gebruik van HANA-Systeemreplicatie als herstel na noodgevallen verboden. Zoek de sectie 'Gebruik HANA grote instantie eenheden in meerdere regio's ' voor recente wijzigingen. 

* SAP HANA op Azure (grote instanties)-eenheden hebben een toegewezen IP-adres uit het adresbereik van server IP-adresgroep die u hebt ingediend bij het aanvragen van de implementatie van HANA grote instantie. Zie voor meer informatie, [SAP HANA (grote instanties)-infrastructuur en connectiviteit in Azure](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Dit IP-adres is toegankelijk via de Azure-abonnementen en -circuit dat is verbonden virtuele netwerken van Azure met HANA grote instanties. Het IP-adres toegewezen van die server IP-adresgroepbereik dat rechtstreeks is toegewezen aan de hardware-eenheid. Er *niet* toegewezen via NAT zich voordoet, net als in de eerste implementatie van deze oplossing. 

### <a name="direct-routing-to-hana-large-instances"></a>Directe routering naar HANA grote instanties
Standaard werkt de transitieve routering tussen HANA grote instantie eenheden en on-premises of tussen HANA grote instantie routering die zijn geïmplementeerd in twee verschillende regio's niet. Er zijn verschillende mogelijkheden om in te schakelen die een transitieve routering.

- Een omgekeerde proxy voor het routeren van gegevens naar en uit. Bijvoorbeeld, F5 BIG-IP en NGINX met Traffic Manager, geïmplementeerd in de Azure-netwerk dat verbinding maakt HANA grote instanties en on-premises als een virtuele firewall/verkeer routeren oplossing.
- Met behulp van [regels van IPTables](http://www.linuxhomenetworking.com/wiki/index.php/Quick_HOWTO_%3a_Ch14_%3a_Linux_Firewalls_Using_iptables#.Wkv6tI3rtaQ) in een Linux-VM om in te schakelen routering tussen on-premises locaties en HANA grote instantie eenheden, of tussen HANA grote instantie eenheden in verschillende regio's. De virtuele machine IPTables die moet worden geïmplementeerd in Azure virtual network die verbinding met HANA grote instanties maakt en naar on-premises. De virtuele machine moet worden dienovereenkomstig aangepast, zo, dat de doorvoer van het netwerk van de virtuele machine voldoende voor het verwachte netwerkverkeer is. Voor meer informatie over VM-netwerk bandbreedte, controleert u het artikel [grootten van Linux virtuele machines in Azure](https://docs.microsoft.com/azure/virtual-machines/linux/sizes?toc=%2fazure%2fvirtual-network%2ftoc.json).
- [Firewall van Azure](https://azure.microsoft.com/services/azure-firewall/) andere oplossing voor het inschakelen van verkeer tussen on-premises en HANA grote instantie eenheden zou zijn. 

Al het verkeer van deze oplossingen zou worden gerouteerd via een Azure-netwerk en als zodanig het verkeer kan worden beperkt door de zachte apparaten of door Azure Netwerkbeveiligingsgroepen, dus die bepaalde IP-adressen of IP-adres een bereik van on-premises kan worden geblokkeerd of expliciet toegestaan toegang tot HANA grote instanties. 

> [!NOTE]  
> Let erop dat de implementatie en ondersteuning voor aangepaste oplossingen van derden met betrekking tot netwerkapparaten of IPTables wordt niet geleverd door Microsoft. Ondersteuning moet worden opgegeven door de leverancier van het onderdeel dat wordt gebruikt of de gegevensintegrator worden verstrekt. 

#### <a name="express-route-global-reach"></a>Express Route wereldwijd bereik
Microsoft een nieuwe functionaliteit, geïntroduceerd [ExpressRoute globaal bereik](https://docs.microsoft.com/azure/expressroute/expressroute-global-reach). Wereldwijd bereik kan worden gebruikt voor HANA grote instanties in twee scenario's:

- Directe toegang vanaf on-premises naar uw eenheden HANA grote instantie is geïmplementeerd in verschillende regio's inschakelen
- Rechtstreekse communicatie tussen de eenheden voor HANA grote instantie geïmplementeerd in verschillende regio's inschakelen


##### <a name="direct-access-from-on-premise"></a>Directe toegang vanaf on-premises
In de Azure-regio's waar globaal bereik wordt aangeboden, kunt u aanvragen voor het inschakelen van de functie globale bereiken voor uw ExpressRoute-circuit dat uw on-premises netwerk verbindt met de Azure-netwerk dat is verbonden met uw HANA grote instantie-eenheden. Er zijn enkele kosten gevolgen voor de on-premises-zijde van uw ExpressRoute-circuit. Controleer voor prijzen, de prijzen voor [globale bereiken invoegtoepassing](https://azure.microsoft.com/pricing/details/expressroute/). Er zijn geen extra kosten voor u met betrekking tot het circuit die de HANA grote instantie eenheid/eenheden verbinding met Azure. 

> [!IMPORTANT]  
> In het geval u een globaal bereik voor het inschakelen van directe toegang tussen uw HANA grote instantie eenheden en on-premises activa, de netwerk-gegevens- en -stroom is **niet doorgestuurd via de Azure-netwerken**, maar rechtstreeks tussen de Microsoft exchange-routers Enterprise. Als gevolg hiervan zijn geen NSG of ASG regels, of elk type van de firewall, NVA of proxy die u hebt geïmplementeerd in een Azure-netwerk niet ophalen van inactief. **Als u een globaal bereik ExpressRoute gebruikt voor rechtstreekse toegang van on-premises tot HANA grote instantie eenheden beperkingen en machtigingen voor toegang tot HANA grote instantie eenheden moeten worden gedefinieerd in de firewalls aan de on-premises** 

##### <a name="connecting-hana-large-instances-in-different-azure-regions"></a>HANA grote instanties in verschillende Azure-regio's verbinden
Op dezelfde manier, zoals ExpressRoute globaal bereik kan worden gebruikt voor de on-premises verbinding met HANA grote instantie eenheden, kan deze worden gebruikt om trekken HANA grote instantie tenants die zijn geïmplementeerd in twee verschillende regio's. De isolatie is de ExpressRoute-circuits die uw tenants HANA grote instantie worden gebruikt om te verbinden met Azure in beide regio's. Er zijn geen extra kosten voor het verbinden van twee HANA grote instantie tenants die zijn geïmplementeerd in twee verschillende regio's. 

> [!IMPORTANT]  
> De gegevensstroom en de Controlestroom van het netwerkverkeer tussen de verschillende HANA grote instantie tenants wordt niet doorgestuurd via de azure-netwerken. U niet als gevolg hiervan de functionaliteit van Azure of NVA's gebruiken om af te dwingen van beperkingen voor communicatie tussen de twee tenants met HANA grote instanties. 

Lees voor meer informatie over hoe u ExpressRoute globaal bereik ingeschakeld, het document [een virtueel netwerk verbinden met HANA grote instanties](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-connect-vnet-express-route).


## <a name="internet-connectivity-of-hana-large-instance"></a>Verbinding met Internet van HANA grote instantie
HANA grote instantie heeft *niet* directe verbinding met internet hebben. Als u bijvoorbeeld mogelijk deze beperking kunt gebruiken om het registreren van de installatiekopie van het besturingssysteem rechtstreeks met de leverancier van de OS beperken. Mogelijk moet u werken met uw lokale abonnement beheerprogramma voor SUSE Linux Enterprise Server-server of de Doelabonnementbeheerder van Red Hat Enterprise Linux.

## <a name="data-encryption-between-vms-and-hana-large-instance"></a>Versleuteling van gegevens tussen VM's en HANA grote instantie
Gegevens die worden overgedragen tussen HANA grote instantie en virtuele machines is niet versleuteld. U kunt echter alleen voor de uitwisseling tussen de HANA DBMS- en JDBC/ODBC-toepassingen, versleutelen van verkeer inschakelen. Zie voor meer informatie, [deze documentatie door SAP](http://help-legacy.sap.com/saphelp_hanaplatform/helpdata/en/db/d3d887bb571014bf05ca887f897b99/content.htm?frameset=/en/dd/a2ae94bb571014a48fc3b22f8e919e/frameset.htm&current_toc=/en/de/ec02ebbb57101483bdf3194c301d2e/plain.htm&node_id=20&show_children=false).

## <a name="use-hana-large-instance-units-in-multiple-regions"></a>HANA grote instantie eenheden gebruiken in meerdere regio 's

Om te realiseren disaster recovery set-ups, moet u grote instantie L'shana eenheden hebben in meerdere Azure-regio's. Zelfs bij het gebruik van Azure [wereldwijde Vnet-Peering], werkt de standaard transitieve routering niet tussen tenants HANA grote instantie in twee verschillende regio's. Echter, globaal bereik opent u het communicatiepad tussen de HANA grote instantie eenheden die u hebt ingericht in twee verschillende regio's. Dit gebruiksscenario van ExpressRoute globaal bereik kunt:

 - HANA-Systeemreplicatie zonder extra proxy's of firewalls
 - Kopiëren back-ups tussen HANA grote instantie eenheden in twee verschillende regio's om uit te voeren system kopieën of systeem wordt vernieuwd


![Virtueel netwerk is verbonden met Azure Large Instance stempels in verschillende Azure-regio 's](./media/hana-overview-architecture/image8-multiple-regions.png)

De afbeelding toont hoe de verschillende virtuele netwerken in beide regio's zijn verbonden met twee verschillende ExpressRoute-circuits verbinding maken met SAP HANA op Azure (grote instanties) worden gebruikt in zowel Azure-regio's (grijze lijnen). Reden voor deze twee cross-verbindingen is om te beschermen tegen een storing optreedt van de msee's aan beide zijden. De communicatiestroom tussen de twee virtuele netwerken in twee Azure-regio's moet worden verwerkt via de [wereldwijde peering](https://blogs.msdn.microsoft.com/azureedu/2018/04/24/how-to-setup-global-vnet-peering-in-azure/) van de twee virtuele netwerken in de twee verschillende regio's (blauw stippellijn). De thick rode lijn beschrijving van de verbinding ExpressRoute globaal bereik, waardoor de eenheden HANA grote instantie van uw tenants in twee verschillende regio's om te communiceren met elkaar. 

> [!IMPORTANT] 
> Als u meerdere ExpressRoute-circuits hebt gebruikt, mogen AS-padtoevoeging en lokale voorkeur BGP-instellingen worden gebruikt om ervoor te zorgen goede routering van verkeer.

**Volgende stappen**
- Raadpleeg [opslagarchitectuur van SAP HANA (grote instanties)](hana-storage-architecture.md)