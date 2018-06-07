---
title: Schakel versleuteling van schijf voor service fabric Windows-clusters | Microsoft Docs
description: In dit artikel wordt beschreven hoe schijfversleuteling voor Service Fabric clusterknooppunten in Azure inschakelen met behulp van Azure Sleutelkluis in Azure Resource Manager.
services: service-fabric
documentationcenter: .net
author: v-viban
manager: navya
ms.assetid: 15d0ab67-fc66-4108-8038-3584eeebabaa
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/23/2018
ms.author: v-viban
ms.openlocfilehash: 0b84d270cc50888822b8463df2b95aedaa34ee9a
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655246"
---
# <a name="enable-disk-encryption-for-service-fabric-windows-cluster-nodes"></a>Versleuteling van schijf voor service fabric Windows clusterknooppunten inschakelen 
> [!div class="op_single_selector"]
> * [Schijfversleuteling voor Windows](service-fabric-enable-azure-disk-encryption-windows.md)
> * [Schijfversleuteling voor Linux](service-fabric-enable-azure-disk-encryption-linux.md)
>
>

Volg onderstaande stappen voor het inschakelen van schijfversleuteling op clusterknooppunten voor Service Fabric Windows. U moet uitvoeren voor elk van de knooppunt typen/virtuele-machineschaalsets. Voor het versleutelen van de knooppunten zullen we de mogelijkheden van Azure Disk Encryption op virtuele-machineschaalsets benutten.

De handleiding wordt ingegaan op de volgende procedures:

* Hoofdconcepten die u moet schijfversleuteling op virtuele-machineschaalset Service Fabric-Cluster van Windows inschakelen uit op de hoogte worden ingesteld.
* Vereisten stappen te volgen voordat u de schijfversleuteling op de Service Fabric-Cluster van Windows virtuele-machineschaalset inschakelt.
* Stappen voor het inschakelen van schijfversleuteling op de virtuele-machineschaalset Service Fabric-Cluster van Windows worden gevolgd ingesteld.


## <a name="prerequisites"></a>Vereisten
1. **Zelfregistratie** : als u wilt gebruiken, virtuele machine scale set schijf versleuteling preview zelfregistratie vereist
2. U kunt uw abonnement zelf registreren door het uitvoeren van de volgende stappen uit: 
```Powershell
Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Compute -FeatureName "UnifiedDiskEncryption"
```
3. Wacht ongeveer 10 minuten totdat de status als 'Geregistreerde'. U kunt de status controleren door de volgende opdracht uit te voeren: 
```Powershell
Get-AzureRmProviderFeature -ProviderNamespace "Microsoft.Compute" -FeatureName "UnifiedDiskEncryption"
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute
```
4. **Azure Sleutelkluis** -een KeyVault in hetzelfde abonnement en dezelfde regio worden gemaakt, omdat de schaal ingesteld en het toegangsbeleid 'EnabledForDiskEncryption' op de KeyVault met behulp van de PS-cmdlet. U kunt ook het beleid in de Azure portal met behulp van de KeyVault UI instellen: 
```Powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -EnabledForDiskEncryption
```
5. Installeer de meest recente [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest) , heeft de nieuwe opdrachten voor versleuteling.
6. Installeer de nieuwste versie van [Azure-SDK van Azure PowerShell](https://github.com/Azure/azure-powershell/releases) release. Hieronder vindt u de schaal van de virtuele machine ingesteld ADE cmdlets om in te schakelen ([ingesteld](https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/set-azurermvmssdiskencryptionextension?view=azurermps-4.4.1)) versleuteling, ophalen ([ophalen](https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/get-azurermvmssvmdiskencryption?view=azurermps-4.4.1)) coderingsstatus en verwijderen ([uitschakelen](https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/disable-azurermvmssdiskencryption?view=azurermps-4.4.1)) versleuteling op schaalset exemplaar.

| Opdracht | Versie |  Bron  |
| ------------- |-------------| ------------|
| Get-AzureRmVmssDiskEncryptionStatus   | 3.4.0 of hoger | AzureRM.Compute |
| Get-AzureRmVmssVMDiskEncryptionStatus   | 3.4.0 of hoger | AzureRM.Compute |
| Schakel AzureRmVmssDiskEncryption   | 3.4.0 of hoger | AzureRM.Compute |
| Get-AzureRmVmssDiskEncryption   | 3.4.0 of hoger | AzureRM.Compute |
| Get-AzureRmVmssVMDiskEncryption   | 3.4.0 of hoger | AzureRM.Compute |
| Set-AzureRmVmssDiskEncryptionExtension   | 3.4.0 of hoger | AzureRM.Compute |


## <a name="supported-scenarios-for-disk-encryption"></a>Ondersteunde scenario's voor schijfversleuteling
* Virtuele machine scale set versleuteling wordt alleen ondersteund voor schaalsets gemaakt met beheerde schijven en niet ondersteund voor systeemeigen (of niet-beheerde) schijf-schaalsets.
* Virtuele machine scale set versleuteling wordt ondersteund voor OS- en gegevensvolumes voor Windows VMSS. Schakel versleuteling wordt ondersteund voor OS- en gegevensvolumes voor Windows-schaalset.
* Virtuele-machineschaalset VM opnieuw een installatiekopie en upgrade bewerkingen worden niet ondersteund in de huidige preview.


### <a name="create-new-cluster-and-enable-disk-encryption"></a>Nieuw cluster maken en schijfversleuteling inschakelen

Gebruik de volgende opdrachten cluster maken en inschakelen van schijfversleuteling met behulp van Azure Resource Manager-sjabloon & zelf-ondertekend certificaat.

### <a name="log-in-to-azure"></a>Meld u aan bij Azure. 

```Powershell

Login-AzureRmAccount
Set-AzureRmContext -SubscriptionId <guid>

```

```CLI

azure login
az account set --subscription $subscriptionId

```

#### <a name="use-the-custom-template-that-you-already-have"></a>Gebruik de aangepaste sjabloon die u al hebt 

Als u maken van een aangepaste sjabloon aanpassen aan uw behoeften wilt, is het raadzaam dat u begint met een van de sjablonen die beschikbaar zijn op de [azure service fabric-sjabloon voorbeelden](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master). Volg de richtlijnen en uitleg [aanpassen van uw sjabloon cluster] [aanpassen-your-cluster-sjabloon] hieronder.

Als u al een aangepaste sjabloon hebt en zorg ervoor dat zijn aan Controleer, dat alle drie certificaat-gerelateerde parameters in de sjabloon en het parameterbestand zijn als volgt de naam en -waarden null als volgt.

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


```Powershell


$resourceGroupLocation="westus"
$resourceGroupName="mycluster"
$CertSubjectName="mycluster.westus.cloudapp.azure.com"
$certPassword="Password!1" | ConvertTo-SecureString -AsPlainText -Force 
$certOutputFolder="c:\certificates"

$parameterFilePath="c:\templates\templateparam.json"
$templateFilePath="c:\templates\template.json"


New-AzureRmServiceFabricCluster -ResourceGroupName $resourceGroupName -CertificateOutputFolder $certOutputFolder -CertificatePassword $certpassword -CertificateSubjectName $CertSubjectName -TemplateFile $templateFilePath -ParameterFile $parameterFilePath 

```


```CLI

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

#### <a name="deploy-application-to-windows-service-fabric-cluster"></a>Implementatie van toepassing op Windows-Service Fabric-cluster
Volg de stappen en richtlijnen voor [toepassing naar uw cluster implementeren](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-deploy-remove-applications)


#### <a name="enable-disk-encryption-for-service-fabric-cluster-virtual-machine-scale-set-created-above"></a>Schakel schijfversleuteling voor Service Fabric-Cluster virtuele-machineschaalset die eerder is gemaakt
 
```Powershell

$VmssName = "nt1vm"
$vaultName = "mykeyvault"
$resourceGroupName = "mycluster"
$KeyVault = Get-AzureRmKeyVault -VaultName $vaultName -ResourceGroupName $rgName
$DiskEncryptionKeyVaultUrl = $KeyVault.VaultUri
$KeyVaultResourceId = $KeyVault.ResourceId

Set-AzureRmVmssDiskEncryptionExtension -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName -DiskEncryptionKeyVaultUrl $DiskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -VolumeType All

```

```CLI

az vmss encryption enable -g <resourceGroupName> -n <VMSS name> --disk-encryption-keyvault <KeyVaultResourceId>

```


#### <a name="validate-if-disk-encryption-enabled-for-windows-virtual-machine-scale-set"></a>Valideren of de schijfversleuteling ingeschakeld voor virtuele-machineschaalset Windows instellen.
Status van een gehele virtuele-machineschaalset of met een instantie in de schaalset ophalen. Zie de onderstaande opdrachten.
Bovendien kan gebruiker aanmelden bij de virtuele machine in de schaalset en zorg ervoor dat de stations zijn versleuteld.

```Powershell

$VmssName = "nt1vm"
$resourceGroupName = "mycluster"
Get-AzureRmVmssDiskEncryption -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName

Get-AzureRmVmssVMDiskEncryption -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName -InstanceId "0"

```

```CLI

az vmss encryption show -g <resourceGroupName> -n <VMSS name>

```


#### <a name="disable-disk-encryption-for-service-fabric-cluster-virtual-machine-scale-set"></a>Schijfversleuteling voor Service Fabric-Cluster virtuele-machineschaalset uitschakelen 
Schakel schijfversleuteling geldt voor de hele virtuele-machineschaalset en niet door de instantie 

```Powershell

$VmssName = "nt1vm"
$resourceGroupName = "mycluster"
Disable-AzureRmVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $VmssName

```

```CLI

az vmss encryption disable -g <resourceGroupName> -n <VMSS name>

```


## <a name="next-steps"></a>Volgende stappen
U hebt op dit moment een beveiligde cluster met het schijfversleuteling voor Service Fabric-Cluster virtuele-machineschaalset in-of uitschakelen. Vervolgens [schijfversleuteling voor Linux](service-fabric-enable-azure-disk-encryption-linux.md) 

