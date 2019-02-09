---
title: Maken van een Azure-machineschaalset die gebruikmaakt van VM's met lage prioriteit (Preview) | Microsoft Docs
description: Informatie over het maken van virtuele Azure-machine-schaalsets met virtuele machines met lage prioriteit om op te slaan op de kosten van
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
ms.openlocfilehash: 861c68ae8163e0ba8c2af2a3d96153ac3e84855f
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2019
ms.locfileid: "55978133"
---
# <a name="low-priority-vms-on-scale-sets-preview"></a>Virtuele machines met lage prioriteit op schaalsets (preview)

Met behulp van virtuele machines met lage prioriteit op schaalsets, kunt u profiteren van onze unutilized capaciteit op een aanzienlijke kostenbesparingen. Op elk gewenst moment in de tijd wanneer de capaciteit terug in Azure nodig heeft, wordt de Azure-infrastructuur onbeschikbaar maken VM's met lage prioriteit. Virtuele machines met lage prioriteit zijn daarom ideaal voor workloads die onderbrekingen, zoals batchverwerking, taken, dev/test-omgevingen en grote rekenworkloads kunnen worden verwerkt.

De hoeveelheid beschikbare unutilized capaciteit kan variëren op basis van grootte, regio en tijd van de dag. Bij het implementeren van VM's met lage prioriteit op schaal wordt ingesteld, wordt Azure de virtuele machines toewijzen als er capaciteit beschikbaar, maar er geen SLA voor deze virtuele machines is. Een schaalset met lage prioriteit wordt geïmplementeerd in een enkele foutdomein en biedt dat geen hoge beschikbaarheid garandeert.

## <a name="eviction-policy"></a>Verwijderingsbeleid

Bij het maken van schaalsets met lage prioriteit, kunt u het verwijderingsbeleid instellen op *toewijzing ongedaan maken* (standaard) of *verwijderen*. 

De *toewijzing ongedaan maken* beleid voor de verwijderde virtuele machines verplaatst naar de status stopped-deallocated hebben zodat u kunt verwijderde exemplaren implementeren. Er is echter geen garantie dat de toewijzing is wel mogelijk. De toewijzing ongedaan gemaakt virtuele machines wordt in mindering uw quotum scale set-exemplaar en u wordt gefactureerd voor de onderliggende schijven. 

Als u uw VM's in uw schaalset met lage prioriteit worden verwijderd wilt als ze zijn verwijderd, kunt u het verwijderingsbeleid instellen op *verwijderen*. Met het verwijderingsbeleid instellen om te verwijderen, kunt u nieuwe virtuele machines maken door het verhogen van de schaal-eigenschap voor count set-exemplaar. De verwijderde virtuele machines worden verwijderd, samen met hun onderliggende schijven, en daarom u wordt niet in rekening gebracht voor de opslag. U kunt ook de functie voor automatisch schalen van schaalsets gebruiken om automatisch te compenseren voor de verwijderde virtuele machines, maar er is geen garantie dat de toewijzing is wel mogelijk. Het verdient aanbeveling om dat u de functie voor automatisch schalen op schaalsets met lage prioriteit alleen gebruiken bij het instellen van het verwijderingsbeleid verwijderen om te voorkomen dat de kosten van uw schijven en de quotumlimiet hebt bereikt. 

> [!NOTE]
> Tijdens de preview, kunt u zich aan het verwijderingsbeleid instellen met behulp van de [Azure-portal](#use-the-azure-portal) en [Azure Resource Manager-sjablonen](#use-azure-resource-manager-templates). 

## <a name="deploying-low-priority-vms-on-scale-sets"></a>Hiermee stelt u het implementeren van virtuele machines met lage prioriteit op schaal

Als u wilt implementeren met lage prioriteit VM's op schaalsets, kunt u instellen de nieuwe *prioriteit* markering *laag*. Alle virtuele machines in uw schaalset worden ingesteld op prioriteit laag. Voor het maken van een schaalset met virtuele machines met lage prioriteit, moet u een van de volgende methoden gebruiken:
- [Azure Portal](#use-the-azure-portal)
- Azure-CLI
- [Azure PowerShell](#use-azure-powershell)
- [Azure Resource Manager-sjablonen](#use-azure-resource-manager-templates)

## <a name="use-the-azure-portal"></a>Azure Portal gebruiken

Het proces voor het maken van een schaalset die gebruikmaakt van virtuele machines met lage prioriteit is hetzelfde als beschreven in de [aan de slag artikel](quick-create-portal.md). Wanneer u een schaalset implementeert, kunt u kiezen of u de vlag met lage prioriteit en het verwijderingsbeleid instellen: ![Maak een schaalset met virtuele machines met lage prioriteit](media/virtual-machine-scale-sets-use-low-priority/vmss-low-priority-portal.png)

## <a name="use-the-azure-cli"></a>Azure CLI gebruiken

Het proces voor het maken van een schaalset met virtuele machines met lage prioriteit is hetzelfde als beschreven in de [aan de slag artikel](quick-create-cli.md). Alleen toe te voegen de '--prioriteit ' parameter voor de cli aanroepen en stel deze in op *laag* zoals wordt weergegeven in het voorbeeld hieronder:

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

Het proces voor het maken van een schaalset met virtuele machines met lage prioriteit is hetzelfde als beschreven in de [aan de slag artikel](quick-create-powershell.md).
Alleen toe te voegen de '-prioriteit ' parameter voor de [New-AzVmssConfig](/powershell/module/az.compute/new-azvmssconfig) en stel deze in op *laag* zoals wordt weergegeven in het voorbeeld hieronder:

```powershell
$vmssConfig = New-AzVmssConfig `
    -Location "East US 2" `
    -SkuCapacity 2 `
    -SkuName "Standard_DS2" `
    -UpgradePolicyMode Automatic `
    -Priority "Low"
```

## <a name="use-azure-resource-manager-templates"></a>Azure Resource Manager-sjablonen gebruiken

Het proces voor het maken van een schaalset die gebruikmaakt van virtuele machines met lage prioriteit is hetzelfde als in de aan de slag-artikel voor gedetailleerde [Linux](quick-create-template-linux.md) of [Windows](quick-create-template-windows.md). Voeg de eigenschap 'priority' aan de *Microsoft.Compute/virtualMachineScaleSets/virtualMachineProfile* resource Typ in de sjabloon en geef *laag* als de waarde. Zorg ervoor dat u *2018-03-01* API-versie of hoger. 

De parameter 'evictionPolicy' toevoegen om in te stellen het verwijderingsbeleid te verwijderen, en stel deze in op *verwijderen*.

Het volgende voorbeeld wordt een Linux met lage prioriteit schaalset met de naam *myScaleSet* in *West-Centraal VS*, welke *verwijderen* de virtuele machines in de schaalset op verwijdering:

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

### <a name="can-i-convert-existing-scale-sets-to-low-priority-scale-sets"></a>Kan ik bestaande schaalsets converteren naar de met lage prioriteit schaalsets?
Nee, als u de vlag met lage prioriteit wordt alleen ondersteund tijdens het maken.

### <a name="can-i-create-a-scale-set-with-both-regular-vms-and-low-priority-vms"></a>Kan ik een schaalset met zowel normale VM's en virtuele machines met lage prioriteit maken?
Nee, kan een schaalset niet meer dan één type van prioriteit ondersteunen.

### <a name="how-is-quota-managed-for-low-priority-vms"></a>Hoe wordt het quotum voor virtuele machines met lage prioriteit beheerd?
Reguliere virtuele machines en virtuele machines met lage prioriteit delen dezelfde groep quotum. 

### <a name="can-i-use-autoscale-with-low-priority-scale-sets"></a>Kan ik automatisch schalen met schaalsets met lage prioriteit gebruiken?
Ja, kunt u regels voor automatisch schalen instellen op uw schaalset met lage prioriteit. Als uw virtuele machines zijn verwijderd, worden voor automatisch schalen kunt proberen te maken van nieuwe VM's met lage prioriteit. Denk eraan dat u deze capaciteit echter niet worden gegarandeerd. 

### <a name="does-autoscale-work-with-both-eviction-policies-deallocate-and-delete"></a>Automatisch schalen werkt met beide beleidsregels voor taakverwijdering (toewijzing ongedaan maken en verwijderen)?
Het is raadzaam dat u het verwijderingsbeleid verwijderen bij het gebruik van automatisch schalen instellen. Dit is omdat de toewijzing ongedaan gemaakt exemplaren op basis van het aantal capaciteit op de schaalset worden geteld. Wanneer automatisch schalen gebruik, bereikt u waarschijnlijk uw doel-exemplaren snel omdat de toewijzing ongedaan wordt gemaakt, verwijderde exemplaren. 

## <a name="next-steps"></a>Volgende stappen
Nu u een schaalset met virtuele machines met lage prioriteit hebt gemaakt, kunt u proberen implementeren onze [automatisch schalen sjabloon gebruiken met lage prioriteit](https://github.com/Azure/vm-scale-sets/tree/master/preview/lowpri).

Bekijk de [virtuele-machineschaalset pagina met prijzen](https://azure.microsoft.com/pricing/details/virtual-machine-scale-sets/linux/) voor prijsgegevens.
