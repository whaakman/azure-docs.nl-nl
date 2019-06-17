---
title: Disk encryption inschakelen voor Azure Service Fabric Windows-clusters | Microsoft Docs
description: In dit artikel wordt beschreven hoe u het inschakelen van disk encryption voor Azure Service Fabric-clusterknooppunten met behulp van Azure Key Vault in Azure Resource Manager.
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
ms.openlocfilehash: c31fc43729bcb58c755959db0c8bc5185b8197f4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66471412"
---
# <a name="enable-disk-encryption-for-azure-service-fabric-cluster-nodes-in-windows"></a>Inschakelen van disk encryption voor Azure Service Fabric-clusterknooppunten in Windows 
> [!div class="op_single_selector"]
> * [Schijfversleuteling voor Windows](service-fabric-enable-azure-disk-encryption-windows.md)
> * [Schijfversleuteling voor Linux](service-fabric-enable-azure-disk-encryption-linux.md)
>
>

In deze zelfstudie leert u hoe u om in te schakelen schijfversleuteling voor de Service Fabric-clusterknooppunten in Windows. U moet als volgt te werk voor elk van de knooppunttypen en virtuele-machineschaalsets. Voor het versleutelen van de knooppunten, gebruiken we de mogelijkheid van Azure Disk Encryption voor virtuele-machineschaalsets.

De handleiding wordt ingegaan op de volgende onderwerpen:

* Belangrijkste concepten rekening mee moet houden wanneer inschakelen schijfversleuteling voor de virtuele-machineschaalset voor Service Fabric-cluster wordt ingesteld in Windows.
* Stappen worden gevolgd voordat u inschakelt schijfversleuteling voor de Service Fabric-clusterknooppunten in Windows.
* Stappen voor het inschakelen van schijfversleuteling op Service Fabric-clusterknooppunten in Windows worden gevolgd.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Vereisten

**Zelfregistratie** 

De preview van de schijf versleuteling voor de virtuele-machineschaalset is vereist voor zelfregistratie. Voer de volgende stappen uit: 

1. Voer eerst de volgende opdracht uit:
    ```powershell
    Register-AzProviderFeature -ProviderNamespace Microsoft.Compute -FeatureName "UnifiedDiskEncryption"
    ```
2. Wacht ongeveer 10 minuten totdat de status *geregistreerde*. U kunt de status controleren met de volgende opdracht: 
    ```powershell
    Get-AzProviderFeature -ProviderNamespace "Microsoft.Compute" -FeatureName "UnifiedDiskEncryption"
    Register-AzResourceProvider -ProviderNamespace Microsoft.Compute
    ```
**Azure Key Vault** 

1. Maak een key vault in hetzelfde abonnement en dezelfde regio bevinden als de schaalset en selecteer vervolgens de **EnabledForDiskEncryption** toegangsbeleid voor de sleutelkluis met behulp van de PowerShell-cmdlet. U kunt ook het beleid instellen met behulp van de Key Vault-gebruikersinterface in Azure portal met de volgende opdracht:
    ```powershell
    Set-AzKeyVaultAccessPolicy -VaultName $keyVaultName -EnabledForDiskEncryption
    ```
2. Installeer de nieuwste versie van de [Azure CLI](/cli/azure/install-azure-cli), heeft de nieuwe opdrachten voor versleuteling.
3. Installeer de nieuwste versie van de [Azure-SDK van Azure PowerShell](https://github.com/Azure/azure-powershell/releases) release. Hieronder vindt u de Azure Disk Encryption-cmdlets voor virtuele-machineschaalset ([ingesteld](/powershell/module/az.compute/set-azvmssdiskencryptionextension)) codering ophalen ([ophalen](/powershell/module/az.compute/get-azvmssvmdiskencryption)) status voor schijfversleuteling en verwijderen ([uitschakelen](/powershell/module/az.compute/disable-azvmssdiskencryption)) versleuteling op schaal exemplaar instellen.

| Opdracht | Version |  source  |
| ------------- |-------------| ------------|
| Get-AzVmssDiskEncryptionStatus   | 1.0.0 of hoger | Az.Compute |
| Get-AzVmssVMDiskEncryptionStatus   | 1.0.0 of hoger | Az.Compute |
| Disable-AzVmssDiskEncryption   | 1.0.0 of hoger | Az.Compute |
| Get-AzVmssDiskEncryption   | 1.0.0 of hoger | Az.Compute |
| Get-AzVmssVMDiskEncryption   | 1.0.0 of hoger | Az.Compute |
| Set-AzVmssDiskEncryptionExtension   | 1.0.0 of hoger | Az.Compute |


## <a name="supported-scenarios-for-disk-encryption"></a>Ondersteunde scenario's voor schijfversleuteling
* Versleuteling voor virtuele-machineschaalsets wordt alleen ondersteund voor schaalsets met beheerde schijven die zijn gemaakt. Het wordt niet ondersteund voor systeemeigen (of niet-beheerde) schijf schaalsets.
* Versleuteling wordt ondersteund voor het besturingssysteem en gegevensvolumes in virtuele-machineschaalset wordt ingesteld in Windows. Schakel versleuteling wordt ook ondersteund voor het besturingssysteem en Hiermee stelt u de gegevensvolumes voor virtuele-machineschaalset in Windows.
* Virtuele machine terugzetten van een installatiekopie en upgrade-bewerkingen voor virtuele-machineschaalsets worden niet ondersteund in de huidige Preview-versie.


## <a name="create-a-new-cluster-and-enable-disk-encryption"></a>Een nieuw cluster maken en inschakelen van versleuteling van schijf

Gebruik de volgende opdrachten in een cluster maken en inschakelen van schijfversleuteling met behulp van een Azure Resource Manager-sjabloon en een zelfondertekend certificaat.

### <a name="sign-in-to-azure"></a>Aanmelden bij Azure 
Meld u aan met de volgende opdrachten:
```powershell
Login-AzAccount
Set-AzContext -SubscriptionId <guid>

```

```azurecli

azure login
az account set --subscription $subscriptionId

```

### <a name="use-the-custom-template-that-you-already-have"></a>Gebruik de aangepaste sjabloon die u al hebt 

Als u maken van een aangepaste sjabloon wilt voor uw behoeften, is het raadzaam dat u begint met een van de sjablonen die beschikbaar zijn op de [voorbeeldsjablonen van Azure Service Fabric-cluster maken](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master) pagina. Naar [aanpassen van de clustersjabloon] [ customize-your-cluster-template] sectie, raadpleegt u de volgende richtlijnen.

Als u al een aangepaste sjabloon hebt, Controleer u dat alle drie certificaat-gerelateerde parameters in de sjabloon en de parameter-bestand zijn met de naam als volgt en dat waarden null als volgt zijn:

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

### <a name="deploy-an-application-to-a-service-fabric-cluster-in-windows"></a>Een toepassing implementeren in een Service Fabric-cluster in Windows
Volg de instructies en richtlijnen op voor het implementeren van een toepassing in het cluster, [implementeren en remove-toepassingen met behulp van PowerShell](service-fabric-deploy-remove-applications.md).


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


### <a name="validate-if-disk-encryption-is-enabled-for-a-virtual-machine-scale-set-in-windows"></a>Valideren of schijfversleuteling is ingeschakeld voor een virtuele-machineschaalset in Windows
Haal de status van een hele virtuele-machineschaalset of met een instantie in een schaalset door het uitvoeren van de volgende opdrachten.

```powershell

$VmssName = "nt1vm"
$resourceGroupName = "mycluster"
Get-AzVmssDiskEncryption -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName

Get-AzVmssVMDiskEncryption -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName -InstanceId "0"

```

```azurecli

az vmss encryption show -g <resourceGroupName> -n <VMSS name>

```


Bovendien kunt u zich aanmeldt bij de virtuele-machineschaalset en zorg ervoor dat de stations zijn versleuteld.

### <a name="disable-disk-encryption-for-a-virtual-machine-scale-set-in-a-service-fabric-cluster"></a>Schijfversleuteling voor een virtuele-machineschaalset in een Service Fabric-cluster uitschakelen 
Schijfversleuteling voor een virtuele-machineschaalset door de volgende opdrachten uit te schakelen. Houd er rekening mee dat uitschakelen schijfversleuteling wordt toegepast op de hele virtuele-machineschaalset en niet een afzonderlijke exemplaar.

```powershell

$VmssName = "nt1vm"
$resourceGroupName = "mycluster"
Disable-AzVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $VmssName

```

```CLI

az vmss encryption disable -g <resourceGroupName> -n <VMSS name>

```


## <a name="next-steps"></a>Volgende stappen
Op dit moment moet u een beveiligd cluster hebt en weet hoe u inschakelen en uitschakelen van schijfversleuteling voor Service Fabric-clusterknooppunten en virtuele-machineschaalsets. Zie voor vergelijkbare begeleiding voor het Service Fabric-clusterknooppunten in Linux, [schijfversleuteling voor Linux](service-fabric-enable-azure-disk-encryption-linux.md).

[customize-your-cluster-template]: https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-1-NodeTypes-Secure#creating-a-custom-arm-template
