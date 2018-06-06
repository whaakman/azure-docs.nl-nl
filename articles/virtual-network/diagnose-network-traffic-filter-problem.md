---
title: Diagnose van een virtuele machine verkeer filter netwerkprobleem | Microsoft Docs
description: Informatie over het probleem op te sporen een virtuele machine netwerkverkeer filteren door de regels voor een effectieve beveiligingsmethode voor een virtuele machine weer te geven.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: a54feccf-0123-4e49-a743-eb8d0bdd1ebc
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/29/2018
ms.author: jdial
ms.openlocfilehash: 1c33a75363eec2b4e338ba64e3d1ad877d8b1610
ms.sourcegitcommit: 4f9fa86166b50e86cf089f31d85e16155b60559f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34757224"
---
# <a name="diagnose-a-virtual-machine-network-traffic-filter-problem"></a>Een virtuele machine verkeer filter netwerkprobleem onderzoeken

In dit artikel leert u hoe op te sporen een netwerkprobleem voor het filteren van verkeer door het netwerk (NSG) beveiligingsregels die geschikt is voor een virtuele machine (VM) weer te geven.

Nsg's kunnen u bepalen welke typen verkeer stromen naar en vanuit een virtuele machine. U kunt een NSG aan een subnet in een Azure-netwerk, een netwerkinterface gekoppeld aan een virtuele machine, of beide koppelen. De effectieve beveiligingsregels toegepast op een netwerkinterface zijn een aggregatie van de regels die zijn opgenomen in de NSG die is gekoppeld aan een netwerkinterface, en het subnet van de netwerkinterface. Regels in verschillende nsg's kunnen soms met elkaar conflicteren en invloed van een virtuele machine netwerkverbinding. U kunt de effectieve beveiligingsregels van nsg's die worden toegepast op de VM-netwerkinterfaces weergeven. Als u niet bekend bent met het virtuele netwerk, netwerkinterface of NSG concepten, Zie [Virtual network-overzicht](virtual-networks-overview.md), [netwerkinterface](virtual-network-network-interface.md), en [netwerk beveiligingsgroepen overzicht](security-overview.md).

## <a name="scenario"></a>Scenario

U probeert verbinding maken met een virtuele machine via poort 80 van het internet, maar de verbinding is verbroken. U kunt de regels voor een effectieve beveiligingsmethode voor een netwerkinterface met de Azure bekijken om te bepalen waarom u geen toegang tot poort 80 van het Internet, [portal](#diagnose-using-azure-portal), [PowerShell](#diagnose-using-powershell), of de [Azure CLI](#diagnose-using-azure-cli).

Welke stappen volgen wordt ervan uitgegaan dat u hebt een bestaande virtuele machine om de regels voor een effectieve beveiligingsmethode voor weer te geven. Als u een bestaande virtuele machine hebt, implementeert eerst een [Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) of [Windows](../virtual-machines/windows/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) VM de taken in dit artikel met voltooien. De voorbeelden in dit artikel zijn van een virtuele machine met de naam *myVM* met een netwerkinterface met de naam *myVMVMNic*. De virtuele machine en een netwerkinterface zijn in een resourcegroep met de naam *myResourceGroup*, en in de *VS-Oost* regio. Wijzig de waarden in de stappen, afhankelijk van de virtuele machine die u bij het vaststellen van het probleem voor.

## <a name="diagnose-using-azure-portal"></a>Analyseren met Azure portal

1. Meld u aan bij de Azure [portal](https://portal.azure.com) met een Azure-account met de [noodzakelijke machtigingen](virtual-network-network-interface.md#permissions).
2. Voer de naam van de virtuele machine in het zoekvak aan de bovenkant van de Azure portal. Wanneer de naam van de virtuele machine wordt weergegeven in de zoekresultaten, selecteert u deze.
3. Onder **instellingen**, selecteer **Networking**, zoals wordt weergegeven in de volgende afbeelding:

    ![Beveiligingsregels voor verbindingen weergeven](./media/diagnose-network-traffic-filter-problem/view-security-rules.png)

    De regels die u ziet in de afbeelding hierboven genoemde zijn voor een netwerkinterface met de naam **myVMVMNic**. U ziet dat er **binnenkomende POORTREGELS** voor de netwerkinterface van twee verschillende netwerkbeveiligingsgroepen:- **mySubnetNSG**: die aan het subnet dat door de netwerkinterface is gekoppeld.
        - **myVMNSG**: die zijn gekoppeld aan de netwerkinterface op de virtuele machine met de naam **myVMVMNic**.

    De regel voor licentiecontrole **DenyAllInBound** is wat houdt binnenkomende communicatie met de virtuele machine via poort 80, via internet, zoals beschreven in de [scenario](#scenario). De regel lijsten *0.0.0.0/0* voor **bron**, inclusief internet. Er zijn geen andere regel met een hogere prioriteit (lager nummer) kunnen poort 80 binnenkomend. Poort 80 zodat binnenkomende naar de virtuele machine vanaf het internet, Zie [problemen op te lossen](#resolve-a-problem). Zie voor meer informatie over beveiligingsregels en hoe Azure toegepast, [Netwerkbeveiligingsgroepen](security-overview.md).

    Aan de onderkant van de afbeelding ziet u ook **uitgaande POORTREGELS**. Onder die bestaan uit de uitgaande poortregels voor de netwerkinterface. Hoewel de afbeelding alleen vier regels voor binnenkomende verbindingen voor elke NSG toont, is uw nsg's mogelijk veel meer dan vier regels. In de afbeelding ziet u **VirtualNetwork** onder **bron** en **bestemming** en **AzureLoadBalancer** onder  **BRON**. **VirtualNetwork** en **AzureLoadBalancer** zijn [service labels](security-overview.md#service-tags). Service-tags vertegenwoordigt een groep van IP-adresvoorvoegsels om te helpen minimaliseren complexiteit voor het maken van de regel voor beveiliging.

4. Zorg ervoor dat de virtuele machine in de actieve status en selecteert u vervolgens **effectieve beveiligingsregels**, zoals weergegeven in de afbeelding hierboven om te zien van de regels voor een effectieve beveiligingsmethode, weergegeven in de volgende afbeelding:

    ![De effectieve beveiligingsregels weergeven](./media/diagnose-network-traffic-filter-problem/view-effective-security-rules.png)

    De regels die vermeld zijn dat dezelfde zoals u hebt gezien in stap 3, maar er verschillende tabbladen voor het NSG die is gekoppeld aan de netwerkinterface en het subnet zijn. Zoals u in de afbeelding ziet, worden alleen de eerste 50 regels worden weergegeven. Selecteer voor het downloaden van een CSV-bestand met alle regels **downloaden**.

    Om te zien die elke servicetag prefixes vertegenwoordigt, selecteert u een regel, zoals de regel voor licentiecontrole **AllowAzureLoadBalancerInbound**. De volgende afbeelding ziet u de voorvoegsels voor de **AzureLoadBalancer** service-tag:

    ![De effectieve beveiligingsregels weergeven](./media/diagnose-network-traffic-filter-problem/address-prefixes.png)

    Hoewel de **AzureLoadBalancer** servicetag alleen een voorvoegsel aangeeft, andere codes service vertegenwoordigen verschillende voorvoegsels.

4. De vorige stappen hebt u geleerd de beveiligingsregels voor een netwerkinterface met de naam **myVMVMNic**, maar u hebt ook een netwerkinterface met de naam gezien **myVMVMNic2** in sommige van de vorige afbeeldingen. De virtuele machine in dit voorbeeld heeft twee netwerkinterfaces die zijn gekoppeld. De effectieve beveiligingsregels kunnen afwijken voor elke netwerkinterface.

    Om te zien van de regels voor de **myVMVMNic2** netwerkinterface, selecteert u deze. Zoals u in de volgende afbeelding, de netwerkinterface heeft dezelfde regels die is gekoppeld aan het subnet als de **myVMVMNic** netwerkinterface, omdat beide netwerkinterfaces in hetzelfde subnet. Als u een NSG aan een subnet koppelt, worden de regels worden toegepast op alle netwerkinterfaces in het subnet.

    ![Beveiligingsregels voor verbindingen weergeven](./media/diagnose-network-traffic-filter-problem/view-security-rules2.png)

    In tegenstelling tot de **myVMVMNic** netwerkinterface, de **myVMVMNic2** netwerkinterface heeft geen een netwerkbeveiligingsgroep gekoppeld. Elke netwerkinterface en elk subnet kan nul, of een NSG gekoppeld. Het NSG die is gekoppeld aan elke netwerkinterface of subnet kan niet hetzelfde zijn, of een ander. Als u kiest, kunt u de dezelfde netwerkbeveiligingsgroep zoveel netwerkinterfaces en subnetten koppelen.

Hoewel een effectieve beveiligingsmethode regels zijn weergegeven via de virtuele machine, kunt u ook effectieve beveiligingsregels voor verbindingen via weergeven een:
- **Afzonderlijke netwerkinterfaces**: meer informatie over hoe [weergeven van een netwerkinterface](virtual-network-network-interface.md#view-network-interface-settings).
- **Afzonderlijke NSG**: meer informatie over hoe [weergeven van een NSG](manage-network-security-group.md#view-details-of-a-network-security-group).

## <a name="diagnose-using-powershell"></a>Analyseren met behulp van PowerShell

U kunt de volgende opdrachten in uitvoeren de [Azure Cloud Shell](https://shell.azure.com/powershell), of door te voeren PowerShell vanaf uw computer. De Azure-Cloud-Shell is een gratis interactieve shell. In deze shell zijn algemene Azure-hulpprogramma's vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. Als u PowerShell vanaf uw computer uitvoeren, moet u de *AzureRM* PowerShell-module, versie 6.0.1 of hoger. Voer `Get-Module -ListAvailable AzureRM` op uw computer, de geïnstalleerde versie vinden. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook uitvoeren `Login-AzureRmAccount` aan te melden bij Azure met een account met de [noodzakelijke machtigingen](virtual-network-network-interface.md#permissions)].

Ophalen van de regels voor een effectieve beveiligingsmethode voor een netwerkinterface met [Get-AzureRmEffectiveNetworkSecurityGroup](/powershell/module/azurerm.network/get-azurermeffectivenetworksecuritygroup). Het volgende voorbeeld wordt de regels voor een effectieve beveiligingsmethode voor een netwerkinterface met de naam *myVMVMNic*, die in een resourcegroep met de naam *myResourceGroup*:

```azurepowershell-interactive
Get-AzureRmEffectiveNetworkSecurityGroup `
  -NetworkInterfaceName myVMVMNic interface `
  -ResourceGroupName myResourceGroup
```

Uitvoer wordt geretourneerd als json-indeling. Zie inzicht in de uitvoer [interpreteren opdrachtuitvoer](#interpret-command-output).
Uitvoer wordt alleen geretourneerd als een NSG is gekoppeld aan de netwerkinterface, het subnet met de netwerkinterface of beide. De virtuele machine moet zich in de actieve status. Een virtuele machine kan meerdere netwerkinterfaces hebben met verschillende nsg's die zijn toegepast. Bij het oplossen, voert u de opdracht voor elke netwerkinterface.

Als u steeds een probleem ondervindt nog, raadpleegt u [extra diagnose](#additional-diagnosis) en [overwegingen](#considerations).

Als u de naam van een netwerkinterface niet kent maar de naam van de virtuele machine weet aan de netwerkinterface is gekoppeld, retourneren de volgende opdrachten in de id's van alle netwerkinterfaces die zijn gekoppeld aan een virtuele machine:

```azurepowershell-interactive
$VM = Get-AzureRmVM -Name myVM -ResourceGroupName myResourceGroup
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

Als u Azure-opdrachtregelinterface (CLI)-opdrachten voor het uitvoeren van taken in dit artikel, ofwel de opdrachten uitvoert in de [Azure Cloud Shell](https://shell.azure.com/bash), of door het uitvoeren van de CLI vanaf uw computer. Dit artikel is vereist voor de Azure CLI versie 2.0.32 of hoger. Voer `az --version` uit om te kijken welke versie is geïnstalleerd. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli). Als u de Azure CLI lokaal uitvoert, moet u ook uitvoeren `az login` en meld u aan bij Azure met een account met de [noodzakelijke machtigingen](virtual-network-network-interface.md#permissions).

Ophalen van de regels voor een effectieve beveiligingsmethode voor een netwerkinterface met [az netwerk nic lijst effectief nsg](/cli/azure/network/nic#az-network-nic-list-effective-nsg). Het volgende voorbeeld wordt de regels voor een effectieve beveiligingsmethode voor een netwerkinterface met de naam *myVMVMNic* die deel uitmaakt van een resourcegroep met de naam *myResourceGroup*:

```azurecli-interactive
az network nic list-effective-nsg \
  --name myVMVMNic \
  --resource-group myResourceGroup
```

Uitvoer wordt geretourneerd als json-indeling. Zie inzicht in de uitvoer [interpreteren opdrachtuitvoer](#interpret-command-output).
Uitvoer wordt alleen geretourneerd als een NSG is gekoppeld aan de netwerkinterface, het subnet met de netwerkinterface of beide. De virtuele machine moet zich in de actieve status. Een virtuele machine kan meerdere netwerkinterfaces hebben met verschillende nsg's die zijn toegepast. Bij het oplossen, voert u de opdracht voor elke netwerkinterface.

Als u steeds een probleem ondervindt nog, raadpleegt u [extra diagnose](#additional-diagnosis) en [overwegingen](#considerations).

Als u de naam van een netwerkinterface niet kent maar de naam van de virtuele machine weet aan de netwerkinterface is gekoppeld, retourneren de volgende opdrachten in de id's van alle netwerkinterfaces die zijn gekoppeld aan een virtuele machine:

```azurecli-interactive
az vm show \
  --name myVM \
  --resource-group myResourceGroup
```

Binnen de resulterende uitvoer ziet u de informatie is vergelijkbaar met het volgende voorbeeld:

```azurecli
"networkProfile": {
    "additionalProperties": {},
    "networkInterfaces": [
      {
        "additionalProperties": {},
        "id": "/subscriptions/<ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myVMVMNic",
        "primary": true,
        "resourceGroup": "myResourceGroup"
      },
```

In de vorige uitvoer is de naam van de interface is *myVMVMNic interface*.

## <a name="interpret-command-output"></a>Opdrachtuitvoer interpreteren

Ongeacht of u gebruikt de [PowerShell](#diangose-using-powershell), of de [Azure CLI](#diagnose-using-azure-cli) voor diagnose van het probleem wordt weergegeven met de volgende informatie:

- **NetworkSecurityGroup**: de ID van de netwerkbeveiligingsgroep.
- **Koppeling**: of de netwerkbeveiligingsgroep is gekoppeld aan een *NetworkInterface* of *Subnet*. Als een NSG gekoppeld aan beide is, wordt de uitvoer geretourneerd met **NetworkSecurityGroup**, **koppeling**, en **EffectiveSecurityRules**, voor elke NSG. Als de NSG is gekoppeld of ontkoppeld direct vóór de opdracht uit te voeren om de effectieve beveiligingsregels weer te geven, moet u wellicht wacht een paar seconden om de wijziging door te geven in de opdrachtuitvoer.
- **EffectiveSecurityRules**: een uitleg van elke eigenschap wordt beschreven in [een beveiligingsregel maken](manage-network-security-group.md#create-a-security-rule). Regel voor namen die vooraf worden gegaan door *defaultSecurityRules /* zijn standaard beveiligingsregels voor verbindingen die aanwezig zijn in elke NSG. Regel voor namen die vooraf worden gegaan door *securityRules /* zijn regels die u hebt gemaakt. Regels die specificeren een [service tag](security-overview.md#service-tags), zoals **Internet**, **VirtualNetwork**, en **AzureLoadBalancer** voor de  **destinationAddressPrefix** of **sourceAddressPrefix** eigenschappen, hebben ook waarden voor de **expandedDestinationAddressPrefix** eigenschap. De **expandedDestinationAddressPrefix** eigenschappenlijsten alle adresvoorvoegsels dat wordt vertegenwoordigd door de servicetag.

Als u dubbele regels die worden vermeld in de uitvoer ziet, is dit omdat een NSG is gekoppeld aan de netwerkinterface en het subnet. Zowel nsg's hebben de dezelfde standaardregels en mogelijk extra dubbele regels, als u uw eigen regels die hetzelfde als in beide nsg's zijn hebt gemaakt.

De regel voor licentiecontrole **defaultSecurityRules/DenyAllInBound** is wat houdt binnenkomende communicatie met de virtuele machine via poort 80, via internet, zoals beschreven in de [scenario](#scenario). Er geen andere regel met een hogere prioriteit (lager nummer) kunt poort 80 voor binnenkomende vanaf internet.

## <a name="resolve-a-problem"></a>Problemen op te lossen

Hiermee wordt aangegeven of het gebruik van de Azure [portal](#diagnose-using-azure-portal), [PowerShell](#diagnose-using-powershell), of de [Azure CLI](#diagnose-using-azure-cli) voor het vaststellen van het probleem dat wordt weergegeven in de [scenario](#scenario) in deze artikel, de oplossing is een netwerkbeveiligingsregel maken met de volgende eigenschappen:

| Eigenschap                | Waarde                                                                              |
|---------                |---------                                                                           |
| Bron                  | Alle                                                                                |
| Poortbereiken van bron      | Alle                                                                                |
| Doel             | Het IP-adres van de virtuele machine, een bereik met IP-adressen of alle adressen in het subnet. |
| Poortbereiken van doel | 80                                                                                 |
| Protocol                | TCP                                                                                |
| Bewerking                  | Toestaan                                                                              |
| Prioriteit                | 100                                                                                |
| Naam                    | Toestaan-HTTP-All                                                                     |

Nadat u de regel hebt gemaakt, poort 80 is toegestaan omdat de prioriteit van de regel hoger dan de standaardregel voor beveiliging met de naam is van het internet voor binnenkomende *DenyAllInBound*, dat het verkeer weigeren. Meer informatie over hoe [een beveiligingsregel maken](manage-network-security-group.md#create-a-security-rule). Als verschillende nsg's gekoppeld aan de netwerkinterface en het subnet zijn, moet u dezelfde regel maken in beide nsg's.

Wanneer Azure processen binnenkomend verkeer, regels in de NSG die is gekoppeld aan het subnet (als er een bijbehorende NSG) worden verwerkt en vervolgens de regels in de NSG die is gekoppeld aan de netwerkinterface worden verwerkt. Als er een NSG die is gekoppeld aan de netwerkinterface en het subnet is, is de poort moet openen in beide nsg's, voor het verkeer te bereiken van de virtuele machine. Beheer- en communicatiekoppelingen problemen vereenvoudigen, is het raadzaam dat u een NSG aan een subnet, in plaats van afzonderlijke netwerkinterfaces koppelen. Als virtuele machines binnen een subnet verschillende beveiligingsregels moeten, kunt u leden van de interfaces van de beveiligingsgroep van een toepassing (ASG) van het netwerk maken en een ASG opgeven als de bron en bestemming van een beveiligingsregel. Meer informatie over [toepassing beveiligingsgroepen](security-overview.md#application-security-groups).

Als u steeds communicatieproblemen ondervindt nog, raadpleegt u [overwegingen](#considerations) en [extra diagnose](#additional-dignosis).

## <a name="considerations"></a>Overwegingen

Houd rekening met de volgende punten bij het oplossen van problemen met de netwerkconnectiviteit:

* Standaardregels voor beveiliging blokkeren binnenkomende toegang via het internet en staan alleen inkomend verkeer van het virtuele netwerk. Toevoegen beveiligingsregels voor verbindingen met een hogere prioriteit dan de standaardregels zodat binnenkomend verkeer van het Internet. Meer informatie over [standaard beveiligingsregels](security-overview.md#default-security-rules), of hoe [een beveiligingsregel toevoegen](manage-network-security-group.md#create-a-security-rule).
* Als u virtuele netwerken hebt brengen standaard de **VIRTUAL_NETWORK** servicetag automatisch uitgebreid voorvoegsels voor virtuele netwerken peer is ingesteld. Voor het oplossen van problemen die betrekking hebben op het virtuele netwerk peering vindt u de voorvoegsels in de **ExpandedAddressPrefix** lijst. Meer informatie over [virtueel netwerk peering](virtual-network-peering-overview.md) en [service labels](security-overview.md#service-tags).
* Effectieve beveiligingsregels worden alleen weergegeven voor een netwerkinterface als er een NSG is gekoppeld aan het VM-netwerkinterface en, of, subnet, en als de virtuele machine uitgevoerd wordt.
* Als er geen nsg's die zijn gekoppeld aan de netwerkinterface of een subnet, en u hebt een [openbaar IP-adres](virtual-network-public-ip-address.md) toegewezen aan een virtuele machine, alle poorten zijn geopend voor binnenkomende toegang uit en uitgaande toegang tot elke locatie. Als de virtuele machine een openbaar IP-adres heeft, raden wij de netwerkinterface een NSG wordt toegepast op het subnet.

## <a name="additional-diagnosis"></a>Aanvullende diagnose

* Voor het uitvoeren van een snelle test om te bepalen of verkeer naar of van een virtuele machine is toegestaan, gebruiken de [IP-stroom controleren](../network-watcher/diagnose-vm-network-traffic-filtering-problem.md) mogelijkheden van Azure-netwerk-Watcher. IP-stroom controleren vertelt u als verkeer wordt toegestaan of geweigerd. Als geweigerd, IP-stroom controleren ziet u welke regel wordt het verkeer geweigerd.
* Als er geen verbinding met het netwerk van een virtuele machine mislukt waardoor beveiligingsregels, kan het probleem worden veroorzaakt:
  * Firewall-software die in de VM-besturingssysteem worden uitgevoerd
  * Routes die zijn geconfigureerd voor virtuele apparaten of lokale verkeer. Internetverkeer kan worden omgeleid naar uw on-premises netwerk via [geforceerde tunneling](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Als u een tunnel internetverkeer geforceerd naar een virtueel apparaat of on-premises, kunt u mogelijk geen verbinding maken met de virtuele machine vanaf het internet. Zie voor informatie over het analyseren van problemen met route die de stroom van verkeer van de virtuele machine belemmeren, [onderzoeken van een virtuele machine verkeer routering netwerkprobleem](diagnose-network-routing-problem.md).

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over alle taken, eigenschappen en instellingen voor een [netwerkbeveiligingsgroep](manage-network-security-group.md#work-with-network-security-groups) en [beveiligingsregels](manage-network-security-group.md#work-with-security-rules).
- Meer informatie over [standaard beveiligingsregels](security-overview.md#default-security-rules), [service labels](security-overview.md#service-tags), en [hoe Azure beveiligingsregels voor binnenkomend en uitgaand verkeer verwerkt](security-overview.md#network-security-groups) voor een virtuele machine.
