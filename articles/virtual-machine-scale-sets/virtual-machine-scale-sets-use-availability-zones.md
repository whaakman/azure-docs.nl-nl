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
ms.date: 04/05/2018
ms.author: iainfou
ms.openlocfilehash: 9093fcf008f199cc1a78d50979f6387a2e1b5262
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2018
---
# <a name="create-a-virtual-machine-scale-set-that-uses-availability-zones"></a>Een virtuele-machineschaalset die gebruikmaakt van beschikbaarheid Zones maken

Om te voorkomen dat uw virtuele-machineschaalsets fouten datacenter-niveau, kunt u een schaal instellen voor beschikbaarheid Zones maken. Azure-regio's die ondersteuning bieden voor beschikbaarheid Zones hebben een minimum van drie afzonderlijke zones, elk met hun eigen onafhankelijke energie-bron-, netwerk- en koeling. Zie voor meer informatie [overzicht van de Zones van de beschikbaarheid](../availability-zones/az-overview.md).

## <a name="availability-considerations"></a>Beschikbaarheidsoverwegingen

Wanneer u een schaal instellen in een of meer zones vanaf API-versie implementeert *2017-12-01*, hebt u de optie om te implementeren met 'max spreiden' of 'statische 5 veroorzaakt domein spreiden'. Met max. spreiden verspreid de schaalaanpassingsset uw virtuele machines op zoals veel domeinen mogelijk binnen elke zone fault. Via deze spreiden kan worden meer of minder dan vijf fault-domeinen per zone. Met 'statische 5 veroorzaakt domein spreiden' verspreid de schaalaanpassingsset uw virtuele machines over exact vijf foutdomeinen per zone. Als de schaalaanpassingsset niet vijf verschillende foutdomeinen per zone om te voldoen aan de aanvraag voor geheugentoewijzing vindt, wordt het verzoek mislukken.

**Het is raadzaam implementeren met maximale spreiden voor de meeste belastingen**, zoals deze aanpak geeft u het beste verspreiden in de meeste gevallen. Als u replica's worden verdeeld over verschillende hardware isolatie-eenheden, we raden verspreid over beschikbaarheid Zones en gebruikmaken van max spreiden binnen elke zone.

Met max. spreiden ziet u alleen een domein met fouten in de instantieweergave van de virtuele machine en in de metagegevens van het exemplaar ongeacht hoeveel foutdomeinen de virtuele machines worden verdeeld over. De spreiding in elke zone is impliciet.

Als u wilt gebruiken voor maximale spreiden, *platformFaultDomainCount* naar *1*. Met statische vijf veroorzaakt domein spreiden, stelt *platformFaultDomainCount* naar *5*. In de API-versie *2017-12-01*, *platformFaultDomainCount* wordt standaard ingesteld op *1* voor scale-één-zone en cross-zone wordt ingesteld. Alleen statische vijf veroorzaakt domein spreiden wordt momenteel ondersteund voor schaalsets van regionale.

### <a name="placement-groups"></a>Plaatsingsgroepen

Wanneer u een schaalset implementeert, hebt u ook de optie om te implementeren met één [plaatsing groep](./virtual-machine-scale-sets-placement-groups.md) per Zone van de beschikbaarheid of met meerdere per zone. De keuze is voor regionale schaalsets, een plaatsing van één groep in het gebied geen of meerdere in de regio. Voor de meeste belastingen wordt aangeraden meerdere plaatsing groepen waarmee voor groter schaal. In de API-versie *2017-12-01*, schaal stelt standaard aan meerdere groepen voor plaatsing voor één-zone en cross-zone-schaalsets, maar ze standaard aan de groep één plaatsing van regionale schaal instellen.

> [!NOTE]
> Als u het maximale spreiden gebruikt, moet u meerdere groepen voor plaatsing.

### <a name="zone-balancing"></a>Zone Netwerktaakverdeling

Ten slotte voor schaal wordt geïmplementeerd op meerdere zones hebt u ook de optie van het kiezen van 'best effort zone saldo' of 'strict zone saldo'. Een schaalset wordt beschouwd als voor 'met gelijke taakverdeling' als het aantal virtuele machines in elke zone zich in een van het aantal VM's in alle zones voor de scale-set. Bijvoorbeeld:

- Een schaal met 2 virtuele machines in de zone 1, 3 VM's in de zone wordt beschouwd als 2 en 3 VM's in de zone 3 ingesteld met gelijke taakverdeling.
- Een schaal met 1 VM in de zone 1 is ingesteld, 3 VM's in de zone wordt beschouwd als 2 en 3 VM's in de zone 3 onbalans gebracht.

Het is mogelijk dat virtuele machines in de schaalset gemaakt, maar extensies op deze VMs mislukken om te implementeren. Deze virtuele machines met fouten voor uitbreiding worden nog steeds meegeteld bij het bepalen van als een scale-set is verdeeld. Bijvoorbeeld, een schaal met 3 VM's in de zone 1, 3 VM's in de zone 2 en 3 virtuele machines in de zone 3 ingesteld wordt beschouwd als de taakverdeling zelfs als alle extensies is mislukt in de zone 1 en alle uitbreidingen is voltooid in zones 2 en 3.

De schaal instellen met best-effort zone-saldo pogingen af te schalen en behoud saldo. Echter, als voor een bepaalde reden dit niet mogelijk is (bijvoorbeeld als een zone uitgeschakeld wordt, de schaalaanpassingsset kan niet maakt een nieuwe virtuele machine in de zone), de schaalaanpassingsset kunt tijdelijke onbalans is schalen in of uit. De schaalaanpassingsset op meerdere scale-out pogingen wordt VMs aan zones die meer virtuele machines nodig voor de schaal is ingesteld op in evenwicht zijn toegevoegd. Op deze manier op latere schaal van pogingen verwijdert de schaalaanpassingsset VM's van zones die minder virtuele machines nodig voor de schaal instelt verdeeld. Met 'strict zone saldo' mislukt de schaalaanpassingsset alle pogingen om te schalen in- of als u dit zou leiden tot ongebalanceerde.

Voor het gebruik van best-effort zone saldo ingesteld *zoneBalance* naar *false*. Dit is de standaardinstelling in API-versie *2017-12-01*. Voor het gebruik van de strikte zone saldo ingesteld *zoneBalance* naar *true*.

## <a name="single-zone-and-zone-redundant-scale-sets"></a>Single-zone en zone-redundante schaalsets

Wanneer u een virtuele-machineschaalset implementeert, kunt u een enkele Zone beschikbaarheid in een regio of meerdere zones te gebruiken.

Als u een schaal instellen in een enkele zone, u de controle over welke zone worden uitgevoerd in alle exemplaren van deze virtuele machine en de schaalaanpassingsset wordt beheerd en autoscales alleen in deze zone maakt. Een zone-redundante schaalset kunt u een set met één scale die meerdere zones omvat maken. Als de VM-exemplaren zijn gemaakt, standaard zijn ze gelijkmatig verdeeld zijn over zones. Moet een onderbreking optreedt op een van de zones, een schaalset wordt niet automatisch worden uitgebreid om capaciteit te vergroten. Een aanbevolen procedure kan worden regels voor automatisch schalen op basis van gebruik van CPU of geheugen configureren. De regels voor automatisch schalen, zou de schaal ingesteld om te reageren op een verlies van de VM-instanties die een zone door het uitbreiden van nieuwe exemplaren in de resterende operationele zones toestaan.

Beschikbaarheid om Zones te gebruiken, moet uw schaalset worden gemaakt in een [ondersteunde Azure-regio](../availability-zones/az-overview.md#regions-that-support-availability-zones). U kunt een schaalset die gebruikmaakt van beschikbaarheid Zones met een van de volgende manieren maken:

- [Azure Portal](#use-the-azure-portal)
- [Azure CLI 2.0](#use-the-azure-cli-20)
- [Azure PowerShell](#use-azure-powershell)
- [Azure Resource Manager-sjablonen](#use-azure-resource-manager-templates)

## <a name="use-the-azure-portal"></a>Azure Portal gebruiken

Het proces voor het maken van een schaalset die gebruikmaakt van een Zone beschikbaarheid is hetzelfde als is beschreven in de [artikel aan de slag](quick-create-portal.md). Wanneer u een ondersteunde Azure-regio selecteert, kunt u een schaal instellen in een of meer van de beschikbare zones kunt maken, zoals wordt weergegeven in het volgende voorbeeld:

![Een schaal instellen in een enkele beschikbaarheid Zone maken](media/virtual-machine-scale-sets-use-availability-zones/vmss-az-portal.png)

De schaalaanpassingsset en de ondersteunende resources, zoals de Azure load balancer en het openbare IP-adres worden gemaakt in de enkele zone die u opgeeft.

## <a name="use-the-azure-cli-20"></a>De Azure CLI 2.0 gebruiken

Het proces voor het maken van een schaalset die gebruikmaakt van een Zone beschikbaarheid is hetzelfde als is beschreven in de [artikel aan de slag](quick-create-cli.md). Beschikbaarheid om Zones te gebruiken, moet u uw scale ingesteld in een ondersteunde Azure-regio.

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
    --zones 1 2 3
```

Het duurt enkele minuten maken en configureren van alle de schaal ingesteld voor resources en virtuele machines in de zones die u opgeeft. Voor een compleet voorbeeld van een zone-redundante schaal en netwerkbronnen, raadpleegt u [dit voorbeeldscript CLI](https://github.com/Azure/azure-docs-cli-python-samples/blob/master/virtual-machine-scale-sets/create-zone-redundant-scale-set/create-zone-redundant-scale-set.sh)

## <a name="use-azure-powershell"></a>Azure PowerShell gebruiken

Beschikbaarheid om Zones te gebruiken, moet u uw scale ingesteld in een ondersteunde Azure-regio. Toevoegen de `-Zone` -parameter voor de [nieuw AzureRmVmssConfig](/powershell/module/azurerm.compute/new-azurermvmssconfig) opdracht en opgeven welke zone moet worden gebruikt (zoals zone *1*, *2*, of *3*).

Het volgende voorbeeld wordt een enkele zone-scale set met de naam *myScaleSet* in *VS-Oost 2* zone *1*. De Azure-netwerkresources voor het virtuele netwerk, het openbare IP-adres en de load balancer worden automatisch gemaakt. Geef desgevraagd uw eigen beheerdersreferenties op voor de VM-exemplaren in de schaalset:

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

Geef voor het maken van een zone-redundante schaalset meerdere zones met de `-Zone` parameter. Het volgende voorbeeld wordt een zone-redundante scale set met de naam *myScaleSet* over *VS-Oost 2* zones *1, 2, 3*. De zone-redundante Azure netwerkbronnen voor virtueel netwerk, openbare IP-adres en load balancer worden automatisch gemaakt. Geef desgevraagd uw eigen beheerdersreferenties op voor de VM-exemplaren in de schaalset:

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

Het proces voor het maken van een schaalset die gebruikmaakt van een Zone beschikbaarheid is hetzelfde als is beschreven in het artikel in ophalen gestart voor [Linux](quick-create-template-linux.md) of [Windows](quick-create-template-windows.md). Beschikbaarheid om Zones te gebruiken, moet u uw scale ingesteld in een ondersteunde Azure-regio. Voeg de `zones` eigenschap in op de *Microsoft.Compute/virtualMachineScaleSets* resource typt u in uw sjabloon en opgeven welke zone moet worden gebruikt (zoals zone *1*, *2*, of *3*).

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

Nu dat u een schaal instellen in een Zone beschikbaarheid hebt gemaakt, leert u hoe u [toepassingen implementeren op virtuele machine schalen sets](tutorial-install-apps-cli.md) of [automatisch schalen gebruiken met virtuele-machineschaalsets](tutorial-autoscale-cli.md).
