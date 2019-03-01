---
title: Een Azure-machine maken met versnelde netwerken | Microsoft Docs
description: Informatie over het maken van een virtuele Linux-machine met versnelde netwerken.
services: virtual-network
documentationcenter: ''
author: gsilva5
manager: gedegrac
editor: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 01/04/2018
ms.author: gsilva
ms.openlocfilehash: c4567919490c8bc9094dea3dddbe22550d9eebb2
ms.sourcegitcommit: cdf0e37450044f65c33e07aeb6d115819a2bb822
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57192902"
---
# <a name="create-a-windows-virtual-machine-with-accelerated-networking"></a>Een Windows-machine maken met versnelde netwerken

In deze zelfstudie leert u hoe u een Windows virtuele machine (VM) maken met versnelde netwerken. Zie voor informatie over het maken van een Linux-VM met versnelde netwerken [een Linux-VM maken met versnelde netwerken](create-vm-accelerated-networking-cli.md). Versneld netwerken kan één i/o-virtualisatie hoofdmap (SR-IOV) aan een virtuele machine, aanzienlijk verbeteren de prestaties van netwerken. Dit pad krachtige omzeilt de host van het gegevenspad verminderen latentie en jitter CPU-gebruik, voor gebruik met de meest veeleisende workloads netwerk op ondersteunde VM-typen. De volgende afbeelding ziet u de communicatie tussen twee virtuele machines met en zonder versnelde netwerken:

![Vergelijking](./media/create-vm-accelerated-networking/accelerated-networking.png)

Alle netwerkverkeer naar en uit de virtuele machine moet zonder versnelde netwerken passeren de host en de virtuele switch. De virtuele switch biedt alle afdwingen van beleid, zoals netwerkbeveiligingsgroepen, toegangsbeheerlijsten, isolatie en andere netwerkservices gevirtualiseerd netwerkverkeer. Zie voor meer informatie over virtuele switches, [Hyper-V-netwerkvirtualisatie en virtuele switch](https://technet.microsoft.com/library/jj945275.aspx).

Netwerkverkeer aankomt op van de VM-netwerkinterface (NIC) en wordt vervolgens doorgestuurd naar de virtuele machine met versneld netwerken. Alle beleidsregels voor network die de virtuele switch is van toepassing zijn nu offloaded en toegepast in de hardware. Toepassen van beleid in de hardware, kunt de NIC voor doorsturen van netwerkverkeer rechtstreeks naar de virtuele machine, overslaan van de host en de virtuele switch, terwijl alle het beleid dat wordt toegepast op de host.

De voordelen van versneld netwerken zijn alleen van toepassing op de virtuele machine die is ingeschakeld op. Voor de beste resultaten is het ideaal voor deze functie inschakelen op ten minste twee virtuele machines die zijn verbonden met de dezelfde Azure-netwerk (VNet). Tijdens de communicatie tussen vnet's of verbinding maakt on-premises, heeft deze functie minimale gevolgen voor de totale latentie.

## <a name="benefits"></a>Voordelen
* **Lagere latentie / hoger pakketten per seconde (pps):** De virtuele switch verwijderen uit het gegevenspad elimineert de tijd voor pakketten uitgaven in de host voor de beleidsverwerking van en het aantal pakketten dat kan worden verwerkt binnen de virtuele machine wordt verhoogd.
* **Minder jitter:** Verwerking van de virtuele switch, is afhankelijk van de hoeveelheid beleid die moet worden toegepast en de werkbelasting van de CPU dat de verwerking. Offloading van het afdwingen van beleid voor de hardware verwijdert die variabiliteit door het leveren van pakketten rechtstreeks naar de virtuele machine, het verwijderen van de host de communicatie van de virtuele machine en alle software-interrupts en context switches.
* **Minder CPU-gebruik:** Overslaan van de virtuele switch op de host leidt tot minder CPU-gebruik voor het verwerken van netwerkverkeer.

## <a name="limitations-and-constraints"></a>Beperkingen en beperkingen

### <a name="supported-operating-systems"></a>Ondersteunde besturingssystemen
De volgende distributies worden gebruiksklaar uit de galerie met Azure ondersteund:
* **Windows Server 2016 Datacenter** 
* **Windows Server 2012 R2 Datacenter**

### <a name="supported-vm-instances"></a>Ondersteunde VM-exemplaren
Versneld netwerken wordt ondersteund op de meest algemene en geoptimaliseerde exemplaargrootten met 2 of meer vcpu's.  Deze ondersteunde reeksen zijn: D/DSv2 en F/Fs

Op de VM-exemplaren met 4 of meer vcpu's wordt versnelde netwerken op instanties die ondersteuning bieden voor hyperthreading is, ondersteund. Ondersteunde reeksen zijn: D/Dsv3, E/Esv3, Fsv2, Lsv2, Ms-/ Mms- en Ms/Mmsv2.

Zie voor meer informatie over VM-exemplaren [Windows VM-grootten](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

### <a name="regions"></a>Regio's
Beschikbaar in alle openbare Azure-regio's en Azure Government-Cloud.

### <a name="enabling-accelerated-networking-on-a-running-vm"></a>Versnelde netwerken op een actieve virtuele machine inschakelen
Een ondersteunde VM-grootte zonder versneld netwerkondersteuning ingeschakeld kan alleen hebben voor de functie is ingeschakeld wanneer deze wordt gestopt en toewijzing ongedaan gemaakt.

### <a name="deployment-through-azure-resource-manager"></a>Implementatie via Azure Resource Manager
Virtuele machines (klassiek) kan niet worden geïmplementeerd met versnelde netwerken.

## <a name="create-a-windows-vm-with-azure-accelerated-networking"></a>Een Windows VM maken met Azure-versnelde netwerken
## <a name="portal-creation"></a>Maken van de portal
Hoewel dit artikel stappen bevat voor het maken van een virtuele machine met versneld netwerken met behulp van Azure Powershell, kunt u ook [maken van een virtuele machine met versneld netwerken met behulp van de Azure-portal](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Bij het maken van een virtuele machine in de portal in de **maken van een virtuele machine** blade, kiest u de **netwerken** tabblad.  Op dit tabblad is een optie voor **versnelde netwerken**.  Als u hebt ervoor gekozen een [ondersteund besturingssysteem](#supported-operating-systems) en [VM-grootte](#supported-vm-instances), deze optie wordt automatisch gevuld op 'Op'.  Zo niet, wordt de optie "Uitgeschakeld" voor versnelde netwerken vullen en geef de gebruiker een reden op waarom is het niet worden ingeschakeld.   
* *Opmerking:* Alleen ondersteunde besturingssystemen kan worden ingeschakeld via de portal.  Als u een aangepaste installatiekopie gebruikt en uw installatiekopie versnelde netwerken ondersteunt, maakt u uw virtuele machine met CLI of Powershell. 

Nadat de virtuele machine is gemaakt, kunt u controleren dat aan de hand van de instructies in de controleren of de versnelde netwerken is ingeschakeld versnelde netwerken is ingeschakeld.

## <a name="powershell-creation"></a>Het maken van PowerShell
## <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Installeer [Azure PowerShell](/powershell/azure/install-az-ps) versie 1.0.0 of hoger. Ga voor de momenteel geïnstalleerde versie uitvoeren `Get-Module -ListAvailable Az`. Als u wilt installeren of upgraden, installeert u de nieuwste versie van de Az-module op basis van de [PowerShell Gallery](https://www.powershellgallery.com/packages/Az). In een PowerShell-sessie, moet u zich aanmelden bij een Azure-account met [Connect AzAccount](/powershell/module/az.accounts/connect-azaccount).

In de volgende voorbeelden kunt u voorbeeldnamen parameter vervangen door uw eigen waarden. Voorbeeld van de parameternamen opgenomen *myResourceGroup*, *myNic*, en *myVM*.

Maak een resourcegroep met [New-AzResourceGroup](/powershell/module/az.Resources/New-azResourceGroup). Het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* in de *centralus* locatie:

```powershell
New-AzResourceGroup -Name "myResourceGroup" -Location "centralus"
```

Maak eerst een subnetconfiguratie met [New-AzVirtualNetworkSubnetConfig](/powershell/module/az.Network/New-azVirtualNetworkSubnetConfig). Het volgende voorbeeld wordt een subnet met de naam *mySubnet*:

```powershell
$subnet = New-AzVirtualNetworkSubnetConfig `
    -Name "mySubnet" `
    -AddressPrefix "192.168.1.0/24"
```

Maak een virtueel netwerk met [New-AzVirtualNetwork](/powershell/module/az.Network/New-azVirtualNetwork), met de *mySubnet* subnet.

```powershell
$vnet = New-AzVirtualNetwork -ResourceGroupName "myResourceGroup" `
    -Location "centralus" `
    -Name "myVnet" `
    -AddressPrefix "192.168.0.0/16" `
    -Subnet $Subnet
```

## <a name="create-a-network-security-group"></a>Een netwerkbeveiligingsgroep maken

Maak eerst een netwerkbeveiligingsgroepregel met [New-AzNetworkSecurityRuleConfig](/powershell/module/az.Network/New-azNetworkSecurityRuleConfig).

```powershell
$rdp = New-AzNetworkSecurityRuleConfig `
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

Maak een netwerkbeveiligingsgroep met [New-AzNetworkSecurityGroup](/powershell/module/az.Network/New-azNetworkSecurityGroup) en toe te wijzen de *toestaan-RDP-All* beveiligingsregel toe. Naast de *toestaan-RDP-All* regel, de netwerkbeveiligingsgroep bevat verschillende standaardregels. Een standaardregel schakelt alle binnenkomende toegang via Internet, is de reden waarom de *toestaan-RDP-All* regel is toegewezen aan de netwerkbeveiligingsgroep zodat u op afstand verbinding met de virtuele machine maken kunt nadat deze gemaakt.

```powershell
$nsg = New-AzNetworkSecurityGroup `
    -ResourceGroupName myResourceGroup `
    -Location centralus `
    -Name "myNsg" `
    -SecurityRules $rdp
```

Koppelen van de netwerkbeveiligingsgroep voor de *mySubnet* subnet met de [Set AzVirtualNetworkSubnetConfig](/powershell/module/az.Network/Set-azVirtualNetworkSubnetConfig). De regel in de netwerkbeveiligingsgroep is geschikt voor alle resources die zijn geïmplementeerd in het subnet.

```powershell
Set-AzVirtualNetworkSubnetConfig `
    -VirtualNetwork $vnet `
    -Name 'mySubnet' `
    -AddressPrefix "192.168.1.0/24" `
    -NetworkSecurityGroup $nsg
```

## <a name="create-a-network-interface-with-accelerated-networking"></a>Maak een netwerkinterface met versneld netwerken
Maak een openbaar IP-adres met [New-AzPublicIpAddress](/powershell/module/az.Network/New-azPublicIpAddress). Een openbaar IP-adres is niet vereist als u niet van plan voor toegang tot de virtuele machine via Internet, maar om de stappen in dit artikel te voltooien, het is vereist.

```powershell
$publicIp = New-AzPublicIpAddress `
    -ResourceGroupName myResourceGroup `
    -Name 'myPublicIp' `
    -location centralus `
    -AllocationMethod Dynamic
```

Maken van een netwerkinterface met [New-AzNetworkInterface](/powershell/module/az.Network/New-azNetworkInterface) met versnelde netwerken die zijn ingeschakeld en het openbare IP-adres toewijzen aan de netwerkinterface. Het volgende voorbeeld wordt een netwerkinterface met de naam *myNic* in de *mySubnet* subnet van de *myVnet* virtueel netwerk en wijst de *myPublicIp*  openbare IP-adres toe:

```powershell
$nic = New-AzNetworkInterface `
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

Eerst definieert u de virtuele machine met [New-AzVMConfig](/powershell/module/az.compute/new-azvmconfig). Het volgende voorbeeld wordt een virtuele machine met de naam gedefinieerd *myVM* met een VM-grootte die ondersteuning biedt voor versnelde netwerken (*Standard_DS4_v2*):

```powershell
$vmConfig = New-AzVMConfig -VMName "myVm" -VMSize "Standard_DS4_v2"
```

Zie voor een lijst van alle VM-grootten en -kenmerken, [Windows VM-grootten](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Maken van de rest van uw VM-configuratie met [Set AzVMOperatingSystem](/powershell/module/az.compute/set-azvmoperatingsystem) en [Set AzVMSourceImage](/powershell/module/az.compute/set-azvmsourceimage). Het volgende voorbeeld wordt een virtuele machine met Windows Server 2016:

```powershell
$vmConfig = Set-AzVMOperatingSystem -VM $vmConfig `
    -Windows `
    -ComputerName "myVM" `
    -Credential $cred `
    -ProvisionVMAgent `
    -EnableAutoUpdate
$vmConfig = Set-AzVMSourceImage -VM $vmConfig `
    -PublisherName "MicrosoftWindowsServer" `
    -Offer "WindowsServer" `
    -Skus "2016-Datacenter" `
    -Version "latest"
```

Koppelen van de netwerkinterface die u eerder hebt gemaakt met [toevoegen AzVMNetworkInterface](/powershell/module/az.compute/add-azvmnetworkinterface):

```powershell
$vmConfig = Add-AzVMNetworkInterface -VM $vmConfig -Id $nic.Id
```

Maak ten slotte uw virtuele machine met [New-AzVM](/powershell/module/az.compute/new-azvm):

```powershell
New-AzVM -VM $vmConfig -ResourceGroupName "myResourceGroup" -Location "centralus"
```

## <a name="confirm-the-driver-is-installed-in-the-operating-system"></a>Controleer of dat het stuurprogramma in het besturingssysteem is geïnstalleerd

Zodra u de virtuele machine in Azure hebt gemaakt, verbinding maken met de virtuele machine en controleer of het stuurprogramma is geïnstalleerd in Windows.

1. Open de Azure in een internetbrowser [portal](https://portal.azure.com) en meld u aan met uw Azure-account.
2. In het vak met de tekst *zoeken naar resources* aan de bovenkant van de Azure-portal, typt u *myVm*. Wanneer **myVm** wordt weergegeven in de lijst met zoekresultaten, klikt u erop. Als **maken** wordt weergegeven onder de **Connect** knop, Azure is nog niet voltooid voor het maken van de virtuele machine. Klik op **Connect** in de linkerbovenhoek van het overzicht alleen nadat u hebt niet langer bekijken **maken** onder de **Connect** knop.
3. Voer de gebruikersnaam en wachtwoord die u hebt ingevoerd in [maken van de virtuele machine](#create-the-virtual-machine). Als u nooit met een Windows-VM in Azure verbonden bent, Zie [verbinding maken met virtuele machine](../virtual-machines/windows/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#connect-to-virtual-machine).
4. Met de rechtermuisknop op de knop Start van Windows en klik op **Apparaatbeheer**. Vouw de **netwerkadapters** knooppunt. Bevestig dat de **Mellanox ConnectX-3 virtuele functie Ethernet-Adapter** wordt weergegeven, zoals wordt weergegeven in de volgende afbeelding:

    ![Apparaatbeheer](./media/create-vm-accelerated-networking/device-manager.png)

Versneld netwerken is nu ingeschakeld voor uw virtuele machine.

## <a name="enable-accelerated-networking-on-existing-vms"></a>Versnelde netwerken op bestaande virtuele machines inschakelen
Als u een virtuele machine zonder versnelde netwerken hebt gemaakt, is het mogelijk is deze functie op een bestaande virtuele machine in te schakelen.  De virtuele machine moet bieden ondersteuning voor versnelde netwerken voldoen aan de volgende vereisten die ook hierboven worden beschreven:

* De virtuele machine moet een ondersteunde grootte voor versnelde netwerken
* De virtuele machine moet een ondersteunde Azure-galerie-installatiekopie (en de kernelversie voor Linux)
* Alle virtuele machines in een beschikbaarheidsset of VMSS moet worden gestopt/de toewijzing ongedaan gemaakt voordat u inschakelt op een NIC versnelde netwerken

### <a name="individual-vms--vms-in-an-availability-set"></a>Afzonderlijke VM's en virtuele machines in een beschikbaarheidsset instellen
Eerst stoppen/toewijzing ongedaan maken de virtuele machine of, als een Beschikbaarheidsset, alle virtuele machines in de Set:

```azurepowershell
Stop-AzVM -ResourceGroup "myResourceGroup" `
    -Name "myVM"
```

Belangrijk Let op: als uw virtuele machine afzonderlijk, is gemaakt zonder een beschikbaarheidsset, u alleen moet stoppen/toewijzing van de afzonderlijke virtuele machine om in te schakelen versnelde netwerken.  Als uw virtuele machine is gemaakt met een beschikbaarheidsset, moet alle virtuele machines die zijn opgenomen in de beschikbaarheidsset worden gestopt/de toewijzing ongedaan gemaakt voordat u inschakelt op een van de NIC's versnelde netwerken. 

Nadat gestopt, schakelt u versnelde netwerken op de NIC van de virtuele machine:

```azurepowershell
$nic = Get-AzNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Name "myNic"

$nic.EnableAcceleratedNetworking = $true

$nic | Set-AzNetworkInterface
```

Opnieuw opstarten van uw virtuele machine of als in een beschikbaarheidsset, alle virtuele machines in de set en Bevestig dat versnelde netwerken is ingeschakeld:

```azurepowershell
Start-AzVM -ResourceGroup "myResourceGroup" `
    -Name "myVM"
```

### <a name="vmss"></a>VMSS
VMSS is iets anders, maar dezelfde werkstroom volgt.  Stop eerst de virtuele machines:

```azurepowershell
Stop-AzVmss -ResourceGroupName "myResourceGroup" `
    -VMScaleSetName "myScaleSet"
```

Nadat de virtuele machines zijn gestopt, moet u de eigenschap versnelde netwerken onder de netwerkinterface bijwerken:

```azurepowershell
$vmss = Get-AzVmss -ResourceGroupName "myResourceGroup" `
    -VMScaleSetName "myScaleSet"

$vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].EnableAcceleratedNetworking = $true

Update-AzVmss -ResourceGroupName "myResourceGroup" `
    -VMScaleSetName "myScaleSet" `
    -VirtualMachineScaleSet $vmss
```

Let op: een VMSS heeft VM-upgrades die met behulp van drie verschillende instellingen, automatische, rolling en handmatige updates toe te passen.  Het beleid is ingesteld op automatisch in deze instructies zodat de VMSS de wijzigingen onmiddellijk na het opnieuw opstarten haalt.  Zodat de wijzigingen direct worden doorgevoerd, moet u deze instellen op automatisch:

```azurepowershell
$vmss.UpgradePolicy.AutomaticOSUpgrade = $true

Update-AzVmss -ResourceGroupName "myResourceGroup" `
    -VMScaleSetName "myScaleSet" `
    -VirtualMachineScaleSet $vmss
```

Ten slotte opnieuw starten de VMSS:

```azurepowershell
Start-AzVmss -ResourceGroupName "myResourceGroup" `
    -VMScaleSetName "myScaleSet"
```

Nadat u opnieuw opstarten, wacht totdat de upgrades te voltooien, maar nadat deze is voltooid, de VF wordt weergegeven in de virtuele machine.  (Zorg ervoor dat u gebruikmaakt van een ondersteund besturingssysteem en de VM-formaat)

### <a name="resizing-existing-vms-with-accelerated-networking"></a>Formaat wijzigen van bestaande VM's met versnelde netwerken

VM's met versnelde netwerken ingeschakeld kunnen alleen worden gewijzigd met VM's die ondersteuning bieden voor versnelde netwerken.  

Een virtuele machine met versnelde netwerken ingeschakeld kan niet worden uitgebreid tot een VM-instantie die geen ondersteuning biedt voor versnelde netwerken met behulp van de bewerking formaat wijzigen.  In plaats daarvan om het formaat van een van deze virtuele machines aan:

* Stoppen/toewijzing ongedaan maken de virtuele machine of als in een beschikbaarheid van set/VMSS, stoppen/toewijzing ongedaan maken alle virtuele machines in de set/VMSS.
* Versneld netwerken moet worden uitgeschakeld op de NIC van de virtuele machine of als in een beschikbaarheid van set/VMSS, alle virtuele machines in de set/VMSS.
* Zodra Accelerated Networking is uitgeschakeld, kan de VM-beschikbaarheid/set/VMSS worden verplaatst naar een nieuwe omvang die biedt geen ondersteuning voor versnelde netwerken en opnieuw gestart.
