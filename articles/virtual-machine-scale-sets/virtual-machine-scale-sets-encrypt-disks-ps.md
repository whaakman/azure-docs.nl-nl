---
title: Virtuele Machine van Azure-Schaalsets schijven versleutelen | Microsoft Docs
description: Informatie over het versleutelen van de gekoppelde schijven in virtuele-machineschaalsets.
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: iainfou
ms.openlocfilehash: dddcece9f7566961b256369330661e5dbd5d4665
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
---
# <a name="encrypt-os-and-attached-data-disks-in-a-virtual-machine-scale-set"></a>OS- en bijgesloten gegevensschijven in een virtuele-machineschaalset coderen
Azure [virtuele-machineschaalsets](/azure/virtual-machine-scale-sets/) biedt ondersteuning voor Azure disk encryption (ADE).  Azure-schijfversleuteling kan worden ingeschakeld voor Windows en Linux-virtuele-machineschaalsets beveiligen en bescherming van de schaal stelt gegevens in rust met toonaangevende standaard versleuteling technologie. Lees voor meer informatie Azure Disk Encryption for Windows- en Linux virtuele machines.

> [!NOTE]
>  Versleuteling van de Azure-schijf voor virtuele-machineschaalsets is momenteel in de openbare preview beschikbaar in alle openbare Azure-regio.

De schijf van Azure-versleuteling wordt ondersteund:
- voor schaal sets met beheerde schijven gemaakt en niet ondersteund voor systeemeigen (of niet-beheerde) schijf-schaalsets.
- voor OS- en gegevensvolumes in Windows-schaalsets. Schakel versleuteling wordt ondersteund voor OS- en gegevensvolumes voor Windows-schaalsets.
- voor de gegevensvolumes in Linux-schaalsets. OS-schijfversleuteling wordt niet ondersteund in het huidige voorbeeld voor Linux-schaalsets.

Scale set VM terugzetten van de installatiekopie en upgrade-bewerkingen worden niet ondersteund in de huidige preview. De Azure disk encryption voor virtuele machine scale sets preview wordt aanbevolen in een testomgeving. In de preview schijfversleuteling in productieomgevingen waar mogelijk moet u een installatiekopie van het besturingssysteem in een versleutelde schaalset upgrade niet is ingeschakeld.

## <a name="prerequisites"></a>Vereisten
Installeer de nieuwste versies van [Azure Powershell](https://github.com/Azure/azure-powershell/releases), die de versleuteling-opdrachten bevat.

De Azure disk encryption voor virtuele machine scale sets preview moet u uw abonnement met de volgende PowerShell-opdrachten zichzelf registreren: 

```powershell
Login-AzureRmAccount
Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Compute -FeatureName "UnifiedDiskEncryption"
```

Wacht ongeveer tien minuten totdat de status 'Geregistreerde' wordt geretourneerd door de volgende opdracht: 

```powershell
Get-AzureRmProviderFeature -ProviderNamespace "Microsoft.Compute" -FeatureName "UnifiedDiskEncryption"
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute
```

## <a name="create-an-azure-key-vault-enabled-for-disk-encryption"></a>Maken van een Azure sleutelkluis is ingeschakeld voor schijfversleuteling
Maak een nieuwe sleutelkluis in hetzelfde abonnement en dezelfde regio als de schaal instellen en stelt u het toegangsbeleid 'EnabledForDiskEncryption'.

```powershell
$rgname="windatadiskencryptiontest"
$VaultName="encryptionvault321"

New-AzureRmKeyVault -VaultName $VaultName -ResourceGroupName $rgName -Location southcentralus -EnabledForDiskEncryption
``` 

Of het inschakelen van een bestaande sleutelkluis in hetzelfde abonnement en dezelfde regio als de schaal voor schijfversleuteling instelt.

```powershell
$VaultName="encryptionvault321"
Set-AzureRmKeyVaultAccessPolicy -VaultName $VaultName -EnabledForDiskEncryption
```

## <a name="enable-encryption"></a>Versleuteling inschakelen
De volgende opdrachten versleutelen een gegevensschijf in een actieve schaal ingesteld met een sleutelkluis in dezelfde resourcegroep. U kunt ook sjablonen gebruiken voor het versleutelen van de schijven in een actieve [Windows schaalset](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-windows-jumpbox) of [Linux schaalset](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-linux-jumpbox).

```powershell
$rgname="windatadiskencryptiontest"
$VmssName="nt1vm"
$DiskEncryptionKeyVaultUrl="https://encryptionvault321.vault.azure.net"
$KeyVaultResourceId="/subscriptions/0754ecc2-d80d-426a-902c-b83f4cfbdc95/resourceGroups/windatadiskencryptiontest/providers/Microsoft.KeyVault/vaults/encryptionvault321"

Set-AzureRmVmssDiskEncryptionExtension -ResourceGroupName $rgName -VMScaleSetName $VmssName `
    -DiskEncryptionKeyVaultUrl $DiskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId –VolumeType Data
```

## <a name="check-encryption-progress"></a>Versleuteling voortgang controleren
Gebruik de volgende opdrachten om coderingsstatus van de schaalaanpassingsset weer te geven.

```powershell
$rgname="windatadiskencryptiontest"
$VmssName="nt1vm"
Get-AzureRmVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $VmssName

Get-AzureRmVmssVMDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $VmssName -InstanceId "4"
```

## <a name="disable-encryption"></a>Versleuteling uitschakelen
Schakel versleuteling in op een actieve virtuele-machineschaalset ingesteld met de volgende opdrachten uit. U kunt ook sjablonen gebruiken om uit te schakelen versleuteling uitgevoerd [Windows schaalset](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-vmss-windows) of [Linux schaalset](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-vmss-linux).

```powershell
$rgname="windatadiskencryptiontest"
$VmssName="nt1vm"
Disable-AzureRmVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $VmssName
```
