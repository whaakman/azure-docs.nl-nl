---
title: Maken van een virtuele machine van Azure met versnelde toegang | Microsoft Docs
description: Informatie over het maken van een virtuele Linux-machine met versnelde netwerken.
services: virtual-network
documentationcenter: ''
author: jdial
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 01/04/2018
ms.author: jimdial
ms.openlocfilehash: 6d7e41b2b631fcecefd835a10e9b91fd9bb3f17d
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/04/2018
---
# <a name="create-a-windows-virtual-machine-with-accelerated-networking"></a>Een Windows-machine maken met versnelde toegang

In deze zelfstudie leert u het maken van virtuele Windows-machine (VM) met versnelde netwerken. Zie voor informatie over het maken van een Linux-VM met versnelde toegang [Maak een Linux-VM met versnelde netwerken](create-vm-accelerated-networking-cli.md). Versnelde netwerken kan één i/o-virtualisatie hoofdmap (SR-IOV) voor een virtuele machine, aanzienlijk verbeteren de prestaties van netwerken. Dit pad hoge prestaties wordt de host van het gegevenspad, waardoor latentie en jitter CPU-gebruik, voor gebruik met de zwaarste netwerkbelasting op ondersteunde VM typen overgeslagen. De volgende afbeelding ziet communicatie tussen twee virtuele machines met en zonder versnelde netwerken:

![Vergelijking](./media/create-vm-accelerated-networking/accelerated-networking.png)

Alle netwerkverkeer naar en vanuit de virtuele machine moet zonder versnelde netwerken passeren de host en de virtuele switch. De virtuele switch biedt alle afdwingen van beleid, zoals netwerkbeveiligingsgroepen, access control list, isolatie- en andere gevirtualiseerd netwerkservices op netwerkverkeer. Lees voor meer informatie over virtuele switches, de [Hyper-V-netwerkvirtualisatie en virtuele switch](https://technet.microsoft.com/library/jj945275.aspx) artikel.

Met versnelde netwerken netwerkverkeer binnenkomt bij de VM-netwerkadapter (NIC) en wordt vervolgens doorgestuurd naar de virtuele machine. Alle netwerkbeleid dat de virtuele switch is van toepassing zijn nu offloaded en toegepast in de hardware. Toepassen van beleid in de hardware, kunt de NIC forward netwerkverkeer rechtstreeks naar de virtuele machine, het omzeilen van de host en de virtuele switch, terwijl alle het beleid dat wordt toegepast op de host.

De voordelen van versnelde netwerken zijn alleen van toepassing op de virtuele machine die is ingeschakeld. Voor de beste resultaten is het ideaal voor het inschakelen van deze functie op ten minste twee virtuele machines die zijn verbonden met de dezelfde Azure Virtual Network (VNet). Tijdens de communicatie tussen VNets of verbindende on-premises, heeft dit onderdeel minimale gevolgen voor de algehele latentie.

## <a name="benefits"></a>Voordelen
* **Lagere latentie / hoger pakketten per seconde (pps):** verwijderen van de virtuele switch uit het gegevenspad verwijdert u de tijd te besteden aan de pakketten in de host voor de verwerking van beleid en verhoogt het aantal pakketten dat kan worden verwerkt in de virtuele machine.
* **Minder jitter:** virtuele switch verwerking is afhankelijk van de hoeveelheid beleid die moet worden toegepast en de werkbelasting van de CPU die bezig is met de verwerking. Offloading van het afdwingen van beleid op de hardware verwijdert die variabiliteit door het leveren van pakketten rechtstreeks naar de virtuele machine, het verwijderen van de host voor VM-communicatie en alle software-interrupts en context switches.
* **CPU-gebruik verlaagd:** overslaan van de virtuele switch op de host leidt tot minder CPU-gebruik voor het verwerken van netwerkverkeer.

## <a name="limitations-and-constraints"></a>Beperkingen en -beperkingen

### <a name="supported-operating-systems"></a>Ondersteunde besturingssystemen
De volgende distributies worden buiten het uit de galerie van Azure ondersteund: 
* **Windows Server 2016 Datacenter** 
* **Windows Server 2012 R2 Datacenter** 

### <a name="supported-vm-instances"></a>Ondersteunde VM-exemplaren
Versnelde netwerken wordt ondersteund op de meest algemene doeleinden en geoptimaliseerd voor compute exemplaar groottes met 2 of hoger vcpu's.  Deze reeks ondersteund zijn: D/DSv2 en F/Fs

Op de exemplaren die ondersteuning bieden voor hyperthreading, wordt versnelde toegang ondersteund op VM-instanties met 4 of meer vcpu's. Ondersteunde reeksen zijn: D/DSv3, E/ESv3 Fsv2 en Ms-en Mms

Zie voor meer informatie over VM-instanties [Windows VM-grootten](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

### <a name="regions"></a>Regio's
Beschikbaar in alle openbare Azure-regio's en Azure Government Cloud.

### <a name="enabling-accelerated-networking-on-a-running-vm"></a>Versnelde netwerkgebruik op een actieve virtuele machine inschakelen
Een ondersteunde VM-grootte zonder versnelde netwerken ingeschakeld kan alleen hebben voor de functie is ingeschakeld wanneer het wordt gestopt en de toewijzing ongedaan gemaakt.

### <a name="deployment-through-azure-resource-manager"></a>Implementatie via Azure Resource Manager
Virtuele machines (klassiek) kan niet worden geïmplementeerd met versnelde toegang.

## <a name="create-a-windows-vm-with-azure-accelerated-networking"></a>Een Windows-VM met Azure versnelde netwerken maken

Hoewel dit artikel stappen bevat voor het maken van een virtuele machine met versnelde netwerken met Azure PowerShell, kunt u ook [maken van een virtuele machine met versnelde netwerken met de Azure portal](../virtual-machines/windows/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Bij het maken van een virtuele machine in de portal onder **instellingen**, selecteer **ingeschakeld**onder **versnelde netwerken**. De optie voor het inschakelen van versnelde netwerken wordt niet weergegeven in de portal, tenzij u hebt geselecteerd een [ondersteund besturingssysteem](#supported-operating-systems) en [VM-grootte](#supported-vm-instances). Nadat de virtuele machine is gemaakt, moet u de instructies in voltooien [Bevestig het stuurprogramma in het besturingssysteem is geïnstalleerd](#confirm-the-driver-is-installed-in-the-operating-system).

## <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

Installeer [Azure PowerShell](/powershell/azure/install-azurerm-ps) versie 5.1.1 of hoger. Ga voor de momenteel geïnstalleerde versie uitvoeren `Get-Module -ListAvailable AzureRM`. Als u wilt installeren of upgraden, installeert u de nieuwste versie van de module AzureRM op basis van de [PowerShell Gallery](https://www.powershellgallery.com/packages/AzureRM). In een PowerShell-sessie, meld u aan bij uw Azure-account met behulp [Connect-AzureRmAccount](/powershell/module/azurerm.profile/connect-azurermaccount).

In de volgende voorbeelden kunt u de parameternamen voorbeeld vervangen door uw eigen waarden. Voorbeeld parameternamen opgenomen *myResourceGroup*, *myNic*, en *myVM*.

Maak een resourcegroep met behulp van de opdracht [New-AzureRmResourceGroup](/powershell/module/AzureRM.Resources/New-AzureRmResourceGroup). Het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* in de *centralus* locatie:

```powershell
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "centralus"
```

Maak eerst een subnetconfiguratie met [nieuw AzureRmVirtualNetworkSubnetConfig](/powershell/module/AzureRM.Network/New-AzureRmVirtualNetworkSubnetConfig). Het volgende voorbeeld wordt een subnet met de naam *mySubnet*:

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig `
    -Name "mySubnet" `
    -AddressPrefix "192.168.1.0/24"
```

Maak een virtueel netwerk met [New-AzureRmVirtualNetwork](/powershell/module/AzureRM.Network/New-AzureRmVirtualNetwork), met de *mySubnet* subnet.

```powershell
$vnet = New-AzureRmVirtualNetwork -ResourceGroupName "myResourceGroup" `
    -Location "centralus" `
    -Name "myVnet" `
    -AddressPrefix "192.168.0.0/16" `
    -Subnet $Subnet
```

## <a name="create-a-network-security-group"></a>Een netwerkbeveiligingsgroep maken

Maak eerst een groep van de netwerkbeveiligingsregel met [nieuw AzureRmNetworkSecurityRuleConfig](/powershell/module/AzureRM.Network/New-AzureRmNetworkSecurityRuleConfig).

```powershell
$rdp = New-AzureRmNetworkSecurityRuleConfig `
    -Name 'Allow-RDP-All' `
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

Maken van een netwerkbeveiligingsgroep met [nieuw AzureRmNetworkSecurityGroup](/powershell/module/AzureRM.Network/New-AzureRmNetworkSecurityGroup) en wijs de *toestaan-RDP-All* beveiligingsregel aan. Naast de *toestaan-RDP-All* regel, de netwerkbeveiligingsgroep bevat verschillende standaardregels. Een standaardregel schakelt alle binnenkomende toegang via Internet, is de reden waarom de *toestaan-RDP-All* regel is toegewezen aan de netwerkbeveiligingsgroep zodat u op afstand verbinding met de virtuele machine, maken kunt zodra deze gemaakt.

```powershell
$nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName myResourceGroup `
    -Location centralus `
    -Name "myNsg" `
    -SecurityRules $rdp
```

Koppelen van de netwerkbeveiligingsgroep voor de *mySubnet* subnet met [Set AzureRmVirtualNetworkSubnetConfig](/powershell/module/AzureRM.Network/Set-AzureRmVirtualNetworkSubnetConfig). De regel in de netwerkbeveiligingsgroep is geschikt voor alle resources die zijn geïmplementeerd in het subnet.

```powershell
Set-AzureRmVirtualNetworkSubnetConfig `
    -VirtualNetwork $vnet `
    -Name 'mySubnet' `
    -AddressPrefix "192.168.1.0/24" `
    -NetworkSecurityGroup $nsg
```

## <a name="create-a-network-interface-with-accelerated-networking"></a>Een netwerkinterface met versnelde netwerken maken
Maak een openbaar IP-adres met [New-AzureRmPublicIpAddress](/powershell/module/AzureRM.Network/New-AzureRmPublicIpAddress). Een openbaar IP-adres is niet vereist als u niet van plan bent voor toegang tot de virtuele machine vanaf het Internet, maar de stappen in dit artikel uit te voeren, dit is vereist.

```powershell
$publicIp = New-AzureRmPublicIpAddress `
    -ResourceGroupName myResourceGroup `
    -Name 'myPublicIp' `
    -location centralus `
    -AllocationMethod Dynamic
```

Maken van een netwerkinterface met [nieuw AzureRmNetworkInterface](/powershell/module/AzureRM.Network/New-AzureRmNetworkInterface) versnelde met netwerken die zijn ingeschakeld en het openbare IP-adres toewijzen aan de netwerkinterface. Het volgende voorbeeld wordt een netwerkinterface met de naam *myNic* in de *mySubnet* subnet van de *myVnet* virtueel netwerk en wijst de *myPublicIp*  openbaar IP-adres toe:

```powershell
$nic = New-AzureRmNetworkInterface `
    -ResourceGroupName "myResourceGroup" `
    -Name "myNic" `
    -Location "centralus" `
    -SubnetId $vnet.Subnets[0].Id `
    -PublicIpAddressId $publicIp.Id `
    -EnableAcceleratedNetworking
```

## <a name="create-the-virtual-machine"></a>De virtuele machine maken

De referenties van uw virtuele machine ingesteld op de `$cred` met behulp van variabele [Get-Credential](/powershell/module/microsoft.powershell.security/get-credential):

```powershell
$cred = Get-Credential
```

Eerste stap definieert u uw virtuele machine met [nieuw AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig). Het volgende voorbeeld definieert een virtuele machine met de naam *myVM* met een VM-grootte die ondersteuning biedt voor versnelde Networking (*Standard_DS4_v2*):

```powershell
$vmConfig = New-AzureRmVMConfig -VMName "myVm" -VMSize "Standard_DS4_v2"
```

Zie voor een lijst van alle VM-grootten en kenmerken [Windows VM-grootten](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Maken van de rest van de VM-configuratie met [Set AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) en [Set AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage). Het volgende voorbeeld wordt een Windows Server 2016 VM:

```powershell
$vmConfig = Set-AzureRmVMOperatingSystem -VM $vmConfig `
    -Windows `
    -ComputerName "myVM" `
    -Credential $cred `
    -ProvisionVMAgent `
    -EnableAutoUpdate
$vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig `
    -PublisherName "MicrosoftWindowsServer" `
    -Offer "WindowsServer" `
    -Skus "2016-Datacenter" `
    -Version "latest"
```

Koppelen van de netwerkinterface die u eerder hebt gemaakt met [toevoegen AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):

```powershell
$vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id
```

Maak ten slotte uw virtuele machine met [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm):

```powershell
New-AzureRmVM -VM $vmConfig -ResourceGroupName "myResourceGroup" -Location "centralus"
```

## <a name="confirm-the-driver-is-installed-in-the-operating-system"></a>Bevestig dat het stuurprogramma in het besturingssysteem is geïnstalleerd

Zodra u de virtuele machine in Azure maakt, verbinding maken met de virtuele machine en controleer of het stuurprogramma in Windows is geïnstalleerd.

1. Open Azure in een internetbrowser [portal](https://portal.azure.com) en meld u aan met uw Azure-account.
2. In het vak dat de tekst bevat *zoeken bronnen* aan de bovenkant van de Azure-portal, typ *myVm*. Wanneer **myVm** wordt weergegeven in de zoekresultaten, klik erop. Als **maken** zichtbaar onder de **Connect** knop Azure is nog niet voltooid voor het maken van de virtuele machine. Klik op **Connect** in de linkerbovenhoek van het overzicht nadat u hebt niet langer bekijken **maken** onder de **Connect** knop.
3. Geef de gebruikersnaam en wachtwoord die u hebt ingevoerd in [Maak de virtuele machine](#create-the-virtual-machine). Als u niet met een Windows-VM in Azure verbonden hebt, Zie [verbinding maken met virtuele machine](../virtual-machines/windows/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#connect-to-virtual-machine).
4. Met de rechtermuisknop op de knop Start en klik op **Apparaatbeheer**. Vouw de **netwerkadapters** knooppunt. Controleer of de **Mellanox ConnectX 3 virtuele functie Ethernet-Adapter** wordt weergegeven, zoals wordt weergegeven in de volgende afbeelding:

    ![Apparaatbeheer](./media/create-vm-accelerated-networking/device-manager.png)

Versnelde netwerken is nu ingeschakeld voor uw virtuele machine.

## <a name="enable-accelerated-networking-on-existing-vms"></a>Versnelde netwerkgebruik op bestaande virtuele machines inschakelen
Als u een virtuele machine zonder versnelde netwerken hebt gemaakt, is het mogelijk is deze functie op een bestaande virtuele machine in te schakelen.  De virtuele machine moet versnelde netwerken via ondersteunen voldoen aan de volgende vereisten die ook hierboven worden beschreven:

* De virtuele machine moet een ondersteunde grootte voor versnelde netwerken
* De virtuele machine moet een ondersteunde Azure-galerie-installatiekopie (en de kernelversie voor Linux)
* Alle virtuele machines in een beschikbaarheidsset of VMSS moet worden gestopt/ongedaan voordat u inschakelt op een NIC versnelde netwerken

### <a name="individual-vms--vms-in-an-availability-set"></a>Afzonderlijke virtuele machines en virtuele machines in een beschikbaarheidsset instellen
Eerst stoppen/ongedaan de virtuele machine of als een Beschikbaarheidsset, alle virtuele machines in de Set:

```azurepowershell
Stop-AzureRmVM -ResourceGroup "myResourceGroup" `
    -Name "myVM"
```

Belangrijk, neem Opmerking: als uw virtuele machine afzonderlijk, is gemaakt zonder een beschikbaarheidsset u alleen moet stoppen/toewijzing van de afzonderlijke VM om in te schakelen versnelde netwerken.  Als uw virtuele machine is gemaakt met een beschikbaarheidsset, moet alle virtuele machines die zijn opgenomen in de beschikbaarheidsset worden gestopt/ongedaan voordat u inschakelt op een van de NIC's versnelde netwerken. 

Nadat gestopt, kunt u inschakelen versnelde netwerken op de NIC van uw virtuele machine:

```azurepowershell
$nic = Get-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Name "myNic"

$nic.EnableAcceleratedNetworking = $true

$nic | Set-AzureRmNetworkInterface
```

Opnieuw opstarten van uw virtuele machine of, als in een Beschikbaarheidsset, alle virtuele machines in de Set en controleer of de versnelde netwerken is ingeschakeld: 

```azurepowershell
Start-AzureRmVM -ResourceGroup "myResourceGroup" `
    -Name "myVM"
```

### <a name="vmss"></a>VMSS
VMSS verschilt enigszins maar dezelfde werkstroom volgt.  Eerst de virtuele machines stoppen:

```azurepowershell
Stop-AzureRmVmss -ResourceGroupName "myResourceGroup" ` 
    -VMScaleSetName "myScaleSet"
```

Nadat de virtuele machines zijn gestopt, moet u de eigenschap versnelde netwerken onder de netwerkinterface bijwerken:

```azurepowershell
$vmss = Get-AzureRmVmss -ResourceGroupName "myResourceGroup" `
    -VMScaleSetName "myScaleSet"

$vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].EnableAcceleratedNetworking = $true

Update-AzureRmVmss -ResourceGroupName "myResourceGroup" `
    -VMScaleSetName "myScaleSet" `
    -VirtualMachineScaleSet $vmss
```

Neem heeft notitie, een VMSS VM upgrades die met drie verschillende instellingen automatische, rolling of handmatige updates toepassen.  Het beleid is ingesteld op automatisch in deze instructies zodat de VMSS opgehaald de wijzigingen onmiddellijk na het opnieuw te starten.  Zodat de wijzigingen onmiddellijk worden opgepikt, moet u deze instellen op automatisch: 

```azurecli
$vmss.UpgradePolicy.AutomaticOSUpgrade = $true

Update-AzureRmVmss -ResourceGroupName "myResourceGroup" `
    -VMScaleSetName "myScaleSet" `
    -VirtualMachineScaleSet $vmss
```

Ten slotte de VMSS starten:

```azurecli
Start-AzureRmVmss -ResourceGroupName "myResourceGroup" ` 
    -VMScaleSetName "myScaleSet"
```

Eenmaal u opnieuw opstarten, wacht u totdat de updates te voltooien maar één keer zijn voltooid, de VF wordt weergegeven in de virtuele machine.  (Zorg ervoor dat u een ondersteund besturingssysteem en de VM-grootte)

### <a name="resizing-existing-vms-with-accelerated-networking"></a>Bestaande virtuele machines met versnelde toegang vergroten of verkleinen

De grootte van virtuele machines met versnelde toegang ingeschakeld kunnen alleen worden gewijzigd voor VM's die ondersteuning bieden voor versnelde netwerken.  

Een virtuele machine met versnelde toegang ingeschakeld kan niet worden aangepast aan een VM-instantie die geen ondersteuning biedt voor versnelde netwerken met behulp van de bewerking formaat wijzigen.  In plaats daarvan het formaat van een van deze virtuele machines: 

* De virtuele machine stoppen/Deallocate of als deze in een beschikbaarheidsgroep set/VMSS, stop/toewijzing alle VM's in de set/VMSS.
* Versnelde netwerken moet worden uitgeschakeld op de NIC van de virtuele machine of als in een beschikbaarheidsgroep set/VMSS, alle virtuele machines in de set/VMSS.
* Zodra de versnelde toegang is uitgeschakeld, kan de set VM/beschikbaarheid/VMSS worden verplaatst naar een nieuwe grootte die biedt geen ondersteuning voor versnelde netwerken en opnieuw opgestart.  



