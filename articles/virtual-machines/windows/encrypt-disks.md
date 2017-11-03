---
title: Schijven op een Windows-VM in Azure versleutelen | Microsoft Docs
description: Het coderen van virtuele schijven op een virtuele machine van Windows voor een betere beveiliging met Azure PowerShell
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 07/10/2017
ms.author: iainfou
ms.openlocfilehash: 98b42b252a601af090579e3939f3c7ab91c3803b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-encrypt-virtual-disks-on-a-windows-vm"></a>Het coderen van virtuele schijven op een virtuele machine van Windows
Voor de uitgebreide virtuele machine (VM) beveiliging en naleving, kunnen virtuele schijven in Azure worden versleuteld. Schijven worden versleuteld met behulp van de cryptografische sleutels die worden beveiligd in een Azure Sleutelkluis. U kunt het gebruik ervan controleren en beheren van deze cryptografische sleutels. Dit artikel wordt uitgelegd hoe u voor het versleutelen van virtuele schijven op een Windows-VM met Azure PowerShell. U kunt ook [versleutelen van een Linux-VM met de Azure CLI 2.0](../linux/encrypt-disks.md).

## <a name="overview-of-disk-encryption"></a>Overzicht van schijfversleuteling
Virtuele schijven op Windows-VM's zijn versleuteld in rust met Bitlocker. Er zijn geen kosten voor het versleutelen van virtuele schijven in Azure. Cryptografische sleutels worden opgeslagen in Azure Key Vault met software-beveiliging, of u kunt importeren of genereren van uw sleutels in Hardware Security Modules (HSM's) die is gecertificeerd voor FIPS 140-2 level 2-standaarden. Deze cryptografische sleutels worden gebruikt voor het versleutelen en ontsleutelen van virtuele schijven gekoppeld aan uw virtuele machine. U beheer behouden over deze cryptografische sleutels en het gebruik ervan kunt controleren. Een Azure Active Directory service-principal biedt een veilige mechanisme voor het uitgeven van deze cryptografische sleutels als virtuele machines van stroom in- of uitschakelen voorzien worden.

Het proces voor het versleutelen van een virtuele machine is als volgt:

1. Maak een cryptografische sleutel in een Azure Sleutelkluis.
2. Configureer de cryptografische sleutel om te worden gebruikt voor het versleutelen van schijven.
3. De cryptografische sleutel om uit te lezen Azure Sleutelkluis, maak een Azure Active Directory service principal met de juiste machtigingen.
4. Geef de opdracht voor het versleutelen van uw virtuele schijven, opgeven van de Azure Active Directory service principal en de juiste cryptografische sleutel moet worden gebruikt.
5. De Azure Active Directory-service-principal vraagt de cryptografiesleutel van de vereiste van Azure Sleutelkluis.
6. De virtuele schijven worden versleuteld met behulp van de cryptografiesleutel van de opgegeven.

## <a name="encryption-process"></a>Versleutelingsproces
Schijfversleuteling is afhankelijk van de volgende extra onderdelen:

* **Azure Sleutelkluis** : wordt gebruikt ter bescherming van de cryptografische sleutels en geheimen gebruikt voor het proces van de codering/decodering schijf. 
  * Als dit bestaat, kunt u een bestaande Azure Sleutelkluis. U hoeft niet te reserveren van een Sleutelkluis voor het versleutelen van schijven.
  * Afzonderlijke administratieve grenzen en sleutel zichtbaarheid, kunt u een speciale Sleutelkluis.
* **Azure Active Directory** -het veilig uitwisselen van vereiste cryptografische sleutels en verificatie voor de aangevraagde acties worden verwerkt. 
  * Doorgaans kunt u een bestaand Azure Active Directory-exemplaar waarin de toepassing.
  * De service-principal biedt een veilige mechanisme om te vragen en de juiste cryptografische sleutels worden uitgegeven. U ontwikkelt een werkelijke toepassing die is geïntegreerd met Azure Active Directory niet.

## <a name="requirements-and-limitations"></a>Vereisten en beperkingen
Ondersteunde scenario's en vereisten voor de schijfversleuteling:

* Inschakelen van versleuteling op nieuwe Windows VM's van Azure Marketplace-installatiekopieën of aangepaste VHD-installatiekopie.
* Inschakelen van versleuteling op bestaande Windows virtuele machines in Azure.
* Inschakelen van versleuteling op Windows-virtuele machines die zijn geconfigureerd met behulp van opslagruimten.
* Het uitschakelen van versleuteling op besturingssysteem en stations voor VM's van Windows.
* Alle resources (zoals Sleutelkluis, een opslagaccount en een VM) moeten zich in dezelfde Azure-regio en abonnement.
* Standard-laag VM's, zoals een, D, DS, G en GS-serie virtuele machines.

Schijfversleuteling is momenteel niet ondersteund in de volgende scenario's:

* Basisstaffel virtuele machines.
* Virtuele machines gemaakt met behulp van het klassieke implementatiemodel.
* Bijwerken van de cryptografische sleutels op een VM al is versleuteld.
* Integratie met on-premises-Service voor sleutelbeheer.

## <a name="create-azure-key-vault-and-keys"></a>Azure Sleutelkluis en de sleutels maken
Voor u begint, moet u ervoor zorgen dat de nieuwste versie van de Azure PowerShell-module is geïnstalleerd. Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview) voor meer informatie. In de opdrachtvoorbeelden vervangen door alle voorbeeldparameters uw eigen namen, locatie en sleutelwaarden. Een conventie voor gebruik van de volgende voorbeelden *myResourceGroup*, *myKeyVault*, *myVM*, enzovoort.

De eerste stap is het maken van een Azure Key Vault voor het opslaan van uw cryptografische sleutels. Azure Sleutelkluis kunt opslaan sleutels, geheimen of wachtwoorden waarmee u kunt ze veilig implementeert in uw toepassingen en services. Voor versleuteling van de virtuele schijf, moet u een Sleutelkluis voor het opslaan van een cryptografische sleutel die wordt gebruikt voor het versleutelen of ontsleutelen van de virtuele schijven maken. 

Inschakelen van de provider voor Azure Sleutelkluis in uw Azure-abonnement met [registreren AzureRmResourceProvider](/powershell/module/azurerm.resources/register-azurermresourceprovider), maakt u een resourcegroep met [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Het volgende voorbeeld wordt een Resourcegroepnaam *myResourceGroup* in de *VS-Oost* locatie:

```powershell
$rgName = "myResourceGroup"
$location = "East US"

Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.KeyVault"
New-AzureRmResourceGroup -Location $location -Name $rgName
```

De Azure Sleutelkluis die de cryptografische sleutels en de bijbehorende compute-bronnen zoals opslag en de virtuele machine zelf moet zich bevinden in dezelfde regio. Maken van een Azure Key Vault met [New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault) en inschakelen van de Sleutelkluis voor gebruik met schijfversleuteling. Geef een unieke naam van de Sleutelkluis voor *keyVaultName* als volgt:

```powershell
$keyVaultName = "myUniqueKeyVaultName"
New-AzureRmKeyVault -Location $location `
    -ResourceGroupName $rgName `
    -VaultName $keyVaultName `
    -EnabledForDiskEncryption
```

U kunt met behulp van software of Hardware Security Model (HSM) protection cryptografiesleutels opslaan. Een HSM, moet een premium Sleutelkluis. Er is een extra kosten voor het maken van een premium Sleutelkluis in plaats van standaard Sleutelkluis waarmee softwarematige beveiligde sleutels worden opgeslagen. Voor het maken van een premium Sleutelkluis in de vorige stap voegt u de *- Sku 'Premium'* parameters. Het volgende voorbeeld wordt de softwarematige beveiligde sleutels omdat er een standaard Sleutelkluis hebt gemaakt. 

Voor beide beveiligingsmodellen moet het Azure-platform toegang om aan te vragen van de cryptografische sleutels wanneer de virtuele machine wordt opgestart voor het ontsleutelen van de virtuele schijven worden verleend. Maken van een cryptografische sleutel in uw Sleutelkluis met [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey). Het volgende voorbeeld wordt een sleutel met de naam *myKey*:

```powershell
Add-AzureKeyVaultKey -VaultName $keyVaultName `
    -Name "myKey" `
    -Destination "Software"
```


## <a name="create-the-azure-active-directory-service-principal"></a>De Azure Active Directory-service-principal maken
Wanneer virtuele schijven worden versleuteld en ontsleuteld, kunt u een account voor het afhandelen van de verificatie- en uitwisselen van cryptografische sleutels uit Sleutelkluis opgeven. Dit account, een Azure Active Directory service-principal kunt de Azure-platform om aan te vragen van de juiste cryptografische sleutels namens de virtuele machine. Een standaardexemplaar van de Azure Active Directory is beschikbaar in uw abonnement, hoewel veel organisaties hebben speciale mappen van Azure Active Directory.

Een service-principal maken in Azure Active Directory met [nieuw AzureRmADServicePrincipal](/powershell/module/azurerm.resources/new-azurermadserviceprincipal). Als u een beveiligd wachtwoord, volgt u de [wachtwoordbeleid en -beperkingen in Azure Active Directory](../../active-directory/active-directory-passwords-policy.md):

```powershell
$appName = "My App"
$securePassword = "P@ssword!"
$app = New-AzureRmADApplication -DisplayName $appName `
    -HomePage "https://myapp.contoso.com" `
    -IdentifierUris "https://contoso.com/myapp" `
    -Password $securePassword
New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId
```

Om deel te versleutelen of ontsleutelen van virtuele schijven, moeten de machtigingen voor de cryptografische sleutel die is opgeslagen in de Sleutelkluis toe te staan van de Azure Active Directory-service-principal te lezen van de sleutels worden ingesteld. Machtigingen instellen voor uw Sleutelkluis met [Set-AzureRmKeyVaultAccessPolicy](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy):

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName $keyvaultName `
    -ServicePrincipalName $app.ApplicationId `
    -PermissionsToKeys "WrapKey" `
    -PermissionsToSecrets "Set"
```


## <a name="create-virtual-machine"></a>Virtuele machine maken
Als u wilt testen het versleutelingsproces, gaan we een virtuele machine maken. Het volgende voorbeeld wordt een virtuele machine met de naam *myVM* met behulp van een *Windows Server 2016 Datacenter* afbeelding:

```powershell
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix 192.168.1.0/24

$vnet = New-AzureRmVirtualNetwork -ResourceGroupName $rgName `
    -Location $location `
    -Name myVnet `
    -AddressPrefix 192.168.0.0/16 `
    -Subnet $subnetConfig

$pip = New-AzureRmPublicIpAddress -ResourceGroupName $rgName `
    -Location $location `
    -AllocationMethod Static `
    -IdleTimeoutInMinutes 4 `
    -Name "mypublicdns$(Get-Random)"

$nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleRDP `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 1000 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 3389 `
    -Access Allow

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName `
    -Location $location `
    -Name myNetworkSecurityGroup `
    -SecurityRules $nsgRuleRDP

$nic = New-AzureRmNetworkInterface -Name myNic `
    -ResourceGroupName $rgName `
    -Location $location `
    -SubnetId $vnet.Subnets[0].Id `
    -PublicIpAddressId $pip.Id `
    -NetworkSecurityGroupId $nsg.Id

$cred = Get-Credential

$vmName = "myVM"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize Standard_D1 | `
Set-AzureRmVMOperatingSystem -Windows -ComputerName myVM -Credential $cred | `
Set-AzureRmVMSourceImage -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer -Skus 2016-Datacenter -Version latest | `
Add-AzureRmVMNetworkInterface -Id $nic.Id

New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vmConfig
```


## <a name="encrypt-virtual-machine"></a>Virtuele machine versleutelen
Voor het versleutelen van de virtuele schijven, brengt u samen de eerdere onderdelen:

1. Geef de Azure Active Directory-service-principal en het wachtwoord.
2. Geef de Sleutelkluis voor het opslaan van de metagegevens van de versleutelde schijven.
3. Geef de cryptografische sleutels moet worden gebruikt voor de daadwerkelijke versleuteling en ontsleuteling.
4. Geef op of u wilt versleutelen schijf met het besturingssysteem, de gegevensschijven of alle.

Versleutelen van uw virtuele machine met [Set AzureRmVMDiskEncryptionExtension](/powershell/module/azurerm.compute/set-azurermvmdiskencryptionextension) met behulp van de sleutel voor Azure Sleutelkluis en de Azure Active Directory-service-principal referenties. Het volgende voorbeeld haalt alle gegevens van de sleutel vervolgens de virtuele machine met de naam versleutelt *myVM*:

```powershell
$keyVault = Get-AzureRmKeyVault -VaultName $keyVaultName -ResourceGroupName $rgName;
$diskEncryptionKeyVaultUrl = $keyVault.VaultUri;
$keyVaultResourceId = $keyVault.ResourceId;
$keyEncryptionKeyUrl = (Get-AzureKeyVaultKey -VaultName $keyVaultName -Name myKey).Key.kid;

Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgName `
    -VMName $vmName `
    -AadClientID $app.ApplicationId `
    -AadClientSecret $securePassword `
    -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl `
    -DiskEncryptionKeyVaultId $keyVaultResourceId `
    -KeyEncryptionKeyUrl $keyEncryptionKeyUrl `
    -KeyEncryptionKeyVaultId $keyVaultResourceId
```

Accepteer de vraag om door te gaan met het VM-versleuteling. De virtuele machine opnieuw wordt opgestart tijdens het proces. Nadat de codering is voltooid en de virtuele machine opnieuw is opgestart, controleert u de versleutelingsstatus met [Get-AzureRmVmDiskEncryptionStatus](/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus):

```powershell
Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $rgName -VMName $vmName
```

De uitvoer lijkt op die in het volgende voorbeeld:

```powershell
OsVolumeEncrypted          : Encrypted
DataVolumesEncrypted       : Encrypted
OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
ProgressMessage            : OsVolume: Encrypted, DataVolumes: Encrypted
```

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over het beheren van Azure Sleutelkluis [Sleutelkluis instellen voor virtuele machines](key-vault-setup.md).
* Zie voor meer informatie over schijfversleuteling, zoals het voorbereiden van een versleutelde aangepaste VM uploaden naar Azure, [Azure Disk Encryption](../../security/azure-security-disk-encryption.md).
