---
title: Maken van een Azure-schaalset met lage prioriteit VMs (Preview) | Microsoft Docs
description: Informatie over het maken van virtuele Azure-machine-schaalsets die gebruikmaken van prioriteit Laag virtuele machines op te slaan op kosten
services: virtual-machine-scale-sets
documentationcenter: ''
author: mmccrory
manager: rajraj
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2018
ms.author: memccror
ms.openlocfilehash: 5c0726ea0da288d5306e28b101e4d3b59605b443
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2018
---
# <a name="low-priority-vms-on-scale-sets-preview"></a>Prioriteit Laag VM's op schaalsets (preview)

Met behulp van de prioriteit Laag VM's op schaalsets, kunt u om te profiteren van onze unutilized capaciteit op een aanzienlijke kostenbesparingen inhouden. Op elk gewenst moment als de capaciteit terug in Azure nodig heeft, wordt de Azure-infrastructuur onbeschikbaar prioriteit Laag virtuele machines te maken. Daarom zijn prioriteit Laag VM's ideaal voor workloads die onderbrekingen zoals taken, ontwikkelen en testen omgevingen en grote compute-workloads voor batchverwerking kunnen verwerken.

De hoeveelheid beschikbare unutilized capaciteit kan variëren, afhankelijk van de grootte, regio en tijd van de dag. Bij het implementeren van virtuele machines prioriteit laag op schaal wordt ingesteld, wordt Azure de virtuele machines toewijzen als capaciteit er beschikbaar is, maar er geen SLA voor deze virtuele machines is. Een lage prioriteit scale set wordt geïmplementeerd in een enkel foutdomein en biedt dat wordt gegarandeerd dat er geen hoge beschikbaarheid.

## <a name="eviction-policy"></a>Beleid verwijderen

Bij het maken van prioriteit Laag-schaalsets, kunt u de taakverwijdering-beleid instellen op *Deallocate* (standaard) of *verwijderen*. 

De *Deallocate* beleid uw verwijderde virtuele machines verplaatst naar de status gestopt ongedaan zodat u verwijderde exemplaren opnieuw implementeren. Er is echter geen garantie dat de toewijzing slaagt. De toewijzing ongedaan is gemaakt virtuele machines wordt in mindering gebracht op uw scale set exemplaar quotum en wordt u gefactureerd voor uw onderliggende schijven. 

Als u uw virtuele machines in de schaal van uw prioriteit laag ingesteld om te worden verwijderd wilt als ze zijn verwijderd, stelt u het beleid voor verwijdering op *verwijderen*. Het beleid voor taakverwijdering ingesteld om te verwijderen, kunt u nieuwe virtuele machines maken door het verhogen van de eigenschap scale set exemplaar count. De verwijderde virtuele machines worden verwijderd samen met hun onderliggende schijven en daarom niet brengt voor de opslag. U kunt ook de functie voor automatisch schalen van schaalsets gebruiken om automatisch te compenseren voor verwijderde virtuele machines, er is echter geen garantie dat de toewijzing slaagt. Het verdient aanbeveling om dat u de functie voor automatisch schalen op prioriteit Laag schaalsets alleen gebruiken bij het instellen van het beleid verwijderen om te voorkomen dat de kosten van uw schijven en roept de quotalimieten voor verwijderen. 

> [!NOTE]
> Tijdens de preview, is het mogelijk om in te stellen van uw beleid voor taakverwijdering met behulp van de [Azure-portal](#use-the-azure-portal) en [Azure Resource Manager-sjablonen](#use-azure-resource-manager-templates). 

## <a name="deploying-low-priority-vms-on-scale-sets"></a>Hiermee stelt u prioriteit Laag VMs op schaal implementeren

Als u wilt implementeren lage prioriteit VM's op schaalsets, kunt u instellen de nieuwe *prioriteit* markering *laag*. Alle virtuele machines in de schaalset worden ingesteld op prioriteit laag. Voor het maken van een schaal prioriteit Laag virtuele machines in te stellen, moet u een van de volgende methoden gebruiken:
- [Azure Portal](#use-the-azure-portal)
- [Azure CLI 2.0](#use-the-azure-cli-20)
- [Azure PowerShell](#use-azure-powershell)
- [Azure Resource Manager-sjablonen](#use-azure-resource-manager-templates)

## <a name="use-the-azure-portal"></a>Azure Portal gebruiken

Het proces voor het maken van een scale-set met lage prioriteit virtuele machines is hetzelfde als is beschreven in de [artikel aan de slag](quick-create-portal.md). Wanneer u een schaalset implementeert, kunt u kiezen om in te stellen de vlag prioriteit Laag en het beleid voor verwijdering: ![een schaal ingesteld met lage prioriteit virtuele machines maken](media/virtual-machine-scale-sets-use-low-priority/vmss-low-priority-portal.png)

## <a name="use-the-azure-cli-20"></a>De Azure CLI 2.0 gebruiken

Het proces voor het maken van een schaal ingesteld met lage prioriteit virtuele machines is hetzelfde als is beschreven in de [artikel aan de slag](quick-create-cli.md). Alleen toe te voegen de '--prioriteit '-parameter voor de cli aanroepen en stel deze in op *laag* zoals weergegeven in het voorbeeld hieronder:

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

Het proces voor het maken van een schaal ingesteld met lage prioriteit virtuele machines is hetzelfde als is beschreven in de [artikel aan de slag](quick-create-powershell.md).
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

Het proces voor het maken van een scale-set met lage prioriteit VM's is hetzelfde als in het artikel voor ophalen gestart voor gedetailleerde [Linux](quick-create-template-linux.md) of [Windows](quick-create-template-windows.md). Toevoegen van de prioriteitseigenschap '' op de *Microsoft.Compute/virtualMachineScaleSets/virtualMachineProfile* resource typt u in uw sjabloon en geef *laag* als de waarde. Zorg ervoor dat u *2018-03-01* API-versie of hoger. 

De parameter 'evictionPolicy' toevoegen om de verwijdering instellen om te verwijderen, en stel op *verwijderen*.

Het volgende voorbeeld wordt een Linux-prioriteit Laag scale set met de naam *myScaleSet* in *West-Centraal VS*, welke *verwijderen* de VM's in de schaal instelt op verwijderen:

```json
{
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  "name": "myScaleSet",
  "location": "East US 2",
  "apiVersion": "2018-03-01",
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
## <a name="faq"></a>Veelgestelde vragen

### <a name="can-i-convert-existing-scale-sets-to-low-priority-scale-sets"></a>Kan ik bestaande schaalsets converteren naar schaalsets met lage prioriteit
Nee, als u de vlag met lage prioriteit wordt alleen ondersteund tijdens het maken.

### <a name="can-i-create-a-scale-set-with-both-regular-vms-and-low-priority-vms"></a>Kan ik een schaal ingesteld met zowel reguliere VM's en VMs met lage prioriteit maken?
Nee, kan een schaalset niet meer dan één prioriteit type ondersteunen.

### <a name="how-is-quota-managed-for-low-priority-vms"></a>Hoe worden de quota voor virtuele machines prioriteit Laag beheerd?
Prioriteit Laag VM's en reguliere VMs delen dezelfde groep quotum. 

### <a name="can-i-use-autoscale-with-low-priority-scale-sets"></a>Kan ik automatisch schalen met lage prioriteit schaalsets gebruiken?
Ja, kunt u regels voor automatisch schalen instellen op uw schaalset met lage prioriteit. Als uw VM's zijn verwijderd, proberen automatisch schalen voor het maken van nieuwe virtuele machines voor lage prioriteit. Denk eraan dat u deze capaciteit echter niet worden gegarandeerd. 

### <a name="does-autoscale-work-with-both-eviction-policies-deallocate-and-delete"></a>Automatisch schalen werkt met beide beleidsregels taakverwijdering (toewijzing ongedaan maken en verwijderen)?
Het is raadzaam dat u uw beleid verwijderen om te verwijderen wanneer ze met automatisch schalen instellen. Dit is omdat de toewijzing ongedaan is gemaakt exemplaren op basis van het aantal capaciteit op de schaalaanpassingsset worden geteld. Wanneer u automatisch schalen, bereikt u waarschijnlijk de exemplaren van uw doel snel omdat de toewijzing ongedaan is gemaakt, verwijderde exemplaren. 

## <a name="next-steps"></a>Volgende stappen
Nu u een schaal ingesteld met lage prioriteit VM's hebt gemaakt, proberen implementeren onze [automatisch schalen sjabloon met lage prioriteit](https://github.com/Azure/vm-scale-sets/tree/master/preview/lowpri).

Bekijk de [virtuele-machineschaalset pagina met prijzen](https://azure.microsoft.com/pricing/details/virtual-machine-scale-sets/linux/) voor prijsinformatie.