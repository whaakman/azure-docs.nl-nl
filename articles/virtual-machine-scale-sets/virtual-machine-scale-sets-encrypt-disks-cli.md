---
title: Versleutelen van schijven voor Azure schaal wordt ingesteld met de Azure CLI | Microsoft Docs
description: Informatie over het gebruik van de Azure CLI 2.0 voor het versleutelen van VM-exemplaren en de gekoppelde schijven in een Linux virtuele-machineschaalset
services: virtual-machine-scale-sets
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/30/2018
ms.author: iainfou
ms.openlocfilehash: 22d3c763317def137b4e0beb155f28585d7c6ae1
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/03/2018
---
# <a name="encrypt-os-and-attached-data-disks-in-a-virtual-machine-scale-set-with-the-azure-cli-20-preview"></a>Versleutelen OS en bijgesloten gegevensschijven in een virtuele-machineschaalset ingesteld met de Azure CLI 2.0 (Preview)

Als u wilt beveiligen en bescherming van gegevens in rust met toonaangevende standaard versleuteling technologie, ondersteuning voor virtuele-machineschaalsets Azure schijf versleuteling (ADE). Versleuteling kan worden ingeschakeld voor Linux en Windows virtuele machine sets schalen. Zie voor meer informatie [Azure Disk Encryption voor Linux en Windows](../security/azure-security-disk-encryption.md).

> [!NOTE]
>  Versleuteling van de Azure-schijf voor virtuele-machineschaalsets is momenteel in de openbare preview beschikbaar in alle openbare Azure-regio.

De schijf van Azure-versleuteling wordt ondersteund:
- voor schaal sets met beheerde schijven gemaakt en niet ondersteund voor systeemeigen (of niet-beheerde) schijf-schaalsets.
- voor OS- en gegevensvolumes in Windows-schaalsets. Schakel versleuteling wordt ondersteund voor OS- en gegevensvolumes voor Windows-schaalsets.
- voor de gegevensvolumes in Linux-schaalsets. OS-schijfversleuteling wordt niet ondersteund in het huidige voorbeeld voor Linux-schaalsets.

Scale set VM terugzetten van de installatiekopie en upgrade-bewerkingen worden niet ondersteund in de huidige preview. De Azure disk encryption voor virtuele machine scale sets preview wordt aanbevolen in een testomgeving. In de preview schijfversleuteling in productieomgevingen waar mogelijk moet u een installatiekopie van het besturingssysteem in een versleutelde schaalset upgrade niet is ingeschakeld.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u wilt installeren en gebruiken van de CLI lokaal, in deze zelfstudie vereist dat u de Azure CLI versie 2.0.31 zijn uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli).

## <a name="register-for-disk-encryption-preview"></a>Registreren voor schijf versleuteling preview

De Azure disk encryption voor virtuele-machineschaalsets preview, moet u uw abonnement met de zelfregistratie [az functie registreren](/cli/azure/feature#az_feature_register). U hoeft alleen de eerste keer dat u de schijf versleuteling preview-functie van de volgende stappen uitvoeren:

```azurecli-interactive
az feature register --name UnifiedDiskEncryption --namespace Microsoft.Compute
```

Het kan maximaal 10 minuten duren voordat de aanvraag voor functieregistratie worden doorgegeven. U kunt controleren op de status van de registratie met [az functie weergeven](/cli/azure/feature#az_feature_show). Wanneer de `State` rapporten *geregistreerde*, opnieuw te registreren de *Mirosoft.Compute* provider met [az provider registreren](/cli/azure/provider#az_provider_register):

```azurecli-interactive
az provider register --namespace Microsoft.Compute
```

## <a name="create-a-scale-set"></a>Een schaalset maken

Voordat u een schaalset kunt maken, moet u eerst een resourcegroep maken met [az group create](/cli/azure/group#az_group_create). In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *eastus*:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Maak nu een virtuele-machineschaalset met [az vmss create](/cli/azure/vmss#az_vmss_create). In het volgende voorbeeld wordt een schaalset met de naam *myScaleSet* gemaakt die automatisch wordt bijgewerkt als er wijzigingen zijn. Daarnaast worden er SSH-sleutels gegenereerd als deze niet bestaan in *~/.ssh/id_rsa*. Een gegevensschijf 32Gb is gekoppeld aan elke VM-instantie en de Azure [aangepaste Scriptextensie](../virtual-machines/linux/extensions-customscript.md) wordt gebruikt voor het voorbereiden van de gegevensschijven met [az vmss extensie set](/cli/azure/vmss/extension#az_vmss_extension_set):

```azurecli-interactive
# Create a scale set with attached data disk
az vmss create \
  --resource-group myResourceGroup \
  --name myScaleSet \
  --image UbuntuLTS \
  --upgrade-policy-mode automatic \
  --admin-username azureuser \
  --generate-ssh-keys \
  --data-disk-sizes-gb 32

# Prepare the data disk for use with the Custom Script Extension
az vmss extension set \
  --publisher Microsoft.Azure.Extensions \
  --version 2.0 \
  --name CustomScript \
  --resource-group myResourceGroup \
  --vmss-name myScaleSet \
  --settings '{"fileUris":["https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/prepare_vm_disks.sh"],"commandToExecute":"./prepare_vm_disks.sh"}'
```

Het duurt enkele minuten om alle schaalsetresources en VM's te maken en te configureren.

## <a name="create-an-azure-key-vault-enabled-for-disk-encryption"></a>Maken van een Azure sleutelkluis is ingeschakeld voor schijfversleuteling

Azure Sleutelkluis kunt opslaan sleutels, geheimen of wachtwoorden waarmee u kunt ze veilig implementeert in uw toepassingen en services. Cryptografische sleutels worden opgeslagen in Azure Key Vault met software-beveiliging, of u kunt importeren of genereren van uw sleutels in Hardware Security Modules (HSM's) die is gecertificeerd voor FIPS 140-2 level 2-standaarden. Deze cryptografische sleutels worden gebruikt voor het versleutelen en ontsleutelen van virtuele schijven gekoppeld aan uw virtuele machine. U beheer behouden over deze cryptografische sleutels en het gebruik ervan kunt controleren.

Definieer uw eigen unieke *keyvault_name*. Vervolgens maakt u een KeyVault met [az keyvault maken](/cli/azure/ext/keyvault-preview/keyvault#ext-keyvault-preview-az-keyvault-create) in hetzelfde abonnement en dezelfde regio bevinden als de schaal worden ingesteld en de *--ingeschakeld-voor--schijfversleuteling* toegangsbeleid.

```azurecli-interactive
# Provide your own unique Key Vault name
keyvault_name=myuniquekeyvaultname

# Create Key Vault
az keyvault create --resource-group myResourceGroup --name $keyvault_name --enabled-for-disk-encryption
```

### <a name="use-an-existing-key-vault"></a>Gebruik een bestaande Sleutelkluis

Deze stap is alleen vereist als u een bestaande Sleutelkluis die u wilt gebruiken met schijfversleuteling hebt. Deze stap overslaan als u een Sleutelkluis hebt gemaakt in de vorige sectie.

Definieer uw eigen unieke *keyvault_name*. Vervolgens bijgewerkt uw KeyVault met [az keyvault update](/cli/azure/ext/keyvault-preview/keyvault#ext-keyvault-preview-az-keyvault-update) en stel de *--ingeschakeld-voor--schijfversleuteling* toegangsbeleid.

```azurecli-interactive
# Provide your own unique Key Vault name
keyvault_name=myuniquekeyvaultname

# Create Key Vault
az keyvault update --name $keyvault_name --enabled-for-disk-encryption
```

## <a name="enable-encryption"></a>Versleuteling inschakelen

Voor het versleutelen van de VM-exemplaren in een schaalset u eerst enkele gegevens op de resource-ID van de Sleutelkluis met [az keyvault weergeven](/cli/azure/ext/keyvault-preview/keyvault#ext-keyvault-preview-az-keyvault-show). Deze variabelen worden gebruikt voor start het versleutelingsproces met [az vmss versleuteling inschakelen](/cli/azure/vmss/encryption#az-vmss-encryption-enable):

```azurecli-interactive
# Get the resource ID of the Key Vault
vaultResourceId=$(az keyvault show --resource-group myResourceGroup --name $keyvault_name --query id -o tsv)

# Enable encryption of the data disks in a scale set
az vmss encryption enable \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --disk-encryption-keyvault $vaultResourceId \
    --volume-type DATA
```

Het duurt enkele minuten duren voor het versleutelingsproces te starten.

Omdat de schaalaanpassingsset Upgradebeleid op de schaal ingesteld worden gemaakt in een eerdere stap is ingesteld op *automatische*, VM-exemplaren automatisch het versleutelingsproces wordt gestart. Start op waar het Upgradebeleid handmatig wordt-schaalsets, het versleutelingsbeleid voor de VM-exemplaren met [az vmss update-exemplaren](/cli/azure/vmss#az-vmss-update-instances).

## <a name="check-encryption-progress"></a>Versleuteling voortgang controleren

Gebruiken om te controleren op de status van schijfversleuteling, [az vmss versleuteling weergeven](/cli/azure/vmss/encryption#az-vmss-encryption-show):

```azurecli-interactive
az vmss encryption show --resource-group myResourceGroup --name myScaleSet
```

Tijdens het versleutelen van VM-exemplaren zijn de statuscode rapporten *EncryptionState/versleuteld*, zoals weergegeven in de volgende voorbeelduitvoer:

```bash
[
  {
    "disks": [
      {
        "encryptionSettings": null,
        "name": "myScaleSet_myScaleSet_0_disk2_3f39c2019b174218b98b3dfae3424e69",
        "statuses": [
          {
            "additionalProperties": {},
            "code": "EncryptionState/encrypted",
            "displayStatus": "Encryption is enabled on disk",
            "level": "Info",
            "message": null,
            "time": null
          }
        ]
      }
    ],
    "id": "/subscriptions/guid/resourceGroups/MYRESOURCEGROUP/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/virtualMachines/0",
    "resourceGroup": "MYRESOURCEGROUP"
  }
]
```

## <a name="disable-encryption"></a>Versleuteling uitschakelen

Als u niet langer wenst versleutelde VM-exemplaren schijven te gebruiken, kunt u uitschakelen versleuteling met [az vmss versleuteling uitschakelen](/cli/azure/vmss/encryption?view=azure-cli-latest#az-vmss-encryption-disable) als volgt:

```azurecli-interactive
az vmss encryption disable --resource-group myResourceGroup --name myScaleSet
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel kunt u de Azure CLI 2.0 gebruikt voor het versleutelen van een virtuele-machineschaalset. U kunt ook [Azure PowerShell](virtual-machine-scale-sets-encrypt-disks-ps.md) of sjablonen voor [Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-windows-jumpbox) of [Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-linux-jumpbox).

Een voorbeeld van een end-to-end batch voor Linux scale set-gegevensversleuteling schijf vindt [hier](https://gist.githubusercontent.com/ejarvi/7766dad1475d5f7078544ffbb449f29b/raw/03e5d990b798f62cf188706221ba6c0c7c2efb3f/enable-linux-vmss.bat). In dit voorbeeld maakt u een resourcegroep, Linux-schaalset, koppelt een schijf 5 GB aan gegevens en versleutelt de virtuele-machineschaalset.