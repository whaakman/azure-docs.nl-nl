---
title: Peering in virtuele netwerken van Azure | Microsoft Docs
description: Meer informatie over peering in virtuele netwerken in Azure.
services: virtual-network
documentationcenter: na
author: NarayanAnnamalai
manager: jefco
editor: tysonn
ms.assetid: eb0ba07d-5fee-4db0-b1cb-a569b7060d2a
ms.service: virtual-network
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: narayan;anavin
ms.openlocfilehash: f43c95753e2cb190270a25fecd4c490e6fb0ed34
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2018
---
# <a name="virtual-network-peering"></a>Peering op virtueel netwerk

Dankzij Virtual Network-peering kunt u naadloos twee [virtuele netwerken](virtual-networks-overview.md) in Azure met elkaar verbinden. Nadat de virtuele netwerken zijn gekoppeld via peering, worden ze voor verbindingsdoeleinden weergegeven als één netwerk. Het verkeer tussen de virtuele machines in de gekoppelde virtuele netwerken wordt doorgestuurd via de Microsoft-backbone-infrastructuur, vergelijkbaar met de manier waarop verkeer tussen virtuele machines in hetzelfde virtuele netwerk alleen wordt gerouteerd via *privé*-IP-adressen. 

Enkele voordelen van peering van virtuele netwerken zijn:

* Netwerkverkeer tussen gekoppelde virtuele netwerken is privé. Verkeer tussen de virtuele netwerken wordt opgeslagen in het Microsoft-backbone-netwerk. Er zijn geen openbare internetverbinding, gateways of versleuteling vereist in de communicatie tussen de virtuele netwerken.
* Een verbinding met lage latentie en hoge bandbreedte tussen resources in verschillende virtuele netwerken.
* De mogelijkheid voor resources in één virtueel netwerk om te communiceren met resources in een ander virtueel netwerk, zodra de virtuele netwerken zijn gekoppeld.
* De mogelijkheid om gegevens over te brengen tussen Azure-abonnementen, implementatiemodellen en Azure-regio's (preview).
* De mogelijkheid om virtuele netwerken die zijn gemaakt via Azure Resource Manager te koppelen of om één virtueel netwerk dat is gemaakt via Resource Manager te koppelen aan een virtueel netwerk dat is gemaakt via het klassieke implementatiemodel. Zie [Over Azure-implementatiemodellen](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) voor meer informatie over Azure-implementatiemodellen.
* Geen downtime tot bronnen in een virtueel netwerk bij het maken van de peering of nadat de peering is gemaakt.

## <a name="requirements-constraints"></a>Vereisten en beperkingen

* Peering van virtuele netwerken in dezelfde regio's is algemeen beschikbaar. Peering van virtuele netwerken in verschillende regio's is momenteel in preview in VS - west-centraal, Canada Centraal, VS West 2, Zuid-Korea, VK Zuid, VK West, Canada Oost, India zuiden, India midden en India westen. Voordat u peering van virtuele netwerken in verschillende regio's gaat instellen, moet u eerst [uw abonnement registreren](virtual-network-create-peering.md#register) voor de preview. Als u dat niet doet, mislukt de peering van virtuele netwerken in verschillende regio's.
    > [!WARNING]
    > Peerings van virtuele netwerken die in verschillende regio's worden gemaakt, hebben mogelijk niet dezelfde mate van beschikbaarheid en betrouwbaarheid als peerings waarbij een versie met algemene beschikbaarheid wordt gebruikt. Sommige functies van gekoppelde virtuele netwerken zijn mogelijk beperkt of zijn niet beschikbaar in alle Azure-regio's. Controleer de pagina [Azure Virtual Network-updates](https://azure.microsoft.com/updates/?product=virtual-network) voor de meest recente meldingen over de beschikbaarheid en de status van deze functie.

* De gekoppelde virtuele netwerken moeten niet-overlappende IP-adresruimten hebben.
* Adresbereiken kunnen niet worden toegevoegd aan of verwijderd uit de adresruimte van een virtueel netwerk wanneer een virtueel netwerk is gekoppeld aan een ander virtueel netwerk. Als u adresbereiken wilt toevoegen aan de adresruimte van een gekoppeld virtueel netwerk, moet u de peering verwijderen, de adresruimte toevoegen en vervolgens de peering opnieuw toevoegen.
* Peering in virtuele netwerken vindt plaats tussen twee virtuele netwerken. Er is geen afgeleide transitieve relatie tussen peerings. Als virtualNetworkA bijvoorbeeld is gekoppeld aan virtualNetworkB en virtualNetworkB is gekoppeld aan virtualNetworkC, is virtualNetwork A *niet* gekoppeld aan virtualNetworkC.
* U kunt virtuele netwerken koppelen die zich in twee verschillende abonnementen bevinden, mits een bevoegde gebruiker (zie [specifieke machtigingen](create-peering-different-deployment-models-subscriptions.md#permissions)) van beide abonnementen de peering toestaat en de abonnementen zijn gekoppeld aan dezelfde Azure Active Directory-tenant. U kunt een [VPN-gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#V2V) gebruiken om virtuele netwerken te verbinden in abonnementen die zijn gekoppeld aan verschillende Active Directory-tenants.
* Virtuele netwerken kunnen worden gekoppeld als beide zijn gemaakt via het Resource Manager-implementatiemodel of als het ene virtuele netwerk is gemaakt via het Resource Manager-implementatiemodel en het andere via het klassieke implementatiemodel. Virtuele netwerken die zijn gemaakt via het klassieke implementatiemodel, kunnen echter niet aan elkaar worden gekoppeld. U kunt een [VPN-gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#V2V) gebruiken om twee virtuele netwerken te verbinden die zijn gemaakt via het klassieke implementatiemodel.
* Hoewel de communicatie tussen virtuele machines in gekoppelde virtuele netwerken geen extra bandbreedtebeperkingen heeft, geldt er nog steeds een bandbreedtelimiet op basis van de grootte van de virtuele machine. Lees de artikelen over de grootte van virtuele machines voor [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) of [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) voor meer informatie over de maximale netwerkbandbreedte voor verschillende groottes van virtuele machines.

     ![Eenvoudige peering van virtuele netwerken](./media/virtual-networks-peering-overview/figure03.png)

## <a name="connectivity"></a>Connectiviteit

Nadat virtuele netwerken aan elkaar zijn gekoppeld, kunnen resources in beide virtuele netwerken rechtstreeks verbinding maken met resources in het gekoppelde virtuele netwerk.

De netwerklatentie tussen virtuele machines in gekoppelde virtuele netwerken in dezelfde regio is gelijk aan de latentie binnen één virtueel netwerk. De netwerkdoorvoer is gebaseerd op de bandbreedte die is toegestaan voor de virtuele machine, evenredig aan de grootte. Er gelden verder geen extra beperkingen voor bandbreedte binnen de peering.

Het verkeer tussen virtuele machines in gekoppelde virtuele netwerken wordt rechtstreeks doorgestuurd via de Microsoft-backbone-infrastructuur, niet via een gateway of via het openbare internet.

Virtuele machines in een virtueel netwerk hebben toegang tot de interne load balancer in het gekoppelde virtuele netwerk in dezelfde regio. Ondersteuning voor een interne load balancer strekt zich niet uit tot wereldwijd gekoppelde virtuele netwerken (preview). In de algemeen beschikbare versie van wereldwijde peering van virtuele netwerken zal een interne load balancer worden ondersteund.

Netwerkbeveiligingsgroepen kunnen worden toegepast in elk virtueel netwerk om, indien gewenst, de toegang tot andere virtuele netwerken of subnetten te blokkeren.
Bij het configureren van peering in virtuele netwerken kunt u de netwerkbeveiligingsgroepen tussen de virtuele netwerken openen of sluiten. Als u besluit om de volledige connectiviteit tussen gelijkwaardige virtuele netwerken open te zetten (dit is de standaardoptie), kunt u vervolgens netwerkbeveiligingsgroepen op specifieke subnetten of virtuele machines gebruiken voor het blokkeren of weigeren van specifieke toegang. Zie [Overzicht van netwerkbeveiligingsgroepen](virtual-networks-nsg.md) voor meer informatie over netwerkbeveiligingsgroepen.

## <a name="service-chaining"></a>Servicechaining

U kunt door gebruikers gedefinieerde routes configureren die naar virtuele machines in gekoppelde virtuele netwerken of naar virtuele netwerkgateways verwijzen als het *volgende hop*-IP-adres om servicechaining in te schakelen. Met servicechaining kunt u verkeer van een virtueel netwerk naar een virtueel apparaat of een virtuele netwerkgateway in een gekoppeld virtueel netwerk leiden, door middel van door gebruikers gedefinieerde routes.

U kunt ook hub en spoke-omgevingen implementeren waarin het virtuele hub-netwerk infrastructuuronderdelen kan hosten, zoals een virtueel netwerkapparaat of een VPN-gateway. Alle virtuele spoke-netwerken kunnen vervolgens worden gekoppeld aan het virtuele hub-netwerk. Door virtuele netwerkapparaten of VPN-gateways die worden uitgevoerd in het virtuele hub-netwerk kan verkeer stromen. 

Met peering van virtuele netwerken kan de volgende hop op de door de gebruiker gedefinieerde route het IP-adres zijn van een virtuele machine in het gekoppelde virtuele netwerk of VPN-gateway. U kunt echter geen verkeer doorsturen tussen virtuele netwerken met een door de gebruiker gedefinieerde route waarin de ExpressRoute-gateway is opgegeven als het volgende hoptype. Zie [Door de gebruiker gedefinieerde routes](virtual-networks-udr-overview.md#user-defined) voor meer informatie over door de gebruiker gedefinieerde routes. Zie [Hub-en-spoke-netwerktopologie](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#virtual network-peering) voor meer informatie over het maken van een hub-en-spokenetwerktopologie.

## <a name="gateways-and-on-premises-connectivity"></a>Gateways en on-premises connectiviteit

Elk virtueel netwerk, ongeacht of het gekoppeld is aan een ander virtueel netwerk, kan nog steeds een eigen gateway hebben en gebruiken om te verbinden met een on-premises netwerk. U kunt ook [virtueel-netwerk-naar-virtueel-netwerk-verbindingen](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json) configureren door gateways te gebruiken, ook al zijn de virtuele netwerken gekoppeld.

Wanneer beide opties voor interconnectiviteit tussen virtuele netwerken zijn geconfigureerd, loopt het verkeer tussen de virtuele netwerken via de peering-configuratie (dat wil zeggen, via de Azure-backbone).

Wanneer virtuele netwerken zijn gekoppeld in dezelfde regio, kunt u ook de gateway in het gekoppelde virtuele netwerk configureren als een doorvoerpunt naar een on-premises netwerk. Het virtuele netwerk dat gebruikmaakt van een externe gateway kan in dit geval niet een eigen gateway hebben. Een virtueel netwerk kan slechts één gateway hebben. De gateway kan een lokale of een externe gateway zijn (in het gekoppelde virtuele netwerk), zoals wordt weergegeven op de volgende afbeelding:

![doorvoer bij peering van virtuele netwerken](./media/virtual-networks-peering-overview/figure04.png)

Gatewaydoorvoer wordt niet ondersteund in de peeringrelatie tussen virtuele netwerken die zijn gemaakt via verschillende implementatiemodellen of in verschillende regio's. Beide virtuele netwerken in de peeringrelatie moeten zijn gemaakt via Resource Manager en moeten zich in dezelfde regio bevinden om ervoor te zorgen dat de doorvoer via een gateway werkt. Wereldwijd gekoppelde virtuele netwerken ondersteunen momenteel geen gatewaydoorvoer.

Wanneer virtuele netwerken die één Azure ExpressRoute-verbinding delen, worden gekoppeld, gaat het verkeer tussen de twee netwerken via de peering-relatie (dat wil zeggen, via het backbone-netwerk van Azure). U kunt nog steeds in elk virtueel netwerk lokale gateways gebruiken om verbinding te maken met het on-premises circuit. U kunt ook een gedeelde gateway gebruiken en de doorvoer voor on-premises connectiviteit configureren.

## <a name="permissions"></a>Machtigingen

Virtueel netwerk-peering is een bevoegde bewerking. Het is een afzonderlijke functie onder de VirtualNetworks-naamruimte. Een gebruiker kan specifieke rechten krijgen voor het machtigen van peering. Een gebruiker met lees-/schrijftoegang tot het virtuele netwerk krijgt automatisch deze rechten.

Een gebruiker die ofwel een beheerder is of een bevoegde gebruiker van de peering-mogelijkheid kan een peering-bewerking op een ander virtueel netwerk initiëren. Het minimaal vereiste machtigingsniveau is Inzender voor netwerken. Als er een overeenkomende aanvraag voor peering van de andere kant komt en aan andere vereisten wordt voldaan, wordt de peering tot stand gebracht.

Als u bijvoorbeeld de virtuele netwerken genaamd mijnVirtueelNetwerkA en mijnVirtueelNetwerkB koppelt, moet aan het account de volgende minimale rol of machtiging voor elk virtueel netwerk worden toegewezen:

|Virtueel netwerk|Implementatiemodel|Rol|Machtigingen|
|---|---|---|---|
|mijnVirtueelNetwerkA|Resource Manager|[Inzender voor netwerken](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write|
| |Klassiek|[Inzender voor klassieke netwerken](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|N.v.t.|
|mijnVirtueelNetwerkB|Resource Manager|[Inzender voor netwerken](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/peer|
||Klassiek|[Inzender voor klassieke netwerken](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|Microsoft.ClassicNetwork/virtualNetworks/peer|

## <a name="monitor"></a>Controleren

Wanneer u twee virtuele netwerken koppelt die zijn gemaakt via Resource Manager, moet voor elk virtueel netwerk een peering worden geconfigureerd. U kunt de status van de peeringverbinding controleren. De peeringstatus is een van de volgende statussen:

* **Gestart**: de status die wordt weergegeven wanneer u de peering vanuit het eerste virtuele netwerk naar het tweede virtuele netwerk maakt.
* **Verbonden**: de status die wordt weergegeven wanneer u de peering vanuit het tweede virtuele netwerk naar het eerste virtuele netwerk maakt. De status van de peering voor het eerste virtuele netwerk wordt gewijzigd van *Gestart* naar *Verbonden*. De peering van het virtuele netwerk is pas gelukt als de status van beide virtueel netwerk-peerings *Verbonden* is.
* **Verbinding verbroken**: de status die wordt weergegeven wanneer de peering van het ene virtuele netwerk naar het andere wordt verwijderd nadat een peering is vastgesteld tussen twee virtuele netwerken.

## <a name="troubleshoot"></a>Problemen oplossen

Om de peering op een virtueel netwerk te bevestigen, kunt u de [effectieve routes controleren](virtual-network-routes-troubleshoot-portal.md) voor een netwerkinterface in een subnet van een virtueel netwerk. Als een peering op een virtueel netwerk bestaat, hebben alle subnets binnen het virtuele netwerk routes met het volgende hoptype *VNet-peering* voor elke adresruimte in elk gekoppeld virtueel netwerk.

U kunt problemen met de verbinding met een virtuele machine in een gekoppeld virtueel netwerk ook oplossen met behulp van de [connectiviteitscontrole](../network-watcher/network-watcher-connectivity-portal.md) van Network Watcher. Met connectiviteitscontrole kunt u zien hoe verkeer wordt doorgestuurd van de netwerkinterface van een virtuele bronmachine naar de netwerkinterface van een virtuele doelmachine.

## <a name="limits"></a>Limieten

Er zijn limieten voor het aantal peerings dat is toegestaan voor één virtueel netwerk. Zie [Azure-netwerkenlimieten](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) voor meer informatie.

## <a name="pricing"></a>Prijzen

Er wordt een nominaal bedrag in rekening gebracht voor inkomend en uitgaand verkeer dat gebruikmaakt van een verbinding voor virtueel netwerk-peering. Zie de pagina [prijzen](https://azure.microsoft.com/pricing/details/virtual-network) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

* Voltooi een zelfstudie voor peering in virtuele netwerken. Peering in virtuele netwerken kan tot stand worden gebracht tussen virtuele netwerken die zijn gemaakt via hetzelfde implementatiemodel of via verschillende implementatiemodellen die tot hetzelfde abonnement of tot verschillende abonnementen behoren. Volg een zelfstudie voor een van de volgende scenario's:

    |Azure-implementatiemodel  | Abonnement  |
    |---------|---------|
    |Beide in Resource Manager |[Hetzelfde](virtual-network-create-peering.md)|
    | |[Verschillend](create-peering-different-subscriptions.md)|
    |Eén in Resource Manager, één klassiek     |[Hetzelfde](create-peering-different-deployment-models.md)|
    | |[Verschillend](create-peering-different-deployment-models-subscriptions.md)|

* Meer informatie over het maken van een [hub-en-spoke-netwerktopologie](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#virtual network-peering).
* Meer informatie over alle [instellingen van peering in virtuele netwerken en hoe u deze kunt aanpassen](virtual-network-manage-peering.md)
