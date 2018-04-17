---
title: Maken van een virtuele-Machineschaalsets voor Windows in Azure | Microsoft Docs
description: Maken en implementeren van een maximaal beschikbare toepassing op Windows-VM's met behulp van een virtuele-machineschaalset
services: virtual-machine-scale-sets
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: ''
ms.topic: article
ms.date: 03/29/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: a4a2d8d616a503a2b200627a52103b78e573f767
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="create-a-virtual-machine-scale-set-and-deploy-a-highly-available-app-on-windows"></a>Een virtuele-Machineschaalset maken en implementeren van een maximaal beschikbare app in Windows
Met een virtuele-machineschaalset kunt u een reeks identieke virtuele machines met automatisch schalen implementeren en beheren. U kunt het aantal VM’s in de schaalset handmatig schalen of regels voor automatisch schalen definiëren op basis van resourcegebruik zoals CPU, vraag naar geheugen, of netwerkverkeer. In deze zelfstudie implementeert u een virtuele-machineschaalset in Azure. In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * De extensie voor aangepaste scripts gebruiken voor het definiëren van een IIS-site om te schalen
> * Een load balancer voor uw scale set maken
> * Een virtuele-machineschaalset maken
> * Het aantal instanties in een schaalset vergroten of verkleinen
> * Regels voor automatisch schalen maken

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Als u wilt installeren en gebruiken van de PowerShell lokaal, wordt in deze zelfstudie Azure PowerShell moduleversie 5,6 of hoger vereist. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Login-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.


## <a name="scale-set-overview"></a>Overzicht van schaalsets
Met een virtuele-machineschaalset kunt u een reeks identieke virtuele machines met automatisch schalen implementeren en beheren. Virtuele machines in een schaalset worden verdeeld over logische fout- en updatedomeinen in een of meer *plaatsingsgroepen*. Dit zijn groepen van op soortgelijke wijze geconfigureerde virtuele machines, vergelijkbaar met [beschikbaarheidssets](tutorial-availability-sets.md).

Virtuele machines worden in een schaalset gemaakt als dat nodig is. U definieert regels voor automatisch schalen om te bepalen hoe en wanneer virtuele machines worden toegevoegd of verwijderd uit de schaalset. Deze regels kunnen activeren op basis van metrische gegevens zoals CPU-belasting, geheugengebruik of netwerkverkeer.

Schaalsets bieden ondersteuning voor maximaal 1000 VM’s wanneer u een Azure-platforminstallatiekopie gebruikt. Voor werkbelastingen met aanzienlijke installatie- of VM-aanpassingsvereisten wilt u mogelijk [een aangepaste VM-installatiekopie maken](tutorial-custom-images.md). U kunt maximaal 300 virtuele machines in een schaalset maken wanneer u een aangepaste installatiekopie gebruikt.


## <a name="create-a-scale-set"></a>Een schaalset maken
Maak een schaalset met virtuele machines met behulp van [New-AzureRmVmss](/powershell/module/azurerm.compute/new-azurermvmss). In het volgende voorbeeld wordt een schaalset met de naam *myScaleSet* gemaakt, die gebruikmaakt van de platforminstallatiekopie van *Windows Server 2016 Datacenter*. De Azure-netwerkresources voor het virtuele netwerk, het openbare IP-adres en de load balancer worden automatisch gemaakt. Geef desgevraagd uw eigen beheerdersreferenties op voor de VM-exemplaren in de schaalset:

```azurepowershell-interactive
New-AzureRmVmss `
  -ResourceGroupName "myResourceGroupScaleSet" `
  -Location "EastUS" `
  -VMScaleSetName "myScaleSet" `
  -VirtualNetworkName "myVnet" `
  -SubnetName "mySubnet" `
  -PublicIpAddressName "myPublicIPAddress" `
  -LoadBalancerName "myLoadBalancer" `
  -UpgradePolicy "Automatic"
```

Het duurt enkele minuten om alle schaalsetresources en VM's te maken en te configureren.


## <a name="deploy-sample-application"></a>Voorbeeldtoepassing implementeren
Als u de schaalset wilt testen, installeert u een eenvoudige webtoepassing. U kunt de aangepaste scriptextensie van Azure downloaden en een script uitvoeren waarmee IIS wordt geïnstalleerd in de VM-exemplaren. Deze uitbreiding is handig voor post-implementatieconfiguraties, software-installaties of andere configuratie-/beheertaken. Zie voor meer informatie het [overzicht van de aangepaste scriptextensie](extensions-customscript.md).

Gebruik de aangepaste scriptextensie om een eenvoudige IIS-webserver te installeren. Pas de aangepaste scriptextensie waarmee IIS wordt geïnstalleerd als volgt toe:

```azurepowershell-interactive
# Define the script for your Custom Script Extension to run
$publicSettings = @{
    "fileUris" = (,"https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/automate-iis.ps1");
    "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File automate-iis.ps1"
}

# Get information about the scale set
$vmss = Get-AzureRmVmss `
            -ResourceGroupName "myResourceGroupScaleSet" `
            -VMScaleSetName "myScaleSet"

# Use Custom Script Extension to install IIS and configure basic website
Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmss `
    -Name "customScript" `
    -Publisher "Microsoft.Compute" `
    -Type "CustomScriptExtension" `
    -TypeHandlerVersion 1.8 `
    -Setting $publicSettings

# Update the scale set and apply the Custom Script Extension to the VM instances
Update-AzureRmVmss `
    -ResourceGroupName "myResourceGroupScaleSet" `
    -Name "myScaleSet" `
    -VirtualMachineScaleSet $vmss
```


## <a name="test-your-scale-set"></a>Uw schaalset testen
Om te zien van de schaal in actie is ingesteld, wordt het openbare IP-adres van de load balancer met [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress). Het volgende voorbeeld verkrijgt het IP-adres voor *myPublicIP* gemaakt als onderdeel van de schaal is ingesteld:

```azurepowershell-interactive
Get-AzureRmPublicIPAddress `
    -ResourceGroupName "myResourceGroupScaleSet" `
    -Name "myPublicIPAddress" | select IpAddress
```

Voer in een webbrowser het openbare IP-adres in. De web-app wordt weergegeven, inclusief de hostnaam van de virtuele machine die de load balancer verkeer naar gedistribueerde:

![Actieve IIS-site](./media/tutorial-create-vmss/running-iis-site.png)

Als u de schaalset in actie wilt zien, kunt u vernieuwing van uw webbrowser afdwingen om te zien dat de load balancer verkeer verdeelt over alle virtuele machines waarop uw app wordt uitgevoerd.


## <a name="management-tasks"></a>Beheertaken
Tijdens de levenscyclus van de schaalset moet u mogelijk een of meer beheertaken uitvoeren. Bovendien wilt u misschien scripts maken die verschillende levenscyclustaken automatiseren. Azure PowerShell biedt een snelle manier om deze taken. Hier volgen enkele algemene taken.

### <a name="view-vms-in-a-scale-set"></a>Virtuele machines weergeven in een schaalset
Als u een overzicht wilt zien van de VM-exemplaren in een schaalset, gebruikt u [Get-AzureRmVmssVM](/powershell/module/azurerm.compute/get-azurermvmssvm) als volgt:

```azurepowershell-interactive
Get-AzureRmVmssVM -ResourceGroupName "myResourceGroupScaleSet" -VMScaleSetName "myScaleSet"
```

In de volgende voorbeelduitvoer ziet u dat de schaalset twee VM-exemplaren bevat:

```powershell
ResourceGroupName                 Name Location             Sku InstanceID ProvisioningState
-----------------                 ---- --------             --- ---------- -----------------
MYRESOURCEGROUPSCALESET   myScaleSet_0   eastus Standard_DS1_v2          0         Succeeded
MYRESOURCEGROUPSCALESET   myScaleSet_1   eastus Standard_DS1_v2          1         Succeeded
```

Als u extra informatie wilt weergeven over een specifiek VM-exemplaar, voegt u de parameter `-InstanceId` toe aan [Get-AzureRmVmssVM](/powershell/module/azurerm.compute/get-azurermvmssvm). In het volgende voorbeeld wordt informatie opgevraagd over het VM-exemplaar *1*:

```azurepowershell-interactive
Get-AzureRmVmssVM -ResourceGroupName "myResourceGroupScaleSet" -VMScaleSetName "myScaleSet" -InstanceId "1"
```


### <a name="increase-or-decrease-vm-instances"></a>VM-instanties vergroten of verkleinen
Als het aantal exemplaren dat u momenteel in een schaalset hebt wilt weergeven, gebruikt [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss) en query's uitvoeren op *sku.capacity*:

```azurepowershell-interactive
Get-AzureRmVmss -ResourceGroupName "myResourceGroupScaleSet" `
    -VMScaleSetName "myScaleSet" | `
    Select -ExpandProperty Sku
```

U kunt vervolgens handmatig vergroten of verkleinen het aantal virtuele machines in de schaal in te stellen [Update AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss). In het volgende voorbeeld wordt het aantal VM's in uw schaal ingesteld op *3*:

```azurepowershell-interactive
# Get current scale set
$scaleset = Get-AzureRmVmss `
  -ResourceGroupName "myResourceGroupScaleSet" `
  -VMScaleSetName "myScaleSet"

# Set and update the capacity of your scale set
$scaleset.sku.capacity = 3
Update-AzureRmVmss -ResourceGroupName "myResourceGroupScaleSet" `
    -Name "myScaleSet" `
    -VirtualMachineScaleSet $scaleset
```

Als duurt een paar minuten bijwerken van het opgegeven aantal exemplaren in uw scale is ingesteld.


### <a name="configure-autoscale-rules"></a>Regels voor automatisch schalen configureren
In plaats van het aantal exemplaren handmatig schalen in uw scale is ingesteld, kunt u regels voor automatisch schalen definiëren. Deze regels bewaken de instanties in uw schaalset en reageren hierop op basis van metrische gegevens en drempels die u definieert. In het volgende voorbeeld wordt het aantal instanties opgeschaald met één wanneer de gemiddelde CPU-belasting gedurende een periode van 5 minuten hoger is dan 60%. Als de gemiddelde CPU-belasting vervolgens onder 30% gedurende een periode van 5 minuten, worden de instanties geschaald in door één exemplaar:

```azurepowershell-interactive
# Define your scale set information
$mySubscriptionId = (Get-AzureRmSubscription)[0].Id
$myResourceGroup = "myResourceGroupScaleSet"
$myScaleSet = "myScaleSet"
$myLocation = "East US"
$myScaleSetId = (Get-AzureRmVmss -ResourceGroupName $myResourceGroup -VMScaleSetName $myScaleSet).Id 

# Create a scale up rule to increase the number instances after 60% average CPU usage exceeded for a 5-minute period
$myRuleScaleUp = New-AzureRmAutoscaleRule `
  -MetricName "Percentage CPU" `
  -MetricResourceId $myScaleSetId `
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
  -MetricResourceId $myScaleSetId `
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
  -TargetResourceId $myScaleSetId `
  -AutoscaleProfiles $myScaleProfile
```

Zie [aanbevolen procedures voor automatisch schalen](/azure/architecture/best-practices/auto-scaling) voor meer ontwerpinformatie over het gebruik van automatisch schalen.


## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u een virtuele-machineschaalset gemaakt. U hebt geleerd hoe u:

> [!div class="checklist"]
> * De extensie voor aangepaste scripts gebruiken voor het definiëren van een IIS-site om te schalen
> * Een load balancer voor uw scale set maken
> * Een virtuele-machineschaalset maken
> * Het aantal instanties in een schaalset vergroten of verkleinen
> * Regels voor automatisch schalen maken

Ga naar de volgende zelfstudie voor meer informatie over load balancing-concepten voor virtuele machines.

> [!div class="nextstepaction"]
> [Werklasten verdelen over virtuele machines](tutorial-load-balancer.md)
