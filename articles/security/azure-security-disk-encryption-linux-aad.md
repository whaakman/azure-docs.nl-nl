---
title: Azure Disk Encryption met Azure AD App Linux IaaS-VM's (vorige versie) | Microsoft Docs
description: In dit artikel vindt u instructies over het inschakelen van Microsoft Azure Disk Encryption voor Linux IaaS-VM's.
services: security
documentationcenter: na
author: mestew
manager: MBaldwin
ms.assetid: 95695b12-0218-4ebf-a0dd-21de52787477
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2018
ms.author: mstewart
ms.openlocfilehash: f711e5acb37eeb45cc2285b64b72102bfe2f44e6
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/24/2018
ms.locfileid: "42889687"
---
# <a name="enable-azure-disk-encryption-for-linux-iaas-vms-previous-release"></a>Azure Disk Encryption inschakelen voor Linux IaaS-VM's (vorige versie)

**De nieuwe versie van Azure Disk Encryption wordt voorkomen dat de vereiste voor het ontwikkelen van een Azure AD-toepassing-parameter voor schijfversleuteling van VM inschakelen. Met de nieuwe versie kunt u niet langer moet Azure AD-referenties opgeven tijdens de stap van de versleuteling inschakelen. Alle nieuwe virtuele machines moeten worden versleuteld zonder de parameters van Azure AD-toepassing met behulp van de nieuwe versie. Voor instructies voor het inschakelen van versleuteling van de VM-schijf met behulp van de nieuwe versie, raadpleegt u [Azure Disk Encryption voor Linux VMS](azure-security-disk-encryption-linux.md). Virtuele machines die al zijn versleuteld met Azure AD-toepassing parameters worden nog steeds ondersteund en worden onderhouden met de syntaxis van de AAD moeten blijven.**

U kunt veel schijfversleuteling scenario's inschakelen en de stappen kunnen variëren op basis van het scenario. De volgende secties voor de scenario's in meer detail voor Linux IaaS-VM's. Voordat u kunt schijfversleuteling, gebruiken de [vereisten voor Azure Disk Encryption](azure-security-disk-encryption-prerequisites-aad.md) moeten worden uitgevoerd en de [aanvullende vereisten voor Linux IaaS-VM's](azure-security-disk-encryption-prerequisites-aad.md#bkmk_LinuxPrereq) sectie moet worden gecontroleerd.

Duren voordat een [momentopname](../virtual-machines/windows/snapshot-copy-managed-disk.md) en/of back-up voordat de schijven worden versleuteld. Back-ups zorgen ervoor dat een optie voor siteherstel mogelijk als er een onverwachte fout optreedt tijdens het versleutelen. Virtuele machines met beheerde schijven moeten u een back-up voordat versleuteling plaatsvindt. Als u een back-up is gemaakt, kunt u de cmdlet Set-AzureRmVMDiskEncryptionExtension kunt gebruiken voor het versleutelen van beheerde schijven met de parameter - skipVmBackup op te geven. Zie voor meer informatie over hoe u een back-up en herstel van versleutelde virtuele machines, de [Azure Backup](../backup/backup-azure-vms-encryption.md) artikel. 

>[!WARNING]
 >Om te verzekeren dat de geheimen van de versleuteling niet overschreden door de regionale grenzen, moet Azure Disk Encryption de Key Vault en de virtuele machines in dezelfde regio worden geplaatst. Maak en gebruik van een Key Vault die zich in dezelfde regio als de virtuele machine moeten worden versleuteld.</br></br>

> Bij het versleutelen van Linux-besturingssysteem, volumes, kan het enkele uren duren. Het is normaal voor Linux-besturingssysteem volumes langer dan gegevensvolumes te versleutelen. 

>Uitschakelen van versleuteling op Linux-VM's wordt alleen ondersteund voor de gegevensvolumes. Het wordt niet ondersteund op de gegevens of besturingssysteemvolumes als het volume met het besturingssysteem is versleuteld.  


## <a name="bkmk_NewLinux"></a> Een nieuwe Linux IaaS-VM implementeren met schijfversleuteling is ingeschakeld 

1. Gebruik de [Resource Manager-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-full-disk-encrypted-rhel) Maak een nieuwe versleuteld Linux IaaS-VM. De sjabloon wordt een nieuwe Red Hat Linux 7.2 maken met een matrix van 200 GB RAID-0 en volledige schijfversleuteling met behulp van beheerde schijven. Op de [Veelgestelde vragen over](azure-security-disk-encryption-faq.md#bkmk_LinuxOSSupport) artikel, zult u merken dat sommige Linux-distributies versleuteling alleen ondersteuning voor gegevensschijven. Deze sjabloon bevat echter een kans om vertrouwd te raken met het implementeren van sjablonen en status voor schijfversleuteling met meerdere methoden of. 
 
1. Klik op **implementeren in Azure** op de Azure Resource Manager-sjabloon.

2. Selecteer het abonnement, resourcegroep, locatie voor resourcegroep, parameters, juridische voorwaarden en -overeenkomst. Klik op **maken** versleuteling op de bestaande of actieve IaaS-VM inschakelen.

3. Nadat u de sjabloon implementeert, controleert u of de status van de VM-versleuteling met behulp van de gewenste manier:
     - Controleren met de Azure CLI met behulp van de [az vm encryption show](/cli/azure/vm/encryption#az-vm-encryption-show) opdracht. 

         ```azurecli-interactive 
         az vm encryption show --name "MySecureVM" --resource-group "MySecureRg"
         ```

     - Met Azure PowerShell controleren met behulp van de [Get-AzureRmVmDiskEncryptionStatus](/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus) cmdlet. 

         ```azurepowershell-interactive
         Get-AzureRmVmDiskEncryptionStatus -ResourceGroupName 'MySecureRg' -VMName 'MySecureVM'
         ```

     - Selecteer de virtuele machine en klik vervolgens op **schijven** onder de **instellingen** kop om te controleren of de status voor schijfversleuteling in de portal. In de grafiek onder **versleuteling**, ziet u of deze ingeschakeld. 

| Parameter | Beschrijving |
| --- | --- |
| AAD Client-ID | Client-ID van de Azure AD-toepassing met machtigingen voor geheimen schrijven naar de key vault. |
| AAD-Clientgeheim | Clientgeheim van de Azure AD-toepassing met machtigingen voor het schrijven van geheimen voor uw key vault. |
| Key Vault-naam | De naam van de key vault dat de sleutel moet worden geplaatst. |
| Key Vault-resourcegroep | De resourcegroep van de key vault. |


## <a name="bkmk_RunningLinux"> </a> Schakelt u versleuteling op een bestaande of actieve IaaS virtuele Linux-machine
In dit scenario kunt u versleuteling inschakelen met behulp van de Resource Manager-sjabloon, de PowerShell-cmdlets of de CLI-opdrachten. 

>[!IMPORTANT]
 >Dit is verplicht op momentopname en/of back-up een beheerde schijf op basis van de VM-exemplaar buiten en voordat Azure Disk Encryption wordt ingeschakeld. Een momentopname van de beheerde schijf kan worden uitgevoerd vanuit de portal of [Azure Backup](../backup/backup-azure-vms-encryption.md) kan worden gebruikt. Back-ups zorgen ervoor dat een optie voor siteherstel mogelijk in het geval van een onverwachte fout opgetreden tijdens het versleutelen is. Als u een back-up is gemaakt, kan de cmdlet Set-AzureRmVMDiskEncryptionExtension worden gebruikt voor het versleutelen van beheerde schijven met de parameter - skipVmBackup op te geven. De opdracht Set-AzureRmVMDiskEncryptionExtension, op basis van virtuele machines op beheerde schijven op basis van mislukken totdat een back-up is gemaakt en deze parameter is opgegeven. 
>
>Versleutelen of als u versleuteling uitschakelt kan ertoe leiden dat de virtuele machine opnieuw op te starten. 
>

### <a name="bkmk_RunningLinuxCLI"> </a>Schakelt u versleuteling op een bestaande of actieve Linux-VM met behulp van Azure CLI 
U kunt schijfversleuteling inschakelen op uw versleutelde VHD te installeren en gebruiken de [Azure CLI 2.0](/cli/azure) opdrachtregel-hulpprogramma. U kunt PowerShell in uw browser gebruiken met [Azure Cloud Shell](../cloud-shell/overview.md), of installeren op uw lokale computer en dan gebruiken in een PowerShell-sessie. Gebruik de volgende CLI-opdrachten voor het inschakelen van versleuteling op bestaande of IaaS virtuele Linux-machines uitvoeren in Azure:

Gebruik de [az vm encryption inschakelen](/cli/azure/vm/encryption#az-vm-encryption-enable) opdracht schakelt u versleuteling op een actieve IaaS-virtuele machine in Azure.

-  **Een actieve virtuele machine met behulp van een clientgeheim versleutelen:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --aad-client-id "<my spn created with CLI/my Azure AD ClientID>"  --aad-client-secret "My-AAD-client-secret" --disk-encryption-keyvault "MySecureVault" --volume-type [All|OS|Data]
     ```

- **Versleutelen van een actieve virtuele machine met behulp van de KEK het inpakken van het clientgeheim:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --aad-client-id "<my spn created with CLI which is the Azure AD ClientID>"  --aad-client-secret "My-AAD-client-secret" --disk-encryption-keyvault  "MySecureVault" --key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type [All|OS|Data]
     ```

    >[!NOTE]
    > De syntaxis voor de waarde van de schijf-versleuteling-keyvault-parameter is de volledige id-reeks: / subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br> </br>
De syntaxis voor de waarde van de encryptiesleutel parameter is de volledige URI naar de KEK-sleutel in: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

- **Controleer of de schijven worden versleuteld:** gebruiken om te controleren op de status voor schijfversleuteling van een IaaS-VM, de [az vm encryption show](/cli/azure/vm/encryption#az-vm-encryption-show) opdracht. 

     ```azurecli-interactive
     az vm encryption show --name "MySecureVM" --resource-group "MySecureRg"
     ```

- **Schakel versleuteling uit:** als wilt uitschakelen versleuteling, gebruikt u de [az vm encryption uitschakelen](/cli/azure/vm/encryption#az-vm-encryption-disable) opdracht. Als u versleuteling uitschakelt is alleen toegestaan op gegevensvolumes voor virtuele Linux-machines.

     ```azurecli-interactive
     az vm encryption disable --name "MySecureVM" --resource-group "MySecureRg" --volume-type DATA
     ```

### <a name="bkmk_RunningLinuxPSH"> </a> Schakelt u versleuteling op een bestaande of actieve Linux-VM met behulp van PowerShell
Gebruik de [Set-AzureRmVMDiskEncryptionExtension](/powershell/module/azurerm.compute/set-azurermvmdiskencryptionextension) cmdlet schakelt u versleuteling op een actieve IaaS-virtuele machine in Azure. 

-  **Versleutelen van een actieve virtuele machine met behulp van een clientgeheim:** het onderstaande script wordt uw variabelen geïnitialiseerd en wordt de cmdlet Set-AzureRmVMDiskEncryptionExtension uitgevoerd. De resourcegroep, de VM, de sleutelkluis, de AAD-app en de clientgeheim moeten al zijn gemaakt voor de vereisten. MySecureRg, MySecureVM, MySecureVault, Mijn-AAD-client-ID en Mijn-AAD-client-secret vervangen door uw waarden. Mogelijk moet u de parameter - VolumeType toevoegen als u het versleutelen bent gegevensschijven en niet de besturingssysteemschijf. 

     ```azurepowershell-interactive
      $rgName = 'MySecureRg';
      $vmName = 'MySecureVM';
      $aadClientID = 'My-AAD-client-ID';
      $aadClientSecret = 'My-AAD-client-secret';
      $KeyVaultName = 'MySecureVault';
      $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
      $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
      $KeyVaultResourceId = $KeyVault.ResourceId;

      Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId;
    ```
- **Versleutelen van een actieve virtuele machine met behulp van de KEK het inpakken van het clientgeheim:** Azure Disk Encryption kunt u een bestaande sleutel opgeven in uw key vault voor het verpakken van schijf encryption geheimen die zijn gegenereerd tijdens het inschakelen van versleuteling. Wanneer een sleutel van versleutelingssleutel is opgegeven, gebruikt Azure Disk Encryption die sleutel het verpakken van de geheimen van de versleuteling voor het schrijven naar de Key Vault. Mogelijk moet u de parameter - VolumeType toevoegen als u het versleutelen bent gegevensschijven en niet de besturingssysteemschijf. 

     ```azurepowershell-interactive
     $rgName = 'MySecureRg';
     $vmName = 'MyExtraSecureVM';
     $aadClientID = 'My-AAD-client-ID';
     $aadClientSecret = 'My-AAD-client-secret';
     $KeyVaultName = 'MySecureVault';
     $keyEncryptionKeyName = 'MyKeyEncryptionKey';
     $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
     $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     $keyEncryptionKeyUrl = (Get-AzureKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;

     Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId;

     ```

    >[!NOTE]
    > De syntaxis voor de waarde van de schijf-versleuteling-keyvault-parameter is de volledige id-reeks: / subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br> De syntaxis voor de waarde van de encryptiesleutel parameter is de volledige URI naar de KEK-sleutel in: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 
    
- **Controleer of de schijven worden versleuteld:** gebruiken om te controleren op de status voor schijfversleuteling van een IaaS-VM, de [Get-AzureRmVmDiskEncryptionStatus](/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus) cmdlet. 
    
     ```azurepowershell-interactive 
     Get-AzureRmVmDiskEncryptionStatus -ResourceGroupName MySecureRg -VMName MySecureVM
     ```
    
- **Schijfversleuteling uitschakelen:** als u wilt de versleuteling uitschakelen, gebruikt u de [Disable-AzureRmVMDiskEncryption](/powershell/module/azurerm.compute/disable-azurermvmdiskencryption) cmdlet. Als u versleuteling uitschakelt is alleen toegestaan op gegevensvolumes voor virtuele Linux-machines.
     
     ```azurepowershell-interactive 
     Disable-AzureRmVMDiskEncryption -ResourceGroupName 'MySecureRG' -VMName 'MySecureVM'
     ```


### <a name="bkmk_RunningLinux"> </a> Schakelt u versleuteling op een bestaande of actieve IaaS virtuele Linux-machine met een sjabloon

U kunt schijfversleuteling op een bestaande of actieve IaaS Linux VM in Azure inschakelen met behulp van de [Resource Manager-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm).

1. Klik op **implementeren in Azure** op de Azure-quickstart-sjabloon.

2. Selecteer het abonnement, resourcegroep, locatie voor resourcegroep, parameters, juridische voorwaarden en -overeenkomst. Klik op **maken** versleuteling op de bestaande of actieve IaaS-VM inschakelen.

De volgende tabel bevat de parameters van de Resource Manager-sjabloon voor bestaande of uitvoeren van virtuele machines die gebruikmaken van een Azure AD-client-ID:

| Parameter | Beschrijving |
| --- | --- |
| AADClientID | Client-ID van de Azure AD-toepassing met machtigingen voor geheimen schrijven naar de key vault. |
| AADClientSecret | Clientgeheim van de Azure AD-toepassing met machtigingen voor het schrijven van geheimen voor uw key vault. |
| keyVaultName | De naam van de sleutelkluis waarin de sleutel moet worden geüpload naar. U kunt deze ophalen met behulp van de Azure CLI-opdracht `az keyvault show --name "MySecureVault" --query resourceGroup`. |
|  KeyEncryptionKeyURL | URL van de sleutel van versleutelingssleutel die wordt gebruikt om de gegenereerde sleutel te versleutelen. Deze parameter is optioneel als u selecteert **nokek** in de vervolgkeuzelijst UseExistingKek. Als u selecteert **kek** in de vervolgkeuzelijst UseExistingKek voert u de _keyEncryptionKeyURL_ waarde. |
| VolumeType | Het type volume dat de versleutelingsbewerking wordt uitgevoerd op. Geldige ondersteunde waarden zijn _OS_ of _alle_ (Zie ondersteund Linux-distributies en hun versies voor het besturingssysteem en gegevensschijven in de sectie vereisten eerder). |
| SequenceVersion | De versie van de volgorde van de BitLocker-bewerking. Dit versienummer verhoogd telkens wanneer een schijfversleuteling bewerking wordt uitgevoerd op dezelfde VM. |
| vmName | De naam van de virtuele machine die de versleutelingsbewerking moet worden uitgevoerd op. |
| wachtwoordzin | Typ een sterke wachtwoordzin in als de versleutelingssleutel voor servicegegevens. |



## <a name="bkmk_EFA"> </a>EncryptFormatAll-functie gebruiken voor gegevensschijven op Linux IaaS-VM 's
De **EncryptFormatAll** parameter vermindert de tijd voor Linux-gegevensschijven moeten worden versleuteld. Partities aan bepaalde criteria voldoen worden, opgemaakt (met het huidige bestandssysteem). Ze gaat vervolgens terug naar waar voordat de uitvoering van de opdracht was worden gekoppeld. Als u uitsluiten van een gegevensschijf die voldoet aan de criteria wilt, kunt u het ontkoppelen voordat u de opdracht uitvoert.

 Na het uitvoeren van deze opdracht wordt alle stations die zijn gekoppeld eerder geformatteerd. En vervolgens de versleutelingslaag boven op het station nu leeg zijn gestart. Wanneer deze optie is geselecteerd, wordt de tijdelijke resource-schijf die is gekoppeld aan de virtuele machine eveneens versleuteld. Als de kortstondige schijf opnieuw wordt ingesteld, wordt deze opnieuw geformatteerd en opnieuw versleuteld voor de virtuele machine met de Azure Disk Encryption-oplossing in de eerstvolgende gelegenheid installeren.

>[!WARNING]
> EncryptFormatAll mag niet worden gebruikt wanneer er benodigde gegevens op gegevensvolumes van een virtuele machine. U kunt schijven uitsluiten van versleuteling door ze te ontkoppelen. U moet eerst de EncryptFormatAll eerst op een virtuele testmachine uitproberen, inzicht in de functie-parameter en de gevolgen voor de voordat u deze op de VM voor productie. De optie EncryptFormatAll maakt op de gegevensschijf en alle gegevens op verloren. Voordat u doorgaat, controleert u of u wilt uitsluiten schijven correct unmouted. </br></br>
 >Als u deze parameter instellen tijdens het bijwerken van instellingen voor codering, kan dit leiden tot een opnieuw opstarten voordat de daadwerkelijke versleuteling. In dit geval is het ook wilt verwijderen van de schijf die u niet opmaken van het fstab-bestand wilt. Op dezelfde manier moet u de partitie die u versleutelen-indeling naar het fstab-bestand wilt voordat u begint de coderingsbewerking toevoegen. 

### <a name="bkmk_EFACriteria"> </a> EncryptFormatAll criteria
De parameter gaat echter alle partities en worden deze versleuteld, zolang ze voldoen aan **alle** van de onderstaande criteria: 
- Is niet een hoofdmap/OS/opstartpartitie
- Is niet versleuteld
- Is geen BEK volume
- Is gekoppeld


### <a name="bkmk_EFATemplate"> </a> Gebruik de parameter EncryptFormatAll met een sjabloon
Voor het gebruik van de optie EncryptFormatAll gebruiken in een bestaande Azure Resource Manager-sjabloon die een Linux-VM versleutelt en wijzig de **EncryptionOperation** veld voor de resource AzureDiskEncryption.

1. Gebruik bijvoorbeeld de [Resource Manager-sjabloon voor het versleutelen van een actieve virtuele machine voor Linux-IaaS](https://github.com/vermashi/azure-quickstart-templates/tree/encrypt-format-running-linux-vm/201-encrypt-running-linux-vm). 
2. Klik op **implementeren in Azure** op de Azure-quickstart-sjabloon.
3. Wijzig de **EncryptionOperation** van **EnableEncryption** naar **EnableEncryptionFormatAl**
4. Selecteer het abonnement, resourcegroep, locatie voor resourcegroep, andere parameters, juridische voorwaarden en -overeenkomst. Klik op **maken** versleuteling op de bestaande of actieve IaaS-VM inschakelen.


### <a name="bkmk_EFAPSH"> </a> Gebruik de parameter EncryptFormatAll met een PowerShell-cmdlet
Gebruik de [Set-AzureRmVMDiskEncryptionExtension](/powershell/module/azurerm.compute/set-azurermvmdiskencryptionextension) cmdlet met de [EncryptFormatAll parameter](https://www.powershellgallery.com/packages/AzureRM/5.0.0). 

**Versleutelen van een actieve virtuele machine met behulp van een clientgeheim en EncryptFormatAll:** bijvoorbeeld het onderstaande script wordt uw variabelen geïnitialiseerd en wordt de cmdlet Set-AzureRmVMDiskEncryptionExtension uitgevoerd met de parameter EncryptFormatAll. De resourcegroep, de VM, de sleutelkluis, de AAD-app en de clientgeheim moeten al zijn gemaakt voor de vereisten. MySecureRg, MySecureVM, MySecureVault, Mijn-AAD-client-ID en Mijn-AAD-client-secret vervangen door uw waarden.
  
   ```azurepowershell-interactive
     $rgName = 'MySecureRg';
     $vmName = 'MySecureVM';
     $aadClientID = 'My-AAD-client-ID';
     $aadClientSecret = 'My-AAD-client-secret';
     $KeyVaultName = 'MySecureVault';
     $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
     $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
      
     Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -EncryptFormatAll
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
    
    4. Voer de cmdlet Set-AzureRmVMDiskEncryptionExtension PowerShell met - EncryptFormatAll voor het versleutelen van deze schijven.
         ```azurepowershell-interactive
         Set-AzureRmVMDiskEncryptionExtension -ResouceGroupName "MySecureGroup" -VMName "MySecureVM" -DiskEncryptionKeyVaultUrl "https://mykeyvault.vault.azure.net/" -EncryptFormatAll
         ```
    5. LVM boven op deze nieuwe schijven instellen. Houd er rekening mee dat de versleutelde schijven worden ontgrendeld nadat de virtuele machine wordt opgestart is voltooid. Het LVM-koppelen heeft dus ook later worden vertraagd.




## <a name="bkmk_VHDpre"> </a> Nieuwe IaaS VM's die worden gemaakt op basis van de klant versleuteld VHD en versleuteling sleutels
In dit scenario kunt u inschakelen met behulp van de Resource Manager-sjabloon, de PowerShell-cmdlets of de CLI-opdrachten te coderen. De volgende secties wordt uitgelegd in meer detail de Resource Manager-sjabloon en de CLI-opdrachten. 

Volg de instructies in de bijlage voor het maken van vooraf gecodeerde-installatiekopieën die kunnen worden gebruikt in Azure. Nadat de installatiekopie is gemaakt, kunt u de stappen in de volgende sectie om een versleutelde Azure-VM te maken.

* [Vooraf gecodeerde Windows VHD voorbereiden](azure-security-disk-encryption-appendix.md#bkmk_preWin)
* [Vooraf gecodeerde Linux-VHD voorbereiden](azure-security-disk-encryption-appendix.md#bkmk_preLinux)

>[!IMPORTANT]
 >Dit is verplicht op momentopname en/of back-up een beheerde schijf op basis van de VM-exemplaar buiten en voordat Azure Disk Encryption wordt ingeschakeld. Een momentopname van de beheerde schijf kan worden uitgevoerd vanuit de portal of [Azure Backup](../backup/backup-azure-vms-encryption.md) kan worden gebruikt. Back-ups zorgen ervoor dat een optie voor siteherstel mogelijk in het geval van een onverwachte fout opgetreden tijdens het versleutelen is. Als u een back-up is gemaakt, kan de cmdlet Set-AzureRmVMDiskEncryptionExtension worden gebruikt voor het versleutelen van beheerde schijven met de parameter - skipVmBackup op te geven. De opdracht Set-AzureRmVMDiskEncryptionExtension, op basis van virtuele machines op beheerde schijven op basis van mislukken totdat een back-up is gemaakt en deze parameter is opgegeven. 
>
>Versleutelen of als u versleuteling uitschakelt kan ertoe leiden dat de virtuele machine opnieuw op te starten. 



### <a name="bkmk_VHDprePSH"> </a> Azure PowerShell gebruiken voor het versleutelen van IaaS-VM's met vooraf gecodeerde VHD 's 
U kunt schijfversleuteling op uw versleutelde VHD inschakelen met behulp van de PowerShell-cmdlet [Set-azurermvmosdisk,](/powershell/module/azurerm.compute/set-azurermvmosdisk#examples). Het onderstaande voorbeeld hebt u enkele algemene parameters. 

```powershell
$VirtualMachine = New-AzureRmVMConfig -VMName "MySecureVM" -VMSize "Standard_A1"
$VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name "SecureOSDisk" -VhdUri "os.vhd" Caching ReadWrite -Windows -CreateOption "Attach" -DiskEncryptionKeyUrl "https://mytestvault.vault.azure.net/secrets/Test1/514ceb769c984379a7e0230bddaaaaaa" -DiskEncryptionKeyVaultId "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.KeyVault/vaults/mytestvault"
New-AzureRmVM -VM $VirtualMachine -ResouceGroupName "MySecureRG"
```

### <a name="bkmk_VHDpreRM"> </a> De Resource Manager-sjabloon gebruiken voor het versleutelen van IaaS-VM's met vooraf gecodeerde VHD 's 
U kunt schijfversleuteling op uw versleutelde VHD inschakelen met behulp van de [Resource Manager-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-pre-encrypted-vm).

1. Klik op de Azure-quickstart-sjabloon, **implementeren in Azure**.

2. Selecteer het abonnement, resourcegroep, locatie voor resourcegroep, parameters, juridische voorwaarden en -overeenkomst. Klik op **maken** versleuteling op de nieuwe IaaS-VM inschakelen.

De volgende tabel bevat de parameters van de Resource Manager-sjabloon voor uw versleutelde VHD:

| Parameter | Beschrijving |
| --- | --- |
| newStorageAccountName | De naam van het opslagaccount voor het opslaan van de versleutelde OS VHD. Dit opslagaccount moet al zijn gemaakt in dezelfde resourcegroep en dezelfde locatie als de virtuele machine. |
| osVhdUri | De URI van de VHD met het besturingssysteem van het opslagaccount. |
| besturingssysteemtype | Type besturingssysteem product (Windows/Linux). |
| virtualNetworkName | De naam van het VNet waarmee de VM NIC tot behoren moet. De naam moet al zijn gemaakt in dezelfde resourcegroep en dezelfde locatie als de virtuele machine. |
| subnetName | Naam van het subnet in het VNet waarmee de VM NIC tot behoren moet. |
| vmSize | Grootte van de virtuele machine. Op dit moment worden alleen Standard A, D en G-serie ondersteund. |
| keyVaultResourceID | De ResourceID die de sleutelkluis-resource in Azure Resource Manager. U kunt deze ophalen met behulp van de PowerShell-cmdlet `(Get-AzureRmKeyVault -VaultName &lt;MyKeyVaultName&gt; -ResourceGroupName &lt;MyResourceGroupName&gt;).ResourceId` of met behulp van de Azure CLI-opdracht `az keyvault show --name "MySecureVault" --query id`|
| keyVaultSecretUrl | URL van de schijf-versleutelingssleutel op die ingesteld in de key vault. |
| keyVaultKekUrl | URL van de sleutel van versleutelingssleutel voor het versleutelen van de gegenereerde versleutelingssleutel van de schijf. |
| vmName | De naam van de IaaS-VM. |
## <a name="enable-encryption-on-a-newly-added-data-disk"></a>Schakelt u versleuteling op een nieuw toegevoegde gegevensschijf
U kunt toevoegen met een nieuwe gegevens schijf met [az vm disk attach](../virtual-machines/linux/add-disk.md), of [via Azure portal](../virtual-machines/linux/attach-disk-portal.md). Voordat u coderen kunt, moet u eerst de zojuist gekoppelde gegevensschijf koppelen. U moet versleuteling van het gegevensstation aanvragen omdat het station onbruikbaar is terwijl versleuteling uitgevoerd wordt. 

### <a name="enable-encryption-on-a-newly-added-disk-with-azure-cli"></a>Schakelt u versleuteling op een nieuw toegevoegde schijf met Azure CLI
 De Azure CLI-opdracht wordt automatisch een nieuwe versie van de reeks voor u opgeeft bij het uitvoeren van de opdracht voor het inschakelen van versleuteling. 
-  **Een actieve virtuele machine met behulp van een clientgeheim versleutelen:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --aad-client-id "<my spn created with CLI/my Azure AD ClientID>"  --aad-client-secret "My-AAD-client-secret" --disk-encryption-keyvault "MySecureVault" --volume-type "Data"
     ```

- **Versleutelen van een actieve virtuele machine met behulp van de KEK het inpakken van het clientgeheim:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --aad-client-id "<my spn created with CLI which is the Azure AD ClientID>"  --aad-client-secret "My-AAD-client-secret" --disk-encryption-keyvault  "MySecureVault" --key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type "Data"
     ```

### <a name="enable-encryption-on-a-newly-added-disk-with-azure-powershell"></a>Schakelt u versleuteling op een nieuw toegevoegde schijf met Azure PowerShell
 Wanneer u Powershell gebruikt voor het versleutelen van een nieuwe schijf voor Linux, moet een nieuwe versie van de takenreeks worden opgegeven. De versie van de reeks moet uniek zijn. Het onderstaande script genereert een GUID voor de versie van de reeks. 
 

-  **Versleutelen van een actieve virtuele machine met behulp van een clientgeheim:** het onderstaande script wordt uw variabelen geïnitialiseerd en wordt de cmdlet Set-AzureRmVMDiskEncryptionExtension uitgevoerd. De resourcegroep, de VM, de sleutelkluis, de AAD-app en de clientgeheim moeten al zijn gemaakt voor de vereisten. MySecureRg, MySecureVM, MySecureVault, Mijn-AAD-client-ID en Mijn-AAD-client-secret vervangen door uw waarden. De parameter - VolumeType is ingesteld op gegevensschijven en niet de besturingssysteemschijf. 

     ```azurepowershell-interactive
      $sequenceVersion = [Guid]::NewGuid();
      $rgName = 'MySecureRg';
      $vmName = 'MySecureVM';
      $aadClientID = 'My-AAD-client-ID';
      $aadClientSecret = 'My-AAD-client-secret';
      $KeyVaultName = 'MySecureVault';
      $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
      $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
      $KeyVaultResourceId = $KeyVault.ResourceId;

      Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -VolumeType 'data' –SequenceVersion $sequenceVersion;
    ```
- **Versleutelen van een actieve virtuele machine met behulp van de KEK het inpakken van het clientgeheim:** Azure Disk Encryption kunt u een bestaande sleutel opgeven in uw key vault voor het verpakken van schijf encryption geheimen die zijn gegenereerd tijdens het inschakelen van versleuteling. Wanneer een sleutel van versleutelingssleutel is opgegeven, gebruikt Azure Disk Encryption die sleutel het verpakken van de geheimen van de versleuteling voor het schrijven naar de Key Vault. Mogelijk moet u de parameter - VolumeType toevoegen als u het versleutelen bent gegevensschijven en niet de besturingssysteemschijf. 

     ```azurepowershell-interactive
     $rgName = 'MySecureRg';
     $vmName = 'MyExtraSecureVM';
     $aadClientID = 'My-AAD-client-ID';
     $aadClientSecret = 'My-AAD-client-secret';
     $KeyVaultName = 'MySecureVault';
     $keyEncryptionKeyName = 'MyKeyEncryptionKey';
     $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
     $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     $keyEncryptionKeyUrl = (Get-AzureKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;

     Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId;

     ```

    >[!NOTE]
    > De syntaxis voor de waarde van de schijf-versleuteling-keyvault-parameter is de volledige id-reeks: / subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br> De syntaxis voor de waarde van de encryptiesleutel parameter is de volledige URI naar de KEK-sleutel in: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 


## <a name="disable-encryption-for-linux-vms"></a>Versleuteling voor Linux-VM's uitschakelen
U kunt versleuteling met Azure PowerShell, de Azure CLI, uitschakelen of met een Resource Manager-sjabloon. 

>[!IMPORTANT]
>Versleuteling met Azure Disk Encryption voor virtuele Linux-machines uitschakelen wordt alleen ondersteund voor de gegevensvolumes. Het wordt niet ondersteund op de gegevens of besturingssysteemvolumes als het volume met het besturingssysteem is versleuteld.  

- **Schijfversleuteling met Azure PowerShell uitschakelen:** als u wilt de versleuteling uitschakelen, gebruikt u de [Disable-AzureRmVMDiskEncryption](/powershell/module/azurerm.compute/disable-azurermvmdiskencryption) cmdlet. 
     ```azurepowershell-interactive
     Disable-AzureRmVMDiskEncryption -ResourceGroupName 'MySecureRG' -VMName 'MySecureVM' [--volume-type {ALL, DATA, OS}]
     ```

- **Met de Azure CLI-versleuteling uitschakelen:** als wilt uitschakelen versleuteling, gebruikt u de [az vm encryption uitschakelen](/cli/azure/vm/encryption#az-vm-encryption-disable) opdracht. 
     ```azurecli-interactive
     az vm encryption disable --name "MySecureVM" --resource-group "MySecureRg" --volume-type [ALL, DATA, OS]
     ```
- **Uitschakelen van versleuteling met Resource Manager-sjabloon:** gebruiken de [Schakel versleuteling uit op een actieve Linux VM](https://aka.ms/decrypt-linuxvm) sjabloon om versleuteling te schakelen.
     1. Klik op **Implementeren in Azure**.
     2. Selecteer het abonnement, resourcegroep, locatie, VM, juridische voorwaarden en -overeenkomst.
     3.  Klik op **aankoop** schijfversleuteling op een actieve Windows-virtuele machine uitschakelen. 


## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Schakel Azure Disk Encryption voor Windows](azure-security-disk-encryption-windows-aad.md)
