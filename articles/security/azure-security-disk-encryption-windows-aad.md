---
title: Azure Disk Encryption met Azure AD App Windows IaaS-VM's (vorige versie)
description: In dit artikel vindt u instructies over het inschakelen van Microsoft Azure Disk Encryption voor Windows IaaS-VM's.
author: mestew
ms.service: security
ms.topic: article
ms.author: mstewart
ms.date: 03/04/2019
ms.custom: seodec18
ms.openlocfilehash: 47310efc32f1fa8d691da21ba30eaccd379cf812
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57764367"
---
# <a name="enable-azure-disk-encryption-for-windows-iaas-vms-previous-release"></a>Azure Disk Encryption voor Windows IaaS-VM's (vorige versie) inschakelen

**De nieuwe versie van Azure Disk Encryption wordt voorkomen dat de vereiste voor het ontwikkelen van een Azure AD-toepassing-parameter voor schijfversleuteling van VM inschakelen. Met de nieuwe versie kunt u niet langer moet Azure AD-referenties opgeven tijdens de stap van de versleuteling inschakelen. Alle nieuwe virtuele machines moeten worden versleuteld zonder de parameters van Azure AD-toepassing met behulp van de nieuwe versie. Voor instructies voor het inschakelen van versleuteling van de VM-schijf met behulp van de nieuwe versie, raadpleegt u [Azure Disk Encryption voor Windows-VM's](azure-security-disk-encryption-windows.md). Virtuele machines die al zijn versleuteld met Azure AD-toepassing parameters worden nog steeds ondersteund en worden onderhouden met de syntaxis van de AAD moeten blijven.**


U kunt veel schijfversleuteling scenario's inschakelen en de stappen kunnen variëren op basis van het scenario. De volgende secties voor de scenario's in meer detail voor Windows IaaS-VM's. Voordat u kunt schijfversleuteling, gebruiken de [vereisten voor Azure Disk Encryption](../security/azure-security-disk-encryption-prerequisites-aad.md) moeten worden uitgevoerd. 

Duren voordat een [momentopname](../virtual-machines/windows/snapshot-copy-managed-disk.md) en/of back-up voordat de schijven worden versleuteld. Back-ups zorgen ervoor dat een optie voor siteherstel mogelijk als er een onverwachte fout optreedt tijdens het versleutelen. Virtuele machines met beheerde schijven moeten u een back-up voordat versleuteling plaatsvindt. Als u een back-up is gemaakt, kunt u de cmdlet Set-AzVMDiskEncryptionExtension kunt gebruiken voor het versleutelen van beheerde schijven met de parameter - skipVmBackup op te geven. Zie voor meer informatie over hoe u een back-up en herstel van versleutelde virtuele machines, de [Azure Backup](../backup/backup-azure-vms-encryption.md) artikel. 

>[!WARNING]
> - Als u eerder hebt gebruikt [Azure Disk Encryption met Azure AD-app](azure-security-disk-encryption-prerequisites-aad.md) voor het versleutelen van deze virtuele machine hebt uitgevoerd, om door te gaan met deze optie gebruiken voor het versleutelen van uw virtuele machine. U kunt geen gebruiken [Azure Disk Encryption](azure-security-disk-encryption-prerequisites.md) op deze versleutelde VM als dit niet een ondersteund scenario betekenis overschakelen van AAD-toepassing voor deze virtuele machine versleuteld wordt niet ondersteund nog.
> - Om te verzekeren dat de geheimen van de versleuteling niet overschreden door de regionale grenzen, moet Azure Disk Encryption de Key Vault en de virtuele machines in dezelfde regio worden geplaatst. Maak en gebruik van een Key Vault die zich in dezelfde regio als de virtuele machine moeten worden versleuteld. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="enable-encryption-on-new-iaas-vms-created-from-the-marketplace"></a>Schakelt u versleuteling op nieuwe IaaS VM's die worden gemaakt op basis van de Marketplace
Schijfversleuteling voor de nieuwe Windows van IaaS-VM vanuit de Marketplace in Azure met behulp van Resource Manager-sjabloon, kunt u inschakelen. De sjabloon maakt een nieuwe versleutelde Windows VM met behulp van de galerie met Windows Server 2012.

1. Op de [Resource Manager-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image), klikt u op **implementeren in Azure**.

2. Selecteer het abonnement, resourcegroep, locatie voor resourcegroep, parameters, juridische voorwaarden en -overeenkomst. Klik op **aankoop** implementeren van een nieuwe IaaS VM waarvoor versleuteling is ingeschakeld.

3. Nadat u de sjabloon implementeert, controleert u of de status van de VM-versleuteling met behulp van de gewenste manier:
     - Controleren met de Azure CLI met behulp van de [az vm encryption show](/cli/azure/vm/encryption#az-vm-encryption-show) opdracht. 

         ```azurecli-interactive 
         az vm encryption show --name "MySecureVM" --resource-group "MyVirtualMachineResourceGroup"
         ```

     - Met Azure PowerShell controleren met behulp van de [Get-AzVmDiskEncryptionStatus](/powershell/module/az.compute/get-azvmdiskencryptionstatus) cmdlet. 

         ```azurepowershell-interactive
         Get-AzVmDiskEncryptionStatus -ResourceGroupName 'MyVirtualMachineResourceGroup' -VMName 'MySecureVM'
         ```

     -  Selecteer de virtuele machine en klik vervolgens op **schijven** onder de **instellingen** kop om te controleren of de status voor schijfversleuteling in de portal. In de grafiek onder **versleuteling**, ziet u of deze ingeschakeld. 
           ![Azure portal - schijf versleuteling ingeschakeld](./media/azure-security-disk-encryption/disk-encryption-fig2.png) de volgende tabel bevat de parameters van de Resource Manager-sjabloon voor nieuwe virtuele machines uit de Marketplace-scenario met behulp van Azure AD-client-ID:

| Parameter | Description | 
| --- | --- |
| adminUserName | De beheerdersnaam voor de virtuele machine. |
| adminPassword | Beheerderswachtwoord voor de virtuele machine. |
| newStorageAccountName | De naam van het opslagaccount voor het opslaan van besturingssysteem- en VHD's. |
| vmSize | Grootte van de virtuele machine. Op dit moment worden alleen Standard A, D en G-serie ondersteund. |
| virtualNetworkName | De naam van het VNet waarmee de VM NIC tot behoren moet. |
| subnetName | De naam van het subnet in het VNet waarmee de VM NIC tot behoren moet. |
| AADClientID | Client-ID van de Azure AD-toepassing die gemachtigd is te schrijven geheimen voor uw key vault. |
| AADClientSecret | Clientgeheim van de Azure AD-toepassing met machtigingen voor het schrijven van geheimen voor uw key vault. |
| keyVaultURL | URL van de sleutelkluis waarin de BitLocker-sleutel moet worden geüpload naar. U kunt deze ophalen met behulp van de cmdlet `(Get-AzKeyVault -VaultName "MyKeyVault" -ResourceGroupName "MyKeyVaultResourceGroupName").VaultURI` of Azure CLI `az keyvault show --name "MySecureVault" --query properties.vaultUri` |
| KeyEncryptionKeyURL | URL van de sleutel van versleutelingssleutel die wordt gebruikt voor het versleutelen van de gegenereerde BitLocker-sleutel (optioneel). </br> </br>KeyEncryptionKeyURL is een optionele parameter. U brengen uw eigen KEK naar verder beschermen de versleutelingssleutel voor servicegegevens (geheime wachtwoordzin) in uw key vault. |
| keyVaultResourceGroup | De resourcegroep van de key vault. |
| vmName | De naam van de virtuele machine die de versleutelingsbewerking moet worden uitgevoerd op. |


## <a name="bkmk_RunningWinVM"></a> Schakelt u versleuteling op bestaande of het uitvoeren van IaaS-VM's voor Windows
In dit scenario kunt u versleuteling inschakelen met behulp van een sjabloon, de PowerShell-cmdlets of de CLI-opdrachten. De volgende secties wordt uitgelegd in meer detail Azure Disk Encryption inschakelen. 

>[!IMPORTANT]
 >Dit is verplicht op momentopname en/of back-up een beheerde schijf op basis van de VM-exemplaar buiten en voordat Azure Disk Encryption wordt ingeschakeld. Een momentopname van de beheerde schijf kan worden uitgevoerd vanuit de portal of [Azure Backup](../backup/backup-azure-vms-encryption.md) kan worden gebruikt. Back-ups zorgen ervoor dat een optie voor siteherstel mogelijk in het geval van een onverwachte fout opgetreden tijdens het versleutelen is. Als u een back-up is gemaakt, kan de cmdlet Set-AzVMDiskEncryptionExtension worden gebruikt voor het versleutelen van beheerde schijven met de parameter - skipVmBackup op te geven. De opdracht Set-AzVMDiskEncryptionExtension, op basis van virtuele machines op beheerde schijven op basis van mislukken totdat een back-up is gemaakt en deze parameter is opgegeven. 
>
>Versleutelen of als u versleuteling uitschakelt kan ertoe leiden dat de virtuele machine opnieuw op te starten. 
>

### <a name="bkmk_RunningWinVMPSH"></a> Schakelt u versleuteling op bestaande of virtuele machines uitvoeren met Azure PowerShell 
Gebruik de [Set AzVMDiskEncryptionExtension](/powershell/module/az.compute/set-azvmdiskencryptionextension) cmdlet schakelt u versleuteling op een actieve IaaS-virtuele machine in Azure. Zie voor informatie over het inschakelen van versleuteling met Azure Disk Encryption met behulp van PowerShell-cmdlets, de blogberichten [Azure Disk Encryption verkennen met Azure PowerShell - deel 1](https://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) en [Azure Disk Encryption verkennen met Azure PowerShell - deel 2](https://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx).

-  **Een actieve virtuele machine met behulp van een clientgeheim versleutelen:** Het onderstaande script wordt uw variabelen geïnitialiseerd en wordt de cmdlet Set-AzVMDiskEncryptionExtension uitgevoerd. De resourcegroep, de VM, de sleutelkluis, de AAD-app en de clientgeheim moeten al zijn gemaakt voor de vereisten. Vervang MyKeyVaultResourceGroup, MyVirtualMachineResourceGroup, MySecureVM, MySecureVault, Mijn-AAD-client-ID en Mijn-AAD-client-secret door uw waarden.
     ```azurepowershell
      $KVRGname = 'MyKeyVaultResourceGroup';
      $VMRGName = 'MyVirtualMachineResourceGroup';
      $vmName = 'MySecureVM';
      $aadClientID = 'My-AAD-client-ID';
      $aadClientSecret = 'My-AAD-client-secret';
      $KeyVaultName = 'MySecureVault';
      $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
      $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
      $KeyVaultResourceId = $KeyVault.ResourceId;

      Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGName -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId;
    ```
- **Versleutelen van een actieve virtuele machine met behulp van de KEK het inpakken van het clientgeheim:** Azure Disk Encryption kunt u een bestaande sleutel opgeven in uw key vault voor het verpakken van schijf encryption geheimen die zijn gegenereerd tijdens het inschakelen van versleuteling. Wanneer een sleutel van versleutelingssleutel is opgegeven, gebruikt Azure Disk Encryption die sleutel het verpakken van de geheimen van de versleuteling voor het schrijven naar de Key Vault. 

     ```azurepowershell
     $KVRGname = 'MyKeyVaultResourceGroup';
     $VMRGName = 'MyVirtualMachineResourceGroup';
     $vmName = ‘MyExtraSecureVM’;
     $aadClientID = 'My-AAD-client-ID';
     $aadClientSecret = 'My-AAD-client-secret';
     $KeyVaultName = 'MySecureVault';
     $keyEncryptionKeyName = 'MyKeyEncryptionKey';
     $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
     $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     $keyEncryptionKeyUrl = (Get-AzureKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;

     Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGname -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId;

     ```
     
   >[!NOTE]
   > De syntaxis voor de waarde van de schijf-versleuteling-keyvault-parameter is de volledige id-reeks: / subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br> De syntaxis voor de waarde van de encryptiesleutel parameter is de volledige URI naar de KEK-sleutel in: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

- **Controleer of dat de schijven worden versleuteld:** Als u wilt controleren op de status voor schijfversleuteling van een IaaS-VM, gebruikt u de [Get-AzVmDiskEncryptionStatus](/powershell/module/az.compute/get-azvmdiskencryptionstatus) cmdlet. 
     ```azurepowershell-interactive
     Get-AzVmDiskEncryptionStatus -ResourceGroupName 'MyVirtualMachineResourceGroup' -VMName 'MySecureVM'
     ```
    
- **Schijfversleuteling uitschakelen:** Als u wilt de versleuteling uitschakelen, gebruikt u de [Disable-AzureRmVMDiskEncryption](/powershell/module/az.compute/disable-azvmdiskencryption) cmdlet. 
     ```azurepowershell-interactive
     Disable-AzVMDiskEncryption -ResourceGroupName 'MyVirtualMachineResourceGroup' -VMName 'MySecureVM'
     ```

### <a name="bkmk_RunningWinVMCLI"></a>Schakelt u versleuteling op bestaande of virtuele machines uitvoeren met Azure CLI
Gebruik de [az vm encryption inschakelen](/cli/azure/vm/encryption#az-vm-encryption-enable) opdracht schakelt u versleuteling op een actieve IaaS-virtuele machine in Azure.

-  **Een actieve virtuele machine met behulp van een clientgeheim versleutelen:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --aad-client-id "<my spn created with CLI/my Azure AD ClientID>"  --aad-client-secret "My-AAD-client-secret" --disk-encryption-keyvault "MySecureVault" --volume-type [All|OS|Data]
     ```

- **Versleutelen van een actieve virtuele machine met behulp van de KEK het inpakken van het clientgeheim:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --aad-client-id "<my spn created with CLI which is the Azure AD ClientID>"  --aad-client-secret "My-AAD-client-secret" --disk-encryption-keyvault  "MySecureVault" --key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type [All|OS|Data]
     ```

     >[!NOTE]
     > De syntaxis voor de waarde van de schijf-versleuteling-keyvault-parameter is de volledige id-reeks: / subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name] </br> De syntaxis voor de waarde van de encryptiesleutel parameter is de volledige URI naar de KEK-sleutel in: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

- **Controleer of dat de schijven worden versleuteld:** Als u wilt controleren op de status voor schijfversleuteling van een IaaS-VM, gebruikt u de [az vm encryption show](/cli/azure/vm/encryption#az-vm-encryption-show) opdracht. 

     ```azurecli-interactive
     az vm encryption show --name "MySecureVM" --resource-group "MyVirtualMachineResourceGroup"
     ```

- **Schakel versleuteling uit:** Als u wilt uitschakelen versleuteling, gebruikt u de [az vm encryption uitschakelen](/cli/azure/vm/encryption#az-vm-encryption-disable) opdracht. 
     ```azurecli-interactive
     az vm encryption disable --name "MySecureVM" --resource-group "MyVirtualMachineResourceGroup" --volume-type [ALL, DATA, OS]
     ```
 
 > [!NOTE]
 >Dit is verplicht op momentopname en/of back-up een beheerde schijf op basis van de VM-exemplaar buiten en voordat Azure Disk Encryption wordt ingeschakeld. Een momentopname van de beheerde schijf kan worden uitgevoerd vanuit de portal of [Azure Backup](../backup/backup-azure-vms-encryption.md) kan worden gebruikt. Back-ups zorgen ervoor dat een optie voor siteherstel mogelijk in het geval van een onverwachte fout opgetreden tijdens het versleutelen is. Als u een back-up is gemaakt, kan de cmdlet Set-AzVMDiskEncryptionExtension worden gebruikt voor het versleutelen van beheerde schijven met de parameter - skipVmBackup op te geven. Met deze opdracht uitvoeren op basis van virtuele machines op beheerde schijven op basis van totdat een back-up is gemaakt en deze parameter is opgegeven. 
>
>Versleutelen of als u versleuteling uitschakelt kan ertoe leiden dat de virtuele machine opnieuw op te starten. 

### <a name="bkmk_RunningWinVMwRM"> </a>Met behulp van de Resource Manager-sjabloon
U kunt inschakelen schijfversleuteling voor de bestaande of IaaS Windows-VM's uitvoeren in Azure met behulp van de [Resource Manager-sjabloon voor het versleutelen van een actieve Windows-VM](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm).


1. Klik op de Azure-quickstart-sjabloon, **implementeren in Azure**.

2. Selecteer het abonnement, resourcegroep, locatie voor resourcegroep, parameters, juridische voorwaarden en -overeenkomst. Klik op **aankoop** versleuteling op de bestaande of actieve IaaS-VM inschakelen.

De volgende tabel bevat de Resource Manager-Sjabloonparameters voor bestaande of uitvoeren van virtuele machines die gebruikmaken van een Azure AD-client-ID:

| Parameter | Description |
| --- | --- |
| AADClientID | Client-ID van de Azure AD-toepassing met machtigingen voor geheimen schrijven naar de key vault. |
| AADClientSecret | Clientgeheim van de Azure AD-toepassing met machtigingen voor geheimen schrijven naar de key vault. |
| keyVaultName | De naam van de sleutelkluis waarin de BitLocker-sleutel moet worden geüpload naar. U kunt deze ophalen met behulp van de cmdlet `(Get-AzKeyVault -ResourceGroupName <MyKeyVaultResourceGroupName>). Vaultname` of de Azure CLI-opdracht ' az keyvault list--resourcegroep 'MySecureGroup" |Convertfrom-JSON'|
|  KeyEncryptionKeyURL | URL van de sleutel van versleutelingssleutel die wordt gebruikt voor het versleutelen van de gegenereerde BitLocker-sleutel. Deze parameter is optioneel als u selecteert **nokek** in de vervolgkeuzelijst UseExistingKek. Als u selecteert **kek** in de vervolgkeuzelijst UseExistingKek voert u de _keyEncryptionKeyURL_ waarde. |
| VolumeType | Het type volume dat de versleutelingsbewerking wordt uitgevoerd op. Geldige waarden zijn _OS_, _gegevens_, en _alle_. |
| SequenceVersion | De versie van de volgorde van de BitLocker-bewerking. Dit versienummer verhoogd telkens wanneer een schijfversleuteling bewerking wordt uitgevoerd op dezelfde VM. |
| vmName | De naam van de virtuele machine die de versleutelingsbewerking moet worden uitgevoerd op. |


## <a name="bkmk_VHDpre"> </a>Nieuwe IaaS VM's die worden gemaakt op basis van de klant versleuteld VHD en versleuteling sleutels
In dit scenario kunt u inschakelen met behulp van de Resource Manager-sjabloon, de PowerShell-cmdlets of de CLI-opdrachten te coderen. De volgende secties wordt uitgelegd in meer detail de Resource Manager-sjabloon en de CLI-opdrachten. 

Volg de instructies in de bijlage voor het maken van vooraf gecodeerde-installatiekopieën die kunnen worden gebruikt in Azure. Nadat de installatiekopie is gemaakt, kunt u de stappen in de volgende sectie om een versleutelde Azure-VM te maken.

* [Vooraf gecodeerde Windows VHD voorbereiden](azure-security-disk-encryption-appendix.md#bkmk_preWin)
* [Vooraf gecodeerde Linux-VHD voorbereiden](azure-security-disk-encryption-appendix.md#bkmk_preLinux)

>[!IMPORTANT]
 >Dit is verplicht op momentopname en/of back-up een beheerde schijf op basis van de VM-exemplaar buiten en voordat Azure Disk Encryption wordt ingeschakeld. Een momentopname van de beheerde schijf kan worden uitgevoerd vanuit de portal of [Azure Backup](../backup/backup-azure-vms-encryption.md) kan worden gebruikt. Back-ups zorgen ervoor dat een optie voor siteherstel mogelijk in het geval van een onverwachte fout opgetreden tijdens het versleutelen is. Als u een back-up is gemaakt, kan de cmdlet Set-AzVMDiskEncryptionExtension worden gebruikt voor het versleutelen van beheerde schijven met de parameter - skipVmBackup op te geven. De opdracht Set-AzVMDiskEncryptionExtension, op basis van virtuele machines op beheerde schijven op basis van mislukken totdat een back-up is gemaakt en deze parameter is opgegeven. 
>
>Versleutelen of als u versleuteling uitschakelt kan ertoe leiden dat de virtuele machine opnieuw op te starten. 



### <a name="bkmk_VHDprePSH"> </a> Versleutelen van VM's met vooraf gecodeerde VHD's met Azure PowerShell
U kunt schijfversleuteling op uw versleutelde VHD inschakelen met behulp van de PowerShell-cmdlet [Set AzVMOSDisk](/powershell/module/az.compute/set-azvmosdisk#examples). Het onderstaande voorbeeld hebt u enkele algemene parameters. 

```powershell
$VirtualMachine = New-AzVMConfig -VMName "MySecureVM" -VMSize "Standard_A1"
$VirtualMachine = Set-AzVMOSDisk -VM $VirtualMachine -Name "SecureOSDisk" -VhdUri "os.vhd" Caching ReadWrite -Windows -CreateOption "Attach" -DiskEncryptionKeyUrl "https://mytestvault.vault.azure.net/secrets/Test1/514ceb769c984379a7e0230bddaaaaaa" -DiskEncryptionKeyVaultId "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myKVresourcegroup/providers/Microsoft.KeyVault/vaults/mytestvault"
New-AzVM -VM $VirtualMachine -ResourceGroupName "MyVirtualMachineResourceGroup"
```


### <a name="bkmk_VHDpreRM"> </a>Versleutelen van IaaS-VM's met vooraf gecodeerde VHD's met een Resource Manager-sjabloon 
U kunt schijfversleuteling op uw versleutelde VHD inschakelen met behulp van de [Resource Manager-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-pre-encrypted-vm).

1. Klik op de Azure-quickstart-sjabloon, **implementeren in Azure**.

2. Selecteer het abonnement, resourcegroep, locatie voor resourcegroep, parameters, juridische voorwaarden en -overeenkomst. Klik op **maken** versleuteling op de nieuwe IaaS-VM inschakelen.

De volgende tabel bevat de parameters van de Resource Manager-sjabloon voor uw versleutelde VHD:

| Parameter | Description |
| --- | --- |
| newStorageAccountName | De naam van het opslagaccount voor het opslaan van de versleutelde OS VHD. Dit opslagaccount moet al zijn gemaakt in dezelfde resourcegroep en dezelfde locatie als de virtuele machine. |
| osVhdUri | De URI van de VHD met het besturingssysteem van het opslagaccount. |
| besturingssysteemtype | Type besturingssysteem product (Windows/Linux). |
| virtualNetworkName | De naam van het VNet waarmee de VM NIC tot behoren moet. De naam moet al zijn gemaakt in dezelfde resourcegroep en dezelfde locatie als de virtuele machine. |
| subnetName | Naam van het subnet in het VNet waarmee de VM NIC tot behoren moet. |
| vmSize | Grootte van de virtuele machine. Op dit moment worden alleen Standard A, D en G-serie ondersteund. |
| keyVaultResourceID | De ResourceID die de sleutelkluis-resource in Azure Resource Manager. U kunt deze ophalen met behulp van de PowerShell-cmdlet `(Get-AzKeyVault -VaultName "MyKeyVaultName"; -ResourceGroupName "MyKeyVaultResourceGroupName").ResourceId` of met behulp van de Azure CLI-opdracht `az keyvault show --name "MySecureVault" --query id`|
| keyVaultSecretUrl | URL van de schijf-versleutelingssleutel op die ingesteld in de key vault. |
| keyVaultKekUrl | URL van de sleutel van versleutelingssleutel voor het versleutelen van de gegenereerde versleutelingssleutel van de schijf. |
| vmName | De naam van de IaaS-VM. |

## <a name="enable-encryption-on-a-newly-added-data-disk"></a>Schakelt u versleuteling op een nieuw toegevoegde gegevensschijf
U kunt [een nieuwe schijf toevoegen aan een Windows-VM met behulp van PowerShell](../virtual-machines/windows/attach-disk-ps.md), of [via Azure portal](../virtual-machines/windows/attach-managed-disk-portal.md). 

### <a name="enable-encryption-on-a-newly-added-disk-with-azure-powershell"></a>Schakelt u versleuteling op een nieuw toegevoegde schijf met Azure PowerShell
 Wanneer u Powershell gebruikt voor het versleutelen van een nieuwe schijf voor Windows-VM's, kan een nieuwe versie van de takenreeks moet worden opgegeven. De versie van de reeks moet uniek zijn. Het onderstaande script genereert een GUID voor de versie van de reeks. In sommige gevallen kan een schijf met toegevoegde gegevens automatisch worden versleuteld door de Azure Disk Encryption-extensie. Automatische versleuteling treedt meestal op wanneer de virtuele machine opnieuw wordt opgestart nadat de nieuwe schijf online komt. Dit wordt meestal veroorzaakt doordat 'Alle' is opgegeven voor het volumetype wanneer schijfversleuteling is eerder is uitgevoerd op de virtuele machine. Als automatische versleuteling in een nieuw toegevoegde gegevensschijf plaatsvindt, wordt u aangeraden de cmdlet Set-AzVmDiskEncryptionExtension nogmaals uit te voeren met de nieuwe versie van de reeks. Als uw nieuwe gegevensschijf automatisch versleuteld is en u niet wenst dat moeten worden versleuteld, alle stations eerst ontsleutelen en opnieuw versleutelen met een nieuwe reeks versie besturingssysteem op te geven voor het volumetype. 
 

-  **Een actieve virtuele machine met behulp van een clientgeheim versleutelen:** Het onderstaande script wordt uw variabelen geïnitialiseerd en wordt de cmdlet Set-AzVMDiskEncryptionExtension uitgevoerd. De resourcegroep, de VM, de sleutelkluis, de AAD-app en de clientgeheim moeten al zijn gemaakt voor de vereisten. Vervang MyKeyVaultResourceGroup, MyVirtualMachineResourceGroup, MySecureVM, MySecureVault, Mijn-AAD-client-ID en Mijn-AAD-client-secret door uw waarden. In dit voorbeeld maakt gebruik van 'Alle' voor de parameter - VolumeType, waaronder zowel besturingssysteem en volumes. Als u wilt dat alleen voor het versleutelen van het volume met het besturingssysteem, gebruikt u 'BS' voor de parameter - VolumeType. 

     ```azurepowershell
      $sequenceVersion = [Guid]::NewGuid();
      $KVRGname = 'MyKeyVaultResourceGroup';
      $VMRGName = 'MyVirtualMachineResourceGroup';
      $vmName = 'MySecureVM';
      $aadClientID = 'My-AAD-client-ID';
      $aadClientSecret = 'My-AAD-client-secret';
      $KeyVaultName = 'MySecureVault';
      $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
      $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
      $KeyVaultResourceId = $KeyVault.ResourceId;

      Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGname -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -VolumeType 'all' –SequenceVersion $sequenceVersion;
    ```
- **Versleutelen van een actieve virtuele machine met behulp van de KEK het inpakken van het clientgeheim:** Azure Disk Encryption kunt u een bestaande sleutel opgeven in uw key vault voor het verpakken van schijf encryption geheimen die zijn gegenereerd tijdens het inschakelen van versleuteling. Wanneer een sleutel van versleutelingssleutel is opgegeven, gebruikt Azure Disk Encryption die sleutel het verpakken van de geheimen van de versleuteling voor het schrijven naar de Key Vault. In dit voorbeeld maakt gebruik van 'Alle' voor de parameter - VolumeType, waaronder zowel besturingssysteem en volumes. Als u wilt dat alleen voor het versleutelen van het volume met het besturingssysteem, gebruikt u 'BS' voor de parameter - VolumeType. 

     ```azurepowershell
     $sequenceVersion = [Guid]::NewGuid();
     $KVRGname = 'MyKeyVaultResourceGroup';
     $VMRGName = 'MyVirtualMachineResourceGroup';
     $vmName = 'MyExtraSecureVM';
     $aadClientID = 'My-AAD-client-ID';
     $aadClientSecret = 'My-AAD-client-secret';
     $KeyVaultName = 'MySecureVault';
     $keyEncryptionKeyName = 'MyKeyEncryptionKey';
     $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
     $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     $keyEncryptionKeyUrl = (Get-AzureKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;

     Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGname -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId -VolumeType 'all' –SequenceVersion $sequenceVersion;

     ```

    >[!NOTE]
    > De syntaxis voor de waarde van de schijf-versleuteling-keyvault-parameter is de volledige id-reeks: / subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br> De syntaxis voor de waarde van de encryptiesleutel parameter is de volledige URI naar de KEK-sleutel in: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

### <a name="enable-encryption-on-a-newly-added-disk-with-azure-cli"></a>Schakelt u versleuteling op een nieuw toegevoegde schijf met Azure CLI
  De Azure CLI-opdracht wordt automatisch een nieuwe versie van de reeks voor u opgeeft bij het uitvoeren van de opdracht voor het inschakelen van versleuteling. Acceptabele waarden voor de parameter volume yype zijn alle, OS- en gegevens. Mogelijk moet u de parameter type volume wijzigen in besturingssysteem of als u slechts één type schijf voor de virtuele machine versleutelen bent. 'Alle' in de voorbeelden gebruiken voor de parameter volumetype. 

-  **Een actieve virtuele machine met behulp van een clientgeheim versleutelen:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --aad-client-id "<my spn created with CLI/my Azure AD ClientID>"  --aad-client-secret "My-AAD-client-secret" --disk-encryption-keyvault "MySecureVault" --volume-type "All"
     ```

- **Versleutelen van een actieve virtuele machine met behulp van de KEK het inpakken van het clientgeheim:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --aad-client-id "<my spn created with CLI which is the Azure AD ClientID>"  --aad-client-secret "My-AAD-client-secret" --disk-encryption-keyvault  "MySecureVault" --key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type "all"
     ```


## <a name="enable-encryption-using-azure-ad-client-certificate-based-authentication"></a>Azure AD op basis van certificaten met clientverificatie via versleuteling inschakelen.
U kunt verificatie van clientcertificaten kunt gebruiken, met of zonder KEK-sleutel. De scripts vereisen dat [vereisten voor Azure Disk Encryption](azure-security-disk-encryption-prerequisites-aad.md) zijn voltooid. Voordat u de PowerShell-scripts gebruikt, moet u het certificaat is geüpload naar de sleutelkluis en geïmplementeerd voor de virtuele machine al hebt. Als u KEK-sleutel te gebruiken, worden de KEK-sleutel moet al bestaan. Zie voor meer informatie de [verificatie op basis van certificaten voor Azure AD](azure-security-disk-encryption-prerequisites-aad.md#bkmk_Cert) gedeelte van het artikel vereisten.


### <a name="enable-encryption-using-certificate-based-authentication-with-azure-powershell"></a>Met behulp van certificaten gebaseerde verificatie met Azure PowerShell-codering inschakelen

```powershell
## Fill in 'MyVirtualMachineResourceGroup', 'MyKeyVaultResourceGroup', 'My-AAD-client-ID', 'MySecureVault, and ‘MySecureVM’.

$VMRGName = 'MyVirtualMachineResourceGroup'
$KVRGname = 'MyKeyVaultResourceGroup';
$AADClientID ='My-AAD-client-ID';
$KeyVaultName = 'MySecureVault';
$VMName = 'MySecureVM';
$KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
$diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
$KeyVaultResourceId = $KeyVault.ResourceId;

# Fill in the certificate path and the password so the thumbprint can be set as a variable. 

$certPath = '$CertPath = "C:\certificates\mycert.pfx';
$CertPassword ='Password'
$Cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($CertPath, $CertPassword)
$aadClientCertThumbprint = $cert.Thumbprint;

# Enable disk encryption using the client certificate thumbprint

Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGName -VMName $VMName -AadClientID $AADClientID -AadClientCertThumbprint $AADClientCertThumbprint -DiskEncryptionKeyVaultUrl $DiskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId
```
  
### <a name="enable-encryption-using-certificate-based-authentication-and-a-kek-with-azure-powershell"></a>Met behulp van verificatie op basis van certificaten en een KEK-sleutel met Azure PowerShell-codering inschakelen

```powershell
# Fill in 'MyVirtualMachineResourceGroup', 'MyKeyVaultResourceGroup', 'My-AAD-client-ID', 'MySecureVault,, 'MySecureVM’, and "KEKName.

$VMRGName = 'MyVirtualMachineResourceGroup';
$KVRGname = 'MyKeyVaultResourceGroup';
$AADClientID ='My-AAD-client-ID';
$KeyVaultName = 'MySecureVault';
$VMName = 'MySecureVM';
$keyEncryptionKeyName ='KEKName';
$KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
$diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
$KeyVaultResourceId = $KeyVault.ResourceId;
$keyEncryptionKeyUrl = (Get-AzureKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;

## Fill in the certificate path and the password so the thumbprint can be read and set as a variable.

$certPath = '$CertPath = "C:\certificates\mycert.pfx';
$CertPassword ='Password'
$Cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($CertPath, $CertPassword)
$aadClientCertThumbprint = $cert.Thumbprint;

# Enable disk encryption using the client certificate thumbprint and a KEK

Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGName -VMName $VMName -AadClientID $AADClientID -AadClientCertThumbprint $AADClientCertThumbprint -DiskEncryptionKeyVaultUrl $DiskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId
```

## <a name="disable-encryption"></a>Schakel versleuteling uit
U kunt versleuteling met Azure PowerShell, de Azure CLI, uitschakelen of met een Resource Manager-sjabloon. 

- **Schijfversleuteling met Azure PowerShell uitschakelen:** Als u wilt de versleuteling uitschakelen, gebruikt u de [Disable-AzureRmVMDiskEncryption](/powershell/module/az.compute/disable-azvmdiskencryption) cmdlet. 
     ```azurepowershell-interactive
     Disable-AzVMDiskEncryption -ResourceGroupName 'MyVirtualMachineResourceGroup' -VMName 'MySecureVM'
     ```

- **Met de Azure CLI-versleuteling uitschakelen:** Als u wilt uitschakelen versleuteling, gebruikt u de [az vm encryption uitschakelen](/cli/azure/vm/encryption#az-vm-encryption-disable) opdracht. 
     ```azurecli-interactive
     az vm encryption disable --name "MySecureVM" --resource-group "MyVirtualMachineResourceGroup" --volume-type [ALL, DATA, OS]
     ```
- **Schakel versleuteling met Resource Manager-sjabloon:** 

    1. Klik op **implementeren in Azure** uit de [schijfversleuteling voor het uitvoeren van Windows-VM uitschakelen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-windows-vm) sjabloon.
    2. Selecteer het abonnement, resourcegroep, locatie, VM, juridische voorwaarden en -overeenkomst.
    3.  Klik op **aankoop** schijfversleuteling op een actieve Windows-virtuele machine uitschakelen. 

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Schakel Azure Disk Encryption voor Linux](azure-security-disk-encryption-linux-aad.md)
