---
title: Zelfstudie - Toepassingen installeren in een schaalset met Azure PowerShell | Microsoft Docs
description: Informatie over het gebruik van Azure PowerShell om toepassingen te installeren in een virtuele-machineschaalsets met de aangepaste scriptextensie
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/27/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 4f162dcf58316e6d9f39b71be37abf5626e93c75
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38295821"
---
# <a name="tutorial-install-applications-in-virtual-machine-scale-sets-with-azure-powershell"></a>Zelfstudie - Toepassingen installeren in een schaalset met Azure PowerShell
Als u toepassingen wilt uitvoeren op de exemplaren van een virtuele machine (VM) in een schaalset, moet u eerst de toepassingsonderdelen en de vereiste bestanden installeren. In een vorige zelfstudie hebt u geleerd om een aangepaste VM-installatiekopie te maken en te gebruiken voor het implementeren van uw VM-exemplaren. Deze aangepaste installatiekopie bevat handmatige installaties van toepassingen en configuraties. U kunt de installatie van toepassingen op een schaalset ook automatiseren nadat elk VM-exemplaar is geïmplementeerd. Bovendien kunt u toepassingen bijwerken die al worden uitgevoerd in een schaalset. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Automatisch toepassingen installeren in een schaalset
> * De aangepaste scriptextensie van Azure gebruiken
> * Een actieve toepassing in een schaalset bijwerken

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Als u PowerShell lokaal wilt installeren en gebruiken, is voor deze zelfstudie versie 6.0.0 of hoger van de Azure PowerShell-module vereist. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure. 


## <a name="what-is-the-azure-custom-script-extension"></a>Wat is de aangepaste scriptextensie van Azure?
Met de aangepaste scriptextensie kunnen scripts worden gedownload en uitgevoerd op virtuele machines in Azure. Deze uitbreiding is handig voor post-implementatieconfiguraties, software-installaties of andere configuratie-/beheertaken. Scripts kunnen worden gedownload uit Azure Storage of GitHub, of worden geleverd in Azure Portal tijdens de uitvoering van extensies.

De aangepaste scriptextensie kan worden geïntegreerd met Azure Resource Manager-sjablonen en ook worden uitgevoerd met Azure CLI 2.0, Azure PowerShell, Azure Portal of de REST API. Zie voor meer informatie het [overzicht van de aangepaste scriptextensie](../virtual-machines/windows/extensions-customscript.md).

Als u de aangepaste scriptextensie in actie wilt zien, maakt u een schaalset die de IIS-webserver installeert en de hostnaam levert van het schaalset-VM-exemplaar. De definitie van de aangepaste scriptextensie downloadt een voorbeeldscript vanuit GitHub, installeert de vereiste pakketten en schrijft de hostnaam van het VM-exemplaar naar een standaard-HTML-pagina.


## <a name="create-a-scale-set"></a>Een schaalset maken
Maak nu een virtuele-machineschaalset met [New-AzureRmVmss](/powershell/module/azurerm.compute/new-azurermvmss). Om het verkeer te distribueren naar de verschillende VM-exemplaren, wordt er ook een load balancer gemaakt. De load balancer bevat regels voor het distribueren van verkeer op TCP-poort 80, en voor het toestaan van verkeer van Extern bureaublad op TCP-poort 3389 en externe toegang via PowerShell op TCP-poort 5985. Geef desgevraagd uw eigen beheerdersreferenties op voor de VM-exemplaren in de schaalset:

```azurepowershell-interactive
New-AzureRmVmss `
  -ResourceGroupName "myResourceGroup" `
  -VMScaleSetName "myScaleSet" `
  -Location "EastUS" `
  -VirtualNetworkName "myVnet" `
  -SubnetName "mySubnet" `
  -PublicIpAddressName "myPublicIPAddress" `
  -LoadBalancerName "myLoadBalancer" `
  -UpgradePolicyMode "Automatic"
```

Het duurt enkele minuten om alle schaalsetresources en VM's te maken en te configureren.


## <a name="create-custom-script-extension-definition"></a>Definitie van de aangepaste scriptextensie maken
Azure PowerShell gebruikt een hashtabel voor het opslaan van het bestand dat moet worden gedownload en de opdracht die moet worden uitgevoerd. In het volgende voorbeeld wordt een voorbeeld van een script vanuit GitHub gebruikt. Maak eerst dit configuratieobject. Het moet er zo uitzien:

```azurepowershell-interactive
$customConfig = @{
  "fileUris" = (,"https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/automate-iis.ps1");
  "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File automate-iis.ps1"
}
```

Pas nu de aangepaste scriptextensie toe met [AzureRmVmssExtension](/powershell/module/AzureRM.Compute/Add-AzureRmVmssExtension). Het eerder gedefinieerde configuratieobject wordt doorgegeven aan de extensie. Werk de extensie voor de VM-exemplaren bij en voer deze uit met [Update-AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss).

```azurepowershell-interactive
# Get information about the scale set
$vmss = Get-AzureRmVmss `
          -ResourceGroupName "myResourceGroup" `
          -VMScaleSetName "myScaleSet"

# Add the Custom Script Extension to install IIS and configure basic website
$vmss = Add-AzureRmVmssExtension `
  -VirtualMachineScaleSet $vmss `
  -Name "customScript" `
  -Publisher "Microsoft.Compute" `
  -Type "CustomScriptExtension" `
  -TypeHandlerVersion 1.9 `
  -Setting $customConfig

# Update the scale set and apply the Custom Script Extension to the VM instances
Update-AzureRmVmss `
  -ResourceGroupName "myResourceGroup" `
  -Name "myScaleSet" `
  -VirtualMachineScaleSet $vmss
```

Elk VM-exemplaar in de schaalset downloadt het script vanuit GitHub en voert het uit. In een meer complex voorbeeld kunnen meerdere toepassingsonderdelen en bestanden worden geïnstalleerd. Als de schaalset omhoog wordt geschaald, passen de VM-exemplaren automatisch dezelfde definitie van de aangepaste scriptextensie toe en installeren deze de vereiste toepassing.


## <a name="test-your-scale-set"></a>Uw schaalset testen
Als u de webserver in actie wilt zien, achterhaalt u het openbare IP-adres van de load balancer met [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress). In het volgende voorbeeld wordt het IP-adres achterhaald dat is gemaakt in de resourcegroep *myResourceGroup*:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress -ResourceGroupName "myResourceGroup" | Select IpAddress
```

Voer in een webbrowser het openbare IP-adres van de load balancer in. Via de load balancer wordt verkeer naar een van uw VM-instanties gedistribueerd, zoals wordt weergegeven in het volgende voorbeeld:

![Basiswebpagina in IIS](media/tutorial-install-apps-powershell/running-iis.png)

Sluit de webbrowser niet af, zodat u in de volgende stap een bijgewerkte versie ziet.


## <a name="update-app-deployment"></a>App-implementatie bijwerken
Gedurende de levenscyclus van een schaalset moet u wellicht een bijgewerkte versie van uw toepassing implementeren. U kunt met de aangepaste scriptextensie verwijzen naar een bijgewerkt implementatiescript en vervolgens de extensie opnieuw op uw schaalset toepassen. Bij het maken van de schaalset in een vorige stap is `-UpgradePolicyMode` ingesteld op *Automatic*. Met deze instelling kunnen de VM-exemplaren in de schaalset automatisch de meest recente versie van uw toepassing bijwerken en toepassen.

Maak een nieuwe definitie van een configuratie met de naam *customConfigv2*. Deze definitie voert een bijgewerkte versie *v2* van het installatiescript van de toepassing uit:

```azurepowershell-interactive
$customConfigv2 = @{
  "fileUris" = (,"https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/automate-iis-v2.ps1");
  "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File automate-iis-v2.ps1"
}
```

Pas opnieuw de configuratie van de aangepaste scriptextensie toe op de VM-exemplaren in uw schaalset met [Add-AzureRmVmssExtension](/powershell/module/AzureRM.Compute/Add-AzureRmVmssExtension). De definitie *customConfigv2* wordt gebruikt voor het toepassen van de bijgewerkte versie van de toepassing:

```azurepowershell-interactive
# Reapply the Custom Script Extension to install the updated website
$vmss = Add-AzureRmVmssExtension `
  -VirtualMachineScaleSet $vmss `
  -Name "customScript" `
  -Publisher "Microsoft.Compute" `
  -Type "CustomScriptExtension" `
  -TypeHandlerVersion 1.9 `
  -Setting $customConfigv2

# Update the scale set and reapply the Custom Script Extension to the VM instances
Update-AzureRmVmss `
  -ResourceGroupName "myResourceGroup" `
  -Name "myScaleSet" `
  -VirtualMachineScaleSet $vmss
```

Alle VM-exemplaren in de schaalset worden automatisch bijgewerkt met de meest recente versie van de voorbeeldwebpagina. Vernieuw de website in uw browser voor een overzicht van de bijgewerkte versie:

![Bijgewerkte webpagina in IIS](media/tutorial-install-apps-powershell/running-iis-updated.png)


## <a name="clean-up-resources"></a>Resources opschonen
Als u de schaalset en aanvullende resources wilt verwijderen, verwijdert u de resourcegroep en alle bijbehorende resources met [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup). De parameter `-Force` bevestigt dat u de resources wilt verwijderen, zonder een extra prompt om dit te doen. De parameter `-AsJob` retourneert het besturingselement naar de prompt zonder te wachten totdat de bewerking is voltooid.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name "myResourceGroup" -Force -AsJob
```


## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u geleerd hoe u automatisch toepassingen kunt installeren of bijwerken in uw schaalset met Azure PowerShell:

> [!div class="checklist"]
> * Automatisch toepassingen installeren in een schaalset
> * De aangepaste scriptextensie van Azure gebruiken
> * Een actieve toepassing in een schaalset bijwerken

Ga door naar de volgende zelfstudie voor informatie over het automatisch schalen van uw schaalset.

> [!div class="nextstepaction"]
> [Uw schaalsets automatisch schalen](tutorial-autoscale-powershell.md)
