---
title: Routes - Portal oplossen | Microsoft Docs
description: Informatie over het oplossen van problemen met routes in het Azure Resource Manager-implementatiemodel met de Azure-Portal.
services: virtual-network
documentationcenter: na
author: AnithaAdusumilli
manager: narayan
editor: 
tags: azure-resource-manager
ms.assetid: bdd8b6dc-02fb-4057-bb23-8289caa9de89
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/23/2016
ms.author: anithaa
ms.openlocfilehash: d9b951ad4c54a3714ba7e857d5198c351215cbac
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="troubleshoot-routes-using-the-azure-portal"></a>Routes met behulp van de Azure Portal oplossen
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

1. Meld u aan bij de Azure portal op https://portal.azure.com. Uw account moet worden toegewezen aan de *Microsoft.Network/networkInterfaces/effectiveRouteTable/action* bewerking voor de netwerkinterface. Zie voor meer bewerkingen toewijzen aan accounts [aangepaste rollen maken voor op rollen gebaseerd toegangsbeheer](../active-directory/role-based-access-control-custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#actions).
2. Klik op **meer services**, klikt u vervolgens op **virtuele machines** in de lijst die wordt weergegeven.
3. Selecteer een virtuele machine om op te lossen in de lijst die wordt weergegeven en een VM-blade met opties wordt weergegeven.
4. Klik op **spoor & oplossen van problemen met** en selecteer vervolgens een veelvoorkomend probleem. In dit voorbeeld **ik kan geen verbinding maken met mijn Windows-VM** is geselecteerd.

    ![](./media/virtual-network-routes-troubleshoot-portal/image1.png)
5. Stappen worden vermeld onder het probleem, zoals wordt weergegeven in de volgende afbeelding:

    ![](./media/virtual-network-routes-troubleshoot-portal/image2.png)

    Klik op *effectieve routes* in de lijst met aanbevolen stappen.
6. De **effectieve routes** blade wordt weergegeven, zoals wordt weergegeven in de volgende afbeelding:

    ![](./media/virtual-network-routes-troubleshoot-portal/image3.png)

    Als uw virtuele machine slechts één NIC heeft, is standaard geselecteerd. Als u meer dan één NIC hebt, selecteert u de NIC waarvoor u wilt weergeven van de effectieve routes.

   > [!NOTE]
   > Als de virtuele machine die is gekoppeld aan de NIC niet actief is is, wordt effectieve routes niet worden weergegeven. Alleen de eerste 200 effectieve routes worden weergegeven in de portal. Voor een volledige lijst, klikt u op **downloaden**. Verder kunt u filteren op de resultaten van het gedownloade CSV-bestand.
   >
   >

    U ziet het volgende in de uitvoer:

   * **Bron**: geeft het type van de route. Systeemroutes worden weergegeven als *standaard*, udr's worden weergegeven als *gebruiker* en routes gateway (statische of BGP) worden weergegeven als *VPNGateway*.
   * **Status**: geeft de status van de effectieve route. Mogelijke waarden zijn *Active* of *ongeldig*.
   * **AddressPrefixes**: Hiermee geeft u het adresvoorvoegsel van de effectieve route in CIDR-notatie.
   * **nextHopType**: Hiermee geeft u de volgende hop voor de opgegeven route. Mogelijke waarden zijn *VirtualAppliance*, *Internet*, *VNetLocal*, *VNetPeering*, of *Null*. Een waarde van *Null* voor **nextHopType** een UDR kan duiden op een ongeldige route. Bijvoorbeeld, als **nextHopType** is *VirtualAppliance* en het netwerk van een virtueel apparaat VM niet in een status ingericht/uitvoeren is. Als **nextHopType** is *VPNGateway* en er is geen gateway ingericht/uitvoeren in het opgegeven VNet de route ongeldig geworden.
7. Er is geen route worden vermeld om het *WestUS VNET3* VNet (10.10.0.0/16 voorvoegsel) van de *WestUS VNet1* (voorvoegsel 10.9.0.0/16) in de afbeelding in de vorige stap. In de volgende afbeelding, de peering koppeling wordt de *verbroken* status:

    ![](./media/virtual-network-routes-troubleshoot-portal/image4.png)

    De koppeling in twee richtingen voor de peering verbroken wordt, waarin wordt uitgelegd waarom VM1 kan geen verbinding maken met VM3 in de *WestUS VNet3* VNet.
8. De volgende afbeelding ziet de routes na het instellen van de peering in twee richtingen-koppeling:

    ![](./media/virtual-network-routes-troubleshoot-portal/image5.png)

Lees voor meer probleemoplossing scenario's voor evaluatie van geforceerde tunneling en route, de [overwegingen](virtual-network-routes-troubleshoot-portal.md#considerations) sectie van dit artikel.

### <a name="view-effective-routes-for-a-network-interface"></a>De effectieve routes weergeven voor een netwerkinterface
Als het netwerkverkeer is van invloed op voor een bepaalde netwerkinterface (NIC), kunt u een volledige lijst van effectieve routes rechtstreeks op een Netwerkinterfacekaart weergeven. Overzicht van de cumulatieve routes die worden toegepast op een NIC, moet u de volgende stappen uitvoeren:

1. Meld u aan bij de Azure portal op https://portal.azure.com.
2. Klik op **meer services**, klikt u vervolgens op **netwerkinterfaces**
3. Zoek in de lijst voor de naam van een NIC of Selecteer deze in de lijst die wordt weergegeven. In dit voorbeeld **VM1 NIC1** is geselecteerd.
4. Selecteer **effectieve routes** in de **netwerkinterface** blade, zoals wordt weergegeven in de volgende afbeelding:

       ![](./media/virtual-network-routes-troubleshoot-portal/image6.png)

    De **bereik** standaard ingesteld op de netwerkinterface die is geselecteerd.

      ![](./media/virtual-network-routes-troubleshoot-portal/image7.png)

### <a name="view-effective-routes-for-a-route-table"></a>Effectieve routes voor een routetabel weergeven
Wanneer de gebruiker gedefinieerde routes (udr's) wijzigen in een routetabel, is het raadzaam om te controleren van de impact van de routes die wordt toegevoegd aan een bepaalde virtuele machine. Een routetabel kan worden gekoppeld aan een willekeurig aantal subnetten. U kunt nu de effectieve routes weergeven voor de NIC's die een bepaalde routetabel is toegepast, zonder de context van de opgegeven route tabel blade overschakelen.

Bijvoorbeeld, een UDR (*UDRoute*) is opgegeven in een routetabel (*UDRouteTable*). Deze route verzendt alle internetverkeer van *Subnet1* in de *WestUS VNet1* VNet via een virtueel apparaat voor netwerk (NVA) in *Subnet2* van hetzelfde VNet. In de volgende afbeelding ziet u de route:

![](./media/virtual-network-routes-troubleshoot-portal/image8.png)

Overzicht van de cumulatieve routes voor een routetabel, moet u de volgende stappen uitvoeren:

1. Meld u aan bij de Azure portal op https://portal.azure.com.
2. Klik op **meer services**, klikt u vervolgens op **routetabellen**
3. Zoek in de lijst voor de routetabel die u wilt cumulatieve routes voor bekijken en deze te selecteren. In dit voorbeeld **UDRouteTable** is geselecteerd. Een blade voor de geselecteerde routetabel wordt weergegeven, zoals wordt weergegeven in de volgende afbeelding:

    ![](./media/virtual-network-routes-troubleshoot-portal/image9.png)
4. Selecteer **effectieve Routes** in de **routetabel** blade. De **bereik** is ingesteld op de routetabel die u hebt geselecteerd.
5. Een routetabel kan worden toegepast op meerdere subnetten. Selecteer de **Subnet** u wilt bekijken in de lijst. In dit voorbeeld **Subnet1** is geselecteerd.
6. Selecteer een **netwerkinterface**. Alle NIC's die zijn verbonden met het geselecteerde subnet worden weergegeven. In dit voorbeeld **VM1 NIC1** is geselecteerd.

    ![](./media/virtual-network-routes-troubleshoot-portal/image10.png)

   > [!NOTE]
   > Als de NIC niet gekoppeld aan een actieve virtuele machine is, worden geen effectieve routes weergegeven.
   >
   >

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
  * Netwerkbeveiligingsgroep (NSG) regels mogelijk van invloed op de verkeersstromen. Zie voor meer informatie de [Netwerkbeveiligingsgroepen oplossen](virtual-network-nsg-troubleshoot-portal.md) artikel.
