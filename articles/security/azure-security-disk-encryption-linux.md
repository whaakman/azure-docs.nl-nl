---
title: Schakel Azure Disk Encryption voor Linux IaaS-VM 's
description: In dit artikel vindt u instructies over het inschakelen van Microsoft Azure Disk Encryption voor Linux IaaS-VM's.
author: msmbaldwin
ms.service: security
ms.topic: article
ms.author: mbaldwin
ms.date: 04/05/2019
ms.custom: seodec18
ms.openlocfilehash: ffdda51c4739dfad6c326a5c90f4a93ebfb321cd
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67294903"
---
# <a name="enable-azure-disk-encryption-for-linux-iaas-vms"></a>Schakel Azure Disk Encryption voor Linux IaaS-VM 's 

U kunt veel schijfversleuteling scenario's inschakelen en de stappen kunnen variëren op basis van het scenario. De volgende secties voor de scenario's in meer detail voor Linux IaaS-VM's. Voordat u kunt schijfversleuteling, gebruiken de [vereisten voor Azure Disk Encryption](azure-security-disk-encryption-prerequisites.md) moeten worden uitgevoerd, en de [aanvullende vereisten voor Linux IaaS-VM's](azure-security-disk-encryption-prerequisites.md#additional-prerequisites-for-linux-iaas-vms) sectie moet worden gecontroleerd.

Duren voordat een [momentopname](../virtual-machines/windows/snapshot-copy-managed-disk.md) en/of back-up voordat de schijven worden versleuteld. Back-ups zorgen ervoor dat een optie voor siteherstel mogelijk als er een onverwachte fout optreedt tijdens het versleutelen. Virtuele machines met beheerde schijven moeten u een back-up voordat versleuteling plaatsvindt. Als u een back-up is gemaakt, kunt u de cmdlet Set-AzVMDiskEncryptionExtension kunt gebruiken voor het versleutelen van beheerde schijven met de parameter - skipVmBackup op te geven. Zie voor meer informatie over hoe u een back-up en herstel van versleutelde virtuele machines, de [Azure Backup](../backup/backup-azure-vms-encryption.md) artikel. 

>[!WARNING]
> - Als u eerder hebt gebruikt [Azure Disk Encryption met Azure AD-app](azure-security-disk-encryption-prerequisites-aad.md) voor het versleutelen van deze virtuele machine hebt uitgevoerd, om door te gaan met deze optie gebruiken voor het versleutelen van uw virtuele machine. U kunt geen gebruiken [Azure Disk Encryption](azure-security-disk-encryption-prerequisites.md) op deze versleutelde VM als dit niet een ondersteund scenario betekenis overschakelen van AAD-toepassing voor deze virtuele machine versleuteld wordt niet ondersteund nog.
 > - Azure Disk Encryption moet de Key Vault en de virtuele machines in dezelfde regio worden geplaatst. Maak en gebruik van een Key Vault die zich in dezelfde regio als de virtuele machine moeten worden versleuteld.
> - Bij het versleutelen van Linux-besturingssysteem, volumes, de virtuele machine moet worden beschouwd als niet beschikbaar. Wordt aangeraden om te voorkomen dat SSH-aanmeldingspogingen terwijl de versleuteling uitgevoerd wordt om te voorkomen dat problemen met alle geopende bestanden dat moeten worden geopend tijdens het versleutelingsproces blokkeren. Om te controleren op wordt uitgevoerd, de [Get-AzVMDiskEncryptionStatus](/powershell/module/az.compute/get-azvmdiskencryptionstatus) of [vm encryption show](/cli/azure/vm/encryption#az-vm-encryption-show) opdrachten kunnen worden gebruikt. Dit proces kan worden verwacht om een paar uur voor een volume met het besturingssysteem 30GB, plus de extra tijd voor het versleutelen van gegevensvolumes. Gegevens volume versleuteling tijd zijn in verhouding met de grootte en aantal van de gegevensvolumes, tenzij de coderen alle optie-indeling wordt gebruikt. 
> - Uitschakelen van versleuteling op Linux-VM's wordt alleen ondersteund voor de gegevensvolumes. Het wordt niet ondersteund op de gegevens of besturingssysteemvolumes als het volume met het besturingssysteem is versleuteld.  

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="bkmk_RunningLinux"> </a> Schakelt u versleuteling op een bestaande of actieve IaaS virtuele Linux-machine
In dit scenario kunt u versleuteling inschakelen met behulp van de Resource Manager-sjabloon, de PowerShell-cmdlets of de CLI-opdrachten. Als u schema-informatie voor de extensie van de virtuele machine nodig hebt, raadpleegt u de [Azure Disk Encryption voor Linux-extensie](../virtual-machines/extensions/azure-disk-enc-linux.md) artikel.

>[!IMPORTANT]
 >Dit is verplicht op momentopname en/of back-up een beheerde schijf op basis van de VM-exemplaar buiten en voordat Azure Disk Encryption wordt ingeschakeld. Een momentopname van de beheerde schijf kan worden uitgevoerd vanuit de portal of [Azure Backup](../backup/backup-azure-vms-encryption.md) kan worden gebruikt. Back-ups zorgen ervoor dat een optie voor siteherstel mogelijk in het geval van een onverwachte fout opgetreden tijdens het versleutelen is. Als u een back-up is gemaakt, kan de cmdlet Set-AzVMDiskEncryptionExtension worden gebruikt voor het versleutelen van beheerde schijven met de parameter - skipVmBackup op te geven. De opdracht Set-AzVMDiskEncryptionExtension, op basis van virtuele machines op beheerde schijven op basis van mislukken totdat een back-up is gemaakt en deze parameter is opgegeven. 
>
>Versleutelen of als u versleuteling uitschakelt kan ertoe leiden dat de virtuele machine opnieuw op te starten. 
>

### <a name="bkmk_RunningLinuxCLI"> </a>Schakelt u versleuteling op een bestaande of actieve Linux-VM met behulp van Azure CLI 
U kunt schijfversleuteling inschakelen op uw versleutelde VHD te installeren en gebruiken de [Azure CLI 2.0](/cli/azure) opdrachtregel-hulpprogramma. U kunt PowerShell in uw browser gebruiken met [Azure Cloud Shell](../cloud-shell/overview.md), of installeren op uw lokale computer en dan gebruiken in een PowerShell-sessie. Gebruik de volgende CLI-opdrachten voor het inschakelen van versleuteling op bestaande of IaaS virtuele Linux-machines uitvoeren in Azure:

Gebruik de [az vm encryption inschakelen](/cli/azure/vm/encryption#az-vm-encryption-enable) opdracht schakelt u versleuteling op een actieve IaaS-virtuele machine in Azure.

- **Een actieve virtuele machine versleutelen:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --disk-encryption-keyvault "MySecureVault" --volume-type [All|OS|Data]
     ```

- **Een actieve virtuele machine met behulp van de KEK versleutelen:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --disk-encryption-keyvault  "MySecureVault" --key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type [All|OS|Data]
     ```

    >[!NOTE]
    > De syntaxis voor de waarde van de schijf-versleuteling-keyvault-parameter is de volledige id-reeks: / subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br>
De syntaxis voor de waarde van de encryptiesleutel parameter is de volledige URI naar de KEK-sleutel in: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

- **Controleer of dat de schijven worden versleuteld:** Als u wilt controleren op de status voor schijfversleuteling van een IaaS-VM, gebruikt u de [az vm encryption show](/cli/azure/vm/encryption#az-vm-encryption-show) opdracht. 

     ```azurecli-interactive
     az vm encryption show --name "MySecureVM" --resource-group "MyVirtualMachineResourceGroup"
     ```

- **Schakel versleuteling uit:** Als u wilt uitschakelen versleuteling, gebruikt u de [az vm encryption uitschakelen](/cli/azure/vm/encryption#az-vm-encryption-disable) opdracht. Als u versleuteling uitschakelt is alleen toegestaan op gegevensvolumes voor virtuele Linux-machines.

     ```azurecli-interactive
     az vm encryption disable --name "MySecureVM" --resource-group "MyVirtualMachineResourceGroup" --volume-type DATA
     ```

### <a name="bkmk_RunningLinuxPSH"> </a> Schakelt u versleuteling op een bestaande of actieve Linux-VM met behulp van PowerShell
Gebruik de [Set AzVMDiskEncryptionExtension](/powershell/module/az.compute/set-azvmdiskencryptionextension) cmdlet schakelt u versleuteling op een actieve IaaS-virtuele machine in Azure. Duren voordat een [momentopname](../virtual-machines/windows/snapshot-copy-managed-disk.md) en/of back-up van de virtuele machine met [Azure Backup](../backup/backup-azure-vms-encryption.md) voordat de schijven worden versleuteld. De parameter - skipVmBackup is al opgegeven in de PowerShell-scripts voor het versleutelen van een actieve Linux-VM.

-  **Een actieve virtuele machine versleutelen:** Het onderstaande script wordt uw variabelen geïnitialiseerd en wordt de cmdlet Set-AzVMDiskEncryptionExtension uitgevoerd. De resourcegroep, de virtuele machine en de key vault, moeten al zijn gemaakt voor de vereisten. MyVirtualMachineResourceGroup, MySecureVM en MySecureVault vervangen door uw waarden. Wijzig de parameter - VolumeType om op te geven welke schijven die u aan het versleutelen bent.

     ```azurepowershell
      $KVRGname = 'MyKeyVaultResourceGroup';
      $VMRGName = 'MyVirtualMachineResourceGroup';
      $vmName = 'MySecureVM';
      $KeyVaultName = 'MySecureVault';
      $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
      $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
      $KeyVaultResourceId = $KeyVault.ResourceId;
      $sequenceVersion = [Guid]::NewGuid();  

      Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGName -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -VolumeType '[All|OS|Data]' -SequenceVersion $sequenceVersion -skipVmBackup;
     ```
- **Een actieve virtuele machine met behulp van de KEK versleutelen:** Mogelijk moet u de parameter - VolumeType toevoegen als u het versleutelen bent gegevensschijven en niet de besturingssysteemschijf. 

     ```azurepowershell
      $KVRGname = 'MyKeyVaultResourceGroup';
      $VMRGName = 'MyVirtualMachineResourceGroup';
      $vmName = 'MyExtraSecureVM';
      $KeyVaultName = 'MySecureVault';
      $keyEncryptionKeyName = 'MyKeyEncryptionKey';
      $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
      $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
      $KeyVaultResourceId = $KeyVault.ResourceId;
      $keyEncryptionKeyUrl = (Get-AzKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;
      $sequenceVersion = [Guid]::NewGuid();  

      Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGName -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId -VolumeType '[All|OS|Data]' -SequenceVersion $sequenceVersion -skipVmBackup;
     ```

    >[!NOTE]
    > De syntaxis voor de waarde van de schijf-versleuteling-keyvault-parameter is de volledige id-reeks: / subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br> De syntaxis voor de waarde van de encryptiesleutel parameter is de volledige URI naar de KEK-sleutel in: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 
    
- **Controleer of dat de schijven worden versleuteld:** Als u wilt controleren op de status voor schijfversleuteling van een IaaS-VM, gebruikt u de [Get-AzVmDiskEncryptionStatus](/powershell/module/az.compute/get-azvmdiskencryptionstatus) cmdlet. 
    
     ```azurepowershell-interactive 
     Get-AzVmDiskEncryptionStatus -ResourceGroupName 'MyVirtualMachineResourceGroup' -VMName 'MySecureVM'
     ```
    
- **Schijfversleuteling uitschakelen:** Als u wilt de versleuteling uitschakelen, gebruikt u de [uitschakelen AzVMDiskEncryption](/powershell/module/az.compute/disable-azvmdiskencryption) cmdlet. Als u versleuteling uitschakelt is alleen toegestaan op gegevensvolumes voor virtuele Linux-machines.
     
     ```azurepowershell-interactive 
     Disable-AzVMDiskEncryption -ResourceGroupName 'MyVirtualMachineResourceGroup' -VMName 'MySecureVM'
     ```


### <a name="bkmk_RunningLinux"> </a> Schakelt u versleuteling op een bestaande of actieve IaaS virtuele Linux-machine met een sjabloon

U kunt schijfversleuteling op een bestaande of actieve IaaS Linux VM in Azure inschakelen met behulp van de [Resource Manager-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm-without-aad).

1. Klik op **implementeren in Azure** op de Azure-quickstart-sjabloon.

2. Selecteer het abonnement, resourcegroep, locatie voor resourcegroep, parameters, juridische voorwaarden en -overeenkomst. Klik op **maken** versleuteling op de bestaande of actieve IaaS-VM inschakelen.

De volgende tabel bevat de parameters van de Resource Manager-sjabloon voor bestaande of uitvoeren van virtuele machines:

| Parameter | Description |
| --- | --- |
| vmName | De naam van de virtuele machine om uit te voeren van de versleutelingsbewerking. |
| keyVaultName | De naam van de sleutelkluis waarin de BitLocker-sleutel moet worden geüpload naar. U kunt deze ophalen met behulp van de cmdlet `(Get-AzKeyVault -ResourceGroupName <MyKeyVaultResourceGroupName>). Vaultname` of de Azure CLI-opdracht `az keyvault list --resource-group "MyKeyVaultResourceGroupName"`|
| keyVaultResourceGroup | Naam van de resourcegroep waarin de key vault|
|  KeyEncryptionKeyURL | URL van de sleutel van versleutelingssleutel die wordt gebruikt voor het versleutelen van de gegenereerde BitLocker-sleutel. Deze parameter is optioneel als u selecteert **nokek** in de vervolgkeuzelijst UseExistingKek. Als u selecteert **kek** in de vervolgkeuzelijst UseExistingKek voert u de _keyEncryptionKeyURL_ waarde. |
| VolumeType | Het type volume dat de versleutelingsbewerking wordt uitgevoerd op. Geldige waarden zijn _OS_, _gegevens_, en _alle_. 
| forceUpdateTag | In een unieke waarde, zoals een GUID doorgeven telkens wanneer de bewerking moet worden geforceerd uitvoeren. |
| resizeOSDisk | Moet de OS-partitie worden kleiner gemaakt zodat ze volledig OS VHD voordat het systeemvolume splitsen in beslag nemen. |
| location | Locatie voor alle resources. |



## <a name="encrypt-virtual-machine-scale-sets"></a>Versleutelen van de virtuele-machineschaalsets
[Azure virtuele-machineschaalsets](../virtual-machine-scale-sets/overview.md) kunt u maken en beheren van een groep identieke, met gelijke taakverdeling VM's worden geladen. Het aantal VM-exemplaren kan automatisch toenemen of afnemen in reactie op vraag of een ingesteld schema. De CLI of Azure PowerShell gebruiken voor het versleutelen van virtuele-machineschaalsets. Alleen de versleuteling van data-schijven wordt ondersteund op Linux scale set virtuele machines.

Een voorbeeld van de batch-bestand voor schijfversleuteling van Linux scale set gegevens vindt [hier](https://github.com/Azure-Samples/azure-cli-samples/tree/master/disk-encryption/vmss). In dit voorbeeld wordt een resourcegroep, het Linux-schaalset gemaakt, koppelt u een schijf van 5 GB aan gegevens en versleutelt de virtuele-machineschaalset.

### <a name="encrypt-virtual-machine-scale-sets-with-azure-cli"></a>Versleutelen van de virtuele-machineschaalsets met Azure CLI

Gebruik de [az vmss versleuteling inschakelen](/cli/azure/vmss/encryption#az-vmss-encryption-enable) versleuteling op een Windows-virtuele-machineschaalset inschakelen. Als u het beleid voor upgrades op de schaal die is ingesteld op handmatig hebt ingesteld, start u de codering met [az vmss update-instances](/cli/azure/vmss#az-vmss-update-instances). De resourcegroep, de virtuele machine en de sleutelkluis moeten al zijn gemaakt voor de vereisten. 

-  **Versleutelen van een actieve virtuele-machineschaalset**
    ```azurecli-interactive
    az vmss encryption enable --resource-group "MyVMScaleSetResourceGroup" --name "MySecureVmss" --volume-type DATA --disk-encryption-keyvault "MySecureVault"
    ```

-  **Versleutelen van een actieve virtuele-machineschaalset met behulp van de KEK-sleutel voor de sleutel Inpakken**
     ```azurecli-interactive
     az vmss encryption enable --resource-group "MyVMScaleSetResourceGroup" --name "MySecureVmss" --volume-type DATA --disk-encryption-keyvault "MySecureVault" --key-encryption-key "MyKEK" --key-encryption-keyvault "MySecureVault"
     ```

    >[!NOTE]
    > De syntaxis voor de waarde van de schijf-versleuteling-keyvault-parameter is de volledige id-reeks: / subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br> De syntaxis voor de waarde van de encryptiesleutel parameter is de volledige URI naar de KEK-sleutel in: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

- **Status voor schijfversleuteling voor een virtuele-machineschaalset ophalen:** Gebruik [az vmss encryption show](/cli/azure/vmss/encryption#az-vmss-encryption-show)

    ```azurecli-interactive
    az vmss encryption show --resource-group "MyVMScaleSetResourceGroup" --name "MySecureVmss"
    ```

- **Schakel versleuteling uit op een virtuele-machineschaalset**: Gebruik [az vmss-versleuteling uitschakelen](/cli/azure/vmss/encryption#az-vmss-encryption-disable)
    ```azurecli-interactive
    az vmss encryption disable --resource-group "MyVMScaleSetResourceGroup" --name "MySecureVmss"
    ```

### <a name="encrypt-virtual-machine-scale-sets-with-azure-powershell"></a>Versleutelen van de virtuele-machineschaalsets met Azure PowerShell

Gebruik de [Set AzVmssDiskEncryptionExtension](/powershell/module/az.compute/set-azvmssdiskencryptionextension) cmdlet schakelt u versleuteling op een virtuele-machineschaalset voor Windows. De resourcegroep, de virtuele machine en de sleutelkluis moeten al zijn gemaakt voor de vereisten.

-  **Versleutelen van een actieve virtuele-machineschaalset**:
      ```powershell
     $KVRGname = 'MyKeyVaultResourceGroup';
     $VMSSRGname = 'MyVMScaleSetResourceGroup';
      $VmssName = "MySecureVmss";
      $KeyVaultName= "MySecureVault";
      $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
      $DiskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
      $KeyVaultResourceId = $KeyVault.ResourceId;
      Set-AzVmssDiskEncryptionExtension -ResourceGroupName $VMSSRGname -VMScaleSetName $VmssName -VolumeType Data -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId;
      ```

-  **Versleutelen van een actieve virtuele-machineschaalset met behulp van de KEK-sleutel voor de sleutel inpakken**:
      ```powershell
     $KVRGname = 'MyKeyVaultResourceGroup';
     $VMSSRGname = 'MyVMScaleSetResourceGroup';
      $VmssName = "MySecureVmss";
      $KeyVaultName= "MySecureVault";
      $keyEncryptionKeyName = "MyKeyEncryptionKey";
      $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
      $DiskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
      $KeyVaultResourceId = $KeyVault.ResourceId;
      $KeyEncryptionKeyUrl = (Get-AzKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;
      Set-AzVmssDiskEncryptionExtension -ResourceGroupName $VMSSRGname -VMScaleSetName $VmssName -VolumeType Data -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl  -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $KeyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId;
      ```

    >[!NOTE]
    > De syntaxis voor de waarde van de schijf-versleuteling-keyvault-parameter is de volledige id-reeks: / subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br> De syntaxis voor de waarde van de encryptiesleutel parameter is de volledige URI naar de KEK-sleutel in: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

- **Versleutelingsstatus ophalen voor een virtuele machine**: Gebruik de [Get-AzVmssVMDiskEncryption](/powershell/module/az.compute/get-azvmssvmdiskencryption) cmdlet.
    
    ```powershell
    Get-AzVmssVMDiskEncryption -ResourceGroupName "MyVMScaleSetResourceGroup" -VMScaleSetName "MySecureVmss"
    ```

- **Schakel versleuteling uit op een virtuele-machineschaalset**: Gebruik de [uitschakelen AzVmssDiskEncryption](/powershell/module/az.compute/disable-azvmssdiskencryption) cmdlet. 

    ```powershell
    Disable-AzVmssDiskEncryption -ResourceGroupName "MyVMScaleSetResourceGroup" -VMScaleSetName "MySecureVmss"
    ```

### <a name="azure-resource-manager-templates-for-linux-virtual-machine-scale-sets"></a>Hiermee stelt u Azure Resource Manager-sjablonen voor virtuele-machineschaalset in Linux

Als u wilt versleutelen of ontsleutelen van virtuele-machineschaalset in Linux wordt ingesteld, gebruikt u de Azure Resource Manager-sjablonen en de onderstaande instructies:

- [Schakelt u versleuteling op een Linux VM-schaalset](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-vmss-linux)
- [Schakel versleuteling uit op een Linux VM-schaalset](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-vmss-linux)

     1. Klik op **Implementeren in Azure**.
     2. Vul de vereiste velden in en ga akkoord met de voorwaarden en bepalingen.
     3. Klik op **aankoop** om de sjabloon te implementeren.

## <a name="bkmk_EFA"> </a>EncryptFormatAll-functie gebruiken voor gegevensschijven op Linux IaaS-VM 's

De **EncryptFormatAll** parameter vermindert de tijd voor Linux-gegevensschijven moeten worden versleuteld. Partities aan bepaalde criteria voldoen worden, opgemaakt (met het huidige bestandssysteem). Ze gaat vervolgens terug naar waar voordat de uitvoering van de opdracht was worden gekoppeld. Als u uitsluiten van een gegevensschijf die voldoet aan de criteria wilt, kunt u het ontkoppelen voordat u de opdracht uitvoert.

 Na het uitvoeren van deze opdracht wordt alle stations die zijn gekoppeld eerder geformatteerd. En vervolgens de versleutelingslaag boven op het station nu leeg zijn gestart. Wanneer deze optie is geselecteerd, wordt de tijdelijke resource-schijf die is gekoppeld aan de virtuele machine eveneens versleuteld. Als de kortstondige schijf opnieuw wordt ingesteld, wordt deze opnieuw geformatteerd en opnieuw versleuteld voor de virtuele machine met de Azure Disk Encryption-oplossing in de eerstvolgende gelegenheid installeren.

>[!WARNING]
> EncryptFormatAll mag niet worden gebruikt wanneer er benodigde gegevens op gegevensvolumes van een virtuele machine. U kunt schijven uitsluiten van versleuteling door ze te ontkoppelen. U moet eerst de EncryptFormatAll eerst op een virtuele testmachine uitproberen, inzicht in de functie-parameter en de gevolgen voor de voordat u deze op de VM voor productie. De optie EncryptFormatAll maakt op de gegevensschijf en alle gegevens op verloren. Voordat u verdergaat, controleert u of schijven die u wilt uitsluiten correct ontkoppeld. </br></br>
 >Als u deze parameter instellen tijdens het bijwerken van instellingen voor codering, kan dit leiden tot een opnieuw opstarten voordat de daadwerkelijke versleuteling. In dit geval is het ook wilt verwijderen van de schijf die u niet opmaken van het fstab-bestand wilt. Op dezelfde manier moet u de partitie die u versleutelen-indeling naar het fstab-bestand wilt voordat u begint de coderingsbewerking toevoegen. 

### <a name="bkmk_EFACriteria"> </a> EncryptFormatAll criteria
De parameter gaat echter alle partities en worden deze versleuteld, zolang ze voldoen aan **alle** van de onderstaande criteria: 
- Is niet een hoofdmap/OS/opstartpartitie
- Is niet versleuteld
- Is geen BEK volume
- Is niet een RAID-volume
- Is niet een LVM-volume
- Is gekoppeld

Versleutel de schijven die het volume RAID- of LVM in plaats van het volume RAID- of LVM opstellen.

### <a name="bkmk_EFAPSH"> </a> Gebruik de parameter EncryptFormatAll met Azure CLI
Gebruik de [az vm encryption inschakelen](/cli/azure/vm/encryption#az-vm-encryption-enable) opdracht schakelt u versleuteling op een actieve IaaS-virtuele machine in Azure.

-  **Een actieve virtuele machine met behulp van EncryptFormatAll versleutelen:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --disk-encryption-keyvault "MySecureVault" --encrypt-format-all
     ```

### <a name="bkmk_EFAPSH"> </a> Gebruik de parameter EncryptFormatAll met een PowerShell-cmdlet
Gebruik de [Set AzVMDiskEncryptionExtension](/powershell/module/az.compute/set-azvmdiskencryptionextension) cmdlet met de parameter EncryptFormatAll. 

**Een actieve virtuele machine met behulp van EncryptFormatAll versleutelen:** Als u bijvoorbeeld het onderstaande script wordt uw variabelen geïnitialiseerd en wordt de cmdlet Set-AzVMDiskEncryptionExtension uitgevoerd met de parameter EncryptFormatAll. De resourcegroep, de virtuele machine en de sleutelkluis moeten al zijn gemaakt voor de vereisten. MyVirtualMachineResourceGroup, MySecureVM en MySecureVault vervangen door uw waarden.
  
```azurepowershell
$KVRGname = 'MyKeyVaultResourceGroup';
$VMRGName = 'MyVirtualMachineResourceGroup';
$vmName = 'MySecureVM';
$KeyVaultName = 'MySecureVault';
$KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
$diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
$KeyVaultResourceId = $KeyVault.ResourceId;

Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGName -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -EncryptFormatAll
```


### <a name="bkmk_EFALVM"> </a> Gebruik de parameter EncryptFormatAll met logische Volume Manager (LVM) 
U wordt aangeraden een LVM-op-crypt-installatie. Voor de volgende voorbeelden wordt vervangen door de apparaat-path en stel de volgende parameter wat aansluit op uw use-casescenario. Deze instelling kan als volgt worden uitgevoerd:

- De gegevensschijven waaruit de virtuele machine toevoegen.
- Indeling, koppelen en deze schijven toevoegen aan het fstab-bestand.

    1. Formatteer de nieuw toegevoegde schijf. We gebruiken symlinks die hier worden gegenereerd door Azure. Met behulp van symlinks voorkomt u problemen met betrekking tot apparaatnamen wijzigen. Zie voor meer informatie de [apparaatnamen oplossen van problemen](../virtual-machines/linux/troubleshoot-device-names-problems.md) artikel.
    
         `mkfs -t ext4 /dev/disk/azure/scsi1/lun0`
    
    2. Koppel de schijven.
         
         `mount /dev/disk/azure/scsi1/lun0 /mnt/mountpoint`
    
    3. Toevoegen aan fstab.
         
        `echo "/dev/disk/azure/scsi1/lun0 /mnt/mountpoint ext4 defaults,nofail 1 2" >> /etc/fstab`
    
    4. Voer de cmdlet Set-AzVMDiskEncryptionExtension PowerShell met - EncryptFormatAll voor het versleutelen van deze schijven.
         ```azurepowershell-interactive
         Set-AzVMDiskEncryptionExtension -ResourceGroupName "MySecureGroup" -VMName "MySecureVM" -DiskEncryptionKeyVaultUrl "https://mykeyvault.vault.azure.net/" -EncryptFormatAll
         ```
    5. LVM boven op deze nieuwe schijven instellen. Houd er rekening mee dat de versleutelde schijven worden ontgrendeld nadat de virtuele machine wordt opgestart is voltooid. Het LVM-koppelen heeft dus ook later worden vertraagd.


## <a name="bkmk_VHDpre"> </a> Nieuwe IaaS VM's die worden gemaakt op basis van de klant versleuteld VHD en versleuteling sleutels
In dit scenario kunt u inschakelen met behulp van PowerShell-cmdlets of de CLI-opdrachten te coderen. 

Volg de instructies in de bijlage voor het maken van vooraf gecodeerde-installatiekopieën die kunnen worden gebruikt in Azure. Nadat de installatiekopie is gemaakt, kunt u de stappen in de volgende sectie om een versleutelde Azure-VM te maken.

* [Vooraf gecodeerde Windows VHD voorbereiden](azure-security-disk-encryption-appendix.md#bkmk_preWin)
* [Vooraf gecodeerde Linux-VHD voorbereiden](azure-security-disk-encryption-appendix.md#bkmk_preLinux)

>[!IMPORTANT]
 >Dit is verplicht op momentopname en/of back-up een beheerde schijf op basis van de VM-exemplaar buiten en voordat Azure Disk Encryption wordt ingeschakeld. Een momentopname van de beheerde schijf kan worden uitgevoerd vanuit de portal of [Azure Backup](../backup/backup-azure-vms-encryption.md) kan worden gebruikt. Back-ups zorgen ervoor dat een optie voor siteherstel mogelijk in het geval van een onverwachte fout opgetreden tijdens het versleutelen is. Als u een back-up is gemaakt, kan de cmdlet Set-AzVMDiskEncryptionExtension worden gebruikt voor het versleutelen van beheerde schijven met de parameter - skipVmBackup op te geven. De opdracht Set-AzVMDiskEncryptionExtension, op basis van virtuele machines op beheerde schijven op basis van mislukken totdat een back-up is gemaakt en deze parameter is opgegeven. 
>
>Versleutelen of als u versleuteling uitschakelt kan ertoe leiden dat de virtuele machine opnieuw op te starten. 



### <a name="bkmk_VHDprePSH"> </a> Azure PowerShell gebruiken voor het versleutelen van IaaS-VM's met vooraf gecodeerde VHD 's 
U kunt schijfversleuteling op uw versleutelde VHD inschakelen met behulp van de PowerShell-cmdlet [Set AzVMOSDisk](/powershell/module/Az.Compute/Set-AzVMOSDisk#examples). Het onderstaande voorbeeld hebt u enkele algemene parameters. 

```azurepowershell
$VirtualMachine = New-AzVMConfig -VMName "MySecureVM" -VMSize "Standard_A1"
$VirtualMachine = Set-AzVMOSDisk -VM $VirtualMachine -Name "SecureOSDisk" -VhdUri "os.vhd" Caching ReadWrite -Windows -CreateOption "Attach" -DiskEncryptionKeyUrl "https://mytestvault.vault.azure.net/secrets/Test1/514ceb769c984379a7e0230bddaaaaaa" -DiskEncryptionKeyVaultId "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.KeyVault/vaults/mytestvault"
New-AzVM -VM $VirtualMachine -ResourceGroupName "MyVirtualMachineResourceGroup"
```

## <a name="enable-encryption-on-a-newly-added-data-disk"></a>Schakelt u versleuteling op een nieuw toegevoegde gegevensschijf

U kunt toevoegen met een nieuwe gegevens schijf met [az vm disk attach](../virtual-machines/linux/add-disk.md), of [via Azure portal](../virtual-machines/linux/attach-disk-portal.md). Voordat u coderen kunt, moet u eerst de zojuist gekoppelde gegevensschijf koppelen. U moet versleuteling van het gegevensstation aanvragen omdat het station onbruikbaar is terwijl versleuteling uitgevoerd wordt. 

### <a name="enable-encryption-on-a-newly-added-disk-with-azure-cli"></a>Schakelt u versleuteling op een nieuw toegevoegde schijf met Azure CLI

 Als de virtuele machine is versleuteld met 'All' vervolgens de--volumetype parameter alle moet blijven. Alle bevat zowel besturingssysteem en gegevensschijven. Als de virtuele machine is versleuteld met een met het volumetype 'BS', en vervolgens de--volumetype parameter moet worden gewijzigd in alle, zodat zowel het besturingssysteem en de nieuwe gegevensschijf opgenomen worden. Als de virtuele machine is versleuteld met alleen het volumetype van "Gegevens", kan het "Gegevens" blijven zoals hieronder wordt gedemonstreerd. Toe te voegen en een nieuwe gegevensschijf koppelen aan een virtuele machine is niet voldoende voorbereiding voor versleuteling. De zojuist gekoppelde schijf moet ook worden ingedeeld en correct is gekoppeld in de VM voordat versleuteling werd ingeschakeld. Op Linux de schijf moet worden gekoppeld in/etc/fstab met een [apparaatnaam permanente blokkeren](https://docs.microsoft.com/azure/virtual-machines/linux/troubleshoot-device-names-problems).  

In tegenstelling tot de Powershell-syntaxis vereist de CLI niet de gebruiker voor de versie van een unieke reeks bij het inschakelen van versleuteling. De CLI genereert automatisch en wordt de waarde van een eigen unieke reeks versie.

-  **Gegevensvolumes van een actieve virtuele machine versleutelen:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --disk-encryption-keyvault "MySecureVault" --volume-type "Data"
     ```

- **Gegevensvolumes van een actieve virtuele machine met behulp van de KEK versleutelen:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --disk-encryption-keyvault  "MySecureVault" --key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type "Data"
     ```

### <a name="enable-encryption-on-a-newly-added-disk-with-azure-powershell"></a>Schakelt u versleuteling op een nieuw toegevoegde schijf met Azure PowerShell
 Wanneer u Powershell gebruikt voor het versleutelen van een nieuwe schijf voor Linux, moet een nieuwe versie van de takenreeks worden opgegeven. De versie van de reeks moet uniek zijn. Het onderstaande script genereert een GUID voor de versie van de reeks. Duren voordat een [momentopname](../virtual-machines/windows/snapshot-copy-managed-disk.md) en/of back-up van de virtuele machine met [Azure Backup](../backup/backup-azure-vms-encryption.md) voordat de schijven worden versleuteld. De parameter - skipVmBackup is al opgegeven in de PowerShell-scripts voor het versleutelen van een toegevoegde gegevensschijf.
 

-  **Gegevensvolumes van een actieve virtuele machine versleutelen:** Het onderstaande script wordt uw variabelen geïnitialiseerd en wordt de cmdlet Set-AzVMDiskEncryptionExtension uitgevoerd. De resourcegroep, de virtuele machine en de sleutelkluis moeten al zijn gemaakt voor de vereisten. MyVirtualMachineResourceGroup, MySecureVM en MySecureVault vervangen door uw waarden. Acceptabele waarden voor de parameter - VolumeType zijn alle, OS- en gegevens. Als de virtuele machine is versleuteld met een volumetype 'BS' of 'Alle', moet klikt u vervolgens de parameter - VolumeType worden gewijzigd in alle zodat zowel het besturingssysteem en de nieuwe gegevensschijf opgenomen worden.

      ```azurepowershell
      $KVRGname = 'MyKeyVaultResourceGroup';
      $VMRGName = 'MyVirtualMachineResourceGroup';
      $vmName = 'MySecureVM';
      $KeyVaultName = 'MySecureVault';
      $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
      $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
      $KeyVaultResourceId = $KeyVault.ResourceId;
      $sequenceVersion = [Guid]::NewGuid();

      Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGName -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -VolumeType 'data' –SequenceVersion $sequenceVersion -skipVmBackup;
      ```
- **Gegevensvolumes van een actieve virtuele machine met behulp van de KEK versleutelen:** Acceptabele waarden voor de parameter - VolumeType zijn alle, OS- en gegevens. Als de virtuele machine is versleuteld met een volumetype 'BS' of 'Alle', moet klikt u vervolgens de parameter - VolumeType worden gewijzigd in alle zodat zowel het besturingssysteem en de nieuwe gegevensschijf opgenomen worden.

     ```azurepowershell
      $KVRGname = 'MyKeyVaultResourceGroup';
      $VMRGName = 'MyVirtualMachineResourceGroup';
      $vmName = 'MyExtraSecureVM';
      $KeyVaultName = 'MySecureVault';
      $keyEncryptionKeyName = 'MyKeyEncryptionKey';
      $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
      $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
      $KeyVaultResourceId = $KeyVault.ResourceId;
      $keyEncryptionKeyUrl = (Get-AzKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;
      $sequenceVersion = [Guid]::NewGuid();

      Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGName -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId -VolumeType 'data' –SequenceVersion $sequenceVersion -skipVmBackup;
     ```

    >[!NOTE]
    > De syntaxis voor de waarde van de schijf-versleuteling-keyvault-parameter is de volledige id-reeks: / subscriptions/[subscription-id-guid]/resourceGroups/[KVresource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br> De syntaxis voor de waarde van de encryptiesleutel parameter is de volledige URI naar de KEK-sleutel in: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 


## <a name="disable-encryption-for-linux-vms"></a>Versleuteling voor Linux-VM's uitschakelen
U kunt versleuteling met Azure PowerShell, de Azure CLI, uitschakelen of met een Resource Manager-sjabloon. 

>[!IMPORTANT]
>Versleuteling met Azure Disk Encryption voor virtuele Linux-machines uitschakelen wordt alleen ondersteund voor de gegevensvolumes. Het wordt niet ondersteund op de gegevens of besturingssysteemvolumes als het volume met het besturingssysteem is versleuteld.  

- **Schijfversleuteling met Azure PowerShell uitschakelen:** Als u wilt de versleuteling uitschakelen, gebruikt u de [uitschakelen AzVMDiskEncryption](/powershell/module/az.compute/disable-azvmdiskencryption) cmdlet. 
     ```azurepowershell-interactive
     Disable-AzVMDiskEncryption -ResourceGroupName 'MyVirtualMachineResourceGroup' -VMName 'MySecureVM' [-VolumeType {ALL, DATA, OS}]
     ```

- **Met de Azure CLI-versleuteling uitschakelen:** Als u wilt uitschakelen versleuteling, gebruikt u de [az vm encryption uitschakelen](/cli/azure/vm/encryption#az-vm-encryption-disable) opdracht. 
     ```azurecli-interactive
     az vm encryption disable --name "MySecureVM" --resource-group "MyVirtualMachineResourceGroup" --volume-type [ALL, DATA, OS]
     ```
- **Schakel versleuteling met Resource Manager-sjabloon:** Gebruik de [Schakel versleuteling uit op een actieve Linux VM](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-linux-vm-without-aad) sjabloon om versleuteling te schakelen.
     1. Klik op **Implementeren in Azure**.
     2. Selecteer het abonnement, resourcegroep, locatie, VM, juridische voorwaarden en -overeenkomst.
     3.  Klik op **aankoop** schijfversleuteling op een actieve Windows-virtuele machine uitschakelen. 


## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Schakel Azure Disk Encryption voor Windows](azure-security-disk-encryption-windows.md)
