---
title: Configureren van privé IP-adressen voor virtuele machines - Azure PowerShell | Microsoft Docs
description: Informatie over het configureren van privé IP-adressen voor virtuele machines met behulp van PowerShell.
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: d5f18929-15e3-40a2-9ee3-8188bc248ed8
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/23/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b0e8153f1d0cecd4efe66dc7cce64addd6ed62aa
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-using-powershell"></a>Configureer persoonlijke IP-adressen voor een virtuele machine met behulp van PowerShell

[!INCLUDE [virtual-networks-static-private-ip-selectors-arm-include](../../includes/virtual-networks-static-private-ip-selectors-arm-include.md)]

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

Azure heeft twee implementatiemodellen: Azure Resource Manager en klassiek. Microsoft raadt aan resources te maken via het Resource Manager-implementatiemodel. Lees het artikel [Azure-implementatiemodellen begrijpen](../azure-resource-manager/resource-manager-deployment-model.md) voor meer informatie over de verschillen tussen de twee modellen. Dit artikel is van toepassing op het Resource Manager-implementatiemodel. U kunt ook [statisch privé IP-adres in het klassieke implementatiemodel beheren](virtual-networks-static-private-ip-classic-ps.md).

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Het voorbeeld PowerShell onderstaande opdrachten een eenvoudige omgeving al gemaakt verwacht op basis van de bovenstaande scenario. Als u wilt de opdrachten uitvoeren zoals ze worden weergegeven in dit document, eerst de testomgeving wordt beschreven in bouwen [een virtueel netwerk maken](quick-create-powershell.md).

## <a name="create-a-vm-with-a-static-private-ip-address"></a>Een virtuele machine met een statisch privé-IP-adres maken
Maken van een virtuele machine met de naam *DNS01* in de *FrontEnd* subnet van een VNet met de naam *TestVNet* met een statisch privé IP-adres van *192.168.1.101*, voer de volgende stappen uit:

1. Stel de variabelen voor de storage-account, locatie, resourcegroep en referenties moet worden gebruikt. U moet een gebruikersnaam en wachtwoord invoeren voor de virtuele machine. De storage-account en resource group moet al bestaan.

    ```powershell
    $stName  = "vnetstorage"
    $locName = "Central US"
    $rgName  = "TestRG"
    $cred    = Get-Credential -Message "Type the name and password of the local administrator account."
    ```

2. Ophalen van het virtuele netwerk en subnet dat u wilt maken van de virtuele machine in.

    ```powershell
    $vnet   = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
    $subnet = $vnet.Subnets[0].Id
    ```

3. Maak indien nodig een openbaar IP-adres voor toegang tot de virtuele machine via het Internet.

    ```powershell
    $pip = New-AzureRmPublicIpAddress -Name TestPIP -ResourceGroupName $rgName `
    -Location $locName -AllocationMethod Dynamic
    ```

4. Maak een NIC met behulp van het statische privé IP-adres dat u wilt toewijzen aan de virtuele machine. Zorg ervoor dat het IP-adres uit het subnetbereik dat u de virtuele machine wilt toevoegen. Dit is de belangrijkste stap voor dit artikel, waarin het instellen van het particuliere IP-adres naar statisch.

    ```powershell
    $nic = New-AzureRmNetworkInterface -Name TestNIC -ResourceGroupName $rgName `
    -Location $locName -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id `
    -PrivateIpAddress 192.168.1.101
    ```

5. De virtuele machine maken met de NIC:

    ```powershell
    $vm = New-AzureRmVMConfig -VMName DNS01 -VMSize "Standard_A1"
    $vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName DNS01 `
    -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm = Set-AzureRmVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
    $osDiskUri = $storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/WindowsVMosDisk.vhd"
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name "windowsvmosdisk" -VhdUri $osDiskUri `
    -CreateOption fromImage
    New-AzureRmVM -ResourceGroupName $rgName -Location $locName -VM $vm 
    ```

Het raadzaam dat u geen statisch het privé IP-adres toegewezen aan de virtuele machine van Azure binnen het besturingssysteem van een VM, toewijst tenzij nodig, zoals wanneer [meerdere IP-adressen toewijzen aan een Windows-VM](virtual-network-multiple-ip-addresses-powershell.md). Als u de privé IP-adres in het besturingssysteem handmatig instelt, moet u zorgen dat het hetzelfde adres als de privé IP-adres is toegewezen aan de Azure [netwerkinterface](virtual-network-network-interface-addresses.md#change-ip-address-settings), of u verbinding met de virtuele machine kunt verliezen. Meer informatie over [privé IP-adres](virtual-network-network-interface-addresses.md#private) instellingen. U moet het openbare IP-adres is toegewezen aan een virtuele machine van Azure in het besturingssysteem van de virtuele machine nooit handmatig toewijzen.

## <a name="retrieve-static-private-ip-address-information-for-a-network-interface"></a>Statische privé IP-adresgegevens voor een netwerkinterface ophalen
Voer de volgende PowerShell-opdracht om weer te geven het statische privé IP-adresgegevens voor de virtuele machine met het bovenstaande script gemaakt, en houd rekening met de waarden voor *PrivateIpAddress* en *PrivateIpAllocationMethod*:

```powershell
Get-AzureRmNetworkInterface -Name TestNIC -ResourceGroupName TestRG
```

Verwachte uitvoer:

    Name                 : TestNIC
    ResourceGroupName    : TestRG
    Location             : centralus
    Id                   : /subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC
    Etag                 : W/"[Id]"
    ProvisioningState    : Succeeded
    Tags                 : 
    VirtualMachine       : {
                             "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/DNS01"
                           }
    IpConfigurations     : [
                             {
                               "Name": "ipconfig1",
                               "Etag": "W/\"[Id]\"",
                               "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC/ipConfigurations/ipconfig1",
                               "PrivateIpAddress": "192.168.1.101",
                               "PrivateIpAllocationMethod": "Static",
                               "Subnet": {
                                 "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd"
                               },
                               "PublicIpAddress": {
                                 "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestPIP"
                               },
                               "LoadBalancerBackendAddressPools": [],
                               "LoadBalancerInboundNatRules": [],
                               "ProvisioningState": "Succeeded"
                             }
                           ]
    DnsSettings          : {
                             "DnsServers": [],
                             "AppliedDnsServers": [],
                             "InternalDnsNameLabel": null,
                             "InternalFqdn": null
                           }
    EnableIPForwarding   : False
    NetworkSecurityGroup : null
    Primary              : True

## <a name="remove-a-static-private-ip-address-from-a-network-interface"></a>Een statisch privé IP-adres verwijderen uit een netwerkinterface
Verwijderen van het statische privé IP-adres toegevoegd aan de virtuele machine in het bovenstaande script Voer de volgende PowerShell-opdrachten:

```powershell
$nic=Get-AzureRmNetworkInterface -Name TestNIC -ResourceGroupName TestRG
$nic.IpConfigurations[0].PrivateIpAllocationMethod = "Dynamic"
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

Verwachte uitvoer:

    Name                 : TestNIC
    ResourceGroupName    : TestRG
    Location             : centralus
    Id                   : /subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC
    Etag                 : W/"[Id]"
    ProvisioningState    : Succeeded
    Tags                 : 
    VirtualMachine       : {
                             "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/WindowsVM"
                           }
    IpConfigurations     : [
                             {
                               "Name": "ipconfig1",
                               "Etag": "W/\"[Id]\"",
                               "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC/ipConfigurations/ipconfig1",
                               "PrivateIpAddress": "192.168.1.101",
                               "PrivateIpAllocationMethod": "Dynamic",
                               "Subnet": {
                                 "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd"
                               },
                               "PublicIpAddress": {
                                 "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestPIP"
                               },
                               "LoadBalancerBackendAddressPools": [],
                               "LoadBalancerInboundNatRules": [],
                               "ProvisioningState": "Succeeded"
                             }
                           ]
    DnsSettings          : {
                             "DnsServers": [],
                             "AppliedDnsServers": [],
                             "InternalDnsNameLabel": null,
                             "InternalFqdn": null
                           }
    EnableIPForwarding   : False
    NetworkSecurityGroup : null
    Primary              : True

## <a name="add-a-static-private-ip-address-to-a-network-interface"></a>Een statisch privé IP-adres toevoegen aan een netwerkinterface
Als u wilt een statisch privé IP-adres toevoegen aan de virtuele machine gemaakt met behulp van het bovenstaande script, voer de volgende opdrachten:

```powershell
$nic=Get-AzureRmNetworkInterface -Name TestNIC -ResourceGroupName TestRG
$nic.IpConfigurations[0].PrivateIpAllocationMethod = "Static"
$nic.IpConfigurations[0].PrivateIpAddress = "192.168.1.101"
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

Het raadzaam dat u geen statisch het privé IP-adres toegewezen aan de virtuele machine van Azure binnen het besturingssysteem van een VM, toewijst tenzij nodig, zoals wanneer [meerdere IP-adressen toewijzen aan een Windows-VM](virtual-network-multiple-ip-addresses-powershell.md). Als u de privé IP-adres in het besturingssysteem handmatig instelt, moet u zorgen dat het hetzelfde adres als de privé IP-adres is toegewezen aan de Azure [netwerkinterface](virtual-network-network-interface-addresses.md#change-ip-address-settings), of u verbinding met de virtuele machine kunt verliezen. Meer informatie over [privé IP-adres](virtual-network-network-interface-addresses.md#private) instellingen. U moet het openbare IP-adres is toegewezen aan een virtuele machine van Azure in het besturingssysteem van de virtuele machine nooit handmatig toewijzen.

## <a name="change-the-allocation-method-for-a-private-ip-address-assigned-to-a-network-interface"></a>De toewijzingsmethode voor een particuliere IP-adres is toegewezen aan een netwerkinterface wijzigen

Een persoonlijke IP-adres is toegewezen aan een NIC met de statische of dynamische toewijzingsmethode. Dynamische IP-adressen kunnen wijzigen na het starten van een virtuele machine die eerder gestopt (toewijzing ongedaan gemaakt is). Dit kan problemen veroorzaken als de virtuele machine host fungeert voor een service die hetzelfde IP-adres zelfs na opnieuw opstarten van gestopt (toewijzing ongedaan gemaakt vereist). Statische IP-adressen worden bewaard totdat de virtuele machine wordt verwijderd. Als u wilt wijzigen van de toewijzingsmethode van een IP-adres, voer het volgende script, waardoor de toewijzingsmethode van dynamisch in statisch is gewijzigd. Als de toewijzingsmethode voor de huidige privé IP-adres statisch is, wijzigt u *statische* naar *dynamische* voordat het script wordt uitgevoerd.

```powershell
$RG = "TestRG"
$NIC_name = "testnic1"

$nic = Get-AzureRmNetworkInterface -ResourceGroupName $RG -Name $NIC_name
$nic.IpConfigurations[0].PrivateIpAllocationMethod = 'Static'
Set-AzureRmNetworkInterface -NetworkInterface $nic 
$IP = $nic.IpConfigurations[0].PrivateIpAddress

Write-Host "The allocation method is now set to"$nic.IpConfigurations[0].PrivateIpAllocationMethod"for the IP address" $IP"." -NoNewline
```

Als u de naam van de NIC niet weet, kunt u een lijst met NIC's binnen een resourcegroep weergeven met de volgende opdracht:

```powershell
Get-AzureRmNetworkInterface -ResourceGroupName $RG | Where-Object {$_.ProvisioningState -eq 'Succeeded'} 
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het beheren van [IP-adresinstellingen](virtual-network-network-interface-addresses.md).