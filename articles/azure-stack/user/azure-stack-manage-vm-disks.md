---
title: VM-schijven in Azure-Stack beheren | Microsoft Docs
description: Schijven voor virtuele machines inrichten voor Azure-Stack.
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 4e5833cf-4790-4146-82d6-737975fb06ba
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/14/2017
ms.author: brenduns
ms.reviewer: jiahan
ms.openlocfilehash: 0c36e2eaaf2d266842b2b7de0b0c8dc0ed1e0145
ms.sourcegitcommit: 3fca41d1c978d4b9165666bb2a9a1fe2a13aabb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2017
---
# <a name="virtual-machine-disk-storage-for-azure-stack"></a>Virtuele Machine schijfopslag voor Azure-Stack

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Ondersteunt het gebruik van Azure Stack [zonder begeleiding schijven](https://docs.microsoft.com/azure/virtual-machines/windows/about-disks-and-vhds#unmanaged-disks) in een virtuele Machine als de schijf van een besturingssysteem (OS) en een gegevensschijf. Voor het gebruik van niet-beheerde schijven die u maakt een [opslagaccount](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account) en de schijven op te slaan als pagina-blobs in containers in het opslagaccount. Deze schijven worden vervolgens VM-schijven genoemd.

Om de prestaties verbeteren, en verlagen de beheerkosten van het Azure-Stack-systeem, wordt u aangeraden dat u elke schijf VM plaatsen in een afzonderlijke container. Een container moet een besturingssysteemschijf of een gegevensschijf maar niet beide op hetzelfde moment bevatten. Er is echter geen beperking waarmee wordt voorkomen dat beide in dezelfde container te plaatsen.

Als u een of meer gegevensschijven aan een virtuele machine toevoegt, moet u extra containers gebruiken als locatie voor het opslaan van deze schijven. Zoals gegevensschijven, dient de besturingssysteemschijf voor extra virtuele machines zich in hun eigen afzonderlijke containers.

Wanneer u meerdere virtuele machines maakt, kunt u hetzelfde opslagaccount opnieuw gebruiken voor elke nieuwe virtuele machine. Alleen de containers die u maakt moet uniek zijn.  

U schijven toevoegen aan een virtuele machine door de gebruikersportal of PowerShell te gebruiken.

| Methode | Opties
|-|-|
|[Gebruikersportal](#use-the-portal-to-add-additional-disks-to-a-vm)|-Nieuwe gegevensschijven toevoegen aan een virtuele machine die eerder zijn ingericht. Nieuwe schijven worden gemaakt door de Azure-Stack. </br> </br>-Een bestaand VHD-bestand als een schijf toevoegen aan een virtuele machine die eerder zijn ingericht. Hiervoor moet u eerst voorbereiden en het VHD-bestand uploaden naar Azure-Stack. |
|[PowerShell](#use-powershell-to-add-multiple-unmanaged-disks-to-a-vm) | -Maak een nieuwe virtuele machine met een besturingssysteemschijf en op hetzelfde moment voegt u een of meer gegevensschijven aan die VM. |


## <a name="use-the-portal-to-add-additional-disks-to-a-vm"></a>Gebruik de portal extra schijven toevoegen aan een virtuele machine
Standaard wordt alleen een besturingssysteemschijf gemaakt wanneer u een virtuele machine voor de meeste marketplace-items maken met de portal. Schijven die zijn gemaakt door Azure worden beheerde schijven genoemd.

Nadat u een virtuele machine inricht, kunt u de portal een nieuwe gegevensschijf of een bestaande gegevensschijf toevoegen aan die VM. Elke extra schijf moet worden geplaatst in een afzonderlijke container. De schijven die u aan een virtuele machine toevoegt worden niet-beheerde schijven genoemd.

### <a name="use-the-portal-to-attach-a-new-data-disk-to-a-vm"></a>De portal gebruiken voor een nieuwe gegevensschijf koppelen aan een virtuele machine

1.  Klik in de portal op **virtuele machines**.    
    ![Voorbeeld: VM-dashboard](media/azure-stack-manage-vm-disks/vm-dashboard.png)

2.  Selecteer een virtuele machine die eerder zijn ingericht.   
    ![Voorbeeld: Een virtuele machine in het dashboard selecteren](media/azure-stack-manage-vm-disks/select-a-vm.png)

3.  Voor de virtuele machine, klikt u op **schijven** > **Attach nieuwe**.       
    ![Voorbeeld: Een nieuwe schijf koppelen aan de virtuele machine](media/azure-stack-manage-vm-disks/Attach-disks.png)    

4.  In de **nieuwe schijf koppelen** deelvenster, klikt u op **locatie**. De locatie is standaard ingesteld op de container die schijf met het besturingssysteem bevat.      
    ![Voorbeeld: Stel de schijflocatie](media/azure-stack-manage-vm-disks/disk-location.png)

5.  Selecteer de **opslagaccount** te gebruiken. Selecteer vervolgens de **Container** waar u de gegevensschijf wilt. Van de **Containers** pagina kunt u een nieuwe container maken als u wilt. Vervolgens kunt u de locatie voor de nieuwe schijf wijzigen naar een eigen container. Wanneer u een afzonderlijke container voor elke schijf gebruikt, kunt u de plaatsing van de gegevensschijf die u kunt de prestaties verbeteren distribueren. Klik op **Selecteer** de selectie opslaan.     
    ![Voorbeeld: Selecteer een container](media/azure-stack-manage-vm-disks/select-container.png)

6.  In de **nieuwe schijf koppelen** pagina, werkt de **naam**, **Type**, **grootte**, en **Hostcaching** instellingen van de schijf. Klik vervolgens op **OK** om op te slaan van de nieuwe schijfconfiguratie voor de virtuele machine.  
    ![Voorbeeld: Volledige schijf koppelen](media/azure-stack-manage-vm-disks/complete-disk-attach.png)  

7.  Nadat de Azure-Stack de schijf wordt gemaakt en gekoppeld aan de virtuele machine, de nieuwe schijf wordt weergegeven in de instellingen voor de schijf van de virtuele machine onder **GEGEVENSSCHIJVEN**.   
    ![Voorbeeld: Weergave-schijf](media/azure-stack-manage-vm-disks/view-data-disk.png)


### <a name="attach-an-existing-data-disk-to-a-vm"></a>Een bestaande gegevensschijf koppelen aan een virtuele machine
1.  [Voorbereiden van een .vhd-bestand](https://docs.microsoft.com/azure/virtual-machines/windows/classic/createupload-vhd) voor gebruik als gegevensschijf voor een virtuele machine. Die VHD-bestand naar een opslagaccount die u gebruikt met de virtuele machine die u wilt koppelen van het VHD-bestand te uploaden.

  Wilt u een andere container gebruiken voor het opslaan van het VHD-bestand dan de container met de besturingssysteemschijf.   
  ![Voorbeeld: Een VHD-bestand uploaden](media/azure-stack-manage-vm-disks/upload-vhd.png)

2.  Nadat het VHD-bestand is geüpload, bent u klaar om te koppelen van de VHD op een virtuele machine. Klik in het menu aan de linkerkant op **virtuele machines**.  
 ![Voorbeeld: Een virtuele machine in het dashboard selecteren](media/azure-stack-manage-vm-disks/vm-dashboard.png)

3.  Selecteer de virtuele machine in de lijst.    
  ![Voorbeeld: Een virtuele machine in het dashboard selecteren](media/azure-stack-manage-vm-disks/select-a-vm.png)

4.  Klik op de pagina voor de virtuele machine op **schijven** > **Attach bestaande**.   
  ![Voorbeeld: Een bestaande schijf koppelen](media/azure-stack-manage-vm-disks/attach-disks2.png)

5.  In de **bestaande schijf koppelen** pagina, klikt u op **VHD-bestand**. De **opslagaccounts** pagina wordt geopend.    
  ![Voorbeeld: Selecteer een VHD-bestand](media/azure-stack-manage-vm-disks/select-vhd.png)

6.  Onder **opslagaccounts**, selecteer het account te gebruiken en selecteer vervolgens een container met het VHD-bestand dat u eerder hebt geüpload. Selecteer het VHD-bestand en klik vervolgens op **Selecteer** de selectie opslaan.    
  ![Voorbeeld: Selecteer een container](media/azure-stack-manage-vm-disks/select-container2.png)

7.  Onder **bestaande schijf koppelen**, het geselecteerde bestand wordt vermeld onder **VHD-bestand**. Update de **Hostcaching** instelling van de schijf en klik vervolgens op **OK** om op te slaan van de nieuwe schijfconfiguratie voor de virtuele machine.    
  ![Voorbeeld: Koppel het VHD-bestand](media/azure-stack-manage-vm-disks/attach-vhd.png)

8.  Nadat de Azure-Stack de schijf wordt gemaakt en gekoppeld aan de virtuele machine, de nieuwe schijf wordt weergegeven in de instellingen voor de schijf van de virtuele machine onder **gegevensschijven**.   
  ![Voorbeeld: Voltooi de schijf koppelen](media/azure-stack-manage-vm-disks/complete-disk-attach.png)


## <a name="use-powershell-to-add-multiple-unmanaged-disks-to-a-vm"></a>PowerShell gebruiken voor meerdere niet-beheerde schijven toevoegen aan een virtuele machine
U kunt PowerShell gebruiken voor het inrichten van een virtuele machine en een nieuwe gegevensschijf toevoegen of koppelen van een reeds bestaande **.vhd** bestand als een gegevensschijf.

De **toevoegen AzureRmVMDataDisk** cmdlet wordt een gegevensschijf toegevoegd aan een virtuele machine. U kunt een gegevensschijf toevoegen wanneer u een virtuele machine maakt, of u een gegevensschijf aan een bestaande virtuele machine toevoegen kunt. Geef de **VhdUri** parameter voor het distribueren van de schijven zich verschillende containers.

### <a name="add-data-disks-to-a-new-virtual-machine"></a>Gegevensschijven toevoegen aan een nieuwe virtuele machine
De volgende voorbeelden PowerShell-opdrachten gebruiken voor het maken van een virtuele machine met drie gegevensschijven, elk in een andere container hebt geplaatst.

De eerste opdracht wordt een object van de virtuele machine gemaakt en opgeslagen in de *$VirtualMachine* variabele. De opdracht wijst een naam en de grootte toe aan de virtuele machine.
  ```
  $VirtualMachine = New-AzureRmVMConfig -VMName "VirtualMachine" `
                                      -VMSize "Standard_A2"
  ```

De volgende drie opdrachten toewijzen paden van drie gegevensschijven voor de *$DataDiskVhdUri01*, *$DataDiskVhdUri02*, en *$DataDiskVhdUri03* variabelen. Definieer de naam van een ander pad in de URL voor het distribueren van de schijven zich verschillende containers.     
  ```
  $DataDiskVhdUri01 = "https://contoso.blob.local.azurestack.external/test1/data1.vhd"
  ```

  ```
  $DataDiskVhdUri02 = "https://contoso.blob.local.azurestack.external/test2/data2.vhd"
  ```

  ```
  $DataDiskVhdUri03 = "https://contoso.blob.local.azurestack.external/test3/data3.vhd"
  ```

De laatste drie opdrachten gegevensschijven toevoegen aan de virtuele machine opgeslagen in *$VirtualMachine*. Elke opdracht geeft de naam, locatie en aanvullende eigenschappen van de schijf. De URI van elke schijf zijn opgeslagen in *$DataDiskVhdUri01*, *$DataDiskVhdUri02*, en *$DataDiskVhdUri03*.
  ```
  $VirtualMachine = Add-AzureRmVMDataDisk -VM $VirtualMachine -Name 'DataDisk1' `
                  -Caching 'ReadOnly' -DiskSizeInGB 10 -Lun 0 `
                  -VhdUri $DataDiskVhdUri01 -CreateOption Empty
  ```

  ```
  $VirtualMachine = Add-AzureRmVMDataDisk -VM $VirtualMachine -Name 'DataDisk2' `
                 -Caching 'ReadOnly' -DiskSizeInGB 11 -Lun 1 `
                 -VhdUri $DataDiskVhdUri02 -CreateOption Empty
  ```

  ```
  $VirtualMachine = Add-AzureRmVMDataDisk -VM $VirtualMachine -Name 'DataDisk3' `
                  -Caching 'ReadOnly' -DiskSizeInGB 12 -Lun 2 `
                  -VhdUri $DataDiskVhdUri03 -CreateOption Empty
  ```

Gebruik de volgende PowerShell-opdrachten de schijf- en configuratie van het besturingssysteem toevoegen aan de virtuele machine en de nieuwe virtuele machine start.
  ```
  #set variables
  $rgName = "myResourceGroup"
  $location = "local"
  #Set OS Disk
  $osDiskUri = "https://contoso.blob.local.azurestack.external/vhds/osDisk.vhd"
  $osDiskName = "osDisk"

  $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $osDiskName -VhdUri $osDiskUri `
      -CreateOption FromImage -Windows

  # Create a subnet configuration
  $subnetName = "mySubNet"
  $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24

  # Create a vnet configuration
  $vnetName = "myVnetName"
  $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
      -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet

  # Create a public IP
  $ipName = "myIP"
  $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
      -AllocationMethod Dynamic

  # Create a network security group cnfiguration
  $nsgName = "myNsg"
  $rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
      -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
      -SourceAddressPrefix Internet -SourcePortRange * `
      -DestinationAddressPrefix * -DestinationPortRange 3389
  $nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
      -Name $nsgName -SecurityRules $rdpRule

  # Create a NIC configuration
  $nicName = "myNicName"
  $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName `
  -Location $location -SubnetId $vnet.Subnets[0].Id -NetworkSecurityGroupId $nsg.Id -PublicIpAddressId $pip.Id

  #Create the new VM
  $VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName VirtualMachine | `
      Set-AzureRmVMSourceImage -PublisherName MicrosoftWindowsServer -Offer WindowsServer `
      -Skus 2016-Datacenter -Version latest | Add-AzureRmVMNetworkInterface -Id $nic.Id
  New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $VirtualMachine
  ```



### <a name="add-data-disks-to-an-existing-virtual-machine"></a>Gegevensschijven toevoegen aan een bestaande virtuele machine
De volgende voorbeelden PowerShell-opdrachten gebruiken drie gegevensschijven toevoegen aan een bestaande virtuele machine.
De eerste opdracht wordt de virtuele machine met de naam virtuele machine met behulp van de **Get-AzureRmVM** cmdlet. De opdracht slaat de virtuele machine in de *$VirtualMachine* variabele.
  ```
  $VirtualMachine = Get-AzureRmVM -ResourceGroupName "myResourceGroup" `
                                  -Name "VirtualMachine"
  ```
De volgende drie opdrachten toewijzen paden van drie gegevensschijven aan de variabelen DataDiskVhdUri01 $ $DataDiskVhdUri02 en $DataDiskVhdUri03.  De namen van ander pad in de vhduri geven aan verschillende containers voor de plaatsing van de schijf.
  ```
  $DataDiskVhdUri01 = "https://contoso.blob.local.azurestack.external/test1/data1.vhd"
  ```
  ```
  $DataDiskVhdUri02 = "https://contoso.blob.local.azurestack.external/test2/data2.vhd"
  ```
  ```
  $DataDiskVhdUri03 = "https://contoso.blob.local.azurestack.external/test3/data3.vhd"
  ```


  De volgende drie opdrachten de gegevensschijven toevoegen aan de virtuele machine opgeslagen in de *$VirtualMachine* variabele. Elke opdracht geeft de naam, locatie en aanvullende eigenschappen van de schijf. De URI van elke schijf zijn opgeslagen in *$DataDiskVhdUri01*, *$DataDiskVhdUri02*, en *$DataDiskVhdUri03*.
  ```
  Add-AzureRmVMDataDisk -VM $VirtualMachine -Name "disk1" `
                        -VhdUri $DataDiskVhdUri01 -LUN 0 `
                        -Caching ReadOnly -DiskSizeinGB 10 -CreateOption Empty
  ```
  ```
  Add-AzureRmVMDataDisk -VM $VirtualMachine -Name "disk2" `
                        -VhdUri $DataDiskVhdUri02 -LUN 1 `
                        -Caching ReadOnly -DiskSizeinGB 11 -CreateOption Empty
  ```
  ```
  Add-AzureRmVMDataDisk -VM $VirtualMachine -Name "disk3" `
                        -VhdUri $DataDiskVhdUri03 -LUN 2 `
                        -Caching ReadOnly -DiskSizeinGB 12 -CreateOption Empty
  ```


  De laatste opdracht werkt u de status van de virtuele machine opgeslagen in *$VirtualMachine* in -*ResourceGroupName*.
  ```
  Update-AzureRmVM -ResourceGroupName "myResourceGroup" -VM $VirtualMachine
  ```
<!-- Pending scripts  

## Distribute the data disks of an existing VM
If you have a VM with more than one disk in the same container, the service operator of the Azure Stack deployment might ask you to redistribute the disks into individual containers.

To do so, use the scripts from the following location in GitHub. These scripts can be used to move the data disks to different containers.
-->

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over virtuele machines van Azure-Stack, [overwegingen voor virtuele Machines in Azure Stack](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-vm-considerations).
