---
title: Opstarten van de virtuele machine is vastgelopen op 'gereed aan Windows. Zet uw computer uit' in Azure | Microsoft Docs
description: Introductie van de stappen voor het oplossen van het probleem waarin het opstarten van de virtuele machine is vastgelopen op 'gereed aan Windows. Schakel de computer niet uit.'
services: virtual-machines-windows
documentationcenter: ''
author: Deland-Han
manager: willchen
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 09/18/2018
ms.author: delhan
ms.openlocfilehash: eb27b4e6c60f23a55a58cd2aae3cff927ffeaf03
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53316094"
---
# <a name="vm-startup-is-stuck-on-getting-windows-ready-dont-turn-off-your-computer-in-azure"></a>Opstarten van de virtuele machine is vastgelopen op 'gereed aan Windows. Zet uw computer uit' in Azure

Dit artikel helpt u het probleem oplossen wanneer uw virtuele machine (VM) is vastgelopen op de 'Windows voorbereiden. Het uitschakelen van de computer niet"fase tijdens het opstarten.

## <a name="symptoms"></a>Symptomen

Bij het gebruik **diagnostische gegevens over opstarten** als u de schermafbeelding van een virtuele machine, het besturingssysteem niet volledig worden opgestart. De virtuele machine wordt het bericht 'gereed aan Windows. Schakel de computer niet uit.'

![Voorbeeld van het bericht voor Windows Server 2012 R2](./media/troubleshoot-vm-configure-update-boot/message1.png)

![Voorbeeld van bericht](./media/troubleshoot-vm-configure-update-boot/message2.png)

## <a name="cause"></a>Oorzaak

Dit probleem treedt meestal op wanneer de server met de laatste keer opnieuw opstarten gaat nadat de configuratie is gewijzigd. Wijzigen van de configuratie kan worden geïnitialiseerd door Windows-updates of door de wijzigingen op de rollen of onderdeel van de server. Als de grootte van de updates groot is, is moet het besturingssysteem voor Windows Update, meer tijd om de wijzigingen opnieuw te configureren.

## <a name="back-up-the-os-disk"></a>Back-up van de besturingssysteemschijf

Voordat u probeert om het probleem te verhelpen, back-up van de besturingssysteemschijf.

### <a name="for-vms-with-an-encrypted-disk-you-must-unlock-the-disks-first"></a>Voor virtuele machines met een versleutelde schijf, moet u eerst de schijven ontgrendelen

Volg deze stappen om te bepalen of de virtuele machine een versleutelde VM.

1. Open uw virtuele machine in Azure portal, en blader vervolgens naar de schijven.

2. Bekijk de **versleuteling** kolom om te zien of versleuteling is ingeschakeld.

Als de besturingssysteemschijf is versleuteld, maak gebruik van de versleutelde schijf. Volg deze stappen voor het ontgrendelen van de schijf.

1. Maak een VM voor herstel die zich in dezelfde resourcegroep, Opslagaccount en locatie als de betrokken virtuele machine.

2. In de Azure-portal, de betrokken virtuele machine verwijderen en behouden van de schijf.

3. Voer PowerShell uit als beheerder.

4. Voer de volgende cmdlet om op te halen van de geheime naam.

    ```Powershell
    Login-AzureRmAccount
 
    $vmName = “VirtualMachineName”
    $vault = “AzureKeyVaultName”
 
    # Get the Secret for the C drive from Azure Key Vault
    Get-AzureKeyVaultSecret -VaultName $vault | where {($_.Tags.MachineName -eq $vmName) -and ($_.Tags.VolumeLetter -eq “C:\”) -and ($_.ContentType -eq ‘BEK‘)}

    # OR Use the below command to get BEK keys for all the Volumes
    Get-AzureKeyVaultSecret -VaultName $vault | where {($_.Tags.MachineName -eq   $vmName) -and ($_.ContentType -eq ‘BEK’)}
    ```

5. Nadat u de naam van de geheime hebt, kunt u de volgende opdrachten uitvoeren in PowerShell.

    ```Powershell
    $secretName = 'SecretName'
    $keyVaultSecret = Get-AzureKeyVaultSecret -VaultName $vault -Name $secretname
    $bekSecretBase64 = $keyVaultSecret.SecretValueText
    ```

6. De met Base64 gecodeerde waarde converteren naar bytes en de uitvoer naar een bestand te schrijven. 

    > [!Note]
    > Als u de USB-poort optie ontgrendelen, de naam van de BEK moet overeenkomen met de oorspronkelijke BEK GUID. Maak een map op station C met de naam 'BEK' voordat u deze stappen volgen.
    
    ```Powershell
    New-Item -ItemType directory -Path C:\BEK
    $bekFileBytes = [Convert]::FromBase64String($bekSecretbase64)
    $path = “c:\BEK\$secretName.BEK”
    [System.IO.File]::WriteAllBytes($path,$bekFileBytes)
    ```

7. Nadat de BEK-bestand is gemaakt op uw PC, moet u het bestand kopiëren naar de herstel-VM die u de vergrendelde besturingssysteemschijf die is gekoppeld hebt aan. Voer de volgende opdrachten met behulp van de locatie van de BEK.

    ```Powershell
    manage-bde -status F:
    manage-bde -unlock F: -rk C:\BEKFILENAME.BEK
    ```
    **Optionele**: In sommige scenario's kan het nodig zijn voor het ontsleutelen van de schijf met behulp van deze opdracht.
   
    ```Powershell
    manage-bde -off F:
    ```

    > [!Note]
    > De vorige opdracht wordt ervan uitgegaan dat de schijf voor het versleutelen is op de stationsletter f kan zijn.

8. Als u nodig hebt om Logboeken te verzamelen, gaat u naar het pad **station stationsletter: \Windows\System32\winevt\Logs**.

9. Ontkoppel de schijf van de machine voor herstel.

### <a name="create-a-snapshot"></a>Een momentopname maken

Volg de stappen in voor het maken van een momentopname, [momentopname maken van een schijf](../windows/snapshot-copy-managed-disk.md).

## <a name="collect-an-os-memory-dump"></a>Een OS-geheugendump verzamelen

Gebruik de stappen in de [verzamelen os dump](troubleshoot-common-blue-screen-error.md#collect-memory-dump-file) sectie voor het verzamelen van een besturingssysteem dump wanneer de virtuele machine is vastgelopen bij de configuratie.

## <a name="contact-microsoft-support"></a>Contact opnemen met Microsoft Ondersteuning

Nadat u het dumpbestand verzameld, neem dan contact op met [Microsoft ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) voor het analyseren van de hoofdoorzaak te achterhalen.


## <a name="rebuild-the-vm-by-using-powershell"></a>Opnieuw opbouwen van de virtuele machine met behulp van PowerShell

Nadat u het geheugendumpbestand verzameld, volg deze stappen om de virtuele machine opnieuw te maken.

**Voor niet-beheerde schijven**

```PowerShell
# To log in to Azure Resource Manager
Login-AzureRmAccount

# To view all subscriptions for your account
Get-AzureRmSubscription

# To select a default subscription for your current session
Get-AzureRmSubscription –SubscriptionID “SubscriptionID” | Select-AzureRmSubscription

$rgname = "RGname"
$loc = "Location"
$vmsize = "VmSize"
$vmname = "VmName"
$vm = New-AzureRmVMConfig -VMName $vmname -VMSize $vmsize;

$nic = Get-AzureRmNetworkInterface -Name ("NicName") -ResourceGroupName $rgname;
$nicId = $nic.Id;

$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nicId;

$osDiskName = "OSdiskName"
$osDiskVhdUri = "OSdiskURI"

$vm = Set-AzureRmVMOSDisk -VM $vm -VhdUri $osDiskVhdUri -name $osDiskName -CreateOption attach -Windows

New-AzureRmVM -ResourceGroupName $rgname -Location $loc -VM $vm -Verbose
```

**Voor beheerde schijven**

```PowerShell
# To log in to Azure Resource Manager
Login-AzureRmAccount

# To view all subscriptions for your account
Get-AzureRmSubscription

# To select a default subscription for your current session
Get-AzureRmSubscription –SubscriptionID "SubscriptionID" | Select-AzureRmSubscription

#Fill in all variables
$subid = "SubscriptionID"
$rgName = "ResourceGroupName";
$loc = "Location";
$vmSize = "VmSize";
$vmName = "VmName";
$nic1Name = "FirstNetworkInterfaceName";
#$nic2Name = "SecondNetworkInterfaceName";
$avName = "AvailabilitySetName";
$osDiskName = "OsDiskName";
$DataDiskName = "DataDiskName"

#This can be found by selecting the Managed Disks you wish you use in the Azure portal if the format below doesn't match
$osDiskResourceId = "/subscriptions/$subid/resourceGroups/$rgname/providers/Microsoft.Compute/disks/$osDiskName";
$dataDiskResourceId = "/subscriptions/$subid/resourceGroups/$rgname/providers/Microsoft.Compute/disks/$DataDiskName";

$vm = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize;

#Uncomment to add Availability Set
#$avSet = Get-AzureRmAvailabilitySet –Name $avName –ResourceGroupName $rgName;
#$vm = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize -AvailabilitySetId $avSet.Id;

#Get NIC Resource Id and add
$nic1 = Get-AzureRmNetworkInterface -Name $nic1Name -ResourceGroupName $rgName;
$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic1.Id -Primary;

#Uncomment to add a secondary NIC
#$nic2 = Get-AzureRmNetworkInterface -Name $nic2Name -ResourceGroupName $rgName;
#$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic2.Id;

#Windows VM
$vm = Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $osDiskResourceId -name $osDiskName -CreateOption Attach -Windows;

#Linux VM
#$vm = Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $osDiskResourceId -name $osDiskName -CreateOption Attach -Linux;

#Uncomment to add additional Data Disk
#Add-AzureRmVMDataDisk -VM $vm -ManagedDiskId $dataDiskResourceId -Name $dataDiskName -Caching None -DiskSizeInGB 1024 -Lun 0 -CreateOption Attach;

New-AzureRmVM -ResourceGroupName $rgName -Location $loc -VM $vm;
```
