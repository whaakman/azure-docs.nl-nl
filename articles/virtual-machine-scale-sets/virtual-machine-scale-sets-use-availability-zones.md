---
title: Een Azure-schaalset die gebruikmaakt van beschikbaarheid Zones maken | Microsoft Docs
description: Informatie over het maken van virtuele Azure-machine-schaalsets die gebruikmaken van beschikbaarheid Zones voor verbeterde redundantie tegen uitval
services: virtual-machine-scale-sets
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm
ms.devlang: na
ms.topic: article
ms.date: 03/07/2018
ms.author: iainfou
ms.openlocfilehash: dee06eee045bc24c2864333a66a6d145a771b3ad
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2018
---
# <a name="create-a-virtual-machine-scale-set-that-uses-availability-zones"></a>Een virtuele-machineschaalset die gebruikmaakt van beschikbaarheid Zones maken
Om te voorkomen dat uw virtuele-machineschaalsets fouten datacenter-niveau, kunt u een schaal instellen voor beschikbaarheid Zones maken. Azure-regio's die ondersteuning bieden voor beschikbaarheid Zones hebben een minimum van drie afzonderlijke zones, elk met hun eigen onafhankelijke energie-bron-, netwerk- en koeling. Zie voor meer informatie [overzicht van de Zones van de beschikbaarheid](../availability-zones/az-overview.md).


## <a name="single-zone-and-zone-redundant-scale-sets"></a>Single-zone en zone-redundante schaalsets
Wanneer u een virtuele-machineschaalset implementeert, kunt u een enkele Zone beschikbaarheid in een regio of meerdere zones te gebruiken.

Als u een schaal instellen in een enkele zone, u de controle over welke zone worden uitgevoerd in alle exemplaren van deze virtuele machine en de schaalaanpassingsset wordt beheerd en autoscales alleen in deze zone maakt. Een zone-redundante schaalset kunt u een set met één scale die meerdere zones omvat maken. Als de VM-exemplaren zijn gemaakt, standaard zijn ze gelijkmatig verdeeld zijn over zones. Moet een onderbreking optreedt op een van de zones, een schaalset wordt niet automatisch worden uitgebreid om capaciteit te vergroten. Een aanbevolen procedure kan worden regels voor automatisch schalen op basis van gebruik van CPU of geheugen configureren. De regels voor automatisch schalen, zou de schaal ingesteld om te reageren op een verlies van de VM-instanties die een zone door het uitbreiden van nieuwe exemplaren in de resterende operationele zones toestaan.

Beschikbaarheid om Zones te gebruiken, moet uw schaalset worden gemaakt in een [ondersteunde Azure-regio](../availability-zones/az-overview.md#regions-that-support-availability-zones). U kunt een schaalset die gebruikmaakt van beschikbaarheid Zones met een van de volgende manieren maken:

- [Azure Portal](#use-the-azure-portal)
- [Azure CLI 2.0](#use-the-azure-cli-20)
- [Azure PowerShell](#use-azure-powershell)
- [Azure Resource Manager-sjablonen](#use-azure-resource-manager-templates)

## <a name="availability-considerations"></a>Beschikbaarheidsoverwegingen
Vanaf API versie 2017-12-01, wanneer u een schaal instellen in een of meer zones implementeert, hebt u de optie voor het implementeren van met 'max spreiden' of 'statische 5 veroorzaakt domein spreiden'. Met max. spreiden verspreid de schaalaanpassingsset uw virtuele machines op zoals veel domeinen mogelijk binnen elke zone fault. Via deze spreiden kan worden meer of minder dan vijf fault-domeinen per zone. Aan de andere kant met 'statische 5 veroorzaakt domein spreiden', de schaalaanpassingsset selecteert, worden uw virtuele machines via precies 5 foutdomeinen per zone. Als de schaalaanpassingsset 5 distinct foutdomeinen per zone om te voldoen aan de aanvraag voor geheugentoewijzing niet wordt gevonden, wordt de aanvraag mislukt.

**Het is raadzaam implementeren met maximale spreiden voor de meeste belastingen** omdat max spreiden biedt de beste spreiden in de meeste gevallen. Als u replica's worden verdeeld over verschillende hardware-isolatie eenheden nodig hebt, raden wij verspreid over beschikbaarheid zones en het gebruik van max spreiden binnen elke zone. Houd er rekening mee dat met maximale spreiden, u alleen een domein met fouten in de instantieweergave van de virtuele machine en in de metagegevens van het exemplaar ongeacht hoeveel foutdomeinen de virtuele machines daadwerkelijk worden verdeeld ziet over; de spreiding in elke zone is impliciet.

Voor het gebruik van max spreiden, stelt u 'platformFaultDomainCount' in op 1. Voor het gebruik van statische 5 veroorzaakt domein spreiden 'platformFaultDomainCount' te ingesteld op 5. In versie 2017-12-01-API 'platformFaultDomainCount' wordt standaard ingesteld op 1 voor één-zone en cross-zone-schaalsets. Alleen statische 5 veroorzaakt domein spreiden wordt momenteel ondersteund voor schaalsets van regionale.

Bovendien wanneer u een schaalset implementeert, hebt u de optie voor het implementeren van met één [plaatsing groep](./virtual-machine-scale-sets-placement-groups.md) per zone van de beschikbaarheid of met meerdere per zone (voor regionale schaalsets, de keuze is voor een groep één positie hebben de regio of meerdere in de regio). Voor de meeste belastingen, wordt u aangeraden meerdere plaatsing groepen waarmee voor groter schaal. In versie 2017-12-01-API, schaal stelt standaard aan meerdere groepen voor plaatsing voor één-zone en cross-zone-schaalsets, maar ze standaard aan de groep één plaatsing van regionale schaal instellen.

>[!NOTE]
> Als u het maximale spreiden gebruikt, moet u meerdere groepen voor plaatsing.

Ten slotte voor schaal wordt geïmplementeerd op meerdere zones hebt u ook de optie van het kiezen van 'best effort zone saldo' of 'strict zone saldo'. Een schaalset wordt beschouwd als voor 'met gelijke taakverdeling' als het aantal virtuele machines in elke zone zich in een van het aantal VM's in alle zones voor de scale-set. Voor het exemplaar, een schaal met 2 virtuele machines in de zone 1, 3 VM's in de zone 2 en 3 virtuele machines in de zone 3 ingesteld wordt beschouwd als met gelijke taakverdeling. Echter, een schaal met 1 VM in de zone 1 is ingesteld, 3 VM's in de zone wordt beschouwd als 2 en 3 VM's in de zone 3 onbalans gebracht. Het is mogelijk dat virtuele machines in de schaalset zijn gemaakt, terwijl de uitbreiding van deze VMs mislukken. Deze virtuele machines met fouten voor uitbreiding worden nog steeds meegeteld bij het bepalen van als een scale-set is verdeeld. Bijvoorbeeld, een schaal met 3 VM's in de zone 1, 3 VM's in de zone 2 en 3 virtuele machines in de zone 3 ingesteld wordt beschouwd als de taakverdeling zelfs als alle extensies is mislukt in de zone 1 en alle uitbreidingen is voltooid in zones 2 en 3. De schaal ingesteld met best effort zone-saldo pogingen af te schalen en behoud verdelen. Echter, als voor een bepaalde reden dit niet mogelijk is (bijvoorbeeld één zone uitvalt, zodat de schaalaanpassingsset kan niet een nieuwe virtuele machine in de zone maken), kunt de schaalaanpassingsset tijdelijke ongebalanceerde om te schalen met succes in of uit. Op de volgende scale-out pogingen voegt de schaalaanpassingsset virtuele machines aan zones die meer virtuele machines nodig voor de schaal instelt verdeeld. Op deze manier op latere schaal van pogingen verwijdert de schaalaanpassingsset VM's van zones die minder virtuele machines nodig voor de schaal instelt verdeeld. Met 'strict zone saldo' mislukt de schaalaanpassingsset aan de andere kant alle pogingen om te schalen in- of als u dit zou leiden tot ongebalanceerde.

Ingesteld voor het gebruik van best effort zone saldo 'zoneBalance' in op false (de standaardinstelling in versie 2017-12-01-API). Voor het gebruik van de strikte zone saldo 'zoneBalance' te ingesteld op true.

## <a name="use-the-azure-portal"></a>Azure Portal gebruiken
Het proces voor het maken van een schaalset die gebruikmaakt van een Zone beschikbaarheid is hetzelfde als is beschreven in de [artikel aan de slag](quick-create-portal.md). Zorg ervoor dat u hebt [geregistreerd voor een voorbeeld bekijken van de beschikbaarheid van Zones](http://aka.ms/azenroll). Wanneer u een ondersteunde Azure-regio selecteert, kunt u een schaal instellen in een van de beschikbare zones kunt maken, zoals wordt weergegeven in het volgende voorbeeld:

![Een schaal instellen in een enkele beschikbaarheid Zone maken](media/virtual-machine-scale-sets-use-availability-zones/create-portal-single-az.png)

De schaalaanpassingsset en de ondersteunende resources, zoals de Azure load balancer en het openbare IP-adres worden gemaakt in de enkele zone die u opgeeft.


## <a name="use-the-azure-cli-20"></a>De Azure CLI 2.0 gebruiken
Het proces voor het maken van een schaalset die gebruikmaakt van een Zone beschikbaarheid is hetzelfde als is beschreven in de [artikel aan de slag](quick-create-cli.md). Beschikbaarheid om Zones te gebruiken, moet u uw scale set maken in een ondersteunde Azure-regio en hebben [geregistreerd voor een voorbeeld bekijken van de beschikbaarheid van Zones](http://aka.ms/azenroll).

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
Maken van een zone-redundante schaal instellen, gebruikt u een *standaard* SKU openbare IP-adres en de load balancer. Voor uitgebreide redundantie de *standaard* SKU maakt zone-redundante netwerkbronnen. Zie voor meer informatie [overzicht van Azure Load Balancer Standard](../load-balancer/load-balancer-standard-overview.md). 

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
Het proces voor het maken van een schaalset die gebruikmaakt van een Zone beschikbaarheid is hetzelfde als is beschreven in de [artikel aan de slag](quick-create-powershell.md). Beschikbaarheid om Zones te gebruiken, moet u uw scale set maken in een ondersteunde Azure-regio en hebben [geregistreerd voor een voorbeeld bekijken van de beschikbaarheid van Zones](http://aka.ms/azenroll). Toevoegen de `-Zone` -parameter voor de [nieuw AzureRmVmssConfig](/powershell/module/azurerm.compute/new-azurermvmssconfig) opdracht en opgeven welke zone moet worden gebruikt (zoals zone *1*, *2*, of *3*). 

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
Maken van een zone-redundante schaal instellen, gebruikt u een *standaard* SKU openbare IP-adres en de load balancer. Voor uitgebreide redundantie de *standaard* SKU maakt zone-redundante netwerkbronnen. Zie voor meer informatie [overzicht van Azure Load Balancer Standard](../load-balancer/load-balancer-standard-overview.md).

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
Het proces voor het maken van een schaalset die gebruikmaakt van een Zone beschikbaarheid is hetzelfde als is beschreven in het artikel in ophalen gestart voor [Linux](quick-create-template-linux.md) of [Windows](quick-create-template-windows.md). Beschikbaarheid om Zones te gebruiken, moet u uw scale set maken in een ondersteunde Azure-regio en hebben [geregistreerd voor een voorbeeld bekijken van de beschikbaarheid van Zones](http://aka.ms/azenroll). Voeg de `zones` eigenschap in op de *Microsoft.Compute/virtualMachineScaleSets* resource typt u in uw sjabloon en opgeven welke zone moet worden gebruikt (zoals zone *1*, *2*, of *3*).

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

Voor een compleet voorbeeld van de schaal van een enkele zone en netwerkbronnen, raadpleegt u [in dit voorbeeld Resource Manager-sjabloon](https://github.com/Azure/vm-scale-sets/blob/master/zones/singlezone.json)

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

Voor een compleet voorbeeld van een zone-redundante schaal en netwerkbronnen, raadpleegt u [in dit voorbeeld Resource Manager-sjabloon](https://github.com/Azure/vm-scale-sets/blob/master/zones/multizone.json)


## <a name="next-steps"></a>Volgende stappen
Nu dat u een schaal instellen in een Zone beschikbaarheid hebt gemaakt, leert u hoe u [toepassingen implementeren op virtuele machine schalen sets](virtual-machine-scale-sets-deploy-app.md) of [automatisch schalen gebruiken met virtuele-machineschaalsets](virtual-machine-scale-sets-autoscale-overview.md).
