---
title: Opstarten van de virtuele machine loopt vast bij 'Windows voorbereiden. Schakel de computer niet uit.' in Azure | Microsoft Docs
description: Introductie van de stappen voor het oplossen van het probleem op welke virtuele machine opstarten is vastgelopen op 'Windows voorbereiden. Zet uw computer uit'.
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
ms.openlocfilehash: 722bf7b42e500e3e6a46f48646ff1fd2edfb68f1
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52955733"
---
# <a name="vm-startup-is-stuck-on-getting-windows-ready-dont-turn-off-your-computer-in-azure"></a>Opstarten van de virtuele machine loopt vast bij 'Windows voorbereiden. Schakel de computer niet uit.' in Azure

Dit artikel helpt u het probleem oplossen wanneer uw virtuele Machine (VM) is vastgelopen op de 'Windows voorbereiden. Schakel de computer niet uit.' fase tijdens het opstarten.

## <a name="symptoms"></a>Symptomen

Bij het gebruik **diagnostische gegevens over opstarten** als u de schermafbeelding van een virtuele machine, vindt u het besturingssysteem is niet volledig opgestart. Bovendien de virtuele machine weer een **'Windows voorbereiden. Zet uw computer uit."** Bericht.

![Voorbeeld van bericht](./media/troubleshoot-vm-configure-update-boot/message1.png)

![Voorbeeld van bericht](./media/troubleshoot-vm-configure-update-boot/message2.png)

## <a name="cause"></a>Oorzaak

Dit probleem treedt meestal op wanneer de server met de laatste keer opnieuw opstarten gaat nadat de configuratie is gewijzigd. Wijzigen van de configuratie kan worden geïnitialiseerd door Windows-updates of door de wijzigingen op de rollen of onderdeel van de server. Voor Windows Update, als de grootte van de updates groot is, is het besturingssysteem wordt meer tijd nodig hebt om de wijzigingen opnieuw te configureren.

## <a name="back-up-the-os-disk"></a>Back-up van de besturingssysteemschijf

Voordat u probeert om het probleem te verhelpen, back-up van de besturingssysteemschijf:

### <a name="for-vms-with-an-encrypted-disk-you-must-unlock-the-disks-first"></a>Voor virtuele machines met een versleutelde schijf, moet u eerst de schijven ontgrendelen

Controleren of de virtuele machine een versleutelde VM is. Voer de volgende stappen uit om dit te doen:

1. Op de portal en open uw virtuele machine en blader vervolgens naar de schijven.

2. Hier ziet u een kolom call '-versleuteling,"wat vertelt u of versleuteling is ingeschakeld.

Als de Besturingssysteemschijf is versleuteld, maak gebruik van de versleutelde schijf. Voer de volgende stappen uit om dit te doen:

1. Maak een VM voor herstel die zich in dezelfde resourcegroep, Opslagaccount en locatie als de betrokken virtuele machine.

2. In Azure portal, de betrokken virtuele machine verwijderen en behouden van de schijf.

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

5. Nadat u de naam van het geheim hebt, voert u de volgende opdrachten in PowerShell:

    ```Powershell
    $secretName = 'SecretName'
    $keyVaultSecret = Get-AzureKeyVaultSecret -VaultName $vault -Name $secretname
    $bekSecretBase64 = $keyVaultSecret.SecretValueText
    ```

6. De met Base64 gecodeerde waarde converteren naar bytes en de uitvoer naar een bestand te schrijven. 

    > [!Note]
    > De naam van de BEK moet overeenkomen met de oorspronkelijke BEK GUID als u de USB-poort ontgrendelen optie. U moet ook, maak een map op station C met de naam 'BEK' voordat u de volgende stappen werken.
    
    ```Powershell
    New-Item -ItemType directory -Path C:\BEK
    $bekFileBytes = [Convert]::FromBase64String($bekSecretbase64)
    $path = “c:\BEK\$secretName.BEK”
    [System.IO.File]::WriteAllBytes($path,$bekFileBytes)
    ```

7. Nadat de BEK-bestand is gemaakt op uw PC, moet u het bestand kopiëren naar de herstel-VM die u de vergrendelde besturingssysteemschijf die is gekoppeld hebt aan. Voer de volgende opdracht uit met behulp van de BEK bestandslocatie:

    ```Powershell
    manage-bde -status F:
    manage-bde -unlock F: -rk C:\BEKFILENAME.BEK
    ```
    **Optionele** in sommige scenario's mogelijk ook het ontsleutelen van de schijf met deze opdracht.
   
    ```Powershell
    manage-bde -off F:
    ```

    > [!Note]
    > Dit overweegt dat de schijf voor het versleutelen op de letter F: is.

8. Als u nodig hebt om Logboeken te verzamelen, kunt u navigeren naar het pad **station stationsletter: \Windows\System32\winevt\Logs**.

9. Ontkoppel de schijf van de machine voor herstel.

### <a name="create-a-snapshot"></a>Een momentopname maken

Volg de stappen in voor het maken van een momentopname, [momentopname maken van een schijf](../windows/snapshot-copy-managed-disk.md).

## <a name="collect-an-os-memory-dump"></a>Een OS-geheugendump verzamelen

Gebruik de stappen in de [verzamelen os dump](troubleshoot-common-blue-screen-error.md#collect-memory-dump-file) sectie voor het verzamelen van een besturingssysteem dump wanneer de virtuele machine is vastgelopen bij de configuratie.

## <a name="contact-microsoft-support"></a>Contact opnemen met Microsoft Ondersteuning

Nadat u het dumpbestand verzameld, neem dan contact op met [Microsoft ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) voor analyse van de hoofdoorzaak te achterhalen.


## <a name="rebuild-the-vm-using-powershell"></a>Opnieuw opbouwen van de virtuele machine met behulp van PowerShell

Nadat u het geheugendumpbestand verzameld, gebruikt u de volgende stappen opnieuw opbouwen van de virtuele machine.

**Voor niet-beheerde schijven**

```PowerShell
# To login to Azure Resource Manager
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
# To login to Azure Resource Manager
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

#This can be found by selecting the Managed Disks you wish you use in the Azure Portal if the format below doesn't match
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
