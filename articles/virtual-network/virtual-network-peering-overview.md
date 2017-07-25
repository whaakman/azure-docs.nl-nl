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
ms.date: 07/17/2017
ms.author: narayan
ms.translationtype: HT
ms.sourcegitcommit: c3ea7cfba9fbf1064e2bd58344a7a00dc81eb148
ms.openlocfilehash: 393557074db2ddbeb53ca20873a33d06874c4dc8
ms.contentlocale: nl-nl
ms.lasthandoff: 07/19/2017

---
# <a name="virtual-network-peering"></a>Peering op virtueel netwerk
Peering in virtuele netwerken is een mechanisme om twee virtuele netwerken in dezelfde regio via het backbone-netwerk van Azure te koppelen. Als de twee virtuele netwerken gekoppeld zijn, worden deze als één netwerk weergegeven voor verbindingsdoeleinden. De twee virtuele netwerken worden nog steeds beheerd als afzonderlijke resources, maar virtuele machines in deze gekoppelde virtuele netwerken kunnen met elkaar communiceren via privé-IP-adressen.

Het verkeer tussen virtuele machines in de gekoppelde virtuele netwerken wordt doorgestuurd via de Azure-infrastructuur zoals het verkeer dat wordt gerouteerd tussen virtuele machines in hetzelfde virtuele netwerk. De voordelen van peering in virtuele netwerken zijn onder andere:

* Een verbinding met lage latentie en hoge bandbreedte tussen resources in verschillende virtuele netwerken.
* De mogelijkheid om resources zoals netwerkapparaten en VPN Gateways als doorvoerpunten te gebruiken in een gekoppeld virtueel netwerk.
* De mogelijkheid om twee virtuele netwerken die zijn gemaakt via het Azure Resource Manager-implementatiemodel te koppelen en de mogelijkheid om één virtueel netwerk dat is gemaakt via Resource Manager te koppelen aan een virtueel netwerk dat is gemaakt via het klassieke implementatiemodel. Lees het artikel [Azure-implementatiemodellen begrijpen](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) voor meer informatie over de verschillen tussen de twee Azure-implementatiemodellen.

## <a name="requirements-constraints"></a>Vereisten en beperkingen

* De virtuele netwerken die worden gekoppeld, moeten zich in dezelfde Azure-regio bevinden. U kunt virtuele netwerken in verschillende Azure-regio's verbinden met een [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#V2V).
* De gekoppelde virtuele netwerken moeten niet-overlappende IP-adresruimten hebben.
* Adresruimten kunnen niet worden toegevoegd aan of verwijderd uit een virtueel netwerk wanneer een virtueel netwerk is gekoppeld aan een ander virtueel netwerk.
* Peering in virtuele netwerken vindt plaats tussen twee virtuele netwerken. Er is geen afgeleide transitieve relatie tussen peerings. Als virtualNetworkA bijvoorbeeld is gekoppeld aan virtualNetworkB en virtualNetworkB is gekoppeld aan virtualNetworkC, is virtualNetwork A *niet* gekoppeld aan virtualNetworkC.
* U kunt virtuele netwerken koppelen die zich in twee verschillende abonnementen bevinden, mits een bevoegde gebruiker (zie [specifieke machtigingen](create-peering-different-deployment-models-subscriptions.md#permissions)) van beide abonnementen de peering toestaat en de abonnementen zijn gekoppeld aan dezelfde Azure Active Directory-tenant. U kunt een [VPN-gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#V2V) gebruiken om virtuele netwerken te verbinden in abonnementen die zijn gekoppeld aan verschillende Active Directory-tenants.
* Virtuele netwerken kunnen worden gekoppeld als beide zijn gemaakt via het Resource Manager-implementatiemodel of als het ene virtuele netwerk is gemaakt via het Resource Manager-implementatiemodel en het andere via het klassieke implementatiemodel. Twee virtuele netwerken die zijn gemaakt via het klassieke implementatiemodel kunnen echter niet aan elkaar worden gekoppeld. U kunt [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#V2V) gebruiken om twee virtuele netwerken te koppelen die zijn gemaakt via het klassieke implementatiemodel.
* Hoewel de communicatie tussen virtuele machines in gekoppelde virtuele netwerken geen extra bandbreedtebeperkingen heeft, geldt er nog steeds een bandbreedtelimiet op basis van de grootte van de virtuele machine. Lees de artikelen over de grootte van virtuele machines voor [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) of [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) voor meer informatie over de maximale netwerkbandbreedte voor verschillende groottes van virtuele machines.
* Door Azure verschafte interne DNS-naamomzetting voor virtuele machines werkt niet in gekoppelde virtuele netwerken. Virtuele machines hebben interne DNS-namen die alleen binnen het lokale virtuele netwerk oplosbaar zijn. U kunt echter virtuele machines die zijn gekoppeld aan virtuele netwerken configureren als DNS-servers voor een virtueel netwerk. Lees voor meer informatie het artikel [Naamomzetting met uw eigen DNS-server](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

![Eenvoudige peering van virtuele netwerken](./media/virtual-networks-peering-overview/figure01.png)

## <a name="connectivity"></a>Connectiviteit
Nadat twee virtuele netwerken aan elkaar zijn gekoppeld, kunnen resources in beide virtuele netwerken rechtstreeks verbinding maken met resources in het gekoppelde virtuele netwerk. De twee virtuele netwerken hebben een volledige IP-verbinding.

De netwerklatentie voor een retour tussen twee virtuele machines in gelijkwaardige virtuele netwerken is dezelfde als die voor een retour binnen een enkel virtueel netwerk. De netwerkdoorvoer is gebaseerd op de bandbreedte die is toegestaan voor de virtuele machine, evenredig aan de grootte. Er gelden verder geen extra beperkingen voor bandbreedte binnen de peering.

Het verkeer tussen virtuele machines in gekoppelde virtuele netwerken wordt rechtstreeks doorgestuurd via de back-end-infrastructuur van Azure, niet via een gateway.

Virtuele machines die zijn gekoppeld aan een virtueel netwerk hebben toegang tot de interne taakverdelingseindpunten in het gekoppelde virtuele netwerk. Netwerkbeveiligingsgroepen kunnen worden toegepast in elk virtueel netwerk om, indien gewenst, de toegang tot andere virtuele netwerken of subnetten te blokkeren.

Bij het configureren van peering in virtuele netwerken kunt u de netwerkbeveiligingsgroepen tussen de virtuele netwerken openen of sluiten. Als u besluit om de volledige connectiviteit tussen gelijkwaardige virtuele netwerken open te zetten (dit is de standaardoptie), kunt u vervolgens netwerkbeveiligingsgroepen op specifieke subnetten of virtuele machines gebruiken voor het blokkeren of weigeren van specifieke toegang. Lees voor meer informatie over netwerkbeveiligingsgroepen het artikel [Overzicht van netwerkbeveiligingsgroepen](virtual-networks-nsg.md).

## <a name="service-chaining"></a>Servicechaining
U kunt door gebruikers gedefinieerde routes configureren die naar virtuele machines in gekoppelde virtuele netwerken verwijzen als het 'volgende hop'-IP-adres om servicechaining in te schakelen. Met servicechaining kunt u verkeer van een virtueel netwerk naar een virtueel apparaat in een gekoppeld virtueel netwerk leiden, door middel van door gebruikers gedefinieerde routes.

U kunt ook effectief hub en spoke-achtige omgevingen bouwen waarin de hub infrastructuuronderdelen kan hosten, zoals een virtueel netwerkapparaat. Alle virtuele spoke-netwerken kunnen vervolgens worden gekoppeld aan het virtuele hub-netwerk. Door virtuele netwerkapparaten die worden uitgevoerd in het virtuele hub-netwerk kan verkeer stromen. Kort gezegd, met peering in virtuele netwerken kan het 'volgende hop'-IP-adres op de door de gebruiker gedefinieerde route het IP-adres zijn van een virtuele machine in het gekoppelde virtuele netwerk. Lees het artikel [user-defined routes overview](virtual-networks-udr-overview.md) (Door de gebruiker gedefinieerde routes) voor meer informatie over door de gebruiker gedefinieerde routes.

## <a name="gateways-and-on-premises-connectivity"></a>Gateways en on-premises connectiviteit
Elk virtueel netwerk, ongeacht of het gekoppeld is aan een ander virtueel netwerk, kan nog steeds een eigen gateway hebben en gebruiken om te verbinden met een on-premises netwerk. Gebruikers kunnen ook [virtueel-netwerk-naar-virtueel-netwerk-verbindingen](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json) configureren door gateways te gebruiken, ook al zijn de virtuele netwerken gekoppeld.

Wanneer beide opties voor interconnectiviteit tussen virtuele netwerken zijn geconfigureerd, loopt het verkeer tussen de virtuele netwerken via de peering-configuratie (dat wil zeggen, via de Azure-backbone).

Wanneer virtuele netwerken gelijkwaardig zijn gemaakt, kunt u ook de gateway in het gelijkwaardige virtuele netwerk configureren als een doorvoerpunt naar een on-premises netwerk. Het virtuele netwerk dat gebruikmaakt van een externe gateway kan in dit geval niet een eigen gateway hebben. Een virtueel netwerk kan slechts één gateway hebben. De gateway kan een lokale of een externe gateway zijn (in het gekoppelde virtuele netwerk), zoals wordt weergegeven op de volgende afbeelding:

![Doorvoer VNet-peering](./media/virtual-networks-peering-overview/figure02.png)

Gateway-doorvoer wordt niet ondersteund in de peering-relatie tussen virtuele netwerken die via verschillende implementatiemodellen zijn gemaakt. Beide virtuele netwerken in de peering-relatie moeten zijn gemaakt via Resource Manager voor de doorvoer van een werkende gateway-doorvoer.

Wanneer virtuele netwerken die één Azure ExpressRoute-verbinding delen, worden gekoppeld, gaat het verkeer tussen de twee netwerken via de peering-relatie (dat wil zeggen, via het backbone-netwerk van Azure). U kunt nog steeds in elk virtueel netwerk lokale gateways gebruiken om verbinding te maken met het on-premises circuit. U kunt ook een gedeelde gateway gebruiken en de doorvoer voor on-premises connectiviteit configureren.

## <a name="provisioning"></a>Inrichten
Virtueel netwerk-peering is een bevoegde bewerking. Het is een afzonderlijke functie onder de VirtualNetworks-naamruimte. Een gebruiker kan specifieke rechten krijgen voor het machtigen van peering. Een gebruiker met lees-/schrijftoegang tot het virtuele netwerk krijgt automatisch deze rechten.

Een gebruiker die ofwel een beheerder is of een bevoegde gebruiker van de peering-mogelijkheid kan een peering-bewerking op een ander virtueel netwerk initiëren. Als er een overeenkomende aanvraag voor peering van de andere kant komt en aan andere vereisten wordt voldaan, wordt de peering tot stand gebracht.

## <a name="limits"></a>Limieten
Er zijn limieten voor het aantal peerings dat is toegestaan voor één virtueel netwerk. Raadpleeg [Limieten voor Azure-netwerken](../azure-subscription-service-limits.md#networking-limits) voor meer informatie.

## <a name="pricing"></a>Prijzen
Er wordt een nominaal bedrag in rekening gebracht voor inkomend en uitgaand verkeer dat gebruikmaakt van een virtueel netwerk-peering. Zie de pagina [prijzen](https://azure.microsoft.com/pricing/details/virtual-network) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

* Voltooi een zelfstudie voor peering in virtuele netwerken. Peering in virtuele netwerken kan tot stand worden gebracht tussen virtuele netwerken die zijn gemaakt via hetzelfde implementatiemodel of via verschillende implementatiemodellen die tot hetzelfde abonnement of tot verschillende abonnementen behoren. Volg een zelfstudie voor een van de volgende scenario's:
 
    |Azure-implementatiemodel  | Abonnement  |
    |---------|---------|
    |Beide in Resource Manager |[Hetzelfde](virtual-network-create-peering.md)|
    | |[Verschillend](create-peering-different-subscriptions.md)|
    |Eén in Resource Manager, één klassiek     |[Hetzelfde](create-peering-different-deployment-models.md)|
    | |[Verschillend](create-peering-different-deployment-models-subscriptions.md)|

* Meer informatie over het maken van een [hub-en-spoke-netwerktopologie](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) 
* Meer informatie over alle [instellingen van peering in virtuele netwerken en hoe u deze kunt aanpassen](virtual-network-manage-peering.md)

