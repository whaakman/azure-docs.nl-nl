---
title: Beheren van VM-schijven in Azure Stack | Microsoft Docs
description: Schijven voor virtuele machines in Azure Stack inricht.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 4e5833cf-4790-4146-82d6-737975fb06ba
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/05/2018
ms.author: mabrigg
ms.reviewer: jiahan
ms.openlocfilehash: bdf31c72fbcd8941161e6b9df0a490df7f6a16e0
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/06/2018
ms.locfileid: "44026515"
---
# <a name="provision-virtual-machine-disk-storage-in-azure-stack"></a>Inrichten van virtuele machine disk-opslag in Azure Stack

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Dit artikel wordt beschreven hoe u om in te richten schijfopslag voor virtuele machine met behulp van de Azure Stack-portal of met behulp van PowerShell.

## <a name="overview"></a>Overzicht

Vanaf versie 1808, Azure Stack biedt ondersteuning voor het gebruik van beheerde schijven en niet-beheerde schijven op virtuele machines, als zowel een besturingssysteem (OS) en een gegevensschijf. Voorafgaand aan versie 1808, worden alleen niet-beheerde schijven ondersteund. 

**[Beheerde schijven](https://docs.microsoft.com/azure/virtual-machines/windows/about-disks-and-vhds#managed-disks)**  Schijfbeheer voor Azure IaaS VM's vereenvoudigen door het beheer van de storage-accounts die zijn gekoppeld aan de VM-schijven. U hoeft alleen te geven van de grootte van de schijf die u nodig hebt en Azure Stack maakt en beheert de schijf voor u.

**[Niet-beheerde schijven](https://docs.microsoft.com/azure/virtual-machines/windows/about-disks-and-vhds#unmanaged-disks)**, vereist dat u maakt een [opslagaccount](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account) voor het opslaan van de schijven. De schijven die u hebt gemaakt, worden aangeduid als VM-schijven en worden opgeslagen in containers in het opslagaccount.

 

### <a name="best-practice-guidelines"></a>Richtlijnen voor aanbevolen procedures

Als u wilt de prestaties verbeteren en de totale kosten te verlagen, wordt u aangeraden dat u elke VM-schijf in een afzonderlijke container plaatsen. Een container moet bevatten een besturingssysteemschijf of een gegevensschijf, maar niet beide op hetzelfde moment. (Er is echter niets om te voorkomen dat u beide soorten schijf plaatsen in dezelfde container.)

Als u een of meer gegevensschijven aan een virtuele machine toevoegen, gebruikt u aanvullende containers als locatie voor het opslaan van deze schijven. De besturingssysteemschijf voor aanvullende virtuele machines moet ook in hun eigen containers.

Wanneer u meerdere virtuele machines maakt, kunt u hetzelfde opslagaccount voor elke nieuwe virtuele machine opnieuw gebruiken. Alleen de containers die u maakt moeten uniek zijn.

### <a name="adding-new-disks"></a>Toevoegen van nieuwe schijven

De volgende tabel geeft een overzicht van hoe u schijven toevoegt met behulp van de portal en met behulp van PowerShell.

| Methode | Opties
|-|-|
|[Gebruikersportal](#use-the-portal-to-add-additional-disks-to-a-vm)|-Nieuwe gegevensschijven toevoegen aan een bestaande virtuele machine. Nieuwe schijven worden gemaakt door Azure Stack. </br> </br>-Een bestaande schijf (VHD)-bestand toevoegen aan een eerder ingerichte virtuele machine. Om dit te doen, moet u de VHD voorbereiden en vervolgens het bestand uploaden naar Azure Stack. |
|[PowerShell](#use-powershell-to-add-multiple-unmanaged-disks-to-a-vm) | -Een nieuwe virtuele machine maken met een besturingssysteemschijf en op hetzelfde moment een of meer gegevensschijven aan die virtuele machine toevoegen. |

## <a name="use-the-portal-to-add-disks-to-a-vm"></a>Gebruik de portal schijven toevoegen aan een virtuele machine

Wanneer u de portal voor het maken van een virtuele machine voor de meeste marketplace-items, wordt standaard alleen de OS-schijf gemaakt.

Nadat u een virtuele machine hebt gemaakt, kunt u de portal om te gebruiken:
* Maak een nieuwe gegevensschijf en deze koppelen aan de virtuele machine.
* Een schijf met bestaande gegevens uploaden en deze koppelen aan de virtuele machine.

Elke niet-beheerde schijf die u toevoegt, moet in een afzonderlijke container worden geplaatst.

>[!NOTE]
>Schijven gemaakt en beheerd door Azure worden aangeroepen [beheerde schijven](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview).

### <a name="use-the-portal-to-create-and-attach-a-new-data-disk"></a>De portal gebruiken om te maken en een nieuwe gegevensschijf koppelen

1.  Kies in de portal **virtuele machines**.    
    ![Voorbeeld:-Dashboard voor VM 's](media/azure-stack-manage-vm-disks/vm-dashboard.png)

2.  Selecteer een virtuele machine die eerder zijn ingericht.   
    ![Voorbeeld: Selecteer een virtuele machine in het dashboard](media/azure-stack-manage-vm-disks/select-a-vm.png)

3.  Selecteer voor de virtuele machine, **schijven** > **koppelen nieuwe**.       
    ![Voorbeeld: Een nieuwe schijf koppelen aan de virtuele machine](media/azure-stack-manage-vm-disks/Attach-disks.png)    

4.  In de **nieuwe schijf koppelen** venster **locatie**. De locatie is standaard ingesteld op de dezelfde container met de besturingssysteemschijf.      
    ![Voorbeeld: Stel de schijflocatie](media/azure-stack-manage-vm-disks/disk-location.png)

5.  Selecteer de **opslagaccount** te gebruiken. Selecteer vervolgens de **Container** waar u de gegevensschijf wilt. Uit de **Containers** pagina, kunt u een nieuwe container maken als u wilt. Vervolgens kunt u de locatie voor de nieuwe schijf wijzigen naar een eigen container. Wanneer u een afzonderlijke container voor elke schijf gebruikt, kunt u de plaatsing van de gegevensschijf die u kunt de prestaties verbeteren distribueren. Kies **Selecteer** om op te slaan van de selectie.     
    ![Voorbeeld: Een container selecteren](media/azure-stack-manage-vm-disks/select-container.png)

6.  In de **nieuwe schijf koppelen** pagina, bij te werken de **naam**, **Type**, **grootte**, en **Hostcaching** instellingen van de schijf. Selecteer vervolgens **OK** om op te slaan van de configuratie van de nieuwe schijven voor de virtuele machine.  
    ![Voorbeeld: Volledige schijf bijlage](media/azure-stack-manage-vm-disks/complete-disk-attach.png)  

7.  Nadat Azure Stack de schijf wordt en gekoppeld aan de virtuele machine, de nieuwe schijf wordt weergegeven in de instellingen van de schijven van de virtuele machine onder **GEGEVENSSCHIJVEN**.   
    ![Voorbeeld: Weergave-schijf](media/azure-stack-manage-vm-disks/view-data-disk.png)


### <a name="attach-an-existing-data-disk-to-a-vm"></a>Een bestaande gegevensschijf koppelen aan een virtuele machine

1.  [Voorbereiden van een VHD-bestand](https://docs.microsoft.com/azure/virtual-machines/windows/classic/createupload-vhd) voor gebruik als gegevensschijf voor een virtuele machine. Die VHD-bestand uploaden naar een opslagaccount die u gebruikt met de virtuele machine die u wilt het VHD-bestand te koppelen.

  Wilt u een andere container gebruiken voor het opslaan van het VHD-bestand dan de container met de besturingssysteemschijf.   
  ![Voorbeeld: Een VHD-bestand uploaden](media/azure-stack-manage-vm-disks/upload-vhd.png)

2.  Nadat het VHD-bestand is geüpload, kunt u bent de VHD koppelen aan een virtuele machine. Selecteer in het menu aan de linkerkant, **virtuele machines**.  
 ![Voorbeeld: Selecteer een virtuele machine in het dashboard](media/azure-stack-manage-vm-disks/vm-dashboard.png)

3.  Kies de virtuele machine in de lijst.    
  ![Voorbeeld: Selecteer een virtuele machine in het dashboard](media/azure-stack-manage-vm-disks/select-a-vm.png)

4.  Selecteer op de pagina voor de virtuele machine, **schijven** > **koppelen aan bestaande**.   
  ![Voorbeeld: Een bestaande schijf koppelen](media/azure-stack-manage-vm-disks/attach-disks2.png)

5.  In de **bestaande schijf koppelen** weergeeft, schakelt **VHD-bestand**. De **Storage-accounts** pagina wordt geopend.    
  ![Voorbeeld: Selecteer een VHD-bestand](media/azure-stack-manage-vm-disks/select-vhd.png)

6.  Onder **opslagaccounts**, selecteert u het account moet worden gebruikt, en kies vervolgens een container met het VHD-bestand dat u eerder hebt geüpload. Selecteer het VHD-bestand en kies vervolgens **Selecteer** om op te slaan van de selectie.    
  ![Voorbeeld: Een container selecteren](media/azure-stack-manage-vm-disks/select-container2.png)

7.  Onder **bestaande schijf koppelen**, het bestand dat u hebt geselecteerd wordt vermeld onder **VHD-bestand**. Update de **Hostcaching** instellen van de schijf en selecteer vervolgens **OK** om op te slaan van de configuratie van de nieuwe schijven voor de virtuele machine.    
  ![Voorbeeld: Koppel het VHD-bestand](media/azure-stack-manage-vm-disks/attach-vhd.png)

8.  Nadat Azure Stack de schijf wordt en gekoppeld aan de virtuele machine, de nieuwe schijf wordt weergegeven in de instellingen van de schijven van de virtuele machine onder **gegevensschijven**.   
  ![Voorbeeld: Voer de schijf koppelen](media/azure-stack-manage-vm-disks/complete-disk-attach.png)


## <a name="use-powershell-to-add-multiple-unmanaged-disks-to-a-vm"></a>PowerShell gebruiken voor meerdere niet-beheerde schijven toevoegen aan een virtuele machine
U kunt PowerShell gebruiken voor het inrichten van een virtuele machine en een nieuwe gegevensschijf toevoegen of koppelen van een reeds bestaande **.vhd** bestand als een gegevensschijf.

De **Add-azurermvmdatadisk en** cmdlet een gegevensschijf aan een virtuele machine toegevoegd. U kunt een gegevensschijf toevoegen wanneer u een virtuele machine maakt, of u een gegevensschijf aan een bestaande virtuele machine toevoegen kunt. Geef de **VhdUri** parameter voor het distribueren van de schijven naar verschillende containers.

### <a name="add-data-disks-to-a-new-virtual-machine"></a>Gegevensschijven toevoegen aan een nieuwe virtuele machine
De volgende voorbeelden PowerShell-opdrachten gebruiken om te maken van een virtuele machine met de drie gegevensschijven, elk in een andere container geplaatst.

De eerste opdracht wordt een object van de virtuele machine gemaakt en wordt vervolgens opgeslagen in de *$VirtualMachine* variabele. De opdracht wordt een naam en de grootte van toegewezen aan de virtuele machine.
  ```
  $VirtualMachine = New-AzureRmVMConfig -VMName "VirtualMachine" `
                                      -VMSize "Standard_A2"
  ```

De volgende drie opdrachten toewijzen paden van drie gegevensschijven die aan de *$DataDiskVhdUri01*, *$DataDiskVhdUri02*, en *$DataDiskVhdUri03* variabelen. Definieer de naam van een ander pad in de URL voor het distribueren van de schijven naar verschillende containers.     
  ```
  $DataDiskVhdUri01 = "https://contoso.blob.local.azurestack.external/test1/data1.vhd"
  ```

  ```
  $DataDiskVhdUri02 = "https://contoso.blob.local.azurestack.external/test2/data2.vhd"
  ```

  ```
  $DataDiskVhdUri03 = "https://contoso.blob.local.azurestack.external/test3/data3.vhd"
  ```

De laatste drie opdrachten gegevensschijven toegevoegd aan de virtuele machine die zijn opgeslagen in *$VirtualMachine*. Elke opdracht geeft u de naam, locatie en de aanvullende eigenschappen van de schijf. De URI van elke schijf wordt opgeslagen in *$DataDiskVhdUri01*, *$DataDiskVhdUri02*, en *$DataDiskVhdUri03*.
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

Gebruik de volgende PowerShell-opdrachten om toe te voegen van de schijf- en configuratie van het besturingssysteem op de virtuele machine en start vervolgens de nieuwe virtuele machine.
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
PowerShell-opdrachten in de volgende voorbeelden gebruiken drie gegevensschijven toevoegen aan een bestaande virtuele machine.
De eerste opdracht wordt de virtuele machine met de naam VirtualMachine met behulp van de **Get-AzureRmVM** cmdlet. De opdracht slaat de virtuele machine in de *$VirtualMachine* variabele.
  ```
  $VirtualMachine = Get-AzureRmVM -ResourceGroupName "myResourceGroup" `
                                  -Name "VirtualMachine"
  ```
De volgende drie opdrachten toewijzen paden van de drie gegevensschijven aan de variabelen $DataDiskVhdUri01 $DataDiskVhdUri02 en $DataDiskVhdUri03.  De namen van ander pad in de vhduri geven aan verschillende containers voor de plaatsing van de schijf.
  ```
  $DataDiskVhdUri01 = "https://contoso.blob.local.azurestack.external/test1/data1.vhd"
  ```
  ```
  $DataDiskVhdUri02 = "https://contoso.blob.local.azurestack.external/test2/data2.vhd"
  ```
  ```
  $DataDiskVhdUri03 = "https://contoso.blob.local.azurestack.external/test3/data3.vhd"
  ```


  De volgende drie opdrachten de gegevensschijven toegevoegd aan de virtuele machine die zijn opgeslagen in de *$VirtualMachine* variabele. Elke opdracht geeft u de naam, locatie en de aanvullende eigenschappen van de schijf. De URI van elke schijf wordt opgeslagen in *$DataDiskVhdUri01*, *$DataDiskVhdUri02*, en *$DataDiskVhdUri03*.
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


  De laatste opdracht werkt de status van de virtuele machine die zijn opgeslagen in *$VirtualMachine* in -*ResourceGroupName*.
  ```
  Update-AzureRmVM -ResourceGroupName "myResourceGroup" -VM $VirtualMachine
  ```
<!-- Pending scripts  

## Distribute the data disks of an existing VM
If you have a VM with more than one disk in the same container, the service operator of the Azure Stack deployment might ask you to redistribute the disks into individual containers.

To do so, use the scripts from the following location in GitHub. These scripts can be used to move the data disks to different containers.
-->

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over virtuele machines van Azure Stack, [overwegingen voor virtuele Machines in Azure Stack](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-vm-considerations).
