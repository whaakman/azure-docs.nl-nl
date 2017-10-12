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
ms.openlocfilehash: 082cd8a6cf50f76c89fe5995047396c734f83034
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="virtual-network-peering"></a>Peering op virtueel netwerk

[Het Azure Virtual Network (VNet)](virtual-networks-overview.md) is uw eigen privénetwerkruimte in Azure waarmee u Azure-resources veilig met elkaar kunt verbinden.

Dankzij Virtual Network-peering kunt u naadloos virtuele netwerken naadloos met elkaar verbinden. Nadat de virtuele netwerken zijn gekoppeld via peering, worden ze voor verbindingsdoeleinden als één netwerk weergegeven. De virtuele machines in de gekoppelde virtuele netwerken kunnen rechtstreeks met elkaar communiceren.
Het verkeer tussen virtuele machines in de gekoppelde virtuele netwerken wordt doorgestuurd via de Microsoft-backbone-infrastructuur, vergelijkbaar met de manier waarop verkeer tussen virtuele machines in hetzelfde virtuele netwerk alleen wordt gerouteerd via *privé*-IP-adressen.

>[!IMPORTANT]
> U kunt virtuele netwerken in verschillende Azure-regio's koppelen. Deze functie is momenteel beschikbaar als preview-product. U kunt [uw abonnement voor de preview registreren](virtual-network-create-peering.md). Peering van virtuele netwerken in dezelfde regio's is algemeen beschikbaar.
>

Enkele voordelen van peering van virtuele netwerken zijn:

* Het verkeer dat via gekoppelde virtuele netwerken loopt, is volledig privé. Het loopt via het Microsoft-backbonenetwerk en niet via het openbare internet of via gateways.
* Een verbinding met lage latentie en hoge bandbreedte tussen resources in verschillende virtuele netwerken.
* De mogelijkheid om resources uit het ene virtuele netwerk te gebruiken in een ander virtueel netwerk als ze de peering eenmaal tot stand is gebracht.
* Als u virtuele netwerken koppelt, kunt u gegevens overzetten tussen Azure-abonnementen, implementatiemodellen en Azure-regio's (preview).
* De mogelijkheid om virtuele netwerken die zijn gemaakt via Azure Resource Manager te koppelen of om één virtueel netwerk dat is gemaakt via Resource Manager te koppelen aan een virtueel netwerk dat is gemaakt via het klassieke implementatiemodel. Lees het artikel [Azure-implementatiemodellen begrijpen](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) voor meer informatie over de verschillen tussen de twee Azure-implementatiemodellen.

## <a name="requirements-constraints"></a>Vereisten en beperkingen

* Peering van virtuele netwerken in dezelfde regio's is algemeen beschikbaar. Peering van virtuele netwerken in verschillende regio's is momenteel als preview beschikbaar in de regio's VS - west-centraal, Canada centraal en VS West 2. U kunt [uw abonnement voor de preview registreren](virtual-network-create-peering.md).
    > [!WARNING]
    > Peerings van virtuele netwerken die in dit scenario worden gemaakt, hebben mogelijk niet dezelfde mate van beschikbaarheid en betrouwbaarheid als scenario's waarbij een versie met algemene beschikbaarheid wordt gebruikt. Sommige functies van gekoppelde virtuele netwerken zijn mogelijk beperkt of zijn niet beschikbaar in alle Azure-regio's. Controleer de pagina [Azure Virtual Network-updates](https://azure.microsoft.com/updates/?product=virtual-network) voor de meest recente meldingen over de beschikbaarheid en de status van deze functie.

* De gekoppelde virtuele netwerken moeten niet-overlappende IP-adresruimten hebben.
* Adresruimten kunnen niet worden toegevoegd aan of verwijderd uit een virtueel netwerk wanneer een virtueel netwerk is gekoppeld aan een ander virtueel netwerk.
* Peering in virtuele netwerken vindt plaats tussen twee virtuele netwerken. Er is geen afgeleide transitieve relatie tussen peerings. Als virtualNetworkA bijvoorbeeld is gekoppeld aan virtualNetworkB en virtualNetworkB is gekoppeld aan virtualNetworkC, is virtualNetwork A *niet* gekoppeld aan virtualNetworkC.
* U kunt virtuele netwerken koppelen die zich in twee verschillende abonnementen bevinden, mits een bevoegde gebruiker (zie [specifieke machtigingen](create-peering-different-deployment-models-subscriptions.md#permissions)) van beide abonnementen de peering toestaat en de abonnementen zijn gekoppeld aan dezelfde Azure Active Directory-tenant. U kunt een [VPN-gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#V2V) gebruiken om virtuele netwerken te verbinden in abonnementen die zijn gekoppeld aan verschillende Active Directory-tenants.
* Virtuele netwerken kunnen worden gekoppeld als beide zijn gemaakt via het Resource Manager-implementatiemodel of als het ene virtuele netwerk is gemaakt via het Resource Manager-implementatiemodel en het andere via het klassieke implementatiemodel. Virtuele netwerken die zijn gemaakt via het klassieke implementatiemodel kunnen echter niet aan elkaar worden gekoppeld. U kunt een [VPN-gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#V2V) gebruiken om twee virtuele netwerken te verbinden die zijn gemaakt via het klassieke implementatiemodel.
* Hoewel de communicatie tussen virtuele machines in gekoppelde virtuele netwerken geen extra bandbreedtebeperkingen heeft, geldt er nog steeds een bandbreedtelimiet op basis van de grootte van de virtuele machine. Lees de artikelen over de grootte van virtuele machines voor [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) of [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) voor meer informatie over de maximale netwerkbandbreedte voor verschillende groottes van virtuele machines.

     ![Eenvoudige peering van virtuele netwerken](./media/virtual-networks-peering-overview/figure03.png)

## <a name="connectivity"></a>Connectiviteit

Nadat virtuele netwerken aan elkaar zijn gekoppeld, kunnen resources in beide virtuele netwerken rechtstreeks verbinding maken met resources in het gekoppelde virtuele netwerk.

De netwerklatentie tussen virtuele machines in gekoppelde virtuele netwerken in dezelfde regio is gelijk aan die binnen één virtueel netwerk. De netwerkdoorvoer is gebaseerd op de bandbreedte die is toegestaan voor de virtuele machine, evenredig aan de grootte. Er gelden verder geen extra beperkingen voor bandbreedte binnen de peering.

Het verkeer tussen virtuele machines in gekoppelde virtuele netwerken wordt rechtstreeks doorgestuurd via de Microsoft-backbone-infrastructuur, niet via een gateway of via het openbare internet.

Virtuele machines in een virtueel netwerk hebben toegang tot de interne load balancer in het gekoppelde virtuele netwerk in dezelfde regio. Ondersteuning voor een interne load balancer strekt zich niet uit tot wereldwijd gekoppelde virtuele netwerken als preview-product. In de algemeen beschikbare versie van wereldwijde peering van virtuele netwerken zal een interne load balancer worden ondersteund.

Netwerkbeveiligingsgroepen kunnen worden toegepast in beide virtuele netwerken om de toegang tot andere virtuele netwerken of subnetten desgewenst te blokkeren.
Bij het configureren van peering in virtuele netwerken kunt u de netwerkbeveiligingsgroepen tussen de virtuele netwerken openen of sluiten. Als u volledige connectiviteit tussen gekoppelde virtuele netwerken openzet (dit is de standaardoptie), kunt u netwerkbeveiligingsgroepen toepassen op specifieke subnetten of virtuele machines om specifieke toegang te blokkeren of te weigeren. Lees voor meer informatie over netwerkbeveiligingsgroepen het artikel [Overzicht van netwerkbeveiligingsgroepen](virtual-networks-nsg.md).

## <a name="service-chaining"></a>Servicechaining

U kunt door gebruikers gedefinieerde routes configureren die naar virtuele machines in gekoppelde virtuele netwerken verwijzen als het 'volgende hop'-IP-adres om servicechaining in te schakelen. Met servicechaining kunt u verkeer van een virtueel netwerk naar een virtueel apparaat in een gekoppeld virtueel netwerk leiden, door middel van door gebruikers gedefinieerde routes.

U kunt ook effectief hub en spoke-achtige omgevingen bouwen waarin de hub infrastructuuronderdelen kan hosten, zoals een virtueel netwerkapparaat. Alle virtuele spoke-netwerken kunnen vervolgens worden gekoppeld aan het virtuele hub-netwerk. Door virtuele netwerkapparaten die worden uitgevoerd in het virtuele hub-netwerk kan verkeer stromen. Kort gezegd, met peering in virtuele netwerken kan het 'volgende hop'-IP-adres op de door de gebruiker gedefinieerde route het IP-adres zijn van een virtuele machine in het gekoppelde virtuele netwerk. Lees het artikel [user-defined routes overview](virtual-networks-udr-overview.md) (Door de gebruiker gedefinieerde routes) voor meer informatie over door de gebruiker gedefinieerde routes. Meer informatie over het maken van een [hub-en-spoke-netwerktopologie](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#virtual network-peering)

## <a name="gateways-and-on-premises-connectivity"></a>Gateways en on-premises connectiviteit

Elk virtueel netwerk, ongeacht of het gekoppeld is aan een ander virtueel netwerk, kan nog steeds een eigen gateway hebben en gebruiken om te verbinden met een on-premises netwerk. U kunt ook [virtueel-netwerk-naar-virtueel-netwerk-verbindingen](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md) configureren door gateways te gebruiken, ook al zijn de virtuele netwerken gekoppeld.

Wanneer beide opties voor interconnectiviteit tussen virtuele netwerken zijn geconfigureerd, loopt het verkeer tussen de virtuele netwerken via de peering-configuratie (dat wil zeggen, via de Azure-backbone).

Wanneer virtuele netwerken zijn gekoppeld in dezelfde regio, kunt u ook de gateway in het gekoppelde virtuele netwerk configureren als een doorvoerpunt naar een on-premises netwerk. Het virtuele netwerk dat gebruikmaakt van een externe gateway kan in dit geval niet een eigen gateway hebben. Een virtueel netwerk kan slechts één gateway hebben. De gateway kan een lokale of een externe gateway zijn (in het gekoppelde virtuele netwerk), zoals wordt weergegeven op de volgende afbeelding:

![doorvoer bij peering van virtuele netwerken](./media/virtual-networks-peering-overview/figure04.png)

Gatewaydoorvoer wordt niet ondersteund in de peeringrelatie tussen virtuele netwerken die zijn gemaakt via verschillende implementatiemodellen of in verschillende regio's. Beide virtuele netwerken in de peeringrelatie moeten zijn gemaakt via Resource Manager en moeten zich in dezelfde regio bevinden om ervoor te zorgen dat de doorvoer via een gateway werkt. Wereldwijd gekoppelde virtuele netwerken ondersteunen momenteel geen gatewaydoorvoer.

Wanneer virtuele netwerken die één Azure ExpressRoute-verbinding delen, worden gekoppeld, gaat het verkeer tussen de twee netwerken via de peering-relatie (dat wil zeggen, via het backbone-netwerk van Azure). U kunt nog steeds in elk virtueel netwerk lokale gateways gebruiken om verbinding te maken met het on-premises circuit. U kunt ook een gedeelde gateway gebruiken en de doorvoer voor on-premises connectiviteit configureren.

## <a name="permissions"></a>Machtigingen

Virtueel netwerk-peering is een bevoegde bewerking. Het is een afzonderlijke functie onder de VirtualNetworks-naamruimte. Een gebruiker kan specifieke rechten krijgen voor het machtigen van peering. Een gebruiker met lees-/schrijftoegang tot het virtuele netwerk krijgt automatisch deze rechten.

Een gebruiker die ofwel een beheerder is of een bevoegde gebruiker van de peering-mogelijkheid kan een peering-bewerking op een ander virtueel netwerk initiëren. Het minimaal vereiste machtigingsniveau is Inzender voor netwerken. Als er een overeenkomende aanvraag voor peering van de andere kant komt en aan andere vereisten wordt voldaan, wordt de peering tot stand gebracht.

Als u bijvoorbeeld de virtuele netwerken genaamd mijnvirtueel netwerkA en mijnvirtueel netwerkB koppelt, moet aan uw account de volgende minimale rol of machtiging voor elk virtueel netwerk worden toegewezen:

|Virtueel netwerk|Implementatiemodel|Rol|Machtigingen|
|---|---|---|---|
|mijnvirtueel netwerkA|Resource Manager|[Inzender voor netwerken](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write|
| |Klassiek|[Inzender voor klassieke netwerken](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|N.v.t.|
|mijnvirtueel netwerkB|Resource Manager|[Inzender voor netwerken](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/peer|
||Klassiek|[Inzender voor klassieke netwerken](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|Microsoft.ClassicNetwork/virtualNetworks/peer|

## <a name="monitor"></a>Bewaken

Wanneer u twee virtuele netwerken koppelt die zijn gemaakt via Resource Manager, moet voor elk virtueel netwerk een peering worden geconfigureerd.
U kunt de status van de peeringverbinding controleren. De peeringstatus kan het volgende zijn:

* **Gestart**: wanneer u de peering naar het tweede virtuele netwerk vanuit het eerste virtuele netwerk maakt, is de peeringstatus Gestart.

* **Verbonden**: wanneer u de peering vanuit het tweede virtuele netwerk naar het eerste virtuele netwerk maakt, is de peeringstatus Verbonden. Als u de peeringstatus voor het eerste virtuele netwerk bekijkt, ziet u dat de status ervan is gewijzigd van Gestart in Verbonden. De peering is pas gelukt als de peeringstatus van beide virtueel netwerk-peerings Verbonden is.

* **Verbinding verbroken**: als een van de peeringkoppelingen wordt verwijderd nadat een verbinding tot stand is gebracht, verandert de peeringstatus in Verbinding verbroken.

## <a name="troubleshoot"></a>Problemen oplossen

U kunt problemen met verkeer dat via de peeringverbinding loopt oplossen door [uw effectieve routes te controleren](virtual-network-routes-troubleshoot-portal.md).

U kunt problemen met de verbinding met een virtuele machine in een gekoppeld virtueel netwerk ook oplossen met behulp van de [connectiviteitscontrole](../network-watcher/network-watcher-connectivity-portal.md) van Network Watcher. De connectiviteitscontrole laat zien hoe verkeer rechtstreeks vanaf de netwerkinterface van de bron-VM wordt doorgestuurd naar de netwerkinterface van de doel-VM.

## <a name="limits"></a>Limieten

Er zijn limieten voor het aantal peerings dat is toegestaan voor één virtueel netwerk. Het standaardaantal peerings is 50. U kunt het aantal peerings verhogen. Raadpleeg [Limieten voor Azure-netwerken](../azure-subscription-service-limits.md#networking-limits) voor meer informatie.

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
