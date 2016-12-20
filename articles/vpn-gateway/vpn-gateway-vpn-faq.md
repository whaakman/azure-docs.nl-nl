---
title: Veelgestelde vragen over VPN-gateways van virtuele netwerken | Microsoft Docs
description: Veelgestelde vragen over VPN-gateways. Veelgestelde vragen over cross-premises verbinding, verbindingen voor hybride configuraties en VPN-gateways voor Microsoft Azure Virtual Network
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
ms.assetid: 6ce36765-250e-444b-bfc7-5f9ec7ce0742
ms.service: vpn-gateway
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/10/2016
ms.author: yushwang
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: e7d0fa43001268fc4747bbf40d3dc209aa037a67


---
# <a name="vpn-gateway-faq"></a>Veelgestelde vragen VPN-gateways
## <a name="connecting-to-virtual-networks"></a>Verbinding maken met Virtual Network
### <a name="can-i-connect-virtual-networks-in-different-azure-regions"></a>Kan ik virtuele netwerken in verschillende Azure-regio's verbinden?
Ja. Er is zelfs helemaal geen regiobeperking. Een virtueel netwerk kan verbinding maken met een ander virtueel netwerk in dezelfde regio of in een andere Azure-regio. 

### <a name="can-i-connect-virtual-networks-in-different-subscriptions"></a>Kan ik virtuele netwerken uit verschillende abonnementen verbinden?
Ja.

### <a name="can-i-connect-to-multiple-sites-from-a-single-virtual-network"></a>Kan ik vanuit één virtueel netwerk verbinding maken met meerdere sites?
U kunt verbinding maken met meerdere sites met behulp van Windows PowerShell en de REST-API's van Azure. Raadpleeg de sectie Veelgestelde vragen bij [Multi-site- en VNet-naar-VNet-connectiviteit](#multi-site-and-vnet-to-vnet-connectivity).

## <a name="what-are-my-cross-premises-connection-options"></a>Wat zijn de opties voor cross-premises-verbinding?
De volgende cross-premises verbindingen worden ondersteund:

* [Site-naar-site](vpn-gateway-site-to-site-create.md): VPN-verbinding via IPsec (IKE v1 en IKE v2). Voor dit type verbinding is een VPN-apparaat of RRAS vereist.
* [Punt-naar-site](vpn-gateway-point-to-site-create.md): VPN-verbinding via SSTP (Secure Socket Tunneling Protocol). Voor deze verbinding is geen VPN-apparaat vereist.
* [VNet-naar-VNet](virtual-networks-configure-vnet-to-vnet-connection.md): dit type verbinding is hetzelfde als de site-naar-site-configuratie. VNet-naar-VNet is een VPN-verbinding via IPsec (IKE v1 en IKE v2). Hiervoor is geen VPN-apparaat vereist.
* [Multi-site](vpn-gateway-multi-site.md): dit is een variant op een site-naar-site-configuratie waarmee u meerdere on-premises sites kunt verbinden met een virtueel netwerk.
* [ExpressRoute](../expressroute/expressroute-introduction.md): ExpressRoute is een directe verbinding van uw WAN met Azure. Deze loopt niet via het openbare internet. Voor meer informatie raadpleegt u het [ExpressRoute Technical Overview](../expressroute/expressroute-introduction.md) (Technisch overzicht van ExpressRoute) en de [ExpressRoute FAQ](../expressroute/expressroute-faqs.md) (Veelgestelde vragen over ExpressRoute).

Voor meer informatie over verbindingen raadpleegt u [Informatie over VPN Gateway](vpn-gateway-about-vpngateways.md).

### <a name="what-is-the-difference-between-a-site-to-site-connection-and-point-to-site"></a>Wat is het verschil tussen een site-naar-site-verbinding en een punt-naar-site?
Met **site-naar-site**-verbindingen kunt u uw computers on-premises verbinden met elke virtuele machine of elke rolinstantie in uw virtuele netwerk, afhankelijk van hoe u routering wilt configureren. Het is een geweldige optie voor een cross-premises-verbinding die altijd beschikbaar is. Dit type verbinding is afhankelijk van een IPsec-VPN-apparaat (hardware of software) dat op de rand van uw netwerk moet worden geïmplementeerd. Als u dit type verbinding wilt maken, hebt u de vereiste VPN-hardware en een extern gericht IPv4-adres nodig.

Met **punt-naar-site**-verbindingen kunt u vanaf één computer waar dan ook verbinding maken met alles binnen het virtuele netwerk. Hiervoor wordt de met Windows meegeleverde VPN-client gebruikt. Als onderdeel van de punt-naar-site-configuratie installeert u een certificaat en een VPN-clientconfiguratiepakket. Dit pakket bevat de instellingen waarmee de computer verbinding kan maken met elke virtuele machine of rolinstantie in het virtuele netwerk. Het is ideaal wanneer u verbinding wilt maken met een virtueel netwerk maar u zich niet on-premises bevindt. Het is ook een goede optie wanneer u geen toegang hebt tot VPN-hardware of een extern gericht IPv4-adres, twee zaken die vereist zijn voor een site-naar-site-verbinding.

U kunt uw virtuele netwerk configureren om tegelijkertijd gebruik te maken van site-naar-site en punt-naar-site, mits u de site-naar-site-verbinding maakt met een op route gebaseerd VPN-type voor uw gateway. Op route gebaseerde VPN-typen worden in het klassieke implementatiemodel dynamische gateways genoemd.

### <a name="what-is-expressroute"></a>Wat is ExpressRoute?
Met ExpressRoute kunt u particuliere verbindingen maken tussen Microsoft-datacenters en infrastructuur on-premises of in een co-locatie-omgeving. Met ExpressRoute kunt u verbindingen instellen met Microsoft-cloudservices, zoals Microsoft Azure en Office 365 op een co-locatiefaciliteit van een ExpressRoute-partner, of u kunt vanaf uw eigen WAN-netwerk (zoals een MPLS VPN), geleverd door een netwerkserviceprovider, rechtstreeks verbinding maken met Azure.

ExpressRoute-verbindingen zijn beter beveiligd, betrouwbaarder en bieden hogere bandbreedte en lagere latenties dan gewone verbindingen via internet. In sommige gevallen levert het gebruik van ExpressRoute-verbindingen voor het overbrengen van gegevens tussen uw on-premises netwerk en Azure ook aanzienlijke kostenvoordelen op. Als u vanaf uw on-premises netwerk al een cross-premises-verbinding hebt gemaakt met Azure, kunt u migreren naar een ExpressRoute-verbinding terwijl het virtuele netwerk intact blijft.

Zie de [Veelgestelde vragen over ExpressRoute](../expressroute/expressroute-faqs.md) voor meer informatie.

## <a name="site-to-site-connections-and-vpn-devices"></a>Site-naar-site-verbindingen en VPN-apparaten
### <a name="what-should-i-consider-when-selecting-a-vpn-device"></a>Waaraan moet ik denken bij het selecteren van een VPN-apparaat?
We hebben samen met apparaatleveranciers een reeks standaard site-naar-site-VPN-apparaten gevalideerd. [Hier](vpn-gateway-about-vpn-devices.md) vindt u een lijst met bekende compatibele VPN-apparaten, hun bijbehorende configuratie-instructies of voorbeelden en apparaatspecificaties. Alle apparaten in de vermelde apparaatfamilies die als compatibel worden weergegeven, zouden met Virtual Network moeten werken. Als hulp bij de configuratie van uw VPN-apparaat kunt u het voorbeeld van de apparaatconfiguratie raadplegen of de koppeling bij de betreffende apparaatfamilie.

### <a name="what-do-i-do-if-i-have-a-vpn-device-that-isnt-in-the-known-compatible-device-list"></a>Wat moet ik doen als ik een VPN-apparaat heb dat niet voorkomt in de lijst met bekende compatibele apparaten?
Als uw apparaat niet wordt vermeld als een bekend compatibel VPN-apparaat en u dit toch wilt gebruiken voor uw VPN-verbinding, moet u controleren of het voldoet aan de [hier](vpn-gateway-about-vpn-devices.md) vermelde ondersteunde opties en parameters voor IPsec/IKE-configuratie. Apparaten die voldoen aan de minimumvereisten zouden met VPN-gateways moeten werken. Neem contact op met de fabrikant van uw apparaat voor aanvullende ondersteuning en configuratie-instructies.

### <a name="why-does-my-policy-based-vpn-tunnel-go-down-when-traffic-is-idle"></a>Waarom wordt mijn op beleid gebaseerde VPN-tunnel inactief als er geen verkeer is?
Dit is normaal voor op beleid gebaseerde VPN-gateways (ook wel bekend als statische routering genoemd). Wanneer er langer dan vijf minuten geen verkeer is via de tunnel, wordt de tunnel verwijderd. Zodra er verkeer is in een van de richtingen, wordt de tunnel onmiddellijk opnieuw ingesteld.

### <a name="can-i-use-software-vpns-to-connect-to-azure"></a>Kan ik software-VPN's gebruiken om verbinding te maken met Azure?
Windows Server 2012 RRAS-servers (Routering en RAS) worden ondersteund voor site-naar-site-cross-premises-configuratie.

Andere VPN-softwareoplossingen zouden in principe met onze gateway moeten werken zolang ze voldoen aan de standaard-IPSec-implementaties. Neem contact op met de leverancier van de software voor configuratie- en ondersteuningsinstructies.

## <a name="point-to-site-connections"></a>Punt-naar-site-verbindingen
### <a name="what-operating-systems-can-i-use-with-point-to-site"></a>Welke besturingssystemen kan ik met punt-naar-site gebruiken?
De volgende besturingssystemen worden ondersteund:

* Windows 7 (32-bits en 64-bits)
* Windows Server 2008 R2 (alleen 64-bits)
* Windows 8 (32-bits en 64-bits)
* Windows 8.1 (32-bits en 64-bits)
* Windows Server 2012 (alleen 64-bits)
* Windows Server 2012 R2 (alleen 64-bits)
* Windows 10

### <a name="can-i-use-any-software-vpn-client-for-point-to-site-that-supports-sstp"></a>Kan ik voor punt-naar-site elke VPN-softwareclient gebruiken die SSTP ondersteunt?
Nee. Ondersteuning is beperkt tot de hierboven vermelde versies van Windows-besturingssystemen.

### <a name="how-many-vpn-client-endpoints-can-i-have-in-my-point-to-site-configuration"></a>Hoeveel VPN-clienteindpunten kan mijn punt-naar-site-configuratie hebben?
Er kunnen maximaal 128 VPN-clients tegelijk met een virtueel netwerk worden verbonden.

### <a name="can-i-use-my-own-internal-pki-root-ca-for-point-to-site-connectivity"></a>Kan ik mijn eigen interne PKI basis-CA voor een punt-naar-site-verbinding gebruiken?
Ja. Voorheen konen alleen zelfondertekende basiscertificaten worden gebruikt. U kunt nog steeds 20 basiscertificaten uploaden.

### <a name="can-i-traverse-proxies-and-firewalls-using-point-to-site-capability"></a>Kan ik met punt-naar-site-functionaliteit proxy's en firewalls passeren?
Ja. SSTP (Secure Socket Tunneling Protocol) wordt gebruikt om firewalls te passeren. Deze tunnel wordt weergegeven als een HTTPs-verbinding.

### <a name="if-i-restart-a-client-computer-configured-for-point-to-site-will-the-vpn-automatically-reconnect"></a>Als ik een clientcomputer die is geconfigureerd voor punt-naar-site opnieuw start, wordt de VPN-verbinding dan automatisch opnieuw tot stand gebracht?
Standaard wordt de VPN-verbinding niet automatisch op de clientcomputer hersteld.

### <a name="does-point-to-site-support-auto-reconnect-and-ddns-on-the-vpn-clients"></a>Biedt punt-naar-site ondersteuning voor automatisch opnieuw verbinding maken en DDNS op de VPN-clients?
Automatisch opnieuw verbinding maken en DDNS worden momenteel niet ondersteund in VPN’s met punt-naar-site-verbinding.

### <a name="can-i-have-site-to-site-and-point-to-site-configurations-coexist-for-the-same-virtual-network"></a>Kunnen site-naar-site- en punt-naar-site-configuraties naast elkaar worden gebruikt in hetzelfde virtuele netwerk?
Ja. Beide oplossingen werken als de gateway een op RouteBased-type VPN heeft. Voor het klassieke implementatiemodel hebt u een dynamische gateway nodig. Punt-naar-site wordt niet ondersteund voor VPN-gateways met statische routering of gateways met een op beleid gebaseerd VPN-type.

### <a name="can-i-configure-a-point-to-site-client-to-connect-to-multiple-virtual-networks-at-the-same-time"></a>Kan ik een punt-naar-site-client configureren om verbinding te maken met meerdere virtuele netwerken tegelijk?
Ja, dat is mogelijk. De virtuele netwerken kunnen echter geen overlappende IP-voorvoegsels hebben en de punt-naar-site-adresruimten tussen de virtuele netwerken mogen elkaar niet overlappen.

### <a name="how-much-throughput-can-i-expect-through-site-to-site-or-point-to-site-connections"></a>Hoeveel doorvoer kan ik verwachten via site-naar-site- of punt-naar-site-verbindingen?
Het is moeilijk om de exacte doorvoer van de VPN-tunnels te onderhouden. IPSec en SSTP zijn cryptografisch zware VPN-protocollen. Doorvoer wordt ook beperkt door de latentie en bandbreedte tussen uw locatie en het internet.

## <a name="gateways"></a>Gateways
### <a name="what-is-a-policy-based-static-routing-gateway"></a>Wat is een op beleid gebaseerde gateway (statische routering)?
Op beleid gebaseerde gateways implementeren op beleid gebaseerde VPN's. Op beleid gebaseerde VPN-verbindingen versleutelen pakketten en versturen deze op basis van de combinaties van adresvoorvoegsels tussen uw on-premises netwerk en het Azure-VNET. Het beleid (of de verkeersselector) wordt gewoonlijk gedefinieerd als een toegangslijst in de VPN-configuratie.

### <a name="what-is-a-route-based-dynamic-routing-gateway"></a>Wat is een op route gebaseerde gateway (dynamische routering)?
Op route gebaseerde gateways implementeren op route gebaseerde VPN's. Op route gebaseerde VPN's gebruiken 'routes' in de IP-doorstuurtabel of routeringstabel om pakketten naar de bijbehorende tunnelinterfaces te sturen. De tunnelinterfaces versleutelen of ontsleutelen de pakketten vervolgens naar en vanuit de tunnels. Het beleid of de verkeersselector voor op route gebaseerde VPN's is geconfigureerd als alles-naar-alles (of jokertekens).

### <a name="can-i-get-my-vpn-gateway-ip-address-before-i-create-it"></a>Kan ik het IP-adres van de VPN-gateway verkrijgen voordat ik de gateway maak?
Nee. U moet een gateway maken voordat u het IP-adres kunt verkrijgen. Het IP-adres verandert als u een VPN-gateway verwijdert en opnieuw maakt.

### <a name="how-does-my-vpn-tunnel-get-authenticated"></a>Hoe wordt mijn VPN-tunnel geverifieerd?
Azure VPN maakt gebruik van PSK-verificatie (verificatie op basis van een vooraf gedeelde sleutel). Er wordt een PSK (vooraf gedeelde sleutel) gegenereerd wanneer de VPN-tunnel wordt gemaakt. Met de PowerShell-cmdlet of REST-API Set Pre-Shared Key kunt u de automatisch gegenereerde PSK wijzigen in een eigen sleutel.

### <a name="can-i-use-the-set-pre-shared-key-api-to-configure-my-policy-based-static-routing-gateway-vpn"></a>Kan ik de API Set Pre-Shared Key gebruiken om een op beleid gebaseerde VPN-gateway (statische routering) te configureren?
Ja, u kunt de API en PowerShell-cmdlet Set Pre-Shared Key gebruiken om zowel op beleid gebaseerde (statische) VPN's als op route gebaseerde VPN's (dynamische routering) van Azure te configureren.

### <a name="can-i-use-other-authentication-options"></a>Kan ik andere verificatieopties gebruiken?
U kunt alleen PSK-verificatie (vooraf gedeelde sleutels) gebruiken.

### <a name="what-is-the-gateway-subnet-and-why-is-it-needed"></a>Wat is het 'gatewaysubnet' en waarom is het nodig?
Er is een gatewayservice die wordt uitgevoerd om cross-premises-connectiviteit mogelijk te maken.

Als u een VPN-gateway wilt configureren, moet u eerst een gatewaysubnet voor het VNet maken. Voor een goede werking moeten alle gatewaysubnetten de naam GatewaySubnet krijgen. Geef het gatewaysubnet geen andere naam. Implementeer ook geen VM's of iets anders in het gatewaysubnet.

De minimale grootte van het gatewaysubnet is volledig afhankelijk van de configuratie die u wilt maken. Hoewel u voor bepaalde configuraties een gatewaysubnet kunt maken dat zo klein is als /29, raden wij aan om een gatewaysubnet van /28 of groter (/28, /27, /26 enzovoort) te maken.

### <a name="can-i-deploy-virtual-machines-or-role-instances-to-my-gateway-subnet"></a>Kan ik Virtual Machines of rolinstanties implementeren in het gatewaysubnet?
Nee.

### <a name="how-do-i-specify-which-traffic-goes-through-the-vpn-gateway"></a>Hoe geef ik op welk verkeer via de VPN-gateway loopt?
Als u de klassieke Azure-beheerportal gebruikt, voegt u elk gewenst bereik dat u via de gateway voor het virtuele netwerk wilt verzenden toe op de pagina Netwerken onder Lokale netwerken.

### <a name="can-i-configure-forced-tunneling"></a>Kan ik geforceerde tunneling configureren?
Ja. Zie [Configure forced tunneling](vpn-gateway-about-forced-tunneling.md) (Geforceerde tunneling configureren).

### <a name="can-i-set-up-my-own-vpn-server-in-azure-and-use-it-to-connect-to-my-on-premises-network"></a>Kan ik mijn eigen VPN-server in Azure instellen en deze gebruiken om verbinding te maken met mijn on-premises netwerk?
Ja, kunt u uw eigen VPN-gateways of -servers in Azure implementeren vanuit de Azure Marketplace of door uw eigen VPN-routers te implementeren. U moet in het virtuele netwerk zelfgedefinieerde routes configureren om ervoor te zorgen dat verkeer juist wordt gerouteerd tussen uw on-premises netwerken en de subnetten van het virtuele netwerk.

### <a name="why-are-certain-ports-opened-on-my-vpn-gateway"></a>Waarom zijn bepaalde poorten geopend op mijn VPN-gateway?
Deze zijn nodig voor communicatie met de Azure-infrastructuur. Ze zijn beveiligd (vergrendeld) met Azure-certificaten. Zonder de juiste certificaten kunnen externe entiteiten, waaronder de klanten van deze gateways, niets uitvoeren op de eindpunten.

Een VPN-gateway is in feite een multihomed apparaat met één NIC die toegang heeft tot het privénetwerk van de klant en één NIC die is gericht op het openbare netwerk. Infrastructuurentiteiten van Azure hebben om wettelijke redenen geen toegang tot particuliere netwerken van klanten. Daarom moeten ze voor infrastructuurcommunicatie gebruikmaken van openbare eindpunten. De openbare eindpunten worden periodiek gescand door de beveiligingscontrole van Azure.

### <a name="more-information-about-gateway-types-requirements-and-throughput"></a>Meer informatie over gatewaytypen, vereisten en doorvoer
Zie [Informatie over VPN Gateway](vpn-gateway-about-vpn-gateway-settings.md) voor meer informatie.

## <a name="multi-site-and-vnet-to-vnet-connectivity"></a>Multi-site- en VNet-naar-VNet-connectiviteit
### <a name="which-type-of-gateways-can-support-multi-site-and-vnet-to-vnet-connectivity"></a>Welk type gateways ondersteunt multi-site- en VNet-naar-VNet-connectiviteit?
Alleen op route gebaseerde VPN's (dynamische routering).

### <a name="can-i-connect-a-vnet-with-a-routebased-vpn-type-to-another-vnet-with-a-policybased-vpn-type"></a>Kan ik een VNet met een op route gebaseerd VPN-type verbinden met een op beleid gebaseerd VPN-type?
Nee, beide virtuele netwerken moeten gebruikmaken van op route gebaseerde VPN's (dynamische routering).

### <a name="is-the-vnet-to-vnet-traffic-secure"></a>Is het VNet-naar-VNet-verkeer beveiligd?
Ja, het is beveiligd met IPsec/IKE-versleuteling.

### <a name="does-vnet-to-vnet-traffic-travel-over-the-azure-backbone"></a>Verplaatst VNet-naar-VNet-verkeer zich via de Azure-backbone?
Ja.

### <a name="how-many-on-premises-sites-and-virtual-networks-can-one-virtual-network-connect-to"></a>Met hoeveel on-premises sites en virtuele netwerken kan één virtueel netwerk verbinding maken?
Met maximaal 10 gecombineerde sites/netwerken voor de gateways met Basic en Standard dynamische routering, en met max. 30 voor VPN-gateways met hoge prestaties.

### <a name="can-i-use-point-to-site-vpns-with-my-virtual-network-with-multiple-vpn-tunnels"></a>Kan ik punt-naar-site-VPN-verbindingen met het virtuele netwerk gebruiken met meerdere VPN-tunnels?
Ja, P2S-VPN-verbindingen (punt-naar-site) kunnen worden gebruikt met de VPN-gateways die verbinding maken met meerdere on-premises sites en andere virtuele netwerken.

### <a name="can-i-configure-multiple-tunnels-between-my-virtual-network-and-my-on-premises-site-using-multi-site-vpn"></a>Kan ik meerdere tunnels tussen mijn virtuele netwerk en mijn on-premises site configureren met multi-site-VPN?
Nee, redundante tunnels tussen een virtueel netwerk van Azure en een on-premises site wordt niet ondersteund.

### <a name="can-there-be-overlapping-address-spaces-among-the-connected-virtual-networks-and-on-premises-local-sites"></a>Mogen er overlappende adresruimten zijn tussen de verbonden virtuele netwerken en on-premises lokale sites?
Nee. Als er overlappende adresruimten zijn, zal het uploaden van het netwerkconfiguratiebestand of het maken van een virtueel netwerk niet lukken.

### <a name="do-i-get-more-bandwidth-with-more-site-to-site-vpns-than-for-a-single-virtual-network"></a>Krijg ik meer bandbreedte met meerdere site-naar-site-VPN-verbindingen dan met één virtueel netwerk?
Nee, alle VPN-tunnels, inclusief punt-naar-site-VPN-verbindingen, delen dezelfde Azure VPN-gateway en beschikbare bandbreedte.

### <a name="can-i-use-azure-vpn-gateway-to-transit-traffic-between-my-on-premises-sites-or-to-another-virtual-network"></a>Kan ik Azure VPN-gateway gebruiken om verkeer tussen mijn on-premises sites of naar een ander virtueel netwerk over te brengen?
**Klassiek implementatiemodel**<br>
Transitverkeer via Azure VPN-gateway is mogelijk met het klassieke implementatiemodel, maar is afhankelijk van statisch gedefinieerde adresruimten in het netwerkconfiguratiebestand. BGP wordt nog niet ondersteund met Azure Virtual Networks en VPN-gateways via het klassieke implementatiemodel. Zonder BGP is het handmatig definiëren van adresruimten voor doorvoer zeer foutgevoelig en het wordt daarom niet aanbevolen.<br>
**Resource Manager-implementatiemodel**<br>
Als u het Resource Manager-implementatiemodel gebruik, raadpleegt u de sectie [BGP](#bgp) voor meer informatie.

### <a name="does-azure-generate-the-same-ipsecike-pre-shared-key-for-all-my-vpn-connections-for-the-same-virtual-network"></a>Genereert Azure dezelfde vooraf gedeelde IPsec/IKE-sleutel voor al mijn VPN-verbindingen voor hetzelfde virtuele netwerk?
Nee, Azure genereert standaard verschillende vooraf gedeelde sleutels voor verschillende VPN-verbindingen. U kunt echter de REST-API of PowerShell-cmdlet Set VPN Gateway gebruiken om de gewenste sleutelwaarde in te stellen. De sleutel moet een alfanumerieke tekenreeks van 1 tot 128 tekens zijn.

### <a name="does-azure-charge-for-traffic-between-virtual-networks"></a>Worden er door Azure kosten in rekening gebracht voor verkeer tussen virtuele netwerken?
Bij verkeer tussen verschillende virtuele netwerken van Azure worden alleen kosten in rekening gebracht voor verkeer van de ene Azure-regio naar een andere. Een overzicht van de kosten vindt u op de Azure-pagina [Prijzen voor VPN-gateway](https://azure.microsoft.com/pricing/details/vpn-gateway/).

### <a name="can-i-connect-a-virtual-network-with-ipsec-vpns-to-my-expressroute-circuit"></a>Kan ik een virtueel netwerk met IPsec-VPN's verbinden met mijn ExpressRoute-circuit?
Ja, dit wordt ondersteund. Voor meer informatie raadpleegt u [Expressroute en site-naar-site-VPN-verbindingen die naast elkaar kunnen worden gebruikt configureren](../expressroute/expressroute-howto-coexist-classic.md)

## <a name="a-namebgpabgp"></a><a name="bgp"></a>BGP
[!INCLUDE [vpn-gateway-bgp-faq-include](../../includes/vpn-gateway-bpg-faq-include.md)]

## <a name="cross-premises-connectivity-and-vms"></a>Cross-premises-connectiviteit en virtuele machines
### <a name="if-my-virtual-machine-is-in-a-virtual-network-and-i-have-a-cross-premises-connection-how-should-i-connect-to-the-vm"></a>Als de virtuele machine zich in een virtueel netwerk bevindt en ik een cross-premises-verbinding heb, hoe moet ik dan verbinding maken met de virtuele machine?
U hebt een aantal opties. Als u RDP hebt ingeschakeld en een eindpunt hebt gemaakt, kunt u het VIP gebruiken om verbinding te maken met uw virtuele machine. In dat geval geeft u het VIP op en de poort waarmee u verbinding wilt maken. U moet de poort op de virtuele machine configureren voor het verkeer. Gewoonlijk gaat u naar de klassieke Azure-portal en slaat u de instellingen voor de RDP-verbinding op uw computer op. De instellingen bevatten de noodzakelijke verbindingsgegevens.

Als u een virtueel netwerk met cross-premises-connectiviteit hebt geconfigureerd, kunt u het DIP of particuliere IP-adres gebruiken om verbinding te maken met uw virtuele machine. U kunt uw virtuele machine ook verbinden met behulp van het interne DIP van een andere virtuele machine die zich in hetzelfde virtuele netwerk bevindt. Het is niet mogelijk om met het DIP een RDP-verbinding met de virtuele machine te maken als u verbinding maakt vanaf een locatie buiten het virtuele netwerk. Als u bijvoorbeeld een virtueel punt-naar-site-netwerk hebt geconfigureerd en u geen verbinding maakt vanaf uw computer, dan kunt u DIP niet gebruiken om verbinding te maken met de virtuele machine.

### <a name="if-my-virtual-machine-is-in-a-virtual-network-with-cross-premises-connectivity-does-all-the-traffic-from-my-vm-go-through-that-connection"></a>Als mijn virtuele machine zich in een virtueel netwerk met cross-premises-connectiviteit bevindt, gaat al het verkeer vanuit mijn VM dan langs die verbinding?
Nee. Alleen het verkeer met een doel-IP dat zich bevindt in de door u opgegeven IP-adresbereiken van het lokale netwerk van het virtuele netwerk, loopt via de gateway van het virtuele netwerk. Verkeer met een doel-IP binnen het virtuele netwerk blijft in het virtuele netwerk. Ander verkeer wordt via de load balancer verzonden naar de openbare netwerken, tenzij geforceerde tunneling wordt gebruikt. In dat geval wordt het verzonden via de Azure VPN-gateway. Als u problemen moet oplossen, is het belangrijk dat u beschikt over alle bereiken in het lokale netwerk die u via de gateway wilt verzenden. Zorg dat de adresbereiken van het lokale netwerk niet overlappen met adresbereiken in het virtuele netwerk. Bovendien moet u controleren of de DNS-server die u gebruikt de naam omzet in het juiste IP-adres.

## <a name="virtual-network-faq"></a>Veelgestelde vragen over Virtual Network
Aanvullende informatie over virtuele netwerken vindt u in de [Veelgestelde vragen over Virtual Network](../virtual-network/virtual-networks-faq.md).



<!--HONumber=Dec16_HO1-->


