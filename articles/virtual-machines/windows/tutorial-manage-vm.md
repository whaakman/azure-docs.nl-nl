---
title: "Zelfstudie: Windows-VM's maken en beheren met de Azure PowerShell | Microsoft Docs"
description: In deze zelfstudie leert u hoe u Azure PowerShell gebruikt voor het maken en beheren van virtuele Windows-machines in Azure
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 03/23/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: d47981042fc13a96bdf5cb9690e4dc83a6aa0162
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37932548"
---
# <a name="tutorial-create-and-manage-windows-vms-with-azure-powershell"></a>Zelfstudie: Windows-VM's maken en beheren met Azure PowerShell

Virtuele machines in Azure bieden een volledig geconfigureerde en flexibele computeromgeving. Deze zelfstudie bevat informatie over basisconcepten voor het implementeren van virtuele Azure-machines, zoals het selecteren van een VM-grootte, het selecteren van een VM-installatiekopie en het implementeren van een virtuele machine. In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * Een virtuele machine maken en verbinding maken met een virtuele machine
> * VM-installatiekopieën selecteren en gebruiken
> * Specifieke VM-grootten weergeven en gebruiken
> * De grootte van een virtuele machine wijzigen
> * De status van een virtuele machine weergeven en begrijpen

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Als u ervoor kiest om PowerShell lokaal te installeren en te gebruiken, moet u moduleversie 5.7.0 of hoger van Azure PowerShell gebruiken voor deze zelfstudie. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.

## <a name="create-resource-group"></a>Een resourcegroep maken

Maak een resourcegroep met de opdracht [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). 

Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. Voordat een virtuele machine wordt gemaakt, moet een resourcegroep worden gemaakt. In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroupVM* gemaakt in de regio *VS - Oost*:

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName "myResourceGroupVM" -Location "EastUS"
```

De resourcegroep wordt opgegeven tijdens het maken of wijzigen van een virtuele machine, zoals in deze zelfstudie te zien is.

## <a name="create-virtual-machine"></a>Virtuele machine maken

Wanneer u een virtuele machine maakt, zijn er diverse opties beschikbaar zoals besturingssysteeminstallatiekopie, netwerkconfiguratie en beheerdersreferenties. In dit voorbeeld wordt een virtuele machine gemaakt met de naam *myVM* waarop de nieuwste standaardversie van Windows Server 2016 Datacenter wordt uitgevoerd.

Stel met [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential) de gebruikersnaam en het wachtwoord in die nodig zijn voor de beheerdersaccount op de virtuele machine:

```azurepowershell-interactive
$cred = Get-Credential
```

Maak de virtuele machine met [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroupVM" `
    -Name "myVM" `
    -Location "EastUS" `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress" `
    -Credential $cred
```

## <a name="connect-to-vm"></a>Verbinding maken met de virtuele machine

Wanneer de implementatie is voltooid, maakt u via een extern bureaublad verbinding met de virtuele machine.

Voer de volgende opdrachten uit om het openbare IP-adres van de virtuele machine te retourneren. Noteer dit IP-adres zodat u vanuit uw browser verbinding kunt maken met het adres om webverbindingen te testen.

```azurepowershell-interactive
Get-AzureRmPublicIpAddress -ResourceGroupName "myResourceGroupVM"  | Select IpAddress
```

Gebruik de volgende opdracht op uw lokale machine om een sessie met een extern bureaublad te starten voor de virtuele machine. Vervang het IP-adres door het *publicIPAddress* van de virtuele machine. Wanneer u hierom wordt gevraagd, typt u de referenties die zijn gebruikt bij het maken van de virtuele machine.

```powershell
mstsc /v:<publicIpAddress>
```

Selecteer in het venster **Windows-beveiliging** **Meer opties** en vervolgens **Een ander account gebruiken**. Typ de gebruikersnaam en het wachtwoord die u hebt gemaakt voor de virtuele machine en klik vervolgens op **OK**.

## <a name="understand-vm-images"></a>Inzicht in VM-installatiekopieën

Azure Marketplace bevat veel installatiekopieën van virtuele machines die kunnen worden gebruikt voor het maken van een nieuwe virtuele machine. In de vorige stappen is een virtuele machine gemaakt met behulp van de Windows Server 2016 Datacenter-installatiekopie. In deze stap wordt de PowerShell-module gebruikt om op de Marketplace te zoeken naar andere Windows-installatiekopieën, die ook als basis voor nieuwe virtuele machines kunnen worden gebruikt. Dit proces bestaat uit het vinden van de uitgever, aanbieding, SKU en eventueel een versienummer om de installatiekopie te [identificeren](cli-ps-findimage.md#terminology). 

Gebruik de opdracht [Get-AzureRmVMImagePublisher](/powershell/module/azurerm.compute/get-azurermvmimagepublisher) om een lijst met uitgevers van installatiekopieën te retourneren:

```powershell
Get-AzureRmVMImagePublisher -Location "EastUS"
```

Gebruik de opdracht [Get-AzureRmVMImageOffer](/powershell/module/azurerm.compute/get-azurermvmimageoffer) om een lijst met aanbiedingen van installatiekopieën te retourneren. Met deze opdracht wordt de geretourneerde lijst gefilterd op de opgegeven uitgever:

```azurepowershell-interactive
Get-AzureRmVMImageOffer -Location "EastUS" -PublisherName "MicrosoftWindowsServer"
```

```azurepowershell-interactive
Offer             PublisherName          Location
-----             -------------          -------- 
Windows-HUB       MicrosoftWindowsServer EastUS 
WindowsServer     MicrosoftWindowsServer EastUS   
WindowsServer-HUB MicrosoftWindowsServer EastUS   
```

Met de opdracht [Get-AzureRmVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) wordt vervolgens gefilterd op de naam van de uitgever en de aanbieding, waarna een lijst met namen van installatiekopieën wordt geretourneerd.

```azurepowershell-interactive
Get-AzureRmVMImageSku -Location "EastUS" -PublisherName "MicrosoftWindowsServer" -Offer "WindowsServer"
```

```azurepowershell-interactive
Skus                                      Offer         PublisherName          Location
----                                      -----         -------------          --------
2008-R2-SP1                               WindowsServer MicrosoftWindowsServer EastUS  
2008-R2-SP1-smalldisk                     WindowsServer MicrosoftWindowsServer EastUS  
2012-Datacenter                           WindowsServer MicrosoftWindowsServer EastUS  
2012-Datacenter-smalldisk                 WindowsServer MicrosoftWindowsServer EastUS  
2012-R2-Datacenter                        WindowsServer MicrosoftWindowsServer EastUS  
2012-R2-Datacenter-smalldisk              WindowsServer MicrosoftWindowsServer EastUS  
2016-Datacenter                           WindowsServer MicrosoftWindowsServer EastUS  
2016-Datacenter-Server-Core               WindowsServer MicrosoftWindowsServer EastUS  
2016-Datacenter-Server-Core-smalldisk     WindowsServer MicrosoftWindowsServer EastUS
2016-Datacenter-smalldisk                 WindowsServer MicrosoftWindowsServer EastUS
2016-Datacenter-with-Containers           WindowsServer MicrosoftWindowsServer EastUS
2016-Datacenter-with-Containers-smalldisk WindowsServer MicrosoftWindowsServer EastUS
2016-Datacenter-with-RDSH                 WindowsServer MicrosoftWindowsServer EastUS
2016-Nano-Server                          WindowsServer MicrosoftWindowsServer EastUS
```

Deze informatie kan worden gebruikt om een virtuele machine met een specifieke installatiekopie te implementeren. In dit voorbeeld wordt een virtuele machine geïmplementeerd met behulp van de nieuwste versie van Windows Server 2016 met Containers-installatiekopie.

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroupVM" `
    -Name "myVM2" `
    -Location "EastUS" `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress2" `
    -ImageName "MicrosoftWindowsServer:WindowsServer:2016-Datacenter-with-Containers:latest" `
    -Credential $cred `
    -AsJob
```

Door de parameter `-AsJob` wordt de VM gemaakt als achtergrondtaak, zodat u weer terugkeert naar de PowerShell-prompts. U kunt details van achtergrondtaken bekijken met de cmdlet `Get-Job`.


## <a name="understand-vm-sizes"></a>Inzicht in VM-grootten

De grootte van een virtuele machine bepaalt de hoeveelheid rekenresources, zoals CPU, GPU en geheugen, die beschikbaar zijn gesteld voor de virtuele machine. Virtuele machines moeten worden gemaakt met een grootte die geschikt is voor de verwachte werkbelasting. Als de werkbelasting toeneemt, kan de grootte van een bestaande virtuele machine worden gewijzigd.

### <a name="vm-sizes"></a>VM-grootten

In de volgende tabel zijn grootten gecategoriseerd in use-cases.  
| Type                     | Veelgebruikte grootten           |    Beschrijving       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| [Algemeen doel](sizes-general.md)         |Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0-7| Evenwichtige CPU-geheugenverhouding. Ideaal voor ontwikkelen/testen en in kleine tot middelgrote toepassingen en gegevensoplossingen.  |
| [Geoptimaliseerde rekenkracht](sizes-compute.md)   | Fs, F             | Hoge CPU-geheugenverhouding. Goed voor middelgrootte verkeerstoepassingen, netwerkapparatuur en batchprocessen.        |
| [Geoptimaliseerd geheugen](sizes-memory.md)    | Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D   | Hoge geheugen-kernverhouding. Uiterst geschikt voor relationele-databases, middelgrote tot grote caches en analysefuncties in het geheugen.                 |
| [Geoptimaliseerde opslag](sizes-storage.md)      | Ls                | Snelle doorvoer van schijfgegevens en IO. Ideaal voor big data-, SQL- en NoSQL-databases.                                                         |
| [GPU](sizes-gpu.md)          | NV, NC            | Gespecialiseerde VM's bedoeld voor intensieve grafische rendering en videobewerking.       |
| [Hoge prestaties](sizes-hpc.md) | H, A8-11          | Onze krachtigste CPU-VM's met optionele netwerkinterfaces (RDMA) voor hoge doorvoer. 


### <a name="find-available-vm-sizes"></a>Beschikbare VM-grootten zoeken

Als u een lijst wilt weergeven met de VM-grootten die beschikbaar zijn in een bepaalde regio, gebruikt u de opdracht [Get-AzureRmVMSize](/powershell/module/azurerm.compute/get-azurermvmsize).

```azurepowershell-interactive
Get-AzureRmVMSize -Location "EastUS"
```

## <a name="resize-a-vm"></a>De grootte van een virtuele machine wijzigen

Nadat een virtuele machine is geïmplementeerd, kan de grootte ervan worden gewijzigd om meer of minder resources toe te wijzen.

Voordat u de grootte van een virtuele machine wijzigt, moet u controleren of de gewenste grootte beschikbaar in het huidige VM-cluster. Met de opdracht [Get-AzureRmVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) wordt een lijst met grootten geretourneerd. 

```azurepowershell-interactive
Get-AzureRmVMSize -ResourceGroupName "myResourceGroupVM" -VMName "myVM"
```

Als de gewenste grootte beschikbaar is, kan de grootte van de virtuele machine worden gewijzigd terwijl de virtuele machine wordt uitgevoerd. De virtuele machine moet wel opnieuw worden opgestart tijdens de bewerking.

```azurepowershell-interactive
$vm = Get-AzureRmVM -ResourceGroupName "myResourceGroupVM"  -VMName "myVM"
$vm.HardwareProfile.VmSize = "Standard_D4"
Update-AzureRmVM -VM $vm -ResourceGroupName "myResourceGroupVM"
```

Als de gewenste grootte niet beschikbaar is in het huidige cluster, moet de toewijzing van de VM ongedaan worden gemaakt voordat de grootte kan worden gewijzigd. Opmerking: als de virtuele machine weer is ingeschakeld, worden alle gegevens op de tijdelijke schijf verwijderd en wordt het openbare IP-adressen gewijzigd, tenzij een statisch IP-adres wordt gebruikt. 

```azurepowershell-interactive
Stop-AzureRmVM -ResourceGroupName "myResourceGroupVM" -Name "myVM" -Force
$vm = Get-AzureRmVM -ResourceGroupName "myResourceGroupVM"  -VMName "myVM"
$vm.HardwareProfile.VmSize = "Standard_F4s"
Update-AzureRmVM -VM $vm -ResourceGroupName "myResourceGroupVM"
Start-AzureRmVM -ResourceGroupName "myResourceGroupVM"  -Name $vm.name
```

## <a name="vm-power-states"></a>Energiestatussen voor de virtuele machine

Een Azure VM kan op een van de vele energiestatussen worden ingesteld. Deze status vertegenwoordigt de huidige status van de virtuele machine vanuit het oogpunt van de hypervisor. 

### <a name="power-states"></a>Energiestatussen

| Energiestatus | Beschrijving
|----|----|
| Starten | Geeft aan dat de virtuele machine wordt gestart. |
| In uitvoering | Geeft aan dat de virtuele machine wordt uitgevoerd. |
| Stoppen | Geeft aan dat de virtuele machine wordt gestopt. | 
| Gestopt | Geeft aan dat de virtuele machine is gestopt. Virtuele machines met de status Gestopt genereren nog steeds rekenkosten.  |
| Vrijgeven | Geeft aan dat de toewijzing van de virtuele machine ongedaan wordt gemaakt. |
| Toewijzing ongedaan gemaakt | Geeft aan dat de virtuele machine volledig is verwijderd uit de hypervisor maar nog steeds beschikbaar is in het vlak van het besturingselement. Virtuele machines met de status Toewijzing ongedaan gemaakt genereren geen rekenkosten. |
| - | Geeft aan dat de Aan-/uitstatus van de virtuele machine onbekend is. |

### <a name="find-power-state"></a>Energiestatus zoeken

Als u de status van een bepaalde virtuele machine wilt ophalen, gebruikt u de opdracht [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm). Zorg ervoor dat u een geldige naam opgeeft voor de virtuele machine en resourcegroep. 

```azurepowershell-interactive
Get-AzureRmVM `
    -ResourceGroupName "myResourceGroupVM" `
    -Name "myVM" `
    -Status | Select @{n="Status"; e={$_.Statuses[1].Code}}
```

Uitvoer:

```azurepowershell-interactive
Status
------
PowerState/running
```

## <a name="management-tasks"></a>Beheertaken

Tijdens de levenscyclus van een virtuele machine wilt u mogelijk beheertaken uitvoeren, zoals het starten, stoppen of verwijderen van een virtuele machine. Misschien wilt u ook scripts maken voor het automatiseren van terugkerende of complexe taken. Met Azure PowerShell kunnen veel algemene beheertaken worden uitgevoerd vanaf de opdrachtregel of in scripts.

### <a name="stop-virtual-machine"></a>Virtuele machine stoppen

U kunt een virtuele machine stoppen en de toewijzing ervan ongedaan maken met de opdracht [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm):

```azurepowershell-interactive
Stop-AzureRmVM -ResourceGroupName "myResourceGroupVM" -Name "myVM" -Force
```

Als u wilt dat de virtuele machine de status Ingericht houdt, gebruikt u de parameter -StayProvisioned.

### <a name="start-virtual-machine"></a>Virtuele machine starten

```azurepowershell-interactive
Start-AzureRmVM -ResourceGroupName "myResourceGroupVM" -Name "myVM"
```

### <a name="delete-resource-group"></a>Resourcegroep verwijderen

Als u een resourcegroep verwijdert, verwijdert u ook alle resources binnen de groep.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name "myResourceGroupVM" -Force
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd over basistaken voor het maken en beheren van een virtuele machine, zoals:

> [!div class="checklist"]
> * Een virtuele machine maken en verbinding maken met een virtuele machine
> * VM-installatiekopieën selecteren en gebruiken
> * Specifieke VM-grootten weergeven en gebruiken
> * De grootte van een virtuele machine wijzigen
> * De status van een virtuele machine weergeven en begrijpen

In de volgende zelfstudie leert u meer over VM-schijven.  

> [!div class="nextstepaction"]
> [VM-schijven maken en beheren](./tutorial-manage-data-disk.md)
