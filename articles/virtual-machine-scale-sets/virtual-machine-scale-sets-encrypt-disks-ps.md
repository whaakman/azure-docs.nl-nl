---
title: Schijven voor schaalsets met Azure PowerShell voor Azure versleutelen | Microsoft Docs
description: Informatie over het gebruik van Azure PowerShell voor het versleutelen van VM-instanties en de gekoppelde schijven in een schaalset voor virtuele machine van Windows
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2019
ms.author: cynthn
ms.openlocfilehash: a582a4787a4b215d82dcbff60be8853793f92c32
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66728373"
---
# <a name="encrypt-os-and-attached-data-disks-in-a-virtual-machine-scale-set-with-azure-powershell"></a>Besturingssysteem en gekoppelde gegevensschijven in een virtuele-machineschaalset met Azure PowerShell versleutelen

Als u wilt beschermen en beveiligen van data-at-rest met behulp van de bedrijfstak versleutelingstechnologie, ondersteuning virtuele-machineschaalsets voor Azure Disk Encryption (ADE). Versleuteling kan worden ingeschakeld voor Windows en Linux virtuele machine schaalsets. Zie voor meer informatie, [Azure Disk Encryption voor Windows en Linux](../security/azure-security-disk-encryption.md).

Azure disk encryption wordt ondersteund:
- voor scale sets met beheerde schijven gemaakt en niet ondersteund voor systeemeigen (of niet-beheerde) schijf schaalsets.
- voor het besturingssysteem en volumes in Windows-schaalsets. Schakel versleuteling wordt ondersteund voor besturingssysteem en volumes voor Windows-schaalsets.
- voor de gegevensvolumes in Linux-schaalsets. OS-schijfversleuteling wordt niet ondersteund op dit moment voor Linux-schaalsets.

[!INCLUDE [updated-for-az.md](../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-an-azure-key-vault-enabled-for-disk-encryption"></a>Maak een Azure Key Vault is ingeschakeld voor schijfversleuteling

Azure Key Vault kunt opslaan, sleutels, geheimen of wachtwoorden waarmee u kunt ze veilig in uw toepassingen en services te implementeren. Cryptografische sleutels worden opgeslagen in Azure Key Vault met behulp van software-beveiliging, of u kunt importeren of genereer uw sleutels in Hardware Security Modules (HSM's) gecertificeerd voor FIPS 140-2 level 2 standaarden. Deze cryptografische sleutels worden gebruikt voor het versleutelen en ontsleutelen van virtuele schijven die zijn gekoppeld aan uw virtuele machine. U behoudt de controle van deze cryptografische sleutels en het gebruik ervan kunt controleren.

Maak een Key Vault met [nieuwe AzKeyVault](/powershell/module/az.keyvault/new-azkeyvault). Instellen als u wilt toestaan dat de Key Vault moet worden gebruikt voor versleuteling van schijf, de *EnabledForDiskEncryption* parameter. Het volgende voorbeeld definieert ook de variabelen voor de naam van resourcegroep, Key Vault-naam en locatie. Geef de naam van uw eigen unieke Key Vault:

```azurepowershell-interactive
$rgName="myResourceGroup"
$vaultName="myuniquekeyvault"
$location = "EastUS"

New-AzResourceGroup -Name $rgName -Location $location
New-AzKeyVault -VaultName $vaultName -ResourceGroupName $rgName -Location $location -EnabledForDiskEncryption
```

### <a name="use-an-existing-key-vault"></a>Gebruik een bestaande Key Vault

Deze stap is alleen vereist als u een bestaande Key Vault hebt die u wilt gebruiken met schijfversleuteling. Deze stap overslaan als u een Key Vault in de vorige sectie hebt gemaakt.

U kunt een bestaande Sleutelkluis in hetzelfde abonnement en dezelfde regio als de schaalset voor schijfversleuteling met inschakelen [Set AzKeyVaultAccessPolicy](/powershell/module/az.keyvault/Set-AzKeyVaultAccessPolicy). Bepaal de naam van uw bestaande Key Vault in de *$vaultName* variabele als volgt te werk:


```azurepowershell-interactive
$vaultName="myexistingkeyvault"
Set-AzKeyVaultAccessPolicy -VaultName $vaultName -EnabledForDiskEncryption
```

## <a name="create-a-scale-set"></a>Een schaalset maken

Stel eerst een beheerdersnaam en -wachtwoord in voor de VM-exemplaren met behulp van [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```azurepowershell-interactive
$cred = Get-Credential
```

Maak nu een virtuele-machineschaalset met behulp van [New-AzVmss](/powershell/module/az.compute/new-azvmss). Om het verkeer te distribueren naar de verschillende VM-exemplaren, wordt er ook een load balancer gemaakt. De load balancer bevat regels voor het distribueren van verkeer op TCP-poort 80, en voor het toestaan van extern bureaubladverkeer op TCP-poort 3389 en externe toegang via PowerShell op TCP-poort 5985:

```azurepowershell-interactive
$vmssName="myScaleSet"

New-AzVmss `
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

Voor het versleutelen van VM-exemplaren in een schaalset, moet u eerst wat informatie over de URI van Key Vault en resource-ID met ontvangen [Get-AzKeyVault](/powershell/module/az.keyvault/Get-AzKeyVault). Deze variabelen worden gebruikt om te beginnen klikt u vervolgens het versleutelingsproces met [Set AzVmssDiskEncryptionExtension](/powershell/module/az.compute/Set-AzVmssDiskEncryptionExtension):


```azurepowershell-interactive
$diskEncryptionKeyVaultUrl=(Get-AzKeyVault -ResourceGroupName $rgName -Name $vaultName).VaultUri
$keyVaultResourceId=(Get-AzKeyVault -ResourceGroupName $rgName -Name $vaultName).ResourceId

Set-AzVmssDiskEncryptionExtension -ResourceGroupName $rgName -VMScaleSetName $vmssName `
    -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $keyVaultResourceId –VolumeType "All"
```

Wanneer u hierom wordt gevraagd, typt u *y* om door te gaan met het versleutelingsproces schijf op de schaal ingesteld VM-exemplaren.

### <a name="enable-encryption-using-kek-to-wrap-the-key"></a>De sleutel inpakken met KEK-codering inschakelen

U kunt ook een coderingssleutel voor extra beveiliging gebruiken bij het versleutelen van de virtuele-machineschaalset.

```azurepowershell-interactive
$diskEncryptionKeyVaultUrl=(Get-AzKeyVault -ResourceGroupName $rgName -Name $vaultName).VaultUri
$keyVaultResourceId=(Get-AzKeyVault -ResourceGroupName $rgName -Name $vaultName).ResourceId
$keyEncryptionKeyUrl = (Get-AzKeyVaultKey -VaultName $vaultName -Name $keyEncryptionKeyName).Key.kid;

Set-AzVmssDiskEncryptionExtension -ResourceGroupName $rgName -VMScaleSetName $vmssName `
    -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $keyVaultResourceId `
    -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $keyVaultResourceId –VolumeType "All"
```

> [!NOTE]
>  De syntaxis voor de waarde van de schijf-versleuteling-keyvault-parameter is de volledige id-reeks:</br>
/subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br></br>
> De syntaxis voor de waarde van de encryptiesleutel parameter is de volledige URI naar de KEK-sleutel in:</br>
https://[keyvault-name].Vault.Azure.NET/Keys/[kekname]/[kek-Unique-ID]

## <a name="check-encryption-progress"></a>Versleuteling voortgang controleren

Gebruiken om te controleren of de status van schijfversleuteling, [Get-AzVmssDiskEncryption](/powershell/module/az.compute/Get-AzVmssDiskEncryption):


```azurepowershell-interactive
Get-AzVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $vmssName
```

Wanneer de VM-exemplaren zijn versleuteld, de *EncryptionSummary* code rapporten *ProvisioningState/geslaagd* zoals wordt weergegeven in de volgende voorbeelduitvoer:

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

## <a name="disable-encryption"></a>Schakel versleuteling uit

Als u niet meer gebruiken van versleutelde VM-exemplaren schijven wilt, kunt u versleuteling met uitschakelen [uitschakelen AzVmssDiskEncryption](/powershell/module/az.compute/Disable-AzVmssDiskEncryption) als volgt:


```azurepowershell-interactive
Disable-AzVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $vmssName
```

## <a name="next-steps"></a>Volgende stappen

- In dit artikel, kunt u Azure PowerShell gebruikt voor het versleutelen van een virtuele-machineschaalset. U kunt ook de [Azure CLI](virtual-machine-scale-sets-encrypt-disks-cli.md) of sjablonen voor [Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-windows-jumpbox) of [Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-linux-jumpbox).
- Als u wilt dat Azure Disk Encryption toegepast nadat een andere extensie is ingericht, kunt u [extensievolgorde](virtual-machine-scale-sets-extension-sequencing.md). U kunt [deze voorbeelden](../security/azure-security-disk-encryption-extension-sequencing.md#sample-azure-templates) aan de slag.
