---
title: Een probleem VM-netwerk-verkeersfilter vaststellen | Microsoft Docs
description: Leer hoe u een probleem VM-netwerk-verkeersfilter vaststellen door de effectieve beveiligingsregels voor een virtuele machine weer te geven.
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
ms.openlocfilehash: 8b494e3f289d7b3a850a77f7f388cee542c088ed
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55821861"
---
# <a name="diagnose-a-virtual-machine-network-traffic-filter-problem"></a>Een probleem VM-netwerk-verkeersfilter vaststellen

In dit artikel leert u hoe u een probleem met netwerk verkeersfilter vaststellen door het netwerk (NSG) beveiligingsregels die voor een virtuele machine (VM gelden) weer te geven.

Nsg's kunnen u bepalen welke typen verkeer wordt de stroom en afmelden bij een virtuele machine. U kunt een NSG koppelen aan een subnet in een Azure-netwerk, een netwerkinterface die is gekoppeld aan een virtuele machine, of beide. De effectieve beveiligingsregels toegepast op een netwerkinterface zijn een aggregatie van de regels die zijn opgenomen in de NSG die is gekoppeld aan een netwerkinterface en het subnet van de netwerkinterface wordt. Regels in verschillende nsg's kunnen soms met elkaar conflicteren en van invloed zijn op de netwerkverbinding van een virtuele machine. U kunt de effectieve beveiligingsregels van nsg's die worden toegepast op netwerkinterfaces van de virtuele machine weergeven. Als u niet bekend met virtuele netwerken, netwerk-interface of NSG-concepten bent, Zie [overzicht van Virtual network](virtual-networks-overview.md), [netwerkinterface](virtual-network-network-interface.md), en [overzichtvannetwerkbeveiligingsgroepen](security-overview.md).

## <a name="scenario"></a>Scenario

U probeert verbinding maken met een virtuele machine via poort 80 vanaf internet, maar de verbinding is verbroken. Om te bepalen waarom u geen toegang tot poort 80 vanaf Internet, kunt u de effectieve beveiligingsregels voor een netwerkinterface met behulp van Azure weergeven [portal](#diagnose-using-azure-portal), [PowerShell](#diagnose-using-powershell), of de [Azure CLI](#diagnose-using-azure-cli).

De volgende stappen wordt ervan uitgegaan dat u hebt een bestaande virtuele machine om de effectieve beveiligingsregels voor weer te geven. Als u een bestaande virtuele machine hebt, implementeert eerst een [Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) of [Windows](../virtual-machines/windows/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) virtuele machine om de taken in dit artikel. De voorbeelden in dit artikel zijn van een virtuele machine met de naam *myVM* met een netwerkinterface met de naam *myVMVMNic*. De virtuele machine en een netwerkinterface zich in een resourcegroep met de naam *myResourceGroup*, en zijn de *VS-Oost* regio. Wijzig de waarden in de stappen zo nodig voor de virtuele machine die u bij het vaststellen van het probleem voor.

## <a name="diagnose-using-azure-portal"></a>Vaststellen met behulp van Azure portal

1. Meld u aan bij de Azure [portal](https://portal.azure.com) met een Azure-account waarvoor de [benodigde machtigingen](virtual-network-network-interface.md#permissions).
2. Aan de bovenkant van de Azure-portal, voer de naam van de virtuele machine in het zoekvak in. Wanneer de naam van de virtuele machine wordt weergegeven in de lijst met zoekresultaten, selecteert u deze.
3. Onder **instellingen**, selecteer **netwerken**, zoals wordt weergegeven in de volgende afbeelding:

   ![Beveiligingsregels weergeven](./media/diagnose-network-traffic-filter-problem/view-security-rules.png)

   De regels die u ziet die worden vermeld in de vorige afbeelding zijn voor een netwerkinterface met de naam **myVMVMNic**. U ziet dat er **regels voor binnenkomende poort** voor de netwerkinterface van twee verschillende netwerkbeveiligingsgroepen:
   
   - **mySubnetNSG**: Gekoppeld aan het subnet waarin de netwerkinterface zich bevindt.
   - **myVMNSG**: Dat is gekoppeld aan de netwerkinterface op de virtuele machine met de naam **myVMVMNic**.

   De regel met de naam **DenyAllInBound** is wat voorkomt dat binnenkomende communicatie met de virtuele machine via poort 80, het internet, zoals beschreven in de [scenario](#scenario). De regel voor een lijst met *0.0.0.0/0* voor **bron**, waaronder het internet. Er zijn geen andere regel met een hogere prioriteit (lager nummer) poort 80 wordt toegestaan binnenkomend verkeer. Om toe te staan van poort 80 inkomend verkeer naar de virtuele machine van het internet, Zie [problemen op te lossen](#resolve-a-problem). Zie voor meer informatie over de beveiligingsregels voor verbindingen en hoe Azure toegepast, [Netwerkbeveiligingsgroepen](security-overview.md).

   Aan de onderkant van de afbeelding ziet u ook **uitgaande POORTREGELS**. Onder dat zijn de regels voor uitgaande poort voor de netwerkinterface. Hoewel de afbeelding ziet u slechts vier regels voor binnenkomende verbindingen voor elke NSG, kunnen uw nsg's veel meer dan vier regels hebben. In de afbeelding ziet u **VirtualNetwork** onder **bron** en **bestemming** en **AzureLoadBalancer** onder  **BRON**. **VirtualNetwork** en **AzureLoadBalancer** zijn [servicetags](security-overview.md#service-tags). Service-tags vertegenwoordigt een groep met IP-adresvoorvoegsels om u te helpen bij het maken van beveiligingsregel vereenvoudigt.

4. Zorg ervoor dat de virtuele machine zich in de die wordt uitgevoerd op de staat en selecteer vervolgens **effectieve beveiligingsregels**, zoals wordt weergegeven in de vorige afbeelding, om te zien van de effectieve beveiligingsregels, weergegeven in de volgende afbeelding:

   ![De effectieve beveiligingsregels bekijken](./media/diagnose-network-traffic-filter-problem/view-effective-security-rules.png)

   De weergegeven regels zijn dat hetzelfde als u hebt gezien in stap 3, maar er verschillende tabbladen voor de Netwerkbeveiligingsgroep die is gekoppeld aan de netwerkinterface en het subnet zijn. Zoals u in de afbeelding ziet, worden alleen de eerste 50 regels weergegeven. Selecteer voor het downloaden van een CSV-bestand met alle regels **downloaden**.

   Om te zien dat elke servicetag prefixen vertegenwoordigt, selecteert u een regel, zoals de regel met de naam **AllowAzureLoadBalancerInbound**. De volgende afbeelding ziet u de voorvoegsels voor de **AzureLoadBalancer** servicetag:

   ![De effectieve beveiligingsregels bekijken](./media/diagnose-network-traffic-filter-problem/address-prefixes.png)

   Hoewel de **AzureLoadBalancer** servicetag vertegenwoordigt alleen een voorvoegsel, andere servicetags verschillende voorvoegsels vertegenwoordigen.

5. De vorige stappen hebt u geleerd de beveiligingsregels voor een netwerkinterface met de naam **myVMVMNic**, maar u hebt ook een netwerkinterface met de naam gezien **myVMVMNic2** in enkele van de vorige afbeeldingen. De virtuele machine in dit voorbeeld heeft twee netwerkinterfaces die zijn gekoppeld aan. De effectieve beveiligingsregels kunnen afwijken voor elke netwerkinterface.

   Om te zien van de regels voor de **myVMVMNic2** netwerkinterface, selecteert u deze. Zoals u in de afbeelding die volgt, de netwerkinterface heeft dezelfde regels die zijn gekoppeld aan een subnet als de **myVMVMNic** netwerkinterface, omdat beide netwerkinterfaces in hetzelfde subnet bevinden. Als u een NSG aan een subnet koppelt, worden de regels worden toegepast op alle netwerkinterfaces in het subnet.

   ![Beveiligingsregels weergeven](./media/diagnose-network-traffic-filter-problem/view-security-rules2.png)

   In tegenstelling tot de **myVMVMNic** netwerkinterface, de **myVMVMNic2** netwerkinterface heeft geen een netwerkbeveiligingsgroep zijn gekoppeld. Elke netwerkinterface en het subnet kan nul, of een NSG die is gekoppeld aan deze. De NSG die is gekoppeld aan elke netwerkinterface of subnet kan niet hetzelfde zijn, of een ander. Als u kiest, kunt u de dezelfde netwerkbeveiligingsgroep aan zo veel netwerkinterfaces en subnetten koppelen.

Hoewel de effectieve beveiligingsregels zijn weergegeven via de virtuele machine, kunt u ook effectieve beveiligingsregels via een afzonderlijke bekijken:
- **Netwerkinterface**: Meer informatie over het [weergeven van een netwerkinterface](virtual-network-network-interface.md#view-network-interface-settings).
- **NSG**: Meer informatie over het [weergeven van een NSG](manage-network-security-group.md#view-details-of-a-network-security-group).

## <a name="diagnose-using-powershell"></a>Vaststellen met behulp van PowerShell

U kunt de opdrachten die volgen in uitvoeren de [Azure Cloud Shell](https://shell.azure.com/powershell), of door te voeren PowerShell vanaf uw computer. De Azure Cloud Shell is een gratis interactieve shell. In deze shell zijn algemene Azure-hulpprogramma's vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. Als u PowerShell vanaf uw computer uitvoeren, moet u de *AzureRM* PowerShell-module, versie 6.0.1 of hoger. Voer `Get-Module -ListAvailable AzureRM` op uw computer, de geïnstalleerde versie te vinden. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/azurerm/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook om uit te voeren `Login-AzureRmAccount` aan te melden bij Azure met een account met de [benodigde machtigingen](virtual-network-network-interface.md#permissions)].

De effectieve beveiligingsregels ophalen voor een netwerkinterface met [Get-AzureRmEffectiveNetworkSecurityGroup](/powershell/module/azurerm.network/get-azurermeffectivenetworksecuritygroup). Het volgende voorbeeld wordt de effectieve beveiligingsregels voor een netwerkinterface met de naam *myVMVMNic*, dat wil zeggen in een resourcegroep met de naam *myResourceGroup*:

```azurepowershell-interactive
Get-AzureRmEffectiveNetworkSecurityGroup `
  -NetworkInterfaceName myVMVMNic interface `
  -ResourceGroupName myResourceGroup
```

Uitvoer is geretourneerd in json-indeling. Zie voor meer informatie over de uitvoer, [interpreteren opdrachtuitvoer](#interpret-command-output).
Uitvoer wordt alleen geretourneerd als een NSG gekoppeld aan de netwerkinterface, het subnet dat de netwerkinterface zich is bevindt, of beide. De virtuele machine moet zich in de actieve status. Een virtuele machine mogelijk meerdere netwerkinterfaces met verschillende nsg's die zijn toegepast. Bij het oplossen van problemen, voert u de opdracht voor elke netwerkinterface.

Als u er nog steeds een probleem met de connectiviteit, raadpleegt u [extra diagnose](#additional-diagnosis) en [overwegingen met betrekking tot](#considerations).

Als u de naam van een netwerkinterface niet weet, maar de naam van de virtuele machine de netwerkinterface is gekoppeld aan, retourneren de volgende opdrachten de id's van alle netwerkinterfaces die zijn gekoppeld aan een virtuele machine:

```azurepowershell-interactive
$VM = Get-AzureRmVM -Name myVM -ResourceGroupName myResourceGroup
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

Als u Azure-opdrachtregelinterface (CLI)-opdrachten voor taken in dit artikel uit te voeren, hetzij de opdrachten uitvoert in de [Azure Cloud Shell](https://shell.azure.com/bash), of door het uitvoeren van de CLI van de computer. In dit artikel gebruikmaken van Azure CLI versie 2.0.32 of hoger. Voer `az --version` uit om te kijken welke versie is geïnstalleerd. Zie [Azure CLI installeren](/cli/azure/install-azure-cli) als u de CLI wilt installeren of een upgrade wilt uitvoeren. Als u de Azure CLI lokaal uitvoert, moet u ook om uit te voeren `az login` en meld u aan bij Azure met een account met de [benodigde machtigingen](virtual-network-network-interface.md#permissions).

De effectieve beveiligingsregels ophalen voor een netwerkinterface met [az network nic list-effectief-nsg](/cli/azure/network/nic#az-network-nic-list-effective-nsg). Het volgende voorbeeld wordt de effectieve beveiligingsregels voor een netwerkinterface met de naam *myVMVMNic* die zich in een resourcegroep met de naam *myResourceGroup*:

```azurecli-interactive
az network nic list-effective-nsg \
  --name myVMVMNic \
  --resource-group myResourceGroup
```

Uitvoer is geretourneerd in json-indeling. Zie voor meer informatie over de uitvoer, [interpreteren opdrachtuitvoer](#interpret-command-output).
Uitvoer wordt alleen geretourneerd als een NSG gekoppeld aan de netwerkinterface, het subnet dat de netwerkinterface zich is bevindt, of beide. De virtuele machine moet zich in de actieve status. Een virtuele machine mogelijk meerdere netwerkinterfaces met verschillende nsg's die zijn toegepast. Bij het oplossen van problemen, voert u de opdracht voor elke netwerkinterface.

Als u er nog steeds een probleem met de connectiviteit, raadpleegt u [extra diagnose](#additional-diagnosis) en [overwegingen met betrekking tot](#considerations).

Als u de naam van een netwerkinterface niet weet, maar de naam van de virtuele machine de netwerkinterface is gekoppeld aan, retourneren de volgende opdrachten de id's van alle netwerkinterfaces die zijn gekoppeld aan een virtuele machine:

```azurecli-interactive
az vm show \
  --name myVM \
  --resource-group myResourceGroup
```

In de resulterende uitvoer ziet u informatie uit die lijkt op het volgende voorbeeld:

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

In de uitvoer van de vorige naam van de netwerk-interface is *myVMVMNic interface*.

## <a name="interpret-command-output"></a>Uitvoer van de opdracht interpreteren

Ongeacht of u gebruikt de [PowerShell](#diagnose-using-powershell), of de [Azure CLI](#diagnose-using-azure-cli) om de oorzaak van het probleem, ontvangt u uitvoer met de volgende informatie:

- **NetworkSecurityGroup**: De ID van de netwerkbeveiligingsgroep.
- **Koppeling**: Of de netwerkbeveiligingsgroep is gekoppeld aan een *NetworkInterface* of *Subnet*. Als een NSG gekoppeld aan beide is, wordt de uitvoer geretourneerd met **NetworkSecurityGroup**, **koppeling**, en **EffectiveSecurityRules**, voor elke NSG. Als de NSG is gekoppeld of losgekoppeld direct vóór het uitvoeren van de opdracht om de effectieve beveiligingsregels weer te geven, moet u mogelijk Wacht een paar seconden om de wijziging doorgevoerd in de uitvoer van de opdracht.
- **EffectiveSecurityRules**: Een uitleg van elke eigenschap wordt beschreven in [maken van een beveiligingsregel](manage-network-security-group.md#create-a-security-rule). Regel namen die worden voorafgegaan door *defaultSecurityRules /* zijn standaard beveiligingsregels die aanwezig zijn in elke NSG. Regel namen die worden voorafgegaan door *securityRules /* zijn regels die u hebt gemaakt. Regels die specificeren een [servicetag](security-overview.md#service-tags), zoals **Internet**, **VirtualNetwork**, en **AzureLoadBalancer** voor de  **destinationAddressPrefix** of **sourceAddressPrefix** eigenschappen, hebben ook waarden voor de **expandedDestinationAddressPrefix** eigenschap. De **expandedDestinationAddressPrefix** eigenschap geeft een lijst van alle adresvoorvoegsels vertegenwoordigd door de servicetag.

Als u dubbele regels die worden vermeld in de uitvoer ziet, is omdat een NSG gekoppeld aan de netwerkinterface en het subnet is. Zowel met nsg's hebben de dezelfde standaardregels en mogelijk extra dubbele regels, als u uw eigen regels die hetzelfde als in beide nsg's zijn hebt gemaakt.

De regel met de naam **defaultSecurityRules/DenyAllInBound** is wat voorkomt dat binnenkomende communicatie met de virtuele machine via poort 80, het internet, zoals beschreven in de [scenario](#scenario). Er zijn geen andere regels met een hogere prioriteit (lager nummer) kunt u poort 80 inkomend verkeer van internet.

## <a name="resolve-a-problem"></a>Problemen op te lossen

Of het gebruik van de Azure [portal](#diagnose-using-azure-portal), [PowerShell](#diagnose-using-powershell), of de [Azure CLI](#diagnose-using-azure-cli) voor het vaststellen van het probleem dat wordt weergegeven in de [scenario](#scenario) in deze artikel, de oplossing is het maken van een regel voor de beveiliging met de volgende eigenschappen:

| Eigenschap                | Value                                                                              |
|---------                |---------                                                                           |
| Bron                  | Alle                                                                                |
| Poortbereiken van bron      | Alle                                                                                |
| Doel             | Het IP-adres van de virtuele machine, een bereik van IP-adressen of alle adressen in het subnet. |
| Poortbereiken van doel | 80                                                                                 |
| Protocol                | TCP                                                                                |
| Bewerking                  | Toestaan                                                                              |
| Prioriteit                | 100                                                                                |
| Name                    | Allow-HTTP-All                                                                     |

Nadat u de regel maakt, is toegestaan voor poort 80 inkomend verkeer van internet, omdat de prioriteit van de regel hoger dan de standaard-beveiligingsregel met de naam is *DenyAllInBound*, dat het verkeer weigeren. Meer informatie over het [maken van een beveiligingsregel](manage-network-security-group.md#create-a-security-rule). Als verschillende nsg's gekoppeld aan de netwerkinterface en het subnet zijn, moet u dezelfde regel in beide nsg's maken.

Wanneer Azure processen binnenkomend verkeer, regels in de NSG die is gekoppeld aan het subnet (als er een gekoppelde NSG) worden verwerkt en vervolgens de regels in de NSG die is gekoppeld aan de netwerkinterface worden verwerkt. Als er een NSG die is gekoppeld aan de netwerkinterface en het subnet, is de poort moet openen in beide nsg's, voor het verkeer naar de virtuele machine kan bereiken. Om de problemen met beheer en communicatie, is het raadzaam dat u een NSG aan een subnet, in plaats van afzonderlijke netwerkinterfaces koppelen. Als VM's binnen een subnet verschillende beveiligingsregels moeten, kunt u het netwerk interfaces lid maken van een toepassingsbeveiligingsgroep (ASG), en een ASG opgeven als de bron en doel van een beveiligingsregel. Meer informatie over [toepassingsbeveiligingsgroepen](security-overview.md#application-security-groups).

Als u nog steeds communicatieproblemen ondervindt, raadpleegt u [overwegingen met betrekking tot](#considerations) en aanvullende diagnose.

## <a name="considerations"></a>Overwegingen

Houd rekening met de volgende punten bij het oplossen van problemen met de netwerkverbinding:

* Standaardbeveiligingsregels binnenkomende toegang via internet te blokkeren, en alleen toegestaan binnenkomend verkeer van het virtuele netwerk. Als u wilt toestaan dat binnenkomend verkeer van Internet, toevoegen beveiligingsregels voor verbindingen met een hogere prioriteit dan de standaardregels. Meer informatie over [standaard beveiligingsregels](security-overview.md#default-security-rules), of hoe u [een beveiligingsregel toevoegen](manage-network-security-group.md#create-a-security-rule).
* Als u virtuele netwerken zijn gekoppeld standaard de **VIRTUAL_NETWORK** servicetag automatisch uitgebreid met voorvoegsels voor de gekoppelde virtuele netwerken. Voor het oplossen van problemen met betrekking tot het virtuele netwerk-peering, vindt u de voorvoegsels in de **ExpandedAddressPrefix** lijst. Meer informatie over [peering op virtueel netwerk](virtual-network-peering-overview.md) en [servicetags](security-overview.md#service-tags).
* Effectieve beveiligingsregels worden alleen weergegeven voor een netwerkinterface als er een NSG is gekoppeld aan de netwerkinterface van de virtuele machine en, of, subnet, en als de virtuele machine in de status running doorbrengt.
* Als er geen nsg's die zijn gekoppeld aan de netwerkinterface of subnet, en u hebt een [openbaar IP-adres](virtual-network-public-ip-address.md) toegewezen aan een virtuele machine, alle poorten zijn geopend voor inkomende toegang uit en uitgaande toegang tot elke locatie. Als de virtuele machine een openbaar IP-adres heeft, raden wij de netwerkinterface toepassen van een NSG op het subnet.

## <a name="additional-diagnosis"></a>Aanvullende diagnose

* Om uit te voeren kort te testen om te bepalen of verkeer naar of van een virtuele machine is toegestaan, gebruiken de [IP-stroom controleren](../network-watcher/diagnose-vm-network-traffic-filtering-problem.md) mogelijkheden van Azure Network Watcher. IP-stroom controleren vertelt u als verkeer wordt toegestaan of geweigerd. Als geweigerd, IP-stroom controleren leest u welke regel wordt het verkeer weigeren.
* Als er geen verbinding met het netwerk van een virtuele machine mislukt waardoor beveiligingsregels, kan het probleem worden veroorzaakt door:
  * Firewallsoftware die wordt uitgevoerd in het besturingssysteem van de virtuele machine
  * Routes die zijn geconfigureerd voor virtuele apparaten of on-premises verkeer. Internet-verkeer kan worden omgeleid naar uw on-premises netwerk via [geforceerd tunnelen](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Als u internetverkeer tunnel naar een virtueel apparaat of on-premises, kunt u mogelijk geen verbinding maken met de virtuele machine via internet. Zie voor informatie over het analyseren van route-problemen die de stroom van het verkeer naar de virtuele machine kunnen verhinderen, [vaststellen van een virtuele machine verkeer routeren netwerkprobleem](diagnose-network-routing-problem.md).

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over alle taken, eigenschappen en instellingen voor een [netwerkbeveiligingsgroep](manage-network-security-group.md#work-with-network-security-groups) en [beveiligingsregels](manage-network-security-group.md#work-with-security-rules).
- Meer informatie over [standaard beveiligingsregels](security-overview.md#default-security-rules), [servicetags](security-overview.md#service-tags), en [hoe Azure beveiligingsregels voor binnenkomend en uitgaand verkeer verwerkt](security-overview.md#network-security-groups) voor een virtuele machine.
