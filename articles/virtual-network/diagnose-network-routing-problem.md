---
title: Een virtuele machine van Azure-routeringsprobleem vaststellen | Microsoft Docs
description: Leer hoe u een VM-routeringsprobleem vaststellen door de effectieve routes voor een virtuele machine weer te geven.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/30/2018
ms.author: jdial
ms.openlocfilehash: 56dd13f5c2c0db4af65d8bc5d4ee5c072a161964
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54429631"
---
# <a name="diagnose-a-virtual-machine-routing-problem"></a>Een VM-routeringsprobleem vaststellen

In dit artikel leert u hoe u een routeringsprobleem vaststellen door de routes die voor een netwerkinterface in een virtuele machine (VM gelden) weer te geven. Azure maakt verschillende standaardroutes voor elk subnet van het virtuele netwerk. U kunt de standaardroutes van Azure overschrijven door het definiëren van routes in een routetabel, en vervolgens koppelt u de routetabel aan een subnet. De combinatie van routes die u maakt, standaardroutes van Azure en alle routes doorgegeven van uw on-premises netwerk via een Azure VPN-gateway (als het virtuele netwerk is verbonden met uw on-premises netwerk) via het border gateway protocol (BGP), zijn de effectieve routes voor alle netwerkinterfaces in een subnet. Als u niet bekend met virtuele netwerken, netwerk-interface of Routering van concepten bent, Zie [overzicht van Virtual network](virtual-networks-overview.md), [netwerkinterface](virtual-network-network-interface.md), en [routeringoverzicht](virtual-networks-udr-overview.md).

## <a name="scenario"></a>Scenario

U probeert verbinding maken met een virtuele machine, maar de verbinding is verbroken. Om te bepalen waarom u geen verbinding maken met de virtuele machine, kunt u de effectieve routes voor een netwerkinterface met behulp van Azure weergeven [portal](#diagnose-using-azure-portal), [PowerShell](#diagnose-using-powershell), of de [Azure CLI](#diagnose-using-azure-cli).

De volgende stappen wordt ervan uitgegaan dat u hebt een bestaande virtuele machine om de effectieve routes voor weer te geven. Als u een bestaande virtuele machine hebt, implementeert eerst een [Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) of [Windows](../virtual-machines/windows/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) virtuele machine om de taken in dit artikel. De voorbeelden in dit artikel zijn van een virtuele machine met de naam *myVM* met een netwerkinterface met de naam *myVMVMNic*. De virtuele machine en een netwerkinterface zich in een resourcegroep met de naam *myResourceGroup*, en zijn de *VS-Oost* regio. Wijzig de waarden in de stappen zo nodig voor de virtuele machine die u bij het vaststellen van het probleem voor.

## <a name="diagnose-using-azure-portal"></a>Vaststellen met behulp van Azure portal

1. Meld u aan bij de Azure [portal](https://portal.azure.com) met een Azure-account waarvoor de [benodigde machtigingen](virtual-network-network-interface.md#permissions).
2. Aan de bovenkant van de Azure-portal, voer de naam van een virtuele machine die in de status running doorbrengt, in het zoekvak in. Wanneer de naam van de virtuele machine wordt weergegeven in de lijst met zoekresultaten, selecteert u deze.
3. Selecteer **vaststellen en oplossen van problemen met**, en klik vervolgens onder **aanbevolen stappen**, selecteer **effectieve routes** item in 7, zoals wordt weergegeven in de volgende afbeelding:

    ![Effectieve routes weergeven](./media/diagnose-network-routing-problem/view-effective-routes.png)

4. De effectieve routes voor een netwerkinterface met de naam **myVMVMNic** worden weergegeven in de volgende afbeelding:

     ![Effectieve routes weergeven](./media/diagnose-network-routing-problem/effective-routes.png)

    Als er meerdere netwerkinterfaces die zijn gekoppeld aan de virtuele machine, kunt u de effectieve routes voor elke netwerkinterface weergeven door deze te selecteren. Omdat elke netwerkinterface kan zich in een ander subnet, kan elke netwerkinterface verschillende effectieve routes hebben.

    In het voorbeeld in de vorige afbeelding wordt weergegeven, zijn de routes in de lijst standaardroutes die Azure voor elk subnet maakt. De lijst met kan ten minste deze routes heeft, maar aanvullende routes, afhankelijk van de mogelijkheden die u mogelijk hebt ingeschakeld voor het virtuele netwerk, zoals deze wordt gekoppeld aan een ander virtueel netwerk of verbonden met uw on-premises netwerk via een Azure VPN-gateway. Zie voor meer informatie over elk van de routes en andere routes u voor de netwerkinterface ziet mogelijk, [routering van verkeer van virtuele netwerken](virtual-networks-udr-overview.md). Als uw lijst een groot aantal routes bevat, wellicht vindt u het eenvoudiger om te selecteren **downloaden**om te downloaden van een CSV-bestand met de lijst van routes.

Hoewel de effectieve routes via de virtuele machine in de vorige stappen zijn bekeken, kunt u ook effectieve routes via weergeven een:
- **Afzonderlijke netwerkinterfaces**: Meer informatie over het [weergeven van een netwerkinterface](virtual-network-network-interface.md#view-network-interface-settings).
- **Afzonderlijke routetabel**: Meer informatie over het [weergeven van een routetabel](manage-route-table.md#view-details-of-a-route-table).

## <a name="diagnose-using-powershell"></a>Vaststellen met behulp van PowerShell

U kunt de opdrachten die volgen in uitvoeren de [Azure Cloud Shell](https://shell.azure.com/powershell), of door te voeren PowerShell vanaf uw computer. De Azure Cloud Shell is een gratis interactieve shell. In deze shell zijn algemene Azure-hulpprogramma's vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. Als u PowerShell vanaf uw computer uitvoeren, moet u de *AzureRM* PowerShell-module, versie 6.0.1 of hoger. Voer `Get-Module -ListAvailable AzureRM` op uw computer, de geïnstalleerde versie te vinden. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/azurerm/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook om uit te voeren `Login-AzureRmAccount` aan te melden bij Azure met een account met de [benodigde machtigingen](virtual-network-network-interface.md#permissions).

De effectieve routes ophalen voor een netwerkinterface met [Get-AzureRmEffectiveRouteTable](/powershell/module/azurerm.network/get-azurermeffectiveroutetable). Het volgende voorbeeld wordt de effectieve routes voor een netwerkinterface met de naam *myVMVMNic*, dat wil zeggen in een resourcegroep met de naam *myResourceGroup*:

```azurepowershell-interactive
Get-AzureRmEffectiveRouteTable `
  -NetworkInterfaceName myVMVMNic `
  -ResourceGroupName myResourceGroup `
  | Format-Table
```

Zie voor meer informatie over de informatie die in de uitvoer is geretourneerd, [routeringoverzicht](virtual-networks-udr-overview.md). Uitvoer wordt alleen geretourneerd als de virtuele machine in de status running doorbrengt is. Als er meerdere netwerkinterfaces die zijn gekoppeld aan de virtuele machine, kunt u de effectieve routes voor elke netwerkinterface kunt bekijken. Omdat elke netwerkinterface kan zich in een ander subnet, kan elke netwerkinterface verschillende effectieve routes hebben. Als u steeds een communicatieprobleem ondervindt nog, raadpleegt u [extra diagnose](#additional-diagnosis) en [overwegingen met betrekking tot](#considerations).

Als u de naam van een netwerkinterface niet weet, maar de naam van de virtuele machine de netwerkinterface is gekoppeld aan, retourneren de volgende opdrachten de id's van alle netwerkinterfaces die zijn gekoppeld aan een virtuele machine:

```azurepowershell-interactive
$VM = Get-AzureRmVM -Name myVM `
  -ResourceGroupName myResourceGroup
$VM.NetworkProfile
```

Ontvangt u uitvoer die vergelijkbaar is met het volgende voorbeeld:

```powershell
NetworkInterfaces
-----------------
{/subscriptions/<ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myVMVMNic
```

In de uitvoer van de vorige naam van de netwerk-interface is *myVMVMNic*.

## <a name="diagnose-using-azure-cli"></a>Vaststellen met behulp van Azure CLI

U kunt de opdrachten die volgen in uitvoeren de [Azure Cloud Shell](https://shell.azure.com/bash), of door het uitvoeren van de CLI van de computer. In dit artikel gebruikmaken van Azure CLI versie 2.0.32 of hoger. Voer `az --version` uit om te kijken welke versie is geïnstalleerd. Zie [Azure CLI installeren](/cli/azure/install-azure-cli) als u de CLI wilt installeren of een upgrade wilt uitvoeren. Als u de Azure CLI lokaal uitvoert, moet u ook om uit te voeren `az login` en meld u aan bij Azure met een account met de [benodigde machtigingen](virtual-network-network-interface.md#permissions).

De effectieve routes ophalen voor een netwerkinterface met [az network nic show-effectief-route-table](/cli/azure/network/nic#az-network-nic-show-effective-route-table). Het volgende voorbeeld wordt de effectieve routes voor een netwerkinterface met de naam *myVMVMNic* die zich in een resourcegroep met de naam *myResourceGroup*:

```azurecli-interactive
az network nic show-effective-route-table \
  --name myVMVMNic \
  --resource-group myResourceGroup
```

Zie voor meer informatie over de informatie die in de uitvoer is geretourneerd, [routeringoverzicht](virtual-networks-udr-overview.md). Uitvoer wordt alleen geretourneerd als de virtuele machine in de status running doorbrengt is. Als er meerdere netwerkinterfaces die zijn gekoppeld aan de virtuele machine, kunt u de effectieve routes voor elke netwerkinterface kunt bekijken. Omdat elke netwerkinterface kan zich in een ander subnet, kan elke netwerkinterface verschillende effectieve routes hebben. Als u steeds een communicatieprobleem ondervindt nog, raadpleegt u [extra diagnose](#additional-diagnosis) en [overwegingen met betrekking tot](#considerations).

Als u de naam van een netwerkinterface niet weet, maar de naam van de virtuele machine de netwerkinterface is gekoppeld aan, retourneren de volgende opdrachten de id's van alle netwerkinterfaces die zijn gekoppeld aan een virtuele machine:

```azurecli-interactive
az vm show \
  --name myVM \
  --resource-group myResourceGroup
```

## <a name="resolve-a-problem"></a>Problemen op te lossen

Het oplossen van problemen met de routering doorgaans bestaat uit:

- Toevoegen van een aangepaste route voor het overschrijven van een van de standaardroutes van Azure. Meer informatie over het [toevoegen van een aangepaste route](manage-route-table.md#create-a-route).
- Wijzig of verwijder de aangepaste route dat kan leiden tot routering met een ongewenst locatie. Meer informatie over het [wijzigen](manage-route-table.md#change-a-route) of [verwijderen](manage-route-table.md#delete-a-route) een aangepaste route.
- Ervoor te zorgen dat de routetabel waarin eventuele aangepaste routes bevat die u hebt gedefinieerd is gekoppeld aan het subnet dat de netwerkinterface zich bevindt. Meer informatie over het [een routetabel aan een subnet koppelen](manage-route-table.md#associate-a-route-table-to-a-subnet).
- Zorg ervoor dat apparaten, zoals Azure VPN-gateway of netwerk virtuele apparaten die u hebt geïmplementeerd bediend worden. Gebruik de [diagnostische gegevens van VPN](../network-watcher/diagnose-communication-problem-between-networks.md?toc=%2fazure%2fvirtual-network%2ftoc.json) mogelijkheden van Network Watcher om eventuele problemen met een Azure VPN-gateway te bepalen.

Als u nog steeds communicatieproblemen ondervindt, raadpleegt u [overwegingen met betrekking tot](#considerations) en [extra diagnose](#additional-dignosis).

## <a name="considerations"></a>Overwegingen

Houd rekening met de volgende punten bij het oplossen van communicatieproblemen met:

- Routering is gebaseerd op de langste voorvoegselovereenkomst (LPM longest prefix) tussen de routes die u hebt gedefinieerd, border gateway protocol (BGP) en system routes. Als er meer dan één route met dezelfde overeenkomende LPM is, wordt een route geselecteerd op basis van de oorsprong in de volgorde [routeringoverzicht](virtual-networks-udr-overview.md#how-azure-selects-a-route). Met effectieve routes, kunt u effectieve routes die een overeenkomende LPM, op basis van de beschikbare routes worden alleen zien. Zien hoe de routes voor een netwerkinterface worden geëvalueerd, maakt het veel eenvoudiger om op te lossen specifieke routes die mogelijk van invloed op communicatie van de virtuele machine.
- Als u aangepaste routes naar een virtueel netwerkapparaat (NVA) hebt gedefinieerd met *virtueel apparaat* als het volgende hoptype, zorg ervoor dat doorsturen via IP is ingeschakeld op de NVA die het verkeer ontvangen, of worden pakketten verwijderd. Meer informatie over [doorsturen via IP in voor een netwerkinterface](virtual-network-network-interface.md#enable-or-disable-ip-forwarding). Het besturingssysteem of toepassing in de NVA ook moet bovendien kunnen netwerkverkeer doorsturen en worden geconfigureerd om dit te doen.
- Als u een route naar 0.0.0.0/0 hebt gemaakt, wordt al het uitgaande internetverkeer wordt doorgestuurd naar de volgende hop die u hebt opgegeven, bijvoorbeeld bij een NVA- of VPN-gateway. Het maken van een route wordt vaak aangeduid als geforceerde tunneling. Externe verbindingen met behulp van de protocollen RDP of SSH van internet met uw virtuele machine werkt mogelijk niet met deze route, afhankelijk van hoe de volgende hop het verkeer verwerkt. Geforceerde tunneling kan worden ingeschakeld:
    - Bij het gebruik van site-naar-site VPN, door het maken van een route met een volgend hoptype van *VPN-Gateway*. Meer informatie over [configureren van geforceerde tunneling](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
    - Als een 0.0.0.0/0 (standaardroute) via de gateway van een virtueel netwerk via BGP wordt aangekondigd bij het gebruik van een site-naar-site-VPN of ExpressRoute-circuit. Meer informatie over het gebruik van BGP met een [site-naar-site VPN](../vpn-gateway/vpn-gateway-bgp-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) of [ExpressRoute](../expressroute/expressroute-routing.md?toc=%2fazure%2fvirtual-network%2ftoc.json#ip-addresses-used-for-azure-private-peering).
- Voor peering verkeer in virtuele netwerken correct te laten werken, een systeemroute met een volgend hoptype van *VNet-Peering* voor het gekoppelde virtuele netwerk voorvoegsel bereik moet bestaan. Als deze een route bestaat niet en het virtuele netwerk-peeringkoppeling is **verbonden**:
    - Wacht een paar seconden en probeer het opnieuw. Als het een nieuw tot stand gebrachte peeringkoppeling, duurt soms het langer te propageren van routes op alle netwerkinterfaces in een subnet. Zie voor meer informatie over virtueel-netwerkpeering [overzicht van Virtual network-peering](virtual-network-peering-overview.md) en [peering in virtuele netwerken beheren](virtual-network-manage-peering.md).
    - Regels voor netwerkbeveiligingsgroepen kunnen van invloed zijn op communicatie. Zie voor meer informatie, [een probleem VM-netwerk-verkeersfilter vaststellen](diagnose-network-traffic-filter-problem.md).
- Hoewel Azure wijst standaardroutes toe aan elke interface Azure-netwerk, hebt u meerdere netwerkinterfaces die zijn gekoppeld aan de virtuele machine, worden alleen de primaire netwerkinterface wordt toegewezen een standaardroute (0.0.0.0/0) of een gateway in het besturingssysteem van de virtuele machine. Informatie over het maken van een standaardroute voor secundaire netwerkinterfaces die zijn gekoppeld aan een [Windows](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) of [Linux](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) VM. Meer informatie over [primaire en secundaire netwerkinterfaces](virtual-network-network-interface-vm.md#constraints).

## <a name="additional-diagnosis"></a>Aanvullende diagnose

* Om uit te voeren kort te testen om te bepalen van het type van de volgende hop voor verkeer dat bestemd is voor een locatie, gebruikt u de [volgende hop](../network-watcher/diagnose-vm-network-routing-problem.md?toc=%2fazure%2fvirtual-network%2ftoc.json) mogelijkheden van Azure Network Watcher. Volgende hop ziet u het volgende hoptype is bedoeld voor verkeer dat bestemd is voor een opgegeven locatie.
* Als er geen routes van een virtuele machine de netwerkcommunicatie mislukken veroorzaakt, kan het probleem worden veroorzaakt door firewallsoftware die wordt uitgevoerd in het besturingssysteem van de virtuele machine
* Als u [geforceerde tunneling](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md?toc=%2fazure%2fvirtual-network%2ftoc.json) verkeer naar een on-premises apparaat via een VPN-gateway of NVA, u wellicht geen verbinding maken met een virtuele machine vanaf het internet, afhankelijk van hoe u routering voor de apparaten hebt geconfigureerd. Bevestig dat de omleiding die u hebt geconfigureerd voor het apparaat verkeer naar ofwel een openbaar of privé IP-adres voor de virtuele machine routeert.
* Gebruik de [probleemoplossing voor verbindingen](../network-watcher/network-watcher-connectivity-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) mogelijkheden van Network Watcher om te bepalen van de routering, filteren en in-OS oorzaken van problemen met uitgaande communicatie.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over alle taken, eigenschappen en instellingen voor een [tabel en routes routeren](manage-route-table.md).
- Meer informatie over alle [typen, systeemroutes en hoe Azure een route selecteert van volgende hop](virtual-networks-udr-overview.md).
