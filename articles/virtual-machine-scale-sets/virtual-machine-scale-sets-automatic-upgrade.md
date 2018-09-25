---
title: Automatische besturingssysteemupgrades met virtuele Azure-machine-schaalsets | Microsoft Docs
description: Meer informatie over het besturingssysteem op de VM-exemplaren in een schaalset automatisch bijwerken
services: virtual-machine-scale-sets
documentationcenter: ''
author: yeki
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/03/2018
ms.author: yeki
ms.openlocfilehash: 935b3ff0fe03984b02dc2e1137f48e53b06ce0c2
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46995105"
---
# <a name="azure-virtual-machine-scale-set-automatic-os-upgrades"></a>Schaalset voor virtuele Azure-machine automatische besturingssysteemupgrades

Automatische upgrade van besturingssysteem-installatiekopie is een preview-functie voor schaalsets voor virtuele machine van Azure die automatisch alle virtuele machines wordt bijgewerkt naar de meest recente installatiekopie van het besturingssysteem.

Automatische upgrade van besturingssysteem heeft de volgende kenmerken:

- Wanneer geconfigureerd, wordt de meest recente installatiekopie van het besturingssysteem gepubliceerd door uitgevers van installatiekopieën wordt automatisch toegepast op de schaalset zonder tussenkomst van de gebruiker.
- Upgrades van batches van exemplaren op een manier die doorlopende telkens wanneer die een nieuwe platforminstallatiekopie van het is gepubliceerd door de uitgever.
- Kan worden geïntegreerd met de statustest van de toepassing (optioneel, maar sterk aanbevolen voor de veiligheid).
- Werkt voor alle VM-grootten.
- Werkt voor Windows en Linux-platforminstallatiekopieën.
- U kunt automatische upgrades afmelden op elk gewenst moment (Upgrades voor het besturingssysteem kan worden gestart handmatig evenals).
- De Besturingssysteemschijf van een virtuele machine wordt vervangen door nieuwe schijf met het besturingssysteem die zijn gemaakt met de meest recente versie van de installatiekopie. Geconfigureerde extensies en aangepaste scripts worden uitgevoerd, terwijl de vastgelegde gegevens schijven worden bewaard.


## <a name="preview-notes"></a>Opmerkingen bij de Preview-versie 
Preview-versie gelden de volgende voorwaarden en beperkingen:

- Automatische besturingssysteem alleen ondersteuning voor upgrades [vier OS-SKU's](#supported-os-images). Er is geen SLA of garanties. U wordt aangeraden dat u geen automatische upgrades op kritieke werkbelastingen voor productie gebruiken tijdens de preview.
- Azure disk encryption is **niet** momenteel niet ondersteund met virtual machine scale set automatisch bijwerken van het besturingssysteem.


## <a name="register-to-use-automatic-os-upgrade"></a>Registreren voor het gebruik van automatische Upgrade van besturingssysteem
Voor het gebruik van de geautomatiseerde functie voor het bijwerken van besturingssysteem, de preview-provider te registreren met Azure Powershell of Azure CLI 2.0.

### <a name="powershell"></a>PowerShell

1. Registreren bij [Register-AzureRmProviderFeature](/powershell/module/azurerm.resources/register-azurermproviderfeature):

     ```powershell
     Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Compute -FeatureName AutoOSUpgradePreview
     ```

2. Het duurt ongeveer 10 minuten voor registratiestatus rapport als *geregistreerde*. U kunt de huidige status van inschrijving met controleren [Get-AzureRmProviderFeature](/powershell/module/AzureRM.Resources/Get-AzureRmProviderFeature). 

3. Zodra geregistreerd, bevestig dat de *Microsoft.Compute* provider is geregistreerd. Het volgende voorbeeld maakt gebruik van Azure Powershell gebruiken met [Register-AzureRmResourceProvider](/powershell/module/AzureRM.Resources/Register-AzureRmResourceProvider):

     ```powershell
     Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute
     ```


### <a name="cli-20"></a>CLI 2.0

1. Registreren bij [az functie registreren](/cli/azure/feature#az-feature-register):

     ```azurecli
     az feature register --name AutoOSUpgradePreview --namespace Microsoft.Compute
     ```

2. Het duurt ongeveer 10 minuten voor registratiestatus rapport als *geregistreerde*. U kunt de huidige status van inschrijving met controleren [az functie show](/cli/azure/feature#az-feature-show). 
 
3. Zodra geregistreerd, zorg ervoor dat de *Microsoft.Compute* provider is geregistreerd. Het volgende voorbeeld wordt de Azure CLI (2.0.20 of hoger) met [az provider register](/cli/azure/provider#az-provider-register):

     ```azurecli
     az provider register --namespace Microsoft.Compute
     ```

> [!NOTE]
> Service Fabric-clusters hebben hun eigen begrip van de toepassingsstatus, maar schaalsets zonder Service Fabric de load balancer-statustest gebruiken voor het bewaken van de status van de toepassing. 
>
> ### <a name="azure-powershell"></a>Azure Powershell
>
> 1. De functie voor de provider registreren voor statuscontroles met [Register-AzureRmProviderFeature](/powershell/module/azurerm.resources/register-azurermproviderfeature):
>
>      ```powershell
>      Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Network -FeatureName AllowVmssHealthProbe
>      ```
>
> 2. Opnieuw, duurt het ongeveer 10 minuten voor registratiestatus rapport als *geregistreerde*. U kunt de huidige status van inschrijving met controleren [Get-AzureRmProviderFeature](/powershell/module/AzureRM.Resources/Get-AzureRmProviderFeature)
>
> 3. Eenmaal geregistreerd ervoor te zorgen dat de *Microsoft.Network* provider is geregistreerd met behulp van [Register-AzureRmResourceProvider](/powershell/module/AzureRM.Resources/Register-AzureRmResourceProvider):
>
>      ```powershell
>      Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
>      ```
>
>
> ### <a name="cli-20"></a>CLI 2.0
>
> 1. De functie voor de provider registreren voor statuscontroles met [az functie registreren](/cli/azure/feature#az-feature-register):
>
>      ```azurecli
>      az feature register --name AllowVmssHealthProbe --namespace Microsoft.Network
>      ```
>
> 2. Opnieuw, duurt het ongeveer 10 minuten voor registratiestatus rapport als *geregistreerde*. U kunt de huidige status van inschrijving met controleren [az functie show](/cli/azure/feature#az-feature-show). 
>
> 3. Eenmaal geregistreerd ervoor te zorgen dat de *Microsoft.Network* provider is geregistreerd met behulp van [az provider register](/cli/azure/provider#az-provider-register) als volgt:
>
>      ```azurecli
>      az provider register --namespace Microsoft.Network
>      ```

## <a name="portal-experience"></a>Portalervaring
Nadat u de bovenstaande registratiestappen volgen, gaat u naar [de Azure-portal](https://aka.ms/managed-compute) om in te schakelen van automatische upgrades voor het besturingssysteem op uw schaalsets en de voortgang van upgrades wilt bekijken:

![](./media/virtual-machine-scale-sets-automatic-upgrade/automatic-upgrade-portal.png)


## <a name="supported-os-images"></a>Ondersteunde OS-installatiekopieën
Alleen bepaalde installatiekopieën voor OS-platform worden momenteel ondersteund. U aangepaste installatiekopieën die u zelf hebt gemaakt op dit moment niet gebruiken. De *versie* eigenschap van de platforminstallatiekopie moet worden ingesteld op *nieuwste*.

De volgende SKU's worden momenteel ondersteund (meer worden toegevoegd):
    
| Uitgever               | Aanbieding         |  Sku               | Versie  |
|-------------------------|---------------|--------------------|----------|
| Canonical               | UbuntuServer  | 16.04-LTS          | meest recente   |
| MicrosoftWindowsServer  | WindowsServer | 2012-R2-Datacenter | meest recente   |
| MicrosoftWindowsServer  | WindowsServer | 2016-Datacenter    | meest recente   |
| MicrosoftWindowsServer  | WindowsServer | 2016-Datacenter-Smalldisk | meest recente   |
| MicrosoftWindowsServer  | WindowsServer | 2016 Datacenter met Containers | meest recente   |



## <a name="application-health-without-service-fabric"></a>Status van de toepassing zonder Service Fabric
> [!NOTE]
> Deze sectie geldt alleen voor schaalsets zonder Service Fabric. Service Fabric heeft een eigen begrip van de toepassingsstatus. Als u automatische Besturingssysteemupgrades voor Service Fabric, wordt de nieuwe installatiekopie van het besturingssysteem Updatedomein per Updatedomein voor hoge beschikbaarheid van de services die worden uitgevoerd in Service Fabric uitgerold. Zie voor meer informatie over de kenmerken van de duurzaamheid van Service Fabric-clusters, [deze documentatie](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster).

Tijdens de Upgrade van een besturingssysteem, VM-exemplaren in een schaalset worden bijgewerkt een batch op een tijdstip. De upgrade moet alleen doorgaan als de Klanttoepassing in orde op de bijgewerkte VM-exemplaren is. Om deze reden moet de toepassing de health-signalen en de scale set OS Upgrade-engine opgeven. Het platform tijdens Upgrades voor het besturingssysteem, rekening gehouden met de voedingsstatus van de virtuele machine en inrichten van de status om te bepalen of een VM-exemplaar in orde na een upgrade van de extensie. Tijdens het bijwerken van het besturingssysteem van een VM-exemplaar, wordt de schijf met het besturingssysteem op een VM-exemplaar vervangen door een nieuwe schijf op basis van de meest recente versie van de installatiekopie. Nadat de Upgrade van het besturingssysteem is voltooid, wordt de geconfigureerde extensies worden uitgevoerd op deze virtuele machines. Alleen wanneer alle extensies op een virtuele machine is ingericht, wordt de toepassing beschouwd als in orde. 

Bovendien de schaalset *moet* worden geconfigureerd met de toepassing statuscontroles voor het platform met de juiste gegevens over de doorlopende status van de toepassing. Statuscontroles van toepassing zijn aangepaste Load Balancer-tests die worden gebruikt als een statussignaal. De toepassing die wordt uitgevoerd op een scale set VM-exemplaar kan reageren op externe HTTP of TCP-aanvragen die aangeeft of deze in orde is. Zie voor meer informatie over de aangepaste Load Balancer controleert werking [begrijpen load balancer-testen](../load-balancer/load-balancer-custom-probe-overview.md).

Als de schaalset is geconfigureerd voor het gebruik van meerdere plaatsingsgroepen, tests met behulp van een [Standard Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview) hoeft te worden gebruikt.

### <a name="important-keep-credentials-up-to-date"></a>Belangrijk: Houd referenties bijgewerkt
Als uw schaalset maakt gebruik van referenties voor toegang tot externe resources, moet u om te controleren of dat de referenties worden up-to-date gehouden. Een VM-extensie kan bijvoorbeeld worden geconfigureerd voor het gebruik van een SAS-token voor storage-account. Er zijn geen referenties, met inbegrip van certificaten en tokens verlopen, de upgrade mislukken als de eerste batch VM's blijven in een foutstatus.

De aanbevolen stappen voor het herstellen van virtuele machines en automatisch bijwerken van het besturingssysteem opnieuw inschakelen als er een fout bij de verificatie van de resource zijn:

* Het token (of andere referenties) aan de extensie (s doorgegeven) opnieuw genereren.
* Zorg ervoor dat referenties gebruikt uit binnen de virtuele machine om te communiceren met externe entiteiten bijgewerkt is.
* Extensie (s) in het model met een schaalset bijwerken met nieuwe tokens.
* Implementeer de bijgewerkte schaalset, die alle VM-exemplaren, waaronder mislukte die wordt bijgewerkt. 

### <a name="configuring-a-custom-load-balancer-probe-as-application-health-probe-on-a-scale-set"></a>Een aangepaste Load Balancer-test configureren als de Statustest van toepassing op een schaal instellen
U *moet* expliciet een load balancer-test maken voor de gezondheid van Virtual Machine scale sets. Voor een bestaande HTTP-test of de TCP-test hetzelfde eindpunt kan worden gebruikt, maar een statustest mogelijk verschillend gedrag van een traditionele load balancer-test. Bijvoorbeeld, kan een traditionele load balancer-test slecht wanneer de belasting van het exemplaar te hoog is retourneren. Daarentegen die mogelijk niet geschikt is voor het bepalen van de status van het exemplaar tijdens een automatische upgrade van het besturingssysteem. Configureer de test om een hoge testinterval frequentie van minder dan twee minuten.

De load balancer-test kan worden verwezen de *networkProfile* van de schaal ingesteld en kan worden gekoppeld aan een van beide een intern of openbaar internetgerichte load balancer als volgt:

```json
"networkProfile": {
  "healthProbe" : {
    "id": "[concat(variables('lbId'), '/probes/', variables('sshProbeName'))]"
  },
  "networkInterfaceConfigurations":
  ...
```


## <a name="enforce-an-os-image-upgrade-policy-across-your-subscription"></a>Het Upgradebeleid van een OS-installatiekopie afdwingen op uw abonnement
Voor upgrades van veilige, is het raadzaam om af te dwingen een beleid voor upgrades. Dit beleid kunt vereisen dat de status van de toepassing tests op uw abonnement. De volgende Azure Resource Manager-beleid weigert implementaties waarvoor geen geautomatiseerde installatiekopie van het besturingssysteem bijwerken van instellingen die zijn geconfigureerd:

### <a name="powershell"></a>PowerShell
1. Ophalen van de ingebouwde beleidsdefinitie van Azure Resource Manager met [Get-AzureRmPolicyDefinition](/powershell/module/AzureRM.Resources/Get-AzureRmPolicyDefinition) als volgt:

    ```powershell
    $policyDefinition = Get-AzureRmPolicyDefinition -Id "/providers/Microsoft.Authorization/policyDefinitions/465f0161-0087-490a-9ad9-ad6217f4f43a"
    ```

2. Beleid toewijzen aan een abonnement met [New-AzureRmPolicyAssignment](/powershell/module/AzureRM.Resources/New-AzureRmPolicyAssignment) als volgt:

    ```powershell
    New-AzureRmPolicyAssignment `
        -Name "Enforce automatic OS upgrades with app health checks" `
        -Scope "/subscriptions/<SubscriptionId>" `
        -PolicyDefinition $policyDefinition
    ```

### <a name="cli-20"></a>CLI 2.0
Beleid toewijzen aan een abonnement met ingebouwde Azure Resource Manager-beleid:

```azurecli
az policy assignment create --display-name "Enforce automatic OS upgrades with app health checks" --name "Enforce automatic OS upgrades" --policy 465f0161-0087-490a-9ad9-ad6217f4f43a --scope "/subscriptions/<SubscriptionId>"
```

## <a name="configure-auto-updates"></a>Automatische updates configureren
Voor het configureren van automatische upgrades, zorg ervoor dat de *automaticosupgrade uit* eigenschap is ingesteld op *waar* instellen in het modeldefinitie. U kunt deze eigenschap configureren met Azure PowerShell of Azure CLI.

### <a name="powershell"></a>PowerShell
Het volgende voorbeeld maakt gebruik van Azure PowerShell (4.4.1 of later) om te configureren van automatische upgrades voor de schaalset met de naam *myVMSS* in de resourcegroep met de naam *myResourceGroup*:

```powershell
$rgname = myResourceGroup
$vmssname = myVMSS
$vmss = Get-AzureRmVMss -ResourceGroupName $rgname -VmScaleSetName $vmssname
$vmss.UpgradePolicy.AutomaticOSUpgrade = $true
Update-AzureRmVmss -ResourceGroupName $rgname -VMScaleSetName $vmssname -VirtualMachineScaleSet $vmss
```

### <a name="cli-20"></a>CLI 2.0
Het volgende voorbeeld wordt de Azure CLI (2.0.20 of hoger) het configureren van automatische upgrades voor de schaalset met de naam *myVMSS* in de resourcegroep met de naam *myResourceGroup*:

```azurecli
rgname="myResourceGroup"
vmssname="myVMSS"
az vmss update --name $vmssname --resource-group $rgname --set upgradePolicy.AutomaticOSUpgrade=true
```


## <a name="check-the-status-of-an-automatic-os-upgrade"></a>Controleer de status van een automatische upgrade van besturingssysteem
U kunt de status van de meest recente bijwerken van het besturingssysteem uitgevoerd op uw schaalset met Azure PowerShell, Azure CLI of de REST API's te controleren.

### <a name="powershell"></a>PowerShell
In volgende voorbeeld maakt gebruik van Azure PowerShell (4.4.1 of later) om te controleren of de status van de schaalset met de naam *myVMSS* in de resourcegroep met de naam *myResourceGroup*:

```powershell
Get-AzureRmVmssRollingUpgrade -ResourceGroupName myResourceGroup -VMScaleSetName myVMSS
```

### <a name="azure-cli"></a>Azure-CLI

Het volgende voorbeeld wordt de Azure CLI (2.0.20 of later) om te controleren of de status van de schaalset met de naam *myVMSS* in de resourcegroep met de naam *myResourceGroup*:

```azurecli
az vmss rolling-upgrade get-latest --resource-group myResourceGroup --name myVMSS
```

### <a name="rest-api"></a>REST-API
Het volgende voorbeeld wordt de REST-API om te controleren of de status van de schaalset met de naam *myVMSS* in de resourcegroep met de naam *myResourceGroup*:

```
GET on `/subscriptions/subscription_id/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/rollingUpgrades/latest?api-version=2017-03-30`
```

De GET-aanroep retourneert eigenschappen die vergelijkbaar is met de volgende voorbeelduitvoer:

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


## <a name="automatic-os-upgrade-execution"></a>Automatische Upgrade van besturingssysteem worden uitgevoerd
Als u wilt uitbreiden voor het gebruik van statuscontroles van toepassing, uitvoeren scale set OS upgrades stappen te volgen:

1. Als er meer dan 20% van de exemplaren niet in orde zijn, stopt u de upgrade. Anders gaat u verder.
2. Identificeren van de volgende batch van VM-instanties om bij te werken met een batch met maximaal 20% van totaal aantal exemplaren.
3. Werk het besturingssysteem van de volgende batch van VM-exemplaren.
4. Als er meer dan 20% van de bijgewerkte exemplaren niet in orde zijn, stopt u de upgrade. Anders gaat u verder.
5. Voor schaalsets die geen deel uitmaken van een Service Fabric-cluster, de upgrade moet wachten tot vijf minuten voor tests om te worden in orde is en vervolgens onmiddellijk gaat door op de volgende batch. Voor schaalsets die deel van een Service Fabric-cluster uitmaken, schaalset de wacht 30 minuten voordat u doorgaat met de volgende batch.
6. Als er resterend exemplaren om bij te werken, Ga naar de stap 1) voor de volgende batch; anders wordt is de upgrade voltooid.

De schaalset OS Upgrade-Engine controles voor de algemene status van de VM-exemplaar vóór de upgrade van elke batch. Tijdens het upgraden van een batch, kunnen er andere gelijktijdige gepland of niet-gepland onderhoud plaatsvinden in de Azure-Datacenters die mogelijk van invloed op de beschikbaarheid van uw VM's. Daarom is het mogelijk dat er tijdelijk meer dan 20% exemplaren mogelijk niet beschikbaar. In dergelijke gevallen schaalset aan het einde van de huidige batch, de upgrade stopt.


## <a name="deploy-with-a-template"></a>Implementeren met een sjabloon

U kunt de volgende sjabloon gebruiken voor het implementeren van een schaalset die gebruikmaakt van automatische upgrades <a href='https://github.com/Azure/vm-scale-sets/blob/master/preview/upgrade/autoupdate.json'>automatische rolling upgrades - Ubuntu 16.04-LTS</a>

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fvm-scale-sets%2Fmaster%2Fpreview%2Fupgrade%2Fautoupdate.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>


## <a name="next-steps"></a>Volgende stappen
Zie voor meer voorbeelden over het gebruik van automatische besturingssysteemupgrades met schaalsets, de [GitHub-opslagplaats voor de preview-functies](https://github.com/Azure/vm-scale-sets/tree/master/preview/upgrade).
