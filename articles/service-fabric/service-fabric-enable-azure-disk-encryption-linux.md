---
title: Disk encryption inschakelen voor Azure Service Fabric Linux-clusters | Microsoft Docs
description: In dit artikel wordt beschreven hoe u schijfversleuteling inschakelen voor Service Fabric-cluster schalen instellen in Azure met behulp van Azure Resource Manager, Azure Key Vault.
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
ms.openlocfilehash: 3de26efb74b9349282d36beb94e8a2a269227fbf
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58488237"
---
# <a name="enable-disk-encryption-for-service-fabric-linux-cluster-nodes"></a>Schijfversleuteling voor service fabric Linux-clusterknooppunten inschakelen 
> [!div class="op_single_selector"]
> * [Schijfversleuteling voor Linux](service-fabric-enable-azure-disk-encryption-linux.md)
> * [Schijfversleuteling voor Windows](service-fabric-enable-azure-disk-encryption-windows.md)
>
>

Volg de stappen hieronder om schijfversleuteling voor de Service Fabric Linux-clusterknooppunten inschakelen. U moet doen dit voor elk van de knooppunt typen/virtuele-machineschaalsets. Voor het versleutelen van de knooppunten, zullen we gebruikmaken van de Azure Disk Encryption-mogelijkheid op schaalsets voor virtuele machines.

De handleiding wordt ingegaan op de volgende procedures:

* Belangrijkste concepten die u nodig hebt om te worden uitgeschakeld op de hoogte om in te schakelen schijfversleuteling voor de virtuele-machineschaalset voor Service Fabric Linux-Cluster ingesteld.
* Vereisten stappen worden gevolgd voor schijfversleuteling voor de Service Fabric-Cluster voor Linux virtuele-machineschaalset inschakelen.
* Stappen worden gevolgd om in te schakelen schijfversleuteling voor de virtuele-machineschaalset voor Service Fabric Linux-Cluster ingesteld.


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
* **Azure Key Vault** -een Key Vault maakt in hetzelfde abonnement en dezelfde regio als de virtuele-machineschaalset ingesteld en het toegangsbeleid 'EnabledForDiskEncryption' voor de Sleutelkluis met behulp van de PS-cmdlet. U kunt ook het beleid met de KeyVault-UI in Azure portal instellen: 
```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -EnabledForDiskEncryption
```
* Installeer de meest recente [Azure CLI 2.0](/cli/azure/install-azure-cli?view=azure-cli-latest) , heeft de nieuwe opdrachten voor versleuteling.
* Installeer de nieuwste versie van [Azure-SDK van Azure PowerShell](https://github.com/Azure/azure-powershell/releases) release. Hieronder vindt u de virtuele-machineschaalset ade-cmdlets om in te schakelen ([ingesteld](/powershell/module/azurerm.compute/set-azurermvmssdiskencryptionextension?view=azurermps-4.4.1)) codering ophalen ([ophalen](/powershell/module/azurerm.compute/get-azurermvmssvmdiskencryption?view=azurermps-4.4.1)) status voor schijfversleuteling en verwijderen ([uitschakelen](/powershell/module/azurerm.compute/disable-azurermvmssdiskencryption?view=azurermps-4.4.1)) versleuteling op schaal set-exemplaar. 

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
* Virtual machine scale set versleuteling wordt ondersteund voor gegevensvolume voor Linux virtuele-machineschaalset. OS-schijfversleuteling wordt niet ondersteund in de huidige Preview-versie voor Linux.
* Virtuele-machineschaalset terugzetten van de virtuele machine en upgrade bewerkingen worden niet ondersteund in de huidige Preview-versie.


### <a name="create-new-linux-cluster-and-enable-disk-encryption"></a>Nieuwe Linux-cluster maken en inschakelen van versleuteling van schijf

De volgende opdrachten gebruiken voor het cluster maken en inschakelen van versleuteling van de schijf met behulp van Azure Resource Manager-sjabloon & zelfondertekend certificaat.

### <a name="sign-in-to-azure"></a>Aanmelden bij Azure  

```powershell

Login-AzureRmAccount
Set-AzureRmContext -SubscriptionId <guid>

```

```CLI

azure login
az account set --subscription $subscriptionId

```

#### <a name="use-the-custom-template-that-you-already-have"></a>Gebruik de aangepaste sjabloon die u al hebt 

Als u maken van een aangepaste sjabloon wilt voor uw behoeften, het is raadzaam dat u begint met een van de sjablonen die beschikbaar zijn op de [voorbeeldsjablonen van azure service fabric](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master) voor Linux-Cluster. 

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

Sinds voor Linux VM-schaalset - alleen schijf gegevensversleuteling wordt ondersteund, zodat we moeten gegevensschijf met behulp van Azure Resource Manager-sjabloon toevoegen. Update voor de sjabloon voor het inrichten van de schijf gegevens zoals hieronder:

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


New-AzureRmServiceFabricCluster -ResourceGroupName $resourceGroupName -CertificateOutputFolder $certOutputFolder -CertificatePassword $certpassword -CertificateSubjectName $CertSubjectName -TemplateFile $templateFilePath -ParameterFile $parameterFilePath 

```

Hier volgt de equivalente CLI-opdracht hetzelfde te doen. Wijzig de waarden in de instructies declare in de juiste waarden. CLI biedt ondersteuning voor alle andere parameters die ondersteuning biedt voor de bovenstaande powershell-opdracht.

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

#### <a name="linux-data-disk-mounting"></a>Linux gegevens schijf koppelen
Voordat we doorgaan met codering van Linux virtuele-machineschaalset, moeten we Zorg ervoor dat toegevoegde gegevensschijf correct is gekoppeld of niet. Meld u aan met Linux-Cluster-VM en voer LSBLK opdracht. De uitvoer moet die gegevensschijf toegevoegde op koppelpunt punt kolom weergeven.


#### <a name="deploy-application-to-linux-service-fabric-cluster"></a>Toepassing implementeren in Linux Service Fabric-cluster
Volg de stappen en richtlijnen voor het [toepassing implementeren in uw cluster](service-fabric-quickstart-containers-linux.md)


#### <a name="enable-disk-encryption-for-service-fabric-linux-cluster-virtual-machine-scale-set-created-above"></a>Inschakelen van disk encryption voor Service Fabric-Cluster voor Linux virtuele-machineschaalset die eerder is gemaakt
 
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

#### <a name="validate-if-disk-encryption-enabled-for-linux-virtual-machine-scale-set"></a>Valideren als schijfversleuteling is ingeschakeld voor virtuele-machineschaalset in Linux is ingesteld.
Status van de schaalset voor een volledige virtuele machine of een VM-exemplaar in de schaalset ophalen. Zie de onderstaande opdrachten.
Gebruiker kan ook aanmelden bij een Cluster virtuele Linux-machine en LSBLK opdracht uitvoeren. De uitvoer moet worden weergegeven die gegevensschijf toegevoegde op het koppelpunt punt en typekolom als Crypt voor toegevoegde gegevensschijf.

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

```azurecli
az vmss encryption disable -g <resourceGroupName> -n <VMSS name>

```


## <a name="next-steps"></a>Volgende stappen
Op dit moment hebt u een beveiligd cluster met het inschakelen/uitschakelen schijfversleuteling voor de virtuele-machineschaalset Linux Service Fabric-Cluster. Volgende [schijfversleuteling voor Windows](service-fabric-enable-azure-disk-encryption-windows.md) 
