---
title: Zelfstudie - Een schaalset voor virtuele Azure-machines maken en beheren | Microsoft Docs
description: Ontdek hoe u met Azure PowerShell een schaalset met virtuele Azure-machines maakt. Leer daarnaast hoe u enkele veelvoorkomende beheertaken uitvoert, zoals het starten en stoppen van een exemplaar of het wijzigen van de capaciteit van de schaalset.
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
ms.openlocfilehash: 80ff76f98e563df14feacafab2420a48611d5828
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/19/2018
---
# <a name="tutorial-create-and-manage-a-virtual-machine-scale-set-with-azure-powershell"></a>Zelfstudie - Een schaalset met virtuele Azure-machines maken met Azure PowerShell
Met een virtuele-machineschaalset kunt u een reeks identieke virtuele machines met automatisch schalen implementeren en beheren. Gedurende de levenscyclus van een schaalset voor virtuele machines moet u mogelijk een of meer beheertaken uitvoeren. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een schaalset voor virtuele machines maken en hiermee verbinding maken
> * VM-installatiekopieën selecteren en gebruiken
> * Specifieke grootten van VM-exemplaren weergeven en gebruiken
> * Een schaalset handmatig schalen
> * Algemene beheertaken voor schaalsets uitvoeren

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Als u PowerShell lokaal wilt installeren en gebruiken, is voor deze zelfstudie versie 5.6.0 of hoger van de Azure PowerShell-module vereist. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure. 


## <a name="create-a-resource-group"></a>Een resourcegroep maken
Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. U kunt pas een schaalset voor virtuele machines maken als er al een resourcegroep is gemaakt. Maak een resourcegroep met de opdracht [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). In dit voorbeeld wordt er een resourcegroep met de naam *myResourceGroup* gemaakt in de regio *EastUS*. 

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName "myResourceGroup" -Location "EastUS"
```
De naam van de resourcegroep moet worden opgegeven als u ergens in deze zelfstudie een schaalset maakt of wijzigt.


## <a name="create-a-scale-set"></a>Een schaalset maken
Stel eerst een beheerdersnaam en -wachtwoord in voor de VM-exemplaren met behulp van [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```azurepowershell-interactive
$cred = Get-Credential
```

Maak nu een virtuele-machineschaalset met [New-AzureRmVmss](/powershell/module/azurerm.compute/new-azurermvmss). Om het verkeer te distribueren naar de verschillende VM-exemplaren, wordt er ook een load balancer gemaakt. De load balancer bevat regels voor het distribueren van verkeer op TCP-poort 80, en voor het toestaan van extern bureaubladverkeer op TCP-poort 3389 en externe toegang via PowerShell op TCP-poort 5985:

```azurepowershell-interactive
New-AzureRmVmss `
  -ResourceGroupName "myResourceGroup" `
  -VMScaleSetName "myScaleSet" `
  -Location "EastUS" `
  -VirtualNetworkName "myVnet" `
  -SubnetName "mySubnet" `
  -PublicIpAddressName "myPublicIPAddress" `
  -LoadBalancerName "myLoadBalancer" `
  -Credential $cred
```

Het duurt enkele minuten om alle schaalsetresources en VM-exemplaren te maken en te configureren.


## <a name="view-the-vm-instances-in-a-scale-set"></a>De VM-exemplaren in een schaalset bekijken
Als u een overzicht wilt zien van de VM-exemplaren in een schaalset, gebruikt u [Get-AzureRmVmssVM](/powershell/module/azurerm.compute/get-azurermvmssvm) als volgt:

```azurepowershell-interactive
Get-AzureRmVmssVM -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"
```

In de volgende voorbeelduitvoer ziet u dat de schaalset twee VM-exemplaren bevat:

```powershell
ResourceGroupName         Name Location             Sku InstanceID ProvisioningState
-----------------         ---- --------             --- ---------- -----------------
MYRESOURCEGROUP   myScaleSet_0   eastus Standard_DS1_v2          0         Succeeded
MYRESOURCEGROUP   myScaleSet_1   eastus Standard_DS1_v2          1         Succeeded
```

Als u extra informatie wilt weergeven over een specifiek VM-exemplaar, voegt u de parameter `-InstanceId` toe aan [Get-AzureRmVmssVM](/powershell/module/azurerm.compute/get-azurermvmssvm). In het volgende voorbeeld wordt informatie opgevraagd over het VM-exemplaar *1*:

```azurepowershell-interactive
Get-AzureRmVmssVM -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "1"
```


## <a name="list-connection-information"></a>Verbindingsgegevens opvragen
Er wordt een openbaar IP-adres toegewezen aan de load balancer die verkeer routeert naar de afzonderlijke VM-exemplaren. Standaard worden er regels voor NAT (Network Address Translation) toegevoegd aan de load balancer van Azure waarmee verkeer van externe verbindingen wordt doorgestuurd naar elke VM op een opgegeven poort. Om verbinding te maken met de VM-exemplaren in een schaalset, maakt u een externe verbinding met een toegewezen openbare IP-adres en poortnummer.

Als u de NAT-poorten wilt opvragen die u moet verbinden met VM-exemplaren in een schaalset, moet u eerst het load balancer-object opvragen met [Get-AzureRmLoadBalancer](/powershell/module/AzureRM.Network/Get-AzureRmLoadBalancer). Bekijk vervolgens de binnenkomende NAT-regels met [Get-AzureRmLoadBalancerInboundNatRuleConfig](/powershell/module/AzureRM.Network/Get-AzureRmLoadBalancerInboundNatRuleConfig):

```azurepowershell-interactive
# Get the load balancer object
$lb = Get-AzureRmLoadBalancer -ResourceGroupName "myResourceGroup" -Name "myLoadBalancer"

# View the list of inbound NAT rules
Get-AzureRmLoadBalancerInboundNatRuleConfig -LoadBalancer $lb | Select-Object Name,Protocol,FrontEndPort,BackEndPort
```

In de volgende voorbeelduitvoer ziet u de exemplaarnaam, het openbare IP-adres van de load balancer en het poortnummer waarnaar de NAT-regels verkeer doorsturen:

```powershell
Name             Protocol FrontendPort BackendPort
----             -------- ------------ -----------
myScaleSet3389.0 Tcp             50001        3389
myScaleSet5985.0 Tcp             51001        5985
myScaleSet3389.1 Tcp             50002        3389
myScaleSet5985.1 Tcp             51002        5985
```

De *naam* van de regel komt overeen met de naam van het VM-exemplaar, zoals wordt weergegeven in een eerdere [Get-AzureRmVmssVM](/powershell/module/azurerm.compute/get-azurermvmssvm)-opdracht. Als u bijvoorbeeld verbinding wilt maken met het VM-exemplaar *0*, gebruikt u *myScaleSet3389.0* en maakt u verbinding met poort *50001*. Als u verbinding wilt maken met het VM-exemplaar *1*, gebruikt u de waarde *myScaleSet3389.1* en maakt u verbinding met poort *50002*. Als u PowerShell wilt gebruiken voor externe toegang, maakt u verbinding met de desbetreffende regel van het VM-exemplaar voor *TCP*-poort *5985*.

Bekijk het openbare IP-adres van de load balancer met [Get-AzureRmPublicIPAddress](/powershell/module/AzureRM.Network/Get-AzureRmPublicIpAddress):

```azurepowershell-interactive
Get-AzureRmPublicIpAddress -ResourceGroupName "myResourceGroup" -Name "myPublicIPAddress" | Select IpAddress
```

Voorbeelduitvoer:

```powershell
IpAddress
---------
52.168.121.216
```

Maak een externe verbinding met uw eerste VM-exemplaar. Geef uw openbare IP-adres en poortnummer van het vereiste VM-exemplaar op, zoals is gedaan in de voorgaande opdrachten. Voer desgevraagd de referenties in die u hebt gebruikt bij het maken van de schaalset (standaard *azureuser* en *P@ssw0rd!* in de voorbeeldopdrachten). Als u Azure Cloud Shell gebruikt, voert u deze stap uit vanaf een lokale PowerShell-prompt of vanuit de Extern bureaublad-client. In het volgende voorbeeld wordt verbinding gemaakt met het VM-exemplaar *1*:

```powershell
mstsc /v 52.168.121.216:50001
```

Zodra u bent aangemeld bij het VM-exemplaar, kunt u indien nodig de configuratie op enkele punten handmatig wijzigen. U kunt de externe verbinding vooralsnog gewoon sluiten.


## <a name="understand-vm-instance-images"></a>Installatiekopieën van VM-exemplaren begrijpen
Toen u in een vorige stap een schaalsetconfiguratie hebt gedefinieerd met [Set-AzureRmVmssStorageProfile](/powershell/module/AzureRM.Compute/Set-AzureRmVmssStorageProfile), hebt u hiervoor een installatiekopie van Windows Server 2016 Datacenter gebruikt. Azure Marketplace bevat allerlei installatiekopieën die kunnen worden gebruikt voor het maken van VM-exemplaren. Gebruik de opdracht [Get-AzureRmVMImagePublisher](/powershell/module/azurerm.compute/get-azurermvmimagepublisher) om een lijst met beschikbare uitgevers te zien.

```azurepowershell-interactive
Get-AzureRmVMImagePublisher -Location "EastUS"
```

Gebruik [Get-AzureRmVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) om een lijst met installatiekopieën voor een bepaalde uitgever op te vragen. De lijst met installatiekopieën kan ook worden gefilterd door `-PublisherName` of `–Offer`. In het volgende voorbeeld wordt de lijst gefilterd voor alle installatiekopieën met de uitgeversnaam *MicrosoftWindowsServer* en een aanbieding die overeenkomt met *WindowsServer*:

```azurepowershell-interactive
Get-AzureRmVMImageSku -Location "EastUS" -PublisherName "MicrosoftWindowsServer" -Offer "WindowsServer"
```

In de volgende voorbeelduitvoer ziet u alle beschikbare installatiekopieën van Windows Server:

```powershell
Skus                                  Offer         PublisherName          Location
----                                  -----         -------------          --------
2008-R2-SP1                           WindowsServer MicrosoftWindowsServer eastus
2008-R2-SP1-smalldisk                 WindowsServer MicrosoftWindowsServer eastus
2012-Datacenter                       WindowsServer MicrosoftWindowsServer eastus
2012-Datacenter-smalldisk             WindowsServer MicrosoftWindowsServer eastus
2012-R2-Datacenter                    WindowsServer MicrosoftWindowsServer eastus
2012-R2-Datacenter-smalldisk          WindowsServer MicrosoftWindowsServer eastus
2016-Datacenter                       WindowsServer MicrosoftWindowsServer eastus
2016-Datacenter-Server-Core           WindowsServer MicrosoftWindowsServer eastus
2016-Datacenter-Server-Core-smalldisk WindowsServer MicrosoftWindowsServer eastus
2016-Datacenter-smalldisk             WindowsServer MicrosoftWindowsServer eastus
2016-Datacenter-with-Containers       WindowsServer MicrosoftWindowsServer eastus
2016-Datacenter-with-RDSH             WindowsServer MicrosoftWindowsServer eastus
2016-Nano-Server                      WindowsServer MicrosoftWindowsServer eastus
```

Toen u aan het begin van de zelfstudie een schaalset hebt gemaakt, werd er een standaard-VM-installatiekopie van *Windows Server 2016 DataCenter* opgegeven voor de VM-exemplaren. U kunt een andere VM-installatiekopie opgeven op basis van de uitvoer van [Get-AzureRmVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku). In het volgende voorbeeld wordt een schaalset gemaakt met de parameter `-ImageName` om de VM-installatiekopie *MicrosoftWindowsServer:WindowsServer:2016-Datacenter-with-Containers:latest* op te geven. Aangezien het enkele minuten duurt om alle schaalsetresources en VM-exemplaren te maken en te configureren, hoeft u de volgende schaalset niet te implementeren:

```azurepowershell-interactive
New-AzureRmVmss `
  -ResourceGroupName "myResourceGroup2" `
  -Location "EastUS" `
  -VMScaleSetName "myScaleSet2" `
  -VirtualNetworkName "myVnet2" `
  -SubnetName "mySubnet2" `
  -PublicIpAddressName "myPublicIPAddress2" `
  -LoadBalancerName "myLoadBalancer2" `
  -UpgradePolicy "Automatic" `
  -ImageName "MicrosoftWindowsServer:WindowsServer:2016-Datacenter-with-Containers:latest" `
  -Credential $cred
```


## <a name="understand-vm-instance-sizes"></a>VM-exemplaargrootten begrijpen
De grootte van een VM-exemplaar, of *SKU*, bepaalt de hoeveelheid rekenresources, zoals CPU, GPU en geheugen die beschikbaar worden gesteld aan het VM-exemplaar. VM-exemplaren in een schaalset moeten de juiste grootte krijgen voor de verwachte werkbelasting.

### <a name="vm-instance-sizes"></a>VM-exemplaargrootten
In de volgende tabel worden enkele veelgebruikte VM-grootten gecategoriseerd naar gebruikssituatie.

| Type                     | Veelgebruikte grootten           |    Beschrijving       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| [Algemeen doel](../virtual-machines/windows/sizes-general.md)         |Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0-7| Evenwichtige CPU-geheugenverhouding. Ideaal voor ontwikkelen/testen en in kleine tot middelgrote toepassingen en gegevensoplossingen.  |
| [Geoptimaliseerde rekenkracht](../virtual-machines/windows/sizes-compute.md)   | Fs, F             | Hoge CPU-geheugenverhouding. Goed voor middelgrootte verkeerstoepassingen, netwerkapparatuur en batchprocessen.        |
| [Geoptimaliseerd geheugen](../virtual-machines/windows/sizes-memory.md)    | Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D   | Hoge geheugen-kernverhouding. Uiterst geschikt voor relationele-databases, middelgrote tot grote caches en analysefuncties in het geheugen.                 |
| [Geoptimaliseerde opslag](../virtual-machines/windows/sizes-storage.md)      | Ls                | Snelle doorvoer van schijfgegevens en IO. Ideaal voor big data-, SQL- en NoSQL-databases.                                                         |
| [GPU](../virtual-machines/windows/sizes-gpu.md)          | NV, NC            | Gespecialiseerde VM's bedoeld voor intensieve grafische rendering en videobewerking.       |
| [Hoge prestaties](../virtual-machines/windows/sizes-hpc.md) | H, A8-11          | Onze krachtigste CPU-VM's met optionele netwerkinterfaces (RDMA) voor hoge doorvoer. 

### <a name="find-available-vm-instance-sizes"></a>Beschikbare VM-exemplaargrootten zoeken
Als u een lijst wilt weergeven met de VM-exemplaargrootten die beschikbaar zijn in een bepaalde regio, gebruikt u de opdracht [Get-AzureRmVMSize](/powershell/module/azurerm.compute/get-azurermvmsize). 

```azurepowershell-interactive
Get-AzureRmVMSize -Location "EastUS"
```

De uitvoer is vergelijkbaar met het volgende verkorte voorbeeld, waarin de resources worden weergegeven die zijn toegewezen aan elke VM-grootte:

```powershell
Name                   NumberOfCores MemoryInMB MaxDataDiskCount OSDiskSizeInMB ResourceDiskSizeInMB
----                   ------------- ---------- ---------------- -------------- --------------------
Standard_DS1_v2                    1       3584                4        1047552                 7168
Standard_DS2_v2                    2       7168                8        1047552                14336
[...]
Standard_A0                        1        768                1        1047552                20480
Standard_A1                        1       1792                2        1047552                71680
[...]
Standard_F1                        1       2048                4        1047552                16384
Standard_F2                        2       4096                8        1047552                32768
[...]
Standard_NV6                       6      57344               24        1047552               389120
Standard_NV12                     12     114688               48        1047552               696320
```

Toen u aan het begin van de zelfstudie een schaalset hebt gemaakt, werd er een standaard-VM-SKU van *Standard_DS1_v2* opgegeven voor de VM-exemplaren. U kunt een andere grootte voor het VM-exemplaar opgeven op basis van de uitvoer van [Get-AzureRmVMSize](/powershell/module/azurerm.compute/get-azurermvmsize). In het volgende voorbeeld wordt een schaalset gemaakt met de parameter `-VmSize` om een VM-exemplaargrootte op te geven van *Standard_F1*. Aangezien het enkele minuten duurt om alle schaalsetresources en VM-exemplaren te maken en te configureren, hoeft u de volgende schaalset niet te implementeren:

```azurepowershell-interactive
New-AzureRmVmss `
  -ResourceGroupName "myResourceGroup3" `
  -Location "EastUS" `
  -VMScaleSetName "myScaleSet3" `
  -VirtualNetworkName "myVnet3" `
  -SubnetName "mySubnet3" `
  -PublicIpAddressName "myPublicIPAddress3" `
  -LoadBalancerName "myLoadBalancer3" `
  -UpgradePolicy "Automatic" `
  -VmSize "Standard_F1" `
  -Credential $cred
```


## <a name="change-the-capacity-of-a-scale-set"></a>De capaciteit van een schaalset wijzigen
Toen u een schaalset hebt gemaakt, hebt u twee VM-exemplaren aangevraagd. Als u het aantal VM-exemplaren in de schaalset wilt vergroten of verkleinen, kunt u de capaciteit handmatig wijzigen. De grootte van de schaalset wordt dan aangepast, waarna de load balancer wordt geconfigureerd voor het verdelen van het verkeer.

Maak eerst een schaalsetobject met [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss) en geef vervolgens een nieuwe waarde op voor `sku.capacity`. Gebruik [Update-AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss) om de capaciteitswijziging toe te passen. In het volgende voorbeeld wordt het aantal VM-exemplaren in de schaalset ingesteld op *3*:

```azurepowershell-interactive
# Get current scale set
$vmss = Get-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"

# Set and update the capacity of your scale set
$vmss.sku.capacity = 3
Update-AzureRmVmss -ResourceGroupName "myResourceGroup" -Name "myScaleSet" -VirtualMachineScaleSet $vmss 
```

Het duurt een paar minuten om de capaciteit van de schaalset bij te werken. Als u het nieuwe aantal exemplaren in de schaalset wilt weergeven, gebruikt u [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss):

```azurepowershell-interactive
Get-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"
```

De volgende voorbeelduitvoer laat zien dat de capaciteit van de schaalset nu *3* is:

```powershell
Sku        :
  Name     : Standard_DS2
  Tier     : Standard
  Capacity : 3
```


## <a name="common-management-tasks"></a>Veelvoorkomende beheertaken
U kunt nu een schaalset maken, verbindingsgegevens opvragen en verbinding maken met VM-exemplaren. U hebt geleerd hoe u een andere installatiekopie van het besturingssysteem kunt gebruiken voor VM-exemplaren, hoe u een andere VM-grootte selecteert of hoe u handmatig het aantal exemplaren aanpast. Als onderdeel van de dagelijkse beheertaken, kan het nodig zijn om de VM-exemplaren in de schaalset te stoppen, starten of opnieuw op te starten.

### <a name="stop-and-deallocate-vm-instances-in-a-scale-set"></a>VM-exemplaren in een schaalset stoppen en hun toewijzing ongedaan maken
Als u een of meer VM's in een schaalset wilt stoppen, gebruikt u [Stop-AzureRmVmss](/powershell/module/azurerm.compute/stop-azurermvmss). Met de parameter `-InstanceId` kunt u een of meer VM's opgeven om te stoppen. Als u geen exemplaar-id opgeeft, worden alle VM's in de schaalset gestopt. In het volgende voorbeeld wordt het exemplaar *1* gestopt:

```azurepowershell-interactive
Stop-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "1"
```

De standaardinstelling is dat de toewijzing van gestopte VM's ongedaan wordt gemaakt en er dus geen compute-kosten meer in rekening worden gebracht. Als u wilt dat de VM de ingerichte status blijft houden nadat deze is gestopt, voeg u de parameter `-StayProvisioned` toe aan de voorgaande opdracht. Voor gestopte VM's die ingericht blijven, worden periodieke compute-kosten in rekening gebracht.

### <a name="start-vm-instances-in-a-scale-set"></a>VM-exemplaren in een schaalset starten
Als u een of meer VM's in een schaalset wilt starten, gebruikt u [Start-AzureRmVmss](/powershell/module/azurerm.compute/start-azurermvmss). Met de parameter `-InstanceId` kunt u een of meer VM's opgeven om te starten. Als u geen exemplaar-id opgeeft, worden alle VM's in de schaalset gestart. In het volgende voorbeeld wordt het exemplaar *1* gestart:

```azurepowershell-interactive
Start-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "1"
```

### <a name="restart-vm-instances-in-a-scale-set"></a>VM-exemplaren in een schaalset opnieuw opstarten
Als u een of meer VM's in een schaalset opnieuw wilt opstarten, gebruikt u [Restart-AzureRmVmss](/powershell/module/azurerm.compute/restart-azurermvmss). Met de parameter `-InstanceId` kunt u een of meer VM's opgeven om opnieuw op te starten. Als u geen exemplaar-id opgeeft, worden alle VM's in de schaalset opnieuw opgestart. In het volgende voorbeeld wordt het exemplaar *1* opnieuw opgestart:

```azurepowershell-interactive
Restart-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "1"
```


## <a name="clean-up-resources"></a>Resources opschonen
Als u een resourcegroep verwijdert, verwijdert u ook alle resources binnen deze groep, zoals de VM-exemplaren, het virtuele netwerk en schijven. De parameter `-Force` bevestigt dat u de resources wilt verwijderen, zonder een extra prompt om dit te doen. De parameter `-AsJob` retourneert het besturingselement naar de prompt zonder te wachten totdat de bewerking is voltooid.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name "myResourceGroup" -Force -AsJob
```


## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u geleerd hoe u een aantal basistaken voor het maken en beheren van schaalsets kunt uitvoeren met Azure PowerShell:

> [!div class="checklist"]
> * Een schaalset voor virtuele machines maken en hiermee verbinding maken
> * VM-installatiekopieën selecteren en gebruiken
> * Specifieke VM-grootten weergeven en gebruiken
> * Een schaalset handmatig schalen
> * Algemene beheertaken voor schaalsets uitvoeren

Ga naar de volgende zelfstudie om alles te leren over schijven in een schaalset.

> [!div class="nextstepaction"]
> [Gegevensschijven met schaalsets gebruiken](tutorial-use-disks-powershell.md)
