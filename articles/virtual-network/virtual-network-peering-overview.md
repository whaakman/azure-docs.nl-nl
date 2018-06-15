---
title: Peering in virtuele netwerken van Azure | Microsoft Docs
description: Meer informatie over peering in virtuele netwerken in Azure.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
ms.assetid: eb0ba07d-5fee-4db0-b1cb-a569b7060d2a
ms.service: virtual-network
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/17/2018
ms.author: jdial
ms.openlocfilehash: 27eaa6582a355198b61e996cce0a4acce48061cb
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35267125"
---
# <a name="virtual-network-peering"></a>Peering op virtueel netwerk

Dankzij Virtual Network-peering kunt u naadloos twee [virtuele netwerken](virtual-networks-overview.md) in Azure met elkaar verbinden. Nadat de virtuele netwerken zijn gekoppeld via peering, worden ze voor verbindingsdoeleinden weergegeven als één netwerk. Het verkeer tussen de virtuele machines in de gekoppelde virtuele netwerken wordt doorgestuurd via de Microsoft-backbone-infrastructuur, vergelijkbaar met de manier waarop verkeer tussen virtuele machines in hetzelfde virtuele netwerk alleen wordt gerouteerd via *privé*-IP-adressen. Azure ondersteunt:
* VNet-peering - VNets verbinden binnen dezelfde Azure-regio
* Globale VNet-peering - VNets verbinden tussen Azure-regio's

Enkele voordelen van het gebruik van peering van virtuele netwerken - lokaal of globaal - zijn:

* Netwerkverkeer tussen gekoppelde virtuele netwerken is privé. Verkeer tussen de virtuele netwerken wordt opgeslagen in het Microsoft-backbone-netwerk. Er zijn geen openbare internetverbinding, gateways of versleuteling vereist in de communicatie tussen de virtuele netwerken.
* Een verbinding met lage latentie en hoge bandbreedte tussen resources in verschillende virtuele netwerken.
* De mogelijkheid voor resources in één virtueel netwerk om te communiceren met resources in een ander virtueel netwerk, zodra de virtuele netwerken zijn gekoppeld.
* De mogelijkheid om gegevens over te brengen tussen Azure-abonnementen, implementatiemodellen en Azure-regio's.
* De mogelijkheid om virtuele netwerken die zijn gemaakt via Azure Resource Manager te koppelen of om één virtueel netwerk dat is gemaakt via Resource Manager te koppelen aan een virtueel netwerk dat is gemaakt via het klassieke implementatiemodel. Zie [Over Azure-implementatiemodellen](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) voor meer informatie over Azure-implementatiemodellen.
* Geen downtime tot bronnen in een virtueel netwerk bij het maken van de peering of nadat de peering is gemaakt.

## <a name="connectivity"></a>Connectiviteit

Nadat virtuele netwerken aan elkaar zijn gekoppeld, kunnen resources in beide virtuele netwerken rechtstreeks verbinding maken met resources in het gekoppelde virtuele netwerk.

De netwerklatentie tussen virtuele machines in gekoppelde virtuele netwerken in dezelfde regio is gelijk aan de latentie binnen één virtueel netwerk. De netwerkdoorvoer is gebaseerd op de bandbreedte die is toegestaan voor de virtuele machine, evenredig aan de grootte. Er gelden verder geen extra beperkingen voor bandbreedte binnen de peering.

Het verkeer tussen virtuele machines in gekoppelde virtuele netwerken wordt rechtstreeks doorgestuurd via de Microsoft-backbone-infrastructuur, niet via een gateway of via het openbare internet.

Netwerkbeveiligingsgroepen kunnen worden toegepast in elk virtueel netwerk om, indien gewenst, de toegang tot andere virtuele netwerken of subnetten te blokkeren.
Bij het configureren van peering in virtuele netwerken kunt u de netwerkbeveiligingsgroepen tussen de virtuele netwerken openen of sluiten. Als u besluit om de volledige connectiviteit tussen gelijkwaardige virtuele netwerken open te zetten (dit is de standaardoptie), kunt u vervolgens netwerkbeveiligingsgroepen op specifieke subnetten of virtuele machines gebruiken voor het blokkeren of weigeren van specifieke toegang. Zie [Overzicht van netwerkbeveiligingsgroepen](security-overview.md) voor meer informatie over netwerkbeveiligingsgroepen.

## <a name="service-chaining"></a>Servicechaining

U kunt door gebruikers gedefinieerde routes configureren die naar virtuele machines in gekoppelde virtuele netwerken of naar virtuele netwerkgateways verwijzen als het *volgende hop*-IP-adres om servicechaining in te schakelen. Met servicechaining kunt u verkeer van een virtueel netwerk naar een virtueel apparaat of een virtuele netwerkgateway in een gekoppeld virtueel netwerk leiden, door middel van door gebruikers gedefinieerde routes.

U kunt ook hub en spoke-omgevingen implementeren waarin het virtuele hub-netwerk infrastructuuronderdelen kan hosten, zoals een virtueel netwerkapparaat of een VPN-gateway. Alle virtuele spoke-netwerken kunnen vervolgens worden gekoppeld aan het virtuele hub-netwerk. Door virtuele netwerkapparaten of VPN-gateways die worden uitgevoerd in het virtuele hub-netwerk kan verkeer stromen. 

Met peering van virtuele netwerken kan de volgende hop op de door de gebruiker gedefinieerde route het IP-adres zijn van een virtuele machine in het gekoppelde virtuele netwerk of VPN-gateway. U kunt echter geen verkeer doorsturen tussen virtuele netwerken met een door de gebruiker gedefinieerde route waarin de ExpressRoute-gateway is opgegeven als het volgende hoptype. Zie [Door de gebruiker gedefinieerde routes](virtual-networks-udr-overview.md#user-defined) voor meer informatie over door de gebruiker gedefinieerde routes. Zie [Hub-en-spoke-netwerktopologie](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json) voor meer informatie over het maken van een hub-en-spokenetwerktopologie.

## <a name="gateways-and-on-premises-connectivity"></a>Gateways en on-premises connectiviteit

Elk virtueel netwerk, ongeacht of het gekoppeld is aan een ander virtueel netwerk, kan nog steeds een eigen gateway hebben en gebruiken om te verbinden met een on-premises netwerk. U kunt ook [virtueel-netwerk-naar-virtueel-netwerk-verbindingen](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json) configureren door gateways te gebruiken, ook al zijn de virtuele netwerken gekoppeld.

Wanneer beide opties voor interconnectiviteit tussen virtuele netwerken zijn geconfigureerd, loopt het verkeer tussen de virtuele netwerken via de peering-configuratie (dat wil zeggen, via de Azure-backbone).

Wanneer virtuele netwerken zijn gekoppeld in dezelfde regio, kunt u ook de gateway in het gekoppelde virtuele netwerk configureren als een doorvoerpunt naar een on-premises netwerk. Het virtuele netwerk dat gebruikmaakt van een externe gateway kan in dit geval niet een eigen gateway hebben. Een virtueel netwerk kan slechts één gateway hebben. De gateway kan een lokale of een externe gateway zijn (in het gekoppelde virtuele netwerk), zoals wordt weergegeven op de volgende afbeelding:

![doorvoer bij peering van virtuele netwerken](./media/virtual-networks-peering-overview/figure04.png)

Gateway-doorvoer wordt niet ondersteund in de peeringrelatie tussen virtuele netwerken die in verschillende regio’s zijn gemaakt. Beide virtuele netwerken in de peeringrelatie moeten zich in dezelfde regio bevinden om ervoor te zorgen dat de doorvoer via een gateway werkt. Gateway-doorvoer tussen virtuele netwerken die zijn gemaakt via verschillende implementatiemodellen (Resource Manager en classic) wordt alleen ondersteund als de gateway zich in het virtuele netwerk (Resource Manager) bevindt. Zie voor meer informatie over het gebruik van een gateway voor de doorvoer, [Een VPN-gateway configureren voor de doorvoer in een virtueel netwerkpeering](../vpn-gateway/vpn-gateway-peering-gateway-transit.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Wanneer virtuele netwerken die één Azure ExpressRoute-verbinding delen, worden gekoppeld, gaat het verkeer tussen de twee netwerken via de peering-relatie (dat wil zeggen, via het backbone-netwerk van Azure). U kunt nog steeds in elk virtueel netwerk lokale gateways gebruiken om verbinding te maken met het on-premises circuit. U kunt ook een gedeelde gateway gebruiken en de doorvoer voor on-premises connectiviteit configureren.

## <a name="troubleshoot"></a>Problemen oplossen

Om de peering op een virtueel netwerk te bevestigen, kunt u de [effectieve routes controleren](diagnose-network-routing-problem.md) voor een netwerkinterface in een subnet van een virtueel netwerk. Als een peering op een virtueel netwerk bestaat, hebben alle subnets binnen het virtuele netwerk routes met het volgende hoptype *VNet-peering* voor elke adresruimte in elk gekoppeld virtueel netwerk.

U kunt problemen met de verbinding met een virtuele machine in een gekoppeld virtueel netwerk ook oplossen met behulp van de [connectiviteitscontrole](../network-watcher/network-watcher-connectivity-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) van Network Watcher. Met connectiviteitscontrole kunt u zien hoe verkeer wordt doorgestuurd van de netwerkinterface van een virtuele bronmachine naar de netwerkinterface van een virtuele doelmachine.

## <a name="requirements-and-constraints"></a>Vereisten en beperkingen

Zie [Vereisten en beperkingen voor peering op een virtueel netwerk](virtual-network-manage-peering.md#requirements-and-constraints) voor meer informatie over vereisten en beperkingen. Zie [Azure-netwerklimieten](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) voor meer informatie over de limieten voor het aantal peerings dat u kunt maken voor een virtueel netwerk. 

## <a name="permissions"></a>Machtigingen

Zie [Machtigingen voor peering op een virtueel netwerk](virtual-network-manage-peering.md#permissions) voor meer informatie over de vereiste machtigingen voor het maken van peering op een virtueel netwerk.

## <a name="pricing"></a>Prijzen

Er wordt een nominaal bedrag in rekening gebracht voor inkomend en uitgaand verkeer dat gebruikmaakt van een verbinding voor virtueel netwerk-peering. Zie de pagina [prijzen](https://azure.microsoft.com/pricing/details/virtual-network) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

* Peering in virtuele netwerken kan tot stand worden gebracht tussen virtuele netwerken die zijn gemaakt via hetzelfde implementatiemodel of via verschillende implementatiemodellen die tot hetzelfde abonnement of tot verschillende abonnementen behoren. Volg een zelfstudie voor een van de volgende scenario's:

    |Azure-implementatiemodel             | Abonnement  |
    |---------                          |---------|
    |Beide in Resource Manager              |[Hetzelfde](tutorial-connect-virtual-networks-portal.md)|
    |                                   |[Verschillend](create-peering-different-subscriptions.md)|
    |Eén in Resource Manager, één klassiek  |[Hetzelfde](create-peering-different-deployment-models.md)|
    |                                   |[Verschillend](create-peering-different-deployment-models-subscriptions.md)|

* Meer informatie over het maken van een [hub-en-spoke-netwerktopologie](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json).
* Meer informatie over alle [instellingen van peering in virtuele netwerken en hoe u deze kunt aanpassen](virtual-network-manage-peering.md).
