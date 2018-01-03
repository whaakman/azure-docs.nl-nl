---
title: Problemen met routes - PowerShell | Microsoft Docs
description: Informatie over het oplossen van problemen met routes in het Azure Resource Manager-implementatiemodel met Azure PowerShell.
services: virtual-network
documentationcenter: na
author: AnithaAdusumilli
manager: narayan
editor: 
tags: azure-resource-manager
ms.assetid: bf7dc5e7-9399-460e-8e0d-8992dbed98a6
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/23/2016
ms.author: anithaa
ms.openlocfilehash: ac7f8ddaf84ba94075a9c9c3195bd57534c6821b
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="troubleshoot-routes-using-azure-powershell"></a>Problemen oplossen met Azure PowerShell routes
> [!div class="op_single_selector"]
> * [Azure-portal](virtual-network-routes-troubleshoot-portal.md)
> * [PowerShell](virtual-network-routes-troubleshoot-powershell.md)
> 
> 

Als u ondervindt problemen met de netwerkverbinding naar of van uw Azure-virtuele Machine (VM), routes mogelijk van invloed op uw verkeersstromen VM. Dit artikel bevat een overzicht van mogelijkheden voor routes om op te lossen verdere diagnostische gegevens.

Routetabellen zijn gekoppeld aan de subnets en effectieve zijn op alle netwerkinterfaces (NIC) in dat subnet. De volgende soorten routes kunnen worden toegepast op elke netwerkinterface:

* **Systeemroutes:** elk subnet dat is gemaakt in een Azure-netwerk (VNet) heeft standaard route systeemtabellen waarmee lokale VNet-verkeer, lokale-verkeer via de VPN-gateways en internetverkeer. Systeemroutes ook aanwezig zijn voor VNets peer is ingesteld.
* **BGP-routes:** doorgegeven aan netwerkinterfaces via ExpressRoute of site-naar-site VPN-verbindingen. Meer informatie over BGP-routering door het lezen van de [BGP met VPN-gateways](../vpn-gateway/vpn-gateway-bgp-overview.md) en [overzicht van ExpressRoute](../expressroute/expressroute-introduction.md) artikelen.
* **Gebruiker gedefinieerde routes (UDR):** als u virtuele netwerkapparaten of zijn geforceerde tunneling verkeer naar een on-premises netwerk via een site-naar-site VPN, wellicht hebt u gebruiker gedefinieerde routes (udr's) die zijn gekoppeld aan de routetabel voor subnet. Als u niet bekend met udr's bent, leest u de [gebruiker gedefinieerde routes](virtual-networks-udr-overview.md#user-defined) artikel.

Met de verschillende routes die aan een netwerkinterface kunnen worden toegepast, kan het lastig zijn om te bepalen welke cumulatieve routes effectieve zijn. U kunt de effectieve routes voor een netwerkinterface voor het oplossen van VM-netwerkverbinding, bekijken in het Azure Resource Manager-implementatiemodel.

## <a name="using-effective-routes-to-troubleshoot-vm-traffic-flow"></a>Problemen met VM-netwerkverkeer met effectieve Routes
In dit artikel wordt het volgende scenario als voorbeeld ter illustratie van de effectieve routes voor een netwerkinterface oplossen:

Een virtuele machine (*VM1*) is verbonden met het VNet (*VNet1*, voorvoegsel: 10.9.0.0/16) geen verbinding maken met een VM(VM3) in een nieuw peered VNet (*VNet3*, 10.10.0.0/16 voorvoegsel). Er zijn geen udr's of BGP-routes aan netwerkinterface VM1 NIC1 verbonden met de virtuele machine is toegepast, alleen de systeemroutes van het worden toegepast.

In dit artikel wordt uitgelegd hoe de oorzaak van de verbindingsfout met van effectieve routes mogelijkheid in Azure Resource Management-implementatiemodel.
Terwijl het voorbeeld alleen de systeemroutes van het wordt, kunnen dezelfde stappen om te bepalen van binnenkomende en uitgaande verbindingsfouten via een routetype worden gebruikt.

> [!NOTE]
> Als uw virtuele machine meer dan één NIC die is gekoppeld heeft, controleert u effectieve routes voor elk van de NIC's voor het vaststellen van problemen met de netwerkverbinding naar en van een virtuele machine.
> 
> 

### <a name="view-effective-routes-for-a-virtual-machine"></a>De effectieve routes weergeven voor een virtuele machine
Overzicht van de cumulatieve routes die worden toegepast op een virtuele machine, moet u de volgende stappen uitvoeren:

### <a name="view-effective-routes-for-a-network-interface"></a>De effectieve routes weergeven voor een netwerkinterface
Overzicht van de cumulatieve routes die worden toegepast op een netwerkinterface, de volgende stappen uitvoeren:

1. Start een Azure PowerShell-sessie en meld u aan bij Azure. Als u niet bekend met Azure PowerShell bent, leest u de [installeren en configureren van Azure PowerShell](/powershell/azure/overview) artikel. Uw account moet worden toegewezen aan de *Microsoft.Network/networkInterfaces/effectiveRouteTable/action* bewerking voor de netwerkinterface. Zie voor meer bewerkingen toewijzen aan accounts [aangepaste rollen maken voor op rollen gebaseerd toegangsbeheer](../active-directory/role-based-access-control-custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#actions).
2. De volgende opdracht retourneert alle routes die worden toegepast op een netwerkinterface met de naam *VM1 NIC1* in de resourcegroep *RG1*.
   
       Get-AzureRmEffectiveRouteTable -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1
   
   > [!TIP]
   > Als u de naam van een netwerkinterface niet weet, typ de volgende opdracht voor het ophalen van de namen van alle netwerkinterfaces in een group.* resource
   > 
   > 
   
       Get-AzureRmNetworkInterface -ResourceGroupName RG1 | Format-Table Name
   
   De volgende uitvoer lijkt op de uitvoer voor elke toegepast op het subnet dat de NIC is verbonden met route:
   
       Name :
       State : Active
       AddressPrefix : {10.9.0.0/16}
       NextHopType : VNetLocal
       NextHopIpAddress : {}
   
       Name :
       State : Active
       AddressPrefix : {0.0.0.0/16}
       NextHopType : Internet
       NextHopIpAddress : {}
   
   U ziet het volgende in de uitvoer:
   
   * **Naam**: naam van de effectieve route mag niet leeg zijn, tenzij expliciet is opgegeven voor de gebruiker gedefinieerde routes. 
   * **Status**: geeft de status van de effectieve route. Mogelijke waarden zijn 'Active' of 'Ongeldig'
   * **AddressPrefixes**: Hiermee geeft u het adresvoorvoegsel van de effectieve route in CIDR-notatie. 
   * **nextHopType**: Hiermee geeft u de volgende hop voor de opgegeven route. Mogelijke waarden zijn *VirtualAppliance*, *Internet*, *VNetLocal*, *VNetPeering*, of *Null*. Een waarde van *Null* voor **nextHopType** een UDR kan duiden op een ongeldige route. Bijvoorbeeld, als **nextHopType** is *VirtualAppliance* en het netwerk van een virtueel apparaat VM niet in een status ingericht/uitvoeren is. Als **nextHopType** is *VPNGateway* en er is geen gateway ingericht/uitvoeren in het opgegeven VNet de route ongeldig geworden.
   * **NextHopIpAddress**: Hiermee geeft u het IP-adres van de volgende hop van de effectieve route.
   
   De volgende opdracht retourneert de routes in een eenvoudiger tabel weergeven:
   
       Get-AzureRmEffectiveRouteTable -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1 | Format-Table
   
   De volgende uitvoer, is enkele van de uitvoer ontvangen voor het scenario zoals eerder beschreven:
   
       Name State AddressPrefix NextHopType NextHopIpAddress
       ---- ----- ------------- ----------- ----------------
       Active {10.9.0.0/16} VnetLocal {}
       Active {0.0.0.0/0} Internet {}
3. Er is geen route worden vermeld om het *WestUS VNet3* VNet (10.10.0.0/16)** van het voorvoegsel *WestUS VNet1* (voorvoegsel 10.9.0.0/16) in de uitvoer van de vorige stap. Zoals u in de volgende afbeelding, de VNet-peering te koppelen met de *WestUS VNet3* VNet is in de *verbroken* status.
   
    ![](./media/virtual-network-routes-troubleshoot-portal/image4.png)
   
    De koppeling in twee richtingen voor de peering verbroken wordt, waarin wordt uitgelegd waarom VM1 kan geen verbinding maken met VM3 in de *WestUS VNet3* VNet. Instellen van een bidirectionele VNet-peeringkoppeling opnieuw voor *WestUS VNet1* en *WestUS VNet3* VNets. Hier volgt de uitvoer die wordt geretourneerd nadat de VNet-peering koppeling correct is ingesteld:
   
        Name State AddressPrefix NextHopType NextHopIpAddress
        ---- ----- ------------- ----------- ----------------
        Active {10.9.0.0/16} VnetLocal {}
        Active {10.10.0.0/16} VNetPeering {}
        Active {0.0.0.0/0} Internet {}
   
    Zodra u hebt vastgesteld dat het probleem, u kunt toevoegen, verwijderen, of wijzig routes en routetabellen. Typ de volgende opdracht om een lijst weer van de opdrachten om dit te doen:
   
        Get-Help *-AzureRmRouteConfig

## <a name="considerations"></a>Overwegingen
Een aantal dingen rekening moet houden bij het controleren van de lijst met routes geretourneerd:

* Routering is gebaseerd op langste voorvoegsel overeen (LPM) tussen udr's, systeem- en BGP-routes. Als er meer dan één route met dezelfde overeenkomende LPM is, klikt u vervolgens een route geselecteerd op basis van de oorsprong in de volgende volgorde:
  
  * Door de gebruiker gedefinieerde route
  * BGP-route
  * Systeemroute (standaard)
    
    U kunt alleen effectieve routes die overeenkomende LPM op basis van alle routes in de beschikbaar zijn met effectieve routes zien. Door te laten zien hoe de routes daadwerkelijk wordt geëvalueerd voor een bepaalde NIC, hierdoor veel eenvoudiger om op te lossen specifieke routes die mogelijk van invloed op de connectiviteit van uw virtuele machine.
* Als u udr's hebt en verkeer naar een virtueel apparaat voor netwerk (NVA) met verzendt *VirtualAppliance* als **nextHopType**, zorg dat doorsturen via IP is ingeschakeld op de ontvangst van het verkeer NVA of pakketten verloren gaan. 
* Als geforceerde tunneling is ingeschakeld, worden alle uitgaand internetverkeer wordt gerouteerd naar on-premises. RDP/SSH van Internet naar uw virtuele machine werkt mogelijk niet met deze instelling kan, afhankelijk van hoe deze verkeer in de on-premises worden verwerkt. 
  Geforceerde tunneling kan worden ingeschakeld:
  * Als u site-naar-site VPN, door een door de gebruiker opgegeven routes (UDR) met nextHopType als VPN-Gateway
  * Als een standaardroute wordt geadverteerd via BGP
* Voor VNet-peering verkeer correct te laten werken, een systeemroute met **nextHopType** *VNetPeering* voor het peered VNet-voorvoegsel bereik moet bestaan. Als deze route bestaat niet en de VNet-peering koppeling OK lijkt:
  * Wacht een paar seconden en probeer het opnieuw als het een nieuw opgezette peeringkoppeling. Tijd tot tijd duurt langer doorgeven routes naar alle netwerkinterfaces in een subnet.
  * Netwerkbeveiligingsgroep (NSG) regels mogelijk van invloed op de verkeersstromen. Zie voor meer informatie de [Netwerkbeveiligingsgroepen oplossen](virtual-network-nsg-troubleshoot-powershell.md) artikel.

