---
title: Versleutelen van schijven voor Azure schaal wordt ingesteld met Azure PowerShell | Microsoft Docs
description: Informatie over het gebruik van Azure PowerShell voor het versleutelen van VM-exemplaren en de gekoppelde schijven in een Windows virtuele-machineschaalset
services: virtual-machine-scale-sets
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/30/2018
ms.author: iainfou
ms.openlocfilehash: 91138ffad0fd906061e0b0ce5cdb251f402d3341
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/03/2018
---
# <a name="encrypt-os-and-attached-data-disks-in-a-virtual-machine-scale-set-with-azure-powershell-preview"></a>Versleutelen OS en bijgesloten gegevensschijven in een virtuele-machineschaalset ingesteld met Azure PowerShell (Preview)

Als u wilt beveiligen en bescherming van gegevens in rust met toonaangevende standaard versleuteling technologie, ondersteuning voor virtuele-machineschaalsets Azure schijf versleuteling (ADE). Versleuteling kan worden ingeschakeld voor Windows en Linux virtuele machine sets schalen. Zie voor meer informatie [Azure Disk Encryption for Windows- en Linux](../security/azure-security-disk-encryption.md).

> [!NOTE]
>  Versleuteling van de Azure-schijf voor virtuele-machineschaalsets is momenteel in de openbare preview beschikbaar in alle openbare Azure-regio.

De schijf van Azure-versleuteling wordt ondersteund:
- voor schaal sets met beheerde schijven gemaakt en niet ondersteund voor systeemeigen (of niet-beheerde) schijf-schaalsets.
- voor OS- en gegevensvolumes in Windows-schaalsets. Schakel versleuteling wordt ondersteund voor OS- en gegevensvolumes voor Windows-schaalsets.
- voor de gegevensvolumes in Linux-schaalsets. OS-schijfversleuteling wordt niet ondersteund in het huidige voorbeeld voor Linux-schaalsets.

Scale set VM terugzetten van de installatiekopie en upgrade-bewerkingen worden niet ondersteund in de huidige preview. De Azure disk encryption voor virtuele machine scale sets preview wordt aanbevolen in een testomgeving. In de preview schijfversleuteling in productieomgevingen waar mogelijk moet u een installatiekopie van het besturingssysteem in een versleutelde schaalset upgrade niet is ingeschakeld.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Als u wilt installeren en gebruiken van de PowerShell lokaal, in deze zelfstudie vereist de Azure PowerShell-moduleversie 5.7.0 of hoger. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Login-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.

## <a name="register-for-disk-encryption-preview"></a>Registreren voor schijf versleuteling preview

De Azure disk encryption voor virtuele-machineschaalsets preview, moet u uw abonnement met de zelfregistratie [registreren AzureRmProviderFeature](/powershell/module/azurerm.resources/register-azurermproviderfeature). U hoeft alleen de eerste keer dat u de schijf versleuteling preview-functie van de volgende stappen uitvoeren:

```azurepowershell-interactive
Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Compute -FeatureName "UnifiedDiskEncryption"
```

Het kan maximaal 10 minuten duren voordat de aanvraag voor functieregistratie worden doorgegeven. U kunt controleren op de status van de registratie met [Get-AzureRmProviderFeature](/powershell/module/AzureRM.Resources/Get-AzureRmProviderFeature). Wanneer de `RegistrationState` rapporten *geregistreerde*, opnieuw te registreren de *Mirosoft.Compute* provider met [registreren AzureRmResourceProvider](/powershell/module/AzureRM.Resources/Register-AzureRmResourceProvider):

```azurepowershell-interactive
Get-AzureRmProviderFeature -ProviderNamespace "Microsoft.Compute" -FeatureName "UnifiedDiskEncryption"
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute
```

## <a name="create-an-azure-key-vault-enabled-for-disk-encryption"></a>Maken van een Azure Key Vault ingeschakeld voor schijfversleuteling

Azure Sleutelkluis kunt opslaan sleutels, geheimen of wachtwoorden waarmee u kunt ze veilig implementeert in uw toepassingen en services. Cryptografische sleutels worden opgeslagen in Azure Key Vault met software-beveiliging, of u kunt importeren of genereren van uw sleutels in Hardware Security Modules (HSM's) die is gecertificeerd voor FIPS 140-2 level 2-standaarden. Deze cryptografische sleutels worden gebruikt voor het versleutelen en ontsleutelen van virtuele schijven gekoppeld aan uw virtuele machine. U beheer behouden over deze cryptografische sleutels en het gebruik ervan kunt controleren.

Maken van een Sleutelkluis met [nieuwe AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault). Instellen zodat de Sleutelkluis moet worden gebruikt voor schijfversleuteling de *EnabledForDiskEncryption* parameter. Het volgende voorbeeld definieert ook variabelen voor de naam van een resourcegroep, de naam Sleutelkluis en de locatie. Geef uw eigen unieke naam van de Sleutelkluis:

```azurepowershell-interactive
$rgName="myResourceGroup"
$vaultName="myuniquekeyvault"
$location = "EastUS"

New-AzureRmResourceGroup -Name $rgName -Location $location
New-AzureRmKeyVault -VaultName $vaultName -ResourceGroupName $rgName -Location $location -EnabledForDiskEncryption
```

### <a name="use-an-existing-key-vault"></a>Gebruik een bestaande Sleutelkluis

Deze stap is alleen vereist als u een bestaande Sleutelkluis die u wilt gebruiken met schijfversleuteling hebt. Deze stap overslaan als u een Sleutelkluis hebt gemaakt in de vorige sectie.

U kunt een bestaande Sleutelkluis in hetzelfde abonnement en dezelfde regio als de schaal voor de schijfversleuteling met instelt inschakelen [Set-AzureRmKeyVaultAccessPolicy](/powershell/module/AzureRM.KeyVault/Set-AzureRmKeyVaultAccessPolicy). Definieer de naam van uw bestaande Sleutelkluis in de *$vaultName* variabele als volgt:

```azurepowershell-interactive
$vaultName="myexistingkeyvault"
Set-AzureRmKeyVaultAccessPolicy -VaultName $vaultName -EnabledForDiskEncryption
```

## <a name="create-a-scale-set"></a>Een schaalset maken

Stel eerst een beheerdersnaam en -wachtwoord in voor de VM-exemplaren met behulp van [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```azurepowershell-interactive
$cred = Get-Credential
```

Maak nu een virtuele-machineschaalset met [New-AzureRmVmss](/powershell/module/azurerm.compute/new-azurermvmss). Om het verkeer te distribueren naar de verschillende VM-exemplaren, wordt er ook een load balancer gemaakt. De load balancer bevat regels voor het distribueren van verkeer op TCP-poort 80, en voor het toestaan van extern bureaubladverkeer op TCP-poort 3389 en externe toegang via PowerShell op TCP-poort 5985:

```azurepowershell-interactive
$vmssName="myScaleSet"

New-AzureRmVmss `
    -ResourceGroupName $rgName `
    -VMScaleSetName $vmssName `
    -Location $location `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -PublicIpAddressName "myPublicIPAddress" `
    -LoadBalancerName "myLoadBalancer" `
    -UpgradePolicy "Automatic" `
    -Credential $cred
```

## <a name="enable-encryption"></a>Versleuteling inschakelen

Voor het versleutelen van de VM-exemplaren in een schaalset u eerst enkele gegevens op de URI van Key-kluis en resource-ID met [Get-AzureRmKeyVault](/powershell/module/AzureRM.KeyVault/Get-AzureRmKeyVault). Deze variabelen worden gebruikt voor start het versleutelingsproces met [Set AzureRmVmssDiskEncryptionExtension](/powershell/module/AzureRM.Compute/Set-AzureRmVmssDiskEncryptionExtension):

```azurepowershell-interactive
$diskEncryptionKeyVaultUrl=(Get-AzureRmKeyVault -ResourceGroupName $rgName -Name $vaultName).VaultUri
$keyVaultResourceId=(Get-AzureRmKeyVault -ResourceGroupName $rgName -Name $vaultName).ResourceId

Set-AzureRmVmssDiskEncryptionExtension -ResourceGroupName $rgName -VMScaleSetName $vmssName `
    -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $keyVaultResourceId –VolumeType "All"
```

Wanneer u wordt gevraagd, typt u *y* om door te gaan met het versleutelingsproces schijf op de schaal ingesteld VM-exemplaren.

## <a name="check-encryption-progress"></a>Versleuteling voortgang controleren

Gebruiken om te controleren op de status van schijfversleuteling, [Get-AzureRmVmssDiskEncryption](/powershell/module/AzureRM.Compute/Get-AzureRmVmssDiskEncryption):

```azurepowershell-interactive
Get-AzureRmVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $vmssName
```

Wanneer de VM-exemplaren zijn versleuteld, de *EncryptionSummary* code rapporten *ProvisioningState/geslaagd* zoals weergegeven in de volgende voorbeelduitvoer:

```powershell
ResourceGroupName            : myResourceGroup
VmScaleSetName               : myScaleSet
EncryptionSettings           :
  KeyVaultURL                : https://myuniquekeyvault.vault.azure.net/
  KeyEncryptionKeyURL        :
  KeyVaultResourceId         : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.KeyVault/vaults/myuniquekeyvault
  KekVaultResourceId         :
  KeyEncryptionAlgorithm     :
  VolumeType                 : All
  EncryptionOperation        : EnableEncryption
EncryptionSummary[0]         :
  Code                       : ProvisioningState/succeeded
  Count                      : 2
EncryptionEnabled            : True
EncryptionExtensionInstalled : True
```

## <a name="disable-encryption"></a>Versleuteling uitschakelen

Als u niet langer wenst versleutelde VM-exemplaren schijven te gebruiken, kunt u uitschakelen versleuteling met [uitschakelen AzureRmVmssDiskEncryption](/powershell/module/AzureRM.Compute/Disable-AzureRmVmssDiskEncryption) als volgt:

```azurepowershell-interactive
Disable-AzureRmVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $vmssName
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel kunt u Azure PowerShell gebruikt voor het versleutelen van een virtuele-machineschaalset. U kunt ook de [Azure CLI 2.0](virtual-machine-scale-sets-encrypt-disks-cli.md) of sjablonen voor [Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-windows-jumpbox) of [Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-linux-jumpbox).
