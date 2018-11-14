---
title: Azure Disk Encryption voor Windows en Linux IaaS-VM's | Microsoft Docs
description: Dit artikel is de bijlage voor Microsoft Azure Disk Encryption voor Windows en Linux IaaS-VM's.
author: mestew
ms.service: security
ms.subservice: Azure Disk Encryption
ms.topic: article
ms.author: mstewart
ms.date: 11/12/2018
ms.openlocfilehash: e5c7d51428c66bf9e6c245f28fb13b8d4a316d18
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2018
ms.locfileid: "51614666"
---
# <a name="appendix-for-azure-disk-encryption"></a>Bijlage voor Azure Disk Encryption 
In dit artikel wordt een bijlage van [Azure Disk Encryption voor IaaS-VM's](azure-security-disk-encryption-overview.md). Zorg ervoor dat u de Azure Disk Encryption voor IaaS-VM's artikelen eerst voor meer informatie over de context te lezen. In dit artikel wordt beschreven hoe u vooraf gecodeerde VHD's en andere taken voorbereiden.

## <a name="connect-to-your-subscription"></a>Verbinding maken met uw abonnement
Voordat u begint, controleert u de [vereisten](azure-security-disk-encryption-prerequisites.md) artikel. Nadat alle vereisten wordt voldaan, kunt u verbinding met uw abonnement door het uitvoeren van de volgende cmdlets:

### <a name="bkmk_ConnectPSH"></a> Verbinding maken met uw abonnement met PowerShell

1. Start een Azure PowerShell-sessie en meld u aan bij uw Azure-account met de volgende opdracht:

     ```powershell
     Connect-AzureRmAccount 
     ```
2. Als u meerdere abonnementen hebt en wilt opgeven om te gebruiken, typt u het volgende als u wilt zien van de abonnementen voor uw account:
     
     ```powershell
     Get-AzureRmSubscription
     ```
3. Als u het abonnement dat u wilt gebruiken, typt u:
 
     ```powershell
      Select-AzureRmSubscription -SubscriptionName <Yoursubscriptionname>
     ```
4. Om te controleren of het abonnement geconfigureerd juist is, typt u:
     
     ```powershell
     Get-AzureRmSubscription
     ```
5. Indien nodig, verbinding maken met Azure AD met [Connect-AzureAD](/powershell/module/azuread/connect-azuread).
     
     ```powershell
     Connect-AzureAD
     ```
6. Om te bevestigen dat de Azure Disk Encryption-cmdlets zijn geïnstalleerd, typt u:
     
     ```powershell
     Get-command *diskencryption*
     ```
                       
7. Beoordeling [aan de slag met Azure PowerShell](/powershell/azure/get-started-azureps) en [AzureAD](/powershell/module/azuread), indien nodig.

### <a name="bkmk_ConnectCLI"></a> Verbinding maken met uw abonnement met de Azure CLI

1. Aanmelden bij Azure met [az login](/cli/azure/authenticate-azure-cli#interactive-log-in). 
     
     ```azurecli
     az login
     ```

2. Als u selecteren van een tenant wilt onder aanmelden, gebruikt:
    
     ```azurecli
     az login --tenant <tenant>
     ```

3. Hebt u meerdere abonnementen en wilt u een specifieke reeks opgeven, krijgen uw lijst met abonnementen met [az account list](/cli/azure/account#az-account-list) en geeft u met [az account set](/cli/azure/account#az-account-set).
     
     ```azurecli
     az account list
     az account set --subscription "<subscription name or ID>"
     ```

4. Controleer of de geïnstalleerde versie.
     
     ```azurecli
        az --version
     ``` 

5. Beoordeling [aan de slag met Azure CLI 2.0](/cli/azure/get-started-with-azure-cli) indien nodig. 

## <a name="sample-powershell-scripts-for-azure-disk-encryption"></a>PowerShell-voorbeeldscripts voor Azure Disk Encryption 

- **Lijst met alle versleutelde virtuele machines in uw abonnement**

     ```azurepowershell-interactive
     $osVolEncrypted = {(Get-AzureRmVMDiskEncryptionStatus -ResourceGroupName $_.ResourceGroupName -VMName $_.Name).OsVolumeEncrypted}
     $dataVolEncrypted= {(Get-AzureRmVMDiskEncryptionStatus -ResourceGroupName $_.ResourceGroupName -VMName $_.Name).DataVolumesEncrypted}
     Get-AzureRmVm | Format-Table @{Label="MachineName"; Expression={$_.Name}}, @{Label="OsVolumeEncrypted"; Expression=$osVolEncrypted}, @{Label="DataVolumesEncrypted"; Expression=$dataVolEncrypted}
     ```

- **Lijst van alle schijf encryption geheimen die worden gebruikt voor het versleutelen van virtuele machines in een key vault** 

     ```azurepowershell-interactive
     Get-AzureKeyVaultSecret -VaultName $KeyVaultName | where {$_.Tags.ContainsKey('DiskEncryptionKeyFileName')} | format-table @{Label="MachineName"; Expression={$_.Tags['MachineName']}}, @{Label="VolumeLetter"; Expression={$_.Tags['VolumeLetter']}}, @{Label="EncryptionKeyURL"; Expression={$_.Id}}
     ```

### <a name="bkmk_prereq-script"></a> Met behulp van de PowerShell-script voor Azure Disk Encryption vereisten
Als u al bekend met de vereisten voor Azure Disk Encryption bent, kunt u de [PowerShell script met vereisten voor Azure Disk Encryption](https://raw.githubusercontent.com/Azure/azure-powershell/master/src/ResourceManager/Compute/Commands.Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1 ). Zie voor een voorbeeld van het gebruik van dit PowerShell-script, de [een snelstartgids van Virtual machines versleutelen](quick-encrypt-vm-powershell.md). U kunt de opmerkingen verwijderen uit een gedeelte van het script, begint bij regel 211, voor het versleutelen van alle schijven voor bestaande virtuele machines in een bestaande resourcegroep. 

De volgende tabel ziet u welke parameters kunnen worden gebruikt in het PowerShell-script: 


|Parameter|Beschrijving|Is verplicht|
|------|------|------|
|$resourceGroupName| De naam van de resourcegroep waaraan de KeyVault behoort.  Een nieuwe resourcegroep met deze naam wordt gemaakt als deze niet bestaat.| True|
|$keyVaultName|De naam van de Sleutelkluis waar versleutelingssleutels in sleutels moeten worden geplaatst. Een nieuwe kluis met deze naam wordt gemaakt als deze niet bestaat.| True|
|$location|Locatie van de Key Vault. Zorg ervoor dat de Key Vault en de virtuele machines moeten worden versleuteld zijn op dezelfde locatie. Haal een locatielijst op met `Get-AzureRMLocation`.|True|
|$subscriptionId|Id van het Azure-abonnement moet worden gebruikt.  U kunt uw abonnements-ID ophalen met `Get-AzureRMSubscription`.|True|
|$aadAppName|De naam van de Azure AD-toepassing die wordt gebruikt voor het schrijven van geheimen naar Key Vault. Als er nog geen toepassing met deze naam bestaat, wordt deze aangemaakt. Als deze app al bestaat, moet u aadClientSecret parameter doorgeven aan het script.|False|
|$aadClientSecret|Clientgeheim van de Azure AD-toepassing die eerder is gemaakt.|False|
|$keyEncryptionKeyName|De naam van de optionele sleutel van versleutelingssleutel in Key Vault. Een nieuwe sleutel met deze naam wordt gemaakt als deze niet bestaat.|False|


## <a name="resource-manager-templates"></a>Resource Manager-sjablonen

<!--   - [Create a key vault](https://github.com/Azure/azure-quickstart-templates/tree/master/101-key-vault-create) -->

### <a name="encrypt-or-decrypt-vms-without-an-azure-ad-app"></a>Versleutelen of ontsleutelen van virtuele machines zonder een Azure AD-app


- [Schijfversleuteling voor de bestaande of het uitvoeren van IaaS Windows-VM's inschakelen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm-without-aad)
- [Schijfversleuteling voor de bestaande of het uitvoeren van IaaS Windows-VM's uitschakelen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-windows-vm-without-aad)
- [Schijfversleuteling voor een bestaande of actieve IaaS virtuele Linux-machine inschakelen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm-without-aad)  
 -  [Schakel versleuteling uit op een actieve Linux VM](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-linux-vm-without-aad) 
    - Als u versleuteling uitschakelt is alleen toegestaan op gegevensvolumes voor virtuele Linux-machines.  

### <a name="encrypt-or-decrypt-vms-with-an-azure-ad-app-previous-release"></a>Versleutelen of ontsleutelen van virtuele machines met een Azure AD-app (vorige versie) 
 
- [Schijfversleuteling voor de bestaande of het uitvoeren van IaaS Windows-VM's inschakelen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm)

- [Schijfversleuteling voor een bestaande of actieve IaaS virtuele Linux-machine inschakelen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm)    

- [Schijfversleuteling voor het uitvoeren van Windows IaaS uitschakelen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-windows-vm) 

-  [Schakel versleuteling uit op een actieve Linux VM](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-linux-vm) 
    - Als u versleuteling uitschakelt is alleen toegestaan op gegevensvolumes voor virtuele Linux-machines. 

- [Schijfversleuteling voor de nieuwe Windows van IaaS-VM vanuit de Marketplace inschakelen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image)
    - Deze sjabloon maakt een nieuwe VM voor het maken van versleutelde Windows die gebruikmaakt van de installatiekopie van de galerie met Windows Server 2012.

- [Maak een nieuwe versleutelde Windows IaaS beheerde schijf VM van image z galerie](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image-managed-disks)
    - Deze sjabloon maakt een nieuwe versleutelde Windows-VM met beheerde schijven met behulp van de galerie met Windows Server 2012.

- [Implementatie van RHEL 7.2 met volledige schijfversleuteling met beheerde schijven](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-full-disk-encrypted-rhel)
    - Deze sjabloon maakt een volledig versleutelde RHEL 7.2 in Azure met behulp van beheerde schijven. Het bevat een versleutelde 30 GB-OS-station en een versleutelde 200 GB-matrix (RAID-0) aan /mnt/raidencrypted gekoppeld. Zie de [Veelgestelde vragen over](azure-security-disk-encryption-faq.md#bkmk_LinuxOSSupport) artikel voor ondersteunde distributies van Linux-server. 

- [Implementatie van RHEL 7.2 met volledige schijfversleuteling met niet-beheerde schijven](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-full-disk-encrypted-rhel-unmanaged)
    - Deze sjabloon maakt een volledig versleutelde RHEL 7.2 in Azure met een besturingssysteemschijf met een versleutelde versie van 30 GB en een versleutelde 200 GB-matrix (RAID-0) aan /mnt/raidencrypted gekoppeld. Zie de [Veelgestelde vragen over](azure-security-disk-encryption-faq.md#bkmk_LinuxOSSupport) artikel voor ondersteunde distributies van Linux-server. 

- [Schijfversleuteling op een vooraf gecodeerde VHD voor Windows of Linux inschakelen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-pre-encrypted-vm)

- [Maak een nieuwe versleutelde beheerde schijf van een vooraf gecodeerde VHD/opslag-blob](https://github.com/Azure/azure-quickstart-templates/tree/master/201-create-encrypted-managed-disk)
    - Maakt u een nieuwe versleutelde beheerde schijf die een vooraf gecodeerde VHD en de bijbehorende instellingen voor codering

- [Schijfversleuteling op een actieve Windows-VM via de vingerafdruk van een Azure AD-client inschakelen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm-aad-client-cert)
    
- [Schakelt u schijfversleuteling op een actieve virtuele-machineschaalset met Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-vmss-linux)

- [Schijfversleuteling op een actieve virtuele-machineschaalset met Windows inschakelen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-vmss-windows)

 - [Een VM Scale instellen van virtuele Linux-machines met een jumpbox en kunnen versleuteling voor VMSS Linux implementeren](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-linux-jumpbox)

 - [Een virtuele machine Schaalsets van Windows van virtuele machines met een jumpbox en kunnen versleuteling voor VMSS Windows implementeren](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-windows-jumpbox)

- [Schijfversleuteling op een actieve Linux virtuele-machineschaalset uitschakelen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-vmss-linux)

- [Schijfversleuteling op een actieve Windows virtuele-machineschaalset uitschakelen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-vmss-windows)

## <a name="bkmk_preWin"></a> Vooraf gecodeerde Windows VHD voorbereiden
De volgende secties zijn die nodig zijn voor het vooraf gecodeerde Windows VHD voorbereiden voor implementatie als een versleutelde VHD in Azure IaaS. Gebruik de informatie voor het voorbereiden en start een nieuwe Windows virtuele machine (VHD) voor Azure Site Recovery of Azure. Zie voor meer informatie over het voorbereiden en een VHD uploaden [een gegeneraliseerde VHD uploaden en maken van nieuwe virtuele machines in Azure](../virtual-machines/windows/upload-generalized-managed.md).

### <a name="update-group-policy-to-allow-non-tpm-for-os-protection"></a>Bijwerken van Groepsbeleid om toe te staan zonder TPM voor OS-beveiliging
Configureren van de groepsbeleidsinstellingen voor BitLocker-instelling **BitLocker-stationsversleuteling**, die u vindt hier onder **lokaal computerbeleid** > **Computerconfiguratie**  >  **Beheersjablonen** > **Windows-onderdelen**. Wijzig deze instelling in **besturingssysteem stations** > **aanvullende verificatie bij opstarten vereisen** > **BitLocker zonder een compatibele TPMtoestaan**, zoals wordt weergegeven in de volgende afbeelding:

![Microsoft Antimalware in Azure](./media/azure-security-disk-encryption/disk-encryption-fig8.png)

### <a name="install-bitlocker-feature-components"></a>BitLocker-onderdelen installeren
Voor Windows Server 2012 en hoger, gebruikt u de volgende opdracht:

    dism /online /Enable-Feature /all /FeatureName:BitLocker /quiet /norestart

Voor Windows Server 2008 R2, gebruikt u de volgende opdracht uit:

    ServerManagerCmd -install BitLockers
### <a name="prepare-the-os-volume-for-bitlocker-by-using-bdehdcfg"></a>Volume met het besturingssysteem voorbereiden voor BitLocker met behulp van `bdehdcfg`
Voor het comprimeren van de OS-partitie en de machine voorbereiden voor BitLocker, uitvoeren van de [bdehdcfg](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-basic-deployment#using-bitlocker-to-encrypt-volumescommand) indien nodig:

    bdehdcfg -target c: shrink -quiet 


### <a name="protect-the-os-volume-by-using-bitlocker"></a>Volume met het besturingssysteem beveiligen met behulp van BitLocker
Gebruik de [ `manage-bde` ](https://technet.microsoft.com/library/ff829849.aspx) opdracht schakelt u versleuteling op het opstartvolume met behulp van een externe sleutelbeveiliging. Ook de externe sleutel (.bek-bestand) op de externe schijf of volume plaatsen. Versleuteling is ingeschakeld op het volume/opstarten van het systeem na het opnieuw opstarten.

    manage-bde -on %systemdrive% -sk [ExternalDriveOrVolume]
    reboot

> [!NOTE]
> De virtuele machine met een afzonderlijke gegevens/resource VHD voorbereiden voor het ophalen van de externe sleutel met BitLocker.

## <a name="bkmk_LinuxRunning"></a> Een besturingssysteemschijf op een actieve Linux-VM versleutelen

### <a name="prerequisites-for-os-disk-encryption"></a>Vereisten voor versleuteling van de OS-schijf

* De virtuele machine moet worden met behulp van een distributiepunt dat compatibel is met schijfversleuteling van OS zoals vermeld in de [Veelgestelde vragen over Azure Disk Encryption](azure-security-disk-encryption-faq.md#bkmk_LinuxOSSupport) 
* De virtuele machine moet worden gemaakt via de Marketplace-installatiekopie in Azure Resource Manager.
* Azure-VM met ten minste 4 GB aan RAM-geheugen (aanbevolen grootte is 7 GB).
* (Voor RHEL en CentOS) SELinux uitschakelen. Als u wilt uitschakelen SELinux, Zie "4.4.2. Uitschakelen van SELinux' in de [SELinux gebruikers en beheerders van handleiding](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/SELinux_Users_and_Administrators_Guide/sect-Security-Enhanced_Linux-Working_with_SELinux-Changing_SELinux_Modes.html#sect-Security-Enhanced_Linux-Enabling_and_Disabling_SELinux-Disabling_SELinux) op de virtuele machine.
* Nadat u SELinux hebt uitgeschakeld, start u de virtuele machine ten minste één keer opnieuw.

### <a name="steps"></a>Stappen
1. Een virtuele machine maken met behulp van een van de distributies die eerder zijn opgegeven.

 OS-schijfversleuteling wordt ondersteund voor 7.2 CentOS, via een speciale installatiekopie. Geef voor het gebruik van deze installatiekopie, "7.2n' als de SKU bij het maken van de virtuele machine:

 ```powershell
    Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName "OpenLogic" -Offer "CentOS" -Skus "7.2n" -Version "latest"
 ```
2. Configureer de virtuele machine op basis van uw behoeften. Als u schijven voor het versleutelen van alle de (OS + gegevens), de gegevensstations moeten worden opgegeven en koppelbaar uit/etc/fstab gaan.

 > [!NOTE]
 > Gebruik de UUID =... om op te geven gegevensstations in/etc/fstab in plaats van de apparaatnaam blokkeren (bijvoorbeeld/dev/sdb1) op te geven. Tijdens het versleutelen wijzigt de volgorde van de stations op de virtuele machine. Als uw virtuele machine is afhankelijk van een specifieke volgorde van de blokapparaten, mislukt dit ze na versleuteling te koppelen.

3. Afmelden bij de SSH-sessies.

4. Geef voor het versleutelen van het besturingssysteem, volumeType als **alle** of **OS** wanneer u versleuteling inschakelt.

 > [!NOTE]
 > Alle gebruikersruimte processen die niet worden uitgevoerd als `systemd` services moeten worden afgesloten met een `SIGKILL`. Start opnieuw op de virtuele machine. Wanneer u versleuteling van OS-schijf op een actieve virtuele machine inschakelt, plan dan op downtime van VM's.

5. Controleer regelmatig de voortgang van versleuteling met behulp van de instructies in de [volgende sectie](#monitoring-os-encryption-progress).

6. Nadat de Get-AzureRmVmDiskEncryptionStatus 'VMRestartPending' ziet, start u uw virtuele machine opnieuw aanmelden bij deze of via de portal, PowerShell of CLI.
    ```powershell
    C:\> Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMName $VMName
    -ExtensionName $ExtensionName

    OsVolumeEncrypted          : VMRestartPending
    DataVolumesEncrypted       : NotMounted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : OS disk successfully encrypted, reboot the VM
    ```
Voordat u het opnieuw opstarten, wordt aangeraden dat u opslaat [diagnostische gegevens over opstarten](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/) van de virtuele machine.

## <a name="monitoring-os-encryption-progress"></a>Voortgang van de OS-versleuteling controleren
U kunt de OS-versleuteling wordt uitgevoerd op drie manieren controleren:

* Gebruik de `Get-AzureRmVmDiskEncryptionStatus` cmdlet en het veld ProgressMessage inspecteren:
    ```powershell
    OsVolumeEncrypted          : EncryptionInProgress
    DataVolumesEncrypted       : NotMounted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : OS disk encryption started
    ```
 Wanneer de virtuele machine bereikt 'OS-schijf versleuteling aan de slag', duurt het ongeveer 40 tot 50 minuten back virtuele machine op een Premium-opslag.

 Vanwege [uitgeven #388](https://github.com/Azure/WALinuxAgent/issues/388) in WALinuxAgent, `OsVolumeEncrypted` en `DataVolumesEncrypted` weergegeven als `Unknown` in bepaalde distributies. Met versie 2.1.5 WALinuxAgent en later, dit probleem automatisch is opgelost. Als u ziet `Unknown` in de uitvoer kunt u schijfversleuteling status controleren met behulp van Azure Resource Explorer.

 Ga naar [Azure Resource Explorer](https://resources.azure.com/), en vouw vervolgens deze hiërarchie in het deelvenster selectie op links:

 ~~~~
 |-- subscriptions
     |-- [Your subscription]
          |-- resourceGroups
               |-- [Your resource group]
                    |-- providers
                         |-- Microsoft.Compute
                              |-- virtualMachines
                                   |-- [Your virtual machine]
                                        |-- InstanceView
~~~~                

 Schuif omlaag om te zien van de coderingsstatus van uw schijven in de InstanceView.

 ![Instantieweergave van virtuele machine](./media/azure-security-disk-encryption/vm-instanceview.png)

* Bekijk [diagnostische gegevens over opstarten](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/). Berichten van de extensie ADE moeten worden voorafgegaan door `[AzureDiskEncryption]`.

* Meld u aan met de virtuele machine via SSH en ophalen van het extensielogboek uit:

    /var/log/azure/Microsoft.Azure.Security.AzureDiskEncryptionForLinux

 We raden aan dat u niet aanmelden bij de virtuele machine terwijl OS-versleuteling uitgevoerd wordt. Kopieer de logboeken alleen als de andere twee methoden zijn mislukt.

## <a name="bkmk_preLinux"></a> Vooraf gecodeerde Linux-VHD voorbereiden
De voorbereiding voor vooraf gecodeerde VHD's kan variëren afhankelijk van de distributie. Voorbeelden over het voorbereiden van [Ubuntu 16](#bkmk_Ubuntu), [openSUSE 13.2](#bkmk_openSUSE), en [CentOS 7](#bkmk_CentOS) beschikbaar zijn. 

### <a name="bkmk_Ubuntu"></a> Ubuntu 16
Versleuteling tijdens de installatie van de distributie configureren met de volgende stappen:

1. Selecteer **configureren van versleutelde volumes** wanneer u de schijven partitioneren.

 ![Ubuntu 16.04 Setup](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig1.png)

2. Maak een afzonderlijke opstartstation, die niet moet worden versleuteld. Codeer uw basisstation.

 ![Ubuntu 16.04 Setup](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig2.png)

3. Geef een wachtwoordzin. Dit is de wachtwoordzin op die u hebt geüpload naar de key vault.

 ![Ubuntu 16.04 Setup](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig3.png)

4. Voltooi partitioneren.

 ![Ubuntu 16.04 Setup](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig4.png)

5. Wanneer u de virtuele machine worden opgestart en wordt gevraagd een wachtwoordzin, gebruikt u de wachtwoordzin die u hebt opgegeven in stap 3.

 ![Ubuntu 16.04 Setup](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig5.png)

6. De virtuele machine voorbereiden voor het uploaden naar Azure met [deze instructies](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-create-upload-ubuntu/). De laatste stap (opheffen van inrichting van de virtuele machine) worden niet uitgevoerd nog.

Configureren van versleuteling voor Azure door de volgende stappen:

1. Maak een bestand onder /usr/local/sbin/azure_crypt_key.sh, met de inhoud in het volgende script. Let op de KeyFileName, omdat het is de bestandsnaam van de wachtwoordzin die wordt gebruikt door Azure.

    ```
    #!/bin/sh
    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying to get the key from disks ..." >&2
    mkdir -p $MountPoint
    modprobe vfat >/dev/null 2>&1
    modprobe ntfs >/dev/null 2>&1
    sleep 2
    OPENED=0
    cd /sys/block
    for DEV in sd*; do

        echo "> Trying device: $DEV ..." >&2
        mount -t vfat -r /dev/${DEV}1 $MountPoint >/dev/null||
        mount -t ntfs -r /dev/${DEV}1 $MountPoint >/dev/null
        if [ -f $MountPoint/$KeyFileName ]; then
                cat $MountPoint/$KeyFileName
                umount $MountPoint 2>/dev/null
                OPENED=1
                break
        fi
        umount $MountPoint 2>/dev/null
    done

      if [ $OPENED -eq 0 ]; then
        echo "FAILED to find suitable passphrase file ..." >&2
        echo -n "Try to enter your password: " >&2
        read -s -r A </dev/console
        echo -n "$A"
     else
        echo "Success loading keyfile!" >&2
    fi
```

2. Wijzigen van de configuratie van de crypt in */etc/crypttab*. Dit ziet er als volgt uit:
 ```
    xxx_crypt uuid=xxxxxxxxxxxxxxxxxxxxx none luks,discard,keyscript=/usr/local/sbin/azure_crypt_key.sh
    ```

3. Als u wilt bewerken *azure_crypt_key.sh* in Windows en u hebt gekopieerd naar Linux voert `dos2unix /usr/local/sbin/azure_crypt_key.sh`.

4. Uitvoerbare machtigingen toevoegen aan het script:
 ```
    chmod +x /usr/local/sbin/azure_crypt_key.sh
 ```
5. Bewerken */etc/initramfs-tools/modules* door regels toe te voegen:
 ```
    vfat
    ntfs
    nls_cp437
    nls_utf8
    nls_iso8859-1
```
6. Voer `update-initramfs -u -k all` om bij te werken van de initramfs waarmee de `keyscript` van kracht.

7. Nu kunt u de virtuele machine inrichting.

 ![Ubuntu 16.04 Setup](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig6.png)

8. Ga door met de volgende stap en uw VHD uploaden naar Azure.

### <a name="bkmk_openSUSE"></a>  openSUSE 13.2
Voer de volgende stappen uit voor het configureren van versleuteling tijdens de installatie van softwaredistributie:
1. Wanneer u de schijven partitioneren, selecteert u **Volumegroep versleutelen**, en voer een wachtwoord. Dit is het wachtwoord die u naar de sleutelkluis uploaden gaat.

 ![openSUSE 13.2 instellen](./media/azure-security-disk-encryption/opensuse-encrypt-fig1.png)

2. Start de virtuele machine met behulp van uw wachtwoord.

 ![openSUSE 13.2 instellen](./media/azure-security-disk-encryption/opensuse-encrypt-fig2.png)

3. De virtuele machine voorbereiden voor het uploaden naar Azure door de instructies in [een SLES of opensuse gebaseerde virtuele machine voor Azure voorbereiden](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-suse-create-upload-vhd/#prepare-opensuse-131). De laatste stap (opheffen van inrichting van de virtuele machine) worden niet uitgevoerd nog.

Voor het configureren van versleuteling met Azure werkt, moet u de volgende stappen uitvoeren:
1. Bewerk de /etc/dracut.conf en voeg de volgende regel toe:
    ```
    add_drivers+=" vfat ntfs nls_cp437 nls_iso8859-1"
    ```
2. Opmerkingen bij de volgende regels aan het einde van het bestand /usr/lib/dracut/modules.d/90crypt/module-setup.sh:
 ```
    #        inst_multiple -o \
    #        $systemdutildir/system-generators/systemd-cryptsetup-generator \
    #        $systemdutildir/systemd-cryptsetup \
    #        $systemdsystemunitdir/systemd-ask-password-console.path \
    #        $systemdsystemunitdir/systemd-ask-password-console.service \
    #        $systemdsystemunitdir/cryptsetup.target \
    #        $systemdsystemunitdir/sysinit.target.wants/cryptsetup.target \
    #        systemd-ask-password systemd-tty-ask-password-agent
    #        inst_script "$moddir"/crypt-run-generator.sh /sbin/crypt-run-generator
 ```

3. De volgende regel aan het begin van het bestand /usr/lib/dracut/modules.d/90crypt/parse-crypt.sh toevoegen:
 ```
    DRACUT_SYSTEMD=0
 ```
En wijzig alle instanties van:
 ```
    if [ -z "$DRACUT_SYSTEMD" ]; then
 ```
in:
```
    if [ 1 ]; then
```
4. Bewerk /usr/lib/dracut/modules.d/90crypt/cryptroot-ask.sh en voegt deze toe aan '# Open LUKS apparaat':

    ```
    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying to get the key from disks ..." >&2
    mkdir -p $MountPoint >&2
    modprobe vfat >/dev/null >&2
    modprobe ntfs >/dev/null >&2
    for SFS in /dev/sd*; do
    echo "> Trying device:$SFS..." >&2
    mount ${SFS}1 $MountPoint -t vfat -r >&2 ||
    mount ${SFS}1 $MountPoint -t ntfs -r >&2
    if [ -f $MountPoint/$KeyFileName ]; then
        echo "> keyfile got..." >&2
        cp $MountPoint/$KeyFileName /tmp-keyfile >&2
        luksfile=/tmp-keyfile
        umount $MountPoint >&2
        break
    fi
    done
    ```
5. Voer `/usr/sbin/dracut -f -v` om bij te werken van de initrd.

6. U kunt nu de inrichting van de virtuele machine ongedaan maken en uw VHD uploaden naar Azure.

### <a name="bkmk_CentOS"></a> CentOS 7
Voer de volgende stappen uit voor het configureren van versleuteling tijdens de installatie van softwaredistributie:
1. Selecteer **mijn gegevens versleutelen** wanneer u schijven partitioneren.

 ![CentOS 7-instellingen](./media/azure-security-disk-encryption/centos-encrypt-fig1.png)

2. Zorg ervoor dat **versleutelen** is geselecteerd als de hoofdpartitie.

 ![CentOS 7-instellingen](./media/azure-security-disk-encryption/centos-encrypt-fig2.png)

3. Geef een wachtwoordzin. Dit is de wachtwoordzin op die u naar de sleutelkluis uploaden gaat.

 ![CentOS 7-instellingen](./media/azure-security-disk-encryption/centos-encrypt-fig3.png)

4. Wanneer u de virtuele machine worden opgestart en wordt gevraagd een wachtwoordzin, gebruikt u de wachtwoordzin die u hebt opgegeven in stap 3.

 ![CentOS 7-instellingen](./media/azure-security-disk-encryption/centos-encrypt-fig4.png)

5. De virtuele machine voorbereiden voor het uploaden naar Azure met behulp van de instructies "CentOS 7.0 +" in [een CentOS gebaseerde virtuele machine voor Azure voorbereiden](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-create-upload-centos/#centos-70). De laatste stap (opheffen van inrichting van de virtuele machine) worden niet uitgevoerd nog.

6. U kunt nu de inrichting van de virtuele machine ongedaan maken en uw VHD uploaden naar Azure.

Voor het configureren van versleuteling met Azure werkt, moet u de volgende stappen uitvoeren:

1. Bewerk de /etc/dracut.conf en voeg de volgende regel toe:
    ```
    add_drivers+=" vfat ntfs nls_cp437 nls_iso8859-1"
    ```

2. Opmerkingen bij de volgende regels aan het einde van het bestand /usr/lib/dracut/modules.d/90crypt/module-setup.sh:
```
    #        inst_multiple -o \
    #        $systemdutildir/system-generators/systemd-cryptsetup-generator \
    #        $systemdutildir/systemd-cryptsetup \
    #        $systemdsystemunitdir/systemd-ask-password-console.path \
    #        $systemdsystemunitdir/systemd-ask-password-console.service \
    #        $systemdsystemunitdir/cryptsetup.target \
    #        $systemdsystemunitdir/sysinit.target.wants/cryptsetup.target \
    #        systemd-ask-password systemd-tty-ask-password-agent
    #        inst_script "$moddir"/crypt-run-generator.sh /sbin/crypt-run-generator
```

3. De volgende regel aan het begin van het bestand /usr/lib/dracut/modules.d/90crypt/parse-crypt.sh toevoegen:
```
    DRACUT_SYSTEMD=0
```
En wijzig alle instanties van:
```
    if [ -z "$DRACUT_SYSTEMD" ]; then
```
tot
```
    if [ 1 ]; then
```
4. Bewerk /usr/lib/dracut/modules.d/90crypt/cryptroot-ask.sh en voegt u het volgende na het '# Open LUKS apparaat':
    ```
    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying to get the key from disks ..." >&2
    mkdir -p $MountPoint >&2
    modprobe vfat >/dev/null >&2
    modprobe ntfs >/dev/null >&2
    for SFS in /dev/sd*; do
    echo "> Trying device:$SFS..." >&2
    mount ${SFS}1 $MountPoint -t vfat -r >&2 ||
    mount ${SFS}1 $MountPoint -t ntfs -r >&2
    if [ -f $MountPoint/$KeyFileName ]; then
        echo "> keyfile got..." >&2
        cp $MountPoint/$KeyFileName /tmp-keyfile >&2
        luksfile=/tmp-keyfile
        umount $MountPoint >&2
        break
    fi
    done
    ```    
5. Voer de ' / usr/sbin/dracut - f - v ' om bij te werken van de initrd.

![CentOS 7-instellingen](./media/azure-security-disk-encryption/centos-encrypt-fig5.png)

## <a name="bkmk_UploadVHD"></a> Versleutelde VHD uploaden naar Azure storage-account
Nadat BitLocker-versleuteling of DM-Crypt-versleuteling is ingeschakeld, wordt de lokaal versleutelde VHD moet worden geüpload naar uw storage-account.
```powershell
    Add-AzureRmVhd [-Destination] <Uri> [-LocalFilePath] <FileInfo> [[-NumberOfUploaderThreads] <Int32> ] [[-BaseImageUriToPatch] <Uri> ] [[-OverWrite]] [ <CommonParameters>]
```
## <a name="bkmk_UploadSecret"></a> Uploaden van het geheim voor het vooraf versleutelde VM tot uw key vault
Wanneer u versleutelt met behulp van een Azure AD-app (vorige versie), moet het geheim voor versleuteling van de schijf die u eerder hebt verkregen worden geüpload als een geheim in uw key vault. De key vault moet schijfversleuteling en machtigingen die zijn ingeschakeld voor uw Azure AD-client bevatten.

```powershell 
 $AadClientId = "My-AAD-Client-Id"
 $AadClientSecret = "My-AAD-Client-Secret"

 $key vault = New-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -Location $Location

 Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -ServicePrincipalName $AadClientId -PermissionsToKeys all -PermissionsToSecrets all
 Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -EnabledForDiskEncryption
``` 

### <a name="bkmk_SecretnoKEK"></a> Versleutelingsgeheim van schijf niet is versleuteld met een KEK-sleutel
Als u het geheim in uw key vault instelt, gebruikt u [Set-AzureKeyVaultSecret](/powershell/module/azurerm.keyvault/set-azurekeyvaultsecret). Als u een Windows-machine hebt, de bek-bestand is als een tekenreeks met Base 64-codering en vervolgens geüpload naar uw sleutelkluis met de `Set-AzureKeyVaultSecret` cmdlet. Voor Linux, is de wachtwoordzin in als een tekenreeks met Base 64 gecodeerde en vervolgens geüpload naar de key vault. Bovendien moet u ervoor dat de volgende codes zijn ingesteld wanneer u het geheim in de key vault maakt.

#### <a name="windows-bek-file"></a>De BEK Windows-bestand
```powershell
# Change the VM Name, key vault name, and specify the path to the BEK file.
$VMName ="MySecureVM"
$BEKFilepath = "C:\test\BEK\12345678-90AB-CDEF-A1B2-C3D4E5F67890A.BEK"
$VeyVaultName ="MySecureVault"

# Get the name of the BEK file from the BEK file path. This will be a tag for the key vault secret.
$BEKFileName =  Split-Path $BEKFilepath -Leaf

# These tags will be added to the key vault secret so you can easily see which BEK file belongs to which VM.
$tags = @{“MachineName” = “$VMName”;"DiskEncryptionKeyEncryptionAlgorithm" = "RSA-OAEP"; "DiskEncryptionKeyFileName" = "$BEKFileName"}

# Convert the BEK file to a Base64 string.
$FileContentEncoded = [System.convert]::ToBase64String((Get-Content -Path $BEKFilepath -Encoding Byte))

# Create a new secret in the vault from the converted BEK file. 
# The file is converted to a secure string before import into the key vault

$SecretName = [guid]::NewGuid().ToString()
$SecureSecretValue = ConvertTo-SecureString $FileContentEncoded -AsPlainText -Force
$Secret = Set-AzureKeyVaultSecret -VaultName $VeyVaultName -Name $SecretName -SecretValue $SecureSecretValue -tags $tags

# Show the secret's URL and store it as a variable. This is used as -DiskEncryptionKeyUrl in Set-AzureRmVMOSDisk when you attach your OS disk. 
$SecretUrl=$secret.Id
$SecretUrl
```

#### <a name="linux"></a>Linux
```powershell

 # This is the passphrase that was provided for encryption during the distribution installation
 $passphrase = "contoso-password"

 $tags = @{"DiskEncryptionKeyEncryptionAlgorithm" = "RSA-OAEP"; "DiskEncryptionKeyFileName" = "LinuxPassPhraseFileName"}
 $secretName = [guid]::NewGuid().ToString()
 $secretValue = [Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes($passphrase))
 $secureSecretValue = ConvertTo-SecureString $secretValue -AsPlainText -Force

 $secret = Set-AzureKeyVaultSecret -VaultName $KeyVaultName -Name $secretName -SecretValue $secureSecretValue -tags $tags
 $secretUrl = $secret.Id
```


Gebruik de `$secretUrl` in de volgende stap voor [de OS-schijf koppelen zonder KEK](#bkmk_URLnoKEK).

### <a name="bkmk_SecretKEK"></a> Versleutelingsgeheim van schijf versleuteld met een KEK-sleutel
Voordat u het geheim naar de key vault uploaden, kunt u deze desgewenst versleutelen met behulp van een sleutel van versleutelingssleutel. Gebruik de wrap [API](https://msdn.microsoft.com/library/azure/dn878066.aspx) voor het eerst het geheim met behulp van de sleutel van versleutelingssleutel te versleutelen. De uitvoer van deze bewerking wrap is een tekenreeks met Base 64 URL-codering, die u vervolgens als een geheim uploaden met behulp van kunt de [ `Set-AzureKeyVaultSecret` ](/powershell/module/azurerm.keyvault/set-azurekeyvaultsecret) cmdlet.

```powershell
    # This is the passphrase that was provided for encryption during the distribution installation
    $passphrase = "contoso-password"

    Add-AzureKeyVaultKey -VaultName $KeyVaultName -Name "keyencryptionkey" -Destination Software
    $KeyEncryptionKey = Get-AzureKeyVaultKey -VaultName $KeyVault.OriginalVault.Name -Name "keyencryptionkey"

    $apiversion = "2015-06-01"

    ##############################
    # Get Auth URI
    ##############################

    $uri = $KeyVault.VaultUri + "/keys"
    $headers = @{}

    $response = try { Invoke-RestMethod -Method GET -Uri $uri -Headers $headers } catch { $_.Exception.Response }

    $authHeader = $response.Headers["www-authenticate"]
    $authUri = [regex]::match($authHeader, 'authorization="(.*?)"').Groups[1].Value

    Write-Host "Got Auth URI successfully"

    ##############################
    # Get Auth Token
    ##############################

    $uri = $authUri + "/oauth2/token"
    $body = "grant_type=client_credentials"
    $body += "&client_id=" + $AadClientId
    $body += "&client_secret=" + [Uri]::EscapeDataString($AadClientSecret)
    $body += "&resource=" + [Uri]::EscapeDataString("https://vault.azure.net")
    $headers = @{}

    $response = Invoke-RestMethod -Method POST -Uri $uri -Headers $headers -Body $body

    $access_token = $response.access_token

    Write-Host "Got Auth Token successfully"

    ##############################
    # Get KEK info
    ##############################

    $uri = $KeyEncryptionKey.Id + "?api-version=" + $apiversion
    $headers = @{"Authorization" = "Bearer " + $access_token}

    $response = Invoke-RestMethod -Method GET -Uri $uri -Headers $headers

    $keyid = $response.key.kid

    Write-Host "Got KEK info successfully"

    ##############################
    # Encrypt passphrase using KEK
    ##############################

    $passphraseB64 = [Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes($Passphrase))
    $uri = $keyid + "/encrypt?api-version=" + $apiversion
    $headers = @{"Authorization" = "Bearer " + $access_token; "Content-Type" = "application/json"}
    $bodyObj = @{"alg" = "RSA-OAEP"; "value" = $passphraseB64}
    $body = $bodyObj | ConvertTo-Json

    $response = Invoke-RestMethod -Method POST -Uri $uri -Headers $headers -Body $body

    $wrappedSecret = $response.value

    Write-Host "Encrypted passphrase successfully"

    ##############################
    # Store secret
    ##############################

    $secretName = [guid]::NewGuid().ToString()
    $uri = $KeyVault.VaultUri + "/secrets/" + $secretName + "?api-version=" + $apiversion
    $secretAttributes = @{"enabled" = $true}
    $secretTags = @{"DiskEncryptionKeyEncryptionAlgorithm" = "RSA-OAEP"; "DiskEncryptionKeyFileName" = "LinuxPassPhraseFileName"}
    $headers = @{"Authorization" = "Bearer " + $access_token; "Content-Type" = "application/json"}
    $bodyObj = @{"value" = $wrappedSecret; "attributes" = $secretAttributes; "tags" = $secretTags}
    $body = $bodyObj | ConvertTo-Json

    $response = Invoke-RestMethod -Method PUT -Uri $uri -Headers $headers -Body $body

    Write-Host "Stored secret successfully"

    $secretUrl = $response.id
```

Gebruik `$KeyEncryptionKey` en `$secretUrl` in de volgende stap voor [Bezig met koppelen van de besturingssysteemschijf met behulp van de KEK](#BKMK_URLKEK).

##  <a name="bkmk_SecretURL"></a> Geef een URL van accountsleutelgeheim wanneer u een besturingssysteemschijf te koppelen

###  <a name="bkmk_URLnoKEK"></a>Zonder een KEK-sleutel
Terwijl u de besturingssysteemschijf koppelen bent, moet u doorgeven `$secretUrl`. De URL is gegenereerd in de sectie 'schijfversleuteling geheim niet versleuteld met een KEK-sleutel'.
```powershell
    Set-AzureRmVMOSDisk `
            -VM $VirtualMachine `
            -Name $OSDiskName `
            -SourceImageUri $VhdUri `
            -VhdUri $OSDiskUri `
            -Linux `
            -CreateOption FromImage `
            -DiskEncryptionKeyVaultId $KeyVault.ResourceId `
            -DiskEncryptionKeyUrl $SecretUrl
```
### <a name="bkmk_URLKEK"></a>Met behulp van een KEK-sleutel
Wanneer u de besturingssysteemschijf koppelen, `$KeyEncryptionKey` en `$secretUrl`. De URL is gegenereerd in de sectie 'Versleutelingsgeheim van schijf versleuteld met een KEK-sleutel'.
```powershell
    Set-AzureRmVMOSDisk `
            -VM $VirtualMachine `
            -Name $OSDiskName `
            -SourceImageUri $CopiedTemplateBlobUri `
            -VhdUri $OSDiskUri `
            -Linux `
            -CreateOption FromImage `
            -DiskEncryptionKeyVaultId $KeyVault.ResourceId `
            -DiskEncryptionKeyUrl $SecretUrl `
            -KeyEncryptionKeyVaultId $KeyVault.ResourceId `
            -KeyEncryptionKeyURL $KeyEncryptionKey.Id
```
