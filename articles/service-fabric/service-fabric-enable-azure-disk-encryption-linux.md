---
title: Schijfversleuteling inschakelen voor Service Fabric Linux-clusters | Microsoft Docs
description: In dit artikel wordt beschreven hoe schijfversleuteling voor Service Fabric-cluster schalen in Azure worden ingesteld met behulp van Azure Resource Manager en Azure Sleutelkluis in te schakelen.
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
ms.date: 05/24/2018
ms.author: v-viban
ms.openlocfilehash: e5caa3a787ceb1c8828b4a52648a3c74546c217b
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36750455"
---
# <a name="enable-disk-encryption-for-service-fabric-linux-cluster-nodes"></a>Schijfversleuteling voor de clusterknooppunten Service Fabric Linux inschakelen 
> [!div class="op_single_selector"]
> * [Schijfversleuteling voor Linux](service-fabric-enable-azure-disk-encryption-linux.md)
> * [Schijfversleuteling voor Windows](service-fabric-enable-azure-disk-encryption-windows.md)
>
>

Gebruik de volgende stappen uit om in te schakelen schijfversleuteling op Azure Service Fabric Linux clusterknooppunten. U moet deze voor elk van de knooppunttypen of virtuele-machineschaalsets doen. Voor het versleutelen van de knooppunten, gebruikt u de mogelijkheid voor Azure Disk Encryption op virtuele-machineschaalsets.

De handleiding wordt ingegaan op de volgende procedures:

* Belangrijkste concepten voor het inschakelen van schijfversleuteling op de virtuele-machineschaalsets voor Service Fabric Linux-clusters.
* Bepaalde vereiste stappen volgen voordat u schijfversleuteling op de virtuele machine inschakelen schalen sets voor Service Fabric Linux-clusters.
* Stappen voor het inschakelen en uitschakelen schijfversleuteling op virtuele-machineschaalsets voor Service Fabric Linux-clusters.


## <a name="prerequisites"></a>Vereisten

1. Uw abonnement zichzelf registreren met de volgende opdracht:

   ```PowerShell
   Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Compute -FeatureName "UnifiedDiskEncryption"
   ```
   
   Wacht ongeveer tien minuten totdat de status is `Registered`. U kunt de status controleren met de volgende opdrachten: 

   ```PowerShell
   Get-AzureRmProviderFeature -ProviderNamespace "Microsoft.Compute" -FeatureName "UnifiedDiskEncryption"
   Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute
   ```

2. Een sleutelkluis maken in hetzelfde abonnement en dezelfde regio als de scale-set. Instellen van het toegangsbeleid `EnabledForDiskEncryption` op de sleutelkluis met behulp van de PowerShell-cmdlet. U kunt ook het beleid instellen via de gebruikersinterface van Azure Key Vault in de Azure-portal.

   ```PowerShell
   Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -EnabledForDiskEncryption
   ```

3. Installeer [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest), maar deze bevat de meest recente versleuteling opdrachten.

4. Installeer de nieuwste versie van de [Azure-SDK van Azure PowerShell](https://github.com/Azure/azure-powershell/releases). De volgende cmdlets gebruiken om in te schakelen ([ingesteld](https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/set-azurermvmssdiskencryptionextension?view=azurermps-4.4.1)) versleuteling, ophalen ([ophalen](https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/get-azurermvmssvmdiskencryption?view=azurermps-4.4.1)) coderingsstatus en verwijderen ([uitschakelen](https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/disable-azurermvmssdiskencryption?view=azurermps-4.4.1)) versleuteling op een schaal exemplaar instellen: 

   | Opdracht | Versie |  Bron  |
   | ------------- |-------------| ------------|
   | Get-AzureRmVmssDiskEncryptionStatus   | 3.4.0 of hoger | AzureRM.Compute |
   | Get-AzureRmVmssVMDiskEncryptionStatus   | 3.4.0 of hoger | AzureRM.Compute |
   | Schakel AzureRmVmssDiskEncryption   | 3.4.0 of hoger | AzureRM.Compute |
   | Get-AzureRmVmssDiskEncryption   | 3.4.0 of hoger | AzureRM.Compute |
   | Get-AzureRmVmssVMDiskEncryption   | 3.4.0 of hoger | AzureRM.Compute |
   | Set-AzureRmVmssDiskEncryptionExtension   | 3.4.0 of hoger | AzureRM.Compute |


## <a name="supported-scenarios-for-disk-encryption"></a>Ondersteunde scenario's voor schijfversleuteling
* Virtuele machine scale set versleuteling wordt alleen ondersteund voor schaalsets gemaakt met beheerde-schijven. Dit wordt niet ondersteund voor systeemeigen (of niet-beheerde) schijf-schaalsets.
* Virtuele machine scale set versleuteling wordt ondersteund voor de gegevensvolumes voor Linux virtuele-machineschaalsets. OS-schijfversleuteling wordt niet ondersteund in het huidige voorbeeld voor Linux.
* Virtuele-machineschaalset VM terugzetten van de installatiekopie en upgrade bewerkingen worden niet ondersteund in de huidige preview.


## <a name="create-a-linux-cluster"></a>Een Linux-cluster maken

Gebruik de volgende opdrachten voor het maken van een cluster en schijfversleuteling inschakelen met behulp van een Azure Resource Manager-sjabloon en een zelfondertekend certificaat.

### <a name="log-in-to-azure"></a>Meld u aan bij Azure.  

```PowerShell

Login-AzureRmAccount
Set-AzureRmContext -SubscriptionId <guid>

```

```CLI

azure login
az account set --subscription $subscriptionId

```

### <a name="use-a-custom-template"></a>Een aangepaste sjabloon gebruiken 

Als u maken van een aangepaste sjabloon aanpassen aan uw behoeften wilt, raden wij aan dat u met een van begint de [voorbeelden van Azure Service Fabric-sjabloon](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master) voor Linux-clusters. 

Als u al een aangepaste sjabloon hebt, ervoor zorgen dat alle drie certificaat-gerelateerde parameters in de sjabloon en het parameterbestand als volgt worden benoemd. Zorg ook dat waarden null zijn.

```JSON
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

Voor Linux virtuele machine-schaalsets wordt alleen schijf gegevensversleuteling ondersteund. Daarom moet u een gegevensschijf toevoegen met behulp van een Azure Resource Manager-sjabloon. De sjabloon voor het gegevens schijf inrichten als volgt bijwerken:

```JSON
   
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
 

```PowerShell


$resourceGroupLocation="westus"
$resourceGroupName="mycluster"
$CertSubjectName="mycluster.westus.cloudapp.azure.com"
$certPassword="Password!1" | ConvertTo-SecureString -AsPlainText -Force 
$certOutputFolder="c:\certificates"

$parameterFilePath="c:\templates\templateparam.json"
$templateFilePath="c:\templates\template.json"


New-AzureRmServiceFabricCluster -ResourceGroupName $resourceGroupName -CertificateOutputFolder $certOutputFolder -CertificatePassword $certpassword -CertificateSubjectName $CertSubjectName -TemplateFile $templateFilePath -ParameterFile $parameterFilePath 

```

Hier volgen de equivalente Azure CLI-opdrachten voor het bijwerken van de sjabloon. Wijzig de waarden in de declare-instructies op de juiste waarden. Azure CLI ondersteunt de parameters die ondersteuning bieden voor de eerdere PowerShell-opdrachten.

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

### <a name="confirm-that-the-linux-data-disk-is-mounted"></a>Controleer of de Linux-gegevensschijf is gekoppeld
Voordat u met versleuteling op de Linux virtuele-machineschaalset doorgaat, ervoor zorgen dat de toegevoegde gegevensschijf correct is gekoppeld. Meld u aan het cluster Linux VM bij en voer de opdracht LSBLK. 

De uitvoer moet de toegevoegde gegevensschijf op een punt koppelpunt kolom weergeven.


### <a name="deploy-an-application-to-the-linux-service-fabric-cluster"></a>Een toepassing met het Linux Service Fabric-cluster implementeren
Volg de stappen en richtlijnen voor [toepassing implementeren naar uw cluster](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-quickstart-containers-linux).


## <a name="enable-disk-encryption-for-a-virtual-machine-scale-set"></a>Schakel schijfversleuteling voor een virtuele-machineschaalset
Schakel schijfversleuteling voor de virtuele-machineschaalset die u eerder hebt gemaakt voor het Service Fabric Linux-cluster.
 
```PowerShell
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

## <a name="validate-that-disk-encryption-is-enabled-for-a-virtual-machine-scale-set"></a>Valideren van die schijf versleuteling is ingeschakeld voor een virtuele-machineschaalset
Gebruik de volgende opdrachten om op te halen van de status van een gehele virtuele-machineschaalset of een instantie van virtuele machine in een schaalset. U kunt ook Meld u aan het cluster Linux VM bij en voer de opdracht LSBLK. De uitvoer de toegevoegde gegevensschijf voor de kolom mount-punt moet worden weergegeven en `Type` kolom als `Crypt`.

```PowerShell

$VmssName = "nt1vm"
$resourceGroupName = "mycluster"
Get-AzureRmVmssDiskEncryption -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName

Get-AzureRmVmssVMDiskEncryption -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName -InstanceId "0"

```

```CLI

az vmss encryption show -g <resourceGroupName> -n <VMSS name>

```



## <a name="disable-disk-encryption-for-a-virtual-machine-scale-set"></a>Schijfversleuteling voor een virtuele-machineschaalset uitschakelen 
Als u uitschakelen schijfversleuteling voor de virtuele-machineschaalset ingesteld voor een Service Fabric Linux-cluster wilt, gebruikt u de volgende opdrachten. Uitschakelen van schijfversleuteling geldt voor de hele virtuele-machineschaalset en niet door de instantie. 

```PowerShell

$VmssName = "nt1vm"
$resourceGroupName = "mycluster"
Disable-AzureRmVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $VmssName

```

```CLI

az vmss encryption disable -g <resourceGroupName> -n <VMSS name>

```


## <a name="next-steps"></a>Volgende stappen
Op dit moment hebt u een beveiligde cluster en u weet hoe inschakelen en uitschakelen van de schijfversleuteling voor een Service Fabric Linux-cluster. Vervolgens leert over [schijfversleuteling voor Windows](service-fabric-enable-azure-disk-encryption-windows.md). 

