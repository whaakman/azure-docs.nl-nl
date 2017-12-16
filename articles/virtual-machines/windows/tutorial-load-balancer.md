---
title: Het laden van een balans vinden tussen Windows virtuele machines in Azure | Microsoft Docs
description: Informatie over het gebruik van de Azure load balancer maken van een maximaal beschikbare en beveiligde toepassing over drie Windows VM 's
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 12/14/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 6eee852e703d25ccc4b13401c3e4ab46d09655da
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2017
---
# <a name="how-to-load-balance-windows-virtual-machines-in-azure-to-create-a-highly-available-application"></a>Het laden van een balans vinden tussen Windows virtuele machines in Azure een maximaal beschikbare toepassing maken
Taakverdeling biedt een hoger niveau van de beschikbaarheid van binnenkomende aanvragen verspreid over meerdere virtuele machines. In deze zelfstudie leert u over de verschillende onderdelen van de Azure load balancer die verkeer distribueren en bieden hoge beschikbaarheid. Procedures voor:

> [!div class="checklist"]
> * Een Azure load balancer maken
> * Een load balancer health test maken
> * Regels voor netwerkverkeer voor de load balancer maken
> * De extensie voor aangepaste scripts gebruiken om te maken van een normale IIS-website
> * Virtuele machines maken en koppelen aan een load balancer
> * Een load balancer in actie weergeven
> * Toevoegen of virtuele machines van een load balancer verwijderen

Voor deze zelfstudie is moduleversie 3,6 of hoger van Azure PowerShell vereist. Voer ` Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps).


## <a name="azure-load-balancer-overview"></a>Overzicht van Azure load balancer
Een Azure load balancer is een Layer-4 (TCP, UDP) load balancer die zorgt voor hoge beschikbaarheid door het distribueren van inkomend verkeer tussen orde virtuele machines. Een load balancer health test controleert een bepaalde poort op elke virtuele machine en wordt alleen verkeer naar een operationele virtuele machine.

U definieert een front-end-IP-configuratie met een of meer openbare IP-adressen. Deze front-end-IP-configuratie kunt de load balancer en toepassingen via Internet toegankelijk zijn. 

Virtuele machines verbinding maken met een load balancer met behulp van de virtuele netwerkinterfacekaart (NIC). Als u wilt distribueren verkeer naar de virtuele machines, een back-end-adresgroep bevat het IP-adressen van de virtuele (NIC's) die is verbonden met de load balancer.

Voor het beheren van de stroom van verkeer, kunt u regels van de load balancer voor bepaalde poorten en protocollen die zijn toegewezen aan uw virtuele machines definiëren.


## <a name="create-azure-load-balancer"></a>Azure load balancer maken
Deze sectie beschrijft hoe u kunt maken en configureren van elk onderdeel van de load balancer. Voordat u de load balancer maken kunt, maakt u een resourcegroep met [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroupLoadBalancer* in de *EastUS* locatie:

```powershell
New-AzureRmResourceGroup `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Location EastUS
```

### <a name="create-a-public-ip-address"></a>Een openbaar IP-adres maken
Voor toegang tot uw app op het Internet, moet u een openbaar IP-adres voor de load balancer. Maken van een openbaar IP-adres met [nieuw AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress). Het volgende voorbeeld wordt een openbaar IP-adres met de naam *myPublicIP* in de *myResourceGroupLoadBalancer* resourcegroep:

```powershell
$publicIP = New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Location EastUS `
  -AllocationMethod Static `
  -Name myPublicIP
```

### <a name="create-a-load-balancer"></a>Een load balancer maken
Maak een frontend-IP-adresgroep met [nieuw AzureRmLoadBalancerFrontendIpConfig](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig). Het volgende voorbeeld wordt een frontend-IP-adresgroep met de naam *myFrontEndPool* en koppelt u de *myPublicIP* adres: 

```powershell
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig `
  -Name myFrontEndPool `
  -PublicIpAddress $publicIP
```

Maak een back-end-adresgroep met [nieuw AzureRmLoadBalancerBackendAddressPoolConfig](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig). De virtuele machines koppelen aan deze back-endpool in de resterende stappen. Het volgende voorbeeld wordt een back-end-adresgroep met de naam *myBackEndPool*:

```powershell
$backendPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name myBackEndPool
```

Maak nu de load balancer met [nieuw AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer). Het volgende voorbeeld wordt een load balancer met de naam *myLoadBalancer* met behulp van de front-end- en back-end-IP-adresgroepen gemaakt in de voorgaande stappen:

```powershell
$lb = New-AzureRmLoadBalancer `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Name myLoadBalancer `
  -Location EastUS `
  -FrontendIpConfiguration $frontendIP `
  -BackendAddressPool $backendPool
```

### <a name="create-a-health-probe"></a>Een health test maken
Als u wilt toestaan dat de load balancer om de status van uw app te controleren, moet u een health test gebruiken. De health-test wordt dynamisch toegevoegd of verwijderd van virtuele machines van de load balancer draaiing op basis van hun reactie op statuscontroles. Standaard wordt een virtuele machine verwijderd uit de load balancer-distributie na twee opeenvolgende fouten met intervallen van 15 seconden. U maakt een health test op basis van een protocol of de pagina voor de controle van een specifieke status voor uw app. 

Het volgende voorbeeld wordt een TCP-test. U kunt ook aangepaste HTTP-tests voor meer fijnmazige statuscontroles maken. Wanneer u een aangepaste HTTP-test, moet u de pagina controle van status, zoals *healthcheck.aspx*. De test moet retourneren een **HTTP 200 OK** antwoord voor de load balancer moet worden gedraaid houden de host.

U gebruikt voor het maken van een TCP health test [toevoegen AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig). Het volgende voorbeeld wordt een health test met de naam *myHealthProbe* die elke VM op gecontroleerd *TCP* poort *80*:

```powershell
Add-AzureRmLoadBalancerProbeConfig `
  -Name myHealthProbe `
  -LoadBalancer $lb `
  -Protocol tcp `
  -Port 80 `
  -IntervalInSeconds 15 `
  -ProbeCount 2
```

Als u wilt toepassen op de health test, werken de load balancer met [Set AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):

```powershell
Set-AzureRmLoadBalancer -LoadBalancer $lb
```

### <a name="create-a-load-balancer-rule"></a>Een load balancer-regel maken
Een regel voor load balancer wordt gebruikt om te definiëren hoe verkeer wordt gedistribueerd naar de virtuele machines. Definieert u de front-end-IP-configuratie voor het binnenkomende verkeer en de back-end-IP-adresgroep voor het ontvangen van het verkeer, samen met de vereiste poort van de bron- en doelserver. Om er zeker van te zijn dat alleen orde virtuele machines verkeer ontvangen, moet u ook de health test gebruiken definiëren.

Maken van een regel voor load balancer met [toevoegen AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig). Het volgende voorbeeld wordt een regel voor load balancer met de naam *myLoadBalancerRule* en verkeer op een compromis tussen *TCP* poort *80*:

```powershell
$probe = Get-AzureRmLoadBalancerProbeConfig -LoadBalancer $lb -Name myHealthProbe

Add-AzureRmLoadBalancerRuleConfig `
  -Name myLoadBalancerRule `
  -LoadBalancer $lb `
  -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
  -BackendAddressPool $lb.BackendAddressPools[0] `
  -Protocol Tcp `
  -FrontendPort 80 `
  -BackendPort 80 `
  -Probe $probe
```

Bijwerken van de load balancer met [Set AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):

```powershell
Set-AzureRmLoadBalancer -LoadBalancer $lb
```


## <a name="configure-virtual-network"></a>Virtueel netwerk configureren
Voordat u een aantal virtuele machines implementeren en uw balancer kunt testen, moet u de resources van de ondersteunende virtueel netwerk maken. Zie voor meer informatie over virtuele netwerken van de [Azure Virtual Networks beheren](tutorial-virtual-network.md) zelfstudie.

### <a name="create-network-resources"></a>Maken van netwerkbronnen
Maak een virtueel netwerk met [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork). Het volgende voorbeeld wordt een virtueel netwerk met de naam *myVnet* met *mySubnet*:

```powershell
# Create subnet config
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
  -Name mySubnet `
  -AddressPrefix 192.168.1.0/24

# Create the virtual network
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Location EastUS `
  -Name myVnet `
  -AddressPrefix 192.168.0.0/16 `
  -Subnet $subnetConfig
```

Maken van een groep van de netwerkbeveiligingsregel met [nieuw AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig), maakt u een netwerkbeveiligingsgroep met [nieuw AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup). De netwerkbeveiligingsgroep toevoegen aan het subnet met [Set AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) en werk vervolgens het virtuele netwerk met [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork). 

Het volgende voorbeeld wordt een groep netwerkbeveiligingsregel met de naam *myNetworkSecurityGroup* en toegepast op *mySubnet*:

```powershell
# Create security rule config
$nsgRule = New-AzureRmNetworkSecurityRuleConfig `
  -Name myNetworkSecurityGroupRule `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 1001 `
  -SourceAddressPrefix * `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 80 `
  -Access Allow

# Create the network security group
$nsg = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Location EastUS `
  -Name myNetworkSecurityGroup `
  -SecurityRules $nsgRule

# Apply the network security group to a subnet
Set-AzureRmVirtualNetworkSubnetConfig `
  -VirtualNetwork $vnet `
  -Name mySubnet `
  -NetworkSecurityGroup $nsg `
  -AddressPrefix 192.168.1.0/24

# Update the virtual network
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

Virtuele NIC's zijn gemaakt met [nieuw AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface). Het volgende voorbeeld maakt drie virtuele NIC's. (Een virtuele NIC voor elke VM die u maakt voor uw app in de volgende stappen uit). U kunt extra virtuele NIC's en virtuele machines maken op elk gewenst moment en toe te voegen aan de load balancer:

```powershell
for ($i=1; $i -le 3; $i++)
{
   New-AzureRmNetworkInterface `
     -ResourceGroupName myResourceGroupLoadBalancer `
     -Name myNic$i `
     -Location EastUS `
     -Subnet $vnet.Subnets[0] `
     -LoadBalancerBackendAddressPool $lb.BackendAddressPools[0]
}
```

## <a name="create-virtual-machines"></a>Virtuele machines maken
Ter verbetering van de hoge beschikbaarheid van uw app in uw virtuele machines in een beschikbaarheidsset te plaatsen.

Maken van een beschikbaarheidsset met [nieuw AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset). Het volgende voorbeeld wordt een benoemde beschikbaarheidsset *myAvailabilitySet*:

```powershell
$availabilitySet = New-AzureRmAvailabilitySet `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Name myAvailabilitySet `
  -Location EastUS `
  -Managed `
  -PlatformFaultDomainCount 3 `
  -PlatformUpdateDomainCount 2
```

Stel dat een beheerder gebruikersnaam en wachtwoord voor de virtuele machines met [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```powershell
$cred = Get-Credential
```

Nu kunt u de virtuele machines met [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Het volgende voorbeeld maakt drie virtuele machines:

```powershell
for ($i=1; $i -le 3; $i++)
{
  $vm = New-AzureRmVMConfig `
    -VMName myVM$i `
    -VMSize Standard_D1 `
    -AvailabilitySetId $availabilitySet.Id
  $vm = Set-AzureRmVMOperatingSystem `
    -VM $vm `
    -Windows `
    -ComputerName myVM$i `
    -Credential $cred `
    -ProvisionVMAgent `
    -EnableAutoUpdate
  $vm = Set-AzureRmVMSourceImage `
    -VM $vm `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest
  $vm = Set-AzureRmVMOSDisk `
    -VM $vm `
    -Name myOsDisk$i `
    -DiskSizeInGB 128 `
    -CreateOption FromImage `
    -Caching ReadWrite
  $nic = Get-AzureRmNetworkInterface `
    -ResourceGroupName myResourceGroupLoadBalancer `
    -Name myNic$i
  $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
  New-AzureRmVM `
    -ResourceGroupName myResourceGroupLoadBalancer `
    -Location EastUS `
    -VM $vm
}
```

Het duurt enkele minuten maken en configureren van alle drie virtuele machines.

### <a name="install-iis-with-custom-script-extension"></a>IIS installeren met de extensie voor aangepaste scripts
In een vorige zelfstudie over [het aanpassen van een virtuele Windows-machine](tutorial-automate-vm-deployment.md), hebt u geleerd hoe u de aanpassing van de virtuele machine met een aangepast Script uitbreiding voor Windows te automatiseren. Dezelfde manier kunt u IIS installeren en configureren op uw virtuele machines.

Gebruik [Set AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) voor het installeren van de aangepaste Scriptextensie. De extensie wordt uitgevoerd `powershell Add-WindowsFeature Web-Server` voor het installeren van de IIS-webserver en updates van de *Default.htm* pagina voor het weergeven van de hostnaam van de virtuele machine:

```powershell
for ($i=1; $i -le 3; $i++)
{
   Set-AzureRmVMExtension `
     -ResourceGroupName myResourceGroupLoadBalancer `
     -ExtensionName IIS `
     -VMName myVM$i `
     -Publisher Microsoft.Compute `
     -ExtensionType CustomScriptExtension `
     -TypeHandlerVersion 1.4 `
     -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
     -Location EastUS
}
```

## <a name="test-load-balancer"></a>Test load balancer
Het openbare IP-adres van de load balancer met [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress). Het volgende voorbeeld verkrijgt het IP-adres voor *myPublicIP* eerder hebt gemaakt:

```powershell
Get-AzureRmPublicIPAddress `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Name myPublicIP | select IpAddress
```

Vervolgens kunt u het openbare IP-adres in invoeren aan een webbrowser. De website wordt weergegeven, inclusief de hostnaam van de virtuele machine die de load balancer verkeer naar het volgende voorbeeld gedistribueerde:

![Actieve IIS-website](./media/tutorial-load-balancer/running-iis-website.png)

Overzicht van de load balancer verkeer verdelen over alle drie virtuele machines waarop uw app wordt uitgevoerd, u kunt force vernieuwen van uw webbrowser.


## <a name="add-and-remove-vms"></a>Toevoegen en verwijderen van virtuele machines
U wilt uitvoeren van onderhoud op de virtuele machines waarop uw app, zoals het installeren van updates voor het besturingssysteem wordt uitgevoerd. Om verkeer naar uw app, moet u wellicht extra virtuele machines toevoegen. Deze sectie wordt beschreven hoe u verwijderen of toevoegen van een virtuele machine van de load balancer.

### <a name="remove-a-vm-from-the-load-balancer"></a>Een virtuele machine van de load balancer verwijderen
Ophalen van de netwerkinterfacekaart met [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface), stelt u de *loadbalancerbackendaddresspools gebruikt* eigenschap van de virtuele NIC *$null*. Tot slot werkt de virtuele NIC:

```powershell
$nic = Get-AzureRmNetworkInterface `
    -ResourceGroupName myResourceGroupLoadBalancer `
    -Name myNic2
$nic.Ipconfigurations[0].LoadBalancerBackendAddressPools=$null
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

Als u wilt zien van de load balancer verkeer verdelen over de resterende twee virtuele machines waarop uw app wordt uitgevoerd. u kunt force vernieuwen uw webbrowser. U kunt nu onderhoud uitvoeren op de virtuele machine, zoals het installeren van updates voor het besturingssysteem of het uitvoeren van een VM opnieuw wordt opgestart.

### <a name="add-a-vm-to-the-load-balancer"></a>Een virtuele machine toevoegen aan de load balancer
Na het uitvoeren van onderhoud van de virtuele machine of als u meer capaciteit wilt, stelt u de *loadbalancerbackendaddresspools gebruikt* eigenschap van de virtuele NIC de *BackendAddressPool* van [Get-AzureRMLoadBalancer](/powershell/module/azurerm.network/get-azurermloadbalancer):

De load balancer ophalen:

```powershell
$lb = Get-AzureRMLoadBalancer `
    -ResourceGroupName myResourceGroupLoadBalancer `
    -Name myLoadBalancer 
$nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$lb.BackendAddressPools[0]
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt gemaakt van een load balancer en virtuele machines is gekoppeld. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Een Azure load balancer maken
> * Een load balancer health test maken
> * Regels voor netwerkverkeer voor de load balancer maken
> * De extensie voor aangepaste scripts gebruiken om te maken van een normale IIS-website
> * Virtuele machines maken en koppelen aan een load balancer
> * Een load balancer in actie weergeven
> * Toevoegen of virtuele machines van een load balancer verwijderen

Ga naar de volgende zelfstudie voor informatie over het beheren van VM-netwerken.

> [!div class="nextstepaction"]
> [Virtuele machines en virtuele netwerken beheren](./tutorial-virtual-network.md)
