---
title: Upgrade van een Azure virtuele-machineschaalset | Microsoft Docs
description: Een Azure virtuele-machineschaalset bijwerken
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: e229664e-ee4e-4f12-9d2e-a4f456989e5d
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: gunegatybo
ms.openlocfilehash: fbdc9d40173a40f35eee60cadfdd258293509d53
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/20/2017
---
# <a name="upgrade-a-virtual-machine-scale-set"></a>Een virtuele-machineschaalset bijwerken
Dit artikel wordt beschreven hoe kunt u een update OS uitrolt naar een virtuele machine van Azure schaal instellen zonder uitvaltijd. In deze context is omvat een update OS het wijzigen van de versie of SKU van het besturingssysteem en het wijzigen van de URI van een aangepaste installatiekopie. Zonder uitvaltijd bijwerken van virtuele machines één op een tijdstip of in groepen (zoals een foutdomein op een tijdstip) in plaats van in één keer wordt bijgewerkt. Op deze manier kunnen alle virtuele machines die niet wordt bijgewerkt blijven uitvoeren.

Om te voorkomen dubbelzinnigheid, gaan we onderscheiden vier typen OS-update die u wilt uitvoeren:

* Het wijzigen van de versie of SKU van een platforminstallatiekopie. Bijvoorbeeld, het wijzigen van Ubuntu 14.04.2-LTS versie van 14.04.201506100 14.04.201507060, of de Ubuntu 15.10/laatste SKU wijzigen naar 16.04.0-LTS/latest. Dit scenario wordt beschreven in dit artikel.
* Het wijzigen van de URI die naar een nieuwe versie van een aangepaste installatiekopie verwijst gemaakt (**eigenschappen** > **virtualMachineProfile** > **storageProfile**  >  **osDisk** > **installatiekopie** > **uri**). Dit scenario wordt beschreven in dit artikel.
* Het wijzigen van de verwijzing naar afbeelding van een scale-set die is gemaakt met beheerde Azure-schijven.
* Het besturingssysteem van een virtuele machine patchen (voorbeelden hiervan zijn een beveiligingspatch installeren en uitvoeren van Windows Update). Dit scenario wordt ondersteund, maar niet behandeld in dit artikel.

Virtuele-machineschaalsets die zijn geïmplementeerd als onderdeel van een [Azure Service Fabric](https://azure.microsoft.com/services/service-fabric/) cluster niet hier worden besproken. Zie voor meer informatie over Service Fabric-patching [Patch Windows-besturingssysteem in uw Service Fabric-cluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-patch-orchestration-application)

De volgorde van de basis voor het wijzigen van de OS-versie/SKU van een platforminstallatiekopie van het of de URI van een aangepaste installatiekopie ziet er als volgt uit:

1. Haal de virtuele machine scale set-model.
2. De versie, de SKU, de verwijzing naar afbeelding of de URI-waarde in het model wijzigen.
3. Het model bijwerken.
4. Voer een *manualUpgrade* aanroepen op de virtuele machines in de schaalset. Deze stap is alleen relevant als *upgradePolicy* is ingesteld op **handmatige** in uw scale ingesteld. Als deze is ingesteld op **automatische**, alle virtuele machines kunnen worden bijgewerkt in één keer dus uitvaltijd.

Met deze informatie in gedachten, gaan we kijken hoe u de versie van een schaal instellen in PowerShell en via de REST-API kan bijwerken. Deze voorbeelden hebben betrekking op het geval van een besturingssysteemkopie, maar dit artikel bevat onvoldoende informatie voor u aan te passen aan een aangepaste installatiekopie van dit proces.

## <a name="powershell"></a>PowerShell
In dit voorbeeld bijgewerkt in een Windows virtuele-machineschaalset ingesteld (maken naar de nieuwe versie 4.0.20160229. Na het bijwerken van het model wordt een update-exemplaar voor een virtuele machine op een tijdstip.

```powershell
$rgname = "myrg"
$vmssname = "myvmss"
$newversion = "4.0.20160229"
$instanceid = "1"

# get the VMSS model
$vmss = Get-AzureRmVmss -ResourceGroupName $rgname -VMScaleSetName $vmssname

# set the new version in the model data
$vmss.virtualMachineProfile.storageProfile.imageReference.version = $newversion

# update the virtual machine scale set model
Update-AzureRmVmss -ResourceGroupName $rgname -Name $vmssname -VirtualMachineScaleSet $vmss

# now start updating instances
Update-AzureRmVmssInstance -ResourceGroupName $rgname -VMScaleSetName $vmssname -InstanceId $instanceId
```

Als u de URI voor een aangepaste installatiekopie in plaats van het wijzigen van een installatiekopie platform versie bijwerken wilt, vervangen door de regel 'instellen voor de nieuwe versie' een opdracht die door de broninstallatiekopie URI worden bijgewerkt. Bijvoorbeeld, als de scale-set is gemaakt zonder gebruik van beheerde Azure-schijven, eruit de update als volgt:

```powershell
# set the new version in the model data
$vmss.virtualMachineProfile.storageProfile.osDisk.image.uri= $newURI
```

Als een set aangepaste schaal op basis van een installatiekopie is gemaakt met Azure beheerd schijven, zou de verwijzing naar afbeelding worden bijgewerkt. Bijvoorbeeld:

```powershell
# set the new version in the model data
$vmss.virtualMachineProfile.storageProfile.imageReference.id = $newImageReference
```

## <a name="the-rest-api"></a>De REST-API
Hier volgen enkele voorbeelden van Python die gebruikmaken van de REST-API van Azure implementeert een update OS-versie. Beide gebruik van de lightweight [azurerm](https://pypi.python.org/pypi/azurerm) bibliotheek met REST API van Azure wrapper functies GET op schaal model, gevolgd door een PUT met een bijgewerkte model instelt. Ze ook bekijken weergaven van de virtuele machine-exemplaren voor het identificeren van de virtuele machines door updatedomein.

### <a name="vmssupgrade"></a>Vmssupgrade
 [Vmssupgrade](https://github.com/gbowerman/vmsstools) is een pythonscript dat wordt gebruikt voor het bijwerken van het besturingssysteem implementeert op een actieve virtuele machine schaal één updatedomein tegelijk worden ingesteld.

![Vmssupgrade script voor het kiezen van virtuele machines of een updatedomein](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssupgrade-screenshot.png)

Dit script kunt u specifieke virtuele machines bijwerken of geef het updatedomein van een te kiezen. Het ondersteunt wijzigen van een platform-versie van de installatiekopie en het wijzigen van de URI van een aangepaste installatiekopie.

### <a name="vmsseditor"></a>Vmsseditor
[Vmsseditor](https://github.com/gbowerman/vmssdashboard) is een algemene editor voor de virtuele-machineschaalsets waarin de virtuele machine de status als een heatmap waarbij één rij één updatedomein vertegenwoordigt. U kunt onder andere het model voor een schaalset bijwerken met een nieuwe versie, SKU of aangepaste installatiekopie URI en selecteer vervolgens de domeinen met fouten om bij te werken. Wanneer u doet dit, worden alle virtuele machines in dat updatedomein bijgewerkt naar het nieuwe model. U kunt ook een uitrollende upgrade op basis van de batchgrootte van uw keuze doen.  

De volgende schermafbeelding ziet een model van een schaal voor Ubuntu 14.04-2LTS versie 14.04.201507060 instelt. Veel meer opties zijn toegevoegd aan dit hulpprogramma sinds deze schermafbeelding is gemaakt.

![Model van een schaal ingesteld voor Ubuntu 14.04-2LTS Vmsseditor](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssEditor1.png)

Nadat u op **Upgrade** en vervolgens **Details ophalen**, virtuele machines in UD 0 starten om bij te werken.

![Vmsseditor tonen worden bijgewerkt](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssEditor2.png)

