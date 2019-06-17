---
title: Implementeren van een IPv6 dual stack-toepassing in Azure-netwerk - PowerShell
titlesuffix: Azure Virtual Network
description: In dit artikel toont hoe een toepassing IPv6 dual-stack in Azure-netwerk met behulp van Azure Powershell implementeren.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/22/2019
ms.author: kumud
ms.openlocfilehash: 5ef051f42f3d092cc1d88008eaa8af981684ac6c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66730043"
---
# <a name="deploy-an-ipv6-dual-stack-application-in-azure---powershell-preview"></a>Implementeren van een IPv6 dual stack-toepassing in Azure - PowerShell (Preview)

Dit artikel ziet u hoe u een dual-stack (IPv4 + IPv6)-toepassing in Azure met een dual-stack virtueel netwerk en een subnet, een load balancer met dual (IPv4 + IPv6) front-configuraties, VM's met NIC's waarvoor een dubbele IP-configuratie implementeert netwerk beveiligingsgroep en openbare IP-adressen.

> [!Important]
> IPv6-ondersteuning voor Azure Virtual Network is momenteel in openbare preview. Deze preview wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. De reden hiervoor is dat bepaalde functies mogelijk niet worden ondersteund of beperkte mogelijkheden hebben. Raadpleeg voor meer informatie de [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u wilt installeren en gebruiken van PowerShell lokaal, in dit artikel is vereist voor de Azure PowerShell-moduleversie 6.9.0 of hoger. Voer `Get-Module -ListAvailable Az` uit om te kijken welke versie is geïnstalleerd. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-Az-ps). Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzAccount` uitvoeren om verbinding te kunnen maken met Azure.

## <a name="prerequisites"></a>Vereisten
Voordat u een dual-stack-toepassing in Azure implementeert, moet u uw abonnement voor deze preview-functie met behulp van de volgende Azure PowerShell configureren:

Registreert u als volgt:
```azurepowershell
Register-AzProviderFeature -FeatureName AllowIPv6VirtualNetwork -ProviderNamespace Microsoft.Network
```
Het duurt maximaal 30 minuten voor de functieregistratie te voltooien. U kunt de registratiestatus van uw controleren door het uitvoeren van de volgende Azure PowerShell-opdracht: Controleer op de registratie als volgt:
```azurepowershell
Get-AzProviderFeature -FeatureName AllowIPv6VirtualNetwork -ProviderNamespace Microsoft.Network
```
Nadat de registratie voltooid is, voert u de volgende opdracht uit:

```azurepowershell
Register-AzResourceProvider -ProviderNamespace Microsoft.Network
```

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Voordat u het virtuele netwerk van dual stack maken kunt, moet u een resourcegroep met [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup). Het volgende voorbeeld wordt een resourcegroep met de naam *myRGDualStack* in de *east us* locatie:

```azurepowershell-interactive
   $rg = New-AzResourceGroup `
  -ResourceGroupName "dsRG1"  `
  -Location "east us"
```

## <a name="create-ipv4-and-ipv6-public-ip-addresses"></a>Maken van openbare IP-adressen van IPv4 en IPv6
Voor toegang tot uw virtuele machines van het Internet, moet u IPv4 en IPv6-openbare IP-adressen voor de load balancer. Maken van openbare IP-adressen met [New-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress). Het volgende voorbeeld wordt IPv4 en IPv6-openbare IP-adres met de naam *dsPublicIP_v4* en *dsPublicIP_v6* in de *dsRG1* resourcegroep:

```azurepowershell-interactive
$PublicIP_v4 = New-AzPublicIpAddress `
  -Name "dsPublicIP_v4" `
  -ResourceGroupName $rg.ResourceGroupName `
  -Location $rg.Location  `
  -AllocationMethod Dynamic `
  -IpAddressVersion IPv4
  
$PublicIP_v6 = New-AzPublicIpAddress `
  -Name "dsPublicIP_v6" `
  -ResourceGroupName $rg.ResourceGroupName `
  -Location $rg.Location  `
  -AllocationMethod Dynamic `
  -IpAddressVersion IPv6
```
Voor toegang tot uw virtuele machines met een RDP-verbinding moet maken van een IPv4-openbare IP-adressen voor de virtuele machines met [New-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress).

```azurepowershell-interactive
  $RdpPublicIP_1 = New-AzPublicIpAddress `
  -Name "RdpPublicIP_1" `
  -ResourceGroupName $rg.ResourceGroupName `
  -Location $rg.Location  `
  -AllocationMethod Dynamic `
  -IpAddressVersion IPv4
  
  $RdpPublicIP_2 = New-AzPublicIpAddress `
   -Name "RdpPublicIP_2" `
   -ResourceGroupName $rg.ResourceGroupName `
   -Location $rg.Location  `
   -AllocationMethod Dynamic `
   -IpAddressVersion IPv4
```

## <a name="create-basic-load-balancer"></a>Load balancer van het type Basic maken

In deze sectie maakt u twee front-end IP-adres (IPv4 en IPv6) en de back-end-adresgroep voor de load balancer configureren en maak vervolgens een Basic Load Balancer.

### <a name="create-front-end-ip"></a>Het front-end-IP-adres maken

Maak een front-end-IP-adres met [New-AzLoadBalancerFrontendIpConfig](/powershell/module/az.network/new-azloadbalancerfrontendipconfig). Het volgende voorbeeld wordt IPv4 en IPv6-frontend-IP-configuraties met de naam *dsLbFrontEnd_v4* en *dsLbFrontEnd_v6*:

```azurepowershell-interactive
$frontendIPv4 = New-AzLoadBalancerFrontendIpConfig `
  -Name "dsLbFrontEnd_v4" `
  -PublicIpAddress $PublicIP_v4

$frontendIPv6 = New-AzLoadBalancerFrontendIpConfig `
  -Name "dsLbFrontEnd_v6" `
  -PublicIpAddress $PublicIP_v6

```

### <a name="configure-back-end-address-pool"></a>De back-endadresgroep configureren

Maak een back-end-adresgroep met [New-AzLoadBalancerBackendAddressPoolConfig](/powershell/module/az.network/new-azloadbalancerbackendaddresspoolconfig). In de resterende stappen worden de VM's aan deze back-end-groep gekoppeld. Het volgende voorbeeld wordt een back-end-adresgroepen met de naam *dsLbBackEndPool_v4* en *dsLbBackEndPool_v6* om op te nemen van virtuele machines met zowel IPV4 als IPv6-NIC-configuraties:

```azurepowershell-interactive
$backendPoolv4 = New-AzLoadBalancerBackendAddressPoolConfig `
-Name "dsLbBackEndPool_v4"

$backendPoolv6 = New-AzLoadBalancerBackendAddressPoolConfig `
-Name "dsLbBackEndPool_v6"
```

### <a name="create-a-load-balancer-rule"></a>Een load balancer-regel maken

Een load balancer-regel wordt gebruikt om de verdeling van het verkeer over de VM's te definiëren. U definieert de front-end-IP-configuratie voor het inkomende verkeer en de back-end-IP-groep om het verkeer te ontvangen, samen met de gewenste bron- en doelpoort. Zorg ervoor dat alleen veilige VM's verkeer ontvangen, kunt u desgewenst een statustest definiëren. Basisversie van load balancer maakt gebruik van een IPv4-test om de status voor zowel IPv4 als IPv6-eindpunten op de VM's te beoordelen. Standaardversie van load balancer biedt ondersteuning voor expliciet statuscontroles van IPv6.

Maak een load balancer-regel met [Add-AzLoadBalancerRuleConfig](/powershell/module/az.network/add-azloadbalancerruleconfig). Het volgende voorbeeld wordt de load balancer-regels met de naam *dsLBrule_v4* en *dsLBrule_v6* waarmee het verkeer op *TCP* poort *80* naar de IPv4 en IPv6 front-end IP-configuraties:

```azurepowershell-interactive
$lbrule_v4 = New-AzLoadBalancerRuleConfig `
  -Name "dsLBrule_v4" `
  -FrontendIpConfiguration $frontendIPv4 `
  -BackendAddressPool $backendPoolv4 `
  -Protocol Tcp `
  -FrontendPort 80 `
  -BackendPort 80

$lbrule_v6 = New-AzLoadBalancerRuleConfig `
  -Name "dsLBrule_v6" `
  -FrontendIpConfiguration $frontendIPv6 `
  -BackendAddressPool $backendPoolv6 `
  -Protocol Tcp `
  -FrontendPort 80 `
  -BackendPort 80
```

### <a name="create-load-balancer"></a>Load balancer maken

Maak de Basic Load Balancer met [New-AzLoadBalancer](/powershell/module/az.network/new-azloadbalancer). Het volgende voorbeeld wordt een openbare Basic Load Balancer met de naam *myLoadBalancer* met behulp van de IPv4 en IPv6-frontend-IP-configuraties, back-endpools en taakverdelingsregels die u hebt gemaakt in de voorgaande stappen:

```azurepowershell-interactive
$lb = New-AzLoadBalancer `
-ResourceGroupName $rg.ResourceGroupName `
-Location $rg.Location  `
-Name "MyLoadBalancer" `
-Sku "Basic" `
-FrontendIpConfiguration $frontendIPv4,$frontendIPv6 `
-BackendAddressPool $backendPoolv4,$backendPoolv6 `
-LoadBalancingRule $lbrule_v4,$lbrule_v6

```

## <a name="create-network-resources"></a>Netwerkbronnen maken
Voordat u enkele VM's implementeert en uw balancer test, moet u ondersteunende netwerkbronnen - beschikbaarheidsset, netwerkbeveiligingsgroep, virtuele netwerken en virtuele NIC's. 
### <a name="create-an-availability-set"></a>Een beschikbaarheidsset maken
Verbeter de hoge beschikbaarheid van uw app door uw VM's in een beschikbaarheidsset te plaatsen.

Maak een beschikbaarheidsset met [New-AzAvailabilitySet](/powershell/module/az.compute/new-azavailabilityset). In het volgende voorbeeld wordt een beschikbaarheidsset met de naam *myAvailabilitySet* gemaakt:

```azurepowershell-interactive
$avset = New-AzAvailabilitySet `
  -ResourceGroupName $rg.ResourceGroupName `
  -Location $rg.Location  `
  -Name "dsAVset" `
  -PlatformFaultDomainCount 2 `
  -PlatformUpdateDomainCount 2 `
  -Sku aligned
```

### <a name="create-network-security-group"></a>Netwerkbeveiligingsgroep maken

Maak een netwerkbeveiligingsgroep voor de regels die binnenkomende en uitgaande communicatie in uw VNET bepalen.

#### <a name="create-a-network-security-group-rule-for-port-3389"></a>Een netwerkbeveiligingsgroepsregel maken voor poort 3389

Maak een netwerkbeveiligingsgroepsregel die RDP-verbindingen via poort 3389 toestaat met [New-AzNetworkSecurityRuleConfig](/powershell/module/az.network/new-aznetworksecurityruleconfig).

```azurepowershell-interactive
$rule1 = New-AzNetworkSecurityRuleConfig `
-Name 'myNetworkSecurityGroupRuleRDP' `
-Description 'Allow RDP' `
-Access Allow `
-Protocol Tcp `
-Direction Inbound `
-Priority 100 `
-SourceAddressPrefix * `
-SourcePortRange * `
-DestinationAddressPrefix * `
-DestinationPortRange 3389
```
#### <a name="create-a-network-security-group-rule-for-port-80"></a>Een netwerkbeveiligingsgroepsregel maken voor poort 80

Maak een regel voor de netwerkbeveiligingsgroep voor internet-verbindingen via poort 80 met [New-AzNetworkSecurityRuleConfig](/powershell/module/az.network/new-aznetworksecurityruleconfig).

```azurepowershell-interactive
$rule2 = New-AzNetworkSecurityRuleConfig `
  -Name 'myNetworkSecurityGroupRuleHTTP' `
  -Description 'Allow HTTP' `
  -Access Allow `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 200 `
  -SourceAddressPrefix * `
  -SourcePortRange 80 `
  -DestinationAddressPrefix * `
  -DestinationPortRange 80
```
#### <a name="create-a-network-security-group"></a>Een netwerkbeveiligingsgroep maken

Maak een netwerkbeveiligingsgroep met [New-AzNetworkSecurityGroup](/powershell/module/az.network/new-aznetworksecuritygroup).

```azurepowershell-interactive
$nsg = New-AzNetworkSecurityGroup `
-ResourceGroupName $rg.ResourceGroupName `
-Location $rg.Location  `
-Name "dsNSG1"  `
-SecurityRules $rule1,$rule2
```
### <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

Maak een virtueel netwerk met [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork). In het volgende voorbeeld wordt een virtueel netwerk gemaakt met de naam *myVnet* met *mySubnet*:

```azurepowershell-interactive
# Create dual stack subnet
$subnet = New-AzVirtualNetworkSubnetConfig `
-Name "dsSubnet" `
-AddressPrefix "10.0.0.0/24","ace:cab:deca:deed::/64"

# Create the virtual network
$vnet = New-AzVirtualNetwork `
  -ResourceGroupName $rg.ResourceGroupName `
  -Location $rg.Location  `
  -Name "dsVnet" `
  -AddressPrefix "10.0.0.0/16","ace:cab:deca::/48"  `
  -Subnet $subnet
```

### <a name="create-nics"></a>NIC's maken

Maken van virtuele NIC's met [New-AzNetworkInterface](/powershell/module/az.network/new-aznetworkinterface). Het volgende voorbeeld maakt twee virtuele NIC met IPv4 en IPv6-configuraties. (Eén virtuele NIC voor elke VM die u in de volgende stappen voor uw app maakt).

```azurepowershell-interactive
  $Ip4Config=New-AzNetworkInterfaceIpConfig `
    -Name dsIp4Config `
    -Subnet $vnet.subnets[0] `
    -PrivateIpAddressVersion IPv4 `
    -LoadBalancerBackendAddressPool $backendPoolv4 `
    -PublicIpAddress  $RdpPublicIP_1
    
  $Ip6Config=New-AzNetworkInterfaceIpConfig `
    -Name dsIp6Config `
    -Subnet $vnet.subnets[0] `
    -PrivateIpAddressVersion IPv6 `
    -LoadBalancerBackendAddressPool $backendPoolv6
    
  $NIC_1 = New-AzNetworkInterface `
    -Name "dsNIC1" `
    -ResourceGroupName $rg.ResourceGroupName `
    -Location $rg.Location  `
    -NetworkSecurityGroupId $nsg.Id `
    -IpConfiguration $Ip4Config,$Ip6Config 
    
  $Ip4Config=New-AzNetworkInterfaceIpConfig `
    -Name dsIp4Config `
    -Subnet $vnet.subnets[0] `
    -PrivateIpAddressVersion IPv4 `
    -LoadBalancerBackendAddressPool $backendPoolv4 `
    -PublicIpAddress  $RdpPublicIP_2  

  $NIC_2 = New-AzNetworkInterface `
    -Name "dsNIC2" `
    -ResourceGroupName $rg.ResourceGroupName `
    -Location $rg.Location  `
    -NetworkSecurityGroupId $nsg.Id `
    -IpConfiguration $Ip4Config,$Ip6Config 

```

### <a name="create-virtual-machines"></a>Virtuele machines maken

Stel een beheerdersnaam en -wachtwoord voor de VM’s in met [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```azurepowershell-interactive
$cred = get-credential -Message "DUAL STACK VNET SAMPLE:  Please enter the Administrator credential to log into the VMs."
```

Nu kunt u de VM’s maken met [New-AzVM](/powershell/module/az.compute/new-azvm). In het volgende voorbeeld worden twee VM's en de vereiste onderdelen van het virtuele netwerk gemaakt als deze nog niet bestaan. 

```azurepowershell-interactive
$vmsize = "Standard_A2"
$ImagePublisher = "MicrosoftWindowsServer"
$imageOffer = "WindowsServer"
$imageSKU = "2016-Datacenter"

$vmName= "dsVM1"
$VMconfig1 = New-AzVMConfig -VMName $vmName -VMSize $vmsize -AvailabilitySetId $avset.Id 3> $null | Set-AzVMOperatingSystem -Windows -ComputerName $vmName -Credential $cred -ProvisionVMAgent 3> $null | Set-AzVMSourceImage -PublisherName $ImagePublisher -Offer $imageOffer -Skus $imageSKU -Version "latest" 3> $null | Set-AzVMOSDisk -Name "$vmName.vhd" -CreateOption fromImage  3> $null | Add-AzVMNetworkInterface -Id $NIC_1.Id  3> $null 
$VM1 = New-AzVM -ResourceGroupName $rg.ResourceGroupName  -Location $rg.Location  -VM $VMconfig1 

$vmName= "dsVM2"
$VMconfig2 = New-AzVMConfig -VMName $vmName -VMSize $vmsize -AvailabilitySetId $avset.Id 3> $null | Set-AzVMOperatingSystem -Windows -ComputerName $vmName -Credential $cred -ProvisionVMAgent 3> $null | Set-AzVMSourceImage -PublisherName $ImagePublisher -Offer $imageOffer -Skus $imageSKU -Version "latest" 3> $null | Set-AzVMOSDisk -Name "$vmName.vhd" -CreateOption fromImage  3> $null | Add-AzVMNetworkInterface -Id $NIC_2.Id  3> $null 
$VM2 = New-AzVM -ResourceGroupName $rg.ResourceGroupName  -Location $rg.Location  -VM $VMconfig2
```

## <a name="determine-ip-addresses-of-the-ipv4-and-ipv6-endpoints"></a>Vast IP-adressen van de IPv4 en IPv6-eindpunten
Ophalen van alle Network Interface-objecten in de resourcegroep om samen te vatten van het IP-adres gebruikt in deze implementatie met `get-AzNetworkInterface`. Bovendien krijgt van de Load Balancer-frontend-adressen van IPv4 en IPv6-eindpunten met `get-AzpublicIpAddress`.

```azurepowershell-interactive
$rgName= "dsRG1"
$NICsInRG= get-AzNetworkInterface -resourceGroupName $rgName 
write-host `nSummary of IPs in this Deployment: 
write-host ******************************************
foreach ($NIC in $NICsInRG) {
 
    $VMid= $NIC.virtualmachine.id 
    $VMnamebits= $VMid.split("/") 
    $VMname= $VMnamebits[($VMnamebits.count-1)] 
    write-host `nPrivate IP addresses for $VMname 
    $IPconfigsInNIC= $NIC.IPconfigurations 
    foreach ($IPconfig in $IPconfigsInNIC) {
 
        $IPaddress= $IPconfig.privateipaddress 
        write-host "    "$IPaddress 
        IF ($IPconfig.PublicIpAddress.ID) {
 
            $IDbits= ($IPconfig.PublicIpAddress.ID).split("/")
            $PipName= $IDbits[($IDbits.count-1)]
            $PipObject= get-azPublicIpAddress -name $PipName -resourceGroup $rgName
            write-host "    "RDP address:  $PipObject.IpAddress
                 }
         }
 }
 
 
 
  write-host `nPublic IP addresses on Load Balancer:
 
  (get-AzpublicIpAddress -resourcegroupname $rgName | where { $_.name -notlike "RdpPublicIP*" }).IpAddress
```
De volgende afbeelding toont een voorbeeld van uitvoer met een lijst met de privé IPv4 en IPv6-adressen van de twee virtuele machines en de frontend IPv4 en IPv6-IP-adressen van de Load Balancer.

![IP-samenvatting van de toepassingsimplementatie dual stack (IPv4/IPv6) in Azure](./media/virtual-network-ipv4-ipv6-dual-stack-powershell/dual-stack-application-summary.png)

## <a name="view-ipv6-dual-stack-virtual-network-in-azure-portal"></a>IPv6 dual-stack virtueel netwerk in Azure portal bekijken
U kunt het virtuele netwerk van IPv6 dual-stack in Azure-portal als volgt bekijken:
1. Voer in de zoekbalk van de portal, *dsVnet*.
2. Wanneer **myVirtualNetwork** wordt weergegeven in de zoekresultaten, selecteert u dit. Dit start de **overzicht** pagina van het virtuele netwerk met de naam van dual-stack *dsVnet*. Het virtuele netwerk van dual-stack ziet u de twee NIC's met zowel IPv4 als IPv6-configuraties die zich in de dual stack-subnet met de naam *dsSubnet*.

  ![IPv6 dual-stack virtueel netwerk in Azure](./media/virtual-network-ipv4-ipv6-dual-stack-powershell/dual-stack-vnet.png)

> [!NOTE]
> De IPv6 voor Azure-netwerk is beschikbaar in Azure portal in alleen-lezen voor deze preview-versie.

## <a name="clean-up-resources"></a>Resources opschonen

U kunt de opdracht [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) gebruiken om de resourcegroep, de VM en alle gerelateerde resources te verwijderen wanneer u ze niet meer nodig hebt.

```azurepowershell-interactive
Remove-AzResourceGroup -Name dsRG1
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel, kunt u een Basic Load Balancer gemaakt met een dubbele front-end-IP-configuratie (IPv4 en IPv6). Hebt u ook een twee virtuele machines die opgenomen NIC's met dubbele IP-configuraties (IPV4 + IPv6) die zijn toegevoegd aan de back-endpool van de load balancer gemaakt. Zie voor meer informatie over IPv6-ondersteuning in virtuele netwerken van Azure, [wat IPv6 voor Azure Virtual Network is?](ipv6-overview.md)