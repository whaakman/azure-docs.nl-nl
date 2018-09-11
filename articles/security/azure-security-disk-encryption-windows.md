---
title: Schakel Azure Disk Encryption voor virtuele Windows IaaS-machines | Microsoft Docs
description: In dit artikel vindt u instructies over het inschakelen van Microsoft Azure Disk Encryption voor Windows IaaS-VM's.
author: mestew
ms.service: security
ms.subservice: Azure Disk Encryption
ms.topic: article
ms.author: mstewart
ms.date: 09/10/2018
ms.openlocfilehash: d827c2f0a53a1292dd3ba6e15015d9132a45441b
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44346301"
---
# <a name="enable-azure-disk-encryption-for-windows-iaas-vms"></a>Schakel Azure Disk Encryption voor virtuele Windows IaaS-machines 

U kunt veel schijfversleuteling scenario's inschakelen en de stappen kunnen variëren op basis van het scenario. De volgende secties voor de scenario's in meer detail voor Windows IaaS-VM's. Voordat u kunt schijfversleuteling, gebruiken de [vereisten voor Azure Disk Encryption](../security/azure-security-disk-encryption-prerequisites.md) moeten worden uitgevoerd. 

Duren voordat een [momentopname](../virtual-machines/windows/snapshot-copy-managed-disk.md) en/of back-up voordat de schijven worden versleuteld. Back-ups zorgen ervoor dat een optie voor siteherstel mogelijk als er een onverwachte fout optreedt tijdens het versleutelen. Virtuele machines met beheerde schijven moeten u een back-up voordat versleuteling plaatsvindt. Als u een back-up is gemaakt, kunt u de cmdlet Set-AzureRmVMDiskEncryptionExtension kunt gebruiken voor het versleutelen van beheerde schijven met de parameter - skipVmBackup op te geven. Zie voor meer informatie over hoe u een back-up en herstel van versleutelde virtuele machines, de [Azure Backup](../backup/backup-azure-vms-encryption.md) artikel. 

>[!WARNING]
> Azure Disk Encryption moet de Key Vault en de virtuele machines in dezelfde regio worden geplaatst. Maak en gebruik van een Key Vault die zich in dezelfde regio als de virtuele machine moeten worden versleuteld. 


## <a name="bkmk_RunningWinVM"></a> Schakelt u versleuteling op bestaande of het uitvoeren van IaaS-VM's voor Windows
In dit scenario kunt u versleuteling inschakelen met behulp van een sjabloon, de PowerShell-cmdlets of de CLI-opdrachten. De volgende secties wordt uitgelegd in meer detail Azure Disk Encryption inschakelen. Als u schema-informatie voor de extensie van de virtuele machine nodig hebt, raadpleegt u de [Azure Disk Encryption voor Windows-extensie](../virtual-machines/extensions/azure-disk-enc-windows.md) artikel.

>[!IMPORTANT]
 >Dit is verplicht op momentopname en/of back-up een beheerde schijf op basis van de VM-exemplaar buiten en voordat Azure Disk Encryption wordt ingeschakeld. Een momentopname van de beheerde schijf kan worden uitgevoerd vanuit de portal of [Azure Backup](../backup/backup-azure-vms-encryption.md) kan worden gebruikt. Back-ups zorgen ervoor dat een optie voor siteherstel mogelijk in het geval van een onverwachte fout opgetreden tijdens het versleutelen is. Als u een back-up is gemaakt, kan de cmdlet Set-AzureRmVMDiskEncryptionExtension worden gebruikt voor het versleutelen van beheerde schijven met de parameter - skipVmBackup op te geven. De opdracht Set-AzureRmVMDiskEncryptionExtension, op basis van virtuele machines op beheerde schijven op basis van mislukken totdat een back-up is gemaakt en deze parameter is opgegeven. 
>
>Versleutelen of als u versleuteling uitschakelt kan ertoe leiden dat de virtuele machine opnieuw op te starten. 
>

### <a name="bkmk_RunningWinVMPSH"></a> Schakelt u versleuteling op bestaande of virtuele machines uitvoeren met Azure PowerShell 
Gebruik de [Set-AzureRmVMDiskEncryptionExtension](/powershell/module/azurerm.compute/set-azurermvmdiskencryptionextension) cmdlet schakelt u versleuteling op een actieve IaaS-virtuele machine in Azure. 

-  **Een actieve virtuele machine versleutelen:** het onderstaande script wordt uw variabelen geïnitialiseerd en wordt de cmdlet Set-AzureRmVMDiskEncryptionExtension uitgevoerd. De resourcegroep, de virtuele machine en de sleutelkluis moeten al zijn gemaakt voor de vereisten. MySecureRg, MySecureVM en MySecureVault vervangen door uw waarden.

     ```azurepowershell-interactive
      $rgName = 'MySecureRg';
      $vmName = 'MySecureVM';
      $KeyVaultName = 'MySecureVault';
      $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
      $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
      $KeyVaultResourceId = $KeyVault.ResourceId;

      Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId;
    ```
- **Een actieve virtuele machine met behulp van de KEK versleutelen:** 

     ```azurepowershell-interactive
     $rgName = 'MySecureRg';
     $KeyVaultName = 'MySecureVault';
     $keyEncryptionKeyName = 'MyKeyEncryptionKey';
     $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
     $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     $keyEncryptionKeyUrl = (Get-AzureKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;

     Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId;

     ```
     
   >[!NOTE]
   > De syntaxis voor de waarde van de schijf-versleuteling-keyvault-parameter is de volledige id-reeks: / subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br> De syntaxis voor de waarde van de encryptiesleutel parameter is de volledige URI naar de KEK-sleutel in: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

- **Controleer of de schijven worden versleuteld:** gebruiken om te controleren op de status voor schijfversleuteling van een IaaS-VM, de [Get-AzureRmVmDiskEncryptionStatus](/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus) cmdlet. 
     ```azurepowershell-interactive
     Get-AzureRmVmDiskEncryptionStatus -ResourceGroupName 'MySecureRg' -VMName 'MySecureVM'
     ```
    
- **Schijfversleuteling uitschakelen:** als u wilt de versleuteling uitschakelen, gebruikt u de [Disable-AzureRmVMDiskEncryption](/powershell/module/azurerm.compute/disable-azurermvmdiskencryption) cmdlet. 
     ```azurepowershell-interactive
     Disable-AzureRmVMDiskEncryption -ResourceGroupName 'MySecureRG' -VMName 'MySecureVM'
     ```

### <a name="bkmk_RunningWinVMCLI"></a>Schakelt u versleuteling op bestaande of virtuele machines uitvoeren met Azure CLI
Gebruik de [az vm encryption inschakelen](/cli/azure/vm/encryption#az-vm-encryption-enable) opdracht schakelt u versleuteling op een actieve IaaS-virtuele machine in Azure.

-  **Een actieve virtuele machine versleutelen:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --disk-encryption-keyvault "MySecureVault" --volume-type [All|OS|Data]
     ```

- **Een actieve virtuele machine met behulp van de KEK versleutelen:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --disk-encryption-keyvault  "MySecureVault" --key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type [All|OS|Data]
     ```

     >[!NOTE]
     > De syntaxis voor de waarde van de schijf-versleuteling-keyvault-parameter is de volledige id-reeks: / subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name] </br> De syntaxis voor de waarde van de encryptiesleutel parameter is de volledige URI naar de KEK-sleutel in: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

- **Controleer of de schijven worden versleuteld:** gebruiken om te controleren op de status voor schijfversleuteling van een IaaS-VM, de [az vm encryption show](/cli/azure/vm/encryption#az-vm-encryption-show) opdracht. 

     ```azurecli-interactive
     az vm encryption show --name "MySecureVM" --resource-group "MySecureRg"
     ```

- **Schakel versleuteling uit:** als wilt uitschakelen versleuteling, gebruikt u de [az vm encryption uitschakelen](/cli/azure/vm/encryption#az-vm-encryption-disable) opdracht. 
     ```azurecli-interactive
     az vm encryption disable --name "MySecureVM" --resource-group "MySecureRg" --volume-type [ALL, DATA, OS]
     ```
 
 > [!NOTE]
 >Dit is verplicht op momentopname en/of back-up een beheerde schijf op basis van de VM-exemplaar buiten en voordat Azure Disk Encryption wordt ingeschakeld. Een momentopname van de beheerde schijf kan worden uitgevoerd vanuit de portal of [Azure Backup](../backup/backup-azure-vms-encryption.md) kan worden gebruikt. Back-ups zorgen ervoor dat een optie voor siteherstel mogelijk in het geval van een onverwachte fout opgetreden tijdens het versleutelen is. Als u een back-up is gemaakt, kan de cmdlet Set-AzureRmVMDiskEncryptionExtension worden gebruikt voor het versleutelen van beheerde schijven met de parameter - skipVmBackup op te geven. Met deze opdracht uitvoeren op basis van virtuele machines op beheerde schijven op basis van totdat een back-up is gemaakt en deze parameter is opgegeven. 
>
>Versleutelen of als u versleuteling uitschakelt kan ertoe leiden dat de virtuele machine opnieuw op te starten. 

### <a name="bkmk_RunningWinVMwRM"> </a>Met behulp van de Resource Manager-sjabloon
U kunt inschakelen schijfversleuteling voor de bestaande of IaaS Windows-VM's uitvoeren in Azure met behulp van de [Resource Manager-sjabloon voor het versleutelen van een actieve Windows-VM](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm-without-aad).


1. Klik op de Azure-quickstart-sjabloon, **implementeren in Azure**.

2. Selecteer het abonnement, resourcegroep, locatie, instellingen, juridische voorwaarden en -overeenkomst. Klik op **aankoop** versleuteling op de bestaande of actieve IaaS-VM inschakelen.

De volgende tabel bevat de Resource Manager-Sjabloonparameters voor bestaande of uitvoeren van virtuele machines:

| Parameter | Beschrijving |
| --- | --- |
| vmName | De naam van de virtuele machine om uit te voeren van de versleutelingsbewerking. |
| keyVaultName | De naam van de sleutelkluis waarin de BitLocker-sleutel moet worden geüpload naar. U kunt deze ophalen met behulp van de cmdlet `(Get-AzureRmKeyVault -ResourceGroupName <MyResourceGroupName>). Vaultname` of de Azure CLI-opdracht ' az keyvault list--resourcegroep 'MySecureGroup" |Convertfrom-JSON'|
| keyVaultResourceGroup | Naam van de resourcegroep waarin de key vault|
|  KeyEncryptionKeyURL | URL van de sleutel van versleutelingssleutel die wordt gebruikt voor het versleutelen van de gegenereerde BitLocker-sleutel. Deze parameter is optioneel als u selecteert **nokek** in de vervolgkeuzelijst UseExistingKek. Als u selecteert **kek** in de vervolgkeuzelijst UseExistingKek voert u de _keyEncryptionKeyURL_ waarde. |
| VolumeType | Het type volume dat de versleutelingsbewerking wordt uitgevoerd op. Geldige waarden zijn _OS_, _gegevens_, en _alle_. 
| forceUpdateTag | In een unieke waarde, zoals een GUID doorgeven telkens wanneer de bewerking moet worden geforceerd uitvoeren. |
| resizeOSDisk | Moet de OS-partitie worden kleiner gemaakt zodat ze volledig OS VHD voordat het systeemvolume splitsen in beslag nemen. |
| locatie | Locatie voor alle resources. |

## <a name="encrypt-virtual-machine-scale-sets"></a>Versleutelen van de virtuele-machineschaalsets
[Azure virtuele-machineschaalsets](../virtual-machine-scale-sets/overview.md) kunt u maken en beheren van een groep identieke, met gelijke taakverdeling VM's worden geladen. Het aantal VM-exemplaren kan automatisch toenemen of afnemen in reactie op vraag of een ingesteld schema. De CLI of Azure PowerShell gebruiken voor het versleutelen van virtuele-machineschaalsets.

###  <a name="encrypt-virtual-machine-scale-sets-with-azure-powershell"></a>Versleutelen van de virtuele-machineschaalsets met Azure PowerShell
Gebruik de [Set-AzureRmVmssDiskEncryptionExtension](/powershell/module/azurerm.compute/set-azurermvmssdiskencryptionextension) cmdlet schakelt u versleuteling op een virtuele-machineschaalset voor Windows.

-  **Versleutelen van een actieve virtuele-machineschaalset**:
    ```azurepowershell-interactive
     $rgName= "MySecureRg";
     $VmssName = "MySecureVmss";
     $KeyVaultName= "MySecureVault";
     $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgName;
     $DiskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     Set-AzureRmVmssDiskEncryptionExtension -ResourceGroupName $rgName -VMScaleSetName $VmssName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId;


-  **Encrypt a running virtual machine scale set using KEK to wrap the key**:
    ```azurepowershell-interactive
     $rgName= "MySecureRg";
     $VmssName = "MySecureVmss";
     $KeyVaultName= "MySecureVault";
     $keyEncryptionKeyName = "MyKeyEncryptionKey";
     $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgName;
     $DiskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     Set-AzureRmVmssDiskEncryptionExtension -ResourceGroupName $rgName -VMScaleSetName $VmssName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId;
    ```

- **Versleutelingsstatus voor een virtuele-machineschaalset ophalen:** gebruiken de [Get-AzureRmVmssVMDiskEncryption](/powershell/module/azurerm.compute/get-azurermvmssvmdiskencryption) cmdlet.
    
    ```azurepowershell-interactive
    get-AzureRmVmssVMDiskEncryption -ResourceGroupName "MySecureRG" -VMScaleSetName "MySecureVmss"
    ```

- **Schakel versleuteling uit op een virtuele-machineschaalset**: Gebruik de [Disable-AzureRmVmssDiskEncryption](/powershell/module/azurerm.compute/disable-azurermvmssdiskencryption) cmdlet. 

    ```azurepowershell-interactive
    Disable-AzureRmVmssDiskEncryption -ResourceGroupName "MySecureRG" -VMScaleSetName "MySecureVmss"
    ```
###  <a name="encrypt-virtual-machine-scale-sets-with-azure-cli"></a>Versleutelen van de virtuele-machineschaalsets met Azure CLI
Gebruik de [az vmss versleuteling inschakelen](/cli/azure/vmss/encryption#az-vmss-encryption-enable) versleuteling op een Windows-virtuele-machineschaalset inschakelen. Als u het beleid voor upgrades op de schaal die is ingesteld op handmatig hebt ingesteld, start u de codering met [az vmss update-instances](/cli/azure/vmss#az-vmss-update-instances). 

-  **Versleutelen van een actieve virtuele-machineschaalset**
    ```azurecli-interactive
     az vmss encryption enable --resource-group "MySecureRG" --name "MySecureVmss" --disk-encryption-keyvault "MySecureVault" 
    ```

-  **Versleutelen van een actieve virtuele-machineschaalset met behulp van de KEK-sleutel voor de sleutel Inpakken**
    ```azurecli-interactive
     az vmss encryption enable --resource-group "MySecureRG" --name "MySecureVmss" --disk-encryption-keyvault "MySecureVault" --key-encryption-key "MyKEK" --key-encryption-keyvault "MySecureVault" 

     ```
- **Versleutelingsstatus voor een virtuele-machineschaalset ophalen:** gebruik [az vmss encryption show](/cli/azure/vmss/encryption#az-vmss-encryption-show)

    ```azurecli-interactive
     az vmss encryption show --resource-group "MySecureRG" --name "MySecureVmss"
    ```

- **Schakel versleuteling uit op een virtuele-machineschaalset**: Gebruik [az vmss-versleuteling uitschakelen](/cli/azure/vmss/encryption#az-vmss-encryption-disable)
    ```azurecli-interactive
     az vmss encryption disable --resource-group "MySecureRG" --name "MySecureVmss"
    ```

## <a name="bkmk_VHDpre"> </a>Nieuwe IaaS VM's die worden gemaakt op basis van de klant versleuteld VHD en versleuteling sleutels
In dit scenario kunt u inschakelen met behulp van PowerShell-cmdlets of de CLI-opdrachten te coderen. 

Volg de instructies in de bijlage voor het maken van vooraf gecodeerde-installatiekopieën die kunnen worden gebruikt in Azure. Nadat de installatiekopie is gemaakt, kunt u de stappen in de volgende sectie om een versleutelde Azure-VM te maken.

* [Vooraf gecodeerde Windows VHD voorbereiden](azure-security-disk-encryption-appendix.md#bkmk_preWin)
* [Vooraf gecodeerde Linux-VHD voorbereiden](azure-security-disk-encryption-appendix.md#bkmk_preLinux)

>[!IMPORTANT]
 >Dit is verplicht op momentopname en/of back-up een beheerde schijf op basis van de VM-exemplaar buiten en voordat Azure Disk Encryption wordt ingeschakeld. Een momentopname van de beheerde schijf kan worden uitgevoerd vanuit de portal of [Azure Backup](../backup/backup-azure-vms-encryption.md) kan worden gebruikt. Back-ups zorgen ervoor dat een optie voor siteherstel mogelijk in het geval van een onverwachte fout opgetreden tijdens het versleutelen is. Als u een back-up is gemaakt, kan de cmdlet Set-AzureRmVMDiskEncryptionExtension worden gebruikt voor het versleutelen van beheerde schijven met de parameter - skipVmBackup op te geven. De opdracht Set-AzureRmVMDiskEncryptionExtension, op basis van virtuele machines op beheerde schijven op basis van mislukken totdat een back-up is gemaakt en deze parameter is opgegeven. 
>
>Versleutelen of als u versleuteling uitschakelt kan ertoe leiden dat de virtuele machine opnieuw op te starten. 


### <a name="bkmk_VHDprePSH"> </a> Versleutelen van VM's met vooraf gecodeerde VHD's met Azure PowerShell
U kunt schijfversleuteling op uw versleutelde VHD inschakelen met behulp van de PowerShell-cmdlet [Set-azurermvmosdisk,](/powershell/module/azurerm.compute/set-azurermvmosdisk#examples). Het onderstaande voorbeeld hebt u enkele algemene parameters. 

```azurepowershell-interactive
$VirtualMachine = New-AzureRmVMConfig -VMName "MySecureVM" -VMSize "Standard_A1"
$VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name "SecureOSDisk" -VhdUri "os.vhd" Caching ReadWrite -Windows -CreateOption "Attach" -DiskEncryptionKeyUrl "https://mytestvault.vault.azure.net/secrets/Test1/514ceb769c984379a7e0230bddaaaaaa" -DiskEncryptionKeyVaultId "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.KeyVault/vaults/mytestvault"
New-AzureRmVM -VM $VirtualMachine -ResouceGroupName "MySecureRG"
```

## <a name="enable-encryption-on-a-newly-added-data-disk"></a>Schakelt u versleuteling op een nieuw toegevoegde gegevensschijf
U kunt [een nieuwe schijf toevoegen aan een Windows-VM met behulp van PowerShell](../virtual-machines/windows/attach-disk-ps.md), of [via Azure portal](../virtual-machines/windows/attach-managed-disk-portal.md). 

### <a name="enable-encryption-on-a-newly-added-disk-with-azure-powershell"></a>Schakelt u versleuteling op een nieuw toegevoegde schijf met Azure PowerShell
 Wanneer u Powershell gebruikt voor het versleutelen van een nieuwe schijf voor Windows-VM's, kan een nieuwe versie van de takenreeks moet worden opgegeven. De versie van de reeks moet uniek zijn. Het onderstaande script genereert een GUID voor de versie van de reeks. In sommige gevallen kan een schijf met toegevoegde gegevens automatisch worden versleuteld door de Azure Disk Encryption-extensie. Als dit het geval is, wordt u aangeraden de cmdlet Set-AzureRmVmDiskEncryptionExtension nogmaals uit te voeren met de nieuwe versie van de reeks.
 

-  **Een actieve virtuele machine versleutelen:** het onderstaande script wordt uw variabelen geïnitialiseerd en wordt de cmdlet Set-AzureRmVMDiskEncryptionExtension uitgevoerd. De resourcegroep, de virtuele machine en de sleutelkluis moeten al zijn gemaakt voor de vereisten. MySecureRg, MySecureVM en MySecureVault vervangen door uw waarden. De parameter - VolumeType is ingesteld op gegevensschijven en niet de besturingssysteemschijf. 

     ```azurepowershell-interactive
      $sequenceVersion = [Guid]::NewGuid();
      $rgName = 'MySecureRg';
      $vmName = 'MySecureVM';
      $KeyVaultName = 'MySecureVault';
      $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
      $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
      $KeyVaultResourceId = $KeyVault.ResourceId;

      Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId  –SequenceVersion $sequenceVersion;
    ```
- **Een actieve virtuele machine met behulp van de KEK-sleutel te versleutelen:** mogelijk moet u de parameter - VolumeType toevoegen als u het versleutelen bent gegevensschijven en niet de besturingssysteemschijf. 

     ```azurepowershell-interactive
     $sequenceVersion = [Guid]::NewGuid();
     $rgName = 'MySecureRg';
     $vmName = 'MyExtraSecureVM';
     $KeyVaultName = 'MySecureVault';
     $keyEncryptionKeyName = 'MyKeyEncryptionKey';
     $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
     $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     $keyEncryptionKeyUrl = (Get-AzureKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;

     Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId –SequenceVersion $sequenceVersion;

     ```

    >[!NOTE]
    > De syntaxis voor de waarde van de schijf-versleuteling-keyvault-parameter is de volledige id-reeks: / subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br> De syntaxis voor de waarde van de encryptiesleutel parameter is de volledige URI naar de KEK-sleutel in: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

### <a name="enable-encryption-on-a-newly-added-disk-with-azure-cli"></a>Schakelt u versleuteling op een nieuw toegevoegde schijf met Azure CLI
 De Azure CLI-opdracht wordt automatisch een nieuwe versie van de reeks voor u opgeeft bij het uitvoeren van de opdracht voor het inschakelen van versleuteling. 
-  **Een actieve virtuele machine versleutelen:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --disk-encryption-keyvault "MySecureVault" --volume-type "Data"
     ```

- **Een actieve virtuele machine met behulp van de KEK versleutelen:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --disk-encryption-keyvault  "MySecureVault"--key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type "Data"
     ```


## <a name="disable-encryption"></a>Schakel versleuteling uit
U kunt versleuteling met Azure PowerShell, de Azure CLI, uitschakelen of met een Resource Manager-sjabloon. 

- **Schijfversleuteling met Azure PowerShell uitschakelen:** als u wilt de versleuteling uitschakelen, gebruikt u de [Disable-AzureRmVMDiskEncryption](/powershell/module/azurerm.compute/disable-azurermvmdiskencryption) cmdlet. 
     ```azurepowershell-interactive
     Disable-AzureRmVMDiskEncryption -ResourceGroupName 'MySecureRG' -VMName 'MySecureVM'
     ```

- **Met de Azure CLI-versleuteling uitschakelen:** als wilt uitschakelen versleuteling, gebruikt u de [az vm encryption uitschakelen](/cli/azure/vm/encryption#az-vm-encryption-disable) opdracht. 
     ```azurecli-interactive
     az vm encryption disable --name "MySecureVM" --resource-group "MySecureRg" --volume-type [ALL, DATA, OS]
     ```
- **Schakel versleuteling met Resource Manager-sjabloon:** 

    1. Klik op **implementeren in Azure** uit de [schijfversleuteling voor het uitvoeren van Windows-VM uitschakelen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-windows-vm-without-aad) sjabloon.
    2. Selecteer het abonnement, resourcegroep, locatie, VM, volumetype, juridische voorwaarden en -overeenkomst.
    3.  Klik op **aankoop** schijfversleuteling op een actieve Windows-virtuele machine uitschakelen. 

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Schakel Azure Disk Encryption voor Linux](azure-security-disk-encryption-linux.md)
