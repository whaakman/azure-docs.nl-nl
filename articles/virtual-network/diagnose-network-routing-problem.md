---
title: Diagnose van een virtuele machine van Azure-routeringsprobleem | Microsoft Docs
description: Informatie over het probleem op te sporen een virtuele machine routering door de effectieve routes voor een virtuele machine weer te geven.
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
ms.openlocfilehash: 07352a5d7c8b465440efab17c654979662a95f8e
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34702361"
---
# <a name="diagnose-a-virtual-machine-routing-problem"></a>Een virtuele machine routeringsprobleem onderzoeken

In dit artikel leert u het probleem op te sporen een routeringsdomein door de routes die geschikt is voor een netwerkinterface in een virtuele machine (VM) weer te geven. Azure maakt verschillende standaardroutes voor elk subnet van het virtuele netwerk. U kunt Azure standaardroutes overschrijven definiëren van routes in een routetabel en vervolgens de routetabel aan een subnet te koppelen. De combinatie van routes die u maakt, Azure standaardroutes en eventuele routes doorgegeven vanuit uw on-premises netwerk via een Azure VPN-gateway (als het virtuele netwerk is verbonden met uw on-premises netwerk) via het border gateway protocol (BGP), zijn de effectieve routes van alle netwerkinterfaces in een subnet. Als u niet bekend bent met het virtuele netwerk, netwerkinterface of routing concepten, Zie [Virtual network-overzicht](virtual-networks-overview.md), [netwerkinterface](virtual-network-network-interface.md), en [Routering-overzicht](virtual-networks-udr-overview.md).

## <a name="scenario"></a>Scenario

U probeert verbinding maken met een virtuele machine, maar de verbinding is verbroken. U kunt de effectieve routes voor een netwerkinterface met de Azure bekijken om te bepalen waarom u geen verbinding maken met de virtuele machine, [portal](#diagnose-using-azure-portal), [PowerShell](#diagnose-using-powershell), of de [Azure CLI](#diagnose-using-azure-cli).

Welke stappen volgen wordt ervan uitgegaan dat u hebt een bestaande virtuele machine om de effectieve routes voor weer te geven. Als u een bestaande virtuele machine hebt, implementeert eerst een [Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) of [Windows](../virtual-machines/windows/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) VM de taken in dit artikel met voltooien. De voorbeelden in dit artikel zijn van een virtuele machine met de naam *myVM* met een netwerkinterface met de naam *myVMVMNic*. De virtuele machine en een netwerkinterface zijn in een resourcegroep met de naam *myResourceGroup*, en in de *VS-Oost* regio. Wijzig de waarden in de stappen, afhankelijk van de virtuele machine die u bij het vaststellen van het probleem voor.

## <a name="diagnose-using-azure-portal"></a>Analyseren met Azure portal

1. Meld u aan bij de Azure [portal](https://portal.azure.com) met een Azure-account met de [noodzakelijke machtigingen](virtual-network-network-interface.md#permissions).
2. Voer de naam van een virtuele machine die zich in de actieve status, in het zoekvak aan de bovenkant van de Azure portal. Wanneer de naam van de virtuele machine wordt weergegeven in de zoekresultaten, selecteert u deze.
3. Selecteer **diagnosticeren en oplossen van problemen**, en klik vervolgens onder **aanbevolen stappen**, selecteer **effectieve routes** in punt 7, zoals wordt weergegeven in de volgende afbeelding:

    ![De effectieve routes weergeven](./media/diagnose-network-routing-problem/view-effective-routes.png)

4. Voor een netwerkinterface met de naam van de effectieve routes **myVMVMNic** worden weergegeven in de volgende afbeelding:

     ![De effectieve routes weergeven](./media/diagnose-network-routing-problem/effective-routes.png)

    Als er meerdere netwerkinterfaces die zijn gekoppeld aan de virtuele machine, kunt u de effectieve routes voor een netwerkinterface weergeven door deze te selecteren. Omdat elke netwerkinterface kan in een ander subnet, kan elke netwerkinterface verschillende effectieve routes hebben.

    In het voorbeeld in de vorige afbeelding, zijn de routes in de lijst standaardroutes die Azure voor elk subnet maakt. Uw lijst met ten minste deze routes heeft, maar mogelijk aanvullende routes, afhankelijk van de mogelijkheden die u mogelijk hebt ingeschakeld voor het virtuele netwerk zoals deze wordt gekoppeld aan een ander virtueel netwerk of verbonden met uw on-premises netwerk via een Azure VPN-gateway. Zie voor meer informatie over elk van de routes en andere deze routes u voor de netwerkinterface ziet mogelijk, [virtuele netwerk voor verkeersroutering](virtual-networks-udr-overview.md). Als uw lijst een groot aantal routes bevat, wellicht vindt u het gemakkelijker om te selecteren **downloaden**, voor het downloaden van een CSV-bestand met de lijst met routes.

Hoewel effectieve routes zijn weergegeven via de virtuele machine in de vorige stappen, kunt u ook weergeven effectieve routes via een:
- **Afzonderlijke netwerkinterfaces**: meer informatie over hoe [weergeven van een netwerkinterface](virtual-network-network-interface.md#view-network-interface-settings).
- **Afzonderlijke routetabel**: meer informatie over hoe [weergeven van een routetabel](manage-route-table.md#view-details-of-a-route-table).

## <a name="diagnose-using-powershell"></a>Analyseren met behulp van PowerShell

U kunt de volgende opdrachten in uitvoeren de [Azure Cloud Shell](https://shell.azure.com/powershell), of door te voeren PowerShell vanaf uw computer. De Azure-Cloud-Shell is een gratis interactieve shell. In deze shell zijn algemene Azure-hulpprogramma's vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. Als u PowerShell vanaf uw computer uitvoeren, moet u de *AzureRM* PowerShell-module, versie 6.0.1 of hoger. Voer `Get-Module -ListAvailable AzureRM` op uw computer, de geïnstalleerde versie vinden. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook uitvoeren `Login-AzureRmAccount` aan te melden bij Azure met een account met de [noodzakelijke machtigingen](virtual-network-network-interface.md#permissions).

De effectieve routes ophalen voor een netwerkinterface met [Get-AzureRmEffectiveRouteTable](/powershell/module/azurerm.network/get-azurermeffectiveroutetable). Het volgende voorbeeld wordt de effectieve routes voor een netwerkinterface met de naam *myVMVMNic*, die in een resourcegroep met de naam *myResourceGroup*:

```azurepowershell-interactive
Get-AzureRmEffectiveRouteTable `
  -NetworkInterfaceName myVMVMNic `
  -ResourceGroupName myResourceGroup `
  | Format-Table
```

Zie voor informatie over de informatie in de uitvoer geretourneerd, [Routering-overzicht](virtual-networks-udr-overview.md). Uitvoer wordt alleen geretourneerd als de virtuele machine uitgevoerd wordt. Als er meerdere netwerkinterfaces die zijn gekoppeld aan de virtuele machine, kunt u de effectieve routes voor elke netwerkinterface bekijken. Omdat elke netwerkinterface kan in een ander subnet, kan elke netwerkinterface verschillende effectieve routes hebben. Als u steeds een communicatieprobleem ondervindt nog, raadpleegt u [extra diagnose](#additional-diagnosis) en [overwegingen](#considerations).

Als u de naam van een netwerkinterface niet kent maar de naam van de virtuele machine weet aan de netwerkinterface is gekoppeld, retourneren de volgende opdrachten in de id's van alle netwerkinterfaces die zijn gekoppeld aan een virtuele machine:

```azurepowershell-interactive
$VM = Get-AzureRmVM -Name myVM `
  -ResourceGroupName myResourceGroup
$VM.NetworkProfile
```

U ontvangt uitvoer ziet er als volgt uitzien:

```powershell
NetworkInterfaces
-----------------
{/subscriptions/<ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myVMVMNic
```

In de vorige uitvoer is de naam van de interface is *myVMVMNic*.

## <a name="diagnose-using-azure-cli"></a>Analyseren met Azure CLI

U kunt de volgende opdrachten in uitvoeren de [Azure Cloud Shell](https://shell.azure.com/bash), of door het uitvoeren van de CLI vanaf uw computer. Dit artikel is vereist voor de Azure CLI versie 2.0.32 of hoger. Voer `az --version` uit om te kijken welke versie is geïnstalleerd. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli). Als u de Azure CLI lokaal uitvoert, moet u ook uitvoeren `az login` en meld u aan bij Azure met een account met de [noodzakelijke machtigingen](virtual-network-network-interface.md#permissions).

De effectieve routes ophalen voor een netwerkinterface met [az netwerk nic weergeven-ingang-route-tabel](/cli/azure/network/nic#az-network-nic-show-effective-route-table). Het volgende voorbeeld wordt de effectieve routes voor een netwerkinterface met de naam *myVMVMNic* die deel uitmaakt van een resourcegroep met de naam *myResourceGroup*:

```azurecli-interactive
az network nic show-effective-route-table \
  --name myVMVMNic \
  --resource-group myResourceGroup
```

Zie voor informatie over de informatie in de uitvoer geretourneerd, [Routering-overzicht](virtual-networks-udr-overview.md). Uitvoer wordt alleen geretourneerd als de virtuele machine uitgevoerd wordt. Als er meerdere netwerkinterfaces die zijn gekoppeld aan de virtuele machine, kunt u de effectieve routes voor elke netwerkinterface bekijken. Omdat elke netwerkinterface kan in een ander subnet, kan elke netwerkinterface verschillende effectieve routes hebben. Als u steeds een communicatieprobleem ondervindt nog, raadpleegt u [extra diagnose](#additional-diagnosis) en [overwegingen](#considerations).

Als u de naam van een netwerkinterface niet kent maar de naam van de virtuele machine weet aan de netwerkinterface is gekoppeld, retourneren de volgende opdrachten in de id's van alle netwerkinterfaces die zijn gekoppeld aan een virtuele machine:

```azurecli-interactive
az vm show \
  --name myVM \
  --resource-group myResourceGroup
```

## <a name="resolve-a-problem"></a>Problemen op te lossen

Het oplossen van problemen met de routering doorgaans bestaat uit:

- Toevoegen van aangepaste routes u kunt een van de Azure standaardroutes overschrijven. Meer informatie over hoe [een aangepaste route toevoegen](manage-route-table.md#create-a-route).
- Wijzigen of verwijderen van een aangepaste route dat kan leiden tot routering naar een ongewenste locatie. Meer informatie over hoe [wijzigen](manage-route-table.md#change-a-route) of [verwijderen](manage-route-table.md#delete-a-route) een aangepaste route.
- Ervoor te zorgen dat de routetabel met eventuele aangepaste routes die u hebt gedefinieerd is gekoppeld aan het subnet met de netwerkinterface. Meer informatie over hoe [koppelen van een routetabel aan een subnet](manage-route-table.md#associate-a-route-table-to-a-subnet).
- Zorg ervoor dat apparaten zoals Azure VPN-gateway of netwerk virtuele apparaten die u hebt geïmplementeerd bediend worden. Gebruik de [diagnostische gegevens van VPN-](../network-watcher/diagnose-communication-problem-between-networks.md?toc=%2fazure%2fvirtual-network%2ftoc.json) mogelijkheid van netwerk-Watcher om te bepalen van eventuele problemen met een Azure VPN-gateway.

Als u steeds communicatieproblemen ondervindt nog, raadpleegt u [overwegingen](#considerations) en [extra diagnose](#additional-dignosis).

## <a name="considerations"></a>Overwegingen

Houd rekening met de volgende punten bij het oplossen van communicatieproblemen:

- Routering is gebaseerd op het langste voorvoegsel overeen (LPM) tussen routes dat u hebt gedefinieerd, border gateway protocol (BGP) en system routes. Als er meer dan één route met dezelfde overeenkomende LPM is, wordt een route geselecteerd op basis van de oorsprong in de volgorde in [Routering-overzicht](virtual-networks-udr-overview.md#how-azure-selects-a-route). U kunt alleen effectieve routes die een overeenkomende LPM, op basis van alle routes die beschikbaar zijn met effectieve routes zien. Zien hoe de routes worden geëvalueerd voor een netwerkinterface gemakkelijker veel oplossen van problemen met specifieke routes die mogelijk van invloed op communicatie van uw virtuele machine.
- Als u aangepaste routes naar een virtueel netwerkapparaat (NVA) hebt gedefinieerd met *virtueel apparaat* als het volgende hoptype, controleert u of doorsturen via IP is ingeschakeld op de ontvangst van het verkeer NVA of pakketten verloren gaan. Meer informatie over [doorsturen via IP voor een netwerkinterface inschakelen](virtual-network-network-interface.md#enable-or-disable-ip-forwarding). Daarnaast moet het besturingssysteem of toepassing binnen de NVA ook kunnen worden doorgestuurd netwerkverkeer en worden geconfigureerd om dit te doen.
- Als u een route naar 0.0.0.0/0 hebt gemaakt, worden alle uitgaand internetverkeer wordt doorgestuurd naar de volgende hop die u hebt opgegeven, zoals het NVA- of VPN-gateway. Deze route maken vaak aangeduid als geforceerde tunneling. Externe verbindingen met de RDP of SSH protocollen van het internet met uw virtuele machine werkt niet met deze route, afhankelijk van hoe het verkeer worden verwerkt door de volgende hop. Geforceerde tunneling kan worden ingeschakeld:
    - Wanneer u site-naar-site VPN, door te maken van een route met een volgend hoptype van *VPN-Gateway*. Meer informatie over [configureren geforceerde tunneling](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
    - Als een 0.0.0.0/0 (standaardroute) via een virtuele netwerkgateway via BGP wordt aangekondigd wanneer u een site-naar-site VPN- of ExpressRoute-circuit. Meer informatie over het gebruik van BGP met een [site-naar-site VPN](../vpn-gateway/vpn-gateway-bgp-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) of [ExpressRoute](../expressroute/expressroute-routing.md?toc=%2fazure%2fvirtual-network%2ftoc.json#ip-addresses-used-for-azure-private-peering).
- Voor virtuele-peering netwerkverkeer correct te laten werken, een systeemroute met een volgend hoptype van *VNet-Peering* peered van het virtuele netwerk voorvoegsel bereik moet bestaan. Als deze route bestaat niet en het virtuele netwerk-peeringkoppeling **verbonden**:
    - Wacht een paar seconden en probeer het opnieuw. Als het een nieuw opgezette peeringkoppeling, duurt soms langer doorgeven routes naar alle netwerkinterfaces in een subnet. Zie voor meer informatie over het virtuele netwerk peering, [peering Virtual network-overzicht](virtual-network-peering-overview.md) en [beheren met het virtuele netwerk peering](virtual-network-manage-peering.md).
    - Netwerkbeveiligingsgroepen mogelijk van invloed op communicatie. Zie voor meer informatie [onderzoeken van een virtuele machine verkeer filter netwerkprobleem](diagnose-network-traffic-filter-problem.md).
- Hoewel Azure standaardroutes op elke interface Azure-netwerk toewijst als u meerdere netwerkinterfaces die zijn gekoppeld aan de virtuele machine hebt, wordt de primaire netwerkinterface toegewezen een standaardroute (0.0.0.0/0) of de gateway in het besturingssysteem van de VM. Informatie over het maken van een standaardroute voor secundaire netwerkinterfaces die zijn gekoppeld aan een [Windows](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) of [Linux](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) VM. Meer informatie over [primaire en secundaire netwerkinterfaces](virtual-network-network-interface-vm.md#constraints).

## <a name="additional-diagnosis"></a>Aanvullende diagnose

* Gebruik voor het uitvoeren van een snelle test om te bepalen van het volgende hoptype voor verkeer dat is bestemd voor een locatie, de [volgende hop](../network-watcher/diagnose-vm-network-routing-problem.md?toc=%2fazure%2fvirtual-network%2ftoc.json) mogelijkheden van Azure-netwerk-Watcher. Volgende hop ziet u het volgende hoptype is voor verkeer dat is bestemd voor een opgegeven locatie.
* Als er geen netwerkcommunicatie van een virtuele machine mislukt waardoor routes, wordt het probleem mogelijk veroorzaakt door de firewall-software die in de VM-besturingssysteem worden uitgevoerd
* Als u [geforceerde tunneling](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md?toc=%2fazure%2fvirtual-network%2ftoc.json) verkeer naar een on-premises-apparaat via een VPN-gateway of NVA u niet mogelijk verbinding met een virtuele machine vanaf het internet, afhankelijk van hoe u routering voor de apparaten hebt geconfigureerd. Bevestig dat de routering die u hebt geconfigureerd voor het apparaat verkeer gerouteerd naar elk een openbare of particuliere IP-adres voor de virtuele machine.
* Gebruik de [verbinding oplossen](../network-watcher/network-watcher-connectivity-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) mogelijkheid van netwerk-Watcher routering filteren en -OS oorzaken van uitgaande communicatieproblemen vaststellen.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over alle taken, eigenschappen en instellingen voor een [doorsturen tabel en routes](manage-route-table.md).
- Meer informatie over alle [volgende hop typen, systeemroutes en hoe Azure een route geselecteerd](virtual-networks-udr-overview.md).
