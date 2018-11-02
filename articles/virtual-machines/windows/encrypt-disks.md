---
title: Schijven op een Windows-VM in Azure versleutelen | Microsoft Docs
description: Versleutelen van virtuele schijven op een Windows-VM voor verbeterde beveiliging met Azure PowerShell
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
ms.openlocfilehash: 28bb4c9fd4827f534c5f7bac2bae15451e3ca16c
ms.sourcegitcommit: 799a4da85cf0fec54403688e88a934e6ad149001
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2018
ms.locfileid: "50914699"
---
# <a name="how-to-encrypt-virtual-disks-on-a-windows-vm"></a>Virtuele schijven op een Windows-VM versleutelen
Voor uitgebreide virtuele machine (VM) beveiliging en naleving, kunnen virtuele schijven in Azure worden versleuteld. Schijven worden versleuteld met behulp van cryptografische sleutels die worden beveiligd in een Azure Key Vault. U bepaalt deze cryptografische sleutels en het gebruik ervan kunt controleren. Dit artikel wordt uitgelegd hoe u voor het versleutelen van virtuele schijven op een Windows-VM met behulp van Azure PowerShell. U kunt ook [versleutelen van een Linux-VM met de Azure CLI](../linux/encrypt-disks.md).

## <a name="overview-of-disk-encryption"></a>Overzicht van schijfversleuteling

Virtuele schijven op Windows-VM's worden in rust versleuteld met Bitlocker. Er zijn geen kosten voor het versleutelen van virtuele schijven in Azure. Cryptografische sleutels worden opgeslagen in Azure Key Vault met behulp van software-beveiliging, of u kunt importeren of genereer uw sleutels in Hardware Security Modules (HSM's) gecertificeerd voor FIPS 140-2 level 2 standaarden. Deze cryptografische sleutels worden gebruikt voor het versleutelen en ontsleutelen van virtuele schijven die zijn gekoppeld aan uw virtuele machine. U behoudt de controle van deze cryptografische sleutels en het gebruik ervan kunt controleren. 

Het proces voor het versleutelen van een virtuele machine is als volgt:

1. Maak een cryptografische sleutel in een Azure Key Vault.
1. Configureer de cryptografische sleutel om te worden gebruikt voor het versleutelen van schijven.
1. Schijfversleuteling voor de virtuele schijven inschakelen.
1. De vereiste cryptografische sleutels worden aangevraagd vanuit Azure Key Vault.
1. De virtuele schijven worden versleuteld met behulp van de cryptografiesleutel van de opgegeven.

### <a name="azure-key-vault"></a>Azure Key Vault

Schijfversleuteling is afhankelijk van [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis). Key Vault wordt gebruikt om cryptografische sleutels en geheimen die worden gebruikt voor het proces van de versleuteling/ontsleuteling schijf te beveiligen. 
  * Als er een bestaat, kunt u een bestaande Azure Key Vault. U hoeft niet te reserveren van een Key Vault voor het versleutelen van schijven.
  * Afzonderlijke administratieve grenzen en de zichtbaarheid van de sleutel, kunt u een toegewezen Sleutelkluis maken.


## <a name="requirements-and-limitations"></a>Vereisten en beperkingen

Ondersteunde scenario's en vereisten voor versleuteling van schijf:

* Inschakelen van versleuteling op nieuwe Windows-VM's van Azure Marketplace-installatiekopieën of aangepaste VHD-installatiekopie.
* Inschakelen van versleuteling op bestaande Windows-VM's in Azure.
* Inschakelen van versleuteling op Windows-VM's die zijn geconfigureerd met behulp van opslagruimten.
* Uitschakelen van versleuteling op besturingssysteem en schijven voor Windows-VM's.
* Alle resources (zoals Key Vault, een Storage-account en een virtuele machine) moeten zich in dezelfde Azure-regio en -abonnement.
* Standard-laag virtuele machines, zoals een, D, DS, G en GS-serie VM's.

Schijfversleuteling wordt momenteel niet ondersteund in de volgende scenario's:

* Basic-laag virtuele machines.
* Virtuele machines die zijn gemaakt met het klassieke implementatiemodel.
* Bijwerken van de cryptografische sleutels op een VM al is versleuteld.
* Integratie met on-premises Key Managementservice.

## <a name="create-azure-key-vault-and-keys"></a>Azure Key Vault en sleutels maken

Voor u begint, moet u ervoor zorgen dat de nieuwste versie van de Azure PowerShell-module is geïnstalleerd. Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview) voor meer informatie. Vervang in de opdrachtvoorbeelden is de alle voorbeeldparameters met uw eigen namen, locatie en sleutelwaarden die zijn. De volgende voorbeelden gebruiken een conventie *myResourceGroup*, *myKeyVault*, *myVM*, enzovoort.

De eerste stap is het maken van een Azure Key Vault voor het opslaan van uw cryptografische sleutels. Azure Key Vault kunt opslaan, sleutels, geheimen of wachtwoorden waarmee u kunt ze veilig in uw toepassingen en services te implementeren. Voor versleuteling van de virtuele schijf maakt u een Sleutelkluis voor het opslaan van een cryptografische sleutel die wordt gebruikt om te versleutelen of ontsleutelen van de virtuele schijven. 

Inschakelen van de provider Azure Key Vault binnen uw Azure-abonnement met [Register-AzureRmResourceProvider](/powershell/module/azurerm.resources/register-azurermresourceprovider), maakt u een resourcegroep met [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Het volgende voorbeeld wordt een Resourcegroepnaam *myResourceGroup* in de *VS-Oost* locatie:

```azurepowershell-interactive
$rgName = "myResourceGroup"
$location = "East US"

Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.KeyVault"
New-AzureRmResourceGroup -Location $location -Name $rgName
```

De Azure Key Vault met de cryptografische sleutels en de bijbehorende rekenresources, zoals opslag en de virtuele machine zelf moet zich in dezelfde regio bevinden. Maak een Azure Key Vault met [New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault) en de Key Vault voor gebruik met schijfversleuteling in te schakelen. Geef een unieke naam van de Key Vault voor *keyVaultName* als volgt:

```azurepowershell-interactive
$keyVaultName = "myKeyVault$(Get-Random)"
New-AzureRmKeyVault -Location $location `
    -ResourceGroupName $rgName `
    -VaultName $keyVaultName `
    -EnabledForDiskEncryption
```

U kunt de cryptografische sleutels met behulp van software of Hardware Security Model (HSM) beveiliging opslaan. Met behulp van een HSM, is een premium Key Vault vereist. Er is een extra kosten voor het maken van een premium Key Vault in plaats van standard Key Vault waarin software beschermde sleutels. Voor het maken van een premium Key Vault, in de vorige stap toevoegen de *- Sku "Premium"* parameters. Het volgende voorbeeld wordt met software beschermde sleutels, omdat we een standaard Sleutelkluis hebt gemaakt. 

Voor beide beveiligingsmodellen moet het Azure-platform worden gemachtigd om aan te vragen van de cryptografische sleutels wanneer de virtuele machine wordt opgestart voor het ontsleutelen van de virtuele schijven. Maken van een cryptografische sleutel in uw Key Vault met [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey). Het volgende voorbeeld wordt een sleutel met de naam *myKey*:

```azurepowershell-interactive
Add-AzureKeyVaultKey -VaultName $keyVaultName `
    -Name "myKey" `
    -Destination "Software"
```

## <a name="create-virtual-machine"></a>Virtuele machine maken
Als u wilt testen het versleutelingsproces, maak een VM met [New-AzureRmVm](/powershell/module/azurerm.compute/new-azurermvm). Het volgende voorbeeld wordt een virtuele machine met de naam *myVM* met behulp van een *Windows Server 2016 Datacenter* installatiekopie. Wanneer u hierom wordt gevraagd om referenties op te geven, voert u de gebruikersnaam en het wachtwoord moet worden gebruikt voor uw virtuele machine:

```azurepowershell-interactive
$cred = Get-Credential

New-AzureRmVm `
    -ResourceGroupName $rgName `
    -Name "myVM" `
    -Location $location `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress" `
    -Credential $cred
```


## <a name="encrypt-virtual-machine"></a>Virtuele machine versleutelen
Voor het versleutelen van de virtuele schijven, breng u samen de eerdere onderdelen:

1. De service-principal voor Azure Active Directory en het wachtwoord opgeven.
2. Geef de Key Vault voor het opslaan van de metagegevens van de versleutelde schijven.
3. Geef de cryptografische sleutels te gebruiken voor de daadwerkelijke versleuteling en ontsleuteling.
4. Geef op of u wilt voor het versleutelen van de besturingssysteemschijf, schijven of alle.

Versleutelen van uw virtuele machine met [Set-AzureRmVMDiskEncryptionExtension](/powershell/module/azurerm.compute/set-azurermvmdiskencryptionextension) met behulp van de Azure Key Vault-sleutel en de referenties voor Azure Active Directory service-principal. Het volgende voorbeeld haalt alle gegevens van de sleutel en vervolgens versleutelt u de virtuele machine met de naam *myVM*:

```azurepowershell-interactive
$keyVault = Get-AzureRmKeyVault -VaultName $keyVaultName -ResourceGroupName $rgName;
$diskEncryptionKeyVaultUrl = $keyVault.VaultUri;
$keyVaultResourceId = $keyVault.ResourceId;
$keyEncryptionKeyUrl = (Get-AzureKeyVaultKey -VaultName $keyVaultName -Name myKey).Key.kid;

Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgName `
    -VMName "myVM" `
    -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl `
    -DiskEncryptionKeyVaultId $keyVaultResourceId `
    -KeyEncryptionKeyUrl $keyEncryptionKeyUrl `
    -KeyEncryptionKeyVaultId $keyVaultResourceId
```

De prompt om door te gaan met de versleuteling van de virtuele machine te accepteren. De virtuele machine opnieuw wordt opgestart tijdens het proces. Nadat de codering is voltooid en de virtuele machine opnieuw is opgestart, controleert u de versleutelingsstatus met [Get-AzureRmVmDiskEncryptionStatus](/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus):

```azurepowershell-interactive
Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $rgName -VMName "myVM"
```

De uitvoer lijkt op die in het volgende voorbeeld:

```azurepowershell-interactive
OsVolumeEncrypted          : Encrypted
DataVolumesEncrypted       : Encrypted
OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
ProgressMessage            : OsVolume: Encrypted, DataVolumes: Encrypted
```

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over het beheren van Azure Key Vault [Key Vault instellen voor virtuele machines](key-vault-setup.md).
* Zie voor meer informatie over schijfversleuteling, zoals het voorbereiden van een versleutelde aangepaste VM uploaden naar Azure, [Azure Disk Encryption](../../security/azure-security-disk-encryption.md).
