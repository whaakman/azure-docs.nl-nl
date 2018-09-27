---
title: Afbeelding van automatische besturingssysteemupgrades met virtuele Azure-machine-schaalsets | Microsoft Docs
description: Meer informatie over de installatiekopie van het besturingssysteem op de VM-exemplaren in een schaalset automatisch bijwerken
services: virtual-machine-scale-sets
documentationcenter: ''
author: rajsqr
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2018
ms.author: rajraj
ms.openlocfilehash: 1ca0ec7185707d9b9f9712c2ace8dacb361f7b5b
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47394366"
---
# <a name="azure-virtual-machine-scale-set-automatic-os-image-upgrades"></a>Afbeelding van automatische besturingssysteemupgrades schaalset van virtuele machine van Azure

Automatische upgrade van besturingssysteem-installatiekopie is een functie van schaalsets voor virtuele machine van Azure die automatisch alle virtuele machines wordt bijgewerkt naar de meest recente installatiekopie van het besturingssysteem.

Automatische upgrade van besturingssysteem heeft de volgende kenmerken:

- Wanneer geconfigureerd, wordt de meest recente installatiekopie van het besturingssysteem gepubliceerd door uitgevers van installatiekopieën wordt automatisch toegepast op de schaalset zonder tussenkomst van de gebruiker.
- Upgrades van batches van exemplaren op een manier die doorlopende telkens wanneer die een nieuwe platforminstallatiekopie van het is gepubliceerd door de uitgever.
- Kan worden geïntegreerd met de statustest van de toepassing.
- Werkt voor alle VM-grootten, en voor zowel Windows als Linux-platforminstallatiekopieën.
- U kunt automatische upgrades afmelden op elk gewenst moment (Upgrades voor het besturingssysteem kan worden gestart handmatig ook).
- De Besturingssysteemschijf van een virtuele machine wordt vervangen door nieuwe schijf met het besturingssysteem die zijn gemaakt met de meest recente versie van de installatiekopie. Geconfigureerde extensies en aangepaste scripts worden uitgevoerd, terwijl de vastgelegde gegevens schijven worden bewaard.
- Azure disk encryption (in preview) is momenteel niet ondersteund.  

## <a name="how-does-automatic-os-image-upgrade-work"></a>Hoe werkt automatische OS upgrade functioneren?

Een upgrade werkt door de besturingssysteemschijf van een virtuele machine vervangen door een nieuw exemplaar gemaakt met behulp van de meest recente versie van de installatiekopie. Alle extensies hebt geconfigureerd en aangepaste scripts worden uitgevoerd, terwijl de vastgelegde gegevens schijven worden bewaard. Om te beperken de downtime van toepassingen, plaatsvinden upgrades in batches van computers met niet meer dan 20% van de schaalset op elk gewenst moment bijwerken. U hebt ook de optie voor het integreren van de statustest van een Azure Load Balancer-toepassing. Het is raadzaam de heartbeat van een toepassing integreren en valideren van de upgrade is geslaagd voor elke batch in het upgradeproces. De stappen worden uitgevoerd zijn: 

1. Voordat u begint met het upgradeproces start, wordt de orchestrator ervoor te zorgen dat niet meer dan 20% van de exemplaren niet in orde zijn. 
2. Identificeren van de batch van VM-instanties om bij te werken met een maximum van 20% van de totale exemplaren met batch.
3. De installatiekopie van het besturingssysteem van deze batch met VM-exemplaren een upgrade uitvoert.
4. Als de klant heeft geconfigureerd statuscontroles van toepassing zijn, wacht de upgrade tot vijf minuten voor tests om te worden in orde is, voordat u doorgaat met het upgraden van de volgende batch. 
5. Als er resterend exemplaren om bij te werken, Ga naar de stap 1) voor de volgende batch; anders wordt is de upgrade voltooid.

De schaalset OS upgrade orchestrator controles voor de algemene status van de VM-exemplaar vóór de upgrade van elke batch. Tijdens het upgraden van een batch, mogelijk zijn er andere gelijktijdige gepland of niet-gepland onderhoud plaatsvinden in de Azure-Datacenters die mogelijk van invloed op de beschikbaarheid van uw VM's. Het is dus mogelijk dat er tijdelijk meer dan 20% exemplaren mogelijk niet actief. In dergelijke gevallen schaalset aan het einde van de huidige batch, de upgrade stopt.

## <a name="supported-os-images"></a>Ondersteunde OS-installatiekopieën
Alleen bepaalde installatiekopieën voor OS-platform worden momenteel ondersteund. U aangepaste installatiekopieën die u zelf hebt gemaakt te hebben op dit moment niet gebruiken. 

De volgende SKU's worden momenteel ondersteund (meer zullen worden in de toekomst toegevoegd):
    
| Uitgever               | OS-aanbieding      |  Sku               |
|-------------------------|---------------|--------------------|
| Canonical               | UbuntuServer  | 16.04-LTS          |
| Canonical               | UbuntuServer  | 18.04 LTS *        | 
| Rogue Wave (OpenLogic)  | CentOS        | 7.5 *              | 
| CoreOS                  | CoreOS        | Stabiel             | 
| Microsoft Corporation   | WindowsServer | 2012-R2-Datacenter | 
| Microsoft Corporation   | WindowsServer | 2016-Datacenter    | 
| Microsoft Corporation   | WindowsServer | 2016-Datacenter-Smalldisk |
| Microsoft Corporation   | WindowsServer | 2016 Datacenter met Containers |

* Ondersteuning voor deze installatiekopieën is momenteel beschikbaar en zijn beschikbaar in alle Azure-regio's over enkele ogenblikken. 

## <a name="requirements-for-configuring-automatic-os-image-upgrade"></a>Vereisten voor het configureren van Automatische upgrade van besturingssysteem image

- De *versie* eigenschap van de platforminstallatiekopie moet worden ingesteld op *nieuwste*.
- Gebruik statuscontroles van toepassing voor niet-Service Fabric-schaalsets.
- Zorg ervoor dat de resources dat het model met een schaalset naar verwijst beschikbaar is en up-to-date gehouden. 
  Exa.SAS URI voor bootstrapping nettolading in VM-extensie-eigenschappen, nettolading in storage-account, de verwijzing naar geheimen in het model. 

## <a name="configure-automatic-os-image-upgrade"></a>Afbeelding van automatische besturingssysteemupgrades configureren
Voor het configureren van Automatische upgrade van besturingssysteem-installatiekopie, zorg ervoor dat de *automaticOSUpgradePolicy.enableAutomaticOSUpgrade* eigenschap is ingesteld op *waar* instellen in het modeldefinitie. 

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

Ondersteuning voor het configureren van deze eigenschap via Azure PowerShell en CLI 2.0 worden bekendgemaakt op 10/09.

## <a name="using-application-health-probes"></a>Met behulp van de status van de toepassing tests 

Tijdens de Upgrade van een besturingssysteem, VM-exemplaren in een schaalset worden bijgewerkt een batch op een tijdstip. De upgrade moet alleen doorgaan als de Klanttoepassing in orde op de bijgewerkte VM-exemplaren is. U wordt aangeraden dat de toepassing status-signalen en de scale set OS Upgrade-engine biedt. Standaard tijdens Upgrades voor het besturingssysteem het platform rekening gehouden met de voedingsstatus van de virtuele machine en inrichten van de status om te bepalen of een VM-exemplaar in orde na een upgrade van de extensie. Tijdens het bijwerken van het besturingssysteem van een VM-exemplaar, wordt de schijf met het besturingssysteem op een VM-exemplaar vervangen door een nieuwe schijf op basis van de meest recente versie van de installatiekopie. Nadat de Upgrade van het besturingssysteem is voltooid, wordt de geconfigureerde extensies worden uitgevoerd op deze virtuele machines. Alleen wanneer alle extensies op een virtuele machine is ingericht, wordt de toepassing beschouwd als in orde. 

Een schaalset kan eventueel worden geconfigureerd met de toepassing statuscontroles om te voorzien van nauwkeurige informatie over de doorlopende status van de toepassing het platform. Statuscontroles van toepassing zijn aangepaste Load Balancer-tests die worden gebruikt als een statussignaal. De toepassing die wordt uitgevoerd op een scale set VM-exemplaar kan reageren op externe HTTP of TCP-aanvragen die aangeeft of deze in orde is. Zie voor meer informatie over de aangepaste Load Balancer controleert werking [begrijpen load balancer-testen](../load-balancer/load-balancer-custom-probe-overview.md). De Statustest voor een toepassing is niet vereist voor automatische besturingssysteemupgrades, maar het wordt aangeraden.

Als de schaalset is geconfigureerd voor het gebruik van meerdere plaatsingsgroepen, tests met behulp van een [Standard Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview) hoeft te worden gebruikt.

### <a name="configuring-a-custom-load-balancer-probe-as-application-health-probe-on-a-scale-set"></a>Een aangepaste Load Balancer-test configureren als de Statustest van toepassing op een schaal instellen
Als een best practice door expliciet maken een load balancer-test voor de gezondheid van Virtual Machine scale sets. Voor een bestaande HTTP-test of de TCP-test hetzelfde eindpunt kan worden gebruikt, maar een statustest mogelijk verschillend gedrag van een traditionele load balancer-test. Bijvoorbeeld, kan een traditionele load balancer-test slecht wanneer de belasting van het exemplaar te hoog is, is terwijl die niet altijd geschikt voor het bepalen van de status van het exemplaar tijdens een automatische upgrade van besturingssysteem retourneren. Configureer de test om een hoge testinterval frequentie van minder dan twee minuten.

De load balancer-test kan worden verwezen de *networkProfile* van de schaal ingesteld en kan worden gekoppeld aan een van beide een intern of openbaar internetgerichte load balancer als volgt:

```json
"networkProfile": {
  "healthProbe" : {
    "id": "[concat(variables('lbId'), '/probes/', variables('sshProbeName'))]"
  },
  "networkInterfaceConfigurations":
  ...
```
> [!NOTE]
> Als u automatische Besturingssysteemupgrades voor Service Fabric, wordt de nieuwe installatiekopie van het besturingssysteem Updatedomein per Updatedomein voor hoge beschikbaarheid van de services die worden uitgevoerd in Service Fabric uitgerold. Zie voor meer informatie over de kenmerken van de duurzaamheid van Service Fabric-clusters, [deze documentatie](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster).

### <a name="keep-credentials-up-to-date"></a>Referenties up-to-date houden
Als uw schaalset maakt gebruik van referenties voor toegang tot externe bronnen, bijvoorbeeld als een VM-extensie is geconfigureerd dat gebruikmaakt van een SAS-token voor storage-account, moet u om te controleren of dat de referenties worden up-to-date gehouden. Er zijn geen referenties, met inbegrip van certificaten en tokens verlopen, de upgrade mislukken als de eerste batch VM's blijven in een foutstatus.

De aanbevolen stappen voor het herstellen van virtuele machines en automatisch bijwerken van het besturingssysteem opnieuw inschakelen als er een fout bij de verificatie van de resource zijn:

* Het token (of andere referenties) aan de extensie (s doorgegeven) opnieuw genereren.
* Zorg ervoor dat referenties gebruikt uit binnen de virtuele machine om te communiceren met externe entiteiten bijgewerkt is.
* Extensie (s) in het model met een schaalset bijwerken met nieuwe tokens.
* Implementeer de bijgewerkte schaalset, die alle VM-exemplaren, waaronder mislukte die wordt bijgewerkt. 

## <a name="get-the-history-of-automatic-os-image-upgrades"></a>De geschiedenis van automatische besturingssysteemupgrades afbeelding ophalen 
U kunt de geschiedenis van de meest recente bijwerken van het besturingssysteem uitgevoerd op uw schaalset met Azure PowerShell, Azure CLI 2.0 of de REST API's kunt controleren. U kunt de geschiedenis ophalen voor de laatste vijf OS upgrade probeert binnen de afgelopen twee maanden.

### <a name="azure-powershell"></a>Azure PowerShell
In volgende voorbeeld wordt Azure PowerShell gebruikt om te controleren of de status van de schaalset met de naam *myVMSS* in de resourcegroep met de naam *myResourceGroup*:

```powershell
Get-AzureRmVmss -ResourceGroupName myResourceGroup -VMScaleSetName myVMSS -OSUpgradeHistory
```

### <a name="azure-cli-20"></a>Azure CLI 2.0
Het volgende voorbeeld wordt de Azure CLI (2.0.20 of later) om te controleren of de status van de schaalset met de naam *myVMSS* in de resourcegroep met de naam *myResourceGroup*:

```azurecli
az vmss rolling-upgrade get-latest --resource-group myResourceGroup --name myVMSS
```

### <a name="rest-api"></a>REST-API
Het volgende voorbeeld wordt de REST-API om te controleren of de status van de schaalset met de naam *myVMSS* in de resourcegroep met de naam *myResourceGroup*:

```
GET on `/subscriptions/subscription_id/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/osUpgradeHistory?api-version=2018-10-01`
```
Raadpleeg de documentatie voor deze API hier: https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/getosupgradehistory.

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

## <a name="how-to-get-the-latest-version-of-a-platform-os-image"></a>Hoe kan ik de meest recente versie van een platforminstallatiekopie besturingssysteem? 

U kunt de versies van een installatiekopie ophalen voor automatische besturingssysteemupgrades zijn ondersteunde SKU's met behulp van de onderstaande PowerShell-cmdlets: 

```powershell
Get-AzureRmVmImage -Location "westus" -PublisherName "Canonical" -Offer "UbuntuServer" -Skus "16.04-LTS"
```

## <a name="deploy-with-a-template"></a>Implementeren met een sjabloon

U kunt de volgende sjabloon gebruiken voor het implementeren van een schaalset die gebruikmaakt van automatische upgrades <a href='https://github.com/Azure/vm-scale-sets/blob/master/preview/upgrade/autoupdate.json'>automatische rolling upgrades - Ubuntu 16.04-LTS</a>

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fvm-scale-sets%2Fmaster%2Fpreview%2Fupgrade%2Fautoupdate.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>

## <a name="next-steps"></a>Volgende stappen
Zie voor meer voorbeelden over het gebruik van automatische besturingssysteemupgrades met schaalsets, de [GitHub-opslagplaats voor de preview-functies](https://github.com/Azure/vm-scale-sets/tree/master/preview/upgrade).
