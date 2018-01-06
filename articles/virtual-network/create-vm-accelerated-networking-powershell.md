---
title: Maken van een virtuele machine van Azure met versnelde toegang | Microsoft Docs
description: Informatie over het maken van een virtuele Linux-machine met versnelde netwerken.
services: virtual-network
documentationcenter: 
author: jdial
manager: jeconnoc
editor: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 01/04/2018
ms.author: jimdial
ms.openlocfilehash: f4908963e0650be9b12b745f6868a1ba6ad933e4
ms.sourcegitcommit: d6984ef8cc057423ff81efb4645af9d0b902f843
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="create-a-windows-virtual-machine-with-accelerated-networking"></a>Een Windows-machine maken met versnelde toegang

> [!IMPORTANT] 
> Virtuele machines moeten worden gemaakt met de versnelde netwerken ingeschakeld. Deze functie kan niet worden ingeschakeld op de bestaande virtuele machines. U kunt de volgende stappen om in te schakelen versnelde netwerken
>   1. Verwijder de virtuele machine
>   2. Maak de virtuele machine met versnelde netwerken is ingeschakeld
>

In deze zelfstudie leert u het maken van virtuele Windows-machine (VM) met versnelde netwerken. Versnelde netwerken kan één i/o-virtualisatie hoofdmap (SR-IOV) voor een virtuele machine, aanzienlijk verbeteren de prestaties van netwerken. Dit pad hoge prestaties wordt de host van het gegevenspad, waardoor latentie en jitter CPU-gebruik, voor gebruik met de zwaarste netwerkbelasting op ondersteunde VM typen overgeslagen. De volgende afbeelding ziet communicatie tussen twee virtuele machines met en zonder versnelde netwerken:

![Vergelijking](./media/create-vm-accelerated-networking/accelerated-networking.png)

Alle netwerkverkeer naar en vanuit de virtuele machine moet zonder versnelde netwerken passeren de host en de virtuele switch. De virtuele switch biedt alle afdwingen van beleid, zoals netwerkbeveiligingsgroepen, access control list, isolatie- en andere gevirtualiseerd netwerkservices op netwerkverkeer. Lees voor meer informatie over virtuele switches, de [Hyper-V-netwerkvirtualisatie en virtuele switch](https://technet.microsoft.com/library/jj945275.aspx) artikel.

Met versnelde netwerken netwerkverkeer binnenkomt bij de VM-netwerkadapter (NIC) en wordt vervolgens doorgestuurd naar de virtuele machine. Alle netwerkbeleid dat de virtuele switch is van toepassing zijn nu offloaded en toegepast in de hardware. Toepassen van beleid in de hardware, kunt de NIC forward netwerkverkeer rechtstreeks naar de virtuele machine, het omzeilen van de host en de virtuele switch, terwijl alle het beleid dat wordt toegepast op de host.

De voordelen van versnelde netwerken zijn alleen van toepassing op de virtuele machine die is ingeschakeld. Voor de beste resultaten is het ideaal voor het inschakelen van deze functie op ten minste twee virtuele machines die zijn verbonden met de dezelfde Azure Virtual Network (VNet). Tijdens de communicatie tussen VNets of verbindende on-premises, heeft dit onderdeel minimale gevolgen voor de algehele latentie.

## <a name="benefits"></a>Voordelen
* **Lagere latentie / hoger pakketten per seconde (pps):** verwijderen van de virtuele switch uit het gegevenspad verwijdert u de tijd te besteden aan de pakketten in de host voor de verwerking van beleid en verhoogt het aantal pakketten dat kan worden verwerkt in de virtuele machine.
* **Minder jitter:** virtuele switch verwerking is afhankelijk van de hoeveelheid beleid die moet worden toegepast en de werkbelasting van de CPU die bezig is met de verwerking. Offloading van het afdwingen van beleid op de hardware verwijdert die variabiliteit door het leveren van pakketten rechtstreeks naar de virtuele machine, het verwijderen van de host voor VM-communicatie en alle software-interrupts en context switches.
* **CPU-gebruik verlaagd:** overslaan van de virtuele switch op de host leidt tot minder CPU-gebruik voor het verwerken van netwerkverkeer.

## <a name="supported-operating-systems"></a>Ondersteunde besturingssystemen
Microsoft Windows Server 2012 R2 Datacenter en WindowsServer 2016.

## <a name="supported-vm-instances"></a>Ondersteunde VM-exemplaren
Versnelde netwerken wordt ondersteund op de meest algemene doeleinden en geoptimaliseerd voor compute exemplaar groottes met 4 of meer vcpu's. Op exemplaren zoals D/DSv3 of E/ESv3 die ondersteuning bieden voor hyperthreading wordt versnelde toegang ondersteund op VM-instanties met 8 vcpu's. Ondersteunde reeksen zijn: D/DSv2, D/DSv3 E/ESv3, Fs-F/Fsv2 en Ms-en Mms.

Zie voor meer informatie over VM-instanties [Windows VM-grootten](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="regions"></a>Regio's
Beschikbaar in alle openbare Azure-regio's en Azure Government Cloud. 

## <a name="limitations"></a>Beperkingen
De volgende beperkingen bestaan wanneer deze wordt met deze mogelijkheid:

* **Interface maken van een netwerk:** Accelerated netwerken kan alleen worden ingeschakeld voor een nieuwe NIC. Deze kan niet worden ingeschakeld voor een bestaande NIC.
* **Maken van VM:** een NIC met versnelde netwerken ingeschakeld kan alleen worden gekoppeld aan een VM wanneer de virtuele machine wordt gemaakt. De NIC kan niet worden gekoppeld aan een bestaande virtuele machine. Als de virtuele machine toe te voegen aan de bestaande beschikbaarheidsset is ingesteld, moeten alle virtuele machines in de beschikbaarheidsset ook versnelde netwerken ingeschakeld.
* **Implementatie via Azure Resource Manager alleen:** virtuele machines (klassiek) kan niet worden geïmplementeerd met versnelde toegang.

## <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

Installeer [Azure PowerShell](/powershell/azure/install-azurerm-ps) versie 5.1.1 of hoger. Ga voor de momenteel geïnstalleerde versie uitvoeren `Get-Module -ListAvailable AzureRM`. Als u wilt installeren of upgraden, installeert u de nieuwste versie van de module AzureRM op basis van de [PowerShell Gallery](https://www.powershellgallery.com/packages/AzureRM). In een PowerShell-sessie, meld u aan bij uw Azure-account met behulp [Add-AzureRmAccount](/powershell/module/AzureRM.Profile/Add-AzureRmAccount).

In de volgende voorbeelden kunt u de parameternamen voorbeeld vervangen door uw eigen waarden. Voorbeeld parameternamen opgenomen *myResourceGroup*, *myNic*, en *myVM*.

Maak een resourcegroep met [New-AzureRmResourceGroup](/powershell/module/AzureRM.Resources/New-AzureRmResourceGroup). Het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* in de *centralus* locatie:

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
Maken van een openbaar IP-adres met [nieuw AzureRmPublicIpAddress](/powershell/module/AzureRM.Network/New-AzureRmPublicIpAddress). Een openbaar IP-adres is niet vereist als u niet van plan bent voor toegang tot de virtuele machine vanaf het Internet, maar de stappen in dit artikel uit te voeren, dit is vereist.

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
