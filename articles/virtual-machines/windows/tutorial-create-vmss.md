---
title: Maken van een virtuele-Machineschaalsets voor Windows in Azure | Microsoft Docs
description: Maken en implementeren van een maximaal beschikbare toepassing op Windows-VM's met behulp van een virtuele-machineschaalset
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: 
ms.topic: article
ms.date: 12/15/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: d190d046f7572c51df0c5c9e14e14a41d93e3248
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="create-a-virtual-machine-scale-set-and-deploy-a-highly-available-app-on-windows"></a>Een virtuele-Machineschaalset maken en implementeren van een maximaal beschikbare app in Windows
Een virtuele-machineschaalset kunt u om te implementeren en beheren van een reeks identiek zijn, automatisch schalen virtuele machines. U kunt het aantal virtuele machines in de schaalset handmatig schalen of regels automatisch te schalen op basis van het gebruik van bronnen zoals CPU, geheugen-aanvraag of netwerkverkeer definiëren. In deze zelfstudie maakt implementeren u een virtuele-machineschaalset instellen in Azure. Procedures voor:

> [!div class="checklist"]
> * De extensie voor aangepaste scripts gebruiken voor het definiëren van een IIS-site om te schalen
> * Een load balancer voor uw scale set maken
> * Maken van een virtuele-machineschaalset
> * Vergroten of verkleinen het aantal exemplaren in een schaalset
> * Maken van regels voor automatisch schalen

Deze zelfstudie vereist de Azure PowerShell moduleversie 5.1.1 of hoger. Voer ` Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps).


## <a name="scale-set-overview"></a>Overzicht van Scale Set
Een virtuele-machineschaalset kunt u om te implementeren en beheren van een reeks identiek zijn, automatisch schalen virtuele machines. Virtuele machines in een schaalset worden gedistribueerd over logica probleem- en domeinen in een of meer *plaatsing groepen*. Dit zijn groepen op vergelijkbare wijze geconfigureerde virtuele machines, vergelijkbaar met [beschikbaarheidssets](tutorial-availability-sets.md).

Virtuele machines worden gemaakt als nodig in een schaalset. Definieert u de regels voor automatisch schalen om te bepalen hoe en wanneer virtuele machines worden toegevoegd of verwijderd uit de schaalaanpassingsset. Deze regels kunnen activeren op basis van metrische gegevens zoals CPU-belasting, geheugengebruik of netwerkverkeer.

Schaal wordt ondersteuning voor maximaal 1000 VMs ingesteld wanneer u een installatiekopie van een Azure-platform. Voor werkbelastingen met aanzienlijke installatie of VM aanpassingsvereisten mogelijk wilt u [maken van een aangepaste VM-installatiekopie](tutorial-custom-images.md). U kunt maximaal 300 virtuele machines in een schaal instelt met behulp van een aangepaste installatiekopie maken.


## <a name="create-an-app-to-scale"></a>Maak een app schalen
Voordat u een schaalset maken kunt, maakt u een resourcegroep met [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroupAutomate* in de *EastUS* locatie:

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroupScaleSet -Location EastUS
```

In een eerdere zelfstudie hebt u geleerd hoe u [automatiseren VM-configuratie](tutorial-automate-vm-deployment.md) met behulp van de aangepaste Scriptextensie. Maken van een scale set-configuratie en vervolgens een aangepaste Scriptextensie voor IIS installeren en configureren van toepassing:

```powershell
# Create a config object
$vmssConfig = New-AzureRmVmssConfig `
    -Location EastUS `
    -SkuCapacity 2 `
    -SkuName Standard_DS2 `
    -UpgradePolicyMode Automatic

# Define the script for your Custom Script Extension to run
$publicSettings = @{
    "fileUris" = (,"https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/automate-iis.ps1");
    "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File automate-iis.ps1"
}

# Use Custom Script Extension to install IIS and configure basic website
Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmssConfig `
    -Name "customScript" `
    -Publisher "Microsoft.Compute" `
    -Type "CustomScriptExtension" `
    -TypeHandlerVersion 1.8 `
    -Setting $publicSettings
```

## <a name="create-scale-load-balancer"></a>Schaal load balancer maken
Een Azure load balancer is een Layer-4 (TCP, UDP) load balancer die zorgt voor hoge beschikbaarheid door het distribueren van inkomend verkeer tussen orde virtuele machines. Een load balancer health test controleert een bepaalde poort op elke virtuele machine en wordt alleen verkeer naar een operationele virtuele machine. Zie voor meer informatie de volgende zelfstudie op [het laden van een balans vinden tussen virtuele machines van Windows](tutorial-load-balancer.md).

Maak een load balancer die een openbaar IP-adres en webverkeer op poort 80 distribueert:

```powershell
# Create a public IP address
$publicIP = New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroupScaleSet `
  -Location EastUS `
  -AllocationMethod Static `
  -Name myPublicIP

# Create a frontend and backend IP pool
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig `
  -Name myFrontEndPool `
  -PublicIpAddress $publicIP
$backendPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name myBackEndPool

# Create the load balancer
$lb = New-AzureRmLoadBalancer `
  -ResourceGroupName myResourceGroupScaleSet `
  -Name myLoadBalancer `
  -Location EastUS `
  -FrontendIpConfiguration $frontendIP `
  -BackendAddressPool $backendPool

# Create a load balancer health probe on port 80
Add-AzureRmLoadBalancerProbeConfig -Name myHealthProbe `
  -LoadBalancer $lb `
  -Protocol tcp `
  -Port 80 `
  -IntervalInSeconds 15 `
  -ProbeCount 2

# Create a load balancer rule to distribute traffic on port 80
Add-AzureRmLoadBalancerRuleConfig `
  -Name myLoadBalancerRule `
  -LoadBalancer $lb `
  -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
  -BackendAddressPool $lb.BackendAddressPools[0] `
  -Protocol Tcp `
  -FrontendPort 80 `
  -BackendPort 80

# Update the load balancer configuration
Set-AzureRmLoadBalancer -LoadBalancer $lb
```

## <a name="create-a-scale-set"></a>Maken van een schaalset
Maak nu de schaal van een virtuele machine in te stellen [nieuw AzureRmVmss](/powershell/module/azurerm.compute/new-azurermvm). Het volgende voorbeeld wordt een set met de naam scale *myScaleSet*:

```powershell
# Reference a virtual machine image from the gallery
Set-AzureRmVmssStorageProfile $vmssConfig `
  -ImageReferencePublisher MicrosoftWindowsServer `
  -ImageReferenceOffer WindowsServer `
  -ImageReferenceSku 2016-Datacenter `
  -ImageReferenceVersion latest

# Set up information for authenticating with the virtual machine
Set-AzureRmVmssOsProfile $vmssConfig `
  -AdminUsername azureuser `
  -AdminPassword P@ssword! `
  -ComputerNamePrefix myVM

# Create the virtual network resources
$subnet = New-AzureRmVirtualNetworkSubnetConfig `
  -Name "mySubnet" `
  -AddressPrefix 10.0.0.0/24
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName "myResourceGroupScaleSet" `
  -Name "myVnet" `
  -Location "EastUS" `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $subnet
$ipConfig = New-AzureRmVmssIpConfig `
  -Name "myIPConfig" `
  -LoadBalancerBackendAddressPoolsId $lb.BackendAddressPools[0].Id `
  -SubnetId $vnet.Subnets[0].Id

# Attach the virtual network to the config object
Add-AzureRmVmssNetworkInterfaceConfiguration `
  -VirtualMachineScaleSet $vmssConfig `
  -Name "network-config" `
  -Primary $true `
  -IPConfiguration $ipConfig

# Create the scale set with the config object (this step might take a few minutes)
New-AzureRmVmss `
  -ResourceGroupName myResourceGroupScaleSet `
  -Name myScaleSet `
  -VirtualMachineScaleSet $vmssConfig
```

Het duurt enkele minuten voor het maken en configureren van de schaal set resources en virtuele machines.


## <a name="test-your-app"></a>Uw app testen
Overzicht van uw IIS-website in actie verkrijgen van het openbare IP-adres van de load balancer met [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress). Het volgende voorbeeld verkrijgt het IP-adres voor *myPublicIP* gemaakt als onderdeel van de schaal is ingesteld:

```powershell
Get-AzureRmPublicIPAddress -ResourceGroupName myResourceGroupScaleSet -Name myPublicIP | select IpAddress
```

Geef het openbare IP-adres in naar een webbrowser. De app wordt weergegeven, inclusief de hostnaam van de virtuele machine die de load balancer verkeer naar gedistribueerde:

![Actieve IIS-website](./media/tutorial-create-vmss/running-iis-site.png)

Overzicht van de schaal instelt in actie, u kunt force vernieuwen uw webbrowser als u wilt zien van de load balancer verkeer verdelen over alle virtuele machines waarop uw app wordt uitgevoerd.


## <a name="management-tasks"></a>Beheertaken
Gedurende de levenscyclus van de schaal is ingesteld, moet u wellicht een of meer beheertaken uitvoeren. Bovendien wilt u scripts maken die verschillende lifecycle-taken automatiseren. Azure PowerShell biedt een snelle manier om deze taken. Hier volgen enkele algemene taken.

### <a name="view-vms-in-a-scale-set"></a>Weergave virtuele machines in een schaalset
Een lijst van virtuele machines die worden uitgevoerd in de schaalset wilt weergeven, gebruikt u [Get-AzureRmVmssVM](/powershell/module/azurerm.compute/get-azurermvmssvm) als volgt:

```powershell
# Get current scale set
$scaleset = Get-AzureRmVmss `
  -ResourceGroupName myResourceGroupScaleSet `
  -VMScaleSetName myScaleSet

# Loop through the instanaces in your scale set
for ($i=1; $i -le ($scaleset.Sku.Capacity - 1); $i++) {
    Get-AzureRmVmssVM -ResourceGroupName myResourceGroupScaleSet `
      -VMScaleSetName myScaleSet `
      -InstanceId $i
}
```


### <a name="increase-or-decrease-vm-instances"></a>Vergroten of verkleinen van VM-exemplaren
Als het aantal exemplaren dat u momenteel in een schaalset hebt wilt weergeven, gebruikt [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss) en query's uitvoeren op *sku.capacity*:

```powershell
Get-AzureRmVmss -ResourceGroupName myResourceGroupScaleSet `
    -VMScaleSetName myScaleSet | `
    Select -ExpandProperty Sku
```

U kunt vervolgens handmatig vergroten of verkleinen het aantal virtuele machines in de schaal in te stellen [Update AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss). Het volgende voorbeeld wordt het aantal VM's in uw ingesteld op schaal *3*:

```powershell
# Get current scale set
$scaleset = Get-AzureRmVmss `
  -ResourceGroupName myResourceGroupScaleSet `
  -VMScaleSetName myScaleSet

# Set and update the capacity of your scale set
$scaleset.sku.capacity = 3
Update-AzureRmVmss -ResourceGroupName myResourceGroupScaleSet `
    -Name myScaleSet `
    -VirtualMachineScaleSet $scaleset
```

Als duurt een paar minuten bijwerken van het opgegeven aantal exemplaren in uw scale is ingesteld.


### <a name="configure-autoscale-rules"></a>Regels voor automatisch schalen configureren
In plaats van het aantal exemplaren handmatig schalen in uw scale is ingesteld, kunt u regels voor automatisch schalen definiëren. Deze regels de exemplaren in de schaalset bewaken en hierop reageren dienovereenkomstig op basis van metrische gegevens en drempels die u definieert. Het volgende voorbeeld uitgeschaald het aantal exemplaren van een wanneer de gemiddelde CPU-belasting groter dan 60% gedurende een periode van 5 minuten is. Als de gemiddelde CPU-belasting vervolgens onder 30% gedurende een periode van 5 minuten, worden de instanties geschaald in door één exemplaar:

```powershell
# Define your scale set information
$mySubscriptionId = (Get-AzureRmSubscription).Id
$myResourceGroup = "myResourceGroupScaleSet"
$myScaleSet = "myScaleSet"
$myLocation = "East US"

# Create a scale up rule to increase the number instances after 60% average CPU usage exceeded for a 5-minute period
$myRuleScaleUp = New-AzureRmAutoscaleRule `
  -MetricName "Percentage CPU" `
  -MetricResourceId /subscriptions/$mySubscriptionId/resourceGroups/$myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/$myScaleSet `
  -Operator GreaterThan `
  -MetricStatistic Average `
  -Threshold 60 `
  -TimeGrain 00:01:00 `
  -TimeWindow 00:05:00 `
  -ScaleActionCooldown 00:05:00 `
  -ScaleActionDirection Increase `
  -ScaleActionValue 1

# Create a scale down rule to decrease the number of instances after 30% average CPU usage over a 5-minute period
$myRuleScaleDown = New-AzureRmAutoscaleRule `
  -MetricName "Percentage CPU" `
  -MetricResourceId /subscriptions/$mySubscriptionId/resourceGroups/$myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/$myScaleSet `
  -Operator LessThan `
  -MetricStatistic Average `
  -Threshold 30 `
  -TimeGrain 00:01:00 `
  -TimeWindow 00:05:00 `
  -ScaleActionCooldown 00:05:00 `
  -ScaleActionDirection Decrease `
  -ScaleActionValue 1

# Create a scale profile with your scale up and scale down rules
$myScaleProfile = New-AzureRmAutoscaleProfile `
  -DefaultCapacity 2  `
  -MaximumCapacity 10 `
  -MinimumCapacity 2 `
  -Rules $myRuleScaleUp,$myRuleScaleDown `
  -Name "autoprofile"

# Apply the autoscale rules
Add-AzureRmAutoscaleSetting `
  -Location $myLocation `
  -Name "autosetting" `
  -ResourceGroup $myResourceGroup `
  -TargetResourceId /subscriptions/$mySubscriptionId/resourceGroups/$myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/$myScaleSet `
  -AutoscaleProfiles $myScaleProfile
```

Zie voor meer ontwerpinformatie over het gebruik van automatisch schalen, [aanbevolen procedures voor automatisch schalen](/azure/architecture/best-practices/auto-scaling).


## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie maakt u een virtuele-machineschaalset gemaakt. U hebt geleerd hoe u:

> [!div class="checklist"]
> * De extensie voor aangepaste scripts gebruiken voor het definiëren van een IIS-site om te schalen
> * Een load balancer voor uw scale set maken
> * Maken van een virtuele-machineschaalset
> * Vergroten of verkleinen het aantal exemplaren in een schaalset
> * Maken van regels voor automatisch schalen

Ga naar de volgende zelfstudie voor meer informatie over taakverdeling concepten voor virtuele machines.

> [!div class="nextstepaction"]
> [Load balance virtuele machines](tutorial-load-balancer.md)
