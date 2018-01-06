---
title: Automatische upgrades voor het besturingssysteem met de virtuele machine van Azure schalen sets | Microsoft Docs
description: Meer informatie over het automatisch bijwerken van het besturingssysteem op de VM-exemplaren in een schaalset
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2017
ms.author: negat
ms.openlocfilehash: 145f4ec92b142a1585ba17bf6e49c7824cc32529
ms.sourcegitcommit: 0e1c4b925c778de4924c4985504a1791b8330c71
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/06/2018
---
# <a name="azure-virtual-machine-scale-set-automatic-os-upgrades"></a>Virtuele machine van Azure-schaalset automatische upgrades voor het besturingssysteem

Automatische bijwerken van de installatiekopie van het besturingssysteem is een preview-functie voor virtuele Azure-machine-schaalsets, die alle virtuele machines automatisch wordt bijgewerkt naar de meest recente OS-installatiekopie.

Automatisch bijwerken van het besturingssysteem heeft de volgende kenmerken:

- Zodra geconfigureerd, wordt automatisch de meest recente OS-installatiekopie gepubliceerd door uitgevers van de installatiekopie van de schaal worden ingesteld zonder dat tussenkomst van de gebruiker toegepast.
- Upgrades van batches van exemplaren op een manier rolling telkens wanneer die een nieuwe platforminstallatiekopie is gepubliceerd door de uitgever.
- Kan worden geïntegreerd met de statuscontrole van de toepassing (optioneel, maar sterk aanbevolen Safety).
- Werkt voor alle VM-groottes.
- Installatiekopieën van het platform werkt voor Windows en Linux.
- U kunt automatische upgrades afmelden op elk gewenst moment (Upgrades voor het besturingssysteem kan worden gestart handmatig ook).
- De schijf met het besturingssysteem van een virtuele machine wordt vervangen door de nieuwe schijf met het besturingssysteem gemaakt met de meest recente versie van de installatiekopie. Geconfigureerde uitbreidingen en aangepaste scripts worden uitgevoerd tijdens de persistente gegevens schijven worden bewaard.


## <a name="preview-notes"></a>Opmerkingen bij de Preview 
In de preview van toepassen de volgende beperkingen en -beperkingen:

- Automatische OS alleen ondersteuning voor upgrades [vier OS-SKU's](#supported-os-images). Er is geen SLA of garanties. U wordt aangeraden dat u gebruik geen automatische upgrades voor productie kritieke werkbelastingen tijdens de preview.
- Ondersteuning voor scale sets in een Service Fabric-clusters is binnenkort beschikbaar.
- De schijf van Azure-versleuteling (momenteel in preview) is **niet** momenteel ondersteund bij virtuele machine scale set automatisch bijwerken van het besturingssysteem.
- Een ervaring portal is binnenkort beschikbaar.


## <a name="register-to-use-automatic-os-upgrade"></a>Registreren voor het gebruik van automatische OS Upgrade
Voor het gebruik van de functie voor het bijwerken van OS geautomatiseerde Registreer de preview-provider met [registreren AzureRmProviderFeature](/powershell/module/azurerm.resources/register-azurermproviderfeature) als volgt:

```powershell
Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Compute -FeatureName AutoOSUpgradePreview
```

Duurt ongeveer 10 minuten zodat registratiestatus rapport als *geregistreerde*. U kunt de huidige registratiestatus met controleren [Get-AzureRmProviderFeature](/powershell/module/AzureRM.Resources/Get-AzureRmProviderFeature). Na de registratie, zorg ervoor dat de *Microsoft.Compute* provider is geregistreerd bij [registreren AzureRmResourceProvider](/powershell/module/AzureRM.Resources/Register-AzureRmResourceProvider) als volgt:

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute
```

Het wordt aangeraden om uw toepassingen statuscontroles gebruiken. Gebruik voor het registreren van de provider-functie voor statuscontroles [registreren AzureRmProviderFeature](/powershell/module/azurerm.resources/register-azurermproviderfeature) als volgt:

```powershell
Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Network -FeatureName AllowVmssHealthProbe
```

Opnieuw, duurt het ongeveer 10 minuten zodat registratiestatus om te rapporteren als *geregistreerde*. U kunt de huidige registratiestatus met controleren [Get-AzureRmProviderFeature](/powershell/module/AzureRM.Resources/Get-AzureRmProviderFeature). Eenmaal geregistreerd ervoor te zorgen dat de *Microsoft.Network* provider is geregistreerd bij [registreren AzureRmResourceProvider](/powershell/module/AzureRM.Resources/Register-AzureRmResourceProvider) als volgt:

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
```


## <a name="supported-os-images"></a>Ondersteunde installatiekopieën van het besturingssysteem
Alleen bepaalde platform installatiekopieën van het besturingssysteem worden momenteel ondersteund. U aangepaste installatiekopieën die u zelf hebt gemaakt te hebben op dit moment niet gebruiken. De *versie* eigenschap van de platform-afbeelding moet worden ingesteld op *nieuwste*.

De volgende SKU's die momenteel worden ondersteund (meer worden toegevoegd):
    
| Uitgever               | Aanbieding         |  Sku               | Versie  |
|-------------------------|---------------|--------------------|----------|
| Canonical               | UbuntuServer  | 16.04 TNS          | meest recente   |
| MicrosoftWindowsServer  | WindowsServer | 2012-R2-Datacenter | meest recente   |
| MicrosoftWindowsServer  | WindowsServer | 2016 Datacenter    | meest recente   |
| MicrosoftWindowsServer  | WindowsServer | Datacenter-2016-Smalldisk | meest recente   |



## <a name="application-health"></a>Toepassingsstatus
Tijdens een OS Upgrade VM-exemplaren in een schaalset zijn bijgewerkt één batch tegelijk. De upgrade moet alleen doorgaan als de Klanttoepassing op de bijgewerkte VM-exemplaren in orde is. We raden aan dat de toepassing health signalen met de scale set OS Upgrade-engine biedt. Standaard tijdens Upgrades voor het besturingssysteem acht het platform VM energiestatus en extensie Inrichtingsstatus om te bepalen of een VM-exemplaar in orde na een upgrade. Tijdens het bijwerken van het besturingssysteem van een VM-instantie, is schijf met het besturingssysteem op een VM-exemplaar vervangen door een nieuwe schijf op basis van de meest recente versie van de installatiekopie. Nadat het bijwerken van het besturingssysteem is voltooid, wordt de geconfigureerde extensies worden uitgevoerd op deze virtuele machines. Alleen wanneer de extensies op een virtuele machine met succes zijn ingericht, wordt de toepassing beschouwd als in orde. 

Een schaalset kan eventueel worden geconfigureerd met de toepassing statuscontroles om te voorzien van het platform nauwkeurige informatie over de actieve status van de toepassing. Statuscontroles van toepassing zijn aangepast Load Balancer-tests die worden gebruikt als een signaal health. De toepassing die wordt uitgevoerd op een scale set VM-instantie kan reageren op de externe HTTP of TCP-aanvragen die aangeeft of het goed is. Zie voor meer informatie over de werking aangepaste voor Load Balancer-tests [Understand load balancer tests](../load-balancer/load-balancer-custom-probe-overview.md). Een toepassing Health test is niet vereist voor automatische upgrades voor het besturingssysteem, maar er wordt sterk aanbevolen.

Als de schaalaanpassingsset is geconfigureerd voor het gebruik van meerdere groepen voor plaatsing, tests met behulp van een [standaard Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview) moet worden gebruikt.

### <a name="important-keep-credentials-up-to-date"></a>Belangrijk: Referenties up-to-date te houden
Als uw scale set referenties gebruikt voor toegang tot externe bronnen, bijvoorbeeld als een VM-extensie is geconfigureerd dat gebruikmaakt van een SAS-token voor storage-account, moet u om ervoor te zorgen dat de referenties actueel worden gehouden. Als er geen referenties, met inbegrip van certificaten en tokens verlopen zijn, mislukt de upgrade en wordt de eerste batch van virtuele machines in een foutstatus achtergelaten.

De aanbevolen stappen voor het herstellen van virtuele machines en schakelt u automatisch bijwerken van het besturingssysteem als er een verificatiefout resource zijn:

* Het token (of andere referenties) aan uw extensie (s doorgegeven) opnieuw genereren.
* Zorg ervoor dat geen referenties gebruikt van in de virtuele machine om te communiceren met externe entiteiten up-to-date te houden.
* Extensie (s) in het model van de set schaal bijwerken met een nieuwe tokens.
* Implementeer de bijgewerkte scale ingesteld die alle VM-instanties, waaronder mislukte die wordt bijgewerkt. 

### <a name="configuring-a-custom-load-balancer-probe-as-application-health-probe-on-a-scale-set"></a>Stel een aangepaste Load Balancer-test als de toepassing Health test op schaal configureren
Als een best practice, maakt u een load balancer-test expliciet voor health-schaalset. Hetzelfde eindpunt voor een bestaande HTTP-test of de TCP-controle kan worden gebruikt, maar een health test mogelijk verschillend gedrag van een traditionele load balancer-test. Zo kan een traditionele load balancer-test slecht als de belasting van het exemplaar te hoog is, is terwijl die niet altijd geschikt voor het bepalen van de status van het exemplaar bij een automatische OS upgrade retourneren. Configureer de test met een hoge Zoek frequentie van minder dan twee minuten.

De load balancer-test kan worden verwezen de *Schaalaanpassingsset* van de schaal ingesteld en kan worden gekoppeld aan beide een intern of openbaar gerichte load balancer als volgt:

```json
"networkProfile": {
  "healthProbe" : {
    "id": "[concat(variables('lbId'), '/probes/', variables('sshProbeName'))]"
  },
  "networkInterfaceConfigurations":
  ...
```


## <a name="enforce-an-os-image-upgrade-policy-across-your-subscription"></a>Afdwingen van beleid voor een upgrade van een OS-installatiekopie in uw abonnement
Voor veilige upgrades, is het raadzaam om af te dwingen van het beleid voor een upgrade. Dit beleid kunt vereisen statuscontroles toepassing in uw abonnement. De volgende Azure Resource Manager-beleid wordt geweigerd implementaties waarvoor geen geautomatiseerde installatiekopie van het besturingssysteem upgraden van de instellingen die zijn geconfigureerd:

1. Verkrijgen van de ingebouwde Azure Resource Manager-beleidsdefinitie met [Get-AzureRmPolicyDefinition](/powershell/module/AzureRM.Resources/Get-AzureRmPolicyDefinition) als volgt:

    ```powershell
    $policyDefinition = Get-AzureRmPolicyDefinition -Id "/providers/Microsoft.Authorization/policyDefinitions/465f0161-0087-490a-9ad9-ad6217f4f43a"
    ```

2. Beleid toewijzen aan een abonnement met [nieuw AzureRmPolicyAssignment](/powershell/module/AzureRM.Resources/New-AzureRmPolicyAssignment) als volgt:

    ```powershell
    New-AzureRmPolicyAssignment `
        -Name "Enforce automatic OS upgrades with app health checks" `
        -Scope "/subscriptions/<SubscriptionId>" `
        -PolicyDefinition $policyDefinition
    ```


## <a name="configure-auto-updates"></a>Automatische updates configureren
Zorg ervoor dat voor het configureren van automatische upgrades, het *automaticOSUpgrade* eigenschap is ingesteld op *true* ingesteld in de schaal modeldefinitie. U kunt deze eigenschap configureren met Azure PowerShell of de Azure CLI 2.0.

Het volgende voorbeeld wordt een Azure PowerShell (4.4.1 of hoger) voor het configureren van automatische upgrades voor de set met de naam scale *myVMSS* in de resourcegroep met de naam *myResourceGroup*:

```powershell
$rgname = myResourceGroup
$vmssname = myVMSS
$vmss = Get-AzureRmVMss -ResourceGroupName $rgname -VmScaleSetName $vmssname
$vmss.UpgradePolicy.AutomaticOSUpgrade = $true
Update-AzureRmVmss -ResourceGroupName $rgname -VMScaleSetName $vmssname -VirtualMachineScaleSet $vmss
```


Het volgende voorbeeld wordt de Azure CLI (2.0.20 of hoger) voor het configureren van automatische upgrades voor de set met de naam scale *myVMSS* in de resourcegroep met de naam *myResourceGroup*:

```azurecli
rgname="myResourceGroup"
vmssname="myVMSS"
az vmss update --name $vmssname --resource-group $rgname --set upgradePolicy.AutomaticOSUpgrade=true
```


## <a name="check-the-status-of-an-automatic-os-upgrade"></a>Controleer de status van een automatisch bijwerken van het besturingssysteem
U kunt de status van de meest recente OS upgrade uitgevoerd op uw scale ingesteld met Azure PowerShell, Azure CLI 2.0 of de REST API's controleren.

### <a name="azure-powershell"></a>Azure PowerShell
In volgende voorbeeld maakt gebruik van Azure PowerShell (4.4.1 of hoger) en controleer de status voor de set met de naam scale *myVMSS* in de resourcegroep met de naam *myResourceGroup*:

```powershell
Get-AzureRmVmssRollingUpgrade -ResourceGroupName myResourceGroup -VMScaleSetName myVMSS
```

### <a name="azure-cli-20"></a>Azure CLI 2.0
Het volgende voorbeeld wordt de Azure CLI (2.0.20 of hoger) en controleer de status voor de set met de naam scale *myVMSS* in de resourcegroep met de naam *myResourceGroup*:

```azurecli
az vmss rolling-upgrade get-latest --resource-group myResourceGroup --name myVMSS
```

### <a name="rest-api"></a>REST-API
Het volgende voorbeeld wordt de REST-API en controleer de status voor de set met de naam scale *myVMSS* in de resourcegroep met de naam *myResourceGroup*:

```
GET on `/subscriptions/subscription_id/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/rollingUpgrades/latest?api-version=2017-03-30`
```

De GET-aanroep retourneert eigenschappen lijken op het volgende voorbeeld:

```json
{
  "properties": {
    "policy": {
      "maxBatchInstancePercent": 20,
      "maxUnhealthyInstancePercent": 5,
      "maxUnhealthyUpgradedInstancePercent": 5,
      "pauseTimeBetweenBatches": "PT0S"
    },
    "runningStatus": {
      "code": "Completed",
      "startTime": "2017-06-16T03:40:14.0924763+00:00",
      "lastAction": "Start",
      "lastActionTime": "2017-06-22T08:45:43.1838042+00:00"
    },
    "progress": {
      "successfulInstanceCount": 3,
      "failedInstanceCount": 0,
      "inprogressInstanceCount": 0,
      "pendingInstanceCount": 0
    }
  },
  "type": "Microsoft.Compute/virtualMachineScaleSets/rollingUpgrades",
  "location": "southcentralus"
}
```


## <a name="automatic-os-upgrade-execution"></a>Automatisch bijwerken van het besturingssysteem worden uitgevoerd
Als u over het gebruik van statustesten van de toepassing wilt uitbreiden, uitvoeren scale set OS upgrades stappen te volgen:

1. Als er meer dan 20% van de exemplaren zijn niet in orde, stopt u de upgrade; Anders gaat u verder.
2. Identificeer de volgende batch van VM-exemplaren te werken met een batch met maximaal 20% van totaal aantal exemplaren.
3. Werk het besturingssysteem van de volgende batch van VM-exemplaren.
4. Als er meer dan 20% van de bijgewerkte exemplaren zijn niet in orde, stopt u de upgrade; Anders gaat u verder.
5. Als de klant heeft geconfigureerd statuscontroles van toepassing, de upgrade wordt gewacht tot vijf minuten voor tests aan de orde, wordt onmiddellijk gaat door op de volgende batch; anders wordt er 30 minuten zijn verstreken voordat u doorgaat naar de volgende batch.
6. Als er nog exemplaren om bij te werken, Ga naar de stap 1) voor de volgende batch; anders is de upgrade is voltooid.

De OS Upgrade-Engine-controles voor de algehele status van de VM-exemplaar vóór de upgrade van elke batch-schaalset. Tijdens het upgraden van een batch, mogelijk zijn er andere gelijktijdige gepland of ongepland onderhoud gebeurt er in de Azure-Datacenters die mogelijk van invloed op beschikbaarheid van uw virtuele machines. Het is daarom mogelijk tijdelijk meer dan 20% van instanties mogelijk uitgeschakeld. In dergelijke gevallen aan het einde van de huidige batch de schaal ingesteld upgrade stopt.


## <a name="deploy-with-a-template"></a>Met een sjabloon implementeren

U kunt de volgende sjabloon gebruiken voor het implementeren van een schaalset die gebruikmaakt van automatische upgrades <a href='https://github.com/Azure/vm-scale-sets/blob/master/preview/upgrade/autoupdate.json'>automatische rolling upgrades - Ubuntu 16.04-TNS</a>

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fvm-scale-sets%2Fmaster%2Fpreview%2Fupgrade%2Fautoupdate.json" target="_blank"> <img src="http://azuredeploy.net/deploybutton.png"/>
</a>


## <a name="next-steps"></a>Volgende stappen
Zie voor meer voorbeelden van het gebruik van automatische upgrades voor het besturingssysteem met schaalsets de [GitHub-opslagplaats voor preview-functies](https://github.com/Azure/vm-scale-sets/tree/master/preview/upgrade).
