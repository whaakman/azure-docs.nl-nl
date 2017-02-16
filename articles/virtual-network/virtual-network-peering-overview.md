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
ms.date: 10/17/2016
ms.author: narayan
translationtype: Human Translation
ms.sourcegitcommit: 15afcad97941fc595478e36e826a73831f40475e
ms.openlocfilehash: eb05b504c5cf13cd852a5e01cc3bec79fd20d547


---
# <a name="virtual-network-peering"></a>Peering op virtueel netwerk
Peering in virtuele netwerken (VNet) is een mechanisme om twee VNets in dezelfde regio via het backbone-netwerk van Azure te koppelen. Als de twee VNets zijn gekoppeld, worden deze als één netwerk weergegeven voor verbindingsdoeleinden. De twee VNets worden nog steeds beheerd als afzonderlijke resources, maar virtuele machines in de gekoppelde VNets kunnen met elkaar communiceren via privé-IP-adressen.

Het verkeer tussen virtuele machines in de gekoppelde VNets wordt doorgestuurd via de Azure-infrastructuur zoals het verkeer dat wordt gerouteerd tussen virtuele machines in hetzelfde VNet. De voordelen van VNet-peering zijn onder andere:

* Een verbinding met lage latentie en hoge bandbreedte tussen resources in verschillende VNets.
* De mogelijkheid om resources zoals netwerkapparaten en VPN Gateways als doorvoerpunten te gebruiken in een gekoppeld VNet.
* De mogelijkheid om twee VNets die zijn gemaakt via het Azure Resource Manager-implementatiemodel te koppelen en de mogelijkheid om één VNet dat is gemaakt via Resource Manager te koppelen aan een VNet dat is gemaakt via het klassieke implementatiemodel. Lees het artikel [Azure-implementatiemodellen begrijpen](../azure-resource-manager/resource-manager-deployment-model.md) voor meer informatie over de verschillen tussen de twee Azure-implementatiemodellen.

Vereisten en de belangrijkste aspecten van VNet-peering:

* De VNets die worden gekoppeld, moeten zich in dezelfde Azure-regio bevinden.
* De VNets die worden gekoppeld, moeten niet-overlappende IP-adresruimten hebben.
* VNet-peering vindt plaats tussen twee VNets en er is geen afgeleide transitieve relatie tussen peerings. Als VNetA bijvoorbeeld is gekoppeld aan VNetB, en als VNetB is gekoppeld aan VNetC, betekent dit *niet* automatisch dat VNetA is gekoppeld aan VNetC.
* U kunt VNets koppelen die zich bevinden in twee verschillende abonnementen, mits een bevoegde gebruiker van beide abonnementen de peering toestaat en de abonnementen zijn gekoppeld aan dezelfde Active Directory-tenant.
* VNets kunnen worden gekoppeld als beide zijn gemaakt via het Resource Manager-implementatiemodel of als een is gemaakt via het Resource Manager-implementatiemodel en het andere is gemaakt via het klassieke implementatiemodel. Twee VNets die zijn gemaakt via het klassieke implementatiemodel kunnen echter niet aan elkaar worden gekoppeld. Wanneer VNets via verschillende implementatiemodellen zijn gemaakt, moeten de VNets beide aanwezig zijn in *hetzelfde* abonnement. De mogelijkheid om VNets te koppelen die via verschillende implementatiemodellen zijn gemaakt die zich in *verschillende* abonnementen bevinden, bevindt zich in de **evaluatie**versie. Lees het artikel [Een peering voor virtuele netwerken maken met behulp van Powershell](virtual-networks-create-vnetpeering-arm-ps.md) voor meer informatie.
* Hoewel de communicatie tussen virtuele machines in gekoppelde VNets geen extra bandbreedtebeperkingen heeft, is er nog steeds een bandbreedtelimiet op basis van VM-grootte van toepassing. Lees de artikelen over VM-grootte voor [Windows](../virtual-machines/virtual-machines-windows-sizes.md) of [Linux](../virtual-machines/virtual-machines-linux-sizes.md) voor meer informatie over de maximale netwerkbandbreedte voor verschillende VM-groottes.

![Basic VNet-peering](./media/virtual-networks-peering-overview/figure01.png)

## <a name="connectivity"></a>Connectiviteit
Nadat twee VNets aan elkaar zijn gekoppeld, kunnen VM’s of Cloud Services-rollen rechtstreeks verbinding maken met andere resources in het gekoppelde virtuele netwerk. Deze twee VNets hebben een volledige IP-verbinding.

De netwerklatentie voor een retour tussen twee virtuele machines is dezelfde als die voor een retour binnen een enkel VNet. De netwerkdoorvoer is gebaseerd op de bandbreedte die is toegestaan voor de virtuele machine, evenredig aan de grootte. Er gelden verder geen extra beperkingen voor bandbreedte binnen de peering.

Het verkeer tussen de virtuele machines in gekoppelde VNets wordt rechtstreeks doorgestuurd via de back-endinfrastructuur van Azure en niet via een gateway.

Virtuele machines die zijn verbonden met een VNet hebben toegang tot de interne taakverdeling (ILB)-eindpunten in het gekoppelde VNet. Netwerkbeveiligingsgroepen (NSG's) kunnen worden toegepast in elk VNet om, indien gewenst, de toegang tot andere VNets of subnetten te blokkeren.

Bij het configureren van peering, kunt u de NSG-regels tussen de VNets openzetten of afsluiten. Als u de volledige connectiviteit tussen gekoppelde VNets openzet (dit is de standaardoptie), kunt u vervolgens NSG's op specifieke subnetten of virtuele machines gebruiken voor het blokkeren of weigeren van specifieke toegang. Lees het artikel [Netwerkbeveiligingsgroepen](virtual-networks-nsg.md) voor meer informatie over NSG's.

Door Azure verschafte interne DNS-naamomzetting voor virtuele machines werkt niet in gekoppelde VNets. Virtuele machines hebben interne DNS-namen die alleen binnen het lokale VNet oplosbaar zijn. U kunt echter virtuele machines die zijn verbonden met gekoppelde VNets als DNS-servers configureren voor een VNet. Lees het artikel [Naamomzetting met uw eigen DNS-server](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) voor meer informatie.

## <a name="service-chaining"></a>Servicechaining
U kunt door gebruikers gedefinieerde routetabellen (UDR) configureren die naar virtuele machines in gekoppelde VNets verwijzen als het 'volgende hop'-IP-adres, zoals verderop in dit artikel wordt weergegeven in het diagram. Hierdoor kunt u servicechaining bereiken, waarmee u verkeer van een VNet naar een virtueel apparaat dat wordt uitgevoerd in een gekoppeld VNet kunt leiden, door middel van door gebruikers gedefinieerde routetabellen (UDR’s).

U kunt ook effectief hub en spoke-achtige omgevingen bouwen waarin de hub infrastructuuronderdelen kan hosten, zoals een virtueel netwerkapparaat. Alle spoke-VNets kunnen hier vervolgens aan worden gekoppeld, terwijl ook een subset van verkeer wordt geleid naar apparaten die worden uitgevoerd in het hub-VNet. Kort gezegd, met VNet-peering kan het 'volgende hop'-IP-adres op de door de gebruiker gedefinieerde routetabel het IP-adres zijn van een virtuele machine in het gekoppelde VNet. Lees het artikel [Door gebruikers gedefinieerde routes](virtual-networks-udr-overview.md) voor meer informatie over door gebruikers gedefinieerde routetabellen.

## <a name="gateways-and-on-premises-connectivity"></a>Gateways en on-premises connectiviteit
Elk VNet, ongeacht of het gekoppeld is aan een ander VNet, kan nog steeds een eigen gateway hebben en gebruiken om te verbinden met een on-premises netwerk. Gebruikers kunnen ook [VNet-naar-VNet-verbindingen](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md) configureren door gateways te gebruiken, ook al zijn de VNets gekoppeld.

Wanneer beide opties voor interconnectiviteit tussen VNets zijn geconfigureerd, loopt het verkeer tussen de VNets via de peering-configuratie (dat wil zeggen, via de Azure-backbone).

Wanneer VNets zijn gekoppeld, kunnen gebruikers ook de gateway in het gekoppelde VNet configureren als een doorvoerpunt naar een on-premises netwerk. Het VNet dat gebruikmaakt van een externe gateway kan in dit geval niet een eigen gateway hebben. Een VNet kan slechts één gateway hebben. De gateway kan een lokale of een externe gateway zijn (in het gekoppelde VNet), zoals wordt weergegeven op de volgende afbeelding:

![Doorvoer VNet-peering](./media/virtual-networks-peering-overview/figure02.png)

Gateway-doorvoer wordt niet ondersteund in de peering-relatie tussen VNets die via verschillende implementatiemodellen zijn gemaakt. Beide VNets in de peering-relatie moeten zijn gemaakt via Resource Manager voor de doorvoer van een werkende gateway-doorvoer.

Wanneer VNets die één Azure ExpressRoute-verbinding delen, worden gekoppeld, gaat het verkeer tussen de twee netwerken via de peering-relatie (dat wil zeggen, via het backbone-netwerk van Azure). U kunt nog steeds in elk VNet lokale gateways gebruiken om verbinding te maken met het on-premises circuit. U kunt ook een gedeelde gateway gebruiken en de doorvoer voor on-premises connectiviteit configureren.

## <a name="provisioning"></a>Inrichten
VNet-peering is een bevoegde bewerking. Het is een afzonderlijke functie onder de VirtualNetworks-naamruimte. Een gebruiker kan specifieke rechten krijgen voor het machtigen van peering. Een gebruiker met lees-/schrijftoegang tot het virtuele netwerk krijgt automatisch deze rechten.

Een gebruiker die ofwel een beheerder is of een bevoegde gebruiker van de peering-mogelijkheid kan een peering-bewerking op een ander VNet initiëren. Als er een overeenkomende aanvraag voor peering van de andere kant komt, en aan andere vereisten wordt voldaan, wordt de peering tot stand gebracht.

Raadpleeg de artikelen in de sectie [Volgende stappen](#next-steps) voor meer informatie over het opzetten van VNet-peering tussen twee VNets.

## <a name="limits"></a>Limieten
Er zijn limieten voor het aantal peerings dat is toegestaan voor één virtueel netwerk. Raadpleeg [Limieten voor Azure-netwerken](../azure-subscription-service-limits.md#networking-limits) voor meer informatie.

## <a name="pricing"></a>Prijzen
Er wordt een nominaal bedrag in rekening gebracht voor inkomend en uitgaand verkeer dat gebruikmaakt van een VNet-peering. Raadpleeg voor meer informatie de [pagina met prijzen](https://azure.microsoft.com/pricing/details/virtual-network).

## <a name="a-namenext-stepsanext-steps"></a><a name="next-steps"></a>Volgende stappen
Lees meer informatie over het maken van een VNet-peering met:

* [Azure Portal](virtual-networks-create-vnetpeering-arm-portal.md)
* [Azure PowerShell](virtual-networks-create-vnetpeering-arm-ps.md)
* [Een Azure Resource Manager-sjabloon](virtual-networks-create-vnetpeering-arm-template-click.md)



<!--HONumber=Feb17_HO1-->


