---
title: Disk encryption inschakelen voor Azure Service Fabric Linux-clusters | Microsoft Docs
description: In dit artikel wordt beschreven hoe u het inschakelen van disk encryption voor Azure Service Fabric-clusterknooppunten in Linux met behulp van Azure Resource Manager en Azure Key Vault.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: navya
ms.assetid: 15d0ab67-fc66-4108-8038-3584eeebabaa
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/22/2019
ms.author: aljo
ms.openlocfilehash: 47b07188d1757708fb494c6a66e93379657e806a
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/28/2019
ms.locfileid: "66258768"
---
# <a name="enable-disk-encryption-for-azure-service-fabric-cluster-nodes-in-linux"></a>Inschakelen van disk encryption voor Azure Service Fabric-clusterknooppunten in Linux 
> [!div class="op_single_selector"]
> * [Schijfversleuteling voor Linux](service-fabric-enable-azure-disk-encryption-linux.md)
> * [Schijfversleuteling voor Windows](service-fabric-enable-azure-disk-encryption-windows.md)
>
>

In deze zelfstudie leert u hoe u om in te schakelen schijfversleuteling voor de Azure Service Fabric-clusterknooppunten in Linux. U moet als volgt te werk voor elk van de knooppunttypen en virtuele-machineschaalsets. Voor het versleutelen van de knooppunten, gebruiken we de mogelijkheid van Azure Disk Encryption voor virtuele-machineschaalsets.

De handleiding wordt ingegaan op de volgende onderwerpen:

* Belangrijkste concepten rekening mee moet houden wanneer inschakelen schijfversleuteling voor de Service Fabric-cluster virtuele-machineschaalset in Linux wordt ingesteld.
* Stappen worden gevolgd voordat u inschakelt schijfversleuteling voor de Service Fabric-clusterknooppunten in Linux.
* Stappen voor het inschakelen van schijfversleuteling op de clusterknooppunten Service Fabric in Linux worden gevolgd.



[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Vereisten

 **Zelfregistratie**

De preview van de schijf versleuteling voor de virtuele-machineschaalset is vereist voor zelfregistratie. Voer de volgende stappen uit:

1. Voer de volgende opdracht uit: 
    ```powershell
    Register-AzProviderFeature -ProviderNamespace Microsoft.Compute -FeatureName "UnifiedDiskEncryption"
    ```
2. Wacht ongeveer 10 minuten totdat de status *geregistreerde*. U kunt de status controleren met de volgende opdracht:
    ```powershell
    Get-AzProviderFeature -ProviderNamespace "Microsoft.Compute" -FeatureName "UnifiedDiskEncryption"
    Register-AzResourceProvider -ProviderNamespace Microsoft.Compute
    ```
**Azure Key Vault**

1. Een key vault maakt in hetzelfde abonnement en dezelfde regio als de schaalset. Selecteer vervolgens de **EnabledForDiskEncryption** toegangsbeleid voor de sleutelkluis met behulp van de PowerShell-cmdlet. U kunt ook het beleid instellen met behulp van de Key Vault-gebruikersinterface in Azure portal met de volgende opdracht:
    ```powershell
    Set-AzKeyVaultAccessPolicy -VaultName $keyVaultName -EnabledForDiskEncryption
    ```
2. Installeer de nieuwste versie van de [Azure CLI](/cli/azure/install-azure-cli), heeft de nieuwe opdrachten voor versleuteling.

3. Installeer de nieuwste versie van de [Azure-SDK van Azure PowerShell](https://github.com/Azure/azure-powershell/releases) release. Hieronder vindt u de Azure Disk Encryption-cmdlets voor virtuele-machineschaalset ([ingesteld](/powershell/module/az.compute/set-azvmssdiskencryptionextension)) codering ophalen ([ophalen](/powershell/module/az.compute/get-azvmssvmdiskencryption)) status voor schijfversleuteling en verwijderen ([uitschakelen](/powershell/module/az.compute/disable-azvmssdiskencryption)) versleuteling op schaal exemplaar instellen.


| Opdracht | Versie |  Source  |
| ------------- |-------------| ------------|
| Get-AzVmssDiskEncryptionStatus   | 1.0.0 of hoger | Az.Compute |
| Get-AzVmssVMDiskEncryptionStatus   | 1.0.0 of hoger | Az.Compute |
| Disable-AzVmssDiskEncryption   | 1.0.0 of hoger | Az.Compute |
| Get-AzVmssDiskEncryption   | 1.0.0 of hoger | Az.Compute |
| Get-AzVmssVMDiskEncryption   | 1.0.0 of hoger | Az.Compute |
| Set-AzVmssDiskEncryptionExtension   | 1.0.0 of hoger | Az.Compute |


## <a name="supported-scenarios-for-disk-encryption"></a>Ondersteunde scenario's voor schijfversleuteling
* Versleuteling voor virtuele-machineschaalsets wordt alleen ondersteund voor schaalsets met beheerde schijven die zijn gemaakt. Het wordt niet ondersteund voor systeemeigen (of niet-beheerde) schijf schaalsets.
* Versleuteling en schakel versleuteling worden ondersteund voor OS- en gegevensvolumes in virtuele-machineschaalsets in Linux. 
* Virtuele machine (VM) terugzetten van een installatiekopie en upgrade-bewerkingen voor virtuele-machineschaalsets worden niet ondersteund in de huidige Preview-versie.


## <a name="create-a-new-cluster-and-enable-disk-encryption"></a>Een nieuw cluster maken en inschakelen van versleuteling van schijf

Gebruik de volgende opdrachten in een cluster maken en inschakelen van schijfversleuteling met behulp van Azure Resource Manager-sjabloon en een zelfondertekend certificaat.

### <a name="sign-in-to-azure"></a>Aanmelden bij Azure  

Meld u aan met de volgende opdrachten:

```powershell

Login-AzAccount
Set-AzContext -SubscriptionId <guid>

```

```CLI

azure login
az account set --subscription $subscriptionId

```

### <a name="use-the-custom-template-that-you-already-have"></a>Gebruik de aangepaste sjabloon die u al hebt 

Als u een aangepaste sjabloon maken wilt, is het raadzaam dat u een van de sjablonen op de [voorbeeldsjablonen van Azure Service Fabric-cluster maken](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master) pagina. 

Als u al een aangepaste sjabloon hebt, moet u controleren dat de naam van alle drie certificaat-gerelateerde parameters in de sjabloon en de parameter-bestand als volgt. Zorg er ook voor dat de waarden null als volgt zijn:

```Json
   "certificateThumbprint": {
      "value": ""
    },
    "sourceVaultValue": {
      "value": ""
    },
    "certificateUrlValue": {
      "value": ""
    },
```

Omdat alleen schijf gegevensversleuteling wordt ondersteund voor virtuele-machineschaalsets in Linux, moet u een gegevensschijf toevoegen met behulp van Resource Manager-sjabloon. De sjabloon voor het inrichten van de schijf gegevens als volgt bijwerken:

```Json
   
   "storageProfile": { 
            "imageReference": { 
              "publisher": "[parameters('vmImagePublisher')]", 
              "offer": "[parameters('vmImageOffer')]", 
              "sku": "[parameters('vmImageSku')]", 
              "version": "[parameters('vmImageVersion')]" 
            }, 
            "osDisk": { 
              "caching": "ReadOnly", 
              "createOption": "FromImage", 
              "managedDisk": { 
                "storageAccountType": "[parameters('storageAccountType')]" 
              } 
           }, 
                "dataDisks": [ 
                { 
                    "diskSizeGB": 1023, 
                    "lun": 0, 
                    "createOption": "Empty" 
   
```
 

```powershell
$resourceGroupLocation="westus"
$resourceGroupName="mycluster"
$CertSubjectName="mycluster.westus.cloudapp.azure.com"
$certPassword="Password!1" | ConvertTo-SecureString -AsPlainText -Force 
$certOutputFolder="c:\certificates"

$parameterFilePath="c:\templates\templateparam.json"
$templateFilePath="c:\templates\template.json"


New-AzServiceFabricCluster -ResourceGroupName $resourceGroupName -CertificateOutputFolder $certOutputFolder -CertificatePassword $certpassword -CertificateSubjectName $CertSubjectName -TemplateFile $templateFilePath -ParameterFile $parameterFilePath 

```

Hier volgt de equivalente CLI-opdracht. Wijzig de waarden in de instructies declare op de juiste waarden. De CLI biedt ondersteuning voor alle andere parameters die ondersteuning biedt voor de bovenstaande PowerShell-opdracht.

```azurecli
declare certPassword=""
declare resourceGroupLocation="westus"
declare resourceGroupName="mylinux"
declare certSubjectName="mylinuxsecure.westus.cloudapp.azure.com"
declare parameterFilePath="c:\mytemplates\linuxtemplateparm.json"
declare templateFilePath="c:\mytemplates\linuxtemplate.json"
declare certOutputFolder="c:\certificates"


az sf cluster create --resource-group $resourceGroupName --location $resourceGroupLocation  \
    --certificate-output-folder $certOutputFolder --certificate-password $certPassword  \
    --certificate-subject-name $certSubjectName \
    --template-file $templateFilePath --parameter-file $parametersFilePath

```

### <a name="mount-a-data-disk-to-a-linux-instance"></a>Een gegevensschijf aan een Linux-exemplaar koppelen
Voordat u met versleuteling op een virtuele-machineschaalset doorgaat, zorg ervoor dat de toegevoegde gegevensschijf correct is gekoppeld. Meld u aan het cluster voor Linux VM en voer de **LSBLK** opdracht. De uitvoer weergegeven die gegevensschijf toegevoegde in de **koppelpunt** kolom.


### <a name="deploy-application-to-a-service-fabric-cluster-in-linux"></a>Toepassing implementeren in een Service Fabric-cluster in Linux
Volg de instructies en richtlijnen op voor het implementeren van een toepassing in het cluster, [Quick Start: Linux-containers implementeren in Service Fabric](service-fabric-quickstart-containers-linux.md).


### <a name="enable-disk-encryption-for-the-virtual-machine-scale-sets-created-previously"></a>Schijfversleuteling inschakelen voor de virtuele-machineschaalsets eerder hebt gemaakt
Hiermee stelt u hebt gemaakt door de vorige stappen, voer de volgende opdrachten uit om in te schakelen schijfversleuteling voor de virtuele-machineschaalset:
 
```powershell
$VmssName = "nt1vm"
$vaultName = "mykeyvault"
$resourceGroupName = "mycluster"
$KeyVault = Get-AzKeyVault -VaultName $vaultName -ResourceGroupName $rgName
$DiskEncryptionKeyVaultUrl = $KeyVault.VaultUri
$KeyVaultResourceId = $KeyVault.ResourceId

Set-AzVmssDiskEncryptionExtension -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName -DiskEncryptionKeyVaultUrl $DiskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -VolumeType All

```

```azurecli

az vmss encryption enable -g <resourceGroupName> -n <VMSS name> --disk-encryption-keyvault <KeyVaultResourceId>

```

### <a name="validate-if-disk-encryption-is-enabled-for-a-virtual-machine-scale-set-in-linux"></a>Valideren of schijfversleuteling is ingeschakeld voor een virtuele-machineschaalset in Linux
Als u de status van een hele virtuele-machineschaalset of met een instantie in een schaalset, moet u de volgende opdrachten uitvoeren.
Bovendien kunt u zich aanmelden bij de virtuele machine van de Linux-cluster en uitvoeren de **LSBLK** opdracht. De uitvoer wordt weergegeven de toegevoegde gegevensschijf in de **koppelpunt** kolom en de **Type** kolom moet lezen *Crypt*.

```powershell

$VmssName = "nt1vm"
$resourceGroupName = "mycluster"
Get-AzVmssDiskEncryption -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName

Get-AzVmssVMDiskEncryption -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName -InstanceId "0"

```

```azurecli
az vmss encryption show -g <resourceGroupName> -n <VMSS name>

```

### <a name="disable-disk-encryption-for-a-virtual-machine-scale-set-in-a-service-fabric-cluster"></a>Schijfversleuteling voor een virtuele-machineschaalset in een Service Fabric-cluster uitschakelen
Schijfversleuteling voor een virtuele-machineschaalset door de volgende opdrachten uit te schakelen. Houd er rekening mee dat uitschakelen schijfversleuteling wordt toegepast op de hele virtuele-machineschaalset en niet een afzonderlijke exemplaar.

```powershell
$VmssName = "nt1vm"
$resourceGroupName = "mycluster"
Disable-AzVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $VmssName

```

```azurecli
az vmss encryption disable -g <resourceGroupName> -n <VMSS name>

```


## <a name="next-steps"></a>Volgende stappen
Op dit moment moet u een beveiligd cluster hebt en weet hoe u inschakelen en uitschakelen van schijfversleuteling voor Service Fabric-clusterknooppunten en virtuele-machineschaalsets. Zie voor vergelijkbare begeleiding voor het Service Fabric-clusterknooppunten in Linux, [schijf versleuteling voor Windows](service-fabric-enable-azure-disk-encryption-windows.md). 
