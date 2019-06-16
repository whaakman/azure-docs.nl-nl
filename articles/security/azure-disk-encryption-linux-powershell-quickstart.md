---
title: Maak en Versleutel een Linux-VM met Azure Powershell
description: In deze snelstartgids leert u hoe u Azure Powershell gebruiken om te maken en een virtuele Linux-machine versleutelen
author: msmbaldwin
ms.author: mbaldwin
ms.service: security
ms.topic: quickstart
ms.date: 05/17/2019
ms.openlocfilehash: 7c44ffefccd4d78713d29aa2f9ab6006b8ba819e
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67081481"
---
# <a name="quickstart-create-and-encrypt-a-linux-virtual-machine-in-azure-with-powershell"></a>Quickstart: Maken en een Linux-machine versleutelen in Azure met PowerShell

De Azure PowerShell-module wordt gebruikt voor het maken en beheren van Azure-resources vanaf de PowerShell-opdrachtregel of in scripts. Deze quickstart laat zien hoe u een Linux-machine (VM) maken, het maken van een Key Vault voor de opslag van versleutelingssleutels en het versleutelen van de virtuele machine met de Azure PowerShell-module. In deze snelstartgids maakt gebruik van de installatiekopie van het Ubuntu 16.04 LTS-marketplace van Canonical en de grootte van een virtuele machine Standard_D2S_V3. 

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een Azure-resourcegroep met behulp van de opdracht [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup). Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd:

```powershell
New-AzResourceGroup -Name "myResourceGroup" -Location "EastUS"
```

## <a name="create-a-virtual-machine"></a>Een virtuele machine maken

Maken van een Azure-machine met [New-AzVM](/powershell/module/az.compute/new-azvm), de VM-configuratie wordt doorgegeven aan het object dat u hierboven hebt gemaakt.

```powershell
$securePassword = ConvertTo-SecureString 'AZUREuserPA$$W0RD' -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential ("azureuser", $securePassword)

New-AzVM -Name MyVm -Credential $cred -ResourceGroupName MyResourceGroup -Image Canonical:UbuntuServer:16.04-LTS:latest  -Size Standard_D2S_V3
```

Het duurt een paar minuten voor uw virtuele machine is geïmplementeerd. 

## <a name="create-a-key-vault-configured-for-encryption-keys"></a>Maak een Key Vault geconfigureerd voor versleutelingssleutels

Azure disk encryption slaat de versleutelingssleutel in een Azure Key Vault. Maak een Key Vault met [nieuwe AzKeyvault](/powershell/module/az.keyvault/new-azkeyvault). Als u wilt de Key Vault voor het opslaan van versleutelingssleutels inschakelen, gebruikt u de parameter - EnabledForDiskEncryption.

> [!Important]
> Elke Key Vault moet een unieke naam hebben. Het volgende voorbeeld wordt een Key Vault met de naam *myKV*, maar u moet de naam die iets anders.

```powershell
New-AzKeyvault -name MyKV -ResourceGroupName myResourceGroup -Location EastUS -EnabledForDiskEncryption
```

## <a name="encrypt-the-virtual-machine"></a>De virtuele machine versleutelen

Versleutelen van uw virtuele machine met [Set AzVmDiskEncryptionExtension](/powershell/module/az.compute/set-azvmdiskencryptionextension). 

Set-AzVmDiskEncryptionExtension u moet enkele waarden uit uw Key Vault-object. U kunt deze waarden verkrijgt door door te geven van de unieke naam van uw key vault [Get-AzKeyvault](/powershell/module/az.keyvault/get-azkeyvault).

```powershell
$KeyVault = Get-AzKeyVault -VaultName MyKV -ResourceGroupName MyResourceGroup

Set-AzVMDiskEncryptionExtension -ResourceGroupName MyResourceGroup -VMName MyVM -DiskEncryptionKeyVaultUrl $KeyVault.VaultUri -DiskEncryptionKeyVaultId $KeyVault.ResourceId -SkipVmBackup -VolumeType All
```

Na een paar minuten wordt het proces voor het volgende geretourneerd:

```
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK
```

U kunt het versleutelingsproces controleren door te voeren [Get-AzVmDiskEncryptionStatus](/powershell/module/az.compute/Get-AzVMDiskEncryptionStatus).

```powershell
Get-AzVmDiskEncryptionStatus -VMName MyVM -ResourceGroupName MyResourceGroup
```

Als versleuteling is ingeschakeld, ziet u het volgende in de resulterende uitvoer:

```
OsVolumeEncrypted          : EncryptionInProgress
DataVolumesEncrypted       : NotMounted
OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
ProgressMessage            : OS disk encryption started
```

## <a name="clean-up-resources"></a>Resources opschonen

U kunt de cmdlet [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) gebruiken om de resourcegroep, de VM en alle gerelateerde resources te verwijderen wanneer u ze niet meer nodig hebt:

```powershell
Remove-AzResourceGroup -Name "myResourceGroup"
```

## <a name="next-steps"></a>Volgende stappen

In deze quickstart maakt u een virtuele machine, een Sleutelkluis die is ingeschakeld voor versleutelingssleutels en de virtuele machine versleuteld gemaakt gemaakt.  Ga naar het volgende artikel voor meer informatie over de vereisten voor Azure Disk Encryption voor IaaS-VM's.

> [!div class="nextstepaction"]
> [Vereisten voor Azure Disk Encryption](azure-security-disk-encryption-prerequisites.md)