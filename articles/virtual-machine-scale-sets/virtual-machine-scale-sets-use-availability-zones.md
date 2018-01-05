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
ms.date: 01/03/2018
ms.author: iainfou
ms.openlocfilehash: 310fdc68d3eb662906053d2bc0c45e6cfa18d4da
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2018
---
# <a name="create-a-virtual-machine-scale-set-that-uses-availability-zones-preview"></a>Maken van een virtuele-machineschaalset die gebruikmaakt van beschikbaarheid Zones (Preview)
Om te voorkomen dat uw virtuele-machineschaalsets fouten datacenter-niveau, kunt u een schaal in een Zone beschikbaarheid instellen. Azure-regio's die ondersteuning bieden voor beschikbaarheid Zones hebben een minimum van drie afzonderlijke zones, elk met hun eigen onafhankelijke energie-bron-, netwerk- en koeling. Zie voor meer informatie [overzicht van de Zones van de beschikbaarheid](../availability-zones/az-overview.md).

Beschikbaarheid om Zones te gebruiken, moet uw schaalset worden gemaakt in een [ondersteunde Azure-regio](../availability-zones/az-overview.md#regions-that-support-availability-zones). U kunt een schaalset die gebruikmaakt van beschikbaarheid Zones met een van de volgende manieren maken:

- [Azure Portal](#use-the-azure-portal)
- [Azure CLI 2.0](#use-the-azure-cli-20)
- [Azure PowerShell](#use-azure-powershell)
- [Azure Resource Manager-sjablonen](#use-azure-resource-manager-templates)


## <a name="use-the-azure-portal"></a>Azure Portal gebruiken
Het proces voor het maken van een schaalset die gebruikmaakt van een Zone beschikbaarheid is hetzelfde als is beschreven in de [artikel aan de slag](virtual-machine-scale-sets-create-portal.md). Wanneer u een ondersteunde Azure-regio selecteert, kunt u een schaal instellen in een van de beschikbare zones kunt maken, zoals wordt weergegeven in het volgende voorbeeld:

![Een schaal instellen in een enkele beschikbaarheid Zone maken](media/virtual-machine-scale-sets-use-availability-zones/create-portal-single-az.png)

De schaalaanpassingsset en de ondersteunende resources, zoals de Azure load balancer en het openbare IP-adres worden gemaakt in de enkele zone die u opgeeft.


## <a name="use-the-azure-cli-20"></a>De Azure CLI 2.0 gebruiken
Het proces voor het maken van een schaalset die gebruikmaakt van een Zone beschikbaarheid is hetzelfde als is beschreven in de [artikel aan de slag](virtual-machine-scale-sets-create-cli.md). Beschikbaarheid om Zones te gebruiken, moet u uw scale ingesteld in een ondersteunde Azure-regio. Voeg de `--zones` -parameter voor de [az vmss maken](/cli/azure/vmss#az_vmss_create) opdracht en opgeven welke zone moet worden gebruikt (zoals zone *1*, *2*, of *3*). Het volgende voorbeeld wordt een set met de naam scale *myScaleSet* in zone *1*:

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

Het duurt enkele minuten maken en configureren van alle de schaal ingesteld voor resources en virtuele machines in de zone die u opgeeft.


## <a name="use-azure-powershell"></a>Azure PowerShell gebruiken
Het proces voor het maken van een schaalset die gebruikmaakt van een Zone beschikbaarheid is hetzelfde als is beschreven in de [artikel aan de slag](virtual-machine-scale-sets-create-powershell.md). Beschikbaarheid om Zones te gebruiken, moet u uw scale ingesteld in een ondersteunde Azure-regio. Toevoegen de `-Zone` -parameter voor de [nieuw AzureRmVmssConfig](/powershell/module/azurerm.compute/new-azurermvmssconfig) opdracht en opgeven welke zone moet worden gebruikt (zoals zone *1*, *2*, of *3*). Het volgende voorbeeld wordt een scale set config met de naam *vmssConfig* in *VS-Oost 2* zone *1*:

```powershell
$vmssConfig = New-AzureRmVmssConfig `
    -Location "East US 2" `
    -SkuCapacity 2 `
    -SkuName "Standard_DS2" `
    -UpgradePolicyMode Automatic `
    -Zone "1"
```

Voor het maken van de werkelijke schaalset, volg de aanvullende stappen zoals beschreven in de [artikel aan de slag](virtual-machine-scale-sets-create-powershell.md).


## <a name="use-azure-resource-manager-templates"></a>Azure Resource Manager-sjablonen gebruiken
Het proces voor het maken van een schaalset die gebruikmaakt van een Zone beschikbaarheid is hetzelfde als is beschreven in het artikel in ophalen gestart voor [Linux](virtual-machine-scale-sets-create-template-linux.md) of [Windows](virtual-machine-scale-sets-create-template-windows.md). Beschikbaarheid om Zones te gebruiken, moet u uw scale ingesteld in een ondersteunde Azure-regio. Voeg de `zones` eigenschap in op de *Microsoft.Compute/virtualMachineScaleSets* resource typt u in uw sjabloon en opgeven welke zone moet worden gebruikt (zoals zone *1*, *2*, of *3*). Het volgende voorbeeld wordt een Linux-scale set met de naam *myScaleSet* in *VS-Oost 2* zone *1*:

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

Volg de aanvullende stappen zoals beschreven in het artikel in ophalen gestart voor voor het maken van de werkelijke schaalset [Linux](virtual-machine-scale-sets-create-template-linux.md) of [Windows](virtual-machine-scale-sets-create-template-windows.md)


## <a name="next-steps"></a>Volgende stappen
Nu dat u een schaal instellen in een Zone beschikbaarheid hebt gemaakt, leert u hoe u [toepassingen implementeren op virtuele machine schalen sets](virtual-machine-scale-sets-deploy-app.md) of [automatisch schalen gebruiken met virtuele-machineschaalsets](virtual-machine-scale-sets-autoscale-overview.md).
