---
title: Maken van een Azure-schaalset die gebruikmaakt van beschikbaarheid Zones (Preview) | Microsoft Docs
description: Informatie over het maken van virtuele Azure-machine-schaalsets die gebruikmaken van beschikbaarheid Zones voor verbeterde redundantie tegen uitval
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm
ms.devlang: na
ms.topic: article
ms.date: 01/11/2018
ms.author: iainfou
ms.openlocfilehash: 397afc28b5f4c4f7f84afde13b6d031d83aaced4
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="create-a-virtual-machine-scale-set-that-uses-availability-zones-preview"></a>Maken van een virtuele-machineschaalset die gebruikmaakt van beschikbaarheid Zones (Preview)
Om te voorkomen dat uw virtuele-machineschaalsets fouten datacenter-niveau, kunt u een schaal instellen voor beschikbaarheid Zones maken. Azure-regio's die ondersteuning bieden voor beschikbaarheid Zones hebben een minimum van drie afzonderlijke zones, elk met hun eigen onafhankelijke energie-bron-, netwerk- en koeling. Zie voor meer informatie [overzicht van de Zones van de beschikbaarheid](../availability-zones/az-overview.md).

[!INCLUDE [availability-zones-preview-statement.md](../../includes/availability-zones-preview-statement.md)]


## <a name="single-zone-and-zone-redundant-scale-sets"></a>Single-zone en zone-redundante schaalsets
Wanneer u een virtuele-machineschaalset implementeert, kunt u een enkele Zone beschikbaarheid in een regio of meerdere zones te gebruiken.

Als u een schaal instellen in een enkele zone, u de controle over welke zone worden uitgevoerd in alle exemplaren van deze virtuele machine en de schaalaanpassingsset wordt beheerd en autoscales alleen in deze zone maakt. Het volgende diagram toont een voorbeeld van hoe u meerdere één zone schaal kunt maken met een zone-redundante load balancer die verkeer wordt ingesteld:

![Implementatie met zone-redundante load balancer-schaalset één zone](media/virtual-machine-scale-sets-use-availability-zones/zonal-vmss.png)

Een zone-redundante schaalset kunt u een set met één scale die meerdere zones omvat maken. Als de VM-exemplaren zijn gemaakt, standaard zijn ze gelijkmatig verdeeld zijn over zones. Moet een onderbreking optreedt op een van de zones, een schaalset wordt niet automatisch worden uitgebreid om capaciteit te vergroten. Een aanbevolen procedure kan worden regels voor automatisch schalen op basis van gebruik van CPU of geheugen configureren. De regels voor automatisch schalen, zou de schaal ingesteld om te reageren op een verlies van de VM-instanties die een zone door het uitbreiden van nieuwe exemplaren in de resterende operationele zones toestaan. Het volgende diagram toont een voorbeeld van een set één scale die is geïmplementeerd in meerdere zones:

![Zonal redundante schaal implementatie instellen en de load balancer](media/virtual-machine-scale-sets-use-availability-zones/zone-redundant-vmss.png)

Beschikbaarheid om Zones te gebruiken, moet uw schaalset worden gemaakt in een [ondersteunde Azure-regio](../availability-zones/az-overview.md#regions-that-support-availability-zones). U moet ook [registreren voor een voorbeeld bekijken van de beschikbaarheid van Zones](http://aka.ms/azenroll). U kunt een schaalset die gebruikmaakt van beschikbaarheid Zones met een van de volgende manieren maken:

- [Azure Portal](#use-the-azure-portal)
- [Azure CLI 2.0](#use-the-azure-cli-20)
- [Azure PowerShell](#use-azure-powershell)
- [Azure Resource Manager-sjablonen](#use-azure-resource-manager-templates)


## <a name="use-the-azure-portal"></a>Azure Portal gebruiken
Het proces voor het maken van een schaalset die gebruikmaakt van een Zone beschikbaarheid is hetzelfde als is beschreven in de [artikel aan de slag](virtual-machine-scale-sets-create-portal.md). Zorg ervoor dat u hebt [geregistreerd voor een voorbeeld bekijken van de beschikbaarheid van Zones](http://aka.ms/azenroll). Wanneer u een ondersteunde Azure-regio selecteert, kunt u een schaal instellen in een van de beschikbare zones kunt maken, zoals wordt weergegeven in het volgende voorbeeld:

![Een schaal instellen in een enkele beschikbaarheid Zone maken](media/virtual-machine-scale-sets-use-availability-zones/create-portal-single-az.png)

De schaalaanpassingsset en de ondersteunende resources, zoals de Azure load balancer en het openbare IP-adres worden gemaakt in de enkele zone die u opgeeft.


## <a name="use-the-azure-cli-20"></a>De Azure CLI 2.0 gebruiken
Het proces voor het maken van een schaalset die gebruikmaakt van een Zone beschikbaarheid is hetzelfde als is beschreven in de [artikel aan de slag](virtual-machine-scale-sets-create-cli.md). Beschikbaarheid om Zones te gebruiken, moet u uw scale set maken in een ondersteunde Azure-regio en hebben [geregistreerd voor een voorbeeld bekijken van de beschikbaarheid van Zones](http://aka.ms/azenroll).

Voeg de `--zones` -parameter voor de [az vmss maken](/cli/azure/vmss#az_vmss_create) opdracht en opgeven welke zone moet worden gebruikt (zoals zone *1*, *2*, of *3*). Het volgende voorbeeld wordt een enkele zone-scale set met de naam *myScaleSet* in zone *1*:

```azurecli
az vmss create \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --image UbuntuLTS \
    --upgrade-policy-mode automatic \
    --admin-username azureuser \
    --generate-ssh-keys \
    --zones 1
```
Voor een compleet voorbeeld van de schaal van een enkele zone en netwerkbronnen, raadpleegt u [dit voorbeeldscript CLI](https://github.com/Azure/azure-docs-cli-python-samples/blob/master/virtual-machine-scale-sets/create-single-availability-zone/create-single-availability-zone.sh.)

### <a name="zone-redundant-scale-set"></a>Zone-redundante schaalset
Maken van een zone-redundante schaal instellen, gebruikt u een *standaard* SKU openbare IP-adres en de load balancer. Voor uitgebreide redundantie de *standaard* SKU maakt zone-redundante netwerkbronnen. Zie voor meer informatie [overzicht van Azure Load Balancer Standard](../load-balancer/load-balancer-standard-overview.md). De eerste keer dat u maakt een zone-redundante schaal ingesteld of load balancer, moet u de volgende stappen voor het registreren van uw account voor deze preview-functies.

1. Uw account voor de zone-redundante schaalset en het netwerk met functies register [az functie registreren](/cli/azure/feature#az_feature_register) als volgt:

    ```azurecli
    az feature register --name MultipleAvailabilityZones --namespace Microsoft.Compute
    az feature register --name AllowLBPreview --namespace Microsoft.Network
    ```
    
2. Het duurt enkele minuten om te registreren voor de functies. U kunt controleren over de status van de bewerking met [az functie weergeven](/cli/azure/feature#az_feature_show):

    ```azurecli
    az feature show --name MultipleAvailabilityZones --namespace Microsoft.Compute
    az feature show --name AllowLBPreview --namespace Microsoft.Network
    ```

    Het volgende voorbeeld ziet u de gewenste status van de functie als *geregistreerde*:
    
    ```json
    "properties": {
          "state": "Registered"
       },
    ```

3. Als de zone-redundante schaal instellen en het netwerk resources rapporteren als *geregistreerde*, opnieuw te registreren de *Compute* en *netwerk* providers met [az Provider registreren](/cli/azure/provider#az_provider_register) als volgt:

    ```azurecli
    az provider register --namespace Microsoft.Compute
    az provider register --namespace Microsoft.Network
    ```

Geef voor het maken van een zone-redundante schaalset meerdere zones met de `--zones` parameter. Het volgende voorbeeld wordt een zone-redundante scale set met de naam *myScaleSet* diverse zones *1,2,3*:

```azurecli
az vmss create \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --image UbuntuLTS \
    --upgrade-policy-mode automatic \
    --admin-username azureuser \
    --generate-ssh-keys \
    --zones {1,2,3}
```

Het duurt enkele minuten maken en configureren van alle de schaal ingesteld voor resources en virtuele machines in de zones die u opgeeft. Voor een compleet voorbeeld van een zone-redundante schaal en netwerkbronnen, raadpleegt u [dit voorbeeldscript CLI](https://github.com/Azure/azure-docs-cli-python-samples/blob/master/virtual-machine-scale-sets/create-zone-redundant-scale-set/create-zone-redundant-scale-set.sh)


## <a name="use-azure-powershell"></a>Azure PowerShell gebruiken
Het proces voor het maken van een schaalset die gebruikmaakt van een Zone beschikbaarheid is hetzelfde als is beschreven in de [artikel aan de slag](virtual-machine-scale-sets-create-powershell.md). Beschikbaarheid om Zones te gebruiken, moet u uw scale set maken in een ondersteunde Azure-regio en hebben [geregistreerd voor een voorbeeld bekijken van de beschikbaarheid van Zones](http://aka.ms/azenroll). Toevoegen de `-Zone` -parameter voor de [nieuw AzureRmVmssConfig](/powershell/module/azurerm.compute/new-azurermvmssconfig) opdracht en opgeven welke zone moet worden gebruikt (zoals zone *1*, *2*, of *3*). 

Het volgende voorbeeld wordt een enkele zone scale set config met de naam *vmssConfig* in *VS-Oost 2* zone *1*:

```powershell
$vmssConfig = New-AzureRmVmssConfig `
    -Location "East US 2" `
    -SkuCapacity 2 `
    -SkuName "Standard_DS2" `
    -UpgradePolicyMode Automatic `
    -Zone "1"
```

Voor een compleet voorbeeld van de schaal van een enkele zone en netwerkbronnen, raadpleegt u [deze PowerShell-voorbeeldscript](https://github.com/Azure/azure-docs-powershell-samples/blob/master/virtual-machine-scale-sets/create-single-availability-zone/create-single-availability-zone.ps1)

### <a name="zone-redundant-scale-set"></a>Zone-redundante schaalset
Maken van een zone-redundante schaal instellen, gebruikt u een *standaard* SKU openbare IP-adres en de load balancer. Voor uitgebreide redundantie de *standaard* SKU maakt zone-redundante netwerkbronnen. Zie voor meer informatie [overzicht van Azure Load Balancer Standard](../load-balancer/load-balancer-standard-overview.md). De eerste keer dat u maakt een zone-redundante schaal ingesteld of load balancer, moet u de volgende stappen voor het registreren van uw account voor deze preview-functies.

1. Uw account voor de zone-redundante schaalset en het netwerk met functies register [registreren AzureRmProviderFeature](/powershell/module/azurerm.resources/register-azurermproviderfeature) als volgt:

    ```powershell
    Register-AzureRmProviderFeature -FeatureName MultipleAvailabilityZones -ProviderNamespace Microsoft.Compute
    Register-AzureRmProviderFeature -FeatureName AllowLBPreview -ProviderNamespace Microsoft.Network
    ```
    
2. Het duurt enkele minuten om te registreren voor de functies. U kunt controleren over de status van de bewerking met [Get-AzureRmProviderFeature](/powershell/module/AzureRM.Resources/Get-AzureRmProviderFeature):

    ```powershell
    Get-AzureRmProviderFeature -FeatureName MultipleAvailabilityZones -ProviderNamespace Microsoft.Compute 
    Get-AzureRmProviderFeature -FeatureName AllowLBPreview -ProviderNamespace Microsoft.Network
    ```

    Het volgende voorbeeld ziet u de gewenste status van de functie als *geregistreerde*:
    
    ```powershell
    RegistrationState
    -----------------
    Registered
    ```

3. Als de zone-redundante schaal instellen en het netwerk resources rapporteren als *geregistreerde*, opnieuw te registreren de *Compute* en *netwerk* providers met [ Register AzureRmResourceProvider](/powershell/module/AzureRM.Resources/Register-AzureRmResourceProvider) als volgt:

    ```powershell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
    ```

Geef voor het maken van een zone-redundante schaalset meerdere zones met de `-Zone` parameter. Het volgende voorbeeld wordt een zone-redundante scale set config met de naam *myScaleSet* over *VS-Oost 2* zones *1, 2, 3*:

```powershell
$vmssConfig = New-AzureRmVmssConfig `
    -Location "East US 2" `
    -SkuCapacity 2 `
    -SkuName "Standard_DS2" `
    -UpgradePolicyMode Automatic `
    -Zone "1", "2", "3"
```

Als u een openbaar IP-adres met maakt [nieuw AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) of een load balancer met [nieuw AzureRmLoadBalancer](/powershell/module/AzureRM.Network/New-AzureRmLoadBalancer), geef de *- SKU 'Standaard'* maken zone-redundante netwerkbronnen. U moet ook een Netwerkbeveiligingsgroep en regels zodanig dat alle verkeer maken. Zie voor meer informatie [overzicht van Azure Load Balancer Standard](../load-balancer/load-balancer-standard-overview.md).

Voor een compleet voorbeeld van een zone-redundante schaal en netwerkbronnen, raadpleegt u [deze PowerShell-voorbeeldscript](https://github.com/Azure/azure-docs-powershell-samples/blob/master/virtual-machine-scale-sets/create-zone-redundant-scale-set/create-zone-redundant-scale-set.ps1)


## <a name="use-azure-resource-manager-templates"></a>Azure Resource Manager-sjablonen gebruiken
Het proces voor het maken van een schaalset die gebruikmaakt van een Zone beschikbaarheid is hetzelfde als is beschreven in het artikel in ophalen gestart voor [Linux](virtual-machine-scale-sets-create-template-linux.md) of [Windows](virtual-machine-scale-sets-create-template-windows.md). Beschikbaarheid om Zones te gebruiken, moet u uw scale set maken in een ondersteunde Azure-regio en hebben [geregistreerd voor een voorbeeld bekijken van de beschikbaarheid van Zones](http://aka.ms/azenroll). Voeg de `zones` eigenschap in op de *Microsoft.Compute/virtualMachineScaleSets* resource typt u in uw sjabloon en opgeven welke zone moet worden gebruikt (zoals zone *1*, *2*, of *3*).

Het volgende voorbeeld wordt een Linux-één zone-scale set met de naam *myScaleSet* in *VS-Oost 2* zone *1*:

```json
{
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  "name": "myScaleSet",
  "location": "East US 2",
  "apiVersion": "2017-12-01",
  "zones": ["1"],
  "sku": {
    "name": "Standard_A1",
    "capacity": "2"
  },
  "properties": {
    "upgradePolicy": {
      "mode": "Automatic"
    },
    "virtualMachineProfile": {
      "storageProfile": {
        "osDisk": {
          "caching": "ReadWrite",
          "createOption": "FromImage"
        },
        "imageReference":  {
          "publisher": "Canonical",
          "offer": "UbuntuServer",
          "sku": "16.04-LTS",
          "version": "latest"
        }
      },
      "osProfile": {
        "computerNamePrefix": "myvmss",
        "adminUsername": "azureuser",
        "adminPassword": "P@ssw0rd!"
      }
    }
  }
}
```

Voor een compleet voorbeeld van de schaal van een enkele zone en netwerkbronnen, raadpleegt u [in dit voorbeeld Resource Manager-sjabloon](https://github.com/Azure/vm-scale-sets/blob/master/preview/zones/singlezone.json)

### <a name="zone-redundant-scale-set"></a>Zone-redundante schaalset
Geef voor het maken van een zone-redundante schaalset meerdere waarden in de `zones` eigenschap voor de *Microsoft.Compute/virtualMachineScaleSets* brontype. Het volgende voorbeeld wordt een zone-redundante scale set met de naam *myScaleSet* over *VS-Oost 2* zones *1,2,3*:

```json
{
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  "name": "myScaleSet",
  "location": "East US 2",
  "apiVersion": "2017-12-01",
  "zones": [
        "1",
        "2",
        "3"
      ]
}
```

Als u een openbaar IP-adres of een load balancer maakt, geeft u de *"sku": {'name': 'Standaard'} '* eigenschap voor het maken van de zone-redundante netwerkbronnen. U moet ook een Netwerkbeveiligingsgroep en regels zodanig dat alle verkeer maken. Zie voor meer informatie [overzicht van Azure Load Balancer Standard](../load-balancer/load-balancer-standard-overview.md).

Voor een compleet voorbeeld van een zone-redundante schaal en netwerkbronnen, raadpleegt u [in dit voorbeeld Resource Manager-sjabloon](https://github.com/Azure/vm-scale-sets/blob/master/preview/zones/multizone.json)


## <a name="next-steps"></a>Volgende stappen
Nu dat u een schaal instellen in een Zone beschikbaarheid hebt gemaakt, leert u hoe u [toepassingen implementeren op virtuele machine schalen sets](virtual-machine-scale-sets-deploy-app.md) of [automatisch schalen gebruiken met virtuele-machineschaalsets](virtual-machine-scale-sets-autoscale-overview.md).
