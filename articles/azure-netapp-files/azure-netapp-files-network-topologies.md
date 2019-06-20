---
title: Richtlijnen voor Azure NetApp Files netwerk planning | Microsoft Docs
description: Hierin wordt beschreven richtlijnen die bij het ontwerpen van een doeltreffende netwerkarchitectuur helpen kunnen met behulp van Azure NetApp bestanden.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/08/2019
ms.author: b-juche
ms.openlocfilehash: bf2262d8a222cec6c5d0d7e53ded7b2994481656
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67205661"
---
# <a name="guidelines-for-azure-netapp-files-network-planning"></a>Richtlijnen voor Azure NetApp Files-netwerkplanning

Planning van architectuur van het netwerk is een belangrijk element van het ontwerpen van een toepassingsinfrastructuur. Dit artikel helpt bij het ontwerpen van een doeltreffende netwerkarchitectuur voor uw workloads profiteren van de uitgebreide mogelijkheden van Azure NetApp bestanden.

Azure NetApp bestanden volumes zijn ontworpen om te worden opgenomen in een speciaal subnet met de naam [overgedragen subnet](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-subnet) binnen uw Azure Virtual Network. Daarom kunt u openen de volumes rechtstreeks vanaf uw VNet, gekoppelde VNets in dezelfde regio of van on-premises via een virtuele netwerkgateway (ExpressRoute of VPN-Gateway) zo nodig. Het subnet is toegewezen aan Azure NetApp bestanden en er is geen verbinding met andere Azure-services of het internet.

## <a name="considerations"></a>Overwegingen  

U moet enkele overwegingen kennen wanneer u van plan voor NetApp-bestanden van Azure-netwerk bent.

### <a name="constraints"></a>Beperkingen

De onderstaande functies worden momenteel niet ondersteund voor Azure NetApp bestanden: 

* Netwerkbeveiligingsgroepen (nsg's) op subnet
* Gebruiker gedefinieerde routes (udr's) met de volgende hop als het subnet voor Azure NetApp-bestanden
* Azure-beleid (bijvoorbeeld aangepaste naamgevingsbeleid) op de interface Azure NetApp bestanden
* Load balancers voor Azure NetApp Files verkeer

De volgende netwerkbeperkingen van toepassing op Azure NetApp bestanden:

* Het aantal IP-adressen in een VNet met Azure NetApp-bestanden (met inbegrip van de gekoppelde VNets) gebruikt, mag niet meer dan 1000.
* In elk virtueel Azure-netwerk (VNet) kan er slechts één subnet aan Azure NetApp Files worden gedelegeerd.


### <a name="supported-network-topologies"></a>Ondersteunde netwerktopologieën

De volgende tabel beschrijft de netwerktopologieën ondersteund door Azure NetApp bestanden.  Hierin wordt ook de tijdelijke oplossingen voor de niet-ondersteunde topologieën. 

|    Topologies    |    wordt ondersteund    |     Tijdelijke oplossing    |
|-------------------------------------------------------------------------------------------------------------------------------|--------------------|-----------------------------------------------------------------------------|
|    Verbinding met volume in een lokale VNet    |    Ja    |         |
|    Verbinding met volume in een gekoppeld VNet (dezelfde regio)    |    Ja    |         |
|    Verbinding met volume in een gekoppeld VNet (Cross-regio of wereldwijde peering)    |    Nee    |    Geen    |
|    Verbinding met een volume via ExpressRoute-gateway    |    Ja    |         |
|    Connectiviteit van on-premises naar een volume in een knooppunt VNet via ExpressRoute-gateway en VNet-peering met de gateway-doorvoer    |    Nee    |    Een gedelegeerde subnet maken in de hub VNet (Azure VNet met Gateway)    |
|    Connectiviteit van on-premises naar een volume in een knooppunt VNet via VPN-gateway    |    Ja    |         |
|    Connectiviteit van on-premises naar een volume in een knooppunt VNet via VPN-gateway en VNet-peering met de gateway-doorvoer    |    Ja    |         |


## <a name="virtual-network-for-azure-netapp-files-volumes"></a>Virtueel netwerk voor Azure NetApp Files volumes

In deze sectie worden de concepten die u bij het plannen van het virtuele netwerk helpen.

### <a name="azure-virtual-networks"></a>Virtuele netwerken van Azure

Voor het inrichten van een Azure NetApp Files-volume, moet u een Azure-netwerk (VNet) maken of gebruiken dat al in uw abonnement bestaat. Het VNet definieert de grens van het netwerk van het volume.  Zie voor meer informatie over het maken van virtuele netwerken, de [documentatie voor Azure Virtual Network](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview).

### <a name="subnets"></a>Subnetten

Het virtuele netwerk segmenteren subnetten in afzonderlijke adresruimten die worden gebruikt door de Azure-resources.  Azure NetApp bestanden volumes zijn opgenomen in een speciale subnet met de naam [overgedragen subnet](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-subnet). 

Subnet delegering biedt expliciete machtigingen voor de service Azure NetApp Files servicespecifieke om resources te maken in het subnet.  Een unieke id in de implementatie van de service wordt gebruikt. In dit geval wordt een netwerkinterface gemaakt voor de verbinding met Azure NetApp bestanden.

Als u een nieuw VNet gebruikt, kunt u een subnet maken en het subnet met de instructies in Azure NetApp bestanden overdragen [een subnet aan Azure NetApp bestanden overdragen](azure-netapp-files-delegate-subnet.md). U kunt ook een bestaand leeg subnet die niet al wordt overgedragen aan andere services delegeren.

Als het VNet is gekoppeld aan een ander VNet, kunt u de VNet-adresruimte niet uitbreiden. Om die reden moet de nieuwe gedelegeerde subnet worden gemaakt binnen de VNet-adresruimte. Als u nodig hebt om uit te breiden de adresruimte, moet u de VNet-peering voor het uitbreiden van de adresruimte verwijderen.

### <a name="udrs-and-nsgs"></a>Udr en nsg 's

Netwerkbeveiligingsgroepen (nsg's) met de volgende hop kunnen niet worden gebruikt als gedelegeerde subnetten voor Azure NetApp-bestanden. Op dezelfde manier de gebruiker gedefinieerde routes (udr's) ook niet ondersteund. 

Als tijdelijke oplossing, kunt u nsg's toepassen op andere subnetten die toestaan of weigeren van het verkeer van en naar het subnet Azure NetApp Files overgedragen.  

## <a name="azure-native-environments"></a>Systeemeigen Azure-omgevingen

Het volgende diagram ziet u een systeemeigen Azure-omgeving:

![Azure-eigen netwerkomgeving](../media/azure-netapp-files/azure-netapp-files-network-azure-native-environment.png)

### <a name="local-vnet"></a>Lokale VNet

Een eenvoudige scenario is het maken of vanaf een virtuele machine (VM) in hetzelfde VNet verbinding maken met een Azure NetApp Files-volume. Voor VNet 2 in het bovenstaande diagram, Volume 1 in een gedelegeerde subnet wordt gemaakt en kunnen worden gekoppeld op 1 van de virtuele machine in de standaard-subnet.

### <a name="vnet-peering"></a>VNet-peering

Als u extra VNets in dezelfde regio die toegang tot elkaars bronnen nodig hebt, de VNets kunnen worden verbonden met [VNet-peering](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) om beveiligde verbindingen via de Azure-infrastructuur. 

Houd rekening met VNet 2 en 3 van de VNet in het bovenstaande diagram. Als VM 1 moet verbinding maken met VM-2- en Volume 2, of als VM 2 moet verbinding maken met VM-1 of Volume 1, moet u om in te schakelen van VNet-peering tussen VNet 2 en 3 van de VNet. 

Daarnaast kunt u een scenario waarbij VNet 1 is gekoppeld aan VNet 2 en VNet-2 is gekoppeld aan VNet 3 in dezelfde regio. De resources van VNet-1 verbinding kunnen maken met resources in VNet 2, maar er geen verbinding met resources in de VNet-3, tenzij VNet-1 en 3 van de VNet aan elkaar zijn gekoppeld. 

In het bovenstaande diagram 3 van de virtuele machine kunt verbinding maken met Volume 1, 4 van de virtuele machine kan geen verbinding maken met Volume 2.  De reden hiervoor is dat het knooppunt VNets aan elkaar zijn niet gekoppeld, en _transitroutering wordt niet ondersteund via VNet-peering_.

## <a name="hybrid-environments"></a>Hybride omgevingen

Het volgende diagram ziet u een hybride omgeving: 

![Hybride-netwerkomgeving](../media/azure-netapp-files/azure-netapp-files-network-hybrid-environment.png)

In het scenario voor hybride nodig toepassingen van on-premises datacenters toegang tot de resources in Azure.  Dit het geval is, of u wilt uw datacenter uitbreiden naar Azure, of u wilt gebruiken van systeemeigen Azure-services of voor herstel na noodgevallen. Zie [VPN-Gateway opties](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways?toc=%2fazure%2fvirtual-network%2ftoc.json#planningtable) over verbinding maken tussen meerdere on-premises bronnen en bronnen in Azure via een site-naar-site-VPN of een ExpressRoute.

In een hybride hub-spoke-topologie, wordt de hub VNet in Azure fungeert als een centraal punt van connectiviteit naar uw on-premises netwerk. De spaken zijn VNets gekoppeld aan de hub, en ze kunnen worden gebruikt om workloads te isoleren.

Afhankelijk van de configuratie. U kunt resources verbinden met on-premises resources in de hub en de knooppunten.

In de topologie die hierboven wordt geïllustreerd, de on-premises-netwerk is verbonden met een hub VNet in Azure en er zijn 2 knooppunt VNets in dezelfde regio aan elkaar gekoppeld met de hub VNet.  In dit scenario zijn de opties voor netwerkconnectiviteit ondersteund voor volumes van Azure NetApp bestanden als volgt:

* On-premises bronnen VM 1 en 2 van de virtuele machine kunnen verbinden met Volume 1 in de hub via een site-naar-site VPN of Express Route. 
* On-premises bronnen VM 1 en 2 van de virtuele machine kunnen verbinden met Volume 2 of 3 Volume op een site-naar-site VPN- en regionale Vnet-peering.
* 3 van de virtuele machine in de hub VNet verbinden met volume 2 in knooppunt 1 VNet en Volume 3 in knooppunt VNet 2.
* 4 van de virtuele machine van knooppunt VNet 1 en 5 van de virtuele machine van knooppunt VNet 2 kunt verbinden met Volume 1 in de hub VNet.

VM-4 in knooppunt 1 VNet kan geen verbinding maken met Volume 3 in knooppunt VNet 2. Ook 5 van de virtuele machine in knooppunt VNet2 kan geen verbinding maken met Volume 2 in knooppunt 1 VNet. Dit is het geval omdat het knooppunt VNets aan elkaar zijn niet gekoppeld en _transitroutering wordt niet ondersteund via VNet-peering_.

## <a name="next-steps"></a>Volgende stappen

[Een subnet delegeren aan Azure NetApp Files](azure-netapp-files-delegate-subnet.md)
