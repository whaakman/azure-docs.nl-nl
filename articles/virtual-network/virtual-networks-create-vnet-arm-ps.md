---
title: Een virtueel Azure-netwerk maken met meerdere subnetten - PowerShell | Microsoft Docs
description: Informatie over het maken van een virtueel netwerk met meerdere subnetten met behulp van PowerShell.
services: virtual-network
documentationcenter: 
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: 
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/01/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: c90bdc9381bf98e5c1457e8e28f74105227d8f8d
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/13/2018
---
# <a name="create-a-virtual-network-with-multiple-subnets-using-powershell"></a>Een virtueel netwerk maken met meerdere subnetten met behulp van PowerShell

Een virtueel netwerk kunt verschillende soorten Azure-resources met het Internet en privé met elkaar communiceren. Meerdere subnetten in een virtueel netwerk maken, kunt u bij het segmenteren van uw netwerk, zodat u kunt filteren of het besturingselement de verkeersstroom tussen subnetten. In dit artikel leert u hoe:

> [!div class="checklist"]
> * Een virtueel netwerk maken
> * Een subnet maken
> * Testen van netwerkcommunicatie tussen virtuele machines

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Als u wilt installeren en gebruiken van PowerShell lokaal, wordt in deze zelfstudie Azure PowerShell moduleversie 3,6 of hoger vereist. Voer ` Get-Module -ListAvailable AzureRM` de geïnstalleerde versie vinden. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Login-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure. 

## <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

Maak een resourcegroep met [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* in de *EastUS* locatie.

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```

Maak een virtueel netwerk met [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork). Het volgende voorbeeld wordt een virtueel netwerk met de naam *myVirtualNetwork* met het adresvoorvoegsel *10.0.0.0/16*.

```azurepowershell-interactive
$virtualNetwork = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork `
  -AddressPrefix 10.0.0.0/16
```

De **AddressPrefix** is opgegeven in CIDR-notatie. Het opgegeven adresvoorvoegsel bevat het IP-adressen 10.0.0.0-10.0.255.254.

## <a name="create-a-subnet"></a>Een subnet maken

Een subnet wordt gemaakt door eerst de configuratie van een subnet maken en vervolgens het virtuele netwerk bij te werken met de subnetconfiguratie. Maken van een subnetconfiguratie met [nieuw AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig). Het volgende voorbeeld maakt u twee subnetconfiguraties voor *openbare* en *persoonlijke* subnetten:

```azurepowershell-interactive
$subnetConfigPublic = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name Public `
  -AddressPrefix 10.0.0.0/24 `
  -VirtualNetwork $virtualNetwork

$subnetConfigPrivate = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name Private `
  -AddressPrefix 10.0.1.0/24 `
  -VirtualNetwork $virtualNetwork
```

De **AddressPrefix** opgegeven voor een subnet binnen het adresvoorvoegsel gedefinieerd voor het virtuele netwerk moet. Azure DHCP IP-adressen worden toegewezen vanuit een adresvoorvoegsel subnet naar bronnen die zijn geïmplementeerd in een subnet. Azure alleen de 10.0.0.4-10.0.0.254 adressen worden toegewezen aan resources die zijn geïmplementeerd in de **openbare** subnet, omdat de eerste vier adressen zijn gereserveerd Azure (10.0.0.0-10.0.0.3 voor het subnet van de in dit voorbeeld) en de laatste adres () 10.0.0.255 voor het subnet van de in dit voorbeeld) in elk subnet.

De subnetconfiguraties schrijven naar het virtuele netwerk met [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/Set-AzureRmVirtualNetwork), waarvan de subnetten maakt in het virtuele netwerk:

```azurepowershell-interactive
$virtualNetwork | Set-AzureRmVirtualNetwork
```

Voordat u deze implementeert in Azure virtuele netwerken en subnetten voor gebruik in productieomgevingen, wordt aanbevolen dat u zorgvuldig vertrouwd raken met de adresruimte [overwegingen](manage-virtual-network.md#create-a-virtual-network) en [virtueel netwerk limieten](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits). Zodra resources zijn geïmplementeerd in subnetten, kunnen sommige virtueel netwerk en subnet wijzigingen, zoals het wijzigen van-adresbereiken opnieuw distribueren van bestaande Azure-resources geïmplementeerd in subnetten vereisen.

## <a name="test-network-communication"></a>Test de netwerkcommunicatie

Een virtueel netwerk kunt verschillende soorten Azure-resources met het Internet en privé met elkaar communiceren. Een type resource dat u in een virtueel netwerk implementeren kunt is een virtuele machine. Twee virtuele machines maken in het virtuele netwerk, zodat u de netwerkcommunicatie tussen deze en het Internet in een later stadium testen kunt. 

### <a name="create-virtual-machines"></a>Virtuele machines maken

Maak een virtuele machine met [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Het volgende voorbeeld wordt een virtuele machine met de naam *myVmWeb* in de *openbare* subnet van de *myVirtualNetwork* virtueel netwerk. De `-AsJob` optie maakt de virtuele machine op de achtergrond, zodat u kunt doorgaan met de volgende stap. Wanneer u wordt gevraagd, typt u de gebruikersnaam en wachtwoord die u wilt aanmelden bij de virtuele machine met.

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Location "East US" `
    -VirtualNetworkName "myVirtualNetwork" `
    -SubnetName "Public" `
    -ImageName "Win2016Datacenter" `
    -Name "myVmWeb" `
    -AsJob
```

10.0.0.4 Azure automatisch toegewezen als de persoonlijke IP-adres van de virtuele machine omdat 10.0.0.4 is het eerste beschikbare IP-adres in de *openbare* subnet. 

Maken van een virtuele machine in de *persoonlijke* subnet.

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Location "East US" `
    -VirtualNetworkName "myVirtualNetwork" `
    -SubnetName "Private" `
    -ImageName "Win2016Datacenter" `
    -Name "myVmMgmt"
```

De virtuele machine duurt een paar minuten maken. Hoewel het niet in de uitvoer van de geretourneerde Azure toegewezen 10.0.1.4 als het privé IP-adres van de virtuele machine omdat 10.0.1.4 is het eerste beschikbare IP-adres in de *persoonlijke* subnet van *myVirtualNetwork*. 

Ga niet verder met de resterende stappen totdat de virtuele machine wordt gemaakt en PowerShell retourneert de uitvoer.

De virtuele machines die in dit artikel hebt [netwerkinterface](virtual-network-network-interface.md) met één IP-adres dat dynamisch wordt toegewezen aan de netwerkinterface. Nadat u de virtuele machine hebt geïmplementeerd, kunt u [meerdere openbare en particuliere IP-adressen toevoegen of wijzigen van de toewijzingsmethode van IP-adres naar statisch](virtual-network-network-interface-addresses.md#add-ip-addresses). U kunt [netwerkinterfaces toevoegen](virtual-network-network-interface-vm.md#vm-add-nic), tot de limiet wordt ondersteund door de [VM-grootte](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) dat u selecteert wanneer u een virtuele machine maken. U kunt ook [één hoofdelement i/o-virtualisatie (SR-IOV) inschakelen](create-vm-accelerated-networking-powershell.md) voor een VM, maar alleen wanneer u een virtuele machine maken met een VM-grootte die ondersteuning de mogelijkheid biedt.

### <a name="communicate-between-virtual-machines-and-with-the-internet"></a>Communiceren tussen virtuele machines en met het internet

U kunt verbinding maken met het openbare IP-adres van een virtuele machine vanaf het Internet. Wanneer Azure gemaakt de *myVmMgmt* virtuele machine, een openbare IP-adres met de naam *myVmMgmt* ook is gemaakt en toegewezen aan de virtuele machine. Hoewel een virtuele machine is niet vereist voor het openbare IP-adres toegewezen, wijst Azure een openbaar IP-adres toe aan elke virtuele machine die u, standaard maakt. Om te communiceren via Internet met een virtuele machine, moet een openbaar IP-adres worden toegewezen aan de virtuele machine. Alle virtuele machines kunnen communiceren met het Internet, uitgaande of er een openbaar IP-adres is toegewezen aan de virtuele machine. Zie voor meer informatie over uitgaande Internet-verbindingen in Azure, [uitgaande verbindingen in Azure](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json). 

Gebruik [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) te retourneren van het openbare IP-adres van een virtuele machine. Het volgende voorbeeld wordt het openbare IP-adres van de *myVmMgmt* virtuele machine:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVmMgmt `
  -ResourceGroupName myResourceGroup | Select IpAddress
```

Gebruik de volgende opdracht voor het maken van een sessie voor extern bureaublad met de *myVmMgmt* virtuele machine van de lokale computer. Vervang `<publicIpAddress>` geretourneerd met het IP-adres van de vorige opdracht.

```
mstsc /v:<publicIpAddress>
```

Een Remote Desktop Protocol (RDP)-bestand is gemaakt, naar de computer gedownload en geopend. Geef de gebruikersnaam en wachtwoord die u hebt opgegeven bij het maken van de virtuele machine (mogelijk moet u selecteren **meer opties**, vervolgens **gebruik een ander account**, om op te geven de referenties die u hebt ingevoerd wanneer u de virtuele machine gemaakt), en klik vervolgens op **OK**. Er wordt mogelijk een certificaatwaarschuwing weergegeven tijdens het aanmelden. Klik op **Ja** of **Doorgaan** om door te gaan met de verbinding. 

In een later stadium ping wordt gebruikt om te communiceren met de *myVmMgmt* virtuele machine van de *myVmWeb* virtuele machine. Ping maakt gebruik van ICMP dat via de Windows Firewall standaard is geweigerd. ICMP inschakelen via de Windows firewall met de volgende opdracht vanaf een opdrachtprompt:

```
netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow
```

Hoewel ping in dit artikel wordt gebruikt, wordt zodat ICMP via de Windows Firewall voor productie-implementaties niet aanbevolen.

Uit veiligheidsoverwegingen is het gebruikelijk om het aantal virtuele machines die worden op afstand verbinding met een virtueel netwerk gemaakt kunnen te beperken. In deze zelfstudie de *myVmMgmt* virtuele machine wordt gebruikt voor het beheren van de *myVmWeb* virtuele machine in het virtuele netwerk. Gebruik de volgende opdracht met extern bureaublad naar de *myVmWeb* virtuele machine van de *myVmMgmt* virtuele machine:

``` 
mstsc /v:myVmWeb
```

Om te communiceren met de *myVmMgmt* virtuele machine van de *myVmWeb* virtuele machine, voer de volgende opdracht uit vanaf de opdrachtprompt:

```
ping myvmmgmt
```

De uitvoer is vergelijkbaar met de volgende voorbeelduitvoer wordt:
    
```
Pinging myvmmgmt.dar5p44cif3ulfq00wxznl3i3f.bx.internal.cloudapp.net [10.0.1.4] with 32 bytes of data:
Reply from 10.0.1.4: bytes=32 time<1ms TTL=128
Reply from 10.0.1.4: bytes=32 time<1ms TTL=128
Reply from 10.0.1.4: bytes=32 time<1ms TTL=128
Reply from 10.0.1.4: bytes=32 time<1ms TTL=128
    
Ping statistics for 10.0.1.4:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms
```
      
Kunt u zien dat het adres van de *myVmMgmt* virtuele machine is 10.0.1.4. 10.0.1.4 is het eerste beschikbare IP-adres in het adresbereik van de *persoonlijke* subnet dat u hebt geïmplementeerd de *myVmMgmt* virtuele machine in de vorige stap.  U ziet dat de volledig gekwalificeerde domeinnaam van de virtuele machine *myvmmgmt.dar5p44cif3ulfq00wxznl3i3f.bx.internal.cloudapp.net*. Hoewel de *dar5p44cif3ulfq00wxznl3i3f* gedeelte van de domeinnaam is verschillend voor de virtuele machine, de resterende gedeelten van de domeinnaam zijn hetzelfde. Alle virtuele machines in Azure gebruiken de standaard Azure DNS-service. Alle virtuele machines binnen een virtueel netwerk, kunnen de namen van alle andere virtuele machines in hetzelfde virtuele netwerk met behulp van Azure standaard DNS-service omzetten. In plaats van Azure standaard DNS-service, kunt u uw eigen DNS-server of de mogelijkheid persoonlijke domein van de Azure DNS-service. Zie voor meer informatie [naamomzetting met uw eigen DNS-server](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) of [Azure DNS gebruiken voor persoonlijke domeinen](../dns/private-dns-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Internet Information Services (IIS) voor Windows Server installeren op de *myVmWeb* virtuele machine, voer de volgende opdracht vanuit een PowerShell-sessie:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

Nadat de installatie van IIS voltooid is, Verbreek de verbinding met de *myVmWeb* extern bureaublad-sessiehost, waarbij u in de *myVmMgmt* extern bureaublad-sessiehost. Open een webbrowser en Ga naar http://myvmweb. U ziet de pagina Welkom IIS.

Verbreek de verbinding met de *myVmMgmt* extern bureaublad-sessiehost.

Ophalen van het openbare adres Azure toegewezen aan de *myVmWeb* virtuele machine:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVmWeb `
  -ResourceGroupName myResourceGroup | Select IpAddress
```

Blader op uw eigen computer naar het openbare IP-adres van de *myVmWeb* virtuele machine. De poging om weer te geven van de IIS-welkomstpagina van uw eigen computer is mislukt. De poging is mislukt omdat wanneer de virtuele machines zijn geïmplementeerd, Azure standaard een netwerkbeveiligingsgroep voor elke virtuele machine gemaakt. 

Een netwerkbeveiligingsgroep bevat beveiligingsregels voor verbindingen toestaan of weigeren van binnenkomende en uitgaande netwerkverkeer op poort en IP-adres. De standaard netwerkbeveiligingsgroep die Azure gemaakt kan communicatie via alle poorten tussen resources in hetzelfde virtuele netwerk. Voor Windows virtuele machines, de standaard netwerkbeveiligingsgroep weigert alle binnenkomend verkeer van Internet via alle poorten, TCP-poort 3389 (RDP) accepteren. Als gevolg hiervan standaard kunt u ook RDP rechtstreeks naar de *myVmWeb* virtuele machine van het Internet, zelfs als u wilt niet poort 3389 openen met een webserver. Omdat websurfen via poort 80 communiceert, wordt de communicatie van het Internet mislukt omdat er is geen regel in de standaard netwerkbeveiligingsgroep verkeer toestaat via poort 80.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer deze niet langer nodig is, gebruik [Remove-AzureRmResourcegroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) verwijderen van de resourcegroep en alle resources bevat.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u een virtueel netwerk met meerdere subnetten te implementeren. U hebt ook geleerd wanneer u een virtuele Windows-computer maakt, Azure een netwerkinterface maakt dat wordt gekoppeld aan de virtuele machine en wordt gemaakt van een netwerkbeveiligingsgroep waarmee alleen verkeer via poort 3389 van Internet. Ga naar de volgende zelfstudie voor informatie over het filteren van netwerkverkeer naar subnetten in plaats van afzonderlijke virtuele machines.

> [!div class="nextstepaction"]
> [Filteren van netwerkverkeer naar subnetten](./virtual-networks-create-nsg-arm-ps.md)
