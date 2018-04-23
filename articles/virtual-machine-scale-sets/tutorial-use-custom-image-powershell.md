---
title: Zelfstudie - Een aangepaste VM-installatiekopie gebruiken in een schaalset met Azure PowerShell | Microsoft Docs
description: Informatie over het gebruik van Azure PowerShell voor het maken van een aangepaste VM-installatiekopie die u kunt gebruiken voor het implementeren van een virtuele-machineschaalset
services: virtual-machine-scale-sets
documentationcenter: ''
author: iainfoulds
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
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 700d9759ae4833145c88fcda346a3fc7aec0ed29
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/19/2018
---
# <a name="tutorial-create-and-use-a-custom-image-for-virtual-machine-scale-sets-with-azure-powershell"></a>Zelfstudie: Een aangepaste installatiekopie voor virtuele-machineschaalsets maken en gebruiken met Azure PowerShell
Wanneer u een schaalset maakt, geeft u een installatiekopie op die moet worden gebruikt wanneer de VM-exemplaren zijn geïmplementeerd. Om het aantal taken na de implementatie van VM-exemplaren te verminderen, kunt u een aangepaste VM-installatiekopie gebruiken. Deze aangepaste VM-installatiekopie bevat alle geïnstalleerde toepassingen of configuraties die vereist zijn. Alle VM-exemplaren die in de schaalset zijn gemaakt, gebruiken de aangepaste VM-installatiekopie en zijn gereed voor uw toepassingsverkeer. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een VM maken en aanpassen
> * De inrichting van de VM ongedaan maken en de VM generaliseren
> * Een aangepaste VM-installatiekopie maken van de bron-VM
> * Een schaalset implementeren die gebruikmaakt van de aangepaste VM-installatiekopie

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Als u PowerShell lokaal wilt installeren en gebruiken, is voor deze zelfstudie versie 5.6.0 of hoger van de Azure PowerShell-module vereist. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure. 


## <a name="create-and-configure-a-source-vm"></a>Een bron-VM maken en configureren

>[!NOTE]
> Deze zelfstudie leidt u door het proces van het maken en gebruiken van een algemene VM-installatiekopie. Het maken van een schaalset op basis van een gespecialiseerde VM-installatiekopie wordt niet ondersteund.

Maak eerst een resourcegroep met [New-AzureRmResourceGroup](/powershell/module/azurerm.compute/new-azurermvm) en vervolgens een VM met [New-AzureRmVM](/powershell/module/azurerm.resources/new-azurermresourcegroup). Deze VM wordt vervolgens gebruikt als bron voor een aangepaste VM-installatiekopie. In het volgende voorbeeld wordt een VM met de naam *myCustomVM* gemaakt in de resourcegroep met de naam *myResourceGroup*. Wanneer u hierom wordt gevraagd, geeft u een gebruikersnaam en wachtwoord op dat moet worden gebruikt als de aanmeldingsreferenties voor de VM:

```azurepowershell-interactive
# Create a resource a group
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "EastUS"

# Create a Windows Server 2016 Datacenter VM
New-AzureRmVm `
  -ResourceGroupName "myResourceGroup" `
  -Name "myCustomVM" `
  -ImageName "Win2016Datacenter" `
  -OpenPorts 3389
```

Geef voor verbinding met uw VM het openbare IP-adres met [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) als volgt op:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup | Select IpAddress
```

Maak een externe verbinding met de VM. Als u Azure Cloud Shell gebruikt, voert u deze stap uit vanaf een lokale PowerShell-prompt of vanuit de Extern bureaublad-client. Geef uw eigen IP-adres op uit de vorige opdracht. Wanneer u hierom wordt gevraagd, voert u de referenties in die zijn gebruikt bij het maken van de VM:

```powershell
mstsc /v:<IpAddress>
```

We gaan een eenvoudige webserver voor het aanpassen van uw VM installeren. Bij implementatie van het VM-exemplaar in de schaalset heeft dit hiermee alle vereiste pakketten om een webtoepassing uit te voeren. Open een lokale PowerShell-opdrachtprompt op de VM en installeer de IIS-webserver met [Install-WindowsFeature](/powershell/module/servermanager/install-windowsfeature) als volgt:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

De laatste stap voor het voorbereiden van uw VM voor gebruik als een aangepaste installatiekopie is het generaliseren van de VM. Sysprep verwijdert al uw persoonlijke gegevens en configuraties en stelt de virtuele machine opnieuw in op een schone toestand voor toekomstige implementaties. Zie voor meer informatie [Sysprep gebruiken: een inleiding](http://technet.microsoft.com/library/bb457073.aspx).

Voer Sysprep uit om de virtuele machine te generaliseren, en stel de virtuele machine in voor een out-of-the-box-ervaring. Als u klaar bent, instrueer dan Sysprep om de virtuele machine af te sluiten:

```powershell
C:\Windows\system32\sysprep\sysprep.exe /oobe /generalize /shutdown
```

De externe verbinding naar de virtuele machine wordt automatisch gesloten wanneer het proces door Sysprep is uitgevoerd en de virtuele machine wordt afgesloten.


## <a name="create-a-custom-vm-image-from-the-source-vm"></a>Een aangepaste VM-installatiekopie maken van de bron-VM
De bron-VM is nu aangepast met de geïnstalleerde IIS-webserver. We maken een aangepaste VM-installatiekopie om deze te gebruiken met een schaalset.

Voor het maken van een installatiekopie moet de toewijzing van de VM ongedaan worden gemaakt. Wijs de virtuele machine toe met [Stop-AzureRmVm](/powershell/module/azurerm.compute/stop-azurermvm). Vervolgens stelt u de status van de virtuele machine in als gegeneraliseerd met [Set-AzureRmVm](/powershell/module/azurerm.compute/set-azurermvm) zodat het Azure-platform weet dat de virtuele machine klaar is voor het gebruik van een aangepaste installatiekopie. U kunt alleen een installatiekopie maken op basis van een gegeneraliseerde virtuele machine:

```azurepowershell-interactive
Stop-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myCustomVM" -Force
Set-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myCustomVM" -Generalized
```

Het duurt enkele minuten om de toewijzing ongedaan te maken en de virtuele machine te generaliseren.

Maak nu een installatiekopie van de virtuele machine met [New-AzureRmImageConfig](/powershell/module/azurerm.compute/new-azurermimageconfig) en [New-AzureRmImage](/powershell/module/azurerm.compute/new-azurermimage). In het volgende voorbeeld wordt er een installatiekopie met de naam *myImage* gemaakt van uw virtuele machine:

```azurepowershell-interactive
# Get VM object
$vm = Get-AzureRmVM -Name "myCustomVM" -ResourceGroupName "myResourceGroup"

# Create the VM image configuration based on the source VM
$image = New-AzureRmImageConfig -Location "EastUS" -SourceVirtualMachineId $vm.ID 

# Create the custom VM image
New-AzureRmImage -Image $image -ImageName "myImage" -ResourceGroupName "myResourceGroup"
```


## <a name="create-a-scale-set-from-the-custom-vm-image"></a>Een schaalset maken op basis van de aangepaste VM-installatiekopie
Maak nu een schaalset met [New-AzureRmVmss](/powershell/module/azurerm.compute/new-azurermvmss) die gebruikmaakt van de parameter `-ImageName` voor het definiëren van de aangepaste VM-installatiekopie die in de vorige stap is gemaakt. Om het verkeer te distribueren naar de verschillende VM-exemplaren, wordt er ook een load balancer gemaakt. De load balancer bevat regels voor het distribueren van verkeer op TCP-poort 80, en voor het toestaan van extern bureaubladverkeer op TCP-poort 3389 en externe toegang via PowerShell op TCP-poort 5985:

```azurepowershell-interactive
New-AzureRmVmss `
  -ResourceGroupName "myResourceGroup" `
  -Location "EastUS" `
  -VMScaleSetName "myScaleSet" `
  -VirtualNetworkName "myVnet" `
  -SubnetName "mySubnet" `
  -PublicIpAddressName "myPublicIPAddress" `
  -LoadBalancerName "myLoadBalancer" `
  -UpgradePolicy "Automatic" `
  -ImageName "myImage"
```

Het duurt enkele minuten om alle schaalsetresources en VM's te maken en te configureren.


## <a name="test-your-scale-set"></a>Uw schaalset testen
Als u de schaalset in actie wilt zien, achterhaalt u het openbare IP-adres van de load balancer met [Get-AzureRmPublicIpAddress](/powershell/module/AzureRM.Network/Get-AzureRmPublicIpAddress). Dit doet u als volgt:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -ResourceGroupName "myResourceGroup" `
  -Name "myPublicIPAddress" | Select IpAddress
```

Voer in uw webbrowser het openbare IP-adres in. De standaard IIS-webpagina wordt weergegeven, zoals in het volgende voorbeeld:

![IIS uitgevoerd vanuit een aangepaste VM-installatiekopie](media/tutorial-use-custom-image-powershell/default-iis-website.png)


## <a name="clean-up-resources"></a>Resources opschonen
Als u de schaalset en aanvullende resources wilt verwijderen, verwijdert u de resourcegroep en alle bijbehorende resources met [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup). De parameter `-Force` bevestigt dat u de resources wilt verwijderen, zonder een extra prompt om dit te doen. De parameter `-AsJob` retourneert het besturingselement naar de prompt zonder te wachten totdat de bewerking is voltooid.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name "myResourceGroup" -Force -AsJob
```


## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u geleerd hoe u een aangepaste VM-installatiekopie maakt en gebruikt voor uw schaalsets met Azure PowerShell:

> [!div class="checklist"]
> * Een VM maken en aanpassen
> * De inrichting van de VM ongedaan maken en de VM generaliseren
> * Een aangepaste VM-installatiekopie maken
> * Een schaalset implementeren die gebruikmaakt van de aangepaste VM-installatiekopie

Ga door naar de volgende zelfstudie voor informatie over het implementeren van toepassingen naar uw schaalset.

> [!div class="nextstepaction"]
> [Toepassingen in uw schaalsets implementeren](tutorial-install-apps-powershell.md)
