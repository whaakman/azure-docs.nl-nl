---
title: Een virtuele machine in Azure maken met behulp van PowerShell | Microsoft Docs
description: Gebruik Azure PowerShell en Azure Resource Manager om eenvoudig een nieuwe virtuele machine met Windows Server te maken.
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 14fe9ca9-e228-4d3b-a5d8-3101e9478f6e
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/21/2016
ms.author: davidmu
translationtype: Human Translation
ms.sourcegitcommit: 94c18aa0c4fe38fb74931d5ed61fece207c8b5ce
ms.openlocfilehash: 701a5515cd1d52f7ca8d3562dabcdf0e4d31183d


---
# <a name="create-a-windows-vm-using-resource-manager-and-powershell"></a>Een virtuele Windows-machine maken met behulp van Resource Manager en PowerShell
Dit artikel laat zien hoe u met behulp van [Resource Manager](../azure-resource-manager/resource-group-overview.md) en PowerShell snel een virtuele machine in Azure maakt met Windows Server, evenals de hiervoor benodigde resources. 

Alle stappen in dit artikel zijn vereist om een virtuele machine te maken. Het uitvoeren van deze stappen kost ongeveer 30 minuten. Vervang voorbeeldparameterwaarden in de opdrachten met namen die logisch zijn voor uw omgeving.

## <a name="step-1-install-azure-powershell"></a>Stap 1: Azure PowerShell installeren
Zie [Azure PowerShell installeren en configureren](/powershell/azureps-cmdlets-docs) voor informatie over het installeren van de nieuwste versie van Azure PowerShell, het selecteren van het abonnement en het aanmelden bij uw account.

## <a name="step-2-create-a-resource-group"></a>Stap 2: Een resourcegroep maken
Alle resources moeten deel uitmaken van een resourcegroep, dus deze maken is de eerste stap.  

1. Haal een lijst op met beschikbare locaties waar resources kunnen worden gemaakt.
   
    ```powershell
    Get-AzureRmLocation | sort Location | Select Location
    ```
2. Stel de locatie in voor de resources. Met deze opdracht wordt de locatie ingesteld als **centralus**.
   
    ```powershell
    $location = "centralus"
    ```
3. Maak een resourcegroep. Met deze opdracht wordt de resourcegroep met de naam **myResourceGroup** gemaakt op de door u ingestelde locatie.
   
    ```powershell
    $myResourceGroup = "myResourceGroup"
    New-AzureRmResourceGroup -Name $myResourceGroup -Location $location
    ```

## <a name="step-3-create-a-storage-account"></a>Stap 3: Een opslagaccount maken
Een [opslagaccount](../storage/storage-introduction.md) is nodig voor het opslaan van de virtuele vaste schijf die wordt gebruikt door de virtuele machine die u maakt. Namen van opslagaccounts moeten tussen 3 en 24 tekens lang zijn en mogen alleen cijfers en kleine letters bevatten.

1. Test of de naam van het opslagaccount uniek is. Met deze opdracht wordt de naam **myStorageAccount** getest.
   
    ```powershell
    $myStorageAccountName = "mystorageaccount"
    Get-AzureRmStorageAccountNameAvailability $myStorageAccountName
    ```
   
    Als deze opdracht **Waar** retourneert, is de voorgestelde naam uniek in Azure. 
2. Maak nu het opslagaccount.
   
    ```powershell    
    $myStorageAccount = New-AzureRmStorageAccount -ResourceGroupName $myResourceGroup `
        -Name $myStorageAccountName -SkuName "Standard_LRS" -Kind "Storage" -Location $location
    ```

## <a name="step-4-create-a-virtual-network"></a>Stap 4: Een virtueel netwerk maken
Alle virtuele machines maken deel uit van een [virtueel netwerk](../virtual-network/virtual-networks-overview.md).

1. Maak een subnet voor het virtuele netwerk. Met deze opdracht wordt een subnet met de naam **mySubnet** gemaakt met een adresvoorvoegsel van 10.0.0.0/24.
   
    ```powershell
    $mySubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnet" -AddressPrefix 10.0.0.0/24
    ```
2. Maak nu het virtuele netwerk. Met deze opdracht wordt een virtueel netwerk met de naam **myVnet** gemaakt met behulp van het door u gemaakte subnet, met het adresvoorvoegsel **10.0.0.0/16**.
   
    ```powershell
    $myVnet = New-AzureRmVirtualNetwork -Name "myVnet" -ResourceGroupName $myResourceGroup `
        -Location $location -AddressPrefix 10.0.0.0/16 -Subnet $mySubnet
    ```

## <a name="step-5-create-a-public-ip-address-and-network-interface"></a>Stap 5: Een openbaar IP-adres en een netwerkinterface maken
Om te kunnen communiceren met de virtuele machine in het virtuele netwerk, hebt u een [openbaar IP-adres](../virtual-network/virtual-network-ip-addresses-overview-arm.md) en een netwerkinterface nodig.

1. Maak het openbare IP-adres. Met deze opdracht wordt een openbaar IP-adres met de naam **myPublicIp** gemaakt met een toewijzingsmethode **Dynamisch**.
   
    ```powershell
    $myPublicIp = New-AzureRmPublicIpAddress -Name "myPublicIp" -ResourceGroupName $myResourceGroup `
        -Location $location -AllocationMethod Dynamic
    ```
2. Maak de netwerkinterface. Met deze opdracht wordt een netwerkinterface met de naam **myNIC** gemaakt.
   
    ```powershell
    $myNIC = New-AzureRmNetworkInterface -Name "myNIC" -ResourceGroupName $myResourceGroup `
        -Location $location -SubnetId $myVnet.Subnets[0].Id -PublicIpAddressId $myPublicIp.Id
    ```

## <a name="step-6-create-a-virtual-machine"></a>Stap 6: Een virtuele machine maken
Nu u alle benodigde onderdelen hebt verzameld, is het tijd om de virtuele machine te maken.

1. Voer deze opdracht uit voor het instellen van de accountnaam en het wachtwoord van de beheerder voor de virtuele machine.

    ```powershell
    $cred = Get-Credential -Message "Type the name and password of the local administrator account."
    ```
   
    Het wachtwoord moet tussen 12 en 123 tekens lang zijn en ten minste één kleine letter, één hoofdletter, één cijfer en één speciaal teken hebben. 
2. Maak een configuratieobject voor de virtuele machine. Met deze opdracht wordt een configuratieobject met de naam **myVmConfig** gemaakt die de naam en de grootte van de virtuele machine definieert.
   
    ```powershell
    $myVm = New-AzureRmVMConfig -VMName "myVM" -VMSize "Standard_DS1_v2"
    ```
   
    Zie [Grootten voor virtuele machines in Azure](virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) voor een lijst met beschikbare grootten voor een virtuele machine.
3. Configureer de besturingssysteeminstellingen voor de virtuele Machine. Met deze opdracht worden de computernaam, het besturingssysteemtype en de accountreferenties ingesteld voor de virtuele machine.
   
    ```powershell
    $myVM = Set-AzureRmVMOperatingSystem -VM $myVM -Windows -ComputerName "myVM" -Credential $cred `
        -ProvisionVMAgent -EnableAutoUpdate
    ```
4. Definieer de installatiekopie die moet worden gebruikt voor de inrichting van de virtuele machine. Met deze opdracht wordt gedefinieerd welke Windows Server-installatiekopie moet worden gebruikt voor de virtuele machine. 
   
    ```powershell
    $myVM = Set-AzureRmVMSourceImage -VM $myVM -PublisherName "MicrosoftWindowsServer" `
        -Offer "WindowsServer" -Skus "2012-R2-Datacenter" -Version "latest"
    ```
   
    Zie [Navigeren door en selecteren van installatiekopieën voor virtuele Windows-machines in Azure met PowerShell of de CLI](virtual-machines-windows-cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) voor meer informatie over het selecteren van de te gebruiken installatiekopieën.
5. Voeg de netwerkinterface die u hebt gemaakt, toe aan de configuratie.
   
    ```powershell
    $myVM = Add-AzureRmVMNetworkInterface -VM $myVM -Id $myNIC.Id
    ```
6. Definieer de naam en de locatie van de vaste schijf van de virtuele machine. Het virtuele-vaste-schijfbestand wordt in een container opgeslagen. Met deze opdracht wordt de schijf in een container met de naam **vhds/myOsDisk1.vhd** gemaakt, in het door u gemaakte opslagaccount.
   
    ```powershell
    $blobPath = "vhds/myOsDisk1.vhd"
    $osDiskUri = $myStorageAccount.PrimaryEndpoints.Blob.ToString() + $blobPath
    ```
7. Voeg informatie over het besturingssysteem van de schijf toe aan de configuratie van de virtuele machine. Vervang de waarde van **$diskName** door de gewenste naam voor de besturingssysteemschijf. Maak de variabele en voeg informatie over de schijf toe aan de configuratie.
   
    ```powershell
    $myVM = Set-AzureRmVMOSDisk -VM $myVM -Name "myOsDisk1" -VhdUri $osDiskUri -CreateOption fromImage
    ```
8. Maak ten slotte de virtuele machine.
   
    ```powershell
    New-AzureRmVM -ResourceGroupName $myResourceGroup -Location $location -VM $myVM
    ```

## <a name="next-steps"></a>Volgende stappen
* Als er problemen zijn met de implementatie raadpleegt u allereerst [Troubleshoot common Azure deployment errors with Azure Resource Manager](../azure-resource-manager/resource-manager-common-deployment-errors.md) (Veelvoorkomende problemen met de implementatie van Azure oplossen met Azure Resource Manager)
* Informatie over het beheren van de virtuele machine die u hebt gemaakt, vindt u in [Virtuele machines beheren met Azure Resource Manager en PowerShell](virtual-machines-windows-ps-manage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Profiteer van het gebruik van een sjabloon voor het maken van een virtuele machine met behulp van de informatie in [Een virtuele Windows-machine maken met een Resource Manager-sjabloon](virtual-machines-windows-ps-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).




<!--HONumber=Jan17_HO2-->


