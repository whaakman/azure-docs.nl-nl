---
title: 'Quickstart: Een Standard Load Balancer maken - Azure PowerShell'
titlesuffix: Azure Load Balancer
description: In deze snelstart vindt u meer informatie over het maken van een Standard Load Balancer met behulp PowerShell
services: load-balancer
documentationcenter: na
author: KumudD
manager: twooley
Customer intent: I want to create a Standard Load balancer so that I can load balance internet traffic to VMs.
ms.assetid: ''
ms.service: load-balancer
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/07/2019
ms.author: kumud
ms:custom: seodec18
ms.openlocfilehash: 04db0232e14ccac7938d7062d77c36a54526489c
ms.sourcegitcommit: 1aefdf876c95bf6c07b12eb8c5fab98e92948000
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/06/2019
ms.locfileid: "66730477"
---
# <a name="quickstart-create-a-standard-load-balancer-using-azure-powershell"></a>Quickstart: Een Standard Load Balancer maken met Azure PowerShell

In deze snelstart vindt u meer informatie over het maken van een Standard Load Balancer met behulp Azure PowerShell. U test de load balancer, moet u drie virtuele machines (VM's) met Windows server implementeren en taakverdeling van een web-app tussen de virtuele machines. Zie [Wat is Standard Load Balancer](load-balancer-standard-overview.md) voor meer informatie over Standard Load Balancer.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u PowerShell lokaal wilt installeren en gebruiken, is voor dit artikel versie 5.4.1 of hoger van de Azure PowerShell-module vereist. Voer `Get-Module -ListAvailable Az` uit om te kijken welke versie is geïnstalleerd. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-Az-ps). Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzAccount` uitvoeren om verbinding te kunnen maken met Azure.

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Voordat u een load balancer kunt maken, moet u eerst een resourcegroep maken met [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup). Het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroupSLB* in de *EastUS* locatie:

```azurepowershell
$rgName='MyResourceGroupSLB'
$location='eastus'
New-AzResourceGroup -Name $rgName -Location $location
```

## <a name="create-a-public-ip-address"></a>Een openbaar IP-adres maken

Om toegang te krijgen tot uw app op internet, hebt u een openbaar IP-adres nodig voor de load balancer. Maak een openbaar IP-adres met [New-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress). Het volgende voorbeeld wordt een openbaar IP-adres met de naam *myPublicIP* in de *myResourceGroupSLB* resourcegroep:

```azurepowershell
$publicIp = New-AzPublicIpAddress `
 -ResourceGroupName $rgName `
 -Name 'myPublicIP' `
 -Location $location `
 -AllocationMethod static `
 -SKU Standard
```

## <a name="create-standard-load-balancer"></a>Een Standard Load Balancer maken

In deze sectie configureert u de front-end-IP en de back-endadresgroep voor de load balancer en maakt u vervolgens de Standard Load Balancer.

### <a name="create-front-end-ip"></a>Maak een front-end IP-adres

Maak een front-end IP-adres met [New-AzLoadBalancerFrontendIpConfig](/powershell/module/az.network/new-azloadbalancerfrontendipconfig). In het volgende voorbeeld wordt een front-end-IP-configuratie met de naam *myFrontEnd* gemaakt en wordt het adres *myPublicIP* eraan gekoppeld:

```azurepowershell
$feip = New-AzLoadBalancerFrontendIpConfig -Name 'myFrontEndPool' -PublicIpAddress $publicIp
```

### <a name="configure-back-end-address-pool"></a>De back-endadresgroep configureren

Maak een back-end-adresgroep met [New-AzLoadBalancerBackendAddressPoolConfig](/powershell/module/az.network/new-azloadbalancerbackendaddresspoolconfig). In de resterende stappen worden de VM's aan deze back-end-groep gekoppeld. In het volgende voorbeeld wordt een back-end-adresgroep met de naam *myBackEndPool* gemaakt:

```azurepowershell-interactive
$bepool = New-AzLoadBalancerBackendAddressPoolConfig -Name 'myBackEndPool'
```

### <a name="create-a-health-probe"></a>Een statustest maken
U gebruikt een statustest om de load balancer de status van uw app te laten bewaken. De statustest voegt dynamisch VM's toe aan de load balancer-rotatie of verwijdert ze, op basis van hun reactie op statuscontroles. Standaard wordt een VM uit de load balancer-distributie verwijderd na twee opeenvolgende fouten met intervallen van 15 seconden. U maakt een statustest op basis van een protocol of een specifieke statuscontrolepagina voor uw app.

In het volgende voorbeeld wordt een TCP-test gemaakt. U kunt ook aangepaste HTTP-tests maken voor meer fijnmazige statuscontroles. Wanneer u een aangepaste HTTP-test maakt, moet u de statustestpagina maken, zoals *healthcheck.aspx*. De test moet het antwoord **HTTP 200 OK** voor de load balancer retourneren om de host in rotatie te houden.

U maakt een TCP-statustest met behulp van [Add-AzLoadBalancerProbeConfig](/powershell/module/az.network/add-azloadbalancerprobeconfig). In het volgende voorbeeld wordt een statustest gemaakt met de naam *myHealthProbe*, die elke VM bewaakt op *HTTP*-poort *80*:

```azurepowershell
$probe = New-AzLoadBalancerProbeConfig `
 -Name 'myHealthProbe' `
 -Protocol Http -Port 80 `
 -RequestPath / -IntervalInSeconds 360 -ProbeCount 5
```

### <a name="create-a-load-balancer-rule"></a>Een load balancer-regel maken
Een load balancer-regel wordt gebruikt om de verdeling van het verkeer over de VM's te definiëren. U definieert de front-end-IP-configuratie voor het inkomende verkeer en de back-end-IP-groep om het verkeer te ontvangen, samen met de gewenste bron- en doelpoort. Om ervoor te zorgen dat alleen VM's met een goede status verkeer ontvangen, moet u ook de te gebruiken statustest definiëren.

Maak een load balancer-regel met [Add-AzLoadBalancerRuleConfig](/powershell/module/az.network/add-azloadbalancerruleconfig). In het volgende voorbeeld wordt een load balancer-regel met de naam *myLoadBalancerRule* gemaakt waarmee het verkeer op *TCP*-poort *80* wordt geregeld:

```azurepowershell
$rule = New-AzLoadBalancerRuleConfig `
  -Name 'myLoadBalancerRuleWeb' -Protocol Tcp `
  -Probe $probe -FrontendPort 80 -BackendPort 80 `
  -FrontendIpConfiguration $feip `
  -BackendAddressPool $bePool
```

### <a name="create-the-nat-rules"></a>De NAT-regels maken

Maak NAT-regels met [Add-AzLoadBalancerRuleConfig](/powershell/module/az.network/new-azloadbalancerinboundnatruleconfig). In het volgende voorbeeld worden NAT-regels genaamd *myLoadBalancerRDP1* en *myLoadBalancerRDP2* gemaakt om RDP-verbindingen met de back-endservers toe te staan op poort 4221 en 4222:

```azurepowershell
$natrule1 = New-AzLoadBalancerInboundNatRuleConfig `
  -Name 'myLoadBalancerRDP1' `
  -FrontendIpConfiguration $feip `
  -Protocol tcp -FrontendPort 4221 `
  -BackendPort 3389

$natrule2 = New-AzLoadBalancerInboundNatRuleConfig `
  -Name 'myLoadBalancerRDP2' `
  -FrontendIpConfiguration $feip `
  -Protocol tcp `
  -FrontendPort 4222 `
  -BackendPort 3389

$natrule3 = New-AzLoadBalancerInboundNatRuleConfig `
  -Name 'myLoadBalancerRDP3' `
  -FrontendIpConfiguration $feip `
  -Protocol tcp `
  -FrontendPort 4223 `
  -BackendPort 3389
```

### <a name="create-load-balancer"></a>Load balancer maken

Maak de Standard Load Balancer met [New-AzLoadBalancer](/powershell/module/az.network/new-azloadbalancer). Het volgende voorbeeld wordt een openbare Standard Load Balancer met de naam myLoadBalancer met behulp van de front-end-IP-configuratie, back-end-pool, statustest, regel voor taakverdeling en NAT-regels die u hebt gemaakt in de voorgaande stappen:

```azurepowershell
$lb = New-AzLoadBalancer `
  -ResourceGroupName $rgName `
  -Name 'MyLoadBalancer' `
  -SKU Standard `
  -Location $location `
  -FrontendIpConfiguration $feip `
  -BackendAddressPool $bepool `
  -Probe $probe `
  -LoadBalancingRule $rule `
  -InboundNatRule $natrule1,$natrule2,$natrule3
```

## <a name="create-network-resources"></a>Netwerkbronnen maken
Voordat u enkele VM's implementeert en uw balancer test, moet u ondersteunende netwerkbronnen maken (virtueel netwerk en virtuele NIC's). 

### <a name="create-a-virtual-network"></a>Een virtueel netwerk maken
Maak een virtueel netwerk met [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork). In het volgende voorbeeld wordt een virtueel netwerk gemaakt met de naam *myVnet* met *mySubnet*:

```azurepowershell
# Create subnet config
$subnetConfig = New-AzVirtualNetworkSubnetConfig `
  -Name "mySubnet" `
  -AddressPrefix 10.0.2.0/24

# Create the virtual network
$vnet = New-AzVirtualNetwork `
  -ResourceGroupName "myResourceGroupSLB" `
  -Location $location `
  -Name "myVnet" `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $subnetConfig
```
### <a name="create-public-ip-addresses-for-the-vms"></a>Openbare IP-adressen voor de virtuele machines maken

Voor toegang tot uw virtuele machines met behulp van een RDP-verbinding, moet u openbare IP-adres voor de virtuele machines. Omdat een Standard Load Balancer wordt gebruikt in dit scenario, moet u de standaard openbare IP-adressen maken voor de virtuele machines met [New-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress).

```azurepowershell
$RdpPublicIP_1 = New-AzPublicIpAddress `
  -Name "RdpPublicIP_1" `
  -ResourceGroupName $RgName `
  -Location $location  `
  -SKU Standard `
  -AllocationMethod static
 

$RdpPublicIP_2 = New-AzPublicIpAddress `
  -Name "RdpPublicIP_2" `
  -ResourceGroupName $RgName `
  -Location $location  `
  -SKU Standard `
  -AllocationMethod static


$RdpPublicIP_3 = New-AzPublicIpAddress `
  -Name "RdpPublicIP_3" `
  -ResourceGroupName $RgName `
  -Location $location  `
  -SKU Standard `
  -AllocationMethod static

```
### <a name="create-network-security-group"></a>Netwerkbeveiligingsgroep maken
Maak een netwerkbeveiligingsgroep om binnenkomende verbindingen met uw virtuele netwerk te definiëren.

#### <a name="create-a-network-security-group-rule-for-port-3389"></a>Een netwerkbeveiligingsgroepsregel maken voor poort 3389
Maak een netwerkbeveiligingsgroepsregel die RDP-verbindingen via poort 3389 toestaat met [New-AzNetworkSecurityRuleConfig](/powershell/module/az.network/new-aznetworksecurityruleconfig).

```azurepowershell

$rule1 = New-AzNetworkSecurityRuleConfig -Name 'myNetworkSecurityGroupRuleRDP' -Description 'Allow RDP' `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 1000 `
  -SourceAddressPrefix Internet -SourcePortRange * `
  -DestinationAddressPrefix * -DestinationPortRange 3389
```

#### <a name="create-a-network-security-group-rule-for-port-80"></a>Een netwerkbeveiligingsgroepsregel maken voor poort 80
Maak een netwerkbeveiligingsgroepsregel die inkomende verbindingen via poort 80 toestaat met [New-AzNetworkSecurityRuleConfig](/powershell/module/az.network/new-aznetworksecurityruleconfig).

```azurepowershell
$rule2 = New-AzNetworkSecurityRuleConfig -Name 'myNetworkSecurityGroupRuleHTTP' -Description 'Allow HTTP' `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 2000 `
  -SourceAddressPrefix Internet -SourcePortRange * `
  -DestinationAddressPrefix * -DestinationPortRange 80
```

#### <a name="create-a-network-security-group"></a>Een netwerkbeveiligingsgroep maken

Maak een netwerkbeveiligingsgroep met [New-AzNetworkSecurityGroup](/powershell/module/az.network/new-aznetworksecuritygroup).

```azurepowershell
$nsg = New-AzNetworkSecurityGroup -ResourceGroupName $RgName -Location $location `
-Name 'myNetworkSecurityGroup' -SecurityRules $rule1,$rule2
```

### <a name="create-nics"></a>NIC's maken
Virtuele NIC's maken en koppelen aan het openbare IP-adres en netwerkbeveiligingsgroepen die zijn gemaakt in de eerdere stappen met [New-AzNetworkInterface](/powershell/module/az.network/new-aznetworkinterface). In het volgende voorbeeld worden drie virtuele NIC's gemaakt. (Eén virtuele NIC voor elke VM die u in de volgende stappen voor uw app maakt). U kunt op elk gewenst moment extra virtuele NIC's en VM's maken en toevoegen aan de load balancer:

```azurepowershell
# Create NIC for VM1
$nicVM1 = New-AzNetworkInterface -ResourceGroupName $rgName -Location $location `
  -Name 'MyNic1' -PublicIpAddress $RdpPublicIP_1 -LoadBalancerBackendAddressPool $bepool -NetworkSecurityGroup $nsg `
  -LoadBalancerInboundNatRule $natrule1 -Subnet $vnet.Subnets[0]

$nicVM2 = New-AzNetworkInterface -ResourceGroupName $rgName -Location $location `
  -Name 'MyNic2' -PublicIpAddress $RdpPublicIP_2 -LoadBalancerBackendAddressPool $bepool -NetworkSecurityGroup $nsg `
  -LoadBalancerInboundNatRule $natrule2 -Subnet $vnet.Subnets[0]

$nicVM3 = New-AzNetworkInterface -ResourceGroupName $rgName -Location $location `
  -Name 'MyNic3' -PublicIpAddress $RdpPublicIP_3 -LoadBalancerBackendAddressPool $bepool -NetworkSecurityGroup $nsg `
  -LoadBalancerInboundNatRule $natrule3 -Subnet $vnet.Subnets[0]
```

### <a name="create-virtual-machines"></a>Virtuele machines maken

Stel een beheerdersnaam en -wachtwoord voor de VM’s in met [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```azurepowershell
$cred = Get-Credential
```

Nu kunt u de VM’s maken met [New-AzVM](/powershell/module/az.compute/new-azvm). In het volgende voorbeeld worden twee VM's en de vereiste onderdelen van het virtuele netwerk gemaakt als deze nog niet bestaan. In dit voorbeeld wordt de NIC's (*MyNic1*, *MyNic2*, en *MyNic3*) gemaakt in de vorige stap zijn toegewezen aan virtuele machines *myVM1* , *myVM2*, en *VM3*. Bovendien, omdat de NIC's gekoppeld aan de load balancer back-endpool zijn, worden de virtuele machines automatisch toegevoegd aan de back-endpool.

```azurepowershell

# ############## VM1 ###############

# Create a virtual machine configuration
$vmConfig = New-AzVMConfig -VMName 'myVM1' -VMSize Standard_DS1_v2 `
 | Set-AzVMOperatingSystem -Windows -ComputerName 'myVM1' -Credential $cred `
 | Set-AzVMSourceImage -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2019-Datacenter -Version latest `
 | Add-AzVMNetworkInterface -Id $nicVM1.Id

# Create a virtual machine
$vm1 = New-AzVM -ResourceGroupName $rgName -Zone 1 -Location $location -VM $vmConfig

# ############## VM2 ###############

# Create a virtual machine configuration
$vmConfig = New-AzVMConfig -VMName 'myVM2' -VMSize Standard_DS1_v2 `
 | Set-AzVMOperatingSystem -Windows -ComputerName 'myVM2' -Credential $cred `
 | Set-AzVMSourceImage -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2019-Datacenter -Version latest `
 | Add-AzVMNetworkInterface -Id $nicVM2.Id

# Create a virtual machine
$vm2 = New-AzVM -ResourceGroupName $rgName -Zone 2 -Location $location -VM $vmConfig

# ############## VM3 ###############

# Create a virtual machine configuration
$vmConfig = New-AzVMConfig -VMName 'myVM3' -VMSize Standard_DS1_v2 `
 | Set-AzVMOperatingSystem -Windows -ComputerName 'myVM3' -Credential $cred `
 | Set-AzVMSourceImage -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2019-Datacenter -Version latest `
| Add-AzVMNetworkInterface -Id $nicVM3.Id

# Create a virtual machine
$vm3 = New-AzVM -ResourceGroupName $rgName -Zone 3 -Location $location -VM $vmConfig
```

Het duurt een paar minuten maken en configureren van de drie virtuele machines.

### <a name="install-iis-with-a-custom-web-page"></a>IIS installeren met een aangepaste webpagina

Installeer IIS als volgt met een aangepaste webpagina op beide back-end-VM's:

1. Ophalen van de openbare IP-adressen van de drie virtuele machines met behulp van `Get-AzPublicIPAddress`.

   ```azurepowershell
     $vm1_rdp_ip = (Get-AzPublicIPAddress -ResourceGroupName $rgName -Name "RdpPublicIP_1").IpAddress
     $vm2_rdp_ip = (Get-AzPublicIPAddress -ResourceGroupName $rgName -Name "RdpPublicIP_2").IpAddress
     $vm3_rdp_ip = (Get-AzPublicIPAddress -ResourceGroupName $rgName -Name "RdpPublicIP_3").IpAddress
    ```
2. Maken van verbindingen met extern bureaublad met *myVM1*, *myVM2*, en *myVM3* met behulp van de openbare IP-adressen van de virtuele machines als volgt: 

   ```azurepowershell    
     mstsc /v:$vm1_rdp_ip
     mstsc /v:$vm2_rdp_ip
     mstsc /v:$vm3_rdp_ip
   
    ```

3. Voer de referenties voor elke virtuele machine om de RDP-sessie te starten.
4. Start Windows PowerShell op elke virtuele machine en met behulp van de volgende opdrachten voor het installeren van IIS-server en het standaard htm-bestand bijwerken.

    ```azurepowershell
    # Install IIS
     Install-WindowsFeature -name Web-Server -IncludeManagementTools
    
    # Remove default htm file
     remove-item  C:\inetpub\wwwroot\iisstart.htm
    
    #Add custom htm file
     Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from host " + $env:computername)
    ```

5. Sluit de RDP-verbindingen met *myVM1*, *myVM2*, en *myVM3*.


## <a name="test-load-balancer"></a>Load balancer testen
Haal het openbare IP-adres van uw load balancer op met [Get-AzPublicIPAddress](/powershell/module/az.network/get-azpublicipaddress). In het volgende voorbeeld wordt het IP-adres opgehaald voor het eerder gemaakte *myPublicIP*:

```azurepowershell
Get-AzPublicIPAddress `
  -ResourceGroupName "myResourceGroupSLB" `
  -Name "myPublicIP" | select IpAddress
```

Vervolgens kunt u het openbare IP-adres invoeren in een webbrowser. De website wordt weergegeven met de hostnaam van de VM waarnaar de load balancer verkeer heeft gedistribueerd, zoals in het volgende voorbeeld:

![Load balancer testen](media/quickstart-create-basic-load-balancer-powershell/load-balancer-test.png)

Als u wilt zien hoe de load balancer verkeer distribueert naar alle drie de VM's waarop uw app wordt uitgevoerd, kunt u vernieuwing van uw webbrowser afdwingen. 

## <a name="clean-up-resources"></a>Resources opschonen

U kunt de opdracht [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) gebruiken om de resourcegroep, de VM en alle gerelateerde resources te verwijderen wanneer u ze niet meer nodig hebt.

```azurepowershell
Remove-AzResourceGroup -Name myResourceGroupSLB
```

## <a name="next-steps"></a>Volgende stappen

In deze snelstart hebt u een standaard Load Balancer gemaakt, VM's daaraan gekoppeld, een regel voor het Load Balancer-verkeer geconfigureerd, een statustest gemaakt en vervolgens de load balancer getest. Voor meer informatie over Azure Load Balancer gaat u verder met de zelfstudies voor Azure Load Balancer.

> [!div class="nextstepaction"]
> [Zelfstudies voor Azure Load Balancer](tutorial-load-balancer-basic-internal-portal.md)
