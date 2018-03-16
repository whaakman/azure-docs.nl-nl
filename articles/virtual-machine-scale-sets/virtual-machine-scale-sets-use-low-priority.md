---
title: Maken van een Azure-schaalset met lage prioriteit VMs (Preview) | Microsoft Docs
description: Informatie over het maken van virtuele Azure-machine-schaalsets die gebruikmaken van prioriteit Laag virtuele machines op te slaan op kosten
services: virtual-machine-scale-sets
documentationcenter: 
author: mmccrory
manager: rajraj
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/15/2018
ms.author: memccror
ms.openlocfilehash: 9e4970ecc538caab537281931b89bfd57d994cfa
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
---
# <a name="low-priority-vms-on-scale-sets-preview"></a>Prioriteit Laag VM's op schaalsets (preview)

Met behulp van de prioriteit Laag VM's op schaalsets, kunt u om te profiteren van onze unutilized capaciteit op een aanzienlijke kostenbesparingen inhouden. Op elk gewenst moment als de capaciteit terug in Azure nodig heeft, wordt de Azure-infrastructuur onbeschikbaar prioriteit Laag virtuele machines te maken. Daarom zijn prioriteit Laag VM's ideaal voor workloads die onderbrekingen zoals taken, ontwikkelen en testen omgevingen en grote compute-workloads voor batchverwerking kunnen verwerken.

De hoeveelheid beschikbare unutilized capaciteit kan variëren, afhankelijk van de grootte, regio en tijd van de dag. Bij het implementeren van virtuele machines prioriteit laag op schaal wordt ingesteld, wordt Azure de virtuele machines toewijzen als capaciteit er beschikbaar is, maar er geen SLA voor deze virtuele machines is. Een lage prioriteit scale set wordt geïmplementeerd in een enkel foutdomein en biedt dat wordt gegarandeerd dat er geen hoge beschikbaarheid.

> [!NOTE]
> Prioriteit Laag schaalsets zijn Preview-versie en gereed voor uw scenario's voor ontwikkeling en tests. 

## <a name="eviction-policy"></a>Beleid verwijderen

Wanneer uw scale met lage prioriteit VM's zijn verwijderd, worden ze verplaatst naar de status gestopt (toewijzing opgeheven) standaard. Met dit beleid taakverwijdering u verwijderde instanties kunt implementeren, maar er is geen garantie dat de toewijzing slaagt. De gestopte virtuele machines wordt in mindering gebracht op uw scale set exemplaar quotum en wordt u gefactureerd voor uw onderliggende schijven. 

Als u uw virtuele machines in de schaal van uw prioriteit laag ingesteld om te worden verwijderd wilt als ze zijn verwijderd, kunt u het beleid taakverwijdering in verwijderen instellen uw [Azure Resource Manager-sjabloon](#use-azure-resource-manager-templates). Het beleid voor taakverwijdering ingesteld om te verwijderen, kunt u nieuwe virtuele machines maken door het verhogen van de eigenschap scale set exemplaar count. De verwijderde virtuele machines worden verwijderd samen met hun onderliggende schijven en daarom niet brengt voor de opslag. U kunt ook de functie voor automatisch schalen van schaalsets gebruiken om automatisch te compenseren voor verwijderde virtuele machines, er is echter geen garantie dat de toewijzing slaagt. Het verdient aanbeveling om dat u de functie voor automatisch schalen op prioriteit Laag schaalsets alleen gebruiken bij het instellen van het beleid verwijderen om te voorkomen dat de kosten van uw schijven en roept de quotalimieten voor verwijderen. 

> [!NOTE]
> Tijdens de preview, is het mogelijk om in te stellen van uw beleid voor taakverwijdering met behulp van [Azure Resource Manager-sjablonen](#use-azure-resource-manager-templates). 

## <a name="deploying-low-priority-vms-on-scale-sets"></a>Hiermee stelt u prioriteit Laag VMs op schaal implementeren

Als u wilt implementeren lage prioriteit VM's op schaalsets, kunt u instellen de nieuwe *prioriteit* markering *laag*. Alle virtuele machines in de schaalset worden ingesteld op prioriteit laag. Voor het maken van een schaal prioriteit Laag virtuele machines in te stellen, moet u een van de volgende methoden gebruiken:
- [Azure CLI 2.0](#use-the-azure-cli-20)
- [Azure PowerShell](#use-azure-powershell)
- [Azure Resource Manager-sjablonen](#use-azure-resource-manager-templates)

## <a name="use-the-azure-cli-20"></a>De Azure CLI 2.0 gebruiken

Het proces voor het maken van een schaal ingesteld met lage prioriteit virtuele machines is hetzelfde als is beschreven in de [artikel aan de slag](virtual-machine-scale-sets-create-cli.md). Alleen toe te voegen de '--prioriteit '-parameter voor de cli aanroepen en stel deze in op *laag* zoals weergegeven in het voorbeeld hieronder:

```azurecli
az vmss create \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --image UbuntuLTS \
    --upgrade-policy-mode automatic \
    --admin-username azureuser \
    --generate-ssh-keys \
    --priority Low
```

## <a name="use-azure-powershell"></a>Azure PowerShell gebruiken

Het proces voor het maken van een schaal ingesteld met lage prioriteit virtuele machines is hetzelfde als is beschreven in de [artikel aan de slag](virtual-machine-scale-sets-create-powershell.md).
Alleen toe te voegen de '-prioriteit '-parameter voor de [nieuw AzureRmVmssConfig](/powershell/module/azurerm.compute/new-azurermvmssconfig) en wordt ingesteld op *laag* zoals weergegeven in het voorbeeld hieronder:

```powershell
$vmssConfig = New-AzureRmVmssConfig `
    -Location "East US 2" `
    -SkuCapacity 2 `
    -SkuName "Standard_DS2" `
    -UpgradePolicyMode Automatic `
    -Priority "Low"
```

## <a name="use-azure-resource-manager-templates"></a>Gebruik Azure Resource Manager-sjablonen

Het proces voor het maken van een scale-set met lage prioriteit VM's is hetzelfde als in het artikel voor ophalen gestart voor gedetailleerde [Linux](virtual-machine-scale-sets-create-template-linux.md) of [Windows](virtual-machine-scale-sets-create-template-windows.md). Toevoegen van de prioriteitseigenschap '' op de *Microsoft.Compute/virtualMachineScaleSets/virtualMachineProfile* resource typt u in uw sjabloon en geef *laag* als de waarde. Zorg ervoor dat u *2017-10-30-preview* API-versie of hoger. 

De parameter 'evictionPolicy' toevoegen om de verwijdering instellen om te verwijderen, en stel op *verwijderen*.

Het volgende voorbeeld wordt een Linux-prioriteit Laag scale set met de naam *myScaleSet* in *West-Centraal VS*, welke *verwijderen* de VM's in de schaal instelt op verwijderen:

```json
{
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  "name": "myScaleSet",
  "location": "East US 2",
  "apiVersion": "2017-12-01",
  "sku": {
    "name": "Standard_DS2_v2",
    "capacity": "2"
  },
  "properties": {
    "upgradePolicy": {
      "mode": "Automatic"
    },
    "virtualMachineProfile": {
       "priority": "Low",
       "evictionPolicy": "delete",
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
## <a name="next-steps"></a>Volgende stappen
Nu u een schaal ingesteld met lage prioriteit VM's hebt gemaakt, proberen implementeren onze [automatisch schalen sjabloon met lage prioriteit](https://github.com/Azure/vm-scale-sets/tree/master/preview/lowpri).

Bekijk de [virtuele-machineschaalset pagina met prijzen](https://azure.microsoft.com/pricing/details/virtual-machine-scale-sets/linux/) voor prijsinformatie.