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
ms.date: 04/30/2018
ms.author: cynthn
ms.openlocfilehash: f4bf1f35a39042fa38e5d7c7e52c4f8593ffd824
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2018
ms.locfileid: "51683522"
---
# <a name="encrypt-os-and-attached-data-disks-in-a-virtual-machine-scale-set-with-azure-powershell-preview"></a>Versleutelen van OS- en gekoppelde gegevensschijven in een virtuele-machineschaalset met Azure PowerShell (Preview)

Als u wilt beschermen en beveiligen van data-at-rest met behulp van de bedrijfstak versleutelingstechnologie, ondersteuning virtuele-machineschaalsets voor Azure Disk Encryption (ADE). Versleuteling kan worden ingeschakeld voor Windows en Linux virtuele machine schaalsets. Zie voor meer informatie, [Azure Disk Encryption voor Windows en Linux](../security/azure-security-disk-encryption.md).

> [!NOTE]
>  Azure disk encryption voor virtuele-machineschaalsets is momenteel in openbare preview beschikbaar in alle Azure openbare regio's.

Azure disk encryption wordt ondersteund:
- voor scale sets met beheerde schijven gemaakt en niet ondersteund voor systeemeigen (of niet-beheerde) schijf schaalsets.
- voor het besturingssysteem en volumes in Windows-schaalsets. Schakel versleuteling wordt ondersteund voor besturingssysteem en volumes voor Windows-schaalsets.
- voor de gegevensvolumes in Linux-schaalsets. OS-schijfversleuteling wordt niet ondersteund in de huidige Preview-versie voor Linux-schaalsets.

Scale set VM terugzetten van een installatiekopie en upgrade-bewerkingen worden niet ondersteund in de huidige Preview-versie. De Azure disk encryption voor virtuele machine scale sets preview wordt aanbevolen alleen in een testomgeving. In de Preview-versie, schijfversleuteling in een productieomgeving waarin u wilt mogelijk bijwerken van een installatiekopie van het besturingssysteem in een versleutelde schaalset niet te schakelen.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Als u ervoor kiest om PowerShell lokaal te installeren en te gebruiken, moet u moduleversie 5.7.0 of hoger van Azure PowerShell gebruiken voor deze zelfstudie. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Login-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.

## <a name="register-for-disk-encryption-preview"></a>Registreren voor de preview van schijf-versleuteling

De Azure disk encryption voor virtuele-machineschaalsets Preview-versie, moet u uw abonnement met zelf registreren [Register-AzureRmProviderFeature](/powershell/module/azurerm.resources/register-azurermproviderfeature). U hoeft alleen de eerste keer dat u de preview-functie van de schijf-codering van de volgende stappen uitvoeren:

```azurepowershell-interactive
Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Compute -FeatureName "UnifiedDiskEncryption"
```

Het kan tot tien minuten voor de registratieaanvraag worden doorgegeven duren. U kunt controleren op de status van de apparaatregistratie met [Get-AzureRmProviderFeature](/powershell/module/AzureRM.Resources/Get-AzureRmProviderFeature). Wanneer de `RegistrationState` rapporten *geregistreerde*, Registreer opnieuw de *Microsoft.Compute* provider met [Register-AzureRmResourceProvider](/powershell/module/AzureRM.Resources/Register-AzureRmResourceProvider):

```azurepowershell-interactive
Get-AzureRmProviderFeature -ProviderNamespace "Microsoft.Compute" -FeatureName "UnifiedDiskEncryption"
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute
```

## <a name="create-an-azure-key-vault-enabled-for-disk-encryption"></a>Maak een Azure Key Vault is ingeschakeld voor schijfversleuteling

Azure Key Vault kunt opslaan, sleutels, geheimen of wachtwoorden waarmee u kunt ze veilig in uw toepassingen en services te implementeren. Cryptografische sleutels worden opgeslagen in Azure Key Vault met behulp van software-beveiliging, of u kunt importeren of genereer uw sleutels in Hardware Security Modules (HSM's) gecertificeerd voor FIPS 140-2 level 2 standaarden. Deze cryptografische sleutels worden gebruikt voor het versleutelen en ontsleutelen van virtuele schijven die zijn gekoppeld aan uw virtuele machine. U behoudt de controle van deze cryptografische sleutels en het gebruik ervan kunt controleren.

Maak een Key Vault met [nieuwe-AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault). Instellen als u wilt toestaan dat de Key Vault moet worden gebruikt voor versleuteling van schijf, de *EnabledForDiskEncryption* parameter. Het volgende voorbeeld definieert ook de variabelen voor de naam van resourcegroep, Key Vault-naam en locatie. Geef de naam van uw eigen unieke Key Vault:

```azurepowershell-interactive
$rgName="myResourceGroup"
$vaultName="myuniquekeyvault"
$location = "EastUS"

New-AzureRmResourceGroup -Name $rgName -Location $location
New-AzureRmKeyVault -VaultName $vaultName -ResourceGroupName $rgName -Location $location -EnabledForDiskEncryption
```

### <a name="use-an-existing-key-vault"></a>Gebruik een bestaande Key Vault

Deze stap is alleen vereist als u een bestaande Key Vault hebt die u wilt gebruiken met schijfversleuteling. Deze stap overslaan als u een Key Vault in de vorige sectie hebt gemaakt.

U kunt een bestaande Sleutelkluis in hetzelfde abonnement en dezelfde regio als de schaalset voor schijfversleuteling met inschakelen [Set-AzureRmKeyVaultAccessPolicy](/powershell/module/AzureRM.KeyVault/Set-AzureRmKeyVaultAccessPolicy). Bepaal de naam van uw bestaande Key Vault in de *$vaultName* variabele als volgt te werk:

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

Voor het versleutelen van VM-exemplaren in een schaalset, moet u eerst wat informatie over de URI van Key Vault en resource-ID met ontvangen [Get-AzureRmKeyVault](/powershell/module/AzureRM.KeyVault/Get-AzureRmKeyVault). Deze variabelen worden gebruikt om te beginnen klikt u vervolgens het versleutelingsproces met [Set-AzureRmVmssDiskEncryptionExtension](/powershell/module/AzureRM.Compute/Set-AzureRmVmssDiskEncryptionExtension):

```azurepowershell-interactive
$diskEncryptionKeyVaultUrl=(Get-AzureRmKeyVault -ResourceGroupName $rgName -Name $vaultName).VaultUri
$keyVaultResourceId=(Get-AzureRmKeyVault -ResourceGroupName $rgName -Name $vaultName).ResourceId

Set-AzureRmVmssDiskEncryptionExtension -ResourceGroupName $rgName -VMScaleSetName $vmssName `
    -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $keyVaultResourceId –VolumeType "All"
```

Wanneer u hierom wordt gevraagd, typt u *y* om door te gaan met het versleutelingsproces schijf op de schaal ingesteld VM-exemplaren.

## <a name="check-encryption-progress"></a>Versleuteling voortgang controleren

Gebruiken om te controleren of de status van schijfversleuteling, [Get-AzureRmVmssDiskEncryption](/powershell/module/AzureRM.Compute/Get-AzureRmVmssDiskEncryption):

```azurepowershell-interactive
Get-AzureRmVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $vmssName
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

Als u niet meer gebruiken van versleutelde VM-exemplaren schijven wilt, kunt u versleuteling met uitschakelen [Disable-AzureRmVmssDiskEncryption](/powershell/module/AzureRM.Compute/Disable-AzureRmVmssDiskEncryption) als volgt:

```azurepowershell-interactive
Disable-AzureRmVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $vmssName
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel, kunt u Azure PowerShell gebruikt voor het versleutelen van een virtuele-machineschaalset. U kunt ook de [Azure CLI](virtual-machine-scale-sets-encrypt-disks-cli.md) of sjablonen voor [Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-windows-jumpbox) of [Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-linux-jumpbox).
