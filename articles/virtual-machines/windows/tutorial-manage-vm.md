---
title: Maken en beheren van Windows virtuele machines met de Azure PowerShell-module | Microsoft Docs
description: Zelfstudie - maken en beheren van Windows virtuele machines met de Azure PowerShell-module
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/02/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: c612a251105197ab2b46bf448ae39253e5a65f36
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/17/2017
---
# <a name="create-and-manage-windows-vms-with-the-azure-powershell-module"></a>Maken en beheren van Windows virtuele machines met de Azure PowerShell-module

Virtuele machines van Azure bieden een volledig worden geconfigureerd en flexibele computeromgeving. Deze zelfstudie bevat informatie over basic virtuele machine van Azure-implementatie items zoals het selecteren van een VM-grootte, een VM-installatiekopie te selecteren en implementeren van een virtuele machine. Procedures voor:

> [!div class="checklist"]
> * Maken en verbinding maken met een virtuele machine
> * Selecteer en gebruik van VM-installatiekopieën
> * Weergeven en gebruiken van specifieke VM-grootten
> * De grootte van een virtuele machine wijzigen
> * Bekijken en te begrijpen status virtuele machine


[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Als u PowerShell lokaal wilt installeren en gebruiken, wordt voor deze zelfstudie moduleversie 3.6 of hoger van Azure PowerShell vereist. Voer ` Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Login-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure. 

## <a name="create-resource-group"></a>Een resourcegroep maken

Maak een resourcegroep met de opdracht [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). 

Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. Een resourcegroep moet worden gemaakt voordat een virtuele machine. In dit voorbeeld wordt een resourcegroep met de naam *myResourceGroupVM* wordt gemaakt in de *EastUS* regio. 

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName myResourceGroupVM -Location EastUS
```

De resourcegroep is opgegeven bij het maken of wijzigen van een virtuele machine die in deze zelfstudie kan worden gezien.

## <a name="create-virtual-machine"></a>Virtuele machine maken

Een virtuele machine moet worden verbonden met een virtueel netwerk. U communiceren met de virtuele machine via een openbaar IP-adres via een netwerkinterfacekaart.

### <a name="create-virtual-network"></a>Virtueel netwerk maken

Maak een subnet met [nieuw AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig):

```azurepowershell-interactive
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24
```

Maak een virtueel netwerk met [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork):

```azurepowershell-interactive
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroupVM `
  -Location EastUS `
  -Name myVnet `
  -AddressPrefix 192.168.0.0/16 `
  -Subnet $subnetConfig
```
### <a name="create-public-ip-address"></a>Openbare IP-adres maken

Maken van een openbaar IP-adres met [nieuw AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress):

```azurepowershell-interactive
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroupVM `
  -Location EastUS `
  -AllocationMethod Static `
  -Name myPublicIPAddress
```

### <a name="create-network-interface-card"></a>Maken van netwerkinterfacekaart

Maken van een netwerkkaart met [nieuw AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface):

```azurepowershell-interactive
$nic = New-AzureRmNetworkInterface `
  -ResourceGroupName myResourceGroupVM  `
  -Location EastUS `
  -Name myNic `
  -SubnetId $vnet.Subnets[0].Id `
  -PublicIpAddressId $pip.Id
```

### <a name="create-network-security-group"></a>Netwerkbeveiligingsgroep maken

Een Azure [netwerkbeveiligingsgroep](../../virtual-network/virtual-networks-nsg.md) (NSG) Hiermee wordt bepaald binnenkomend en uitgaand verkeer voor een of meer virtuele machines. Netwerkbeveiligingsgroepen toestaan of weigeren van netwerkverkeer op een specifieke poort of een poortbereik. Deze regels kunnen ook een voorvoegsel voor bronadres bevatten, zodat alleen verkeer dat afkomstig is op een vooraf gedefinieerde bron met een virtuele machine communiceren kan. Voor toegang tot de IIS-webserver die u installeert, moet u een inkomende NSG-regel toevoegen.

Gebruik voor het maken van een inkomende regel voor het NSG [toevoegen AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/add-azurermnetworksecurityruleconfig). Het volgende voorbeeld wordt een NSG regel voor licentiecontrole *myNSGRule* opent die poort *3389* voor de virtuele machine:

```azurepowershell-interactive
$nsgRule = New-AzureRmNetworkSecurityRuleConfig `
  -Name myNSGRule `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 1000 `
  -SourceAddressPrefix * `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 3389 `
  -Access Allow
```

Maak het NSG met *myNSGRule* met [nieuw AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup):

```azurepowershell-interactive
$nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName myResourceGroupVM `
    -Location EastUS `
    -Name myNetworkSecurityGroup `
    -SecurityRules $nsgRule
```

Het NSG toevoegen aan het subnet in het virtuele netwerk met [Set AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig):

```azurepowershell-interactive
Set-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -VirtualNetwork $vnet `
    -NetworkSecurityGroup $nsg `
    -AddressPrefix 192.168.1.0/24
```

Bijwerken van het virtuele netwerk met [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork):

```azurepowershell-interactive
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

### <a name="create-virtual-machine"></a>Virtuele machine maken

Wanneer u een virtuele machine maakt, er zijn diverse opties beschikbaar zoals besturingssysteemkopie schijf sizing en administratieve referenties. In dit voorbeeld wordt een virtuele machine gemaakt met de naam *myVM* de nieuwste versie van Windows Server 2016 Datacenter.

Stel de gebruikersnaam en wachtwoord nodig voor het beheerdersaccount op de virtuele machine met [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```azurepowershell-interactive
$cred = Get-Credential
```

Maken van de eerste configuratie voor de virtuele machine met [nieuw AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig):

```azurepowershell-interactive
$vm = New-AzureRmVMConfig -VMName myVM -VMSize Standard_D1
```

Informatie over het besturingssysteem toevoegen aan de virtuele-machineconfiguratie met [Set AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem):

```azurepowershell-interactive
$vm = Set-AzureRmVMOperatingSystem `
    -VM $vm `
    -Windows `
    -ComputerName myVM `
    -Credential $cred `
    -ProvisionVMAgent -EnableAutoUpdate
```

De informatie van de installatiekopie toevoegen aan de virtuele-machineconfiguratie met [Set AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage):

```azurepowershell-interactive
$vm = Set-AzureRmVMSourceImage `
    -VM $vm `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest
```

De instellingen van het besturingssysteem schijf toevoegen aan de virtuele-machineconfiguratie met [Set AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk):

```azurepowershell-interactive
$vm = Set-AzureRmVMOSDisk `
    -VM $vm `
    -Name myOsDisk `
    -DiskSizeInGB 128 `
    -CreateOption FromImage `
    -Caching ReadWrite
```

Toevoegen van de netwerkinterfacekaart die u eerder hebt gemaakt voor de virtuele-machineconfiguratie met [toevoegen AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):

```azurepowershell-interactive
$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

Maak de virtuele machine met [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).

```azurepowershell-interactive
New-AzureRmVM -ResourceGroupName myResourceGroupVM -Location EastUS -VM $vm
```

## <a name="connect-to-vm"></a>Verbinding maken met virtuele machine

Wanneer de implementatie is voltooid, maakt u via een extern bureaublad verbinding met de virtuele machine.

Voer de volgende opdrachten uit om het openbare IP-adres van de virtuele machine te retourneren. Noteer dit IP-adres zodat u vanuit uw browser verbinding kunt maken met het adres om webverbindingen te testen.

```azurepowershell-interactive
Get-AzureRmPublicIpAddress -ResourceGroupName myResourceGroupVM  | Select IpAddress
```

Gebruik de volgende opdracht op uw lokale machine om een sessie met een extern bureaublad te starten voor de virtuele machine. Vervang het IP-adres door het *publicIPAddress* van de virtuele machine. Wanneer u hierom wordt gevraagd, typt u de referenties die zijn gebruikt bij het maken van de virtuele machine.

```powershell
mstsc /v:<publicIpAddress>
```

## <a name="understand-vm-images"></a>VM-installatiekopieën begrijpen

De Azure marketplace bevat veel installatiekopieën van virtuele machines die kunnen worden gebruikt voor het maken van een nieuwe virtuele machine. In de vorige stappen is een virtuele machine gemaakt met behulp van de installatiekopie van het Windows Server 2016-Datacenter. In deze stap wordt de PowerShell-module gebruikt voor het zoeken van de marketplace voor andere Windows-installatiekopieën ook als basis voor de nieuwe virtuele machines kunnen. Dit proces bestaat uit het vinden van de uitgever, aanbieding en de naam van de installatiekopie (Sku). 

Gebruik de [Get-AzureRmVMImagePublisher](/powershell/module/azurerm.compute/get-azurermvmimagepublisher) opdracht voor het retourneren van een lijst van uitgevers van de installatiekopie.  

```powersehll
Get-AzureRmVMImagePublisher -Location "EastUS"
```

Gebruik de [Get-AzureRmVMImageOffer](/powershell/module/azurerm.compute/get-azurermvmimageoffer) retourneren een lijst met aanbiedingen van de installatiekopie. Met deze opdracht wordt de geretourneerde lijst gefilterd op de opgegeven uitgever. 

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

De [Get-AzureRmVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) opdracht filter vervolgens op de naam van de uitgever en de aanbieding te retourneren van een lijst met namen van afbeeldingen.

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

Deze informatie kan worden gebruikt voor het implementeren van een virtuele machine met een specifieke installatiekopie. In het volgende voorbeeld wordt de naam van de installatiekopie van het VM-object. Raadpleeg de eerdere voorbeelden in deze zelfstudie voor volledige implementatiestappen.

```azurepowershell-interactive
$vm = Set-AzureRmVMSourceImage `
    -VM $vm `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter-with-Containers `
    -Version latest
```

## <a name="understand-vm-sizes"></a>VM-grootten begrijpen

De grootte van een virtuele machine bepaalt de hoeveelheid compute-bronnen zoals CPU en GPU geheugen die beschikbaar zijn gesteld voor de virtuele machine. Virtuele machines moeten worden gemaakt met een geschikt is voor de werklast van de verwachte grootte. Als de werkbelasting toeneemt, kan een bestaande virtuele machine kan worden gewijzigd.

### <a name="vm-sizes"></a>VM-grootten

De volgende tabel categoriseert grootten in gebruiksvoorbeelden.  

| Type                     | Grootten           |    Beschrijving       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| Algemeen doel         |DSv2, Dv2, DS, D, Av2, A0 7| Taakverdeling CPU-naar-geheugen. Ideaal voor dev / testen en in kleine tot middelgrote oplossingen voor toepassingen en gegevens.  |
| Geoptimaliseerde rekenkracht      | FS, F             | Hoog CPU-naar-geheugen. Goede voor gemiddeld verkeer toepassingen, netwerkapparatuur en batchprocessen.        |
| Geoptimaliseerd geheugen       | GS, G, DSv2, DS, Dv2, D   | Hoge geheugen-naar-CPU. Ideaal voor relationele databases, middelgrote tot grote caches en in het geheugen analytics.                 |
| Geoptimaliseerde opslag       | Ls                | Snelle doorvoer van schijfgegevens en IO. Ideaal voor big data-, SQL- en NoSQL-databases.                                                         |
| GPU           | NV, NC            | Gespecialiseerde VMs gericht voor zware grafische weergave en het bewerken van video's.       |
| Hoge prestaties | H, A8 11          | De krachtigste CPU VMs met optionele hoge gegevensdoorvoer netwerkinterfaces (RDMA). 


### <a name="find-available-vm-sizes"></a>Zoeken naar beschikbare VM-grootten

Als een lijst met VM-grootten die beschikbaar zijn in een bepaalde regio wilt weergeven, gebruikt de [Get-AzureRmVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) opdracht.

```azurepowershell-interactive
Get-AzureRmVMSize -Location EastUS
```

## <a name="resize-a-vm"></a>De grootte van een virtuele machine wijzigen

Nadat een virtuele machine is geïmplementeerd, kan het vergroten of verkleinen resourcetoewijzing worden gewijzigd.

Grootte wijzigen van een virtuele machine, Controleer of de gewenste grootte beschikbaar op de huidige VM-cluster is. De [Get-AzureRmVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) opdracht retourneert een lijst met grootten. 

```azurepowershell-interactive
Get-AzureRmVMSize -ResourceGroupName myResourceGroupVM -VMName myVM 
```

Als de gewenste grootte beschikbaar is, kan de virtuele machine worden gewijzigd van een status ingeschakeld op, maar deze opnieuw wordt opgestart tijdens de bewerking.

```azurepowershell-interactive
$vm = Get-AzureRmVM -ResourceGroupName myResourceGroupVM  -VMName myVM 
$vm.HardwareProfile.VmSize = "Standard_D4"
Update-AzureRmVM -VM $vm -ResourceGroupName myResourceGroupVM 
```

Als de gewenste grootte niet op het huidige cluster is, moet de VM ongedaan voordat de bewerking formaat kan plaatsvinden. Opmerking: als de virtuele machine weer is ingeschakeld, alle gegevens op de tijdelijke schijf worden verwijderd en wordt de openbare IP-adressen wijzigen tenzij u een statisch IP-adres wordt gebruikt. 

```azurepowershell-interactive
Stop-AzureRmVM -ResourceGroupName myResourceGroupVM -Name "myVM" -Force
$vm = Get-AzureRmVM -ResourceGroupName myResourceGroupVM  -VMName myVM
$vm.HardwareProfile.VmSize = "Standard_F4s"
Update-AzureRmVM -VM $vm -ResourceGroupName myResourceGroupVM 
Start-AzureRmVM -ResourceGroupName myResourceGroupVM  -Name $vm.name
```

## <a name="vm-power-states"></a>VM-energiestatus

Een Azure VM kan een van de vele energiestatussen hebben. Deze status vertegenwoordigt de huidige status van de virtuele machine vanuit het oogpunt van de hypervisor. 

### <a name="power-states"></a>Energiestatus

| Energieniveau | Beschrijving
|----|----|
| Starting | Geeft aan dat de virtuele machine wordt gestart. |
| Actief | Hiermee wordt aangegeven dat de virtuele machine wordt uitgevoerd. |
| Stopping | Hiermee wordt aangegeven dat de virtuele machine wordt gestopt. | 
| Stopped | Hiermee wordt aangegeven dat de virtuele machine is gestopt. Houd er rekening mee dat virtuele machines in de gestopte status nog steeds compute worden kosten in rekening.  |
| De toewijzing wordt ongedaan gemaakt | Hiermee wordt aangegeven dat de virtuele machine wordt opgeheven. |
| Toewijzing ongedaan gemaakt | Hiermee wordt aangegeven dat de virtuele machine volledig verwijderd van de hypervisor, maar nog steeds beschikbaar in het vlak van het besturingselement wordt. Virtuele machines in de status van de Deallocated kan niet worden compute-kosten in rekening. |
| - | Hiermee wordt aangegeven dat de voedingsstatus van de virtuele machine onbekend is. |

### <a name="find-power-state"></a>Energieniveau vinden

Voor het ophalen van de status van een bepaalde virtuele machine, gebruikt u de [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) opdracht. Zorg ervoor dat een geldige naam voor een virtuele machine en de resourcegroep opgeven. 

```azurepowershell-interactive
Get-AzureRmVM `
    -ResourceGroupName myResourceGroupVM `
    -Name myVM `
    -Status | Select @{n="Status"; e={$_.Statuses[1].Code}}
```

Uitvoer:

```azurepowershell-interactive
Status
------
PowerState/running
```

## <a name="management-tasks"></a>Beheertaken

Tijdens de levenscyclus van een virtuele machine, kan u wilt uitvoeren van beheertaken, zoals starten, stoppen of een virtuele machine wordt verwijderd. Bovendien wilt u maken van scripts voor terugkerende of complexe taken automatiseren. Met Azure PowerShell, kunnen veel algemene beheertaken worden uitgevoerd vanaf de opdrachtregel of in scripts.

### <a name="stop-virtual-machine"></a>Virtuele machine stoppen

Stop en toewijzing van een virtuele machine met [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm):

```azurepowershell-interactive
Stop-AzureRmVM -ResourceGroupName myResourceGroupVM -Name "myVM" -Force
```

Als u behouden van de virtuele machine in een ingerichte staat wilt, moet u de parameter - StayProvisioned gebruiken.

### <a name="start-virtual-machine"></a>Virtuele machine starten

```azurepowershell-interactive
Start-AzureRmVM -ResourceGroupName myResourceGroupVM -Name myVM
```

### <a name="delete-resource-group"></a>Resourcegroep verwijderen

Verwijderen van een resourcegroep, worden ook alle resources binnen verwijderd.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroupVM -Force
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd over basic VM maken en beheren zoals het:

> [!div class="checklist"]
> * Maken en verbinding maken met een virtuele machine
> * Selecteer en gebruik van VM-installatiekopieën
> * Weergeven en gebruiken van specifieke VM-grootten
> * De grootte van een virtuele machine wijzigen
> * Bekijken en te begrijpen status virtuele machine

Ga naar de volgende zelfstudie voor meer informatie over de VM-schijven.  

> [!div class="nextstepaction"]
> [Maken en beheren van VM-schijven](./tutorial-manage-data-disk.md)
