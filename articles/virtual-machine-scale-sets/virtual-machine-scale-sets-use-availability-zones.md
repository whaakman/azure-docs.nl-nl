---
title: Maken van een Azure-machineschaalset die gebruikmaakt van Beschikbaarheidszones | Microsoft Docs
description: Informatie over het maken van virtuele Azure-machine-schaalsets met Beschikbaarheidszones voor verhoogde redundantie tegen storingen
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm
ms.devlang: na
ms.topic: article
ms.date: 08/08/2018
ms.author: cynthn
ms.openlocfilehash: 18d9a2dade271e61fa3db423da610a7f982aa47b
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46949669"
---
# <a name="create-a-virtual-machine-scale-set-that-uses-availability-zones"></a>Maken van een virtuele-machineschaalset die gebruikmaakt van Beschikbaarheidszones

Als u wilt voorkomen dat uw virtuele-machineschaalsets datacentrumniveau fouten, kunt u een schaalset in meerdere Beschikbaarheidszones. Azure-regio's die ondersteuning bieden voor Beschikbaarheidszones hebben een minimum van drie afzonderlijke zones, elk met hun eigen onafhankelijke voeding bron, het netwerk en koeling. Zie voor meer informatie, [overzicht van Beschikbaarheidszones](../availability-zones/az-overview.md).

## <a name="availability-considerations"></a>Beschikbaarheidsoverwegingen

Bij het implementeren van een schaalset in een of meer zones vanaf API-versie *2017-12-01*, hebt u de mogelijkheid om te implementeren met 'maximale spreiding' of 'statische 5 fouttolerantie domein spreiden'. De schaalset verspreidt met maximale spreiding uw VM's op zoals veel mogelijk in elke zone foutdomeinen. Deze spreiden kan zijn in meer of minder dan vijf foutdomeinen per zone. Uw virtuele machines worden in de schaalset met "statische 5 fouttolerantie domein spreiden" verspreidt over exact vijf foutdomeinen per zone. Als de schaalset niet vijf verschillende foutdomeinen per zone om te voldoen aan de aanvraag voor geheugentoewijzing vinden, de aanvraag is mislukt.

**Het is raadzaam implementeren met maximale spreiding voor de meeste workloads**, zoals deze aanpak geeft u het beste verspreiden in de meeste gevallen. Als u replica's worden verdeeld over verschillende hardware isolatie-eenheden, we raden u aan te spreiden over Beschikbaarheidszones en gebruikmaken van maximale spreiding in elke zone.

Met maximale spreiding ziet u slechts één domein met fouten in de weergave scale set VM-exemplaar en de metagegevens van het exemplaar, ongeacht het aantal foutdomeinen die de virtuele machines worden verdeeld over. De spreiding in elke zone is impliciete.

Als u wilt gebruiken voor maximale spreiding, *platformFaultDomainCount* naar *1*. Voor het gebruik van statische vijf fouten-domain netwerkmappen, stel *platformFaultDomainCount* naar *5*. In API-versie *2017-12-01*, *platformFaultDomainCount* standaard ingesteld op *1* voor één zone en zoneoverschrijdende schaal wordt ingesteld. Op dit moment wordt alleen statische vijf foutdomeinen domein spreiding ondersteund voor regionale schaalsets.

### <a name="placement-groups"></a>Plaatsingsgroepen

Wanneer u een schaalset implementeert, hebt u ook de mogelijkheid om te implementeren met één [plaatsingsgroep](./virtual-machine-scale-sets-placement-groups.md) per binnen een Beschikbaarheidszone of met meerdere per zone. Voor regionale schaalsets is de keuze hebt van één plaatsingsgroep in de regio of meerdere in de regio. Voor de meeste workloads, wordt aangeraden meerdere plaatsingsgroepen, zodat voor grotere schaal. In API-versie *2017-12-01*, standaard-schaalsets naar meerdere plaatsingsgroepen voor één zone en zoneoverschrijdende schaalsets, maar worden standaard één plaatsingsgroep voor regionale schaalsets.

> [!NOTE]
> Als u de maximale spreiding gebruikt, moet u meerdere plaatsingsgroepen.

### <a name="zone-balancing"></a>Zone Netwerktaakverdeling

Ten slotte voor schaalsets geïmplementeerd in meerdere zones, hebt u ook de mogelijkheid van een "aanbevolen inspanning zone balance" of "strikte zonebalans" te kiezen. Een schaalset wordt beschouwd als 'met gelijke taakverdeling' als elke zone hetzelfde aantal virtuele machines of +\\-1 virtuele machine in andere zones voor de schaalset. Bijvoorbeeld:

- Een schaalset met 2 virtuele machines in zone 1, 3 VM's in zone 2 en 3 virtuele machines in zone 3 wordt beschouwd als met gelijke taakverdeling. Er is slechts één zone met een ander aantal voor VM's en is slechts 1 kleiner is dan de andere zones. 
- Een schaalset instellen met 1 virtuele machine in zone 1, 3 VM's in zone 2 en 3 virtuele machines in zone 3 wordt beschouwd als niet-regelmatige. Zone 1 heeft 2 minder virtuele machines dan zones 2 en 3.

Het is mogelijk dat virtuele machines in de schaalset zijn gemaakt, maar niet voldoen aan-extensies op deze virtuele machines te implementeren. Deze VM's met extensies oplossen worden nog steeds geteld wanneer u bepaalt als een schaalset met gelijke taakverdeling. Bijvoorbeeld: een schaalset met 3 VM's in zone 1, 3 VM's in zone 2 en 3 VM's in zone 3 wordt beschouwd als met gelijke taakverdeling, zelfs als alle extensies is mislukt in zone 1 en alle extensies geslaagd in zones 2 en 3.

Met best-effort zonebalans schaalset de pogingen tot inschalen en uitschalen behoud verdelen. Echter, als voor een of andere reden dit niet mogelijk is (bijvoorbeeld, als één zone uitgeschakeld wordt, de schaalset kan niet maakt een nieuwe virtuele machine in de zone), de schaalset kunt tijdelijke imbalance met succes kunt in- of uitschalen. Op volgende pogingen tot scale-out, wordt de schaalset VM's toegevoegd voor zones die u hebt meer virtuele machines nodig voor de schaalset worden verdeeld. Op dezelfde manier op schaal van de volgende pogingen verwijdert de schaalset VM's van de zones die minder VM's nodig hebt voor de schaalset worden verdeeld. Met "strikte zonebalans" mislukt de schaalset elke poging om te schalen in- of uitgeschaald als dit leiden ongebalanceerde tot zou.

Instellen voor het gebruik van best-effort zonebalans *zoneBalance* naar *false*. Dit is de standaardinstelling in API-versie *2017-12-01*. Instellen voor het gebruik van strikte zonebalans *zoneBalance* naar *waar*.

## <a name="single-zone-and-zone-redundant-scale-sets"></a>Één zone en zone-redundante schaalsets

Wanneer u een virtuele-machineschaalset implementeert, kunt u kiezen om één Beschikbaarheidszone in een regio of meerdere zones te gebruiken.

Wanneer u een schaalset maken in een enkele zone, u bepaalt welke zone alle deze VM-exemplaren in de uitgevoerd en de schaalset wordt beheerd en automatisch wordt geschaald alleen in deze zone. Een zone-redundante schaalset kunt u één schaalset die meerdere zones omvat maken. Als de VM-exemplaren zijn gemaakt, wordt standaard worden ze gelijkmatig verdeeld over zones. Moet een onderbreking optreedt op een van de zones, een schaalset wordt niet automatisch worden uitgebreid om capaciteit te vergroten. Een best practice is het configureren van regels voor automatisch schalen op basis van CPU-en geheugengebruik. De regels voor automatisch schalen kunnen de schaal die is ingesteld om te reageren op een verlies van de VM-exemplaren in een zone door nieuwe instanties in de resterende operationele zones uitschaling.

Als u wilt gebruiken voor Beschikbaarheidszones, moet uw schaalset worden gemaakt in een [ondersteunde Azure-regio](../availability-zones/az-overview.md#regions-that-support-availability-zones). U kunt een schaalset die gebruikmaakt van Beschikbaarheidszones met een van de volgende manieren maken:

- [Azure Portal](#use-the-azure-portal)
- [Azure-CLI](#use-the-azure-cli-20)
- [Azure PowerShell](#use-azure-powershell)
- [Azure Resource Manager-sjablonen](#use-azure-resource-manager-templates)

## <a name="use-the-azure-portal"></a>Azure Portal gebruiken

Het proces voor het maken van een schaalset die gebruikmaakt van een Beschikbaarheidszone is hetzelfde als beschreven in de [aan de slag artikel](quick-create-portal.md). Wanneer u een ondersteunde Azure-regio selecteert, kunt u een schaalset in een of meer van de beschikbare zones maken zoals wordt weergegeven in het volgende voorbeeld:

![Een schaalset maken in één Beschikbaarheidszone](media/virtual-machine-scale-sets-use-availability-zones/vmss-az-portal.png)

De schaalset en ondersteunende resources, zoals de Azure load balancer en het openbare IP-adressen worden gemaakt in de één zone die u opgeeft.

## <a name="use-the-azure-cli"></a>Azure CLI gebruiken

Het proces voor het maken van een schaalset die gebruikmaakt van een Beschikbaarheidszone is hetzelfde als beschreven in de [aan de slag artikel](quick-create-cli.md). Als u wilt gebruiken voor Beschikbaarheidszones, moet u de schaalset in een ondersteunde Azure-regio maken.

Toevoegen de `--zones` parameter voor de [az vmss maken](/cli/azure/vmss#az_vmss_create) opdracht en geeft u welke zone te gebruiken (zoals zone *1*, *2*, of *3*). Het volgende voorbeeld wordt een één zone schaalset met de naam *myScaleSet* in zone *1*:

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

Voor een compleet voorbeeld van een schaalset met één zone en -netwerkbronnen, raadpleegt u [dit CLI-voorbeeldscript](https://github.com/Azure/azure-docs-cli-python-samples/blob/master/virtual-machine-scale-sets/create-single-availability-zone/create-single-availability-zone.sh.)

### <a name="zone-redundant-scale-set"></a>Zone-redundante schaalset

Om te maken van een zone-redundante schaalset, gebruikt u een *Standard* SKU openbare IP-adres en de load balancer. Voor uitgebreide redundantie, de *Standard* SKU maakt zone-redundante netwerkbronnen. Zie voor meer informatie, [overzicht van Azure Load Balancer Standard](../load-balancer/load-balancer-standard-overview.md) en [Standard Load Balancer en Beschikbaarheidszones](../load-balancer/load-balancer-standard-availability-zones.md).

Geef voor het maken van een zone-redundante schaalset meerdere zones met de `--zones` parameter. Het volgende voorbeeld wordt een zone-redundante schaalset met de naam *myScaleSet* in zones *1,2,3*:

```azurecli
az vmss create \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --image UbuntuLTS \
    --upgrade-policy-mode automatic \
    --admin-username azureuser \
    --generate-ssh-keys \
    --zones 1 2 3
```

Het duurt enkele minuten om te maken en configureren van alle de schaalset VM's en resources in de zone (s) die u opgeeft. Voor een compleet voorbeeld van een zone-redundante schaalset ingesteld en netwerkbronnen, Zie [dit CLI-voorbeeldscript](https://github.com/Azure/azure-docs-cli-python-samples/blob/master/virtual-machine-scale-sets/create-zone-redundant-scale-set/create-zone-redundant-scale-set.sh)

## <a name="use-azure-powershell"></a>Azure PowerShell gebruiken

Als u wilt gebruiken voor Beschikbaarheidszones, moet u de schaalset in een ondersteunde Azure-regio maken. Toevoegen de `-Zone` parameter voor de [New-AzureRmVmssConfig](/powershell/module/azurerm.compute/new-azurermvmssconfig) opdracht en geeft u welke zone te gebruiken (zoals zone *1*, *2*, of *3*).

Het volgende voorbeeld wordt een één zone schaalset met de naam *myScaleSet* in *VS-Oost 2* zone *1*. De Azure-netwerkresources voor het virtuele netwerk, het openbare IP-adres en de load balancer worden automatisch gemaakt. Geef desgevraagd uw eigen beheerdersreferenties op voor de VM-exemplaren in de schaalset:

```powershell
New-AzureRmVmss `
  -ResourceGroupName "myResourceGroup" `
  -Location "EastUS2" `
  -VMScaleSetName "myScaleSet" `
  -VirtualNetworkName "myVnet" `
  -SubnetName "mySubnet" `
  -PublicIpAddressName "myPublicIPAddress" `
  -LoadBalancerName "myLoadBalancer" `
  -UpgradePolicy "Automatic" `
  -Zone "1"
```

### <a name="zone-redundant-scale-set"></a>Zone-redundante schaalset

Geef voor het maken van een zone-redundante schaalset meerdere zones met de `-Zone` parameter. Het volgende voorbeeld wordt een zone-redundante schaalset met de naam *myScaleSet* via *VS-Oost 2* zones *1, 2, 3*. De zone-redundante Azure-netwerkresources voor virtueel netwerk, de openbare IP-adres en de load balancer worden automatisch gemaakt. Geef desgevraagd uw eigen beheerdersreferenties op voor de VM-exemplaren in de schaalset:

```powershell
New-AzureRmVmss `
  -ResourceGroupName "myResourceGroup" `
  -Location "EastUS2" `
  -VMScaleSetName "myScaleSet" `
  -VirtualNetworkName "myVnet" `
  -SubnetName "mySubnet" `
  -PublicIpAddressName "myPublicIPAddress" `
  -LoadBalancerName "myLoadBalancer" `
  -UpgradePolicy "Automatic" `
  -Zone "1", "2", "3"
```

## <a name="use-azure-resource-manager-templates"></a>Azure Resource Manager-sjablonen gebruiken

Het proces voor het maken van een schaalset die gebruikmaakt van een Beschikbaarheidszone is hetzelfde als in de aan de slag-artikel voor gedetailleerde [Linux](quick-create-template-linux.md) of [Windows](quick-create-template-windows.md). Als u wilt gebruiken voor Beschikbaarheidszones, moet u de schaalset in een ondersteunde Azure-regio maken. Toevoegen de `zones` eigenschap in op de *Microsoft.Compute/virtualMachineScaleSets* resource typt u in uw sjabloon en geef welke zone te gebruiken (zoals zone *1*, *2*, of *3*).

Het volgende voorbeeld wordt een Linux één zone schaalset met de naam *myScaleSet* in *VS-Oost 2* zone *1*:

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

Voor een compleet voorbeeld van een schaalset met één zone en -netwerkbronnen, raadpleegt u [in dit voorbeeld Resource Manager-sjabloon](https://github.com/Azure/vm-scale-sets/blob/master/preview/zones/singlezone.json)

### <a name="zone-redundant-scale-set"></a>Zone-redundante schaalset

Geef voor het maken van een zone-redundante schaalset meerdere waarden in de `zones` eigenschap voor de *Microsoft.Compute/virtualMachineScaleSets* resourcetype. Het volgende voorbeeld wordt een zone-redundante schaalset met de naam *myScaleSet* via *VS-Oost 2* zones *1,2,3*:

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

Als u een openbaar IP-adres of een load balancer maakt, geeft u de *"sku": {'name': 'Standaard'} '* eigenschap om zone-redundante netwerkresources te maken. U moet ook een Netwerkbeveiligingsgroep en regels om verkeer toe te maken. Zie voor meer informatie, [overzicht van Azure Load Balancer Standard](../load-balancer/load-balancer-standard-overview.md) en [Standard Load Balancer en Beschikbaarheidszones](../load-balancer/load-balancer-standard-availability-zones.md).

Voor een compleet voorbeeld van een zone-redundante schaalset ingesteld en netwerkbronnen, Zie [in dit voorbeeld Resource Manager-sjabloon](https://github.com/Azure/vm-scale-sets/blob/master/preview/zones/multizone.json)

## <a name="next-steps"></a>Volgende stappen

Nu dat u een schaalset in een Beschikbaarheidszone hebt gemaakt, leert u hoe u [toepassingen implementeren op de virtuele machine-schaalsets](tutorial-install-apps-cli.md) of [automatisch schalen gebruiken met virtuele-machineschaalsets](tutorial-autoscale-cli.md).
