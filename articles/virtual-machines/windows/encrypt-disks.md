---
title: Schijven op een Windows-VM in Azure versleutelen | Microsoft Docs
description: Versleutelen van virtuele schijven op een Windows-VM voor verbeterde beveiliging met behulp van Azure PowerShell
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 10/30/2018
ms.author: cynthn
ms.openlocfilehash: 4875464d7e7a7f49c1532871a69f4d2224b271a6
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56108242"
---
# <a name="encrypt-virtual-disks-on-a-windows-vm"></a>Virtuele schijven op een Windows-VM versleutelen
Voor uitgebreide virtuele machine (VM) beveiliging en naleving, kunnen virtuele schijven in Azure worden versleuteld. Schijven worden versleuteld met behulp van cryptografische sleutels die worden beveiligd in een Azure Key Vault. U bepaalt deze cryptografische sleutels en het gebruik ervan kunt controleren. In dit artikel wordt beschreven hoe u virtuele schijven op een Windows-VM versleutelen met behulp van Azure PowerShell. U kunt ook [een Linux-VM versleutelen met behulp van de Azure CLI](../linux/encrypt-disks.md).

[!INCLUDE [updated-for-az-vm.md](../../../includes/updated-for-az-vm.md)]

## <a name="overview-of-disk-encryption"></a>Overzicht van schijfversleuteling
Virtuele schijven op Windows-VM's worden in rust versleuteld met BitLocker. Er zijn geen kosten voor het versleutelen van virtuele schijven in Azure. Cryptografische sleutels worden opgeslagen in een Azure Key Vault met behulp van software protection, of u kunt importeren of genereer uw sleutels in Hardware Security Modules (HSM's) gecertificeerd voor FIPS 140-2 level 2 standaarden. Cryptografische sleutels worden gebruikt voor het versleutelen en ontsleutelen van virtuele schijven die zijn gekoppeld aan uw virtuele machine. U behoudt de controle van deze cryptografische sleutels en het gebruik ervan kunt controleren. 

Het proces voor het versleutelen van een virtuele machine is als volgt:

1. Maak een cryptografische sleutel in een Azure Key Vault.
1. Configureer de cryptografische sleutel om te worden gebruikt voor het versleutelen van schijven.
1. Schijfversleuteling voor de virtuele schijven inschakelen.
1. De vereiste cryptografische sleutels worden aangevraagd vanuit Azure Key Vault.
1. De virtuele schijven worden versleuteld met behulp van de cryptografiesleutel van de opgegeven.


## <a name="requirements-and-limitations"></a>Vereisten en beperkingen

Ondersteunde scenario's en vereisten voor versleuteling van schijf:

* Inschakelen van versleuteling op nieuwe Windows-VM's van Azure Marketplace-installatiekopieën of aangepaste VHD-installatiekopieën.
* Inschakelen van versleuteling op bestaande Windows-VM's in Azure.
* Inschakelen van versleuteling op Windows-VM's die zijn geconfigureerd met behulp van opslagruimten.
* Uitschakelen van versleuteling op besturingssysteem en schijven voor Windows-VM's.
* Standard-laag virtuele machines, zoals een, D, DS, G en GS-serie VM's.

    > [!NOTE]
    > Alle resources (met inbegrip van de Key Vault, de Storage-account en de virtuele machine) moeten zich in dezelfde Azure-regio en -abonnement.

Schijfversleuteling wordt momenteel niet ondersteund in de volgende scenario's:

* Basic-laag virtuele machines.
* VM's gemaakt met behulp van het klassieke implementatiemodel.
* Bijwerken van de cryptografische sleutels op een VM al is versleuteld.
* Integratie met on-premises Key Management Service.


## <a name="create-an-azure-key-vault-and-keys"></a>Een Azure Key Vault en sleutels maken
Voordat u begint, zorg er dan voor dat de nieuwste versie van de Azure PowerShell-module is geïnstalleerd. Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview) voor meer informatie. In de volgende opdrachtvoorbeelden vervangen door alle voorbeeldparameters van uw eigen namen, locatie en sleutelwaarden die zijn, zoals *myResourceGroup*, *myKeyVault*, *myVM*, en enzovoorts bedekt.

De eerste stap is het maken van een Azure Key Vault voor het opslaan van uw cryptografische sleutels. Sleutelkluizen van Azure kunt opslaan, sleutels, geheimen of wachtwoorden waarmee u kunt ze veilig in uw toepassingen en services te implementeren. Voor de versleuteling van de virtuele schijf maakt u een Key Vault voor het opslaan van een cryptografische sleutel die wordt gebruikt om te versleutelen of ontsleutelen van de virtuele schijven. 

Inschakelen van de provider Azure Key Vault binnen uw Azure-abonnement met [registreren AzResourceProvider](https://docs.microsoft.com/powershell/module/az.resources/register-azresourceprovider), maakt u een resourcegroep met [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup). Het volgende voorbeeld wordt een Resourcegroepnaam *myResourceGroup* in de *VS-Oost* locatie:

```azurepowershell-interactive
$rgName = "myResourceGroup"
$location = "East US"

Register-AzResourceProvider -ProviderNamespace "Microsoft.KeyVault"
New-AzResourceGroup -Location $location -Name $rgName
```

De Azure Key Vault met de cryptografische sleutels en de bijbehorende rekenresources, zoals opslag en de virtuele machine zelf moeten zich allemaal in dezelfde regio. Maak een Azure Key Vault met [New-AzKeyVault](https://docs.microsoft.com/powershell/module/az.keyvault/new-azkeyvault) en de Key Vault voor gebruik met schijfversleuteling in te schakelen. Geef een unieke naam van de Key Vault voor *keyVaultName* als volgt:

```azurepowershell-interactive
$keyVaultName = "myKeyVault$(Get-Random)"
New-AzKeyVault -Location $location `
    -ResourceGroupName $rgName `
    -VaultName $keyVaultName `
    -EnabledForDiskEncryption
```

U kunt de cryptografische sleutels opslaan met behulp van software of Hardware Security Model (HSM)-beveiliging.  Softwarematige beveiligde sleutels worden alleen opgeslagen in een standard Key Vault. Met behulp van een HSM, moet een premium Key Vault een extra kosten. Voor het maken van een premium Key Vault, in de vorige stap toevoegen de *- Sku "Premium"* parameter. Het volgende voorbeeld wordt met software beschermde sleutels, omdat we een standaard Sleutelkluis hebt gemaakt. 

Voor beide beveiligingsmodellen moet het Azure-platform worden gemachtigd om aan te vragen van de cryptografische sleutels wanneer de virtuele machine wordt opgestart voor het ontsleutelen van de virtuele schijven. Maken van een cryptografische sleutel in uw Key Vault met [Add-AzureKeyVaultKey](https://docs.microsoft.com/powershell/module/az.keyvault/add-azkeyvaultkey). Het volgende voorbeeld wordt een sleutel met de naam *myKey*:

```azurepowershell-interactive
Add-AzureKeyVaultKey -VaultName $keyVaultName `
    -Name "myKey" `
    -Destination "Software"
```

## <a name="create-a-virtual-machine"></a>Een virtuele machine maken
Als u wilt testen het versleutelingsproces, maak een VM met [New-AzVm](https://docs.microsoft.com/powershell/module/az.compute/new-azvm). Het volgende voorbeeld wordt een virtuele machine met de naam *myVM* met behulp van een *Windows Server 2016 Datacenter* installatiekopie. Wanneer u hierom wordt gevraagd om referenties op te geven, voert u de gebruikersnaam en het wachtwoord moet worden gebruikt voor uw virtuele machine:

```azurepowershell-interactive
$cred = Get-Credential

New-AzVm `
    -ResourceGroupName $rgName `
    -Name "myVM" `
    -Location $location `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress" `
    -Credential $cred
```


## <a name="encrypt-a-virtual-machine"></a>Een virtuele-machine versleutelen
Versleutelen van uw virtuele machine met [Set AzVMDiskEncryptionExtension](https://docs.microsoft.com/powershell/module/az.compute/set-azvmdiskencryptionextension) met behulp van de Azure Key Vault-sleutel. Het volgende voorbeeld haalt alle gegevens van de sleutel en vervolgens versleutelt u de virtuele machine met de naam *myVM*:

```azurepowershell-interactive
$keyVault = Get-AzKeyVault -VaultName $keyVaultName -ResourceGroupName $rgName;
$diskEncryptionKeyVaultUrl = $keyVault.VaultUri;
$keyVaultResourceId = $keyVault.ResourceId;
$keyEncryptionKeyUrl = (Get-AzureKeyVaultKey -VaultName $keyVaultName -Name myKey).Key.kid;

Set-AzVMDiskEncryptionExtension -ResourceGroupName $rgName `
    -VMName "myVM" `
    -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl `
    -DiskEncryptionKeyVaultId $keyVaultResourceId `
    -KeyEncryptionKeyUrl $keyEncryptionKeyUrl `
    -KeyEncryptionKeyVaultId $keyVaultResourceId
```

De prompt om door te gaan met de versleuteling van de virtuele machine te accepteren. De virtuele machine opnieuw wordt opgestart tijdens het proces. Nadat de codering is voltooid en de virtuele machine opnieuw is opgestart, controleert u de versleutelingsstatus met [Get-AzVmDiskEncryptionStatus](https://docs.microsoft.com/powershell/module/az.compute/get-azvmdiskencryptionstatus):

```azurepowershell-interactive
Get-AzVmDiskEncryptionStatus  -ResourceGroupName $rgName -VMName "myVM"
```

De uitvoer lijkt op die in het volgende voorbeeld:

```azurepowershell-interactive
OsVolumeEncrypted          : Encrypted
DataVolumesEncrypted       : Encrypted
OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
ProgressMessage            : OsVolume: Encrypted, DataVolumes: Encrypted
```

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over het beheren van een Azure Key Vault [een Key Vault voor virtuele machines instellen](key-vault-setup.md).
* Zie voor meer informatie over schijfversleuteling, zoals het voorbereiden van een versleutelde aangepaste VM uploaden naar Azure, [Azure Disk Encryption](../../security/azure-security-disk-encryption.md).
