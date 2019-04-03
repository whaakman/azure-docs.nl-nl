---
title: Afbeelding van automatische besturingssysteemupgrades met virtuele Azure-machine-schaalsets | Microsoft Docs
description: Meer informatie over de installatiekopie van het besturingssysteem op de VM-exemplaren in een schaalset automatisch bijwerken
services: virtual-machine-scale-sets
documentationcenter: ''
author: mayanknayar
manager: drewm
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/25/2019
ms.author: manayar
ms.openlocfilehash: 007f2801efed8da4964808056563418dec7f64d5
ms.sourcegitcommit: 04716e13cc2ab69da57d61819da6cd5508f8c422
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/02/2019
ms.locfileid: "58849701"
---
# <a name="azure-virtual-machine-scale-set-automatic-os-image-upgrades"></a>Afbeelding van automatische besturingssysteemupgrades schaalset van virtuele machine van Azure

Inschakelen van automatische installatiekopie van het besturingssysteem upgrades op uw updatebeheer scale set helpt eenvoudig door veilig en automatisch upgraden van de besturingssysteemschijf voor alle exemplaren in de schaalset.

Automatische upgrade van besturingssysteem heeft de volgende kenmerken:

- Wanneer geconfigureerd, wordt de meest recente installatiekopie van het besturingssysteem gepubliceerd door uitgevers van installatiekopieën wordt automatisch toegepast op de schaalset zonder tussenkomst van de gebruiker.
- Upgrades van batches van exemplaren op een manier die doorlopende telkens wanneer die een nieuwe platforminstallatiekopie van het is gepubliceerd door de uitgever.
- Kan worden geïntegreerd met statuscontroles van toepassingen en [toepassingsstatus extensie](virtual-machine-scale-sets-health-extension.md).
- Werkt voor alle VM-grootten, en voor zowel Windows als Linux-platforminstallatiekopieën.
- U kunt automatische upgrades afmelden op elk gewenst moment (Upgrades voor het besturingssysteem kan worden gestart handmatig ook).
- De Besturingssysteemschijf van een virtuele machine wordt vervangen door nieuwe schijf met het besturingssysteem die zijn gemaakt met de meest recente versie van de installatiekopie. Geconfigureerde extensies en aangepaste scripts worden uitgevoerd, terwijl de vastgelegde gegevens schijven worden bewaard.
- [Extensievolgorde](virtual-machine-scale-sets-extension-sequencing.md) wordt ondersteund.

## <a name="how-does-automatic-os-image-upgrade-work"></a>Hoe werkt automatische OS upgrade functioneren?

Een upgrade werkt volgens het vervangen van de besturingssysteemschijf van een virtuele machine met een nieuwe schijf gemaakt met behulp van de meest recente versie van de installatiekopie. Alle geconfigureerde extensies en aangepaste scripts worden uitgevoerd op de schijf met het besturingssysteem, tijdens het persistente gegevens schijven worden bewaard. Om te beperken de downtime van toepassingen, plaatsvinden upgrades in batches met niet meer dan 20% van de schaalset op elk gewenst moment bijwerken. U kunt ook de statustest van een Azure Load Balancer-toepassing integreren of [toepassingsstatus extensie](virtual-machine-scale-sets-health-extension.md). We aanbevolen opnemen van de heartbeat van een toepassing en valideren van de upgrade is geslaagd voor elke batch in het upgradeproces.

Het upgradeproces werkt als volgt:
1. Voordat u begint met het upgradeproces start, wordt de orchestrator ervoor te zorgen dat er niet meer dan 20% van de exemplaren in de volledige schaalset niet in orde (voor een of andere reden zijn).
2. De upgrade orchestrator identificeert de batch van VM-instanties om bij te werken met een maximum van 20% van de totale exemplaren met één batch.
3. De besturingssysteemschijf van de geselecteerde batch van VM-exemplaren wordt vervangen door een nieuwe OS-schijf gemaakt op basis van de meest recente installatiekopie en alle opgegeven extensies en -configuraties in het model met een schaalset worden toegepast op het bijgewerkte exemplaar.
4. Voor schaalsets met statuscontroles van de geconfigureerde toepassing of de status van de toepassing-uitbreiding wacht de upgrade tot vijf minuten voor het exemplaar in orde is, worden voordat u doorgaat met het upgraden van de volgende batch.
5. De upgrade orchestrator wordt ook het percentage exemplaren die niet in orde na een upgrade worden bijgehouden. De upgrade gestopt, als meer dan 20% van de bijgewerkte exemplaren niet in orde tijdens het upgradeproces geworden.
6. Het bovenstaande proces wordt voortgezet totdat alle exemplaren in de schaalset zijn bijgewerkt.

De schaalset OS upgrade orchestrator controles voor de algemene status van scale set vóór de upgrade van elke batch. Tijdens het upgraden van een batch, is het mogelijk dat er een andere gelijktijdige gepland of ongepland onderhoud-activiteiten die invloed kunnen hebben op de status van exemplaren in uw schaalset. In dergelijke gevallen als meer dan 20% van de schaalset instanties worden niet in orde is, wordt de schaal ingesteld upgrade stopt aan het einde van de huidige batch.

## <a name="supported-os-images"></a>Ondersteunde OS-installatiekopieën
Alleen bepaalde installatiekopieën voor OS-platform worden momenteel ondersteund. Aangepaste installatiekopieën worden momenteel niet ondersteund.

De volgende SKU's worden momenteel ondersteund (en meer tijd tot tijd worden toegevoegd):

| Uitgever               | OS-aanbieding      |  Sku               |
|-------------------------|---------------|--------------------|
| Canonical               | UbuntuServer  | 16.04-LTS          |
| Canonical               | UbuntuServer  | 18.04-LTS          |
| Rogue Wave (OpenLogic)  | CentOS        | 7.5                |
| CoreOS                  | CoreOS        | Stabiel             |
| Microsoft Corporation   | WindowsServer | 2012-R2-Datacenter |
| Microsoft Corporation   | WindowsServer | 2016-Datacenter    |
| Microsoft Corporation   | WindowsServer | 2016-Datacenter-Smalldisk |
| Microsoft Corporation   | WindowsServer | 2016 Datacenter met Containers |
| Microsoft Corporation   | WindowsServer | 2019-Datacenter |
| Microsoft Corporation   | WindowsServer | 2019-Datacenter-Smalldisk |
| Microsoft Corporation   | WindowsServer | 2019 Datacenter met Containers |


## <a name="requirements-for-configuring-automatic-os-image-upgrade"></a>Vereisten voor het configureren van Automatische upgrade van besturingssysteem image

- De *versie* eigenschap van de platforminstallatiekopie moet worden ingesteld op *nieuwste*.
- Statuscontroles toepassing gebruiken of [toepassingsstatus extensie](virtual-machine-scale-sets-health-extension.md) ingesteld voor Service Fabric-schaal.
- Zorg ervoor dat externe bronnen die zijn opgegeven in het model met een schaalset beschikbaar en bijgewerkt. Voorbeelden zijn onder andere SAS-URI voor het opstarten van de nettolading in VM-extensie-eigenschappen, nettolading in storage-account, de verwijzing naar de geheimen in het model en meer.

## <a name="configure-automatic-os-image-upgrade"></a>Afbeelding van automatische besturingssysteemupgrades configureren
Voor het configureren van Automatische upgrade van besturingssysteem-installatiekopie, zorg ervoor dat de *automaticOSUpgradePolicy.enableAutomaticOSUpgrade* eigenschap is ingesteld op *waar* instellen in het modeldefinitie.

### <a name="rest-api"></a>REST-API
Het volgende voorbeeld wordt beschreven hoe u automatische besturingssysteemupgrades instellen op een model met een schaalset:

```
PUT or PATCH on `/subscriptions/subscription_id/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet?api-version=2018-10-01`
```

```json
{
  "properties": {
    "upgradePolicy": {
      "automaticOSUpgradePolicy": {
        "enableAutomaticOSUpgrade":  true
      }
    }
  }
}
```

### <a name="azure-powershell"></a>Azure PowerShell
Gebruik de [Update AzVmss](/powershell/module/az.compute/update-azvmss) cmdlet om te controleren op OS upgrade geschiedenis voor uw schaalset. Het volgende voorbeeld wordt automatische upgrades voor de schaalset met de naam *myVMSS* in de resourcegroep met de naam *myResourceGroup*:

```azurepowershell-interactive
Update-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -AutomaticOSUpgrade $true
```

### <a name="azure-cli-20"></a>Azure CLI 2.0
Gebruik [az vmss update](/cli/azure/vmss#az-vmss-update) om te controleren of de geschiedenis van de OS upgrade voor uw schaalset. Azure CLI 2.0.47 gebruiken of hoger. Het volgende voorbeeld wordt automatische upgrades voor de schaalset met de naam *myVMSS* in de resourcegroep met de naam *myResourceGroup*:

```azurecli-interactive
az vmss update --name myVMSS --resource-group myResourceGroup --set UpgradePolicy.AutomaticOSUpgradePolicy.EnableAutomaticOSUpgrade=true
```

## <a name="using-application-health-probes"></a>Met behulp van de status van de toepassing tests

Tijdens de Upgrade van een besturingssysteem, VM-exemplaren in een schaalset worden bijgewerkt een batch op een tijdstip. De upgrade moet alleen doorgaan als de Klanttoepassing in orde op de bijgewerkte VM-exemplaren is. Het is raadzaam om dat de toepassing biedt status-signalen en de scale set OS Upgrade-engine. Standaard tijdens Upgrades voor het besturingssysteem het platform rekening gehouden met de voedingsstatus van de virtuele machine en inrichten van de status om te bepalen of een VM-exemplaar in orde na een upgrade van de extensie. Tijdens het bijwerken van het besturingssysteem van een VM-exemplaar, wordt de schijf met het besturingssysteem op een VM-exemplaar vervangen door een nieuwe schijf op basis van de meest recente versie van de installatiekopie. Nadat de Upgrade van het besturingssysteem is voltooid, wordt de geconfigureerde extensies worden uitgevoerd op deze virtuele machines. De toepassing wordt beschouwd als in orde alleen wanneer alle extensies op het exemplaar is ingericht.

Een schaalset kan eventueel worden geconfigureerd met de toepassing statuscontroles om te voorzien van nauwkeurige informatie over de doorlopende status van de toepassing het platform. Statuscontroles van toepassing zijn aangepaste Load Balancer-tests die worden gebruikt als een statussignaal. De toepassing die wordt uitgevoerd op een scale set VM-exemplaar kan reageren op externe HTTP of TCP-aanvragen die aangeeft of deze in orde is. Zie voor meer informatie over de aangepaste Load Balancer controleert werking [begrijpen load balancer-testen](../load-balancer/load-balancer-custom-probe-overview.md). De Statustest voor een toepassing is niet vereist voor Service Fabric-schaalsets, maar het wordt aanbevolen. Niet-Service Fabric-schaalsets vereisen dat de status van de toepassing van de Load Balancer-tests of [toepassingsstatus extensie](virtual-machine-scale-sets-health-extension.md).

Als de schaalset is geconfigureerd voor het gebruik van meerdere plaatsingsgroepen, tests met behulp van een [Standard Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview) hoeft te worden gebruikt.

### <a name="configuring-a-custom-load-balancer-probe-as-application-health-probe-on-a-scale-set"></a>Een aangepaste Load Balancer-test configureren als de Statustest van toepassing op een schaal instellen
Als een best practice door expliciet maken een load balancer-test voor de gezondheid van Virtual Machine scale sets. Voor een bestaande HTTP-test of de TCP-test hetzelfde eindpunt kan worden gebruikt, maar een statustest heeft mogelijk verschillend gedrag van een traditionele load balancer-test. Bijvoorbeeld, kan een traditionele load balancer-test retourneren niet in orde als de belasting van het exemplaar is te hoog, maar die niet kunnen worden bijgehouden voor het bepalen van de status van het exemplaar tijdens een automatische upgrade van het besturingssysteem. Configureer de test om een hoge testinterval frequentie van minder dan twee minuten.

De load balancer-test kan worden verwezen de *networkProfile* van de schaal ingesteld en kan worden gekoppeld aan een van beide een intern of openbaar internetgerichte load balancer als volgt:

```json
"networkProfile": {
  "healthProbe" : {
    "id": "[concat(variables('lbId'), '/probes/', variables('sshProbeName'))]"
  },
  "networkInterfaceConfigurations":
  ...
}
```

> [!NOTE]
> Als u automatische Besturingssysteemupgrades voor Service Fabric, wordt de nieuwe installatiekopie van het besturingssysteem Updatedomein per Updatedomein voor hoge beschikbaarheid van de services die worden uitgevoerd in Service Fabric uitgerold. Uw cluster moet voor het gebruik van automatische Besturingssysteemupgrades in Service Fabric worden geconfigureerd om te gebruiken de Duurzaamheidslaag Silver- of hoger. Zie voor meer informatie over de kenmerken van de duurzaamheid van Service Fabric-clusters, [deze documentatie](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster).

### <a name="keep-credentials-up-to-date"></a>Referenties up-to-date houden
Als uw schaalset maakt gebruik van referenties voor toegang tot externe bronnen, bijvoorbeeld als een VM-extensie is geconfigureerd dat gebruikmaakt van een SAS-token voor storage-account, zorgt u ervoor dat de referenties worden bijgewerkt. Als referenties, met inbegrip van certificaten en tokens, zijn verlopen, mislukt de upgrade en wordt de eerste batch VM's blijven in een foutstatus.

De aanbevolen stappen voor het herstellen van virtuele machines en automatisch bijwerken van het besturingssysteem opnieuw inschakelen als er een fout bij de verificatie van de resource zijn:

* Het token (of andere referenties) aan de extensie (s doorgegeven) opnieuw genereren.
* Zorg ervoor dat referenties gebruikt uit binnen de virtuele machine om te communiceren met externe entiteiten bijgewerkt is.
* Extensie (s) in het model met een schaalset bijwerken met nieuwe tokens.
* Implementeer de bijgewerkte schaalset, die alle VM-exemplaren, waaronder mislukte die wordt bijgewerkt.

## <a name="using-application-health-extension"></a>Met de status van de toepassing-extensie
De extensie van de status van de toepassing wordt geïmplementeerd in een instantie van virtuele-machineschaalset en rapporten op de virtuele machine wordt de status van binnen het exemplaar in de schaalset. U kunt configureren dat de extensie test op een toepassingseindpunt en de status van de toepassing op dat exemplaar. De status van dit exemplaar wordt gecontroleerd door Azure om te bepalen of een exemplaar in aanmerking voor upgrade operations.

Als de extensie rapporten wordt de status van binnen een virtuele machine, kan de extensie worden gebruikt in situaties waarbij externe tests zoals statuscontroles van toepassing (die gebruikmaken van aangepaste Azure Load Balancer [tests](../load-balancer/load-balancer-custom-probe-overview.md)) kan niet worden gebruikt.

Er zijn meerdere manieren van de implementatie van de status van de toepassing uitbreiding van uw schaal ingesteld zoals beschreven in de voorbeelden in [in dit artikel](virtual-machine-scale-sets-health-extension.md#deploy-the-application-health-extension).

## <a name="get-the-history-of-automatic-os-image-upgrades"></a>De geschiedenis van automatische besturingssysteemupgrades afbeelding ophalen
U kunt de geschiedenis van de meest recente bijwerken van het besturingssysteem uitgevoerd op uw schaalset met Azure PowerShell, Azure CLI 2.0 of de REST API's kunt controleren. U kunt de geschiedenis ophalen voor de laatste vijf OS upgrade probeert binnen de afgelopen twee maanden.

### <a name="rest-api"></a>REST-API
Het volgende voorbeeld wordt [REST-API](/rest/api/compute/virtualmachinescalesets/getosupgradehistory) om te controleren of de status van de schaalset met de naam *myVMSS* in de resourcegroep met de naam *myResourceGroup*:

```
GET on `/subscriptions/subscription_id/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/osUpgradeHistory?api-version=2018-10-01`
```

De GET-aanroep retourneert eigenschappen die vergelijkbaar is met de volgende voorbeelduitvoer:

```json
{
    "value": [
        {
            "properties": {
        "runningStatus": {
          "code": "RollingForward",
          "startTime": "2018-07-24T17:46:06.1248429+00:00",
          "completedTime": "2018-04-21T12:29:25.0511245+00:00"
        },
        "progress": {
          "successfulInstanceCount": 16,
          "failedInstanceCount": 0,
          "inProgressInstanceCount": 4,
          "pendingInstanceCount": 0
        },
        "startedBy": "Platform",
        "targetImageReference": {
          "publisher": "MicrosoftWindowsServer",
          "offer": "WindowsServer",
          "sku": "2016-Datacenter",
          "version": "2016.127.20180613"
        },
        "rollbackInfo": {
          "successfullyRolledbackInstanceCount": 0,
          "failedRolledbackInstanceCount": 0
        }
      },
      "type": "Microsoft.Compute/virtualMachineScaleSets/rollingUpgrades",
      "location": "westeurope"
    }
  ]
}
```

### <a name="azure-powershell"></a>Azure PowerShell
Gebruik de [Get-AzVmss](/powershell/module/az.compute/get-azvmss) cmdlet om te controleren op OS upgrade geschiedenis voor uw schaalset. Het volgende voorbeeld wordt uitgelegd hoe u de bekijken OS upgrade voor een schaalset met de naam *myVMSS* in de resourcegroep met de naam *myResourceGroup*:

```azurepowershell-interactive
Get-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myVMSS" -OSUpgradeHistory
```

### <a name="azure-cli-20"></a>Azure CLI 2.0
Gebruik [az vmss get-os-upgrade-geschiedenis](/cli/azure/vmss#az-vmss-get-os-upgrade-history) om te controleren of de geschiedenis van de OS upgrade voor uw schaalset. Azure CLI 2.0.47 gebruiken of hoger. Het volgende voorbeeld wordt uitgelegd hoe u de bekijken OS upgrade voor een schaalset met de naam *myVMSS* in de resourcegroep met de naam *myResourceGroup*:

```azurecli-interactive
az vmss get-os-upgrade-history --resource-group myResourceGroup --name myVMSS
```

## <a name="how-to-get-the-latest-version-of-a-platform-os-image"></a>Hoe kan ik de meest recente versie van een platforminstallatiekopie besturingssysteem?

U kunt de versies van een installatiekopie ophalen voor automatische besturingssysteemupgrades zijn ondersteunde SKU's met behulp van de onderstaande voorbeelden:

### <a name="rest-api"></a>REST-API
```
GET on `/subscriptions/subscription_id/providers/Microsoft.Compute/locations/{location}/publishers/{publisherName}/artifacttypes/vmimage/offers/{offer}/skus/{skus}/versions?api-version=2018-10-01`
```

### <a name="azure-powershell"></a>Azure PowerShell
```azurepowershell-interactive
Get-AzVmImage -Location "westus" -PublisherName "Canonical" -Offer "UbuntuServer" -Skus "16.04-LTS"
```

### <a name="azure-cli-20"></a>Azure CLI 2.0
```azurecli-interactive
az vm image list --location "westus" --publisher "Canonical" --offer "UbuntuServer" --sku "16.04-LTS" --all
```

## <a name="deploy-with-a-template"></a>Implementeren met een sjabloon

U kunt sjablonen gebruiken voor het implementeren van een schaalset met automatische besturingssysteemupgrades voor installatiekopieën van ondersteunde zoals [Ubuntu 16.04-LTS](https://github.com/Azure/vm-scale-sets/blob/master/preview/upgrade/autoupdate.json).

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fvm-scale-sets%2Fmaster%2Fpreview%2Fupgrade%2Fautoupdate.json" target="_blank"><img src="https://azuredeploy.net/deploybutton.png"/></a>

## <a name="next-steps"></a>Volgende stappen
Raadpleeg voor meer voorbeelden over het gebruik van automatische besturingssysteemupgrades met schaalsets, de [GitHub-opslagplaats](https://github.com/Azure/vm-scale-sets/tree/master/preview/upgrade).
