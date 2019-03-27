---
title: Disk encryption inschakelen voor Azure Service Fabric Windows-clusters | Microsoft Docs
description: In dit artikel wordt beschreven hoe u schijfversleuteling voor Service Fabric-clusterknooppunten in Azure inschakelen met behulp van Azure Resource Manager, Azure Key Vault.
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
ms.openlocfilehash: a620563be9ffe18ae0f7fa4a78df83ea5b35a5d2
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58488228"
---
# <a name="enable-disk-encryption-for-service-fabric-windows-cluster-nodes"></a>Schijfversleuteling voor service fabric Windows-clusterknooppunten inschakelen 
> [!div class="op_single_selector"]
> * [Schijfversleuteling voor Windows](service-fabric-enable-azure-disk-encryption-windows.md)
> * [Schijfversleuteling voor Linux](service-fabric-enable-azure-disk-encryption-linux.md)
>
>

Volg de stappen hieronder om schijfversleuteling voor de Service Fabric Windows-clusterknooppunten inschakelen. U moet doen dit voor elk van de knooppunt typen/virtuele-machineschaalsets. Voor het versleutelen van de knooppunten, zullen we gebruikmaken van de Azure Disk Encryption-mogelijkheid op schaalsets voor virtuele machines.

De handleiding wordt ingegaan op de volgende procedures:

* Belangrijkste concepten die u nodig hebt om te worden uitgeschakeld op de hoogte om in te schakelen schijfversleuteling voor de virtuele-machineschaalset voor Service Fabric Windows-Cluster ingesteld.
* Vereisten stappen worden gevolgd voordat u inschakelt schijfversleuteling voor de virtuele-machineschaalset van Service Fabric Windows-Cluster.
* Stappen worden gevolgd om in te schakelen schijfversleuteling voor de virtuele-machineschaalset voor Service Fabric Windows-Cluster ingesteld.


## <a name="prerequisites"></a>Vereisten
* **Zelfregistratie** : als u wilt gebruiken, VM scale set schijf versleuteling preview zelfregistratie is vereist
* U kunt uw abonnement zelf registreren door het uitvoeren van de volgende stappen uit: 
```powershell
Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Compute -FeatureName "UnifiedDiskEncryption"
```
* Wacht ongeveer 10 minuten totdat de status als 'Registered'. U kunt de status controleren door de volgende opdracht uit: 
```powershell
Get-AzureRmProviderFeature -ProviderNamespace "Microsoft.Compute" -FeatureName "UnifiedDiskEncryption"
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute
```
* **Azure Key Vault** -een Key Vault maakt in hetzelfde abonnement en dezelfde regio als de schaal ingesteld en het toegangsbeleid 'EnabledForDiskEncryption' voor de Sleutelkluis met behulp van de PS-cmdlet. U kunt ook het beleid met de KeyVault-UI in Azure portal instellen: 
```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -EnabledForDiskEncryption
```
* Installeer de meest recente [Azure CLI 2.0](/cli/azure/install-azure-cli?view=azure-cli-latest) , heeft de nieuwe opdrachten voor versleuteling.
* Installeer de nieuwste versie van [Azure-SDK van Azure PowerShell](https://github.com/Azure/azure-powershell/releases) release. Hieronder vindt u de virtuele-machineschaalset ade-cmdlets om in te schakelen ([instellen](/powershell/module/azurerm.compute/set-azurermvmssdiskencryptionextension?view=azurermps-4.4.1)) codering ophalen ([ophalen](/powershell/module/azurerm.compute/get-azurermvmssvmdiskencryption?view=azurermps-4.4.1)) status voor schijfversleuteling en verwijderen ([uitschakelen](/powershell/module/azurerm.compute/disable-azurermvmssdiskencryption?view=azurermps-4.4.1)) codering in de schaalset het exemplaar.

| Opdracht | Versie |  Bron  |
| ------------- |-------------| ------------|
| Get-AzureRmVmssDiskEncryptionStatus   | 3.4.0 of hoger | AzureRM.Compute |
| Get-AzureRmVmssVMDiskEncryptionStatus   | 3.4.0 of hoger | AzureRM.Compute |
| Disable-AzureRmVmssDiskEncryption   | 3.4.0 of hoger | AzureRM.Compute |
| Get-AzureRmVmssDiskEncryption   | 3.4.0 of hoger | AzureRM.Compute |
| Get-AzureRmVmssVMDiskEncryption   | 3.4.0 of hoger | AzureRM.Compute |
| Set-AzureRmVmssDiskEncryptionExtension   | 3.4.0 of hoger | AzureRM.Compute |


## <a name="supported-scenarios-for-disk-encryption"></a>Ondersteunde scenario's voor schijfversleuteling
* Virtual machine scale set versleuteling wordt alleen ondersteund voor schaalsets met beheerde schijven gemaakt en niet ondersteund voor systeemeigen (of niet-beheerde) schijf schaalsets.
* Virtual machine scale set versleuteling wordt ondersteund voor besturingssysteem en volumes voor Windows-virtuele-machineschaalset. Schakel versleuteling wordt ondersteund voor besturingssysteem en volumes voor Windows-schaalset.
* Virtuele-machineschaalset terugzetten van de virtuele machine en upgrade bewerkingen worden niet ondersteund in de huidige Preview-versie.


### <a name="create-new-cluster-and-enable-disk-encryption"></a>Nieuw cluster maken en inschakelen van versleuteling van schijf

De volgende opdrachten gebruiken voor het cluster maken en inschakelen van versleuteling van de schijf met behulp van Azure Resource Manager-sjabloon & zelfondertekend certificaat.

### <a name="sign-in-to-azure"></a>Aanmelden bij Azure 

```powershell
Login-AzureRmAccount
Set-AzureRmContext -SubscriptionId <guid>

```

```azurecli

azure login
az account set --subscription $subscriptionId

```

#### <a name="use-the-custom-template-that-you-already-have"></a>Gebruik de aangepaste sjabloon die u al hebt 

Als u maken van een aangepaste sjabloon wilt voor uw behoeften, het is raadzaam dat u begint met een van de sjablonen die beschikbaar zijn op de [voorbeeldsjablonen van azure service fabric](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master). Volg de richtlijnen en uitleg [aanpassen van de clustersjabloon] [ customize-your-cluster-template] onderstaande sectie.

Als u al een aangepaste sjabloon hebt, zorg ervoor dat zijn aan Controleer, dat alle drie certificaat-gerelateerde parameters in de sjabloon en het parameterbestand zijn als volgt de naam en -waarden null als volgt.

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


New-AzureRmServiceFabricCluster -ResourceGroupName $resourceGroupName -CertificateOutputFolder $certOutputFolder -CertificatePassword $certpassword -CertificateSubjectName $CertSubjectName -TemplateFile $templateFilePath -ParameterFile $parameterFilePath 

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

#### <a name="deploy-application-to-windows-service-fabric-cluster"></a>Toepassing implementeren in Windows Service Fabric-cluster
Volg de stappen en richtlijnen voor het [toepassing implementeren in uw cluster](service-fabric-deploy-remove-applications.md)


#### <a name="enable-disk-encryption-for-service-fabric-cluster-virtual-machine-scale-set-created-above"></a>Inschakelen van disk encryption voor Service Fabric-Cluster virtuele-machineschaalset die eerder is gemaakt
 
```powershell

$VmssName = "nt1vm"
$vaultName = "mykeyvault"
$resourceGroupName = "mycluster"
$KeyVault = Get-AzureRmKeyVault -VaultName $vaultName -ResourceGroupName $rgName
$DiskEncryptionKeyVaultUrl = $KeyVault.VaultUri
$KeyVaultResourceId = $KeyVault.ResourceId

Set-AzureRmVmssDiskEncryptionExtension -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName -DiskEncryptionKeyVaultUrl $DiskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -VolumeType All

```

```azurecli

az vmss encryption enable -g <resourceGroupName> -n <VMSS name> --disk-encryption-keyvault <KeyVaultResourceId>

```


#### <a name="validate-if-disk-encryption-enabled-for-windows-virtual-machine-scale-set"></a>Valideren als schijfversleuteling is ingeschakeld voor virtuele-machineschaalset in Windows is ingesteld.
Status van een hele virtuele-machineschaalset of met een instantie ophalen in de schaalset. Zie de onderstaande opdrachten.
Daarnaast kan gebruiker zich aanmelden bij de virtuele machine in de schaalset en zorg ervoor dat stations zijn versleuteld

```powershell

$VmssName = "nt1vm"
$resourceGroupName = "mycluster"
Get-AzureRmVmssDiskEncryption -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName

Get-AzureRmVmssVMDiskEncryption -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName -InstanceId "0"

```

```azurecli

az vmss encryption show -g <resourceGroupName> -n <VMSS name>

```


#### <a name="disable-disk-encryption-for-service-fabric-cluster-virtual-machine-scale-set"></a>Schijfversleuteling voor Service Fabric-Cluster virtuele-machineschaalset uitschakelen 
Uitschakelen schijfversleuteling geldt voor de hele virtuele-machineschaalset en niet door exemplaar 

```powershell

$VmssName = "nt1vm"
$resourceGroupName = "mycluster"
Disable-AzureRmVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $VmssName

```

```CLI

az vmss encryption disable -g <resourceGroupName> -n <VMSS name>

```


## <a name="next-steps"></a>Volgende stappen
Op dit moment hebt u een beveiligd cluster met het inschakelen/uitschakelen disk encryption voor virtuele-machineschaalset van Service Fabric-Cluster. Volgende [schijfversleuteling voor Linux](service-fabric-enable-azure-disk-encryption-linux.md) 

[customize-your-cluster-template]: https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-1-NodeTypes-Secure#creating-a-custom-arm-template