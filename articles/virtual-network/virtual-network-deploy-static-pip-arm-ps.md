---
title: Een virtuele machine maken met een statisch openbaar IP-adres - Azure PowerShell | Microsoft Docs
description: Informatie over het maken van een virtuele machine met een statisch openbaar IP-adres met behulp van PowerShell.
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: ad975ab9-d69f-45c1-9e45-0d3f0f51e87e
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 68656db0b76a29e7ab36fd6fa9ad4647712233ee
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38696580"
---
# <a name="create-a-vm-with-a-static-public-ip-address-using-powershell"></a>Een virtuele machine maken met een statisch openbaar IP-adres met behulp van PowerShell

> [!div class="op_single_selector"]
> * [Azure Portal](virtual-network-deploy-static-pip-arm-portal.md)
> * [PowerShell](virtual-network-deploy-static-pip-arm-ps.md)
> * [Azure-CLI](virtual-network-deploy-static-pip-arm-cli.md)
> * [PowerShell (klassiek)](virtual-networks-reserved-public-ip.md)

[!INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

> [!NOTE]
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en het klassieke model](../resource-manager-deployment-model.md). In dit artikel bevat informatie over het Resource Manager-implementatiemodel, dat door Microsoft wordt aanbevolen voor de meeste nieuwe implementaties in plaats van het klassieke implementatiemodel.

[!INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="start-your-script"></a>Start uw script
U kunt het volledige PowerShell-script gebruikt downloaden [hier](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/virtual-network-deploy-static-pip-arm-ps.ps1). Volg de stappen hieronder om het script te laten werken in uw omgeving wijzigen.

Wijzig de waarden van de variabelen op basis van de waarden die u wilt gebruiken voor uw implementatie. De volgende waarden worden toegewezen aan het scenario in dit artikel gebruikt:

```powershell
# Set variables resource group
$rgName                = "IaaSStory"
$location              = "West US"

# Set variables for VNet
$vnetName              = "WTestVNet"
$vnetPrefix            = "192.168.0.0/16"
$subnetName            = "FrontEnd"
$subnetPrefix          = "192.168.1.0/24"

# Set variables for storage
$stdStorageAccountName = "iaasstorystorage"

# Set variables for VM
$vmSize                = "Standard_A1"
$diskSize              = 127
$publisher             = "MicrosoftWindowsServer"
$offer                 = "WindowsServer"
$sku                   = "2012-R2-Datacenter"
$version               = "latest"
$vmName                = "WEB1"
$osDiskName            = "osdisk"
$nicName               = "NICWEB1"
$privateIPAddress      = "192.168.1.101"
$pipName               = "PIPWEB1"
$dnsName               = "iaasstoryws1"
```

## <a name="create-the-necessary-resources-for-your-vm"></a>De benodigde resources voor uw virtuele machine maken
Voordat u een virtuele machine maakt, moet u een resourcegroep, VNet openbare IP-adres en NIC moet worden gebruikt door de virtuele machine.

1. Maak een nieuwe resourcegroep.

    ```powershell
    New-AzureRmResourceGroup -Name $rgName -Location $location
    ```

2. De VNet en subnet maken.

    ```powershell
    $vnet = New-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName `
        -AddressPrefix $vnetPrefix -Location $location

    Add-AzureRmVirtualNetworkSubnetConfig -Name $subnetName `
        -VirtualNetwork $vnet -AddressPrefix $subnetPrefix

    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```

3. Het openbare IP-resource maakt. 

    ```powershell
    $pip = New-AzureRmPublicIpAddress -Name $pipName -ResourceGroupName $rgName `
        -AllocationMethod Static -DomainNameLabel $dnsName -Location $location
    ```

4. De netwerkinterface (NIC) voor de virtuele machine in het hierboven gemaakte met het openbare IP-subnet maken. U ziet de eerste cmdlet bij het ophalen van het VNet van Azure, dit is nodig omdat een `Set-AzureRmVirtualNetwork` is uitgevoerd als u wilt wijzigen van het bestaande VNet.

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName
    $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name $subnetName
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName `
        -Subnet $subnet -Location $location -PrivateIpAddress $privateIPAddress `
        -PublicIpAddress $pip
    ```

5. Maak een opslagaccount voor het hosten van de besturingssysteemschijf voor virtuele machine.

    ```powershell
    $stdStorageAccount = New-AzureRmStorageAccount -Name $stdStorageAccountName `
    -ResourceGroupName $rgName -Type Standard_LRS -Location $location
    ```

## <a name="create-the-vm"></a>De virtuele machine maken
Nu dat alle benodigde resources gemaakt zijn, kunt u een nieuwe virtuele machine kunt maken.

1. Maak een configuratieobject voor de virtuele machine.

    ```powershell
    $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize
    ```

2. Referenties ophalen voor het lokale beheerdersaccount van de virtuele machine.

    ```powershell
    $cred = Get-Credential -Message "Type the name and password for the local administrator account."
    ```

3. Maak een VM-configuratieobject.

    ```powershell
    $vmConfig = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $vmName `
        -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    ```

4. De installatiekopie van het besturingssysteem voor de virtuele machine instellen.

    ```powershell
    $vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -PublisherName $publisher `
        -Offer $offer -Skus $sku -Version $version
    ```

5. Configureer de OS-schijf.

    ```powershell
    $osVhdUri = $stdStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $osDiskName + ".vhd"
    $vmConfig = Set-AzureRmVMOSDisk -VM $vmConfig -Name $osDiskName -VhdUri $osVhdUri -CreateOption fromImage
    ```

6. De NIC toevoegen aan de virtuele machine.

    ```powershell
    $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id -Primary
    ```

7. De virtuele machine maken.

    ```powershell
    New-AzureRmVM -VM $vmConfig -ResourceGroupName $rgName -Location $location
    ```

8. Sla het scriptbestand.

## <a name="run-the-script"></a>Het script uitvoeren

Nadat u de benodigde wijzigingen, moet u het vorige script uitvoeren. De virtuele machine is gemaakt na een paar minuten.

## <a name="set-ip-addresses-within-the-operating-system"></a>IP-adressen binnen het besturingssysteem instellen

U moet het openbare IP-adres toegewezen aan een virtuele machine van Azure binnen het besturingssysteem van de virtuele machine nooit handmatig toewijzen. Het verdient aanbeveling dat u geen statisch het privé IP-adres toegewezen aan de virtuele machine van Azure binnen het besturingssysteem van een virtuele machine, toewijst tenzij die nodig zijn, bijvoorbeeld wanneer [meerdere IP-adressen toewijzen aan een Windows-VM](virtual-network-multiple-ip-addresses-powershell.md). Als u het particuliere IP-adres binnen het besturingssysteem handmatig instellen wilt, controleert u of deze hetzelfde adres als de privé IP-adres toegewezen aan de Azure [netwerkinterface](virtual-network-network-interface-addresses.md#change-ip-address-settings), of het verlies van connectiviteit met de virtuele machine. Meer informatie over [privé IP-adres](virtual-network-network-interface-addresses.md#private) instellingen.

## <a name="next-steps"></a>Volgende stappen

Al het netwerkverkeer kan stromen naar en van de virtuele machine die in dit artikel hebt gemaakt. U kunt inkomende en uitgaande beveiligingsregels binnen een netwerkbeveiligingsgroep die het verkeer dat naar en van de netwerkinterface, het subnet of beide stromen kan beperken definiëren. Zie voor meer informatie over netwerkbeveiligingsgroepen, [overzicht van netwerkbeveiligingsgroepen](security-overview.md).